## 1강. intro
+ 쉡(shell)에서 명령을 내리고 엔터를 치면 곧바로 컴퓨터가 알아듣고 실행하여 결과를 보여주는 "인터렉티브(대화형)모드"
+ 프로그램 초보자도 쉽게 배울수 있는 언어
+ 다른 저수준 언어와의 호환성이 좋은 언어

### 빅데이터?
+ 데이터의 규모에 초점을 맞춘 정의
  - 기존 데이터베이스 관리도구의 데이터 수집, 저장, 관리, 분석 역량을 넘어서는 데이터

+ 업무 수행 방식에 초점을 맞춘 정의
  - 다양한 종류의 대규모 데이터로부터 저렴한 비용으로 가치를 추출하고, 데이터의 빠른 수집, 발굴, 분석을 지원하도록 고안된 차세대 기술 및 아키텍처



### 빅데이터의 3요소
#### 크기(Volume)
+ 일반적으로 수십 테라바이트(terabyte) 혹은 수십 페타바이트(petabyte) 이상이 빅데이터의 범위에 해당.
+ 빅데이터는 기존 파일 시스템에 저장하기 어려울 뿐 아니라 데이터 분석을 위해 사용하는 기존 DW 같은 솔루션에서 소화하기 어려울 정도로 급격하게 데이터의 양이 증가하고 있음.
+ 이러한 문제를 극복하려면 확장 가능한 방식으로 데이터를 저장하고 분석하는 분산 컴퓨팅 방식으 접근해야함.

### 속도(Velocity)
+ 데이터처리를 크게 실시간 처리와 장기적인 접근으로 나눌 수 있음
+ 오늘날 디지털 데이터는 매우 빠른 속도로 생성되기 때문에 데이터의 수집, 저장, 분석 등이 실시간으로 처리돼야 함.
+ 모든 데이터가 실시간 처리만을 요구하는 것은 아님
+ 수집된 대량의 데이터를 다양한 분석 기법과 표현 기술로 분석해야 하는데, 이는 장기적이고 전략적인 차원에서 접근할 필요가 있음
+ 통계학과 전산학에서 사용하는 데이터 마이닝, 기계 학습, 자연어 처리, 패턴 인식 등이 분석기법에 해당함.

### 다양성(Variety)
+ 데이터는 정형화 정도에 따라 정형(Structured), 반정형(Semi-Structured), 비정형(Unstructured)으로 구분.
+ 정형 데이터는 고정된 필드에 저장되는 데이터를 의미하며, 일정한 형식을 갖추고 있음.
+ 반정형 데이터는 고정된 필드로 저장되지는 않지만, XML이나 HTML 같이 메타데이터나 스키마 등을 포함
+ 비정형 데이터는 고정된 필드에 저장되지 않는 데이터를 의미함. 사진, 동영상, 메신저로 주고 받은 대화내용, 스마트폰애 기록되는 위치정보, 통화 내용 등이 이에 해당.
+ 빅데이터는 비정형 데이터도 처리할 수 있어야 함.



### 데이터 분석을 위한 파이썬 프로그램 과정
+ numpy 모듈 - numpy배열객체 생성방법, 난수 생성, 산술계산방법 설명
+ pandas 모듈 - 데이터 가공을 위한 핵심 패키지. Series, DataFrame 자료구조 생성방법 및 관련 기본메소드, 데이터를 읽어오는 방법 및 연산작업
+ matplotlib 모듈 - 데이터 시각화를 위한 패키지. jupyter notebook에서 Integrative한 그래픽을 표현


--------------------------------------------------------------------
## 2강. 분석을 위한 환경설정-1/2

### 아나콘다(ANACONDA)가 뭐죠?
+ 데이터 분석에 필요한 라이브러리를 모아놓은 파이썬 기반의 플랫폼.
+ Conda = mini + Ana
+ https://ananonda.com/download

+ 통합개발도구(ANACONDA)...에디터 + 디버깅 + 실행...등
+ Jupyter Notebook (ipython이 바뀜)
--------------------------------------------------------------------
## 3강. 분석을 위한 환경설정-2/2
+ Spyder Editor 사용법
+ Jupyter Editor 사용

--------------------------------------------------------------------
## 4강. Jupyter Notebook(=ipython) 기본기능
+ GUI방식의 Editor 형식.
+ ```*.ipynb```형식으로 저장.

--------------------------------------------------------------------
## 5강. Jupyter Notebook 매직명령어 설명 및 실습
+ %run mysample.py.... 각각 줄단위로 실행가능.

|:-----------|:-------------------------------------------------------:|
|%cd         | 디렉토리 경로변경시 사용                                |
|%ls         | 파일이나 폴더목록표시                                   |
|%paste      |    클립보드에서 들여쓰기 된 상태로 파이썬 코드 붙여넣기 |
|%magic      |    도움말 출력                                          |
|%quickeref  |        빠른 도움말 표시                                 |
|%hist       |   명령어 히스토리 출력                                  |
|%run        |  Jupyter에서 파이썬 스크립트 실행                       |
|%time       |   명령 실행시간 출력                                    |
--------------------------------------------------------------------
--------------------------------------------------------------------
## 6강. Markdown 기능을 이용하여 웹문서 작성
+ ####, ###, ##, # 사용법
+ pandas를 이용한 서울 지하철 평균 이용객 수 분석 작성.

--------------------------------------------------------------------
## 7강. 모듈 이해하기
+ matplotlib - 시각화
+ pandas - 정제역할
+ 배열, 리스트, 튜플을 다차원 배열로 만들수 있다.

+ Numpy란?
  - 고차원 과학계산 및 데이터 분석을 위한 기본 패키지.
  - 다차원 배열 객체와 이를 다루기위한 다양한 도구를 제공.
  - 여러 형태의 배열계산을 손쉽게 하기위한 라이브러리이고, 이를 이해함으로써 분석에 한층 더 가까워 질 수 있다.
  - 난수(random)을 발생한다.(random보다 빠름)

### 리스트 - 배열과 달리 크기 변경이 가능하고 서로 다른 자료형으로도 데이터 구성이 됨.
### 튜플(tugle) - 요소들 간 순서가 있으며 값이 변하지 않는 리스
--------------------------------------------------------------------
## 8강. 배열 생성하기
+ Numpy의 배열 - ndarray 생성
  - 동일한 자료타입을 갖는 값들이 n개의 차원으로 구성된 형태.

|:------------------|:---------------------------------------------------|
|함수               |기능                                                 |
|array              |리스트, 튜플, 배열과 같은 순차 데이터를 ndarray로 변환  |
|arange             |range함수와 비슷. (ndarray 데이터타입 반환)            |
|zeros              |배열 요소 모두 0으로 초기화한 배열 생성                |
|ones               |배열 요소 모두 1로 초기화한 배열 생성                  |
|empty              |새로운 배열 생성시 값을 초기화 하지 않음               |
|eye / identity     |n행 n열의 단위행렬 생성                               |

+ numpy.array - numpy패키지를 이용하여 array함수로 리스트, 튜플, 배열을 생성한다.
  - ndarray(shape, dtype=float, buffer=None, offset=0, strides=None, order=None)

--------------------------------------------------------------------
## 9강. 정밀계산을 위한 numpy의 배열 생성방법
|:-------------|:----------------------------------------------------|
|함수          |기능                                                  |
|Abs / abs     |절대값 계산                                           |
|sqrt          |제곱근 계산                                           |
|square        |제곱 계산                                             |
|exp           |지수 계산                                             |
|Cell / floor  |소수자릿수 올림/소수자릿수 내림                         |
|rint          |소수자릿수 반올림                                      |
|modf          |원소의 몫과 나머지를 각 배열로 변환                     |
|isnan         |각 원소가 숫자인지 아닌지를 나타내는 불리언 배열 리턴     |

|:----------------|:----------------------------------------------------|
|함수             |기능                                                  |
|Add / subtract   |두 배열의 같은 위치의 요소끼리 덧셈/뺄셈                |
|Multiply         |배열 원소끼리 곱셈                                     |
|divide           |첫번째 배열의 원소에서 두 번째 배열 원소로 나눔          |
|power            |첫번째 배열의 원소에 두 번째 배열의 원소만큼 제곱        |
|maximum          |비교하는 두 원소 중 큰 값                              |
|mod              |첫번째 배열의 원소에서 두 번째 배열 원소로 나눈 나머지   |


### 배열 관련 기본 통계 메소드
|:-------------|:----------------------------------------------------|
|함수          |기능                                                  |
|sum           |배열의 전체 혹은 일부원소의 합 계산                     |
|mean          |산술 평균 계산                                         |
|Std / var     |표준편차 계산/분산 계산                                |
|Min           |최소 값 계산                                          |
|Cumsum        |각 원소의 누적값                                      |
|cumprod       |각 원소의 누적곱                                      |

--------------------------------------------------------------------
## 10강. 기본 연산동작 실습
```python
data[data<0]=0
```

+ 난수생성
  - 파이썬 내장 random모듈보다 더 빠르게 더 많은 난수 표본 데이터를 생성할 수 있음

### numpy.random에 포함된 주요함
|:-------------|:----------------------------------------------------|
|함수          |기능                                                  |
|shuffle       |리스트나 배열의 순서를 뒤섞음                           |
|rand          |균등분포에서 표본 추출                                 |
|randint       |최소, 최대 범위내에서 임의의 난수 추출                  |
|randn         |표준편차1, 평균값이 0인 정규분포에서 표본추출            |
|normal        |정규분포에서 표본추출                                  |

+ rp.random : 0.0 ~ 1.0 사이의 난수를 발생하는 함수.(양수)
+ plt.imshow
  - cmap=None : 색깔
  - interpolation=None : 형태
--------------------------------------------------------------------
## 11강. Pandas 기본 사용 예
+ 데이터가 들어오면 '분석'을 거쳐, 자료로 이어진다.
+ 이 자료 안에서 연산작업을 가능하게 하는 것이 Pandas이다.

### pandas란?
+ 데이터 분석을 용이하게 할 수 있는 패키지
+ Pandas에서 가장 많이 사용하는 자료구조 - Series, DataFrame

+ 엑셀에서 해당 값의 숫자 카운트
```python
df['Political Party'].value_counts()
```
--------------------------------------------------------------------
## 12강. Series, DataFrame 자료구조객체 생성방법

### Series
+ 배열 구조와 같은 자료구조로써 '색인-값' 형태를 취함.
+ Series는 Pandas안에 있는 모듈이기에 필요한 Series만을 import한다.
```python
from pandas import Series
```

+ 인덱스를 바꾸기
```python
Series(rnp.random(5),index=['1회','2회','3회','4회','5회'])
info_list={'kim':26,'yoon':30,'min':33}
```
딕셔너리의 key값이 index가 된다.


### DataFrame
+ 엑셀문서처럼 여러 타입의 데이터를 저장할 수 잇는 자료구조
```python
mydata={'irum':['홍길동','장길산','임꺽정'],'addr':['서울','인천','서울'],
       'age':[23,30,29]}
df=DataFrame(mydata)
```
--------------------------------------------------------------------
## 13강. Series, DataFrame 접근방법설명
+ 2차원 구조의 접근
```python
df2['irum']
df2.irum

df2.irum['no.2']
df2['irum']['no.2']
```

+ 2차원 구조의 값 변경
```python
df2['gender']=['male','female','male']  # gender를 추가하고 각각 값추가.
df2.irum['no.2']='장길연'   # no.2에 해당하는 이름을 바꿈.
```

+ 2차원 구조의 정렬
```python
df2.sort_index                # 인덱스 정렬하기
df2.sort_index(ascending=0)   # 인덱스를 오름차순(0)으로 정렬(내림차순=1)
df2.sort_values(by='age')     # 값을 'age'에 맞춰서 정렬(오름차순)
```
--------------------------------------------------------------------
## 14강. pandas에서 제공하는 산술연산 메소드 학습
### 산술연산 작업
+ 객체내의 원소들에 대한 산술연산작업은 물론, 서로 다른 객체간의 산술연산작업도 지원됨.

```python
from pandas import DataFrame
import numpy as np

df1=DataFrame(np.zeros((3,3),dtype='int32'),index=['one','two','three'])
df2=df1+3

df3=DataFrame(np.ones((4,4),dtype='int32'),index=['one','two','three','four'])
df4=df2+df3
```

+ 산술연산 메소드를 이용한 객체간 연산작업

```python
df2.add(df3)
#결측값이 존재하는 즉, 인덱스의 갯수, 컬럼의 불일치등으로 인해 발생한 경우 결측값을 제거
df2.add(df3,fill_value=5)
```

+ 통계 계산 및 요약작업
  - pandas는 Series, DataFrame에 대해 컬럼, 행에 대한 계산 및 요약작업을 가능하게끔 다양한 함수 제공

--------------------------------------------------------------------
## 15강.결측데이터 확인과 처리방법 학습

|:------------|:------------------------------|
|메소드        |기능                           |
|count        |결측값을 제외한 값의 수를 반환   |
|sum          |합 계산                        |
|mean         |산술 평균 계산                  |
|std / var    |표준편차 계산/분산계산           |
|min / max    |최소 값 / 최대 값 계산           |
|Median       |중위수 반환                     |
|Cumsum       |누적값 계산                     |
|Cumprod      |누적곱 계산                     |
|describe     |각 컬럼에 대한 요약통계를 계산   |

+ 결측 데이터 처리
  - 결측값의 문제를 어떻게 어리하느냐에 따라 데이터 분석결과에 영향을 미치므로 결측값의 존재유무부터 확인하는게 중요.
  - 'none'값도 결측값으로 인식하며, 결측값인 경우 'NaN'으로 표시
  - 결측값 존재유무 확인 메소드 - isnull(), notnull()

```python
import numpy as np
import pandas as pd

df=pd.DataFrame(np.ones((4,4)),index=['a','b','c','d'])
df[1]['b']=np.nan
df[3]['b':]=np.nan
pd.isnull(df)
pd.notnull(df)
df[3].isnull().sum()

df2=df.dropna()
df3=df.dropna(how='all')

#결측값을 임의의 값으로 대체
df.fillna(1)
df.fillna({1:3,3:7})
df.fillna('unknown')
```
--------------------------------------------------------------------
## 16강.외부에서 파일을 읽어오는 방법학습
### pandas에서 파일 읽어오기
+ pandas 패키지에 일부형식의 파일을 간편하게 읽어올 수 있는 기능 제공
|:------------|:--------------------------------------------------------|
|메소드        |기능                                                     |
|read_csv     |csv형식 포함한 텍스트 파일을 읽어옴.(구분자는 쉼표(,) 기본값)|
|read_table   |일반 텍스트파일을 읽어옴(기본 구분자는 tab(\t))             |
|read_excel   |엑셀파일을 읽어옴                                         |

```python
# csv파일로 저장하는 방법 ( endcoding='ms949'를 해야 한글 안깨짐)
df.to_csv('sample1.csv',encoding='ms949')
# csv파일을 로딩하는 방법
df1=pd.read_csv('sample1.csv', encoding='cp949')
#컬럼명을 지정
df1=pd.read_csv('sample1.csv', names=['no','시도','구분','인구','면적'],encoding='cp949')
# 컬럼명으 인덱스로 지정
df1=pd.read_csv('sample1.csv', index_col='no',names=['no','시도','구분','인구','면적'], encoding='cp949')
# 불필요한 행 제외하고 로딩시키기
df1=pd.read_csv('sample1.csv', index_col='no',names=['no','시도','구분','인구','면적'], encoding='cp949',skiprows=[0,3,5])
df1=pd.read_csv('sample1.csv', index_col='no',names=['no','시도','구분','인구','면적'], encoding='cp949',nrows=3)
```
--------------------------------------------------------------------
## 17강.데이터필터
### JSON 형식 파일 읽어오기
+ JSON(JavaScript Object Notation)은 자바스크립트로부터 파생된 데이터포맷으로, 웹브라우저와 서버간 데이터전송을 위한 표준포맷

### 데이터처리를 위한 조작작업
+ 분석에 필요한 데이터만 - 데이터 필터링 작업
```python
df=pd.read_excel('highwaybus.xlsx',sheetname='highway',encoding='cp949')
df.head()

#필터링1-우등고속에 대한 데이터대상
df_ex=df[df.차종=='우등']
df_ex.head()

#경부선 총이용인원1000명 이상인 데이터에 대한 분석
df_gx=df[(df.선별=='경부선') & (df.총이용인원>=1000)]
df_gx.sort_values(by='총이용인원',ascending=0).head(3)
```
--------------------------------------------------------------------
## 18강.그룹화 방법 학습
### 그룹화 연산
+ 분석목적에 맞게 필요한 함수를 이용하여 연산
  - count() - 건수 계산
  - sum() - 합계 계산
  - mean() - 평균 계산
  - max() - 최댓값 계산
  - first() - 그룹화 작업에서 최초값 계산

### 빈도계산을 위한 피벗 - 교차표(cross_tab)

```python
# 그룹화작업-1
df.groupby('선별').count()

d_group1=df.groupby('선별')
d_group1['차종'].count().sort_values(ascending=False)

%matplotlib notebook
d_group1['차종'].count().plot(kind='bar')

# 차종, 선별 그룹화 작업
d_avg=df.groupby(['차종','선별'])
d_avg['총이용인원'].mean()

# 교차테이블 작성하는 작업
pd.crosstab(df['차종'],df['선별'], margins=True)
```

--------------------------------------------------------------------
## 19강.파이썬 내장 문자열 메소드 사용법 설명
### 텍스트 처리관련 기본메소드
|:-------------------|:-------------------------------------------------------------------|
|메소드              |기능                                                                 |
|Upper / lower       |문자열 대문자로 변경/소문자로 변경                                     |
|title               |각 단어의 첫 문자를 대문자로 변경                                      |
|count               |문자열 출현 횟수 리턴                                                 |
|find                |찾고자하는 단어의 첫글자 위치를 리턴(*찾고자하는 단어가 없을 경우 -1리턴) |
|replace             |문자열을 다른 문자열로 치환                                           |
|Strip/rstrip/lstrip |문자열 좌우공백을 제거/오른쪽 문자열 공백제거/왼쪽 문자열 공백 제거       |
|split               |구분자 기준으로 단어리스트로 분리                                      |
|Ljust / rjust       |지정된 문자열 길이에서 왼쪽/오른쪽 정렬하고 남은 길이만큼 공백처리 리턴   |

### 정규표현식 이용한 문자열 처리
+ 정규표현식 - 문자열 처리를 유연하게 하기위한 기법
+ 파이썬에서는 정규표현식 이용하여 문자열을 처리하는 re모듈을 제공

|:--------|:---------------------------------------------------------|
|표현식    |설명                                                      |
|?        |문자열 시작                                                |
|^        |문자열 종료                                                |
|$        |앞 문자가 없을 수도 무한정 많을 수도 있음.                   |
|*        |앞 문자가 하나 이상                                         |
|+        |앞 문자 없거나 하나 있음                                    |
|[]       |문자집합이나 범위를 나타내며, 두 문자 사이에는 -기호로 범위표시|
|{}       |횟수 또는 범위를 나타냄                                     |
|()       |소괄호 안의 문자를 하나의 문자로 인식                        |
|   |     |패턴 안에서 or 연산을 수행할때 사용                          |
|\s       |공백문자                                                   |
|\S       |공백문자가 아닌 나머지 문자                                 |
|\w       |알파벳이나 숫자                                            |
|\W       |알파벳이나 숫자를 제외한 문자                               |
|\d       |숫자[0-9]와 동일                                           |
|\D       |숫자를 제외한 모든 문자                                     |
|\특수문자 |해당 특수문자를 의미                                        |
|(?!)     |대/소문자 구분하지 않음                                      |

--------------------------------------------------------------------
## 20강. Re모듈 이용한 정규표현식 작성
+ 정규표현식 - 문자열 처리를 유연하게 하기 위한 기법
+ 파이썬에서는 정규표현식 이용하여 문자열 을 처리하는 re모듈을 제공


```python
import re

p=re.compile(r'(\w+)\s+\d+-\d+-\d+')
result=p.search("honggildong   010-1234-4321")
print(result)
# <re.Match object; span=(0, 27), match='honggildong   010-1234-4321'>
result=p.search("honggildong   010-1234-4321")
print(result.group(1))
# honggildong

source1="modelA 공급면적 220.98제곱미터 전용면적 193.43제곱미터"
result1=re.findall('공급면적\s+(.*?)제곱미터',source1)[0]
result2=re.findall('전용면적\s+(.*?)제곱미터',source1)[0]
print('공급면적:',result1)
print('전용면적:',result2)
# 공급면적: 220.98 / 전용면적: 193.43
```

--------------------------------------------------------------------
## 21강.pandas에서 정규표현식 이용하여 문자열 검색하는 방법 학습

```python
import re
source_a="dodo/mimi//solsol///lala,sisi"

mat_1=re.compile('\/+')  # '\'뒤에 필요한 기호'/'추가, '+'붙여 여러개 가능.
mat_1.split(source_a)
#결과 : ['dodo', 'mimi', 'solsol', 'lala,sisi']

mat_2=re.compile('\/+|\,') # |를 이용하여 조건추가. (','조건.)
mat_2.split(source_a)
#결과 : ['dodo', 'mimi', 'solsol', 'lala', 'sisi']

# compile메소드와 split을 한 줄로 코딩
re.split('\/+|\,',source_a)
['dodo', 'mimi', 'solsol', 'lala', 'sisi']
```
​
### pandas에서 text분석을 위한 문자열 함수
+ 데이터 분석을 위해 문자열 정제작업은 꼭 필요.
+ 이를 간결하게 처리하기 위한 문자열 메소드 제공
|:--------|:------------------------------------------------------------------------------|
|메소드    |설명                                                                           |
|cat      |선택적인 구분자와 함께 요소별 문자열 이어붙임                                      |
|contains |문자열이 패턴이나 정규표현식을 포함하는지 나타내는 불리언 배열 반환                  |
|count    |일치하는 패턴의 개수를 반환                                                       |
|findall  |각 문자열에 대해 일치하는 정규표현식의 전체 목록을 구함                             |
|join     |Series외 각 요소를 주어진 구분자로 연결                                           |
|match    |주어진 정규표현식으로 각 요소에 대한 re.match를 수행하여 일치하는 그룹의 리스트로 반환|
|get      |1번째 요소를 반환                                                                |

```python
from pandas import Series
import numpy as np
import re

people_info={'hong':'mrhong@gmail.com','jang':'roadmount@naver.com',
            'gilsoon':'hks900@naver.com','lim':'tmplim1@gmail.com',
            'mimi':'mimi@daum','unknown':np.nan}

emailinfo=Series(people_info)
emailinfo
/*
hong          mrhong@gmail.com
jang       roadmount@naver.com
gilsoon       hks900@naver.com
lim          tmplim1@gmail.com
mimi                 mimi@daum
unknown                    NaN
dtype: object
*/

emailinfo.str.contains('naver')
/*
hong       False
jang        True
gilsoon     True
lim        False
mimi       False
unknown      NaN
dtype: object
*/

p=r'[a-z0-9._%+-]+@[a-z0-9]+\.[a-z]{2,3}'
emailinfo.str.findall(p,flags=re.IGNORECASE)
/*
hong          [mrhong@gmail.com]
jang       [roadmount@naver.com]
gilsoon       [hks900@naver.com]
lim          [tmplim1@gmail.com]
mimi                          []
unknown                      NaN
dtype: object
*/

p1=r'([a-z0-9._%+-]+)@([a-z0-9]+)\.([a-z]{2,3})'
a=emailinfo.str.findall(p1,flags=re.IGNORECASE)
a
/*
hong          [(mrhong, gmail, com)]
jang       [(roadmount, naver, com)]
gilsoon       [(hks900, naver, com)]
lim          [(tmplim1, gmail, com)]
mimi                              []
unknown                          NaN
dtype: object
*/

a.get(1) # a의 1번째 자료를 가져옴.
# 결과 : [('roadmount', 'naver', 'com')]
```
​
--------------------------------------------------------------------
## 22강.데이터 시각화 작업
### matplotlib
+ 파이썬 프로그램에서 일반적으로 많이 사용하는 시각화 모듈

+ matplotlib로 초간단 그래프 작성하기 - plot()
```python
import matplotlib.pyplot as plt

plt.plot([10,20,30,40,50])
plt.show()


plt.plot([2015,2016,2017],[3.2,2.8,3.6])  # [x축], [y축]
plt.show()
```

+ plot() 함수의 기본 옵션 - 색상, 선, 스타일, 강조문자 표시
  - linestyle = '' 선스타일 지정
  - color = '' 색상지정
  - marker = '' 강조를 위한 마커 스타일 설정
  ```python
  plt.plot([12,15.3,10.9,14,16],'ro--')

  plt.plot([12,15.3,10.9,14,16],color='b',linestyle='dashed',marker='+')
  ```

+ 하나의 화면에 여러 개의 그래프 그려보기 - figure & subplot
  - 그래프는 figure 객체내에 존재함

--------------------------------------------------------------------
## 23강. matplotlib의 plot메소드

```python
%matplotlib notebook   # 그래프 오류가 있을경우 추가하는 구문
import matplotlib.pyplot as plt  # matplotlib의 pyplot을 plt로 선언
import numpy.random as nrp  # numpy의 random메소드 nrp로 선언

fig=plt.figure()  # fig라는 변수에 plt를 figure메소드로 그린다.

ax1=fig.add_subplot(2,1,1) # fig라는 형식으로 2열 1행 1번째에 그래프를 넣음.
ax2=fig.add_subplot(2,1,2) # fig라는 형식으로 2열 1행 2번째에 그래프를 넣음.

ax1.plot(nrp.randn(100)) # nrp로 랜덤 100개의 정수로 뽑아냄
ax2.plot(nrp.randn(200).cumsum())  # nrp로 랜덤 200개의 정수로 뽑아 누적값 뽑아냄
fig.show()   # fig를 show하라
```

### x,y축 값 범위 지정하는 set_xlim(), set_ylim()함수
+ set_xticks()함수는 눈금의 간격을 정함.
```python
%matplotlib notebook
import matplotlib.pyplot as plt
import numpy.random as nrp

fig=plt.figure()

a1=fig.add_subplot(111)
a1.plot(nrp.randn(50),'b*--')
a1.set_xlim([1,50])   # x축 1부터 50까지
a1.set_ylim([-1,3])   # y축 -1부터 3까지
a1.set_title('basic graph')
a1.set_xlabel('data_x')
a1.set_ylabel('data_y')


plt.plot(nrp.randn(10))
plt.xlabel('data_x')
plt.ylabel('value_y')
plt.title('just sample')
plt.text(0.5,0.5,'random value')  # 0.5, 0.5에 'random value'기입
plt.grid(True)
plt.show()
```
--------------------------------------------------------------------
## 24강.그래프요소 설정방법학습
```python
fig=plt.figure()
a2=fig.add_subplot(111)
a2.plot(nrp.randn(50),'r-+')
a2.set_xticks([10,20,30,40,50])
a2.set_xticklabels(['서울','경기','강원','충북','세종'])
```

+ 한글 깨짐
  - matplotlib에 한글폰트를 등록시켜야 한다.
```python
%matplotlib notebook
import matplotlib.pyplot as plt
import numpy.random as nrp
from matplotlib import font_manager,rc

#한글깨지는 문제 보완
font_location="c:/windows/fonts/malgun.ttf"
font_name=font_manager.FontProperties(fname=font_location).get_name()
rc('font',family=font_name)
```

### pandas + matplotlib
+ pandas에서 처리된 행, 열, 그룹 정보등을 이용하여 시각화 도표 작성이 가능

--------------------------------------------------------------------
## 25강.pandas모듈에서 차트 작성방법학습
|:--------------|:--------------------------------------------------|
|인자           |설명                                                |
|label          |범례이름                                            |
|style          |matplotlib의 color, linestyle에 전달할 값            |
|alpha          |그래프 투명도 설정                                    |
|kind           |그래프 종류(line-선, bar-막대, barh-수평막대, pie-원형)|
|Xticks / yticks|X,y 축으로 사용할 값                                 |
|Xlim / ylim    |X,y 축한계값                                        |
|grid           |그리드 표시여부                                      |

```python
%matplotlib notebook
from pandas import Series,DataFrame

s=Series([1.5,2.4,3.7],index=['no1','no2','no3'])
s.plot(kind='bar')

from matplotlib import pyplot as plt

s.plot.bar()
plt.xlabel("item")
plt.ylabel("value")
plt.title('sample series')
```

### 데이터 프레임 객체에서 그래프 작성 - DataFrame-plot
|:-----------|:-----------------------------------------|
|인자         |설명                                      |
|subplots     |데이터프레임 걸럼을 독립된 서브플롯에 그림   |
|figsize      |생성될 그래프의 크기를 튜플로 지정          |
|title        |그래프 제목을 문자열로 지정                 |
|legend       |서브플롯의 범례를 추가                      |
|Sort_columns |컬럼을 알파벳 순서로 작성                   |

```python
#데이터 프레임에서 시각화 작업
import numpy.random as nrp
import numpy as np
df=DataFrame(nrp.randn(10,4),columns=['foo','pitch','mimi','tomo'],
          index=np.arange(0,100,10))
d1=df.plot(kind='barh',title='dataframe chart',legend=True)
d1.set_xlabel('value sequence',fontsize=14)
```
--------------------------------------------------------------------
## 26강.2012-2014년도 교통사건 사고파일을 읽어들여 아래와 같은 분석 실습
### 우리가 사용할 데이터에 대해
+ accidentdata.csv - 2012-2014년도 교통사건 사고파일 제(공공데이터 포털사이트)
+ 분석포커스
  - 요일별 중대교통사고 사상자 합계 (사상자 3명이상인 데이터에 대해)
  - 경기도에서 교통사망사고가 많은 5지역을 분석하여 도식화
  - 요일별/발생지시도별 교통사고 빈도분석

### 1.요일별 중대교통사고 사상자 합계분석
+ 제일 먼저 해야할 작업은 '필터링'작업.
```python
%matplotlib notebook
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import font_manager,rc

#한글깨짐방지작업
font_info='c:/windows/fonts/malgun.ttf'
font_name=font_manager.FontProperties(fname=font_info).get_name()
rc('font',family=font_name)

data=pd.read_csv('accidentdata.csv')
data.head(5)
########################## 여기까지가 공통작업 ###########

d2=data[(data.사상자수>=3)]  #사상자수가 '3'이상인 것들만 필터링=d2
d3=d2.groupby('요일')   # 필터링한 d2를 '요일'별로 그룹화=d3
d3['사상자수'].sum().plot(kind='bar',title='2012-2014요일별 중대교통사고 사상자분석')
#d3를 가지고 막대그래프 그리기
```

--------------------------------------------------------------------
## 27강.경기도-시군별 교통사망사고 빈발지역 분석
+ x축 글자수정
```python
result=d3['사상자수'].sum().plot(kind='bar',title='2012-2014요일별 중대교통사고 사상자분석')
result.set_xticklabels(['금요일','목요일','수요일','월요일','일요일','토요일','화요일'], fontsize=10, rotation=0)
```

### 2.경기도내 교통사망사고가 높은 5지역 분석하여 도식화
+ 1. 경기지역을 대상으로 분석하기 위해 필터링하여 d1 객체생성.
+ 2. 경기지역의 시,군별 그룹화 작업 실행하는 sa1 객체생성.
+ 3. 사망자 수 합계를 요약하여 da2 객체생성.

```python
df1=data[data.발생지시도=='경기']  #'경기'로 필터링=df1
df2=df1.groupby(['발생지시군구'])  #'발생지시군구'로 그룹화=df2
df3=df2['사망자수'].sum()  #'사망자수'의 합계구하기=df3
result1=df3.sort_values(ascending=False).head(5) #df3를 내림차순하여 앞의 5개만 출력=result1
result2=df3.sort_values(ascending=True).tail(5) #df3를 오름차순하여 뒤의 5개만 출력=result2
```
--------------------------------------------------------------------
## 28강.요일별-지역별 교통사고빈도 분석

```python
result1.plot(kind='pie',title='2012-2014 경기도내 교통사망사고 빈번지역')

#이렇게도 작성할 수 있어요. matplotlib.pyplot 이용하여 파이차트 작성
c=['red','brown','green','purple','yellow']
plt.pie(result1,colors=c,autopct='%.2f')
plt.title('2012-2014 경기도내 교통사망사고 빈발지역')
```

### 3.요일별/발생지시도별 교통사고 분석
+ 1.교차분석 실행하여 요일별 발생지시도별 데이터 건수를 계산
+ 2.각 행의 합이 1이 되도록 비율로 설정한 후 그 내용을 누적막대형 차트로 시각화

```python
d_pv=pd.crosstab(data['요일'],data['발생지시도'])
#'요일','발생지시도' 크로스탭으로 만들기=d_pv
result=d_pv.div(d_pv.sum(1).astype(float),axis=0)
#한행을 전체 1로 놓고 소수점으로 표현하여 나누어 백분율로 표현=result
result.plot(kind='bar',stacked=True)
#'result'값을 막대그래프 형태로 표현(누적=True)

#발생지시도별 교통사고 요일분석
d_pv1=pd.crosstab(data['발생지시도'],data['요일'])
#'요일','발생지시도' 크로스탭으로 만들기=d_pv1
result1=d_pv1.div(d_pv1.sum(1).astype(float),axis=0)
#한행을 전체 1로 놓고 소수점으로 표현하여 나누어 백분율로 표현=result1
result1.plot(kind='bar',stacked=True)
#'result1'값을 막대그래프 형태로 표현(누적=True)
```
