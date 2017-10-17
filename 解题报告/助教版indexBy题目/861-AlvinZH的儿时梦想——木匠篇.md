# 861 AlvinZH的儿时梦想--------木匠篇

## 前言

由于助教的失误，本题数据的精度出现严重问题，以为取10位小数完全没问题，考虑不周，望见谅。

本题当中，PI最好取值的方法是 $PI = acos(-1.0)$。

## 思路

中等题。先知道题目要求的是什么： $V = PI _(x_1 - x_2)^2 / 4_ min(h_1,h_2)$ 。PI我们可以先不管，其实就是在找 $（x_1 - x_2)^2* min(h_1,h_2)$ 的最大值。

本题数据很弱，数据范围也都很小，所以有多种方法解题，甚至可以试试暴力求解。

## 分析

> 方法一：暴力解法 本题由于n、x和h都很小，所以暴力解法（直接遍历所有组合）没问题，虽然不是助教的初衷，助教表示不会出题，啊啊啊~

> 方法二：从位置x的角度出发，移动指针法

描述：采用两个变量分别代表最左和最右的木条，由两边向中间靠拢，移动较短的一边，直至靠拢到圆心处。

证明：当左端线段L小于右端线段R时，我们把L右移，这时舍弃的是L与右端其他线段（R-1, R-2, ...）组成的木桶，这些木桶是没必要判断的因为这些木桶的容积肯定都没有L和R组成的木桶容积大。

具体做法：可以有两种做法，一是开一个数组 $H[205]$ ， $H[i]$ 代表 $i-100$ 位置木条的高度，注意这种下标的处理方式，在题目"四和归零"中也有用到。左右指针初始化为0、200，向100靠拢。另一种做法是使用STL容器map， $m[i]$ 表示位置i木条的高度，这样可以不用考虑高度为0的地方。

具体可参考参考代码一。

> 方法三：从高度的角度出发，遍历高度。

描述：利用两个数组，下标代表高度，值分别为左右两边同一高度离圆心最远的位置。遍历一次高度数组，即可求得最大值。

具体可参考参考代码二（感谢郭镕昊dalao）。

## 参考代码一

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <map>
#include <algorithm>

using namespace std;
const double PI = acos(-1.0);

int n, pos, h;
map<int, int> m;

int main()
{
    //freopen("in2.txt", "r", stdin);
    //freopen("outme.txt", "w", stdout);
    while (~scanf("%d", &n))
    {
        m.clear();
        for (int i = 0; i < n; i++)
        {
            scanf("%d %d", &pos, &h);
            m[pos] = h;
        }

        double ans = 0.0;
        map<int, int>::iterator m_Head = m.begin();//最左边木条
        map<int, int>::iterator m_Tail = prev(m.end());//最右边木条

        while(m_Head != m_Tail && m_Head->first <= 0 && m_Tail->first >= 0)
        {
            double r = (m_Head->first - m_Tail->first) / 2.0;//半径
            int h = min(m_Head->second, m_Tail->second);

            ans = max(ans, r * r * h);

            if(m_Head->second <= m_Tail->second && m_Head->first < 0)
                advance(m_Head, 1);
            else if (m_Tail->second <= m_Head->second && m_Tail->first > 0)
                advance(m_Tail, -1);
            else if (m_Head->first == 0 && m_Tail->first > 0)
                advance(m_Tail, -1);
            else if (m_Tail->first == 0 && m_Head->first < 0)
                advance(m_Head, 1);
        }

        printf("%.3lf\n", ans * PI);
    }
}
```

## 参考代码二

```c++
/*
 Author: 郭镕昊(12622)
 Result: AC    Submission_id: 318166
 Created at: Fri Oct 13 2017 20:14:13 GMT+0800 (CST)
 Problem: 861    Time: 79    Memory: 2696
*/

#include<bits/stdc++.h>

const double pi = std::acos(-1.0);
const int N = 104;
inline void Max(int &a, int b) {
    if(b > a) a = b;
}
int n, a[N], b[N];

int main() {
    while(~scanf("%d", &n)) {
        memset(a, -1, sizeof a);
        memset(b, -1, sizeof b);
        int tmp = -1;
        for(int i = 1, x, h; i <= n; ++i) {
            scanf("%d%d", &x, &h);
            if(!x) Max(tmp, h);//记录0位置高度
            if(x > 0) Max(a[h], x);//右边
            if(x < 0) Max(b[h], -x);//左边
        }
        int x = -1, y = -1, ans = 0;
        for(int h = 100; h; --h) {
            Max(x, a[h]);
            Max(y, b[h]);
            if(x > 0 && y > 0) Max(ans, h * (x + y) * (x + y));
            if(tmp > 0 && x > 0) Max(ans, std::min(h, tmp) * x * x);
            if(tmp > 0 && y > 0) Max(ans, std::min(h, tmp) * y * y);
        }
        printf("%.3lf\n", pi * ans / 4.0);
    }
    return 0;
}
```
