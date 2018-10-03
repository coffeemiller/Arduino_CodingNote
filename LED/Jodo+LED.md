/*
 * 작성자 : coffeemiller
 * 작성일 : 2018-10-02
 * 내  용 : CDS 조도센서 읽어서, LED작동하기
 *
 * 1. 필요부품 : 아두이노 UNO, CDS 센서, 삼색 LED  <<<<<<<<<<<<< 변수
 * 2. 필요작업 : 조도센서가 빛의 세기를 인식하여,변수에 저장한다.
 *               저장된 값을 통해, 값이 "0"이면 LED를 끈다.
 *               값이 "10-100"이면 R  
 *               값이 "100-300"이면 G
 *               값이 "300-"이면 B  <<<<<<<< 함수
 * 3. 부가작업 : 추가로... 빨간색 LED가 3번 작동하면, 완전히 종료됨.
 */


//정의부 영역============================ START
int cds = 0;   //cds 센서 핀 설정
int val = 0;    //센서 값을 닮을 그릇변수
const int RED_PIN = 10;      //빨간색 핀번호를 10번으로 정의한다. - 상수(const)선언
const int GREEN_PIN = 11;    //녹색 핀번호를 11번으로 정의한다. - 상수(const)선언
const int BLUE_PIN = 9;      //파란색 핀번호를 9번으로 정의한다. - 상수(const)선언
int R_co = 0;                 //빨간색 변수선언
//정의부 영역============================ END



//셋팅 영역============================ STARTD
void setup() {
  // put your setup code here, to run once:
//각각의 핀번호를 OUTPUT설정한다.
 pinMode(RED_PIN, OUTPUT);   //10핀번호를 출력으로 설정한다.
 pinMode(GREEN_PIN, OUTPUT); //11핀번호를 출력으로 설정한다.
 pinMode(BLUE_PIN, OUTPUT);  //9핀번호를 출력으로 설정한다.
 Serial.begin(9600);    //PC에서 조도센서 값을 출력해주기위해 아두이노 보드와 PC의 통신속도 지정
}
//셋팅 영역============================ END



//반복 영역============================ START
void loop() {
  // put your main code here, to run repeatedly:

     digitalWrite(RED_PIN,HIGH);    //빨간색 LED를 끈다.
     digitalWrite(GREEN_PIN,HIGH); //녹색 LED를 끈다.
     digitalWrite(BLUE_PIN,HIGH);  //파란색 LED를 끈다.
     val= analogRead(cds);
     //Serial.print("CDS = ");
     //Serial.print(val);
     //Serial.print("\n");

     if(val == 0){
      digitalWrite(RED_PIN,HIGH);    //빨간색 LED를 끈다.
      digitalWrite(GREEN_PIN,HIGH); //녹색 LED를 끈다.
      digitalWrite(BLUE_PIN,HIGH);  //파란색 LED를 끈다.
      //Serial.print("CDS = ");
      //Serial.print(val);
      //Serial.print("\n");
     }


      if(val > 1 && val < 100){          //cds 값이 10-100 사이.       
       digitalWrite(RED_PIN,LOW);    //빨간색 LED를 켠다.
       digitalWrite(GREEN_PIN,HIGH); //녹색 LED를 끈다.
       digitalWrite(BLUE_PIN,HIGH);  //파란색 LED를 끈다.
       R_co = R_co + 1;


       Serial.print("R_CDS = ");
       Serial.print(val);
       Serial.print("\n");
       delay(2000);                   //2초간 대기.

     }

     if(val > 100 && val < 150){          //cds 값이 100-300 사이.
       digitalWrite(RED_PIN,HIGH);    //빨간색 LED를 끈다.
       digitalWrite(GREEN_PIN,LOW);   //녹색 LED를 켠다.
       digitalWrite(BLUE_PIN,HIGH);   //파란색 LED를 끈다.
       Serial.print("G_CDS = ");
       Serial.print(val);
       Serial.print("\n");
       delay(2000);                   //2초간 대기.
     }

     if(val > 150){                    //cds 값이 300이상.
       digitalWrite(RED_PIN,HIGH);    //빨간색 LED를 끈다.
       digitalWrite(GREEN_PIN,HIGH);  //녹색 LED를 끈다.
       digitalWrite(BLUE_PIN,LOW);    //파란색 LED를 켠다.
       Serial.print("B_CDS = ");
       Serial.print(val);
       Serial.print("\n");
       delay(2000);                   //2초간 대기.
     }

     Serial.print("R_co = ");
     Serial.print(R_co);
     Serial.print("\n");

     if(R_co>3){
      return;
     }

}
//반복 영역============================ END
