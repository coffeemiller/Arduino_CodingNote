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
    jsp페이지 전체에 적용되는 정보를 기술

2)<%@ include  속성="값",....%>
    jsp가 서블릿으로 변환될때 정적으로 외부파일내용 전체를 복사하여 소스를 포함시킨다.

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
        내부적으로 출력버퍼(out객체의 내부버퍼) 버퍼의 모든 내용이 웹브라우저로 자동응답되어진다. 이것또한 이미 설정되어 있다.
        <%page autoFlush="true" %> 이것을 상황에 따라 false로 변경할 수 있다.
        이때는 사용자가 강제로 출력시켜야 한다.
        이때, out.flush(); 표현을 사용한다.
        
```

2. include지시어 : 정적인 포함(jsp--->서블릿으로 변환될때 소스를 그대로 복사하여 포함시킨다)
```
   <%@ include file="포함될파일" %>
```


+ Menu.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>오늘의 메뉴</TITLE></HEAD>
    <BODY>
        <H3>오늘의 메뉴</H3>
        - 삼계탕 <BR>
        - 돈까스 <BR>
        - 튀김국수 <BR><BR>
        <%@ include file="Today.jsp" %>
        <hr>
        <%--jsp주석
        	include지시어는 jsp가 서블릿으로 바뀔때
        	file속성에 지정된 파일내용 전체가 복사된다.
        	이후, 서블릿 코드가 컴파일될때 동일변수(now객체)가 
        	2번 선언되었으므로 error발생  
        	<%@ include file="Today.jsp" %>
         --%>
    </BODY>
</HTML>
```

+ Today.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="java.util.*"%>
<% GregorianCalendar now = new GregorianCalendar(); %>
<%= String.format("%TY년 %Tm월 %Td일", now, now, now) %>
```






3. taglib지시어는 외부 라이브러리를 사용할때
    - 외부라이브러리 태그의 접두사(prefix속성)와 해당 태그에 적용되는 작성법을 기술한 dtd파일의 속성명(uri속성)을 지정한다. 우리는 jstl사용시 자세한 사용법을 학습.
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
```
+ TenMultiply.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>1부터 10까지의 곱</TITLE></HEAD>
    <!--
    	HTML주석) JSP가 서블릿을 변환될때 포한되고 최종 클라이언트에 응답내용에도 포함된다.
    	단, 웹브라우저에서 해석될때 반영되지 않으므로 화면에 보여지지는 않지만
    	소스보기를 하면 보여진다.
    	
    	이것은 JSP에 의해 생성된 HTML문서입니다.  
	 -->
    <BODY>
        <%-- JSP주석(xml문서에도 동일)
        	JSP가 서블릿으로 코드변환시 반영하지 않는다.
        	 
        	다음은 데이터를 처리하는 스크립틀릿입니다.
        --%>
        <% 
        	//스크립트렛의 내부는 자바코드를 작성하는 장소이므로
        	//자바 주석기호를 그대로 사용한다.
        	//스크립트렛의 모든 코드는 jsp가 서블릿으로 변환될때 반영되고
        	//서버에서 이미 실행시킨 내용이므로 최종 사용자(client)에게는 공개되지 않는다.
        	
        	int result = 1;    // 곱을 저장할 변수
        	/* 1부터 10까지 곱하는 반복문 */
           	for (int cnt = 1; cnt <= 10; cnt++)
            	result *= cnt;
        %>
        1부터 10까지 곱한 값은? <%= result %> 
    </BODY>
</HTML>
```



```
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
<%= 익스프레션 %>		jspService(request, response){
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



#### 2. 내장객체 (내장변수-implicit variable)
+ 선언하거나 생성하지 않고도 jsp페이지에서 사용할수 있는 객체들
```
   ==>실질적인 의미 : jsp ---> 서블릿으로 변환될때
   _jspService()메서드의 인자값으로 전달되는 변수(request,response) 혹은 내부에서 선언 및 생성한 변수이다.
```

+ 내장객체를 사용하는 장소 (아래의 2곳뿐)
```
<% 스크립트렛 %>
<%= 익스프레션 %>
```
```java
//위 2가지 표현은 jsp가 서블릿으로 변환될때 
public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response) throws java.io.IOException, javax.servlet.ServletException {
    
    //요청방식 검사(GET/POST/HEAD만 허용)

    //지역변수 선언
    PageContext pageContext;
    HttpSession session = null;
    ServletContext application;
    ServletConfig config;
    JspWriter out = null;
    Object page = this;
    JspWriter _jspx_out = null;
    PageContext _jspx_page_context = null;


    try {
      // 지역변수 생성(정의)
      //...
      pageContext = _jspxFactory.getPageContext(this, request, response, null, true, 8192, true);
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      //...

      out.write("html태그들");
      //...
    //이곳에 스크립트렛(<% %>, 익스프레션(<%= %>)의 내용이 포함된다.)
    //그래서 내장변수를 스크립트렛이나 익스프레션에서 자유롭게 사용할 수 있는 것.
```



+ 내장객체(변수)명칭
```
    request
    response
    out
    application ==> ServletContext sc = getServletContext();
    page
    pageConext
    session
    exception
```



+ 1) 내장객체의 종류
```
   _jspService()메서드의 인자값               Servlet에서의 클래스
   ------------------------------------------------------- 
   HttpServletRequest request                동일
        <form태그로 클라이언트에서 입력데이터 서버로 전송할때(요청파라메터)...
        get방식일때는 상관없지만 post방식일때는 반드시
        request.getParameter()사용전에 request.setChraterEncoding()을 적용해야
        한글이 깨지는 것을 방지할수 있다.

   HttpServletResponse response              동일
   


   _jspService()메서드의 지역변수
   --------------------------             
    JspWriter out;                           PrintWriter와 유사
        jsp내부에서 자바코드의 결과값을 html태그로 출력할때 주로 사용
    
    ServletContext application;              동일
        서블릿에서 아래의 sc와 같은 ServletContext객체를 JSP에서 application으로 작성.
        ServletContext sc = getServletContext();
        String path = sc.getRealPath("/photo");
    
    ServletConfig config;                    동일   
    HttpSession session;                     동일(jsp에서는 기본으로 세션객체가 만들어짐==><%@page session="true" %>)
    Exception exception;                     동일(예외처리 코드 작성시 만들어짐 ==> <%@page errorPage="페이지" isErrorPage="true" %>
    ----------------여기까지는 서블릿의 객체와 99% 동일하다 -----------------
    PageContext pageContext;                 현재jsp의 모든실행상태정보를 가진 객체 
    Object page;                             현재jsp페이지 객체 자체 즉, this
```



+ 2) 내장객체의 사용
```
   - <% 내장객체사용가능 %>, <%= 내장객체의 메서드호출가능  %>  
   - <%! 사용못함  %> 
         ------->멤버변수이거나 메서드 정의부이다
   
   client                       server
      -------------------------> test.jsp
                                 //요청파라메터값 얻기 request.getParameter()
                                 //로직처리
                                 //결과값을 직접 응답하거나
      <-----------------------   //response.sendRedirect("url/컴퍼넌트")할수 있다.
    웹브라우저가 자체적으로
    재요청한다. ("url/컴퍼넌트")------->            
    
    
    client                      server
       ------------------------> test.jsp (RequestDispatcher가 필요)
                                        -----include,forward-----------> *.jsp
       <------------------------forward---------------------------------- 
```


+ out 내장 객체(변수)
    - jsp내부에서 자바코드의 결과값을 html태그로 출력할때 주로 사용


+ OneToTen_old.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>1부터 10까지 순서대로</TITLE></HEAD>
    <BODY>
        <H3>1부터 10까지 순서대로</H3>
        <% for (int cnt = 1; cnt <= 10; cnt++) { %>
            <%= cnt %> <BR>
        <% } %>    
    </BODY>
</HTML>
```

+ OneToTen.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>1부터 10까지 순서대로</TITLE></HEAD>
    <BODY>
        <H3>1부터 10까지 순서대로</H3>
        <% 
            for (int cnt = 1; cnt <= 10; cnt++)
                out.println(cnt + "<BR>"); 
        %>    
    </BODY>
</HTML>
```


+ 참고) jsp내장객체중에서 정보교환을 수행할수 있는 내장객체
    - setAttribute(), getAttribute(), removeAttribute()를 가진 내장객체
```
<scope를 가지는 내장객체>
1) pageContext
2) request
3) session
4) application -- 동일 웹어플리케이션 내부의 모든 동적웹컴퍼넌트에서 공유하는 정보를 설정/사용

위 4개의 내장객체에 setAttribute()값을 설정했을때 그 값이 유지되는 범위를 JSP에서는 scope라고 부른다.
```


+ 1. 단독으로 실행되는 자바 프로그램 - 스탠드얼론(standalone) 프로그램
+ 2. jsp에서 파일입출력도 Java의 프로그램과 동일하지만
    - 실제 입출력파일의 경로는 달라진다. ==> application.getRealPath("파일명")
+ 3. jsp에서 파일에 내용을 저장하는 코드를 jsp로 작성했을때
    - 결과를 받은 화면에서 웹페이지를 새로고침하면, 직전의 실행코드가 다시 작동하여 동일한 내용이 여러번 저장될 경우가 있다.
    - 이를 개선하기 위해 response.sendRedirect를 사용할 수 있다.
        1) 파일에 저장만하는 jsp페이지
        2) 최종결과만 응답하는 jsp페이지
```
BBSInput_new.html  -------------------------->  BBSPost_new.jsp
게시판글 입력                                 글내용을 파일로 저장
                   <------------------------redirect(BBSPostResult.jsp)
                   -------재요청------------> BBSPostResult.jsp
새로고침(재요청으로) <----------------------- 최종결과응답
```

+ BBSPost_new.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="java.io.*, java.util.Date"%>
<%
	//요청객체의 문자셋 설정
    request.setCharacterEncoding("euc-kr");

	//요청파라메터값 얻기
    String name = request.getParameter("NAME");
    String title = request.getParameter("TITLE");
    String content = request.getParameter("CONTENT");
    
    //오늘 날짜 얻기
    Date date = new Date();
    //시간(파일명) 얻기
    Long time = date.getTime();
    
    String filename = time + ".txt";
    
    //작업결과를 나타내는 변수
    String result = null;
    PrintWriter writer = null;
    try {
        String filePath = application.getRealPath("/WEB-INF/bbs/" + filename);
        writer = new PrintWriter(filePath);
        writer.printf("제목: %s %n", title);
        writer.printf("글쓴이: %s %n", name);
        writer.println(content);
        result = "SUCCESS";
    }
    catch (IOException ioe) {
        result = "FAIL";
    }
    finally {
        try {
            writer.close();
        } 
        catch (Exception e) {
        }
    } 
    //결과만을 보여주는 페이지로 redirect시킨다.
    response.sendRedirect("BBSPostResult.jsp?RESULT=" + result); 
%>
```

+ BBSPostResult.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>게시판 글쓰기 - 결과 화면</TITLE></HEAD>
    <BODY>
        <H2>글쓰기</H2>
        <% 
        	//글저장 결과값을 요청파라메터에서 얻어온다.
            String str = request.getParameter("RESULT");

			
            if (str.equals("SUCCESS"))
                out.println("저장되었습니다.");
            else
                out.println("파일에 데이터를 쓸 수 없습니다.");
        %>
    </BODY>
</HTML>
```


+ 서블릿에서의 제어이동
```
1)   response.sendRedirect("url");
  |- response.setHeader("Location","url")
  |- response.sendError(304);
   
2)    --request------->A 서블릿  ---include---->B 서블릿 (공통기능)
      <-response-------       <---------------
      
3)    --request------->A 서블릿  
                          |
                          | forward
                          v
      <-response-------B 서블릿(최종결과를 보여주는 페이지)
      
      참고) RequestDispatcher 객체를 얻어서 include/forward 시켰다.
==============================================================
 JSP에서도 동일한 기능을 사용해보자(서블릿과 동일)
 ====> 나중에는 액션태그를 이용하여 
```


+ include/forward되는 page에 정보를 전달할때....request.setAttribute("키",값);

+ ForRulesInput.html
```html
<HTML>
    <HEAD>
        <META http-equiv="Content-Type" content="text/html;charset=euc-kr">
        <TITLE>사칙연산</TITLE>
    </HEAD>
    <BODY>
        <FORM ACTION=FourRules.jsp>
            첫 번째 수: <INPUT TYPE=TEXT NAME=NUM1><BR>
            두 번째 수: <INPUT TYPE=TEXT NAME=NUM2><BR>
            <INPUT TYPE=SUBMIT VALUE='입력'>
        </FORM>
    </BODY>
</HTML>
```


+ FourRules.jsp
```jsp
<%
	//현재의 JSP코드는 로직처리만 수행하고 그결과를 보여주는 JSP페이지로
	//forward 시킬것이므로 별도의 html태그가 불필요하다.
	
	//요청파라메터 얻기
    String str1 = request.getParameter("NUM1"); //"5"
    String str2 = request.getParameter("NUM2"); //"7"
    
    //작업하기 적합한 자료형으로 변환 -- 내부적으로 예외가 발생할수도 있다.
    int num1 = Integer.parseInt(str1);
    int num2 = Integer.parseInt(str2);
    
    //사칙연산을 수행후 그결과값은 request객체에 저장
    request.setAttribute("SUM", new Integer(num1 + num2));
    request.setAttribute("DIFFERENCE", new Integer(num1 - num2));
    request.setAttribute("PRODUCT", new Integer(num1 * num2));
    request.setAttribute("QUOTIENT", new Integer(num1 / num2));
    
    //결과를 보여줄 페이지로 forward
    RequestDispatcher dispatcher = request.getRequestDispatcher("FourRulesResult.jsp");
    
    //forward -- 제어이동(다시 돌아오지 않음)
    dispatcher.forward(request, response); 
%>
```


+ FourRulesResult.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>사칙연산</TITLE></HEAD>
    <BODY>
        입력된 수: <%= request.getParameter("NUM1") %>,
             <%= request.getParameter("NUM2") %> <BR><BR>
     <hr>
       forward된 page와 이전페이지에서 reqeust객체를 공유하고 있으므로<br>
       forward된 page에서도 요청 파라메터를 당연히 사용할 수 있다.
     <hr>
        
        덧셈의 결과는? <%= request.getAttribute("SUM") %> <BR>
        뺄셈의 결과는? <%= request.getAttribute("DIFFERENCE") %> <BR>
        곱셈의 결과는? <%= request.getAttribute("PRODUCT") %> <BR>
        나눗셈의 결과는? <%= request.getAttribute("QUOTIENT") %> <BR>
        <hr>
        <%
        	int sum = (int)request.getAttribute("SUM");
        	int difference = (int)request.getAttribute("DIFFERENCE");
        	int product = (int)request.getAttribute("PRODUCT");
        	int quotient = (int)request.getAttribute("QUOTIENT");     
        	
        	out.println("덧셈의 결과는? "+sum+"<br>");
        	out.println("뺄셈의 결과는? "+difference+"<br>");
        	out.println("곱셈의 결과는? "+product+"<br>");
        	out.println("나눗셈의 결과는? "+quotient+"<br>");
        %>
    </BODY>
</HTML>
```

+ ChineseMenu.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>오늘의 메뉴</TITLE></HEAD>
    <BODY>
        <H3>오늘의 메뉴</H3>
        - 짜장면 <BR>
        - 볶음밥 <BR>
        - 짬뽕 <BR><BR>
        <hr>
        아래의 코드는 Now.jsp에서 출력된 내용입니다.<br>
        아래의 코드는 소스가 포함되는 것이 아니라 독립적으로 Now.jsp가 실행되고 <br>
        결과값만 받아오는 것이다.<br>
        include는 정적인 포함이 아니라 동적인 실행결과만을 가져온 것이다.<br>
        <hr>
        <%
            out.flush(); //강제출력버퍼 비우기(여기까지의 html태그내용이 응답)
            //아래의 코드로 request, response값을 공유함.
            RequestDispatcher dispatcher = request.getRequestDispatcher("Now.jsp");
            dispatcher.include(request, response); 
        %>
    </BODY>
</HTML>
```

+ Now.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="java.util.*"%>
<% 
	GregorianCalendar now = new GregorianCalendar(); 
%>
[현재의 시각] <%= String.format("%TF %TT ", now, now) %>
<hr>
<br>
위의 코드는 내부적으로 out.print(String.format(String.format("%TF %TT ", now, now)))실행
```

##### [오늘의 과제]
    - <form태그관련 서블릿 --> jsp 만들어보기!!

#### 5. 실습
#### 6. Summary / Close



-----------------------------------------------------------


### [2019-05-30]

#### 1. Review

#### 2. 쿠키와 세션
+ HTTP프로토콜의 비연결지향성을 보완하고자 만듦(지난정리 참고).

+ StoreCookies.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
	//쿠키의 생성및 사용은 서블릿과 동일
    //response.addCookie(new Cookie("NAME", "John"));
	Cookie c = new Cookie("NAME", "John");
	//필요하다면 c.setXXX()사용하여 부가정보를 저장.
	response.addCookie(c);
	
    response.addCookie(new Cookie("GENDER", "Male"));
    response.addCookie(new Cookie("AGE", "15"));
%>
<HTML>
    <HEAD><TITLE>쿠키 데이터 저장하기</TITLE></HEAD>
    <BODY>
        쿠키 데이터가 저장되었습니다.<BR><BR>
    </BODY>
</HTML>
```


+ ReadCookies.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<% 
	Cookie[] cookies = request.getCookies();
	if ( cookies != null) {
		out.println("전달된 쿠키정보는 다음과 같습니다.<br>");
		for ( int i=0; i<cookies.length; i++) {
			Cookie c = cookies[i];
			String cName = c.getName();
			String cValue = c.getValue();
			out.println(cName+" : "+cValue+"<br>");
		}
	} else {
		out.println("쿠키정보가 없습니다.<br>");
	}
	out.println("<hr>");
%>
<HTML>
    <HEAD><TITLE>쿠키 데이터 읽기</TITLE></HEAD>
    <BODY>
        이름: <%= getCookieValue(cookies, "NAME") %> <BR>
        성별: <%= getCookieValue(cookies, "GENDER") %> <BR>
        나이: <%= getCookieValue(cookies, "AGE") %>
    </BODY>
</HTML>
<%!
    private String getCookieValue(Cookie[] cookies, String name) {
        String value = null;
        if (cookies == null)
            return null;
        for (Cookie cookie : cookies) {
            if (cookie.getName().equals(name))
                return cookie.getValue();
        }
        return null;
    }
%>
```


+ 쿠키의 수정
    - 쿠키값 변경은 동일한 명칭의 쿠키를 만들어서 값을 설정하면 기존 쿠키값이 변경된다.

+ ModifyCookie.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
	//쿠키이름이 동일한 새로운 쿠키를 만들면 기존 쿠키의 내용이 수정되는 효과가 있다.
	response.addCookie(new Cookie("AGE", "16")); 
%>
<HTML>
    <HEAD><TITLE>쿠키 데이터 수정하기</TITLE></HEAD>
    <BODY>
        AGE 쿠키에 새로운 값이 저장되었습니다.<BR><BR>
    </BODY>
</HTML>
```



+ 쿠키의 삭제
    - 1) c.setMaxAge(분단위시간); ==> 지정한 시간 경과후 자동삭제
    - 2) c.setMaxAge(0); ==> 즉시 쿠키가 삭제된다.

+ DeleteCookie.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    Cookie cookie = new Cookie("GENDER", "");
    cookie.setMaxAge(0);
    response.addCookie(cookie);
%>
<HTML>
    <HEAD><TITLE>쿠키 삭제하기</TITLE></HEAD>
    <BODY>
        GENDER 쿠키가 삭제되었습니다.
    </BODY>
</HTML>
```



+ 

+ StoreJobCookie.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    Cookie cookie = new Cookie("JOB", "programmer");
	//위의 setPath에 의해 JOB쿠키는 /brain4/sub1/ 웹컴퍼넌트에 저장된다.
    cookie.setPath("/brain4/sub1/");
    response.addCookie(cookie);
%>
<HTML>
    <HEAD><TITLE>쿠키 데이터 저장하기</TITLE></HEAD>
    <BODY>
        JOB 쿠키가 저장되었습니다. <BR><BR>
    </BODY>
</HTML>
```

+ ReadJobCookie.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<% Cookie[] cookies = request.getCookies(); %>
<HTML>
    <HEAD><TITLE>쿠키 데이터 읽기</TITLE></HEAD>
    <BODY>
        JOB: <%= getCookieValue(cookies, "JOB") %> <BR>
    </BODY>
</HTML>
<%!
    private String getCookieValue(Cookie[] cookies, String name) {
        String value = null;
        if (cookies == null)
            return null;
        for (Cookie cookie : cookies) {
            if (cookie.getName().equals(name))
                return cookie.getValue();
        }
        return null;
    }
%>
```


#### 3. 예외처리

#### 4. 서블릿과의 관계

#### 5. EL

#### 6. 실습
#### 7. Summary / Close


-----------------------------------------------------------


### [2019-05-31]

#### 1. Review


#### 4. 실습
#### 5. Summary / Close




-----------------------------------------------------------


### [2019-06-03]

#### 1. Review


#### 4. 실습
#### 5. Summary / Close