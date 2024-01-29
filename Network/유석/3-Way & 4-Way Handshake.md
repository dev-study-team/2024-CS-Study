# 📚 3-Way Handshake와 4-Way Handshake
 
 3-Way Handshake는 TCP의 접속, 4-Way-Handshake TCP의 접속 해제 과정이다.
 
 - 포트 상태 정보
   - CLOSED: 포트가 닫힌 상태
   - LISTEN:포트가 열린 상태로 연결 요청 대기 중
   - SYN_RCV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
   - STABLISHED: 포트 연결 상태
 - 플래그 정보
   - TCP Header 에는 Control Bit(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
     - 즉, 해당 위치의 BIT가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
   - SYN(Synchronize Sequence Number) / 000010
     - 연결 설정, sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
   - ACK(Acknowledgement) / 010000
     - 응답 확인, 패킷을 받았다는 것을 의미한다.
     - Acknowledgement Number 필드가 유요한지를 나타낸다.
     - 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
   - FIN(Finish)
     - 연결 해제, 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.
     - 4way handshake에서 사용한다.
     
# 📘 TCP의 3-Way Handshake
- TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 연결을 설정(Connection Establish) 하는 과정
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
- 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.

![](https://velog.velcdn.com/images/gkdlzjaqor92/post/e47e69b8-4055-4738-83be-8a44fa3b4dc7/image.png)

- **Signal 1 (SYN)**
클라이언트는 서버와 커넥션을 텍스트연결하기 위해 SYN을 보낸다. (seq: x)
  - 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  - Port 상태:
  	- Client: CLOSED-SYN_SENT 로 변함
    - Server: LISTEN
    
- **Signal 2 (ACK)**
서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (seq: y, ACK: x+1)
  - 접속요청을 받은 Q가 요청을 수락했으며, 접속 요청 프로세스인 P도 포트를 열어달라는 메세지를 전송 (SYN-ACKK signal bits set)
  - ACK Number 필드를 Sequence Number + 1로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 세그먼트 전송 (seq=y, Ack=x+1, SYN, ACK)
   - Port 상태
     - Client: CLOSED
     - Server: SYN_RECEIVED
  
- **Signal 3 (ACK)**
  클라이언트는 서버의 응답으로부터 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄
  - 마지막으로 접속 요청 프로세스가 수락 확인을 보내 연결을 맺음 (ACK)
  - 이때, 전송할 데이터가 있으면 이 단계에서 데이터르 전송할 수 있다.
  - Port 샅애:
    - Client: ESTABLISED
    - Server: SYN_RCV => ACK =>ESTABLISED
     
  위 과정을 통해 full-duplex 통신이 구축됩니다.
  
# 📙 TCP의 4-Way Handshake
4-Way Handshake는 연결을 해제(Connection Termination)하는 과정이다. 여기서는 FIN 플래그를 이용한다.
- FIN(Finish): 세션을 종료시키는데 사용되며, 더 이상 보낸 데이터가 없음을 나타낸다.

## Termination의 종류
TCP는 대부분의 connection-oriented 프로토콜과 같은 두 가지 연결 해제 방식이 있다.

**1. Graceful connection release(정상적인 연결 해제)**
   정상적인 연결 해제에서는 양쪽이 커넥션이 서로 모두 커넥션을 닫을 때까지 연결되어 있다.
   
**2. Abrupt connection release(갑작스런 연결 해제)**
   - 갑자기 한 TCP 엔티티가 연겨릉ㄹ 강제로 닫는 경우
   - 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우
   
> RST(TCP reset) 세그먼트가 전송되면 갑작스러운 연결 해제가 수행되는데, RST 세그먼트는 아래와 같은 경우에 전송된다.


1. 존재하지 않는 TCP 연결에 대해 비SYN 세그먼트가 수신된 경우
2. 열린 커넥션에서 일부 TCP 구현은 잘못된 헤더가 있는 세그먼트가 수신된 경우
	- RST 세그먼트를 보내, 해당 커넥션을 닫아 공격을 방지한다.
3. 일분 구현에서 기존 TCP 연결을 종료해야 하는 경우
	- 연결을 지원하는 리소스가 부족할 때
    - 원격 호스트에 연결할 수 없고 응답이 중지되었을 때


![](https://velog.velcdn.com/images/gkdlzjaqor92/post/5e3aabe2-1ee5-4004-b56a-04e403454b8e/image.png)

- Signal 1 (FIN + ACK)
  - 서버와 클라이언트가 연결된 상태에서 클라이언트가 서버와 연결을 해제하기 위해 close()를 호출한다.
  - 이때, 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.
    - 이때 FIN 패킷에는 실질적으로 ACK도 포함되어있다.
    
- Signal 2 (ACK)
  - 서버는 FIN을 받고, 확인했다는  ACK를 클라이언트에게 보내고 자신의 통신이 끝날때까지 기다린다. (TIME_WAIT 상태로 진입)
   - 서버는 클라이언트에게 응답을 보내고 CLOSE_WAIT 상태에 들어간다. 그리고 **아직 남은 데이터가 있다면 마저 전송을 마친 후에 close()를 호출**
   - 클라이언트에서는 서버에서 ACK를 받은 후에 서버가 남은 데이터 처리를 끝내고 FIN 패킷을 보낼 때까지 기다리게 된다. (FIN_WAIT2 상태로 진입)

- Signal 3 (FIN)
  - 서버가 데이터를 모두 보냈다면, 서버는 연결이 종료에 합의 한다는 의미로 FIN 패킷을 클라이언트에게 보낸 후에, 승인 번호를 보내줄 때까지 기다리는 LAST_ACK 상태로 진입한다.
 
- Signal 4 (ACK)
  - 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다.
  - 아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT 상태를 통해 기다린다.
    - 이때 TIME_WAIT 상태는 **의도치 않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지**
    - 만약 에러로 인해 종료가 지연되다가 시간이 초과되면 CLOSED로 들어간다.
- 서버는 ACK를 받은 이후 소켓을 닫는다.
- TIME_WAIT 시간이 끝나면 클라이언트도 닫는다.
  
# 🔎 예상 질문
> **Q1. TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유는?**


Client가 데이터 전송을 마쳤다고 하더라도 Server는 아직 보낼 데이터가 남아있을 수 있어서 일단 수신받은 FIN에 대한 ACK를 먼저 보내고, 데이터를 모두 전송한 후에 최종적으로 FIN을 보내기 때문이다.

> **Q2. 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?**

이러한 현상에 대비하여 Client는 Server로부터 FIN 플래그를 수신하더라도 일정시간(Default: 240sec)동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다. (TIME_WAIT 과정)

> **Q3. 초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?**

Connection을 맺을 때 사용하는 포트(Port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다. 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용하는 가능성이 존재한다. 서버 측에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데 난수가 아닌 순처적인 Number가 전송된다면 이전의 Connection으로부터 오는 패킷으로 인식할 수 있다. 이런 문제가 발생할 가능성을 줄이기 위해서 난수로 ISN을 설정한다.

### References
https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake
https://jeongkyun-it.tistory.com/180
https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%203%20way%20handshake%20%26%204%20way%20handshake.md
https://seongonion.tistory.com/74
