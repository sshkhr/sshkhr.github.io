---
title: 'Writing your custom sort functions in C++'
date: 2021-03-11
permalink: /posts/2021/03/cpp-custom-sort/
tags:
  - Algorithms
  - C++
---
In this blog post I will quickly go over how to define your custom comparator for `sort()` for your data structure in C++ based on two example cases.


I decided to start programming in C++ again after a long time (end of 2017!!) because I was practising algorithm-style problems for job interviews and C++ seemed like a natural choice. It's Standard Template Library makes working with data structures very easy by providing a uniform api and inbuild data structures. I noticed that a lot of the questions that I came across required me to overload the comparator operator in C++ `sort()` function since I was working with custom data structures. Next, I will mention two example cases of such problems and how I went around writing the sort comparator for my requirements.


# Case 1. Sort a `vector<vector<int>>` based on two different conditions

```cpp
struct contestComparator
{
    bool operator ()(const vector<int> & contest1, const vector<int> & contest2)
    {
        if(contest1[1] != contest2[1])
            return contest1[1] >  contest2[1]; // consider important contests first
        return contest1[0] < contest2[0];
    }
};

// Complete the luckBalance function below.
int luckBalance(int k, vector<vector<int>> contests) {
    
    int luck_balance = 0;
    int len = contests.size();
    
    sort(contests.begin(), contests.end(), contestComparator());
    
    int num_important = 0;
    
    for(int i = 0; i<len && contests[i][1] == 1; i++)
        num_important++;
        
    k = num_important - k;
    
    for(int i = 0; i < len; i++)
    {
        cout<<contests[i][0]<<" ";
        if(i<k)
            luck_balance -= contests[i][0];
        else
            luck_balance += contests[i][0];
    }
    
    return luck_balance;
}

```


# Case 1. Sort a `vector<pair<int, int>>` based on the first number in the `pair`


```cpp
struct pairComp
{
    bool operator ()(const pair<int, int> & pair1, const pair<int, int> & pair2)
    {
        return pair1.first < pair2.first;
    }
};

// Complete the whatFlavors function below.
void whatFlavors(vector<int> cost, int money) {
    
    vector<pair<int,int>> costidx;
    
    for(int i=0; i<cost.size(); i++)
        costidx.push_back(make_pair(cost[i], i+1));
    
    sort(costidx.begin(), costidx.end());
    
    int i=0;
    int j=cost.size()-1;
    
    while(i<j)
    {
        if(costidx[i].first + costidx[j].first > money)
            j--;
        else if(costidx[i].first + costidx[j].first < money)
            i++;
        else
            break;
    }
    
    int min = (costidx[i].second < costidx[j].second)? costidx[i].second: 
                costidx[j].second;
    int max = (costidx[i].second > costidx[j].second)? costidx[i].second: 
                costidx[j].second;
    
    
    cout<<min<<" "<<max<<endl;
}
```