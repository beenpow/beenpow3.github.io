---
title : COFO::Educational Round xxx

categories:
    - COFO
    - 코드포스
tag:
    - COFO
    - 코드포스
---
# COFO::Educational Round #xxx
- [Link : COFO::](x)
- solved : 
  - A :  ( 00:11 )
  - rank : 
  - score :

## Problem A : String Similarity

- tag : brute force, constructive algorithms, strings
- time : 00:11

### Point
- n이 주어집니다.
- 2n-1 길이의 binary string s가 주어집니다.
- 이때, 조건을 만족하는 string w를 출력합니다.
- 조건은 다음과 같습니다.
  - s의 모든 substring은 s와 similar해야합니다.
  - 여기서 similar의 의미는, 
    - 해당 s의 substring의 원소와 s의 원소중 공통된 원소가 1개라도 있어야 similar하다고 말합니다.

### Design
- substring을 모두 찾고 이것들을 행으로 배치해봅니다.
- 이후, 1번째 서브스트링에서의 첫번째 원소를 취하고
- 2번째 서브스트링에서는 두번째 원소를 취하고
- ... 를 반복합니다.
- 이는 곧 s의 짝수번째 원소만 취하는 것을 의미하고, 이를 출력합니다.
- 혹은, s[n-1]원소를 n번 출력해도 됩니다.
  - n-1번째 원소는 모든 substring에 포함됩니다.

#### Big O(time)
- O(N)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define pb push_back
#define f first
#define s second
#define all(v) (v).begin(), (v).end()
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
#define vi vector<int>
#define vll vector<ll>
#define vpi vector<pair<int, int> >
typedef long long ll;
const int MAXN = 100000 + 10;
#define MOD 1000000007
using namespace std;
int n;
string s;
void process(){
    cin >> n >> s;
    for(int i = 0; i < s.size(); i += 2){
        cout << s[i];
    }
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

## Problem B : RPG Protagonist

- tag : binary search, brute force, greedy, math
- brute force 아이디어가 떠올랐지만 greedy하게 생각하지 못하여 해결하지 못했습니다.
  - f 와 p 각각이 brute force를 해야한다고 생각하여 O(N^2)으로 생각했었습니다.
  - 하지만, 둘 중 한명만 brute force를 해주고,
  - 다른 한명은 가벼운 무게의 무기를 가장 많이 택하면 되었습니다.
    - 이미 다른 한 명은 완전 탐색이니 신경쓸 것이 없고, 이제 본인만 처리하면 되는데, 갯수가 많으려면 가벼운 것을 많이 들어야 하기 때문입니다.

### Point
- 2명의 사람 f, p가 등장합니다.
- 각 사람이 carry 할 수 있는 총 무게가 주어집니다.
- 2가지의 무기 a, b가 등장합니다.
  - 가벼운 무기를 a라 하겠습니다.
- 각 무기의 갯수와 무게가 주어집니다.
- 이때, 2명은 최대 갯수의 무기를 carry하고 싶습니다.
- 최대 갯수를 출력합니다.

### Design
- 1명을 brute force로 fix하면, 다른 1명은 얼마나 들지 정해집니다.
- f의 a를 brute force합니다.
  - 그럼 자동으로 f가 들 수 있는 b의 최대 갯수가 정해집니다.
- 이때, p를 위한 brute force를 따로 돌리는 것이 아니라, p는 가벼운 a를 최대한 많은 갯수 들도록 O(1)에 구해줍니다.
- 이를 통해 최대 carry 갯수를 구할 수 있습니다.

#### Big O(time)
- O(N)

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
typedef long long ll;
const int MAXN = 100000 + 10;
#define MOD 1000000007
using namespace std;

ll p, f; // p < f
ll cnts, cntw;
ll wes, wew; // wes < wew
void process(){
    cin >> p >> f >> cnts >> cntw >> wes >> wew;
    if(p > f) swap(p, f);
    if(wes > wew){
        swap(cnts, cntw);
        swap(wes, wew);
    }
    ll ans = 0;
    // brute force
    rep(si, 0, cnts+1){
        ll curp = p, curf = f, curcnts = cnts, curcntw = cntw;
        ll cntAll = si;
        curcnts -= si;
        curp -= si * wes;
        if(curp < 0) break;
        
        ll takew = min(curp / wew, curcntw);
        cntAll += takew;
        curp -= takew * wew;
        curcntw -= takew;
        
        ll takes = min(curf / wes, curcnts);
        cntAll += takes;
        curf -= takes * wes;
        curcnts -= takes;
        
        takew = min(curf / wew, curcntw);
        cntAll += takew;
        curf -= takew * wew;
        curcntw -= takew;
        
        ans = max(ans, cntAll);
    }
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

## Problem C : Binary String Reconstruction

- tag : 2-sat, constructive algorithms, greedy, strings
- 문제이해도 제대로 못하였지만, 조건을 만족시켜주는 능력이 부족하다는 점을 배웠습니다.

### Point
- string s와 integer x가 주어집니다.
- w를 출력합니다.
- string s는 w를 토대로 만들어진 배열이고, 다음의 조건을 만족합니다.
  - 조건1) 만약 w_i-x가 존재하고, 그 값이 1이라면, si의 값은 1입니다.
  - 조건2) 만약 w_i+x가 존재하고, 그 값이 1이라면, si의 값은 1입니다.
  - 조건3) 만약 위의 두 조건을 모두 만족하지 않는다면 si의 값은 0 입니다.
- 만약 이러한 w가 존재하지 않는 경우 -1 을 출력합니다.

### Design
- w->s 이고, w를 구하는 문제입니다.
  - 라운드 진행당시엔 반대로 생각했습니다.
  - 시간이 촉박해도 문제와 변수를 꼼꼼히 읽어야겠습니다.
- 코드는 가장 직관적인 cheetose님의 코드를 보고 이해하였습니다.
- 문제에서 이해해야할 가장 중요한 부분은 다음과 같습니다.
  - s[i]의 값이 1인 경우는, w[i-x] 혹은 w[i+x] 중 1이 있기 때문입니다.
  - 즉, 양쪽 모두도 될 수 있고, 한 쪽만 될 수도 있습니다.
  - 따라서, 생각의 범위를 좁히기 위해 모두 1로 초기화 해준 후에, s[i]의 값이 0인 경우만 처리해줍니다.
  - s[i]의 값이 0인 경우는 명백합니다.
    - w[i-x] 와 w[i+x] 에 0을 넣어주면 됩니다.
  - 그리고 이제, s[i]의 값이 1인 부분을 통해 w[i-x]와 w[i+x]중 1이 있는지 확인해주는 로직입니다.
- 저는 위와 같이 깔끔한 로직을 세우지 못하여, 이리저리 명쾌하지 않은 답만을 구하려 하였습니다.
- 문제 이해가 안될수록, 난해해보일수록 명확히 로직을 세우고 코딩하는 것이 백번 맞습니다.

#### Big O(time)
- O(N)

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
typedef long long ll;
const int MAXN = 100000 + 10;
using namespace std;

int x, n;
char s[MAXN];
int w[MAXN];
void process(){
    memset(w, -1, sizeof(w));
    memset(s, 0, sizeof(s));
    
    cin >> (s+1) >> x;
    n = (int) strlen(s+1);
    rep(i, 1, n+1) w[i] = 1;
    rep(i, 1, n+1){
        if(s[i] == '0'){
            if(i-x >= 1) w[i-x] = 0;
            if(i+x <= n) w[i+x] = 0;
        }
    }
    rep(i, 1, n+1){
        if(s[i] == '1'){
            bool f = true;
            if(i-x >= 1 && w[i-x] == 1) f = false;
            if(i+x <= n && w[i+x] == 1) f = false;
            if(f){
                cout << "-1" << endl;
                return;
            }
        }
    }
    
    rep(i, 1, n+1) cout << w[i];
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

## Problem D : Zigzags

- tag : brute force, data structures, dp, math, two pointers
- galen colin의 설명이 매우 유익합니다.
  - 이런 유형의 문제는 보통 특정 인덱스 or 특정 i-th 를 고정시키고 풀어야 한다고 알려줍니다.
  - 또한 해당 문제를 풀이하는 방법이 매우 신기하고 배울점이 많다고 생각합니다.
- [Link::galen_colin](https://www.youtube.com/watch?v=bQaLxT38TH8)

### Point
- n이 주어집니다.
- n개의 원소로 이루어진 배열 a가 주어집니다.
- 이때 다음을 만족하는 match의 갯수를 출력합니다.
  - 1 <= i < j < k < l <= n 이고, ai == ak && aj == al

### Design
- 문제는 매우 짧고 명확하지만, 풀이가 쉽지 않습니다.
- colin이 설명하는 재밌는 투포인터 방법을 설명합니다.
- 그는 먼저, 이런 류의 문제는 대게 특정 숫자 혹은 i-th 인덱스를 고정해야한다고 말해줍니다.
- 1, 2, 3, 4 네 개의 숫자 중 1번째와 3번째 숫자를 고정하는 것이 가장 현명합니다.
  - 둘은 같은 숫자이고, 이 경우 O(N^2)에 문제를 풀 수 있기 때문입니다.
- i를 1번째 숫자라고 하고,  k를 3번째 숫자라고 하고, O(N^2) brute force를 돌립니다.
  - 이때, O(1)에 현재 상황에서 가능한 match의 수를 구할 수 있습니다.
  - lcnt와 rcnt라는 배열을 통해, k기준으로 좌측(i부터 k사이)와 우측(k보다 우측)에 존재하는 수들을 관리해줍니다.
  - 즉, lcnt는 2번째 숫자를 위한 pool 이고, rcnt는 4번째 숫자를 위한 pool입니다.
  - 3번째 숫자를 고정함에 따라, 
    - 현재 k는 우측 pool에서 제외시켜 주고, (--rcnt[ak]])
    - 이때 해당 숫자와 좌측 pool과 match되어있던 숫자도 감소시켜줍니다. ( -lcnt[a[k]] )
    - 이제 3번째 숫자는 처리하였고, 기존에 3번째 숫자였던 수를 2번째 숫자 pool에 추가해줍니다.
    - 이때 해당 숫자와 같은 수를 가지는 수가 4번째 수 pool에 존재하면 카운팅해준 후, 해당 숫자는 좌측 pool에 옮겨줍니다.
    - 이렇게 진행하며, 현재 match의 수인 nmatch를 관리하고, 1번째 숫자와 3번째 숫자가 같은 수일때만 정답에 카운팅해주는 로직입니다.

#### Big O(time)
- O(N^2)

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
typedef long long ll;
const int MAXN = 3000 + 10;
using namespace std;

int n;
int a[MAXN];
ll lcnt[MAXN], rcnt[MAXN];
void process(){
    cin >> n;
    rep(i, 0, n){
      cin >> a[i];
      a[i]--;
    }
    
    ll ans = 0;
    rep(i, 0, n){
        // init
        rep(j, 0, n) lcnt[j] = rcnt[j] = 0;
        rep(j, i+1, n) ++rcnt[a[j]];
        
        ll nmatch = 0;
        // nmatch : 현재 상태에서 연결가능한 두번째 숫자와 네번째 숫자의 match 갯수
        // lcnt[] : 2번째 숫자의 pool
        // rcnt[] : 4번째 숫자의 pool
        rep(k, i+1, n){
            // third one
            nmatch -= lcnt[a[k]]; // a[k]는 이제 3번째 숫자가 되므로, 해당 숫자가 2번째 숫자의 pool에 있는 수와 match되어 있는 갯수만큼 빼줌.
            --rcnt[a[k]]; // a[k]는 이제 3번째 숫자가 되므로, 4번째 숫자의 pool에서 하나 제거해줘야함.
            
            if (k > i + 1) {
                // second one
                nmatch += rcnt[a[k - 1]]; // a[k-1]은 원래 3번째 숫자였는데, 이제 2번째 숫자 pool에 들어가게 됨.
                                          // 따라서, 해당 숫자와 match될 수 있는 4번째 숫자 pool에 있는 갯수만큼 더해줌.
                ++lcnt[a[k - 1]]; // a[k-1]을 2번째 숫자 pool로 옮겨줌.
            }
            
            if (a[i] == a[k]) ans += nmatch;
        }
    }
    
    cout << ans << '\n';
}
int main(){
    freopen("input.txt", "r", stdin);
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int tc; cin >> tc;
    while(tc--)
        process();
    return 0;
}
```
