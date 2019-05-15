
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

+ [first.html](./first.html)
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

+ [second.html](./second.html)
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
```
  - 현재경로에 같은 파일(이미지) : `*.jpg`
```

+ 절대경로
```
  - ./ : 현재경로
  - ../ : 부모경로
```


+ 글자태그
```
  - <h1> ~ </h>
  - <font color="rgb값이 색상문자열"> 글자 </font>
  - argb : Alpha(투명도:0~255, 0 완전투명, 255 완전불투명)
  - 삼원색(Red,Green,Blue)
  - <b>
  - <i>
  - <sup>
  - <sub>
```


+ [third.html](third.html)
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


+ [forth.html](./forth.html)
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

+ [list.html](./list.html)
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



+ 이미지 태그

+ [image.html](./image.html)
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
<HEAD>
<TITLE> 이미지 태그 연급 </TITLE>
</HEAD>

<BODY>
<PRE>
	IMG태그 : "<IMG SRC="" WIDTH="" HEIGHT="" BORDER="0" ALT="">"
	SRC = 이미지 화일명과 경로: URL 설명을 참조하라.
	WIDTH = LENGTH: 이미지의 너비를 픽셀로 지정한다.
	HEIGHT = LENGTH: 이미지의 높이를 픽셀로 지정한다.
	BORDER = PIXELS: 테두리(BORDER)의 폭을 픽셀(PIXEL)로 지정한다.
	ALT = TEXT 이미지을 디스플레이 할 수 없는 사용도구에서,
		이 애트리뷰트는 설명을 제공하는 대체 텍스트를 지정한다.
		또한 마우스를 올려 놓을때 대체 텍스트가 나타난다.
	ALIGN = "LEFT","RIGHT","TOP","MIDDLE","BOTTOM"
</PRE>
<HR>
	<ul>
		<li> 현재 html문서내의 폴더에 파일이 있다. 파일명만 기술</li>
		<li> 현재 html문서의 하위 폴더에 있다.     폴더/파일명</li>
		<li> 현재 html문서의 상위폴더를 지칭할때 ../하위폴더/파일명</li>
	</ul>
	<pre>
		/home/image/log.gif
			 /doc/index.html
			 /index.html
	</pre>
<hr>
<IMG SRC = "./img/dog1.gif" /><br> <!-- 상대경로 -->
<IMG SRC = "./img/dog1.gif" Width="192" height="192" align="left">내용이 없다면
<HR>
<br>
<br>
<P>
<P>

<IMG SRC = "./img/dog2.gif" WIDTH = "120" HEIGHT = "100" ALIGN ="RIGHT" /><BR>
진돗개2<BR>
이미지를 보여주고 텍스트를 기술합니다.<br>
내용이 많으면 어떻게<BR>
기술될까요<BR>
<P>
<HR>

<P>
<IMG SRC = "./img/dog4.gif" WIDTH = "120" HEIGHT = "100" ALIGN="LEFT">진돗개43<BR>
진돗개3<BR>
이미지를 보여주고 텍스트를 기술합니다.<BR>
내용이 많으면 어떻게<BR>
기술될까요<BR>
</P>
<HR>
<CENTER><IMG SRC = "./img/dog3.gif" WIDTH = "120" HEIGHT = "100" ALT="난 삽살개야! 멍~" >삽살개<BR></CENTER>
<HR>
<IMG SRC = "./img/dog4.gif" WIDTH = "120" HEIGHT = "100" border="1" ALIGN="TOP">진돗개4<BR><BR>
<HR>
<IMG SRC = "./img/dog5.gif" WIDTH = "120" HEIGHT = "100" border="3" ALIGN="MIDDLE">진돗개5<BR><BR>
<HR>
<IMG SRC = "./img/dog6.gif" WIDTH = "120" HEIGHT = "100" ALIGN="BOTTOM">진돗개6<BR><BR>
<HR>
<FONT SIZE = "5">bibi1004의 도트캐릭터</FONT><BR><BR>
<!-- 외부사이트의 이미지를 가져 올수도 있다-->
<IMG SRC = "https://www.sports-g.com/wp-content/uploads/2018/12/%EC%86%90%ED%9D%A5%EB%AF%BC-%ED%86%A0%ED%8A%B8%EB%84%98-EPL-696x976.jpg"  ALT = "도트"  BORDER = "2" ALIGN="LEFT">

</BODY>
</HTML>
```



#### 5. 테이블 태그
+ CELLPADDING 하나의 셀 내용값이 경계와의 거리(위,왼쪽,오른쪽,아래)
+ CELLSPACING 셀과 셀끼리의 간격

+ [table1.html](./table1.html)
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 TRANSITIONAL//EN">
<HTML>
<HEAD>
<TITLE> 테이블 만들기 연습 </TITLE>

</HEAD>

<BODY>

<TABLE BORDER = "1" WIDTH = "70%">
	<TR><TH>제목1</TH><TH>제목2</TH></TR>
	<TR><TD></TD><TD>셀2</TD></TR>
	<TR><TD>&nbsp;</TD><TD>셀4</TD></TR>
</TABLE>
<BR>

BORDER: 테두리의 두께
<TABLE WIDTH = "300" BORDER="4" ALIGN = "CENTER">
<CAPTION>테이블의 구성 </CAPTION>
<TR>
	<TH>제목1</TH><TH>제목2</TH><TH>제목3</TH>
</TR>
<TR ALIGN = "CENTER">
	<TD>1번</TD><TD>2번</TD><TD>3번</TD>
</TR>
<TR>
	<TD>4번</TD><TD>5번</TD><TD>6번</TD>
</TR>
</TABLE>
<BR>

<P>&LT;TABLE <FONT COLOR=BLUE><B>BORDER=10</B></FONT> CELLPADDING=0 CELLSPACING=0 BGCOLOR=FFBBBB WIDTH=80%&GT;</P>
<TABLE BORDER=10 CELLPADDING=0 CELLSPACING=0 BGCOLOR=FFBBBB WIDTH=80%>
<TR BGCOLOR=FFFF00>
	<TD>1번 줄 - 1번 칸</TD>
	<TD>1번 줄 - 2번 칸</TD>
	<TD>1번 줄 - 3번 칸</TD>
</TR>
<TR BGCOLOR=FF0000>
	<TD>2번 줄 - 1번 칸</TD>
	<TD>2번 줄 - 2번 칸</TD>
	<TD>2번 줄 - 3번 칸</TD>
</TR>
<TR>
	<TD>3번 줄 - 1번 칸</TD>
	<TD>3번 줄 - 2번 칸</TD>
	<TD>3번 줄 - 3번 칸</TD>
</TR>
<TR>
	<TD>4번 줄 - 1번 칸</TD>
	<TD>4번 줄 - 2번 칸</TD>
	<TD>4번 줄 - 3번 칸</TD>
</TR>
</TABLE>

<P><B>BORDER</B>: 테두리의 두께</P><BR>

<P>&LT;TABLE BORDER=1 <FONT COLOR=BLUE><B>CELLPADDING=10</B></FONT> CELLSPACING=0 BGCOLOR=FFBBBB WIDTH=80%&GT;</P>
<TABLE BORDER=1 CELLPADDING=10 CELLSPACING=0 BGCOLOR=FFBBBB WIDTH=80%>
<TR BGCOLOR=FFFF00>
	<TD BGCOLOR=0000FF>1번 줄 - 1번 칸</TD>
	<TD BGCOLOR=0000FF>1번 줄 - 2번 칸</TD>
	<TD >1번 줄 - 3번 칸</TD>
</TR>
<TR BGCOLOR=FFFF00>
	<TD>2번 줄 - 1번 칸</TD>
	<TD>2번 줄 - 2번 칸</TD>
	<TD>2번 줄 - 3번 칸</TD>
</TR>
</TABLE>

<P><B>CELLPADDING</B>: 테두리와 내용(글자)와의 거리
</P><BR>

<P>&LT;TABLE BORDER=1 CELLPADDING=0 <FONT COLOR=BLUE><B>CELLSPACING=10</B></FONT> BGCOLOR=FFBBBB WIDTH=80%&GT;</P>
<TABLE BORDER=1 CELLPADDING=0 CELLSPACING=10 BGCOLOR=FFBBBB WIDTH=80%>
<TR BGCOLOR=FFFF00>
	<TD>1번 줄 - 1번 칸</TD>
	<TD>1번 줄 - 2번 칸</TD>
	<TD>1번 줄 - 3번 칸</TD>
</TR>
<TR BGCOLOR=FFFF00>
	<TD>2번 줄 - 1번 칸</TD>
	<TD>2번 줄 - 2번 칸</TD>
	<TD>2번 줄 - 3번 칸</TD>
</TR>
</TABLE>
<P><B>CELLSPACING</B>: 칸과 칸의 공간 거리</P><BR>

<P>&LT;TABLE BORDER=10 <FONT COLOR=BLUE><B>CELLPADDING=10 CELLSPACING=10</B></FONT> BGCOLOR=FFBBBB WIDTH=80%&GT;</P>
<TABLE BORDER=10 CELLPADDING=10 CELLSPACING=10 BGCOLOR=FFBBBB WIDTH=80%>
<TR BGCOLOR=FFFF00>
	<TD>1번 줄 - 1번 칸</TD>
	<TD>1번 줄 - 2번 칸</TD>
	<TD>1번 줄 - 3번 칸</TD>
</TR>
<TR BGCOLOR=FFFF00>
	<TD>2번 줄 - 1번 칸</TD>
	<TD>2번 줄 - 2번 칸</TD>
	<TD>2번 줄 - 3번 칸</TD>
</TR>
</TABLE><BR>

<TABLE CELLPADDING = "20" BGCOLOR ="#FFFFAA" BORDER = "1" BORDERCOLORLIGHT = "#FF2400" BORDERCOLORDARK= "#660033">
<TR>
	<TD>1번</TD><TD>2번</TD><TD>3번</TD>
</TR>
</TABLE>
<BR>

<TABLE CELLSPACING = "20" BACKGROUND = "./image/bg.gif" BORDER = "1" BORDERCOLORLIGHT = "#FF2400" BORDERCOLORDARK= "#660033">
<TR>
	<TD>1번</TD><TD>2번</TD><TD>3번</TD>
</TR>
</TABLE>

</BODY>
</HTML>
```


```
필요에 따라 행(줄) <TR>
           열(칸) <TD> 을 병합할 수 있다.
```

+ [table2.html](table2.html)
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
<HEAD>
<TITLE> 테이블 만들기 2 </TITLE>
</HEAD>

<BODY>
<TABLE BORDER="1" WIDTH="300">
    <TR>
		<TH>제목1</TH>
		<TH>제목2</TH>
    </TR>
    <TR>
		<TD COLSPAN="2" BGCOLOR="GRAY">COLSPAN="2"</TD>
    </TR>
    <TR>
		<TD ROWSPAN="2" BGCOLOR="GREEN">ROWSPAN="2"</TD>
		<TD>셀4</TD>
    </TR>
    <TR>
		<TD>셀6</TD>
    </TR>
</TABLE>

<P>
<TABLE BORDER="1" CELLSPACING="10" WIDTH="300">
    <TR>
		<TD>셀1</TD>
		<TD>셀2</TD>
    </TR>
    <TR>
		<TD>셀3</TD>
		<TD>셀4</TD>
    </TR>
</TABLE>
</P>

<P>
<TABLE BORDER="1" CELLSPACING="5" CELLPADDING="10" WIDTH="300">
    <TR>
		<TD>셀1</TD>
		<TD>셀2</TD>
    </TR>
    <TR>
		<TD>셀3</TD>
		<TD>셀4</TD>
    </TR>
</TABLE>
</P>

<P>
<TABLE BORDER="1" WIDTH="300" HEIGHT="50%">
    <TR>
	<TD>셀1</TD>
	<TD>셀2</TD>
    </TR>
</TABLE>

<TABLE BORDER="1" WIDTH="50%" HEIGHT="100">
    <TR>
	<TD>셀1</TD>
	<TD>셀2</TD>
    </TR>
</TABLE>
</P>

<P>
<TABLE BORDER="1" WIDTH="300">
    <TR>
		<TD ALIGN="LEFT">셀1(LEFT)</TD>
    </TR>
    <TR>
		<TD ALIGN="CENTER">셀2(CENTER)</TD>
    </TR>
    <TR>
		<TD ALIGN="RIGHT">셀3(RIGHT)</TD>
    </TR>
</TABLE>
</P>

<P>
<TABLE BORDER="1" WIDTH="300" HEIGHT="300">
    <TR>
		<TD VALIGN="TOP">셀1(TOP)</TD>
		<TD VALIGN="MIDDLE">셀2(MIDDLE)</TD>
		<TD VALIGN="BOTTOM">셀3(BOTTOM)</TD>
    </TR>
</TABLE>
</P>

<P>
<TABLE BORDER="1" WIDTH="300" HEIGHT="300" BGCOLOR="#2B2B2B">
    <TR>
		<TD VALIGN="TOP" BGCOLOR="RED">셀1(LEFT)</TD>
		<TD VALIGN="MIDDLE">셀2(CENTER)</TD>
		<TD VALIGN="BOTTOM">셀3(RIGHT)</TD>
    </TR>
</TABLE>

</P>
</BODY>
</HTML>
```


+ [table3.html](table3.html)
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
<HEAD>
<TITLE> 테이블 병합2 </TITLE>
</HEAD>

<BODY>
<TABLE WIDTH = "300" BORDER ALIGN = "CENTER">
<CAPTION>테이블 좌우 합치기 </CAPTION>
<TR>
	<TD COLSPAN = "2">1번2번</TD>
	<TD>3번</TD>
</TR>
<TR>
	<TD>4번</TD>
	<TD>5번</TD>
	<TD>6번</TD>
</TR>
<TR>
	<TD>7번</TD>
	<TD COLSPAN = "2">8번9번</TD>
</TR>
</TABLE>

<HR>
<TABLE WIDTH = "300" HEIGHT = "200" BORDER ="1" ALIGN = "CENTER">
<CAPTION>셀의 간격</CAPTION>
<TR>
	<TD WIDTH = "100">1번</TD>
	<TD WIDTH = "200">2번</TD>
	<TD WIDTH = "100">3번</TD>
</TR>
<TR>
	<TD WIDTH = "100">4번</TD>
	<TD WIDTH = "100">5번</TD>
	<TD WIDTH = "100">6번</TD>
</TR>
</TABLE>
<HR>
<TABLE WIDTH = "300" HEIGHT = "200" ALIGN = "CENTER">
<TR>
	<TD WIDTH = "100" ALIGN = "LEFT">1번</TD>
	<TD WIDTH = "100" ALIGN = "CENTER">2번</TD>
	<TD WIDTH = "100" ALIGN = "RIGHT">3번</TD>
</TR>
<TR>
	<TD VALIGN = "TOP" ALIGN = "CENTER">4번</TD>
	<TD VALIGN = "MIDDLE">5번</TD>
	<TD VALIGN = "BOTTOM">6번</TD>
</TR>
</TABLE>

</BODY>
</HTML>
```


+ link태그
  1) 다른 페이지와 연결
  2) 현재 페이지에서의 화면 이동
  `<a href="연결될페이지">글자나 그림</a>`


+ 예제1 - [main.html](main.html)
```html
<HTML>
<HEAD>
	<TITLE> 페이지 링크1 </TITLE>
</HEAD>

<BODY>
	<a href="#" name="top" />
	<A href="page_1.html">페이지1 가기</A>로가기<BR>
	<A HREF="page_2.html">페이지2 가기</A><BR>
	<A HREF="page_3.html">페이지3 가기</A><BR><BR><BR>

	<A HREF="http://www.daum.net">다음사이트로 이동...<P></A>
</BODY>
</HTML>
```

+ 예제2 - [Home.html](Home.html)
```html
<HTML>
<HEAD>
	<TITLE> Home </TITLE>
</HEAD>

<BODY>
	<TABLE border="0" width="100%" cellspacing="0" cellpadding="4"
	  style="border:1px solid gray;margin-bottom:5px">
	  <TR>
		<TD width="100%" height="50" class="fontbasic">
		  <A href="http://www.daum.net"><img border="0" alt="js guide - professional javascript" src="jsguide.png" width="180" height="50"></A>
		</TD>
	  </TR>
	  <TR>
		<TD width="100%" style="border-top:1px solid lightgrey" class="topmenu">
		  <A HREF="Home.html">처음으로</A> |
		  <A HREF="HtmlPage.html">HTML</A> |
		  <A HREF="JScriptPage.html">JScript</A> |
		  <A HREF="StyleSheetPage.html">StyleSheet</A> |
		  <A HREF="JSPPage.html">JSP</A> |
		</TD>
	  </TR>
	</TABLE>
	<!-- Homl.html의 구체적인 화면구성 Tag를 표현 -->
	Home.html
</BODY>
</HTML>

```

+ 예제3 - [imgMapTest.html](imgMapTest.html)
```html
<!-- imgMapTest.html -->
<HTML>
<HEAD>
	<TITLE>이미지맵</TITLE>
</HEAD>
<BODY>
	<IMG src="./img.jpg" usemap="#myimg">
	<MAP name="myimg">
		<!-- 원 모양 : 중심점 반지름 X,Y,R-->
		<AREA shape="circle" coords="130,110,52" href="https://www.daum.net/" target="_blank">
		<!-- 사각 모양 : 시작좌표, 마지막좌표, x1,y1,x2,y2-->
		<AREA shape="rect" coords="270,51,377,158" href="https://www.naver.com/" target="_blank">
		<!-- 각 꼭지점의 좌표들..-->
		<AREA shape="poly" coords="520,60,450,160,597,158" href="http://www.itkor.co.kr/" target="_blank">
	</MAP>
</BODY>
</HTML>
```

+ fram, iframe ==> <div>, <span>

+ form태그 - 사용자로부터 선택받거나 입력받은 데이터를 서버로 전송하는 기능.
  - JavaScript와 밀접한 관련이 있다.

#### 6. 실습
#### 7. Summary / Close



-----------------------------------------------------------

### [2019-05-15]

#### 1. Review
+ 한글이 깨질때
```html
<META CHARSET="EUC-KR">
<!--또는-->
<META CHARSET="UTF-8">
```


#### 2. FRAME 태그
+ 프레임을 상하로 나눌때
```html
<frameset rows=“”>
    <frame src=“” name=“”>
    …
</frameset>

```

+ 프레임을 좌우로 나눌때
```html
<frameset cols=“”>
    <frame src=“” name=“”>
    …
</frameset>
```


#### 3. FORM 태그
+ 사용자로부터 선택받거나 입력받은 데이터를 서버로 전송하는 기능

+ html5에서 좀더 추가된 많은 form 내부 태그가 있다.
+ html에서는 form태그의 기능적인 측면을 살펴보고 JavaScript에서 이벤트처리와 관련된 내용을 보자.

```html
<form action="http://localhost:8088/html/Hundred" method="get">
  <!--다양한 내부태그-->
</form>
```

+ get 방식은 주소창에 모든것이 들어나지만.... post 방식은 노출되지 않는다.
```html
<form id="식별자" class="분류기준" name="명칭" action="http://localhost:8088/html/Hundred" method="post">
  <!--다양한 내부태그-->
</form>
```

+ multipart/form-data를 이요하여, 사진이나 기타 미디어를 전송하게 한다.
```html
<form action="http://localhost:8088/html/Hundred"
                  method="post" enctype="multipart/form-data">
  <!--다양한 내부태그-->
</form>
```

+ form태그의 action속성 : 서버의 어느 동적웹컴퍼넌트(서블릿,JSP)에 사용자가 입력한 데이터를 진송할 것인가를 지정.
  - method : 데이터잔송하는 http프로토콜의 내부형식을 지정.
    - get => 데이터 노출
    - post => 데이터 노출X
![HTTP 요청](./img/html_1.png "HTTP 요청")
    - enctype => 사진과 같은 파일을 서버로 전송할때 필요한 옵션



+ form태그에서 실제 데이터를 선택하거나 입력하는 기능을 수행하는 기능
  - 내부태그는 name속성
  - 한줄로 문자열을 입력받는다.
    1) `<input type="text">`
    2) `<input type="password">`
  - 여러줄로 문자열을 입력받는다.
    3) `<textarea rows="줄수" col="칸수">초기문자열</textarea>`
  - 여러항목중 한개의 항목만 선택하는 radio button
    4) `<input type="radio" name="fruit" value="yes">사과`
       `<input type="radio" name="fruit" value="포도" checked>포도`
       `<input type="radio" name="fruit" value="메론">메론`
  - 여러개 항목을 선택할 수 있는 checkbox
    5) `<input type=checkbox name="text" value="value">명칭`
  - 목록에서 항목을 선택할수 있는 select
    6) `<select name="text">`
      `<option value="text">text name`
      `</select>`
    - size속성을 사용하면 리스트형식으로 동작.
    7) `<select name="text" size="number">`
      `<option value="text">text name`
      `</select>`
    - multiple속성을 사용하면 복수(여러항목_ctrl)선택 가능으로 동작.
    8) `<select name="text" size="number" multiple>`
      `<option value="text">text name`
      `</select>`
  - form작업을 마무리하고 선택/입력
    9) `<input type="submit" value="전송하기">`
      `<input type="reset" value = "다시쓰기">`
      `<input type="image" name="submit" src="button1.gif" border="0">`

+ [Form.html](Form.html)
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
 <HEAD>
  <TITLE> 폼태그 연습 </TITLE>
  <META NAME="Generator" CONTENT="EditPlus">
  <META NAME="Author" CONTENT="">
  <META NAME="Keywords" CONTENT="">
  <META NAME="Description" CONTENT="">
 </HEAD>

 <BODY>

  <FORM action="서버url" method="get" enctype="">
     <!-- TEXT 입력 -->
	 이 름 <INPUT type="text" name="id" size="10"><BR>  
 	 주 소 <INPUT type="text" name="addr" size="50" value="강원도 춘천시 교동"><BR>
 	 전화번호<INPUT type="text" name="phone" size="14" maxlength="14"><BR>
     E-Mail <INPUT type="text" name="email" value="argus10@hanmail.net" readonly><BR>
     <BR><BR>

     나 이 <INPUT type="text" name="age" size="20">
	 <BR><BR>

     <!-- 암호 입력 -->
	 암 호 <INPUT TYPE="password" NAME="pass">
	 <BR><BR>

	 <!-- 여러줄 입력 -->
	 자기소개<BR>
	 <TEXTAREA NAME="memo" ROWS="5" COLS="40">자기소개를 입력하세요</TEXTAREA>
	 <BR><BR>
	 <HR>

 	 <!-- radio box-->
     <INPUT type="radio" name="fruit" value="yes">사과 &nbsp;&nbsp;
     <INPUT type="radio" name="fruit" value="포도" checked>포도&nbsp;&nbsp;
     <INPUT type="radio" name="fruit" value="메론">메론<br>
	 <BR><BR>

 	 <!-- check box-->
	가보고 싶은 곳은?<BR>
	<INPUT type=checkbox name="city" value="Roma">Roma<BR>
	<INPUT type=checkbox name="city" value="Paris" checked>Paris<BR>
	<INPUT type=checkbox name="city" value="London">London<BR>
	<INPUT type=checkbox name="city" value="NewYork">New York
     <BR><BR>
	 <HR>

    <SELECT name="favorite1">
	    <OPTION value="사과">사과</OPTION>
		<OPTION value="포도">포도</OPTION>
	    <OPTION value="복숭아">복숭아</OPTION>
		<OPTION value="수박">수박</OPTION>
	    <OPTION value="바나나" selected>바나나</OPTION>
    </SELECT>
	<BR><BR><BR>

	<SELECT name="favorite2" size="10" multiple>
		<OPTION value="사과">사과</OPTION>
	    <OPTION value="포도">포도</OPTION>
	    <OPTION value="복숭아">복숭아</OPTION>
	    <OPTION value="수박">수박</OPTION>
	    <OPTION value="바나나" selected>바나나</OPTION>
	    <OPTION value="감" selected>감</OPTION>
	    <OPTION value="귤" selected>귤</OPTION>
	    <OPTION value="파인애플" >파인애플</OPTION>
	 </SELECT>
	 <BR><BR>
	 <HR>

	 <!-- 버튼 - TEXT -->
	<INPUT TYPE="submit" value="전송하기">&nbsp;&nbsp;<INPUT TYPE="reset" value = "다시쓰기">
	 <BR><BR>
	 <HR>

	<!-- 버튼 - 이미지 -->
	<INPUT type="image" name="submit" src="button1.gif" border="0">&nbsp;
	<INPUT type="image" name="reset" src="button2.gif" border="0">
	<BR><BR>
    <HR>

	<!-- hidden 화면에 보여지지 않지만 전송버튼(submit)에 의해서
	action에서 지정한 서버프로그램으로 데이타는 전송된다.-->
	<INPUt type="hidden" name="key" value="ABC">
	<BR><BR>
	<HR>

	<!-- file -->
	파일첨부 : <input type="file" name="file" size="13">
	<BR><BR>
	<HR>

  </FORM>
 </BODY>
</HTML>
```

+ 이미지 버튼을 만들때, reset활용
  - onclick : 클릭했을때 수행하는 명령
  - document.form[0] : 현재 띄워진 문서
  - .reset() : 리셋 명령어
  - return false : 서버로 전송하지 않도록 명령...(;는 추가 명령시...)
```html
<INPUT type="image" name="submit" src="button1.gif" border="0">&nbsp;
<INPUT type="image" name="reset" src="button2.gif" border="0" onclick="document.forms[0].reset();return false">
```



+ 화면에는 보이지 않지만 서버로 전송되는 데이터
```html
<input type="hidden" name="식별자" value="값" >
```
  - 프로그램 로직과 연관시켜서 많이 사용된다. -> 쿠키,세션관리처리에서 사용



+ 서버로 파일을 전송하고 싶다면
```html
<input type="file" name="file" size="">
```
  - 단, 파일명뿐만아니라 파일내용도 서버로 전송하려면 <form ... enctype="multipart/form-data"를 적용해야한다.



#### 4. JavaScript 개요
+ HTML에서 동작하는 인터프리터 언어
+ 웹브라우저에서 자체반응하는 기능을 수행
  - 즉, 서버로 요청을 보내지 않고 웹브라우저 자체에서 HTML문서의 내용을 검색/편집하는 기능을 수행하여 결과적으로 사용자에게 보여지는 내용이 달라지도록(반응하도록) 한다.

+ 언어이므로 자료형/벼수, 연산자, 제어문, 객체도 존재
+ 함수(Function)를 많이 사용

+ 메서드(method)는 객체의 멤버로서 존재하는 실행코드이다.
  그러나!!! 함수(function)는 독자적으로 존재하는 실행코드의 모음.
```
Java언어                  JavaScript언어
객체.멤버변수              객체.property, arribute
객체.메서드()              객체.메서드()
```

+ 변수사용
```
<Java 언어(인터프리터)>
int a = 10;
1) a는 프로그램시작부터 끝까지 정수값만 저장하여 사용할수 있다.
2) 반드시 선언후에 사용해야한다.
3) 선언시 반드시 자료형을 명시해야 한다.

a = "abc"; //error
b = 3.14;  //error


<JavaScript 언어(스크립트)>
var a = 10;  //ok
b = "3.14";   //ok
...
a = "abc";    //ok
```

+ 대소문자를 구분한다. (A != a)


+ JavaScript코드는
  1) html 문서 내부에 script태그 내부에 작성한다.
```javascript
  <script>
    JS코드
  </script>
```
  2) 별도의 독자적인 파일 형태로 코드가 존재할수 있다.
```javascript
<script src="chek.js">
</script>
```

  3) script태그는 html문서 내부에서 여러번 사용할 수 있다.
  - [script1.html](script1.html)
```javascript
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> JavaScript 개요 </title>
  <!-- HTML문서가 웹브라우저에 의해 해석되기 시작하면
       1) 자동으로 window객체가 자동으로 만들어진다.
           document객체
		   location객체
		   history객체
		   navigator 개체
		   event 객체
		   screen 객체
		   --------------------------------
		   window.document
		   window.location
		   window.history
		   window.event
		   window.screen
		   window.navigator
       2) html화일의 내부 구성요소 즉, 태그들을 하나하나 읽으면서
	      그 구성요소에 따라서 documnet객체의 내부 구성요소로
		  해석되어 객체로 표현되어진다.
		  DOM(Document Object Model) ==> 이진 Tree 구조
       3) 사용자의 행위에 따라 발생하는 이벤트 처리 코드를
	      JavaScript를 이용하여 작성하고 사용하게 된다.
		  ==> 동적인 처리를 지원한다.
	-->
   <script LANGUAGE="javascript">
		var test;  //변수선언 --> 자료형을 결정하지 않는다.

		//아래의 코드가 자동으로 만들어진 window객체의 메서드를 호출한 표현이다.
		window.document.write("자바 스크립트 입니다.<br>");
		//window객체의 속성을 사용할때는 window를 생략할 수 있다.
		document.write("<h2> 예제 1 : 이것이 기본 태그입니다.</h2>");
   </script>

   <script type="text/javascript">
		window.document.write("<h2> 예제2 : language속성에는 스트립트 언어의 종류를 기술합니다. </h2>");
   </script>

   <script type="text/javascript" src="first.js" charset="utf-8">
   		//src속성을 사용했을때 내부코드는 무시된다.
		window.document.write("외부의 스크립트 화일을 참조할 때는 스크립트 태그 내부의 문장은 작동하지 않습니다.<br>");
   </script>

   <script type="text/javascript">
   		//윈도우 화면에 경고창을 보여주세요.
		window.alert("스크립트도 독립된 프로그래밍 언어입니다.");
   </script>

   <script type="text/javascript">
		function test(){
			//html문서가 최초 load되어 한번 작동한 이후에
			//document의 write()메서드를 사용하면 이전내용은 모두 지워진다.
			document.write("함수는 호출될때만 작동합니다.<br>");
			window.alert("함수 호출에의해 동작했습니다.");
		}
   </script>
 </head>

 <body>
	<br>자바 스크립트의 작동구조를 이해하는 것이 중요합니다.<br>
	<script type="text/javascript">
		// 스크립트는 <head>태그 내부에 작성하기를 권장하지만
		// 상황에 따라 <body>태그 내부에 작성 할 수도 있다.
		document.write(" body 태그 내부에서 스크립트를 실행합니다.");
	</script>
	<form>
		<input type="button" value="함수호출" onClick="test()"/>
	</form>
 </body>
</html>

```

+ [script2.html](script2.html)
  - 지역변수와 전역변수
  - 함수 안에서 변수를 선언했어도...var로 선언하면 함수 내에서만 사용가능한 지역변수, 그냥 문자로 선언하면 어디서든 쓸수 있는 전역변수로 사용된다.
```javascript
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
 <HEAD>
  <TITLE> 자바 스크립트 자료형과 변수 </TITLE>
  <SCRIPT LANGUAGE="JavaScript">
	document.write(" 조금있으면 자율학습시간 입니다.");
	// 변수 선언 :
	// 함수밖에 선언되거나 직접 사용한 모든 변수는 전역변수 이다.
	var i;           //초기값 지정이 없다(undefined)
	var j = 20;      //초기값 지정
	i = 10;
	k = 30; // 선언 없이 변수에 값을 직접 치환했다.
	document.write("<p>");
	document.write(i+j);
	document.write("</p>");

	document.bgColor = "yellow";

	// 경고창 띄우기
	window.alert("자바스크립트의 변수사용방법입니다.");

	document.write(k + "<BR>");

	// 자바스크립트의 함수 기본 형태
	function test(){
		var l = 50;		// 지역변수 선언
		g = 30;			// 함수안에서 선언없이
						// 값을 직접 치환하면 g는 전역변수가 된다.
		document.write("test()가 호출되었습니다.<BR>");
		document.write(l + " " + g + "<BR>");
	}

	function test2(){
		document.write("test2()가 호출되었습니다.<BR>");
		//i,j,k는 함수밖에서 선언하거나 확보했으므로 전역변수이다.
		//g는 함수내부에서 선언했지만 var가 없으므로 전역변수이다.
		//l은 test()함수 내부에서 var과 함께 선언했으므로 test()에서만 사용할 수 있는 지역변수이다.
		//document.write(i+" " + j + " " + k + " " + g + " " + l);
		document.write(i+" " + j + " " + k + " " + g );
	}
  </SCRIPT>
 </HEAD>

 <BODY>
  <SCRIPT LANGUAGE="JavaScript">
	document.bgColor = "blue";
	document.write((i+j+k) + "<BR>" );

	//자바스크립트에서 Java언어와 마찬가지로
	//객체를 사용할때 속성(property)과 메서드(method)를 사용한다.

	// 함수 호출
	test();
	test2();
  </SCRIPT>
 </BODY>
</HTML>

```

```
class Test{
  int age; //단일값 속성
  Date today; //개체 속성
  Profile profiles[] = new Profile[5];  //컬렉션 속성
}

속성
  1) 단일값 속성
  2) 개체 속성
  3) 컬렉션 속성

메서드
  1) 일반메서드
  2) 이벤트처리와 관련된 메서드 : onXXXX()
```


##### [오늘의 과제]
+ html태그중 <form>관련 태그를 웹상에서 찾아보기


#### 5. 실습
#### 6. Summary / Close




-----------------------------------------------------------

### [2019-05-16]

#### 1. Review
#### 2. FORM 태그

#### 5. 변수


```javascript
```


```javascript
```


```javascript
```


```javascript
```
#### 6. 제어문
#### 7. 함수
#### 8. 객체
####
####
####
####
#### 4. 실습
#### 5. Summary / Close


-----------------------------------------------------------

### [2019-05-17]

#### 1. Review
#### 2. FORM 태그

#### 5. 변수


```javascript
```


```javascript
```


```javascript
```


```javascript
```
#### 6. 제어문
#### 7. 함수
#### 8. 객체
####
####
####
####
#### 4. 실습
#### 5. Summary / Close
