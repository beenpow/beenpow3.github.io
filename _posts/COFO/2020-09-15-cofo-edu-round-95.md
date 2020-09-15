---
title : COFO::Educational Round 95 (div 2)

categories:
    - COFO
    - 코드포스
tag:
    - COFO
    - 코드포스
---
# COFO::Educational Round #95 ( div 2 )
- [Link : COFO::round 95 (div 2)](https://codeforces.com/contest/1418)
- solved : 
  - A :  ( 00:23 )
  - rank : 
  - score :

## Problem A : Buying Torches

- level : 1000
- tag : math
- time : 00:23

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define pb push_back
//#define f first
//#define s second
#define all(v) (v).begin(), (v).end()
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int, int> >
#define vpll vector<pair<ll, ll> >
typedef long long ll;
using namespace std;
#define MOD 1000000007

void process(){
    ll x, y, k; cin >> x >> y >> k;
    ll ans = k;
    ll cnts = 1 - y * k;
    ll q = (-cnts + k) / (x - 1);
    ll r = (-cnts + k) % (x - 1);
    ans += q;
    if(r > 0) ans++;
    cout << ans << endl;
}
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int tc; cin >> tc;
    while(tc--)
    process();
    return 0;
}
```

## Problem B : Negative Prefixes

- level : 1300
- tag : greedy, sortings

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define pb push_back
//#define f first
//#define s second
#define all(v) (v).begin(), (v).end()
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int, int> >
#define vpll vector<pair<ll, ll> >
typedef long long ll;
using namespace std;
#define MOD 1000000007

int n;
vi a, b;
vi pl;
void process(){
    cin >> n;
    pl.clear();
    a.resize(n); b.resize(n);
    
    rep(i, 0, n) cin >> a[i];
    rep(i, 0, n){
        cin >> b[i];
        if(b[i] == 0) pl.pb(a[i]);
    }
    sort(pl.rbegin(), pl.rend());
    
    int p1 = 0;
    rep(i, 0, n) if(b[i] == 0){
        a[i] = pl[p1++];
    }
    rep(i, 0, n) cout << a[i] <<" ";
    cout << endl;
}
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int tc; cin >> tc;
    while(tc--)
        process();
    return 0;
}
```

## Problem C : Mortal Kombat Tower

- level : 1500
- tag : dp, greedy

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define pb push_back
//#define f first
//#define s second
#define all(v) (v).begin(), (v).end()
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int, int> >
#define vpll vector<pair<ll, ll> >
typedef long long ll;
const int MAXN = 2 * 100000 + 100, inf = 0x3f3f3f3f;
enum{ F = 0, M = 1};
enum{ EASY = 0, HARD = 1};
using namespace std;
#define MOD 1000000007

int n;
vi a;
int dp[MAXN][2];
// dp[i][who] : i번째 보스를 잡을 차례에 who의 차례일때, [i:n) 범위의 보스를 잡는데 소모되는 최소 skip points
int dynamic(int i, bool who){
    if(i >= n) return 0;
    int& ret = dp[i][who];
    if(ret != -1) return ret;
    ret = inf;
    if(who == F){
        if(a[i] == HARD){
            ret = min(ret, dynamic(i+1, !who) + 1);
            if(a[i+1] == HARD){
                ret = min(ret, dynamic(i+2, !who) + 2);
            }else{
                ret = min(ret, dynamic(i+2, !who) + 1);
            }
        }else{
            ret = min(ret, dynamic(i+1, !who));
            if(a[i+1] == HARD){
                ret = min(ret, dynamic(i+2, !who) + 1);
            }else{
                ret = min(ret, dynamic(i+2, !who) );
            }
        }
    }else{
        ret = min(ret, dynamic(i+1, !who));
        ret = min(ret, dynamic(i+2, !who));
    }
    return ret;
}
void process(){
    cin >> n;
    a.resize(n+2); rep(i, 0, n) cin >> a[i];
    a[n] = a[n+1] = 0;
    memset(dp, -1, sizeof(dp));
    dynamic(0,  F);
    cout << dp[0][F] << endl;
}
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int tc; cin >> tc;
    while(tc--)
        process();
    return 0;
}
```

## Problem D : Trash Problem

- level : 2100
- tag : data structures, hashing, implmentation, two pointers

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define pb push_back
//#define f first
//#define s second
#define all(v) (v).begin(), (v).end()
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int, int> >
#define vpll vector<pair<ll, ll> >
typedef long long ll;
const int MAXN = 2 * 100000 + 100, inf = 0x3f3f3f3f;
using namespace std;
#define MOD 1000000007

int n, Q;
set<int> s;
priority_queue<int> pq, del;

int MAX(){
    while(!pq.empty() && !del.empty() && pq.top() == del.top())
        pq.pop(), del.pop();
    if(pq.empty()) return 0;
    else return pq.top();
}
void input(){
    cin >> n >> Q;
    rep(i, 0, n){
        int x; cin >> x;
        s.insert(x);
    }
    int prev = 0;
    for(auto x : s){
        if(prev) pq.push(x - prev);
        prev = x;
    }
}
void process(){
    input();
    if(s.empty()) cout << "0\n";
    else cout << *(prev(s.end())) - *(s.begin()) - MAX() << endl;
    while(Q--){
        int ti, xi; cin >> ti >> xi;
        if(ti == 0){
            auto itr = s.find(xi);
            if(itr != s.begin()) del.push(xi - *prev(itr));
            if(itr != s.end() && next(itr) != s.end()) del.push(*next(itr) - xi);
            if(itr != s.begin() && itr != s.end() && next(itr) != s.end()) pq.push(*next(itr) - *prev(itr));
            s.erase(xi);
        }else{
            auto itr = s.lower_bound(xi); //lower_bound(s.begin(), s.end(), xi);
            if(itr != s.begin()) pq.push(xi - *prev(itr));
            if(itr != s.end()) pq.push(*itr - xi);
            if(itr != s.begin() && itr != s.end()) del.push(*(itr) - *prev(itr));
            s.insert(xi);
        }
        
        if(s.empty()) cout << "0\n";
        else cout << *(prev(s.end())) - *(s.begin()) - MAX() << endl;
    }
}
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    process();
    return 0;
}
```
