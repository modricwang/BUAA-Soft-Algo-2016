# 862-AlvinZH的儿时梦想——运动员篇

## 思路

难题。

应该想到，不管给出的数据如何，每一个淘汰的人不会对最终答案产生任何影响，所以每次淘汰就把人除掉就可以了，最后剩下的两个人计算它们从开始到相遇需要的时间就可以了。

首先对每个人根据初始位置进行排序，因为相遇总是先发生在相邻的两个人身上的，所以一开始先对**相邻的人**两两计算相遇时间，然后把相遇时间放进优先队列里（保证时间短的优先出队），然后依次出队，判定见面的两个人中哪个会被淘汰，然后把淘汰的人除去，维护新建立起来的相邻关系，以及新的相遇时间放进优先队列，一直处理直到队列只剩最后一对，然后取出来计算时间就可以了。

需要使用循环链表记录每一个人的相邻位置是谁，简单使用两个数组即可模拟循环链表。

本题还要注意的是环形跑道，也就是说在计算时间的时候记得相应处理，比如对跑道长度取模。注意看下列的求时间函数：

    double getTime(int rear, int front)
    {
        int dx = (P[front].pos - P[rear].pos + L) % L;//相对距离
        int dv = P[rear].v - P[front].v;//相对速度
        if (dv < 0)//front追rear
        {
            dv = -dv;
            dx = L - dx;
        }
        return (dx * 1.0 / dv);
    }

## 分析

考察的是**优先队列**和**循环链表**。

最初状态环上有n个人，每次淘汰的必然是环上相邻的选手。注意到第一个被淘汰的人不会对后续过程有任何影响，所以找到这个人并把他从状态环上删去，就能把问题变成一个只有n-1人的子问题，此子问题与原问题有相同的答案。

利用优先队列维护状态环上所有相邻的人相遇的时间，每次取出最小值，可以淘汰一人，注意淘汰一人后，原本不相邻的人就相邻了，需要求得新的相遇时间入队，重复这一过程，直到队列剩余元素为1时结束。

考察了大家的模拟能力和手速，代码挺长，想起来还是挺简单的，对吧？

## 参考代码

```c++
//
// Created by AlvinZH on 2017/9/25.
// Copyright (c) AlvinZH. All rights reserved.
//

#include <iostream>
#include <cstdio>
#include <cstring>
#include <queue>
#include <functional>
#include <algorithm>
#define MaxSize 100005
using namespace std;

struct Person {
    int pos,v,power;
    bool operator < (const Person& x) const {
        return pos < x.pos;//按位置从小到大排序，pos小的优先级大（sort函数）
    }
};

struct Race {
    int front,rear;//两个人对应下标
    double time;//相遇时间
    Race(int r = 0,int f = 0,double t = 0.0) {
        rear = r;front = f;time = t;
    }

    bool operator < (const Race& r) const {
        return time > r.time;//相遇时间小的位于队首（优先队列）
    }
};

int n,L;
Person P[MaxSize];
int nextP[MaxSize],lastP[MaxSize];//记录下一个和上一个人的标号
bool isOUT[MaxSize];//记录是否被淘汰
priority_queue<Race> Q;//优先队列

double getTime(int rear, int front)
{
    int dx = (P[front].pos - P[rear].pos + L) % L;//相对距离
    int dv = P[rear].v - P[front].v;//相对速度
    if (dv < 0)//front追rear
    {
        dv = -dv;
        dx = L - dx;
    }
    return (dx * 1.0 / dv);
}

int main()
{
    //freopen("in1.txt", "r", stdin);
    //freopen("out2.txt", "w", stdout);
    int T;
    scanf("%d", &T);
    while (T--)
    {
        scanf("%d %d", &n, &L);

        for (int i = 0; i < n; i++) {
            scanf("%d %d %d", &P[i].pos, &P[i].v, &P[i].power);
        }
        sort(P, P + n);

        while (!Q.empty()) Q.pop();

        for (int i = 0; i < n; i++) {
            double t = getTime(i, (i + 1) % n);
            Q.push(Race(i,(i+1)%n,t));

            lastP[i] = (i - 1 + n) % n;
            nextP[i] = (i + 1) % n;
        }

        int All = n;//剩余比赛人数
        memset(isOUT, false, sizeof(isOUT));

        while (Q.size() > 1)
        {
            Race tmp = Q.top();
            Q.pop();
            int rear = tmp.rear,front = tmp.front;
            if(isOUT[rear] || isOUT[front])
                continue;
            if(lastP[rear] == front && nextP[front] == rear)//剩余最后两人
                break;

            if(P[rear].power > P[front].power)//rear追上front，淘汰front
            {
                isOUT[front] = true;
                nextP[rear] = nextP[front];
                lastP[nextP[front]] = rear;
                double tt = getTime(rear, nextP[front]);
                Q.push(Race(rear, nextP[rear], tt));
            }
            else
            {
                isOUT[rear] = true;
                lastP[front] = lastP[rear];
                nextP[lastP[rear]] = front;
                double tt = getTime(lastP[front], front);
                Q.push(Race(lastP[front], front, tt));
            }

            if(--All <= 0)
                break;
        }

        Race tmp = Q.top();
        Q.pop();
        printf("%.3lf\n", tmp.time);
    }
}

/*
 * 考点：优先队列
 * 坑：数据量较大
 */
```
