# String

- [String](#string)
	- [计数](#计数)


## 计数

原题链接：[D. Distinct Split](https://codeforces.com/problemset/problem/1791/D)

本题最常规的做法为**前后缀分解**，分别记录每个下标前缀与后缀的不同字符数量，然后求和即可：

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;
typedef pair<int, int> PII;
const int N = 1e5 + 10, INF = 0x3f3f3f3f;

void solve(){
	int n;
	string s;
	cin >> n >> s;
	vector<int>pre(n + 1, 0), suf(n + 1, 0);
	unordered_set<char>S;
	for(int i = 0; i < n; i ++){
		S.insert(s[i]);
		pre[i] = S.size();
	}
	S.clear();
	int ans = 0;
	for(int i = n - 1; i >= 0; i --){
		S.insert(s[i]);
		suf[i] = S.size();
	}
	for(int i = 0; i < n; i ++){
		//cout << pre[i] << " " << suf[i] << endl;
		ans = max(ans, pre[i] + suf[i + 1]);
	}
	cout << ans << endl;
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

另一种做法是先动态枚举边界线左右两边不同字符的数量

具体地，创建两个哈希表，先统计边界线右边每个字符出现的数量。随着边界线向右移动，右边哈希表中的字符逐渐减小，左边哈希表中的字符逐渐增加，然后每次遍历两个哈希表求不同字符的个数即可

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;
typedef pair<int, int> PII;
const int N = 1e5 + 10, INF = 0x3f3f3f3f;

void solve(){
	int n;
	string s;
	cin >> n >> s;
	vector<int> cnt(26, 0), p(26, 0);
	for(char x : s) cnt[x - 'a']++;
	int ans = 0;
	for(char x : s){
		cnt[x - 'a']--, p[x - 'a']++;
		int cur = 0;
		for(int i = 0; i < 26; i ++){
      //我们只需要统计不同字符的数量，因此只要这个字符不为零，那么便统计一次即可
			cur += min(1, cnt[i]) + min(1, p[i]);
		}
		ans = max(ans, cur);
	}
	cout << ans << endl;
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

