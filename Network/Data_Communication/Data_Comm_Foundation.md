## [데이터 통신 기초 1]

+ Analog VS Digital
아날로그 : 손실이 있고 활용도가 떨어진다.
디지털 : 정보손실이 적고 정보의 암호화와 활용도가 높다.

+ 직렬전송 VS 병렬전송
직렬전송이 골목길이라면, 별렬전송은 고속도로!
그러나 직렬전송이 고속화에 유리하다.

+ 동기식전송 VS 비동기식전송
동기식전송 : 고속통신에 유리
비동기식 전송 : 하드웨어 비용을 절감할 수 있다.


+ 단방향(simplex) 전송
+ 반이중(half duplex) 전송
+ 전이중(full duplex) 전송

+ 회선 접속 방식에 따른 분류
    + 점대점방식
    + 다지점 방식

+ 망구성 범위에 따른 분류
    - LAN 근거리 통신망
    - MAN 도시지역 통신망
    - WAN 광역 통신망


+ OSI 7-Layer 모델
    1. Application
    2. Presentation
    3. Session
    4. Transport
    5. Network
    6. Data Link
    7. Physical


------------------------------------------------------------------




## [데이터 통신 기초 2]
+ 매체
    - Coaxial(동축)
    - UTP(LAN)
    - Fibre Optic(광)
    - Radio(무선)

+ 리피터(Repeaters)
    - 동일 LAN상에서 거리르 연장하거나 접속 시스템의 수를 증가시키기 위해 사용
    - 신호의 잡음을 감쇄하는 효과

+ 허브(Hubs)
    - 더미 허브(dummy hub) : 동일매체이용.... 중계만 담당
    - 스위칭 허브(switching hub) : 연결 장치간 스위칭 역할...(Layer2에서 동작...L2)

+ 브릿지(Bridges)
    - 복수의 LAN을 서로 연결하기 위함. Layer2에서 동작

+ 라우터(Routers)
    - 인터넷에 상이하게 구축된 IP네트워크 사이를 연결...(Layer3에서 동작...L3)

+ 이더넷(Ethernet)
    - IEEE 802.3에 규정된 데이터 네트워크 구성.
    - 매체 활용 규격.
    - MAC주소를 통해 충돌을 감지한다.

+ 와이파이(Wi-Fi)
    - IEEE 802.11에 규정된 매체 활용 규격.


------------------------------------------------------------------




## [데이터 통신 기초 3]


+ 데이터 링크 계층
    - LLC (Logical Link Control) 계층 : 오류제어, 흐름제어
    - MAC (Medium Access Control) 게층 : 다중 접근 제어


+ 이더넷 프레임(Ethernet Frames)
+ 이더넷의 다중 접근 제어 : CSMA/CD (Carrier-Sense Multiple Access with Collision Detection)
    1. 노드(또는 호스트)는 데이터 전송 이전에 회선이 사용 중인지 점검
    2. 회선이 사용 중이면 임의의 시간만큼 기다린 뒤 다시 시도
    3. 회선이 사용 중이 아님이 확인되면 데이터 전송 시작
    4. 데이터 전송 중 충돌이 검출되면 충돌 발생 사실을 모든 노드에게 통보
    5. 충돌이 발생하면 임의의 시간동안 대기한 후 다시 시도


+ 무선 랜의 다중 접근 제어 : CSMA/CA (Carrier-Sense Multiple Access with Collision Avoidance)
    - RTS(request to send)와 CTS(clear to send)를 이용하여 어느 순간이든 허락된 단일 노드 쌍에만 데이터 전송 (매체 이용)이 이루어지도록 함


+ 슬라이딩 윈도우 프로토콜(Sliding Window Protocol)
    - 송신측에서는 매 프레임에 순차 번호(sequence number)를 매김
    - 순차 번호 및 오류 검출 코드 등을 프레임에 표기하고 순서대로 전송
    - 수신측에서는 매 프레임에 대한 응답(ACK)프레임으로 회신
    - 슬라이딩 윈도우 : 송신측에서 응답을 받지 않고도 연속하여 전송할 수 있는 연속한 프레임 갯수에 대한 제한

+ 재전송 방식의 종류
    - 슬라이딩 윈도우의 크기가 1이면?......Stop-and-Go
    - Go-Back-N 방식......오류가 발생한 프레임뿐만 아니라, 그 이후의 모든 프레임 재전송
    - Selective Retransmission 방식.......오류가 발생한 프레임만 선택적으로 재전송하여 복구



+ 오류검출(Error Detection)
    - 패리티 비트(Parity Bits) : 가장 간단한 형태의 오류 검출 코드(1비트의 에러만 검출-홀수)
    - 체크섬(Checksum)
------------------------------------------------------------------