#### 为什么叫“模拟退火”：

##### · 高温物体的降温过程：
###### · 一个高温物体的降温过程中，其温度为 T 时出现能量差为 dE 的降温概率为 $P(dE)=e^{dE/(k\cdot T)}$
###### · 简单而言，温度越高降温的概率越大，温度越低降温的概率越小，模拟退火利用这样的思想搜索：
1. 在进行搜索的时候首先定义一个初始值（温度）T，一个系数 r（降温速度，0 < r < 1 ）
2. 假设当前状态为 $f_i$，下一个状态为 $f_{i+1}$，对这两个状态进行评价：
	1. 如果更接近想要的结果，则更新到这个状态
	2. 否则以 $P(dE)$ 的概率去更新到这个状态
· 随着搜索次数的不断增多，搜索范围将越来越趋于稳定，也就是随着时间增长，温度降低的概率越来越低，直到趋近于 0；对应搜索就是，随着搜索的次数越多，搜索到的值是想要值的概率越大


#### 基本介绍：

##### 1. 初始解生成：
###### · 从问题的解空间中随机生成一个初始解作为当前解

##### 2. 定义能量函数：
###### · 为了衡量解的质量，需要定义一个能量函数（也称目标函数或代价函数），它会根据问题的性质而定，目标是使能量值越低越好（或者是越高越好，取决于问题类型），即目标函数，比如路程和

##### 3. 设置参数：
###### · 模拟退火算法有一些关键参数需要设置，如初始温度、退火速率等；初始温度通常较高，而退火速率则决定了温度如何逐渐降低

```C++
double startT=？;    //起始温度
double endT=？;    //终止温度
double changeT=？;    //温度变化率
double L=？;    //最大迭代次数
```

```C++
· 定义模拟退火算法的参数：
double initialTemperature=100.0;    //初始温度
double coolingRate=0.95;    //退火速率
double currentSolution=(rand()%2000-1000)/100.0;    //初始解在 [-10,10]
double bestSolution=currentSolution;    //最优解
double bestEnergy=energyFunction(currentSolution);    //最优解对应的能量
```

##### 4. 主循环：
###### · 在一系列的迭代中，算法会根据当前温度接受新的解，或者以一定概率接受劣质解
###### · 在每次迭代中，会产生一个邻近解，可以通过微小改变当前解来得到
###### · 随机产生新的解、相邻解

##### 5. 能量差与概率：
###### · 计算当前解与邻近解之间的能量差，如果邻近解更优，则直接接受它
###### · 如果邻近解不如当前解好，根据一个概率计算公式决定是否接受劣质解
###### · 随着迭代进行，温度逐渐降低，概率逐渐减小，更有可能只接受更优解

```C++
· 模拟退火主循环：
while(initialTemperature>0.1)
{
	//生成邻近解：
	double newSolution=currentSolution+(rand()%201-100)/100.0;    //在 [-1,1] 范围内微调节
	//计算能量差：
	double deltaEnergy=energyFunction(newSolution)-energyFunction(currentSolution);
	//判断是否接受新解：
	if(deltaEnergy<0||exp(-deltaEnergy/initialTemperature)>(rand()/static_cast<double>RAND_MAX))
	{
		currentSolution=newSolution;
		//更新最优解：
		if(energyFunction(currentSolution)<bestEnergy)
		{
			bestSolution=currentSolution;
			bestEnergy=energyFuncdion(currentSolution);
		}
	}
	//降低温度：
	initialTemperature *= coolingRate;
}
```

```C++
· 主要逻辑：
double df=get_sum(cur)-sum;    //计算代价
double sj=rand()%10000/10000;    // sj 为 0 到 1 之间的随机数
if(df<0)    //如果结果更优，选择接受
{
	p=cur;
	sum+=df;    //若 df 是负值，则总代价会减少
}
else if(exp(-df/T)>sj)    //否则以一定概率接受
{
	p=cur;
	sum+=df;
}
T *= at;    //降温
if(T<e) break;    // T 降到终止温度后退出
```

##### 6. 温度下降：
###### · 在每一次循环迭代之后，根据设定的退火速率，降低温度，模拟退火过程中的温度逐渐下降，也就是概率逐渐减小

##### 7. 终止条件：
###### · 算法会不断地迭代，直到达到终止条件，比如达到一定迭代次数或温度降低到一定程度


#### 实例应用：

##### 1. 一维坐标搜索：

###### · 求多项式函数的最小值：
$$设\ \ F(x)=6x^7+8x^6+7x^3+5x^2-y\cdot x\ \ (0\leq x\leq 100)，求\ F(x)\ 的最小值$$

· 分析：
```C++
· 初始温度 T 设为 100
· 降温系数取 0.98
· 温度下界（精度）取 10^(-6)
· 搜索起点取 (0,0)
```

```C++
#include<iostream>
#include<cmath>
#include<iomanip>
using namespace std;
const double EPS=1e-6;    //温度下界
const double r=0.98;    //降温系数
const int dx[2]={-1,1};    // x 变化的步长
double y;

double F(double x)    //目标函数
{
	return 6*pow(x,7)+8*pow(x,6)+7*pow(x,3)+5*pow(x,2)-y*x;
}

int main()
{
	int T;
	cin>>T;    //测试用例数量
	while(T--)
	{
		cin>>y;
		double step=100;    //初始步长
		double x=0;    //初始解
		while(step>EPS)    //模拟退火算法主循环
		{
			for(int i=0; i<2; i++)
			{
				double next_x=x+dx[i]*step;    //计算下一个可能的解
				if(next_x<0||next_x>100) continue;    //超出边界点舍去
				if(F(next_x)<F(x)) x=next_x;    //更优时更新解
			}
			step*=r;    //降温
		}
		cout<<fixed<<setprecision(4)<<F(x)<<endl;    //四位浮点
	}
	return 0;
}
```

##### 2. 二维坐标搜索：

###### · 最短距离点问题：
· 给一个矩形区域：$(0,0)\ 到\ (X,Y)$，作为目标搜索范围，给定 N 个点，要求找出一个点使这个点到这 N 个点的最大距离最小
· 分析：一维扩展为二维，单点搜索变成随机多点并行搜索，原来的双向移动转变为 360° 随机转动，最后在多点的搜索结果中取最优
```C++
· 温度上界 T 取矩形对角线，即圆的最大半径，精度为小数点后一位
· 精确度 EPS 取 10^(-3)
· 降温速率 r 取 0.8
· 每个点 36 次移动
```

```C++
#include<iostream>
#include<cmath>
#include<stdio.h>
using namespace std;
const double EPS=1e-3;    //最低温度（精确度）
const PI=acos(-1.0);    //圆周率
const double INF=0x3f3f3f3f    //无穷量化
const double r=0.8;    //退火速率
const int maxn=1e4+5;    //最大点数量
const int num=20;    //粒子数量

struct Point
{
	int x,y;
}

Point a[maxn], m[maxn];    //输入点数组和随机生成点数组
int n;    //输入点数量
double d[maxn];    //每个随机点对应的最大距离

double dist(Point A, Point B)    //计算两个点之间的距离
{
	return sqrt((A.x-B.x)*(A.x-B.x)+(A.y-B.y)*(A.y-B.y));
}

double MAX(Point t)    //计算随机点到所有点的最大距离
{
	double res=-1;
	for(int i=0; i<n; i++)
	{
		res=max(res, dist(t,a[i]));
	}
	return res;
}

int main()
{
	double xx,yy;
	srand(time(0));    //使用当前时间作为随机种子
	while(cin>>xx>>yy>>n)    //输入 xx，yy （二维平面的宽和高）以及点是数量 n
	{
		for(int i=0; i<n; i++)
		{
			cin>>a[i].x>>a[i].y;    //输入每个点的坐标
		}
		for(int i=0; i<num; i++)
		{    //生成 num（20）个粒子，随机初始化
			m[i].x=(rand()%1000+1)/1000.0*xx;
			m[i].y=(rand()%1000+1)/1000.0*yy;
			d[i]=MAX(d[i]);    //计算每个随机点的最大距离
		}
		double T=sqrt(x*x+y*y);    //计算初始温度
		Point next, now;    //下一个点和当前点
		while(T>EPS)    //当温度大于最低温度界限时
		{
			for(int i=0; i<num; i++)    //每个粒子考量
			{
				now.x=m[i].x;    //更新当前 x 坐标
				now.y=m[i].y;    //更新当前 y 坐标
				for(int j=0; j<36; j++)    // 36 个方位考察
				{
					double ang=(rand()%1000+1)/1000.0*2*PI;    //随机角度
					next.x=now.x+cos(ang)*T;    //随机方向前进
					next.y=now.y+sin(ang)*T;    //随机方向前进
					if(next.x<0||next.x>xx||next.y<0||next.y>yy) continue;    //越界
					if(MAX(next)<d[i])    //如果最大距离变小了，更新粒子坐标
					{
						m[i].x=next.x;
						m[i].y=next.y;
						d[i]=MAX(next);
					}
				}
			}
			T*=r;    //降温
		}
		int res;    //答案标志
		double ans=INF;    // ans 的初值设为无限大
		for(int i=0; i<num; i++)    //寻找最小的最大距离
		{
			if(d[i]<ans)
			{
				ans=d[i];
				res=i;
			}
		}
		printf("(%.1f,%.1f).\n",m[res].x,m[res].y);
		printf("%.1f\n",ans);    //输出坐标与最大距离
	}
	return 0;
}
```

###### · TSP（货郎担问题）：
· 假设有一个旅行商人要拜访n个城市，他必须选择所要走的路径，路径的限制是每个城市只能拜访一次，而且最后要回到原来出发的城市
· 路径的选择目标是要求得的路径路程为所有路径之中的最小值

· 算法分析：
```C++
1. 初始化：起始温度、终止温度、温度变化率、最优路径顶点集、最短长度 S
2. 利用蒙特卡洛法得到一个比较好的初始解
3. 当前温度大于终止温度时，利用两点交换法在当前最优路径基础上构造一条新路径，计算新路径长度 S1，差值 dE=S-S1
4. 若 dE<0，当前最优路径更新为新路径，最短长度更新为 S1；否则产生一个介于 0~1 的概率 rt，并计算
exp(-dE/T)，T 为当前温度；若 exp(-dE/T)>rt，当前最优路径更新为新路径，最短长度更新为 S1，更新温度
```

```C++
#include<iostream>
#include<cstdlib>
#include<vector>
#include<cmath>
#include<ctime>
using namespace std;

void TSP(vector < vector<int> > W)
{
	int n=W.size();    //顶点数量

	//初始化参数：
	int startT=3000;    //初始温度
	double endT=1e-8;    //结束温度
	double delta=0.999;    //温度变化率
	int limit=10000;    //概率选择上限，表示已经接近最优解

	vector <int> path;    //最优路径
	int length_sum=0;    //最优路径长度和
	for(int i=0; i<n; i++)    //初始化最优路径
	{
		path.push_back(i);
	}

	for(int i=0; i<n-1; i++)
	{
		length_sum+=W[path[i]][path[i+1]];
	}
	length_sum+=W[path[n-1]][path[0]];

	//蒙特卡洛算法得到一个好的初始解：
	vector <int> cur=path;
	int cur_sum=0;
	for(int i=0; i<8000; i++)
	{
		for(int k=0; k<n; k++)
		{
			int j=rand()%n;
			swap(cur[k],cur[j]);
		}
		for(int k=1; k<n-1; k++)
		{
			cur_sum+=W[cur[k]][cur[k+1]];
		}
		cur_sum+=W[cur[n-1]][cur[0]];
		if(cur_sum<length_sum)
		{
			path=cur;
			length_sum=cur_sum;
		}
	}

	//模拟退火具体过程：
	srand((int)(time(NULL)));    //确定一个随机种子
	while(startT>endT)
	{
		vector <int> path_new=path;
		int length_sum_new=0;    //新的路径总和
		int P_L=0;    //以一定概率接受次数
		int x=rand()%n, y=rand()%n;    //随机产生两个点，交换，得到一个新路径
		while(x==y)    //直到随机产生两个互异顶点才继续向下执行
		{
			x=rand()%n;
			y=rand()%n;
		}

		swap(path_new[x], path_new[y]);    //等价于在原最优路径上随机交换两个互异顶点得到新路径
		for(int i=0; i<n-1; i++)
		{
			length_sum_new+=W[path_new[i]][path_new[i+1]];
		}
		length_sum_new+=W[path_new[n-1]][path_new[0]];

		//比较新旧路径，取优：
		double dE=length_sum_new-length_sum;
		if(dE<0)    //新路径更短，直接取用
		{
			path=path_new;
			length_sum=length_sum_new;
		}
		else    //新路径不会更优，按一定概率接受
		{
			double rd=rand()/(RAND_MAX+1.0);    //随机产生概率：0~1
			if(exp(-dE/startT)>rd)
			{
				path=path_new;
				length_sum=length_sum_new;
				P_L++;    //一定概率接受次数
			}
		}
		if(P_L==limit) break;    //达到限制，直接退出
		startT *= delta;    //温度变化，降低
	}
}
```



~~~
参考：
1. numberer《模拟退火的一些个人见解》
（CSDN：https://blog.csdn.net/numberer/article/details/79996753）
2. jokery《优化算法——模拟退火算法（C++）》
（CSDN：https://blog.csdn.net/jokery404/article/details/132354811）
3. 是阿俊呐《TSP--模拟退火算法（c++实现+详细解释）》
（CSDN：https://blog.csdn.net/qq_40738840/article/details/84324494）
~~~

