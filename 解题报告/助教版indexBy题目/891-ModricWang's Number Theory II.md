# 891 ModricWang's Number Theory II

## 思路
使得序列的最大公约数不为1，就是大于等于2，就是找到一个大于等于2的数，它能够整除序列中的所有数。
考虑使得一个数d整除数组中所有数的代价：

如果一个数不能被b整除，那么可以花费x的代价删掉它，或者通过多次加1使得它可以被d整除，代价应该为 $(d - a[i]\%d) * y$  ,  $(a[i] \% d == 0s时特判，应该为0)$

令 $l = x / y$

如果$d - a[i] \% d <= l$  $(a[i]\%d != 0)$, 这个数产生的代价是 $(d - a[i] \% d) * y$ , 否则是$x$。

所有代价求和就是总代价，最小的总代价就是答案。

但是这样枚举了d和a[i], 复杂度是$O(n^2)$ 的。
考虑将a[i]换一种方式存储：b[i]表示值为i的数出现的次数。
这样d可以将b分成如下若干段：

$[0, d - 1], [d, d * 2 - 1], [d * 2, d * 3 - 1], ... ,[d * i, d * (i + 1) - 1]$

对于每一段而言，
$[d * (i + 1) - l, d * (i + 1) - 1]$ 内的数应该通过多次加1变成$d * (i + 1)$ ,

代价应为 $(该区间内数的个数 * d * (i + 1) - 该区间内的数之和) * y$

$[d * i + 1 , d * (i + 1) - l - 1]$ 内的数应该直接删除,

代价应为 $该区间内的个数 * x$

通过构造相应的前缀和数组，上述操作均可以在$O(1)$ 的时间复杂度内完成

具体操作时应该注意边界

因为合数会被质数整除，因此d可以只枚举质数。

计算时间复杂度需要一些数论知识。首先素数密度(也就是 $\frac{小于n的素数}{n}$ )可以参见[oeis A006880](https://oeis.org/A006880),一个近似解析式为 $\frac{1}{ln(n)}$，那么$小于n的素数的总个数$可以近似为 $\frac{n}{ln(n)}$

设小于等于n的素数为$prime[i]$，素数总数为$P$,取近似$P=\frac{n}{ln(n)}$

求结果部分的复杂度可以写为 $\sum_{1}^{P} \frac{n}{prime[i]}$

参见[wikipedia](https://zh.wikipedia.org/wiki/%E7%B4%A0%E6%95%B0%E7%9A%84%E5%80%92%E6%95%B0%E4%B9%8B%E5%92%8C),素数的倒数和又可以近似为 $\sum_{1}^{p} \frac{1}{prime[i]}=ln(ln(n))$

因此 $\sum_{1}^{P} \frac{n}{prime[i]} = O(n* ln(ln(n)))$

这里得到了计算结果部分的复杂度，还需要加上求素数这个过程的时间复杂度。如果使用朴素筛法，求复杂度的过程正好的上文所述的完全一致，其复杂度为$O(n*ln(ln(n)))$。如果使用欧拉筛求素数，复杂度为$O(n)$。

因此$O(运行时间)=O(求素数)+O(计算结果)=O(n*ln(ln(n)))$

## 代码
```c++
#include<iostream>
#include<cstring>

using namespace std;

const long long Max_Ai = 1000000*2;
long long n, x, y, l;
long long nums[Max_Ai + 10];
long long s[Max_Ai + 10], sum[Max_Ai + 10];

bool valid[Max_Ai + 10];
long long prime[Max_Ai + 10];
long long tot;

//线性筛求素数
void init_prime() {
	memset(valid, true, sizeof(valid));

	for (int i = 2; i <= Max_Ai; i++) {
		if (valid[i]) prime[++tot] = i;

		for (int j = 1; j <= tot && i*prime[j] <= Max_Ai; j++) {
			valid[i*prime[j]] = false;
			if (i%prime[j]==0) break;
		}
	}
}

int main() {
#ifdef ONLINE_JUDGE
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
#endif

	init_prime();

	cin >> n >> x >> y;
	l = x/y;
	for (long long i = 1; i <= n; i++) {
		long long p;
		cin >> p;
		nums[p]++;    //这是一种比较特别的数字记录方法，原理类似于基数排序radix sort
	}

	for (long long i = 1; i <= Max_Ai; i++) {
		s[i] = s[i - 1] + nums[i];    //数量和
		sum[i] = sum[i - 1] + nums[i]*i;    //前缀和
	}

	auto min_cost = (long long) 1e18;
	for (long long i = 1; i <= tot; i++) {
		long long k = prime[i];
		long long now_cost = 0;

		for (long long j = 0; j <= Max_Ai; j += k) {
			long long mid = max(j + k - l - 1, j);
			long long bound = min(j + k - 1, Max_Ai);

			if (bound > mid) {
				now_cost += ((s[bound] - s[mid])*(j + k) - (sum[bound] - sum[mid]))*y;
				now_cost += (s[mid] - s[j])*x;
			} else {
				now_cost += (s[bound] - s[j])*x;
			}
		}

		min_cost = min(min_cost, now_cost);
	}

	cout << min_cost << "\n";

	return 0;
}
```
