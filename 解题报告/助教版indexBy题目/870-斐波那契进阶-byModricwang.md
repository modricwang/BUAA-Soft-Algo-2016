# 870 斐波那契进阶

## 思路

斐波那契数列在取模意义下具有周期性。因此可以找到这个数列在这个模下的循环节，然后把数列元素的计算范围缩小到循环节内。

## 代码

```c++
#include <iostream>

using namespace std;
const int Mod = 10007;
const int Magic = 20016;
int arr[Magic + 10];

int main() {
    arr[0] = 0;
    arr[1] = 1;
    for (int i = 2; i < Magic+10; i++) {
        arr[i] = arr[i - 1] + arr[i - 2];
        if (arr[i] >= Mod) arr[i] -= Mod;
    }
    int x;
    while (cin >> x) {
        cout << arr[x % Magic] << "\n";
    }
}
```

## 附录

### 如何找循环节？

(1)斐波那契数列每一项的值只和前两项有关。因此只要找到在取模意义下第二次出现的相邻的0，1，这就是第二个循环节的开始。

(2)循环节一定会在$n^2$内出现，通常会在$n$的几倍之内出现。

对于(2)的证明其实也很简单。如果对于$i\\neq j$出现$a_i=a_j$ 且 $a_{i+1}=a_{j+1}$，那么循环节长度必然是 $\\left|i-j\\right|$ 的因子。因此如果找到最近的一对 $i,j$ ，那么循环节找到了。而 $a_i，a_{i+1}$ 的组合只有 $n^2$ 个，因此循环节最长为$n^2$

```cpp
#include <iostream>

using namespace std;
const int Mod = 10007;
const long long upLim = (long long) Mod * Mod;
int now, prev1, prev2;

int main() {
    prev1 = prev2 = 0;
    now = 1;
    for (int i = 2; i < upLim; i++) {
        prev2 = prev1;
        prev1 = now;
        now = prev1 + prev2;
        if (now >= Mod) now -= Mod;
        if (prev1 == 0 && now == 1) {
            cout << i - 1 << "\n";
            return 0;
        }
    }
}
```

## 参考内容

#### [Pisano periods (Period of Fibonacci numbers mod n)](http://oeis.org/A001175)

#### [Pisano period From Wikipedia](https://en.wikipedia.org/wiki/Pisano_period)
