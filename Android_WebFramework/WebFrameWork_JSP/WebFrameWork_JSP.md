### [2019-05-28]

#### 3. JSP 개요
+ 웹어플리케이션의 구조-웹서버(Tomcat)
```
brain3\*.html, *.js, *.jsp, 다양한 리소스(*.jpg, *.wav,...)  <== 내부폴더를 가질수O
      \WEB-INF\web.xml
              \classes\패키지형태의 컴파일된 서블릿코드
              \lib\서블릿코드에서 사용하는 외부 라이브러리파일(*.jar)
```

+ jsp코드가 실행될때 서블릿을 변환되는데 이때 서블릿은 get/post방식으로 따로 처리하지않고 무조건 service(,)메서드로 변환된다.


+ 1) hteml 태그
+ 2) jsp 태그
```
1) 자바코드와 관련된 태그 -- scriptting element
<%  %>  스크립트렛(scriptlet)
<%= %>  표현(expression)
<%! %>  선언(declation)


2) 지시어 -- directive element
    jsp가 서블릿으로 변환될때 필요한 정보를 설정하는 태그
<%@ page .... %>
<%@ inclue ...%>
<%@ taglib ...%>


3) EL -- Expression Language
    자바변수의 값을 손쉽게 출력하는 기능
${ 표현식 }


4) 표준액션태그와 외부액션태그(JSTL) ==> 태그의 구조가 xml문법형식을 따른다.
<jsp: 

<c:
<fmt:
``` 



   - 지시어 요소 - directive element (<%@ 속성=값,...  %>
```
     <%@page                  %>
     <%@include               %>
     <%@taglib                %>
```     
   - 스크립팅 요소(scriptting element) ===> 자바코드
 ```
     <% 자바코드 %>                        스크립트렛(scriptlet)
     <%= 자바변수,메서드호출 %>              표현(expression)  
     <%! 멤버변수선언 및 메서드정의부 %>        선언(declaration)
   ==============================================================       
```


   - 액션 요소(jsp에서 기능을 수행하는 표준 태그)
```
     <jsp:include >
     <jsp:forward
     <jsp:usebean
     <jsp:setProperty
     <jsp:getProperty
     ...  
```
   - Expression Language  : 다양한 출력을 편하게 사용하도록 지원하는 언어
```
     ${ 표현식 }
```     


   - 외부 태그 라이브러리
     1) 사용자가 만들수도 있다.
     2) 이미 만들어진것을 사용할수도 있다.
```
    JSTL(Jsp Standard Tag Library)
    <c:
    <fmt:
    <x:
    <sql:
    ....
```



   - 위의 요소 내부에서 자유롭게 사용할수 있는 객체들이 있다(declation요소는 제외). 이것을 JSP내장객체라고 부른다. 
    - --> 지금까지 우리가 서블릿에서 사용했던 객체들(request, response, session, application, ...)
    - jsp페이지가 요청되면 jsp페이지가 서블릿코드로 변환시킬때 내부적으로 추가되어지는 변수이다.
    - 즉, jspService()메서드 내부에 인자로 전달된 변수이거나 내부에서 선언한 변수들이다.
```     
<내장객체명칭>
     request
     response
     out
     application  ==> ServletContext sc = getServletContext(); [웹전체를 관리하는 놈]
     page
     pageConext
     session
     exception
```



+ JSP파일은 실행될때 서블릿으로 변환되 컴파일되고, 서블릿 객체가 생성된 후 실행된다.!!!

+ JSP페이지 실행과정
  1. 최초 요청시 웹서버는 해당 jsp의 서블릿객체가 메모리에 있는지 검사한후
```
   ----------->  JSP                  Servlet
            없으면 1) Hundred.jsp --변환--> Hundred_jsp.java --컴파일-> Hundred_jsp.class
               2) 서블릿클래스를 메모리에 로드후 객체 생성후 초기화기능 수행
                                          -------  -----------
                                                      생성자      jspInit()
               3) 쓰레드 단위로 jspService(HttpServletRequest request,
                                        HttpServletResponse response) 메서드 작동후
   <----------- 실행결과를 응답한다.                
```

  2. 이후요청부터는 3)의 작업만 동작한다.     
  3. jsp소스가 수정되었다면 그내용이 반영되어야 하므로 메모리의 서블릿객체를 소멸시키기 직전에  4) jspDestroy()메서드를 동작시킨다. 
```
   변환된 코드에서의 핵심 메서드는
   jspInit()
   jspService() ****  핵심 ****
            우리가 작성한 jsp페이지의 내용은 거의 대부분 jspService()메서드 내부에 포함된다.
            이때 html태그는 출력스트림을 이용하여 그대로 출력되는 코드로 변환되고
            자바코드(<% %>)는 그대로 포함된다. 
            표현 즉, <%= 변수  %>는 out.println(변수)로 변환된다.         
   jspDestroy()              

   1) 지역변수가 선언및 정의 ==> 내장객체
   2) JSP내부의 모든 HTML태그는 그대로 출력된다.
   3) 자바코드 즉, <% %>, <%= %>는 그대로 적용된다.
   4) <%@ 지시어요소는 설정과 관련되어 적절한 자바코드로 변환된다.
   5) 액션태그, EL, JSTL등도 자바코드의 특정메서드들도 호출하는 표현으로 모두 변환된다.
```

+ Hundred.jsp
```jsp
<%@ page import="java.util.Random"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
		<title>1부터 100까지의 합</title>
	</head>
	<body>
		<%
            //request객체를 자유롭게 사용가능.
			String name = request.getParameter("addr");
		
			//이곳에 자바코드를 자유롭게 작성한다.
			int total = 0;
			for(int cnt=1; cnt<=100; cnt++){
				total += cnt;
			}
			
            //필요한 구문에 해당하는 import를 앞쪽에 배치
			Random random = new Random();
			int value = random.nextInt();
		%>
		1부터 100까지의 합계? <%= total %>
		<hr>
		<%
			out.println("1부터 100까지의 합계 ? "+ total+"<br>");
			out.println("발생된 난수값은 ? "+value);
		%>
	</body>
</html>
```


+ 향후 jsp의 학습이 마무리될 즈음 우리가 작성하는 서블릿과 jsp의 협업은 주로 다음의 형태를 가진다.
```
client  --------request------->  servlet
                            (로직처리 -- 결과)
                                  |request.setAttribute("키",값);
                                  |forward(request,response)
                                  |
                                  V
  <---------response---------- JSP페이지
                        가급적이면 java코드가 없이 태그로만 작성을 권한다.
                        EL, 표준액션태그, JSTL로만 작성
                                ${ 키 }
```

+ Winneers.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR" pageEncoding="EUC-KR"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>복권 당첨 번호</title>
</head>
<body>
	전달된 배열 내용값은 ? ${ ARR }
	<hr>
	EL표현은 반복기능과 같은 제어기능이 없고 단지 값을 출력하기만 한다.<br>
	그래서 태그를 이용해서 프로그램처럼 변수도 사용하고 제어문처리도 가능한<br>
	외부태그라이브러리로 JSTL을 사용한다.
	<hr>
	참고) jstl을 사용하는 절차<br>
	<li> 라이브러리를 lib폴더에 복사
	<li> taglib지시어 사용
	<li> jstl 태그 사용
	<hr>
	전달된 배열의 내용값들<br>
	<c:forEach var="num" items="${ARR}">
		${num} <BR>
	</c:forEach>
</body>
</html>
```

+ WinnerServlet.java
```java
package com.jica.brain3;

import java.io.IOException;
import java.util.Random;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/WinnerServlet")
public class WinnerServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//로직처리의 결과값으로 배열이 만들어졌다고 가정
		int arr[] = new int[5];
		Random random = new Random();
		for (int cnt = 0; cnt < arr.length; cnt++) {
			arr[cnt] = random.nextInt(10000000);
		}
		request.setAttribute("ARR", arr);
		RequestDispatcher rd = request.getRequestDispatcher("Winners.jsp");
		rd.forward(request, response);
	}
}
```


+ Hundred2.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<%
		int total = (int)request.getAttribute("total");
		int value = (int)request.getAttribute("value");
	%>
	
	1부터 1000까지의 합? <%= total %><br>
	발생된 난수값 ?  <%= value %>
	<hr>
	1부터 1000까지의 합? ${ total }<br>
	발생된 난수값 ? ${ value }
</body>
</html>
```

+ HundredServlet.java
```java
package com.jica.brain3;

import java.io.IOException;
import java.util.Random;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/HundredServlet")
public class HundredServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//여기서는 html태그로 결과를 만드는데 필요한 코드는 전혀 관심두지말고
		//로직처리만 한다.
		int total = 0;
		for (int cnt = 0; cnt <= 1000; cnt++) {
			total += cnt;
		}
		
		Random random = new Random();
		int value = random.nextInt(1000);
		
		RequestDispatcher rd = request.getRequestDispatcher("Hundred2.jsp");
//		request.setAttribute("total", new Integer(total));
		request.setAttribute("total", total);
		request.setAttribute("value", value);
		rd.forward(request,response);
	}
}
```





#### 4. Scripting element
+ Script let
+ Expression
+ declation


+ 스크립팅요소(scripting elements)란 ?
    - jsp페이지에 포함된 자바코드와 관련된 요소를 말한다.
```
1) 스크립트렛(scriptlet)         <% 자바코드  %>
    이때의 자바코드는 jsp가 서블릿으로 변환될때
    _jspService(,){
        ...
        자바코드
        ...
    }

2) 익스프레션-표현(expression)    <%= 자바변수  혹은 리턴값이 있는 메서드호출() %>
    이때의 자바코드는 jsp가 서블릿으로 변환될때
    _jspService(,){
        ...
        out.print(자바변수)
        ...
    }

------------------------------------------------
3) 선언부(declaration)          <%! 멤버변수 혹은 메서드정의부작성 %>
    jsp가 서블릿으로 변환될때
    서블릿의 멤버변수 즉, 인스턴스변수를 선언하거나
    메서드를 선언/정의할때 사용한다.


============================================================
jsp페이지에 포함된 모든 html태그는 그대로 응답(출력된다.)
  jsp ---변환----> 서블릿
  
  jspService(HttpServletRequest request, HttpServletResponse response){
  	지역변수 선언 및 생성               --------                     ---------
  	------  ===================>+    jsp의 내장객체라고 부른다.
  	html태그 출력
  	스크립팅요소가 포함된다.
  	익스프레션은 out.print()형태로 포함된다.
  	선언요소는 여기가 아니라 클래스의 멤버영역에 멤버변수와 메서드로 포함된다.
  }
```


+ TwoHundred.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>1부터 200까지의 합</TITLE></HEAD>
    <BODY>
        <%
        	//스크립트렛 내부에서는 자바언어의 문법을 그대로 적용한다.
            int total = 0;  //_jspService()메서드 내부의 지역변수
            for (int cnt = 1; cnt <= 100; cnt++)
                total += cnt;
        %>
        1부터 100까지의 합 = <%= total %> <BR>
        <% 
            for (int cnt = 101; cnt <= 200; cnt++)
                total += cnt;
        %>
        1부터 200까지의 합 = <%= total %> <BR>
        <%!
        %>
    </BODY>
</HTML>

<%!
%>
```

+ TwoPower.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>2의 거듭제곱</TITLE></HEAD>
    <%!
    	int total = 0;  //_jspService() 메서드 내부의 지역변수가 아니라
    					// 현재 jsp페이지(서블릿)의 인스턴스 멤버변수가 된다.
    %>
    <BODY>
        <%
    		total = 0;  // 초기화하지 않으면... 실행시마다 값이 누적된다!!!
    	%>
        2 ^ 1 = <%= power(2, 1) %> <BR>
        2 ^ 2 = <%= power(2, 2) %> <BR>
        <%
        	total += 500;
        	out.println("현재까지 total의 값 : "+total+"<br>");
        %>
        2 ^ 3 = <%= power(2, 3) %> <BR>
        2 ^ 4 = <%= power(2, 4) %> <BR>
        2 ^ 5 = <%= power(2, 5) %> <BR>
        <%
        	total += 100;
        	out.println("현재까지 total의 값 : "+total+"<br>");
        	total += power(2,10);
        	out.println("현재까지 total의 값 : "+total+"<br>");
        %>
    </BODY>
</HTML>
<%! 
	//아래의 메서드도 클래스의 일반메서드가 된다.      
    private int power(int base, int exponent) {
        int result= 1;
        for (int cnt = 0; cnt < exponent; cnt++)
            result *= base;
        return result;
     } 
%>
```



#### 5. 지시어
+ `<%@` 
+ page
+ include
+ taglib

+ 지시어요소(directive element)
    - jsp가 서블릿으로 변환될때 서블릿에 적용되는 설정정보를 작성하는 태그
```
다음 세가지 지시어 요소가 사용된다.
----------------------------------------
1)<%@ page 속성="값",.... %>
2)<%@ include  속성="값",....%>
3)<%@ taglib 속성="값",.... %>  ==> JSTL학습
----------------------------------------
```

1. page지시어 속성
```
   contentType,import,buffer,autoFlush,isThreadSafe,session,
   errorPage,isErrorPage,isELIgnored,info,extends,language,..
   
   1) contentType="text/html; charset=euc-kr"
      ==> response.setContentType("text/html; charset=euc-kr");

   2) import="패키지명.클래스"
      ==> import 패키지명.클래스;
      
      import속성을 사용하지 않아도 자동으로 포함되는 내용
      -----------------------
      java.lang.*
      javax.servlet.*;
      javax.servlet.http.*;
      javax.servlet.jsp.*;
    import속성은 여러번 사용할수있는 유일한 구문이다.

   3) buffer="4kb"
        출력버퍼의 크기를 설정한다.
```

2. include지시어 : 정적인 포함(jsp--->서블릿으로 변환될때 소스를 그대로 복사하여 포함시킨다)
```
   <%@ include file="포함될파일" %>
```

3. taglib지시어는 외부 라이브러리를 특히 jstl에서 사용한다.
```
   <%@ taglib prefix="접두어" uri="uri" %>   
```


+ JSP페이지에서 사용하는 주석 기호들
```
1) <!-- HTML 주석-->
      jsp가 서블릿으로 바뀔때 그대로 응답내용에 출력된다.
        단, 응답을 받은 웹브라우저에서 그대로 해석해서 화면에 보여줄때 주석이므로 처리하지 않는다.
        
   <%-- JSP주석 --%>   
      jsp가 서블릿으로 바뀔때 그내용이 웹서버(웹컨테이너)에서 주석이므로 처리하지 않는다.  

2) 스크립팅요소(<% %>, <%= %>, <%! %>)에서 주석 ==> Java Code이므로 Java언어의 주석을 그대로 사용
   한라인 주석 : //
   블럭 주석   : /*    */ 

===============================================
  jsp --------------> 서블릿
Test.jsp           class Test_jsp extends HttpJspBase{

<%@page             선언의 표현은 이자리에 멤버변수혹은 메서드형태로 나타난다.  
   include
   taglib  ... %>
html태그                 jspInit(){}
<% 스크립트렛 %>	    jspDestroy(){}
<%= 익스프레션 %>		jspService(request, resoponse){
<%! 선언부   %> 		  지역변수 선언및 생성
					  지시어요소의 값이 자바코드로 변환되어 포함된다.
					  out.write("html태그");
					  ....  
                            스크립트렛코드
                            익스프레션코드==>out.print(변수)
                    }

```




##### 정리

1. 지시어 : <%@ 문서의 상단에서 지시할 내용 %>
    - 지시어는 페이지의 속성, 정보 등을 선언 또는 지시하는 역할을 하는 부분으로 jsp파일의 최상단에 위치합니다.
```jsp
<%@ page
    contentType="text/html;charset=UTF-8"
    language="java"   // java외에 다른언어는 존재하지 않음. 안써도 됨.
    import="java.util.Scanner"    // import는 여러개를 쓸 수 있음.
    import="java.sql.*"
    session="true"    // default가 true이고 false를 주면 세션 사용 않겠다는 뜻
    buffer="8kb"      // default가 8kb이고 버퍼사이즈를 조정하는 속성
    autoFlush="true"  // out.close()의 역할
    info="jsp 페이지 연습"
    errorPage="error.jsp"    // 에러페이지는 리다이렉션이 아닌 서버에서 바로 푸시하는 방식
%>
```


2. 선언 : <%! 전역변수, 메소드 %>
    - 스크립틀릿의 자바코드는 하나의 메소드이기 때문에 전역변수를 선언하거나(접근지정자 사용 X) 새로운 메소드를 선언할 수 없습니다. 때문에 이 선언부의 <%! 키워드를 이용해 코드를 분리하여 선언합니다.
    - 선언부에 선언된 클래스변수와 멤버 메서드는 서블릿 코드로 변환되었을 때 클래스 최상단에 작성되는 것을 확인할 수 있습니다. 



3. 식(표현) : <%= 클라이언트에 출력할 내용 %>
    - jsp는 out이라는 내장객체를 이용해서 클라이언트에게 결과출력을 합니다. expression단은 이 out을 딱 한 번 사용한 것과 같습니다. 코드를 간결하게 만드는 효과를 가집니다. 코드를 딱 한 줄 사용하기 때문에 스크립틀릿과 달리 세미콜론(;)을 사용하면 안됩니다. (쓰면 에러)



4. 스크립틀릿(scriptlet) : <% 순수 자바 코드 기술 %>
    - 스크립틀릿은 완전한 자바코드로 서버단에서 처리되고 out객체나 <%= 요소를 통해서 결과만 출력합니다. 
    - 따라서 브라우저를 통해 소스를 확인해 보면 java코드는 확인할 수 없습니다.
```jsp
<script>
    var a = 10;    // javascript code
    document.write("a: ", a);    // js 는 브라우저가 해석
</script>
<%
    int b = 10;    // java code
    out.println("b: " + b);    // jsp는 서버에서 해석
%>
<br>
<%
    /* jsp파일은 service 메소드를 오버라이딩한 하나의 메소드이다. */
    //private String str = "";    // * err : jsp 는 전체가 하나의 메소드
    String ir = "홍길동";    // 지역변수
    out.println(ir + "의 홈페이지입니다.");
%>
<h1>제목 태그</h1>
<h3>제목 태그</h3>
<h6>제목 태그</h6>
<%
    for(int i = 1; i < 7; i++){
        out.print("<h" + i + ">");
        out.print("제목 태그(자바로 작성)");
        out.println("</h" + i + ">");
    }
%>
```
    - HTML에 위와같이 삽입되는데 소스가 복잡해지고 길어지면 HTML문서를 읽는 것이 매우 어려워 집니다. 이 때 사용되는 것이 바로 빈즈입니다.


5. 액션 태그 : <jsp: … />


#### 7. 실습
#### 8. Summary / Close



-----------------------------------------------------------


### [2019-05-29]

#### 1. Review
+ JSP(HTML태그)  -> `*.java`(서블릿) -> `*.class`

+ 1) HTML태그  -> out.write("내용");
+ 2) JSP태그
    - (1) 스크립팅요소 - (`<%@ %>`scriptlet, `<%= 변수 %>`expression, `<%! 멤버변수정의/메서드선언%>`declation-인스턴스멤버변수)
    - (2) 지시어(directive element) - `<%@page 속성값 %>`
    - (3) 표준액션태크
    - (4) EL
    - (5) 외부태그라이브러리(JSTL)



#### 2. 내장객체

#### 3. 쿠키와 세션

#### 4. 예외처리

#### 5. 실습
#### 6. Summary / Close



-----------------------------------------------------------


### [2019-05-30]

#### 1. Review


#### 4. 실습
#### 5. Summary / Close


-----------------------------------------------------------


### [2019-05-31]

#### 1. Review


#### 4. 실습
#### 5. Summary / Close