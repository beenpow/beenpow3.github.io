---
title : COFO::CodeCraft-20 ( div 2 )

categories:
    - sweaB
    - COFO
tag:
    - sweaB
    - COFO
---
# COFO::CodeCraft-20 ( Div 2 )
- [Link : COFO::CodeCraft-20 ( div 2 )](https://codeforces.com/contest/1316/problem/D)
- 정말 어려운 그래프 탐색 문제입니다.
- 하지만, 관점을 다르게 생각하는 면에서 좋은 문제라고 생각합니다.

## Problem D : Nash Matrix

- level : 2000
- tag : constructive algorithms, dfs and similar, graphs, implementation

### Point
- 특이한 형태의 입력이 주어집니다.
- map의 사이즈 n이 주어집니다.
- 이후, n * n map이 주어집니다.
  - 이떄 map의 원소는 x, y pair로 나타낼 수 있습니다.
  - 또한 이 수의 범위는 다음과 같습니다.
  - (1<=x<=n, 1<=y<=n or (x = -1, y = -1))
  - 이 pair는 해당 i, j에서 출발하면 도착하는 곳의 위치를 나타냅니다.
  - 또한, 무한루프에 빠지는 경우엔 -1, -1이 주어집니다.

### Design(x)
- 문제에서 캐치해야할 점이 많습니다만 영어라서 그런지 놓치는 점이 항상 많습니다.
- (-1,-1)에 대한 처리로 고민을 꽤 많이 했습니다.
  - 하지만, 이내 무한루프에 빠진 정점의 갯수는 무조건 짝수라는 사실을 알아냈습니다.
  - 그리고, 주먹구구 식으로 구현하다보니 dfs함수만 3개를 사용했습니다.
  - 하지만, 이렇게 푸는 경우 예외처리를 확실히 하지 못하고, 이와 같은 강력한 문제에서 AC를 받을 수 없는 것 같습니다.

- editorial에서는 다음과 같이 풀이합니다.
  - (-1,-1)인 그룹과, 그렇지 않은 그룹으로 나눕니다.
  - 저는 (-1,-1)인 정점의 갯수가 짝수라는 사실만 발견하고 말았지만, 사실상 짝수이므로 2개씩 짝지을 수 있습니다.
  - 무한 루프 내의 모든 정점은 짝수, 즉 연결되어 있으므로 둘씩 짝지어도 각각 무한루프에 빠지게 됩니다.
  - 따라서, 현재 정점이 (-1,-1)이고, 상/하/좌/우 정점 중 (-1,-1)이 있다면
    - 현재 정점이 해당 방향을 바라보게 해주면 됩니다.
    - 이때 주의할 점은, 상/하/좌/우 중 하나에 해당하는 다음정점은 이미 방문했고 누군가와 짝을 이뤘을 수 있습니다.
    - 하지만, 중요한 점은 이미 무한루프가 존재하고, 현재 정점이 그 무한루프 방향을 바라본다면 같이 묶어질 수 있다는 점입니다.
    - (-1,-1)이 아닌 정점들은 다음과 같이 탐색을 합니다.
      - 현재 정점이 'X'인 지점에서 출발을 시작합니다.
      - 그리고, 4방향 dfs를 시작합니다.
      - 이때, 현재 정점과 다음 정점의 도착점이 같은 경우에만 dfs를 진행시켜주는데요.
      - 그 이유는 다음과 같습니다.
      - 무한 루프에 빠지지 않은 정점들은 어딘가에 멈춰서야 합니다.
      - 이때, 멈춰설 수 있는 지점은 'X'인 지점뿐입니다.
      - 따라서, 'X'의 갯수이하만큼으로 그룹을 나눌 수 있겠습니다.
      - 즉, 같은 그룹에 속하는(도착점이 같은) 정점들의 방향을 정해주는 역할을 합니다.


#### Big O(time)
- O(N * N)
#### Big O(memory)

### Code(x)

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define se second
#define fi first
#define pb push_back
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
using namespace std;
const int MAXN = 1000 + 1;

int n;
char mat[MAXN][MAXN];
int x[MAXN][MAXN], y[MAXN][MAXN];
void input(){
    cin >> n;
    rep(i, 1, n+1){
        rep(j, 1, n+1){
            int u, v; cin >> u >> v;
            x[i][j] = u, y[i][j] = v;
        }
    }
}
bool connected(int r, int c, int q, int s, char d, char nd){
    if(x[q][s] == -1){
        mat[r][c] = d;
        if(mat[q][s] == '\0') mat[q][s] = nd;
        return true;
    }else return false;
}
void dfs(int r, int c, char d){
    if(mat[r][c]) return;
    mat[r][c] = d;
    if(x[r][c] == x[r+1][c] && y[r][c] == y[r+1][c])
        dfs(r+1, c, 'U');
    if(x[r][c] == x[r-1][c] && y[r][c] == y[r-1][c])
        dfs(r-1, c, 'D');
    if(x[r][c] == x[r][c+1] && y[r][c] == y[r][c+1])
        dfs(r, c+1, 'L');
    if(x[r][c] == x[r][c-1] && y[r][c] == y[r][c-1])
        dfs(r, c-1, 'R');
}
void process(){
    input();
    rep(i, 1, n+1){
        rep(j, 1, n+1){
            if(x[i][j] == -1){
                bool found = (mat[i][j] != '\0');
                if(!found) found = connected(i, j, i, j+1, 'R', 'L');
                if(!found) found = connected(i, j, i, j-1, 'L', 'R');
                if(!found) found = connected(i, j, i+1, j, 'D', 'U');
                if(!found) found = connected(i, j, i-1, j, 'U', 'D');
            }else{
                if(x[i][j] == i && y[i][j] == j)
                    dfs(i, j, 'X');
            }
        }
    }
}
void check(){
    rep(i, 1, n+1) rep(j, 1, n+1) if(mat[i][j] == '\0'){
        cout << "INVALID" << endl;
        return;
    }
    cout << "VALID" << endl;
    rep(i, 1, n+1){
        rep(j, 1, n+1){
            cout << mat[i][j];
        }cout << endl;
    }
}
int main(){
#ifdef DEBUG
    freopen("input.txt", "r", stdin);
#endif
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    process();
    check();
    return 0;
}
```
