---
title : COFO EDU::Round #84

categories:
    - sweaB
    - COFO
tag:
    - sweaB
    - COFO
---
# COFO EDU::Round #84
- [Link : COFO EDU::round 84] (https://codeforces.com/contest/1327)
- 두 번째 코포 라운드였습니다.
- 2개 풀이를 목표로 하였지만, greedy 하게 생각하지 못한 탓에 하나밖에 풀지 못하였습니다.
- A, B, C 모두 이리저리 짜보았지만 B만 맞았습니다.
  - A와 C의 풀이법이 매우 재밌습니다.

## Problem A : Sum of Odd Integers

- level :
- tag

### Point
- 좋은 수학 문제입니다.

### Key

### Understanding(x)
- n과 k가 주어집니다.
- 중복되지 않는 k개의 홀수의 합으로 n을 만들 수 있으면 "YES", 
  - 그렇지 못하면 "NO" 를 출력합니다.

### Design(x)
- 해당 문제는, 2가지 수학적 접근을 통해 풀 수 있습니다.
> 첫 번째
- n과 k는 둘 다 홀 수이거나, 둘 다 짝수인 경우에만 Yes를 만들 수 있습니다.
- n이 짝수이고 k가 홀수라면, k개의 홀수의 합으로 짝수를 만들라는 말인데, 성립하지 않습니다.(vice
  versa)

> 두 번째
- 정말 오랫동안 이해가 되지않던 성립조건입니다.
- 1부터 k개의 등차가 2인 등차수열이 있다고 생각해봅시다.
- 이때 k개의 등차수열의 합은 아래와 같이 표현할 수 있습니다.
- 1 + 3 + 5 + ... + 2 * k - 1
- 이를 등차수열의 합 공식을 이용해 아래와 같이 표현할 수 있습니다.
- a는 첫항, d는 공차를 의미한다고 할때,
  - S_k = k{2 * a + (k-1) * d}/2
  - 해당 등식을 d = 2이고 a = 1인 1부터 시작하는 등차가 2인 수열에 대입해본다면,
  - S_k = k * k 가 됩니다.
- 이를 통해 k * k <= n 이 만족해야 "YES"를 출력할 수 있습니다.
- 이제, 해당 문장을 해석해보겠습니다.
  - S_k는 1부터 k개의 합을 의미합니다.
  - 즉, 1부터 시작한다는 말은 가장 작은 합을 의미합니다.
  - n과 k가 주어졌을때, 가장 작은 k개의 합이 n보다 크면 당연히 불가능하겠지요.
  - 그럼 위의 한 문장은 이해가 되지만, 어떻게 " k * k <= n " 이 공식이 문제의 필요충분 조건일까요?
  - 손으로 조금 예를 들면 이해가 쉽습니다.
  - 11 3 이 주어졌다고 해봅시다.
    - 가장 작은 수 3개는 1 + 3 + 5 입니다.
    - 하지만 합이 9밖에 되지 않습니다.
    - 저희는 11을 만들어야 합니다.
    - 그렇다면 5와 7을 swap해버리면 됩니다.
    - 그럼, 1 + 3 + 7이 되어서 성립하게 됩니다.
  - 22 4 가 주어졌다고 해봅시다.
    - 가장 작은 수 4개는 1 + 3 + 5 + 7 입니다.
    - 하지만 합이 16밖에 되지 않습니다.
    - 22 - 16 = 6 이므로, 8이 부족한 상태입니다.
    - 그렇다면 1, 3, 5, 7 중에 아무 수나 골라서 총합이 6이 되도록 더 큰 수와 swap해주면 됩니다.
    - 따라서 답은 여러개가 될 수 있습니다.
    - 가장 간단한 예로, 1 + 3 + 5 + 13 가 될 수 있습니다.
    - 또한, 1 + 3 + 7 + 11 도 될 수 있구요.

- 즉, 첫번째 조건을 통해 짝/짝 혹은 홀/홀의 조건을 이미 맞추어 두었고, 그런 상태에서 k * k <= n 이
  성립한다면, 최소합과 n은 2의 배수 차이가 나므로, 공차가 2인 등차수열에서 충분히 swap가능한 상태가
  됩니다.


#### Big O(time)

#### Big O(memory)

### Review, after coding in manual way

### Code(x)

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
using namespace std;

int main(){
    //freopen("input.txt", "r", stdin);
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int tc;
    cin >> tc;
    while(tc--){
        long long n, k;
        cin >> n >> k;
        if((n%2 == k%2)  && k * k <= n) cout << "YES"<<endl;
        else cout << "NO"<<endl;
    }
    return 0;
}
```

#### functions

#### updated values

### Debugging(x)

### Good code

### Optimization

## Problem B : Princesses and Princes

- level :
- tag

### Point
- 문제의 길이에 비해 생각보다 구현은 간단한 시뮬레이션 문제입니다.
- 단지, 설명이 길고 그림도 있어서 집중하지 않으면 전체를 이해하기 쉽지않습니다.

### Key

### Understanding(x)
- n명의 공주가 있습니다.
- n명의 왕자가 있습니다.
- 각 공주는 마음에 드는 왕자들이 있습니다.
- 1번 공주부터 규칙에 따라서 왕자를 선택합니다.
  - 규칙은, 낮은 인덱스의 왕자가 아직 짝이 없으면 짝을 맺는다 입니다.
  - 모든 왕자를 다 스캔했는데, 모두 짝이 있으면 해당 공주는 짝을 만날 수 없습니다.
- 위의 규칙을 따라서 시뮬레이션을 진행하고 나면, 2가지 경우의 결과가 있을 수 있습니다.
  - 하나는, 모든 공주와 모든 왕자가 짝이 있는 것입니다.
    - 이때는, "OPTIMAL"을 출력합니다.
  - 두 번째는, 짝을 만나지 못한 공주와 왕자가 존재하는 것입니다.
    - "IMPROVE"를 출력합니다.
    - 이때, 가장 작은 인덱스의 공주와 가장 작은 인덱스의 왕자를 출력합니다.

### Design(x)
- 이해 챕터에 써 놓은대로 구현하면 되는 시뮬레이션 문제입니다.
- 단지, given과 taken 변수를 통해 각 공주나 왕자가 짝이 있는지를 표시하였습니다.

#### Big O(time)

#### Big O(memory)

### Review, after coding in manual way

### Code(x)

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
const int MAXN = 100000+1;
using namespace std;
 
int n;
vector<int> adj[MAXN];
bool given[MAXN];
bool taken[MAXN];
void simulate(){
    memset(taken, false, sizeof(taken));
    memset(given, false, sizeof(given));
    int coupled = 0, givenIdx = -1;
    rep(i, 1, n+1){
        rep(j, 0, adj[i].size()){
            int man = adj[i][j];
            if(taken[man] == false){
                coupled++;
                taken[man] = true;
                given[i] = true;
                break;
            }
        }
        if(given[i] == false && givenIdx == -1) givenIdx = i;
    }
    if(coupled == n)cout << "OPTIMAL"<<endl;
    else{
        cout << "IMPROVE" << endl;
        rep(i, 1, n+1) if(taken[i] == false){
            cout << givenIdx << " "<<i << endl;
            break;
        }
    }
}
int main(){
    //freopen("input.txt", "r", stdin);
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int tc;
    cin >> tc;
    while(tc--){
        cin >> n;
        rep(i, 1, n+1){adj[i].clear();
            int cnt; cin >> cnt;
            rep(j, 0, cnt){
                int x; cin >> x;
                adj[i].push_back(x);
            }
        }
        simulate();
    }
    return 0;
}
```


#### functions

#### updated values

### Debugging(x)

### Good code

### Optimization
~

## Problem C : Game with Chips

- level :
- tag

### Point
- 정말 재밌는 greedy 문제입니다.
- 문제에 주어진 n, m, k는 매우 크다는 것을 파악할 수 있습니다.
- 단순히 경로탐색 문제로 인식하면, 시간초과는 뻔하다는 것을 알 수 있습니다.
- 따라서 문제에서 주어진 하나의 힌트를 찾아내는 것이 관건입니다.
  - 최대 2nm번의 액션을 취할 수 있다.(L,R,U,D)
  - n * m 사이즈의 맵에 k개의 구슬이 랜덤한 위치에 놓여져 있다고 합시다.
  - L, R, U, D 중 하나를 선택해 액션을 취하면, 모든 구슬은 해당 방향으로 한 칸 움직입니다.
    - 이때 이동하려는 방향에 벽이 있으면 이동하지 않습니다.
  - 2nm번 내에, 각 구슬들의 destination위치를 지나가도록 하는 것이 목표입니다.

- 해당 문제는 greedy하게 접근이 가능하고, 그렇게 풀어야만 풀리는 n,m,k 사이즈를 가집니다.
- 보통 수가 크면 greedy 혹은 dp인 것 같습니다.
- k개의 구슬이 맵에 랜덤하게 산재해 있습니다.
  - 먼저, 'U'를 n번 진행해줘서 모든 구슬을 1행에 모이도록 합니다.
  - 그리고, 'L'을 m번 진행해줘서 모든 구슬을 1행 1열에 모을 수 있습니다.
    - 문제에서, 구슬의 위치는 겹칠 수 있다고 하였기 때문이죠
  - 그럼이제, (1,1)부터 (N,M)까지 모든 정점을 방문해보면 최대 2nm내에 문제를 풀 수 있다는 것을 알 수
    있습니다.
  - (1,1)부터 행을 하나씩 늘려가며,
    - 'R'방향으로 m번 움직이고, 'D'방향으로 한번 움직이고,
    - 'L'방향으로 m번 움직이고, 'D'방향으로 한번 움직이고,
  - 위 과정을 반복하면, 따지고 잴 것도 없이 모든 구슬이 모든 정점을 지나치므로 당연히 답이 되게
    됩니다.
  - 문제에도 나와있듯이, 최소한의 이동으로 하라는 말이 없기 때문에 가능한 방법입니다.


### Key

### Understanding(x)
- 시사점 챕터에 충분히 설명해두었습니다.

### Design(x)

#### Big O(time)

#### Big O(memory)

### Review, after coding in manual way

### Code(x)

```cpp
#include<bits/stdc++.h>
#define LE 'L'
#define RI 'R'
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
using namespace std;

int main(){
    freopen("input.txt", "r", stdin);
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int n, m, k; int x, y;
    cin >> n >> m >> k;
    rep(i, 0, k)cin >> x >> y;

    cout << (n+m) + (m-1) * n + 1 * (n-1) << endl;

    // 'U', n times
    rep(i, 0, n) cout <<'U';
    // 'L', m times
    rep(i, 0, m) cout <<'L';

    char seq[]={'R','L'};
    for(int i = 0; i < n; i++){
        rep(j, 0, m-1) cout << seq[i%2];
        if(i != n-1) cout<<'D';
    }
    return 0;
}
```

#### functions

#### updated values

### Debugging(x)

### Good code

### Optimization
~
