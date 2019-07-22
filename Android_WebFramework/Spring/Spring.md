

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

#### 2. 들어가기...
+ 개발자가 코딩으로 의존주입을 직접 구현하지않고, 스프링에서 제공하는 방법으로 처리!!
  + 다양한 클래스 라이브러리사용, Anotation사용

+ AOP(Aspect-Oriented Programing)
  + 특정기능수행을 하는 객체들의 공통기능들을 묶어서 편리하게 사용하자!

+ MVC패턴을 적용하여 프로젝트를 개발 ---> 작동구조, 작동순서, 작동기능

+ JDBC 지원기능 --> 드라이버로드, Connection(커넥션풀), Statement, ResultSet
  + 간편하게 사용할수 있는 라이브러리나 방법들을 제공.



##### 프레임워크(framework) ?
+ 소프트웨어 제작을 편리하게 할 수 있도록, 미리 뼈대를 이루는 클래스와 인터페이스를 제작하여 이것들은 모아둔 것.

```
   ========================================================================
   GoF의 디자인 패턴으로 유명한 랄프 존슨(Ralph Johnson) 교수의 정의 : 
     "소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 
         재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것"
   
   - 프레임워크는 라이브러리와 달리 애플리케이션의 틀과 구조를 결정할 뿐 아니라
       그 위에 개발된 개발자의 코드를 제어한다.
   - 프레임워크는 구체적이며 확장 가능한 기반 코드를 가지고 있으며,
       설계자가 의도하는 여러 디자인 패턴의 집합으로 구성되어 있다.
   ========================================================================
```


##### 라이브러리 ?
```
라이브러리 : 자주 쓰일 만한 기능들을 모아 놓은 유틸리티(클래스)들의 모음집.
           필요할때 가져다 쓰는 기능이지 그 기능을 쓰기 위해 필요한(혹은 효율적인) 구조에 대해 말해주지는 않는다. 
           즉, 설계를 대신해 주지 않는다. (동일한 라이브러리를 쓰는 동일한 기능의 프로그램일지라도
           클래스 관계 구조나, 데이터를 처리하는 절차,프로그램이 화면에 그려지는 방식 같은 등등의 요소들은 
           프로그램을 짜는 프로그래머마다 천차만별 일수 밖에 없으며 프로그램을 완성하는 데에 걸리는 시간도,
           완성된 코드의 품질도 프로그래머의 역량에 따라 제각각일 수밖에 없다)
```



##### 프레임워크와 라이브러리의 가장 큰 차이점 ?
```
    - 프레임워크는 라이브러리에 추가적으로  뼈대가 되는 클래스들과 그 클래스들의 관계로 만들어진 일종의 '설계의 기본 틀'이 추가된다.
                                                                       ============ 
                                                           '확장 가능한 기반코드', '재사용 가능한 형태의 협업화된 클래스들'
    - 프레임워크에는 프레임워크의 제작자가 '이걸 기초로 해서 만드세요'라고  만들어 놓은(제공되는) '기반 코드'가 있다.
      (이 코드, 혹은 클래스들은 차후 사용자들에 의해 확장될 것을 충분히 고려해서 만들어졌기 때문에 사용자 입장에서는 그저 이 것을 가지고,
          여기 저기를 자기 입맛대로 바꾸고 살을 덧붙여 자기만의 프로그램을 완성해 나가면 된다.)
    
    - '확장 가능한 기반 코드'에는 프레임워크 제작자 나름대로의 설계 철학이 담겨 있으며, 차후 이 프레임워크의 사용자가
          제작자가 설계한 구조를 유지하면서 확장할 수 있도록, 제작자에 의해 의도된 제약 사항이 존재한다.
                                                        =====
                               보통 디자인 패턴을 구현한 코드의 형태로 표현된다(예 - MVC 패턴)
    - 기반 코드는 프레임웍 제작자가 사용자들로 하여금 세세하게 신경 쓰지 않아도 쉽고 빠르게 기능을 확장하거나 유지보수할 수 있게 해주는 
      구조에 대한 가이드라인(제약을 가하고 싶은 사항) 을 여러 디자인 패턴을 조합해 표현해 놓은 결과물이다.
         
          
  결론 ) 프레임워크란 ?
          설계의 기반이 되는 부분을 기술한  확장 가능한 기반 코드와 사용자가 이 코드를 자기 입맛대로 확장하는 데 필요한 라이브러리
          이 두 가지 요소가 통합되어 제공되는 형태를 말하며, 사용자가 이를 이용해 일정 수준 이상의 품질을 보장받는 코드를, 
          비교적 빠른 시간에 완성 및 유지 보수할 수 있는환경을 제공해주는 솔루션으로
          "기본적인 설계나 필요한 라이브러리는 알아서 제공해 줄꺼니깐 프로그래머는 구현 하고 싶은 기능 구현에만 전념하도록 만들어진 도구"
          이다.
```



##### 스프링 프레임워크
```
스프링 프레임워크 : 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크로서,
                        자바 엔터프라이즈 즉 서버 개발을 편하게 해주는 오픈 소스 경량급 애플리케이션 프레임워크이다
```



##### 개발환경 설치 : eclipse , jdk, tomcat, maven
```
     maven : 프로젝트 관리도구(원래는 cmd Mode에서 사용 --> 윈도우즈용으로 eclipse,sts에서 사용)
             1) 라이브러리 관리 기능 :
                개발 시 스프링,  jdbc, junit, mybatis 등 필요한 라이브러리들을 zip 형태로 다운로드해 추가해도 되지만
                maven을 사용하면 필요한 라이브러리를 pom.xml 파일에 적어놓으면 자동으로 다운 및 설치를 가능하게 해준다.
             2) 빌드 기능 :
                        컴파일, 테스트, 실행기능을 편리하게 사용할 수 있다.
             -------------------------------------------------------
                   내가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는데에 필요한 다른 라이브러리들까지 관리하여 네트워크를 통해서 자동으로 다운받아준다.
             Spring 라이브러리를 효율적으로 관리해주기위해 사용       
                               
             maven 다운로드 ==> http://maven.apache.org/download.cgi
	           1) 압축해제후 하위폴더를 원하는 위치로 이동
		   2) 시스템환경변수 설정
		      M2_HOME   : 
		      Path : 
	     spring 다운로드 ==> https://spring.io/tools(See all version에서 64비트용 다운)
                   1) 압축해제후 C:\sts-bundle폴더를 c드라이브로 옮기기
		   2) C:\sts-bundle\sts-3.9.5.RELEASE\STS.exe실행

  스프링 개발환경
    1) jdk, eclipse, spring plug-in, maven plug-in, tomcat
    2) jdk, sts(스프링 전용 tool), maven, tomcat
            =====================
  스프링 도움말 : https://spring.io/docs/reference
```



##### 스프링의 특징
```
   ## 특징
    > 스프링은 경량의 프레임워크
      . 자바의 객체를 담고 있는 컨테이너
      . 객체의 생성, 소멸과 같은 생명주기를 관리한다.
  
    > DI(Dependency Injection):의존성 주입 패턴을 지원한다.    
      . 설정파일을 통해서 의존관계를 설정해 주는 패턴
      
    > AOP(Aspect Oriented Programming)을 지원
      . 트랜잭션이나, 로깅, 보안과 같은 엔터프라이즈 어플리케이션에서 공통으로 필요로 하는 기능을 분리해서 
        각 모듈에 적용할 수 있도록 하는 기능
    
    > 스프링은 POJO(Plain Old Java Object)를 지원한다.
      .특정 인터페이스나 클래스를 상속받지 않는 순수한 자바 객체를 스프링 컨테이너가 저장하고 있다.
      
    > 트랜잭션 처리를 위한 일관된 방식을 제공한다.
    
    > 영속성과 관련된 다양한 API를 제공한다.
     . JDBC, IBatis, MyBatis, JPA, Hibernate 등과 같은 프레임워크와 연동을 지원한다.
```



##### 자주 사용하는 용어정리
```
CRUD / DAO / DTO
 CRUD : Create, Retrieve, Update, Delete의 약어이다.
     즉, 데이터베이스에 데이터를 입력(insert) 하고, 읽어(select) 오고, 수정(update) 하고, 삭제(delete) 하는
   DML(Data Manipulation Language) 작업을 의미한다.(DML 기본 사용방법) 

 
 DAO(data Access Object) : 실질적으로 데이터베이스에 접근하여 작업을 수행하는 객체이다. 
     즉, 데이터베이스에 CRUD 작업을 수행하기 위한 로직(데이터베이스 연결, SQL 문, 해제 등)들이 정의되어 있는 객체를 의미한다.
     참고로 DAO는 주로 싱글톤 패턴으로 생성하게 된다.

 DTO(Data Transfer Object) 또는 VO(Value Object) :
     뷰 페이지(jsp, html)로부터 입력된 데이터를 DTO에서 일시적으로 저장하여 DAO로 넘겨주거나, 
     데이터베이스로부터 읽어온 데이터를 DAO로부터 넘겨받아 DTO에서 일시적으로 저장하여 뷰 페이지로 이동시켜 주는 역할을 한다.
     즉, 데이터 임시 저장 및 교환을 위한 객체(자바빈/Java Bean) 이다. 
   DTO는 대부분 필드(프로퍼티)와 getter, setter 메서드로만 이루어진다.
```


##### 설치과정 모습 : mvn archetype:genarate
```
C:\SCWork\SpringWork> mvn archetype:genarate
...
...
...
...
...
1387:
Choose org.apache.maven.archetypes:maven-archetype-quickstart version:
1: 1.0-alpha-1
2: 1.0-alpha-2
3: 1.0-alpha-3
4: 1.0-alpha-4
5: 1.0
6: 1.1
7: 1.3
8: 1.4
Choose a number: 8:
.....
.....
.....
.....
Define value for property 'groupId': com.jica
Define value for property 'artifactId': intro
Define value for property 'version' 1.0-SNAPSHOT: :
Define value for property 'package' com.jica: : com.jica.intro
Confirm properties configuration:
groupId: com.jica
artifactId: intro
version: 1.0-SNAPSHOT
package: com.jica.intro
 Y: :
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.jica
[INFO] Parameter: artifactId, Value: intro
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.jica.intro
[INFO] Parameter: packageInPathFormat, Value: com/jica/intro
[INFO] Parameter: package, Value: com.jica.intro
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: groupId, Value: com.jica
[INFO] Parameter: artifactId, Value: intro
[INFO] Project created from Archetype in dir: C:\SCWork\SpringWork\intro
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  06:52 min
[INFO] Finished at: 2019-07-16T14:40:52+09:00
[INFO] ------------------------------------------------------------------------
C:\SCWork\SpringWork>
```


##### maven 프로젝트 구조
```
intro\src\main\java\패키지\App.java
              \resources\여러설정파일(xml파일)  ---  필요한경우 생성
              \webapp\웹프로그램소스(html,jsp...)
          \test\java\패키지\AppTest.java
      \pom.xml  <===== (Project Object Model)의 약자를 사용한 설정파일
                      컴파일 테스트 배포폰 설치등의 여러 옵션을 작성.
```

##### maven 컴파일 : mvn compile
```
C:\SCWork\SpringWork\intro> mvn compile
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------------------< com.jica:intro >---------------------------
[INFO] Building intro 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.0.2/maven-resources-plugin-3.0.2.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.0.2/maven-resources-plugin-3.0.2.pom (7.1 kB at 6.7 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/30/maven-plugins-30.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-plugins/30/maven-plugins-30.pom (10 kB at 32 kB/s)
...
...
...
...
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.jar (21 kB at 27 kB/s)
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to C:\SCWork\SpringWork\intro\target\classes
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  32.936 s
[INFO] Finished at: 2019-07-16T14:55:28+09:00
[INFO] ------------------------------------------------------------------------
C:\SCWork\SpringWork\intro>
```


##### mven 배포본 만들기 : mvn package
```
C:\SCWork\SpringWork\intro> mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------------------< com.jica:intro >---------------------------
[INFO] Building intro 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.1/maven-surefire-plugin-2.22.1.pom
...
...
...
[INFO]
[INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ intro ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ intro ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ intro ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory C:\SCWork\SpringWork\intro\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ intro ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to C:\SCWork\SpringWork\intro\target\test-classes
[INFO]
[INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ intro ---
...
...
...
[INFO]
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] Running com.jica.intro.AppTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.023 s - in com.jica.intro.AppTest
[INFO]
[INFO] Results:
[INFO]
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO]
[INFO]
[INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ intro ---
...
...
...
Downloaded from central: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-archiver/3.4/plexus-archiver-3.4.jar (187 kB at 97 kB/s)
[INFO] Building jar: C:\SCWork\SpringWork\intro\target\intro-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  24.556 s
[INFO] Finished at: 2019-07-16T14:58:52+09:00
[INFO] ------------------------------------------------------------------------
C:\SCWork\SpringWork\intro>
```



##### mven 실행하기 : java -cp [압축파일명] [파일명]
```
C:~~\intro\target> java -cp intro-1.0-SNAPSHOT.jar com.jica.intro.App
Hello World!
```


#### 3. project 실습
#### 4. Summary / Clos

-------------------------------------------------------------------------

### [2019-07-17]

#### 1. Review

#### 2. Spring 사용하기

+ AppContext.java
```java
package com.jica.chap02;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

//AppContext클래스는 스프링 설정 클래스라고 지정한다.
@Configuration
public class AppContext {
	
	//Bean객체생성메서드 지정(ApplicationContext 즉 스프링프레임워크에서 생성하는 객체==> Bean객체)
	@Bean
	@Scope("prototype")  // <---- 새로운 객체가 각각 생성.
  // @Scope("singleton")  // <---- 하나의 객체를 이용하여 실행(없던상태와 같음)
	public Greeter greeter() {
		System.out.println("AppContext::greeter() 작동함");
		
		Greeter g = new Greeter();
		g.setFormat("%s 안녕하세요");
		return g;
	}
}
```

+ Main.java
```java
package com.jica.chap02;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {

	public static void main(String[] args) {
		// 자유롭게 사용자가 직접 코딩하여 Greeter객체를 생성하고 작성한다.
//		Greeter g = new Greeter();
//		g.setFormat("%s 님! 반갑습니다.");
//		
//		String message = g.greet("스프링");
//		System.out.println(message);
//		
//		message = g.greet("JICA");
//		System.out.println(message);
		
		
		//spring framwork를 사용할때는 필요할때 자바객체를 사용자가 위처럼
		//직접 코드로 생성하지 않고, 객체생성설정파일을 사용하거나...
		//resource설정파일을 이용하여 객체를 생성하고, 생성된 객체를 spring framework이 관리.
		//(생성, 검색, 삭게...)
		//그래서 spring을 spring container라고도 부른다.
		
		
		//우리는 AppContext클래스를 사용하여 간접적으로 Greeter객체를 얻어서 사용한다.
		AnnotationConfigApplicationContext ctx 
				= new AnnotationConfigApplicationContext(AppContext.class);
		System.out.println("Main::Context객체 생성 직후 ------------------");
		
		//Context가 관리하는 객체중에서 검색하여 Bean객체를 리턴해달라.
		Greeter g = ctx.getBean("greeter",Greeter.class);
		String message = g.greet("스프링(spring)");
		System.out.println(message);
		
		Greeter g2 = ctx.getBean("greeter",Greeter.class);
		String message2 = g.greet("프레임워크");
		System.out.println(message2);
		
		//위의 g,g2객체는 동일한 객체일까? 다른 객체일까?
		System.out.println(g == g2);

    //컨텍스트 사용후 종료
    ctx.close();
	}
}
```

+ 위 코드의 핵심은 AnnotationConfigApplicationContext 클래스이다. 스프링의 핵심 기능은 객체를 생성하고 초기화하는 것. 관련 기능은 AppicationContext라는 인터페이스에 정의되어 있다.



+ applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- 객체 생성 설정 -->
	<!-- chap02.Greeter클래스의 객체를 생성하고 해당 객체를 식별하기 위한 식별자로
	     greeter라고 지정해서 관리하겠다.
	     setFormat("%s, spring이 관리하는 빈객체입니다.")을 작동시킨다. 
	     
	     scope="prototype|singleton" -->
	     
	<bean id="greeter" class="com.jica.chap02.Greeter" >
		<property name="format" value="%s, spring이 관리하는 빈객체입니다." />
	</bean>
	
	<bean id="greeterX" class="com.jica.chap02.Greeter">
		<property name="format" value="%s, 빈객체는 기본적으로 동일하다. 기본 scope가 singleton이기때문" />
	</bean>
</beans>
```


+ Main2.java
```java
package com.jica.chap02;

import org.springframework.context.support.GenericXmlApplicationContext;

public class Main2 {

	public static void main(String[] args) {
		// GenericXmlApplicationContext를 이용한 Bean객체 생성
		GenericXmlApplicationContext ctx
			= new GenericXmlApplicationContext("classpath:applicationContext.xml");
		
		System.out.println("main()에서 Context객체 생성 직후---------------------------");
		
		Greeter g = ctx.getBean("greeter", Greeter.class);
		String message = g.greet("JICA");
		System.out.println(message);
		System.out.println();
		
		Greeter g2 = ctx.getBean("greeter", Greeter.class);
		message = g2.greet("JICA2");
		System.out.println(message);
		System.out.println();
		
		System.out.println(g == g2);
		System.out.println();
		//-------------------------------------------
		
		Greeter gx = ctx.getBean("greeterX", Greeter.class);
		message = gx.greet("JICA-X");
		System.out.println(message);
		System.out.println();
				
		//아래의 코드는 예외가 발생한다.(greeterX2를 id로 가지는 Bean태그가 없다.)
		Greeter gx2 = ctx.getBean("greeterX2", Greeter.class);
	}
}
```





#### 2. 의존주입(DI : Dependency Injection)
+ 클래스의 관계성
1. 상속관계(extends)
```
   A클래스 <----- 부모클래스
   
   B클래스  <---- 자식클래스
   
   class A{
   }
   
   class B extends A{
   }  
```


2. 포함관계
```
   A클래스 <---- 전체
   B클래스, C클래스, D클래스 <--- 부분
   
   class B{
   }
   class C{
   }
   class D{
   }
   
   class A{
   	 B b = new B();  // 자체적으로 부분에 속하는 클래스를 항상 가지고 있다.
   	 C c = new C();
   	 D d = new D();
   	 .....
   }   
   
   A obj = new A();

   포함관계는 전체와 부분이 생사를 같이한다.
   즉, 전체 객체가 생성되면 내부의 부분객체도 모두 생성되어 사용되고
   전체객체가 소거되면 내부의 부분 객체도 동일하게 소멸된다.
   
```




3. 의존 관계
```
   class B{
      ....
      
      methodB(){
           다른 클래스형 객체를 사용하여 기능이 작성된다.
         ------------->1)생성자의 인자로 전달받거나
                     2)set메서드로 전달받는다.    
         .....
      }
   }

   =================================================
   class A{
     B b;  //멤버변수로 B형객체 b가 있지만
           //A객체가 생성될때 b멤버가 생성되지 않았다.
     
     A(){
       
     }
     
     //생성자에 의한 의존성 주입(DI)
     A(B b){
       this.b = b;
     }
     
     //set메서드에 의한 의존성 주입(DI)
     setB(B b){
     	this.b = b;
     }
   
     ...
     methodA(){
        ...
        b객체의 메서드를 호출하여 사용
        ....
     }
   }
      
   B b = new B();   
   // 생성자에 의한 의존객체 연결
   A obj = new A(b); 
   obj.methodA();    // A객체의 기능은 B객체에 의존되어 있다.
   					 // 즉, B객체의 내용이 바뀌면 A객체의 기능도 변경된다 즉, 의존관계이다.
   					
   // set 메서드에의한 의존객체 연결   					 
   A obj = new A(); 
   obj.setB(b);		  	
```


#### 4. project 실습
#### 5. Summary / Close



-------------------------------------------------------------------------

### [2019-07-18]

#### 1. Review

#### 2. SpringFramework
+ ApplicationContext의 설정파일 사용법 2가지
  1. JavaCode 설정파일(*.java)
  ```
  @Configuration
  @Bean
  ...
  Annotation으로 정보를 설정
  ```

  2. xml 설정파일(*.xml --> resources폴더 내부)
  ```
  <bean>
  </bean>
  ....
  다양한 태그를 사용하여 정보를 설정
  ```


+ 실습하려는 기능부터 익혀보자
```
회원등록, 회원목록보기, 회원정보조회, 패스워드변경....

회원등록
  키보드로 회원정보 입력 ---> RegisterRequest 객체에 저장
  이메일/성명/PW/PW확인       MemberRegisterService.regist(RegisterRequest)
                                //데이터의 유효성검증 -- 동일 email가진 회원존재여부.
                                //       예외처리(예외클래스)

                                //RegisterRequest 객체 ---> Member 객체로 변환
                                //DB작업전용 객체....  MemberDao 객체생성(얻기)
                                //memberDao.insert 회원등록(DB관련기능 - HashMap,JDBC)

  에메일, 기존PW, 새PW  --->  ChangePasswordService(email,oldPW,newPW)
                                //휴효성 검증

                                //Member객체 -- 패스워드변경
                                //memberDao.update(member객체)
                              
```



##### spring없이, 사용자가 직접 '의존주입' - 로직이해

+ RegisterRequest.java
```java
package spring;

// 사용자의 요청정보를 저장하는 클래스
public class RegisterRequest {
	private String email;
	private String password;
	private String confirmPssword;
	private String name;
	
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getConfirmPssword() {
		return confirmPssword;
	}
	public void setConfirmPssword(String confirmPssword) {
		this.confirmPssword = confirmPssword;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	
	public boolean isPasswordEqualToConfirmPassword() {
		return password.equals(confirmPssword);
	}	
}
```

+ MemberDao.java
```java
package spring;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

//db처리 전담하는 클래스
public class MemberDao {
	private static long nextId = 0;
	
	//db대신 HashMap으로 데이타를 저장하자
	private Map<String,Member> map = new HashMap<String,Member>();
	// map ---->[{"jica@daum.net", 멤버객체},
	//           {"jica2@naver.com", 멤버객체},
	//           {"jica3@gmail.com", 멤버객체}]

	public MemberDao() {
		super();
		System.out.println("MemberDao::MemberDao()...");
	}
	
	public Member selectByEmai(String email) {
		return map.get(email);
	}
	
	public void insert(Member member) {
		member.setId(++nextId);
		
		//아래코드에 실제db에 저장될때 JDBC기능이 사용될 것이다.
		//현재예제에서는 HashMap으로 대용하고 있다.
		map.put(member.getEmail(), member);
	}
	
	public void update(Member member) {
		map.put(member.getEmail(), member);
	}
	
	public Collection<Member> selectAll(){
		return map.values();
	}	
}
```



+ Member.java
```java
package spring;

import java.util.Date;

//db에 저장되는 1건의 데이타를 표현한 클래스
public class Member {
	private Long id;
	private String email;
	private String password;
	private String name;
	private Date registerDate;
	
	public Member(String email, String password, String name, Date registerDate) {
		super();
		this.email = email;
		this.password = password;
		this.name = name;
		this.registerDate = registerDate;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getEmail() {
		return email;
	}

	public String getPassword() {
		return password;
	}

	public String getName() {
		return name;
	}

	public Date getRegisterDate() {
		return registerDate;
	}
	
	public void changePassword(String oldPassword, String newPassword) {
		if( !password.equals(oldPassword)) {
			throw new IdPasswordNotMatchingException();
		}
		this.password = newPassword;
	}
}
```



+ ChangePasswordService.java
```java
package spring;

//실제요청을 처리하는 클래스 - 암호변경
public class ChangePasswordService {
	private MemberDao memberDao;

	public ChangePasswordService() {
		super();
		System.out.println("ChangePasswordService::ChangePasswordService()...");
	}
	
	public ChangePasswordService(MemberDao memberDao) {
		super();
		this.memberDao = memberDao;
		System.out.println("ChangePasswordService::ChangePasswordService(MemberDao)...");		
	}	
	
	//                           jica@daum.net    1234           5678
	public void changePassword(String email, String oldPwd, String newPwd) throws MemberNotFoundException, IdPasswordNotMatchingException {
		Member member = memberDao.selectByEmai(email);
		
		// 존재하지 않는 객체의 패스워드 변경시도이므로 예외발생
		if( member == null) {
			throw new MemberNotFoundException();
		}
		
		member.changePassword(oldPwd, newPwd);
		
		memberDao.update(member);
	}
	
	public void setMemberDao(MemberDao memberDao) {
		this.memberDao = memberDao;
	}
}
```


+ MemberRegisterService.java
```java
package spring;

import java.util.Date;

//실제요청을 처리하는 클래스 - 등록처리
public class MemberRegisterService {
	// 의존관계에 있는 클래스
	private MemberDao memberDao;

	public MemberRegisterService() {
		super();
		System.out.println("MemberRegisterService::MemberRegisterService()...");
	}

	// 생성자에 의한 의존성 주입
	public MemberRegisterService(MemberDao memberDao) {
		super();
		this.memberDao = memberDao;
		System.out.println("MemberRegisterService::MemberRegisterService(MemberDao)...");		
	}  
	
	// 요청정보를 db에 저장하는 기능(등록)
	public void regist(RegisterRequest req) throws AlreadyExistingMemberException {
		// 동일한 email을 가진 회원이 있는지 검색
		Member member = memberDao.selectByEmai(req.getEmail());
		
		if( member != null) {
			throw new AlreadyExistingMemberException("dup email : " + req.getEmail());
		}
		
		// RegisterRequest정보를 Member객체로 변환
		Member newMember = new Member(req.getEmail(), req.getPassword(), req.getName(), new Date());
		memberDao.insert(newMember);
	}	
}
```


+ MainForAssembler.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import spring.AlreadyExistingMemberException;
import spring.ChangePasswordService;
import spring.IdPasswordNotMatchingException;
import spring.MemberDao;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;

public class MainForAssembler {
	//spring에서의 DI기능을 처리하기 전에
	//기존의 프로그램코드에서 직접 DI를 지정하는 방법을 살펴보자 
	
	//방법1)
	static private MemberDao memberDao;
	// 회원 등록 서비스
	static private MemberRegisterService regSvc;
	// 회원 정보 변경 서비스
	static private ChangePasswordService pwdSvc;
	
	static {
		//직접 코드로 의존성 주입
		memberDao = new MemberDao();
		regSvc = new MemberRegisterService(memberDao);
		pwdSvc = new ChangePasswordService(memberDao);
	}
	
	public static void main(String[] args) {
		printHelp();   // 메뉴 보여주기
		
		BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
		while(true) {
			System.out.println("명령어를 입력하세요");
			try {
				String command = reader.readLine();
				//command --> "new jica@daum.net JICA 1234 1234"
				//            "change jica@daum.net 1234 5678"
				if( command.equalsIgnoreCase("exit")) {
					System.out.println("종료합니다.");
					break;
				}
				
				// "new jica@daum.net JICA 1234 1234"
				if( command.startsWith("new")) {
					processNewCommand(command.split(" "));
					continue;
				// "change jica@daum.net 1234 5678"	
				}else if( command.startsWith("change")) {
					processChangeCommand(command.split(" "));
					continue;
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
			
		}

	}
	
	//         "change jica@daum.net 1234 5678"
	//           0           1           2    3  
	// arg ==> [change, jica@daum.net, 1234, 5678]
	private static void processChangeCommand(String arg[]) {
		if( arg.length != 4 ) {
			printHelp();
			return;
		}
		
		try {
			pwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.");
		}catch( MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.");
		}catch( IdPasswordNotMatchingException e1) {
			System.out.println("이메일과 암호가 일치하지 않습니다.");
		}
		
	}
	
	
	//         "new jica@daum.net JICA 1234 1234"
	//           0      1           2    3    4 
	// arg ==> [new,jica@daum.net,JICA,1234,1234]
	private static void processNewCommand(String arg[]) {
		if( arg.length != 5 ) {
			printHelp();
			return;
		}
		
		//사용자가 입력한 내용을 객체로 만든다.
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		// 암호와 확인암호가 같은지 알아본다.
		if( !req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.");
			return;
		}
		
		// 위의 코드는 유효성 검사기능이라고 할수 있다.
		// 실제 기능을 작동
		try {
			regSvc.regist(req);
			System.out.println("신규회원을 등록했습니다.");
		}catch( AlreadyExistingMemberException e) {
			System.out.println("이미 존재하는 이메일 입니다.");
		}
		
	}
	private static void printHelp() {
		System.out.println("--------------------");
		System.out.println("명령어 사용법");
		System.out.println("신규회원 등록 : new 이메일 이름 암호 암호확인");
		System.out.println("암호 변경     : change 이메일 현재비번 변경비번");
		System.out.println("====================");
		
	}
}
```

```java
static {
		//직접 코드로 의존성 주입
		memberDao = new MemberDao();
		regSvc = new MemberRegisterService(memberDao);
		pwdSvc = new ChangePasswordService(memberDao);
	}
```
+ 사용자가 위 코드부분을 직접 입력하여, 강제로 의존성을 주입한 경우이다.
+ 사용자가 직접 개입해서, 모든 컨트롤을 해야하는 부담감이 주어진다.




##### Sptring을 이용한 '의존주입'
+ 위의 사용자 강제 주입코드를.... sptring으로 편하게 관리해보자.


+ Assembler.java
```java
package assembler;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberRegisterService;


//아래의 코드는 프로그램에서 필요한 객체를 관리하는 클래스를 별도로 작성하여 객체를 관리했다.
//아래클래스와 동일한 역활을 spring framework가 담당한다.
public class Assembler {
	private MemberDao memberDao;
	private MemberRegisterService memberRegisterService;
	private ChangePasswordService changePasswordService;
	
	public Assembler() {
		super();
		
		memberDao = new MemberDao();
		memberRegisterService = new MemberRegisterService(memberDao);
		changePasswordService = new ChangePasswordService(memberDao);
	}

	public MemberDao getMemberDao() {
		return memberDao;
	}

	public MemberRegisterService getMemberRegisterService() {
		return memberRegisterService;
	}

	public ChangePasswordService getChangePasswordService() {
		return changePasswordService;
	}
}
```



+ MainForAssembler2.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import assembler.Assembler;

import spring.AlreadyExistingMemberException;
import spring.ChangePasswordService;
import spring.IdPasswordNotMatchingException;
import spring.MemberDao;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;

public class MainForAssembler2 {
	//spring에서의 DI기능을 처리하기 전에
	//기존의 프로그램코드에서 직접 DI를 지정하는 방법을 살펴보자 
	
	//방법2)
	private static Assembler assembler;
	
	public static void main(String[] args) {
		
		assembler = new Assembler();
		System.out.println("---위의코드에 의해 생성/관리되는 객체 ==> spring framework가 대신하게 할것이다--------");
		
		printHelp();   // 메뉴 보여주기
		
		BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
		while(true) {
			System.out.println("명령어를 입력하세요");
			try {
				String command = reader.readLine();
				if( command.equalsIgnoreCase("exit")) {
					System.out.println("종료합니다.");
					break;
				}
				
				// "new jica@daum.net 홍길동 1234 1234"
				if( command.startsWith("new")) {
					processNewCommand(command.split(" "));
					continue;
				// "change jica@daum.net 1234 5678"	
				}else if( command.startsWith("change")) {
					processChangeCommand(command.split(" "));
					continue;
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
			
		}

	}
	
	//           0           1           2    3  
	// arg ==> [change, jica@daum.net, 1234, 5678]
	private static void processChangeCommand(String arg[]) {
		if( arg.length != 4 ) {
			printHelp();
			return;
		}
		
		try {
			ChangePasswordService pwdSvc = assembler.getChangePasswordService();
			pwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.");
		}catch( MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.");
		}catch( IdPasswordNotMatchingException e1) {
			System.out.println("이메일과 암호가 일치하지 않습니다.");
		}
		
	}
	
	
	//           0      1           2    3    4 
	// arg ==> [new,jica@daum.net,홍길동,1234,1234]
	private static void processNewCommand(String arg[]) {
		if( arg.length != 5 ) {
			printHelp();
			return;
		}
		
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		// 암호와 확인암호가 같은지 알아본다.
		if( !req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.");
			return;
		}
		try {
			MemberRegisterService regSvc = assembler.getMemberRegisterService();
			regSvc.regist(req);
			System.out.println("신규회원을 등록했습니다.");
		}catch( AlreadyExistingMemberException e) {
			System.out.println("이미 존재하는 이메일 입니다.");
		}
		
	}
	private static void printHelp() {
		System.out.println("--------------------");
		System.out.println("명령어 사용법");
		System.out.println("신규회원 등록 : new 이메일 이름 암호 암호확인");
		System.out.println("암호 변경     : change 이메일 현재비번 변경비번");
		System.out.println("====================");
		
	}
}
```


```java
private static Assembler assembler;
assembler = new Assembler();

ChangePasswordService pwdSvc = assembler.getChangePasswordService();
MemberRegisterService regSvc = assembler.getMemberRegisterService();
```
+ 위에 코드를 통해 Spring에 의한 의존주입을 만들었다.



##### 교재의 예제를 살펴보자.
+ AppCtx.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberPrinter;
import spring.MemberRegisterService;
import spring.VersionPrinter;

@Configuration
public class AppCtx {

	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	public MemberRegisterService memberRegSvc() {
		return new MemberRegisterService(memberDao());
	}
	
	@Bean
	public ChangePasswordService changePwdSvc() {
		ChangePasswordService pwdSvc = new ChangePasswordService();
		pwdSvc.setMemberDao(memberDao());
		return pwdSvc;
	}
	
	@Bean
	public MemberPrinter memberPrinter() {
		return new MemberPrinter();
	}
	
	@Bean
	public MemberListPrinter listPrinter() {
		return new MemberListPrinter(memberDao(), memberPrinter());
	}
	
	@Bean
	public MemberInfoPrinter infoPrinter() {
		MemberInfoPrinter infoPrinter = new MemberInfoPrinter();
		infoPrinter.setMemberDao(memberDao());
		infoPrinter.setPrinter(memberPrinter());
		return infoPrinter;
	}
	
	@Bean
	public VersionPrinter versionPrinter() {
		VersionPrinter versionPrinter = new VersionPrinter();
		versionPrinter.setMajorVersion(5);
		versionPrinter.setMinorVersion(0);
		return versionPrinter;
	}
}
```


+ MemberInfoPrinter.java
```java
package spring;

public class MemberInfoPrinter {
	private MemberDao memberDao;
	private MemberPrinter printer;
	
	public MemberInfoPrinter() {
		super();
		System.out.println("MemberInfoPrinter::MemberInfoPrinter()...");
	}

	//set 메서드에의한 의존성 주입
	//<bean id="infoPrinter" class="spring.MemberInfoPrinter">
	//   <property name="memberDao"></property>
    //</bean>
	
	public void setMemberDao(MemberDao memberDao) {
		System.out.println("MemberInfoPrinter::setMemberDao(MemberDao)...");
		this.memberDao = memberDao;
	}

	public void setPrinter(MemberPrinter printer) {
		System.out.println("MemberInfoPrinter::setPrinter(MemberPrinter)...");		
		this.printer = printer;
	}
	
	public void printMemberInfo(String email) {
		Member member = memberDao.selectByEmai(email);
		if( member == null) {
			System.out.println("데이타 없음");
			//throw new MemberNotFoundException();
			return;
		}
		
		printer.print(member);
	}
}
```



+ MemberListPrinter.java
```java
package spring;

import java.util.Collection;

public class MemberListPrinter {
	private MemberDao memberDao;
	private MemberPrinter printer;
		
	public MemberListPrinter() {
		super();
		System.out.println("MemberListPrinter::MemberListPrinter()...");
	}


	public MemberListPrinter(MemberDao memberDao, MemberPrinter printer) {
		super();
		System.out.println("MemberListPrinter::MemberListPrinter(MemberDao,MemberPrinter)...");
		this.memberDao = memberDao;
		this.printer = printer;
	}
	
	public void printAll() {
		Collection<Member> members = memberDao.selectAll();
		
		for(Member member : members) {
			printer.print(member);
		}
	}
}
```



+ MemberPrinter.java
```java
package spring;

public class MemberPrinter {
	
	
	public MemberPrinter() {
		super();
		System.out.println("MemberPrinter::MemberPrinter()...");
	}

	public void print(Member member) {
		System.out.printf("회원정보 : 아이디=%d, 이메일=%s, 이름:%s, 등록일=%tF\n",
				member.getId(), member.getEmail(), member.getName(), member.getRegisterDate());
	}
}
```


+ VersionPrinter.java
```java
package spring;

public class VersionPrinter {
	private int majorVersion;
	private int minorVersion;
	
	public VersionPrinter() {
		super();
		System.out.println("VersionPrinter::VersionPrinter()...");
	}

	public void setMajorVersion(int majorVersion) {
		System.out.println("VersionPrinter::setMajorVersion(int)...");
		this.majorVersion = majorVersion;
	}

	public void setMinorVersion(int minorVersion) {
		System.out.println("VersionPrinter::setMinorVersion(int)...");		
		this.minorVersion = minorVersion;
	}
	
	public void print() {
		System.out.printf("이 프로그램의 버전은 %d.%d 입니다\n\n", majorVersion, minorVersion);
	}
}
```



+ MainForSpringBook.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppCtx;
import spring.AlreadyExistingMemberException;
import spring.ChangePasswordService;
import spring.DuplicateMemberException;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.VersionPrinter;
import spring.WrongIdPasswordException;

public class MainForSpringBook {

	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) throws IOException {
		ctx = new AnnotationConfigApplicationContext(AppCtx.class);
		
		BufferedReader reader = 
				new BufferedReader(new InputStreamReader(System.in));
		while (true) {
			System.out.println("명령어를 입력하세요:");
			String command = reader.readLine();
			if (command.equalsIgnoreCase("exit")) {
				System.out.println("종료합니다.");
				break;
			}
			if (command.startsWith("new ")) {
				processNewCommand(command.split(" "));
				continue;
			} else if (command.startsWith("change ")) {
				processChangeCommand(command.split(" "));
				continue;
			} else if (command.equals("list")) {
				processListCommand();
				continue;
			} else if (command.startsWith("info ")) {
				processInfoCommand(command.split(" "));
				continue;
			} else if (command.equals("version")) {
				processVersionCommand();
				continue;
			}
			printHelp();
		}
	}

	private static void processNewCommand(String[] arg) {
		if (arg.length != 5) {
			printHelp();
			return;
		}
		MemberRegisterService regSvc = 
				ctx.getBean("memberRegSvc", MemberRegisterService.class);
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		if (!req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.\n");
			return;
		}
		try {
			regSvc.regist(req);
			System.out.println("등록했습니다.\n");
		} catch (DuplicateMemberException e) {
			System.out.println("이미 존재하는 이메일입니다1.\n");
		} catch (AlreadyExistingMemberException e) {
			System.out.println("이미 존재하는 이메일입니다2.\n");
		}
	}

	private static void processChangeCommand(String[] arg) {
		if (arg.length != 4) {
			printHelp();
			return;
		}
		ChangePasswordService changePwdSvc = 
				ctx.getBean("changePwdSvc", ChangePasswordService.class);
		try {
			changePwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.\n");
		} catch (MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.\n");
		} catch (WrongIdPasswordException e) {
			System.out.println("이메일과 암호가 일치하지 않습니다.\n");
		}
	}

	private static void printHelp() {
		System.out.println();
		System.out.println("잘못된 명령입니다. 아래 명령어 사용법을 확인하세요.");
		System.out.println("명령어 사용법:");
		System.out.println("new 이메일 이름 암호 암호확인");
		System.out.println("change 이메일 현재비번 변경비번");
		System.out.println("전체 회원정보 출력 : list");
		System.out.println("회원정보 출력 : info 이메일");
		System.out.println("버전정보 : version");		
		System.out.println();
	}

	private static void processListCommand() {
		MemberListPrinter listPrinter = 
				ctx.getBean("listPrinter", MemberListPrinter.class);
		listPrinter.printAll();
	}

	private static void processInfoCommand(String[] arg) {
		if (arg.length != 2) {
			printHelp();
			return;
		}
		MemberInfoPrinter infoPrinter = 
				ctx.getBean("infoPrinter", MemberInfoPrinter.class);
		infoPrinter.printMemberInfo(arg[1]);
	}
	
	private static void processVersionCommand() {
		VersionPrinter versionPrinter = 
				ctx.getBean("versionPrinter", VersionPrinter.class);
		versionPrinter.print();
	}
}
```

+ 스프링 내부에서 사용자가 만든 설정파일 클래스를 상속받아새로운 클래스를 만들어서 이것을 사용한다.

+ 위 프로그램을 실행하면 다음과 같이 출력된다.
```
MemberDao::MemberDao()...
MemberRegisterService::MemberRegisterService(MemberDao)...
ChangePasswordService::ChangePasswordService()...
MemberPrinter::MemberPrinter()...
MemberListPrinter::MemberListPrinter(MemberDao,MemberPrinter)...
MemberInfoPrinter::MemberInfoPrinter()...
MemberInfoPrinter::setMemberDao(MemberDao)...
MemberInfoPrinter::setPrinter(MemberPrinter)...
VersionPrinter::VersionPrinter()...
VersionPrinter::setMajorVersion(int)...
VersionPrinter::setMinorVersion(int)...
명령어를 입력하세요:

```
+ 즉, 프로그램이 실행이 되기도 전에... Spring이 관리하는 모든 객체들이 생성된다.

+ RegisterRequest, Member 는 사용자가 필요한 시점에서 생성해서 사용
  + 단독으로 사용하는 기능이기 때문.

+ 주로 여러클래스에서 공유되어질 객체와 핵심서비스를 수행하는 객체를 ApplicationContext에서 관리
  + MemberDao, MemberInfoPrint.... MemberRegisterService, ChangePasswordService, MemberPrinter





##### Spring을 이제 Annotaion xml으로 만들어보자.
+ appCtx.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd">
		
	<!-- spring.MemberDao형 객체를 생성할때 인자가없는 생성자를 호출하고 생성된 객체의 식별자를
	     memberDao로 한다.  이후 이 객체를 spring framework이 관리한다. -->	
	<bean id="memberDao" class="spring.MemberDao">
	</bean>
	
	<!-- spring.MemberRegisterService형 객체를 생성할때 인자값으로 이미 만들어진 객체 즉,
	      memberDao를 식별자로 가지는 객체를 전달하여 객체를 생성하고 생성된 객체의 식별자를 
	      memberRegSvc로 한다. 이후 이 객체를 spring framework이 관리한다. -->	
	<bean id="memberRegSvc" class="spring.MemberRegisterService">
		<constructor-arg ref="memberDao"/>
	</bean>
	
	<bean id="changePwdSvc" class="spring.ChangePasswordService">
		<constructor-arg ref="memberDao"/>
	</bean>
</beans>
```

+ 생성자에 의한 의존성주입부분
```xml
<constructor-arg ref="memberDao"/>
```



+ MainForSpring.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import assembler.Assembler;
import spring.AlreadyExistingMemberException;
import spring.ChangePasswordService;
import spring.IdPasswordNotMatchingException;
import spring.MemberDao;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;

public class MainForSpring {
	//spring에서의 DI기능 - 설정정보화일(xml) 사용
	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) {
		
		ctx = new GenericXmlApplicationContext("classpath:appCtx.xml");
		System.out.println("위의코드에 의해 설정정보화일(appCtx.xml)의 내용대로 객체가 생성되어 진다.--------");
		
		printHelp();   // 메뉴 보여주기
		
		BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
		while(true) {
			System.out.println("명령어를 입력하세요");
			try {
				String command = reader.readLine();
				if( command.equalsIgnoreCase("exit")) {
					System.out.println("종료합니다.");
					break;
				}
				
				// "new jica@daum.net 홍길동 1234 1234"
				if( command.startsWith("new")) {
					processNewCommand(command.split(" "));
					continue;
				// "change jica@daum.net 1234 5678"	
				}else if( command.startsWith("change")) {
					processChangeCommand(command.split(" "));
					continue;
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
	
	//           0           1           2    3  
	// arg ==> [change, jica@daum.net, 1234, 5678]
	private static void processChangeCommand(String arg[]) {
		if( arg.length != 4 ) {
			printHelp();
			return;
		}
		
		try {
			ChangePasswordService pwdSvc = ctx.getBean("changePwdSvc",ChangePasswordService.class);
			pwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.");
		}catch( MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.");
		}catch( IdPasswordNotMatchingException e1) {
			System.out.println("이메일과 암호가 일치하지 않습니다.");
		}
		
	}
	
	
	//           0      1           2    3    4 
	// arg ==> [new,jica@daum.net,홍길동,1234,1234]
	private static void processNewCommand(String arg[]) {
		if( arg.length != 5 ) {
			printHelp();
			return;
		}
		
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		// 암호와 확인암호가 같은지 알아본다.
		if( !req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.");
			return;
		}
		try {
			/*
			<bean id="memberRegSvc" class="spring.MemberRegisterService">
				<constructor-arg ref="memberDao"/>
			</bean>
			 */
			MemberRegisterService regSvc = ctx.getBean("memberRegSvc", MemberRegisterService.class);			
			regSvc.regist(req);
			System.out.println("신규회원을 등록했습니다.");
		}catch( AlreadyExistingMemberException e) {
			System.out.println("이미 존재하는 이메일 입니다.");
		}
		
	}
	private static void printHelp() {
		System.out.println("--------------------");
		System.out.println("명령어 사용법");
		System.out.println("신규회원 등록 : new 이메일 이름 암호 암호확인");
		System.out.println("암호 변경     : change 이메일 현재비번 변경비번");
		System.out.println("====================");
		
	}
}
```

+ 실행화면
```
MemberDao::MemberDao()...
MemberRegisterService::MemberRegisterService(MemberDao)...
ChangePasswordService::ChangePasswordService(MemberDao)...
위의코드에 의해 설정정보화일(appCtx.xml)의 내용대로 객체가 생성되어 진다.--------
--------------------
명령어 사용법
신규회원 등록 : new 이메일 이름 암호 암호확인
암호 변경     : change 이메일 현재비번 변경비번
====================
명령어를 입력하세요

```

##### 인자가 2개인 생성자 사용
+ appCtx2.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd">
	
		
	<!-- spring.MemberDao형 객체를 생성할때 인자가없는 생성자를 호출하고 생성된 객체의 식별자를
	     memberDao로 한다.  이후 이 객체를 spring framework이 관리한다. -->	
	<bean id="memberDao" class="spring.MemberDao">
	</bean>
	
	
	<!-- spring.MemberRegisterService형 객체를 생성할때 인자값으로 이미 만들어진 객체 즉,
	      memberDao를 식별자로 가지는 객체를 전달하여 객체를 생성하고 생성된 객체의 식별자를 
	      memberRegSvc로 한다. 이후 이 객체를 spring framework이 관리한다. -->	
	<bean id="memberRegSvc" class="spring.MemberRegisterService">
		<constructor-arg ref="memberDao"/>
	</bean>
	
	<bean id="changePwdSvc" class="spring.ChangePasswordService">
		<constructor-arg ref="memberDao"/>
	</bean>
	
	<bean id="memberPrinter" class="spring.MemberPrinter">
	</bean>
	
	<!-- 아래의 설정에 의해서 MemberListPrinter객체가 생성되어질때 인자가 2개인 생성자가 호출되어진다. -->
	<bean id="listPrinter" class="spring.MemberListPrinter">
		<constructor-arg ref="memberDao"/>
		<constructor-arg ref="memberPrinter"/>
	</bean>
	
	<!-- 아래의 설정에 의핸 객체가 생성되어지고 set메서드가 동작한다 -->
	<bean id="infoPrinter" class="spring.MemberInfoPrinter">
		<property name="memberDao" ref="memberDao"/>
		<property name="printer" ref="memberPrinter" />
	</bean>
	
	<!-- 생성자, set메서드에 기본 데이타를 전달할 수 도 있다. -->
	<bean id="versionPrinter" class="spring.VersionPrinter">
		<property name="majorVersion" value="4"/>
		<property name="minorVersion" value="1"/>
	</bean>
			<!-- 
			  <property name="minorVersion">
			      <value>1</value>
		    </property>
		    -->
</beans>
```

+ MemberInfoPrinter obj가 있다면... obj.setMemberDao(memberDao객체)
  + set메서드에 의한 의존성주입!!
```xml
<!-- ref : 객체 -->
		<property name="memberDao" ref="memberDao"/>
		<property name="printer" ref="memberPrinter" />
<!-- value : 기본자료형 -->
		<property name="majorVersion" value="4"/>
		<property name="minorVersion" value="1"/>
```




+ MainForSpring2.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import assembler.Assembler;
import spring.AlreadyExistingMemberException;
import spring.ChangePasswordService;
import spring.IdPasswordNotMatchingException;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.VersionPrinter;

public class MainForSpring2 {
	//spring에서의 DI기능 - 설정정보화일 사용
	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) {
		
		ctx = new GenericXmlApplicationContext("classpath:appCtx2.xml");
		System.out.println("위의코드에 의해 설정정보화일(appCtx2.xml)의 내용대로 객체가 생성되어 진다.--------");
		
		printHelp();   // 메뉴 보여주기
		
		BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
		while(true) {
			System.out.println("명령어를 입력하세요");
			try {
				String command = reader.readLine();
				if( command.equalsIgnoreCase("exit")) {
					System.out.println("종료합니다.");
					break;
				}
				
				// "new jica@daum.net 홍길동 1234 1234"
				if( command.startsWith("new")) {
					processNewCommand(command.split(" "));
					continue;
				// "change jica@daum.net 1234 5678"	
				}else if( command.startsWith("change")) {
					processChangeCommand(command.split(" "));
					continue;
				}else if( command.equals("list")) {
					processListCommand();
					continue;
				// "info jica@daum.net"	
				}else if( command.startsWith("info")) {
					processInfoCommand(command.split(" "));
					continue;
				}else if(command.equals("version")) {
					processVersionCommand();
					continue;
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
			
		}

	}
	
	
	private static void processVersionCommand() {
		VersionPrinter versionPrinter = ctx.getBean("versionPrinter", VersionPrinter.class);
		versionPrinter.print();
	}
	//           0           1      
	// arg ==> [info, jica@daum.net]
	private static void processInfoCommand(String arg[]) {
		if( arg.length != 2) {
			printHelp();
			return;
		}
		
		MemberInfoPrinter infoPrinter = ctx.getBean("infoPrinter", MemberInfoPrinter.class);
		infoPrinter.printMemberInfo(arg[1]);
	}
	
	private static void processListCommand() {
		MemberListPrinter listPrinter = ctx.getBean("listPrinter", MemberListPrinter.class);
		listPrinter.printAll();
	}
	
	//           0           1           2    3  
	// arg ==> [change, jica@daum.net, 1234, 5678]
	private static void processChangeCommand(String arg[]) {
		if( arg.length != 4 ) {
			printHelp();
			return;
		}
		
		try {
			ChangePasswordService pwdSvc = ctx.getBean("changePwdSvc",ChangePasswordService.class);
			pwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.");
		}catch( MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.");
		}catch( IdPasswordNotMatchingException e1) {
			System.out.println("이메일과 암호가 일치하지 않습니다.");
		}
	}
	
	
	//           0      1           2    3    4 
	// arg ==> [new,jica@daum.net,홍길동,1234,1234]
	private static void processNewCommand(String arg[]) {
		if( arg.length != 5 ) {
			printHelp();
			return;
		}
		
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		// 암호와 확인암호가 같은지 알아본다.
		if( !req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.");
			return;
		}
		try {
			/*
			<bean id="memberRegSvc" class="spring.MemberRegisterService">
				<constructor-arg ref="memberDao"/>
			</bean>
			 */
			MemberRegisterService regSvc = ctx.getBean("memberRegSvc", MemberRegisterService.class);			
			regSvc.regist(req);
			System.out.println("신규회원을 등록했습니다.");
		}catch( AlreadyExistingMemberException e) {
			System.out.println("이미 존재하는 이메일 입니다.");
		}
		
	}
	private static void printHelp() {
		System.out.println("--------------------");
		System.out.println("명령어 사용법");
		System.out.println("신규회원 등록 : new 이메일 이름 암호 암호확인");
		System.out.println("암호 변경     : change 이메일 현재비번 변경비번");
		System.out.println("전체 회원정보 출력 : list");
		System.out.println("회원정보 출력 : info 이메일");
		System.out.println("버전정보 : version");
		System.out.println("====================");	
	}
}
```



#### 3. project 실습
#### 4. Summary / Close



-------------------------------------------------------------------------

### [2019-07-19]

#### 1. Review

#### 2. SpringFramework


##### Spring을 활용한 의존주입 4번째 방법

+ MainForSpring3.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;
import assembler.Assembler;
import spring.AlreadyExistingMemberException;
import spring.ChangePasswordService;
import spring.IdPasswordNotMatchingException;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.VersionPrinter;

public class MainForSpring3 {
	//spring에서의 DI기능 - 설정정보화일 사용
	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) {
		
		//설정화일을 여러개 사용할때
		String conf[] = {"classpath:conf1.xml","classpath:conf2.xml"};		
		ctx = new GenericXmlApplicationContext(conf);
		System.out.println("위의코드에 의해 설정정보화일 2개를 이용하여 빈객체를 생성하면서 의존성 주입을 한다.--------");
		
		printHelp();   // 메뉴 보여주기
		
		BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
		while(true) {
			System.out.println("명령어를 입력하세요");
			try {
				String command = reader.readLine();
				if( command.equalsIgnoreCase("exit")) {
					System.out.println("종료합니다.");
					break;
				}
				
				// "new jica@daum.net 홍길동 1234 1234"
				if( command.startsWith("new")) {
					processNewCommand(command.split(" "));
					continue;
				// "change jica@daum.net 1234 5678"	
				}else if( command.startsWith("change")) {
					processChangeCommand(command.split(" "));
					continue;
				}else if( command.equals("list")) {
					processListCommand();
					continue;
				// "info jica@daum.net"	
				}else if( command.startsWith("info")) {
					processInfoCommand(command.split(" "));
					continue;
				}else if(command.equals("version")) {
					processVersionCommand();
					continue;
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
	
	
	private static void processVersionCommand() {
		VersionPrinter versionPrinter = ctx.getBean("versionPrinter", VersionPrinter.class);
		versionPrinter.print();
	}
	//           0           1      
	// arg ==> [info, jica@daum.net]
	private static void processInfoCommand(String arg[]) {
		if( arg.length != 2) {
			printHelp();
			return;
		}
		
		MemberInfoPrinter infoPrinter = ctx.getBean("infoPrinter", MemberInfoPrinter.class);
		infoPrinter.printMemberInfo(arg[1]);
	}
	
	private static void processListCommand() {
		MemberListPrinter listPrinter = ctx.getBean("listPrinter", MemberListPrinter.class);
		listPrinter.printAll();
	}
	
	//           0           1           2    3  
	// arg ==> [change, jica@daum.net, 1234, 5678]
	private static void processChangeCommand(String arg[]) {
		if( arg.length != 4 ) {
			printHelp();
			return;
		}
		
		try {
			ChangePasswordService pwdSvc = ctx.getBean("changePwdSvc",ChangePasswordService.class);
			pwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.");
		}catch( MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.");
		}catch( IdPasswordNotMatchingException e1) {
			System.out.println("이메일과 암호가 일치하지 않습니다.");
		}
		
	}
	
	
	//           0      1           2    3    4 
	// arg ==> [new,jica@daum.net,홍길동,1234,1234]
	private static void processNewCommand(String arg[]) {
		if( arg.length != 5 ) {
			printHelp();
			return;
		}
		
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		// 암호와 확인암호가 같은지 알아본다.
		if( !req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.");
			return;
		}
		try {
			/*
			<bean id="memberRegSvc" class="spring.MemberRegisterService">
				<constructor-arg ref="memberDao"/>
			</bean>
			 */
			MemberRegisterService regSvc = ctx.getBean("memberRegSvc", MemberRegisterService.class);			
			regSvc.regist(req);
			System.out.println("신규회원을 등록했습니다.");
		}catch( AlreadyExistingMemberException e) {
			System.out.println("이미 존재하는 이메일 입니다.");
		}
		
	}
	private static void printHelp() {
		System.out.println("--------------------");
		System.out.println("명령어 사용법");
		System.out.println("신규회원 등록 : new 이메일 이름 암호 암호확인");
		System.out.println("암호 변경     : change 이메일 현재비번 변경비번");
		System.out.println("전체 회원정보 출력 : list");
		System.out.println("회원정보 출력 : info 이메일");
		System.out.println("버전정보 : version");
		System.out.println("====================");	
	}
}
```



+ MainForSpring4.java
```java
```




#### 3. project 실습
#### 4. Summary / Close




-------------------------------------------------------------------------

### [2019-07-22]

#### 1. Review

#### 2. SpringFramework
+ MainForSpringBook2.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppConf1;
import config.AppConf2;
import spring.ChangePasswordService;
import spring.DuplicateMemberException;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.VersionPrinter;
import spring.WrongIdPasswordException;

public class MainForSpringBook2 {

	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) throws IOException {
		ctx = new AnnotationConfigApplicationContext(AppConf1.class, AppConf2.class);
		
		BufferedReader reader = 
				new BufferedReader(new InputStreamReader(System.in));
		while (true) {
			System.out.println("명령어를 입력하세요:");
			String command = reader.readLine();
			if (command.equalsIgnoreCase("exit")) {
				System.out.println("종료합니다.");
				break;
			}
			if (command.startsWith("new ")) {
				processNewCommand(command.split(" "));
				continue;
			} else if (command.startsWith("change ")) {
				processChangeCommand(command.split(" "));
				continue;
			} else if (command.equals("list")) {
				processListCommand();
				continue;
			} else if (command.startsWith("info ")) {
				processInfoCommand(command.split(" "));
				continue;
			} else if (command.equals("version")) {
				processVersionCommand();
				continue;
			}
			printHelp();
		}
	}

	private static void processNewCommand(String[] arg) {
		if (arg.length != 5) {
			printHelp();
			return;
		}
		MemberRegisterService regSvc =
				ctx.getBean("memberRegSvc", MemberRegisterService.class);
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		if (!req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.\n");
			return;
		}
		try {
			regSvc.regist(req);
			System.out.println("등록했습니다.\n");
		} catch (DuplicateMemberException e) {
			System.out.println("이미 존재하는 이메일입니다.\n");
		}
	}

	private static void processChangeCommand(String[] arg) {
		if (arg.length != 4) {
			printHelp();
			return;
		}
		ChangePasswordService changePwdSvc = 
				ctx.getBean("changePwdSvc", ChangePasswordService.class);
		try {
			changePwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.\n");
		} catch (MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.\n");
		} catch (WrongIdPasswordException e) {
			System.out.println("이메일과 암호가 일치하지 않습니다.\n");
		}
	}

	private static void printHelp() {
		System.out.println();
		System.out.println("잘못된 명령입니다. 아래 명령어 사용법을 확인하세요.");
		System.out.println("명령어 사용법:");
		System.out.println("new 이메일 이름 암호 암호확인");
		System.out.println("change 이메일 현재비번 변경비번");
		System.out.println();
	}

	private static void processListCommand() {
		MemberListPrinter listPrinter = 
				ctx.getBean("listPrinter", MemberListPrinter.class);
		listPrinter.printAll();
	}

	private static void processInfoCommand(String[] arg) {
		if (arg.length != 2) {
			printHelp();
			return;
		}
		MemberInfoPrinter infoPrinter = 
				ctx.getBean("infoPrinter", MemberInfoPrinter.class);
		infoPrinter.printMemberInfo(arg[1]);
	}
	
	private static void processVersionCommand() {
		VersionPrinter versionPrinter = 
				ctx.getBean("versionPrinter", VersionPrinter.class);
		versionPrinter.print();
	}
}
```



+ AppConfi1.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import spring.MemberDao;
import spring.MemberPrinter;

@Configuration
public class AppConf1 {
	public AppConf1() {
		System.out.println("AppConfi1 생성자 작동");
	}

	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	public MemberPrinter memberPrinter() {
		return new MemberPrinter();
	}
}
```



+ AppConfi2.java
```java
package config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberPrinter;
import spring.MemberRegisterService;
import spring.VersionPrinter;

@Configuration
public class AppConf2 {
		
	@Autowired
	//현재 ApplicationContext가 관리하는 객체를 자동으로 연결...(같은자료형)의존자동주입
	private MemberDao memberDao;
	
	@Autowired  //의존자동주입
	private MemberPrinter memberPrinter;
		
	
	
	public AppConf2() {
		System.out.println("AppConf2 생성자 작동");
	}

	@Bean
	public MemberRegisterService memberRegSvc() {
		return new MemberRegisterService(memberDao);
	}
	
	@Bean
	public ChangePasswordService changePwdSvc() {
		ChangePasswordService pwdSvc = new ChangePasswordService();
		pwdSvc.setMemberDao(memberDao);
		return pwdSvc;
	}
	
	@Bean
	public MemberListPrinter listPrinter() {
		return new MemberListPrinter(memberDao, memberPrinter);
	}
	
	@Bean
	public MemberInfoPrinter infoPrinter() {
		MemberInfoPrinter infoPrinter = new MemberInfoPrinter();
		infoPrinter.setMemberDao(memberDao);
		infoPrinter.setPrinter(memberPrinter);
		return infoPrinter;
	}
	
	@Bean
	public VersionPrinter versionPrinter() {
		VersionPrinter versionPrinter = new VersionPrinter();
		versionPrinter.setMajorVersion(5);
		versionPrinter.setMinorVersion(0);
		return versionPrinter;
	}
}

```



+ 의존자동주입
```java
	@Autowired  //의존자동주입
	private MemberDao memberDao;
	
	@Autowired  //의존자동주입
	private MemberPrinter memberPrinter;

	//다른 클래스에서 만들어진 memverDao의 객체를 함께 사용하기 위한 자동주입.
```





+ AppConfImport.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

import spring.MemberDao;
import spring.MemberPrinter;

@Configuration
@Import({AppConf2.class})  // AppConf2.class를 그대로 가져와 사용함.
public class AppConfImport {

	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	public MemberPrinter memberPrinter() {
		return new MemberPrinter();
	}
}
```



+ MainForImport.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppConfImport;
import spring.ChangePasswordService;
import spring.DuplicateMemberException;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.VersionPrinter;
import spring.WrongIdPasswordException;

public class MainForImport {

	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) throws IOException {
		ctx = new AnnotationConfigApplicationContext(AppConfImport.class);
		// AppConfImport.class 하나만 참조하지만.... 결극 그 안에 다른 클래스를 Import!
		// 두개의 클래스를 쓰는것과 같은 효과
		
		BufferedReader reader = 
				new BufferedReader(new InputStreamReader(System.in));
		while (true) {
			System.out.println("명령어를 입력하세요:");
			String command = reader.readLine();
			if (command.equalsIgnoreCase("exit")) {
				System.out.println("종료합니다.");
				break;
			}
			if (command.startsWith("new ")) {
				processNewCommand(command.split(" "));
				continue;
			} else if (command.startsWith("change ")) {
				processChangeCommand(command.split(" "));
				continue;
			} else if (command.equals("list")) {
				processListCommand();
				continue;
			} else if (command.startsWith("info ")) {
				processInfoCommand(command.split(" "));
				continue;
			} else if (command.equals("version")) {
				processVersionCommand();
				continue;
			}
			printHelp();
		}
	}

	private static void processNewCommand(String[] arg) {
		if (arg.length != 5) {
			printHelp();
			return;
		}
        MemberRegisterService regSvc = 
                ctx.getBean("memberRegSvc", MemberRegisterService.class);
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPssword(arg[4]);
		
		if (!req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.\n");
			return;
		}
		try {
			regSvc.regist(req);
			System.out.println("등록했습니다.\n");
		} catch (DuplicateMemberException e) {
			System.out.println("이미 존재하는 이메일입니다.\n");
		}
	}

	private static void processChangeCommand(String[] arg) {
		if (arg.length != 4) {
			printHelp();
			return;
		}
        ChangePasswordService changePwdSvc = 
                ctx.getBean("changePwdSvc", ChangePasswordService.class);
		try {
			changePwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.\n");
		} catch (MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.\n");
		} catch (WrongIdPasswordException e) {
			System.out.println("이메일과 암호가 일치하지 않습니다.\n");
		}
	}

	private static void printHelp() {
		System.out.println();
		System.out.println("잘못된 명령입니다. 아래 명령어 사용법을 확인하세요.");
		System.out.println("명령어 사용법:");
		System.out.println("new 이메일 이름 암호 암호확인");
		System.out.println("change 이메일 현재비번 변경비번");
		System.out.println();
	}

	private static void processListCommand() {
		MemberListPrinter listPrinter = 
				ctx.getBean("listPrinter", MemberListPrinter.class);
		listPrinter.printAll();
	}

	private static void processInfoCommand(String[] arg) {
		if (arg.length != 2) {
			printHelp();
			return;
		}
		MemberInfoPrinter infoPrinter = 
				ctx.getBean("infoPrinter", MemberInfoPrinter.class);
		infoPrinter.printMemberInfo(arg[1]);
	}
	
	private static void processVersionCommand() {
		VersionPrinter versionPrinter = 
				ctx.getBean("versionPrinter", VersionPrinter.class);
		versionPrinter.print();
	}
}
```




+ getBean()의 사용
```java
MemberListPrinter listPrinter = 
				ctx.getBean("listPrinter", MemberListPrinter.class);
				//ctx.getBean("listPrinter2", MemberListPrinter.class);
				// 식발자 listPrinter2가 없으므로 NoSuchBeanDefinitionException 발생

VersionPrinter versionPrinter = 
				ctx.getBean("versionPrinter", VersionPrinter.class);

////////////////////////////////////////////////////////////////
//아래의 이름이 다음과 같기때문에 위와 같이 맞춰서 작성해야한다.
@Bean
	public MemberListPrinter listPrinter() {
		return new MemberListPrinter(memberDao(), memberPrinter());
	}

@Bean
	public VersionPrinter versionPrinter() {
		VersionPrinter versionPrinter = new VersionPrinter();
		versionPrinter.setMajorVersion(5);
		versionPrinter.setMinorVersion(0);
		return versionPrinter;
	}
```

+ 만약 빈 이름을 지정하지 않고 타입으로 빈을 구할 수 있다.
```java
VersionPrinter versionPrinter = ctx.getBean(MemberPrinter.class);
```


+ 만약 클래스에 "@Bean"이 없는 메서드가 있다면 그것은 빈이 아니라 불러올 수 없다.
  + 기능은 정상으로 작동하나....ApplicationContext가 찾아내주 못해서 쓸수 없다.





##### 의존자동주입
+ 지금까지 우리는 생성자를 이용한 주입, set메서드를 이용한 주입을 해왔으나... 자동으로 가능.

+ DI(의존 주입방법 -- 1) 명시적 의존주입-chap03  2) 자동의존주입-chap04

1. 설정화일의 '<bean>' 태그를 이용한 명시적인 의존 주입
```xml
   	<bean id="memberDao" class="spring.MemberDao">
	</bean>
	
	<!-- 생성자에 의한 의존성 주입 -->
	<bean id="memberRegSvc" class="spring.MemberRegisterService">
		<constructor-arg ref="memberDao" />
	</bean>
	
	<bean id="memberPrinter" class="spring.MemberPrinter">
	</bean>
	
	<!-- 멤버변수 즉 set Method에 의한 의존성 주입 -->
	<bean id="infoPrinter" class="spring.MemberInfoPrinter">
		<property name="memberDao" ref="memberDao" />
		<property name="printer" ref="memberPrinter" />
	</bean>	
```



2. 자동 의존 주입
```
    1) 설정화일에 <construct-arg, <property 태그를 사용하지 않는다.==> 자동주입

		<교재>
    2) 빈객체를 생성하려는 설정파일이 있고(인자가 없는생성자) 이때 작동하는 자바클래스에 
      @Autowired, @Resource를 사용하여 의존을 자동주입한다.
                                      ======
      springframework에서의 spring container 즉, ApplicationContext객체가
        내부적으로 생성하여 관리하고 있는 객체에서 검색하여 의존성을 주입해 준다.
                                                  ------
                                 (byName(식별자) , byType(클래스형))     
      
    ****  설정화일에 아래의 태그를 반드시 추가해야 한다. ****  
	<context:annotation-config />
	     springframework가 관리하는 빈객체들을 검색하여 자동의존을 설정하는 기능을 수행하는
	      내부적인 빈을 생성하는 코드다
   	
   	<bean id="memberDao" class="spring.MemberDao">
	</bean>
	
	<bean id="memberRegSvc" class="spring.MemberRegisterService">
	</bean>
	
	*** 자바 코드에서 자동의존주입과 관련된 Annotation을 사용한다.
	MemberRegistService.java화일의 
	       필드나 생성자, setMethod에 @Autowired , @Resources Annotation 을 사용-->




	1) 생성자에 Autowired       
	<bean id="memberRegSvc" class="spring.MemberRegisterService">
	</bean>
	



	2) 필드에 Autowired
	<bean id="infoPrinter" class="spring.MemberInfoPrinter">
	</bean>
	
	@Autowired   ----> 해당객체 생성직후에 ApplicationContext가 관리하는 객체중
	                            자료형이 MemberDao객체의 참조값을 직접 연결한다.  
	MemberDao memDao; 
	



	3) set Method 에 Autowired
	<bean id="infoPrinter" class="spring.MemberInfoPrinter">
	</bean>

	@Autowired  ---> 해당객체 생성직후에 아래의 메서드를 실행시키되
				인자로 전달되는 값은 	ApplicationContext가 관리하는 객체중
				MemberPrinter형 객체의 참조값을 전달하여 실행시킨다.				
	public void setPrinter(MemberPrinter printer) {
		System.out.println("MemberInfoPrinter::setPrinter(MemberPrinter)..." + printer);
		this.printer = printer;
	}
	===========================================================
	@Autowired  ---> 해당객체 생성직후에 아래의 메서드를 실행시키되
				인자로 전달되는 값은 	ApplicationContext가 관리하는 객체중
				MemberPrinter형 객체의 참조값을 전달하여 실행시킨다.
				 
				만일 동일한 자료형의 객체가 여러개일때는 qualifier의 value가 sysout 객체로 연결시켜 실행하시오
	<bean id="printer1" class="spring.MemberPrinter">
 		<qualifier value="sysout" />
	</bean>
	
	@Autowired	
	@Qualifier("sysout")						
	public void setPrinter(MemberPrinter printer) {
		System.out.println("MemberInfoPrinter::setPrinter(MemberPrinter)..." + printer);
		this.printer = printer;
	}	
	==============================================
	여기까지의 내용으로 주의할점은 @Autowired는 자료형을 기준으로 연결된다. ****
		@Autowird가 적용되는 순서
		1. 타입이 같은 빈객체를 검색한다.  없으면 예외발생
				1개면 해당 빈객체를 사용
				@Qualifier가 명시되어 있으면 해당 빈객체를 사용--> 없으면 예외발생
		2. 타입이 같은 빈객체가 여러개이면 @Qualifier가 명시되어 있으면 해당 빈객체를 적용
		3. @Qualifier가 없다면 이름이 같은 객체를 찾는다. 존재하면 적용하고 없으면 예외 발생   




	4) @Resource --> 객체명 즉, 식별자로 연결시킨다.
	 빈객체의 id를 기준으로 연결시킬수 있다.	
	    
	  사용법 1)
	   필드에 사용
	   @Resource(name="memberDao")
	   private MemberDao memDao;
	   
	   @Resource(name="memberPrinter")
	   public void setPrinter(MemberPrinter printer) {
		  System.out.println("MemberInfoPrinter::setPrinter(MemberPrinter)..." + printer);
		  this.printer = printer;
	   }
	   ==> 어플리케이션 컨텍스트 에서 id가 memberDao , id가 memberPrinter인 객체를 찾아
	   연결시킨다.  만일 동일 id를 가지는 객체가 존재하지 않으면 예외를 발생시킨다.


	  ** 생성자에 적용할 수 없고 필드나 메서드에만 적용할 수 있다. 
	  
	 사용법 2)
	   @Resource
	   private MemberDao memberDao;
	   
	   @Resource
	   public void setPrinter(MemberPrinter printer) {
		  System.out.println("MemberInfoPrinter::setPrinter(MemberPrinter)..." + printer);
		  this.printer = printer;
	   }
	   ==> @Resource(name="식별자명")에서 name속성이 생략될때는 자료형을 기준으로 연결한다.
	          즉, MemberDao형 spring빈객체, MemberPrinter형 spring빈객체를 연결한다.
	          
      @Resource의 적용순선
      1. name속성에 지정된 빈객체를 찾는다. 존재하면 해당 객체 주입
      2. name속성이 생략되었다면 동일한 자료형의 빈객체를 찾아서 주입한다.
      3. name속성이 없고 동일한 자료형의 빈객체가 두개 이상일 경우 같은 이름을 가지 빈개를 주입한다.
      4.  //            //                  //    같은 이름을 가진 객체가 없다면 @Qualifier를 이용하여 빈객체를 찾는다.
           없다면 예외발생	          
-------------------------------
만일 의존주입이
명시적주입과(<constructor-arg
          <property)
자동주입이(@Autowired,@Qulifier
        @Resource)   
 동시에 사용되었다면 명시적주입을 우선한다.
```

+ chap04/AppCtx.java
```java
package config;

import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberPrinter;
import spring.MemberRegisterService;
import spring.MemberSummaryPrinter;
import spring.VersionPrinter;

@Configuration
public class AppCtx {
	
	

	public AppCtx() {
		System.out.println("AppCtx::AppCtx().... 인자없는 생성자 작동!! ");
	}

	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	public MemberRegisterService memberRegSvc() {
		return new MemberRegisterService();
	}
	
	@Bean
	public ChangePasswordService changePwdSvc() {
		return new ChangePasswordService();
	}
	
	//식별자를 별도로 지정하지 않으면 메서드명이 식별자가 된다. -- memberPrinter
	//@Bean
	//public MemberPrinter memberPrinter() {
	//	return new MemberPrinter();
	//}
	
	@Bean
	@Qualifier("printer")
	public MemberPrinter memberPrinter1() {
		return new MemberPrinter();
	}
	
	//@Bean
	//public MemberPrinter memberPrinter2() {
	//	return new MemberPrinter();
	//}
	

	@Bean
	@Qualifier("summaryPrinter")
	public MemberSummaryPrinter memberPrinter2() {
		return new MemberSummaryPrinter();
	}

	@Bean
	public MemberListPrinter listPrinter() {
		return new MemberListPrinter();
	}
	
	@Bean
	public MemberInfoPrinter infoPrinter() {
		MemberInfoPrinter infoPrinter = new MemberInfoPrinter();
		//위의 코드가 실행될때
		//1) MemberInfoPrinter클래스의 인스턴스가 만들어지고
		//2) Autowired 어토테이션이 set메서드에 적용되어있으면, 자동으로 set메서드 동작.
		//infoPrinter.setPrinter(memberPrinter2());
		return infoPrinter;
	}
	
	@Bean
	public VersionPrinter versionPrinter() {
		VersionPrinter versionPrinter = new VersionPrinter();
		versionPrinter.setMajorVersion(5);
		versionPrinter.setMinorVersion(0);
		return versionPrinter;
	}
}
```


+ chap04/MemberRegisterService.java
```java
package spring;

import java.time.LocalDateTime;

import org.springframework.beans.factory.annotation.Autowired;

public class MemberRegisterService {
	
	//필드에 적용한 의존자동주입 -- 식별자가 아니라 자료형이 같은 객체를 검색하여 자동연결.
	@Autowired
	private MemberDao memberDao;

	public MemberRegisterService() {
		System.out.println("MemberRegisterService::MemberRegisterService()...");
	}
	
	//아래의 메서드는 작동되지 않는다. -- 필드에 의한 자동주입.
	public MemberRegisterService(MemberDao memberDao) {
		System.out.println("MemberRegisterService::MemberRegisterService(MemberDao)...");
		this.memberDao = memberDao;
	}

	public Long regist(RegisterRequest req) {
		Member member = memberDao.selectByEmail(req.getEmail());
		if (member != null) {
			throw new DuplicateMemberException("dup email " + req.getEmail());
		}
		Member newMember = new Member(
				req.getEmail(), req.getPassword(), req.getName(), 
				LocalDateTime.now());
		memberDao.insert(newMember);
		return newMember.getId();
	}
}
```

+ chap04/MainForSpring.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppCtx;
import spring.ChangePasswordService;
import spring.DuplicateMemberException;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.VersionPrinter;
import spring.WrongIdPasswordException;

public class MainForSpring {

	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) throws IOException {
		ctx = new AnnotationConfigApplicationContext(AppCtx.class);
		
		BufferedReader reader = 
				new BufferedReader(new InputStreamReader(System.in));
		while (true) {
			System.out.println("명령어를 입력하세요:");
			String command = reader.readLine();
			if (command.equalsIgnoreCase("exit")) {
				System.out.println("종료합니다.");
				break;
			}
			if (command.startsWith("new ")) {
				processNewCommand(command.split(" "));
				continue;
			} else if (command.startsWith("change ")) {
				processChangeCommand(command.split(" "));
				continue;
			} else if (command.equals("list")) {
				processListCommand();
				continue;
			} else if (command.startsWith("info ")) {
				processInfoCommand(command.split(" "));
				continue;
			} else if (command.equals("version")) {
				processVersionCommand();
				continue;
			}
			printHelp();
		}
	}

	private static void processNewCommand(String[] arg) {
		if (arg.length != 5) {
			printHelp();
			return;
		}
		MemberRegisterService regSvc = 
				ctx.getBean("memberRegSvc", MemberRegisterService.class);
		RegisterRequest req = new RegisterRequest();
		req.setEmail(arg[1]);
		req.setName(arg[2]);
		req.setPassword(arg[3]);
		req.setConfirmPassword(arg[4]);
		
		if (!req.isPasswordEqualToConfirmPassword()) {
			System.out.println("암호와 확인이 일치하지 않습니다.\n");
			return;
		}
		try {
			regSvc.regist(req);
			System.out.println("등록했습니다.\n");
		} catch (DuplicateMemberException e) {
			System.out.println("이미 존재하는 이메일입니다.\n");
		}
	}

	private static void processChangeCommand(String[] arg) {
		if (arg.length != 4) {
			printHelp();
			return;
		}
		ChangePasswordService changePwdSvc =
				ctx.getBean("changePwdSvc", ChangePasswordService.class);
		try {
			changePwdSvc.changePassword(arg[1], arg[2], arg[3]);
			System.out.println("암호를 변경했습니다.\n");
		} catch (MemberNotFoundException e) {
			System.out.println("존재하지 않는 이메일입니다.\n");
		} catch (WrongIdPasswordException e) {
			System.out.println("이메일과 암호가 일치하지 않습니다.\n");
		}
	}

	private static void printHelp() {
		System.out.println();
		System.out.println("잘못된 명령입니다. 아래 명령어 사용법을 확인하세요.");
		System.out.println("명령어 사용법:");
		System.out.println("new 이메일 이름 암호 암호확인");
		System.out.println("change 이메일 현재비번 변경비번");
		System.out.println();
	}

	private static void processListCommand() {
		MemberListPrinter listPrinter = 
				ctx.getBean("listPrinter", MemberListPrinter.class);
		listPrinter.printAll();
	}

	private static void processInfoCommand(String[] arg) {
		if (arg.length != 2) {
			printHelp();
			return;
		}
		MemberInfoPrinter infoPrinter = 
				ctx.getBean("infoPrinter", MemberInfoPrinter.class);
		infoPrinter.printMemberInfo(arg[1]);
	}
	
	private static void processVersionCommand() {
		VersionPrinter versionPrinter = 
				ctx.getBean("versionPrinter", VersionPrinter.class);
		versionPrinter.print();
	}
}
```

+ chap04/MemberListPrinter.java
```java
package spring;

import java.util.Collection;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class MemberListPrinter {

	private MemberDao memberDao;
	private MemberPrinter printer;

	public MemberListPrinter() {
	}
	
	public MemberListPrinter(MemberDao memberDao, MemberPrinter printer) {
		this.memberDao = memberDao;
		this.printer = printer;
	}

	public void printAll() {
		Collection<Member> members = memberDao.selectAll();
		members.forEach(m -> printer.print(m));
	}

	@Autowired
	public void setMemberDao(MemberDao memberDao) {
		System.out.println("@Autowired MemberListPrinter::setMemberDao(MemberDao)..........................................");

		this.memberDao = memberDao;
	}
	
	@Autowired
	@Qualifier("printer")
	public void setMemberPrinter(MemberPrinter printer) {
		System.out.println("@Autowired MemberListPrinter::setMemberDao(MemberSummaryPrinter)..........................................");

		this.printer = printer;
	}
	
	@Autowired
	public void setMemberPrinter(MemberSummaryPrinter printer) {
		System.out.println("@Autowired MemberListPrinter::setMemberDao(MemberSummaryPrinter)..........................................");

		this.printer = printer;
	}	
}
```



#### 4. project 실습
#### 5. Summary / Close







-------------------------------------------------------------------------

### [2019-07-23]

#### 1. Review

#### 2. SpringFramework
#### 4. project 실습
#### 5. Summary / Close