# OSI 7 layers

## 역사

네트워크의 연결 → 각기 다른 역할을 담당하는 기능/장비들이 어떠한 절차와 복잡한 규약을 바탕으로 한 논리 구조 위에서 이루어진다. 

이러한 기능과 절차를 국제기구인 International Organization for Standard (ISO / 국제표준화기구)에서 1983년도에 OSI(Open Systems Interconnection) 7계층으로 표준화하여 정리

## OSI 7 계층

: 통신이 일어나는 과정을 7단계로 정의한 국제 통신 표준 규약

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1350b1d2-2534-4161-963d-de80702d6a77/68100a8f-0a21-417d-927b-1507674927bf/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1350b1d2-2534-4161-963d-de80702d6a77/2865926b-2754-412c-b9bd-3f89e1070630/Untitled.png)

### 1. 물리 계층 (Physical Layer)

전기, 물리 신호에 대한 계층. 

단순하게 장비를 가동시키기 위한 전기 공급부터 장비끼리의 물리적인 연결을 위한 랜케이블, 그리고 무선 통신을 위한 주파수까지 다양한 전기적/물리적인 것들을 모두 포함. 

데이터를 전달하기만 함 → 어떤 에러가 있든 전혀 신경쓰지 않음

단위: ‘Bit’

### 2. 데이터 링크 계층 (Data Link Layer)

물리계층으로 송/수신되는 정보를 확인하고 오류없는 통신을 위해 여러 역할을 수행한다.

Mac 주소를 통해 통신한다. 프레임에 Mac 주소를 부여하고 에러 검출, 재전송, 흐름 제어를 진행한다. (물리 주소 : 네트워크에서 실제로 사용되는 하드웨어 장치의 고유한 주소)

단위 : ‘Frame’

### 3. 네트워크 계층 (Network Layer)

데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능(라우팅)을 담당.

- 각 단말을 구분하도록 IP 주소를 할당하는 논리 주소 (Logical Address)기능
- 복잡하고 다양한 경로들 중 가장 최적의 경로를 찾기 위한 경로 설정 (Path Determination) 기능

→ 해당 경로에 따라 패킷을 전달한다.

단위 : Packet

### 4. 전송 계층 (Transport Layer)

각 종단 간의 데이터 전송에 대한 전반적인 조율을 담당하는 계층.( ex> 통신의 신뢰성을 보장)

- 쪼개진 데이터 유닛인 ‘세그먼트(Segment)’를 포트 번호 등으로 구분하여 각 응용 계층이 나눠 받도록 하는 분할(Segmentation)작업
- TCP, UDP

단위 : Segment

### 5. 세션 계층 (Session Layer)

데이터가 서로 만나는 환경을 조성해주는 단계(Session ManageMent).

- 통신 시스템 사용자 간의 연결을 유지 및 설정
- API, Socket

### 6. 표현 계층 (Presentation Layer)

데이터를 더 빠르고 안전하게 전송하기 위한 압축 및 암호화/복호화 작업을 하는 단계.

- 데이터를 압축하여 크기를 줄이면 전송 속도가 빨라짐.
- 누군가 데이터를 중간에 가로챌 수 있어 암호화/복호화 작업이 필요함.
- JPEG, MPEG

### 7. 응용 계층 (Application Layer)

도착한 데이터를 최종 사용자가 확인하는 마지막 단계. 

- 브라우저, 메일 등 네트워크를 활용하는 다양한 응용 프로그램
- HTTP, HTTPS

# TCP 3-way Handshake & 4-way Handshake

# TCP 3-way Handshake

> TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 ‘정확한 전송’을 보장하기 위해서 거치는 과정 (대상 서버가 패킷을 받을 수 있는 상태인지 모름)
> 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1350b1d2-2534-4161-963d-de80702d6a77/b56cc64e-c888-40c3-851f-3882f73b9a09/Untitled.png)

1. 클라이언트가 서버에 SYN 을 보낸다.

→ 클라이언트의 요청 보냄

1. 서버가 클라이언트에게 SYN + ACK 을 보낸다.

→ 클라이언트의 요청 수락, 서버 요청 보냄

1. 클라이언트가 서버에게 ACK 을 보낸다. → 연결완료

→ 서버의 요청 수락 

> SYN : 접속요청 |  ACK : 요청 수락
> 
- 이후, 데이터 전송 (3번이랑 같이 진행되기도 한다.)

# TCP 4-way Handshake

> 세션을 종료하기 위해 수행되는 절차.
> 

→ 계속해서 서버와 클라이언트의 연결을 유지하게 되면 서버의 자원이 소모된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1350b1d2-2534-4161-963d-de80702d6a77/06c548de-450f-41e5-8360-5c7cf581ca87/Untitled.png)

1. 클라이언트가 연결을 종료하겠다는 FIN 플래그를 전송한다.
2. 서버는 ACK 패킷을 보내고 연결을 종료할 때까지 TIME_WAIT 상태가 된다.
3. 서버의 통신이 끝나면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송한다.
4. 클라이언트는 ACK를 보냄.

> "Server에서 FIN을 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN패킷보다 늦게 도착하는 상황"이 발생한다면?
> 

→ Client에서 세션을 종료시킨 후 뒤늦게 도착하는 패킷이 있다면 이 패킷은 Drop되고 데이터는 유실될 것입니다. 이러한 현상에 대비하여 Client는 Server로부터 FIN을 수신하더라도 일정시간(디폴트 240초) 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거치게 되는데 이 과정을 "TIME_WAIT" 라고 함.
