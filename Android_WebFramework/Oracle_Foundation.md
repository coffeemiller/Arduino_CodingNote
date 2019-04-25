
### [2019-04-24]

#### 1. Review
+ 지금까지 자바를 통해 프로그램 개발의 기초를 배워왔다.
+ 프로그램을 작성하는데, 필요한 데이터들은 메모리상에 올려 작동하는 형태.
+ 대량의 데이터를 사용자가 손쉽게 관리하는 전체적인 시스템을 "데이터페이스 시스템"이라고 한다.

#### 2. 데이터베이스(오라클) 개요
+ 한 조직의 응용 시스템에서 공유하기 위해서 통합되어 있고 저장된 운영 데이터의 집합.
  - 통합 데이터 : 최소한의 중복 허용
  - 저장되어진 데이터 : 자료구조를 가지고 있다.(일관성, 일치성, 강건성)
    - 저장되어진 데이터는 변경 시 관련되어진 작업영역에 동일 영향
  - 운용 데이터 : 자료의 조작, 정보의 산출
  - 공유 데이터 : 보안과 권한.

+ 데이타베이스(Database) : 한 조직의 응용 시스템에서 공유하기 위해서 통합되어 있고 저장된 운영 데이터의 집합(SW)

+ 용어설명
  - Table : RDBMS의 기본 Data 저장구조
  - Column : Table의 한 부분으로 Column명과 Data Type을 가지고 있음
  - Row : Table에 있는 필드집합의 하나(레코드의 동의어)
  - Primary key : Table에서 각 row를 유일하게 구분해주는 Column
  - Foreign key : 다른 table과 loin하기 위해 사용되어진 Column
  - Field : Table에서 row와 Column이 교차하는 곳에 있는 data
  - DBMS (Database Management System) : 데이타베이스의 자료를 저장할 구조를 만들고, 조작하고, 값의 일치성을 보장하기 위한  물적자원(하드웨어,소프트웨어)과 인적자원(관리자,분석가,설계가,프로그래머,DBA,OP등) 및  그 관리 체계를 통칭

##### [오라클 학습]
+ 1) 직접 tool을 사용하여 oracle서버에 접속하여 sql명령을 사용하여 db를 조작/운영
  - (1)기본제공 tool : sql*plus(SQL Command Line)  --- Text mode(CUI)
  - (2)외부 tool (다양) : sqldeveloper (그중 하나)  --- Window mode(GUI)
+ 2) Java Application에서의 사용
  - JDBC학습 --> java.sql 패키지의 다양한 클래스 사용

```
오라클을 설치한후 오라클데이타베이스 서버 작동구조
 ==================================================

 tool(sql*plus)            오라클 서버
사 ------------| SQL명령 ------>1)SQL 엔진         실제데이타를 저장/관리 하는 객체
용|		 | 	                            --------------------------------------------------------
자| 		 | <------결과---프로그램실행<--->|테이블(table), 인덱스(index), 시퀀스(sequence) ,.....   |
  |            |                        --------------------------------------------------------
   ------------|
   자체명령
   SQL명령  ==> 명령끝에 세미콜론(;)을 사용한다.
```

##### 명령어 사용
```
--------------------------------------------- sql*plus 자체명령
- SQL> CONNECT
- SQL> SHOW USER  ==> 현재 사용자 즉, 접속자명 보여주기
- SQL> CLEAR SCREEN ==> 화면지우기
- SQL> SET PAGESIZE 50
- SQL> SET LINESIZE 120 ==>화면에대한 설정
- SQL> DESC 테이블명      ==> 테이블 구조 보여주기
***SQL*PLUS자체명령은 앞4글자만 사용해도 된다.
---------------------------------------------- sql명령
- SQL> SELECT * FROM TAB;      ==> 현재 사용자가 사용할 수 있는 테이블 정보 보여달라.
- SQL> SELECT * FROM 테이블명;==> 해당 테이블의 내용을 보여달라
```

```
1.1 관계형 모델의 구성요소
1)	데이터를 저장하는 객체(object) 또는 관계(relation)들의 집합
2)	다른 관계를 생성하기 위해 관계에 가해지는 일련의 연산자 집합
3)	정확성과 일관성을 위한 데이터의 무결성(Integrity)

1.2 관계형 데이터베이스 기능
관계(relation)형 데이터베이스는 정보 저장을 위해 관계나 2차원 테이블을 이용한다.
1)	데이터의 저장을 관리한다.
2)	데이터에 대한 ACCESS을 통제한다.
3)	데이터를 검색 및 수정하기 위한 수단을 제공한다.
```

+ 관계형 모델의 3대 구성요소
  - 1) 자료구조(데이터 저장구조 및 다른 데이터와의 관계)
  - 2) 정보추출에 필요한 기능 ==> 연산자
  - 3) 결함이 없는 데이터 관리 ==> 무결성 제약조건


+ 관계형 데이터베이스의 정리
  - 1)E.F.Codd 박사는 1970년 데이터베이스 시스템용 관계형 모델을 제안
  - 2)제시한 관계형 모델은 관계형 데이터베이스 관리 시스템의 기본이 된다.
  - 3)관계형 모델링은 다음 구성요소를 포함하고 있다.
    - (1)객체(object) 또는 관계(relation)의 집합
    - (2)관계(relation)에 가해지는 연산의 집합
    - (3)정확성 및 일관성을 위한 데이터의 무결성
  - 4)관계형 데이터베이스는 2차원 테이블 형태로 구성된다.
  - 5)각 테이블은 Row와 Column으로 구성되어 있다.
  - 6)각 행의 데이터는 유일하다.
  - 7)각 column은 데이터 무결성을 유지한다.
  - 8)SQL 명령어를 실행함으로 행들의 데이터를 조작 가능하다.
```
[반드시 기억하자!!]
테이블에 데이터를 저장한다.
테이블은 행과 열로 구서되어 있다. (ROW, COLUMN)
행(ROW)은 1건의 논리적인 데이터를 의미한다.
1건의 데이터를 구성하는 개별 항목을 열(COLUMN)이라고 부른다.
모든 행은 식별성을 가지고 있어야 한다.(중복불가)

기본키(Primary Key : PK)
다른테이블과 연결되는 컬럼을 외래키(Foreign Key : FK)
NULL(널) 값 : 값이 없다. 값이 지정되지 않았다. (0아님)

데이터베이스에서 스키마(저장구조)
내부적인 상세한 구조 -- 내부 스키마(공개되지 않는다.)
외부적인 사용법에 필요한 구조 -- 외부 스키마(공개됨.)

테이블의 외부 스키마 ==> DESC 테이블명

SQL명령을 사용하여 DB관리에 필요한 모든 기능을 수행한다.
즉, 추가/수정/삭제/정렬/검색 등의 모든 명령
```

+ 오라클에서 사용되는 객체들
  - 1) 테이블(TABLE) : 핵심. ROW와 COLUMN으로 구성된 기본적인 저장 단위
  - 2) VIEW : 하나 이상의 TABLE로부터 논리적으로 데이터를 분류한 부분집합
  - 3) INDEX : 포인터를 사용하여 행의 검색 속도를 향상
  - 4) SEQUENCE : 자동적으로 ORACLE SERVER이 유일 번호를 생성
  - 5) WYNONYM : 객체에 대체 이름을 부여
  - 6) PROGRAM UNIT : SQL또는 PL/SQL 문으로 작성한 PROCEDURE, FUNCTION, PACKAGE
  - 7) SQL로 작성된 프로그램(변수, 제어문 ==> 함수, 프로시저, 패키지)

+ 현재의 TOOL에서 사용하는 명령어
  - 1) 자체명령어(CONNECT, DESC, SHOW USER, CLEAR SCREEN, SPOOL) - 오라클
  - 2) SQL명령(SELECT)

```
  [용어정리]
  1) SQL : 데이터페이스를 조작하는 명령
  2) sql*plus : 현재 사용하고 있는 tool 명칭(SQL Command Line)
    -----> 여기서 SQL명령을 작성하고 실행시키면 데이터베이스 서버로 명령을 넘겨준다.
          서버에서 명령을 해석하여 해당 기능을 작동시키고(SQL엔진), 그결과를 다시 tool에 전송.
          이 결과값을 보기 좋게 보여준다.
  3) PL/SQL : SQL명령을 사용하여 프로그램 작성가능. 이를 DB에 저장해 놓고 필요할때 호출사용.
```

+ SQL명령의 종류
  - 1) 검색/질의(Query) : SELECT
  - 2) DML(데이터 조작 명령) : 추가(INSERT), 수정(UPDATE), 삭제(DELETE)
  - 3) DDL(데이터저장구조) : 테이블/생성(CREATE TABLE), /구조변경(ALTER TABLE), /삭제(DROP TABLE)
  - 4) TCL(트랜잭션 관리) : COMMIT, ROLLBACK, SAVEPOINT
  - 5) DCL(권한, 보안) : GRANT, REVOKE

#### 3. SELECT문
![SELECT FROM](select.png "SELECT FROM")
```
1.특정 종업원에 대한 모든 데이터를 나타내는 단일 row(오라클) 또는 tuple 입니다.
  기본 키(primary key)에 의해 식별되어져야 합니다.

2.기본 키(primary key)인 종업원 번호를 포함하는 열(column) 또는 속성(attribute)입니다.
  종업원 번호는 EMP테이블에서 유일한(unique) 종업원을 식별합니다.

3.키 값이 아닌 열 입니다. 열은 테이블에서 한 종류의 데이터를 나타냅니다.

4.부서 번호를 포함하는 열은 외래 키(foreign key)입니다.
  외래 키는 테이블 간에 서로 어떻게 관련되었는가를 정의합니다.
  외래 키는 다른 테이블의 기본 키 또는 고유 키를 참조합니다.

5.필드(field) 는 행과 열의 교차되는 곳에 있습니다.

6.필드는 그 안에 값을 가지지 않을 수도 있습니다. 이것은 null value라 불립니다.
  EMP테이블에서 영업 사원인 종업원만이 COMM(commission)필드에서 값을 가집니다.
```

+ 자체명령과 SQL명령 구분 ==> 세미콜론(;)
+ 모든명령에서 keyword(예약어)는 대문자로 기술하자!! (관례적 규칙)
+ 사용자정의어(테이블명, 컬럼명)은 소문자로 기술하자!! (관례적 규칙)

+ 가장 기본적인 명령
  - 테이블에서 데이터를 검색하는 질의명령 ==> SELECT

+ SELECT
  - SELECT문을 사용하여 원하는 정보를 테이블에서 추출할 수 있다.
  - SELECT문은 내부적으로 (원하는 행추출 ==> selection 연산, 원하는 컬럼추출 ==> projection, 다른테이블과 연결하는 것 ==> join) 기능을 수행한다.
  - 사용법
    - ```SELECT {*,column명 [별칭],...} FROM 테이블명 [WHERE 조건] [ORDER BY {컬럼, 표현식}] [ASC|DESC]; ```

  - 현재 계정소유자가 사용할 수 있는 테이블 목록보기
    - ```SELECT * FROM TAB```
    - 원래 테이블명은 사용자가 지정하여 만든다. 단, 내부적으로 저장될때는 대문자로 저장된다.
    - 사용할때는 소문자로 테이블명을 기술하겠지만... 내부적으로 대문자로 저장되어 있음.(주의)

  - SELECT문의 결과값이 보여지는 순서는 ORDER BY절을 사용하지 않는이상 임의적이다(교재와 결과가 다를 수 있다.)

+ SET HEADING ON/OFF
  - 최상단의 구분행을 보여줄지 여부.
  - 사용법
    - ```SET HEADING OFF``` 또는 ```SET HEADING ON```

+ SQL명령을 사용할때 미리 알고 있어야 하는 테이블의 개략구조 알아보기
  - DESCRIBE(DESC) 테이블명
  - emp테이블의 구조
    - ```DESC emp```
    - 테이블에 저장될 값의 종류 ==> SQL 데이터 타입(자료형)
    - 대표적인 자료형
      + 문자 : 고정길이문자(CHAR), 가변길이문자(VARCHAR2)
      + 숫자 : 정수(NUMBER)or(NUMBER(자릿수)), 실수(NUMBER(전체자릿수,소숫점이하자릿수))
      + 날짜 : DATE(년월일시분초MS) ==> 내부적으로는 무조건 7byte사용

+ 현재의 날짜를 표현하는 예약어 ==> SYSDATE
+ 참고) literal 값을 직접 표현할때 숫자는 그대로, 문자는'가','JICA'

+ NULL값의 처리
  - NULL은 이용할 수 없고 할당되지 않고 알려져 있지않고 적용 불가한 값을 의미한다.
  - NULL이란 0나 공백(space)과 다르다.
  - 널 값을 포함한 산술 표현식 결과는 NULL이 된다.
  - column에 데이터 값이 없으면 그 값 자체가 널 또는 널 값을 포함하고 있다.
  - 널 값은 1바이트의 내부 저장 장치를 오버헤드로 사용하고 있으며 어떠한 datatype column들이라도 널 값을 포함할 수 있다.
  - NULL이면 0으로 처리되게 하는 함수(FUNCTION)이 ==> ```NVL(컬럼명,대체값)```이다.
    - ```SELECT empno, ename, sal, comm, sal*12+NVL(comm,0) FROM emp;```
  - SELECT문의 컬럼명을 대신하는 별칭을 만들어서 사용할 수 있다.
    - ```SELECT 컬럼명 AS 별칭```
    - ```SELECT 컬럼명 별칭```
    - 단, 공백을 포함한 별칭을 사용할때는 " "을 사용한다.
    - Oracle의 SQL명령에서 더블쿼테이션("")의 사용은 극히 드물다. 거의 이때만 사용.

+ SELECT문을 사용하면서, '자료형', '연산자', '함수' 등을 사용한다.

+ 중복행을 제거할때 DISTINCT를 사용한다.
  - EMP 테이블에서 담당하고 있는 업무의 종류를 출력 : ```SELECT DISTINCT job FROM emp;```
  - 부서별로 담당하는 업무를 한번씩 부서번호 오름차순 출력 : ```SELECT DISTINCT deptno, job FROM emp ORDER BY deptno;```


#### 4. 실습
#### 7. Summary / Close

-----------------------------------------------------------

### [2019-04-25]

#### 1. Review

#### 2. SQL문 실습
+ 조건을 기술할때 다양한 연산자를 사용할 수 있다.
  - ```SELECT * FROM emp WHERE sal >= 3000;```
  - INITCAP() : 괄호안의 정보에서 앞글자만 대문자로 변환

+ 연산자의 종류
  - 1) 산술연산자 : +, -, `*`, /, MOD()
  - 2) 비교연산자 : =, !=, <>, ^=, >, >=, <, <=
  - 3) 논리연산자 : AND, OR, NOT
  - 4) SQL연산자 : BETWEEN AND, IN, LIKE, IS NULL, NOT NULL
  - 5) 기타연산자 : DECODE, CASE WHEN

+ 날짜값(DATE)을 표현할때는 사용하는 지역과 언어에 따라 표현형식이 달라질수 있다.
  - SQL문장에서 문자데이터와 날짜데이터를 직접 기술할때는 반드시 단일 인용 부호('')사용.
  - 미국표현식으로...
    + ```ALTER SESSION SET NLS_DATE_FORMAT = 'DD-MON-YY';``` 날짜형식 변환
      - DD는 후에 4자리로 항상 검색해야됨.... RR을 쓰면 2자리로도 인식가능.
    + ```ALTER SESSION SET NLS_LANGUAGE = AMERCIAN;``` 미국식으로 언어를 바꿈
    + ```SELECT empno, ename, job, sal, hiredate, deptno FROM emp WHERE hiredate >= '01-JAN-1982';``` 4자리 연도를 표시해야 인식됨.

  - 한국표현식으로...
    + ```ALTER SESSION SET NLS_DATE_FORMAT = 'RR/MM/DD';``` 2자리연도/월/날
    + ```ALTER SESSION SET NLS_LANGUAGE = KOREAN;``` 한국식으로 표기

+ 논리연산자(AND, OR, NOT)  ==> JAVA에서 &&, ||, !
  - AND연산자 : ```SELECT empno,ename,job,sal,hiredate,deptno FROM emp WHERE sal >= 1100 AND job = 'MANAGER';```
  - OR연산자 :  ```SELECT empno,ename,job,sal,hiredate,deptno FROM emp WHERE sal >= 1100 OR job = 'MANAGER';```
  - OR연산자 : ```SELECT * FROM emp WHERE job='MANAGER' OR job='CLERK' OR job='ANALYST';```
  - NOT연산자 :  ```SELECT * FROM emp WHERE NOT(job='MANAGER' OR job='CLERK' OR job='ANALYST');```

  - AND, OR, NOT의 연산자 사용법은 Java언어에서 &&,||,!의 사용법과 동일하다.

+ SQL에서만 사용하는 SQL연산자가 있다.
  - 1) BETWEEN AND ====> AND연산의 단순표현
    + ```SELECT * FROM emp WHERE sal >= 1250 AND sal <= 1500;```
    + 같은표현으로 ```SELECT * FROM emp WHERE sal BETWEEN 1250 AND 1500;```
    + AND연산으로 표현된 식의 결과 범위를 가지고 표현될때 BETWEEN a AND b 로 대치가능.

  - 2) IN ====> OR연산의 단순표현
    + ```SELECT * FROM emp WHERE empno=7902 OR empno=7788 OR empno=7566;```
    + 같은표현으로 ```SELECT * FROM emp WHERE empno IN(7902,7788,7566);```
    + OR연산으로 표현된 등가비교의 값이 여러개일때 IN연산자로 대치가능.

  - 3) LIKE
    + LIKE연산은 문자열의 값을 와일드카드(%,`_`)를 사용하여 패턴비교를 수행한다.
    + S로시작하는 검색...```SELECT * FROM emp WHERE ename LIKE 'S%';```
    + S로끝나는 검색...```SELECT * FROM emp WHERE ename LIKE '%S';```
    + S가(어디든)들어가는 검색...```SELECT * FROM emp WHERE ename LIKE '%S%';```
    + 와일드카드 ```%``` 한글자도 없거나 한글자이거나, 두글자이거나, 여러글자여도 상관없다.
    + 와일드카드 ```_``` 한글자만 의미하고 어느글자이든 상관없다.
    + 5번째가S인 검색...```SELECT * FROM emp WHERE ename LIKE '____S';```
    + 82년도 검색... ```SELECT * FROM emp WHERE hiredate LIKE '82%';```
    + 참고
      - name에 값이 X_Y가 포함되어 있는 문자열을 조회하고자 할 경우 Escape를 사용한다
      - ```WHERE name LIKE ‘%X\_Y%’ ESCAPE ‘\’;```

  - 4) IS NULL, NOT NULL
    + 추가수당이 없는(null)인사람...```SELECT * FROM emp WHERE comm IS NULL;```
    + 추가수당이 없지(null) 않은사람...```SELECT * FROM emp WHERE NOT comm IS NULL;```
      - 위와 같은 표현.... ```SELECT * FROM emp WHERE comm IS NOT NULL;```
    + 에러에 유의.... 'IS NULL' <> 'IS NOT NULL' 또는 'NOT 컬럼 IS NULL'

+ WHERE조건절에 산술연산, 비교연산, 논리연산, SQL연산(BETWEEN AND, IN, LIKE, IS NULL)사용가능.

+ 우선 순위 규칙
  - 괄호 > 모든 비교 연산자 > NOT > AND > OR
  - 연산자도 우선순위가 있지만, 직접 괄호를 사용하여 우선순위를 명시적으로 지정하는 것이 좋음.

+ ORDER BY절을 사용하여 출력순서를 결정하자(SORT ==> 오름차순 ASC/내림차순 DESC)
  - 이름으로 오름차순... ```SELECT * FROM emp ORDER BY ename (ASC);```
  - 이름으로 내림차순... ```SELECT * FROM emp ORDER BY ename DESC;```
  - 급여1500이상 오름차순...```SELECT * FROM emp WHERE sal > 1500 ORDER BY sal;```
  - 정렬의 순서
    + 작은숫자 < 빠른날짜 < A-Z-a-z < NULL

  - 같은표현 3가지
    + ```SELECT empno, ename, job, sal, sal*12 AS Annsal FROM emp ORDER BY Annsal;```
    + ```SELECT empno, ename, job, sal, sal*12 AS Annsal FROM emp ORDER BY sal*12;```
    + ```SELECT empno, ename, job, sal, sal*12 AS Annsal FROM emp ORDER BY 5;```

  - 여러개 기준정렬
    + ```SELECT deptno, empno, ename, job, sal FROM emp ORDER BY deptno, sal DESC;```

```
SELECT 컬럼
FROM 테이블명
WHERE 조건
ORDER BY 정렬기준
GROUP BY
HAVING
```
+ 먼저 함수를 학습해야 GROUP BY와 HAVING을 적용할때 용이하다.

#### 3. 단일행 함수
+ 함수 : 특정 기능을 수행하는 명령어들의 모임(JAVA언어에서 메서드와 비슷)

+ 함수의 종류
  - 1) 단일행 함수 : 각 행(row)별로 함수를 적용시켜 결과를 만든다.
    + 각 함수마다 리턴값이 정해져 있다.
    + (1) 문자형 함수 : 문자를 입력 받고 문자와 숫자 값 모두를 RETURN할 수 있다.
      - 해당 컬럼을 소문자로...```SELECT empno, ename, LOWER(ename), sal FROM emp;```
      - ```SELECT empno, ename FROM emp WHERE LOWER(ename)='smith';```
      - 해당 문자의 숫자계산...```SELECT LENGTH('Oracle') FROM DUAL;```
```
[대소문자변환]
SQL> SELECT empno, ename, LOWER(job), deptno FROM emp WHERE LOWER(ename)='scott';

     EMPNO ENAME                LOWER(JOB)             DEPTNO
---------- -------------------- ------------------ ----------
      7788 SCOTT                analyst                    20

SQL> SELECT empno, ename, job FROM emp WHERE ename=UPPER('scott');

     EMPNO ENAME                JOB
---------- -------------------- ------------------
      7788 SCOTT                ANALYST

[문자열의 첫글자만 대문자로 변환 : INITCAP]
SQL> SELECT 'ORACLE DATABASE', INITCAP('ORACLE DATABASE') FROM DUAL;

'ORACLEDATABASE'               INITCAP('ORACLEDATABASE')
------------------------------ ------------------------------
ORACLE DATABASE                Oracle Database


[두 문자열을 합치는 함수 ==> CONCAT()]
SQL> COL e_name FORMAT A15
SQL> COL e_empno FORMAT A15
SQL> COL e_job FORMAT A15
SQL> SELECT empno,ename,job,CONCAT(empno,ename) e_name,
  2  CONCAT(ename,empno) e_empno,
  3  CONCAT(ename,job) e_job
  4  FROM emp
  5  WHERE deptno = 10;

긴 문장일때는 파일(*.sql)을 만들어서 실행(@경로\파일.sql)
이때... 실행과정을 보려면, ( SQL> SET ECHO ON )으로 설정

\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
[숫자와 문자를 합치도록 CONCAT()을 사용하면 내부적으로 자동현변환]
SQL> SELECT empno, ename, CONCAT(empno,ename), CONCAT(ename,empno) FROM emp;
---------------------------------------> 가독성이 없어서 수정이 필요함.
SQL> COL e_name FORMAT A15  -> 결과값으로 문자 15자리를 출력할수 있는 공간확보.


[SQL 자료형]
SQL> DESC emp
 Name                               Null?    Type
 --------------------------------- -------- --------------------------------------------
 EMPNO                             NOT NULL  NUMBER(4)
 ENAME                                       VARCHAR2(10)
 JOB                                         VARCHAR2(9)
 MGR                                         NUMBER(4)
 HIREDATE                                    DATE
 SAL                                         NUMBER(7,2)
 COMM                                        NUMBER(7,2)
 DEPTNO

CHAR(10) ==> 고정길이 문자열
VARCHAR2(10) ==> 가변길이 문자열
NUMBER(5) ==> 고정길이 정수
NUMBER ==> 가변길이 정수
============================================================
[SUBSTR() : 부분문자열 발췌함]
SQL> SELECT 'ORACLE JICA', SUBSTR('ORACLE JICA', 8, 4) FROM DUAL;

'ORACLEJICA'           SUBSTR('
---------------------- --------
ORACLE JICA            JICA
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
SQL> SELECT * FROM emp WHERE SUBSTR(ename, 1,1) > 'K' AND SUBSTR(ename,1,1) < 'Y' ORDER BY ename;

EMPNO ENAME         JOB                MGR HIREDATE        SAL       COMM     DEPTNO
----- -------  ---------------- ---------- -------- ---------- ---------- ----------
7654 MARTIN        SALESMAN          7698 81-09-28       1250       1400         30
7934 MILLER        CLERK             7782 82-01-23       1300                    10
7788 SCOTT         ANALYST           7566 87-04-19       3000                    20
7369 SMITH         CLERK             7902 80-12-17        800                    20
7844 TURNER        SALESMAN          7698 81-09-08       1500          0         30
7521 WARD          SALESMAN          7698 81-02-22       1250        500         30

6 rows selected.
=============================================================

[LENGTH() 문자열의 길이 구하기]
SQL> SELECT LENGTH('전주'), LENGTH('jeonju') FROM DUAL;

LENGTH('전주') LENGTH('JEONJU')
-------------- ----------------
             2                6
LENGTH(문자열), LENGTH(숫자)-내부적으로 자동형변환(NUMBER->CHAR)
문자함수는 문자형을 매개변수로 사용하고 결과값을 문자값 혹은 숫자값을 리턴한다.


[INSTR() 문자열에서 지정한 문자열의 위치를 검색]
SQL> SELECT INSTR('DataBase', 'B') FROM DUAL;

INSTR('DATABASE','B')
---------------------
                    5
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
SQL> SELECT INSTR('DataBase', 'a', 1, 3) FROM DUAL;

INSTR('DATABASE','A',1,3)
-------------------------
                        6
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
SQL> SELECT ename, INSTR(ename,'L') e_null,
  2                          INSTR(ename,'L',1,1) e_11,
  3                          INSTR(ename,'L',1,2) e_12,
  4                          INSTR(ename,'L',4,1) e_41,
  5                          INSTR(ename,'L',4,2) e_42
  6  FROM emp
  7  ORDER BY ename;

ENAME                    E_NULL       E_11       E_12       E_41       E_42
-------------------- ---------- ---------- ---------- ---------- ----------
ADAMS                         0          0          0          0          0
ALLEN                         2          2          3          0          0
BLAKE                         2          2          0          0          0
CLARK                         2          2          0          0          0
FORD                          0          0          0          0          0
JAMES                         0          0          0          0          0
JONES                         0          0          0          0          0
KING                          0          0          0          0          0
MARTIN                        0          0          0          0          0
MILLER                        3          3          4          4          0
SCOTT                         0          0          0          0          0
SMITH                         0          0          0          0          0
TURNER                        0          0          0          0          0
WARD                          0          0          0          0          0

===============================================================

[LPAD(), RPAD() : 지정한 문자열의 왼/오른쪽에 특정문자로 채움]
SQL> SELECT 'MILLER', LPAD('MILLER',10,'*'), RPAD('MILLER',10,'X') FROM DUAL;

'MILLER'     LPAD('MILLER',10,'*' RPAD('MILLER',10,'X'
------------ -------------------- --------------------
MILLER       ****MILLER           MILLERXXXX

================================================================

[LTRIM() : 왼쪽에서 지정문자제거]
SQL> SELECT 'MILLER', LTRIM('MILLER','M'), RTRIM('MILLER','R') FROM DUAL;

'MILLER'     LTRIM('MIL RTRIM('MIL
------------ ---------- ----------
MILLER       ILLER      MILLE

SQL> SELECT ename, job, LTRIM(job,'A'), sal, LTRIM(sal,'1') FROM emp;

==============================================================

[TRANSLATE, REPLACE ==> 다른 문자열로 대체]
SQL> SELECT TRANSLATE('MILLER','L','*') FROM DUAL;

TRANSLATE('M
------------
MI**ER
///////////////////////////////////////////////////////
SQL> SELECT TRANSLATE('MILLER','MLE','X*@') FROM DUAL;

TRANSLATE('M
------------
XI**@R
///////////////////////////////////////////////////////

TRANSLATE()는 지정한 글자를 새로운 문자로 변환

==================================================================

[REPLACE() : 원래 문자열(여러문자)을 새로운 문자열로 대체시킨다.]
SQL> SELECT REPLACE('JACK AND JUE', 'J', 'BL') FROM DUAL;

REPLACE('JACKANDJUE','J','BL
----------------------------
BLACK AND BLUE


오라클 내부에서는 한글한글자를 저장하기 위해 실제 사용하는 BYTE는 3byte
```
    + (2) 숫자형 함수 : 숫자를 입력 받고 숫자를 RETURN한다.
    + (3) 날짜형 함수 : 날짜형에 대해 수행하고 숫자를 RETURN하는 MONTHS_BETWEEN 함수를 제외하고 모두 날짜 데이터형의 값을 RETURN한다.
    + (4) 형변환함수 : 어떤 데이터형의 값을 다른 데이터형으로 변환한다.
    + (5) 기타함수

  - 2) 그룹 함수 : 여러행(전체행, GROUP지정행들)을 대상으로 하나의 결과를 만든다.
    + 그룹함수는 7개 밖에 없다.
    + ```SELECT deptno, COUNT(deptno) FROM emp GROUP BY deptno;```

```
SQL> SELECT 2*5 FROM dept;

       2*5
----------
        10
        10
        10
        10
```

+ 오라클 내부에 1개의 row를 가진 임시테이블이 있다. 이 테이블을 DUAL테이블이다.
```
SQL> SELECT 2*5 FROM DUAL;

       2*5
----------
        10
```
  - DUAL : 임의의 테이블, 가상테이블(dummy 테이블) ==> DUAL

+ 오라클 학습시 참고 사이트(http://www.gurubee.net/)
  - 키워드는 대문자로 하자
  - 사용자 정의어 즉, 테이블명, 컬럼명은 소문자로 기술하자
  - 함수명도 대문자로 작성하자  <=======관례적 규칙

  - 문자열 합치기 연산자 ==> ||

##### [오늘의 과제]
+ 3.doc 연습문제 풀기

#### 3. 실습
#### 4. Summary / Close

-----------------------------------------------------------


### [2019-04-26]

#### 1. Review
#### 3. 실습
#### 4. Summary / Close

-----------------------------------------------------------
