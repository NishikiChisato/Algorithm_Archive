# MISC

- [MISC](#misc)
  - [求所有长度大于等于 L 的子段和的最大值](#求所有长度大于等于-l-的子段和的最大值)


## 求所有长度大于等于 L 的子段和的最大值

给定一个序列 $A$ ，求某个长度大于等于 $L$ 的子段，使得其子段和最大

转换为求前缀和，设 $sum[i]$ 表示前 $i$ 个数的前缀和：

$$
\max_{i-j\ge L}\{A_{j+1}+A_{j+2}+\cdots+A_{i}\}=\max_{L\le i \le n}\{ sum[i] - \min_{1\le j\le i-L}\{sum[j]\} \}
$$

$i$ 每增大一次，就会有一个新值进入 $\min\{sum[j]\}$ 的候选集合，而该集合表示 $sum$ 数组中从第一个数到第 $i-L$ 个数的最小值

因此我们可以记录一个前缀最小值，每次先于新进入候选区间的值比较大小，随后再对其计算即可：

```
int ans = -1e9, minv = 1e9
for(int i = L; i <= n; i ++)
{
    minv = min(minv, sum[i - L]);
    ans = max(ans, sum[i] - minv);
}
```

参考：[AcWing 102. 最佳牛围栏](https://www.acwing.com/problem/content/104/)

