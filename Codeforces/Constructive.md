# Constructive

- [Constructive](#constructive)
  - [排列相关](#排列相关)


## 排列相关

原题链接：[B. Indivisible](https://codeforces.com/problemset/problem/1818/B)

我们需要构造出一个排列，使得对于任意的 $1\le l\le r\le n$ ，序列和 $a_l+a_{l+1}+\cdots + a_r$ 都不能被 $r-l+1$ 整除

我们按如下方式构造：

* 从排列 $P_1=1,2,3,\cdots, n$ 开始
* 交换**相邻**的**一对数**，得到 $P_2=2,1,4,3,6,5,\cdots, n-1,n$

对于排列 $P_2$ ，我们考察下标 $l,r$ 当中的序列和，有如下两种情况：

* 当 $l,r$ 的奇偶性不同时，$\sum_{i=l}^{r}p_i=\frac{(r-l+1)(l+r)}{2}$ ，这个数无法被 $r-l+1$ 整除，这是因为 $l+r$ 为偶数，无法被 $2$ 整除
* 当 $l,r$ 的奇偶性相同时，$\sum_{i=l}^{r}p_i$ 的值为 $\frac{(r-l+1)(l+r)}{2}+1$ 或 $\frac{(r-l+1)(l+r)}{2}-1$ ，这取决于 $l,r$ 同为偶数或奇数。由于 $l+r$ 为偶数，因此 $\sum_{i=l}^{r}p_i$ 的值与 $r-l+1$ **取模**的结果永远为 $1$ 或 $-1$ ，因此同样无法整除

特别地，对于 $n\gt 1$ 而言的**奇数**，它们不存在满足条件的排列，直接返回（因为 $\frac{(1+n)n}{2}$ 永远可以被 $n$ 整除）

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;
typedef pair<int, int> PII;
const int N = 1e5 + 10, INF = 0x3f3f3f3f;

void solve(){
	int n;
	cin >> n;
	if(n == 1){
		cout << 1 << "\n";
		return;
	}
	else if(n % 2){
		cout << -1 << "\n";
		return;
	}
	vector<int>a(n);
	iota(a.begin(), a.end(), 1);
	for(int i = 0; i < n; i += 2){
		swap(a[i], a[i + 1]);
	}
	for(int x : a) cout << x << " ";
	cout << endl;
}

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0);
	int t;
	cin >> t;
	while(t--)
    	solve();
    return 0;
}
```