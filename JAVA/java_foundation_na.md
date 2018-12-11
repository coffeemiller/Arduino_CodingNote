# 자바 기초 프로그래밍 강좌 (강사:나동빈)
---------------------------------------------------------
## 제1강.Hello World!


---------------------------------------------------------
## 제2강.변수(Variable)
```java
int intType = 100;
double doubleType = 150.5;
String stringType = "자바";

System.out.println(intType);
System.out.println(doubleType);
System.out.println(stringType);
```

```java
int a= 1;
int b = 2;

System.out.println("a + b =" + (a + b));
System.out.println("a - b =" + (a - b));
System.out.println("a * b =" + (a * b));
System.out.println("a / b =" + (a / b));
```
+ 자바에서는 변수 초기화를 하지 않으면 사용할 수 없다.
+ 정수를 나타내는 타입만 해도 short, int, long으로 다양하다.
+ 정수 변수 안에 실수를 넣으면 정수 부분만 변수에 저장한다.
+ 실수 값을 반올림할 때는 변수에 0.5를 더한 뒤에 정수형으로 '형변환''을 하면 된다.
+ 반올림한 값 = (int)(실 + 0.5);
---------------------------------------------------------
## 제3강.자료형(Data Type)
+ 자바에서는 다른 프로그래밍 언어에서 사용하는 것과 거의 동일한 자료형이 존재하낟.
+ 다양한 자료형이 존재하여 C언어와 두드러지는 차이점은 boolean형과 String형 정도만 있을 뿐이다. 이 중에서 가장 많이 사용되는 자료형은 String, Array, boolean char, int, double이다.
+ 자료형 자체는 굳이 암기할필요가 없으나 프로그래밍을 통해 많은 훈련을 거치게 되면 알아서 잘 활용할 수 있다. 또한 문자열을 나타내는 자료형인 String형이 아주 편리하게 사용되며 String 내부적으로 substring 등의 함수를 제공하기 때문에 활용도가 높다.

```java
System.out.println("10진수 : " + a);
System.out.format("8진수 : %o\n", a);
System.out.format("16진수 : %x", a);


String name = "John Doe";
System.out.println(name);    //John Doe
System.out.println(name.substring(0,1));  //J
System.out.println(name.substring(3,6));  //n D
System.out.println(name.substring(5,8));  //Doe
System.out.println(name.substring(0,4));  //John
```
---------------------------------------------------------
## 제4강.연산자(Operator)
+ 기본적으로 정수를 나타내는 자료형이 많은 이유는 각 자료형이 차지하는 메모리 공간의 크기가 다르기 때문이다.
+ double 형이라고 하더라도 과도하게 큰 수를 저장하고자 하면 잘못된 계산 결과가 나올 수 있다.
+ 소수점 표기 형식을 지수 형식으로 출력하고 싶으면 %e를 이용하면 된다.
+ 그렇다면 자바에서 String의 최대크기는 어떻게 될까? (4GB)
+ 자바의 String은 클래스 기반의 비원시적인 자료형이다.

+ 연산자는 하나의 기호 체계이다. 흔히 1+2에서 1과 2는 피연산자(operand)이고 +는 연산자(Operand)라고 한다. 프로그래밍에서 연산자란 없어서는 안 될 만큼 아주 중요하며 계산에 있어서 기초적인 부분이다. 프로그래밍에서 가장 많이 사용되는 연산자는 +,-,*,/,%이다. 추가적으로 ++와 ! 등과 같이 다양한 연산자가 사용되는 경우가 있기 때문에 이를 정확히 숙지하는 것이 중요하다.

```java
int minute = SECOND / 60;
int second = SECOND % 60;

System.out.println(minute + "분 " + second + "초");
// 16분 40초

int a = 10;
System.out.println("현재의 a는 " + a + "입니다.");  //10
a++;                                               //내부적으로 11
System.out.println("현재의 a는 " + a + "입니다.");  //11
System.out.println("현재의 a는 " + ++a + "입니다."); //12
System.out.println("현재의 a는 " + a++ + "입니다."); //12출력하고 내부13
System.out.println("현재의 a는 " + a + "입니다."); //13

System.out.println(1 % 3); //1
System.out.println(2 % 3); //2
System.out.println(3 % 3); //0
System.out.println(4 % 3); //1
System.out.println(5 % 3); //2
System.out.println(6 % 3); //0

System.out.println("a와 b가 같은가요? " + (a == b));
//a와 b가 같은가요? true
System.out.println("a가 b가 큰가요? " + (a > b));
//a가 b가 큰가요? false
System.out.println("a가 b가 작은가요? " + (a < b));
//a가 b가 작은가요? false
System.out.println("a가 b와 같으면서 a가 30보다 큰가요? " + ((a =b) && (a > 30)));
//a가 b와 같으면서 a가 30보다 큰가요? true
System.out.println("a가 50이 아닌가요? " + !(a == 50));
//a가 50이 아닌가요? false


int x = 50;
int y = 60;
System.out.println("최댓값은 " + max(x,y) + "입니다.");

// 반환형, 함수 이름, 매개변수
static int max(int a, int b) {
int result = (a > b) ? a : b;
return result;
}
// 최댓값은 60입니다.


double a = Math.pow(3.0, 20.0);
System.out.println("3의 20제곱은 " + (int) a + "입니다.");
// 3의 20제곱은 2147483647입니다.
```
---------------------------------------------------------
## 제5강. 조건문 & 반복문-1
+ 조건문과 반복문은 프로그래밍에 있어서 논리의 흐름을 정하는 가장 기본적인 기술이다.
+ 조건문은 조건에 따라 결정을 내리는 것을 말함.
+ 반복문은 반복적인 같은 처리를 되풀이하는 것을 말함.
```java
public static void main(String[] args) {

		String a = "I Love You.";
		if(a.contains("Love"))
		{
			// 포함하는 경우 실행되는 부분
			System.out.println("Me Too.");

		}
		else
		{
			//포함하지 않느 ㄴ경우 실행되는 부분
			System.out.println("I Hate You.");
		}
	}
```
---------------------------------------------------------
## 제6강. 조건문 & 반복문-2
```java
int score = 95;

		if(score >= 90)
		{
			System.out.println("A+입니다.");
		}
		else if(score >= 80)
		{
			System.out.println("B+입니다.");
		}
		else if(score >= 70)
		{
			System.out.println("C+입니다.");
		}
		else
		{
			System.out.println("F입니다.");
		}



int i = 1, sum = 0;
while(i <= 1000)
{
	sum += i++;
}
System.out.println("1부터 1000까지의 합은 " + sum + "입니다.");
//1부터 1000까지의 합은 500500입니다.


// for문 : 초기화 부분, 조건 부분, 연산 부분
for(int i = N; i > 0; i--)
{
	for(int j = i; j > 0; j--)
		{
			System.out.print("*");
		}
	System.out.println();
}
```
---------------------------------------------------------
## 제7강. 기본 입출력(Input & Output)
+ 자바에서는 대표적으로 Scanner()클래스를 이용하여 사용자와 상호작용할 수 있다.
+  Scanner sc = new Scanner(System.in);
  - 위 같이 클래스 객체를 생성한 뒤에 sc nextint(); 와 같은 방법으로
  - int형을 입력받을 수 있다. 입력 받은 자료는 내부적으로 어떠한 처리를 한 뒤에 다시 사용자에게 그 값을 반환할 수 있다.
+ 프로그램이 입출력을 잘 지원한다는 것은 사용자 인터페이스가 뛰어나다는 의미와 같다.




---------------------------------------------------------




---------------------------------------------------------




---------------------------------------------------------





---------------------------------------------------------





---------------------------------------------------------



---------------------------------------------------------
