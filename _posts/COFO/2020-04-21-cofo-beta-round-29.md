---
title : COFO::Beta Round 29 ( div 2 )

categories:
    - sweaB
    - COFO
tag:
    - sweaB
    - COFO
---
# COFO::Beta Round 29 ( div 2 )
- [Link : COFO::Beta round 29 ( div 2 ) ](https://codeforces.com/problemset/problem/29/C)
- [Link :MarioYC](https://codeforces.com/contest/29/submission/3858618)
- 처음 발견한 좋은 좌표압축 방법이 있어서 블로그합니다.

## Problem C : Mail Stamp

- level : 1700
- tag : data structures, dfs and similar, graphs, implementation
- time :

### Point
- 좋은 hashing 및 좌표압축 문제입니다.
- n개의 노드가 있습니다.
- 맨끝 노드 두개를 A, B라고 했을때, A에서 B로 가는 경로는 하나 뿐입니다.
- 또한, 일렬로 연결되어있다고 생각할 수 있습니다.

### Design(x)
- 정점 u, v쌍이 n개 주어집니다.
- 1 <= n <= 10^5, 정점 x 의 범위 : [1, 10^9]
- 따라서, 좌표압축이 명백히 필요합니다.
- lower_bound 및 sort등의 접근을 해보았지만,
- Logical하게 명백한 설계를 하지 않고 키보드를 두드리는 행위는 오히려 시간만 잡아먹는 것 같습니다.
- 어떻게 하면 u와 v모두 key값으로 사용될 수 있을까를 고민하였습니다. ( map 사용 )
- 해당 대회 상위권 랭커의 풀이가 매우 로지컬하여 해당 코드를 보고 이해하였습니다.

- map<int,int> id, rev; 이렇게 두개의 map을 사용합니다.
  - 이후, 정점 u에 대하여
  - id[u] = cnt;
  - rev[cnt] = u;
  - cnt++;
  - 위의 과정을 진행하여 줍니다.
  - 즉, id에서는 값으로 해당 값의 인덱스를 찾을 수 있고,
  - rev에서는 인덱스로 해당 인덱스의 값을 찾을 수 있습니다.
  - 즉, 좌표압축이 가능해집니다.
- 따라서, 압축된 인덱스를 adj[i]에 넣어주고, 시작점을 찾은 후 dfs를 돌려주면 됩니다.

#### Big O(time)

#### Big O(memory)

### Code(x)

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define se second
#define fi first
#define pb push_back
#define rep(i,a,b) for(int i=a;i<b;i++)
using namespace std;
const int MAXN = 100 * 1000+2;

int n;
map<int,int> id, rev;
vector<int> adj[MAXN];
vector<int> ans;
void input(){
    cin >> n;
    int cnt = 0;
    rep(i, 0, n){
        int u, v; cin >> u >> v;
        if(id.find(u) == id.end()){
            id[u] = cnt;
            rev[cnt] = u;
            ++cnt;
        }
        if(id.find(v) == id.end()){
            id[v] = cnt;
            rev[cnt] = v;
            ++cnt;
        }
        u = id[u];
        v = id[v];
        adj[u].pb(v);
        adj[v].pb(u);
    }

}

void process(){
    input();
    int pos = -1;
    rep(i, 0, n+1) if(adj[i].size() == 1){
        pos = i;
        break;
    }
    int prev = -1;
    rep(i, 0, n+1){
        cout << rev[pos] << " ";
        rep(j, 0, (int)adj[pos].size()){
            if(adj[pos][j] != prev){
                prev = pos;
                pos = adj[pos][j];
                break;
            }
        }
    }
    cout << endl;
}
int main(){
#ifdef DEBUG
    freopen("input.txt", "r", stdin);
#endif
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    process();
    return 0;
}
```

