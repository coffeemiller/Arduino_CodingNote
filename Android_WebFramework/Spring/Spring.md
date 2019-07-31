

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

#### 2. 컴퍼넌트스캔(ComponentScan)
1. 설정파일을 사용하면, 설정파일의 @Bean 어노테이션을 가진 메서드를 실행시켜 빈객체를 생성하고 의존주입(명시적의존주입, 자동의존주입)을 수행하여 빈객체를 ApplicationContext가 관리한다. ------> 기존방법.

2. 설정파일을 사용할때 @ComponentScan을 사용하여 명시되지 않은 빈객체도 생성하고(@Component), 설정파일에 @Bean으로 명시된 빈객체도 생성하여 관리한다.  ------> 오늘 배울 내용.



+ AppCtx.java
```java
package config;

import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

import spring.MemberPrinter;
import spring.MemberSummaryPrinter;
import spring.VersionPrinter;

@Configuration
@ComponentScan(basePackages = {"spring"}) //basePackages는  스캔대상 패키지목록을 지정한다. spring패키지와 그하위패지지에서 스캔
public class AppCtx {

	@Bean
	@Qualifier("printer")
	public MemberPrinter memberPrinter1() {
		return new MemberPrinter();
	}
	
	@Bean
	@Qualifier("summaryPrinter")
	public MemberSummaryPrinter memberPrinter2() {
		return new MemberSummaryPrinter();
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




+ MainForSpring.java
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
		System.out.println("------------------------------------------------------------------------");
		//확인
		String[] names = ctx.getBeanDefinitionNames();
		for(String name : names) {
			System.out.println(name);
		}
		System.out.println("------------------------------------------------------------------------");
		
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
				ctx.getBean(MemberRegisterService.class);
		//MemberRegisterService regSvc = 
		//		ctx.getBean(memberRegisterService,MemberRegisterService.class);
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
				ctx.getBean(ChangePasswordService.class);
		//ChangePasswordService changePwdSvc =
		//		ctx.getBean(changePasswordService,ChangePasswordService.class);		
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





+ MemberListPrinter.java
```java
package spring;

import java.util.Collection;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component("listPrinter")
public class MemberListPrinter {

	private MemberDao memberDao;
	private MemberPrinter printer;

	public MemberListPrinter() {
		System.out.println("@Component--MemberListPrinter:MemberListPrinter()...");
	}
	
	public MemberListPrinter(MemberDao memberDao, MemberPrinter printer) {
		System.out.println("@Component--MemberListPrinter:MemberListPrinter(MemberDao,MemberPrinter)...");		
		this.memberDao = memberDao;
		this.printer = printer;
	}

	public void printAll() {
		Collection<Member> members = memberDao.selectAll();
		members.forEach(m -> printer.print(m));
	}

	@Autowired
	public void setMemberDao(MemberDao memberDao) {
		this.memberDao = memberDao;
	}
	
	@Autowired
	public void setMemberPrinter(MemberSummaryPrinter printer) {
		this.printer = printer;
	}
}
```




```
설정파일에 @ComponentScan을 넣으면...
@Component를 기입한 모든 클래스를 자동주입하고 객체를 생성하여 관리를 시작한다.

@ComponentScan("이름")  -> 생성자명이 '이름'으로 관리된다.
```




##### 실습
```
0-1. ApplicationContext에 등록되는 빈객체 확인
   MainForList.java 실습
   
0-2. Bean객체를 직접 등록했을때 동일 id와 동일 클래스일때 1개만 동작한다.
   MainforManual.java 실습
```

1. Java설정화일에 Bean을 등록하지 않고 Java화일 자체에 @Component를 사용하여 스프링컨테이너가 검색하여 생성할수 있도록 설정할수 있다.
   + 단,Java설정화일에 반드시 @ComponentScan(basePackages = {"spring"})을 지정해야 한다.
   + MainForSpring.java실습
   


2. 스캔할때 특정 대상을 자동 등록 대상에서 제외할수 있다.
   + MainForExclude.jva실습
```   
  방법 1) 정규표현식을 사용해서 제외 대상을 지정
         "spring"로 시작하고 Dao로 끝나는 클래스 제외 -- spring.MemberDao제외
          
		@Configuration
		@ComponentScan(basePackages = {"spring"}, 
			excludeFilters = @Filter(type = FilterType.REGEX, pattern = "spring\\..*Dao")
		)	
   
  방법 2) ASPECTJ를 이용하여 제외(pom.xml의 dependency와 aaspectjweaver 모듈 추가) 
		@Configuration
		@ComponentScan(basePackages = {"spring"}, 
			excludeFilters = @Filter(type = FilterType.ASPECTJ, pattern = "spring.*Dao")			
		)
   
  방법 3) 특정 Annotaion을 붙인 타입을 컴퍼넌트 대상에서 제외
      @NoProduct @ManualBean어노테이션을 붙인 클래스는 컴퍼넌트 스캔 대상에서 제외
      ----------------------사용자가 만든 어노테이션

    @Configuration
    @ComponentScan(basePackages = {"spring","spring2"}, 
       excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = {NoProduct.class, ManualBean.class})			
    )
    
 	방법4) 특정 타입이나 그하위 타입을 컴퍼넌트 대상에서 제외 - ASSIGNABLE_TPPE    
    @Configuration
    @ComponentScan(basePackages = {"spring"}, 
      excludeFilters = @Filter(type = FilterType.ASSIGNABLE_TYPE, classes = MemberDao.class)
    )
     
 	방법5) 설정필터가 2개 이상이면 다음과 같이 사용할 수 있다.
    @Configuration
    @ComponentScan(basePackages = {"spring"}, 
    excludeFilters = {
		 @Filter(type = FilterType.ANNOTATION, classes = ManualBean.class),
		 @Filter(type = FilterType.REGEX,pattern = "spring2\\..*")}
    )      
```      




+ AppCtxWithExclude.java
```java
//정규표현식으로 제외
@Configuration
@ComponentScan(basePackages = {"spring"}, 
  excludeFilters = @Filter(type = FilterType.REGEX, pattern = "spring\\..*Dao")
																														//"클래스이름\\..*문자"
)			

//ASPECTJ를 이용한 제외
@Configuration
@ComponentScan(basePackages = {"spring"}, 
excludeFilters = @Filter(type = FilterType.ASPECTJ, pattern = "spring.*Dao")			
																														//"클래스이름.*문자"
)

//Annotaion(사용자가 만든 어노테이션)을 붙인 대상에 관한 제외 -- NoProduct, ManualBean
@Configuration
@ComponentScan(basePackages = {"spring","spring2"}, 
excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = {NoProduct.class, ManualBean.class})		              
																														// 어노테이션이름.class
)

//특정타입 or 그 하위에서 제외 -- ASSIGNABLE_TYPE
@Configuration
@ComponentScan(basePackages = {"spring"}, 
excludeFilters = @Filter(type = FilterType.ASSIGNABLE_TYPE, classes = MemberDao.class)			
)

//설정필터가 2개 이상일때...
@Configuration
@ComponentScan(basePackages = {"spring"}, 
 excludeFilters = {
		 @Filter(type = FilterType.ANNOTATION, classes = ManualBean.class),
		 @Filter(type = FilterType.REGEX,pattern = "spring2\\..*")}
)

public class AppCtxWithExclude {
	//아래 코드는 @ComponentScan에 의해서 자동등록되는 빈이아니라 사용자가 직접등록한 빈.
	@Bean
	public MemberDao memberDao() {
		return new MemberDao();
	}
	
	@Bean
	@Qualifier("printer")
	public MemberPrinter memberPrinter1() {
		return new MemberPrinter();
	}

	@Bean
	@Qualifier("summaryPrinter")
	public MemberSummaryPrinter memberPrinter2() {
		return new MemberSummaryPrinter();
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



3. @ComponentScan은 @Component 어노테이션 뿐만아니라 다음의 어노테이션도 컴퍼넌트 스캔 대상에 포함된다.
```
   @Controller
   @Service
   @Repository
   @Aspect
   @Configuration
```




##### <주의할 점>
4. 컴퍼넌트 스캔기능을 사용해서 자동으로 빈을 등록할대 충돌에 주의 한다.
   - 빈 이름 충돌
     - @Component에서 자동등록된 빈객체의 식별자명과 설정파일에서 등록한 빈객체의 명칭충돌.
     - 패키지가 여러개일때 주로 발생.
   
	 - 수동 등록에 따른 충돌
  	 - 자동등록된 빈객체와 설정파일에서 등록한 빈객체가 함께 존재할때.
   
	 - MainForExplicait.java 실습


+ AppCtxWithExplicit.java
```java
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

import spring.MemberDao;
import spring.MemberPrinter;
import spring.MemberSummaryPrinter;
import spring.VersionPrinter;

@Configuration
@ComponentScan(basePackages = {"spring"})
//@ComponentScan(basePackages = {"spring","spring2"})   //--> 빈충돌
//exception is org.springframework.context.annotation.ConflictingBeanDefinitionException

public class AppCtxWithExplicit {
	
	//자동등록과 수동등록이 함께 지정되었을때는.... 수동등록이 우선(적용)
	@Bean
	public MemberDao memberDao() {
		MemberDao memberDao = new MemberDao();
		System.out.println("explicit : " + memberDao);
		return memberDao;
	}
	
	//수동으로 같은 객체 2개를 지정하면.... 수동으로 2개의 객체가 생성
	// memberDao, memberDao2
	/*
	@Bean
	public MemberDao memberDao2() {
		MemberDao memberDao = new MemberDao();
		System.out.println("explicit : " + memberDao);
		return memberDao;
	}
	*/
	
	@Bean
	@Qualifier("printer")
	public MemberPrinter memberPrinter1() {
		return new MemberPrinter();
	}
	
	@Bean
	@Qualifier("summaryPrinter")
	public MemberSummaryPrinter memberPrinter2() {
		return new MemberSummaryPrinter();
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



+ MainForExplicit.java
```java
import java.io.IOException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.AbstractApplicationContext;
import config.AppCtxWithExplicit;

public class MainForExplicit {

	private static AbstractApplicationContext ctx = null;
	
	public static void main(String[] args) throws IOException {
		ctx = new AnnotationConfigApplicationContext(
				AppCtxWithExplicit.class);
		ctx.close();
	}
}
```



#### 4. project 실습
#### 5. Summary / Close



-------------------------------------------------------------------------

### [2019-07-29]

#### 1. Review
+ Spring
	1. Library
	2. framework
   	1. 표준화된 설계기법 - 공통의 사용법(표준) / 유지보수용이
   	2. 어느 단계에서 생성 -> 사용 -> 소멸
      	- 동작구조 / 작동순서  ---> (template-틀)


+ 의존주입
  1. 명시적 의존주입
     - 생성자에 의한 의존주입
     - set 메서드에 의한 의존주입
  
	```java
	@Bean
	public MemberRegisterService memberRegSvc() {
		//명시적 의존주입
		return new MemberRegisterService(memberDao());
	}
	
	@Bean
	public ChangePasswordService changePwdSvc() {
		ChangePasswordService pwdSvc = new ChangePasswordService();
		//set메서드에 의한 의존주입
		pwdSvc.setMemberDao(memberDao());
		return pwdSvc;
	}
	```
	


	2. 자동 의존주입
     - @Autowired =====> [@ComponentScan /  @Configuration]




#### 2. Bean's Lifecycle
+ 일반 빈 객체의 생명주기
```
1. 생성-->생성자-->의존주입(자바설정파일)
AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtx.class);

@Configuration
@Bean     -------| 빈객체생성-식별자(@Qualifier)
@ComponentScan --| 명시적의존주입, 자동의존주입(@Autowired)
==============================================================

2. 사용
클래스명 객체명 = ctx.getBean("식별자명", Client.class);
								= ctx.getBean(Client.class);
객체명.메서드();
==============================================================

3. 메모리에서 소멸
ctx.close();



InitializingBean, DisposableBean 인터페이스를 implements한 빈클래스 객체의 생명주기
1. 생성 --> 생성자 -->의존주의 ==> afterPropertiesSet():초기화 기능
   메모리를 할당
           초기화작업==>생성자의 역활이 미비하다.  그래서 기능보완을 위해서 별도의 메서드를 만들었다.
                  의존주입관련 기능(필드,인자가있는생성자,set메서드)
2. 사용

3. 메모리에서 소멸되기 직전에 ==> destroy() : 마무리 기능
4. 메모리에서 소멸


빈객체의 scope가 ==> singleton일때


==================================================================
위의 방법을 사용하면 초기화메서드와 종료메서드의 명칭이 모두 같다.
상황에 따라 초기화메서드, 종료메서드를 지정해서 사용할 수 도 있다.
1) xml설정화일 ==> GenericXmlApplicationContext
   <bean ...  init-method="초기화메서드명" destroy-method="마무리메서드명">
   </bean> 
2) Java설정화일 ==> AnnotationConfigApplicationContext
   @Bean(initMethod="connect", destroyMethod="close")   

   
   -----------------------------------------------
   //빈객체의 scope ==> 객체가 메모리에 존재하는 기간
                      1) singleton : Context초기화시 생성되고 close()시 소멸 ==> 1개만 객체가 만들어 진다.
```

+ chap06/AppCtx.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import spring.Client;
import spring.Client2;

@Configuration
public class AppCtx {

	@Bean	
	public Client client() {
		//한번만 객체가 생성된다 --- getBean(Client.class);
		Client client = new Client();
		client.setHost("host");
		return client;
	}
	
	//빈객체는 주로 인자가 없는 생성자에 의해 만들어진다.
	//그러다보니 생성자에서 수행할 준비기능을 작동시키기가 불편하다.
	//그래서 초기화 기능을 전담하는 메서드를 별도로 지정할 수 있다.
	
	@Bean(initMethod = "connect", destroyMethod = "close")
	public Client2 client2() {
		//한번만 객체가 생성된다 --- getBean(Client2.class);
		Client2 client = new Client2();
		client.setHost("host");
		return client;
	}
}
```


+ chap06/Main.java
```java
package main;

import java.io.IOException;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.AbstractApplicationContext;

import config.AppCtx;
import spring.Client;

public class Main {

	public static void main(String[] args) throws IOException {
		AbstractApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtx.class);
		System.out.println("ApplicationContext초기화------------------------------");
		
		Client client = ctx.getBean(Client.class);
		client.send();
		
		System.out.println("ApplicationContext소멸------------------------------");
		ctx.close();
	}
}
```


+ chap06/Client.java
```java
package spring;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class Client implements InitializingBean, DisposableBean {

	private String host;
	
	public Client() {
		System.out.println("Client::Client()....");
	}
	
	//생성된 직후에 자동으로 작동하는 메서드
	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("생성된 직후에 작동하는 메서드 Client::afterPropertiesSet() 실행");
	}
	//-------------------------------------
	public void setHost(String host) {
		this.host = host;
	}



	public void send() {
		System.out.println("Client.send() to " + host);
	}
	//-------------------------------------
	
	//소멸되기 직전에 작동하는 메서드
	@Override
	public void destroy() throws Exception {
		System.out.println("소멸 직전에 작동하는 메서드 Client::destroy() 실행");
	}
}
```



+ chap06/Client2.java
```java
package spring;

public class Client2 {

	private String host;

	public Client2() {
		System.out.println("Client2:Client2()....");
	}
	
	//설정화일지정 생성직후 작동하는 메서드
	public void connect() {
		System.out.println("생성된 직후에 작동하는 메서드 지정 Client2::connect() 실행");
	}
	
	public void setHost(String host) {
		this.host = host;
	}


	public void send() {
		System.out.println("Client2.send() to " + host);
	}

	//설정화일지정 소멸직전 작동하는 메서드
	public void close() {
		System.out.println("소멸 직전에 작동하는 메서드 지정 Client2::close() 실행");
	}
}
```




+ chap06/MainWithPrototype.java
```java
package main;

import java.io.IOException;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.AbstractApplicationContext;

import config.AppCtxWithPrototype;
import spring.Client;
import spring.Client2;

public class MainWithPrototype {

	public static void main(String[] args) throws IOException {
		AbstractApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtxWithPrototype.class);
		System.out.println("ApplicationContext초기화------------------------------");

		Client client1 = ctx.getBean(Client.class);
		Client client2 = ctx.getBean(Client.class);
		System.out.println("client1 == client2 : " + (client1 == client2));
		
		Client2 client21 = ctx.getBean(Client2.class);
		Client2 client22 = ctx.getBean(Client2.class);
		System.out.println("client21 == client22 : " + (client21 == client22));
		System.out.println("ApplicationContext소멸------------------------------");
		ctx.close();
	}
}
```

+ chap06/AppCtxWithPrototype.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

import spring.Client;
import spring.Client2;

@Configuration
public class AppCtxWithPrototype {

	@Bean
	@Scope("prototype")
	public Client client() {
		//ApplicationContext가 작동할 아래의 코드는 동작하지 않는다.
		//사용자가 getBean()을 사용할때마다 작동한다.
		//그리고 매번 새로운 객체를 생성하여 리턴한다.
		Client client = new Client();
		client.setHost("host");
		return client;
	}
	
	@Bean(initMethod = "connect", destroyMethod = "close")
	@Scope("singleton")
	public Client2 client2() {
		//ApplicationContext가 작동할 아래의 코드는 단 1번만 작동한다.
		//이후 사용자가 getBean()을 사용할때마다 작동하지 않는다.
		//그리고 기존의 객체를 검색해서 리턴한다.		
		Client2 client = new Client2();
		client.setHost("host");
		return client;
	}
}
```


+ MainWithPrototype.java
```java
package main;

import java.io.IOException;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.AbstractApplicationContext;

import config.AppCtxWithPrototype;
import spring.Client;
import spring.Client2;

public class MainWithPrototype {

	public static void main(String[] args) throws IOException {
		AbstractApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtxWithPrototype.class);
		System.out.println("ApplicationContext초기화------------------------------");

		Client client1 = ctx.getBean(Client.class);
		Client client2 = ctx.getBean(Client.class);
		System.out.println("client1 == client2 : " + (client1 == client2));
		
		Client2 client21 = ctx.getBean(Client2.class);
		Client2 client22 = ctx.getBean(Client2.class);
		System.out.println("client21 == client22 : " + (client21 == client22));
		System.out.println("ApplicationContext소멸------------------------------");
		ctx.close();
	}
}
```



+ 결과
```Client2:Client2()....
생성된 직후에 작동하는 메서드 지정 Client2::connect() 실행
ApplicationContext초기화------------------------------
Client::Client()....
생성된 직후에 작동하는 메서드 Client::afterPropertiesSet() 실행
Client::Client()....
생성된 직후에 작동하는 메서드 Client::afterPropertiesSet() 실행
client1 == client2 : false
client21 == client22 : true
ApplicationContext소멸------------------------------
.....
소멸 직전에 작동하는 메서드 지정 Client2::close() 실행
```




#### 3. AoP (Aspect Oriented Programming)
+ A, B, C가 가지는 공통기능 = Aspect

+ Calculator.java
```java
package chap07;

public interface Calculator {

	public long factorial(long num);

}
```


+ ImpeCalculator.java
```java
package chap07;

public class ImpeCalculator implements Calculator {

	public ImpeCalculator() {
		System.out.println("ImpeCalculator::ImpeCalculator()...");
	}

	@Override
	public long factorial(long num) {
		//반복문으로 팩토리얼 계산
		//System.out.println("ImpeCalculator::factorial(long)..." + num);
		//long start = System.currentTimeMillis();
		
		long result = 1;
		for (long i = 1; i <= num; i++) {
			result *= i;
		}
		
		//long end = System.currentTimeMillis();
		//System.out.printf("ImpeCalculator.factorial(%d) 실행시간 = %d\n", num,(end-start));
		
		return result;
	}
}
```

+ RecCalculator.java
```java
package chap07;

public class RecCalculator implements Calculator {
	
	public RecCalculator() {
		System.out.println("RecCalculator::RecCalculator()...");
	}

	@Override
	public long factorial(long num) {
		//재귀호출에 의한 팩토리얼 계산
		System.out.println("RecCalculator::factorial(long)..." + num);
		//long start = System.currentTimeMillis();
		
		try {
	        if (num == 0)
	            return 1;
	        else
	            return num * factorial(num - 1);
		}finally{
			//long end = System.currentTimeMillis();
			//System.out.printf("RecCalculator.factorial(%d) 실행시간 = %d\n", num,(end-start));			
		}
	}
}
```

+ MainJava.java
```java
package main;

import chap07.ImpeCalculator;
import chap07.RecCalculator;
import chap07.ExeTimeCalculator;

public class MainJava {

	public static void main(String[] args) {
		ImpeCalculator impeCalculator = new ImpeCalculator();		
		System.out.println(impeCalculator.factorial(20));
		System.out.println("----------------------------------------------");
		
		RecCalculator recCalculator =new RecCalculator();
		System.out.println(recCalculator.factorial(20));
	}
}
```



##### 3.1. 개념
##### 3.2. 사용법
##### 3.3. 실습

+ AOP의 개념을 이해하기위해 다음의 순서로 실습해보자

1. MainJava.java
```
   ImpeCalculator -- 반복문
   RecCalculator  -- 재귀호출 사용
     두가지 factorial을 구하는 클래스 사용 -- 시간측정(클래스 내부에 표현)
```

2. MainJava2.java 
```
   ImpeCalculator -- 반복문
   RecCalculator  -- 재귀호출 사용
     두가지 factorial을 구하는 클래스 사용 -- 시간측정(외부에 별도 표현)
     
==> 좀더 자세한 시간측정이 필요하다면 소스코드가 중복되어 변경이 번거롭다. 
    이를 해결하는 일반적인 방법이 프록시(Proxy) 객체를 사용하는 것이다.

		시간측정기능을 별도의 클래스로 작성하는 것이다.
		그리고 기존의 프로그램에서는 시간측정은 자신이 수행하지 않고,
		다른 클래스에 위임시키는 delegate패턴을 일반적으로 많이 사용한다.
    
    프록시(Proxy) 객체 : 핵심기능의 실행은 다른 객체에 위임하고 부가적인 기능을 제공하는 객체
                    --------------------------  --------------------(공통기능구현객체)
                                     대상객체  
```



+ MainJava2.java
```java
package main;

import chap07.ImpeCalculator;
import chap07.RecCalculator;
import chap07.ExeTimeCalculator;

public class MainJava2 {

	public static void main(String[] args) {
		ImpeCalculator impeCalculator = new ImpeCalculator();	
		long start1 = System.currentTimeMillis();
		System.out.println(impeCalculator.factorial(20));
		long end1 = System.currentTimeMillis();
		System.out.printf("ImpeCalculator.factorial(20) 실행시간 = %d\n", (end1-start1));
		
		System.out.println("----------------------------------------------");
		
		RecCalculator recCalculator =new RecCalculator();
		long start2 = System.currentTimeMillis();
		System.out.println(recCalculator.factorial(20));
		long end2 = System.currentTimeMillis();
		System.out.printf("RecCalculator.factorial(20) 실행시간 = %d\n", (end2-start2));	
	}
}
```










3. MainProxy.java             
```
   ExeTimeCalculator클래스 -- delegate패턴 적용
     실제 factorial을 계산하는 클래스를 인자로 전달받아 사용
   
   ExeTimeCalculator객체 ==> Proxy 객체  
   ImpeCalculator객체 --| 대상객체
   RecCalculator 객체 --| 
   
================================================================
```

+ MainProxy.java
```java
package main;

import chap07.ImpeCalculator;
import chap07.RecCalculator;
import chap07.ExeTimeCalculator;

public class MainProxy {

	public static void main(String[] args) {
		ExeTimeCalculator ttCal1 = new ExeTimeCalculator(new ImpeCalculator());
		System.out.println(ttCal1.factorial(20));
		System.out.println("----------------------------------------------");
		
		ExeTimeCalculator ttCal2 = new ExeTimeCalculator(new RecCalculator());
		System.out.println(ttCal2.factorial(20));

		//------------ExeTimeCalculator 객체
		//					시간계산시작
		//                 delegate ------------> ImpeCalculator객체
		//                                             factorial() 계산 메서드
		// 					시간계산끝
		//
		//            ExeTimeCalculator 객체
		//      			delegate ------------> RecCalculator객체
		//                                             factorial() 계산 메서드
	}
}
```



+ ExeTimeCaluator.java
```java
package chap07;

public class ExeTimeCalculator implements Calculator {

	private Calculator delegate;

	public ExeTimeCalculator(Calculator delegate) {
		System.out.println("ExeTimeCalculator::ExeTimeCalculator(Calculator)...");
        this.delegate = delegate;
    }

	@Override
	public long factorial(long num) {
		//공통기능 - 시간 측정
		long start = System.nanoTime();
		long result = delegate.factorial(num);  // 핵심기능
		long end = System.nanoTime();
		System.out.printf("%s.factorial(%d) 실행 시간 = %d\n",
				delegate.getClass().getSimpleName(),
				num, (end - start));
		return result;
	}
}
```









1. AOP(Aspect Oriented Programming) : 관점 지향 프로그래밍
```
    여러 객체에 공통으로 적용할 수 있는 기능을 분리해서 재사용성을 높여주는 프로그래밍 기법
    핵심기능과 공통기능의 구현을 분리함으로써 핵심 기능을 구현한 코드의 수정없이 공통기능을 적용할수 있게 만들어 준다.   
  
 *** 스프링에서의 AOP적용 ***
  공통기능을 제공하는 Aspect 구현 클래스를 만들고 자바설정화일을 이용해서 Aspect를
  어디에 적용할지를 설정한다. 프록시는 springframework가 알아서 해준다.
  
 1) 공통기능 (Aspect)만들기 : 시간측정
 2) Java설정화일에서
       
       어떤 핵심기능에 어떤 공통기능을 언제 적용할것인지 설정 : Advice를 설정
    ======================> 자바 POJO클래스  	                                    	
	       				    @Aspect 어노테이션 사용 
    - @EnableAspectJAutoProxy을 사용하여 @Aspect 어노테이션을 붙인 클래스를 공통기능으로 적용하도록 설정
    - 공통기능 빈객체 생성설정
 3) 공통기능 클래스에서
    - @Aspect적용
    - 공통기능을 적용할 대상을 설정한다==>chap07패키지와 그 하위 패키지에 위치한 타입의 public메서드를 pointcut으로 설정.
	@Pointcut("execution(public * chap07..*(..))")
	private void publicTarget() {
		System.out.println("@Pointcut -- ExeTimeAspect::publicTarget()...");
	}

	- Around Advice 즉, publicTarget()에 적용된 대상의 메서드가 호출전/후에 아래의 공통기능이 적용된다. 
	 @Around("publicTarget()")  
	 public Object measure(ProceedingJoinPoint joinPoint) throws Throwable {
		...
		Object result = joinPoint.proceed(); //핵심기능 호출
		...
     }
     
     - Pointcut지정 방식
     @Pointcut("execution(public * chap07..*(..))")
     @Pointcut("execution(수식어패턴? 리턴타입패턴 클래스이름패턴?메서드이름패턴(파라미터패턴)")
                          -------- --------- ---------- ----------(---------)
                          public       *     chap07..        *        ..  
                          
       ● 수식어 패턴
          - 생략가능한 부분.
          - public, protected 등이 옴.
        ● 리턴타입패턴
          - 리턴 타입을 명시
        ● 클래스이름 패턴, 이름패턴
          - 클래스 이름 및 메서드 이름을 패턴으로 명시.
        ● 파라미터패턴
          - 매칭될 파라미터에 대해서 명시.
      
      ○ 특징
        - 각 패턴은 '*'를 이용하여 모든 값을 표현.
        - '..'을 이용하여 0개 이상이라는 의미를 표현.
      
      ○ 설정 예
        ● execution(public void set*(..))
          - 리턴 타입이 void이고 메서드 이름이 set으로 시작하고, 파라미터가 0개 이상인 메서드 호출.
        ● execution(* com.jica.chap07.*.*())
          - com.jica.chap07 패키지의 모든 클래스의 파라미터가 없는 모든 메서드 호출.
        ● execution(* com.jica.chap07..*.*(..))
          - com.jica.chap07 패키지 및 하위 패키지에 있는 모든 클래스의 파라미터가 0개 이상인 메서드 호출.
        ● execution(Integer com.jica.chap07.WriteArticleService.write(..))
          - 리턴 타입이 Integer인 com.jica.chap07패키지의 WriteArticleService 인터페이스의 write() 메서드 호출.
        ● execution(* get*(*))
          - 이름이 get으로 시작하고 1개의 파라미터를 갖는 메서드 호출.
        ● execution(* get*(*, *))
          - 이름이 get으로 시작하고 2개의 파라미터를 갖는 메서드 호출.
        ● execution(* read*(Integer, ..))
          - 메서드 이름이 read로 시작하고, 첫 번째 파라미터 타입이 Integer이며, 1개 이상의 파라미터를 갖는 메서드 호출.

   실습 : MainAspect.java
       MainAspectWithClassProxy.java
       
```


+ MainAspect.java
```java
package main;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import chap07.Calculator;
import chap07.RecCalculator;
import config.AppCtx;

public class MainAspect {
	
	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtx.class);
		System.out.println("ApplicationContext초기화 끝-------------------------------------------------------------------");
		
		String[] names = ctx.getBeanDefinitionNames();
		for(String name : names) {
			System.out.println(name);
		}
		
		System.out.println("ApplicationContext사용----------------------------------------------------------------------");
		
		Calculator cal = ctx.getBean("calculator", Calculator.class);
		//RecCalculator cal = ctx.getBean("calculator", RecCalculator.class);
		//오류발생 : 이유) 내부으로 Calculator를 구현한 Proxy17클래스형 객체가 만들어 지므로 형변환을 할 수 없다.
		//이를 해결한 코드 MainAspectWithClassproxy.java
		
		long fiveFact = cal.factorial(5);
		
		System.out.println("cal.factorial(5) = " + fiveFact);
		System.out.println(cal.getClass().getName());
		
		System.out.println("ApplicationContext사용끝----------------------------------------------------------------------");
				
		ctx.close();
	}
}
```


+ AppCtx.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

import aspect.ExeTimeAspect;
import chap07.Calculator;
import chap07.RecCalculator;

@Configuration
@EnableAspectJAutoProxy  //@Aspect 어노테이션을 붙인 클래스를 공통기능으로 적용하시오
public class AppCtx {
	//공통기능 객체를 빈으로 생성
	@Bean
	public ExeTimeAspect exeTimeAspect() {
		return new ExeTimeAspect();
	}

	@Bean
	public Calculator calculator() {
		return new RecCalculator();
	}
}
```





+ ExeTimeAspect.java
```java
package aspect;

import java.util.Arrays;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.core.annotation.Order;

@Aspect
//@Order(1)
public class ExeTimeAspect {
	public ExeTimeAspect() {
		System.out.println("@Aspect -- ExeTimeAspect::ExeTimeAspect()....");
	}
	
	//공통기능을 적용할 대상을 설정한다==>chap07패키지와 그 하위 패키지에 위치한 타입의 public메서드를 pointcut으로 설정.
	@Pointcut("execution(public * chap07..*(..))")
	private void publicTarget() {
		System.out.println("@Pointcut -- ExeTimeAspect::publicTarget()...");
	}

	//Around Advice 즉, publicTarget()에 적용된 대상의 메서드가 호출전/후에 아래의 공통기능이 적용된다. 
	@Around("publicTarget()")
	public Object measure(ProceedingJoinPoint joinPoint) throws Throwable {
		System.out.println("@Pointcut -- ExeTimeAspect::measure(ProceedingJointPoint)...");
		long start = System.nanoTime();
		try {
			Object result = joinPoint.proceed(); //핵심기능 호출
			return result;
		} finally {
			long finish = System.nanoTime();
			Signature sig = joinPoint.getSignature();
			System.out.printf("%s.%s(%s) 실행 시간 : %d ns\n",
					joinPoint.getTarget().getClass().getSimpleName(),
					sig.getName(), Arrays.toString(joinPoint.getArgs()),
					(finish - start));
		}
	}
}
```







5. 한 Pointcut에 여러 Advice 적용
```
     실습 : MainAspectWithCache.java
        MainAspectWithCache.java -- @Order()적용하여 실습
```

6. @Pointcut 어노테이션이 아닌 @Around 어노테이션에 execution명시자 직접 지정
```
     실습 : MainAspectCommonPointcut.java
```

#### 4. Summary / Close




-------------------------------------------------------------------------

### [2019-07-30]

#### 1. Review
+ interface를 만드는 것은 해당 인터페이스를 구현한 모든 클래스들은 인터페이스의 추상메서드를 반드시 재정의 해야한다.
	- 즉, 사용법을 표준화시키는 효과가 있다.
	- 상위클래스로 하위형 객체를 가리킬수 있다.

#### 2. AOP(Aspect Oriented Programming) : 관점 지향 프로그래밍

+ AOP:Aspect Oriented Programming(관점지향프로그래밍)
```
   코딩의 효율을 중시 -- 코드의 중복발생(원인:여러곳에서 동일한 기능이 사용)
                                     ----------------------
                                                        공통기능
            핵심기능 : 자신의 기능만 집중하고 필요하다면 공통기능을 호출하여 사용
         =======================================================
         spring framework에서는 설정화일을 이용하여 핵심기능이 필요한 공통기능을 설정해주면
            공통기능이 동작하는 과정중에 핵심이 동작하도록하는 방식을 취한다.
         =======================================================                        
```

 - 개선방법:기존)
```
    공통기능(Aspect)과 핵심기능을 분리해서 작성하여 sw의 생산성과 유지보수성을 증대
   ---------------------------------> 공통기능을 작성하고 핵심기능은 다른곳으로 위임시키는 방법으로
    코드를 개선하여 사용하고 있다.
```    
 - spring에서의 방법  
```
     이것을 spring framework에서는 Proxy를 사용한 AOP로 지원하고 있다. 
 
    factorial계산 ---> 핵심기능
       시간측정          ---> 공통기능(Aspect)
 
   1) 공통기능과 핵심기능을 한꺼번에 작성했을때의 예
   public class ImpeCalculator implements Calculator {

	@Override
	public long factorial(long num) {

		long start = System.currentTimeMillis();

		long result = 1;
		for (long i = 1; i <= num; i++) {
			result *= i;
		}

		long end = System.currentTimeMillis();
		System.out.printf("ImpeCalculator.factorial(%d) 실행시간 = %d\n", num, (end-start)); 
		
		return result;
	}
  }   
  
  2) 공통기능과 핵심기능을 분리의 예 
  //핵심기능
  public class ImpeCalculator implements Calculator {

	@Override
	public long factorial(long num) {

		long result = 1;
		for (long i = 1; i <= num; i++) {
			result *= i;
		}
		return result;
	}
  }
  
  //공통기능
	ImpeCalculator impeCalculator = new ImpeCalculator();
	long start1 = System.nanoTime();
	//핵심기능
	long result1 = impeCalculator.factorial(20);
	
	long end1 = System.nanoTime();
	System.out.printf("ImpeCalculator.factorial(%d) 실행시간 = %d\n", 20, (end1-start1)); 

	System.out.println("결과1 : " + result1); 
	
	============>문제점은 다른 핵심기능에서도 공통기능이 필요하다면
	공통기능은 중복해서 작성할 수밖에 없다. 
	
	이를 개선시키기 위해서 Proxy개념을 도입했다.
	Proxy는 핵심기능을 delegate로 별도로 가지고 있고
	공통기능 만을 구현해서 가진다.
	
	핵심기능의 실행은 다른 객체에 위임하고 부가적인 기능을 제공하는 객체를 프록시(Proxy)라고 부르고,
	실제 핵심 기능을 실행하는 객체를 대상객체라고 부른다.
	여기에서는 ExeTimeCalculator객체가 프록시 객체이고 ==> 공통기능
	ImpeCalculator,RecCalculator이 대상객체이다    ==>  핵심기능
	 
	3) Proxy클래스의 예
	public class ExeTimeCalculator implements Calculator {
		//핵심기능을 멤버변수로 가지고 있어서 생성자에서 전달받는다.
		private Calculator delegate;

		public ExeTimeCalculator(Calculator delegate) {
			this.delegate = delegate;
		}
		
		//공통기능
		@Override
		public long factorial(long num) {

			long start = System.nanoTime();
			
			//핵심기능을 수행할때 다른 클래스에 기능을 위임시킨다.
			long result = delegate.factorial(num);
			
			//공통기능
			long end = System.nanoTime();
			System.out.printf("%s.factorial(%d) 실행 시간 = %d\n",
					delegate.getClass().getSimpleName(),
					num, (end - start));
			return result;
		}
	}
	
	위의 기능이 동작할때는
	기능요청  프록시객체
	----->       ----->공통기능호출1 
	             ------------------->핵심기능호출
	             ----->공통기능호출2   
	<-----
	      =======================
	      springframework에서는 프록시객체를 내부적으로 자동생성하여 지원해주므로
	      우리들은 공통기능(Aspect 객체)과 핵심기능(factorial계산 : 대상(Target)객체)만을 작성해서
	      어떤핵심기능이 동작할때 공통기능도 동작해야 한다는것을 설정해 주기만 하면 된다.(xml설정화일, Annotation)
	                                   -----------이때 사용되어지는 전문 용어가 있다.
	      interface TestAspect    공통기능 interface        핵심기능
	                
	      class TimeAspect implements TestAspect         class MyImportent                                  
	                  공통기능메서드(){                                 핵심기능메서드(){
	                     ....                                 ...
	          }                                             }
	                   
	                     공통기능과 핵심기능을 설정(연결) <=== Annotain : @Aspect 
	      
	      springframework에서 핵심기능이 동작할때 공통기능을 작동시켜주기위해
	          실행시간에 프록시 객체를 생성한다. 이때 프록시 객체는 핵심기능의 interface를 구현한 클래스에 의해서 만들어진다.
	          	      
	         
        1) 공통기능 : Aspect
        2) 공통기능이 핵심기능에 언제 적용될지 정의 : Advice(메서드호출,필드값변경 등)                     
        3) Advide의 적용시점 : JoinPoint(메서드 호출- 스프링에서는 이것만 가능)
             -Before Advice			  : 핵심 메서드 호출전에 공통기능 적용
             -After  Advice			  : 핵심 메서드 호출후 예외발생혹은 정상종료시 공통기능 모두 적용 
             -After Returning Advice  : 핵심 메서드 호출호 정상종료시만 공통기능 적용
             -Atfer Throwing Advice	  : 핵심 메서드 호출후 예외발생시만 공통기능 적용 
             -Around Advice 		  : 핵심 메서드 호출전/후나 공통기능 모두 적용	
             
	                                         
 ---------------------------------------------------------------------
 스프링에서의 AOP적용
 1) 공통기능 (Aspect)만들기 : 시간측정
 2) 어떤 핵심기능에 어떤 공통기능을 언제 적용할것인지 설정 : Advice를 설정
    ======================> 자바 POJO클래스  	                                    
	       					@Aspect 어노테이션 사용

  0. □ XML 스키마를 이용한 AOP 설정
    - Spring 2 버전부터 AOP 설정과 관련된 "aop" 네임스페이스 및 "aop" 네임스페이스와 관련된 XML 스키마가 추가되었다.
    ■ <beans> 태그에 "aop" 네임스페이스와 관련된 XML 스키마 지정
	 <beans xmlns="http://www.springframework.org/schema/beans"
	     ...
	     xmlns:aop="http://www.springframework.org/schema/aop"
	     ...
	       http://www.springframework.org/schema/aop
	       http://www.springframework.org/schema/aop/spring-aop-2.5.xsd">
	    ...
	 </beans>
```	 
    					
 1. Aspect를 만들때 - POJO클래스 사용
```
   1) POJO클래스 만들기
      - ProceedingJoinPoint를 인자로 가지는 메서드 작성 	
        ------------------->핵심기능을 가진 객체의 정보를 가진 객체 즉, 대상객체 정보를 가지고 있다.
               					 
	  public class ExeTimeAspect {
	
		public Object measure(ProceedingJoinPoint joinPoint) throws Throwable {
			long start = System.nanoTime();
			try {
				System.out.println("ExeTimeAspect::measure() -->핵심기능 수행전" );
				Object result = joinPoint.proceed(); //핵심기능 수행
				System.out.println("ExeTimeAspect::measure() -->핵심기능 수행후1" );
				return result;
			} finally {
				System.out.println("ExeTimeAspect::measure() -->핵심기능 수행후2" );
				long finish = System.nanoTime();
				Signature sig = joinPoint.getSignature();
				System.out.printf("%s.%s(%s) 실행 시간 : %d ns\n",
						joinPoint.getTarget().getClass().getSimpleName(),
						sig.getName(), Arrays.toString(joinPoint.getArgs()),
						(finish - start));
			}
		}
	}


  2) xml 설정화일 작성
     ■ <aop:config> 태그를 사용하여 AOP 관련 정보 설정
  
  	<!-- 공통 기능을 제공할 클래스를 빈으로 등록 -->
	<bean id="exeTimeAspect" class="aspect.ExeTimeAspect" />

	<!-- Aspect 설정: Advice를 어떤 Pointcut에 적용할 지 설정 -->
	<aop:config >
	    <!-- 공통 기능으로 사용할 Aspect 지정 -->
		<aop:aspect id="measureAspect" ref="exeTimeAspect">
			<!-- 어떤 핵심기능이 동작할때 적용하느냐 설정 즉, cahp07패키지 및 그 하위패키지의 모든 public 메서드가 동작할때-->
			<aop:pointcut id="publicMethod"
				expression="execution(public * chap07..*(..))" />
			<aop:around pointcut-ref="publicMethod" method="measure" />
		</aop:aspect>
	</aop:config>

 	<!-- 핵심기능을 가진 클래스도 빈으로 등록 -->
	<bean id="impeCal" class="chap07.ImpeCalculator">
	</bean>

	<bean id="recCal" class="chap07.RecCalculator">
	</bean>
    ===============================================================
```	

+ spring에서 AOP적용 코드 작성시 수행해 주어야 할 작업
```
1) 공통기능을 작성
   - 공통기능 수행 메서드는 인자값으로 반드시 ProceedingJoinPoint를 전달받도록 해야 한다.
                                   ====================>핵심기능을 수행하는 객체의 메서드정보가 들어가 있다. 
2) 핵심기능을 작성한다.

3) 설정화일(xml)에 공통기능과 핵심기능을 연결시키는 <aop:config>를 작성한다.
	<aop:config proxy-target-class="true">
	            =========================> 생성된 핵심기능객체 즉 bean얻은때 상위인터페이스명으로 검색하는게 기본이다.
	                                    이때 생성한 하위클래스정보로 객체를 얻고 싶다면 설정한다. 	              
		<aop:aspect id="measureAspect" ref="exeTimeAspect"> ==>공통기능객체가 어떤 bean이라고 설정
			<aop:pointcut id="publicMethod"
				expression="execution(public * chap07..*(..))" />				
			<aop:around pointcut-ref="publicMethod" method="measure" /> ==> 공통기능객체의 어떤 메서드가 공통기능을 수행하는 메서드라는 것을 설정함과 동시에
			                          언제 작동할지 즉, 어떤 핵심기능이 작동할때 호출될지 설정한다.
			
		</aop:aspect>
	</aop:config>

    ***우리가 직접 코딩할때는 일반적으로
       핵심기능에서 공통기능을 호출하는 형태를 가진다.
       그런데 AOP에서는 공통기능에서 핵심기능을 호출하는 형태로 프로그램실행이 역제어 되어 있는 형태다. ***
```       

+ 공통기능이 여러개이다.
```
 1. 공통기능
     시간 측정기능
     한번이라도 factorial을 계산했으면 그값을 저장했다가 얻어서 사용하는 기능

 2. 핵심기능

    factorial 계산
```         
 
```    
     ○ <aop:*>에 해당하는 태그 정보
          - <aop:config> : AOP 설정 정보를 나타냄.
          - <aop:aspect> : Aspect를 설정.
          - <aop:pointcut> : Pointcut을 설정.
          - <aop:around> : Around Advice를 설정. 이 외에도 다양한 Advice를 설정.
     
     ○ <aop:aspect> 태그
          ● ref 속성
          - Aspect로서 기능을 제공할 빈을 설정할  때 사용.
     ■ 특징
      - Advice를 대상 객체에 적용하기 위해 더 이상 프록시 객체를 사용할 필요가 없음.
      - <aop:pointcut>의 expression 속성에 Advice를 적용할 Pointcut에 대한 정보를 지정하기만 하면 
            자동으로 Advice가 해당 Pointcut 에 적용.
      - 설정 파일만 보더라도 어렵지 않게 어떤 코드에 어떤 Aspect가 어떤 타입의 Advice로 적용되는지를 파악할 수 있음.
     
     ■ Aspect 설정
      ○ <aop:aspect> 태그
        - 한 개의 Aspect를 설정.
        - ref 속성에는 공통 기능을 구현하고 있는 빈을 전달.
	  ○ 설정
		<aop:config>
			<aop:aspect id="measureAspect" ref="exeTimeAspect">
				<aop:pointcut id="publicMethod"
					expression="execution(public * chap07..*(..))" />				
				<aop:around pointcut-ref="publicMethod" method="measure" />
			</aop:aspect>
		</aop:config>
      ○ Advice를 적용한 Pointcut
        - <aop:pointcut> 태그를 이용하여 설정.
      ○ <aop:pointcut> 태그
        - id 속성 : Pointcut을 구분하는데 사용되는 식별 값을 입력 받음.
        - expression 속성 : Pointcut을 정의하는 AspectJ의 표현식을 입력 받음.
      ○ Advice 정의 관련 태그
        - <aop:before> 메서드 실행 전에 적용되는 advice를 정의
        - <aop:after-returning> 메서드가 정상적으로 실행된 후에 적용되는 advice를 정의
        - <aop:after-throwing> 메서드가 예외를 발생시킬 때 적용되는 advice를 정의, try-catch블럭의 catch블럭과 비슷
        - <aop:after> 메서드가 정상적으로 실행되는지 또는 예외를 발생시키는지 여부에 상관없이 적용되는 advice를 정의. try-catch-finally에서 finally블럭과 비슷
        - <aop:around> MethodInterceptor와 마찬가지로 메서드 호출 이전, 이후, 예외발생등 모든 시점에 적용 가능한 advice를 정의
        --------------------------------------------------------------------------------------------
        - 각 태그는 pointcut 속성 또는 pointcut-ref 속성을 사용하여 Advice가 적용될 Pointcut을 지정.
        - 각 태그는 Pointcut에 포함되는 대상 객체의 메서드가 호출될 때 <aop:aspect> 태그의 ref 속성으로 지정한 빈 객체에서 어떤
               메서드를 실행할지를 지정
      ○ pointcut-ref 속성
        - <aop:pointcut> 태그를 이용하여 설정한 Pointcut을 참조할 때 사용.
      ○ pointcut 속성
        - 직접 AspectJ 표현식을 이용하여 Pointcut을 지정할 때에 사용.
        

    ===============================================================	
```



2. @Aspect 애노테이션 사용
```
   1) POJO클래스로 Aspect 클래스를 만든다. 단 xml Aspect를 설정하지 않고
         아래의 애노테이션을 적용하여 만든다.
      @Aspect, 	
      @Pointcut("execution(public * chap07..*(..))")
      @Around("publicTarget()")
        애노테이션을 사용한 Aspect 클래스 만들기
   2) xml 설정화일 작성
      <!-- @Aspect 애노테이션을 인지하여 프록시객체를 생성하도록 설정해야함 -->
      <aop:aspectj-autoproxy />
	
	  <!-- Aspect를 빈으로 등록 -->
	  <bean id="exeTimeAspect" class="aspect.ExeTimeAspect2" />       
```


3. 자바설정화일을 이용하여 빈객체를 생성한다면 @EnableAspectJAutoProxy을 사용한다.
```
    --------------------------------------------
    
*** 프록시 객체가 인터페이스를 구현하여 자동으로 생성될때와 
             클래스를 이용하여 자동으로 생성될때의 차이점
             
***	<aop:pointcut id="publicMethod"
				expression="execution(public * chap07..*(..))" />
				execution(수식어? 리턴타입 클래스이름?메서드이름(파라미터))
						* ==> 모든값 의미
						..==>0개이상 이라는 의미 
	<aop:around pointcut-ref="publicMethod" method="measure" />
```
	



4. Advice의 적용 순서
```
   <aop:aspect id="calculatorCache" ref="cacheAspect" order="1">
   order속성값이 작은 Aspect부터 적용된다.   
   
   @Order(1)  
   -----------------------------------
   	Calculator impeCal = ctx.getBean("impeCal", Calculator.class);
	impeCal.factorial(5);  
      위 메서드의 내부적인 작동순서
     
    ExeTimeAspet ---> CacheAspect ---> ImpeCalculator.factorial(5)
     										120계산
     				  map에 [5,120] 저장
      경과시간 계산하여 출력
     		
    impeCal.factorial(5);  	
    ExeTimeAspet ---> CacheAspect             
					  캐쉬에서 검색하여 값구한다	
      경과시간 계산하여 출력					  
```



+ chap07/MainAspect.java
```java
package main;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import chap07.Calculator;
import chap07.RecCalculator;
import config.AppCtx;

public class MainAspect {
	
	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtx.class);
		System.out.println("ApplicationContext초기화 끝-------------------------------------------------------------------");
		
		String[] names = ctx.getBeanDefinitionNames();
		for(String name : names) {
			System.out.println(name);
		}
		
		System.out.println("ApplicationContext사용----------------------------------------------------------------------");
		
		Calculator cal = ctx.getBean("calculator", Calculator.class);
		//RecCalculator cal = ctx.getBean("calculator", RecCalculator.class);
		//오류발생 : 이유) 내부으로 Calculator를 구현한 Proxy17클래스형 객체가 만들어 지므로 형변환을 할 수 없다.
		//이를 해결한 코드 MainAspectWithClassproxy.java
		
		//아래의 코드에서 어느 핵심기능을 작동시킬때 공통기능이 동작해야 하는지에 대한 설정을 
		//Aspect객체 즉, 공통기능객체에 설정하면 아래의 코드에서 factorial()을 호출하면,
		//직접 factorial()이 호출되는 것이 아니라, 공통기능의 메서드가 동작하고 
		//그 메서드 내부에서 핵심기능을 호출하는 간접호출방식이 사용된다.
		long fiveFact = cal.factorial(5);
		
		System.out.println("cal.factorial(5) = " + fiveFact);
		System.out.println(cal.getClass().getName());
		
		System.out.println("ApplicationContext사용끝----------------------------------------------------------------------");
				
		ctx.close();
	}
}
```


+ chap07/AppCtx.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

import aspect.ExeTimeAspect;
import chap07.Calculator;
import chap07.RecCalculator;

@Configuration
@EnableAspectJAutoProxy  //@Aspect 어노테이션을 붙인 클래스를 공통기능으로 적용하시오
public class AppCtx {
	//공통기능 객체를 빈으로 생성
	@Bean
	public ExeTimeAspect exeTimeAspect() {
		return new ExeTimeAspect();
	}

	@Bean
	public Calculator calculator() {
		return new RecCalculator();
	}
}
```


+ chap07/ExeTimeAspect.java
```java
package aspect;

import java.util.Arrays;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.core.annotation.Order;


//AOP에서는 공통기능을 수행하는 클래스를 아무런 제약이 없는 일반 자바클래스로 만든다.

@Aspect
//@Order(1)
public class ExeTimeAspect {
	public ExeTimeAspect() {
		System.out.println("@Aspect -- ExeTimeAspect::ExeTimeAspect()....");
	}
	
	//공통기능을 적용할 대상을 설정한다==>chap07패키지와 그 하위 패키지에 위치한 타입의 public메서드를 pointcut으로 설정.
	@Pointcut("execution(public * chap07..*(..))")
	private void publicTarget() {
		System.out.println("@Pointcut -- ExeTimeAspect::publicTarget()...");
	}

	//Around Advice 즉, publicTarget()에 적용된 대상의 메서드가 호출전/후에 아래의 공통기능이 적용된다.
	//아래의 measure()메서드의 인자로 전달된 객체안에 사용자가 실제호출할 핵심기능객체및 메서드의 모든정보가 저장되어 있다.
	@Around("publicTarget()")
	public Object measure(ProceedingJoinPoint joinPoint) throws Throwable {
		System.out.println("@Pointcut -- ExeTimeAspect::measure(ProceedingJointPoint)...");
		//핵심기능 작동이전의 공통기능 -- 웹프로그램이라면, 로그인체크기능
		long start = System.nanoTime();
		//핵심기능 수행
		try {
			Object result = joinPoint.proceed(); //핵심기능 호출
			return result;
		} finally {
			//로그파일에 접속정보 저장 -- 로그파일에 접속정보 저장
			long finish = System.nanoTime();
			System.out.println("걸린시간은 "+(finish - start)+" 나노초입니다.");
			
			Signature sig = joinPoint.getSignature();
			System.out.printf("%s.%s(%s) 실행 시간 : %d ns\n",
					joinPoint.getTarget().getClass().getSimpleName(), //클래스명 : RecCalculator
					sig.getName(),                                    //메서드명 : factorial
					Arrays.toString(joinPoint.getArgs()),             //인자정보 : [5]
					(finish - start));                                //실행시간 : 끝-시작
		}
	}
}
```



##### PointCut
```
 AspectJ의 Pointcut 표현식
    - AspectJ는 Pointcut을 명시할 수 있는 다양한 명시자를 제공.
    - 스프링은 메서드 호출과 관련된 명시자만을 지원.
    ■ execution 명시자
      - Advice를 적용할 메서드를 명시할 때 사용.
      ○ 기본 형식
        execution(수식어패턴? 리턴타입패턴 패키지패턴?이름패턴(파라미터패턴)
        ● 수식어 패턴
          - 생략가능한 부분.
          - public, protected 등이 옴.
        ● 리턴타입패턴
          - 리턴 타입을 명시
        ● 클래스이름 패턴, 이름패턴
          - 클래스 이름 및 메서드 이름을 패턴으로 명시.
        ● 파라미터패턴
          - 매칭될 파라미터에 대해서 명시.
      ○ 특징
        - 각 패턴은 '*'를 이용하여 모든 값을 표현.
        - '..'을 이용하여 0개 이상이라는 의미를 표현.
      ○ 설정 예
        ● execution(public void set*(..))
          - 리턴 타입이 void이고 메서드 이름이 set으로 시작하고, 파라미터가 0개 이상인 메서드 호출.
        ● execution(* com.jica.chap07.*.*())
          - com.jica.chap07 패키지의 모든 클래스의 파라미터가 없는 모든 메서드 호출.
        ● execution(* com.jica.chap07..*.*(..))
          - com.jica.chap07 패키지 및 하위 패키지에 있는 모든 클래스의 파라미터가 0개 이상인 메서드 호출.
        ● execution(Integer com.jica.chap07.WriteArticleService.write(..))
          - 리턴 타입이 Integer인 com.jica.chap07패키지의 WriteArticleService 인터페이스의 write() 메서드 호출.
        ● execution(* get*(*))
          - 이름이 get으로 시작하고 1개의 파라미터를 갖는 메서드 호출.
        ● execution(* get*(*, *))
          - 이름이 get으로 시작하고 2개의 파라미터를 갖는 메서드 호출.
        ● execution(* read*(Integer, ..))
          - 메서드 이름이 read로 시작하고, 첫 번째 파라미터 타입이 Integer이며, 1개 이상의 파라미터를 갖는 메서드 호출.
    ■ within 명시자
      - 메서드가 아닌 특정 타입에 속하는 메서드를 Pointcut으로 설정할 때 사용.
      ○ 설정 예
        ● within(com.jica.chap07.WriteArticleService)
          - WriteArticleService 인터페이스의 모든 메서드 호출.
        ● within(com.jica.chap07.*)
          - com.jica.chap07 패키지에 있는 모든 클래스의 모든 메서드 호출.
        ● within(com.jica.chap07..*)
          - com.jica.chap07 패키지 및 그 하위 패키지에 있는 모든 메서드 호출.
    ■ bean 명시자
      - 스프링 2.5 버전부터 스프링에서 추가적으로 제공하는 명시자.
      - 스프링 빈 이름을 이용하여 Pointcut을 정의.
      - 빈 이름의 패턴을 갖는다.
      ○ 설정 예
        ● bean(writeArticleService)
          - 이름이 writeArticleService인 빈의 메서드 호출.
        ● bean(*ArticleService)
          - 이름이 ArticleServie로 끝나는 빈의 메서드 호출.
    ■ 표현식 연결
      - 각각의 표현식은 '&&' 나 '||' 연산자를 이용하여 연결 가능.
      ○ @Aspect 어노테이션을 이용하는 경우
        - '&&' 연산자를 사용하여 두 표현식을 모두 만족하는 Joinpoint에만 Advice가 적용.
          @AfterThrowing(
          pointcut = "execution(public * get*()) && execution(public void set*(..))")
          public void throwingLogging() {
            ...
          }
      ○ XML 스키마를 이용하여 Aspect를 설정하는 경우
        - '&&'나 '||' 연산자를 사용.
        <aop:pointcut id="propertyMethod"
            expression="execution(public * get*()) &amp;&amp; execution(public void set*(..))" />
         - XML 문서이기 때문에 값에 들어가는 '&&' '||'를 '&amp;&amp;'로 표현.
         - 설정파일에서 '&&'나 '||' 대신 'and'와 'or'를 사용할 수 있도록 하고 있음.
        <aop:pointcut id="propertyMethod"
           expression="execution(public * get*()) and execution(public void set*(..))" />

    □ 프록시 구현 방식에 따른 execution 적용 차이
    - Pointcut은 실제로 프록시가 구현하고 있는 인터페이스를 기준으로 Pointcut을 적용.
    - 인터페이스를 구현하고 있는 대상 객체에 대해서 Pointcut을 정의하려면 인터페이스를 기준으로 Pointcut을 작성.
      <aop:aspect id="cacheAspect" ref="cache">
         <aop:around method="read"
             pointcut="execution(public * com..core.ReadArticleServiceImpl.get*(..))" />
         </aop:aspect>

        <bean id="readArticleService" class="com.jica.chap07.core.ReadArticleServiceImpl" />
        
    - ReadArticleServiceImpl 클래스가 ReadArticleService 인터페이스를 구현하고 있다면, 
        <aop:around>는 ReadArticleServiceImpl 클래스의 get으로 시작하는 메서드에 적용.
        (<aop:around> 태그에서 명시한 Pointcut은 readArticleService 빈에는 적용되지 않음.)
    - ReadArticleServiceImpl 클래스가 인터페이스를 구현하고 있지 않다면,
           생성된 프록시는 ReadArticleServiceImpl 클래스를 상속받아 생성됨. (Pointcut은 readArticleService 빈에만 적용.) AspectJ의 Pointcut 표현식
    - AspectJ는 Pointcut을 명시할 수 있는 다양한 명시자를 제공.
    - 스프링은 메서드 호출과 관련된 명시자만을 지원.
    ■ execution 명시자
      - Advice를 적용할 메서드를 명시할 때 사용.
      ○ 기본 형식
        execution(수식어패턴? 리턴타입패턴 패키지패턴?이름패턴(파라미터패턴)
        ● 수식어 패턴
          - 생략가능한 부분.
          - public, protected 등이 옴.
        ● 리턴타입패턴
          - 리턴 타입을 명시
        ● 클래스이름 패턴, 이름패턴
          - 클래스 이름 및 메서드 이름을 패턴으로 명시.
        ● 파라미터패턴
          - 매칭될 파라미터에 대해서 명시.
      ○ 특징
        - 각 패턴은 '*'를 이용하여 모든 값을 표현.
        - '..'을 이용하여 0개 이상이라는 의미를 표현.
      ○ 설정 예
        ● execution(public void set*(..))
          - 리턴 타입이 void이고 메서드 이름이 set으로 시작하고, 파라미터가 0개 이상인 메서드 호출.
        ● execution(* com.jica.chap07.*.*())
          - com.jica.chap07 패키지의 모든 클래스의 파라미터가 없는 모든 메서드 호출.
        ● execution(* com.jica.chap07..*.*(..))
          - com.jica.chap07 패키지 및 하위 패키지에 있는 모든 클래스의 파라미터가 0개 이상인 메서드 호출.
        ● execution(Integer com.jica.chap07.WriteArticleService.write(..))
          - 리턴 타입이 Integer인 com.jica.chap07패키지의 WriteArticleService 인터페이스의 write() 메서드 호출.
        ● execution(* get*(*))
          - 이름이 get으로 시작하고 1개의 파라미터를 갖는 메서드 호출.
        ● execution(* get*(*, *))
          - 이름이 get으로 시작하고 2개의 파라미터를 갖는 메서드 호출.
        ● execution(* read*(Integer, ..))
          - 메서드 이름이 read로 시작하고, 첫 번째 파라미터 타입이 Integer이며, 1개 이상의 파라미터를 갖는 메서드 호출.
    ■ within 명시자
      - 메서드가 아닌 특정 타입에 속하는 메서드를 Pointcut으로 설정할 때 사용.
      ○ 설정 예
        ● within(com.jica.chap07.WriteArticleService)
          - WriteArticleService 인터페이스의 모든 메서드 호출.
        ● within(com.jica.chap07.*)
          - com.jica.chap07 패키지에 있는 모든 클래스의 모든 메서드 호출.
        ● within(com.jica.chap07..*)
          - com.jica.chap07 패키지 및 그 하위 패키지에 있는 모든 메서드 호출.
    ■ bean 명시자
      - 스프링 2.5 버전부터 스프링에서 추가적으로 제공하는 명시자.
      - 스프링 빈 이름을 이용하여 Pointcut을 정의.
      - 빈 이름의 패턴을 갖는다.
      ○ 설정 예
        ● bean(writeArticleService)
          - 이름이 writeArticleService인 빈의 메서드 호출.
        ● bean(*ArticleService)
          - 이름이 ArticleServie로 끝나는 빈의 메서드 호출.
    ■ 표현식 연결
      - 각각의 표현식은 '&&' 나 '||' 연산자를 이용하여 연결 가능.
      ○ @Aspect 어노테이션을 이용하는 경우
        - '&&' 연산자를 사용하여 두 표현식을 모두 만족하는 Joinpoint에만 Advice가 적용.
          @AfterThrowing(
          pointcut = "execution(public * get*()) && execution(public void set*(..))")
          public void throwingLogging() {
            ...
          }
      ○ XML 스키마를 이용하여 Aspect를 설정하는 경우
        - '&&'나 '||' 연산자를 사용.
        <aop:pointcut id="propertyMethod"
            expression="execution(public * get*()) &amp;&amp; execution(public void set*(..))" />
         - XML 문서이기 때문에 값에 들어가는 '&&' '||'를 '&amp;&amp;'로 표현.
         - 설정파일에서 '&&'나 '||' 대신 'and'와 'or'를 사용할 수 있도록 하고 있음.
        <aop:pointcut id="propertyMethod"
           expression="execution(public * get*()) and execution(public void set*(..))" />

    □ 프록시 구현 방식에 따른 execution 적용 차이
    - Pointcut은 실제로 프록시가 구현하고 있는 인터페이스를 기준으로 Pointcut을 적용.
    - 인터페이스를 구현하고 있는 대상 객체에 대해서 Pointcut을 정의하려면 인터페이스를 기준으로 Pointcut을 작성.
      <aop:aspect id="cacheAspect" ref="cache">
         <aop:around method="read"
             pointcut="execution(public * com..core.ReadArticleServiceImpl.get*(..))" />
         </aop:aspect>

        <bean id="readArticleService" class="com.jica.chap07.core.ReadArticleServiceImpl" />
        
    - ReadArticleServiceImpl 클래스가 ReadArticleService 인터페이스를 구현하고 있다면, 
        <aop:around>는 ReadArticleServiceImpl 클래스의 get으로 시작하는 메서드에 적용.
        (<aop:around> 태그에서 명시한 Pointcut은 readArticleService 빈에는 적용되지 않음.)
    - ReadArticleServiceImpl 클래스가 인터페이스를 구현하고 있지 않다면,
           생성된 프록시는 ReadArticleServiceImpl 클래스를 상속받아 생성됨. (Pointcut은 readArticleService 빈에만 적용.)
```



+ AOP관련 api 도움말
	- https://www.eclipse.org/aspectj/doc/next/runtime-api/index.htm





+ AppCtxWithCache.java
```java
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

import aspect.CacheAspect;
import aspect.ExeTimeAspect;
import chap07.Calculator;
import chap07.RecCalculator;

@Configuration
@EnableAspectJAutoProxy
public class AppCtxWithCache {

	//공통기능1
	@Bean
	public CacheAspect cacheAspect() {
		return new CacheAspect();
	}

	//공통기능2
	@Bean
	public ExeTimeAspect exeTimeAspect() {
		return new ExeTimeAspect();
	}

	@Bean
	public Calculator calculator() {
		return new RecCalculator();
	}
}
```

+ ExeTimeAspect.java
```java
package aspect;

import java.util.Arrays;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.core.annotation.Order;


//AOP에서는 공통기능을 수행하는 클래스를 아무런 제약이 없는 일반 자바클래스로 만든다.

@Aspect
@Order(1)
public class ExeTimeAspect {
	public ExeTimeAspect() {
		System.out.println("@Aspect -- ExeTimeAspect::ExeTimeAspect()....");
	}
	
	//공통기능을 적용할 대상을 설정한다==>chap07패키지와 그 하위 패키지에 위치한 타입의 public메서드를 pointcut으로 설정.
	@Pointcut("execution(public * chap07..*(..))")
	private void publicTarget() {
		System.out.println("@Pointcut -- ExeTimeAspect::publicTarget()...");
	}

	//Around Advice 즉, publicTarget()에 적용된 대상의 메서드가 호출전/후에 아래의 공통기능이 적용된다.
	//아래의 measure()메서드의 인자로 전달된 객체안에 사용자가 실제호출할 핵심기능객체및 메서드의 모든정보가 저장되어 있다.
	@Around("publicTarget()")
	public Object measure(ProceedingJoinPoint joinPoint) throws Throwable {
		System.out.println("@Pointcut -- ExeTimeAspect::measure(ProceedingJointPoint)...");
		//핵심기능 작동이전의 공통기능 -- 웹프로그램이라면, 로그인체크기능
		long start = System.nanoTime();
		//핵심기능 수행
		try {
			Object result = joinPoint.proceed(); //핵심기능 호출
//			System.out.println("결과값은 "+result);
			return result;
		} finally {
			//로그파일에 접속정보 저장 -- 로그파일에 접속정보 저장
			long finish = System.nanoTime();
			System.out.println("걸린시간은 "+(finish - start)+" 나노초입니다.");
			
			Signature sig = joinPoint.getSignature();
			System.out.printf("%s.%s(%s) 실행 시간 : %d ns\n",
					joinPoint.getTarget().getClass().getSimpleName(), //클래스명 : RecCalculator
					sig.getName(),                                    //메서드명 : factorial
					Arrays.toString(joinPoint.getArgs()),             //인자정보 : [5]
					(finish - start));                                //실행시간 : 끝-시작
		}
	}
}
```

+ CacheAspect.java
```java
package aspect;

import java.util.HashMap;
import java.util.Map;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.core.annotation.Order;

@Aspect
@Order(2)
public class CacheAspect {

	private Map<Long, Object> cache = new HashMap<>();

	public CacheAspect() {
		System.out.println("@Aspect --CacheAspect::CacheAspect()...");
	}
	
	@Pointcut("execution(public * chap07..*(long))")
	public void cacheTarget() {
		System.out.println("@Pointcut -- CacheAspect::cacheTarget()...");		
	}
	
	@Around("cacheTarget()")
	public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
		System.out.println("@Around -- CacheAspect::execute(ProceedingJoinPoint)...");
		Long num = (Long) joinPoint.getArgs()[0];
		if (cache.containsKey(num)) {
			System.out.printf("CacheAspect: Cache에서 구함[%d]\n", num);
			return cache.get(num);
		}

		Object result = joinPoint.proceed();
		cache.put(num, result);
		System.out.printf("CacheAspect: Cache에 추가[%d]\n", num);
		return result;
	}
}
```

+ MainAspectWithCache.java
```java
package main;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import chap07.Calculator;
import config.AppCtxWithCache;

public class MainAspectWithCache {
	
	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtxWithCache.class);
		System.out.println("ApplicationContext초기화 끝-------------------------------------------------------------------");
		
		String[] names = ctx.getBeanDefinitionNames();
		for(String name : names) {
			System.out.println(name);
		}
		
		System.out.println("ApplicationContext사용----------------------------------------------------------------------");
	
		Calculator cal = ctx.getBean("calculator", Calculator.class);
		long v1 = cal.factorial(7);
		System.out.println("v1결과값 : "+v1);
		//작동순서 Order(1)....ExeTimeAspect.measure() <-| 결과값과 시간 출력
		//		Order(2)....CacheAspect.execute()     | <--| 내부 HashMap 7! 계산값
		//		RecCalculator.factorial(7) 게산 ----------> |
		long v2 = cal.factorial(7);
		System.out.println("v1결과값 : "+v2);
		long v3 = cal.factorial(5);
		System.out.println("v1결과값 : "+v3);
		long v4 = cal.factorial(5);
		System.out.println("v1결과값 : "+v4);
		
		System.out.println("ApplicationContext끝----------------------------------------------------------------------");
		
		ctx.close();
	}
}
```


#### 4. 실습
#### 5. Summary / Close





-------------------------------------------------------------------------

### [2019-07-31]

#### 1. Review
+ 반복적인 커넥션... 반복적인 예외처리 같은 작업을 ApplicationContext가 관리하도록 해야한다.

#### 2. JDBC 사용하기
##### 스프링에서 JDBC사용하기
0. 실습 순서
   1) main.MainForMemberDao.java  ----> JdbcTemplate클래스 사용법 설명
   2) main.MainForCPS.java        ----> 트랜잭션 처리(xml설정)
   3) main.MainForJS.java         ----> 트랜잭션 처리(어노테이션 설정화일)
   
1. 스프링에서의 jdbc
   1) JdbcTemplate클래스를 이용하여 JDBC프로그램에서의 코드 중복을 극복하고 있다.
   2) 트랜잭션(transaction)처리를 위한 
      connection 객체의 setAutoCommit(false), commit(), rollback()사용을
      @Transaction 애노테이션을 이용하여 간소화 한다.
   3) 컨넥션풀을 사용
   
2. 준비
   - pom.xml화일 설정 -- 다음의 의존성을 추가한다.
```xml   
   		<!-- JdbcTemplate등 JDBC 연동에 필요한 기능 제공 --> 
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring-framework.version}</version>
		</dependency>	

		
		<!-- Oracle 연결에 필요한 jdbc 드라이버 제공 -->
    	<dependency>
		    <groupId>oracle</groupId>
		    <artifactId>ojdbc6</artifactId>
		    <version>11.2.0</version>
            <systemPath>C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib\ojdbc6.jar</systemPath>
            <scope>system</scope>
        </dependency> 
```        

3. 테이블 생성(member.sql)
```sql
    DROP TRIGGER member_id_trigger;
	DROP SEQUENCE member_id_seq;
	DROP TABLE member;

	CREATE TABLE member(
		id 			NUMBER PRIMARY KEY,
		email 		VARCHAR2(255) UNIQUE,
		password 	VARCHAR2(255),
		name 		VARCHAR2(100),
		regdate 	DATE
	);

	CREATE SEQUENCE member_id_seq
	START WITH 1;
	        
	-- member 테이블에 insert 할 때마다 id column에 이 sequence 를 적용시킬 수 있도록 trigger를 걸어보도록 하자.
	CREATE OR REPLACE TRIGGER member_id_trigger
	BEFORE INSERT	ON member
	REFERENCING NEW AS NEW
	FOR EACH ROW
	BEGIN
		SELECT member_id_seq.NEXTVAL INTO :NEW.ID FROM dual;
	END;
	/
	
	--INSERT INTO member(id, email, password, name, regdate)
	--VALUES(member_id_seq.NEXTVAL, 'madvirus@madvirus.net', '1234', 'JICA', SYSDATE);
	
	INSERT INTO member(email, password, name, regdate)
	VALUES('madvirus@madvirus.net', '1234', 'JICA', SYSDATE);
	
	COMMIT;
```

4. 어플리케이션 컨텍스트 설정화일
```java
  	@Bean(destroyMethod = "close")
	public DataSource dataSource() {
		DataSource ds = new DataSource();
		ds.setDriverClassName("oracle.jdbc.driver.OracleDriver");
		ds.setUrl("jdbc:oracle:thin:@127.0.0.1:1521:XE");
		ds.setUsername("SCOTT");
		ds.setPassword("TIGER");
		ds.setInitialSize(2);
		ds.setMaxActive(10);
		ds.setTestWhileIdle(true); //유휴 컨텍션 검사
		ds.setMinEvictableIdleTimeMillis(1000* 60 * 3); //최소 유휴시간 3qns
		ds.setTimeBetweenEvictionRunsMillis(10 * 1000);	//10초 주기	

		return ds;
	}
```

5. JdbcTemplate 사용하기
```java   
   import javax.sql.DataSource;
   import org.springframework.jdbc.core.JdbcTemplate;
   
   public class MemberDao {
		private JdbcTemplate jdbcTemplate;
	
		public MemberDao(DataSource dataSource) {
			1)JdbcTemplate객체 생성
			this.jdbcTemplate = new JdbcTemplate(dataSource);
		}
		...
   }
```   

1)JdbcTemplate객체 사용( 드라이버 로드, 컨넥션 얻기,   close()등은 생략해도
                          우리가 사용하는 메서드 내부에서 처리해 준다. sql명령 자체에만 집중하면 된다.)
    
    - DDL, DML(select, insert, update, delete), DTL(rollback, commit)
                          
   	- SELECT구문의 사용법
   	1) 결과값이 단일 row, 단일 column :  
   	      SELECT COUNT(*) FROM member;
   	      SELECT COUNT(*) FROM emp;
   	   queryForObject(String sql, Integer.class)
   	      SELECT COUNT(*) FROM emp WHERE deptno = 20 AND sal > 2000;
   	   queryForObject(Sting sql, Integer.class, Object args...);
   	   queryForObject("SELECT COUNT(*) FROM emp WHERE deptno=? AND sal > ?",
   	                  Integer.class, 20,2000}); 
   	                  
   	2) 결과값이 여러 row이고, 여러 column으로 구성되어 있을때
   	   SELECT * FROM emp WHERE deptno = 20;                  
     - 조회 1
     Statement 사용시
     public List<T> query(String sql, RowMapper<T> rowMapper) throws DataAccessException
    
     PreparedStatement 사용시 
	 public List<T> query(String sql, RowMapper<T> rowMapper, Object... args) throws DataAccessException
     public List<T> query(String sql, Object[] args, RowMapper<T> rowMapper) throws DataAccessException
     -------------------------------------------------------------
     RowMapper 인터페이스(클래스)를 이용하여 ResultSet의 결과를 자바 객체로 변환한다.
     sql파라미터가 PreparedStatement의 ?형태일때는 args 파라메터값을 사용한다.
     -------------------------------------------------------------
     
     RowMapper 인터페이스     
     public interface RowMapper<T>{
     	T mapRow(ResultSet rs, int rowNum) throws SQLException;
     } 
     
       사용예 1)
     public Member selectByEmail(String email) {
		List<Member> results = jdbcTemplate.query(
				"select * from MEMBER where EMAIL = ?",
				new RowMapper<Member>() {

					public Member mapRow(ResultSet rs, int rowNum)
							throws SQLException {
						Member member = new Member(rs.getString("EMAIL"),
								rs.getString("PASSWORD"),
								rs.getString("NAME"),
								rs.getTimestamp("REGDATE"));
						member.setId(rs.getLong("ID"));
						return member;
					}
				},
				email);

		return results.isEmpty() ? null : results.get(0);
	}
	
	사용예2)
	public List<Member> selectAll() {
		List<Member> results = jdbcTemplate.query("select * from MEMBER",
				new RowMapper<Member>() {
					//@Override
					public Member mapRow(ResultSet rs, int rowNum)
							throws SQLException {
						Member member = new Member(rs.getString("EMAIL"),
								rs.getString("PASSWORD"),
								rs.getString("NAME"),
								rs.getTimestamp("REGDATE"));
						member.setId(rs.getLong("ID"));
						return member;
					}
				});
		return results;
	}
	
	- 조회 2
	단일값을 리턴받는 쿼리문일때는  다음의 메서드를 사용하자
	public T queryForObject(String sql, Class<T> requiredType) throws DataAccessException
     
      사용예) 
    public int count() {
		Integer count = jdbcTemplate.queryForObject(
					"select count(*) from MEMBER", 
					Integer.class);
		return count;
	}   
	
	인자가 있는 경우는 다음 메서드를 사용한다.
	public <T> T queryForObject(String sql,Class<T> requiredType,Object... args) throws DataAccessException
        double avg = jdbcTemplate.queryForObject(
        			"select avg(height) from 테이블 where type=? and status=?",
        			Double.class,
        			100, "S");
        						
      실행결과가 두개 이상의 칼럼을 갖고 있는경우는 RowMapper를 사용    
	public <T> T queryForObject(String sql, RowMapper<T> rowMapper) throws DataAccessException      
    public <T> T queryForObject(String sql, RowMapper<T> rowMapper, Object... args) throws DataAccessException
        
    
    - INSERT, UPDATE, DELETE : update()메서드 사용
    public int update(String sql) throws DataAccessException
    public int update(String sql, Object... args) throws DataAccessException
    
    =======================================================
    PreparedStatementCreator 인터페이스 사용
    =======================================================
    INSERT시 KeyHolder를 이용한 자동키 생성
    
    	public void insert(final Member member) {
		KeyHolder keyHolder = new GeneratedKeyHolder();
		jdbcTemplate.update(new PreparedStatementCreator() {
			//@Override
			public PreparedStatement createPreparedStatement(Connection con) 
					throws SQLException {
				PreparedStatement pstmt = con.prepareStatement(
						"insert into MEMBER (EMAIL, PASSWORD, NAME, REGDATE) "+
						"values (?, ?, ?, ?)",
						new String[] {"ID"});
				pstmt.setString(1,  member.getEmail());
				pstmt.setString(2,  member.getPassword());
				pstmt.setString(3,  member.getName());
				pstmt.setTimestamp(4,  
						new Timestamp(member.getRegisterDate().getTime()));
				return pstmt;
			}
		}, keyHolder);
		Number keyValue = keyHolder.getKey();
		member.setId(keyValue.longValue());
	}
      
1) 예외처리 :jdbc 프로그램에서는 명령어를 사용하면서 SQLException철리를 했다.
    JdbcTemplate에서는 SQLException이 발생하면 내부적으로  DataAccessException으로 변환시켜 발생해준다.
    
     예외가 발생하면 BadSqlGrammarException이 발생한다.
     
     다양한 종류의 예외에 대해서 모두 혹은 반드시 예외처리구분을 사용하는 것이 아니라
     사용자가 개입할 필요성이 있든 부분에만 선택적으로 예최처리 구문을 작성하면 된다. 
     
     jdbc에서는 반드시 try catch로 처리해야만 했다면
     JdbcTemplate에서는 안해도 된다. 내가 개입할 필요가 있을때만 선택적으로 SQLException을 사용하지 말고
```	 
                                 try{
                                   sql명령실행코드...
                                 } catch(DataAccessException){
                                 }          						
```


##### 실습
+ chap08/AppCtx.java
```java
package config;

import org.apache.tomcat.jdbc.pool.DataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import spring.ChangePasswordService;
import spring.MemberDao;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberPrinter;
import spring.MemberRegisterService;

@Configuration
@EnableTransactionManagement //<---- 트렌젝션 메니저가 활성화하도록 함.
public class AppCtx {

	@Bean(destroyMethod = "close")
	public DataSource dataSource() {
		DataSource ds = new DataSource();
		//mysql --> "com.mysql.jdbc.Driver"
		ds.setDriverClassName("oracle.jdbc.driver.OracleDriver");
		//mysql --> "jdbc:mysql:thin://localhost/db명?charsetEncoding=utf8"
		ds.setUrl("jdbc:oracle:thin:@127.0.0.1:1521:XE");
		ds.setUsername("SCOTT");
		ds.setPassword("TIGER");
		ds.setInitialSize(2);
		ds.setMaxActive(10);
		ds.setMaxIdle(10);
		ds.setTestWhileIdle(true); //유휴 컨텍션 검사
		ds.setMinEvictableIdleTimeMillis(1000* 60 * 3); //최소 유휴시간 3qns
		ds.setTimeBetweenEvictionRunsMillis(10 * 1000);	//10초 주기	

		return ds;
	}

	//트렌젝션 예외처리 자동을 위한 구문
	@Bean
	public PlatformTransactionManager transactionManager() {
		DataSourceTransactionManager tm = new DataSourceTransactionManager();
		tm.setDataSource(dataSource());
		return tm;
	}

	@Bean
	public MemberDao memberDao() {
		return new MemberDao(dataSource());
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
}
```



+ chap08/pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.jica</groupId>
  <artifactId>chap08</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>chap08</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    
 		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>5.0.2.RELEASE</version>
		</dependency>
		
		<!-- JDBC Template을 사용하기위한 설정 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>5.0.2.RELEASE</version>
		</dependency>
		
		<!-- 여러 Connection Pool을 사용하는 방법중에서
		Tomcat의 JDBC기능의 커텍션풀을 사용하기위한 설정 -->
		<dependency>
			<groupId>org.apache.tomcat</groupId>
			<artifactId>tomcat-jdbc</artifactId>
			<version>8.5.27</version>
		</dependency>
		<!-- 
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.45</version>
		</dependency>
		-->
				
		<!-- Oracle 연결에 필요한 jdbc 드라이버 제공 -->
    	<dependency>
		    <groupId>oracle</groupId>
		    <artifactId>ojdbc6</artifactId>
		    <version>11.2.0</version>
            <systemPath>C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib\ojdbc6.jar</systemPath>
            <scope>system</scope>
        </dependency>

		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>1.7.25</version>
		</dependency>

		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>1.2.3</version>
		</dependency>
  </dependencies>
</project>
```

+ chap08/MemberDao.java
```java
package spring;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.List;

import javax.sql.DataSource;

import org.springframework.dao.DataAccessException;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.PreparedStatementCreator;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.support.GeneratedKeyHolder;
import org.springframework.jdbc.support.KeyHolder;

public class MemberDao {

	private JdbcTemplate jdbcTemplate;

	public MemberDao(DataSource dataSource) {
		// JdbcTemplate객체가 만들어지면 
		// 내부적으로 오라클드라이버도 로드하고
		// 컨넥션풀도 만들어서 컨넥션을 유지하고 있다.
		// 우리는 JdbcTemplate객체를 이용하여 다음의 기능을 사용할 것이다.
		// 1. SELECT 
		//    결과값이 단일값  (SQL> SELECT COUNT(*) FROM member;)
		//    결과값이 여러 컬럼 (SQL> SELECT * FROM member WHERE id=1;)
		//    결과값이 여러 row이고 여러 컬럼 (SQL> SELECT * FROM member;)
		//    조건등 여러곳에 값을 사용한 표현(파라메터)
		// 2. UPDATE(INSERT, UPDATE, DELETE)
		this.jdbcTemplate = new JdbcTemplate(dataSource);
		
		// 이제부터는 JdbcTemplate객체의 모든 메서드 내부에서
		// connection을 얻거나 반납할 필요가 있을때 datasource를 이용한
		// connection pool에서 동작하게 한다.
	}

	public Member selectByEmail(String email) {
		List<Member> results = jdbcTemplate.query(
				"select * from MEMBER where EMAIL = ?",
				new RowMapper<Member>() {
					@Override
					public Member mapRow(ResultSet rs, int rowNum) throws SQLException {
						Member member = new Member(
								rs.getString("EMAIL"),
								rs.getString("PASSWORD"),
								rs.getString("NAME"),
								rs.getTimestamp("REGDATE").toLocalDateTime());
						member.setId(rs.getLong("ID"));
						return member;
					}
				}, email);

		return results.isEmpty() ? null : results.get(0);
	}

	public void insert(Member member) {
		KeyHolder keyHolder = new GeneratedKeyHolder();
		jdbcTemplate.update(new PreparedStatementCreator() {
			@Override
			public PreparedStatement createPreparedStatement(Connection con)
					throws SQLException {
				// 파라미터로 전달받은 Connection을 이용해서 PreparedStatement 생성
				PreparedStatement pstmt = con.prepareStatement(
						"insert into MEMBER (EMAIL, PASSWORD, NAME, REGDATE) " +
						"values (?, ?, ?, ?)",
						new String[] { "ID" });
				// 인덱스 파라미터 값 설정
				pstmt.setString(1, member.getEmail());
				pstmt.setString(2, member.getPassword());
				pstmt.setString(3, member.getName());
				pstmt.setTimestamp(4,
						Timestamp.valueOf(member.getRegisterDateTime()));
				// 생성한 PreparedStatement 객체 리턴
				return pstmt;
			}
		}, keyHolder);
		Number keyValue = keyHolder.getKey();
		member.setId(keyValue.longValue());
	}

	public void update(Member member) {
		//JdbcTemplate사용시
		//우리는 Driver load, Connectiont얻기, Statement생성, ResultSet조작, 예외처리등의 작업을
		//전혀 사용하지않고 단, sql명령과 결과를 처리하는데 집중하면 된다.
		jdbcTemplate.update(
				"update MEMBER set NAME = ?, PASSWORD = ? where EMAIL = ?",
				member.getName(), member.getPassword(), member.getEmail());
	}

	public List<Member> selectAll() {
		/*
		List<Member> results = jdbcTemplate.query("select * from MEMBER",
				(ResultSet rs, int rowNum) -> {
					Member member = new Member(
							rs.getString("EMAIL"),
							rs.getString("PASSWORD"),
							rs.getString("NAME"),
							rs.getTimestamp("REGDATE").toLocalDateTime());
					member.setId(rs.getLong("ID"));
					return member;
				});
		return results;
		*/
		
		//<T> List<T>	query(String sql, RowMapper<T> rowMapper)
		//                                ======================
		//                                select문으로 얻어진 ResultSet의 데이타가 여러건일때
		//                                1건의 데이타를 사용자가 관리하는 객체로 변환시켜주는 클래스
		
		//별도로 RowMapper 클래스를 만들어 놓았을때
		//MemberMapper memberMapper = new MemberMapper();		
		//List<Member> results = jdbcTemplate.query("select * from MEMBER", memberMapper);
				
		//bof-------------------------
		//   0번째 row
		//   1번재 row
		//   2번재 row
		//   3번재 row		
		//   4번재 row		
		//eof-------------------------
		
		// RowMapper를 익명의 클래스로 사용할때
		List<Member> results = jdbcTemplate.query("select * from MEMBER",
				new RowMapper<Member>() {
					//@Override
					public Member mapRow(ResultSet rs, int rowNum)
							throws SQLException {
						//디버깅
						System.out.println("mapRow() : " + rowNum);

						Member member = new Member(rs.getString("EMAIL"),
								rs.getString("PASSWORD"),
								rs.getString("NAME"),
								rs.getTimestamp("REGDATE").toLocalDateTime());
						member.setId(rs.getLong("ID"));
						return member;  //리턴된 member객체는 List에 추가된다.
					}
				});
		
		return results;
	}

	public int count() {
		
		// public <T> T queryForObject(String sql,    Class<T> requiredType) throws DataAccessException
		// select문의 결과 row가  1건이고 컬럼도 1일때 사용
		Integer count = null;
		try {
			count = jdbcTemplate.queryForObject("select count(*) from MEMBER", Integer.class);
		}catch(DataAccessException e) {
			//예외처리 코드
		}
		return count;
	}
}
```



+ chap08/MainForMemberDao.java
```java
package main;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.List;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppCtx;
import spring.Member;
import spring.MemberDao;

public class MainForMemberDao {
	private static MemberDao memberDao;

	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtx.class);
		System.out.println("ApplicationContext초기화 끝-------------------------------------------------------------------");
		
		String[] names = ctx.getBeanDefinitionNames();
		for(String name : names) {
			System.out.println(name);
		}
		
		System.out.println("ApplicationContext사용----------------------------------------------------------------------");

		
		memberDao = ctx.getBean(MemberDao.class);

		selectAll();
		
		selectByEmail();
		
		updateMember();
		
		insertMember();
		
		System.out.println("ApplicationContext끝----------------------------------------------------------------------");

		ctx.close();
	}

	private static void selectAll() {
		System.out.println("----- selectAll");
		int total = memberDao.count();		
		System.out.println("전체 데이터: " + total);
		
		List<Member> members = memberDao.selectAll();
		//                               ============
		//                            x  1) connection 얻기
		//                               2) Statement, preparedStatement 객체 얻기
		//                               3) sql문장 작성
		//                               4) 실행시켜 결과를 ResultSet으로 받는다.
		//                            x  5) ArrayList<Member>()를 생성한후
		//                            x  6) while( rs.next() ){
		//                            x          rs.getXXX() 컬럼값얻기
		//                            x         객체생성
		//                            x         list.add(객체);
		//                            x     }
		//                            x  7) connection.close()
		//                               8) list객체 리턴
		for (Member m : members) {
			System.out.println(m.getId() + ":" + m.getEmail() + ":" + m.getName() + " 패스워드 :" + m.getPassword());
		}		
		for (Member m : members) {
			System.out.println(m.getId() + ":" + m.getEmail() + ":" + m.getName());
		}
	}

	private static void selectByEmail() {
		System.out.println("----- selectByEmail");
		Member member = memberDao.selectByEmail("jica03@daum.net");
			
		System.out.println(member);
	}
	
	private static void updateMember() {
		System.out.println("----- updateMember");
		Member member = memberDao.selectByEmail("jica02@daum.net");
		String oldPw = member.getPassword();
		String newPw = Double.toHexString(Math.random());
		member.changePassword(oldPw, newPw);

		memberDao.update(member);
		System.out.println("암호 변경: " + oldPw + " > " + newPw);
	}

	private static DateTimeFormatter formatter = 
			DateTimeFormatter.ofPattern("MMddHHmmss");

	private static void insertMember() {
		System.out.println("----- insertMember");

		String prefix = formatter.format(LocalDateTime.now());
		Member member = new Member(prefix + "@test.com", 
				prefix, prefix, LocalDateTime.now());
		memberDao.insert(member);
		System.out.println(member.getId() + " 데이터 추가");
	}
}
```



+ chap08/main/sql/ddl.sql
```sql
create user 'spring5'@'localhost' identified by 'spring5';

create database spring5fs character set=utf8;

grant all privileges on spring5fs.* to 'spring5'@'localhost';

create table spring5fs.MEMBER (
    ID int auto_increment primary key,
    EMAIL varchar(255),
    PASSWORD varchar(100),
    NAME varchar(100),
    REGDATE datetime,
    unique key (EMAIL) 
) engine=InnoDB character set = utf8;
```


+ chap08/main/sql/member.sql
```sql
-- Oracle에서 예제 테이블을 생성

DROP TRIGGER member_id_trigger;
DROP SEQUENCE member_id_seq;
DROP TABLE member;

CREATE TABLE member(
	id 			NUMBER PRIMARY KEY,
	email 		VARCHAR2(255) UNIQUE,
	password 	VARCHAR2(255),
	name 		VARCHAR2(100),
	regdate 	DATE
);

CREATE SEQUENCE member_id_seq
START WITH 1;
        
-- member 테이블에 insert 할 때마다 id column에 이 sequence 를 적용시킬 수 있도록 trigger를 걸어보도록 하자.
CREATE OR REPLACE TRIGGER member_id_trigger
BEFORE INSERT	ON member
REFERENCING NEW AS NEW
FOR EACH ROW
BEGIN
SELECT member_id_seq.NEXTVAL INTO :NEW.ID FROM dual;
END;
/

--INSERT INTO member(id, email, password, name, regdate)
--VALUES(member_id_seq.NEXTVAL, 'madvirus@madvirus.net', '1234', 'JICA', SYSDATE);

INSERT INTO member(email, password, name, regdate)
VALUES('jica01@daum.net', '1234', 'JICA', SYSDATE);

INSERT INTO member(email, password, name, regdate)
VALUES('jica02@daum.net', '1234', 'JICA2', SYSDATE);

INSERT INTO member(email, password, name, regdate)
VALUES('jica03@daum.net', '1234', 'JICA3', SYSDATE);

INSERT INTO member(email, password, name, regdate)
VALUES('jica04@daum.net', '1234', 'JICA4', SYSDATE);

INSERT INTO member(email, password, name, regdate)
VALUES('jica05@daum.net', '1234', 'JICA5', SYSDATE);

COMMIT;
```




+ chap08/ChangePasswordService.java
```java
package spring;

import org.springframework.transaction.annotation.Transactional;

public class ChangePasswordService {

	private MemberDao memberDao;

	@Transactional    //<------ 아래 메서드가 작동할때, 트렌젝션 메니저 작동.
	public void changePassword(String email, String oldPwd, String newPwd) {
		Member member = memberDao.selectByEmail(email);
		if (member == null)
			throw new MemberNotFoundException();

		member.changePassword(oldPwd, newPwd);

		memberDao.update(member);
	}

	public void setMemberDao(MemberDao memberDao) {
		this.memberDao = memberDao;
	}
}
```


+ chap08/MainForCPS.java
```java
package main;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppCtx;
import spring.ChangePasswordService;
import spring.MemberNotFoundException;
import spring.WrongIdPasswordException;

public class MainForCPS {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = 
				new AnnotationConfigApplicationContext(AppCtx.class);

        ChangePasswordService cps = 
                ctx.getBean("changePwdSvc", ChangePasswordService.class);
        try {
            cps.changePassword("madvirus@madvirus.net", "1234", "1111");
            System.out.println("암호를 변경했습니다.");
        } catch (MemberNotFoundException e) {
            System.out.println("회원 데이터가 존재하지 않습니다.");
        } catch (WrongIdPasswordException e) {
            System.out.println("암호가 올바르지 않습니다.");
        }

		ctx.close();
	}
}
```





##### log4j

+ 스프링의 기본 로그메세지를 표현하는 JCL(Java Commons Logging) API 대신에
+ Log4j 모듈(log4j.xml)이 존재할 경우 Log4j를 이용해서 로그를 기록할수 있다.


1) 로깅 레벨 !
① FATAL : 가장 크리티컬한 에러가 일어 났을 때 사용합니다.
② ERROR : 일반 에러가 일어 났을 때 사용합니다.
③ WARN : 에러는 아니지만 주의할 필요가 있을 때 사용합니다.
④ INFO : 일반 정보를 나타낼 때 사용합니다.
⑤ DEBUG : 일반 정보를 상세히 나타낼 때 사용합니다.


2) 로그정보 형식 
```
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
	<appender name="console" class="org.apache.log4j.ConsoleAppender">
		<layout class="org.apache.log4j.PatternLayout">
			<param name="ConversionPattern" 
				value="[%t] [%d{yyyy-MM-dd HH:mm:ss}] %-5p %c:%M - %m%n" />
		</layout>
	</appender>

  appender 는 로그정보를 어디에다가 남길것인가를 정하는 부분이다 .
   콘솔이나, log파일, xml파일 등으로 남길수 있다. 여기서는 콘솔에 남긴다.

   내가 원하는 정보를 꾸미기를는 ConversionPattern 의 vaule에다가 해주면 된다 .
   상세 정보는 아래 표와 같다. 

   형식	설명
  ------------------------------------------------------------- 
  %p	debug, info, warn, error, fatal 등의 priority 가 출력된다.
  %m	로그내용이 출력됩니다
  %d	로깅 이벤트가 발생한 시간을 기록합니다.
          포맷은 %d{HH:mm:ss, SSS}, %d{yyyy MMM dd HH:mm:ss, SSS}같은 형태로 사용하며 SimpleDateFormat에 따른 포맷팅을 하면 된다
  %t	로그이벤트가 발생된 쓰레드의 이름을 출력합니다.
  %%	% 표시를 출력하기 위해 사용한다.
  %n	플랫폼 종속적인 개행문자가 출력된다. \r\n 또는 \n 일것이다.
  %c	카테고리를 표시합니다 
          예) 카테고리가 a.b.c 처럼 되어있다면 %c{2}는 b.c가 출력됩니다.
  %C	클래스명을 포시합니다. 
          예) 클래스구조가 org.apache.xyz.SomeClass 처럼 되어있다면 %C{2}는 xyz.SomeClass 가 출력됩니다
  %F	로깅이 발생한 프로그램 파일명을 나타냅니다.
  %l	로깅이 발생한 caller의 정보를 나타냅니다
  %L	로깅이 발생한 caller의 라인수를 나타냅니다
  %M	로깅이 발생한 method 이름을 나타냅니다.
  %r	어플리케이션 시작 이후 부터 로깅이 발생한 시점의 시간(milliseconds)
  %x	로깅이 발생한 thread와 관련된 NDC(nested diagnostic context)를 출력합니다.
  %X	로깅이 발생한 thread와 관련된 MDC(mapped diagnostic context)를 출력합니다.
  ------------------------------------------------------------
```

3. 어디서 나오는 로그를 볼 것인지도 정해줄수 있다 .
```
   디비쪽의 Connection 이라든가, preparedStatement 정보 든가, Bean 정보 등을
    <logger name="java.sql.Connection">
        <level value="DEBUG" />
    </logger>

    이런식으로 계속 추가해주면 그 부분에서의 로그를 볼 수 있다. 
	
	<root>
		<priority value="INFO" />
		<appender-ref ref="console" />
	</root>
```	

4. 이문서에는 앨리먼트 작성 순서가 있다.
``` 
   순서가 틀리면 
  log4j:WARN 요소 유형 "log4j:configuration"의 콘텐츠는 "(renderer*,appender*,plugin*,(category|logger)*,root?,(categoryFactory|loggerFactory)?)"과(와) 일치해야 합니다.
   이런 메시지가 뜬다 . 괄호 안에 있는 저 앨리먼트 순서대로 나열해줘야 한다.

```



##### 종합실행

+ chap08/Main.java
```java
package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import config.AppCtx;
import spring.ChangePasswordService;
import spring.DuplicateMemberException;
import spring.MemberInfoPrinter;
import spring.MemberListPrinter;
import spring.MemberNotFoundException;
import spring.MemberRegisterService;
import spring.RegisterRequest;
import spring.WrongIdPasswordException;

public class Main {

	private static AnnotationConfigApplicationContext ctx = null;

	public static void main(String[] args) throws IOException {
		ctx = new AnnotationConfigApplicationContext(AppCtx.class);

		BufferedReader reader =
				new BufferedReader(new InputStreamReader(System.in));
		while (true) {
			System.out.println("명렁어를 입력하세요:");
			String command = reader.readLine();
			if (command.equalsIgnoreCase("exit")) {
				System.out.println("종료합니다.");
				break;
			}
			if (command.startsWith("new ")) {
				processNewCommand(command.split(" "));
			} else if (command.startsWith("change ")) {
				processChangeCommand(command.split(" "));
			} else if (command.equals("list")) {
				processListCommand();
			} else if (command.startsWith("info ")) {
				processInfoCommand(command.split(" "));
			} else {
				printHelp();
			}
		}
		ctx.close();
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

	private static void printHelp() {
		System.out.println();
		System.out.println("잘못된 명령입니다. 아래 명령어 사용법을 확인하세요.");
		System.out.println("명령어 사용법:");
		System.out.println("new 이메일 이름 암호 암호확인");
		System.out.println("change 이메일 현재비번 변경비번");
		System.out.println("info 이메일");

		System.out.println();
	}
}
```



#### 3. Summary / Close