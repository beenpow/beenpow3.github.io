---
title : Pro::section press

categories:
    - sweaB
tag:
    - sweaB
---
# 좌표압축

> 좌표 압축이란, 문제에서 제시되는 좌표의 범위보다 입력의 갯수가 적을때 사용하는 기법입니다.
- ex) -2^30 <= x, y <= 2^30 이고 x,y는 총 5000개가 주어질 떄 사용할 수 있습니다.
- 추가로, 이때의 입력을 쿼리(query)라고도 흔히 말합니다.


```cpp
2*2^30개의 입력을 int array[1 << 30][1 << 30]와 같이 index와 1대 1 matching 하여 사용하려 한다면 
memory overflow를 피할 수 없습니다. 또한 시간초과의 가능성에도 가까워지게 됩니다.
```

## 사용방법

- 문제에 제시된 좌표의 범위가 -10억 ~ 10억 이라면, 입력받는 좌표의 수는 많아야 N + 2 * M개 입니다.
- 따라서 우리는 최대 N + 2 * M개의 좌표를 indexing하여 정렬된 순서대로 1 ~ (N+2 * M)의 번호를 매겨준
  다음 답을 구할 수 있습니다.

### push

```cpp
vector<int> idx;
// 입력 a의 값을 idx에 push 합니다.
for (in i = 0; i < n; ++i){
    scanf("%d", &a[i]);
    idx.push_back(a[i]);
}
// 좌표 압축을 x, y에 관계 없이 사용하므로
// 압축할 값을 push합니다.
for (int i = 0; i < m; ++i){
    scanf("%d %d", qry[i].first, qry[i].second);
    idx.push_back( qry[i].first );
    idx.push_back( qry[i].second );
}
// 벡터 idx를 정렬합니다.
sort( idx.begin(), idx.end() );
// idx의 원소들 중 중복되는 원소들을 제거합니다.
idx.erase(unique(idx.begin(), idx.end()), idx.end());
```

### pop

```cpp
// x(key)이상의 가장 작은 정수에 matching되는 index를 리턴합니다.
int getidx(int x){
    // lower_bound는 x(key)이상의 가장 작은 정수에 matching되는 
    // index에 해당하는 iterator를 리턴해줍니다.
    return lower_bound(idx.begin(), idx.end(), x) - idx.begin();
}
```

# 좌표압축 2

- BOJ<등고선 지도>문제를 풀던 중 좌표 압축의 활용법이 나와 내용을 추가합니다.
- 기존 방식처럼 lower_bound를 사용하는 것 같지만, lower_bound 의 결과로 나온 상대적인 index 값으로
  원래의 값을 바꿔버립니다.
  - 먼저 x좌표는 모두 vector<int> x에 push 합니다.
  - y 좌표는 모두 vector<int> y에 push 합니다.
  - 이후 원래의 값이 들어있던 위치인 grp[i][j]에
    - lower_bound한 결과인 상대적 인덱스 값을 대입합니다.


```cpp
vector<int> x;
vector<int> y;
vector<pair<int,int> > grp[20000];

cin >> n;
        for(int i = 0; i < n; ++i){
            cin >> sz;
            for(int j = 0; j < sz; j++){
                int a,b;
                cin >> a >> b;
                grp[i].push_back({a, b});
            }
        }
        for(int i = 0; i < n; i++){
            for(int j = 0; j < grp[i].size(); j++){
                x.push_back(grp[i][j].first);
                y.push_back(grp[i][j].second);
            }
        }
        sort(x.begin(), x.end());
        for(int i = 0; i < n; i++){
            for(int j = 0; j < grp[i].size(); j++){
                grp[i][j].first  = (int)(lower_bound(x.begin(), x.end(), grp[i][j].first)  - x.begin());
                grp[i][j].second = (int)(lower_bound(y.begin(), y.end(), grp[i][j].second) - y.begin());
            }
        }
```
