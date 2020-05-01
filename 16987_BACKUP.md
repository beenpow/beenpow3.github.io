---
title : BOJ::16987 계란으로 계란치기

categories:
    - BOJ
tag:
    - BOJ
---
# BOJ::16987 계란으로 계란치기
- [Link : BOJ::16987](https://www.acmicpc.net/problem/16987)
- Level : Silver 2
- tag : brute force, backtrack

# 시사점
- naive 하게 backtrack을 돌리면 되는 문제입니다.

# 이해(14)
- 각 계란은 내구도와 무게로 이루어져 있습니다.
- 두 개의 계란씩 집어서 서로 충돌시킬 수 있으며,
- 충돌시 각 내구도는 상대방의 무게만큼 깎입니다.
- 이후, 내구도가 0이하로 떨어진 경우 해당 계란은 사용할 수 없습니다.
- 최대 몇 개의 계란을 깨뜨릴 수 있는지 출력합니다.

# 설계, 손 코딩(3)
- 손으로 중심이 되는 코드를 구현합니다.
- dfs 경로찾기 문제처럼 해결할 수 있습니다.
- n이 8이므로, naive하게 구현해도 시간이 충분합니다.

## 시간 복잡도
- O(N^N) 

## 공간 복잡도

# 손 코딩 후 문제 리뷰(x)
- 빼먹고 손 코딩 한 것이 있는지 확인합니다.

# 구현(8)

## 함수 List 

## 업데이트 되는 변수
- 대부분의 디버깅 문제는 업데이트 되는 변수에서 발생합니다.

## 실제 구현 

```cpp
#include<bits/stdc++.h>
#define endl '\n'
#define pb push_abck
#define rep(i,a,b) for(int i=a;i<b;i++)
using namespace std;
const int MAXN = 10;

int n, ans;
int weight[MAXN], oHP[MAXN];
void input(){
    cin >> n;
    rep(i, 0, n) cin  >> oHP[i] >> weight[i]; // 실수(2m) : 반대로 받음
}
void backtrack(int idx, int broken, int (&HP)[MAXN]){
    if(idx == n){
        ans = max(ans, broken);
        return;
    }
    bool exist = false;
    if(HP[idx] > 0){
        rep(i, 0, n) if(i != idx && HP[i] > 0){ // 실수(2m) : if(HP[i]) 는 음수, 양수 모두인 경우에 true가 된다.
            exist = true;
            HP[idx] -= weight[i];
            HP[i] -= weight[idx];
            backtrack(idx+1, broken + (HP[idx] <= 0?1:0) + (HP[i] <=0 ?1:0), HP);
            HP[idx] = min(HP[idx] + weight[i],   oHP[idx]);
            HP[i]   = min(HP[i]   + weight[idx], oHP[i]);

        }
        if(!exist) backtrack(idx+1, broken, HP);
    }else backtrack(idx+1, broken, HP);
}
void process(){
    input();
    int HP[MAXN];
    memcpy(HP, oHP, sizeof(HP));
    backtrack(0, 0, HP);
    cout << ans << endl;
}
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    process();
    return 0;
}
```

# 구현 후 코드리뷰 + 예제 돌려보기(x)
- 바로 예제를 넣어서 테스트 해보기 전에, 코드를 한 번 리뷰하고, 예제의 입력이 이런식으로 왔을때
  전체 코드가 어떤 식으로 순차적으로 진행되는지 답은 잘 도출될 것 같은지 확인합니다.

# 디버깅(4)
- 2가지 실수를 하였습니다.
  - (2m) : 내구도와 무게를 반대로 입력받았습니다.
  - (2m) : HP[i]가 양수인 경우에만 처리하려 하였지만, if(HP[i]) 로 구성하였습니다.
    - 이 경우 HP[i]가 0이 아닌 경우 모두 true로 처리됩니다.

# 좋은 코드

# 최적화
