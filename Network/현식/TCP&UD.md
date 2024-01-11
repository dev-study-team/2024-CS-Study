

# TCP, UDP
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/9d599b11-b312-456b-8562-5866f0111270)

- Transport Layer 해당
    - Transport Layer에서 사용하는 프로토콜 (Host 도달 이후 특정 프로세스에게 까지 전달하는 역할, 포트 번호 필요)
    - Transport Layer에서는 SocketAddress 사용 = IP 주소 + Port 번호
- Port 필요
    - 포트는 65,535까지 지원하며, 1,023까지는 약속된 포트들이 있어 이보다 큰 것을 사용하기를 권고

    ![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/ab1bee64-152d-4972-95b2-d1c1cf83c105)

    
- 오류 검출
    - 인터넷 프로토콜(IP)을 통해 메시지 전달 중 오류가 발생하는 경우, ICMP를 통해 해당 사실을 알 수 있음
    - ICMP는 오류를 알려주기만 할 뿐(Exception throw/raise) 대처할 수 있는 능력이 없어 TransportLayer(TCP, UDP …)가 에러 처리를 위임 받음
 
</br>
</br>
</br>
  
# TCP (Transmission Control Protocol)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/088d9683-9fbe-4c35-b001-86607c9041e6)

- 연결형이며 신뢰성 있는 전송 프로토콜
- 3/4 way handshake 를 통해 연결(Connection)을 관리
- 송신자가 보낸 순서대로 수신자가 데이터를 받음 (데이터의 순서 보장, 가상회선을 통해 순차적으로 전송됨)
- 수신자로부터 ACK(데이터 받았음)를 받지 않으면, 송신자는 데이터를 재전송
    - 송신자는 수신자가 아직 ACK를 보내지 않은 데이터까지 버퍼에 보관하여 기억
- 흐름제어, 혼잡제어를 통해 데이터 신뢰성을 보장
- 1:1(서버 대 클라이언트) 연결 (멀티캐스팅, 브로드캐스팅 X)
- 양방향 데이터 통신을 하고 바이트 스트림을 사용
    - 수신된 메세지는 버퍼에 보관됨

<br/>

## 흐름 제어 (Flow Control)

![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/02707ada-8a23-4cc6-b4bb-ed8308a48ecf)


- 수신자가 메세지를 받아서 처리하는 속도보다, 송신되는 속도가 더 빠를 경우 데이터 손실이 발생
    - 데이터가 손실되는 만큼 복구를 위한 비용이 발생
- 송신 측의 데이터 전송량을 수신 측의 상황(처리 능력)에 맞게 조절하여 이를 해결하는 방법
- 흐름 제어를 위해 `슬라이딩 윈도우` 사용
    - 송신자는 윈도우 사이즈만큼만 패킷 전송 가능
        - window는 메모리 버퍼의 일정 영역을 뜻 함
        - 전송시마다 윈도우 사이즈가 감소
        - 정상 수신시마다 윈도우 사이즈가 증가
    - 윈도우의 크기는 수신자가 제어함 (최초 윈도우 크기는 3-way-handshake시에 설정됨)
    - DataLink Layer의 Window와는 다르게 Byte지향적이며 윈도우 크기가 가변적이라는 차이가 있음
    - 슬라이딩 윈도우 프로토콜에는 Go-Back-N ARQ, Selective Repeat ARQ 두 종류가 있으며, TCP는 이 둘의 개념을 적절히 혼합하여 사용

ARQ: Automatic Repeat Request

Go-Back-N ARQ: https://javatpoint.com/go-back-n-arq

SR ARQ: https://www.scaler.com/topics/computer-network/selective-repeat-arq/

TCP는 GoBackN or SelectiveRepeat?: https://blog.naver.com/whdgml1996/222148180270

<br/>

## 혼잡 제어 (Congestion Control)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/d17cb6dc-50b3-4c85-be1f-a88df88fa8a5)



- 네트워크 내의 패킷 수가 과도하게 증가하여  일정 수준을 넘지 않도록 방지하여, 오버플로우 발생을 막는 것
- 라우터의 처리 속도가 느린 경우, 패킷을 일부만 전송하여 `혼잡 붕괴 현상`이 일어나는 것을 막는다.
- 혼잡 제어를 위한 별도의 송신용 `윈도우(CongestionWindow)` 를 사용하며 이를 유연하게 조정하는 것이 혼잡 제어의 핵심
    - Flow Control과 달리 송신측에서 단독으로 구현
    - TCP Vegas, Westwood, BIC, CUBIC 등의 알고리즘 사용

혼잡 붕괴 현상: 특정 라우터에 처리 능력 이상으로 데이터가 몰릴 경우 자신에게 온 데이터를 모두 처리할 수 없게 됨 → 송신자들은 정상적으로 도달하지 못했다고 판단해 재전송을 시도 → 이렇게 처리되지 못한 데이터들이 쌓여만 감 → 오버플로우 등 데이터 손실 발생 가능

<br/>

### AIMD (Additive increase & multiplicative decrease)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/2880b8c9-119e-49f6-8262-6350938e20f3)


- 패킷 로스가 발생하지 않는다면 선형으로 윈도우 크기 증가
- 패킷 로스 발생시 윈도우 크기를 절반으로 줄임


<br/>

### 혼잡제어 방법 (느린시작 + 혼잡회피 + 빠른회복)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/cd0d17c2-5218-4f60-85a2-c12fbcfb2ed4)


- TCP 버전
    - TCP Tahoe: 초기 TCP 버전으로 (느린시작 + 혼잡회피)
    - TCP Reno: 손실 이벤트 발생시 더 둔감하게 대응 (느린시작 + 혼잡회피 + 빠른회복)
- Slow Start
    - AIMD 방식의 초반 증가 폭이 너무 느린 단점을 보완하기 위해 고안
    - 패킷 로스가 발생하지 않는다면 윈도우 크기를 2배씩 늘림
    - SlowStartThreshold에 도달할 때까지만 진행
- Congestion Avoidance
    - SlowStartThreshold 도달 이후, 패킷 로스가 발생하지 않는다면 윈도우 크기를 선형으로 증가
- Fast Recovery
    - 기존 TCP Tahoe에서 패킷 손실 이벤트 발생시, 윈도우 사이즈가 지나치게 작아지는 것을 보완
    - Duplicat ACKs가 발생하는 순간에만 동작
    - 네트워크 환경이 좋은 경우 Timeout이 잘 발생하지 않아 Congenstion Avoidance가 성능에 영향을 미치게 됨
- 패킷 로스 발생시
    - Duplicate ACKs
        - 중복된 ACK가 계속 온다는 것은 특정 패킷만 loss되고 나머지는 제대로 전송된 것을 의미
        - 이 때는, 윈도우 크기를 절반으로 줄이고, ssthresh 값을 절반으로 감소
    - Timeout
        - 타임아웃의 경우 전송한 모든 패킷이 로스 되었다는 것을 의미하므로 심각한 경우
        - 이 때는, 윈도우 크기를 1로 줄이고, ssthresh 값을 절반으로 감소

TahoeTCP: https://velog.io/@nsh7293/TCP-Taho-TCP-Reno-TCP

혼잡제어: https://ddongwon.tistory.com/87

TCP/IP(NaverD2): https://d2.naver.com/helloworld/47667

RenoTCP?: https://studyandwrite.tistory.com/438

<br/>

## 세그먼트 (TCP Packet)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/fc20b355-fa2f-4c3b-8057-9e685ceed0b6)


- 헤더
    - 발신지 포트 번호
    - 수신지 포트 번호
    - 순서 번호 (seq)
    - 확인 응답 번호 (ack)
    - 헤더 길이
    - 제어 비트 (urg, ack, psh, rst, syn, fin)
    - 윈도우 크기 (상대방이 유지해야하는 바이트 단위의 창 크기 ← 수신자: 윈도우 크기 이정도로 해주세용)
    - 체크섬 (오류 검사용)
    - 긴급 지시자
    - 옵셔널한 값들
- 데이터

<br/>

## 넘버링 시스템 (seq, ack)

- 세그먼트 헤더에는 순서번호(seq)와 확인 응답 번호(ack)가 존재
- 연결 상태에서 전송되는 모든 데이터 바이트에 번호를 부여 (각 방향에서 독립적으로 부여)
- 번호는 임의로 생성된 번호를 가지고 시작
- 프로세스로부터 데이터를 수신하여 송신 버퍼에 저장할 때 번호가 부여됨
- 확인 응답 번호(ack)는 누적된다. 예를 들어 확인응답으로 1,000을 사용한다면 999까지 모든 바이트를 정상적으로 수신했다는 것을 의미

</br>
</br>
</br>

# UDP (User Datagram Protocol)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/49402dec-d263-48d6-8cc5-61dc9c7a19d3)


- 비연결형이며 신뢰성이 없는 전송 프로토콜
- 연결 개념이 존재하지 않음
- 데이터그램의 패킷교환은 데이터의 순서가 보장되지 않음
- TCP에 비해 상대적으로 통신 비용이 낮고 빠름
- 1:1, 1:n, n,m 통신을 모두 가능하며, 단방향 통신만 지원

<br/>

## 데이터그램 (UDP Packet)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/267ec66f-06f9-4141-8e34-bdd228ed3e23)


- 헤더
    - 발신지 포트 번호
    - 수신지 포트 번호
    - 데이트그램의 크기 (length)
    - 체크섬 (오류 검사용)
- 데이터

</br>
</br>
</br>

# TCP vs UDP
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/beee53ad-3082-4768-a036-346bde0821d6)

<br/>

## 비교 표

|  | TCP | UDP |
| --- | --- | --- |
| 풀네임 | Transmission Control Protocol | User Datagram Protocol |
| 특징 | 연결,형 신뢰, 느림 | 비연결형, 신뢰성 X, 빠름 |
| Connection | O (3 way-handshake) | X |
| 데이터 순서 보장 | O | X |
| 흐름제어 (Flow) | O | X |
| 혼잡제어 (Congestion) | O | X |
| 오류 감지 | O | O |
| 데이터 단위 | Segment (TCP Header + Data) | Datagram (UDP Header + Data) |
| 사용 예 | 카톡 처럼 신뢰성이 중요할 때 | 스트리밍, 프로토콜을 커스텀하고 싶을 때 (HTTP3) |

<br/>

### 기타

- TCP와 UDP는 포트를 고유하게 관리하므로 같은 포트 번호를 사용해도 문제가 되지 않는다.
    - TCP의 포트 1234 ≠ UDP의 포트 1234
- 클라이언트 프로그램에서 동일 포트로 여러 커넥션이 생성되는 경우, 내부적으로 Dynamic Range에 있는 Port들을 배정하여 사용

</br>
</br>
</br>

# 기타

## DNS에서 UDP를 사용하는 이유

- 짧은 요청을 보내고 짧은 응답을 기대하는 경우 중에 하나
    - Request의 양이 작음 -> UDP Request에 담길 수 있다.
- 만약 Timeout이 되어 패킷을 못받더라도 다시 시도하면 된다.
    - 3 way handshaking으로 연결을 유지할 필요가 없다. (오버헤드 X)
    - Request에 대한 손실은 Application Layer에서 제어가 가능하다.
- 기타
    - DNS : port 53번
    - TCP가 사용되는 경우
        - 메세지 크기가 512Byte(UDP 제한)이 넘을 때
        - ZoneTransfer (다수의 DNS 서버간 데이터베이스를 복제하는 경우)\

DNS는 TCP/UDP 모두 작동: https://learn.microsoft.com/ko-kr/troubleshoot/windows-server/networking/dns-works-on-tcp-and-udp (마이크로소프트)

DNS질의가 TCP를 사용하는 경우: https://dev.dwer.kr/2020/03/dns-tcp.html


</br>
</br>
</br>

# 예상 질문

### TCP와 UDP의 차이는 무엇인가요?

모범답안: TCP는 연결형이며 신뢰성 있는 전송을 제공하는 프로토콜로, 3-way handshake와 데이터의 순서 보장, 흐름 제어, 혼잡 제어 등이 특징입니다. UDP는 비연결형이며 신뢰성이 없는 전송을 제공하는 프로토콜로, 연결 개념이 없고 데이터의 순서가 보장되지 않습니다.


### TCP의 흐름 제어에 대해 설명해보세요.

모범답안: 흐름 제어는 수신자가 데이터를 처리하는 속도와 송신자가 데이터를 전송하는 속도 간의 불일치를 해결하기 위한 메커니즘입니다. TCP에서는 슬라이딩 윈도우를 사용하여 송신자가 전송할 수 있는 데이터의 양을 제어하고, 수신자는 윈도우 크기를 조절하여 송신자에게 허용되는 데이터의 양을 알려줍니다.
### TCP의 혼잡 제어에 대해 설명해보세요.

모범답안: 혼잡 제어는 네트워크 내의 패킷 수가 과도하게 늘어나 혼잡을 방지하기 위한 메커니즘입니다. TCP는 별도의 송신용 윈도우(Congestion Window)를 사용하여 혼잡을 감지하고 유연하게 조절합니다. 혼잡 붕괴를 방지하기 위해 라우터의 처리 속도를 고려하여 패킷을 전송하며, 여러 알고리즘(AIMD, TCP Vegas, Westwood 등)을 사용하여 혼잡을 효과적으로 제어합니다.
### TCP Tahoe와 TCP Reno의 차이는 무엇인가요?

모범답안: TCP Tahoe는 초기 버전으로 느린 시작과 혼잡 회피를 지원합니다. 반면에 TCP Reno는 패킷 손실 이벤트에 둔감하게 대응하여 빠른 회복까지 지원합니다. 이들은 TCP의 버전으로, TCP Reno는 TCP Tahoe의 단점을 보완한 것으로 볼 수 있습니다.
### UDP의 특징은 무엇이며, 어떤 상황에서 사용되나요?

모범답안: UDP는 비연결형이며 신뢰성이 없는 전송 프로토콜입니다. 데이터의 순서가 보장되지 않지만 상대적으로 통신 비용이 낮고 빠르며, 1:1, 1:n, n:m 통신을 모두 지원합니다. 주로 신뢰성이 중요하지 않은 환경에서 사용되며, 예를 들면 스트리밍, 커스텀 프로토콜 (예: HTTP3) 등의 상황에서 활용됩니다.
### TCP와 UDP의 포트 번호는 왜 고유하게 관리되며, 같은 포트 번호를 사용해도 문제가 없는 이유는 무엇인가요?

모범답안: TCP와 UDP는 서로 다른 프로토콜이지만 같은 포트 번호를 사용해도 문제가 없습니다. 이는 각각의 프로토콜은 독립적으로 동작하며, 클라이언트 프로그램에서 동일한 포트로 여러 커넥션이 생성되어도 내부적으로는 Dynamic Range에 있는 포트들을 할당하여 사용하기 때문입니다.
### DNS에서 UDP를 사용하는 이유는 무엇인가요?

모범답안: DNS에서 UDP를 사용하는 이유 중 하나는 짧은 요청과 짧은 응답이 예상되는 경우입니다. UDP는 작은 양의 데이터를 빠르게 전송할 수 있고, 요청이 작기 때문에 Timeout이 되더라도 다시 시도할 수 있습니다. 또한, 오버헤드가 적어서 효율적으로 사용될 수 있습니다.

### TCP에서 3방향 핸드셰이크의 목적은 무엇입니까?

예상 답변: 3방향 핸드셰이크는 TCP에서 클라이언트와 서버 간의 안정적인 연결을 설정하는 데 사용됩니다. 여기에는 시퀀스 번호를 동기화하고 성공적인 연결 설정을 확인하기 위한 SYN, SYN-ACK 및 ACK 패킷 교환이 포함됩니다.
### TCP의 시퀀스 번호 개념을 설명하세요.

예상 답변: TCP의 시퀀스 번호는 연결 내에서 데이터의 각 바이트를 고유하게 식별하는 데 사용됩니다. 이는 수신된 데이터의 올바른 순서를 유지하고 안정적인 데이터 전송을 보장하는 데 도움이 됩니다.
### 패킷 손실 시 TCP는 데이터 재전송을 어떻게 처리합니까?

예상 답변: TCP는 승인(ACK)과 타임아웃 메커니즘을 결합하여 데이터 재전송을 처리합니다. 보낸 사람이 지정된 시간 초과 기간 내에 ACK를 받지 못하면 패킷 손실을 가정하고 데이터를 다시 전송합니다.
### 어떤 시나리오에서 TCP보다 UDP를 사용하는 것을 선호합니까?

예상 답변: 실시간 통신이 중요하고 일부 데이터 손실이 허용되는 시나리오에서는 UDP가 선호됩니다. 예를 들어 온라인 게임, 라이브 스트리밍, 보장된 전송보다 짧은 대기 시간이 더 중요한 애플리케이션 등이 있습니다.
### TCP 세그먼트에서 긴급 포인터의 중요성은 무엇입니까?

예상 답변: TCP 세그먼트의 긴급 포인터는 TCP 스트림의 긴급 데이터가 끝났음을 나타냅니다. 즉각적인 주의나 처리가 필요한 데이터 부분을 수신자에게 알리는 데 사용됩니다.
### TCP의 창 크기 개념을 설명할 수 있습니까?

예상 답변: TCP의 창 크기는 보낸 사람이 보낼 수 있고 승인을 받기 전에 받는 사람의 버퍼에 보관될 수 있는 데이터의 양(바이트)을 나타냅니다. 이는 흐름 제어에서 중요한 역할을 하며 송신자가 수신자의 용량에 따라 전송 속도를 조정할 수 있도록 합니다.
### TCP는 혼잡 회피를 어떻게 처리합니까?

예상 답변: TCP는 인지된 네트워크 정체에 따라 전송 속도를 조정하여 정체 회피를 처리합니다. AIMD(Additive 증가, 곱셈 감소)와 같은 알고리즘을 사용하여 네트워크가 깨끗할 때 창 크기를 선형적으로 늘리고 혼잡이 감지되면 곱셈적으로 감소합니다.
### TCP의 Window Scale 옵션의 목적은 무엇입니까?

예상 답변: TCP의 창 크기 조정 옵션은 TCP 헤더의 창 크기 필드를 확장하는 데 사용됩니다. 더 큰 창 크기를 허용하여 특히 고속, 대기 시간이 긴 연결에서 네트워크 리소스를 보다 효율적으로 사용하고 성능을 향상시킬 수 있습니다.
