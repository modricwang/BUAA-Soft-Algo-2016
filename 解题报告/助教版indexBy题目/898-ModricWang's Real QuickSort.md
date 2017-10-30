# 873

## 思路

这是一道非常基础的题，目的是帮助大家回顾快排相关的知识。大家完成此题之后应该就对快排有比较深刻的印象了。

对于整个快排的流程，题目描述中已经给了清晰完整的伪代码。需要自己加工的部分就是，需要手动记录下每次划分后的分界线，也就是划分时的变量$i$。

由于数据较为简单，要求的层数也较浅，实现划分函数后手工调用即可。

时间复杂度$O(n)$，空间复杂度$O(n)$

## 代码
```cpp
#include <iostream>

using namespace std;

const int MaxN = (int) 1e7 + 10;

int n;
int nums[MaxN];

int partition(int *arr, int n) {
	int mid = arr[n/2];
	int i = 0, j = n - 1;
	while (i <= j) {
		while (arr[i] < mid) i++;
		while (arr[j] > mid) j--;
		if (i <= j) {
			swap(arr[i], arr[j]);
			i++;
			j--;
		}
	}
	return i;
}

int main() {
#ifdef ONLINE_JUDGE
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
#endif

	cin >> n;
	for (int i = 0; i < n; i++)
		cin >> nums[i];

	int r = partition(nums, n);
	int l = partition(nums, r);

	for (int i = l; i < r; i++)
		cout << nums[i] << " ";

	cout << "\n";

}
```
