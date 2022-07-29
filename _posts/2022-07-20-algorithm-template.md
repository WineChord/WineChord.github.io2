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
  - [Merge Sort](#merge-sort)
    - [Template](#template-1)
    - [Count Inversions](#count-inversions)
  - [Binary Search](#binary-search)
    - [Integer](#integer)
    - [Floating Point](#floating-point)
  - [High Precision](#high-precision)
    - [Add](#add)
    - [Sub](#sub)
    - [Mul](#mul)
    - [Div](#div)

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

### Merge Sort

#### Template

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN],tmp[MAXN];
void ms(int l,int r){
    if(l>=r)return;
    int m=l+r>>1;
    ms(l,m);ms(m+1,r);
    int k=0,i=l,j=m+1;
    while(i<=m&&j<=r)
        if(a[i]<=a[j])tmp[k++]=a[i++];
        else tmp[k++]=a[j++];
    while(i<=m)tmp[k++]=a[i++];
    while(j<=r)tmp[k++]=a[j++];
    for(int i=l,j=0;i<=r;i++,j++)a[i]=tmp[j];
}
int main(){
    int n;scanf("%d",&n);
    for(int i=0;i<n;i++)scanf("%d",&a[i]);
    ms(0,n-1);
    for(int i=0;i<n;i++)printf("%d ",a[i]);
    return 0;
}
```

#### Count Inversions

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using ll=long long;
int a[MAXN],tmp[MAXN];
ll ms(int l,int r){
    if(l>=r)return 0;
    ll res=0;int m=l+r>>1;
    res+=ms(l,m)+ms(m+1,r);
    int i=l,j=m+1,k=0;
    while(i<=m&&j<=r)
        if(a[i]<=a[j])tmp[k++]=a[i++];
        else{
            tmp[k++]=a[j++];
            res+=m-i+1;
        }
    while(i<=m)tmp[k++]=a[i++];
    while(j<=r)tmp[k++]=a[j++];
    for(int i=l,j=0;i<=r;i++,j++)a[i]=tmp[j];
    return res;
}
int main(){
    int n;scanf("%d",&n);
    for(int i=0;i<n;i++)scanf("%d",&a[i]);
    printf("%lld\n",ms(0,n-1));
    return 0;
}
```

### Binary Search

#### Integer

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN];
int main(){
    int n,q;scanf("%d%d",&n,&q);
    for(int i=0;i<n;i++)scanf("%d",&a[i]);
    while(q--){
        int k;scanf("%d",&k);
        int l=0,r=n-1;
        while(l<r){
            int m=l+r>>1; // Type 1.
            if(a[m]>=k)r=m;
            else l=m+1;
        }
        if(a[r]!=k){
            printf("-1 -1\n");
            continue;
        }
        printf("%d ",r);
        l=0,r=n-1;
        while(l<r){
            int m=l+r+1>>1; // Type 2.
            if(a[m]<=k)l=m;
            else r=m-1;
        }
        printf("%d\n",r);
    }
    return 0;
}
```

#### Floating Point

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int main(){
    // Find x s.t. x*x*x = n.
    double n;scanf("%lf",&n);
    double l=-10000,r=10000;
    while(r-l>1e-8){
        double m=(l+r)/2;
        if(m*m*m>=n)r=m;
        else l=m;
    }
    printf("%.6lf\n",r);
    return 0;
}
```

### High Precision

#### Add

```c++
#include<iostream>
#include<cstdio>
#include<vector>
using namespace std;
vector<int> add(vector<int>& a,vector<int>& b) {
    vector<int> c;
    if(a.size()<b.size())return add(b,a);
    int up=0;
    for(int i=0;i<a.size();i++){
        up+=a[i];
        if(i<b.size())up+=b[i];
        c.push_back(up%10);
        up/=10;
    }
    if(up)c.push_back(1);
    return c;
}
int main(){
    string a,b;
    cin>>a>>b;
    vector<int> A,B;
    for(int i=a.length()-1;i>=0;i--)A.push_back(a[i]-'0');
    for(int i=b.length()-1;i>=0;i--)B.push_back(b[i]-'0');
    auto C=add(A,B);
    for(int i=C.size()-1;i>=0;i--)printf("%d",C[i]);
    return 0;
}
```

#### Sub

```c++
#include<iostream>
#include<cstdio>
#include<vector>
using namespace std;
bool cmp(vector<int>& a,vector<int>& b){
    if(a.size()!=b.size())return a.size()>b.size();
    for(int i=a.size()-1;i>=0;i--)
        if(a[i]!=b[i])return a[i]>b[i];
    return true;
}
vector<int> sub(vector<int>& a,vector<int>& b) {
    vector<int> c;
    int t=0;
    for(int i=0;i<a.size();i++){
        t=a[i]-t;
        if(i<b.size())t-=b[i];
        c.push_back((t+10)%10);
        if(t<0)t=1;
        else t=0;
    }
    while(c.size()>1&&c.back()==0)c.pop_back();
    return c;
}
int main(){
    string a,b;
    cin>>a>>b;
    vector<int> A,B;
    for(int i=a.length()-1;i>=0;i--)A.push_back(a[i]-'0');
    for(int i=b.length()-1;i>=0;i--)B.push_back(b[i]-'0');
    if(cmp(A,B)){
        auto C=sub(A,B);
        for(int i=C.size()-1;i>=0;i--)printf("%d",C[i]);
    }else{
        auto C=sub(B,A);printf("-");
        for(int i=C.size()-1;i>=0;i--)printf("%d",C[i]);        
    }
    return 0;
}
```

#### Mul

```c++
#include<iostream>
#include<cstdio>
#include<vector>
using namespace std;
vector<int> mul(vector<int>& A,int b){
    vector<int> C;
    int up=0;
    for(int i=0;i<A.size()||up;i++){
        if(i<A.size())up+=A[i]*b;
        C.push_back(up%10);
        up/=10;
    }
    while(C.size()>1&&C.back()==0)C.pop_back();
    return C;
}
int main(){
    string a;int b;
    cin>>a>>b;
    vector<int> A;
    for(int i=a.length()-1;i>=0;i--)A.push_back(a[i]-'0');
    auto C=mul(A,b);
    for(int i=C.size()-1;i>=0;i--)printf("%d",C[i]);
}
```

#### Div

```c++
#include<iostream>
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
vector<int> div(vector<int>& A,int b, int& r){
    vector<int> C;
    r=0;
    for(int i=A.size()-1;i>=0;i--){
        r=r*10+A[i];
        C.push_back(r/b);
        r%=b;
    }
    reverse(C.begin(),C.end());
    while(C.size()>1&&C.back()==0)C.pop_back();
    return C;
}
int main(){
    string a;int b;cin>>a>>b;
    vector<int> A;
    for(int i=a.size()-1;i>=0;i--)A.push_back(a[i]-'0');
    int r;
    auto C=div(A,b,r);
    for(int i=C.size()-1;i>=0;i--)printf("%d",C[i]);
    printf("\n%d",r);
}
```

<!-- ## Basic Data Structures -->