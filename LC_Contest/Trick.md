# Trick

- [Trick](#trick)
  - [Sliding Window](#sliding-window)
  - [Backtrace](#backtrace)
  - [Binary Search](#binary-search)


## Sliding Window

通常写法：

```cpp
int l = 0, ans = 0;
for(int r = 0; r < n; r ++) {
	//update something
	//condition unconsistent, go into while loop
	while() {
	
	}
	//condition consistent, can update ans
	//update ans
}
```

[2730. 找到最长的半重复子字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-longest-semi-repetitive-substring/description/)

求第 $k$ 小的数，用计数排序：

- 统计每个数出现的次数，记在 $cnt$ 中
- 依次遍历这个数组，用 $sum$ 累加 $cnt$
- 如果当前 $sum\ge k$ 满足，则说明**第 $k$ 小的数为此时的下标**

[2653. 滑动子数组的美丽值 - 力扣（LeetCode）](https://leetcode.cn/problems/sliding-subarray-beauty/description/)

## Backtrace

对字符串进行划分，判断最终得到的子串的和是否满足条件

- [2698. 求一个整数的惩罚数 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-punishment-number-of-an-integer/description/)
    
  定义 `check(int x, int p)` 表示对 `x` 进行划分，考虑其总和能否等于 `p`
  
  边界：如果 `x` 直接等于 `p` 那么直接返回 `true`
  
  循环：我们依次将 $x$ 的个位、十位、百位等依次取出，然后 `x` 减去得到的数， `p` 减去得到的数，进行递归
    

## Binary Search

`lower_bound` 和 `upper_bound` 得到的结果**一定是满足条件**的，因此如果最终得到的数是 `end()` ，就说明当前数组当中的所有元素都不满足条件

如果是 `lower_bound` ，就说明当前数组中没有一个数是大于等于给定数的

如果是 `upper_bound` ，就说明当前数组中没有一个数是大于给定数的

[2563. 统计公平数对的数目 - 力扣（LeetCode）](https://leetcode.cn/problems/count-the-number-of-fair-pairs/description/)

