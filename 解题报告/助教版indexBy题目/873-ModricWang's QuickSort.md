# 873

## 思路

首先需要向大家道歉，题目描述中的划分函数并没有实现快排的划分函数的完整功能，给大家带来了困扰。在思考此题时，将其看作一种与快排无关的独特的划分方式即可。题目中已经提供了伪代码，只需要将其写成实际代码即可。以后会放出一个此题修复以后的版本给大家。

需要注意的是，后一层递归时的位置划分取决于前一层递归时mid元素最终的位置，因此在划分函数中需要以返回值或其他形式来提供前一次递归时mid元素的位置。同时也要注意，i和j作为两个位置指示符，需要严格管理相对大小，不可以出现划分的两个区域出现重叠的情况。

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
    int mid = arr[n / 2];
    int i = 0, j = n - 1;
    while (i < j) {
        while (arr[i] <= mid && i <= j) i++;
        while (arr[j] > mid && i <= j) j--;
        if (i < j) {
            swap(arr[i], arr[j]);
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
    for (int i = 0; i < n; i++) {
        cin >> nums[i];
    }
    int l, r;
    r = partition(nums, n);
    l = partition(nums, r);

    for (int i = l; i < r; i++) {
        cout << nums[i] << " ";
    }
    cout << "\n";

}
```
