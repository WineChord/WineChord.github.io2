---
layout: post
title: "Algorithm Template"
author: "Wine&Chord"
categories: coding
tags: [coding]
---

- [Basic Algorithms](#basic-algorithms)
  - [Quick Sort](#quick-sort)
    - [Template](#template)
    - [Top-K: Quick Select](#top-k-quick-select)

## Basic Algorithms

### Quick Sort

#### Template

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

#### Top-K: Quick Select

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN];
int qs(int l,int r,int k){
    if(l==r)return a[l];
    int x=a[(l+r)>>1],i=l-1,j=r+1;
    while(i<j){
        do i++;while(a[i]<x);
        do j--;while(a[j]>x);
        if(i<j)swap(a[i],a[j]);
    }
    int cnt=j-l+1;
    if(k<=cnt)return qs(l,j,k);
    return qs(j+1,r,k-cnt);
}
int main(){
    // k starts from 1.
    int n,k;scanf("%d%d",&n,&k);
    for(int i=0;i<n;i++)scanf("%d",&a[i]);
    printf("%d\n",qs(0,n-1,k));
}
```

<!-- ## Basic Data Structures -->