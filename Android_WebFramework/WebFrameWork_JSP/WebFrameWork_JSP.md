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
------------------------------------------------
3) 선언부(declaration)          <%! 멤버변수 혹은 메서드정의부작성 %>

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


#### 5. 지시어
+ `<%@` 
+ page
+ include
+ taglib
#### 6. 내장객체
#### 7. 실습
#### 8. Summary / Close



-----------------------------------------------------------


### [2019-05-29]

#### 1. Review


#### 4. 실습
#### 5. Summary / Close



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