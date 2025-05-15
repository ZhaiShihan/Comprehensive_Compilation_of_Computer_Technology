#### 线性筛选加速：
##### · 根据埃拉托斯特尼素数筛算法，如果一个合数有多个质因数，那么就会被筛去多次
##### · 合数被筛去的次数等于其质因数的个数
![[质数第三-图/质数第三-欧拉素数筛图一.png|650]]
                       （图一：合数被筛去的次数等于其质因数的个数）
##### · 埃拉托斯特尼素数筛的复杂度非线性，原因在于一个合数被多次筛去
##### · 欧拉素数筛通过避免重复筛除合数，将复杂度降为线性

#### 欧拉素数筛：
##### 1. 再创建一个只存放质数的数组，从小到大保存筛选出的质数
##### 2. 每检测到一个数 n 时，都会将这个数与之前保存的质数（如果 n 是质数，n 也会被保存进去）的乘积筛除

###### · <font color="#ffc000">代码实现：</font>
```C++
（bool number[n+1];）

void Sieve_of_Euler(bool number[] ,int n)
{
	memset(number, true, sizeof(bool)*(n+1));    // number 数组用来判定是否为质数
	number[0]=false, number[1]=false;

	int* prime=(int*)malloc(sizeof(int)*(n/3+2));
	//创建一个存放质数的数组
	//大小为 n/3+2 原因：每第 6 个数前后两个数的值可能是质数，故质合数比例最大为三比一，再加上质数 2 和 3
	int length=0;

	for(int i=2; i<=n/2; i++)    //大于 n/2 的值在范围内没有倍数
	{
		if(number[i]==true) prime[length++]=i;
		for(int j=0; j<length; j++)
		{
			if(i*prime[j]>n) break;
			number[i*prime[j]]=false;
		}
	}
	free(prime);
	return;
}    //程序结果：数组 number 中所有的“true”都是质数，所有的“false”都是合数
```



~~~ 参考：“CSDN”【依稀_yixy】《（四）质数（素数）》
~~~ 网站：https://blog.csdn.net/qq_39151563/article/details/108294687