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
  - rank :  5910
  - score : 1 solved
  - A, B 문제가 오류를 포함하여 출제되어, unrated된 대회입니다.

## Problem A : Buying Torches

- level : 1000
- tag : math
- time : 00:23

### Point
- x, y, k가 주어집니다.
- 이 수를 이용하여 k개의 torches를 만드는데 필요한 최소 작업의 수를 출력합니다.
- 하나의 torches를 만들기 위해서는 하나의 스틱과 하나의 coal이 사용됩니다.
- 또한, 최초 시작엔 하나의 스틱만 존재합니다.
- 작업은 다음과 같이 2가지가 있습니다.
  - 스틱 1개를 잃고, 스틱 x개를 얻는 작업
  - 스틱 y개를 잃고, coal 1개를 얻는 작업

### Design
- coal을 만드는 방법은 작업 2방법 뿐입니다.
  - 따라서 coal은 만들어야하는 torches 갯수인 k개만큼 필요합니다.
  - 즉, coal k개를 얻기 위해서는 스틱 y * k 개가 필요합니다.
- 위에서 필요한 만큼에서 하나를 뺀 수만큼의 스틱이 있어야 총 스틱이 0이됩니다.
- 또한, 스틱도 k개 필요하므로 이 두 수를 더해주어 답을 구합니다.

#### Big O(time)
- O(1)

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
- 대회 중 다음과 같이 풀이하였지만 WA를 받았습니다.
  - unlocked인 수를 모두 모은 다음, 내림차순 양수를 순서대로 넣어주고, 오름차순 음수를 순서대로 넣어주었습니다.
  - 양수가 먼저 차있어야한다는 아이디어는 캐치하였지만, 음수또한 내림차순으로 넣어주어야 했었기에 맞추지 못하였습니다.

### Point
- n이 주어집니다.
- n개의 수로 이루어진 배열 a가 주어집니다.
- n개의 수로 이루어진 배열 l이 주어집니다.
- l의 원소는 0 또는 1이며, 1인경우 locked를 0인 경우 unlocked를 의미합니다.
- unlocked에 있는 수들을 마음대로 permutation할 수 있을때, 가장 작은 k를 출력합니다.
  - 여기서 k는 permute된 배열에서 prefix sum을 구했을때, 가장 늦게나오는 음수값의 인덱스를 의미합니다.

### Design
- 라운드 진행중엔 작은 생각차이로 WA를 받았습니다.
  - 문제 설명에 이유를 적어두었습니다.
  - 음수는 오름차순으로 진행했던 이유는, 그래야 마이너스 될것이 빨리되므로 k의 값이 작아질 것이라 생각했기 떄문입니다.

- 제가 위와 같은 느낌적인 느낌으로 진행했었고, 그렇지 않음을 editorial이 증명해두었습니다.
  - 아래와 같은 설명을 통해, 오름차순을 내림차순으로 바꿨을때 손해볼 것이 없다는 것을 증명합니다.
  - 즉, 주안점은 전체 수의 내림과 오름이지, 양수와 음수를 따로 구분하여 풀이하는 것이 옳지 않다는 의미도 내포하고 있습니다.

```cpp
In other words the unlocked 𝑎𝑖 should be sorted in decreasing order from left to right. 
To prove this, you can use an exchange argument: if you consider an arrangement of the 𝑎𝑖 where two consecutive 
unlocked values are not in decreasing order, we can swap them with each other. 
This swap does not make any of the 𝑝𝑖 smaller (it can only make some 𝑝𝑖 bigger). 
Thus we can start with the optimal ordering and repeatedly apply swaps until the unlocked values are sorted, without making anything worse.
```

- unlocked되어있는 모든 수를 내림차순 정렬하여 순서대로 재배열해주면 됩니다.


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
- 대회 도중에 제출한 코드와 99퍼센트 유사하지만, 라운드 중엔 WA를 받았습니다.
- dp에 대한 이해도가 낮았고, 다음의 사실을 몰랐기 때문입니다.
- dp[i] : 범위 [i:n]을 진행했을때의 최소값일때, 이 값은 뒤에서부터 계산되며 앞으로 넘어옵니다.
- 이러한 dp의 경우, 더해지는 값(score)을 인자로 넘기지 않으며, 재귀함수의 리턴값에 바로 더해서 min 혹은 max를 계산해야합니다.

### Point
- n이 주어집니다.
- n개의 원소로 구성된 배열 a가 주어지고, 각 원소는 0 혹은 1입니다.
  - 0은 쉬운 상대를, 1은 강한 상대를 의미합니다.
- 친구와 내가 턴을 번갈아가며 게임을 진행합니다.
- 친구가 먼저 시작하며, 우리는 적을 1명만 잡아도 되고, 2명만 잡아도 됩니다.
- 단, 친구의 턴에 잡는 적 중 강한 상대가 있는 경우, 그 수만큼 skip point가 증가합니다.
- 이렇게 게임을 진행할때, 최소 skip point를 출력합니다.

### Design
- 제가 dp에 대해 부족했던 부분은 문제 설명부에 적어두었습니다.
- 해당 문제는 여러 경우의 수가 존재하는 결정문제이기 때문에 dp[i]는 범위 [i:n]에 대해 계산되어야 하며, 뒤에서부터 앞으로 계산되어야 합니다.
- 이외에는, 문제에서 주어진대로 스코어 계산을 case 분류 후 진행해주면 됩니다.

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
- 수학문제에서 좌표평면이 나오는 경우, 해당 문제처럼 가능한 복잡도 내에서 해결할 수 있게 해주는 테크닉이 종종 존재하는 것 같습니다.
- 해당 문제에서는 '배열의 원소들의 차'만을 바라볼 것이 아니라, 이들의 합은 결국 양 끝값의 차를 통해 구할 수 있다는 점을 눈치채야 합니다.

### Point
- n과 q가 주어집니다.
  - n은 원소의 갯수, q는 쿼리의 갯수를 의미합니다.
- n개의 원소를 가진 배열 a가 주어집니다.
- q개의 쿼리문이 주어집니다.
  - 각 쿼리문은 ti와 xi로 주어집니다.
  - ti가 0인 경우, 점 xi에 존재하는 pile을 제거한다는 의미입니다.
  - ti가 1인 경우, 점 xi에 pile을 부여한다는 의미입니다.
- 쿼리문의 시작전과 매 쿼리문의 종료 후, 다음 조건을 만족하는 값을 출력합니다.
  - 주어진 원소들 중 2개를 제외한 모든 원소를 둘 중 하나의 위치까지 옮겨야 합니다.
  - 원소의 위치가 x인 경우, x+1과 x-1로 이동하는데 작업 1이 소요됩니다.
  - 이때, 최소 작업수를 각각 출력합니다.

### Design
- 해당 풀이는 'edenooo'님의 코드를 참고하였습니다.
- 수학 문제의 묘미는 복잡도를 줄일 수 있는 방법이 매번 신기하다는 점입니다.
- 이 문제 또한, 점을 하나씩 옮기는 것이 결국 양쪽 맨 끝값의 차를 의미한다는 점을 캐치해야 합니다.
- 풀이는 다음과 같습니다.
  - 점들을 set을 이용하여 OX 라인에 정렬해둔다고 합시다.
  - 이때 작업의 수는 max(s) - min(s) - maxLen_between_two_points 가 됩니다.
  - 즉, 주어진 범위의 길이에서 가장 거리가 먼 두 점사이의 거리를 뺀 값입니다.
- 이를 풀이하기 위해, set과 우선순위 큐가 사용되었습니다.
- s에는 주어진 원소들을 저장해둡니다.
- pq에는 x_i - x_i-1 즉, 인접한 두 점사이의 차를 저장해둡니다.
- del에는 pq에서 삭제되어야할 값들을 저장합니다.
- 이후, 매 쿼리마다 다음과 같은 작업을 진행합니다.
  - t = 0인 경우(x를 삭제하라)
    - x를 삭제하기 전에, prev(x) -- x , x -- next(x) 이렇게 2가지 길이를 pq에서 삭제해줘야 하므로 del에 push해 둡니다.
    - 또한, 삭제된 후, 새로 발생한 길이인 prev(x) ---- next(x) 사이의 길이를 pq에 푸쉬해줍니다.
    - x를 set에서 삭제합니다.
  - t = 1인 경우(x를 추가하라)
    - x를 추가하기 전에, next(x) -- prev(x)를 연결하는 길이를 pq에서 삭제하기 위해 del에 푸쉬해둡니다.
    - 이후, prev(x) -- x, x -- next(x)를 pq에 푸쉬해줍니다.
    - x를 set에 추가합니다.
- 이후, 출력할때마다, 가장 큰값에서 가장 작은 값 그리고 pq의 탑을 가감하여 출력합니다.
  - 또한, 이때 del도 같이 처리해줍니다.
  - del이 비어있진 않지만, pq의 탑과 del의 탑이 다른 경우 출력에 영향을 주지 않습니다.

#### Big O(time)
- O(NlogN)

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
