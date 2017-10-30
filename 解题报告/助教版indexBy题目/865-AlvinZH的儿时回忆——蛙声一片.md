# 864 AlvinZH的儿时回忆----蛙声一片

## 思路

中等题。难点在于理解题意！仔细读题才能弄懂题目规则。整个过程是通过模拟位置变化进行的。

第一个问题是AlvinZH的情绪变化，忽略某一位置的青蛙条件是：刚刚经历失败，即前一位置没有抓到青蛙。

第二个问题是什么情况抓到青蛙：不灰心的情况遇到多只青蛙，除去跳的最远的青蛙（可能多只），剩下的都被抓。

## 分析

像这种数据循环利用且不断变化，但是有序的数据集，应该想到使用优先队列。优先队列可以保证所有青蛙按一定顺序排列。

有些同学使用优先队列数组，有点浪费空间。这里只需要一个优先队列即可，具体可看代码注释。

> 考点：优先队列。

> 难点：分析出各种情况，不能乱，逻辑要清晰。

## 参考代码

```c++
//
// Created by AlvinZH on 2017/9/29.
// Copyright (c) AlvinZH. All rights reserved.
//

#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <queue>
#define MaxSize 100005

using namespace std;

struct Frog {
    int pos;//位置
    int dis;//跳程
    bool operator < (const Frog& f) const {
        if(pos != f.pos) return  pos > f.pos;//小值优先
        return dis < f.dis;//大值优先
    }
};

int main()
{
    int T, n;
    Frog nowF;
    scanf("%d", &T);
    while (T--)
    {
        scanf("%d", &n);
        priority_queue<Frog> Q;
        for (int i = 0; i < n; i++) {
            scanf("%d %d", &nowF.pos, &nowF.dis);
            Q.push(nowF);
        }

        int numF = 0;
        bool Fail = false;//初始不灰心
        while (!Q.empty())
        {
            nowF = Q.top();//距离最近且跳的最远的青蛙
            Q.pop();
            if(!Fail)
            {
                Fail = true;
                Frog nextF = Q.top();
                while (!Q.empty() && nextF.pos == nowF.pos)//存在多只
                {
                    Q.pop();
                    if(nextF.dis == nowF.dis)//比翼双飞
                    {
                        nextF.pos += nextF.dis;
                        Q.push(nextF);
                    }
                    else//抓住它
                    {
                        numF++;
                        Fail = false;
                    }
                    nextF = Q.top();
                }
                nowF.pos += nowF.dis;
                Q.push(nowF);
            }
            else//一起忽略，记得pop这些青蛙
            {
                Fail = false;
                Frog nextF = Q.top();
                while (!Q.empty() && nextF.pos == nowF.pos)
                {
                    Q.pop();
                    nextF = Q.top();
                }
            }
        }

        printf("%d %d\n", nowF.pos, numF);
    }
}
```
