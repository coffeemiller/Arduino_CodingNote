

### [2019-07-15]

#### 1. JSP Review
+ Spring(스프링) - 웹 프레임워크(framework)
+ 기존 서버에서 JSP/Servlet에서 하던 작업을 표준하된 방법으로 개발하도록 한다.
+ 손쉽고 적은 비용으로 유지보수가 가능하게 한다.


##### 웹프로그램 기술
+ client
  + html / css / javaScript
+ Server
  + Servlet / JSP


1. 게시판 예제(복습)
2. MVC패턴적용 -> 게시판



+ scope를 가지는 4개의 객체
  1. 서블릿객체 - pageContext
  2. request
  3. session
  4. Servlet Context - application

+ getAttrubute(키);  /  setAttrubute(키,값);



#### 2. MVC (Model View Controller) 패턴 적용

##### MVC 모델2 작동구조이해(Command Pattern)
```
[ MVC 패턴 ]
----------------------------------------------------------------------------
M(Model) --> 자바빈즈와 데이타베이스를 이용해서 비지니스 로직(수행할 기능)을 수행하는 대상
                 일반적으로 자바코드로 작성한다. 
             1) 비즈니스 로직 수행 및 Entity 표현 클래스(BoardDataBean, BoardDBBean)
             2) 서비스 기능 제어(Action : 기능수행후 결과를 저장하고 최종 결과를 보여줄 페이지 결정)    
V(View)  --> 사용자에게 정보를 표현하는 기능을 수행한다.(Presentaiton 기능)
                 일반적으로 JSP로 작성한다.(EL, JSTL의 사용 권장, 액션태그)
C(Controller) --> 사용자의 요청을 받아 요청을 해석하여(설정화일을 사용)
				  1) 비지니스 로직을 수행시킨고(Action)  ==> Model 작동시킨다.
                  2) 그결과를 표현하도록 제어를 이동한다 ==> View 작동시킨다.
                       일반적으로 자바코드(서블릿) 작성한다.                     
----------------------------------------------------------------------------

[ 모델2에서 사용하는 용어 ]
    1)Action --> 자바코드으로 만들어진 객체
               - 컨트롤러 기능을 수행하는 액션(ControllerAction.java)<---이것만 서블릿이다.
               - 비지니스 로직을 수행하는 액션 (나머지 XXXAction.java) <---일반자바객체
    2)Result --> JSP                         
----------------------------------------------------------------------------
Command Pattern 적용


1) 컨틀롤러 기능수행하는 컨트롤러를 개별적으로 작성하지 않고 1개의 화일만 작성한다.
   
   - 모든 Action 에서 공통으로 사용하는 기능을 interface로 작성해 놓는다.
   
   public interface CommandAction {
    public String requestPro(HttpServletRequest request, 
		HttpServletResponse response)throws Throwable;
   }

   - 사용자의 요청을 일반화시켜서 부르는 용어 : Commond
      사용자요청정보 와 수행할 기능(Action)을 담은 클래스에 대한 정보를  외부화일로 작성하여
     WEB-INF 폴더에 저장한다. 이러한 화일 Property file이라고 한다.
     
     WEB-INF\CommandPro.properties 
     -----------------------------
	 /list.do=com.jica.board3.action.ListAction
	 /writeForm.do=com.jica.board3.action.WriteFormAction
	 /writePro.do=com.jica.board3.action.WriteProAction
	 /content.do=com.jica.board3.action.ContentAction
	 /updateForm.do=com.jica.board3.action.UpdateFormAction
	 /updatePro.do=com.jica.board3.action.UpdateProAction
	 /deleteForm.do=com.jica.board3.action.DeleteFormAction
	 /deletePro.do=com.jica.board3.action.DeleteProAction     

	 위의 화일내용은 사용자가 list.do 라고 요청하면(Command)
	 com.jica.board3.action 패키지의 ListAction.java(자바 코드: Model) 를 작동시키도록
	 사용되어 질것이다.
	
   - HttpServlet을 상속받은  컨트롤러 클래스를 만든다.(ControllerAction.java)
     
     *** 이 컨트롤러 클래스는 서블릿이므로 최초에 동작할때 단 한번, init()메서드가 동작하게 된다.
     init()메서드 내부에서 위에 작성한 CommandPro.properties 화일을 읽어들어
      키와 Value의 쌍으로 이루어진 Map객체에 Command와 그와 짝을 이루는 클래스형 객체를 생성하여
      저장하여 놓는다.
      
	    web.xml의 초기화 파라메타를 이용
	   	-----------------------------------------------------------------
	   	<servlet>
	        <servlet-name>ControllerAction</servlet-name>
	        <servlet-class>com.jica.board3.controller.ControllerAction</servlet-class>
	        <init-param>
	            <param-name>propertyConfig</param-name>
	            <param-value>C:\work\WebWork2\jspboard3\WebContent\WEB-INF\CommandPro.properties</param-value>
	        </init-param>
	    </servlet> 
      
     *** 이후 사용자의 요청이 있을 때마다(doGet(), doPost()) 실제로 컨트롤러의 기능을 수행한
     requestPro()메서드를 작동시킨다.
     requestPro()메서드에서는 사용자의 요청정보 즉, request객체에서 요청URI(getRequestURI())을 얻어
      이것을 Key로 사용하여 작동시킬 Model(XXXAction.java)을 구하여 실행시키면
      실행된 모델에서는 비지니스로직을 수행하고 그 결과를 request객체에 담고 그리고 리턴값으로 
      결과를 Presentaion시킬 JSP화일명 을 리턴한다.
      
    *** 결과를 보여줄 JSP 화일로 forward를 시켜서 최종 응답이 이루어지게 한다.  
   
  - ControllerAction.java의 기능이 사용자의 요청정보에 의해 실행될 수 있도록 
    web.xml 화일에  서블릿과 서블릿 매핑정보를 기록해 놓는다. 
   	web.xml의 내용
   	-----------------------------------------------------------------
    <servlet-mapping>
        <servlet-name>ControllerAction</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping> 
	 
2) 비지니스 로직을 수행하는 xxxAction.java 코드는
    기능을 수행한후 결과를 request 객체에 저장한후 그 결과를 보여줄 jsp화일명을 리턴하도록
    개별적으로 작성한다.
    
    
===================================================
               목록보기 요청
client   ---------------------> ControllerAction(서블릿)
             list.do            1) 요청정보 해석
                                2) listAciton.java작동시켜 결과값과 결과를 보여줄 View를 얻어온다.
         <--------------------  3) list.jsp작동시켜 결과를 응답한다. 
         
                글작성 폼 요청
client   ---------------------> ControllerAction(서블릿)
             writeForm.do       1) 요청정보 해석
                                2) writeFormAciton.java작동시켜 결과값과 결과를 보여줄 View를 얻어온다.
         <--------------------  3) writeForm.jsp작동시켜 결과를 응답한다.         
           
            글작성후 저장을 요청
client   ---------------------> ControllerAction(서블릿)
             writePro.do        1) 요청정보 해석
                                2) writeProAciton.java작동시켜 결과값과 결과를 보여줄 View를 얻어온다.
         <--------------------  3) writePro.jsp작동시켜 결과를 응답한다.         
            
            ....
            여기까지의 내용에서 
         1) 어떤 요청을 하면
         2) 어떤 모델 즉, Action이 작동한다는 것이 정해져 있다. 
         ============================================> 그러므로
           컨트롤러는 최종 생성시 이러한 정보를 이용하여 필요한 Action객체를 미리 만들어 놓는다.
                          ----------WEB-INF/CommandPro.properties화일
                          
                          
         Model, View, Controller를 이용하는 전체적인 작동구조가 정해져 있다.        
```


#### 3. project 실습
#### 4. Summary / Close

-------------------------------------------------------------------------

### [2019-07-16]

#### 1. Review

#### 2. MVC (Model View Controller) 패턴 적용

#### 3. spring tool 설치
#### 4. project 실습
#### 5. Summary / Close