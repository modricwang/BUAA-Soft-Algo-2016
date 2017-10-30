# 872 AlvinZH的儿时梦想----坦克篇

## 思路

简单题。仔细看题，题目意在找到直线穿过的矩形数最小，不能从两边穿过。那么我们只要知道每一行矩形之间的空隙位置就可以了。

如果这里用二维数组记住每一个空隙的位置，一是没有必要，二是记录了还要大量的处理才能得到答案。反正我是没想过要怎么处理。

可以发现，要得到本题的答案，只要找到空隙最多的哪个位置，我们取左边参考点，每一行的空隙位置我们可以记录到同一个数组里，即用A[pos]代表pos位置的直线有多少个空隙。但是发现总长度有点大，用数组是不可能了，有没有什么东西可以存下我这样的数对呢？

答案是map或轻便的pair。map的使用方法之前公邮里给大家发过了，不知道大家有没有好好学习。至于对组（pair），是一个稍微封装了一下的结构体模板，可以花一分钟看一下什么是[对组](http://www.cnblogs.com/ttzm/p/5881003.html)。有着这个这题就简单了，两个值一个记录位置，一个记录出现次数，最后找到最大出现次数，n减去此数便可得到答案。具体可参考参考代码一。

非要这样做吗？？？我不会STL就做不了？

不是的，为什么非要把那么大的数当做索引呢？我就想把它当做数组的值，那下标是什么呢？答案是从0自增的一个计数变量。即A[cnt]记录空隙出现的位置，我们将它排序一下，相同位置会被放在一起，统计相同值的区间跨度一样可以找到空隙出现次数的最大值。具体见参考代码二。

## 分析

解法一使用map，时间复杂度将达到 $O(NMlogNM)$ 。

解法二由于需要手动排序，时间复杂度一样是 $O(NMlogNM)$ 。

## 参考代码一

```c++
//
// Created by AlvinZH on 2017/10/8.
// Copyright (c) AlvinZH. All rights reserved.
//

#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <map>
using namespace std;

int main()
{
    //freopen("in.txt", "r", stdin);
    //freopen("out.txt", "w", stdout);
    int n, m, x;
    while(~scanf("%d %d", &n, &m))
    {
        map<int, int> sameSum;//统计相同和的个数

        for (int i = 0; i < n; ++i) {
            int sum = 0;
            for (int j = 0; j < m; ++j) {
                scanf("%d", &x);
                if(j == m - 1) continue;
                sum += x;
                sameSum[sum]++;
            }
        }
        int maxSame = 0;
        for(map<int, int>::iterator it = sameSum.begin(); it != sameSum.end(); it++)
            if(maxSame < it->second) maxSame = it->second;

        printf("%d\n", n - maxSame);
    }
}
```

## 参考代码二

```c++
/*
 Author: 蒋泳波(41)
 Result: AC    Submission_id: 343393
 Created at: Thu Oct 26 2017 14:48:19 GMT+0800 (CST)
 Problem: 872    Time: 106    Memory: 5292
*/

#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
const int maxn = 1e6 + 10;

int a[maxn],cnt,x,n,m;

int main()
{
    while(~scanf("%d%d",&n,&m))
    {
        cnt = 0;
        for(int i = 1; i <= n; i++)
        {
            int now = 0;
            for(int j = 1; j < m; j++)
            {
                scanf("%d",&x);
                now += x;
                a[cnt++] = now;
            }
            scanf("%d",&x);
        }
        sort(a,a+cnt);
        int last = 0,ans = 0;
        a[cnt] = -1;//注意初始化值
        for(int i = 1; i <= cnt; i++)
        {
            if(a[i] != a[i-1])//找到分界位置
            {
                if(ans < i - last)
                    ans = i - last;
                last = i;
            }
        }
        printf("%d\n",n - ans);
    }
    return 0;
}
```
