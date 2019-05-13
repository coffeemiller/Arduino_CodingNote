
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

#### 2. Web Program
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



#### 3. 개발환경설치(웹서버)
#### 4. HTML과 Servlet/JSP
#### 5. HTML태그
#### 6. 실습
#### 7. Summary / Close



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
