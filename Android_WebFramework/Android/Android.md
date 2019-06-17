
### [2019-06-10]

#### 1. Review
+ Java ----(JDBC)----- Oracle ------- WebProgram(HTML/JavaScript/Savlet/JSP)



#### 2. Android
+ 대부분이 Android Studio를 통한 개발로 이루어짐.

+ Android Studio
  1. Editor + 화면구성(직접개체생성-배치 or 툴사용-개체생성-배치)
  2. 컴파일(Android SDK) ---> 실행도구


##### 2.1. 개발환경설치

0) Java SDK설치

1) 전통적인 방법
```
   Eclipse + plug-in
           + Android SDK(클래스 라이브러리) - 도움말(Reference)
           + AVD(가상기기-시뮬레이터) | 실제기기 
```
2) Android Studio(안드로이드 통합개발환경)
```
   개발자 사이트(http://developer.android.com/studio)
   android-studio-ide-181.5014246-windows.exe 다운받아 실행
   1) tool 자체 필요내용(editor, 디자인도구, 시뮬레이터)
   2) 최신 Android SDK설치 ----> 도움말문서
   3) AVD tool , SDK tool
```
3) 프로젝트 만들기
```
   특이점)Java를 컴파일/실행코드를 만들때 Gradle을 이용하여  Project 관리
```

+ 참고) 안드로이드 버전
```
코드네임                               버전                        내부 build-version
---------------------------------------------------------------------------------------
컵케이크(Cupcake)                      -Android 1.5                API Lavel 3
도넛(Donut)                            -Android 1.6                4
이클레어(프랑스어: Eclair 에클레르[*]) -Android 2.0~2.1            5,6,7
프로요(Froyo, 프로즌 요구르트)         -Android 2.2                8
진저브레드(Gingerbread, 생강빵)        -Android 2.3                ....
허니콤(Honeycomb, 허니콤 토피)         -Android 3.0
아이스크림 샌드위치(Ice Cream Sandwich)-Android 4.0
젤리빈(Jelly Bean)                     -Android 4.1~4.3
킷캣(KitKat)                           -Android 4.4
롤리팝(Lollipop)                       -Android 5.0~5.1
마시멜로(Marshmallow)                  -Android 6.0                23
누가(Nougat)                           -Android 7.0~7.1            24,25
오레오(Oreo)                           -Android 8.0~8.1            26,27
파이(Pie)                              -Android 9.0                28,29

Android Studio를 설치하면 최신  Android SDK, 즉 현재로서는 Android 9.0(API Lavel 29)가 자동 설치된다. 자신의 기기(Device-Phone)에 맞는 SDK Manager를 이용하여 설치하기를 권장한다.
```



+ Android 플랫폼 구조
```
----------------------------------------------------------------------------
0. H/W - CPU(ARM process)
------------------------------
- O/S - Linux 커널계층
- 내부 라이브러리 계층 -- 자바가상머신
- 안드로이드 Framework계층 -- 안드로이드 클래스 라이브러리  <------|
- Application ==> 여기가 우리가 개발할 APP(앱) --------------------|
----------------------------------------------------------------------------

프로젝트 - Application개발
    *.java -------자바컴파일러----------------------------> *.class --|(*.dex 가상머신용)
    *.xml(디자인 tool의 결과물) ----(aapt.exe 컴파일)-----> *.aapt    |*.apk => 실행
    기타 리소스(resource) --------------------------------> 그대로 ---|
```


+ Android의 특징
```
1. 어플리케이션프레임워크제공
   -마법사를 통해 개발프로젝트의 성격에 맞는 기본적인 프레임워크를 제공
   -컴포넌트들의 재활용 및 교체 가능
   -자바 언어를 이용하여 개발

2. Dalvik  가상머신
  -안드로이드용 가상머신, 모바일 장치에 최적화

3. 최적화된 그래픽
   -기본적으로2D 그래픽 라이브러리를 제공한다.
   -OpenGL ES 1.0 스팩에 기반한 3D 그래픽 라이브러리를 제공한다.

4. SQLite
   -데이터를 저장하고 검색하기 위해 SQLite를 사용 한다.
   -일종의 데이터베이스 시스템이다.

5. 미디어 지원
    -오디오, 비디오, 정지 화상 포멧 지원
    -MPEG4,  H.264,  MP3,  AAC,  AMR,  JPG, PNG, GIF 등

6. 기타 지원사항
   -하드웨어 의존적이지만, 다음 항목에 대해 지원 가능하다.
   -GSM 테크놀로지, 블루투스, EDGE, 3G, WiFi, 
     카메라, GPS, 나침반, 가속도계

7. 풍부한 개발환경
   - 디바이스 에뮬레이터, 디버깅 도구, 메모리 및 성능 프로파일링,
      Eclipse IDE
```

![안드로이드 아키텍쳐](안드로이드_아키텍쳐.png "view계층구조")




##### 2.2. project구조
+ 실제 프로젝트 폴더의 구조와 개발화면의 구조가 다르다.

1. 개발화면에서의 프로젝트 구조
```
  app : 프로젝트 관련 모든 작업내용
        manifests
          AndroidManifest.xml(폰에 현재 프로젝트를 설치할때 현재App에 대한 정보(4대구성요소,권한)를 Android시스템에게 제공)
        
        java(소스 프로그램이 위치)
          com.jica.firstandroid
               Android  App의 4대구성요소에 대한 Java Code
                   1.Activity,
                   2.Broadcast Rreciver,
                   3.Service,
                   4.Contents Provider
        
        res(자바코드에서 사용하는 자원(resource)을 모아 놓은곳) : 주의점(모든화일명은 소문자로만 기술되어야 한다)
          drawable  - 이미지화일 (*.jpg, *.png ... *.xml)
          layout    - Activity의 화면구성을 디자인 tool을 사용했을때 결과물이 저장(xml)
          mipmap    - 런처이미지(스마트폰의 바탕화면에서 App을 식별하는 아이콘)
          values    - 값으로 표현될 내용을 저장해 놓고 Java Code나 layout에서 사용
                      값의 종류(용도)에 따라 화일명이 정해져 있다.

        참고) Java Code에서 res의 자원을 접근할때는 R.리소스종류.리소스식별자
            기타의 장소에서 res의 자원을 접근할때는 @리소스종류.리소스식별자
  
  Gradle Scripts : 프로젝트관리(컴파일/링크/설치/테스트-디버깅)에 필요한 정보를 설정(외부라이브러리 사용시)
      build.gradle(Project:프로젝트명)
      build.gradle(Module:app)

    화면하단의 탭구성 <- App개발도중에 실행중 상태정보 제공하는 창
```



2. 프로젝트를 실행
```
    - AVD( AVD Manager를 이용하여 AVD를 생성)
     - 실제기기( 1. 컴퓨터에서 스마트폰 인식 : 자동인식, 모델번호별 USB 드라이버 설치해서 인식-권장)
                 2. 스마트폰 설정메뉴 - 개발자옵션(없으면 폰정보에 빌드버전을 여러번 클릭) - USB 디버깅 ON )

참고사항) -- UI객체의 사용법
    1. Java Code에서는 생성자, 멤버변수, 메서드(set/get, 일반메서드, ...)
    2. 디자인 tool에서 즉, xml문서에서 접근 ----^
                                                속성(attribute)

```



##### 2.3. 4대 구성요소 (Intent:인텐트..정보교환)
1. Activity
2. Broadcast Receiver
3. Service (Background 기능)
4. Contents Provider




##### 2.4. Activity와 View
+ Activity : 화면구성요소를 관리하는 안드로이드 App의 구성요소
  - 보여지는 모든 화면요소(View)를 관리 : 추가/수정/삭제
  - 화면요소의 기능에 따른 분류
    1) 위젯(wiget) --> 특정 기능을 가졌다.
    2) 레이아웃(layout) --> 특정 기능보다는 구성요소 자체의 위치및 크기설정 즉, 화면배치에 그 기능이 집중된 위젯을 말함.
    *** 화면구성은 먼저 레이아웃을 결정하고,
        레이아웃내부에 위젯을 정해진 방식대로 배치하여 구성한다.


  - 화면구성 방법
    1) Java Code로 작성
    2) 디자인 Tool 사용(*.xml)  ==> 우선 학습내용.
    3) 두 방법을 함께 사용


  - LinearLayout을 이용한 화면구성
    1) 순차적으로(수평,수직) 내부 구성요소를 배치하는 Layout


  - TextView : 문자열을 보여주는 위젯


  - 액티비티에의해 보여지는 모든 구성요소들은 최상위 클래스로 View를 상속받아 작동
```
                             Object
                             View
                                    단일기능위젯(계층구조)
               ViewGroup            TextView, ImageView, Button, EditText, Checkbox,                            RadioButton
                xxxLayout
                ^           AdapterView(여러건의 데이터를 다양하게 관리)
                (복합위젯)      ListView
                                ExpandableListView
                                Spinner
                                GridView

    모든 보여지는 요소의 최상위 클래스는 View 클래스이다.
    View의 속성값은 향후 우리가 학습할 모든 위젯에서 공통적으로 사용하는 속성이된다.
    -------------------------------------------------------------------------------
        Java Code                       Design Tool
        ==================================================
        생성자
        멤버변수     
        set/get메서드  --------------->  xml attribute
        메서드
    -------------------------------------------------------------------------------

```

+ view계층구조
![view계층구조](view계층구조.jpg "view계층구조")

+ viewgroup-layout계층구조
![viewgroup-layout](viewgroup-layout계층구조.jpg "viewgroup-layout계층구조")



##### 2.5. View의 공통속성
```
View의 속성중 자주 사용하는 속성값들(중요)
=============================================================
멤버변수       (80 ~ 90%)                xml속성
-----------------------------------------------------------------------------
1) id      setId(속성명), getId()       android:id = 속성명       객체식별
2) backtround                           android:background         배경색
                                ----------------------------------------------
                                        색상지정 #AARRGGBB, #RRGGBB
3) width       ?                        android:layout_width       폭
   height      ?                        android:layout_height      크기
                                ----------------------------------------------
                                        1. 부모크기만큼 : MATCH_PARENT
                                        2. 자신의 내용물만큼 : WRAP_CONTENT
                                        3. 값지정 ==> dp(기본), px, dp, sp(글자크기), in, mm, ...

4) padding                              android:padding          경계선과 내용과 여백

5) gravity                              android:gravity           내용물의 위치(여백)
                                    ----------------------------------------------
                                            top         ldft   h_center   right
                                            v_center
                                            bottom
                                            
6) margine                              android:layout_margin    부모뷰or형제뷰 간격

7) layout gravity                       android:layout_gravity   부모뷰에게 자신의 위치!!

8) visibility                           android:visibility       가시성




속성값의 종류
- 자체 속성 : 독단적으로 값을 사용
    JavaCode에서 사용할수 있도록 set/get메서드가 제공된다.

- layout 속성 : 현재의 위젯이 어느 layout의 구성요소가 되느냐에 따라 달라지는 값
    JavaCode에서 set/get메서드 아닌, 별도방법 즉, layoutParam을 사용하여 별도로 설정.
1) text    setText("예제")              android:text = "예제"
```



#### 3. 실습
+ 묻지도 따지지도 말고 교재 35~57page까지만 그대로 실습!!
  + ConstraintLayout을 사용하여 화면 구성
  + 버튼 이벤트처리
  + 안드로이드에 이미 설치되어 있는 웹브라우저호출, 전화걸기 호출기능



#### 4. Summary / Close




-----------------------------------------------------------


### [2019-06-11]

#### 1. Review

#### 2. Activity와 View
+ 버튼에 이벤트를 줄수 있는 3가지 방법


+ MainActivity.java(BookHello_Project)
```java
package com.jica.bookhello;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.graphics.Color;
import android.net.Uri;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    LinearLayout rootLayout;
    Button btnMessage2;
    Button btnMessage3;
    Button btnCallBrowser, btnCallPhone;

    //생성자
    public MainActivity(){
        Log.d("TAG", "BookHello 앱의 MainActivity객체 생성됨!");
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Log.d("TAG","MainActivity::onCreate(Bundle)...");

        //화면구성내용을 resource file에서 가져온다
        //내부적으로 실행되는 내용
        //1) 지정된 layout화일의 모든 내용을 참조하여 객체생성 및 구조결정
        //2) 지정된 layout화일의 root element 객체를 액티비티와 연걸
        setContentView(R.layout.activity_main);

        //참고) tool내에서 생성하는 모든 자원들은 자동으로 식별자를 부여한다.
        //      이러한 식별자들만 정의해 놓는 내부화일 R.java화일이 만들어져서 사용된다.

        //버튼 이벤트 처리방법
        //1) 교재 -- button 리소스의 onClick속성사용(메서드명 작성)
        //2) 일반적인 이벤트 처리 방법 - Java의 AWT방식과 유사


        //UI객체 찿기 - findViewById(id)
        rootLayout = findViewById(R.id.layoutRoot);
        btnMessage2 = findViewById(R.id.btnMessage2);
        btnMessage3 = findViewById(R.id.btnMessage3);
        btnCallBrowser = findViewById(R.id.btnCallBrowser);
        btnCallPhone = findViewById(R.id.btnCallPhone);

        //배경색 바꾸기
        rootLayout.setBackgroundColor(Color.WHITE);

        //두번째 버튼 클릭시 이벤트핸들러 설정 - 방법2)
        //MyOnClickListener btnHandler = new MyOnClickListener();
        //btnMessage2.setOnClickListener(btnHandler);

        btnMessage2.setOnClickListener(new MyOnClickListener());


        //세번째 버큰 클릭시 이벤트핸들러 설정 - 방법3)
        btnMessage3.setOnClickListener(new View.OnClickListener(){

            @Override
            public void onClick(View view) {
                Log.d("TAG","세번째 버튼이 클릭되었습니다");
                Toast.makeText(getApplicationContext(),"세번째 버튼이 클릭되었습니다.", Toast.LENGTH_SHORT).show();
            }
        });

        //브라우저호출,전화걸기 클릭시 이벤트핸들러 설정 - 방법4)
        btnCallBrowser.setOnClickListener(this);
        btnCallPhone.setOnClickListener(this);

    }

    //버튼을 클릭했을때 작동할 메서드 - 방법1)
    public void onButton1Clicked(View view){
        System.out.println("consol 화면에 정보 출력");
        Log.v("TAG","logcat에 정보를 출력 verbose");
        Log.d("TAG","logcat에 정보를 출력 debug");

        //스마트폰 화면에 간단 메세지 출력(잠시나타났다가 사라지는 메서지)--> Toast
        Toast.makeText(this,"버튼이 클릭되었습니다.",Toast.LENGTH_SHORT).show();

    }

    //브라우저호출,전화걸기 버튼클릭시 동작할 이벤트핸들러 메서드 - 방법4)
    @Override
    public void onClick(View view) {
        //어느버튼이 클릭되어서 호출되었는지를 판단하는 방법 2가지
        /* 방법1)
        Button curButtton = (Button)view;
        if(curButtton == btnCallBrowser){
            Log.d("TAG", "웹브라우저 호출버튼이 클릭되었습니다.");
        }else{ //btnCallPhone
            Log.d("TAG", "전화걸기 버튼이 클릭되었습니다.");
        }
        */

        //인자로 전달된 객체의 id값을 구하여 비교--방법2)
        int curId = view.getId();
        Log.d("TAG", "현재 id값 : " + curId);
        if(curId == R.id.btnCallBrowser){
            Log.d("TAG", "웹브라우저 호출버튼이 클릭되었습니다.");

            //실제 웹브라우저(외부) 호출
            //Intent(String action, Uri uri) -- 암시적 인텐트
            Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://m.naver.com"));
            startActivity(intent);

       }else{ //R.id.btnCallPhone
            Log.d("TAG", "전화걸기 버튼이 클릭되었습니다.");

            //전화걸기 기능 호출
            Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("tel:010-7242-9424"));
            startActivity(intent);
        }
    }

    //방법2)
    class MyOnClickListener implements View.OnClickListener{

        @Override
        public void onClick(View view) {
            Log.d("TAG","두번째 버튼이 클릭되었습니다.");

            //외부클래스객체를 지칭할때 : 외부클래스명.this ==> MainActivity.this
            Toast toast = Toast.makeText(getApplicationContext(),"두번째 버튼이 클릭되었습니다.", Toast.LENGTH_SHORT );
            toast.show();
        }
    }
}
```



#### 3. 기본위젯과 배치관리자
1. 배치관리자 -- xxxLayout
   - LinearLayout
   - RelativeLayout
   - FrameLayout
   - TableLayout
   - GridLayout
   - ConstraintLayout

2. 기본위젯(단일기능 위젯)
   - TextView, EditText, Button, ImageView, Checkbox, RadioButton,



+ 실습1)
  + 1. MainActivity에서 다른 Activity를 호출해 보자
  + 2. 현재화면 구성내용을 바꾸어 보자.
  - res폴더에 정의된 식별자를 사용하여 값을 이용할때
    1) Java Code에서 R.color.colorPrimary
    2) res폴더내부에서  @color/colorPrimary 
    - res폴더의 values폴더는 종류에 따라 식별자명=값의 형태로 
      - 문자열(strings.xml), 색상코드(colors.xml), 스타일과 테마(styles.xml)
      - 별도의 파일에 값이 저장된다


##### [오늘의 과제]
+ 수업 후 실습 (교재 70~85 Page)

#### 4. 실습
#### 5. Summary / Close




-----------------------------------------------------------


### [2019-06-12]

#### 1. Review
1. Activity의 생명주기 (생성 -> 사용 -> 소멸)
2. View의 공통속성 -> 위젯(widget)
   1. 자체속성 ..... android:속성명=값
   2. layout속성 ... android:layout.속성명
3. Event처리 방법


#### 2. Activity와 View
##### Activity 생명주기
+ 기본으로 사용하는 AppComatActivity의 상속계층구조
```
javajava.lang.Object
   ↳	android.content.Context
 	   ↳	android.content.ContextWrapper
 	 	   ↳	android.view.ContextThemeWrapper
 	 	 	   ↳	android.app.Activity
 	 	 	 	   ↳	android.support.v4.app.FragmentActivity
 	 	 	 	 	   ↳	android.support.v7.app.AppCompatActivity
```



+ 액티비티 생명주기 관련 메서드들
```
- 생성 ------> 생성자()
             onCreate(Bundle)
             onStart() <-- onRestart()
             onResume()<-| |
                         | |
- 사용자 <---> 활성화    | |
          || onPause() --| |
 강제종료 || onStop() -----|
             onDestroy()
                  |
                소멸(정상소멸-사용자 백버튼 클릭, ProCode-finish())
                        |--> 스마트폰 회전(가로모드, 세로모드)
```



+ onCreate(Bundle) 메서드의 인자로 전달되는 Bundle의 역할
  - 안드로이드 시스템에 의해 액티비티가 소멸되었다가 다시 생성되어야 할때
                            ================
                            1) 회전
                            2) 강제종료

  - 이전의 상태정보를 복구할 목적으로 만들어 놓은 것이다.



+ Bundle객체에 임시 상태정보를 저장하거나
    - 아래의 메서드는 정상 종료시는 작동하지 않는다.
    - 위의 안드로이드 시스템에 의해 액티비티가 소멸되고
    - 다시 생성될 필요가 있을때만 동작한다.
```
    protected void onSaveInstanceState(Bundle outState)
    protected void onRestoreInstanceState(Bundle outState)
```




##### 뷰(View) : 화면에 보여지는 모든 요소(객체)
1) Layout(레이아웃) : 구성요소를 배치하는데 기능이 집중된 View
2) Widget(위젯) : 특정 기능을 수행하는 View


```
Object
    View
        단일기능위젯(계층구조)
        TextView, ImageView, Button, EditText, Checkbox, RadioButton

        ViewGroup
            xxxLayout : 배치관리자
                복합위젯
            AdapterView : 대량의 데이터를 관리하는 복합
                ListView, Gallery, GridView
                    ExpandableListView, Spinner
=======================================================================
    View ------- 기능위젯(TextView, Button)
    상속            실제 화면구성에서 사용하는 관계(포함)
    Viewgroup -- 레이아웃(LinearLayout)
=======================================================================

    모든 View 즉, 위젯들은 자신만의 속성(멤버변수)들을 가지고 있다.
    모든 View가 공통으로 가지고 있는 속성을 상위클래스인 View클래스에 있다.

```


+ View의 속성(xml - attribute, java - 멤버변수)
         ==== 속성의 종류
            1) 자체 속성    android:속성명
            2) layout 속성  android:layout_속성명



1. id : 개체를 식별하는 값
   1. 식별자 : 정수값(자동생성-R.java)-R.id.id명
   2. 사용 : Java code => R.id.id명 / res내부 => @id/id명
   3. Java Code => getId()

2. View의 크기 : 폭, 높이 ==> match_parent, wrap_content, 직접지정(기본단위:dp,..)
   - Java Code에서의 기본단위는 pixel단위이다.
   - layout_width
   - layout_height


+ 참고) res폴더에는 새로운 폴더를 앞으로 만들어 나갈것이다.
  - 아무이름이나 상요하지 않고 미리 정해진 명칭들이 있다.
  - res폴더의 모든 명칭은(폴더명, 파일명)은 소문자이어야 한다.


3. 마진(margin) 속성 : 부모뷰나 형제뷰간의 간격
   - layout_margin
   - layout_marginLeft, layout_marginRight
   - layout_marginTop, layout_marginBottom
   


4. 패딩(padding)속성 : 내용물과 현재뷰의 간격
   - padding
   - paddingLeft, paddingRight
   - paddingTop, paddingBottom



##### 전체영역, 내용물, 여백, padding, margin의 관계
```
```
+ 전체영역에 내용물을 표시하고 남는 공간이 있다면 -- 여백
+ 테두리선과 내용물간의 간격을 설정 -- padding
+ 그리고 남은 공간에서의 내용물의 위치 설정 -- gravity

+ 현재의 위젯과 바깥 즉, 부모위젯이나 형제위젯과의 간격 -- layoutMargin





5. 배경색(background)
   - #AARRGGBB, #RRGGBB, #ARGB, #RGB





##### 자주사용하는 Layout(결론:ConstraintLayout, LinearLayout이 제일 많이 사용)
1. LinearLayout : 순서대로 구성요소(위젯) 배치
2. RelativeLayout : 부모뷰나 형제뷰를 기준으로 구성요소의 위치 결정
3. FrameLayout : 구성요소를 겹쳐서 배치
4. TableLayout : TableRow를 이용하여 행담위로 규격화시켜 배치
5. GridLayout : row, column(엑셀화면-표현식)으로 규격화시켜 배치
--------------------------
5. ConstraintLayout : 부모뷰나 형제뷰를 기준으로 구성요소의 위치결정
                        추가적으로 기준이 되는 요소(가이드라인)를 추가.



##### LinearLayout : 좌-->우, 위-->아래로 순차적으로 위젯을 배치한다.
+ 중요한 속성
    1) orientation  : 방향
    2) gravity      : 내부 내용물 즉, 위젯의 위치설정
    3) baselineAligned : 글자를 내용물로 하는 위젯이 옆으로 배치될때
                        글자의 baseline에 맞추어 위젯의 위치를 자동으로 맞춰준다.
    ----------------------------------------------------
    
    LinearLayout 내부에 존재하는 위젯의 Layout 속성
        1) layout_width, layout_height ---|
        2) layout_margin -----------------| 모든 layout에서 공통사용
        3) layout_weight ---> LinearLayout내부의 위젯에만 존재하는 속성으로
                                화면을 차지할 비중을 지정한다.
                                (1) layout_weight 속성에 값을 지정X- > 원래크기 영역
                                (2) 값을 지정할때 특정위젯 1, 나머지영역 독차지
                                (3) 몇개의 위젯 1, 영역을 공평하게 나눠서 차지.
                                (4) 모든 위젯에 서로다른 값지정, 비율로나눠서 차지.
        4) layout_gravity --> LinearLayout내부의 위젯에만 존재하는 속성
                             부모위젯의 고유배치방식(좌-우,위-아래)에 영향X 영역요청
    --------------------------------------------------
    위의 layout속성들은 set/get 메서드가 존재하지 않으므로
    java code로 객체를 만들때는 특별한 방법을 상요하여 해당 특성ㅇ르 지정해야한다.




##### LinearLayout을 Java Code로 작성하기
  
+ Linear2Activity.java
```java
package com.jica.basicwidget;

import android.graphics.Color;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.Button;
import android.widget.LinearLayout;

public class Linear2Activity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        //UI xml파일 전개
        //setContentView(R.layout.activity_linear2);

        //직접 코드로 화면구성
        //1.LinearLayout객체 생성
        LinearLayout linearLayout = new LinearLayout(this);
        linearLayout.setOrientation(LinearLayout.VERTICAL);
        linearLayout.setBackgroundColor(Color.GREEN);

        //2.버튼객체생성
        Button button = new Button(this);
        button.setText("Java Code로 작성");
        button.setTextSize(30);  //pixel
        button.setTextColor(Color.RED);

        //3.버튼을 LinearLayout에 등록
        linearLayout.addView(button);

        //4.LinearLayout을 액티비티에 연결
        setContentView(linearLayout);
    }
}
```


##### [오늘의 과제]
+ ConstraintLayout
+ 별도의 Activity를 만들되 좋아하는 영화배우 프로필 화면 만들기
+ 



#### 3. 실습
#### 4. Summary / Close





-----------------------------------------------------------


### [2019-06-13]

#### 1. Review
+ 데이터 복구
  1) 임시정보
  2) 반영구정보
     1) java file i/o
     2) sharedPreforence -> put(키,값) / getXXX(키)
     3) 데이터베이스(sqlite)




#### 2. 기본위젯과 배치관리자

##### layout속성들을 표현하는 Java class들도 뷰의 계층구조와 유사하게 계층구조를 형성하고 있다.
```
[뷰의 상속계층구조]
Object
    View
        ViewGroup
            LinearLayout

    layout속성을 표현하는 LayoutParams의 상속계층구조
    --------------------------------------------------
    java.lang.Object
   ↳	android.view.ViewGroup.LayoutParams  ----  [layout_height/layout_width]
 	   ↳	android.view.ViewGroup.MarginLayoutParams
                    -- [layout_margin
                        layout_marginBottom	
                        layout_marginLeft
                        layout_marginRight
                        layout_marginTop]
 	 	   ↳	android.widget.LinearLayout.LayoutParams
                    --- [layout_gravity/layout_weight]

```

+ 참고) xml로 화면을 디자인한것은 정적인 화면구성이다.
  + 만일 실행시 화면구성요소에 대한 변화가 필요하다면 (추가/삭제),
  + 코드로 작성해야 한다.



##### 영구정보를 저장하고 사용하는 방법
+ 영구정보 : 정상적으로 액티비티가 종료되었을때 현재의 작업내용(상태정보)이 이후에 다시 액티비티를 실행시켰을때 이용할 수 있도록 영속성을 가지는 정보를 관리하는 것을 말한다.

+ 방법)
  1) java언어 file i/o
  2) SharedPreference 사용 (키-값)
  3) SQLite(데이터베이스)




##### 액티비티 호출시 정보를 전달하거나 작업결과를 되돌려 받는 방법
1. 액티비티 호출시 정보전달
[호출하는 액티비티]
   - Intent 생성
     - Intent intent = new Intent(this, AgumentActivity.class);

   - Intent 객체에 정보 저장 ==> putExtra("키",값);
     - intent.putExtra("title", "기생충");
     - intent.putExtra("poster", R.drawable.prasite);
     - intent.putExtra("actor", "봉준호");
     - intent.putExtra("director", "송강호");

   - 액티비티 호출
     - startActivity(intent);


[호출하는 액티비티] - ArgumentActivity
   - OnCreate(), onResum()메서드에서 정보 얻기
     - Intent intent = getIntent();
   
   - 전달된 인자정보를 intent객체에 getXXXExtra(키,[디폴트값])
     - title = intent.getStringExtra("title");
     - posterId = intent.getIntExtra("poster",-1);
     - actor = intent.getStringExtra("actor");
     - director = intent.getStringExtra("director");
   
   - 인자값을 목적에 맞게 사용




2. 액티비티에 작업한 결과내용을 호출한 액티비티에서 사용하기[내일]


+ Sub3Activity.java
```java
package com.jica.basicwidget;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.RadioGroup;

public class Sub3Activity extends AppCompatActivity {

    RadioGroup grMovie;

    String movieTitles[] = {"알라딘", "기생충", "맨인블랙", "천로역전", "이웃집토토로"};
    int moviePosterIds[] = {R.drawable.aladdin, R.drawable.parasite, R.drawable.meninblack, R.drawable.the_pilgrim, R.drawable.totoro};
    String moviesActors[] = {"메나마수드, 웰 스미스",
            "송강호, 이선균",
            "크리스 헴스워스, 테사 톰슨",
            "폰 라이스 데이비스",
            "토토로"};
    String moviesDirectors[] = {"가이리치", "봉준호", "F.게리 그레이","로버트 페르난데스", "미야자키 하야오"};


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sub3);

        grMovie = findViewById(R.id.grMovie);
    }

    public void onMovieDetail(View view) {
        int curId = grMovie.getCheckedRadioButtonId();
        int index = 0;
        Intent intent = new Intent(this, AgumentActivity.class);

        //액티비티 호출시 정보를 전달하고 싶다면 Intent의 extra속성에 전달
        switch (curId) {
            case R.id.rbAladdin:
                index = 0;
                break;
            case R.id.rbParasite:
                index = 1;
                break;
            case R.id.rbMeninblack:
                index = 2;
                break;
            case R.id.rbPilgrim:
                index = 3;
                break;
            case R.id.rbTotoro:
                index = 4;
                break;
        }
       intent.putExtra("title", movieTitles[index]);
       intent.putExtra("poster", moviePosterIds[index]);
       intent.putExtra("actor", moviesActors[index]);
       intent.putExtra("director", moviesDirectors[index]);
       startActivity(intent);
    }
}
```

+ AgumentActivity.java
```java
package com.jica.basicwidget;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ImageView;
import android.widget.TextView;

public class AgumentActivity extends AppCompatActivity {
    //UI객체
    TextView tvtitle, tvActor, tvDirector;
    ImageView ivPoster;

    //UI객체에 내용값 - 호출될때 인자로 전달됨
    String title, actor, director;
    int posterId;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        //UI xml 전개
        setContentView(R.layout.activity_agument);

        //UI객체 참조값 얻기
        tvtitle = findViewById(R.id.tvTitle);
        ivPoster = findViewById(R.id.ivPoster);
        tvActor = findViewById(R.id.tvActors);
        tvDirector = findViewById(R.id.tvDirector);

        //현재의 액티비티가 호출될때 전달한 인자값 얻기
        Intent intent = getIntent();
       Intent intent;

       title = intent.getStringExtra("title");
       posterId = intent.getIntExtra("poster",-1);
       actor = intent.getStringExtra("actor");
       director = intent.getStringExtra("director");

        tvtitle.setText(title);
        ivPoster.setImageResource(posterId);
        tvActor.setText(actor);
        tvDirector.setText(director);
    }
}
```



##### [오늘의 과제]
+ 액티비티 호출시, putExtra에 사용자가 만든 객체를 전달할 수 있는 방법
+ MovieInfo 객체를 넘겨주는 방법 찾기~!!!
```
public class MovieInfo implements Serializable {
    private String title;
    private int posterId;
    private String actor;
    private String director;

    public MovieInfo() {
    }
}
//////////////////////////////////////
MovieInfo m = new MovieInfo();
intent.putExtra("info", m);
/////////////////////////////////////
intent.getStringExtra("info");
```
+ 현재 예제의 MainActivity와 ScrollView을 사용하여 메뉴항목이 늘어나 보이도록 변경.





##### RelativeLayout(상대레이아웃)
+ 부모뷰나 형제뷰를 기준으로 현재뷰(위젯)의 위치를 설정한다.
```
java.lang.Object
   ↳	android.view.View
 	   ↳	android.view.ViewGroup
 	 	   ↳	android.widget.RelativeLayout


android:layout_above	
android:layout_alignBaseline
android:layout_alignBottom	
android:layout_alignEnd	
android:layout_alignLeft
android:layout_alignParentBottom	
android:layout_alignParentEnd	
android:layout_alignParentLeft	
android:layout_alignParentRight	
android:layout_alignParentStart	
android:layout_alignParentTop	
android:layout_alignRight	
android:layout_alignStart	
android:layout_alignTop	
android:layout_alignWithParentIfMissing	
android:layout_below	
android:layout_centerHorizontal	
android:layout_centerInParent	
android:layout_centerVertical	
android:layout_toEndOf	
android:layout_toLeftOf	
android:layout_toRightOf
android:layout_toStartOf


java.lang.Object
   ↳	android.view.ViewGroup.LayoutParams
 	   ↳	android.view.ViewGroup.MarginLayoutParams
 	 	   ↳	android.widget.RelativeLayout.LayoutParams

```



##### TableLayout : 행단위(TableRow)의 위젯배치
```
java.lang.Object
   ↳	android.view.View
 	   ↳	android.view.ViewGroup
 	 	   ↳	android.widget.LinearLayout
 	 	 	   ↳	android.widget.TableLayout


java.lang.Object
   ↳	android.view.ViewGroup.LayoutParams
 	   ↳	android.view.ViewGroup.MarginLayoutParams
 	 	   ↳	android.widget.LinearLayout.LayoutParams
 	 	 	   ↳	android.widget.TableLayout.LayoutParams



 java.lang.Object
↳	android.view.ViewGroup.LayoutParams
  ↳	android.view.ViewGroup.MarginLayoutParams
	   ↳	android.widget.LinearLayout.LayoutParams
            ↳	android.widget.TableLayout.LayoutParams   layout_span, layout_column
```


+ TableLayout에서 중요속성(셀을 확장하거나, 합치는 속성)
  - 여분의 공간이 있으면 몇번째 셀이 나머지공간을 차지하라는 설정
    - android:stretchColumns="0,1,2"
  - 공간이 부족할때 특정컬럼을 자동으로 축소하라는 설정
    - android:shrinkColoumns
  - TableLayout내부의 layout속성
    - android:layout_span --------- 현재위젯의 폭을 확장
    - android:layout_column ------- 현재위젯을 몇번째 



#### 3. 실습
#### 4. Summary / Close



-----------------------------------------------------------


### [2019-06-14]

#### 1. Review

#### 2. Activity 호출 (정보받기)
##### 액티비티 호출시 정보를 전달하거나 작업결과를 되돌려 받는 방법
1. 액티비티 호출시 정보전달
[호출하는 액티비티]
   - Intent 생성
     - Intent intent = new Intent(this, AgumentActivity.class);

   - Intent 객체에 정보 저장 ==> putExtra("키",값);
     - intent.putExtra("title", "기생충");
     - intent.putExtra("poster", R.drawable.prasite);
     - intent.putExtra("actor", "봉준호");
     - intent.putExtra("director", "송강호");

   - 액티비티 호출
     - startActivity(intent);


[호출하는 액티비티] - ArgumentActivity
   - OnCreate(), onResum()메서드에서 정보 얻기
     - Intent intent = getIntent();
   
   - 전달된 인자정보를 intent객체에 getXXXExtra(키,[디폴트값])
     - title = intent.getStringExtra("title");
     - posterId = intent.getIntExtra("poster",-1);
     - actor = intent.getStringExtra("actor");
     - director = intent.getStringExtra("director");
   
   - 인자값을 목적에 맞게 사용




1. 액티비티에 작업한 결과내용을 호출한 액티비티에서 사용하기
```
        Sub4Activity  ------------------------->  InputActivity
                                                 성명, 나이, 점수 입력
성명, 나이, 점수 사용  <------------------------
``` 
```
    1) 액티비티 호출시
        - startActivityForResult(Intent객체);
    2) InputActivity에서 작업후 자신이 
        - Intent객체를 생성하여 정보를 저장
        - setResult(Intent객체, 결과코드)
        - finish();
    3) @Overrride
       onActivityResult(int 요청코드, int 결과코드, Intent 객체) {
           .... Intent객체로부터 정보 추출 사용           
       }
```


+ 위의 액티비티 호출관련 내용은.... 교재 246 ~ 260 Page 
+ Inttent에 대한 상세 설명......... 교재 280 Page ~





#### 3. FrameLayout : 내부구성요소들을 겹쳐서 보여준다.
+ 그대로 사용할때는 의미가 없다.
    - 내부 구성요소의 Visibility 속성을 사용하여 1개의 구성요소만 보이게하고
    - 나머지 구성요소는 보이지 않도록 하는 방법으로 응용한다.
```
java.lang.Object
   ↳	android.view.View
 	   ↳	android.view.ViewGroup
 	 	   ↳	android.widget.FrameLayout


java.lang.Object
   ↳	android.view.ViewGroup.LayoutParams              layout_width, layout_height
 	   ↳	android.view.ViewGroup.MarginLayoutParams    layout_margin,....
 	 	   ↳	android.widget.FrameLayout.LayoutParams  layout_gravity

```


+ 참고) 현재의 Project에 사용자가 추가한 이미지는
    - R.drawable.이미지식별자
    - 안드로이드 플랫폼 자체에서 내장하여 제공하는 자원(Resource)
        - android.R.자원종류.식별자
        - 자바코드에서 --> android.R.drawable.이미지식별자
        - res 내부에서 --> @android:drawable/식별자
    - 실제로 존재하는 장소는?
        - C:\Users\사용자명\AppData\Local\Android\res





#### 4. Layout전개
+ Inflation(전개) : xml로 화면의 일부내용만을 디자인 해놓고, 프로그램이 작동중에 화면내용이 필요한 시점에서 xml를 전개(inflation)시켜 현재화면에 반영하여 사용하는 것.
    - 원래는 Java Code로 UI객체를 생성, 속성설정, 화면연결(Activity연결)해서 사용.
    - 이는 너무 복잡하기때문에 손쉽게 xml로 미리 만들어 놓았다가 사용하기위함.


+ Inflation은 LinearLayout일때가 가장 사용하기 손쉽다.





#### 5. 실습
#### 6. Summary / Close




-----------------------------------------------------------


### [2019-06-17]

#### 1. Review

#### 2. 토스트와 대화상자
1. 토스트(toast) : 간단한 알림메세지
    - 잠시 나타났다가 사라진다.
    - 기본 모양과 위치가 정해져 있다.
    - 모양과 위치 변경이 가능하다.


2. 외부라이브러리로 스낵바를 사용하여 기능을 대체할 수 있다.
    - 디자인 라이브러리를 프로젝트에 추가해야 한다.



+ BookToastActivity.java
```java
package com.jica.dialogactionbar;

import android.content.Context;
import android.os.Bundle;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.view.Gravity;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class BookToastActivity extends AppCompatActivity {
    EditText editText;
    EditText editText2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        //UI xml 전개
        setContentView(R.layout.activity_book_toast);

        //UI 객체얻기
        editText = findViewById(R.id.editText);
        editText2 = findViewById(R.id.editText2);
    }

    public void onButtonTosat(View view) {
        Toast.makeText(this,"기본 토스트입니다.",Toast.LENGTH_SHORT).show();
        //기본토스트에 내부적으로 설정된 위치값
        //setGravity(Gravity.CENTER_HORIZONTAL|Gravity.BOTTOM, 0, -100);
    }

    public void onButton1Clicked(View v) {
        try {
            Toast toastView = Toast.makeText(this, "위치가 바뀐 토스트 메시지입니다.", Toast.LENGTH_LONG);

            //x,y 위치값을 입력하지 않으면 아래 코드에서 예외가 발생하므로 아무일도 하지 않는다.
            int xOffset = Integer.parseInt(editText.getText().toString());
            int yOffset = Integer.parseInt(editText2.getText().toString());

            //토스트의 위치를 변경시킨다.
            toastView.setGravity(Gravity.TOP | Gravity.TOP, xOffset, yOffset);

            toastView.show();
        } catch (NumberFormatException e) {
            e.printStackTrace();
        }
    }

    public void onButton2Clicked(View v) {
        //LayoutInflater 구하기
//        LayoutInflater layoutInflater
//                = (LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        LayoutInflater inflater = getLayoutInflater();

        //방법2
        View layout = inflater.inflate(
                R.layout.toastborder,
                (ViewGroup) findViewById(R.id.toast_layout_root));

        TextView text = layout.findViewById(R.id.text);

        //시스템 내부의 토스트 객체를 사용하지 않고 직접 Toast생성
        Toast toast = new Toast(this);
        text.setText("모양 바꾼 토스트");   //토스트에 의해 보여질 내용
        toast.setView(layout);           //토스트와 뷰 연결

        //토스트의 위치설정정
       toast.setGravity(Gravity.CENTER, 0, -100);
        toast.setDuration(Toast.LENGTH_SHORT);

        toast.show();

    }

    public void onButton3Clicked(View v) {
        Snackbar.make(v, "스낵바입니다.", Snackbar.LENGTH_LONG).show();
    }
}
```

```
토스트나 스낵바는 잠시 나왔다가 사라진다. 즉 사용자의 확인을 100% 보장할 수 없다.
중요한 정보에 대한 사용자의 확인이나 추가적인 데이터의 입력이 필요할때, 대화상자(Dialog)를 사용한다.

일반적인 사용법은 Java AWT의 Dialog와 비슷하지만(모달대화상자),
기본적인 작동구조가 Modalless이다.
```


+ 대화상자 
    - 아이콘 + 제목 + 메세지 내용 + 버튼

+ Android Dialog는...
    - 제목 + 메세지 내용만 제공.... 거의 쓸모가 없다.
    
+ Android AlertDialog를 사용한다.
    - 직접생성
    - AlertDialog.Builder로 생성(권장)
    - 버튼은 기본 3가지 제공
        1. 기본버튼
        2. 긍정버튼
        3. 부정버튼




+ 목록대화상자
    - setMassage(String)에 의한 문자내용을 확인
    
    - setItems(목록)를 사용하여 목록에서 항목을 선택하게 한다.
    - setSingleItems()
    - setMultiItems() 를 사용하여 목록에서 항목을 선택하게 한다.
    
    - setView(사용자화면) -- 사용자가 선택하거나 입력하도록 자유롭게 화면설계.




+ AlertDialogActivity.java
```java
```




#### 5. 기본위젯 사용법과 Event처리

#### 3. Event처리

#### 4. 실습
#### 5. Summary / Close





-----------------------------------------------------------


### [2019-06-18]

#### 1. Review

#### 2. 기본위젯과 배치관리자

#### 5. 기본위젯 사용법과 Event처리

#### 3. Event처리

#### 4. 실습
#### 5. Summary / Close




-----------------------------------------------------------


### [2019-06-19]

#### 1. Review

#### 2. 기본위젯과 배치관리자

#### 5. 기본위젯 사용법과 Event처리

#### 3. Event처리

#### 4. 실습
#### 5. Summary / Close




-----------------------------------------------------------


### [2019-06-20]

#### 1. Review

#### 2. 기본위젯과 배치관리자

#### 5. 기본위젯 사용법과 Event처리

#### 3. Event처리

#### 4. 실습
#### 5. Summary / Close