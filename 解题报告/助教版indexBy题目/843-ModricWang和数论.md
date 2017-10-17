# 843 ModricWang和数论

## 思路

设$m = a \% b$, 分为3种情况：

1.  $b<a$时，$m$一定在 $1.. \lfloor \frac{a-1}{2} \rfloor$ 之间，共 $\lfloor \frac{a-1}{2} \rfloor$个数
2.  $b=a$时，$m=0$
3.  $b>a$时，$m=a$

因此，共 $\lfloor \frac{a-1}{2} \rfloor+2$ 个 $m$，也就是 $\lfloor \frac{a+1}{2} \rfloor+1$

时间复杂度$O(1)$，空间复杂度$O(1)$
## 代码

```c++
#include <iostream>

using namespace std;

int main() {
    long long a;
    cin >> a;
    cout << (a + 1) / 2 + 1;
    return 0;
}
```
