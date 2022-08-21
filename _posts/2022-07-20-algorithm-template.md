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
    - [Merge Sort Template](#merge-sort-template)
    - [Count Inversions](#count-inversions)
  - [Binary Search](#binary-search)
    - [Integer](#integer)
    - [Floating Point](#floating-point)
  - [High Precision](#high-precision)
    - [Add](#add)
    - [Sub](#sub)
    - [Mul](#mul)
    - [Div](#div)
  - [Prefix Sum and Difference](#prefix-sum-and-difference)
    - [One Dimensional Prefix Sum](#one-dimensional-prefix-sum)
    - [Two Dimensional Prefix Sum](#two-dimensional-prefix-sum)
    - [One Dimensional Difference](#one-dimensional-difference)
    - [Two Dimensional Difference](#two-dimensional-difference)
  - [Two Pointers](#two-pointers)
    - [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters)
    - [Target Sum](#target-sum)
    - [Is Subsequence](#is-subsequence)
  - [Bit Operation](#bit-operation)
  - [Discretization](#discretization)
  - [Merge Intervals](#merge-intervals)
- [Basic Data Structures](#basic-data-structures)
  - [Singly Linked List](#singly-linked-list)
  - [Doubly Linked List](#doubly-linked-list)
  - [Stack](#stack)
    - [Evaluate Equations](#evaluate-equations)
  - [Queue](#queue)
  - [Monotonic Stack](#monotonic-stack)
  - [Monotonic Queue](#monotonic-queue)
  - [KMP](#kmp)
  - [Trie](#trie)

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

#### Merge Sort Template

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

### Prefix Sum and Difference

#### One Dimensional Prefix Sum

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN];
int main(){
    int n,m;
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++){
        scanf("%d",&a[i]);
        a[i]+=a[i-1];
    }
    for(int i=0;i<m;i++){
        int l,r;scanf("%d%d",&l,&r);
        printf("%d\n",a[r]-a[l-1]);
    }
    return 0;
}
```

#### Two Dimensional Prefix Sum

```c++
#include<iostream>
#include<cstdio>
#define MAXN 1010
int a[MAXN][MAXN];
using namespace std;
int main(){
    int n,m,q;scanf("%d%d%d",&n,&m,&q);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            scanf("%d",&a[i][j]);
            a[i][j]+=a[i-1][j]+a[i][j-1]-a[i-1][j-1];
        }
    }
    while(q--){
        int x1,y1,x2,y2;scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
        printf("%d\n",a[x2][y2]-a[x2][y1-1]-a[x1-1][y2]+a[x1-1][y1-1]);
    }
    return 0;
}
```

#### One Dimensional Difference

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN];
int main(){
    int n,m;scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)scanf("%d",&a[i]);
    for(int i=n;i>1;i--)a[i]-=a[i-1];
    while(m--){
        int l,r,c;scanf("%d%d%d",&l,&r,&c);
        a[l]+=c;a[r+1]-=c;
    }
    for(int i=1;i<=n;i++){
        a[i]+=a[i-1];
        printf("%d ",a[i]);
    }
    return 0;
}
```

#### Two Dimensional Difference

```c++
#include<iostream>
#include<cstdio>
#define MAXN 1010
using namespace std;
int a[MAXN][MAXN];
int b[MAXN][MAXN];
void insert(int x1,int y1,int x2,int y2,int c){
    b[x1][y1]+=c;
    b[x1][y2+1]-=c;
    b[x2+1][y1]-=c;
    b[x2+1][y2+1]+=c;
}
int main(){
    int n,m,q;scanf("%d%d%d",&n,&m,&q);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            scanf("%d",&a[i][j]);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            insert(i,j,i,j,a[i][j]);
    while(q--){
        int x1,y1,x2,y2,c;
        scanf("%d%d%d%d%d",&x1,&y1,&x2,&y2,&c);
        insert(x1,y1,x2,y2,c);
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            b[i][j]+=b[i-1][j]+b[i][j-1]-b[i-1][j-1];
            printf("%d ",b[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```

### Two Pointers

#### Longest Substring Without Repeating Characters

```c++
#include<iostream>
#include<cstdio>
#include<unordered_map>
#define MAXN 100010
using namespace std;
int a[MAXN];
int main(){
    int n;scanf("%d",&n);
    unordered_map<int,int> m;
    int res=0;
    for(int i=1;i<=n;i++)scanf("%d",&a[i]);
    for(int i=1,j=1;i<=n;i++){
        if(m.find(a[i])!=m.end())j=max(j,m[a[i]]+1);
        m[a[i]]=i;
        res=max(res,i-j+1);
    }
    printf("%d\n",res);
    return 0;
}
```

#### Target Sum

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN],b[MAXN];
int main(){
    int n,m,x;scanf("%d%d%d",&n,&m,&x);
    for(int i=0;i<n;i++)scanf("%d",&a[i]);
    for(int j=0;j<m;j++)scanf("%d",&b[j]);
    int j=m-1;
    for(int i=0;i<n;i++){
        while(j>=0&&a[i]+b[j]>x)j--;
        if(a[i]+b[j]==x){
            printf("%d %d\n",i,j);
            break;
        }
    }
    return 0;
}
```

#### Is Subsequence

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int a[MAXN],b[MAXN];
int main(){
    int n,m;scanf("%d%d",&n,&m);
    for(int i=0;i<n;i++)scanf("%d",&a[i]);
    for(int j=0;j<m;j++)scanf("%d",&b[j]);
    int i=0,j=0;
    while(i<n&&j<m){
        if(a[i]==b[j])i++;
        j++;
    }
    if(i==n)printf("Yes\n");
    else printf("No\n");
    return 0;
}
```

### Bit Operation

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int lowbit(int x) {return x&(-x);}
int main(){
    int n;scanf("%d",&n);
    while(n--){
        int x;scanf("%d",&x);
        int res=0;
        while(x){
            res++;
            x=x&(x-1);
        }
        printf("%d ",res);
    }
    return 0;
}
```

### Discretization

```c++
#include<iostream>
#include<cstdio>
#include<vector>
#include<algorithm>
#define MAXN 500010
using namespace std;
int a[MAXN];
int main(){
    int n,m;scanf("%d%d",&n,&m);
    vector<int> alls;
    vector<pair<int,int>> add;
    vector<pair<int,int>> query;
    for(int i=0;i<n;i++){
        int x,c;scanf("%d%d",&x,&c);
        alls.push_back(x);
        add.push_back({x,c});
    }
    for(int i=0;i<m;i++){
        int l,r;scanf("%d%d",&l,&r);
        alls.push_back(l);
        alls.push_back(r);
        query.push_back({l,r});
    }
    sort(alls.begin(),alls.end());
    alls.erase(unique(alls.begin(),alls.end()),alls.end());
    auto find=[&](int x){
        int l=0,r=alls.size()-1;
        while(l<r){
            int m=(l+r)/2;
            if(alls[m]>=x)r=m;
            else l=m+1;
        }
        return r+1;
    };
    for(auto [x,c]:add){
        int idx=find(x);
        a[idx]+=c;
    }
    for(int i=1;i<=alls.size();i++)a[i]+=a[i-1];
    for(auto [l,r]:query){
        int ll=find(l),rr=find(r);
        printf("%d\n",a[rr]-a[ll-1]);
    }
    return 0;
}
```

### Merge Intervals

```c++
#include<iostream>
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
int main(){
    int n;scanf("%d",&n);
    vector<pair<int,int>> segs;
    for(int i=0;i<n;i++){
        int l,r;scanf("%d%d",&l,&r);
        segs.push_back({l,r});
    }
    sort(segs.begin(),segs.end());
    int st=-2e9,ed=-2e9;
    vector<pair<int,int>> res;
    for(auto [l,r]:segs){
        if(ed<l){
            if(st!=-2e9)res.push_back({st,ed});
            st=l;ed=r;
        }else ed=max(ed,r);
    }
    if(st!=-2e9)res.push_back({st,ed});
    printf("%d\n",res.size());
    return 0;
}
```

## Basic Data Structures

### Singly Linked List

```c++
#include<iostream>
#include<cstdio>
#include<string>
#define MAXN 100100
using namespace std;
int e[MAXN],ne[MAXN],head,idx;
void init(){
    head=-1;idx=0;
}
void add_to_head(int x){
    e[idx]=x;
    ne[idx]=head;
    head=idx;
    idx++;
}
void add(int k,int x){
    e[idx]=x;
    ne[idx]=ne[k];
    ne[k]=idx;
    idx++;
}
void remove(int k){
    ne[k]=ne[ne[k]];
}
int main(){
    int m;scanf("%d",&m);
    init();
    while(m--){
        char c; scanf(" %c ",&c);
        if(c=='H'){
            int x;scanf("%d",&x);
            add_to_head(x);
        }else if(c=='D'){
            int k;scanf("%d",&k);
            if(k==0)head=ne[head];
            else remove(k-1);
        }else{
            int k,x;scanf("%d%d",&k,&x);
            add(k-1,x);
        }
    }
    int k=head;
    while(k!=-1){
        printf("%d ",e[k]);
        k=ne[k];
    }
    return 0;
}
```

### Doubly Linked List

```c++
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int e[MAXN],l[MAXN],r[MAXN],idx;
void init(){
    l[1]=0;r[0]=1;idx=2;
}
void add(int a,int x){
    e[idx]=x;
    l[idx]=a;r[idx]=r[a];
    l[r[a]]=idx;r[a]=idx++;
}
void remove(int a){
    r[l[a]]=r[a];
    l[r[a]]=l[a];
}
int main(){
    int m;scanf("%d",&m);
    init();
    while(m--){
        char c;scanf(" %c ",&c);
        if(c=='L'){
            int x;scanf("%d",&x);
            add(0,x);
        }else if(c=='R'){
            int x;scanf("%d",&x);
            add(l[1],x);
        }else if(c=='D'){
            int k;scanf("%d",&k);
            remove(k+1);
        }else if(c=='I'){
            scanf(" %c ",&c);
            if(c=='L'){
                int k,x;scanf("%d%d",&k,&x);
                add(l[k+1],x);
            }else if(c=='R'){
                int k,x;scanf("%d%d",&k,&x);
                add(k+1,x);
            }
        }
    }
    for(int i=r[0];i!=1;i=r[i])printf("%d ",e[i]);
    return 0;
}
```

### Stack

```cpp
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int stk[MAXN],tt;
int main(){
    int m;scanf("%d",&m);
    while(m--){
        string s;cin>>s;
        if(s=="push"){
            int x;scanf("%d",&x);
            stk[++tt]=x;
        }else if(s=="pop"){
            tt--;
        }else if(s=="query"){
            printf("%d\n",stk[tt]);
        }else if(s=="empty"){
            if(tt==0)printf("YES\n");
            else printf("NO\n");
        }
    }
    return 0;
}
```

#### Evaluate Equations

```c++
#include<iostream>
#include<cstdio>
#include<stack>
#include<unordered_map>
using namespace std;
stack<int> num;
stack<char> op;
void eval(){
    int b=num.top();num.pop();
    int a=num.top();num.pop();
    char c=op.top();op.pop();
    int x;
    if(c=='+')x=a+b;
    if(c=='-')x=a-b;
    if(c=='*')x=a*b;
    if(c=='/')x=a/b;
    num.push(x);
}
int main(){
    unordered_map<char,int> pr{
        {'+',1},{'-',1},{'*',2},{'/',2}
    };
    string s;cin>>s;
    for(int i=0;i<s.length();i++){
        char c=s[i];
        if(isdigit(c)){
            int x=0,j=i;
            while(j<s.length()&&isdigit(s[j]))
                x=x*10+s[j++]-'0';
            i=j-1;
            num.push(x);
        }else if(c=='(')op.push(c);
        else if(c==')'){
            while(op.top()!='(')eval();
            op.pop();
        }else{
            while(op.size()&&pr[op.top()]>=pr[c])eval();
            op.push(c);
        }
    }
    while(op.size())eval();
    cout<<num.top()<<endl;
    return 0;
}
```

### Queue

```c++
#include<iostream>
#include<cstdio>
#include<string>
#define MAXN 100010
using namespace std;
int q[MAXN],hh=0,tt=-1;
int main(){
    int m;scanf("%d",&m);
    while(m--){
        string s;cin>>s;
        if(s=="push"){
            int x;cin>>x;
            q[++tt]=x;
        }else if(s=="pop"){
            hh++;
        }else if(s=="empty"){
            if(hh==tt+1)printf("YES\n");
            else printf("NO\n");
        }else if(s=="query"){
            printf("%d\n",q[hh]);
        }
    }
    return 0;
}
```

### Monotonic Stack

```cpp
#include<iostream>
#include<cstdio>
#define MAXN 100010
using namespace std;
int stk[MAXN],tt=0;
int main(){
    int n;cin>>n;
    for(int i=0;i<n;i++){
        int x;cin>>x;
        while(tt&&stk[tt]>=x)tt--;
        if(tt)printf("%d ",stk[tt]);
        else printf("-1 ");
        stk[++tt]=x;
    }
    return 0;
}
```

### Monotonic Queue

```cpp
#include<iostream>
#include<cstdio>
#define MAXN 1000010
using namespace std;
int q[MAXN],hh=0,tt=-1;
int a[MAXN];
int main(){
    int n,k;scanf("%d%d",&n,&k);
    for(int i=0;i<n;i++){
        scanf("%d",&a[i]);
        if(q[hh]<i-k+1)hh++;
        while(hh<=tt&&a[i]<=a[q[tt]])tt--;
        q[++tt]=i;
        if(i+1>=k)printf("%d ",a[q[hh]]);
    }
    printf("\n");
    hh=0;tt=-1;
    for(int i=0;i<n;i++){
        if(q[hh]<i-k+1)hh++;
        while(hh<=tt&&a[i]>=a[q[tt]])tt--;
        q[++tt]=i;
        if(i+1>=k)printf("%d ",a[q[hh]]);
    }
    return 0;
}
```

### KMP

```cpp
#include<iostream>
#include<cstdio>
#define MAXN 100010 
using namespace std;
int nxt[MAXN];
int main(){
    string p,s;
    int n,m;cin>>n>>p>>m>>s;
    int i,j;
    j=nxt[0]=-1;
    i=0;
    while(i<n){
        while(j!=-1&&p[i]!=p[j])j=nxt[j];
        // nxt[++i]=++j;
        if(p[++i]==p[++j])nxt[i]=nxt[j];
        else nxt[i]=j;
    }
    i=j=0;
    while(i<m){
        while(j!=-1&&s[i]!=p[j])j=nxt[j];
        i++;j++;
        if(j>=n){
            printf("%d ",i-j);
            j=nxt[j];
        }
    }
    return 0;
}
```

### Trie

```cpp
#include<iostream>
#include<cstdio>
#define MAXN 100020
using namespace std;
int ch[MAXN][26],cnt[MAXN],idx;
void insert(string& s){
    int p=0;
    for(char cc:s){
        int c=cc-'a';
        if(!ch[p][c])ch[p][c]=++idx;
        p=ch[p][c];
    }
    cnt[p]++;
}
int query(string& s){
    int p=0;
    for(char cc:s){
        int c=cc-'a';
        if(!ch[p][c])return 0;
        p=ch[p][c];
    }
    return cnt[p];
}
int main(){
    int n;cin>>n;
    while(n--){
        string t,s;cin>>t>>s;
        if(t=="I"){
            insert(s);
        }else{
            cout<<query(s)<<endl;
        }
    }
    return 0;
}
```
