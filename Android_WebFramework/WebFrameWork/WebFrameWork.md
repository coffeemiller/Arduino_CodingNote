
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

```java
//NetworkEx1.java
import java.util.Arrays;

class NetworkEx1 {
	public static void main(String[] args)
	{
		InetAddress ip = null;
		InetAddress[] ipArr = null;

		try {
			//우리가 웹프로그램에서 많이 사용하는 포털사이트의 주소라고 칭하는 명칭
			//Domain Name
			ip = InetAddress.getByName("www.naver.com");
			System.out.println("getHostName() :"   +ip.getHostName());
			System.out.println("getHostAddress() :"+ip.getHostAddress());
			System.out.println("toString() :"      +ip.toString());

			byte[] ipAddr = ip.getAddress();
			System.out.println("getAddress() :"+Arrays.toString(ipAddr));

			String result = "";
			for(int i=0; i < ipAddr.length;i++) {
				result += (ipAddr[i] < 0) ? ipAddr[i] + 256 : ipAddr[i];
				result += ".";
			}
			result += result.length()-1;
			System.out.println("getAddress()+256 :"+result);
			System.out.println();
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}

		try {
			ip = InetAddress.getLocalHost();
			System.out.println("getHostName() :"   +ip.getHostName());
			System.out.println("getHostAddress() :"+ip.getHostAddress());
			System.out.println();
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}


		try {
			ipArr = InetAddress.getAllByName("www.naver.com");

			for(int i=0; i < ipArr.length; i++) {
				System.out.println("ipArr["+i+"] :" + ipArr[i]);
			}			
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}

	} // main
}
```

```java
//NetworkEx2.java
package com.jica.net;
import java.net.URL;

class NetworkEx2 {
	public static void main(String args[]) throws Exception {
		URL url = new URL("http://www.codechobo.com:80/sample/"+"hello.html?referer=javachobo#index1");
//		URL url = new URL("https://docs.oracle.com/javase/9/docs/api/java/net/URL.html");

		System.out.println("url.getAuthority():"+ url.getAuthority());
//		System.out.println("url.getContent():"+ url.getContent());
		//실제로 접속해서 요청정보를 가져오려고 한다. 이때 존재하지 않는 웹 컴퍼넌트이므로
		//예외발생
		System.out.println("url.getDefaultPort():"+ url.getDefaultPort());
		System.out.println("url.getPort():"+ url.getPort());
		System.out.println("url.getFile():"+ url.getFile());
		System.out.println("url.getHost():"+ url.getHost());
		System.out.println("url.getPath():"+ url.getPath());
		System.out.println("url.getProtocol():"+ url.getProtocol());
		System.out.println("url.getQuery():"+ url.getQuery());
		System.out.println("url.getRef():"+ url.getRef());
		System.out.println("url.getUserInfo():"+ url.getUserInfo());
		System.out.println("url.toExternalForm():"+ url.toExternalForm());
		System.out.println("url.toURI():"+ url.toURI());
	}
}
```

+ 안드로이드에서는 URL과 이를 좀 더 상세하게 표현하도록 만든 URI도 사용한다.

+ 우리 입장에서의 URL은 웹프로그램(웹브라우저를 이용하는)에서 사용하는 것이 아니라 일반 JAVA프로그램에서 웹상의 정보를 요청/응답받을때 주로 사용할 것이다.(안드로이드 프로그램)


+ NetworkEx3.java
```java
package com.jica.net;

import java.net.URL;
import java.net.URLConnection;

public class NetworkEx3 {
	public static void  main(String args[]) {
		URL url = null;
		String address = "http://www.naver.com:80/index.html";
		String line = "";

		try {
			url = new URL(address);
			//아래처럼 URLConnection객체가 만들어지면 내부적으로
			//해당 URL에 접속하여 응답을 받아왔다.
			URLConnection conn = url.openConnection();

			System.out.println("conn.toString():"+conn);
			System.out.println("getAllowUserInteraction():"+conn.getAllowUserInteraction());
			System.out.println("getConnectTimeout():"+conn.getConnectTimeout());
			System.out.println("getContent():"+conn.getContent());
			System.out.println("getContentEncoding():"+conn.getContentEncoding());
			System.out.println("getContentLength():"+conn.getContentLength());
			System.out.println("getContentType():"+conn.getContentType());
			System.out.println("getDate():"+conn.getDate());
			System.out.println("getDefaultAllowUserInteraction():"+conn.getDefaultAllowUserInteraction());
			System.out.println("getDefaultUseCaches():"+conn.getDefaultUseCaches());
			System.out.println("getDoInput():"+conn.getDoInput());
			System.out.println("getDoOutput():"+conn.getDoOutput());
			System.out.println("getExpiration():"+conn.getExpiration());
			System.out.println("getHeaderFields():"+conn.getHeaderFields());
			System.out.println("getIfModifiedSince():"+conn.getIfModifiedSince());
			System.out.println("getLastModified():"+conn.getLastModified());
			System.out.println("getReadTimeout():"+conn.getReadTimeout());
			System.out.println("getURL():"+conn.getURL());
			System.out.println("getUseCaches():"+conn.getUseCaches());
		} catch(Exception e) {
			e.printStackTrace();
		}
	} // main
}
```
```
conn.toString():sun.net.www.protocol.http.HttpURLConnection:http://www.naver.com:80/index.html
getAllowUserInteraction():false
getConnectTimeout():0
getContent():sun.net.www.protocol.http.HttpURLConnection$HttpInputStream@53d8d10a
getContentEncoding():null
getContentLength():-1
getContentType():text/html
getDate():1557457235000
getDefaultAllowUserInteraction():false
getDefaultUseCaches():true
getDoInput():true
getDoOutput():false
getExpiration():0
getHeaderFields():{Transfer-Encoding=[chunked], null=[HTTP/1.1 302 Moved Temporarily], Server=[NWS], Connection=[keep-alive], Vary=[Accept-Encoding,User-Agent], Date=[Fri, 10 May 2019 03:00:35 GMT], Location=[https://www.naver.com/index.html], Content-Type=[text/html]}
getIfModifiedSince():0
getLastModified():0
getReadTimeout():0
getURL():http://www.naver.com:80/index.html
getUseCaches():true
```

+ NetworkEx4.java
```java
package com.jica.net;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;

public class NetworkEx4 {
	public static void  main(String args[]) {
		URL url = null;
		BufferedReader input = null;
		//정확한 URL이면 해당 파일내용을 읽어올수 있다.(text문서일때)
		String address = "http://www.hani.co.kr/";
		String line = "";

		try {
			url = new URL(address);
		    input = new BufferedReader(new InputStreamReader(url.openStream()));
			while((line=input.readLine()) !=null) {
				System.out.println(line);
			}
			input.close();
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
}
```

+ NetworkEx5.java
```java
package com.jica.net;

import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.URL;

public class NetworkEx5 {
	public static void  main(String args[]) {
		URL url = null;
		InputStream in = null;
		FileOutputStream out = null;
	    String address = "http://t1.daumcdn.net/news/201905/10/SpoChosun/20190510082017649zoij.jpg";

		int ch = 0;

		try {
			url = new URL(address);
			in = url.openStream();
			out = new FileOutputStream("20190510082017649zoij.jpg");

			while((ch=in.read()) !=-1) {
				out.write(ch);
			}
			in.close();
			out.close();
		} catch(Exception e) {
			e.printStackTrace();
		}
	} // main
}
```

+ TCP/IP를 사용한 채팅프로그램 ==> 소켓 프로그램

+ 소켓(Socket) : 프로세스간에 통신에 사용되는 양쪽 끝단(endpoint)
```
A사람                              B사람
    <----전화통화-------->
       전화기                             전화기   --- 전화번호
-----------------------------
 A컴퓨터                              B컴퓨터
    <------통신---------->  
       소켓                                 소켓   --- IP:PORT
```

+ TCP방식 : 연결지향
  - 1:1, 1:(서버)다수
  - 데이터길이가 자유롭다.
  - 신뢰성을 기반

+ UDP방식 : 비연결지향
  - 1:1, 1:(서버)다수, 다수:다수
  - 패킷단위의 전송-관리

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
