
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
A사람                     B사람
    <----전화통화-------->
전화기                    전화기   --- 전화번호
-----------------------------
 A컴퓨터                  B컴퓨터
    <------통신---------->  
소켓                       소켓   --- IP:PORT
```

+ TCP방식 : 연결지향
  - 1:1, 1:(서버)다수
  - 데이터길이가 자유롭다.
  - 신뢰성을 기반
  - ---------------------ServerSocket, Socket

+ UDP방식 : 비연결지향
  - 1:1, 1:(서버)다수, 다수:다수
  - 패킷단위의 전송-관리
  - 비신뢰성(반대개념X, 자료손실or순서바뀜이 혹시라도 있을수 있는...)
  - ---------------------DatagramSocket, DatagramPacket


+ 채팅프로그램을 실습해보자
```
    Client                      Server
(1) 접속요청------------------->ServerSocket
(2)(Socket) <----------------> (Socket)
TcpIpClient.java              TcpIpServer.java
```
  - 0) 서버소켓생성
```java
ServerSocket ss = new Socket(7777);
```
  - 0-1) 클라이언트의 접속대기
```java
//서버로 연결요청이 들어오면...
Socket s = ss.accept(); //accept내부에서 응답(Socket::s)
```
  - 1) 소켓생성
```java
Socket s = new Socket("127.0.0.1",7777);
// 여기까지가 "연결확보""
```
```
     Socket::s ===========연결확보==========  Socket::s
dis<-InputStream <------------------------ OutputStream <----- dos
     OuputStream ------------------------> InputStream   
```

  - 2) Socket으로부터 IO stream을 얻어서 데이터를 송수신할 준비를 한다.
```java
DataInputStream dis = new DataInputStream(s.getInputStream()); //Client
DataOutputStream dos = new DataOutputStream(s.getOutputStream()); //Server
```

  - 3) 데이터 송수신
```java
dis.readUTF();  //클라이언트-수신대기
System.out.println(message);

dos.writeUTF("메세지") //서버-송신
```

  - 4) Socket Close = 연결해제
```java
s.close() //클라이언트
s.close() //서버
```


+ TcpIpClient.java
```java
package com.jica.tcp;

import java.io.DataInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ConnectException;
import java.net.Socket;

public class TcpIpClient {
	public static void main(String args[]) {
		try {
			//자신의 컴퓨터를 서버로 사용
			String serverIp = "192.168.20.27";
//			String serverIp = "127.0.0.1";

			System.out.println("서버에 연결중입니다. 서버IP :" + serverIp);
			// 클라이언트에서는 소켓을 생성하는 코드 자체가 서버에게 접속요청을 하는 것이다.
			// 생성자가 종료해서 Socket객체가 만들어지면 서버와 정상연결이 된것이다.

			// 소켓을 생성하여 연결을 요청한다.
			// 이때 서버쪽 프로그램은 이미 실행되어 있어야 한다.
			Socket socket = new Socket(serverIp, 7777);

			//아래의 코드가 실행된다는 것은 정상적으로 서버와 연결이 되었다는 뜻이다.
			// 소켓의 입력스트림을 얻는다.
			InputStream in = socket.getInputStream();
			DataInputStream dis = new DataInputStream(in);

			// 소켓으로 부터 받은 데이터를 출력한다.
			System.out.println("서버로부터 받은 메시지 :"+dis.readUTF());      
			System.out.println("연결을 종료합니다.");

			// 스트림과 소켓을 닫는다.
			dis.close();
			socket.close();
			System.out.println("연결이 종료되었습니다.");
		} catch(ConnectException ce) {
			ce.printStackTrace();
		} catch(IOException ie) {
			ie.printStackTrace();
		} catch(Exception e) {
			e.printStackTrace();  
		}  
	} // main
} // class

```

+ TcpIpServer4.java
```java
package com.jica.tcp;

import java.io.DataOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TcpIpServer4 implements Runnable {
	ServerSocket serverSocket;
	//클라이언트가 접속할때마다 통신을 할수있도록 쓰레드를 만들어서 처리하자
	//쓰레드객체 배열
	Thread[] threadArr;

	public static void main(String args[]) {
		// 5개의 쓰레드를 생성하는 서버를 생성한다.
		TcpIpServer4 server = new TcpIpServer4(5);
		server.start();
	} // main

	public TcpIpServer4(int num) {
		try {
			// 서버소켓을 생성하여 7777번 포트와 결합(bind)시킨다.
			serverSocket = new ServerSocket(7777);

			System.out.println(getTime()+"서버가 준비되었습니다.");

			//미리 쓰레드객체를 5개 준비한다. 단, 쓰레드는 아직 생성되지 않았다.
			threadArr = new Thread[num];
		} catch(IOException e) {
			e.printStackTrace();
		}
	}

	public void start() {
		for(int i=0; i < threadArr.length; i++) {
			threadArr[i] = new Thread(this);
			threadArr[i].start();
		}
	}

	public void run() {
		while(true) {
			try {
				//현재 작동하는 쓰레드를 출력해보자
				Thread curThread = Thread.currentThread();
				System.out.println(curThread+" "+getTime()+ "가 연결요청을 기다립니다.");

				Socket socket = serverSocket.accept();
				System.out.println(getTime()+ socket.getInetAddress() + "로부터 연결요청이 들어왔습니다.");

				// 소켓의 출력스트림을 얻는다.
				OutputStream out = socket.getOutputStream();
				DataOutputStream dos = new DataOutputStream(out);

				// 원격 소켓(remote socket)에 데이터를 보낸다.
				dos.writeUTF("[Notice] Test Message1 from Server.");
				System.out.println(getTime()+"데이터를 전송했습니다.");

				// 스트림과 소켓을 닫아준다.
				dos.close();
				socket.close();
		    } catch (IOException e) {
				e.printStackTrace();
			}
		} // while
	} // run

	// 현재시간을 문자열로 반환하는 함수
	static String getTime() {
		String name = Thread.currentThread().getName();
		SimpleDateFormat f = new SimpleDateFormat("[hh:mm:ss]");

		return f.format(new Date()) + name ;
	}
} // class

```
##### [1:1 Chat]
```
-------------------1대 1 채팅 프로그램 -------------------------------
client                                    Server

Socket s = new Socket("192.168.20.7",7777);
           ---------------------------------> Socket s = ss.accept();       
====================================================================
Socket::s                                Socket::s
|---- InputStream <--------------------------OutputStream <----|
| |-->OuputStream -------------------------->InputStream ----| |
| |                                                          | |
| |---Sender쓰레드                          Receiver쓰레드<---| |
|---> Receiver쓰레드                       Sender쓰레드 --------|
```
+ TcpIpClient5.java
```java
package com.jica.tcp;

import java.io.IOException;
import java.net.ConnectException;
import java.net.Socket;

public class TcpIpClient5 {
	public static void main(String args[]) {
		try {
			String serverIp = "192.168.20.27";
//			String serverIp = "127.0.0.1";

			// 소켓을 생성하여 연결을 요청한다.
			Socket socket = new Socket(serverIp, 7777);

			System.out.println("서버에 연결되었습니다.");
			//서버로 메세지를 보내는 기능 즉, 키보드로 채팅메세지를 입력받아 전송하는 쓰레드
			Sender sender = new Sender(socket);
			//언제 상대방으로부터 메세지가 올지 모르므로 메세지를 대기하고 있는 수신 쓰레드
			Receiver receiver = new Receiver(socket);

			sender.start();
			receiver.start();
		} catch(ConnectException ce) {
			ce.printStackTrace();
		} catch(IOException ie) {  
			ie.printStackTrace();
		} catch(Exception e) {
			e.printStackTrace();  
		}  
	} // main
} // class
```

+ TcpIpServer5.java
```java
package com.jica.tcp;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class TcpIpServer5 {
	public static void main(String args[]) {
		ServerSocket serverSocket = null;
		Socket socket = null;

		try {
			// 서버소켓을 생성하여 7777번 포트와 결합(bind)시킨다.
			serverSocket = new ServerSocket(7777);
			System.out.println("서버가 준비되었습니다.");

			socket = serverSocket.accept();

			Sender   sender   = new Sender(socket);
			Receiver receiver = new Receiver(socket);

			sender.start();
			receiver.start();
		} catch (Exception e) {
			e.printStackTrace();
		}
	} // main
} // class

class Sender extends Thread {
	Socket socket;
	DataOutputStream out;  //출력스트림
	String name;

	Sender(Socket socket) {
		this.socket = socket;
		try {
			out = new DataOutputStream(socket.getOutputStream());
			name = "["+socket.getInetAddress()+":"+socket.getPort()+"]";
		} catch(Exception e) {}
	}

	public void run() {
		//키보드로 입력받아 상대편에게 전송
		Scanner scanner = new Scanner(System.in);
		while(out!=null) {
			try {
				out.writeUTF(name+scanner.nextLine());		
			} catch(IOException e) {}
		}
	} // run()
}

class Receiver extends Thread {
	Socket socket;
	DataInputStream in;

	Receiver(Socket socket) {
		this.socket = socket;
		try {
			in = new DataInputStream(socket.getInputStream());
		} catch(IOException e) {}

	}

	public void run() {
		while(in!=null) {
			try {
				System.out.println(in.readUTF());
			} catch(IOException e) {}
		}
	} // run
}
```


##### [Multi Chat]
```
-------------------다 대 대 채팅프로그램 ----------------------------------------
client                                    Server
                                         ServerSocket ss = new ServerSocket(8000);
      A                                  while(true){
Socket s = new Socket(ip,8000) -------->Socket s = ss.accept();
                                    |        s로 Thread만든다.
                                    |        쓰레드 시작
      B                             |        쓰레드 정보저장
Socket s = new Socket(ip,8000)------|
                                    |     }
      C                             |
Socket s = new Socket(ip,8000)------|
```

+ TcpIpMultichatClient.java
```java
package com.jica.tcpchat;

import java.net.*;
import java.io.*;
import java.util.Scanner;

public class TcpIpMultichatClient {
	public static void main(String args[]) {

		try {
      // 채팅서버에 접속
  		String serverIp = "192.168.20.27";  //강사컴
      //String serverIp = "127.0.0.1";
      // 소켓을 생성하여 연결을 요청한다.
			Socket socket = new Socket(serverIp, 8888);

			System.out.println("서버에 연결되었습니다.");
			Thread sender   = new Thread(new ClientSender(socket, "강사"));
			Thread receiver = new Thread(new ClientReceiver(socket));

			sender.start();
			receiver.start();
		} catch(ConnectException ce) {
			ce.printStackTrace();
		} catch(Exception e) {}
	} // main

	static class ClientSender extends Thread {
		Socket socket;
		DataOutputStream out;
		String name;

		ClientSender(Socket socket, String name) {
			this.socket = socket;
			try {
				out = new DataOutputStream(socket.getOutputStream());
				this.name = name;
			} catch(Exception e) {}
		}

		public void run() {
			Scanner scanner = new Scanner(System.in);
			try {
				if(out!=null) {
					out.writeUTF(name); // 최초 접속시 접속자 명을 전송한다.
				}

				while(out!=null) {
					out.writeUTF("["+name+"]"+scanner.nextLine());					}
			} catch(IOException e) {}
		} // run()
	} // ClientSender

	static class ClientReceiver extends Thread {
		Socket socket;
		DataInputStream in;

		ClientReceiver(Socket socket) {
			this.socket = socket;
			try {
				in = new DataInputStream(socket.getInputStream());
			} catch(IOException e) {}
		}

		public void run() {
			while(in!=null) {
				try {
					System.out.println(in.readUTF());
				} catch(IOException e) {}
			}
		} // run
	} // ClientReceiver
} // class

```

+ TcpIpMultichatServer.java
```java
package com.jica.tcpchat;

import java.net.*;
import java.io.*;
import java.util.*;

public class TcpIpMultichatServer {
	//현재 접속된 클라이언트의 정보 (OutputStream 저장)
	HashMap clients;

	TcpIpMultichatServer() {
		clients = new HashMap();
		//동기화 코드
		Collections.synchronizedMap(clients);
	}

	public void start() {
		ServerSocket serverSocket = null;
		Socket socket = null;

		try {
			serverSocket = new ServerSocket(8888);
			System.out.println("서버가 시작되었습니다.");

			while(true) {
				socket = serverSocket.accept();
				System.out.println("["+socket.getInetAddress()+":"+socket.getPort()+"]"+"에서 접속하였습니다.");

				//접속자가 서버로 전송하는 데이타를 읽는 수신 쓰레드를 만든다.
				ServerReceiver thread = new ServerReceiver(socket);
				thread.start();
			}
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
	//                        Socket을가진 쓰레드
	//                      |-->[[홍길동,out],[장길산,out],[임꺽정,out]]  
  //              clients |
	//tms 0x100 ---------[0x200]
	void sendToAll(String msg) {
		Iterator it = clients.keySet().iterator();

		while(it.hasNext()) {
			try {
				DataOutputStream out = (DataOutputStream)clients.get(it.next());
				out.writeUTF(msg);
			} catch(IOException e){}
		} // while
	} // sendToAll

	public static void main(String args[]) {
		//	new TcpIpMultichatServer().start();
		TcpIpMultichatServer tms = new TcpIpMultichatServer();
		tms.start();
	}

	class ServerReceiver extends Thread {
		Socket socket;
		DataInputStream in;
		DataOutputStream out;

		ServerReceiver(Socket socket) {
			this.socket = socket;
			try {
				in  = new DataInputStream(socket.getInputStream());
				out = new DataOutputStream(socket.getOutputStream());
			} catch(IOException e) {}
		}

		public void run() {
			String name = "";
			try {
				name = in.readUTF();
				sendToAll("#"+name+"님이 들어오셨습니다.");

				clients.put(name, out);
				System.out.println("현재 서버접속자 수는 "+ clients.size()+"입니다.");

				while(in!=null) {
					sendToAll(in.readUTF());
				}
			} catch(IOException e) {
				// ignore
			} finally {
				sendToAll("#"+name+"님이 나가셨습니다.");
				clients.remove(name);
				System.out.println("["+socket.getInetAddress() +":"+socket.getPort()+"]"+"에서 접속을 종료하였습니다.");
				System.out.println("현재 서버접속자 수는 "+ clients.size()+"입니다.");
			} // try
		} // run
	} // ReceiverThread
} // class
```

#### 3. 실습
#### 4. Summary / Close


-----------------------------------------------------------

### [2019-05-13]

#### 1. Review
+ Java ---> Network(IP,Port,Protocol)
  - Protocol(TCP:전화, UDP:편지, HTTP:)
  - Socket Program(TCP,UDP)
  - Web Program(HTTP)

+ Oracle
+ Web Program
  - 인터넷(www)
  - 정보제공자(웹서버) - Tomcat, Begin,...

#### 2. Web Program / 개발환경설치(웹서버)
##### 웹 프로그램 개발환경 설정
+ 0. 교재에서의 웹환경
```
cmd mode에서 실행        웹서버(Tomcat)
웹브라우저               웹어플리케이션(직접작성)
         ---request--->    html, *.java, *jsp
         <--response----
```

+ 1. 우리의 실습 웹환경
```
cmd mode에서 실행        웹서버(Tomcat)
웹브라우저               웹어플리케이션(직접작성)
         ---request--->    프로젝트 단위 -- html, *.java, *jsp
         <--response----
```
  - 0) Jdk 설치
  - 1) Eclipse EE 설치
  - 2) Tomcat설치  -- 주의)http/1.1 port 8088로 변경했음.
    - 실행방법(우측하단 트레이, cmd mode에서 직접실행)
    - 우리가 사용할 방법(Eclipse EE 내부에 Tomcat서버를 복사하여 내부에서 실행)

    - 환경변수 설정을 권장
      + (1) PATH -- cmd모드의 어느곳에서든 Tomcat8이라고 입력하면 Tomcat이 기동되도록
        - ```C:\tomcat85\bin```
      + (2) CLASS PATH 추가 -- JAVA프로그램 컴파일시 기본 라이브러리뿐만아니라 추가 라이브러리를 자동인식할수 있도록 설정(Java프로그램==>Servlet, JSP)
        - ```C:\tomcat85\lib\servlet-api.jar```
      + (3) TOMCAT_HOME 설정
        - ```C:\tomcat85```
      + (4) CATALINA_HOME 설정
        - ```C:\tomcat85```
      + 위 설정내용을 교제 cmd mode에서 웹어플리케이션을 사용할때 반드시 설정해야 한다. 우리 Eclipse EE 내부에서 톰킷을 사용하므로 설정하지 않아도 무방하다.

  - 3) Eclipse EE내부에서 웹서버로 Tomcat85를 연동시키자.	 - 서버 생성
    + 이클립스 화면구성 - 좌측(window - show view - general - Project Explorer)


+ 웹어플리케이션의 구조(이클립스 내부에서 보여지는 구조)
  - Eclipse EE에서의 프로젝트 구조(File/New/Dynamic Web Project, "html")
  - html <== 웹어플리케이션 명칭
  - html\Java Resources\src\패키지단위의 Servlet코드(`*.java`)
  - html\build\패키지단위로 컴파일된 Servlet코드(`*.class`)
  - html\WebContent\html문서(`*.html`), javaScript코드(`*.js`), JSP코드(`*.jsp`), 기타 다양한 resource(`*.gif, *.jpg, *.avi`)... 필요하다면 하위폴더를 만들어서 저장.
    - 실행시에는 Servlet으로 작동




#### 3. HTML과 Servlet/JSP

+ 실습으로... 가장 기본적인 html문서를 작성해보자.
  - WebContent\jica.html
  - 웹브라우저에서 jica.html이 작동하게된 작업절차
    - 1) 웹브라우저의 주소표시줄 : http://localhost:8088/html/jica.html
      - 웹브라우저가 http프로토콜의 형식(Get방식의 요청정보)을 준수하여 서버로 요청정보를 보낸다.
    - 2) 웹서버(Tomcat8.5)
      - 웹컨테이너 프로그램이 요청정보를 해석하여, 자신이 관리하는 웹어플리케이션의 내부구조에서 요청한 html문서를 찾는다.(정적인 구성요소의 요청)
      - http프로토콜 형식에 맞추어 응답정보에 해당 파일 내용도 담아서 응답한다.
    - 3) 웹브라우저는 응답내용중 html문서내용을 추출하여 그 내용을 해석(메모리에 DOM을 만들어)하여 태그형식에 맞추어 정보를 보여준다.

    - HTML문서가 파일의 형태로 저장되어 있다면... 정적인 웹컴퍼넌트다. 즉, 항상 동일한 내용만 응답해준다.
    - 그래서 동적으로 결과를 만들어내기 위해, Servlet과 JSP를 사용한다.
    - 클라이언트(웹브라우저)의 요청이 왔을때, 웹컨테이너가 정보를 해석하여 동적인 웹컴퍼넌트가 실행되어 HTML형식을 만들어 응답한다.


+ Servlet 실습으로... 1부터 100까지 합계를 구하는 서블릿
```
일반 Logic은 Java프로그램으로 작성하고 프로그램 실행결과를 html 태그로 작성한다.
```
  - Eclipse EE 내부에서의 Project구조와 탐색기의 구조, 실제 Tomcat웹서버에서 인식하는 Project구조는 다른다.
```
html/WEB-INF/web.xml  <== 현재 Web Application의 모든 구성요소
            /classes/패키지단위의 컴파일된 서블릿코드(*.class)
            /lib/서블릿에서 사용하는 외부라이브러리파일(*.jar)
    /html파일, 기타 다양한 resource(*.gif, *.wav, *.jpg,...)
    jsp파일
```

  - 서블릿의 구성 => Java코드가 중심이고 + html코드는 결과를 만들어내기위한 보조벅인 작업

  - 서블릿 객체는 클라이언트의 최초 요청시 Tomcat웹서버에 의해 생성되고, 이후부터의 요청은 쓰레드 처리에 의해 doGet()/doPost()의 메서드가 실행되어 그 결과값이 응답된다.

  - html 호출시는 http://localhost:8088/html/jica.html
  - 서블릿 호출시는 http://localhost:8088/html/HundredServlet
  - jsp 호출시는 http://localhost:8088/html/Hundred.jsp


  - JSP(Java Server Page) ==> HTML태그가 중심이고 + 필요할때 Java코드를 호출하여 사용.
  - jsp(Hundred.jsp)코드도 실행될때는 웹서버에 의한 Servlet(Hundred_jsp.java --> Hundred_jsp.class)으로 변환되고 변환된 JSP에서는 다음의 구성요소를 자유롭게 사용한다.
    - 1) html 태그
    - 2) jsp 태그 및 추가 라이브러리
    - 3) java코드를 호출하여 사용
    - 4) EL 및 JSTL등의 외부라이브러리



##### [오늘의 과제]
+ 1) 실습내용 이해
+ 2) 수업참고자료 "서블릿_JSP개요.ppt" 읽어볼것
+ 3) 교재1장 읽어볼것


#### 4. 실습
#### 5. Summary / Close



-----------------------------------------------------------

### [2019-05-14]

#### 1. Review
```
Network - Socket(TCP,UDP)
        - Web Program(HTTP)
```
+ client(사용자)
  1) HTML문서를 해석/표현
  2) 사용자에반응 -> 자체(javaScript)
  3) 서버에 요청

+ server(정보제공자)
  1) 웹사이트 - 웹어플리케이션
  2) html, jpg, JSP, PHP...


+ 개발환경설치
  1. Eclipse EE - 웹 어플리케이션
  2. 웹서버에서 동작 - Tomcat8.5

```
웹어플리케이션명 /html문서, 기타 resource
                /WZB-inf/web.xml
                /classes/패키지단위
                /lib/*.jar
```

+ 향후 웹프로그램 학습순서
  1) HTML 태그
  2) JavaScript
  3) Servlet
  4) JSP
    html태그, JavaScript, JSP태그, [자바코드]
    자바코드를 전혀 사용하지 않고 작성(외부 라이브러리 사용...EL, JSTL)
```
  MVC패턴적용(모델1)   
  -----request--------->서블릿작동(Logic처리)
                           |
                           |
                           V
 <-----response----------  JSP(Presentation-View처리)
```

#### 2. HTML(HyperText Markup Language)태그
+ HTML의 정의와 특징
  - 모든 월드 와이드 웹 문서(웹브라우저)를 작성하는데 쓰이는 표준 파일 형식
  - 파일 확장자는 `*.htm` 또는 `*.html`
  - 웹 브라우저는 확장자에 의해 HTML문서로 인식
  - HTML은 대소문자 구분을 하지 않음
  - 태그로 구성되어 있다.
    - 기본형식 : <태그명 속성=값,...>   </태그명>
    - 기본형식 : <태그명 속성=값,.../>
![HTML의 동작원리](html.jpg "HTML의 동작원리")


+ 태그?
  - HTML 문서의 모양과 행동 양식을 정해주는 하나의 명령어
  - 시작 태그;<>와 종료 태그;</>가 짝을 이뤄 사용되는 것이 일반적임(짝없는것도!)
  - 태그들은 서로 중복 사용될 수 없다
  - 태그의 속성값을 기술할때 ""을 사용할수도 있고 안할수도 있다.
  - 특정 리소스(이미지,음원)를 접근할때는 상대경로와 절대경로로 접근할수 있다.
```html
<html>
  <head>
      <title>Document</title>
  </head>
  <body>

  </body>
</html>
```


#### 3. 기본태그

+ first.html
```html
<!DOCTYPE HTML>
<html>
 <head>
  <title> 기본 태그 연습 </title>
 </head>

 <body>
  안녕하세요.<br><br><br><br>
  처음으로 HTML을 학습합니다.<br>
  전주정보문화산업진흥원의 스마트폰 개발자 과정! 화 이   팅!<br><br>
  화 이 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;팅!<br><br>
  지금까지는 기본 태그만 사용하여 본문을 작성하고 있습니다.<br>
  화일 저장시 확장자는 *.html, *.htm으로 저장해야 합니다.<br>
  <!-- 주석표시 입니다. <br> 태그는 줄바꿈기능을 수행합니다. -->
  위의 내용은 주석표시입니다.
  <p>
  열심히 공부합시다.<br>
  <!-- <P> 태그는 문단의 단락을 표현하는 태그입니다. -->
  </p>
  HTML과 JavaScript는 웹프로그램의 기본구성요소입니다.
 </body>
</html>
```



#### 4. Link iamge 태그

+ second.html
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> HTML 기본태그 연습 </title>
  <meta name="Generator" content="EditPlus">
  <meta name="Author" content="argus">
  <meta name="Keywords" content="HTML 기본태그">
  <meta name="Description" content="HTML 기본태그 학습">
  <script>
  	//javaScript문법에 맞는 표현을 사용하여 코딩
  	//인터프리터 언어(변수,연산자,제어문,다양한 형태의 객체, 함수-function)
  </script>
  <style>
  	<!-- 본문에서 사용되는 다양한 태그들의 속성값을 정의해 놓고 편리하게 사용 -->  	
  </style>

  <!-- HEAD 태그에 기술되는 내용
    0) 문서 제목 - TITLE 태그
	1) 문서의 정보를 검색엔진에게 제공 - META 태그
	2) JavaScript 코드 : 이곳에 작성하기를 권장
	3) css : 문서의 스타일 쉬트
  -->
 </head>

<!--
	상대경로 : 현재위치를 기준으로 위치 지정  "bg.gif"
	절대경로 : 웹어플리케이션을 기준으로 경로지정  "./image/bg.gif"
													"/html/image/bg.gif"
													"http://localhost:8088/html/image/bg.gif"													
 -->
 <body BGCOLOR="00FF00" TEXT="#FF0000" BACKGROUND="./image/bg.gif">
  <!-- BODY 태그에 기술되는 내용
    다양한 종류의 태그를 사용하여 정보를 표현한다.
 *  0) 글자 및 이미지 태그
	2) 문장 장식 태그, 움직이는 글자
	3) 테이블 태그
 *  4) 링크 태그
	5) 프래임 태그(영역구분) -- <div> 태그 사용을 권장
 *  6) 사용자와 상호작용하는 태그(정보의 입력, 선택) -- <form>
  -->
  <img src="./img/frog.jpg" width="200" height="200" border="1" alt="풍선도움말"/>
  <BR>
	아래의 참고사이트는 여러분들이 학습할때 방문하여 정보를 얻으세요.<br>
	아주 유용한 사이트입니다.<br>
  <h2>참고사이트</h2>
  <a href="http://www.trio.co.kr">트리오 사이트</a><br>
  <a href="http://www.w3schools.com">W3 학습 사이트</a><br>
 </body>
</html>
```


+ 상대경로
  - 현재경로에 같은 파일(이미지) : `*.jpg`
+ 절대경로
  - ./ : 현재경로
  - ../ : 부모경로



+ 글자태그
  - <h1> ~ </h>
  - <font color="rgb값이 색상문자열"> 글자 </font>
  - argb : Alpha(투명도:0~255, 0 완전투명, 255 완전불투명)
  - 삼원색(Red,Green,Blue)
  - <b>
  - <i>
  - <sup>
  - <sub>



+ third.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> 글자 태그 </title>
 </head>

 <body>
  <!-- 글자의 크기를 지정하는 헤드라인 태그 -->
  <h1>문서의 제목 1</h1>
  <h2>문서의 제목 1</h2>
  <h3>문서의 제목 1</h3>
  <h4>문서의 제목 1</h4>
  <h5>문서의 제목 1</h5>
  <h6>문서의 제목 1</h6>

  <p> FONT 태그 연습 -- 글자의 속성(글꼴, 크기, 색상등)	</p>

  전주정보문화산업진흥원 JICA jica 보통글씨<br>
  <hr>
  <font COLOR="FF0000" >전주정보문화산업진흥원 JICA jica</font><br>
  <font COLOR="CCCCCC" >전주정보문화산업진흥원 JICA jica</font><br>
  <font COLOR="GOLD" >전주정보문화산업진흥원 JICA jica</font><br>
  <font COLOR="BLUE" >전주정보문화산업진흥원 JICA jica</font><br>

  <hr>
  <font SIZE="1">전주정보문화산업진흥원 JICA jica</font><br>
  <font SIZE="3">전주정보문화산업진흥원 JICA jica</font><br>
  <font SIZE="5">전주정보문화산업진흥원 JICA jica</font><br>
  <font SIZE="7">전주정보문화산업진흥원 JICA jica</font><br>

  <hr>
  <font FACE="궁서">전주정보문화산업진흥원 JICA jica</font><br>
  <font FACE="바탕">전주정보문화산업진흥원 JICA jica</font><br>
  <font FACE="Comic Sans MS">전주정보문화산업진흥원 JICA jica</font><br>
  <font FACE="Impact">전주정보문화산업진흥원 JICA jica</font><br>

  <!-- 수평선 긋기 -->
  <hr>
  수평으로(Horizental)으로 선을 긋는다.
  <hr width="50%" align=left>
  <hr width="50">
  <hr width="50%" size=4 color=silver>
  <hr width="50%" size=1 align=right>

  <!-- 특수기능을 가진 특수문자 -->  
  &lt; 혹은 &#60; : (<)의 문자기호 <br>
  &gt; 혹은 &#62; : (>)의 문자기호 <br>
  &amp; 혹은 &#38; : (&)의 문자기호 <br>
  &nbsp; 혹은 &#160; : ( ) 공백의 문자기호<br>
  &copy; 혹은 &#169; : () 저작권의 문자기호<br>

  &int; 혹은 &#8747; : 인터그랄<br>
  &sum; 혹은 &#8721; : 합계기호<br>
  &Aring; 혹은 &#197; :
 </body>
</html>

```


+ forth.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> 글자 장식태그 </title>
 </head>

 <body>
  스타일 태그 - 장식 태그<br>
  <font SIZE="4" COLOR="#000000">전주정보문화산업진흥원!</font><br>
  <b>안드로이드 개발자반 화이팅!</b><br>
  <i>전주(Jeonju)</i><br>
  <s>정보문화(취소선)</s><br>
  <u>산업진흥원</u><br>
  x<sup>2</sup> 1og<sub>10</sub> 윗첨자와 아래첨자<br>
  <tt>스마트폰(smart phone)</tt><br>
  <small>스마트폰</small><br>
  <big>스마트폰</big><br>

  <hr>
  <pre>
  html문서내용에서는 기본으로 공백,탭(1번만 적용)하고 엔터기호는 무시된다.
  pre태그는 공백,탭,엔터기호를 그대로 인식하도록 표현하는 태그이다.
  HTML 태그를 주마간산 형식으로 살펴보고 있   습   니  다.   쭉~

             안녕!
  복습한다 생각하고 살펴봅시다.
  </pre>
  <hr>
  점심시간!<br>
  <center>내용을 수평 중앙에 포시합니다.  속성은 없습니다.</center>
  단, center태그는 HTML5에서 권장하지 않는다. 이제 식사하러 갑시다<br>

  <center> <h4>중앙 위치</h4> </center>

  <hr>
  <hr width="70%" align="left" size="1" >
  <hr width="70%" align="center" size="2" >
  <hr width="70%" align="right" size="3" >
  <hr width="70%" align="right" size="4" >
 </body>
</html>

```


+ 목록태그 : 여러개의 항목을 보여주는 태크

+ list.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> 목록(리스트) 태그 사용법 </title>
 </head>

 <body>
	<marquee WIDTH="200" SCROLLAMOUT="10" BEHAVIOR="ALTERNATE" >목록 태그 사용법</marquee><br>

	<pre>
	리스트 태그는 상위와 하위가 있는 체계화된 목록을 만들때 사용되며
	그 종류는 다음과 같다.
		    1. 정의목록(DL:definition List),
	        2. 번호가 없는 목록(UL:unordered List),
			3. 번호가 있는 목록(OL:ordered List)의 세가지가 있다.

    정의목록(DL) 태그는 그내용으로 다음 두가지 요소(Element)를 갖는다.
	    1. 정의목록 제목(DT:definition title).
		2. 정의목록 데이타(DD:definition data).

    번호가 없는 목록(UL) 태그는 그내용으로 갖는 Element는
	    LI(List Item)을 사용한다.
		UL혹은 LI모두 type 속성을 이용하여 그 모양을 설정한다.
		type속성은(disc:꽉찬 원, circle:테두리 원, square:네모모양)

    번호가 있는 목록(OL) 태그도 마찬가지로 LI을 사용한다.
	    OL, LI 모두 type속성을 사용한다.
		typ속성은("a", "A", "i", "I")
	</pre>
	<hr>

	<dl>
	<dt>언어 수업내용
		<dd> JAVA Language
		<dd> Web Programming
    <dt>취업분야
		<dd> 기본 Web 프로그램 작성분야
		<dd> 안드로이드 APP 개발분야
	</dl>

	<hr>
	<ul type="disc" >
		<li> Review
		<li> HTML 개요
		<li> 태그작성 연습1
		     <ul type="circle">
			     <li> 글자와 문자태그
				 <li> 이미지 태그
			 </ul>
		<li> 태그작성 연습2
			 <ul type="square">
				 <li> 테이블 태그
				 <li> form 태그
			 </ul>
	</ul>
	<hr>

	<ol type="A">
		<li> JAVA언어 학습
		<li> Oracle
		<li> WEB Programming
			<ol type="I">
				<li> HTML
				<li> JavaScript
				<li> Servlet
				<li> JSP
				<li> Framework
			</ol>
        <li> Android
		    <ol type="a" start="3">
				<li> 기본 Widget 사용법
				<li> 고급 Widget 사용법
			</ol>
	</ol>
 </body>
</html>
```

#### 5. 테이블 태그
#### 6. FORM 태그
#### 7. 실습
#### 8. Summary / Close



-----------------------------------------------------------

### [2019-05-15]

#### 1. Review

#### 4. 실습
#### 5. Summary / Close
