---
title : COFO::Educational Round 93 ( div 2 )

categories:
    - COFO
    - 코드포스
tag:
    - COFO
    - 코드포스
---
# COFO::Educational Round 93 ( div 2 )
- [Link : COFO::Educational Round 93 ( div 2 )](https://codeforces.com/contest/1398)
- solved : 
  - A :  ( 00:06 )
  - B :  ( 00:14 )
  - rank : 5064
  - score : 2 solved. penalty : 20
- A, B까지 순조로웠습니다.
- D는 dp로 풀이했지만, 앞에서부터 채우는 dp가 아닌 뒤에서부터 채우는 dp로 풀었어야 했고
- 작은 조작이지만 해당 개념에 대한 정립이 되어있지 않아서 마무리 짓지 못하였습니다.

## Problem A : Bad Triangles

- level :
- tag :
- time :

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define all(v) (v).begin(), (v).end()
#define f first
#define s second
#define pb push_back
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int,int> >
#define vvpi vector<vector<pair<int,int> > >
typedef long long ll;
using namespace std;

int n;
vll a;
void process(){
    cin >> n;
    a.resize(n);
    rep(i, 0, n) cin >> a[i];
    if(a[0] + a[1] <= a.back()) cout << 1 << " " << 2 << " " << a.size() << endl;
    else cout << "-1" << endl;
}

int main(){
    int tc; cin >> tc;
    while(tc--)
        process();
    return 0;
}
```

## Problem B : Substring Removal Game

- level :
- tag :
- time :

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define all(v) (v).begin(), (v).end()
#define f first
#define s second
#define pb push_back
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int,int> >
#define vvpi vector<vector<pair<int,int> > >
typedef long long ll;
using namespace std;

vi ans;
void process(){
    ans.clear();
    string s; cin >> s;
    s += '0';

    int cnt1 = 0;
    rep(i, 0, s.size()){
        if(s[i] == '0'){
            if(cnt1 > 0) ans.pb(cnt1);
            cnt1 = 0;
        }else{
            cnt1++;
        }
    }
    sort(all(ans));
    reverse(all(ans));
    int answer = 0;
    for(int i = 0; i < ans.size(); i += 2){
        answer += ans[i];
    }
    cout << answer << endl;
}

int main(){
    int tc; cin >> tc;
    while(tc--)
        process();
    return 0;
}
```

## Problem C : Good Subarrays

- level :
- tag :

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

## Problem D : Colored Rectangles

- level :
- tag :
- time :

### Point

### Design

#### Big O(time)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define all(v) (v).begin(), (v).end()
//#define f first
//#define s second
#define pb push_back
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int,int> >
#define vvpi vector<vector<pair<int,int> > >
typedef long long ll;
const int MAXN = 200 + 10;
using namespace std;

int nr, ng, nb;
vi r, g, b;
ll dp[MAXN][MAXN][MAXN];
// maxsum = 4 * 10^6 * 300 pairs = 12 * 10^8
ll f(int ri, int gi, int bi){
    ll &ret = dp[ri][gi][bi];
    if(ret != -1) return ret;
    if((ri >= nr) + (gi >= ng) + (bi >= nb) >= 2) return ret = 0;
    // (r, g)
    if(ri < nr && gi < ng)
        ret = max(ret, f(ri+1, gi+1, bi) + 1LL * r[ri] * g[gi]);
    // (r, b)
    if(ri < nr && bi < nb)
        ret = max(ret, f(ri+1, gi, bi+1) + 1LL * r[ri] * b[bi]);
    // (g, b)
    if(gi < ng && bi < nb)
        ret = max(ret, f(ri, gi+1, bi+1) + 1LL * g[gi] * b[bi]);
    return ret;
}
void process(){
    cin >> nr >> ng >> nb;
    r.resize(nr); g.resize(ng); b.resize(nb);
    rep(i, 0, nr) cin >> r[i]; sort(r.rbegin(), r.rend());
    rep(i, 0, ng) cin >> g[i]; sort(g.rbegin(), g.rend());
    rep(i, 0, nb) cin >> b[i]; sort(b.rbegin(), b.rend());

    memset(dp, -1, sizeof(dp));

    cout << f(0, 0, 0) << endl;
}

int main(){
    process();
    return 0;
}
```
