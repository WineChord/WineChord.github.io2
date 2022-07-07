---
layout: post
title: "Codeforces Contests"
author: "Wine&Chord"
categories: coding
tags: [coding]
---

# Codeforces Round #804 (Div. 2)

## A. The Third Three Number Problem

```cpp
#include<iostream>
#include<cstdio>
using namespace std;
int main(){
#ifdef WINE
    freopen("data.in","r",stdin);
#endif
    int t;cin>>t;
    while(t--){
        int n;cin>>n;
        int num=n;
        if(n&1){
            cout<<-1<<"\n";
            continue;
        }
        int a=0,b=0,c=0;
        int i=0;
        while(n){
            n>>=1;
            int x=n&1;
            if(x)a|=(1<<i);
            i++;
        }
        cout<<a<<" "<<"0 0\n";
    }
    return 0;
}
```

## B. Almost Ternary Matrix

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

void testcase(){
    ll n,m;
    cin>>n>>m;

    for(ll i=0;i<n;i++){
        for(ll j=0;j<m;j++){
            int x=i%4; // 0 1 2 3 
            int y=j%4; // 0 1 2 3 
            if(x==y||x+y==3)cout<<1;
            else cout<<0;
            cout<<" \n"[j==m-1];
        }
    }
}
int main()
{
#ifdef WINE
    freopen("data.in","r",stdin);
#endif
    ios_base::sync_with_stdio(false); cin.tie(0);
    int t;
    cin>>t;
    while(t--)
        testcase();
    return 0;
}
```

