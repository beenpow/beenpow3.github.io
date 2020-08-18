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

- level : 800
- tag : geometry, math
- time : 00:06

### Point
- n이 주어집니다.
- n개의 원소로 이루어진 배열이 주어집니다.
- 주어진 원소들 중 3개를 뽑아서, 삼각형을 만들 수 있다면 그 원소들의 인덱스를 출력하고
- 불가능한 경우 '-1'을 출력합니다.

### Design
- 세 변 a <= b <= c 가 있을때, 다음 조건을 만족해야 삼각형을 구성할 수 있습니다.
- c > a + b 
- 이를 만족시키기 위해, 먼저 배열을 정렬합니다.
- 이후, 가장 작은 두 수를 a, b라 하고, 가장 큰 수를 c라고 합니다.
- 그리고 위의 부등호관계가 성립하는지 확인하면 됩니다.

#### Big O(time)
- O(n)

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

- level : 800
- tag : games, greedy, sortings
- time : 00:14

### Point
- 0과 1로만 구성된 binary string s가 주어집니다.
- Alice와 Bob의 턴이 돌아가며 게임이 진행됩니다.
- 자신의 차례가 오면 다음의 행동을 할 수 있습니다.
  - 연속된 같은 문자들을 원하는 만큼 삭제할 수 있습니다.
- 다음의 방법으로 스코어가 정해집니다.
  - 삭제한 '1'의 갯수 하나당 1점을 받습니다.
- 이후, Alice가 얻은 점수를 출력합니다.
- 또한, Each player는 자신의 스코어를 최대화하려고 합니다.

### Design
- 해당 문제에서 가장 중요한 포인트는 'optimal' 이라고 생각합니다.
- 각 플레이어가 자신의 점수를 최대로 만드는, 즉 optimal한 선택을 해야합니다.
- 이를 통해 아이디어를 얻을 수 있습니다.
- 결국 연속된 1의 갯수를 vector에 넣어주고
- 크기 순으로 정렬한 후 돌아가며 할당받으면 됩니다.
  - '0'은 점수에 영향이 없고, 각자가 optimal한 선택을 하므로
  - 1이 연속 3개인데, 2개만 제거하는 경우는 없습니다.

#### Big O(time)
- O(NlogN)

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

- level : 1600
- tag : data structures, dp, math
- 문제에서 요구하는 바를 손으로 그려보는 것도 중요하지만,
- 이런 문제의 경우 수식으로도 요구하는 바를 표현해보는 것도 중요하므로 익숙해져야겠습니다.

### Point
- n이 주어집니다.
- 길이 n인 string s가 주어집니다.
- s의 원소 s[i] 는 0과 9사이의 값을 갖습니다.
- 이때 s의 subarray 중, subarray의 길이와 원소의 합이 같은 경우의 수를 출력합니다.

### Design
- prefix, suffix에 제가 매우 약한 것 같습니다.
- 이번 문제는 문제에서 요청하는 바를 수식으로 변환해야 눈에 보이는 문제입니다.
  - 물론 잘하시는 분들은 'prefix sum' 이라고 하시며 술술 푸십니다.
- naive하게는 구간 [l,r]에 대한 합을 구해야 하므로 O(N^2)이 걸리고 TLE를 받습니다.
- 문제에서 주어진 부분을 수식으로 변환하여 풀이합니다.
- 구간의 합이 구간의 길이와 같아야 하며, prefix sum을 배열 p라고 합시다.
- 그럼 다음과 같은 수식이 성립합니다.
  - p[r] - p[l] = r - l
  - 위의 수식은 다음과 같이 변형할 수 있습니다.
  - p[r] - r = p[l] - l
- 위의 수식을 이제 코드로 적용시켜 보면 다음과 같습니다.

```cpp
    map<int, int> mp;
    ll ans = 0;
    rep(i, 0, n+1)
        ans += mp[psum[i] - i]++;
```

- 저도 이해하는데 시간이 꽤 걸렸기에 자세하게 설명하려 합니다.
  - O(N^2)을 O(N)으로 줄이는 것이 어떻게 가능한지 이해가 되지 않았었습니다.
- i를 0부터 n까지 순회합니다.
  - psum[i] - i의 값을 이전에 사용한 적이 있다면 그 값을 ans에 더합니다.
  - mp의 갯수를 하나 증가시켜줍니다.
- 일단, i가 0부터 n까지 순회한다는 것은 l부터 r까지 순서에 역행하지 않음을 알 수 있습니다.
- 따라서,
  - int t = psum[i] - i 
  - t의 값을 가진 적이 이전에 3번 있었다고 하고, 현재 i가 5라고 해봅시다.
  - 같은 t의 값을 가진 인덱스가 1, 3, 4였다면,
  - 이번에 같은 t를 가진 인덱스 5를 찾았으므로 답에 3을 더해줘야 합니다.
  - 그 이유는 다음과 같습니다.
  - 구간 [1,5], [3,5], [4,5]가 새로 만족하는 쌍이기 때문입니다.
- 즉, p[r] - r = p[l] - l 이므로 같은 값을 갖는 해당 식의 결과쌍의 갯수를 카운트해주는 것입니다.

#### Big O(time)
- O(nlogn)

#### Big O(memory)

### Code

```cpp
// https://beenpow.github.io/
#include<bits/stdc++.h>
#define endl '\n'
#define pb push_back
#define all(v) (v).begin(), (v).end()
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
typedef long long ll;
const int MAXN = 1e5 + 10;
using namespace std;

int n;
int psum[MAXN];
string s;
map<int,int> mp;
void process(){
    mp.clear();
    cin >> n >> s;
    s = '#' + s;
    rep(i, 1, n+1) psum[i] = psum[i-1] + (s[i] - '0');
    
    ll ans = 0;
    rep(i, 0, n+1){
        ans += mp[psum[i] - i]++;
    }
    cout << ans << endl;
}
int main(){
    int tc; cin >> tc;
    while(tc--)
        process();
    return 0;
}
```

## Problem D : Colored Rectangles

- level : 1800
- tag : dp, greedy, sortings
- 정답과 거의 유사한 방법으로 풀었지만 가장 큰 차이점이 있었고, 이번 문제를 통해 해당 부분을 배우게 되었습니다.
- dp에는 크게 2가지 종류가 있습니다.
  - dp[i][j][k] : [0][0][0]에서부터 [i][j][k]까지 오면서 쌓인 값들을 저장
    - 일반적인 방법입니다.
    - [0][0][0] 에서 [1][0][0] 으로 가는 경우가 한가지만 존재하거나, 식으로 바로 정의할 수 있는 경우입니다.
  - dp[i][j][k] : [i][j][k]에서부터 [maxn][maxn][maxn]까지 가면서 쌓인 값들을 저장
    - [0][0][0]에서부터 [i][j][k]까지 오는데 여러 경우가 있는 경우 사용합니다.
    - 따라서, 뒤에서부터 계산하여 max값을 찾아서 dp[i][j][k]에 넣으며 최대를 확정화해오는 방법으로 진행됩니다.

### Point
- 3가지 색의 막대쌍이 있습니다. ( Red, Blue, Green )
- 색깔별 막대의 갯수가 주어집니다.
- 이후, 각 색깔 내에 막대의 길이가 주어집니다.
- 막대로 만든 사각형 넓이의 최대합을 출력합니다.
- 사각형을 만드는 방법은 다음과 같습니다.
  - 서로 다른 색의 막대쌍 i, j 를 선택하고, 각각을 가로와 세로로 정합니다.
  - 이를 통해 해당 square의 넓이를 도출할 수 있습니다.

### Design
- 계속해서 2가지의 색상 중 하나씩을 선택해야 합니다.
- dp로 접근합니다.
- dp[i][j][k] : red를 i개 , green을 j개, blue를 k개 사용했을때의 최대 넓이
- 위와 같이 식을 정의하여 풀이해보려 하였지만 문제가 있다는 점을 발견했습니다.
- 예를 들어,
  - dp[0][0][0] 에서 dp[2][2][2]를 가는 경우는 매우 많습니다.
  - (1, 1, 0) -> (1, 2, 1) -> (2, 2, 2)
  - (1, 1, 0) -> (2, 1, 1) -> (2, 2, 2)
  - (0, 1, 1) -> (1, 2, 1) -> (2, 2, 2)
  - (1, 0, 1) -> (2, 1, 1) -> (2, 2, 2)
  - ...
  - 즉, 경우의 수가 매우 많고 식으로 정의할 수 없는 수준입니다.
    - 만약 경우의 수가 별로 없거나 dp이해도가 높다면 보통의 dp처럼 식으로 정의하여 풀이할 수 있겠지만요
- 위의 방식대로 진행하면 [0][0][0]에서 어떤 선택을 하냐에 따라 [2][2][2]에 들어가는 값이 바뀌어버립니다.
  - dp 특성상 -1이 아닌 어떤 값이 생기면 바로 저장을 합니다.
- 따라서 다음과 같이 정의해야 답을 도출할 수 있습니다.
- dp[i][j][k] : red가 nr-i개, green이 ng-j개, blue가 nb-k개 남았을때, 남은 부분을 진행했을때의 최대값
- 즉, 끝까지 가고 돌아오면서 값을 정하기 때문에 ( 모든 경로 탐색 )
- max값을 찾아낼 수 있습니다.

#### Big O(time)
- O(n^3)

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
