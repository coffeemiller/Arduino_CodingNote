
### [2019-05-10]

#### 1. Prolog
+ 3가지 구성요소
  - 1) IP : 컴퓨터를 식별하는 값
    - 의미있는 문자열로 만들어 명칭을 등록하여 사용.
  - 2) Port 번호 : PROCESS를 식별하는 번호(0~65535)
    - 127.0.0.1 or localhost.... 자신(컴퓨터)을 지칭하는 말.
    - 특정프로그램에 대하여 정해진 번호가 있음 (0~1024)
    - 사용자가 작성하는 Network Program에서 포트를 결정 or 임의(1024~65535)
  - 3) 통신규약(Protocol)
    - TCP
    - UDP
    - HTTP

+ 클라이언트(Client) : html, javascript  <- 디자이너 + 프로그래머
+ 서버(Server) : php, Servlet, Jsp  <- 프로그래머


#### 2. Network Programming
+ 1) Network ==> 두대이상의 컴퓨터를 케이블(유/무선)로 연결한 망
+ 2) Network Program ==> 두대 이상의 컴퓨터에서 데이터를 주고/받는 프로그램
+ 3) 3대 필수요소
  - (1) IP : 컴퓨터 주소(address) ==> 컴퓨터를 식별하기위해 부여한 명칭
    - Ipv4 : 4byte를 이용하여 주소부여
      - 1byte에 표현할 수 있는 숫자값(-128~0~127 ==양수 ==> 0~255)
      - 정수숫자배열 byte[] <== 내부적으로 저장된 값
      - "192.168.20.27" ----> DNS(도메인서버)에 손쉬운 문자열로 등록사용 => 웹사이트주소(www.jica.kr) => 웹프로그램
    - Ipv6 : 6byte를 이용하여 주소부여
  - (2) Port번호 : 컴퓨터 내에 동작중인 네트워크 프로그램을 식별하기 위한 Process 번호(id)
    - 0~65535
      - 1) 0~1024 : 잘 알려진 port번호(특정 프로그램에 할당되어 있다)
      - 2) 1025~65535 : 우리가 작성하는 프로그램에 부여되는 번호

  - (3) Protocol(통신규약) : 네트워크로 연결된 컴퓨터끼리 데이터를 주고받는 약속
    - TCP/IP : 연결된 상태에서만 데이터를 주고 받는다. (전화)
      - Socket, ServerSocket
    - UDP    : 연결되지 않은 상태에서도 데이터를 전송할수 있다. (문자메세지/메일)
      - DatagramSocket, DatagramPacket
    - HTTP   : html문서를 주고받는 통신규약
```
client  ---request-----> server(웹서버)
        <--response-----      서버프로그램(Servlet/JSP) --실행
          (html문서)
-front end----------------back end---------------------
 html                     Servlet/Jsp
 javaScript/jQuery
                          ASP
                          PHP
```
    - Java 언어에서는 java.net패키지에서 위의 클래스들을 지원하고 있다
      - 1) 웹브라우저
      - 2) URL, URL Connection <== http 프로토콜을 사용하는 java의 클래스들

    - TCP/IP, UDP = Socket Program..... HTTP = Web Program

+ 현재 프로그램이 동작하는 컴퓨터 자체의 IP를 지칭하는 표현하는 방법
  - 1) "127.0.0.1"
  - 2) "localhost"
  - 3) "192.168.20.27"

+ 웹(Web)프로그램에서 원하는 정보가 있는 상세한 위치정보를 표현 => URL(Uniform Resource Locator)
  - 원하는 정보 ===> html문서, 사진, 동영상... <-- 다른표현으로 resource라고 한다.
```
http://www.jica.or.kr:80/2016/inner.php?sMenu=A2000&mode=view&no=164
----  --------------- -- ----- -------- ----------------------------
protocol  ip          port path 웹컴퍼넌트 필요한정보(쿼리+참조)
```


#### 3. TCP/IP
#### 4. UDP
#### 5. 실습
#### 6. Summary / Close


-----------------------------------------------------------

### [2019-05-13]

#### 1. Review

#### 4. 실습
#### 5. Summary / Close



-----------------------------------------------------------

### [2019-05-14]

#### 1. Review

#### 4. 실습
#### 5. Summary / Close



-----------------------------------------------------------

### [2019-05-15]

#### 1. Review

#### 4. 실습
#### 5. Summary / Close
