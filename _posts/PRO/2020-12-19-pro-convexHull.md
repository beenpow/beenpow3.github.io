---
title : Pro::Convex Hull

categories:
    - professional
tag:
    - professional
---
# Convex Hull (볼록 껍질) 정의
- 영어로 보면 매우 생소한 단어이지만, 한국말로 보면 뜻을 알 수 있습니다.
- 볼록 껍질은 집합으로 주어진 점이나 영역을 포함하는 가장 작은 볼록 집합입니다.
- 따라서 볼록 껍질 알고리즘은 볼록 껍질을 만들어가는 알고리즘이고, 기하 문제에서 해당 알고리즘이 필요한 이유를 아래 글을 따라 읽으며 이해할 수 있습니다.


# 알고리즘
- 여러 개의 점이 주어졌을때 볼록 껍질을 구하는 알고리즘은 대표적으로 2가지가 있습니다.
  - Graham's Scan
  - Andrew's Algorithm

- 저도 Convex Hull 이라는 단어 자체에서 얻는 공포가(난이도가 높은 문제에 주로 나오는 기하)있었지만,
- 간단한 Convex Hull 알고리즘은 외적에 대한 이해만으로도 충분히 모두가 이해할 수 있다고 생각합니다.
- 따라서 ccw라는 것에 대해 먼저 설명해보려 합니다.

## Convex Hull 알고리즘을 배우기 위한 벡터의 외적과 CCW

![img1](/img/2020-12-19-pro-convexHull-1.png)

- 위와 같이 주어진 모든 점을 포함하는 볼록한 껍질 형태를 볼록 껍질이라고 합니다.
- 우리는 해당 문제를 풀기 위해, '벡터'의 개념을 이해하고 있어야 합니다.
  - 스칼라'는 오직 크기만을 갖는 값의 형태입니다.
  - 이에 반해 벡터'는 크기와 방향을 모두 갖는 형태입니다.
  - 따라서, 벡터는 n차원 좌표평면에서 직선 + 화살표 로 표현할 수 있습니다.

- 또한, 우리는 해당 문제에서 벡터의 외적'이라는 개념을 사용해야합니다.
  - 벡터의 외적은 다른말로 벡터의 곱 이라고도 표현합니다.
    - 따라서, 외적을 구하고자 하는 두 벡터 사이에 'x'라는 곱하기 기호를 사용하죠.
  - 고등학교때 배웠던 개념이고 공식이라 낯설게 다가오지만, 계산은 다음과 같이합니다.
  - 벡터 u = (m1, m2, m3), 벡터 v = (n1, n2, n3)라고 하겠습니다.
  - 벡터 u x 벡터 v 의 결과는 다음과 같습니다.
  - (m2n3 - m3n2, m3n1 - m1n3, m1n2 - m2n1)

> 이때, 우리가 아래에서 사용하는 알고리즘은 평면상에서 convex hull을 구하는 알고리즘입니다.
- 따라서, m3와 n3라는 z성분 값을 0으로 만들어서 식을 재정리할 수 있습니다.
  - 벡터 u x 벡터 v = (0, 0, m1n2 - m2n1)

- 그럼 이제 외적이라는 개념을 통해 CCW 라는 개념을 도입할 수 있습니다.
  - CCW : Counter Clockwise의 약자로, 평면 위에 놓여진 세 점의 방향 관계를 구할 수 있는 알고리즘입니다.
  - 아래 그림과 같이 세 점이 놓여있다고 할때, 세 점의 방향 관계를 구할 수 있습니다.

![img2](/img/2020-12-19-pro-convexHull-2.png)

- 위와 같이 세점을 통해 두 벡터 a, b를 생성할 수 있습니다.
- 그리고 해당 두 벡터의 외적 값을 구해서 세 점이 A->B->C순서로 시계방향인지 반시계방향인지 판단이 가능합니다.
  - 세 점 A, B, C 는 다음과 같이 정의할 수 있습니다.
  - A = (x1, y1, 0), B = (x2, y2, 0), C = (x3, y3, 0)
  - 벡터 a = 벡터AB = (x2-x1, y2-y1, 0), 벡터 b = 벡터AC = (x3-x1, y3-y1), 0)
  - 위에서 설명한 바와 같이 2차원 평면상에서의 값을 구해야하므로 결과값의 x y 성분은 0이라고 할 수 있습니다.
  - = (0, 0, (x2-x1) * (y3-y1) - (x3-x1) * (y2 - y2)

- z성분의 결과값만을 통해 세 점이 시계방향으로 놓여있는지 등의 형태를 구할 수 있습니다.
  - z성분의 값이 0보다 크다면, 반시계 방향
  - z성분의 값이 0보다 작으면, 시계 방향
  - z성분의 값이 0인 경우, 세 점이 일직선 상에 놓여있는 경우

- 위에서 구한 z성분이 곧 ccw의 계산식이 되고 펼쳐서 정리하면 아래와 같은 코드를 만들 수 있습니다.

```cpp
struct Point {
	int x, y;
};
ll ccw(const Point a, const Point b, const Point c) {
	return (1LL * a.x * b.y + 1LL * b.x * c.y + 1LL * c.x * a.y) - (1LL * b.x * a.y + 1LL * c.x * b.y + 1LL * a.x * c.y);
}
```

## Andrew's Algorithm
- 이제 위에서 배운 벡터의 외적을 통한 CCW 라는 개념을 통해 Convex Hull을 구하는 알고리즘을 배워보겠습니다.
- 알고리즘은 다음과 같습니다.
  - 먼저 모든 점을 X 값의 오름차순으로 정렬합니다.
    - X 값이 같은 경우 Y값의 오름차순으로 정렬합니다.
  - 볼록외피를 시계방향으로 탐색할지 반시계방향으로 탐색할지 결정합니다.
  - 스택을 사용합니다.
    - 스택에는 볼록외피를 구성하는 점이라고 판단되는 점들이 포함됩니다.
    - 처음 스택에 들어갈땐 볼록외피라고 판단되어서 넣었지만, 추후에 해당 점은 볼록외피가 아니라고 판단되면
    - 다시 빼내는 구조입니다.
  - prev, cur, next 를 통해 ccw를 계산합니다.
    - 이때 prev는 스택의 마지막에서 2번째 값
    - cur는 스택의 마지막 값
  - while(ccw(prev, cur, next) >= 0) pop()
  - push(next)

- 과정이 진행되는 그림은 다음과 같습니다.


![img3](/img/2020-12-19-pro-convexHull-3.png)

![img4](/img/2020-12-19-pro-convexHull-4.png)

![img5](/img/2020-12-19-pro-convexHull-5.png)

![img6](/img/2020-12-19-pro-convexHull-6.png)

![img7](/img/2020-12-19-pro-convexHull-7.png)

![img8](/img/2020-12-19-pro-convexHull-8.png)

![img9](/img/2020-12-19-pro-convexHull-9.png)

![img10](/img/2020-12-19-pro-convexHull-10.png)

![img11](/img/2020-12-19-pro-convexHull-11.png)

![img12](/img/2020-12-19-pro-convexHull-12.png)



# 복잡도

# 구현
- CCW와 stack사용을 익힐 수 있습니다.
- 하기의 백준문제의 풀이와 동일합니다.
- [BOJ::1708](https://www.acmicpc.net/problem/1708)

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#define endl '\n'
#define rep(i, a, b) for(int i=(a);i<(b); i++)
#define r_rep(i,a,b) for(int i=(a);i>(b); i--)
typedef long long ll;
struct Point {
	int x, y;
	int px, py;
};
const int MAXN = 100000 + 100;
using namespace std;

int n;
Point point[MAXN];
int rear;
int stk[MAXN];
void input() {
	cin >> n;
	rep(i, 0, n) {
		cin >> point[i].x >> point[i].y;
		point[i].px = 1, point[i].py = 0;
	}
}
bool comp(const Point a, const Point b) {
	if (1LL * a.py * b.px != 1LL * a.px * b.py)
		return 1LL * a.py * b.px < 1LL * a.px * b.py;
	if (a.y != b.y)
		return a.y < b.y;
	return a.x < b.x;
}
ll ccw(const Point a, const Point b, const Point c) {
	return (1LL * a.x * b.y + 1LL * b.x * c.y + 1LL * c.x * a.y) - (1LL * b.x * a.y + 1LL * c.x * b.y + 1LL * a.x * c.y);
}
void solve() {
	rear = 0;
	stk[rear++] = 0;
	stk[rear++] = 1;
	int next = 2;
	while (next < n) {
		while (rear >= 2) {
			int second = stk[--rear];
			int first = stk[rear - 1];
			if (ccw(point[first], point[second], point[next]) > 0) {
				stk[rear++] = second;
				break;
			}
		}
		stk[rear++] = next++;
	}
}
template <typename It>
void _swap(It &a, It &b) {
	It c(a); a = b; b = c;
}
template <typename It, typename Comp>
void _sort(It begin, It end, Comp comp) {
	if (begin == end)
		return;
	_swap(*begin, *((end - begin) / 2 + begin));
	It pi = begin;
	It le = begin + 1;
	It ri = end - 1;
	while (le <= ri) {
		while (le <= ri && !comp(*pi, *le))
			le++;
		while (le <= ri && !comp(*ri, *pi))
			ri--;
		if (le <= ri)
			_swap(*le, *ri);
	}
	_swap(*pi, *ri);
	_sort(begin, ri, comp);
	_sort(ri + 1, end, comp);
}

void process() {
	input();
	_sort(point, point + n, comp);
	rep(i, 1, n) {
		point[i].px = point[i].x - point[0].x;
		point[i].py = point[i].y - point[0].y;
	}
	_sort(point + 1, point + n, comp);
	solve();
	cout << rear << endl;
}
int main() {
	int tc; cin >> tc;
	for (int rnd = 1; rnd <= tc; rnd++) {
		cout << "#" << rnd << " ";
		process();
	}
	return 0;
}
```
