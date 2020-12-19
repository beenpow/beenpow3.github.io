---
title : Pro::Rabin-Karp + Rolling Hash

categories:
    - professional
tag:
    - professional
---
# Rabin-Karp 도입
- 라빈카프 알고리즘은 긴 문자열을 해싱하는 대표적인 알고리즘입니다.
- 긴 문자열 S가 주어지고 이 중 pattern P를 찾을때 매우 유용합니다.
  - KMP알고리즘의 용도와 비슷하다고 볼 수 있습니다.

- 문자열 S에서 pattern P를 찾는 brute force한 방법은 아래와 같습니다.

![img1](/img/2020-12-19-pro-RabinKarp_RollingHash-1.png)

![img2](/img/2020-12-19-pro-RabinKarp_RollingHash-2.png)

![img3](/img/2020-12-19-pro-RabinKarp_RollingHash-3.png)

- 이는 복잡도 O(NM)이 나오게 됩니다.
  - S의 길이를 N, P의 길이를 M이라 하겠습니다.

# 알고리즘

- 위와 같이 Brute Force하게 하지말고, 다음과 같은 방법으로 구해보자는 것이 라빈카프 알고리즘입니다.
- 문자열을 숫자로 수치화(해싱)하여 계산하자는 것입니다.
- 문자열을 수치화해서 계산하면 아래와 같이 표현될 수 있습니다.

![img4](/img/2020-12-19-pro-RabinKarp_RollingHash-4.png)

![img5](/img/2020-12-19-pro-RabinKarp_RollingHash-5.png)

... 

![img6](/img/2020-12-19-pro-RabinKarp_RollingHash-6.png)

- 문자열 s를 훑으며 길이 M에 대해서 H(x)값을 만들고, 이를 패턴의 H(x)값과 계속 비교해나가는 것입니다.
- 하지만 이런식으로 계산 s의 H(x)를 구하면 O(NM)은 변함이없습니다.

- 이를 해결하기 위해 드디어 라빈-카프 알고리즘이 사용됩니다.
- 이는 H(x)를 다항식으로 표현할 수 있다는 점에 착안하고 f(x)와 H(x)는 동치입니다.

![img7](/img/2020-12-19-pro-RabinKarp_RollingHash-7.png)

- 위와 같은 식을 실제로 사용하기 위해 아래와 같이 바꿀 수 있습니다.
  - H[i] : 문자열 s 에서 범위 S[i:i+M-1]로 이루어진 길이 M인 문자열의 해싱 값

![img8](/img/2020-12-19-pro-RabinKarp_RollingHash-8.png)

- 여기서 x는 적당한 소수를 선택합니다.
- 위의 식을 해석하자면,
  - 길이 M까지는 값을 하나씩 더해주고
  - 길이 M이상인 순간부터는 O(1)만에 바로 해싱값을 구해낼 수 있습니다.
  - 가장 먼저 들어간 값을 빼고, 가장 나중에 들어오는 값을 더하면 됩니다.
  - 위의식을 참고로 설명하자면
  - S[i] * x^(M-1)을 빼고, 기존에 있던 값들에 x를 모두 곱해준 후, 새로 S[i+M] * x^0을 더해줍니다.

- 물론 STL해싱을 사용할 수도 있겠지만, Rolling Hash를 사용할 수도 있습니다.
- 주로 Bucket을 이용한 해싱이라고도 부르는 형태입니다.
- HASHSIZE만큼의 열을 만들고,
- 계산된 Hash값이 해당 열의 값인 경우, 
- 해당열에 행을 계속 추가해나가는 방법입니다.
- 즉, 충돌이 발생하면 단순히 그 하단에 엮어주는 방법입니다.

# 복잡도
- O(N)
  - 이상적인 hashing인 경우

# 구현
- 구현부는 octorbirth님의 블로그를 참고하였습니다.
- [octorbirth](https://octorbirth.tistory.com/536)
- 해당 코드는 아래의 BOJ문제의 코드도 될 수 있습니다.
- [BOJ::3033](https://www.acmicpc.net/problem/3033)

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#define endl '\n'
#define rep(i, a, b) for(int i=(a);i<(b); i++)
#define r_rep(i,a,b) for(int i=(a);i>(b); i--)
typedef long long ll;
const int MAXN = 2 * 100000 + 100;
const int HASHSIZE = 1 << 16;
const int MOD = HASHSIZE - 1;
struct Node {
	char* key;
	Node* next;
	Node* alloc(char* nk, Node* np) {
		key = nk, next = np;
		return this;
	}
}buf[MAXN], *table[HASHSIZE]; 
int bufCnt, h;
using namespace std;

int n, ans;
char a[MAXN];
int val[MAXN];

void input() {
	// init
	ans = 0;
	cin >> n;
	cin >> a + 1;
	val[0] = 1;
	rep(i, 1, n + 1) val[i] = val[i - 1] * 33;
}
void init() {
	bufCnt = h = 0;
	for (int i = 0; i < HASHSIZE; i++) table[i] = NULL;
}
int _strncmp(const char* s, const char* t, int len) {
	int i = 0;
	for (; i < len && s[i] == t[i]; ++i);
	return i == len;
}
bool possible(int len) {
	init();
	rep(i, 1, n + 1) {
		h = h * 33 + a[i];
		// Rolling hash
		if (i >= len) {
			h -= val[len] * a[i - len];
			int hidx = h & MOD;
			Node* p = table[hidx];
			for (; p; p = p->next) {
				if (_strncmp(p->key, &a[i - len + 1], len))
					return true;
			}
			table[hidx] = buf[bufCnt++].alloc(a + i - len + 1, table[hidx]);
		}
	}
	return false;
}
void bs(int st, int en) {
	int mid = 0;
	while (st <= en) {
		mid = (st + en) / 2;
		if (possible(mid)) {
			ans = ans < mid ? mid : ans;
			st = mid + 1;
		}
		else {
			en = mid - 1;
		}
	}
}
void process() {
	input();
	bs(1, n - 1);
	cout << ans << endl;
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	int tc; cin >> tc;
	for (int rnd = 1; rnd <= tc; rnd++) {
		cout << "#" << rnd << " ";
		process();
	}
	return 0;
}
```
