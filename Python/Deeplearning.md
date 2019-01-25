[깊은 관계]......팀장="이동은"

-------------------------------------------------------------
[Jupyter Lab + google Drive] install
$ conda install nodejs
$ jupyter labextension install @jupyterlab/google-drive
$ jlpm install
$ jlpm run build
$ jupyter labextension install

$ jlpm run watch
$ jupyter lab --watch

& pip install jupyterlab
& jupyter labextension install @jupyterlab/google-drive
& jupyter lab



{"clientId":"1061424794848-1vco6ra5lr2n586srntf11p9m205o1nr.apps.googleusercontent.com"}


------------------------------------------------------------
#### CHAPTER 2. 퍼셉트론(Perceptron)
+ 퍼셉트론 = 프랑크 로젠블라트(Frank Rosenblatt), 1957년 고안한 알고리즘
+ 퍼셉트론이 신경망(딥러닝)의 기원이 되는 알고리즘이다.

### 2.1 퍼셉트론이란?
+ 퍼셉트론 = 다수 신호를 입력받아, 하나의 신호로 출력
+ 신호 = 전류 or 강물처럼... 흐름이 있는 것. (흐른다 = 1, 흐르지X = 0)

# x1 --(W1)--> y <--(W2)-- x2
+ x1, x2 = 입력신호  -----> "뉴런" or "노드"
+ y = 출력신호  -----> "뉴런" or "노드"
+ W1, W2 = 가중치

+ 입력신호의 보내는 방식 = 입력신호(x1,x2) X 가중치(W1,W2)....[x1*W1,x2*W2]

+ 뉴런에서 보내온 신호의 '총합(W1*x1 + W2*x2)' >= '한계 or 임계값(θ)'를 넘어설때..."1"출력
+ 뉴런에서 보내온 신호의 '총합(W1*x1 + W2*x2)' < '한계 or 임계값(θ)'를 넘지못할때..."0"출력
# y = 0 ( W1*x1 + W2*x2 <= θ )
# y = 1 ( W1*x1 + W2*x2 > θ )

+ 퍼셉트론은 복수의 입력 신호 각각에 고유한 가중치를 부여함.
+ 가중치는 각 신호가 결과에 주는 영향력을 조절하는 요소로 작용함.
+ 가중치가 클수록 해당 신호가 그만큼 더 중요함을 뜻한다.
  - 가중치는 전류에서 말하는 저항. 그 값이 클수록 강한 신호를 보냄.(But! 저항성질과 반대)

### 2.2 단순한 논리 회로
## 2.2.1 AND 게이트
+ AND게이트는 입력이 '둘', 출력은 '하나'.
+ 두 입력이 모두 '1'일 때만, '1'을 출력. 그 외는 '0'출력.

## 2.2.2 NAND 게이트와 OR 게이트
+ NAND = Not AND. 즉, AND 게이트의 출력을 뒤집은 것.
  - 두 입력이 모두 '1'일 때만, '0'을 출력. 그 외는 '1'출력.
+ OR = 입력신호 중 하나 이상이 '1'이면, 출력이 '1'.

+ 퍼셉트론으로 AND, NAND, OR 논리 회로를 표현할 수 있다.
+ 세 가지 게이트... 구조는 같으나, 다른 것은 "매개변수(가중치와 임계값)"의 값.


### 2.3 퍼셉트론 구현하기
## 2.3.1 간단한 구현부터
```python
def AND(x1, x2):
    w1, w2, theta = 0.5, 0.5, 0.7
    tmp = x1*w1 + x2*w2
    if tmp <= theta:
        return 0
    elif tmp > theta:
        return 1

AND(0,0)    # 0출력
AND(1,0)    # 0출력
AND(0,1)    # 0출력
AND(1,1)    # 1출력
```

## 2.3.2 가중치와 편향 도입
+ y = 0(b + w1*x1 + w2*x2 <= 0)
+ y = 1(b + w1*x1 + w2*x2 > 0)

+ b = 편향(blas)
  - 값이 '0'을 넘으면 '1'을... 그렇지 않으면 '0'을 출력.

```python
import numpy as np
x = np.array([0,1])   # 입력
w = np.array([0.5,0.5])   # 가중치
b = -0.7    # 편향
w*x
# array([0.,0.5])
np.sum(w*x)   # 배열에 담긴 모든 원소의 총합을 계산.
# 0.5
np.sum(w*x) + b
# -0.19999999999999996
```
+ 넘파이 배열끼리의 곱셈...원소가 같다면 각 원소끼리 곱.
  - "w*x"에서, 인덱스가 같은 원소끼리 곱함.([0.1]x[0.5,0.5] => [0,0.5])

## 2.3.3 가중치와 편향 구현하기
+ '가중치와 편향을 도입'한 AND 게이트는 다음과 같이 구형가능.
```python
def AND(x1, x2):
  x = np.array([x1, x2])
  w = np.array([0.5,0.5])
  b = -0.7
  tmp = np.sum(w*x) + b
  if tmp <= 0:
    return 0
  else:
    return 1

def NAND(x1, x2):
  x = np.array([x1, x2])
  w = np.array([-0.5,-0.5])
  b = 0.7
  tmp = np.sum(w*x) + b
  if tmp <= 0:
    return 0
  else:
    return 1

def OR(x1,x2):
    x = np.array([x1,x2])
    w = np.array([0.5,0.5])
    b = -0.2
    tmp = np.sum(w*x) + b
    if tmp <= 0:
        return 0
    else:
        return 1
```

### 2.4 퍼셉트론의 한계
## 2.4.1 도전! XOR 게이트
+ XOR 게이트는 "배타적 논리합"이라는 논리회로.
