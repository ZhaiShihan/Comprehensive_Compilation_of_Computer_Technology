#### 埃拉托斯特尼素数筛：
##### 1. 创建一个用于记录每个数是否是质数的数组
##### 2. 从 2 开始遍历，每遍历到一个数，如果没有被标记为合数，那么它就是质数
##### 3. 将质数位于求解范围内的所有倍数标记为合数
![[质数第三-图/质数第三-埃拉托斯特尼素数筛第一.png|600]]
                        （图一：每找到一个质数，将其倍数筛去）
#### 优化思路：
##### · 合数的质因数不大于开根号的值，故只遍历到根号 n 即可
##### · 要将一个质数 p 的倍数筛除时，从 p<sup>2</sup> 开始筛除即可，因为 2p、3p、……、(p-1)p 已经被之前的2、3、……、(p−1) 筛除过了

###### · <font color="#ffc000">代码实现：</font>
```C++
（bool number[n+1];）

void Sieve_of_Eratosthenes(bool number[], int n)    // number 数组用来判定是否为质数
{
	memset(number, true, sizeof(bool)*(n + 1));    //初始化，全部标记为质数，之后从中“抹去”合数
	number[0]=false, number[1]=false;
	for(int i=2; i*i<=n; i++)
	{
		if(number[i]==true)    //质数的倍数标记为合数
		{
			for(int j=i*i; j<=n; j+=i) number[j]=false;
		}
		else continue;
	}
}    //程序结果：数组 number 中所有的“true”都是质数，所有的“false”都是合数
```



~~~ 参考：“CSDN”【依稀_yixy】《（四）质数（素数）》
~~~ 网站：https://blog.csdn.net/qq_39151563/article/details/108294687