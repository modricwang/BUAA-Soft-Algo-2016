# 864 AlvinZH的儿时梦想----机器人篇

## 思路

中等题。

判断无限玩耍： $p$ 的值能够承担的起所有机器的消耗。即比较 $\sum_{i=1}^{n}a_i$ 与 $p$ 的大小。

## 分析

本题有两种解法。

方法一：二分。

> 即二分枚举最长时间，判断充电器能否支撑此时间所有的耗能，不断缩小时间范围，最后可求得答案。难点在判断函数，比较在当前时间条件下，所有需要充电的机器需要的能量能量总和与充能器提供的能量总和，每次需要遍历一次数组。

方法二：贪心

> 感谢李析航同学。这道题还有另外一种想法，结构体排序+贪心。

> 把所有机器在不充电时能用的时间算出来(假设第 $i$ 个设备能用 $t_i$ 秒)，对时间进行**升序排序**。让时间短的设备先充电，让尽可能多的机器能用更长的时间，这就是贪心所在。

> 假设已经保证**前m个机器**玩耍时间达到 $t_m$（即所有机器至少可以玩耍 $t_m$，初始值为 $t_0$ ），前m个机器耗能效率 $A_m=\sum_{i=1}^{m}a_i$ 。现在判断充电能否让玩耍时间延长到 $t_{m+1}$ ，如果不能，说明答案 $ans$ 就在 $t_m$ 和 $t_{m+1}$ 之间。

> 由于已经满足前m个机器玩耍时间达到 $t_m$，为了方便计算，我们假设充电器一直在充电，那么多余的电量设为 $power$，令 $x=ans-t_m$，则有 $power+p_x=A_m$ 。解得 $x=\frac{power}{A_m-p}$ ，$ans=x+t_m$。

> 还有一种情况，就是已经满足n的机器玩耍时间达到 $t_n$ 了，这时可以直接计算x就可以得到答案了。

## 参考代码（解法一）

```c++
//
// Created by AlvinZH on 2017/9/30.
// Copyright (c) AlvinZH. All rights reserved.
//

#include<iostream>
#include<cstdio>
#include<vector>
#include<cstring>
#include<map>
#include<cstdlib>
#include<cmath>
#include<string>
#include<algorithm>
#include<set>
#include<stack>
#include<queue>
#include<utility>
#define INF 0x3f3f3f3f
#define eps 1e-8
#define MaxSize 100005
typedef long long LL;

using namespace std;

int n, p;
int A[MaxSize], B[MaxSize];

bool judge(double t)
{
    double inPower = 1.0 * p * t;//可充电总量
    double needPower;
    for (int i = 0; i < n && inPower > eps; i++) {
        needPower = A[i] * t - B[i];//每个机器需要的充电量
        if (needPower > eps)
            inPower -= needPower;
    }
    if (inPower < eps) return false;
    else return true;
}

int main()
{
    while (~scanf("%d %d", &n, &p))
    {
        LL sumA = 0;
        for (int i = 0; i < n; i++) {
            scanf("%d %d", &A[i], &B[i]);
            sumA += (LL)A[i];
        }

        if (sumA <= p) printf("Great Robot!\n");
        else
        {
            double ltime = 0.0, rtime = 1e12, mid, ans;
            while (rtime - ltime >= eps)
            {
                mid = (ltime + rtime) / 2.0;
                if (judge(mid))
                {
                    ltime = mid;
                    ans = mid;
                }
                else rtime = mid;
            }

            printf("%.3lf\n", ans);
        }
    }
}
```

## 参考代码二

```c++
//
// Created by AlvinZH on 2017/9/30.
// Copyright (c) AlvinZH. All rights reserved.
//

#include<iostream>
#include<cstdio>
#include<vector>
#include<cstring>
#include<map>
#include<cstdlib>
#include<cmath>
#include<string>
#include<algorithm>
#include<set>
#include<stack>
#include<queue>
#include<utility>
#define INF 0x3f3f3f3f
#define eps 1e-8
#define MaxSize 100005
typedef long long LL;

using namespace std;

int n, p, b;
struct Robot {
    int a;
    double t;

    bool operator < (const Robot& r) const {
        return t < r.t;
    }
};

Robot R[MaxSize];

int main()
{
    //freopen("in.txt", "r", stdin);
    //freopen("outme.txt", "w", stdout);
    while (~scanf("%d %d", &n, &p))
    {
        LL sumA = 0;
        for (int i = 0; i < n; i++) {
            scanf("%d %d", &R[i].a, &b);
            sumA += (LL)R[i].a;
            R[i].t = b * 1.0 / R[i].a;
        }

        if (sumA <= p) printf("Great Robot!\n");
        else
        {
            sort(R, R + n);
            double ans = R[0].t;//最小玩耍时间
            double power = R[0].t * p;//初始剩余能量
            double timeX;//时间差
            double x;//续命时间
            sumA = R[0].a;//需要充电机器的总功率

            int i = 1;
            while(i < n)
            {
                timeX = R[i].t - ans;
                if(sumA > p)//需要充电
                {
                    x = power / (sumA - p);
                    if(x <= timeX)//无法坚持到R[i]充电
                    {
                        printf("%.3f\n", ans + x);
                        break;
                    }
                }

                power = power + timeX * (p - sumA);//计算剩余能量
                sumA += R[i].a;
                ans = R[i].t;
                i++;
            }
            if(i == n)//全部机器都要充电
            {
                x = power / (sumA - p);
                printf("%.3f\n", ans + x);
            }
        }
    }
}
```
