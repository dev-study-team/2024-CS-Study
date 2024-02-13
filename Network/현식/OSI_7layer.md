
# OSI 7계층이란
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/9c12d2bf-d320-487f-8ba2-41b221fcf1ac)

- OSI는 상이한 컴퓨터 시스템이 서로 통신할 수 있는 표준을 제공
- 통신 시스템을 7개의 추상적 계층으로 나눔
    - 각 계층간 상호 작동하는 방식을 정의
    - 각 계층은 다음 계층 위에 스택형태로 쌓임
    - 각 계층은 독립되어있어, 서로의 인접한 계층 간에만 의존

## 특징
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/93331ebd-f5c6-4939-b27c-5acaa1fde33d)


- 데이터 캡슐화
    - 하위 계층은 상위 계층으로부터 온 정보를 데이터로 취급
    - 자신의 계층 특성을 담은 제어정보(주소, 에러 제어 등)을 헤더화 시켜 붙이는 일련의 과정을 진행
    - PDU(Protocol Data Unit)은 프로토콜 데이터 단위로 각 계층마다 용어가 다름
- 현대 인터넷은 OSI 모델을 엄격하게 따르지 않음
    - 하지만 OSI 모델은 네트워크 문제 해결을 위해 유용하게 활용됨
    - 문제 발생시 문제의 원인을 특정 계층으로 분리하고 불필요한 작업을 피할 수 있도록 도움을 줌

# 요약
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/374f5c39-e879-4bb9-bc1a-a9cd19fb13f7)


| 계층 | 이름 | 단위(PDU) | 예시 | 프로토콜(Protocols) | 디바이스(Device) |
|------|------|---------|------|-----------------|---------------|
| 7 | 응용 계층 (Application Layer) | Data | 텔넷(Telnet), 구글 크롬, 이메일, 데이터베이스 관리 | HTTP, SMTP, SSH, FTP, Telnet, DNS, modbus, SIP, AFP, APPC, MAP | - |
| 6 | 표현 계층 (Presentation Layer) | Data | 인코딩, 디코딩, 암호화, 복호화 | ASCII, MPEG, JPEG, MIDI, EBCDIC, XDR, AFP, PAP | - |
| 5 | 세션 계층 (Session Layer) | Data | - | NetBIOS, SAP, SDP, PIPO, SSL, TLS, NWLink, ASP, ADSP, ZIP, DLC | - |
| 4 | 전송 계층 (Transport Layer) | TCP-Segment, UDP-datagram | 특정 방화벽 및 프록시 서버 | TCP, UDP, SPX, SCTP, NetBEUI, RTP, ATP, NBP, AEP, OSPF | 게이트웨이 |
| 3 | 네트워크 계층 (Network Layer) | Packet | 라우터 | IP, IPX, IPsec, ICMP, ARP, NetBEUI, RIP, BGP, DDP, PLP | 라우터 |
| 2 | 데이터링크 계층 (DataLink Layer) | Frame | MAC 주소, 브리지 및 스위치 | Ethernet, Token Ring, AppleTalk, PPP, ATM, MAC, HDLC, FDDI, LLC, ALOHA | 브릿지, 스위치 |
| 1 | 물리 계층 (Physical Layer) | Bit | 전압, 허브, 네트워크 어댑터, 중계기 및 케이블 사양, 신호 변경(디지털,아날로그) | 10BASE-T, 100BASE-TX, ISDN, wired, wireless, RS-232, DSL, Twinax | 허브, 리피터 |


# 7계층: Application
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/57340d95-4211-4c5a-938e-1d16c4adb3d7)


- 기본 정보
    - PDU: Data or Message
    - 대표 장치: L7 Switch (방화벽 용도)
- 특징
    - 사용자의 데이터와 직접 상호 작용하는 유일한 계층
        - 웹 브라우저 및 이메일 클라이언트와 같은 소프트웨어 애플리케이션은 통신을 개시하기 위해 애플리케이션 계층에 의지
    - 애플리케이션 계층은 소프트웨어가 사용자에게 의미 있는 데이터를 제공하기 위해 의존하는 프로토콜과 데이터를 조작하는 역할 
- 프로토콜
    - FTP, HTTP, HTTPS, XML, Telenet, SSH, SMTP, POP3, IMAP 등

# 6계층: Presentation
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/c9e81145-3975-4d41-b6ea-086f0d7fe4a4)


- 기본 정보
    - PDU: Data or Message
- 특징
    - 데이터를 준비하는 역할을 하여 애플리케이션 계층이 이를 사용할 수 있게 함
        - 즉, 애플리케이션이 소비할 수 있또록 데이터를 프레젠테이션 하는 역할
    - 데이터의 변환, 암호화, 압축을 담당함
        - 장치간 서로 다른 인코딩으로 통신하는 경우, 이해할 수 있게 데이터를 변환함
        - 암호화된 통신을 하는 경우, 구문을 암호화/복호화하는 일을 담당
        - 5계층(Session)으로 전달하기 전, 전송할 데이터 양을 최소화함으로써 속도와 효율을 높임
            - 문자 데이터 외에도 음성, 동영상 등 멀티미디어 트래픽도 포함
- 프로토콜
    - ASCII, 유니코드, MIME, EBCDIC, UTF-8, MBCS, EUC-KR, JPG, MP3, MPEG 등

# 5계층: Session
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/7c02ce54-8154-4ed8-8a56-ae757a09f5ab)

- 기본 정보
    - PDU: Data or Message
- 특징
    - 두 기기 사이의 통신을 시작하고 종료하는 일을 담당하는 계층
    - 통신이 시작될 때부터 종료될 때까지의 시간을 세션이라고 함
    - 세션 계층은 교환되고 있는 모든 데이터를 전송할 수 있도록 충분히 오랫동안 세션을 개방한 다음 리소스를 낭비하지 않기 위해 세션을 즉시 닫을 수 있도록 보장 ????????? (TCP 4-way handshake의 Timeout 관련 내용인지..?)
    - 세션 계층은 데이터 전송을 체크포인트와 동기화
        - 데이터 전송 중간에 연결이 끊어지는 경우, 다시 재개시 해당 체크포인트부터 세션을 재개하는 것이 가능

# 4계층: Transport
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/3ad94196-098d-448f-b0e9-a3535d152e06)

- 기본 정보
    - PDU: Segement, Datagram
    - 대표 장치: L4 Switch
- 특징
    - 종단(외부 네트워크)간 통신을 담당
    - 세션 계층에서 데이터를 가져와서 세그먼트라고 하는 조각으로 분할
        - 수신 기기의 전송 계층은 세그먼트를 데이터로 재조립
    - 흐름 제어 및 오류 제어
        - 흐름제어: 연결 속도가 빠른 송신자가 연결 속도가 느린 수신자를 압도하지 않도록 전송 속도 조정
        - 오류제어: 정상적으로 데이터를 수신하지 못한경우 재전송을 요청
- 프로토콜
    - TCP, UDP 등

# 3계층: Network
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/5a1e8cf5-50dd-4450-b050-527e52e95e9a)

- 기본 정보
    - PDU: Packet(패킷)
    - 대표 장치: 라우터(L3 Switch)
- 특징
    - 서로 다른 두 네트워크 간 데이터 전송을 위해 사용
        - 서로 통신하는 두 장치가 동일한 네트워크에 있는 경우 네트워크 계층이 필요하지 않음
    - 전송 계층의 세그먼트를 패킷이라고 불리는 더 작은 단위로 세분화하여 전송
        - 수신 장치는 이러한 패킷을 다시 세그먼트 단위로 재조립함
    - 라우팅 동작함 (데이터가 표적에 도달하기 위한 최상의 물리적 경로를 찾음)
- 프로토콜
    - IP, ICMP, IGMP, IPsec, ARP, RARP, RIP, OSPF, IGRP, EIGRP, NAT 등

# 2계층: DataLink
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/47bf6c74-517f-44f2-89ff-d21cf4a3dd93)

- 기본 정보
    - PDU: Frame
    - 대표 장치: 브리지, 스위치(L2 Switch), 모뎀, 기지국, AP 등
- 특징
    - 네트워크 계층의 패킷을 가져와 프레임이라고 불리는 더 작은 조각으로 세분화
    - 인접한 네트워크(인트라 네트워크)간의 통신을 담당
    - 인트라 네트워크 통신에서의 흐름 제어 및 오류제어를 담당
        - 전송 계층은 네트워크 간 통신에 대해서만 흐름 제어 및 오류 제어를 담당함
    - 네트워크 계층에서 라우팅을 진행하기 위해서는 IP가 필요한데 Frame에는 해당 데이터가 없어 3계층 데이터가 필요함
- 프로토콜
    - CSMA/CD, CSMA/CA, Slott Aloha, DAC/ADC, Multiplexer, Demultiplexer, MAC 주소 관리 등

# 1계층: Physical
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/5902ee80-541c-4a99-8697-9679c3fee9cf)

- 기본 정보
    - PDU: Bit
    - 대표 장치: 네트워크 허브, 리피터, LAN카드, 통신 선로(케이블), 안테나, 엠프 등
- 특징
    - 전송과 관련된 물리적 장비가 포함됨 (케이블, 스위치 등)
    - 1과 0의 문자열인 비트 스트림으로 변환되는 계층
    - 두 장치의 물리적 계층은 신호 규칙에 동의해서 두 장치의 1이 0과 구별될 수 있어야 함
- 프로토콜
    - Ethernet, USB, Bluetooth, Wi-Fi, LTE, 5G 등 안테나 








# TCP/IP 4계층
- 실질적으로 실무에서 사용되는 계층
    - TCP/IP 프로토콜 스택은 1970년대 초반에 미국의 DARPA에서 처음 개발되었음
    - TCP/IP는 컴퓨터 네트워크의 초기 시절에 만들어진 것으로, 현재 인터넷의 핵심 통신 프로토콜로 사용되고 있음
- OSI 모델
    - OSI 모델은 1980년대에 국제 표준화 기구인 ISO에서 개발하였음
    - OSI 모델은 네트워크 통신 과정을 7단계로 나눠 각 단계의 역할과 상호작용을 정의함
    - OSI 모델은 네트워크 통신을 이해하고, 다른 네트워크 시스템과의 상호운용성을 증진하는 것을 목표로 함
- TCP/IP 프로토콜 스택이 먼저 나온 후, 그 다음에 OSI 모델이 나옴
- 현재는 두 모델(TCP/IP와 OSI) 모두 네트워크 통신을 이해하고 설계하는데 중요한 역할을 하고 있음



# 예상 질문
- OSI 7계층에 대해 설명해 주세요.
- Transport Layer와, Network Layer의 차이에 대해 설명해 주세요.
- L3 Switch와 Router의 차이에 대해 설명해 주세요.
- 각 Layer는 패킷을 어떻게 명칭하나요? 예를 들어, Transport Layer의 경우 Segment라 부릅니다.
- 각각의 Header의 Packing Order에 대해 설명해 주세요.
- ARP에 대해 설명해 주세요.

# 참고자료
http://wiki.hash.kr/index.php/OSI_7_%EA%B3%84%EC%B8%B5
https://namu.wiki/w/OSI%20%EB%AA%A8%ED%98%95?from=OSI
https://community.fs.com/article/tcpip-vs-osi-whats-the-difference-between-the-two-models.html

