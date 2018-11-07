
#2 프로그램과 프로그래밍
### 프로그램이란?
문제를 해결하기 위해서 명령을 모아놓은 것을 말합니다.
컴퓨터를 이용해서 우리 일상의 문제들을 해결할 수 있습니다.
프로그램이라는 말은 고대 그리스어에서 유래되었습니다. 미리 정해놓은 것이라는 뜻을 가지고 있습니다.
방송 프로그램, 운동 프로그램처럼 일상 속에서도 프로그램이라는 말을 사용하고 있습니다.
일상 속 여러 프로그램들처럼 미리 정해놓은 것을 순서대로 실행하는 것을 말합니다.
코딩(coding)과 프로그래밍(programming)은 엄밀히 말하면 다르지만 혼용해서 많이 사용합니다.
프로그래밍 언어란 컴퓨터가 알아듣는 언어를 말합니다. 세상에는 다양한 프로그래밍 언어가 있습니다.

### 프로그램의 기본 구조
모든 프로그램은 순차, 선택, 반복 세 가지 기본 구조로 구성되어 있습니다.
순차 : 정해진 순서대로 명령을 수행하는 것을 말합니다.
선택 : 조건에 따라 흐름을 바꾸는 것을 말합니다.
반복 : 같은 명령을 조건이나 횟수에 따라 반복하는 것을 말합니다.

-------------------------------------------------------------------

#3 왜 파이썬일까요?
### 쉽습니다
문법이 쉽고 간결해서 배우고 사용하기 쉽습니다.
코딩 자체는 쉽지 않지만 파이썬은 다른 언어들보다는 비교적 쉽습니다.
우리가 사용하는 영어와 유사하기 때문에 친숙한 느낌을 받을 수 있습니다.
외국의 경우 파이썬을 입문 언어로 많이 사용합니다. 우리나라도 바뀌고 있는 추세입니다.
범용 프로그래밍 언어이기 때문에 데이터 분석, 게임 개발, 보안 등 다방면에 사용이 가능합니다.
아래는 자바라는 언어로 Life is Short!를 분리하고 출력하는 코드입니다.

``` java
public static void main(String[] args) {
    String msg = "Life is Short!";
        for(String m : msg.split(" "))
            System.out.println(m);
}
```

아래는 파이썬으로 Life is Short!를 분리하고 출력하는 코드입니다.

``` py
msg = 'Life is Short!'
print(msg.split())
```

같은 기능인데도 파이썬이 자바보다 간결한 것을 볼 수 있습니다.


### 많습니다
파이썬을 사용하는 사람이 아주 많기 때문에 참고할 자료도 많습니다.
스택오버플로우 (https://stackoverflow.com/) : 코딩계의 네이버 지식인이라 할 수 있습니다.
질문도 엄청 많지만 답변은 이보다 훨씬 많습니다. 훑어보는 것만으로도 공부가 됩니다.
파이썬의 모토 : 어떤 문제를 해결하는 가장 아름다운 방법이 있다.
파이썬과 자주 비교되는 언어로 펄이 있는데 펄은 문제를 해결할 때 다양한 방법을 선호합니다.


###파이썬의 도
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.  
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!

빠릅니다
실행속도가 빠르다는 것이 아니라 개발을 빠르게 할 수 있다는 말입니다.
배우는데 걸리는 시간도 다른 언어에 비해 비교적 적게 드는 편입니다.
제품을 구매했을 때 배터리가 포함되어 있어 따로 구매하거나 필요가 없는 경우가 있습니다.
파이썬은 제공하는 라이브러리들이 많아서 구현할 필요 없이 바로 가져다 쓸 수 있습니다.
보통 이런 라이브러리들은 충분히 검증이 되어있기 때문에 직접 만드는 것보다 성능이 좋습니다.

----------------------------------------------------------------------
