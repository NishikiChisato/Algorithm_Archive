# MISC

- [MISC](#misc)
  - [求所有长度大于等于 L 的子段和的最大值](#求所有长度大于等于-l-的子段和的最大值)
  - [均分纸牌](#均分纸牌)


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

## 均分纸牌

有 $M$ 个人排成一行，每个人手中都有 $C[i]$ 张纸牌

在每一步操作中，某个人可以将自己手指的**一张**牌交给**旁边**的人，求至少需要多少次交换才能使得**所有人手中的纸牌相等**

我们设纸牌的总数为 $T$，显然只有在 $T/M$ 为零时该问题才有解

我们考虑第一个人，情况如下：

* 若 $C[1]\gt T/M$，那么第一个人可以将 $C[i]-T/M$ 张牌交给第二个人
* 若 $C[1]\lt T/M$，那么第一个人要从第二个人那里拿 $C[i]-T/M$ 张牌

更一般地，如果想让前 $i$ 个人的纸牌数相同（如果可以的话），需要交换的次数为：

$$
\lvert C[1]-T/M \rvert + \lvert C[2]-T/M \rvert + \cdots + \lvert C[i]-T/M \rvert
$$

整理一下就是：

$$
\sum_{i=1}^{M}\lvert i\times \frac{T}{M}-G[i] \rvert \ ,\ 其中G[i] 为 C[i] 的前 i 项和
$$

如果我们初始让 $C[i]$ 中的每个数都减去 $T/M$，得到数组 $A[i]$，即 $A[i]=C[i]-T/M$，记 $S[i]$ 为 $A[i]$ 的前 $i$ 项和，有：

$$
\sum_{i=1}^{M}\lvert i\times \frac{T}{M}-G[i] \rvert=\sum_{i=1}^{M}\lvert S[i] \rvert 
$$

代码如下：

```cpp
int n, T, C[N], S[N];
cin >> n;
for(int i = 1; i <= n; i ++) 
    cin >> C[i], T++;
for(int i = 1; i <= n; i ++)
    S[i] = S[i - 1] + C[i] - T / n;
int ans = 0;
for(int i = 1; i <= n; i ++)
    ans += abs(S[i]);
```

进一步，如果是**环形**均分纸牌问题，也就是最后一个人可以与第一个人交换纸牌，那么最优解一定为：**存在相邻的两个人之间没有交换纸牌**

我们可以将这个环断开，一共有 $M$ 种方式，我们只需要在这 $M$ 种中取最小值即可，断开点说明「相邻」的两个人之间没有交换纸牌

假设我们在第 $k$ 个人处断开，此时序列 $A[i]$ 与前缀和 $S[i]$ 为：

$$
\begin{align*}
\begin{array}{l}
&A[k+1], &A[k+2],\cdots, &A[M], &A[1], &A[2], \cdots,&A[k]\\
&S[k+1]-S[k],&S[k+2]-S[k],\cdots,&S[M]-S[k],&S[M]-S[k]+S[1],&S[M]-S[k]+S[2],&S[M]-S[k]+S[k]
\end{array}
\end{align*}
$$

对于 $A$ 的前缀和序列 $S$ 而言，有 $S[M]=0$，因此对应每个 $i$，其前缀和都为 $S[i]-S[k]$，因此总的最小操作次数为：

$$
\sum_{i=1}^{M}\lvert S[i] - S[k]\rvert\
$$

这其实就是[货仓选址](https://www.acwing.com/problem/content/106/)的问题，我们将 $S$ 数组从小到大排序，那么使得上式最小的 $k$ 的取值在 $S[i]$ 序列的中点处

具体地，我们 $S[i]$ 下表从 $1$ 开始，若长度为奇数，那么取 $(N+1)/2$ 为最优解；若长度为偶数，那么任取 $N/2\sim N/2+1$ 之间的任意数均为最优解

在代码实现上，我们统一使用 $(N+1)/2$ 即可包含所有情况

```cpp
int n, T, C[N], S[N];
cin >> n;
for(int i = 1; i <= n; i ++) 
    cin >> C[i], T++;
for(int i = 1; i <= n; i ++)
    S[i] = S[i - 1] + C[i] - T / n;
int ans = 0;
for(int i = 1; i <= n; i ++)
    ans += abs(S[i] - S[(n + 1) / 2]);
```

参考：[AcWing 105. 七夕祭](https://www.acwing.com/problem/content/107/)

