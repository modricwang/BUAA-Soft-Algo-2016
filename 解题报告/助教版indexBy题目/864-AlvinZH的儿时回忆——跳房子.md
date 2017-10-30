# 864 AlvinZH的儿时回忆----跳房子

## 思路

这是一道简单题，但是同样有人想复杂了，DP？大模拟？。

本题只要判断能不能到达最后一个格子，又没有问方法数。所以，只需要一个单变量rightMost记录最远能到达的地方，遍历一次数组后比较其与n的大小即可。

## 分析

经典染色问题的转化：可以把跳格子的过程看成是染色，设在某一时刻，index=m的位置已经被染色了，那么 index=n (n&lt;=m) 的位置肯定已经被染色过了，我们维护一个最右边被染色的点，如果当前枚举点在该点的左侧，那么当前点已经被染色，否则即可停止遍历（因为右边的点再也不可能被染色到了）。最后判断最后一格是否被染色即可解决此问题。

本题可参考：[leetcode 55.Jump Game 的三种思路](http://www.cnblogs.com/zichi/p/4808025.html)

> 时间复杂度：$O(N)$。
>
> 空间复杂度：$O(N)$;

## 参考代码

```c++
//
// Created by AlvinZH on 2017/9/28.
// Copyright (c) AlvinZH. All rights reserved.
//

#include <cstdio>
#define MaxSize 100005

int T,n;
int Step[MaxSize];

int main()
{
    scanf("%d", &T);
    while (T--)
    {
        scanf("%d", &n);
        for (int i = 1; i < n; i++) {
            scanf("%d", &Step[i]);
        }

        int rightMost = 1;
        for (int i = 1; i < n; i++) {
            if(rightMost < i) break;
            rightMost = rightMost > i + Step[i] ? rightMost : i + Step[i];
        }

        if(rightMost >= n) printf("I Win!\n");
        else printf("Too Far!\n");
    }
}
```
