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
```




#### 4. Scripting element
+ Script let
+ Expression
+ declation
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