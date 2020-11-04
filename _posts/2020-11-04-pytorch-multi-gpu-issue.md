---
title: 'A gotcha with multi-GPU training of dynamic neural networks in PyTorch'
date: 2020-11-04
permalink: /posts/2020/11/pytorch-multi-gpu-issue/
tags:
  - Deep Learning
  - PyTorch
---
I recently ran into an issue with training/testing dynamic neural network architectures on multiple GPUs in PyTorch. In this short blog post I will summarize the issue and suggest a possible workaround for others who might come across it.  

# The issue

I made a post on the PyTorch forums discussing my issue ([linked here](https://discuss.pytorch.org/t/device-mismatch-error-with-nn-dataparallel/100048/3)). To summarise my issue:

Dynamic binding of a function to an attribute of the `nn.Module` class leads to a device mismatch error when loading the model to multiple GPUs using `nn.DataParallel()`. The error message you'll get looks something like this:

```
Expected tensor for argument #1 'input' to have the same device as tensor for argument #2 'weight'; but device 1 does not equal 0
```


# A working example

Suppose I have a small feedforward Residual module which also takes a parameter called `add_output`. If `add_output==True` then a small auxilliary classifier is attached to the Residual module so as not only feedforward the input but also return classification outputs. The Pytorch code for doing this is shown below:

 ```python
import torch
from torch import nn

from external_file import aux_classifier

class ResBlock(nn.Module):
    expansion = 1

    def __init__(self, in_channels, channels, stride=1, num_classes, input_size, add_output, output_id):
        super(ResBlock, self).__init__()
        self.output_id = output_id
        self.depth = 2

        layers = nn.ModuleList()

        conv_layer = []
        conv_layer.append(nn.Conv2d(in_channels, channels, kernel_size=3, stride=stride, padding=1, bias=False))
        conv_layer.append(nn.BatchNorm2d(channels))
        conv_layer.append(nn.ReLU())
        conv_layer.append(nn.Conv2d(channels, channels, kernel_size=3, stride=1, padding=1, bias=False))
        conv_layer.append(nn.BatchNorm2d(channels))

        layers.append(nn.Sequential(*conv_layer))

        shortcut = nn.Sequential()

        if stride != 1 or in_channels != self.expansion*channels:
            shortcut = nn.Sequential(
                nn.Conv2d(in_channels, self.expansion*channels, kernel_size=1, stride=stride, bias=False),
                nn.BatchNorm2d(self.expansion*channels)
            )

        layers.append(shortcut)
        layers.append(nn.ReLU())

        self.layers = layers

        if add_output:
            self.output = aux_classifier(input_size, self.expansion*channels, num_classes) 
            self.no_output = False
        else:
            self.output = None
            self.forward = self.only_forward
            self.no_output = True
            
    def forward(self, x):
        fwd = self.layers[0](x) # conv layers
        fwd = fwd + self.layers[1](x) # shortcut
        return self.layers[2](fwd), 1, self.output(fwd)   # output layers for this module

    def only_output(self, x):
        fwd = self.layers[0](x) # conv layers
        fwd = fwd + self.layers[1](x) # shortcut
        fwd = self.layers[2](fwd) # activation
        out = self.output(fwd)         # output layers for this module
        return out
    
    def only_forward(self, x):
        fwd = self.layers[0](x) # conv layers
        fwd = fwd + self.layers[1](x) # shortcut
        return self.layers[2](fwd), 0, None # activation
 ```

Now the issue arises from the way I assign `self.forward = self.only_forward` in the code block where `add_output` is False. As pointed out by Tongzhou Wang (one of PyTorch's developers) in this GitHub discussion ([link to Github issue](https://github.com/pytorch/pytorch/issues/8637)), the issue (paraphrased for our example residual block) is that:

> you saved the method `self.only_forward`, which is bound to this particular `ResBlock` instance, as the attribute `self.forward`. When broadcasting the module to different GPUs, this attribute, as it is not a tensor, is just simply duplicated, which means that all broadcast copies of this module have the attribute refer to the same bound method, and that this method is bound to the same instance and thus using the same `self.layers`, which has all parameters only on GPU 0. Therefore it errors on GPU 1. 

This is a design choice by the creators of PyTorch in order to reduce the overhead while broadcasting models to multiple GPUs (as discussed on Github). However this causes an issue when creating a computational graph which might need to be modified at run time based on input conditions (as highlighted by the `add_output` parameter in our case). This happens to be exactly the type of computation that **dynamic neural network architecutres** perform. These models are popular in resource-efficient machine learning as well as semantic parsing based machine reasoning literature: some examples being the Multi-Scale Dense Net (https://arxiv.org/abs/1703.09844) and the really popular Neural Module Networks (https://arxiv.org/abs/1511.02799) To read more about dynamic (also referred to as **input-adaptive**) neural networks refer to [this excellent blogpost by Petuum](https://medium.com/@Petuum/intro-to-dynamic-neural-networks-and-dynet-67694b18cb23). 

# The fix

I worked around this issue by not dynamically binding the `forward` function to the `only_output` member function. Instead I performed the evaluation fo my conditional inside the forward function (as was suggested Tongzhou on the Github issue). This solved my issue with multi-GPU training as now all the parameters from `self.layers` linked to `self.only_forward` were not associated with the object of my ResBlock class and were being broadcasted across GPUs in the `nn.DataParallel` module

The final part of the code which was modified is present below for reference:

 ```python
        if add_output:
            self.output = aux_classifier(input_size, self.expansion*channels, num_classes) 
            self.no_output = False
        else:
            self.output = None
            #self.forward = self.only_forward
            self.no_output = True
            
    def forward(self, x):
			if self.no_output:
				return self.only_forward(x) 
			else:
		       fwd = self.layers[0](x) # conv layers
		       fwd = fwd + self.layers[1](x) # shortcut
		       return self.layers[2](fwd), 1, self.output(fwd)   # output layers for this module
 ```

And it worked! I was able to train my dynamic neural net on multiple GPUs using the `nn.DataParallel` API. I've also read that PyTorch launched a new parallelization API called `DistributedDataParallel` which is objectively better but I've yet to try it out. Hopefully this post helps other people who might run into this issue.
