
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
    + ```ALTER SESSION SET NLS_LANGUAGE = AMERICAN;``` 미국식으로 언어를 바꿈
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
#### 2. 단일행 함수
+ 숫자함수 : 숫자데이터를 매개변수로 사용하여 결과값을 숫자로 가진다.
```
[ROUND() 반올림 함수]
SQL> SELECT 456.789, ROUND(456.789, 2) FROM DUAL;

   456.789 ROUND(456.789,2)
---------- ----------------
   456.789           456.79
=========================================================================
DUAL테이블 : 현재 계정사용자가 만들지 않았지만 사용가능한 테이블(DUMMY TABLE)

SQL> DESC DUAL
Name                               Null?    Type
-------------------------------- -------- -------------------------------------------
DUMMY                                                                      VARCHAR2(1)

SQL> SELECT * FROM DUAL;

DU
--
X
============================================================================
[TRUNC() 버림(절삭)함수]
SQL> SELECT TRUNC(456.789,2) FROM DUAL;

TRUNC(456.789,2)
----------------
          456.78

SQL> SELECT TRUNC(4567.678), TRUNC(45673.678,0), TRUNC(4567.678,2), TRUNC(4567.678, -2) FROM DUAL;

TRUNC(4567.678) TRUNC(45673.678,0) TRUNC(4567.678,2) TRUNC(4567.678,-2)
--------------- ------------------ ----------------- ------------------
           4567              45673           4567.67               4500
=============================================================================
[MOD() 자바언어의 %연산자와 동일하게 나머지를 구한다.]
- 자바에서는 정수끼리 여산을 하면 정수값을 구해주었다. SQL에서는 정수끼리 연산도 실수결과값을 나타낸다.
SQL> SELECT 10/3, MOD(10,3) FROM DUAL;

      10/3  MOD(10,3)
---------- ----------
3.33333333          1

SQL> SELECT TRUNC(10/3) FROM DUAL;

TRUNC(10/3)
-----------
          3
===============================================================================

[POWER(), SQRT(), SIGN()]
SQL> SELECT POWER(2,3), SQRT(4), SIGN(-100) FROM DUAL;

POWER(2,3)    SQRT(4) SIGN(-100)
---------- ---------- ----------
         8          2         -1
===============================================================================

[CHR(아스키코드값)->문자]
SQL> SELECT empno,ename,job,ename || CHR(10) || job FROM emp WHERE deptno=20;

     EMPNO ENAME                JOB                ENAME||CHR(10)||JOB
---------- -------------------- ------------------ ---------------------
      7369 SMITH                CLERK              SMITH
                                                   CLERK

      7566 JONES                MANAGER            JONES
                                                   MANAGER
```

+ 날짜함수
  - 날짜데이터를 대상으로 기능을 수행하여 날짜값을 리턴한다. (단, MONTHS_BETWEEN() 숫자리턴)
```
[DATE 내부적으로 7BYTE 사용, 세기 년/월/일/시/분/초/1/1000초]
- 현재 날짜와 시각 ==> SYSDATE
SQL> SELECT SYSDATE FROM DUAL;
SYSDATE
--------
19/04/26
////////////////////////////////////////////////////////////////////
- 기본적으로 정수 +,- 가능
SQL> SELECT SYSDATE, SYSDATE+1, SYSDATE-1, SYSDATE+30 FROM DUAL;

SYSDATE  SYSDATE+ SYSDATE- SYSDATE+
-------- -------- -------- --------
19/04/26 19/04/27 19/04/25 19/05/26
////////////////////////////////////////////////////////////////////
- 날짜끼리의 -(뺄셈)연산
- 형변환 함수(TO_DATE())가 필요하다.
SQL> SELECT SYSDATE-TO_DATE('19/03/11','RR/MM/DD') FROM DUAL;

SYSDATE-TO_DATE('19/03/11','RR/MM/DD')
--------------------------------------
                            46.4315625
///////////////////////////////////////////////////////////////////
[날짜표현 방법]
- ALTER SESSION SET NLS_DATE_FORMAT='RR/MM/DD';
- ALTER SESSION SET NLS_LANGUAGE='KOREAN';

<문제풀이>
SQL> SELECT ename,hiredate,SYSDATE,SYSDATE - hiredate "Total Days",
  2          TRUNC((SYSDATE - hiredate) / 7, 0) Weeks,
  3          ROUND(MOD((SYSDATE - hiredate), 7), 0) DAYS
  4  FROM emp
  5  ORDER BY SYSDATE - hiredate DESC;

ENAME                HIREDATE SYSDATE  Total Days      WEEKS       DAYS
-------------------- -------- -------- ---------- ---------- ----------
SMITH                80/12/17 19/04/26 14009.4433       2001          2
ALLEN                81/02/20 19/04/26 13944.4433       1992          0
WARD                 81/02/22 19/04/26 13942.4433       1991          5
JONES                81/04/02 19/04/26 13903.4433       1986          1


//////////////////////////////////////////////////////////////////////////////
[MONTHS_BETWEEN() 두날짜 사이의 월수를 계산]
SQL> SELECT MONTHS_BETWEEN(SYSDATE, TO_DATE('20150508','YYYYMMDD')) FROM DUAL;

MONTHS_BETWEEN(SYSDATE,TO_DATE('20150508','YYYYMMDD'))
------------------------------------------------------
                                            47.5950329

SQL> SELECT ename,hiredate,SYSDATE,MONTHS_BETWEEN(SYSDATE,hiredate) m_between,
  2          TRUNC(MONTHS_BETWEEN(SYSDATE,hiredate),0) t_between
  3  FROM emp
  4  WHERE deptno = 10
  5  ORDER BY MONTHS_BETWEEN(SYSDATE,hiredate) DESC;

ENAME                HIREDATE SYSDATE   M_BETWEEN  T_BETWEEN
-------------------- -------- -------- ---------- ----------
CLARK                81/06/09 19/04/26 454.562819        454
KING                 81/11/17 19/04/26 449.304755        449
MILLER               82/01/23 19/04/26 447.111206        447
//////////////////////////////////////////////////////////////////////
[ADD_MONTHS() 날짜에 월(개월수)을 더한다]
SQL> SELECT SYSDATE, ADD_MONTHS(SYSDATE, 5) FROM DUAL;

SYSDATE  ADD_MONT
-------- --------
19/04/26 19/09/26
/////////////////////////////////////////////////////////////////////
[NEXT_DAY() 돌아오는 요일을 계산]
- 요일지정 숫자 : 일요일(1), 월요일(2)....토요일(7)
SQL> SELECT SYSDATE, NEXT_DAY(SYSDATE, 2) FROM DUAL;

SYSDATE  NEXT_DAY
-------- --------
19/04/26 19/04/29

SQL> SELECT SYSDATE, NEXT_DAY(SYSDATE, '월요일') FROM DUAL;

SYSDATE  NEXT_DAY
-------- --------
19/04/26 19/04/29

<문제풀이>
SQL> SELECT ename,hiredate,NEXT_DAY(hiredate,'금요일') n_day,
  2                        NEXT_DAY(hiredate,6) n_6,
  3                        NEXT_DAY(hiredate,7) n_7
  4  FROM emp
  5  WHERE deptno = 10
  6  ORDER BY hiredate DESC;

ENAME                HIREDATE N_DAY    N_6      N_7
-------------------- -------- -------- -------- --------
MILLER               82/01/23 82/01/29 82/01/29 82/01/30
KING                 81/11/17 81/11/20 81/11/20 81/11/21
CLARK                81/06/09 81/06/12 81/06/12 81/06/13
/////////////////////////////////////////////////////////////////////

[LAST_DAY() 해당월의 마지막 날짜]
SQL> SELECT SYSDATE, LAST_DAY(SYSDATE) FROM DUAL;

SYSDATE  LAST_DAY
-------- --------
19/04/26 19/04/30

<문제풀이 23>
SQL> SELECT empno,ename,hiredate,LAST_DAY(hiredate) l_last,
  2                              LAST_DAY(hiredate) - hiredate l_day
  3  FROM emp
  4  ORDER BY LAST_DAY(hiredate) - hiredate DESC;

     EMPNO ENAME                HIREDATE L_LAST        L_DAY
---------- -------------------- -------- -------- ----------
      7698 BLAKE                81/05/01 81/05/31         30
      7902 FORD                 81/12/03 81/12/31         28
      7900 JAMES                81/12/03 81/12/31         28

=============================================================================

[날짜를 대상으로 한 반올림, 버림 ==> ROUND(), TRUNC()]

SQL> SELECT ename,hiredate,ROUND(hiredate,'MONTH') m_round,
  2  TRUNC(hiredate,'MONTH') m_trunc, ROUND(hiredate,'YEAR') y_round,
  3  TRUNC(hiredate,'YEAR') y_trunc
  4  FROM emp
  5  WHERE deptno = 10
  6  ORDER BY hiredate DESC;

ENAME      HIREDATE    M_ROUND     M_TRUNC    Y_ROUND     Y_TRUNC
---------- ----------- ----------- ---------- ----------- -----------
MILLER     23-JAN-82   01-FEB-82   01-JAN-82  01-JAN-82   01-JAN-82
KING       17-NOV-81   01-DEC-81   01-NOV-81  01-JAN-82   01-JAN-81
CLARK      09-JUN-81   01-JUN-81   01-JUN-81  01-JAN-81   01-JAN-81
```

+ 형변환함수 : TO_CHAR(), TO_NUMBER(), TO_DATE()
  - 오라클에서의 형변환은....자동형변환 / 강제형변환-> 형변환 함수를 명시적으로 사용.
```
[TO_CHAR(날짜, '형식문자')]
SQL> ALTER SESSION SET NLS_LANGUAGE='AMERICAN';
SQL> COL t_hiredate FORMAT a30
SQL> COL t_kor FORMAT a20
SQL> SELECT ename,hiredate,TO_CHAR(hiredate, 'fmDD Month YYYY') t_hiredate,
  2                        TO_CHAR(hiredate, 'YYYY"년" MM"월" DD"일"') t_kor
  3  FROM emp
  4  WHERE deptno = 10
  5  ORDER BY hiredate DESC;

ENAME                HIREDATE T_HIREDATE                     T_KOR
-------------------- -------- ------------------------------ --------------------
MILLER               82/01/23 23 January 1982                1982년 01월 23일
KING                 81/11/17 17 November 1981               1981년 11월 17일
CLARK                81/06/09 9 June 1981                    1981년 06월 09일
///////////////////////////////////////////////////////////////////////////////

[TO_CHAR(숫자, '형식지정자') 형식지정자를 통한 문자를 숫자로]
SQL> SELECT empno, ename, job, sal, TO_CHAR(sal,'L099,999') FROM emp;

EMPNO ENAME                JOB                       SAL TO_CHAR(SAL,'L099,999')
----- -------------------- ------------------ ---------- ---------------------------------
7369 SMITH                CLERK                     800         ￦000,800
7499 ALLEN                SALESMAN                 1600         ￦001,600
7521 WARD                 SALESMAN                 1250         ￦001,250
7566 JONES                MANAGER                  2975         ￦002,975
7654 MARTIN               SALESMAN                 1250         ￦001,250
//////////////////////////////////////////////////////////////////////////////

[TO_NUMBER('문자') 문자를 숫자로 바꿔줌]
SQL> SELECT TO_NUMBER('1234') FROM DUAL;

TO_NUMBER('1234')
-----------------
             1234


<문제풀이 27>
SQL> SELECT ename, job, TO_CHAR(hiredate, 'RR/MM/DD') t_hire FROM emp WHERE hiredate=TO_DATE('19810222','RR/MM/DD');

ENAME                JOB                T_HIRE
-------------------- ------------------ ----------------
WARD                 SALESMAN           81/02/22
```

+ 기타함수
  - NVL(), DECODE(), CASE WHEN
```
[DECODE() 조건적 조회 -> CASE/IF]
SQL> SELECT empno, ename, job, sal, DECODE(job, 'ANALYST', sal*1.1, 'CLERK', sal*1.15, 'MANAGER', sal*1.2) d_sal FROM emp ORDER BY sal DESC;

     EMPNO ENAME                JOB                       SAL      D_SAL
---------- -------------------- ------------------ ---------- ----------
      7839 KING                 PRESIDENT                5000
      7902 FORD                 ANALYST                  3000       3300
      7788 SCOTT                ANALYST                  3000       3300
      7566 JONES                MANAGER                  2975       3570
      7698 BLAKE                MANAGER                  2850       3420
      7782 CLARK                MANAGER                  2450       2940

[CASE WHEN()도 있다!!!]
SQL> SELECT empno, ename, deptno,
      CASE WHEN deptno=10 THEN 'ACCOUNTING'
           WHEN deptno=20 THEN 'RESEAECH'
           WHEN deptno=30 THEN 'SALES'
           WHEN deptno=40 THEN 'OPERATIONS'
      END
   FROM emp;

     EMPNO ENAME                    DEPTNO CASEWHENDEPTNO=10THE
---------- -------------------- ---------- --------------------
      7369 SMITH                        20 RESEAECH
      7499 ALLEN                        30 SALES
      7521 WARD                         30 SALES

///////////////////////////////////////////////////////////////////////
<문제풀이 28>
SQL> COL t_rpad format a20
SQL> COL r_r format a20
SQL> SELECT deptno,dname,RPAD(dname,20,'*') t_rpad,
  2                      RPAD(RTRIM(dname),20,'*') r_r,loc
  3  FROM dept;

DEPTNO DNAME          T_RPAD               R_R                  LOC
------ -------------- -------------------- -------------------- --------
    10 ACCOUNTING     ACCOUNTING********** ACCOUNTING********** NEW YORK
    20 RESEARCH       RESEARCH  ********** RESEARCH************ DALLAS
    30 SALES          SALES*************** SALES*************** CHICAGO
    40 OPERATIONS     OPERATIONS********** OPERATIONS********** BOSTON


SQL> SELECT deptno, dname || '|', loc FROM dept;

DEPTNO DNAME||'|'       LOC
------ ---------------- -----------
    10 ACCOUNTING|      NEW YORK
    20 RESEARCH  |      DALLAS
    30 SALES     |      CHICAGO
    40 OPERATIONS|      BOSTON
```

#### 3. 그룹함수

+ GROUP함수
  - 전체행이나 GROUP BY절에 의한 그룹에 적용하여 1개의 결과를 리턴하는 함수
  - COUNT(), SUM(), AVG(), MAX(), VARIANCE(), STDDEV()
```
[그룹 함수 사용]
SELECT		group_function(column) [,group_function(column), . . .]
    FROM		table_name
    [WHERE   	condition]
    [ORDER BY	column];

///////////////////////////////////////////
SQL> SELECT AVG(sal) FROM emp;

  AVG(SAL)
----------
2073.21429
///////////////////////////////////////////
SQL> SELECT SUM(sal) FROM emp;

  SUM(SAL)
----------
     29025
//////////////////////////////////////////
SQL> SELECT SUM(sal)/COUNT(*) FROM emp;

SUM(SAL)/COUNT(*)
-----------------
       2073.21429
/////////////////////////////////////////
SQL> SELECT MAX(sal) FROM emp;

  MAX(SAL)
----------
      5000
/////////////////////////////////////////
SQL> SELECT MIN(sal) FROM emp;

  MIN(SAL)
----------
       800
//////////////////////////////////////////
<문제풀이 1>
SQL> SELECT AVG(sal), MAX(sal), MIN(sal), SUM(sal) FROM emp WHERE job LIKE 'SAL%';

  AVG(SAL)   MAX(SAL)   MIN(SAL)   SUM(SAL)
---------- ---------- ---------- ----------
      1400       1600       1250       5600
///////////////////////////////////////////
<문제풀이 2>
SQL> SELECT MIN(ename), MAX(ename), MIN(hiredate), MAX(hiredate), MIN(sal), MAX(sal) FROM emp;

MIN(ENAME)           MAX(ENAME)           MIN(HIRE MAX(HIRE   MIN(SAL)   MAX(SAL)
-------------------- -------------------- -------- -------- ---------- ----------
ADAMS                WARD                 80/12/17 87/05/23        800       5000
//////////////////////////////////////////
COUNT() : 갯수 구하기
COUNT(*) : 전체 갯수 구하기
COUNT(컬럼명) : NULL값이 아닌 ROW의 갯수

<문제풀이 3>
SELECT COUNT(*) c_inwon, COUNT(comm) c_comm, AVG(comm) a_comm, AVG(NVL(comm,0)) n_comm, COUNT(deptno) c_dept, COUNT(DISTINCT deptno) c_dis FROM emp;
```

+ SELECT문의 COLUMN을 기술할때 그룹함수를 함께 사용할 수 없다.
```
SQL> SELECT COUNT(*), MAX(ename) FROM emp;

  COUNT(*) MAX(ENAME)
---------- --------------------
        14 WARD

SQL> SELECT ename, MAX(ename) FROM emp;
SELECT ename, MAX(ename) FROM emp
       *
ERROR at line 1:
ORA-00937: not a single-group group function
```

+ 단, GROUP BY절에서 사용한 컬럼명은 사용할 수 있다.
```
SQL> SELECT COUNT(*) FROM emp GROUP BY deptno;

  COUNT(*)
----------
         6
         5
         3
///////////////////////////////////////////////////////
SQL> SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;

   DEPTNO   COUNT(*)
---------- ----------
       30          6
       20          5
       10          3
///////////////////////////////////////////////////////
SQL> SELECT job, COUNT(*) FROM emp GROUP BY job;

JOB                  COUNT(*)
------------------ ----------
CLERK                       4
SALESMAN                    4
PRESIDENT                   1
MANAGER                     3
ANALYST                     2
/////////////////////////////////////////////////////////
SQL> SELECT deptno, COUNT(*), AVG(sal) FROM emp GROUP BY deptno HAVING deptno>=20;

    DEPTNO   COUNT(*)   AVG(SAL)
---------- ---------- ----------
        30          6 1566.66667
        20          5       2175


SQL> SELECT deptno, COUNT(*), AVG(sal) FROM emp WHERE deptno>=20 GROUP BY deptno;

    DEPTNO   COUNT(*)   AVG(SAL)
---------- ---------- ----------
        30          6 1566.66667
        20          5       2175
////////////////////////////////////////////////////
SQL> SELECT deptno,COUNT(*),AVG(sal),MIN(sal),MAX(sal),SUM(sal) FROM emp WHERE sal>=1250 GROUP BY deptno ORDER BY deptno;

    DEPTNO   COUNT(*)   AVG(SAL)   MIN(SAL)   MAX(SAL)   SUM(SAL)
---------- ---------- ---------- ---------- ---------- ----------
        10          3 2916.66667       1300       5000       8750
        20          3 2991.66667       2975       3000       8975
        30          5       1690       1250       2850       8450

SQL> SELECT deptno,COUNT(*),AVG(sal),MIN(sal),MAX(sal),SUM(sal) FROM emp WHERE sal>=1250 GROUP BY deptno HAVING deptno>=20 ORDER BY deptno;

    DEPTNO   COUNT(*)   AVG(SAL)   MIN(SAL)   MAX(SAL)   SUM(SAL)
---------- ---------- ---------- ---------- ---------- ----------
        20          3 2991.66667       2975       3000       8975
        30          5       1690       1250       2850       8450
```
+ SELECT문의 작동순서
  - 1) FROM절에 의해 테이블 결정
  - 2) WHERE절이 없으면 모든 ROW, 있으면 조건에 부합하는 ROW
  - 3) 지정한 컬럼 추출
  - 4) GROUP BY에 의해 그룹 설정
  - 5) 그룹함수 적용
  - 6) 최종 결과로 포함시킬 그룹결정(HAVING절)
  - 7) ORDER BY에 의해 출력내용순서 결정

```
SQL> SELECT deptno, job, empno, ename, sal FROM emp ORDER BY deptno, job;

    DEPTNO JOB                     EMPNO ENAME                       SAL
---------- ------------------ ---------- -------------------- ----------
        10 CLERK                    7934 MILLER                     1300
        10 MANAGER                  7782 CLARK                      2450
        10 PRESIDENT                7839 KING                       5000
        20 ANALYST                  7788 SCOTT                      3000
        20 ANALYST                  7902 FORD                       3000
        20 CLERK                    7876 ADAMS                      1100
        20 CLERK                    7369 SMITH                       800
        20 MANAGER                  7566 JONES                      2975
        30 CLERK                    7900 JAMES                       950
        30 MANAGER                  7698 BLAKE                      2850
        30 SALESMAN                 7654 MARTIN                     1250
        30 SALESMAN                 7521 WARD                       1250
        30 SALESMAN                 7499 ALLEN                      1600
        30 SALESMAN                 7844 TURNER                     1500

```
+ 주의점) WHERE절에 GROUP함수를 사용할수 없다. 이유) WHERE절의 조건에 부합하는 ROW를 추출한후 그다음 그룹을 만들기 때문이다.
```
SQL> SELECT deptno, COUNT(*), SUM(sal) FROM emp WHERE COUNT(*)>4 GROUP BY deptno;
SELECT deptno, COUNT(*), SUM(sal) FROM emp WHERE COUNT(*)>4 GROUP BY deptno
                                                 *
ERROR at line 1:
ORA-00934: group function is not allowed here
```
+ 최종 결과물에 포함 여부는 HAVING절에서 기술한다.
```
SQL> SELECT deptno, COUNT(*), SUM(sal) FROM emp GROUP BY deptno HAVING COUNT(*)>=4;

    DEPTNO   COUNT(*)   SUM(SAL)
---------- ---------- ----------
        30          6       9400
        20          5      10875
/////////////////////////////////////////////////////////////////
SQL> SELECT job, AVG(sal), SUM(sal) FROM emp GROUP BY job;

JOB                  AVG(SAL)   SUM(SAL)
------------------ ---------- ----------
CLERK                  1037.5       4150
SALESMAN                 1400       5600
PRESIDENT                5000       5000
MANAGER            2758.33333       8275
ANALYST                  3000       6000

SQL> SELECT job, AVG(sal), SUM(sal) FROM emp GROUP BY job HAVING AVG(sal)>=3000;

JOB                  AVG(SAL)   SUM(SAL)
------------------ ---------- ----------
PRESIDENT                5000       5000
ANALYST                  3000       6000
```
+ 그룹함수를 중첩해서 사용할 수 있다.
```
SQL> SELECT deptno, AVG(sal) FROM emp GROUP BY deptno;

    DEPTNO   AVG(SAL)
---------- ----------
        30 1566.66667
        20       2175
        10 2916.66667

SQL> SELECT MAX(AVG(sal)) FROM emp GROUP BY deptno;

MAX(AVG(SAL))
-------------
   2916.66667
/////////////////////////////////////////////////////////////////////////
SQL> SELECT deptno, empno, ename, sal FROM emp ORDER BY deptno;

    DEPTNO      EMPNO ENAME                       SAL
---------- ---------- -------------------- ----------
        10       7782 CLARK                      2450
        10       7839 KING                       5000
        10       7934 MILLER                     1300
        20       7566 JONES                      2975
        20       7902 FORD                       3000
        20       7876 ADAMS                      1100
        20       7369 SMITH                       800

SQL> SELECT MIN(MIN(sal)) FROM emp GROUP BY deptno;

MIN(MIN(SAL))
-------------
          800
```

##### [오늘의 과제]
+ 4시 이후 5.doc의 연습문제 풀기

+ 1) SELECT <- SQL자료형, 연산자, 함수
+ 2) DML
+ 3) DDL  ================> 가장 간단한 DML, DDL명령을 사용할 것임.
+ 무결성제약조건


#### 4. DML명령
+ Java AWT에서 학습했던 Profile 내용을 테이블로 만들어서 데이터를 추가해 보자

+ DDL(Data Definition Language)명령
  - 테이블을 생성, 구조를 변경, 테이블을 삭제하는 명령
```
[테이블을 만들어보자]
성명 : name  CHAR(15)
나이 : age   NUMBER(3)
전화 : phone CHAR(13)
메일 : email VARCHAR2(20)
생년월일 : birthday DATE

SQL> CREATE TABLE profile(
  2  name CHAR(15),
  3  age NUMBER(3),
  4  phone CHAR(13),
  5  email VARCHAR2(20),
  6  birthday DATE
  7  );

SQL> SELECT * FROM TAB;

TNAME                                                        TABTYPE         CLUSTERID
------------------------------------------------------------ -------------- ----------
BONUS                                                        TABLE
DEPT                                                         TABLE
EMP                                                          TABLE
PROFILE                                                      TABLE
SALGRADE                                                     TABLE

SQL> DESC profile
Name                             Null?    Type
------------------------------- -------- --------------------------------------------
NAME                                      CHAR(15)
AGE                                       NUMBER(3)
PHONE                                     CHAR(13)
EMAIL                                     VARCHAR2(20)
BIRTHDAY                                  DATE

- 테이블에 데이터를 추가, 수정, 삭제하는 명령
- DML(Data Manuflation Language) 명령
  - 추가 : INSERT INTO
  - 수정 : UPDATE SET
  - 삭제 : DELETE FROM

SQL> INSERT INTO profile VALUES('김동민',27,'010-7245-1269','kdm9214@naver.com','');
SQL> INSERT INTO profile(name,age,phone) VALUES('서영곤',25,'010-8561-2763');

SQL> SELECT * FROM profile;                                                );

NAME           AGE PHONE              EMAIL                            BIRTHDAY
------------ ----- ------------------ -------------------------------- --------
김동민          27 010-7245-1269       kdm9214@naver.com                94/09/04
빈태욱          25 010-8441-2782
서영곤          25 010-8561-2763
///////////////////////////////////////////////////////////////////////

- 기존 데이터값을 수정
SQL> UPDATE profile SET email='qlsxodnr1@naver.com' WHERE name='빈태욱';
SQL> SELECT * FROM profile;
NAME           AGE PHONE              EMAIL                            BIRTHDAY
------------ ----- ------------------ -------------------------------- --------
김동민          27 010-7245-1269       kdm9214@naver.com                94/09/04
빈태욱          25 010-8441-2782       qlsxodnr@naver.com
서영곤          25 010-8561-2763

//////////////////////////////////////
SQL> UPDATE profile SET email='tjdudrhs26@naver.com', birthday=TO_DATE('19941201','YYYYMMDD') WHERE name='서영곤';

SQL> SELECT * FROM profile;
NAME           AGE PHONE              EMAIL                            BIRTHDAY
------------ ----- ------------------ -------------------------------- --------
김동민          27 010-7245-1269       kdm9214@naver.com                94/09/04
빈태욱          25 010-8441-2782       qlsxodnr@naver.com
서영곤          25 010-8561-2763       tjdudrhs26@naver.com             94/12/01

///////////////////////////////////////////////////////////////////////

- 기존 데이터(row)를 삭제하자
SQL> DELETE FROM profile WHERE name='김동민';
SQL> DELETE FROM profile WHERE SUBSTR(name,1,1)='김';

///////////////////////////////////////////////////////////////////

- 테이블 구조변경
SQL> ALTER TABLE profile MODIFY(email VARCHAR2(30));
Table altered.

SQL> UPDATE profile SET email='tjdydrhs2661@naver.com' WHERE name='서영곤';
1 row updated.

SQL> SELECT * FROM profile;                                              ';
NAME           AGE PHONE              EMAIL                            BIRTHDAY
------------ ----- ------------------ -------------------------------- --------
김동민          27 010-7245-1269       kdm9214@naver.com                94/09/04
빈태욱          25 010-8441-2782       qlsxodnr@naver.com
서영곤          25 010-8561-2763       tjdudrhs2661@naver.com           94/12/01

//////////////////////////////////////////////////////////////////

- 테이블 삭제 (DROP TABLE profile==> profile테이블이 삭제되고 데이타도 사라진다.)
SQL> DROP TABLE profile;
```

#### 5. 실습
#### 6. Summary / Close

-----------------------------------------------------------

### [2019-04-29]

#### 1. Review
+ DML명령 - 데이터 조작/운용
  - 추가 INSERT INTO
  - 수행 UPDATE SET
  - 삭제 DELETE FROM

+ DDL명령 - 데이터 제반구조
  - 생성 CREATE TABLE
  - 변경 ALTER TABLE, ADD MODIFY
  - 삭제 DROP TABLE

#### 2. 무결성 제약조건(constraint)
+ 관계형 모델의 3대 핵심개념
  - 1) 저장구조(row, column ==> 2차원 테이블 구조)
  - 2) 연산자(조건에 맞는 row추출, column추출, 다른테이블과 연결....)
  - 3) 제약조건(운용되는 데이터에 결함이 없어야 한다. ==> 무결성 제약조건)

+ 무결성 제약조건(constraint) : 데이터를 추가, 수정, 삭제하는 과정에서 무결성을 유지할수 있도록 제약을 주는 것(Table구조에.... TABLE생성시 지정).

+ 제약조건이 없는 테이블
```
SQL> CREATE TABLE emp01(empno NUMBER(4), ename VARCHAR2(10), job VARCHAR2(9), deptno NUMBER(2));

Table created.

SQL> DESC emp01
Name                     Null?    Type
------------------------ -------- --------------------------------------------
EMPNO                             NUMBER(4)
ENAME                             VARCHAR2(10)
 JOB                               VARCHAR2(9)
 DEPTNO                            NUMBER(2)

```

+ emp01테이블에 사원정보를 추가
```
SQL> INSERT INTO emp01 VALUES(NULL,NULL,'SALESMAN',30);

1 row created.

SQL> SELECT * FROM emp01;

     EMPNO ENAME                JOB                    DEPTNO
---------- -------------------- ------------------ ----------
                                SALESMAN                   30
+ 현재 추가된 row는 아무 의미도 없는 데이터이다.
+ 성명이 없고, 다른 다양한 검색에서 식별할 수 있는 방법이 없다.
+ 반드시 사원번호와 성명은 추가시 지정되어야 한다. 없으면 안됨.
+ 위 기능 정상작동? ==> 테이블생성시 empno, ename 컬럼의 제약조건으로 NOT NULL제약조건 지정.
```

+ 제약조건이 있는 emp02테이블 생성
```
SQL> CREATE TABLE emp02(empno NUMBER(4) NOT NULL, ename VARCHAR2(10) NOT NULL, job VARCHAR2(9), deptno NUMBER(2));

Table created.

SQL> DESC emp02
 Name                     Null?    Type
 ------------------------ -------- --------------------------------------------
 EMPNO                    NOT NULL NUMBER(4)
 ENAME                    NOT NULL VARCHAR2(10)
 JOB                               VARCHAR2(9)
 DEPTNO                            NUMBER(2)


+ DESC 명령은 제약조건 중 NOT NULL(NULL을 허용하지 않는다. 즉, 반드시 값이 있어야 한다)을 보여준다.
```

+ 제약조건이 있는 테이블에 사원정보 추가
```
SQL> INSERT INTO emp02 VALUES(8000,'홍길동','영업',30);

1 row created.

SQL> SELECT * FROM emp02;

     EMPNO ENAME                JOB                    DEPTNO
---------- -------------------- ------------------ ----------
      8000 홍길동               영업                       30

SQL> INSERT INTO emp02 VALUES(NULL,'장길산','기획',10);
INSERT INTO emp02 VALUES(NULL,'장길산','기획',10)
                         *
ERROR at line 1:
ORA-01400: cannot insert NULL into ("SCOTT"."EMP02"."EMPNO")

+ null이 허용되지 않기때문에 오류
+ empno, ename은 데이터 추가시 반드시 입력되어야 한다. NULL을 허용하지X.
```

+ Oracle에서 지원하는 무결성 제약조건의 종류
  - 1) 개체무결성 : 모든 row는 다른 row와 식별될수 있는 특성이 있어야 한다.
    + 기본키(Primary Key : PK)
  - 2) 참조무결성 : 관계를 가진 테이블간에 결함이 없어야 한다.
    + 참조키(외래키_Foreign Key : FK)
  - 3) 영역무결성 : 특정 컬럼 하나,하나는 자기 스스로의 역할이 있다. 그 역할에 부합하는 값만 가져야 한다.
    + 1. NOT NULL : 추가시 반드시 값이 있어야 한다.
    + 2. UNIQUE : 추가되는 값은 다른 row의 값과 중복되지 않아야 한다.
    + 3. CHECK : 지정된 값만 가질수 있다.
    + 4. 그외는 모두 사용자가 데이터운용시 해결(JDBC프로그램에서 Logic으로 처리).

+ 무결성 제약조건을 기술하는 방법
  - 테이블 생성시 지정
    + 1. 컬럼 level지정
      - CREATE TABLE 테이블명(컬럼명 자료형 제약조건, ....);
    + 2. 테이블 level지정
      - CREATE TABLE 테이블명(컬럼명 자료형,... 제약조건을 별도로 지정);

+ 제약조건에 대한 실습
+ NOT NULL : row추가시 반드시 값이 있어야 한다. NULL은 안된다.

+ 제약조건이 위배되면 에러가 발생한다. 이때 위배된 제약조건 몇칭은 특별히 생성시 지정않았다면 자동으로 부여되어 나타난다.    
```
SQL> CREATE TABLE emp021(empno NUMBER(4) CONSTRAINT emp02_empno_nn NOT NULL , ename VARCHAR2(10) CONSTRAINT emp02_ename_nn NOT NULL, job VARCHAR2(9), deptno NUMBER(2));

Table created.

SQL> DESC emp021
Name                                                              Null?    Type
----------------------------------------------------------------- -------- --------------------------------------------
EMPNO                                                             NOT NULL NUMBER(4)
ENAME                                                             NOT NULL VARCHAR2(10)
JOB                                                                        VARCHAR2(9)
DEPTNO                                                                     NUMBER(2)

INSERT INTO emp02 VALUES(NULL,'고주몽','기획',10);

+ NOT NULL제약조건만 에러발생시 제약조건명을 보여주지 않았다.
+ 나머지 제약조건(PK,FK,Unigue, Check)은 에러발생시 제약조건명을 보여준다.
```

+ UNIQUE 제약조건 : 이미 추가된 row의 값과 동일한 값은 입력할수 없다. 유일한 값만 추가.
  - 대부분의 경우 설계속성에 지정한다.
  - 원래 존재하는 값이 아니라 관리하기 위해 특별하게 지정한 값
  - 현재 emp테이블에서는 사원번호(empno)가 해당되는 특성이다. 동일한 사원번호를 가진 사원은 존재X.
```
SQL> CREATE TABLE emp03(empno NUMBER(4) UNIQUE, ename VARCHAR2(10) NOT NULL, job VARCHAR2(9), deptno NUMBER(2));

Table created.

SQL> INSERT INTO emp03 VALUES(8000,'홍길동','영업',30);

1 row created.

SQL> SELECT * FROM emp03;

     EMPNO ENAME                JOB                    DEPTNO
---------- -------------------- ------------------ ----------
      8000 홍길동               영업                       30
```

+ 사원번호가 같은 row를 추가해보자(에러발생)
```
SQL> INSERT INTO emp03 VALUES(8000,'장길산','기획',10);
INSERT INTO emp03 VALUES(8000,'장길산','기획',10)
*
ERROR at line 1:
ORA-00001: unique constraint (SCOTT.SYS_C007007) violated
==================================================================
+ 주의점) : UNIQUE 제약조건은 NULL은 허용한다.
SQL> INSERT INTO emp03 VALUES(NULL,'이순신','대표',20);;;););;;

1 row created.

SQL> INSERT INTO emp03 VALUES(8002,'장길산','영업',30);

1 row created.

SQL> SELECT * FROM emp03;                          20);

     EMPNO ENAME                JOB                    DEPTNO
---------- -------------------- ------------------ ----------
      8000 홍길동               영업                       30
      8001 장길산               기획                       10
           이순신               대표                       20
      8002 장길산               영업                       30
```
+ NOT NULL은 동일한 값이든 뭐든 무조건 값만 입력되면 된다.
+ UNIQUE는 동일한 값은 입력될수 없다. 단, NULL은 허용한다.
  - 그래서 두가지 특성... 즉, NOT NULL과 UNIQUE를 한꺼번에 가지는 대표속성을 설정하고 이를 기본키라고 부른다.(Primary Key)
```
SQL> CREATE TABLE emp04(empno NUMBER(4) PRIMARY KEY, ename VARCHAR2(10) NOT NULL, job VARCHAR2(9), deptno NUMBER(2));

Table created.

SQL> DESC emp04
 Name                  Null?    Type
 --------------------- -------- --------------------------------------------
 EMPNO                 NOT NULL NUMBER(4)
 ENAME                 NOT NULL VARCHAR2(10)
 JOB                            VARCHAR2(9)
 DEPTNO                         NUMBER(2)
```
+ emp04테이블에서 empno는 반드시 값을 입력해야 하고, 동일한 값은 입력할수 없는 기본키 특성을 가졌다.
```
SQL> INSERT INTO emp04 VALUES(8000,'홍길동','판매',30);

1 row created.

SQL> INSERT INTO emp04 VALUES(8001,'장길산','기획',10);

1 row created.

SQL> INSERT INTO emp04 VALUES(NULL,'이순신','연구',20);
INSERT INTO emp04 VALUES(NULL,'이순신','연구',20)
                         *
ERROR at line 1:
ORA-01400: cannot insert NULL into ("SCOTT"."EMP04"."EMPNO")


SQL> INSERT INTO emp04 VALUES(8002,'이순신','연구',20);

1 row created.

SQL> SELECT * FROM emp04;

     EMPNO ENAME                JOB                    DEPTNO
---------- -------------------- ------------------ ----------
      8000 홍길동               판매                       30
      8001 장길산               기획                       10
      8002 이순신               연구                       20
```

+ 테이블 생성시 지정된 제약조건은 별도의 테이블에 저장된다.
+ 실제사용시 테이블에는 데이터 즉, row만 저장된다.
  - 해당 테이블의 정보 즉, 테이블명, 컬럼명, 자료형, 제약조건 등은 자료사전(Data Dictionary)이라는 별도의 테이블에 정보가 저장된다.
  - SELECT * FROM TAB; 명령으로는 자료사전을 확인할수 없다.

+ 자료사전도 테이블이다.
  - 테이블명 ==> user_xxx로 정해져있다.
  - 1) 테이블 정보 자료사전 ==> user_tables
  - 2) 제약조건 정보 자료사전 ==> user_constraints
  - 3) 제약조건 컬럼 자료사전 ==> user_cons_columns
```
SQL> SELECT table_name, status FROM user_tables;

TABLE_NAME                                                   STATUS
------------------------------------------------------------ ----------------
DEPT                                                         VALID
EMP                                                          VALID
BONUS                                                        VALID
SALGRADE                                                     VALID
PROFILE                                                      VALID
EMP01                                                        VALID
EMP02                                                        VALID
EMP021                                                       VALID
EMP03                                                        VALID
EMP04                                                        VALID

10 rows selected.
```

+ 제약조건을 지정하면서 CONSTRAINT 키워드를 사용하여 [CONSTRAINT 제약조건명 제약조건종류(NOT NULL, UNIQUE, PRIMARY KEY)] 제약조건명을 지정할 수 있다.
+ CONSTRAINT 키워드를 사용하지 않았다면 제약조건명을 '오라클 시스템'에서 자동으로 부여했다.
  - SYS_CNNNN

+ 내부적으로 저장될때 제약조건 종류는
  - NOT NULL ==> C
  - UNIQUE ==> U
  - PRIMARY KEY ==> P
  - FOREIGN KEY ==> R
  - CHECK ==> C
```
SQL> SELECT table_name, constraint_name, constraint_type FROM user_constraints;

TABLE_NAME
------------------------------------------------------------
CONSTRAINT_NAME                                              CO
------------------------------------------------------------ --
EMP03
SYS_C007007                                                  U

EMP04
SYS_C007008                                                  C

EMP04
SYS_C007009                                                  P
```
+ 테이블 생성시 제약조건을 기술하면 그 내부정보는 자료사전에 저장되고, 이후 DML명령사용시 그 내용이 참조되어 데이터 조작(추가/수정/삭제)이 이루어지고... 이때 제약조건 위배여부를 판단한다.

+ 남은 제약조건
  - 두개 이상의 테이블을 연결하는 제약조건
  - 참조키 제약조건(Forein Key : FK)

+ PK : 한개의 테이블에서 각 row를 식별하는 특성
+ FK : 최소한의 중복을 허용하면서 통합된 데이터를 관리하기 위해 특정항목의 상세데이터는 다른 테이블에 분리하여 관리한다. 이 두테이블 정보를 연결시키는 것이 FK의 역할이다.

+ emp 테이블의 deptno는 부서코드만 가지고 있다. 상세부서정보는 deptno

+ SCOTT사원이 근무하는 부서명을 알고 싶다면?
```
SQL> SELECT * FROM emp WHERE ename='SCOTT';

     EMPNO ENAME                JOB                       MGR HIREDATE        SAL       COMM     DEPTNO
---------- -------------------- ------------------ ---------- -------- ---------- ---------- ----------
      7788 SCOTT                ANALYST                  7566 87/04/19       3000                    20

SQL> SELECT * FROM dept WHERE deptno=20;

    DEPTNO DNAME                        LOC
---------- ---------------------------- --------------------------
        20 RESEARCH                     DALLAS

+ 따로 검색할 수 밖에 없는 불편함...
```

+ 참조키로 연결되어진 여러테이블을 연결하여 정보를 얻는 방법
  - 1) Sub Query
  - 2) Join


+ 현재의 emp테이블과 dept테이블은, emp테이블의 deptno 컬럼에 의해 연결된다.
  - 이때 상세 데이터를 가진 테이블을 부모테이블이라고 부른다.
  - emp테이블이 자식테이블이고 dept테이블이 부모테이블이다.
  - 두 테이블을 연결시킬때, 자식테이블에 FK를 설정한다.
```
CREATE TABLE emp (컬럼정보들....  deptno NUMBER(2) REFERENCES dept(deptno));
```
  - FK를 설정할때는 반드시 부모테이블부터 먼저 만들어져 있어야 한다.
  - 이미 dept테이블이 만들어져 있다.(부모테이블)
  - 자식테이블 emp05를 만들면서 FK를 설정하자.
```
SQL> CREATE TABLE emp05(empno NUMBER(4) PRIMARY KEY, ename VARCHAR(10) NOT NULL, job VARCHAR2(9), dno NUMBER(2) REFERENCES dept(deptno));

Table created.
```

+ emp05테이블에 row를 추가할때 반드시 dno는 dept테이블에 존재하는 deptno만 가질수 있다.
```
SQL> INSERT INTO emp05 VALUES(8000,'홍길동','판매',30);

1 row created.

SQL> INSERT INTO emp05 VALUES(8001,'장길산','기획',40);

1 row created.

SQL> INSERT INTO emp05 VALUES(8002,'이순신','연구',50);
INSERT INTO emp05 VALUES(8002,'이순신','연구',50)
*
ERROR at line 1:
ORA-02291: integrity constraint (SCOTT.SYS_C007012) violated - parent key not found
```
+ 자식테이블(detail)에 row를 추가할때는 반드시 부모테이블(master)에 존재하는 값만 사용할수 있다.
+ 부모테이블의 row를 삭제할때 그 값을 참조하는 자식테이블의 row가 존재하면 삭제할 수 없다.
```
SQL> SELECT * FROM dept;

    DEPTNO DNAME                        LOC
---------- ---------------------------- --------------------------
        10 ACCOUNTING                   NEW YORK
        20 RESEARCH                     DALLAS
        30 SALES                        CHICAGO
        40 OPERATIONS                   BOSTON

SQL> DELETE FROM dept WHERE deptno=10;
DELETE FROM dept WHERE deptno=10
*
ERROR at line 1:
ORA-02292: integrity constraint (SCOTT.EMP_DEPTNO_FK) violated - child record found
```

+ 40부서를 삭제하려고 해도 삭제가 안된다. 이유)emp05테이블도 자식테이블이다.
```
SQL> SELECT * FROM emp05;

     EMPNO ENAME                JOB                       DNO
---------- -------------------- ------------------ ----------
      8000 홍길동               판매                       30
      8001 장길산               기획                       40

SQL> DELETE FROM dept WHERE deptno=40;
DELETE FROM dept WHERE deptno=40
*
ERROR at line 1:
ORA-02292: integrity constraint (SCOTT.SYS_C007012) violated - child record found
```

+ CHECK 제약조건(특정컬럼이 지정한 값만 가질수 있다.)
```
SQL> CREATE TABLE emp06 (empno NUMBER(4) PRIMARY KEY, ename VARCHAR2(10) NOT NULL, gender CHAR(1) CHECK(gender IN('M','F')));
Table created.

SQL> INSERT INTO emp06 VALUES(8000,'홍길동','F');
1 row created.

SQL> SELECT * FROM emp06;
EMPNO ENAME GE
----- ----- --
8000 홍길동  F                                             

SQL> INSERT INTO emp06 VALUES(8001,'이순신','M');
1 row created.

SQL> SELECT * FROM emp06;
EMPNO ENAME                GE
----- -------------------- --                                                            
8000 홍길동           F
8001 이순신           M

SQL> INSERT INTO emp06 VALUES(8002,'고주몽','A');
INSERT INTO emp06 VALUES(8002,'고주몽','A')
*
ERROR at line 1:
ORA-02290: check constraint (SCOTT.SYS_C007014) violated
```

+ 지금까지 제약조건을 지정한 방법이 가장 단순한 방법이다.
```sql
-- 가장 단순한 제약조건 기술 방법

CREATE TABLE emp07(
	empno NUMBER(4) PRIMARY KEY,
	ename VARCHAR2(10) NOT NULL,
	phone VARCHAR2(13) UNIQUE,
	gender CHAR(1) CHECK(gender IN('M','m','F','f')),
	deptno NUMBER(2) REFERENCES dept(deptno)
);


-- 제약조건명을 지정할 수 있다.
-- 제약조건명을 테이블명_걸럼명_제약조건종류약어와 같은 형태를 권장한다.
-- 제약조건 약어(PRIMARY KEY ==> PK, NOT NULL ==> NN, UNIQUE ==> UK, CHECK ==> CK, REFERENCES KEY ==> FK
CREATE TABLE emp07(
	empno NUMBER(4) CONSTRAINT emp07_empno_pk PRIMARY KEY,
	ename VARCHAR2(10) CONSTRAINT emp07_ename_nn NOT NULL,
	phone VARCHAR2(13) CONSTRAINT emp07_phone_uk UNIQUE,
	gender CHAR(1) CONSTRAINT emp07_gender_ck CHECK(gender IN('M','m','F','f')),
	deptno NUMBER(2) CONSTRAINT emp07_deptno_fk REFERENCES dept(deptno)
);

-- 위와 같이 제약조건을 기술할때 제약조건명도 기술하면,
-- 제약조건이 위배되었을때 제약조건명이 에러메세지와 함께 나타난다.
-- 우리는 이 정보를 확인하여 제약조건에 대한 정보를 얻을 수 있다.
```

+ 위의 제약조건 기술방법을 컬럼레벨 제약조건 기술이라고 한다.
+ 동일한 내용을 테이블레벨 제약조건으로 기술할 수도 있다.

+ 필요에 따라서 제약조건을 테이블레벨형태로 지정할수 있다.
  - 1) 복합키 사용시
  - 2) 제약조건 변경시..... 이때는 테이블레벨 형태로 지정해야 한다.

+ PK, UK, FK를 복합키로 지정할 수 있다.
  - 두개이상의 컬럼을 합쳐서 하나의 KEY로 지정하는 방법.
```sql
-- 간단한 우편번호 테이블
-- 현재는 5자리의 통합된 우편번호 코드를 사용한다.
-- 이전에는 3-3자리로 구성된 우편번호 체계를 사용했다.
CREATE TABLE post(
	post1 CHAR(3),
	post2 CHAR(3),
	sido  VARCHAR2(30) NOT NULL,
	gugun VARCHAR2(30) NOT NULL,
	dong  VARCHAR2(30) NOT NULL,
	PRIMARY KEY(post1,post2)
);

-- 우편번호 2개를 합쳐서 PRIMARY KEY로 설정
```
+ 이와같이 복합키 지정은 테이블레벨 지정방법만 가능하다!!!!!!!!
+ 또한

+ 테이블 레벨 제약조건 기술방법 실습
```sql
-- 테이블레벨 제약조건(NOT NULL 컬럼레벨 제약조건만 가능)
CREATE TABLE emp08(
  empno NUMBER(4),
  ename VARCHAR2(10) NOT NULL,
  phone VARCHAR2(13),
  gender CHAR(1),
  deptno NUMBER(2),
PRIMARY KEY(empno),
UNIQUE(phone),
CHECK(gender IN('M','m','F','f')),
FOREIGN KEY(deptno) REFERENCES dept(deptno)
);


-- 테이블 레벨 제약조건에 제약조건명을 지정할때 CONSTRAINT 키워드를 사용한다.
CREATE TABLE emp08(
empno NUMBER(4),
ename VARCHAR2(10) NOT NULL,
phone VARCHAR2(13),
gender CHAR(1),
deptno NUMBER(2),
CONSTRAINT emp08_empno_pk PRIMARY KEY(empno),
CONSTRAINT emp08_phone_uk UNIQUE(phone),
CONSTRAINT emp08_gender_ck CHECK(gender IN('M','m','F','f')),
CONSTRAINT emp08_deptno_fk FOREIGN KEY(deptno) REFERENCES dept(deptno)
);
```

+ 여기까지의 내용에서 컬럼레벨과 동일하게 테이블 레벨로 제약조건을 기술할 수 있다.
  - 주의점) NOT NULL은 반드시 컬럼레벨로만 지정할 수 있다.
  - 기능상에는 차이가 없으나....'복합키 지정'이나, '제약조건의 수정'은 컬럼레벨에서 지정할 수 없다.
===========================================================================
+ 지금까지는 교재를 참조하지 않고 제약조건을 설명함. 이제 실습 교재를 참고하여 실습해보자.

#### 3. DDL명령
+ DDL(Data Definition Language)
  - 테이블 생성, 구조변경, 테이블 삭제+무결성 제약조건

+ 테이블을 생성하면 해당 정보는 자료사전(Data Dictionary)에 저장된다. 또한 테이블 생성, 구조변경, 테이블 삭제 등의 명령(DDL)은 적접 데이터베이스에 영향을 미치고 그 내용은 복구할 수 없다.
  - 단, 새로운 데이터 추가, 수정, 삭제 명령(DML)은 임시작업영역에 영향을 미치고, 직접 데이터베이스에는 반영되지 않음으로 복구할 수 있다.
  - 참고사항) SQL명령이 동작하는 임시메모리 영역을 '커서'라고 부른다.
  - 결론적....DDL명령은 커서가 아닌 데이터베이스에 직접 영향을 주고, DML명령은 커서에 영향을 준다.

+ 테이블 생성 CREATE TABLE
```
CREATE  TABLE		[schema.]table_name
        ( column	datatype	  [DEFAULT  expr] [column_constraint],
        . . . . . . . .
         [table_constraint]);

schema			테이블의 소유자
table_name		생성하고자 하는 테이블 이름. 사용자 단위로 유일한 이름
column			테이블에서 사용하는 열 이름. 테이블 단위로 유일한 이름
datatype		열의 자료형
DEFAULT expr		INSERT문장에서 값을 생략시 기본적으로 입력되는 값을 명시
column_constraint 	열정의 부분에서 무결성 제약 조건을 기술
table_constraint	테이블 정의 부분에서 무결성 제약 조건을 기술


CREATE TABLE 테이블명(컬럼명 자료형 [제약조건],
  .....
  [제약조건]
);


[테이블명이나 컬럼명, 제약조건명은 규칙]
문자로 시작, 30글자이내, 영문자, 숫자, 특수기호(_,&,#)

[SQL에서의 자료형]
- VARCHAR2(n) : 가변길이 문자열(4000byte)
- CHAR(n) : 고정길이 문자열(2000byte)
- NUMBER, NUMBER(n), NUMBER(p,s) : 가변정수/정수/실수 (숫자를 나타냄)
- DATE : 내부적으로 7byte사용, 날짜와 시간정보 기술.
===================================================
- LONG : 가변길이 문자(2Gbyte)
- CLOB : 단일byte 가변길이 문자데이터 (4Gbyte)
- RAW(n), LONG RAW : 원시 이진 데이터
- BLOB : 가변길이 이진 데이터
- BFILE : 외부 파일 이진 데이터
=================================================================

[제약조건은 아니지만 DEFAULT키워드를 알아보자]

+ row(행) 추가시 명시적으로 NULL 입력하면 NULL이 추가된다.
+ 단, 암시적으로 입력하지 않으면... DEFAULT값이 적용된다.

SQL> CREATE TABLE dept01(
  2  deptno NUMBER(2) PRIMARY KEY,
  3  dname VARCHAR2(30) NOT NULL,
  4  loc VARCHAR2(20) DEFAULT '전주'
  5  );
Table created.

SQL> INSERT INTO dept01(deptno, dname) VALUES(30,'총무부');
1 row created.

SQL> SELECT * FROM dept01;                               );
    DEPTNO DNAME                    LOC
---------- ------------------------ ----------------------------------------
        10 영업부                   대구
        20 기획부
        30 총무부                   전주
```

+ NOT NULL은 테이블레벨 안된다!!!
```
-- 컬럼레벨 NOT NULL 제약조건 지정(ok)
CREATE TABLE NN_TAB1 (
	DEPTNO             NUMBER(2) CONSTRAINT UNI_TAB_DEPTNO_NN NOT NULL,
	DNAME              CHAR(14),
	LOC                CHAR(13)
);

-- 아래의 테이블레벨 NOT NULL 제약조건 지정은 error발생
CREATE TABLE NN_TAB2 (
	DEPTNO             NUMBER(2),
	DNAME              CHAR(14),
	LOC                CHAR(13),
-- NOT NULL은 테이블레벨로 지정할 수 없다.
CONSTRAINT UNI_TAB_DEPTNO_NN NOT NULL (DEPTNO)
);
```


+ CHECK(CK)
```
-- deptno컬럼에 값을 추가할때 반드시 10,20,30,40,50만 가질수 있도록
-- 컬럼레벨 제약조건 CHECK 지정
CREATE TABLE CK_TAB1 (
	DEPTNO  NUMBER(2) CONSTRAINT UNI_TAB_DEPTNO_CK CHECK (DEPTNO IN (10,20,30,40,50)),
	DNAME   CHAR(14),
	LOC     CHAR(13)
);

-- 테이블레벨로  제약조건 CHECK 지정
CREATE TABLE CK_TAB2 (
	DEPTNO  NUMBER(2),
	DNAME   CHAR(14),
	LOC     CHAR(13),
CONSTRAINT UNI_TAB_DEPTNO_CK CHECK (DEPTNO IN (10,20,30,40,50))
;
```

+ 실습한 내용을 종합하여 아래의 테이블 생성을 실습해 보자.
```sql
--테이블 생성은 자식 테이블부터 한다.
DROP TABLE member;
DROP TABLE post;

-- 테이블 생성은 부모 테이블부터 한다.
-- 우편번호 테이블 만들기(부모 테이블)
CREATE TABLE post(
	post1	CHAR(3),
	post2	CHAR(3),
	addr	VARCHAR2(180) CONSTRAINT post_addr_nn NOT NULL,
-- post1과 post2를 묶어서 pk로 지정
CONSTRAINT post_post12_pk PRIMARY KEY (post1,post2)
);

-- 우편번호 테이블에 연습용 데이터를 5개정도 입력해 보자
INSERT INTO post VALUES('137','070','서울시 서초구 서초동');
INSERT INTO post VALUES('137','071','서울시 서초구 서초2동');
INSERT INTO post VALUES('137','072','서울시 서초구 서초3동');
INSERT INTO post VALUES('063','070','전라북도 전주시 완산구 중노송동');
INSERT INTO post VALUES('063','071','전라북도 전주시 완산구 서노송동');



CREATE TABLE member(
	id	NUMBER(4) CONSTRAINT member_id_pk PRIMARY KEY,
	name	VARCHAR(30) CONSTRAINT member_name_nn NOT NULL,
	sex	CHAR(1) CONSTRAINT member_sex_ck CHECK ( sex IN ('1','2')),
	jumin1	CHAR(6),
	jumin2	CHAR(7),
	tel	VARCHAR2(15),
	post1	CHAR(3),
	post2	CHAR(3),
	addr	VARCHAR2(180),
CONSTRAINT member_jumin12_uk UNIQUE (jumin1,jumin2),
CONSTRAINT member_post12_fk FOREIGN KEY (post1,post2) REFERENCES post (post1,post2)
);


-- 회원정보 테이블에 연습용 데이터 추가
INSERT INTO member VALUES(1000,'홍길동','1','871214','1128136','010-1111-1111','063','070','대송빌라 B동 302호');
INSERT INTO member VALUES(1001,'장길산','2','880512','2128136','010-2222-2222','063','071','125번지');
INSERT INTO member VALUES(1002,'이순신','1','851111','1234124','010-3333-3333','137','070','영광 오피스텔 402호');


-- 데이터베이스에 DML 명령의 모든 내용을 반영시키시오.
COMMIT;
```
```
SQL> SELECT * FROM TAB;
TNAME                      TABTYPE         CLUSTERID
-------------------------- -------------- ----------
BONUS                      TABLE
DEPT                       TABLE
DEPT01                     TABLE
EMP                        TABLE
MEMBER                     TABLE
POST                       TABLE
SALGRADE                   TABLE


SQL> SELECT * FROM POST;
POST1  POST2
------ ------
ADDR
---------------------------------------------------------------------------------
137    070
서울시 서초구 서초동

137    071
서울시 서초구 서초2동

137    072
서울시 서초구 서초3동

063    070
전라북도 전주시 완산구 중노송동

063    071
전라북도 전주시 완산구 서노송동


SQL> SELECT * FROM member;
ID NAME                                                         SE JUMIN1       JUMIN2
-- ------------------------------------------------------------ -- ------------ ---------
TEL                            POST1  POST2
------------------------------ ------ ------
ADDR
------------------------------------------------------------------------------------------------------------------------
      1000 홍길동                                                       1  871214       1128136
010-1111-1111                  063    070
대송빌라 B동 302호

      1001 장길산                                                       2  880512       2128136
010-2222-2222                  063    071
125번지

      1002 이순신                                                       1  851111       1234124
010-3333-3333                  137    070
영광 오피스텔 402호


SQL> SELECT * FROM post WHERE post1='137' AND post2='070';

POST1  POST2
------ ------
ADDR
------------------------------------------------------------------------------------------------------------------------
137    070
서울시 서초구 서초동
```


+ 테이블 구조 변경 ALTER TABLE
+ 테이블 삭제 DROP TABLE


##### [오늘의 과제]
+ 복습, 프로젝트 기획 작업
+ 함수관련 연습문제(4.doc, 5.doc)풀어보기

#### 4. 실습
#### 5. Summary / Close


-----------------------------------------------------------

### [2019-04-30]

#### 1. Review

#### 4. DML명령
#### 4. 실습
#### 5. Summary / Close


-----------------------------------------------------------

### [2019-04-30]

#### 1. Review

#### 4. DML명령
#### 4. 실습
#### 5. Summary / Close
