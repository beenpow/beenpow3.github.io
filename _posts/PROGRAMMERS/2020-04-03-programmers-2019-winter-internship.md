---
title : Programmers::2019 카카오 겨울 인턴십::호텔방 배전

categories:
    - programmers
tag:
    - programmers
---

- 2019 카카오 겨울 인턴십 문제에 대한 풀이입니다.
- 1, 2 번도 제출하였지만, 대회끝나고 나니 코드가 따로 저장되어 있지 않네요.
- 따라서, 재밌는 문제인 4, 5 번에 대해서만 포스트하려고 합니다.

# programmers::불량사용자

## 시사점
- 대부분의 사람들이 backtrack으로 푼 것 같습니다.
- 저는 next_permutation과 hash로 풀이하였기에 참고용으로 업로드 합니다.

## 이해
- user_id 와 banned_id가 있습니다.
- banned_id 로 추측되는 조합의 갯수를 출력합니다.

## 설계
- user_id 가 최대 8개인 점을 이용합니다.
- next_permutation으로 해도 충분한 복잡도입니다.
- 단, 중복이 없어야 하므로 bitmask를 이용하여 hashing하여 사용하였습니다.

## 구현

```cpp
#include<vector>
#include<map>
#include<algorithm>
#define endl '\n'
#define pb push_back
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
typedef long long ll;
using namespace std;

map<int,int> mp;
bool able(string user, string banned){
    if(user.size() != banned.size()) return false;
    rep(i, 0, banned.size()){
        if(banned[i] == '*') continue;
        if(user[i] != banned[i]) return false;
    }
    return true;
}

int solution(vector<string> user_id, vector<string> banned_id) {
    int answer = 0;
    vector<int> combi;
    rep(i, 0, user_id.size()) combi.pb(i);

    do {
        int cmp = 0;
        bool impossible = false;
        rep(i, 0, banned_id.size()){
            cmp |= (1 << combi[i]);
            if(!able(user_id[combi[i]], banned_id[i])){
                impossible = true;
                break;
            }
        }
        if(!impossible && mp.count(cmp) == 0){
            answer++;
            mp[cmp]++;
        }
    } while (next_permutation(combi.begin(), combi.end()));
    return answer;
}
```

# programmers::호텔방배정
- [Link : Programmers::호텔방배정](https://programmers.co.kr/learn/courses/30/lessons/64063)
- Level : 

## 시사점
- 굉장히 많이 헤맨 문제입니다.
- 최초 접근은 map을 이용하긴 했습니다.
  - map은 삽입과 동시에 정렬이 이루어지므로, 정렬된 상태를 유지할 수 있기 때문입니다.
  - 하지만 map의 원소들을 이분탐색하는 것이 쉽지 않았습니다.
  - 결론적으로 이분탐색을 구현했지만, TLE 의 원인이 따로 있었습니다.
    - 이분탐색으로 찾은 position에 특정 원소를 삽입하는 배열을 두었기에, 해당 배열을 뒤로 1칸씩
      shift해야하므로 결국 O(N)의 시간이 여기서 소모되었습니다.

- 하지만, 해당 문제의 정해는 union-find 였습니다.
- map<long long, long long> prn; 이라는 parent를 담는 해쉬테이블 하나면 해결되는 문제였습니다.
- 설명을 덧붙이자면,
  - 1 3 1 이 입력으로 들어온 경우,
  - 1
    - prn[1]이 존재하는지 확인합니다.
    - 존재하지 않으므로 1을 채택하고, prn[1] = 2 를 삽입합니다.
  - 3
    - prn[3]이 존재하는지 확인합니다.
    - 존재하지 않으므로 3을 채택하고, prn[3] = 4 를 삽입합니다.
  - 1
    - prn[1]의 값이 2로 존재합니다.
    - prn[2]가 존재하는지 확인합니다.
    - 존재하지 않으므로 2를 채택하고 prn[1] = 3, prn[2] = 3 으로 업데이트합니다. 


### 키

## 이해(x)
- n명의 사람이 들어가고 싶은 방의 넘버를 입력합니다.
- 들어갈 수 있는 방의 넘버 k는 최대 10^12 이고, 실제 들어가는 사람의 수는 2 * 10^5 명입니다.
- 배정방에 사람이 이미 들어가있는 경우, 그 다음 방을 받는 방식으로 방을 배정받습니다.
- 이때, 결국 N명이 들어간 실제 방의 넘버를 출력합니다.


## 설계, 손 코딩(x)
- 손으로 중심이 되는 코드를 구현합니다.
- k에 비해 실제 N은 현저히 작습니다.
- 배열을 k개 잡아주기엔 너무 크므로, 메모리 압축이 필요합니다.
- 이를 해결하기 위해 map을 사용합니다.
- 그리고 시사점 챕터에서 설명하였듯이, 특정 방의 부모가 비어있을때까지 while문을 돌려서
  찾아나섭니다.
  - 비어있는 곳을 찾은 경우 해당 지점을 할당하고, 다시 while문을 돌려서 빠져나오며 지나쳐간 모든
    path의 부모를 마지막으로 찾은 지점으로 변경합니다.
  - 즉, path에 있는 모든 정점들은 바로 가능한 지점을 가리키게 됩니다.

### 시간 복잡도

### 공간 복잡도

## 손 코딩 후 문제 리뷰(x)
- 빼먹고 손 코딩 한 것이 있는지 확인합니다.

## 구현(x)

### 함수 List 

### 업데이트 되는 변수
- 대부분의 디버깅 문제는 업데이트 되는 변수에서 발생합니다.

### 실제 구현 

```cpp
#include <string>
#include <vector>
#include <map>
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
using namespace std;
typedef long long ll;

map<ll,ll> prn;

ll _find(ll number){
    ll x = number, ret = 0;
    while(true){
        if(prn.count(x) > 0){
            x = prn[x];
        }else{
            ll nx = 0;
            while(number != x){
                nx = prn[number];
                prn[number] = x+1;
                number = nx;
            }
            ret = x;
            break;
        }
    }
    return ret;
}

vector<long long> solution(long long k, vector<long long> room_number) {
    vector<long long> answer;
    rep(i, 0, room_number.size()){
        ll ret = _find(room_number[i]);
        answer.push_back(ret);
        prn[ret] = ret+1;
        prn[room_number[i]] = ret+1;
    }
    return answer;
}
```

## 구현 후 코드리뷰 + 예제 돌려보기(x)
- 바로 예제를 넣어서 테스트 해보기 전에, 코드를 한 번 리뷰하고, 예제의 입력이 이런식으로 왔을때
  전체 코드가 어떤 식으로 순차적으로 진행되는지 답은 잘 도출될 것 같은지 확인합니다.

## 디버깅(x)

## 좋은 코드

## 최적화

# programmers::징검다리 건너기
- [Link : Programmers::징검다리 건너기](https://programmers.co.kr/learn/courses/30/lessons/64062)
- Level :

## 시사점
- 재밌고 좋은 문제라고 생각합니다.
- 아이디어만 떠오르면 풀 수 있지만, 그 통찰력을 가지기까지 시간이 꽤 소모되는 것 같습니다.
- stones의 값을 정렬해서 사용해야할 것 같은 감은 왔지만, 이분탐색까지 생각이 미치지 못했습니다.

### 키

## 이해(x)
- 징검다리를 stones 배열로 입력받습니다.
- stones 배열의 크기는 1 이상 2 * 10^5 이하입니다.
- stones 배열 각 원소들의 값은 1이상 2 * 10^8 입니다.
- k는 1 이상 stones의 길이 이하인 자연수입니다.
- 사람의 수가 무한대일때, 징검다리를 건널 수 있는 최대 사람 수를 출력합니다.
  - stone[i] 의 값은 한명이 한번 밟을때마다 1씩 감소합니다.
  - 또한, stones를 연속으로 밟아야 합니다.
  - 연속으로 밟을 수 없는 경우 최대 k칸 점프를 할 수 있습니다.
  - 그렇지 않은 경우 더이상 징검다리를 건널 수 없습니다.

## 설계, 손 코딩(x)
- 손으로 중심이 되는 코드를 구현합니다.
- 여러가지 개념이 등장하고, 필요한 값들을 잘 정리해야 할 것 같습니다.
- 모든 stones의 값이 일정한 값 5라면, 총 5명이 건널 수 있는 것은 자명한 사실입니다.
- k가 3이고 stones의 값이 1 1 3 5 인 경우는 어떨까요?
- 이때 1 1 3 5 중 최솟값인 1에 대해서, 즉 1명이 건널 수 있는 사실은 자명한 사실입니다.
- 그렇다면 1 다음으로 2로 해보면 좋을까요? 3으로 해보면 좋을까요?
  - 1명이 지나간 후의 상태는 0 0 2 4 입니다.
  - 2로 접근해보겠습니다.
    - 0 0 1 3 이 되고, 같은 방법으로 1명 더 건널 수 있는 사실이 자명합니다.
  - 3으로 접근해보겠습니다.
    - 0 0 0 2가 됩니다. 즉 2로 접근해서 * 2 할 필요없이, 바로 다음 배열 값인 3으로 접근하면 됩니다.

- 결론적으로, stones 의 max값을  end로 두고 0을 start로 두고 이분탐색을 시작합니다.
- 즉 mid값이 가능한 사람의 수가 됩니다.
- stones의 사이즈가 n이고, stones의 max값이 m 이라고 할때,
- O(nlogM)의 값을 갖게 됩니다.

### 시간 복잡도

### 공간 복잡도

## 손 코딩 후 문제 리뷰(x)
- 빼먹고 손 코딩 한 것이 있는지 확인합니다.

## 구현(x)

### 함수 List

### 업데이트 되는 변수
- 대부분의 디버깅 문제는 업데이트 되는 변수에서 발생합니다.

### 실제 구현

```cpp
#include <string>
#include <vector>
#include <algorithm>
#define endl '\n'
#define rep(i,a,b) for(int i=a;i<b;i++)
#define r_rep(i,a,b) for(int i=a;i>b;i--)
using namespace std;

bool possible(const vector<int>& stones, int k, int offset){
    int len = 0;
    rep(i, 0, stones.size()){
        if(stones[i] - offset <= 0){
            len++;
        }else{
            if(len >= k) return false;
            len = 0;
        }
    }
    if(len >= k) return false;
    return true;
}

int binarySearch(const vector<int>& stones, int k, int mx){
    int ret = 0;
    int st = 0, en = mx, mid = 0;
    while(st <= en){
        mid = (st + en) / 2;
        if(possible(stones, k, mid)){
            ret = max(ret, mid);
            st = mid + 1;
        }else{
            en = mid - 1;
        }
    }
    
    return ret + 1;
}

int solution(vector<int> stones, int k) {
    int answer = 0, mx = 0;
    for(auto x : stones)
        mx = max(mx, x);
    
    answer = binarySearch(stones, k, mx);
    
    return answer;
}

```

## 구현 후 코드리뷰 + 예제 돌려보기(x)
- 바로 예제를 넣어서 테스트 해보기 전에, 코드를 한 번 리뷰하고, 예제의 입력이 이런식으로 왔을때
  전체 코드가 어떤 식으로 순차적으로 진행되는지 답은 잘 도출될 것 같은지 확인합니다.

## 디버깅(x)

## 좋은 코드

## 최적
