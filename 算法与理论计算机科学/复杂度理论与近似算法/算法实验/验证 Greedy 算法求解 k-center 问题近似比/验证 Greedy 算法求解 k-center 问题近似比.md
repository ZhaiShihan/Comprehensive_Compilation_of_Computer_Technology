#### 一、问题描述
##### · K-center 问题的描述：
输入一个无向图 $G=(V,E)$，$V$ 中每一对定点 $i,j$ 之间的距离为 $d_{ij}$；假定 $d_{ii}=0$ 且 $d_{ij}=d_{ji}$，并且这个距离符合几何中的三角不等式，给定常数 $k$；目标是找到 $k$ 聚类将最相似的顶点分组到一起，选择顶点 $V$ 的子集 $S$，使得 $\lvert S\rvert=k$，每个顶点作为最近的聚类中心，将顶点分组到 $k$ 个不同的聚类中；目标是将所有顶点到聚类中心的最大距离 $r$ 最小化

##### · k-center 问题的数学描述：
$$S\ 中每个点到\ i\ 的最小距离:d(i,S)=min_{(j\in S)}\ d_{ij}\ ——(1)$$$$S\ 中最大半径(求解目标):r=max_{(j\in S)}\ d(i,S)\ ——(2)$$

##### · 实验目的：
实现 k-center 问题的 greedy 算法，来验证 greedy 算法求解 k-center 的近似比

#### 二、算法设计
##### · 定义问题：
将问题限制在二维平面，第一象限 100×100 的坐标范围内，且所有点坐标都为整数；首先验证 greedy 算法的近似比，利用 C++ 程序设计一个求解二维 k-center 问题的 greedy 算法（初始数据按照要求随机生成），再利用暴力破解（brute-force）方法求得问题的最优解 OPT，验证以每一个点为 greedy 初始点得到的各解中没有大于 2×OPT 的，以此说明 2 - 近似性

##### · Greedy 算法求解思路：
1. 从 $V$ 中选择任意一点 $i$ 放入中心集 $S$ 中；
2. 下一个 center 要尽可能远离所有的已有 center，即选择距离已覆盖点最远的未覆盖点作为下一个中心点并放入 $S$；
3. 重复操作，直到 $\lvert S\rvert=k$

· 算法伪代码如下：
~~~
Pick arbitrary i∈V
S ← {i}
while |S|＜k do
	j ← arg max{j∈V} d(j,S)
	S ← S∪{j}
~~~

##### · 暴力破解算法：
1. 枚举所有可能的 k-center 组合，即任意选取 $k$ 个点进行遍历式考察，递归实现；
2. 对每个组合计算其最大覆盖距离 $max_{(j\in S)}\ d(i,S)$；
3. 选择最小的最大覆盖距离作为解

· 算法伪代码如下：
~~~
Pick arbitrary S
	r'= max{j∈V} d(i,S)
	r = r > r'? r': r
Cyclic repitition
~~~

##### · “k-center” 算法代码：

```C++
#include<iostream>
#include<cmath>
#include<cstdlib>
#include<ctime>
using namespace std;

//限制在二维平面，第一象限 100 ×100 的坐标范围内，且所有点坐标都为整数
//验证 k-center 问题的 greedy 算法的近似比为 2
//只需要依次输入点的个数、k 值两个数据即可，点的坐标随机生成

/*定义变量*/
int number_of_points=0;    //点的个数
int in_center_set;    //选入 k-center 集 S 的点的个数

typedef struct
{
	int x;
	int y;
	bool choose_to_S=false;
} stu;
stu points[100];


/*问题参数*/
int k;    // k-center 中心个数
double r;    // 表示输出解的半径值


/*输入初始点坐标*/
void generate_point(int number_of_points)
{
	srand(time(0));
    for (int i=1; i<=number_of_points; i++)
	{
		points[i].x=rand()%100;
		points[i].y=rand()%100;
		cout<<i<<" ("<<points[i].x<<", "<<points[i].y<<')'<<endl;
    }
    cout<<endl;
}


/*计算两个点之间的距离*/
double distance(int a, int b)
{
	return (double)sqrt((double)(points[a].x-points[b].x)*(points[a].x-points[b].x)
	+(double)(points[a].y-points[b].y)*(points[a].y-points[b].y));
}

/*计算每个点到 S 的最小距离 min_d(i,S) */
double min_d_iS(int i)
{
	double d=999;
	for(int j=1; j<=number_of_points; j++)
	{
		if(points[j].choose_to_S==true)
		{
			d=(d>distance(i,j)? distance(i,j): d);
		}
	}
	return d;
}


/*初始化所有点都没有选入 S */
void initial(int number_of_points)
{
	in_center_set=0;
	r=0;
	for(int i=1; i<=number_of_points; i++)
	{
		points[i].choose_to_S=false;
	}
	return;
}


/* greedy 求解 k-center 的解*/
void find_k_center(int k, int number_of_points, int ii)
{
	points[ii].choose_to_S=true;    //起始点
	in_center_set+=1;
	
	while(in_center_set<k)
	{
		double d=0;
		int p=0;    //追踪下一个目标 center
		for(int j=1; j<=number_of_points; j++)
		{
			if(points[j].choose_to_S==false)
			{
				if(d<=min_d_iS(j))    //寻找 min_d(i,S) 中的最大值
				{
					d=min_d_iS(j);
					p=j;
				}
			}
		}
		points[p].choose_to_S=1;
		in_center_set+=1;
	}
}


/*求解半径 r */
double radius()
{
	for(int i=1; i<=number_of_points; i++)
	{
		if(points[i].choose_to_S==false)
		{
			r=(r<min_d_iS(i)? min_d_iS(i): r);
		}
	}
	return r;
}


int p[100];    //暴力搜索用于暂存选择结果

int main()
{
	cin >> number_of_points >>k;
	cout<<endl<<"points:"<<endl;
	generate_point(number_of_points);
	for(int i=1; i<=number_of_points; i++)    //逐个作为起始选择点试验
	{
		initial(number_of_points);
		find_k_center(k, number_of_points, i);
		cout<<"k-center start from point "<<i<<" : r = "<<radius()<<endl;
		for(int j=1; j<=number_of_points; j++)
		{
			if(points[j].choose_to_S==true)
			{
				cout<<j<<" ("<<points[j].x<<", "<<points[j].y<<')'<<endl;
			}
		}
		cout<<endl;
	}
	
	initial(number_of_points);    //暴力搜索
	in_center_set=0;
	r=999;
	void brute_force(int k, int number_of_points);
	brute_force(k, number_of_points);
	cout<<"OPT: r = "<<r<<endl;
	for(int j=1; j<=k; j++)
	{
		cout<<p[j]<<" ("<<points[p[j]].x<<", "<<points[p[j]].y<<')'<<endl;
	}
	return 0;
}


/*暴力破解法*/
void brute_force(int k, int number_of_points)
{
	double rr=0;
	for(int i=1; i<=number_of_points; i++)
	{
		if(points[i].choose_to_S==false && in_center_set<=k)
		{
			in_center_set+=1;
			points[i].choose_to_S=true;
			brute_force(k, number_of_points);
			points[i].choose_to_S=false;
			in_center_set-=1;
		}
	}
	if(in_center_set!=k) return;
	for(int j=1; j<=number_of_points; j++)
	{
		rr=(rr>min_d_iS(j)? rr: min_d_iS(j));
	}
	if(rr>0 && rr<=r)
	{
		r=rr;
		int pp=0;
		for(int l=1; l<=number_of_points; l++)
		{
			if(points[l].choose_to_S==true) p[++pp]=l;
		}
	}
	return;
}
```


#### 三、验证 greedy 求解 k-center 是 2-近似算法

1. 对于仿真数据验证近似比，第一次随机生成平面上 13 个点，并设定中心点个数 $k$ 值为 3，随机生成的 13 个点如下：
~~~
points:
1 (84, 92)
2 (62, 8)
3 (77, 40)
4 (73, 37)
5 (94, 43)
6 (4, 4)
7 (68, 82)
8 (69, 62)
9 (59, 63)
10 (2, 40)
11 (81, 90)
12 (56, 0)
13 (34, 18)
~~~

2. 运行程序 “k-center”，用暴力搜索求最优解，求得结果如下：
~~~
OPT：r=33.541
8 (69, 62)
10 (2, 40)
13 (34, 18)
~~~
即最优半径值为 33.541，此时三个中心点为：8(69,62)，10(2,40)，13(34,18)，利用 python 程序画图示意如下：
![800](greedy%20的%20k-center%20图/验证%20Greedy%20算法求解%20k-center%20问题近似比图一.png)
（图一：k-center（$\lvert V \rvert=13,\ k=3$）的最优解）

3. 运行程序 “k-center”，用 greedy 算法求解，分别以这十三个点作为初始点求解十三次，求解结果如下表：

| 起始选入 S 的点 | 中心点集 S                               | 半径 r    |
| :-------- | :----------------------------------- | :------ |
| 1         | 1 (84,92)<br>2 (62,8)<br>6 (4,4)     | 47.4236 |
| 2         | 1 (84,92)<br>2 (62,8)<br>10 (2,40)   | 47.4236 |
| 3         | 1 (84,92)<br>3 (77,40)<br>6 (4,4)    | 45.1774 |
| 4         | 1 (84,92)<br>4 (73,37)<br>6 (4,4)    | 40.7185 |
| 5         | 5 (94,43)<br>6 (4,4)<br>12 (56,0)    | 50.01   |
| 6         | 1 (84,92)<br>2 (62,8)<br>6 (4,4)     | 47.4236 |
| 7         | 2 (62,8)<br>6 (4,4)<br>7 (68,82)     | 46.8722 |
| 8         | 2 (62,8)<br>6 (4,4)<br>8 (69,62)     | 36.0555 |
| 9         | 2 (62,8)<br>6 (4,4)<br>9 (59,63)     | 40.3113 |
| 10        | 1 (84,92)<br>2 (62,8)<br>10 (2,40)   | 47.4236 |
| 11        | 2 (62,8)<br>6 (4,4)<br>11 (81,90)    | 47.4236 |
| 12        | 1 (84,92)<br>10 (2,40)<br>12 (56,0)  | 50.01   |
| 13        | 1 (84,92)<br>5 (94,43)<br>13 (34,18) | 38.833  |

可以看到，greedy 所求解的半径在 $36.0555$ 到 $50.01$ 之间，对于 $OPT=33.541$，有 $Greedy(r_{max})\leq 2*OPT$，不违反 $2$ 的近似比

4. 进行若干次实验，得到结果如下：

| 总点数，k | Greedy 最大半径 | OPT 的半径 | $Greedy_{max}/OPT$ |
| :---- | :---------- | :------ | :----------------- |
| 8，2   | 81.0555     | 65      | 1.2470             |
| 8，2   | 79.0063     | 49.2443 | 1.6044             |
| 8，2   | 77.4145     | 48.0521 | 1.6110             |
| 14，3  | 61.131      | 44.5982 | 1.3709             |
| 14，3  | 38.2099     | 29.6816 | 1.2873             |
| 14，3  | 53.8516     | 37.8021 | 1.4246             |
| 18，4  | 44.3847     | 30.4138 | 1.4594             |
| 18，4  | 38.0789     | 24.3516 | 1.5637             |
| 18，4  | 44.2832     | 31.4006 | 1.4103             |
| 15，5  | 33.1361     | 26.9258 | 1.2306             |
| 20，3  | 79.511      | 43.8292 | 1.8141             |
| 16，5  | 36.8917     | 25.632  | 1.4393             |
| 50，2  | 92.3472     | 51.8845 | 1.7799             |
| 50，3  | 81.0062     | 42.0119 | 1.9282             |
| 20，4  | 49.5782     | 37      | 1.3400             |

可以看到，$Greedy(r_{max})\leq 2*OPT$ 在这十五组测试中始终成立，其中最大的 $Greedy_{max}/OPT$ 的值是 $1.9282$（倒数第二组测试），比较接近 $2$，但是不超过，由此验证 $2$ 的近似比很有可能是正确的

