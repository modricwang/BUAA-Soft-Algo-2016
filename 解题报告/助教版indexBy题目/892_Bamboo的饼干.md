#Bamboo的饼干
##分析
从两个数组中各取一个数，使两者相加等于给定值。要注意去重和排序  
难度不大，方法很多，基本只要不大于O(n^2 ) 的都可以过。本意想考察二分搜索  
还可以借助stl中的map，set以及lower_bound等，当然只用数组也可以做。由于数据范围不大，也可以直接用数组下标来计数。  

提起去重，有同学似乎一直纠结 （2,3）和（3,2）算不算重复数对..不算!只有（2,1）（2,1）这样的是真·重复对  

###map
这是很多AC代码用到的方法。因为map的key值是不重复且有序的，因此很适合本题。
#####参考代码
```cpp
int n, t, x;
map<int, int> m;
int A[MaxSize];

int main()
{
    while(~scanf("%d", &n))
    {
        m.clear();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            scanf("%d", &x);
            m[x] = 1;
        }
        for (int i = 0; i < n; ++i) {
            scanf("%d", &A[i]);
        }
        scanf("%d", &t);
        sort(A, A+n);

        for (int i = n-1; i >= 0; --i) {
            long long tem = (long long)t - (long long)A[i];
            if(m[tem] > 0)//查询方便
            {
                ans++;
                printf("%lld %d\n", tem, A[i]);
                m[tem] = 0;
            }
        }

        if(ans == 0) printf("OTZ\n");
        printf("\n");
    }
}
```
###数组
此处**@周宏建**，数组下标计数的方式，与上面map功能相似，注意map键值可为负但是数组下标不可以。当数据范围过大时该方法可能受限。  
下面是这位同学上机时的AC代码：
```cpp
#define bias 10000001
using namespace std;
bool a[20000010];
int b[100005];
int main()
{
	int n,x,target;
	while(~scanf("%d",&n))
	{
		memset(a,0,sizeof(a));
		bool flag=false;
		for(int i=0;i<n;++i)
		{
			scanf("%d",&x);
			a[x+bias]=1;
		}
		for(int i=0;i<n;++i)
		scanf("%d",b+i);
		scanf("%d",&target);
		sort(b,b+n);
		int tmp;
		for(int i=n-1;i>=0;--i)
		{
			tmp=target-b[i]+bias;
			if(tmp>=0&&tmp<20000010&&a[tmp])
			{
				a[tmp]=0;
				flag=true;
				printf("%d %d\n",target-b[i],b[i]);
			}
		}
		if(!flag)
		printf("OTZ\n");
		printf("\n");
	}
}
```
###数组+二分
这也是非常常见的思路。因为只有两个数，确定一个查找另一个，本质就是个查找。当普通查找TLE时应当会想到用二分查找来做。手写和STL均可。  
#####参考代码
```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;
const int maxx = 100004;
int a[maxx],b[maxx];
int BinarySearch(int a[], int l ,int r,int val)
{
    int mid  ;
    while(l<=r)
    {
        mid = (l+r)/2;
        if(a[mid]==val)return mid;
        else if(a[mid]>val)r = mid-1;
        else l  = mid+1;
    }
    return -1;
}
int main()
{
   int n,t,tt,pos;
   while(~scanf("%d",&n))
   {
       for(int i = 0;i<n;i++)
        scanf("%d",&a[i]);
       for(int j = 0;j<n;j++)
        scanf("%d",&b[j]);
       scanf("%d",&t);
       sort(a,a+n);sort(b,b+n);
       bool flag = false;
       for(int i = 0;i<n;i++)
       {
           if(i>0&&a[i]==a[i-1])continue;
           tt = t-a[i];
           pos = BinarySearch(b,0,n-1,tt);
           if(pos>-1&&pos<n)
           {
               printf("%d %d\n",a[i],b[pos]);
               flag = true;
           }
       }
       if(flag==false)printf("OTZ\n");
       printf("\n");
   }
}

```
当然，因为查找一个数，哈希表还能更快

##拓展
请大家思考一下，如果给一个数组找三个数之和为某一值呢？四个数之和呢？