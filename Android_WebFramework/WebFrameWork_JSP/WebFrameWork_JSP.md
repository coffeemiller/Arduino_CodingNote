### [2019-05-28]

#### 3. JSP 개요
+ 웹어플리케이션의 구조-웹서버(Tomcat)
```
brain3\*.html, *.js, *.jsp, 다양한 리소스(*.jpg, *.wav,...)  <== 내부폴더를 가질수O
      \WEB-INF\web.xml
              \classes\패키지형태의 컴파일된 서블릿코드
              \lib\서블릿코드에서 사용하는 외부 라이브러리파일(*.jar)
```

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
   - 위의 요소 내부에서 자유롭게 사용할수 있는 객체들이 있다(declation요소는 제외). 이것을 내장 객체라고 부른다.
    - jsp페이지가 요청되면 jsp페이지가 서블릿코드로 변환시킬때 내부적으로 추가되어지는 변수이다.
     즉, jspService()메서드 내부에 인자로 전달된 변수이거나 내부에서 선언한 변수들이다.
```     
     request
     response
     out
     application
     page
     pageConext
     session
     exception
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