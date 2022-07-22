---
layout: post
title: "Algorithm Template"
author: "Wine&Chord"
categories: coding
tags: [coding]
---

- [Basic Algorithms](#basic-algorithms)
  - [Quick Sort](#quick-sort)

## Basic Algorithms

### Quick Sort

[https://www.luogu.com.cn/problem/P1177](https://www.luogu.com.cn/problem/P1177)

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN];
void qs(int *a,int l,int r){
    if(l>=r)return;
    int x=a[l+r>>1],i=l-1,j=r+1;
    while(i<j){
        do i++;while(a[i]<x);
        do j--;while(a[j]>x);
        if(i<j)swap(a[i],a[j]);
    }
    qs(a,l,j);qs(a,j+1,r);
}
int main(){
    int n;scanf("%d",&n);
    for(int i=0;i<n;i++)scanf("%d",&a[i]);
    qs(a,0,n-1);
    for(int i=0;i<n;i++)printf("%d ",a[i]);
}
```

<!-- ## Basic Data Structures -->