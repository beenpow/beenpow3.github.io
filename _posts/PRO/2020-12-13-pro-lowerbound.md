---
title : Pro::lower bound

categories:
    - sweaB
tag:
    - sweaB
---
# lower_bound 정의

- lower_bound(arr, arr+n, key);
- 주어진 배열 범위 (arr[0] ~ arr[0+n]) 내에 찾으려는 key값이
    - 없으면 key값 이상인 가장 작은 정수에 해당하는 iterator를 리턴합니다.
    - 있다면 key값에 해당하는 iterator를 리턴합니다.
- 이진탐색(Binary Search)기반의 탐색 방법입니다.
- #include<algorithm> 에 포함되어 있습니다.

## 구현


- return 형이 iterator이므로,
    - vector를 input으로 사용하는 경우 iterator에서 v.begin()을 뺀 값을 인덱스로 사용하고
    - 배열인 경우 첫번째 주소를 가리키는 배열 주소에서 차감하여 인덱스로 사용합니다.

```cpp
// arr의 원소중 key이상의 가장 작은수를 가진 index의 iterator를 반환한다.
int my_lower_bound(int * arr, int n, int key){
    int s = 0; // start
    int e = n; // end
    int m = n; // middle
    // start와 end가 같지 않고, end가 start보다 더 큰 index인 동안
    while(e - s > 0){
        m = (s + e) / 2;
        // key값이 중앙값 보다 크면 mid의 우측을 start로 지정
        if (arr[mid] < key){
            s = m + 1;
        // key값이 중앙값보다 작거나 같으면 mid의 좌측을 end로 지정
        }else{
            e = m;
        }
    }
    return e + 1;
}
int my_lower_bound2(int A[], int n, int val){
    int begin = 0, end = n;
    int mid = begin;
    while(begin <= end){
        mid = (begin + end)/ 2;
        if(A[mid] < val)
            begin = mid+1;
        else
            end = mid-1;
    }
    return end+1;
}
// my_lower_bound의 사용
int get_idx(int x) {
    // int it = lower_bound(ss, ss + MAX, x) -ss; // 일반 lower_bound
    int it = my_lower_bound(ss, MAX, x) -1;
    if (it == MAX || ss[it] != x)return -1;
    return it;
}

int main(){
    // 배열 사용하는 경우
    int arr[5] = {1, 2, 3, 4, 5};
    cout << "lower_bound(3) : " << lower_bound(arr, arr + 5, 3) - arr + 1 << endl;
    // vector를 사용하는 경우
    vector<int> a[5] = {1,, 2, 3, 4, 5};
    cout << "lower_bound(3) : " << lower_bound(a.begin(), a.end(), 3) - a.begin() << endl;
    return 0;
}
```


# upper_bound 정의

- 필요시 upper_bound 내용 업데이트 하겠습니다.
