Jump To Java!!
==============

``` python
작성자 : coffeemiller
```

###### JAVA를 들어가며...
1995년 자바의 아버지라고 불리는 제임스 고슬링과 그의 동료들에 의해서 시작된 프로젝트다.
한국에서는 정부나 기업의 시스템 통합 프로젝트가 대부분 자바로 구현되기 때문에 자바는 기업용 시장에서 두각을 나타내고 있다.

시스템 통합이란? System Integration의 약자로 기관이나 기업의 업무 관리를 소프트웨어화하는 것을 의미한다. 예를 들어 병원에 대한 SI라고 한다면 환자의 상태와 의료진의 상태에 따라서 효율적으로 진료가 이루어지게 한다거나, 제조 공정이라고 한다면 생산설비의 상태를 시스템적으로 관리하는 것이 있을 것이다.

자바는 처음 프로그래밍을 배우는 사람이 쉽게 적응하기 어려운 언어라고 할 수 있다.
이것은 언어가 추구하는 방향성 때문이기도 한데 모든것이 클래스 기반에서 동작해야 한다.
자바의 이런 특징이 나중에는 무척 편리하지만 처음 시작에는 넘기 힘든 장벽이 되기도 한다.


###### JDK설치
자바코딩을 시작하기 전에 개발환경을 세팅한다.
개발환경이란 자바로 프로그램을 만들 수 있는 컴퓨팅 환경을 말한다.
자바 프로그래밍을 위해 필수적으로 필요한 [**JDK(Java Development Kit)**](http://bitly.kr/YSBS)를 먼저 설치 한다.

자바는 원래 '썬 마이크로시스템즈'에서 만들고 배포했지만, 오라클이 썬을 인수하면서부터 라이센스가 좀 복잡하게 변경된 것 같다.


    - Java SE(Java Platform Stanard Edition) : 자바 언어의 문법적 구성의 정의.  
    - JDK(Java Development Kit) : 개발자버전. 컴파일러와 개발 도구들.
    - JRE(Java Runtime Environment) : 자바가 동작하는데 필요한 JVM, 라이브러리, 각종 파일들이 포함.
    - JVM(Java Virtual Machine) : 자바가 실제로 구동하는 환경.
![Alt text](/JAVA/java-1.png "JAVA 구동과정")


###### 자바소스와 컴파일
JDK를 설치했다면 jdk가 설치된 디렉토리의 bin이라는 하위 디렉토리에 javac.exe와 java.exe 파일이 저장되어 있을 것이다. 혹시라도 javac.exe가 없다면 jdk가 아닌 jre를 설치한 것이다.
![Alt text](/JAVA/java-2.png "JAVA 컴파일과정")

###### 자바파일?
자바파일이란 우리가 작성해야 할 자바 프로그램을 말한다. ".java"라는 확장자를 가진 파일로 저장하게 되는데... 이렇게 저장되는 파일을 '자바소스'라고 한다.
만약 우리가 'MyProgram.java'라는 자바 파일을 작성했다면, 프로그램이 정상적으로 동작하는 지 확인하기 위해서 프로그램을 실행시키고 싶을 것이다.

자바로 작성한 파일을 실행하기 위해서는 두 번의 단계를 거쳐야만 한다. 하나는 .java 파일을  .class 파일로 바꾸어 주는 컴파일 단계이고.... 두 번째는  .class 파일을 실행시키는 실행단계이다. 이 두 단계를 거치면 우리는 작성한 프로그램을 실행시켜 볼 수 있다.

![Alt text](/JAVA/Java_compile.png "JAVA 컴파일과정")

Compiler는 javac.exe에 해달되고, java VM은 java.exe에 해당된다.

      1. 소스코드(MyProgram.java)를 작성한다.
      2. 컴파일러(Compiler)는 자바 소스코드를 이용하여 클래스 파일(MyProgram.class)을 생성한다. 컴파일 된 클래스 파일은 Java VM(Java Virtual Machine)이 인식할 수 있는 바이너리 파일이다.
      3. Java VM(JVM)은 클래스 파일의 바이너리 코드를 해석하여 프로그램을 수행한다.
      4. MyProgram 수행결과가 컴퓨터에 반영된다.

*왜 귀찮게 class라는 걸 만드는가?*  
C,C++과 같은 언어는 컴파일 된 실행파일을 모든 운영체제에서 동일하게 사용할 수 없다.  
Java는 JVM같은 중간단계를 꼭 거쳐 느리긴 하지만, 어떤 OS에서라도 사용할 수 있다.

######


### 자바 시작하기
#### 변수(variable)
##### 변수명
+ 변수명은 숫자로 시작할 수 없다.
+ _ (underscore)와 $ 문자 이외의 특수문자는 사용할 수 없다.
+ 자바의 키워드는 변수명으로 사용할 수 없다.(예:int, class, return 등)
``` JAVA
    int a;
		a = 1;
		System.out.println(a+1);    //2

		a = 2;
		System.out.println(a+1);    //3
```


##### 자료형(Type)
+ int a; ... "변수 a는 int자료형 변수이다. 즉 a라는 변수에 정수값만 담을 수 있다."
+ string b; ... "변수 b는 String 자료형 변수이다. 즉 b라는 변수에 문자열값만 담을 수 있다."

##### 자주 쓰이는 자료형
  - int              // 정수... 0을 기준으로 한 양수와 음수를 의미.
  - long
  - double           // 정수와 정수 사이의 존재하는 무한대의 많은 수. ex) 1.1, 1.11
  - boolean
  - char
  - String
  - StringBuffer     // ex) StringBuffer sb;
  - List             // ex) List myList;
  - Map

##### 사용자 정의 자료형
``` java
class Animal {

}

Animal cat;
```
cat이라는 현수는 Animal 자료형 변수이다.  cat이라는 변수에는 Animal 자료형에 해당되는 값만 담을 수 있다.


#### 주석
##### 블록 주석
``` JAVA
/*
프로그램의 저작권
이 프로그램의 저작권은 홍길동에게 있습니다.
Copyright 2018.
*/
public class MyProgram {
}
```

##### JavaDoc 주석
``` JAVA
/**
 * Prints an integer and then terminate the line.  This method behaves as
 * though it invokes <code>{@link #print(int)}</code> and then
 * <code>{@link #println()}</code>.
 *
 * @param x  The <code>int</code> to be printed.
 */
public void println(int x) {
    synchronized (this) {
        print(x);
        newLine();
    }
}
```

##### 세미콜론
세미콜론을 이용하면 여러개의 문장을 한줄에 표현할 수 있다.
``` JAVA
int a = 100; double b = 10.1;
```


#### main 메소드
main 메소드는 프로그램의 시작을 의미한다. 만약 main 메소드가 없다면 프로그램을 단독으로 수행시킬 수 없다.  

``` java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
main 메소드를 보면 메소드명(main) 앞에 public, static 그리고 void 라는 키워드가 있다.   
메인 메소드는 프로그램 실행시 파라미터를 입력으로 받을 수 있다. 프로그램 실행시 전달되는 파라미터는 메소드의 입력 파라미터 String[] args에 저장된다.  
프로그램을 실행하기 위해서는 반드시 main 메소드를 통해야 한다.


### 3장.자료형
자료형이란, 프로그래밍을 할 때 쓰이는 '숫자', '문자열' 등의 자료 형태로 사용하는 그 모든 것을 뜻한다. 프로그램의 가장 기본이 되고 핵심적인 단위가 되는 것이 바로 자료형이다.  
프로그래밍 언어를 배울 때 '그 언어의 자료형을 알게 된다면, 이미 그 언어의 반을 터득한 것이나 다름없다'. 언어형은 중요하다.  

#### 숫자(Number)
##### [ 정수 ]
자바의 정수를 표현하기 위한 자료형은 int, long이다.  
int와 long의 차이는 표현할 수 있는 숫자의 범위이다.

|자료형 |표현범위                                   |
|-------|------------------------------------------|
|int    |-2147483648 ~ 2147483647                  |
|long   |-9223372036854775808 ~ 9223372036854775807|


##### [ 실수 ]
자바의 실수를 표현하기 위한 자료형은 float, double 이다.  
float와 double의 차이 역시 표현할 수 있는 숫자의 범위이다.  

자바에서 실수형은 디폴트가 double이므로, float 변수에 값을 대입할 때는 3.14F와 같이 'F'접미사(or 'f')를 꼭 붙여 주어야 한다. 접미사를 누락하면 컴파일 에러가 발생한다.


##### [ 8진수와 16진수 ]
8진수와 16진수는 int 자료형을 사용하여 표기할 수 있다.  
0으로 시작하면 8진수, 0x로 시작하면 16진수가 된다.  


##### [ 숫자연산 ]
``` java
System.out.println(1+2);
결과 : 3

System.out.println(1.2+1.3);
결과 : 2.5

곱하기를 할 때는 *(에스터리스크, Asterisk, 키보드 자판상으로 숫자 8 위)를 사용한다.
System.out.println(2*5);
결과 : 10

나누기를 할 때는 /(슬래쉬, slash, 키보드 자판상으로 오른쪽 shift 키 왼쪽)를 사용한다.
System.out.println(6/2);
```


##### [문자와 문자열]

자바는 문자(Character)와 문자열(String)을 구분한다.  
문자는 한 글자를 의미하고, 문자열은 여러 개의 문자가 결합한 것을 의미한다.  
자바에서 문자는 '(작은따옴표)로 감싸야 한다.
``` java
System.out.println('생');
```

문자열은 "(큰따옴표)로 감싸야 한다.
``` java
System.out.println("생활코딩");

만약 문자열을 작은 따옴표로 감싸면 에러가 발생한다.  
System.out.println('생활코딩');

하나의 문자를 큰따옴표로 감싼다고 에러가 발생하지는 않는다.
한 글자도 문자열이 될 수 있기 때문이다.
System.out.println("생");
```
문자 또는 문자열을 합칠때는 '+' 연산자를 이용한다.


###### 만약 문자열 안에 큰 따옴표를 넣고 싶다면 어떻게 해야 할까?
``` java
System.out.println("egoing said "Welcome programming world"");
```
오류가 발생한다.
이럴때는 아래와 같이 처리하면 된다.
``` JAVA
System.out.println("egoing said \"Welcome programming world\"");   
```  

###### 여러 줄의 표시
여러 줄을 표시하고 싶을 때는 아래와 같이 하면 된다.
``` JAVA
System.out.println("HTML\nCSS\nJavaScript\n");
```


##### 데이터 타입
###### 데이터 크기
|8 bit (비트)	               |1 byte      |
|1024 byte (바이트)	        |1 kilobyte  |
|1024 kilobyte (킬로바이트)	|1 megabyte  |
|1024 megabyte (메가바이트)	|1 gigabyte  |
|1024 gigabyte  (기가바이트)	|1 terabyte  |
|1024 terabyte (테라바이트)	|1 petabyte  |
|1024 petabyte (페타바이트)	|1 exabyte   |
|1024 exabyte (엑사바이트)	  |1 zettabyte |

###### 정수형
|데이터 타입 |메모리의 크기| 표현 가능 범위                                          |
|byte	      |1 byte      |	-128 ~ 127                                            |
|short	    |2 byte	     |-32,768 ~ 32,767                                        |
|int	      |4 byte	     |-2,147,483,648~2,147,483,647                            |
|long	      |8 byte	     |-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807  |


### 4장.제어문
### 5장.객체지향 프로그래밍
### 6장.입출력
### 7장.자바 날개 달기
### 8장.연습문제
### 9장.작은 프로젝트
