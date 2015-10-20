---
layout: post
section-type: post
title: "Arduino Reset Hack "
category: tech
tags: [ 'software', 'hardware', 'opensource' ]
---
Arduino is great for hardware prototyping and for hobbyists but hardware runs software and every software once in a while needs magic "self-recovery", which practically is a reboot.

But, the common way of resetting your Arduino is by wiring a special pin and then sending 5 Volts to it.
This means that if you want to make your Arduino reset-able (which you will always will), then you sacrifice one pin.
A pin that its lack could ruin the size of your project, because if you run out of pins on the board you have to get a bigger one.

So, I needed one pin, and instead of rushing into buying a bigger board, I found a way to reset it from the software:

<script src="https://gist.github.com/PanosSakkos/7766479.js"></script>

Very straightforward, and I don't see any caveats so far 😊.

Also, like it was pointed out on reddit, you can do cool stuff like resetting remotely your board!

P.S: Mads found <a href="https://www.atmel.com/webdoc/AVRLibcReferenceManual/FAQ_1faq_softreset.html" target="_blank">this</a>, so test it in case you are using an new AVR
