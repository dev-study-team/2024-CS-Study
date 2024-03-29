# OSI-7 Layer

## 1. OSI 7 계층 개념
OSI 7 계층(OSI 7 Layer)은 'Open System Interconnection'의 참조 모델로, 1984년 국제 표준화 기구인 ISO에서 발표하였다. 통신 기술의 도입 및 기능 확장의 편의를 위해 프로토콜을 7 계층으로 나눴다. 이렇게 통신 기능을 계층화하여 프로토콜을 규정한 규격을 OSI 모델이라고 한다. 쉽게 말해 우리가 인터넷을 사용하는 등 네트워크간 통신이 발생할 때의 과정을 7계층으로 분류한 것이다. OSI 모델은 프로토콜이 아니라 유연하며 안전하고, 상호 연동이 가능한 네트워크 구조를 설계하고 이해할 수 있다.
 

OSI 7 계층은 말 그대로 각각 특정 기능을 수행하는 서로 다른 7단계의 흐름을 나타낸다. 각 계층은 '헤더(Header)'와 '데이터 단위(Data Unit / Protocol Data Unit)'으로 정의되며, 헤더에는 계층별 기능과 관련 정보가 포함된다. 만약 송신 측에서 헤더를 생성하여 추가하면, 수신 측에선 해당 계층이 헤더를 사용한다.
 

상위 계층이나 하위 계층 사이에 주고받는 것은 'Service Data Unit(SDU)', 같은 계층 사이에서 주고받는 것을 'Protocol Data Unit(PDU)'라고 한다. 즉, 해당 모델에서 데이터는 같은 계층 혹은 위 아래 계층으로만 순차적으로 이동할 수 있다. 이렇게 계층 구조를 사용하는 목적은 먼저 흐름이 직관적이며, 특정 구간에서 문제가 발생하였을 때 쉽게 파악할 수 있기 때문이다. 물리 계층과 응용 계층을 제외한 나머지 계층에서는 데이터의 시작과 끝 부분에 헤더나 트레일러 형태로 정보를 추가한다.
<br><br>

![image](https://github.com/junseoparkk/til/assets/98972385/e09c651e-54d9-460c-b846-2220a1b8e9a1)
<br>

![image](https://github.com/junseoparkk/til/assets/98972385/507b08c2-106d-47f8-a1ea-63ddb362d253)
<br>

![image](https://github.com/junseoparkk/til/assets/98972385/be4f2f79-0781-495f-9a0f-e7d384d62248)
<br><br>

위 그림처럼 각 계층별로 사용되는 프로토콜이 구분되어있으며, 특정 기능을 수행한다. 아래 계층으로 구성된다.

1. 물리 계층 : Physical Layer   (1계층)
2. 데이터 링크 계층 : Data Link Layer (2계층)
3. 네트워크 계층 : Network Layer (3계층)
4. 전송 계층 : Transpost Layer (4계층)
5. 세션 계층 : Session Layer (5계층)
6. 표현 계층 : Presentation Layer (6계층)
7. 응용 계층 : Application Layer (7계층)
<br><br>

실제 네트워크 프로토콜은 7계층을 모두 사용하지 않고, 처음 1~3계층만 사용한다. 이렇게 망과 접속하는 계층을 '하위 계층', 나머지 사용자와 접속하는 4~7계층을 '상위 계층'이라고 한다. 이들 모두 서로 독립적이므로 특정 계층의 변경이 다른 계층에는 영향을 미치지 않기 때문에 기능에 필요한 몇 개의 계층만 표준화하면 정상적인 통신이 가능하다.
<br><br>

## 2. OSI 7 계층

### 1. 물리 계층 (Physical Layer) : 1계층

![image](https://github.com/junseoparkk/til/assets/98972385/2b60ce53-4a7d-47d5-ad39-abb147a8cbb6)
<br>

#### 1) 특성
물리 계층은 OSI 7 계층 중 최하위 계층인 첫 번째 계층으로, 상위 계층으로부터 전달 받은 데이터를 물리매체를 통해 다른 시스템에 전기적 신호를 전송한다. 즉, 두 시스템 간 데이터 통신을 위해 링크를 활성화하고 관리하는 전기적, 기계적, 절차적, 기능적 특성 등을 정의한다. 특히 허브, 라우터, 네트워크 카드, 케이블 등의 전송매체를 통해 비트(bit)로 데이터를 전송한다.
<br>

- 기계적 특성 : 시스템과 주변장치를 연결하기 위해 정의
- 전기적 특성 : 두 시스템간 상호 접속 회로의 전기적 특성 등을 정의
- 기능적 특성 : 상호 접속 회로의 기능을 정의
- 절차적 특성 : 데이터 전송에 필요한 순서를 정의
<br><br>

#### 2) 데이터 단위
송신 측의 물리 계층은 데이터 링크 계층에서 0과 1로 구성된 비트열의 데이터(프레임)을 받아 전기적 신호로 변환, 전송매체를 통해 전송한다. '비트(bit)' 라고 불리는 비트열의 데이터가 물리 계층의 데이터 단위이다. 수신 측의 물리 계층은 통신을 통해 전달 받은 전기 신호를 0과 1로 구성된 비트열로 복원하여 데이터 링크 계층으로 전송한다.
<br><br>

#### 3) 네트워크 접속 장치
1. 리피터(Repeater) : 신호의 세기가 약해질 때 전기 신호를 복원하고 증폭. 멀리 떨어진 상대와도 통신할 수 있도록 파형을 정상적으로 만드는 기능을 한다. 일대일 통신만 가능하다.
2. 허브(Hub) : 실제로 통신하는 통로인 포트가 여러 개이다. 따라서 여러 컴퓨터와도 통신할 수 있다.

---

### 2. 데이터 링크 계층 (Data Linke Layer) : 2계층
![image](https://github.com/junseoparkk/til/assets/98972385/ceccfbfc-c98a-4a1f-8faa-61bcaf9ac554)
<br>

#### 1) 특성
데이터 링크 계층은 물리적 링크를 이용해 신뢰성 있는 데이터를 전송하는 계층으로, 네트워크 통신에서 전송로 역할을 한다. OSI 7 계층 중 하위 계층인 두 번째 계층이다. 시스템간 오류 없이 데이터를 전송하려고 네트워크 계층으로부터 전달 받은 데이터 단위를 프레임으로 구성하여 물리 계층으로 전송한다.
<br><br>

#### 2) 데이터 단위
데이터 링크 계층의 데이터 단위는 '프레임(Frame)'이며, 비트의 논리적 단위로 구성된다. 여기엔 전송 데이터에 인접하는 노드의 주소가 더해진다. 이는 최종 수신지가 아닌 다음으로 전송되는 노드의 주소가 된다. 즉, 송신 측의 경우 네트워크 계층으로부터 패킷을 받아 인접한 노드 주소를 더해 프레임을 생성하여 물리 계층으로 전달된다. 프레임은 데이터 + 헤더 + 트레일러로 구성된다.
<br><br>

#### 3) 기능
1. 주소 지정 : 가장 최근에 데이터가 지나온 노드와 다음에 접근할 노드의 물리 주소를 지정한다.
2. 순서 제어 : 데이터를 순차적으로 전송하기 위해 프레임 번호를 부여한다.
3. 흐름 제어 : 한 번에 전송할 수 있는 데이터 양을 조절하고, 연속으로 프레임을 전송할 때 수신 여부를 확인한다.
4. 오류 처리 : 오류 검출과 정정 기능을 수행, 오류가 발생한 프레임의 재전송을 요구할 수 있다.
5. 이외 : 동기화, 데이터 링크 설정 등
<br><br>

#### 4) 규칙
데이터 링크 계층은 네트워크 접속 장치간 신호를 주고받는 규칙을 정하는 계층이다. 그 중 가장 일반적으로 사용되는 규칙은 '이더넷(Ethernet)'으로, 허브와 같은 네트워크 접속 장치에 연결된 컴퓨터와 데이터를 통신할 때 사용한다.

이더넷 헤더의 구조는 다음과 같으며, 총 14byte로 구성된다.

- 수신지 MAC 주소 (6byte)
- 송신지 MAC 주소 (6byte)
- 유형 (2byte)
<br><br>

또한 'FCS(Frame Check Sequence)'라는 트레일러는 데이터 송신 도중 오류 발생 여부를 확인하기 위해 사용된다. 이러한 이더넷 헤더와 트레일러가 추가된 데이터를 프레임이라고 한다.

 전송 규칙으로는 데이터가 동시에 케이블을 지날 때 충돌이 발생하지 않도록 'CSMA/CD' 방식을 사용한다. 여기서 'CS'는 데이터를 전송하려는 컴퓨터가 케이블에 데이터 신호가 흐르는지 확인하는 규칙, 'MA'는 데이터 신호가 흐르지 않는다면 데이터를 전송해도 되는 규칙이다. 마지막으로 'CD'는 충돌이 발생하는지 확인하는 규칙이다.

 ---

 ### 3. 네트워크 계층 (Network Layer) : 3계층

![image](https://github.com/junseoparkk/til/assets/98972385/3f2f625e-43de-45ab-88cb-7dbb8020249a)
<br><br>

#### 1) 특성

네트워크 계층은 데이터 링크 기능을 이용해 경로 배정, 중계, 흐름 제어, 오류 제어 등을 수행한다. 네트워크 연결의 설정, 유지 및 종료 기능을 수행하며, 물리 계층과 데이터 링크 계층과는 달리 종단간의 개념이 적용된다. 라우팅 알고리즘을 사용하여 데이터 전송시 최적의 경로를 선택한다. 전송되는 데이터는 '패킷(Packet)' 단위로 분할되어 데이터 링크 계층으로 전달된다. 
<br><br>

#### 2) 기능
라우팅(Routing) 기능을 담당한다. 이는 특정 네트워크 내에서 통신 데이터를 다양한 라우팅 알고리즘 중 가장 빨리 보낼 수 있는 최적의 경로를 선택하는 과정을 말한다. 목적지 네트워크 주소인 'IP'를 지정하여 선택된 경로를 따라 패킷을 전달해준다. 또한 다양한 길이의 데이터들을 네트워크를 통해 전달하고, 그 과정에서 전송 계층이 요구하는 서비스 품질을 제공하기 위해 기능적, 절차적 수단을 제공한다.
<br><br>

#### 3) 서비스
1. 연결형 네트워크 서비스 (ISO 8348) : 논리적 데이터 통신 회선을 먼저 설정한 뒤 데이터를 전송한다. 전송이 끝나면 회선을 해제한다. (연결 설정 -> 데이터 전송 -> 연결 종료). 대량의 데이터를 연속 전송할 때 효과적이다.
2. 비연결형 네트워크 서비스 : 논리적 데이터 통신 회선을 설정하지 않고 전송 단위인 PDU를 전송한다. 네트워크 연결 설정을 하지 않으므로 데이터 분실, 중계 지연 등에 대한 제어 기능을 갖지 않는다. 다른 통신 네트워크와 상호 연결하기 쉽다는 장점이 있고, 경우에 따라 네트워크 기능을 간소화할 수 있다.

---

### 4. 전송 계층 (Transport Layer) : 4계층

![image](https://github.com/junseoparkk/til/assets/98972385/ff257487-c2ae-467c-bbaf-d73a819f9850)
<br><br>

#### 1) 특성
하부 네트워크와 독립적으로 신뢰성 있는 프로세스 상호 간의 메시지 전달 기능을 제공한다. 흐름 제어, 오류 제어, 메시지 전달 등의 기능을 수행하며 연결형(주로 TCP)과 비연결형(주로 UDP)의 두 가지 운용 모드를 지원한다.

하위 계층을 구성하는 다양한 데이터 통신 네트워크의 품질 차이를 보상한다. 또한 종단 프로세스 간의 데이터 전송을 보장한다. (End-to-End). 네트워크 계층이 제공하는 서비스 품질에 따라 5가지 등급으로 프로토콜을 규정한다.

- 등급 0 : 단순 등급
- 등급 1 : 기본 오류 복원 등급
- 등급 2 : 다중화 등급
- 등급 3 : 오류 복원 및 다중화 등급
- 등급 4 : 오류 검출과 복원 등급
<br><br>

#### 2) TCP vs UDP
||TCP|UDP|
|:---:|:---:|:---:|
|신뢰성|높음|낮음|
|속도|느림|빠름|
|전송 방법|순서대로 전달됨|순서대로 전달되지 않음|
|오류 감지 및 수정|있음|없음|
|혼잡도 제어|있음|없음|
|전송 인정|있음|오직 체크섬만|

---

### 5. 세션 계층 (Session Layer) : 5계층

#### 1) 특성
세션 계층은 서로 다른 컴퓨터에서 동작되고 있는 두 개의 응용 계층 프로토콜 개체가 데이터를 전송하는 데에 필요한 대화를 관리하고 조정한다. 순서에 따라 데이터를 조합하고 동기화하는 수단과 응용 계층 프로토콜 개체 간에 대화 채널을 설정, 해제하는 수단을 제공한다. 

즉, 통신 세션을 구성하는 계층이며 포트 번호를 기반으로 연결된다. TCP/IP 세션을 만들고 없애는 역할을 수행하며, 네트워크 통신 연결을 관리하고 연결을 지속시켜주는 계층이다.
<br><br>

#### 2) 세션
'세션(Session)'은 웹 사이트의 여러 페이지에 걸쳐 사용되는 사용자 정보를 저장하는 방법이다. 즉, 사용자가 브라우저를 닫아 서버와의 연결을 끝내는 시점까지를 세션이라고 한다. 

쿠키(Cookie)와는 달리 세션은 서비스가 돌아가는 서버 측에 데이터를 저장하고, 세션의 키값만 클라이언트 측에 남긴다. 반면 쿠키는 모든 데이터가 클라이언트 측에 저장된다. 따라서 세션은 보안에 취약한 쿠키를 보완하는 역할을 해준다.
<br><br>

---

### 6. 표현 계층 (Presentation Layer) : 6계층
표현 계층은 송수신 측 사이의 데이터 형식을 정해준다. 두 사용자 응용 프로게스 간에 교환될 데이터의 형식과 관련되어 있으다. 즉, 사용자 데이터 전송을 위해 상호 동의하고 이해하는 형식으로 협상할 수 있는 수단을 제공한다. 예를 들어 jpeg, jpg, png 등의 이미지 형식의 변환, 구문 검색, 인코딩 및 디코딩, 암호화, 압축 등의 과정이 있다. 

---

### 7. 응용 계층 (Application Layer) : 7계층

![image](https://github.com/junseoparkk/til/assets/98972385/e3fc36a9-dbeb-4cda-a169-08fa43f05905)
<br><br>

#### 1) 특성

OSI 7 계층 중 최상위 계층으로, 자원 결정, 구문 확인 등의 기능과 정보 처리를 수행하는 응용 프로그램과 프로세스 간의 인터페이스를 제공한다. 또한 데이터 통신을 수행하기 위한 기본적인 응용 기능을 제공한다. 실제로 사용자가 사용하는 프로그램에 해당되며, 사용자로부터 데이터를 받아 하위 계층으로 전달 또는 하위 계층의 데이터를 사용자에게 전달한다.

예를 들어 이메일, 파일 전송, DB 접근 등의 응용 서비스를 네트워크에 접속하여 사용자와 상호작용한다.
<br><br>

#### 2) 구성 요소
- AE : 응용 프로그램에서 OSI에 관련된 기능
- UE : 응용 프로그램이 데이터 통신을 수행하기 위해 응용 서비스를 이용하는 요소
- CASE : 응용 계층 내에서 공통으로 사용되는 서비스 요소로, 공통 기능으로 모으기 위해 연계 제어, 문맥 제어, 정보 전송, CCR 제어 등을 수행
- SASE : 특정 기능에 사용되는 서비스 요소로, 가상 단말기, 파일 전송 액세스 관리 작업, 전송 조작 등이 있다.
<br><br>


## 3. OSI 7 계층 요약
|계층(Layer)|이름(Name)|단위(PDU)|예시(Example)|프로토콜(Protocol|
|:---:|:---:|:---:|:---:|:---:|
|7|응용 계층(Application Layer)|Data|Telnet, Google Chrome, E-mail, Database Management|HTTP, SMTP, SSH, FTP, Telnet, DNS, SIP 등|
|6|표현 계층(Presentation Layer)|Data|인코딩, 디코딩, 암호화, 복호화|ASCII, MPEG, JPEG, MIDI, AFP, XDR 등|
|5|세션 계층(Session Layer)|Data|-|SSL, TLS, NetBIOS, SAP, SDP, DLC 등|
|4|전송 계층(Transport Layer)|TCP-Segment, UDP-Segment|특정 방화벽 및 프록시 서버|TCP, UDP, SCTP, RTP, ATP, OSPF 등|
|3|네트워크 계층(Network Layer)|Packet|라우터|IP, IPsec, ICMP, ARP, RIP, BGP, DDP, PLP 등|
|2|데이터링크 계층(DataLink Layer)|Frame|MAC  주소, 브릿지 및 스위치|Ethernet, Token Ring, MAC, HDLC, PPP, LLC 등|
|1|물리 계층(Physical Layer)|Bit|전압, 허브, 네트워크 어댑터, 중계기 및 케이블 사양, 신호 변경|10BASE-T, 100BASE-TX, ISDN, wired, wireless등|

<br><br><br>

##### 블로그 
https://jnsodevelop.tistory.com/74
<br><br><br>

###### 참고
https://backendcode.tistory.com/167<br>
https://m.hanbit.co.kr/store/books/book_view.html?p_code=B7721595096<br>
https://tcpschool.com/php/php_cookieSession_session
