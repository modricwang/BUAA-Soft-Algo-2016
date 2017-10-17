# 867 水水的Horner Rule

## 思路

经鉴定，这道题是真水题。题目的意思很简单，给你两个不同进制的数，把他们加起来用十进制表示。

方法：先把两个不同进制的数都转化成十进制，再相加就ok。

## 分析

注意两个简单问题，由于两个数都是在int范围内的正整数（降低了难度），题目已经说明是实际大小在int范围内，也就是说这个数很可能有31位，不过没有关系，我们才不管它多长，用 **字符串读入** 一点问题也没有！而且使用字符串还有一个优点就是我不用去操作这个数，除法or求模什么的。

第二个问题是转化十进制，题目描述中也提供了伪代码方法，采用最小乘法策略，其中的x就是我们的进制数，可以快速转化，另外两个int相加有可能超过int哦！

**思考题：** 本题降低了难度，想一想，如果有可能是负数又该如何处理？超过十进制的进制又如何转换呢？

## 参考代码

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#define MaxSize 100005

using namespace std;

long long ConvertToD(char *s, int H)
{
    long long result = 0;
    for (int i = 0; i < strlen(s); i++) {
        result = result * H + (s[i] - '0');
    }
    return result;
}

int main()
{
    int n;
    while (~scanf("%d", &n))
    {
        int H1, H2;
        char x[32], y[32];
        while (n--)
        {
            scanf("%d %s %d %s", &H1, &x, &H2, &y);

            printf("%lld\n",ConvertToD(x, H1) + ConvertToD(y, H2));
        }
    }
}
```
