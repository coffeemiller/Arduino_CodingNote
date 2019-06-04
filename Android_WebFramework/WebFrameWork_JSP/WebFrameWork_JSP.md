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
     pageContext
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
    pageContext
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
```
쿠키 예)

 사용자                                   게임서버(1단계~10단계)
   A   ---------------> 3단계
       <---level=3-----
       
       ----level=3----> 3단계까지 진행했으므로 4단계부터 시작
       
----------------------------------------------------- 
세션 예)
 사용자                                   게임서버(1단계~10단계)
   A   ---------------> 3단계(다양한 장비 구매)
     <--JSESSIONID=인식표-----      인식표----|저장 (setAttribute("key",값);
       
     ---JSESSIONID=인식표----> 인식표이용하여 상세데이타를 검색하여 식별
                                                getAttribute("key")  
-----------------------------------------------------
```

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



+ 쿠키가 전달될 path설정    

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



+ jsp에서의 세션
    - 1) `<@page ... session="true" %>`가 기본으로 설정되므로 세션객체는 자동생성.
        ==> 서블릿으로 바뀔때
        session = request.getSession("true");
        ==> 변환된 코드에서 관련있는 부분(_jspService(,))
        ```

        ```
    - 2) 자동으로 생성된다는 의미는
        - 전송된 JSESSIONID와 일치하는 세션 객체가 없을때는 생성.
        - 전송된 JSESSIONID와 일치하는 세션 객체가 있을때는 검색해서 리턴.
    - 3) 이때의 세션객체는 내장객체로 취급된다. ==> session
    - 4) session 내장객체에 값을 설정하거나, 읽어오거나, 제거하는 메서드만 사용.
        `session.setAttribute("key",값);`
        `session.setAttribute("key");`
        `session.removeAttribute("key");`



+ Food.html
```html
<HTML>
    <HEAD>
        <META http-equiv="Content-Type" content="text/html;charset=euc-kr">
        <TITLE>성격 테스트</TITLE>
    </HEAD>
    <BODY>
        <H3>좋아하는 음식은?</H3>
        <!-- ACTION=animal ==> /brain4/ptest/animal
         -->
        <FORM ACTION=/brain4/animal>
            <INPUT TYPE=TEXTFIELD NAME=FOOD>
            <INPUT TYPE=SUBMIT VALUE='확인'>
        </FORM>
    </BODY>
</HTML>
```


+ AnimalServlet.java
```java
package com.jica.brain4;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.io.*;

@WebServlet(description = "서블릿에서의 세션예제", urlPatterns = { "/animal" })
public class AnimalServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	public void doGet(HttpServletRequest request, 
			          HttpServletResponse response) 
                                  throws IOException, ServletException {
        
		response.setContentType("text/html;charset=euc-kr"); 
        PrintWriter out = response.getWriter();
        request.setCharacterEncoding("euc-kr");
        
		//요청파라메터값 얻기
        String food = request.getParameter("FOOD");
        
        //세션생성 -- 전송된 JSESSIONID없거나 , JSESSIONID와 일치하는 세션객체가 없을때 생성하여 리턴하고
        //                               JSESSIONID와 일치하는 세션객체가 있으면 검색하여 리턴
        HttpSession session = request.getSession();
        //디버깅용
        if(session.isNew()) {
        	System.out.println("AnimalServlet::==>" + session.getId() + " 세션객체 신규 생성");
        }else {
        	System.out.println("AnimalServlet::==>" + session.getId() + " 기존 세션객체 사용");
        }
        //세션에 속성값 설정
        session.setAttribute("FOOD", food); 

        out.println("<HTML>");
        out.println("<HEAD><TITLE>성격 테스트</TITLE></HEAD>");
        out.println("<BODY>");
        out.println("<H3>좋아하는 동물은?</H3>");
        out.println("<FORM ACTION=/brain4/result>");
        out.println("<INPUT TYPE=TEXTFIELD NAME=ANIMAL>");
        out.println("<INPUT TYPE=SUBMIT VALUE='확인'>");
        out.println("</FORM>");
        out.println("</BODY>");
        out.println("</HTML>");
    }
}
```


+ ResultServlet.jsp
```jsp
package com.jica.brain4;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.io.*;

@WebServlet(description = "서블릿에서의 세션예제", urlPatterns = { "/result" })
public class ResultServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	public void doGet(HttpServletRequest request, HttpServletResponse response) 
                                  throws IOException, ServletException {
      
        response.setContentType("text/html;charset=euc-kr"); 
        PrintWriter out = response.getWriter();
        request.setCharacterEncoding("euc-kr");
        
        //요청 파라메터값 얻기
        String animal = request.getParameter("ANIMAL");
        
        //이전 AnimalServlet에서 저장했던 음식명 구하기
		HttpSession session = request.getSession();
        if(session.isNew()) {
        	System.out.println("ResultServlet::==>" + session.getId() + " 세션객체 신규 생성");
        }else {
        	System.out.println("ResultServlet::==>" + session.getId() + " 기존 세션객체 사용");
        }
        String food = (String) session.getAttribute("FOOD");

        //세션을 제거하자
        session.invalidate();

        out.println("<HEAD><TITLE>성격 테스트</TITLE></HEAD>");
        out.println("<BODY>");
        out.println("<H3>성격 테스트</H3>");
        out.printf("당신은 %s와 %s를 좋아하는 성격입니다.", food, animal);
        out.println("</BODY>");
        out.println("</HTML>");
    }
}
```



#### 3. 예외처리
+ Java에서의 예외처리 --> Servlet도 java코드이므로 동일하게 사용.
```
예외의 종류
        Object
        Throwable
   Error       Exception(예외) -- 복구가능하거나 최소한 정상적으로 종료시킬수 있다.   
하위클래스          IOException   ==>반드시 2가지 예외처리방법으로  예외를
                    ...                 처리해야 *.class를 만들수 있다.
                    RuntimeException      -------|  
                       NullPointException        | 프로그래머의 실수에 의한 예외
                       ArithmeticException       | 특별히 예외처리를 해주지 않아도 
                       ArrayIndexOuofException   | 실행은 시킬수 있다.
                       NumberFormatException
                       ...               --------| 


2가지 예외처리 방법
1) 예외 던지기
    void method() throws 예외클래스 {
    ...
    예외발생 가능성이 있는 코드
    ...
    }

2) 직접 예외처리하기
    void method() {
    ...
    try {
        예외발생 가능성이 있는 코드
    } catch (예외클래스 객체) {
        예외처리코드(복구코드)
    }
    ...
    }
```

+ Adder.java
```java
package com.jica.brain5;

public class Adder {
    public static void main(String args[]) {
        try {
            int num1 = Integer.parseInt("5");
            int num2 = Integer.parseInt("7");
            int result = num1 + num2;
            System.out.printf("%d + %d = %d", num1, num2, result);
        }
        catch (NumberFormatException e) {
            System.out.println("잘못된 데이터가 입력되었습니다.");
        }
    }        
}
```



+ jsp에서의 예외처리
```
1) <% 자바코드 - 예외처리 %>
   <%!
        리텀값 메서드명() {
            실행코드 -- 예외처리
        }
   %>

2) html태그 + jsp코드 + 자바코드 => 위의 자바코드에서의 예외처리방식을 사용하면                                                             코드가 복잡(지져분해진다)

    그래서 개선책으로 별도의 에러처리결과만을 보여주는 에러페이지를 만들어서 명시적으로 forward시킬수 있다. 단, 예외가 발생한 원인을 접근하려고 하면... 즉, exception 내장객체를 사용하려고 하면 NullPointException이 발생한다.
================================================================================
* forward에 의한 명시적 호출이 아니라 다음의 page지시어를 사용한다.
  <%@ page ... errorPage="에러결과출력전담페이지" %>
  해당 페이지에서 예외가 발생하면 자동으로 에러결과출력전담페이지가 작동하고
  이곳에서는 exception 내장객체를 정상적으로 사용할 수 있다.
================================================================================

3) 설정파일에 미리 예외정보를 등록시켜서 특정 예외가 발생하면 자동으로 호출되도록 할 수 있다.
   (web.xml)
    1) 예외종류별로 등록
       NullPointerException  ---->  NullException.jsp
       NumberFormatException ---->  NumberFormatError.jsp
    2) 최종응답코드별로 등록
       404 최종응답 -------> 별도의 웹페이지 
```

+ AdderServlet.java
```java
package com.jica.brain5;

import javax.servlet.http.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;

import java.io.*;

@WebServlet(description = "서블릿에서 예외처리 예제", urlPatterns = { "/AdderServlet" })
public class AdderServlet extends HttpServlet {
    
	private static final long serialVersionUID = 1L;

	public void doGet(HttpServletRequest request, HttpServletResponse response) 
                             throws IOException, ServletException {
        
		//요청파라메터 얻기
		String str1 = request.getParameter("NUM1");
        String str2 = request.getParameter("NUM2");
        
        //출력스트림 얻기
        response.setContentType("text/html;charset=euc-kr"); 
        PrintWriter out = response.getWriter();
        
        try {
            int num1 = Integer.parseInt(str1);
            int num2 = Integer.parseInt(str2);
            int result = num1 + num2;
            out.println("<HTML>");
            out.println("<HEAD><TITLE>덧셈 프로그램</TITLE></HEAD>");
            out.println("<BODY>");
            out.printf("%d + %d = %d", num1, num2, result);
            out.println("</BODY>");
            out.println("</HTML>");
        }
        catch (NumberFormatException e) {
            out.println("<HTML>");
            out.println("<HEAD><TITLE>덧셈 프로그램 - 에러 화면</TITLE></HEAD>");
            out.println("<BODY>");
            out.println("잘못된 데이터가 입력되었습니다.");
            out.println("</BODY>");
            out.println("</HTML>");
        }
    }        
}
```

+ Adder_old.jsp.... 지저분한 코딩의 예!! 
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    try {
        String str1 = request.getParameter("NUM1");
        String str2 = request.getParameter("NUM2");
        int num1 = Integer.parseInt(str1);
        int num2 = Integer.parseInt(str2);
        int result = num1 + num2;
%>
        <HTML>
            <HEAD><TITLE>덧셈 프로그램</TITLE></HEAD>
            <BODY>
                <%= num1 %> + <%= num2 %> = <%= result %>
            </BODY>
        </HTML>
<% 
    } 
    catch (NumberFormatException e) {
%>
        <HTML>
            <HEAD><TITLE>덧셈 프로그램 - 에러 화면</TITLE></HEAD>
            <BODY>
                잘못된 데이터가 입력되었습니다.
            </BODY>
        </HTML>
<%
    }
%>
```
+ DataError_old.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>덧셈 프로그램 - 에러 화면</TITLE></HEAD>
    <BODY>
        잘못된 데이터가 입력되었습니다. 
    </BODY>
</HTML>
```







+ Adder.jsp..... 간결한 코딩의 예!!  그러나...exception 객체를 읽을수 X!!
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    int num1 = 0, num2 = 0, result = 0;
    try {
        String str1 = request.getParameter("NUM1");
        String str2 = request.getParameter("NUM2");
        num1 = Integer.parseInt(str1);
        num2 = Integer.parseInt(str2);
        result = num1 + num2;
    } 
    catch (NumberFormatException e) {
        RequestDispatcher dispatcher = request.getRequestDispatcher("DataError.jsp");
        dispatcher.forward(request, response);
    }
%>
<HTML>
    <HEAD><TITLE>덧셈 프로그램</TITLE></HEAD>
    <BODY>
        <%= num1 %> + <%= num2 %> = <%= result %>
    </BODY>
</HTML>
```

+ DataError.jsp ==> page지시어에 'true'을 주고, setStatus를 200으로 정상처리~
```jsp
<%@page contentType="text/html; charset=euc-kr" isErrorPage="true" %>
<% response.setStatus(200); %>
<HTML>
    <HEAD><TITLE>덧셈 프로그램 - 에러 발생</TITLE></HEAD>
    <BODY>
        잘못된 데이터가 입력되었습니다. <BR><BR>
        상세 에러 메시지: <%= exception.getMessage() %>
        <hr>
        exciption 내장객체를 사용하여 원인을 출력하고 있다.<br>
        그런데 현재의 코드에서는 내부에러 즉, exception객체를 인식하지 못한다는 <br>
        NullporintException이 발생했다.<br>
        이것을 해결하려면 예외가 발생한 페이지에서 (Adder.jsp) 인위적으로<br>
        forward시키지 말고 페이지 지시어를 사용하여 에러처리페이지를 명시적으로<br>
        지정하면 문제를 해결할 수 있다.<br>
        즉, exception 내장객체는 에러페이지일때만 생성되는 객체다.
    </BODY>
</HTML>
```



+ NewAdder.jsp  ==> page 지시어를 이용해서 'error'전담 페이지로 이동!!
```jsp
<%@page contentType="text/html; charset=euc-kr" errorPage="DataError.jsp" %>
<%
    String str1 = request.getParameter("NUM1");
    String str2 = request.getParameter("NUM2");
    int num1 = Integer.parseInt(str1);
    int num2 = Integer.parseInt(str2);
    int result = num1 + num2;
%>
<HTML>
    <HEAD><TITLE>덧셈 프로그램</TITLE></HEAD>
    <BODY>
        <%= num1 %> + <%= num2 %> = <%= result %>
    </BODY>
</HTML>
```



+ web.xml을 이용한 예외처리

+ Multiplyer.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    String str1 = request.getParameter("NUM1");
    String str2 = request.getParameter("NUM2");
    int num1 = Integer.parseInt(str1);
    int num2 = Integer.parseInt(str2);
    int result = num1 * num2;
%>
<HTML>
    <HEAD><TITLE>곱셈 프로그램</TITLE></HEAD>
    <BODY>
        <%= num1 %> * <%= num2 %> = <%= result %>
    </BODY>
</HTML>
```



+ Divider.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    String str1 = request.getParameter("NUM1");
    String str2 = request.getParameter("NUM2");
    int num1 = Integer.parseInt(str1);
    int num2 = Integer.parseInt(str2);
    int result = num1 / num2;
%>
<HTML>
    <HEAD><TITLE>나눗셈 프로그램</TITLE></HEAD>
    <BODY>
        <%= num1 %> / <%= num2 %> = <%= result %>
    </BODY>
</HTML>
```



+ NumberFormatError.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" isErrorPage="true" %>
<% response.setStatus(200); %>
<HTML>
    <HEAD><TITLE>숫자 포맷 에러</TITLE></HEAD>
    <BODY>
        숫자 포맷이 잘못되었습니다. <BR><BR>
        상세 에러 메시지: <%= exception.getMessage() %>
    </BODY>
</HTML>
```


+ NotFoundError.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<% response.setStatus(200); %>
<HTML>
    <HEAD><TITLE>페이지 없음 에러</TITLE></HEAD>
    <BODY>
        해당 페이지를 찾을 수 없습니다. 
    </BODY>
</HTML>
```


+ ServerInternalError.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<%response.setStatus(200);%>
<HTML>
    <HEAD><TITLE>웹 서버 에러</TITLE></HEAD>
    <BODY>
        웹 서버 내부에서 에러가 발생했습니다.<br>
        서버를 갱신중이므로 오전 02:00~04:00까지 사용할 수 없습니다.
    </BODY>
</HTML>
```


+ PrimeNumbers.jsp
```jsp
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>brain5</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

   <!-- 예외종류별 예외페이지 등록 -->
    <error-page>
        <exception-type>java.lang.NumberFormatException</exception-type>
        <location>/NumberFormatError.jsp</location>
    </error-page>
    <error-page>
        <exception-type>java.lang.ArithmeticException</exception-type>
        <location>/ArithmeticError.jsp</location>
    </error-page>
    
   <!-- 최종응답코드별로 등록 -->
   <error-page>
   		<error-code>404</error-code>
   		<location>/NotFoundError.jsp</location>
   </error-page>
   <error-page>
   		<error-code>500</error-code>
   		<location>/ServerInternalError.jsp</location>
   </error-page>
</web-app>
```



+ web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>brain5</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

<!-- 예외종류별 예외페이지 등록 -->
    <!-- 숫자가 아닌 문자 에러 -->
    <error-page>
        <exception-type>java.lang.NumberFormatException</exception-type>
        <location>/NumberFormatError.jsp</location>
    </error-page>
    <!-- 0으로 나눌때 에러 -->
    <error-page>
        <exception-type>java.lang.ArithmeticException</exception-type>
        <location>/ArithmeticError.jsp</location>
    </error-page>

    <!-- 최종응답코드별로 등록 -->
    <error-page>
    		<error-code>404</error-code>
    		<location>/NotFoundError.jsp</location>
    </error-page>
    <error-page>
    		<error-code>500</error-code>
    		<location>/ServerInternalError.jsp</location>
    </error-page>
</web-app>
```


##### [오늘의 과제]
+ basic/com.jica.guestbook2  ---> `*.java`
+ guestbook2/GuestBookWrite.html  ----> GuestBookWrite.jsp / GuestBookRead.jsp


#### 4. 실습
#### 5. Summary / Close


-----------------------------------------------------------


### [2019-05-31]

#### 1. Review

#### 2. 서블릿과의 관계
+ 서블릿의 생명주기
```
--------최초요청---->    1) 요청한 서블릿객체를 검색 --> 없다
                        - 서블릿코드를 메모리로 적제(LOAD)
                        - 서블릿객체 생성 --인스턴스화 --> 생성자()
                        - 초기화작업 --> init()
--------이후요청--->    2) 요청한 서블릿객체가 있다.(쓰레드단위로 실행)
                        - service() --> doGet()/doPost()
<-------응답-----------  -------------작업결과----------------------------
                        1) 서블릿코드의 유지보수
                        - 마무리 작업 ---> destroy()
                        - 객체소멸
```

+ JSP의 생명주기
```
 jsp--------------변환--------------> Servlet
 Adder.jsp                          class Adder_jsp extends HttpJspBase
                                    <%! %> --> 사용자가 만든 멤버변수, 메서드

<%! void jspInit(){                 멤버변수와 메서드들
     초기화                         public void _jspInit() {   }
    }
    void jspDestroy(){              public void _jspDestroy() {    }
     마무리
    }                               public void _jspService(HttpServletRequest request,
%>                                                          HttpServletResponse response)
                                                            throws IOException, 
                                                            ServletException
                                    1) 요청방식 확인
                                    2) 내장객체 선언 및 생성
                                    3) 순서대로 html태그 --> 그대로 출력
                                        scriptlet, expression -> 그대로 포함, 출력표현
                                        EL, 액션태그, JSTL -> 독립적 메서드 호출
```

+ FibonacciServlet.java
```java
package com.jica.brain6;

import javax.servlet.http.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;

import java.io.*;
import java.math.BigInteger;

@WebServlet(description = "서블릿 생명주기 예제", urlPatterns = { "/fibonacci" })
public class FibonacciServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	//멤버변수
    private BigInteger arr[];
    
    
    
    public FibonacciServlet() {
    	System.out.println("FibonacciServlet::FibonacciServlet()...."+this);
	}


	//초기화 메서드
    public void init() {
    	System.out.println("FibonacciServlet::init()...."+this);
 
        arr = new BigInteger[100];
        arr[0] = new BigInteger("1");
        arr[1] = new BigInteger("1");
        for (int cnt = 2; cnt < arr.length; cnt++) {
            arr[cnt] = arr[cnt-2].add(arr[cnt-1]);
        }
    }
    
    
    public void doGet(HttpServletRequest request, HttpServletResponse response) 
                             throws IOException, ServletException {
    	System.out.println("FibonacciServlet::doGet(,)...."+this);
    	
    	//기본준비
    	response.setContentType("text/html;charset=euc-kr");
    	response.setCharacterEncoding("EUC-kr");
    	PrintWriter out = response.getWriter();
    	
    	//요청파라메터 얻기
        String str = request.getParameter("NUM");
        int num = Integer.parseInt(str);
        if (num > 100)
            num = 100;
        
        out.println("<HTML>");
        out.println("<HEAD><TITLE>피보나치 수열</TITLE></HEAD>");
        out.println("<BODY>");
        for (int cnt = 0; cnt < num; cnt++)
            out.println((cnt+1)+" : "+arr[cnt] + " <br>");
        out.println("</BODY>");
        out.println("</HTML>");
    }


	@Override
	public void destroy() {
		System.out.println("FibonacciServlet::destroy()...."+this);
		super.destroy();
	}           
}
```

+ YourName.html
```html
<HTML>
    <HEAD>
        <META http-equiv="Content-Type" content="text/html;charset=utf-8">
        <TITLE>이름 입력</TITLE>
    </HEAD>
    <BODY>
        <H3>이름을 입력하십시오.</H3>
        <FORM ACTION=greeting>
            이름: <INPUT TYPE=TEXT NAME=NAME>
            <INPUT TYPE=SUBMIT VALUE='확인'>
        </FORM>
    </BODY>
</HTML>
```

+ GreetingServlet.java
```java
package com.jica.brain6;

import javax.servlet.http.*;
import javax.servlet.*;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;

import java.io.*;
import java.util.*;


@WebServlet(
		description = "초기파라메터 예제", 
		urlPatterns = { "/greeting" }, 
		initParams = { 
				@WebInitParam(name = "FILE_NAME", value = "C:\\data\\greeting_log.txt", description = "로그기록"), 
		})


public class GreetingServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	//멤버변수
    private PrintWriter logFile;
    
    public void init() throws ServletException {
        String filename = getInitParameter("FILE_NAME");
        try {
            logFile = new PrintWriter(new FileWriter(filename, true));
        }
        catch (IOException e) { 
            throw new ServletException(e);
        }
    }
    
    public void doGet(HttpServletRequest request, HttpServletResponse response) 
                             throws IOException, ServletException {
    	//기본준비작업
    	response.setContentType("text/html;charset=euc-kr");
    	response.setCharacterEncoding("EUC-KR");
    	PrintWriter out = response.getWriter();
    	
    	//요청파라메터 얻기
        String name = request.getParameter("NAME");
        String greeting = "안녕하세요, " + name + "님.";
        if (logFile != null) {
            GregorianCalendar now = new GregorianCalendar();
            logFile.printf("%TF %TT  - %s %n", now, now, name);
        }
        
        out.println("<HTML>");
        out.println("<HEAD><TITLE>인사하기</TITLE></HEAD>");
        out.println("<BODY>");
        out.println(greeting);
        out.println("방문기록이 저장되었습니다.<br>");
        out.println("</BODY>");
        out.println("</HTML>");
    }        
    
    public void destroy() {
        if (logFile != null)
            logFile.close();
    }
}
```

+ DateTime.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" import="java.io.*, java.util.*" %>
<%!
	//멤버변수 선언
    private PrintWriter logFile;

	//초기화메서드
    public void jspInit() {
        String filename = "c:\\data\\datetime_log.txt";
        try {
            logFile = new PrintWriter(new FileWriter(filename, true));
        }
        catch (IOException e) {
            System.out.printf("%TT - %s 파일을 열 수 없습니다. %n", new GregorianCalendar(), filename);
        }
    }
%>
<HTML>
    <HEAD><TITLE>현재의 날짜와 시각</TITLE></HEAD>
    <BODY>
    <%
    	//날짜(시간)정보얻기
        GregorianCalendar now = new GregorianCalendar();
		//날짜/시간 출력형식
        String date = String.format("현재 날짜: %TY년 %Tm월 %Te일", now, now, now);
        String time = String.format("현재 시각: %TI시 %Tm분 %TS초", now, now, now);
		
        out.println(date + "<BR>");
        out.println(time + "<BR>");
        out.println("jspInit(), jspDestroy() 연습중!");
        
        if (logFile != null)
            logFile.printf("%TF %TT에 호출되었습니다.%n", now, now);
    %>
    </BODY>
</HTML>      
<%!
    public void jspDestroy() {
        if (logFile != null)
            logFile.close();
    }
%>
```


+ 서블릿에서의 초기화파라메터 전달
    1) web.xml
    2) @WebServlet(,,initParams = { @WebInitParam(name = "FILE_NAME", value = "C:\\log.txt")})


+ jsp에서의 초기화파라메터 전달 - web.xml

+ 참고)
    - 개별 서블릿에 전달하는 초기화파라메터 getInitParameter("파라메터명")
    - 웹어플리케이션 전체의 초기화파라메터 application.getInitParameter("파라메터명")



+ getInitParameter()메서드는 init()메서드 이외의 다른 곳에서도 자유롭게 사용할 수 있다.

+ web.xml
```xml
   <servlet>
        <servlet-name>dbname-jsp</servlet-name>
        <jsp-file>/DBName.jsp</jsp-file>
        <init-param>
            <param-name>DB_NAME</param-name>
            <param-value>jicadb</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>dbname-jsp</servlet-name>
        <url-pattern>/dbname</url-pattern>
    </servlet-mapping>
```
+ `/dbname`으로 호출시 `null`아닌 초기값이 등록.


+ DBName.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>초기화 파라미터 테스트</TITLE></HEAD>
    <BODY>
        <% String dbName = getInitParameter("DB_NAME"); %>
        데이터베이스 이름: <%= dbName %> <BR>
    </BODY>
</HTML>    
```


+ jsp파일은 web.xml에 별도로 등록하지 않아도 원래의 명칭을 호출할 수 있다.
    단, 초기화파라메터가 필요할때는 web.xml등록하여야 한다.
    또한 호출하는 명칭 즉, url-pattern을 다르게 등록할수도 있다.


+ web.xml -> 추가
```xml
    <servlet-mapping>
        <servlet-name>dbname-jsp</servlet-name>
        <url-pattern>/DBName.jsp</url-pattern>
    </servlet-mapping>
    <servlet>
        <servlet-name>server-info-servlet</servlet-name>
        <servlet-class>com.jica.brain6.ServerInfoServlet</servlet-class>
        <load-on-startup>0</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>server-info-servlet</servlet-name>
        <url-pattern>/server-info</url-pattern>
    </servlet-mapping>
```

+ ServerInfo.java
```java
package com.jica.brain6;

import javax.servlet.http.*;
import javax.servlet.*;
import java.io.*;

//web.xml에 필요정보를 설정했으므로 @WebServlet() Annotation
public class ServerInfoServlet extends HttpServlet {
	
	private static final long serialVersionUID = 1L;
	
    public void doGet(HttpServletRequest request, HttpServletResponse response) 
                             throws IOException, ServletException {
    	
    	response.setContentType("text/html;charset=euc-kr"); 
    	PrintWriter out = response.getWriter();
    	
    	//현재 서블릿이 동작하는 웹어플리케이션(brain6)의
    	//모든 실행상태정보를 관리
        ServletContext context = getServletContext();
        //JSP에서는 내장객체로 application이라는 명칭으로 사용한다.
        
        String serverInfo = context.getServerInfo();
        int majorVersion = context.getMajorVersion();
        int minorVersion = context.getMinorVersion();
        
        out.println("<HTML>");
        out.println("<HEAD><TITLE>웹 서버의 정보</TITLE></HEAD>");
        out.println("<BODY>");
        out.printf("웹 서버의 종류: %s <BR>", serverInfo);
        out.printf("지원하는 서블릿 버전: %d.%d <BR>", majorVersion, minorVersion);
        out.println("</BODY>");
        out.println("</HTML>");
    }        
}
```

+ ServerInfo.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>웹 서버의 정보</TITLE></HEAD>
    <BODY>
    <hr>
    	서블릿에서의 servletContext객체가 JSP에서는<br>
    	application 내장객체로 이미 준비되어 있다.
    <hr>
        웹 서버의 종류: <%= application.getServerInfo() %> <BR>
        지원하는 서블릿 버전: <%= application.getMajorVersion() %>.<%= application.getMinorVersion() %> <BR>
    </BODY>
</HTML>    
```


+ web.xml  --> 추가
```xml
    <context-param>
        <param-name>DB_NAME</param-name>
        <param-value>bankdb</param-value>
    </context-param>
```

+ AppParamTest.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>웹 애플리케이션의 초기화 파라미터</TITLE></HEAD>
    <BODY>
    <hr>
    	웹어플리케이션 전체의 초기화 파라메터는<br>
    	application.getInitParameter("파라메터명");을 사용한다.<br>
    	단, web.xml에 최기화 파라메터를 등록해야 한다.
    <hr>
         데이터베이스 이름: <%= application.getInitParameter("DB_NAME") %> <BR>
    </BODY>
</HTML>
```



+ Hello.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>인사하기</TITLE></HEAD>
    <BODY>
        안녕하세요, <%= request.getParameter("NAME") %>님
        <%
        	application.log("[인사하기] JSP 페이지가 호출되었습니다. ");
        	System.out.println("[인사하기] JSP 페이지가 호출되었습니다. ");
        %>
    </BODY>
</HTML>
```



+ scope를 가지는 jsp의 내장객체들
    - 특정값을 저장/유지/삭제하는 주기
    ```
    setAttribute("키",값)
    getAttribute("키")
    removeAttribute("키")
    ```
    1) pageContext -- 현재페이지에서만 정보유지
    2) request     -- 요청이 가고 최종응답이 올대까지(include/forward)
    3) session     -- 세션객체가 유지되는 동안
    4) applicaion  -- 서버가 시작되고 끝날때까지 정보유지


+ StoreName.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    String name = request.getParameter("NAME");
    application.setAttribute("NAME", name); 
%>
<HTML>
    <HEAD><TITLE>웹 애플리케이션 데이터 저장하기</TITLE></HEAD>
    <BODY>
         NAME 데이터가 저장되었습니다.          
    </BODY>
</HTML>
```

+ ReadName.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>웹 애플리케이션 데이터 조회하기</TITLE></HEAD>
    <BODY>
         이름: <%= application.getAttribute("NAME") %>
    </BODY>
</HTML>
```

+ DeleteName.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<% application.removeAttribute("NAME"); %>
<HTML>
    <HEAD><TITLE>웹 애플리케이션 데이터 삭제하기</TITLE></HEAD>
    <BODY>
         NAME 데이터가 삭제되었습니다.   
    </BODY>
</HTML>
```



#### 3. EL(Expression Language)
+ 1) 최종응답 코드 즉, 출력을 목적으로 한다.
+ 2) ${표현} <== EL
    ----> 자바변수나 코드는 사용할 수 없다.
        (0) 속성명 -- scope를 가지는 내장객체의 속성명
            ${ RESULT }

        (1) JSP의 내장객체는 사용하지 못하고...자체의 내장객체를 사용한다.
            ${ requestScope.RESULT }
            pageScopre, requestScope, sessionScope, applicationScope
            ${ param.NAME }
            param, paramValue
            header, headerValue
            cookie
            initParam
            pageContext(특별한 목적 : getXXX()메서드 호출목적)

        (2) 연산자를 사용할 수 있다.
            산술연산자, 관계연산자, 논리연산자, 삼항연산자(?:), empty, [], ., ()

        (3) 사용자가 만든 객체의 getMethod호출기능

        (4) 특정클래스의 static 메서드만 호출할 수 있다.=>별도의 표기법
            ${ m:sqrt(4) }  ==> java.lang.Math.sqrt(4)

    ${ cnt + 1 }
        ===> 자체내장객체의 속성명 (ok)



+ 서블릿이나 JSP에서 Logic을 처리하고 그결과값이 만들어졌을때, 결과값을 직접 사용하는 것이 아니라, scope를 가진 내장객체에 속성으로 값을 저장한 후.... EL표현에서 사용한다.

```
[ scope를 가지는 jsp의 내장객체들 ]
- 특정값을 저장/유지/삭제하는 주기

setAttribute("키",값)
getAttribute("키")
removeAttribute("키")

1) pageContext -- 현재페이지에서만 정보유지
2) request     -- 요청이 가고 최종응답이 올대까지(include/forward)
3) session     -- 세션객체가 유지되는 동안
4) applicaion  -- 서버가 시작되고 끝날때까지 정보유지

===========================================================================
<%
    int total = 0;
    for(in cnt=1; cnt<=100; cnt++) {
        total += cnt;
    }
    pageContext.setAttribute("TOTAL",total);
    //방법1)
    out.println("100까지의 합 : "+total+"<br>);
$>
//방법2)
100까지의 합 : <%= total %><br>
100까지의 합 : <%= request.getAtrribute("TOTAL") %><br>  ==> Expression

//방법3) EL표현
100까지의 합 : ${ TOTAL }<br>  -- ok (속성명)
100까지의 합 : ${ total }<br>  -- error
100까지의 합 : ${ request.getAttribute("TOTAL") }<br>  -- error
100까지의 합 : ${ requestScope.TOTAL }<br>  -- ok (자체내장객체명.속성명)
==========================================================================
```

+ Hundred.jsp
```jsp
<%
	//logic처리만 전담하고 최종결과를 보여주는 별도의 페이지로 제어이동
	//forward를 통해 전해준다.
    int sum = 0;
    for (int cnt = 1; cnt <= 100; cnt++){
        sum += cnt;
    }
    
    RequestDispatcher dispatcher = request.getRequestDispatcher("HundredResult.jsp");
    request.setAttribute("RESULT", new Integer(sum));
    dispatcher.forward(request, response); 
%>
```

+ HundredResult.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>1부터 100까지의 합</TITLE></HEAD>
    <BODY>
    <%
    	//Integer iSum = request.getAttribute("RESULT");
    	//int sum = iSum.intValue();
    	
    	int sum = (int)request.getAttribute("RESULT");
    	out.println("자바코드 사용 - scriptlet<br>");
    	out.println("1부터 100까지 더한 결과는? "+sum);
    %>
    <hr>
    	Expression 사용<br>
        1부터 100까지 더한 결과는? <%= request.getAttribute("RESULT") %><br>
    <hr>
        EL 사용<br>
        1부터 100까지 더한 결과는? ${RESULT}<br>
        ---------------------------------------------------<br>
        위의 EL표현은 내부적으로 scope를 가진 내장객체들의 속성에서<br>
        RESULT명으로 저장된 값이 있는지를 차례로 검사하여,<br>
        일치하는 속성명이 존재하면 값을 추출하여 출력시키고
        <br> 없으면 아무일도 하지 않는다.
    </BODY>
</HTML>
```


+ EL의 실제 사용예제
```
EL 자체사용
 1) ${ 속성명 }

 2) ${ pageScope.속성명 }
    ${ requestScope.속성명 }
    ${ sessionScope.속성명 }
    ${ applicationScope.속성명 }

 3) ${ param.파라메터명 }
    ${ paramValues.파라메터명[0] }

 4) ${ header.헤더명 }, ${ header["헤더명"] } 
                -----> 헤더명에 영문자,숫자,$,_가 아닌 특수문자가 포함되어 있을때
                                 반드시 지금형태로 사용해야 한다.      
    ${ headerValues.헤더명[0] }, ${ headerValues["헤더명"][0] } 

 5) ${ cookie.쿠키명 }        ==> Cookie객체의 참조값 출력
    ${ cookie.쿠키명.value }  ==> 쿠키의 값이 출력
    ${ cookie["쿠키명"].value }
    ${ cookie.쿠키명["value"] }
    ${ cookie.["쿠키명"]["value] }
    ========================================
     ${ cookie.쿠키명.path }
     ${ cookie.쿠키명.maxAge }
    <%
        Cookie cookies[] = request.getCookies();
        if(cookies != null) {
            for(Cookie c : cookies) {
                String cName = c.getName(); //쿠키명
                String cValue = c.getValue(); //쿠키값
                String cPath = c.getPath(); //경로
                int max = c.getMaxAge;  //시간
            }
        }
    %>

 6) ${ initParam.파라메터명 }

 7) ${ pageContext.get을 제외한 메서드명 } ==> 아주 예외적인 코드가 가능
    자바에서 get메서드호출을 수행하는 코드를 EL로 표현할 수 있다.
    pageContext.getRequest().getRequestURI() --> EL표현가능
    ${ pageContext.request.requestURI } 

```


+ Thousand.jsp
```jsp
<%@page import="java.nio.channels.SeekableByteChannel"%>
<%@page contentType="text/html; charset=euc-kr"%>
<%
    int sum = 0;
    for (int cnt = 1; cnt <= 1000; cnt++)
        sum += cnt;
    
    //scope를 가진 내장객체들에 속성명을 정하여 값을 저장했다.
    //pageContext.setAttribute("RESULT", new Integer(sum)); // -- pageContext
    //request.setAttribute("RESULT", new Integer(sum));  // -- request
    //session.setAttribute("RESULT", new Integer(sum));  // -- 세션
    //application.setAttribute("RESULT", new Integer(sum));
    
    
    //같은 속성명이 여러개라면???
    //pageContext.setAttribute("RESULT", new Integer(sum));
    //request.setAttribute("RESULT", new Integer(sum+100));
    //session.setAttribute("RESULT", new Integer(sum+500));
    //application.setAttribute("RESULT", new Integer(sum+1000));
    
    //---?
    pageContext.setAttribute("RESULT", "pageContext의 속성값");
    request.setAttribute("RESULT", "request의 속성값");
    session.setAttribute("RESULT", "session의 속성값");
    application.setAttribute("RESULT", "application의 속성값");
    
%>

<HTML>
    <HEAD><TITLE>1부터 1000까지의 합</TITLE></HEAD>
    <BODY>
        1부터 1000까지 더한 결과는? ${RESULT}<br>
        ${ pageScope.RESULT }<br>
        ${ requestScope.RESULT }<br>
        ${ sessionScope.RESULT }<br>
        ${ applicationScope.RESULT }<br>
        
        <%
        	session.invalidate(); 
        %>
        EL내부표현에 속성명이 단독으로 나타나면<br>
        pageContext -> request -> session -> application 내장객체의 순서로<br>
        차례로 해당 속성명이 존재하는지 검사하여 값을 얻어온다.<br>
        만약 없다면 아무일도 하지 않는다.
        <hr>
        만일 공교롭게도 내장객체의 속성명이 동일한 것이 존재하면<br>
        scope가 작은 내장객체의 속성 값이 출력된다.<br>
        특정내장객체의 속성을 지정하고 싶다면 EL내부에서만 별도로<br>
        지칭할 수 있는 명칭을 만들었다.<br>
        <ul>
        <li> pageContext --> pageScope
        <li> request ------> requestScope
        <li> session ------> sessionScope
        <li> application --> applicationScope
        </ul>
    </BODY>
</HTML>
```

+ PetsInput.html
```html
<HTML>
    <HEAD>
        <META http-equiv="Content-Type" content="text/html;charset=utf-8">
        <TITLE>아이 러브 펫</TITLE>
    </HEAD>
    <BODY>
    	<FORM ACTION=PetsResult.jsp>
          아이디: <INPUT TYPE=TEXT NAME=ID><BR><BR>
          다음 중 회원님이 키우고 있는 애완 동물을 선택하십시오.<BR><BR>
            개<INPUT TYPE=CHECKBOX NAME=ANIMAL VALUE="개">
            고양이<INPUT TYPE=CHECKBOX NAME=ANIMAL VALUE="고양이">
            금붕어<INPUT TYPE=CHECKBOX NAME=ANIMAL VALUE="금붕어"><BR><BR>
          <INPUT TYPE=RESET VALUE="취소">
          <INPUT TYPE=SUBMIT VALUE="확인">
        </FORM>
    </BODY>
</HTML>
```


+ PetsResult.jsp
```jsp
<%@page contentType="text/html; charset=utf-8"%>
<HTML>
    <HEAD><TITLE>아이 러브 펫</TITLE></HEAD>
    <BODY>
    <%
    	String id = request.getParameter("ID");
		String animals[] = request.getParameterValues("ANIMAL");
		
		out.println("아이디 : "+id+"<br>");
		out.print("선택한 동물 : ");
		if(animals != null) {
			for(String animal : animals) {
				out.print(animal + " ");
			}
		}
    %>
    <br>위의 코딩은 기존 자바코딩으로...
    <hr>
        아이디: ${param.ID} <BR>
        선택한 동물: ${paramValues.ANIMAL[0]} ${paramValues.ANIMAL[1]} ${paramValues.ANIMAL[2]}
    </BODY>
</HTML>
```



+ Header.html
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
	<title>요청 헤더정보 확인</title>
</head>
<body>
	<center><h2>Heder정보 테스트</h2></center>
	<br>
	<a href="/brain7/Header">헤더정보 확인 - 서블릿</a><br><br>
	<a href="/brain7/Header.jsp">헤더정보 확인 - JSP(EL)</a>
</body>
</html>
```


+ Header.java
```html
package com.jica.brain7;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(description = "요청header정보 확인 예제", urlPatterns = { "/Header" })
public class Header extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, 
			             HttpServletResponse response) throws ServletException, IOException {
		
		response.setContentType("text/html;charset=euc-kr");
		PrintWriter out = response.getWriter();
		request.setCharacterEncoding("euc-kr");
		
		out.println("<html>");
		out.println("<head><title> 헤더 정보 </title></head");
		out.println("<body>");
		out.println("<center><h2> 헤더 정보 </h2></center>");
		
		//헤더명을 얻어 헤더값을 추출
		Enumeration<String> en = request.getHeaderNames();
		out.println("<ul type='square'>");
		while(en.hasMoreElements()) {
			String headerName = en.nextElement();
			String headerValue = request.getHeader(headerName);
			out.println("<li> " + headerName + " : " + headerValue);
		}
		out.println("</ul>");
		out.println("<br><br><a href='Header.html'>뒤로</a>");
		out.println("</body>");
		out.println("</html>");
	}
}
```


+ Header.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
		<title>EL을 사용한 Heder정보확인</title>
	</head>
	<body>
		host 헤더값 : ${ header.host }<br>
		accept 헤더값 : ${ header.accept }<br>
		user-agent : ${ header["user-agent"] }<br>
	</body>
</html>
```

+ CookieDataWriter.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<%
    Cookie cookie = new Cookie("NAME", "John");
    response.addCookie(cookie);
%>
<HTML>
    <HEAD><TITLE>쿠키 데이터 저장 프로그램</TITLE></HEAD>
    <BODY>
        쿠키 값이 설정되었습니다.
    </BODY>
</HTML>
```


+ CookieDataReader.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
<HEAD>
<TITLE>쿠키 데이터 출력 프로그램</TITLE>
</HEAD>
<BODY>
	<%
		//헤더정보를 이용해서 쿠키정보를 한꺼번에 얻을때
		String cValue = request.getHeader("Cookie");

		//NAME쿠키값을 로직으로 검출
		int pos = cValue.indexOf("NAME");
		if(pos != -1) {
			cValue = cValue.substring(pos+5);
		}
		out.println("NAME 쿠키 데이터의 값은? " + cValue + "<br>");
		
		
		//쿠키객체에서 쿠키값 얻기
		Cookie cookies[] = request.getCookies();
		if (cookies != null) {
			for (Cookie c : cookies) {
				String cName = c.getName(); //쿠키명
				if(cName.equals("NAME")){
					cValue = c.getValue(); //쿠키값
					//cPath = c.getPath(); //경로
					//int max = c.getMaxAge;
					out.println("NAME 쿠키 데이터의 값은? " + cValue + "<br>");
				}
			}
		}
	%>
	<hr>
	NAME 쿠키 데이터의 값은? ${cookie.NAME.value}
	<hr>
	EL표현에서 출력되는 값을 변수에 저장할수 있다면? JSTL에서 가능
	
</BODY>
</HTML>
```



+ web.xml --> 추가등록
```xml
    <context-param>
        <param-name>DB_NAME</param-name>
        <param-value>malldb</param-value>
    </context-param>
```


+ InitParam.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>애플리케이션 초기화 파라미터 예제</TITLE></HEAD>
    <BODY>
    <%
    	String db_name = application.getInitParameter("DB_NAME");
		out.println("DB_NAME 초기화 파라메터 값은? "+db_name+"<br>");
    %>
        DB_NAME 초기화 파라미터의 값은? ${initParam.DB_NAME}
    </BODY>
</HTML>
```


+ PageContext.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>PageContext 내장 개체의 사용 예</TITLE></HEAD>
    <BODY>
    <%
    	String uri = ((HttpServletRequest)pageContext.getRequest()).getRequestURI();
		out.println("요청 URL: " + uri);
    %>
        요청 URL: ${pageContext.request.requestURI} <BR>
    </BODY>
</HTML>
```


#### 5. 실습
#### 6. Summary / Close




-----------------------------------------------------------


### [2019-06-03]

#### 1. Review
#### 2. EL
+ EL 표현에 사용할수 있는 연산자
1) 산술연산
```
+, -, *, /, %
    div, mod <---------- 추가 문자로 표현된 연산자도 사용가능
                   %
    ${ param.NUM1 mod parma.NUM2 }

    //위의 표현을 Java Code로 작성한다면 아래처럼 작성할 것이다.
    String strNum1

```

2) 비교(관계)연산
```
>,  >=, <,  <=, ==, !=
gt, ge, lt, le, eq, ne   <===== 추가로 문자로 표현된 연산자도 사용가능

java code에서는 문자열을 비교할때는
- 직접 연산자 사용  ==> 참조값이 비교대상이 되었다.
- equals()  ==> 내용값 비교
- compareTo() ==> 내용값 비교
```


3) 논리연산
```
&&,  ||,  !
and, or, not   <====== 추가로 문자로 표현된 연산자도 사용가능
```


4) 삼항(조건)연산자
```
?  :

int a=5;
if(a>10){
    a=100;
}else{
    a-100;
}
------------------> a = (a > 10 ) ? 100 : -100;
```


5) 속성값이  



6)


+ 연산자의 우선순위는 괄호를 사용하여 명시적으로 나타내기를 권장합니다.



+ Operators.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>익스프레션 언어 연산자 연습</TITLE></HEAD>
    <BODY>
        X = ${param.NUM1}, Y = ${param.NUM2} <BR><BR>    	  
        X + Y = ${param.NUM1 + param.NUM2} <BR>
        X - Y = ${param.NUM1 - param.NUM2} <BR>
        X * Y = ${param.NUM1 * param.NUM2} <BR>
        X / Y = ${param.NUM1 / param.NUM2} <BR>
        X % Y = ${param.NUM1 % param.NUM2} <BR><BR>
        X가 더 큽니까? ${param.NUM1 - param.NUM2 > 0} <BR>
        Y가 더 큽니까? ${param.NUM1 - param.NUM2 < 0} <BR><BR>
        X와 Y가 모두 양수입니까? ${(param.NUM1 > 0) && (param.NUM2 > 0)} <BR><BR>
        X와 Y가 같습니까? ${param.NUM1 == param.NUM2? "예" : "아니오"} <BR> <BR>
    </BODY>
</HTML>
```



+ CharOperators.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>익스프레션 언어 연산자 연습</TITLE></HEAD>
    <BODY>
    	EL 표현내부에서는 문자로 이루어진 연산자도 사용할 수 있습니다.
    	<hr>
    
        ${param.NUM1}을 ${param.NUM2}로 나눈 몫은? ${param.NUM1 div param.NUM2} <BR>
        나머지는? ${param.NUM1 mod param.NUM2} <BR><BR>
        둘 다 양수입니까? ${(param.NUM1 gt 0) and (param.NUM2 gt 0)} <BR>
        둘 다 음수입니까? ${(param.NUM1 lt 0) and (param.NUM2 lt 0)} <BR>
       <hr>
        ${param.NUM1}을 ${param.NUM2}로 나눈 몫은? ${param.NUM1 / param.NUM2} <BR>
        나머지는? ${param.NUM1 % param.NUM2} <BR><BR>
        둘 다 양수입니까? ${(param.NUM1 > 0) and (param.NUM2 > 0)} <BR>
        둘 다 음수입니까? ${(param.NUM1 < 0) and (param.NUM2 < 0)} <BR>
    </BODY>
</HTML>
```



+ StringOperators.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>문자열 비교</TITLE></HEAD>
    <BODY>
    <%
    	//java code에서는 참조값을 비교
    	String str1 = "JICA";
		String str2 = "JICA";
		String str3 = new String("JICA");
		
		out.println((str1 == str2) +"<br>");   //true
		out.println((str1 == str3) +"<br>");   //false
		out.println("<hr>");
        //문자열 비교에서는 내용값을 비교하기 위해 다음을 사용했다.
		// 1) equals()
		// 2) compareTo()
		// EL에서는 연산자를 사용해도 내부적으로 equals(), compareTo()을 자동으로 작동시킨다.
    %>
        입력 문자열 : ${param.STR1}, ${param.STR2} <BR><BR>
        두 문자열이 같습니까? ${param.STR1 == param.STR2} <BR>
        어느 문자열이 먼저입니까? ${param.STR1 < param.STR2 ? param.STR1 : param.STR2}
    </BODY>
</HTML>
```



+ EmptyOperatoer.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr" %>
<HTML>
    <HEAD><TITLE>엠프티 연산자</TITLE></HEAD>
    <BODY>
    	empty 연산자는 ID파라메터가 존재하지 않거나, 값이 null이거나, 값이 빈문자열일때<br>
    	참을 리턴한다.<br><br>
        안녕하세요, ${empty param.ID ? "guest" : param.ID}님
        <hr>
        안녕하세요, 
        <%
        	String id = request.getParameter("ID");  // null, "", "jica"
        	if(id == null || id.length() <= 0) {
        		out.println("guest님");
        	} else {
        		out.println(id+"님");
        	}
        %>
    </BODY>
</HTML>
```


+ 사용자가 만든 객체, 컬렉션 객체(ArrayList, HashMap, ...) 가 내장객체의 속성값으로 표현되었을때 사용법
```
여러개의 요소를 관리하는 표현---------------------
배열, 컬렉션(ArrayList, HashMap, HashSet)

EL 내부에서는 [첨자]의 표현을 사용한다.
${ 속성명[첨자] }

주의) HashMap은 key와 value의 쌍으로 이ㅜㄹ어진 표현이다.
==> ${ 속성명["키"] }
```



+ Winners.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%
    String winners[] = new String[3];
    winners[0] = "이수현";
    winners[1] = "정세훈";
    winners[2] = "김진희";
    
    //WinnersView.jsp페이지로 제어를 옮길 준비를 한다.
    RequestDispatcher dispatcher = request.getRequestDispatcher("WinnersView.jsp");
    
    //제어를 옮길때 전달할 정보를 설정한다.
    request.setAttribute("NAME", "홍길동");  //단일값 속성
    request.setAttribute("WINNERS", winners);
    
    //제어 이동
    dispatcher.forward(request, response); 
%>
```


+ WinnersView.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>우승자 명단</TITLE></HEAD>
    <BODY>
        <H3>우승자 명단</H3>
        <%
        	String name = (String)request.getAttribute("NAME");
			out.println("성명 : "+name+"<br>");
        	String winners[] = (String[])request.getAttribute("WINNERS");
			for(int i=0; i<winners.length; i++) {
				out.println((i+1)+"등. "+winners[i]+"<br>");
			}
			//자바코드에서는 잘못된 첨자사용은 Runtime Error를 발생시킨다.
			
        %>
        <br>
		<hr>
        성명 : ${ NAME }<br>
        1등. ${WINNERS[0]} <BR>
        2등. ${WINNERS[1]} <BR>
        3등. ${WINNERS[2]} <BR>
        4등. ${WINNERS[3]}
        <hr>
        EL표현 내부에서는 값이 존재하지 않으면 아무일도 하지 않는다.
    </BODY>
</HTML>
```


+ Fruits.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="java.util.*" %>
<%
    ArrayList<String> items = new ArrayList<String>();
    items.add("딸기");
    items.add("오렌지");
    items.add("복숭아");
    
    //제어이동 준비
    RequestDispatcher dispatcher = request.getRequestDispatcher("FruitsView.jsp");
    
    //request객체의 속성값으로 전달할 값 저장
    request.setAttribute("FRUITS", items);
    //제어이동
    dispatcher.forward(request, response); 
%>
```


+ FruitsView.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="java.util.*" %>
<HTML>
    <HEAD><TITLE>인기 상품 목록</TITLE></HEAD>
    <BODY>
        <H3>이달에 가장 많이 팔린 과일입니다.</H3>
        <%= request.getAttribute("FRUITS")%> <BR>
        <%
        	ArrayList<String> fruits = (ArrayList<String>)request.getAttribute("FRUITS");
        	
        	//직접출력
        	out.println(fruits+"<br>");
        	
        	//ArrayList의 개별요소 접근
        	for(String a : fruits){
        		out.println(fruits.indexOf(a)+1+"위. "+a+"<br>");
        	}
        %>
        <hr>
        1위. ${FRUITS[0]} <BR>
        2위. ${FRUITS[1]} <BR>
        3위. ${FRUITS[2]} <BR>
    </BODY>
</HTML>
```



+ Address.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="java.util.*" %>
<%
	//     key(성명) value(출생지)
    HashMap<String, String> map = new HashMap<String, String>();
	//데이터 추가
    map.put("Edgar", "보스턴");
    map.put("Thomas", "오하이오");
    map.put("John", "워싱턴");
	
    //제어이동 준비
    RequestDispatcher dispatcher = request.getRequestDispatcher("AddressView.jsp");
    //제어이동 값 설정
    request.setAttribute("ADDRESS", map);
    //제어이동
    dispatcher.forward(request, response); 
%>
```


+ AddressView.jsp
```jsp
<%@page import="java.util.*"%>
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>주소록</TITLE></HEAD>
    <BODY>
    	<%
    		HashMap<String,String> map = (HashMap<String,String>)request.getAttribute("ADDRESS");
			//map에 설정된 key값들을 알고 있다면
			out.println(map.get("Edgar")+"<BR>");
			out.println(map.get("Thomas")+"<BR>");
			out.println(map.get("John")+"<BR>");
			
			//key값을 모른다면
			Set<String> keys = map.keySet();
			//set의 전체요소를 접근하려면 iterator가 필요하다.
			Iterator<String> it = keys.iterator();
			while(it.hasNext()){
				String key = it.next();
				out.println(key+"의 주소는 "+map.get(key)+"<br>");
			}
    	%>
    	<hr>
        ${param.NAME}의 주소는? ${ADDRESS[param.NAME]}
        <hr>
        map에 저장된 값을 추출할때는 key를 알고 있을때 속성명["key"]를 사용하면 된다.<br>
        ${ ADDRESS["Edgar"] }<br>
        ${ ADDRESS['Thomas'] }<br>
        ${ ADDRESS.John }<br>        
    </BODY>
</HTML>
```




+ 사용자가 만든 객체의 값을 추출하여 출력할수도 있다.

+ ProductInfo2.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="com.jica.brain7.ProductInfo,java.util.*"%>
<%
	//사용자 정의객체 생성
	ProductInfo product = new ProductInfo();
	//멤버변수에 값 설정
	product.setName("초코케이크 3호");
	product.setPrice(20000);

	ArrayList<ProductInfo> products = new ArrayList<ProductInfo>();
	products.add(product);

	product = new ProductInfo();
	product.setName("삼각깁밥");
	product.setPrice(1200);

	products.add(product);

	//제어이동준비    
	RequestDispatcher dispatcher = request.getRequestDispatcher("ProductInfoView2.jsp");
	//사용자 정의 객체를 속성값으로 저장하여 제어이동시 전달
	request.setAttribute("PRODUCTS", products);
	//제어이동
	dispatcher.forward(request, response);
%>
```

+ ProductInfoView2.jsp
```jsp
<%@page import="com.jica.brain7.ProductInfo"%>
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>상품 정보</TITLE></HEAD>
    <BODY>
        <H3>상품 정보</H3>
        
        <hr>
        상품명: ${PRODUCTS[0].name} <BR>
        가격: ${PRODUCTS[0].price}원 <BR>
        <hr>
        상품명: ${PRODUCTS[1].name} <BR>
        가격: ${PRODUCTS[1].price}원 <BR>
        <hr>
        내부적으로 get method를 작동시켜 그 값을 출력시켰다.
    </BODY>
</HTML>
```










+ web.xml  ==> 추가
```xml
   <jsp-config>
        <taglib>
            <taglib-uri>http://www.jica.kr/brain7/math-functions.tld</taglib-uri>
            <taglib-location>/WEB-INF/tlds/math-functions.tld</taglib-location>
        </taglib>
    </jsp-config>
```

+ math-functions.xml
```xml
<taglib xmlns="http://java.sun.com/xml/ns/javaee" version="2.1">
    <tlib-version>1.0</tlib-version>
    <short-name>math</short-name>
    <function>
        <name>squareroot</name>
        <function-class>java.lang.Math</function-class>
        <function-signature>double sqrt(double)</function-signature>
    </function>
    <function>
        <name>total</name>
        <function-class>com.jica.brain7.MyMath</function-class>
        <function-signature>int sum(int, int)</function-signature>
    </function>
</taglib>
```


+ SquareRoot.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@taglib prefix="m" uri="http://www.jica.kr/brain7/math-functions.tld"%>
<HTML>
    <HEAD><TITLE>제곱근 구하기</TITLE></HEAD>
    <BODY>
    <hr>
    	EL표현 내부에서는 JAVA CODE를 전혀 사용할수 없다.<br>
    	EL자체 내장객체 표현을 사용하였다.<br>
    	Java 클래스의 static메서드는 호출할수 있는데,<br>
    	그 호출절차는 다음과 같다.<br>
    	<br>
    	1. TLD 작성<br>
    	2. web.xml에서 TLD파일 인식하도록 등록<br> 
    	3. 정적메서드를 호출하는 jsp페이지에서 taglib지시어를 사용하여 인식<br>
    	4. 정적메서드 호출
    <HR>
    <%
    	double num = Double.parseDouble(request.getParameter("NUM"));
    	double result = Math.sqrt(num);
    	out.println(num+"의 제곱근은? "+result+"<br>");
    %>
    
        ${param.NUM}의 제곱근은? ${m:squareroot(param.NUM)} <BR>
    </BODY>
</HTML>
```





+ MyMath.java
```java
package com.jica.brain7;

public class MyMath {
    public static int sum(int start, int end) {
        int sum = 0;
        for (int cnt = start; cnt <= end; cnt++)
            sum += cnt;
        return sum;
    }
}
```


+ Sigma.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@taglib prefix="m" uri="http://www.jica.kr/brain7/math-functions.tld"%>
<%@page import="com.jica.brain7.*" %>
<HTML>
    <HEAD><TITLE>주어진 범위의 합 구하기</TITLE></HEAD>
    <BODY>
        ${param.NUM1}부터 ${param.NUM2}까지의 합은? <BR><BR>
        답: ${m:total(param.NUM1, param.NUM2)}
        <hr>
        <%
        	int num1 = Integer.parseInt(request.getParameter("NUM1"));
        	int num2 = Integer.parseInt(request.getParameter("NUM2"));
        	out.println("답 : "+MyMath.sum(num1,num2));
        %> 
    </BODY>
</HTML>
```



#### 3. 액션태그
+ 액션 태그(action tag)
    - jsp에서 java코드없이 java코드를 사용한 효과를 나타낼수 있는 xml형태의 태그들을 제공한다. 이러한 태그들을 표준 액션태그라고 부른다.(jsp문법 확장)
    - 액션태그는 원래 java code로 작성할 기능을 태그로 손쉽게 사용하도록 만들어 놓은 것.

+ 액션태그의 종류
    1) JSP 자체에서 제공하는 액션태그 ==> 교재8장
```
[표준액션태그]
`<jsp:태그명 속성=값,... />`
`<jsp:태그명 속성=값,...> </jsp:태그명>`
```
    2) 외부에서 제공하는 액션태그를 외부태그라이브러리(커스텀액션)라고 부른다. ==> 교재9장
        - 그중에서 대표적인 것이 JSTL(Jsp Standard Tag Library)이다.
```
<c:>
<fmt:>
<fn:>
<x:>
<sql:>
```

+ 참고) 커스텀(Custom)이라는 용어
    - 프로그램에서 사용될때 사용자가 만든이라는 의미로 주로 사용한다.
    - 커스텀 class ==> 사용자가 만든 class
    - 커스텀 tag  ===> 사용자가 만든 tag






+ 표준 액션 태그 사용법
##### 1) JSP페이지 모듈화 지원 액션 태그
    - 우리가 이미 많이 사용했던 기능으로 프로그램제어를 이동시키는 Include와 Forward기능.
    - 자동적으로 내부에서 reqeust, response객체를 전달해 준다.
```
<jsp:include page="jsp페이지" />
<jsp:forward page="jsp페이지" />

<jsp:include page="jsp페이지">
    <jsp:param 파라메터정보.... />
</jsp:include>

모듈화 =================<기능별로 코드를 분리해서 작성하는 기법>====================
서블릿코드 ------forward----------> 한화면을 구성하기위한 jsp페이지(View)
(로직처리)                          화면 상단 --------include--------> top.jsp
                                    화면 중단 --------include--------> contents.jsp
                                    화면 하단 --------include--------> bottom.jsp
                                    ||
           <--------최종응답-------
```


+ Winners.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>당첨자 명단</TITLE></HEAD>
    <BODY>
        <H3>당첨자 명단</H3>
        14553 연흥부 <BR>
        63563 심청 <BR>
        73992 이몽룡 <BR><BR>
        <jsp:include page="Now.jsp"/>
        <hr>
        <jsp:include page="Now.jsp" />
    </BODY>
</HTML>
```


+ Now.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<%@page import="java.util.*"%>
<% 
    GregorianCalendar now = new GregorianCalendar();
    String date = String.format("%TY년 %Tm월 %Td일", now, now, now); 
    String time = String.format("%Tp %TR", now, now); 
%>
[현재 시각] <%= date %> <%= time %>
```





+ Hundred.jsp
```jsp
<%
    int sum = 0;
    for (int cnt = 1; cnt <= 100; cnt++)
        sum += cnt;
    request.setAttribute("RESULT", new Integer(sum));
%>
<jsp:forward page="HundredResult.jsp" />
```


+ HundredResult.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>1부터 100까지의 합</TITLE></HEAD>
    <BODY>
    	<%
    		int result = (int)request.getAttribute("RESULT");
			out.println("1부터 100까지 더한 결과는? "+result+"<br>");
    	%>
    	<hr>
    	1부터 100까지 더한 결과는? <%= request.getAttribute("RESULT") %><br>
    	<hr>
        1부터 100까지 더한 결과는? ${RESULT}
    </BODY>
</HTML>
```


##### 2) 사용자 객체의 생성 및 사용
```
사용자 객체 생성       <jsp:useBean>
객체에 set메서드 사용  <jsp:setProperty>
객체에 get메서드 사용  <jsp:getProperty>


<jsp:useBean class="com.jica.brain8.PersonalInfo" id="pinfo" scope="page"/> 
                                                             scope="request"
                                                             scope="session"
                                                             scope="application"
속성으로 scope를 사용하지 않으면 기본이 page이다.

useBean태그는 객체를 생성하고
내장객체의 속성값으로 객체를 설정한다.

-------------------------------------------------------------------------------
<jsp:useBean class="com.jica.brain8.PersonalInfo" id="pinfo"/> 
       
위의 useBean태그는
pageContext내장객체에서 pinfo 속성이 있는지 검사하여, 있다면 그 값을 변수로 만든다.(없다면 신규생성)
    PersonalInfo pinfo = null;
    if(pageContext.findAttribute("pinfo")) {
        pinfo = pageContext.getAttribute("pinfo");
    }else{
        prinfo = new PersonalInfo();
        pageContext.setAttribute("pinfo", pinfo);
    }
-------------------------------------------------------------------------------
<jsp:setProperty name="pinfo" property="name" value="김연희" /> 
<jsp:setProperty name="pinfo" property="name" param="name" /> 
<jsp:setProperty name="pinfo" property="gender" value="여" /> 
<jsp:setProperty name="pinfo" property="age" value="29" />
<jsp:setProperty name="pinfo" property="*" />

위의 setProperty는 내장객체의 속성값 객체에 setMethod() 호출하여 값을 저장한다.
pinfo.setName("김연희");
pinfo.setGender('여');
pinfo.setAge(29);
-------------------------------------------------------------------------------
이름: <jsp:getProperty name="pinfo" property="name" /> 
성별: <jsp:getProperty name="pinfo" property="gender" /> 
나이: <jsp:getProperty name="pinfo" property="age" />

위의 getProperty는 내장객체의 속성값 객체에 setMethod()를 작동시켜 멤버변수값을 읽어온다.
pinfo.getName()
pinfo.getGender()
pinfo.getAge()    


<jsp:useBean class="패키지명.클래스">
<jsp:useBean type="패키지명.추상클래스">
```


+ PersonalInfo.java
```java
package com.jica.brain8;

//커스텀 클래스
//1건의 데이터를 표현하는 클래스 -- 자바빈즈
public class PersonalInfo {
	private String name; // 이름
	private char gender; // 성별
	private int age; // 나이

	public PersonalInfo() {
		// 디버깅용
		System.out.println("PersonalInfo::PersonalInfo()....");
	}

	public void setName(String name) {
		// 디버깅용
		System.out.println("PersonalInfo::setName(" + name + ")....");
		this.name = name;
	}

	public String getName() {
		// 디버깅용
		System.out.println("PersonalInfo::getName()....");
		return name;
	}

	public void setGender(char gender) {
		// 디버깅용
		System.out.println("PersonalInfo::setGender("+gender+")....");
		this.gender = gender;
	}

	public char getGender() {
		// 디버깅용
		System.out.println("PersonalInfo::getGender()....");
		return gender;
	}

	public void setAge(int age) {
		// 디버깅용
		System.out.println("PersonalInfo::setAge(" + age + ")....");
		this.age = age;
	}

	public int getAge() {
		// 디버깅용
		System.out.println("PersonalInfo::getAge()....");
		return age;
	}
}
```


+ CustomerInfo.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>회원 정보</TITLE></HEAD>
    <BODY>
        <jsp:useBean class="com.jica.brain8.PersonalInfo" id="pinfo"/> 
        <hr>
        <pre>
        위의 useBean태그는
        pageContext내장객체에서 pinfo 속성이 있는지 검사하여, 있다면 그 값을 변수로 만든다.(없다면 신규생성)
        	PersonalInfo pinfo = null;
        	if(pageContext.findAttribute("pinfo")) {
        		pinfo = pageContext.getAttribute("pinfo");
        	}else{
        		prinfo = new PersonalInfo();
        		pageContext.setAttribute("pinfo", pinfo);
        	}
        </pre>
        <hr>
        <jsp:setProperty name="pinfo" property="name" value="김연희" /> 
        <jsp:setProperty name="pinfo" property="gender" value="여" /> 
        <jsp:setProperty name="pinfo" property="age" value="29" />
        <pre>
        위의 setProperty는 내장객체의 속성값 객체에 setMethod() 호출하여 값을 저장한다.
        pinfo.setName("김연희");
        pinfo.setGender('여');
        pinfo.setAge(29);
        </pre>
        이름: <jsp:getProperty name="pinfo" property="name" /> <BR>
        성별: <jsp:getProperty name="pinfo" property="gender" /> <BR>
        나이: <jsp:getProperty name="pinfo" property="age" />
        <hr>
        <pre>
        위의 getProperty는 내장객체의 속성값 객체에 setMethod()를 작동시켜 멤버변수값을 읽어온다.
        pinfo.getName()
        pinfo.getGender()
        pinfo.getAge()    
        </pre>
    </BODY>
</HTML>
```



+ CustomerInfoSaver.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<!-- 객체를 생성하여 request내장객체의 속성값으로 저장 -->
<jsp:useBean class="com.jica.brain8.PersonalInfo" id="pinfo" scope="request" />
 
<jsp:setProperty name="pinfo" property="name" value="김현수" /> 
<jsp:setProperty name="pinfo" property="gender" value="남" /> 
<jsp:setProperty name="pinfo" property="age" value="23" /> 

<!-- 내부적으로 request객체가 전달되므로 forward된 페이지에서도 객체를 사용할 수 있다. -->
<jsp:forward page="CustomerInfoViewer.jsp" />
```



+ CustomerInfoViewer.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>회원 정보</TITLE></HEAD>
    <BODY>
    <!-- 결과적으로 객체를 생성한 것이 아니라 이전의 jsp페이지에서 생성저장한 객체를 얻어와서 사용. -->
        <jsp:useBean class="com.jica.brain8.PersonalInfo" id="pinfo" scope="request" /> 
        이름: <jsp:getProperty name="pinfo" property="name" /> <BR>
        성별: <jsp:getProperty name="pinfo" property="gender" /> <BR>
        나이: <jsp:getProperty name="pinfo" property="age" />
    </BODY>
</HTML>
```



+ NewCustomerInfo.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>회원 정보</TITLE></HEAD>
    <BODY>
        <jsp:useBean class="com.jica.brain8.PersonalInfo" id="pinfo" />
         
        <jsp:setProperty name="pinfo" property="name" param="NAME" /> 
        <jsp:setProperty name="pinfo" property="gender" param="GENDER" /> 
        <jsp:setProperty name="pinfo" property="age" param="AGE" />
         
        이름: <jsp:getProperty name="pinfo" property="name" /> <BR>
        성별: <jsp:getProperty name="pinfo" property="gender" /> <BR>
        나이: <jsp:getProperty name="pinfo" property="age" />
    </BODY>
</HTML>
```



+ NewerCustomerInfo.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>
<HTML>
    <HEAD><TITLE>회원 정보</TITLE></HEAD>
    <BODY>
        <jsp:useBean class="com.jica.brain8.PersonalInfo" id="pinfo" />
        
        <!-- html문서에서 전송한 요청 파라메터명과
        사용자가 만든 클래스의 벰버변수명이 일치할때는 아래의 코드로 단순화 시킬수 있다. --> 
        <jsp:setProperty name="pinfo" property="*" /> 
        
        이름: <jsp:getProperty name="pinfo" property="name" /> <BR>
        성별: <jsp:getProperty name="pinfo" property="gender" /> <BR>
        나이: <jsp:getProperty name="pinfo" property="age" />
    </BODY>
</HTML>
```




##### 3) 기타 액션태그
```
<jsp:plugin  --> 자동 applet 사용(생략)
<jsp:param   --> forward/include시 추가 파라메터 전달

스크립팅요소를 태그로 표현
<%  %>   ==>  <jsp:scriptlet>  </jsp:scriptblet>
```



##### [오늘의 과제]
+ EL / Action tag 복습

#### 4. 실습
#### 5. Summary / Close




-----------------------------------------------------------


### [2019-06-04]

#### 1. Review

#### 2. Action tag
+ BookInfo.java
```java
package com.jica.brain8;
public class BookInfo extends ProductInfo {
    private short page;         // 페이지수
    private String writer;      // 저자
    
    public void setPage(short page) {
        this.page = page;
    }
    public void setWriter(String writer) {
        this.writer = writer;
    }
    public short getPage() {
        return page;
    }
    public String getWriter() {
        return writer;
    }
}
```

+ BookInfoSaver.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>

<jsp:useBean class="com.jica.brain8.BookInfo" id="pinfo" scope="request"/>
 
<jsp:setProperty name="pinfo" property="code" value="50001" /> 
<jsp:setProperty name="pinfo" property="name" value="의뢰인" /> 
<jsp:setProperty name="pinfo" property="price" value="9000" /> 
<jsp:setProperty name="pinfo" property="writer" value="존 그리샴" /> 
<jsp:setProperty name="pinfo" property="page" value="704" /> 

<HTML>
    <HEAD><TITLE>책 정보 관리</TITLE></HEAD>
    <BODY>
        책 정보가 저장되었습니다. <BR>
        --------------------------<BR>
        <H3>제품 개략 정보</H3>
        <jsp:include page="ProductInfo.jsp" />
        <!-- include할때 내부적으로 request객체가 전달되고
         request객체의 pinfo속성명으로 BookInfo객체가 저장되어 있다. -->
        <h4>책 상세 정보</h4>
        페이지: <jsp:getProperty name="pinfo" property="page" /> <BR>
		저  자:<jsp:getProperty name="pinfo" property="writer" /> <BR>	
    </BODY>
</HTML>
```

+ ClothingInfo.java
```java
package com.jica.brain8;
public class ClothingInfo extends ProductInfo {
    private char size;         // 사이즈 (L/M/S)
    private String color;      // 색상
    
    public void setSize(char size) {
        this.size = size;
    }
    public void setColor(String color) {
        this.color = color;
    }
    public char getSize() {
        return size;
    }
    public String getColor() {
        return color;
    }
}
```

+ ClothingInfoSaver.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>

<jsp:useBean class="com.jica.brain8.ClothingInfo" id="pinfo" scope="request"/> 

<jsp:setProperty name="pinfo" property="code" value="70002" /> 
<jsp:setProperty name="pinfo" property="name" value="반팔 티셔츠" /> 
<jsp:setProperty name="pinfo" property="price" value="15000" /> 
<jsp:setProperty name="pinfo" property="size" value="M" /> 
<jsp:setProperty name="pinfo" property="color" value="베이지" /> 

<HTML>
    <HEAD><TITLE>의류 정보 관리</TITLE></HEAD>
    <BODY>
        의류 정보가 저장되었습니다. <BR>
        --------------------------<BR>
        <H3>제품 개략 정보</H3>
        <jsp:include page="ProductInfo.jsp" />
        <h4>옷 상세 정보</h4>
        사이즈: <jsp:getProperty name="pinfo" property="size" /> <BR>
		색  상:<jsp:getProperty name="pinfo" property="color" /> <BR>	
    </BODY>
</HTML>
```

+ ProductInfo.java
```java
package com.jica.brain8;
public class ProductInfo {
    private String code;     // 제품코드
    private String name;     // 제품명
    private int price;       // 가격
    public void setCode(String code) {
        this.code = code;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void setPrice(int price) {
        this.price = price;
    }
    public String getCode() {
        return code;
    }
    public String getName() {
        return name;
    }
    public int getPrice() {
        return price;
    }
}
```

+ ProductInfoSaver.jsp
```jsp
<%@page contentType="text/html; charset=euc-kr"%>

<jsp:useBean class="com.jica.brain8.ProductInfo" id="pinfo" scope="request" />
<!-- request객체의 속성 pInfo에 저장된 객체를 검색해서 리턴 -->
코드: <jsp:getProperty name="pinfo" property="code" /> <BR>
제품명:<jsp:getProperty name="pinfo" property="name" /> <BR>
가격: <jsp:getProperty name="pinfo" property="price" /> <BR>
```

```
<jsp:useBean class="패키지명.클래스">
<jsp:useBean type="패키지명.추상클래스">
```

+ 

#### 3. JSTL

#### 4. 게시판 만들기

#### 5. 실습
#### 6. Summary / Close


-----------------------------------------------------------


### [2019-06-05]

#### 1. Review
#### 4. 액션tag


#### 4. 실습
#### 5. Summary / Close



-----------------------------------------------------------


### [2019-06-07]

#### 1. Review
#### 4. 액션tag


#### 4. 실습
#### 5. Summary / Close




-----------------------------------------------------------


### [2019-06-10]

#### 1. Review
#### 4. 액션tag


#### 4. 실습
#### 5. Summary / Close