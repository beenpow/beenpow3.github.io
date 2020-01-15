---
title : COFO::Round Good Bye 2016

categories:
    - sweaB
    - COFO
tag:
    - sweaB
    - COFO
---
# COFO::Round Good Bye 2016
[COFO]:<https://codeforces.com/contest/750>

## Problem A : New Year and Hurry

- 난이도 : 800
- 문제 분류 : 이분 탐색(기초)

### 시사점

- 이분 탐색 기본 문제입니다.
- 제가 아직 이분탐색을 정형화하지 못하여 간단한 문제로 테스트 중입니다.
- 어떤 경우엔 return end; 로 하고, 어떤 경우에는 retuen end+1; 로 하고 있습니다. 
- 뚜렷한 차이를 느끼지 못하여, 몇 문제 더 풀어볼 계획입니다.
- 이 문제는 특이하게 중간에 값을 찾으면 mid를 return해줍니다.


### 이해(x)

- 총 남은 시간 240분
- 이동하는 데 걸리는 시간 k분
- 문제를 풀 수 있는 시간 240-k분

### 설계(x)

#### 시간 복잡도

#### 공간 복잡도

### 구현(x)

```cpp
#include<iostream>
using namespace std;
int n, k, last = 240;
int spent[11];
int binarySearchMax(int A[], int sz){
    int begin = 1, end = sz;
    int mid = begin;
    while(begin <= end){
        mid = (begin + end) /2;
        if(A[mid] < last)
            begin = mid+1;
        else if(A[mid] > last)
            end = mid-1;
        else return mid;
    }
    return end;
}
int main(){
    freopen("input.txt", "r", stdin);
    cin >> n >> k;
    last -= k;
    for(int i = 1; i<= n;i++)
        spent[i] = spent[i-1] + i*5;
    cout << binarySearchMax(spent, n) << endl;
    return 0;
}
```

### 디버깅(x)

### 좋은 코드


