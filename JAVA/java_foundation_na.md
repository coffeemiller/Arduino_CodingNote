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

```java
import java.util.Scanner;
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.print("정수를 입력하세요 : ");
		int i = sc.nextInt();
		System.out.println("입력된 정수는 " + i +"입니다.");
		sc.close();
	}
}
```
+ txt파일을 불러와서 정해진 계산작업하기
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		File file = new File("input.txt");
		try {
			Scanner sc = new Scanner(file);
			while(sc.hasNextInt())
			{
				System.out.println(sc.nextInt() * 100);
			}
			sc.close();
		} catch (FileNotFoundException e) {
			System.out.println("파일을 읽어오는 도중에 오류가 발생했습니다.");
		}
	}
}
```

---------------------------------------------------------
## 제8강.배운 내용 정리하기
+ 1. 개발환경 설치
	- 이클립스는 JDK로서 대표적인 개발 환경이다. 이클립스를 이용하면 JSP, Java Swing부터 시작해 다양한 자바 개발을 시작할 수 있다.

+ 2. 변수
	- 변수는 그 내부에 잇는 값을 프로그램이 실행되는 도중에 언제든지 교체할 수 있다. 하지만 상수는 한 번 설정되면 프로그램이 종료될 때까지 변경죄지 않는 데이터라고 할 수 있다.

+ 3. 자료형
	- 일반적인 프로그래밍 언어에는 정수 자료형 int, 실수 자료형 float과 double, 문자 자료형 char, 문자열 자료형 String이 사용된다. 자바에선 특히 String을 많이 활용하게 된다.

+ 4. 연산자
	- 연산자는 >,<,++,** 등의 다양한 형태로 다수의 데이터를 비교하거나 데이터를 변경할 수 있게 한다. 특히 &&와 ||는 AND, OR 연산으로서 조건문과 반복문에서도 많이 사용된다.

+ 5. 조건문 & 반복문
	- 조건문과 반복문은 프로그래밍의 논리적 흐름에 있어 가장 중요한 부분이다. 여러 개의 데이터를 서로 비교하거나 1부터 100까지 반보하는 등 다양한 논리적 흐름에 따라 프로그램을 전개할 수 있도록 해준다.

+ 6. 기본 입출력
	- 자바에서는 입력을 받을 대 기본적으로 Scanner클래스를 가장 많이 활용한다. Scanner클래스는 정수를 입력 받는 nextInt(), 문자열을 입력받는 next() 등 다양한 함수를 지원한다.

```java
// 1번 문제.
public class Main {
	public static void main(String[] args) {
		System.out.print("염상민");
	}
}
// 2번 문제.
public class Main2 {
	public static void main(String[] args) {
		System.out.println(10 + 10);
		System.out.println(30 * 30);
		System.out.println(20 - 5);
		System.out.println(40 / 3);
		System.out.println(395 % 18);
	}
}
// 3번 문제.
public class Main3 {
	public static void main(String[] args) {
		for(int i = 0; i < 10; i++)
		{
			for(int j = 0; j < 10; j++)
				{
					System.out.print("*");
				}
			System.out.println();
		}
	}
}
```
---------------------------------------------------------
## 제9강.사용자 정의 함수-1
### 자바 객체지향 프로그래밍
+ 객체지향 : 객체는 일반적으로 말하는 물건을 의마하며 여기서 물건은 단순한 데이터 아니고 그 데이터의 조작 방법에 대한 정보 또한 포함하고 있어 그것을 대상으로 다루는 수법을 객체지향이라고 한다.

### 사용자 정의 함수
+ 사용자 정의 함수는 정해진 특정한 기능을 수행하는 모듈을 의마하며 함수를 적절히 활용하면 하나의 문제를 잘게 분해 수 있다.
+ 예를 들어 이 탐색 트리는 삽입, 삭제, 순회 등 다양한 함수의 집합으로 구성된다. 만약 사용자 정의 함수가 없다면 오직 메인 함수에서 모든 알고리즘을 처리해야 하는데, 이는 작업의 효율성을 저하시킬 수 있다. 또한 함수는 각각의 모듈로서 쉽게 수정되고 보완될 수 있다는 장점이 있다.

```java
public class Main {

	// public static 반환형, 함수명, 매개변수
	public static int function(int a, int b, int c) {
		int min;
		if(a>b)
		{
			if(b>c)
			{
				min = c;
			}
			else
			{
				min = b;
			}
		}
		else
		{
			if(a>c)
			{
				min = c;
			}
			else
			{
				min = a;
			}
		}
		for(int i=min; i>0; i++)
		{
			if(a % i ==0 && b % i == 0 && c % i == 0)
			{
				return i;
			}
		}
		return -1;
	}
	public static void main(String[] args) {
		System.out.println("(400, 300, 750)의 최대 공약수 : " + function(400, 300, 750));
	}
}
```

---------------------------------------------------------
## 제10강.사용자 정의 함수-2
+ "어떤"수의 "몇"번째 약수찾기.
```java
public class Main {

	public static int function(int number, int k) {
		for(int i=1; i<=number; i++)
		{
			if(number%i==0)
			{
				k--;
				if(k==0)
				{
					return i;
				}
			}
		}
		return -1;
	}

	public static void main(String[] args) {
		int result = function(3050, 10);
		if(result==-1)
		{
			System.out.println("3050의 10번째 약수는 없습니다.");
		}
		else
		{
			System.out.println("3050의 10번째 약수는 "+result+"입니다.");
		}
	}
}
```

+ 마지막 글자 알아내기.
```java
public class Main {
	public static char function(String input) {
		return input.charAt(input.length() -1 );
		// charAt는 input.length의 공백포함 11개를 가져온다. 여기서 공백 1을 빼준다.
	}
	public static void main(String[] args) {
		System.out.println("Hello World의 마지막 단어는 "+function("Hello World"));
	}
}
```

+ 세 숫자중에 가장 큰 값 찾기
```java
public class Main {
	public static int max(int a, int b) {
		return (a>b) ? a : b;
		// a가 b보다 크다면 a를 반환하고, 그렇지 않으면 b를 반환하라.
	}

	public static int function(int a, int b, int c) {
		int result = max(a,b);
		result = max(result, c);
		return result;
	}

	public static void main(String[] args) {
		System.out.println("(345, 567, 789)중에서 가장 큰 값은 "+function(345,567,789));
	}
}
```
---------------------------------------------------------
## 제11강.반복 함수와 재귀 함수-1
### 반복함수
+ 반복함수는 단순히 while 혹은 for 문법을 이용하여 특정한 처리를 반복하는 방식을 ㅗ문제를 해결하는 함수이다.
+ 반면에 '재귀 함수'는 자신의 함수 내부에서 자기 자신을 스스로 호출함으로써 재귀적으로 문제를 해결하는 함수이다.
+ 재귀 함수는 경우에 따라서는 아주 간결하고 직관적인 코드로 문제를 해결할 수 있게 해주지만 때에 따라서는 심각한 비효율성을 낳을 수 있기 때문에 알고리즘을 작성할 때 유의할 필요가 있다.

+ 10팩토리얼 구하기
```java
public class Main {
	// 5! = 5 * 4 * 3 * 2 * 1 = 120
	// 6! = 720
	public static int factorial(int number) {
		int sum = 1;
		for(int i = 2; i <= number; i++)
		{
			sum *= i;
		}
		return sum;
	}

	public static void main(String[] args) {
		System.out.println("10팩토리얼은 " + factorial(10));
	}
}
```

+ 재귀함수 이용하기
```java
public class Main {
	public static int factorial(int number) {
		if(number == 1)
			return 1;
		else
			return number * factorial(number - 1);
			// 5! = 5 * 4!(3*2!)
	}
	public static void main(String[] args) {
		System.out.println("10펙토리얼은 " + factorial(10));
	}
}
```

---------------------------------------------------------
## 제12강.반복 함수와 재귀 함수-2
+ 피보나치 수열 만들기
```java
// 반복함수로 구하기
public class Main {
	public static int fibonacci(int number) {
		int one = 1;
		int two = 1;
		int result = -1;
		if(number == 1)
		{
			return one;
		}
		else if(number == 2)
		{
			return two;
		}
		else
		{
			for(int i = 2; i < number; i++)
			{
				result = one + two;
				one = two;
				two = result;
			}
		}
		return result;
	}
	public static void main(String[] args) {
		System.out.println("피보나치 수열의 10번째 원소는 " + fibonacci(10) + "입니다.");
	}
}
```

+ 재귀함수로 피보나치수열 구하기
```java
public class Main {
	public static int fibonacci(int number) {
		if(number==1)
		{
			return 1;
		}
		else if(number==2)
		{
			return 1;
		}
		else
		{
			return fibonacci(number - 1) + fibonacci(number - 2);
		}
	}
	public static void main(String[] args) {
		System.out.println("피보나치 수열의 10번째 원소는 " + fibonacci(10) + "입니다.");
	}
}
```
---------------------------------------------------------
## 제13강. 배열
+ 배열은 쉽게 말해 데이터가 많을 때 사용하는 것.
+ 간단한 프로그램 예제에서는 단순히 한 두개의 변수만으로 프로그램을 작동시킬수 있었지만 현실에서의 다양한 프로그램에는 아주 만은 양의 데이터가 사용되는 것이 일반적이다.
+ 따라서 데이터가 많을 때 주로 배열을 이용할 수 있다. 이때 배열은 한없이 많을 수 있으면서도 그 데이터 개수가 변경될 수 있는 데이터들의 집합을 지정해줄 수 있기에 효과적으로 대부분의 프로그램에 사용된다.
```java
import java.util.Scanner;
public class Main {
	public static int max(int a, int b) {
		return ( a > b ) ? a : b;
	}
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		System.out.print("생성할 배열의 크기를 입력하세요 : ");
		int number = scanner.nextInt();
		int[] array = new int[number];
		for(int i = 0; i < number; i++)
		{
			System.out.print("배열에 입력할 정수를 하나씩 입력하세요 : ");
			array[i] = scanner.nextInt();
		}
		int result = -1;
		for(int i = 0; i < number; i++)
		{
			result = max(result, array[i]);
		}
		System.out.println("입력한 모든 정수 중에서 가장 큰 값은 : " + result + " 입니다." );
	}
}

```
+ 100까지 랜덤구해서 평균값 구하기
```java
public class Main {
	public static void main(String[] args) {
		int[] array = new int[100];
		for(int i =0 ; i<100 ; i++) {
			array[i] = (int) (Math.random() * 100 + 1);
		}
		int sum = 0;
		for(int i = 0; i<100 ; i++) {
			sum += array[i];
		}
		System.out.println("100개의 랜덤 정수의 평균값은 " + sum / 100 + "입니다.");
	}
}
```
---------------------------------------------------------
## 제14강.다차원 배열
+ 배열이 배열의 원소로 들어가는 구조를 말한다. 흔히 이차원 배월은 M X N 형태의 지도를 나타내고자 할 때 많이 사용된다. 이러한 다차원 배열을 적절하게 활용하게 되면 현실 세계의 다양한 문제에 보다 쉽게 접근할 수 있다.

+ 0-9까지 2차원배열 만들어 출력하기
```java
public class Main {
	public static void main(String[] args) {
		int N = 50;
		int[][] array = new int[N][N];
		for(int i=0; i<N; i++) {
			for(int j=0; j<N; j++) {
				array[i][j] = (int)(Math.random() * 10);
			}
		}
		for(int i=0; i<N; i++) {
			for(int j=0; j<N; j++) {
				System.out.print(array[i][j]+" ");
				//한칸씩 뛰어서 인쇄되도록
			}
			System.out.println();
			//50개가 되면, 열을 바꿔서 출력되도록
		}
	}
}
```
---------------------------------------------------------
## 제15강.클래스
+ 클래스는 객체 지향 프로그래밍에 있어서 가장 기본이 되는 것이다. 클래스를 이용하여 현실 세계의 특정한 물건을 지정할 수 있다. 가장 대표적으로 많이 사용되는 것이 Node 클래스이다. 이는 하나의 장소나 위치를 의미할 수도 있으며 자료구조에서 말하는 이전 탐색 트리의 하나의 서브 트리가 될 수 도 있다. 또한 개발 프로젝트에서는 종종 Student 클래스와 같이 하나의 처리할 데이터 단위를 명시하는 데 사용이 된다.

+ 객체라는 것은... 실세계의 사물이라고 할 수 있다.

+ Main.java
```JAVA
public class Main {
	public static void main(String[] args) {
		Node one = new Node(10, 20);
		Node two = new Node(30, 40);
		Node result = one.getCenter(two);
		System.out.println("X : " + result.getX() + ", Y : " + result.getY() );
	}
}
```

+ Node.java
```JAVA
public class Node {
	private int x;
	private int y;

	public int getX() {
		return x;
	}
	public void setX(int x) {
		this.x = x;
	}

	public int getY() {
		return y;
	}
	public void setY(int y) {
		this.y = y;
	}

	public Node(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public Node getCenter(Node other) {
		return new Node((this.x + other.getX()) / 2, (this.y + other.getY()) / 2);
	}
}
```
#### 결과... X : 20, Y : 30
---------------------------------------------------------
## 제16강.상속-1
+ 상속이란 쉽게 말해, 다른 클래스가 가지고 있는 정보를 자신이 포함하겠다는 의미이다. 즉 다른 클래스에 대한 정보를 상속받아 자신이 그대로 사용할 수 있도록 한다. 상속을 적절히 활용할 때 불필요한 코드의 수를 줄일 수 있어서 상당히 효율적인 개발이 이루어질 수 있다.


+ Person 클래스 만들기
```java
public class Person {
	private String name;
	private int age;
	private int height;
	private int weight;

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public int getHeight() {
		return height;
	}
	public void setHeight(int height) {
		this.height = height;
	}
	public int getWeight() {
		return weight;
	}
	public void setWeight(int weight) {
		this.weight = weight;
	}

	//초기화 하기.
	public Person(String name, int age, int height, int weight) {
		super(); //부모에로부터의 상속들 선언
		this.name = name;
		this.age = age;
		this.height = height;
		this.weight = weight;
	}
}
```

+ Student 클래스 만들기
```java
public class Student extends Person {
	private String studentID;
	private int grade;
	private double GPA;

	public String getStudentID() {
		return studentID;
	}
	public void setStudentID(String studentID) {
		this.studentID = studentID;
	}
	public int getGrade() {
		return grade;
	}
	public void setGrade(int grade) {
		this.grade = grade;
	}
	public double getGPA() {
		return GPA;
	}
	public void setGPA(double gPA) {
		GPA = gPA;
	}

	public Student(String name, int age, int height, int weight, String studentID, int grade, double gPA) {
		super(name, age, height, weight);
		this.studentID = studentID;
		this.grade = grade;
		GPA = gPA;
	}

	public void show() {
		System.out.println("-----------------------------");
		System.out.println("학생 이름 : " + getName());
		System.out.println("학생 나이 : " + getAge());
		System.out.println("학생 키 : " + getHeight());
		System.out.println("학생 몸무게 : " + getWeight());
		System.out.println("학번 : " + getStudentID());
		System.out.println("학년 : " + getGrade());
		System.out.println("학점 : " + getGPA());
	}
}
```

+ Main 클래스 만들기
```java
public class Main {
	public static void main(String[] args) {
		// Student라는 클래스 형태로 'student1'변수 선언
		// stuent1에 Student클래스 형식의 인자('홍길도')를 넣어 대입.
		Student student1 = new Student("홍길동", 20, 175, 70, "20170101", 1, 4.5);
		Student student2 = new Student("이순신", 20, 175, 70, "20090105", 4, 3.5);
		// Student 클래스 형식으로 만든 변수들을 show하라
		student1.show();
		student2.show();
	}
}
```
---------------------------------------------------------
## 제17강.상속-2
+ 배열을 활용한 여러명의 학생정보 등록하기.
```java
import java.util.Scanner;
public class Main {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.print("총 몇 명의 학생이 존재합니까? ");
		int number = scan.nextInt();
		Student[] students = new Student[number];
		for(int i=0;i<number;i++) {
			String name;
			int age;
			int height;
			int weight;
			String studentID;
			int grade;
			double gPA;
			System.out.print("학생의 이름을 입력하세요 : ");
			name = scan.next();  //name 변수가 String이기 때문에 'next()'
			System.out.print("학생의 나이를 입력하세요 : ");
			age = scan.nextInt();
			System.out.print("학생의 키를 입력하세요 : ");
			height = scan.nextInt();
			System.out.print("학생의 몸무게를 입력하세요 : ");
			weight = scan.nextInt();
			System.out.print("학생의 학번을 입력하세요 : ");
			studentID = scan.next(); //studentID 변수가 String이기 때문에 'next()'
			System.out.print("학생의 학년을 입력하세요 : ");
			grade = scan.nextInt();
			System.out.print("학생의 학점을 입력하세요 : ");
			gPA = scan.nextDouble();
			students[i] = new Student(name, age, height, weight, studentID, grade, gPA);
		}
		for(int i=0;i<number;i++) {
			students[i].show();
		}
	}
}
```
---------------------------------------------------------
## 제18강.추상


---------------------------------------------------------
## 제19강.Final 키워드


---------------------------------------------------------
## 제20강.인터페이스



---------------------------------------------------------
## 제21강.다형성



---------------------------------------------------------
## 제22강.Object 클래스



---------------------------------------------------------
## 제23강.객체 지향 기법의 활용



---------------------------------------------------------




---------------------------------------------------------
