#include <stdio.h>
#include <math.h>
#include <string>
#include <assert.h>
#include <iostream>
#include <conio.h>  
#include <cstdlib> 
#include <time.h>

using namespace std;

/*定义一个结构体:分数*/
struct fs
{
	int a;//above分子 
	char line;
	int b;// below分母 
};

/*求最大公约数*/
int gys(int a, int b)
{
	if (!b) return a;
	else return gys(b, a%b);
}  //辗转相除法 

/*生成随机数（100以内、真分数）*/
fs generateNum()
{
	int a, b, t;
	fs num;
	if (rand() % 4 == 1)//随机产生0,1,2,3，当刚好是1时，产生分数 （这里概率一定是1/4吗？有没有让它概率不固定的办法？） 
	{
		a = rand() % 10 + 1;
		b = rand() % 10 + 1;
		if (a>b) { t = a; a = b; b = t; }//使分子小于分母，确保真分数 
		t = gys(b, a);
		a = a / t;
		b = b / t;
	}//生成分数 
	else
	{
		a = rand() % 100 + 1;
		b = 1;
	}//生成整数，即分母为1 
	num.a = a;
	num.b = b;
	num.line = '/';
	return num;//返回结构体：分数的值 
}

/*生成操作符*/
char generateOp()
{
	char op;
	int n;
	n=rand()%4;//根据随机数的值，确定运算符，然后输出
	if (n==0) op='+';
	if (n==1) op='-';
	if (n==2) op='*'; 
	if (n==3) op='/';
	return op;
}

/*两个“分数”的四则运算*/
fs cal(fs n1, char op, fs n2)
{
	fs num;
	switch (op){
	    case '+':{
				 num.a = n1.a*n2.b + n2.a*n1.b;
				 num.b = n1.b*n2.b;
				 break;
	    }
	    case '-':{
				 num.a = n1.a*n2.b - n2.a*n1.b;
				 num.b = n1.b*n2.b;
				 break;
	    }
	    case '*':{
				 num.a = n1.a*n2.a;
				 num.b = n1.b*n2.b;
				 break;
	    }
	    case '/':{
				 num.a = n1.a*n2.b;
				 num.b = n1.b*n2.a;
				 break;
	    }
		default:{
				num.a = 0;
				num.b = 1;
		}
	}
	int t;
	if (num.a<num.b)
		t = gys(num.b, num.a);
	else
		t = gys(num.a, num.b);
	num.a = num.a / t;
	num.b = num.b / t;//约分
	if (num.a*num.b < 0) {
		num.a = -abs(num.a);
		num.b = abs(num.b);
	}//保证分数为负数时，负号加在分子上 （是不是要考虑不让题目中出现任一负数结果？）
	return num;
}

/*主函数*/
int main(){
	int begin = 1;	
	while (begin == 1) {
		srand((unsigned)time(NULL));           //随机数生成函数初始化 
		char op[10];
		struct fs num[10];
		int m, n, i, j, q, h, k = 0;
		float s;
		cout << "_________________\n本次满分100。" << endl << "请输入题目数：";
		cin >> n;//用户选择的题数
		for (m = 1; m <= n; m++){
			j = rand() % 10 + 1;               // 随机生成该题的操作符数目：1~10
			/*随机生成数和运算符，写入数组*/
			for (i = 0; i < j; i++){
				num[i] = generateNum();
				op[i] = generateOp();
			}
			num[i] = generateNum();                              
			
			/*输出计算式*/
			cout << endl << m << ':';
			for (i = 0; i < j; i++){
				/*每循环一次，输出一个“分数”*/
				if (num[i].b == 1) cout << num[i].a;              //当生成整数时，只输出结构体fs中的a，也就是分子部分
				else cout << num[i].a << num[i].line << num[i].b; //生成分数时，输出a/b形式的数
				/*分别输出运算符号的书面形式*/
				if (op[i] == '*')cout << "×";
				else if (op[i] == '/')cout << "÷";
				else if (op[i] == '-')cout << "-";
				else cout << "+";
			}
			/*运算式的最后一个数，在循环外单独输出，并输出等号*/
			if (num[i].b == 1)cout << num[i].a << '=';
			else cout << num[i].a << num[i].line << num[i].b << '=';;

			/*进行计算：先算乘除*/
			for (q = 0; q < j; q++){
				/*每循环一次，计算乘除运算符前后两数*/
				if (op[q] == '*' || op[q] == '/') {	               //这里的错误找得好苦				
					num[q + 1] = cal(num[q], op[q], num[q + 1]);   //利用前面定义的函数进行两数的乘除，得到的是一个结构体
					if (q > 0 && op[q - 1] == '-') {                    //从第2个运算符开始，检测前一个运算符是否为负号						
						op[q - 1] = '+';                           //如果前一个运算符是负号，把它改为加号
						num[q + 1].a = -num[q + 1].a;              //把负号放到这个运算符后面的数的分子上
						num[q + 1].line = '/';
						num[q + 1].b = num[q + 1].b;
					}                                              //这样负号就被移动到正确的位置,不会因为计算方法的缺陷失效
					num[q].a = 0;num[q].line = '/';num[q].b = 1;
					op[q] = '+';                                   //把已经被计算了的前一个数赋值为0，操作符改为+号
				}			
			}
			/*后算加减：*/
			for (h = 0; h < j; h++)
				num[h + 1] = cal(num[h], op[h], num[h + 1]);

			/*答题者输入答案，此时num[h]为正确答案*/			
			fs answer;
			if (num[h].b == 1){
				cin >> answer.a;
				answer.b = 1;
			}
			else cin >> answer.a >> answer.line >> answer.b;
			
			/*判断答案对错*/
			if (answer.a == num[h].a && answer.b == num[h].b){
				printf("正确！");
				k += 1;//正确题数计数器
			}
			else {
				cout << "不正确！正确答案=";
				if (num[h].b == 1) cout << num[h].a;
				else cout << num[h].a << '/' << num[h].b;
			}
			cout << endl;
		}
		s = 100 * k / n;
		cout << endl << "正确率：" << k << '/' << n << endl;
		cout << "本次得分：" << s << endl << "再来一套？Y/N：";
		char yes;
		cin >> yes;
		if (yes == 'y' || yes == 'Y')begin = 1;
		else begin = 0;
	}
	system("pause");
	return 0;
}
