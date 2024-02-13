- TCP는 신뢰성을 확보하기 위해 논리적으로 연결을 맺고 끊는 handshake를 진행
- 연결을 위해 3-way handshake를, 연결 해제를 위해 4-way handshake를 진행

![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/242c0c21-fa5e-425d-9818-855a3e42a884)


```ad-note
### TCP 제이비트(플래그) 참고
SYN (Synchronizatino): 연결 요청 플래그
ACK (Acknowledgement): 응답 플래그
ISN (Initial Sequence Numbers): 초기 네트워크 연결을 할 때 할당되는 32비트 고유 시퀀스 번호
FIN (Finish): 연결 해제 요청 플래그. 마지막 패킷임을 의미함
RST (Reset): 강제 세션 종료 플래그. 양방향에서 동시에 중단 작업 발생
URG (Urgent): 긴급 데이터 전송 시 사용되는 플래그. (e.g. Ping명령어 실행중 Ctrl+C)
```

# 3-way handshake


과정
1. SYN (클라이언트)
   - 클라이언트는 서버에 Client ISN + SYN를 전송
      - ISN은 새로운 TCP 연결에서 사용되는 임의의 시퀀스 번호
2. SYN + ACK (서버)
   - 서버는 클라이언트의 SYN을 수신
   - 서버의 ISN을 보내며 승인번호로 클라이언트의 ISN+1을 전송
3. ACK (클라이언트)
   - 클라이언트는 승인번호로 서버의 ISN + 1을 전송

## 2-way handshake를 하지않는 이유
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/e7d727a4-f63d-4098-87e5-88d25712b06c)

- 2-way handshake
    - 클라이언트가 SYN를 보내면, 서버는 ACK를 보내어 연결을 맺는 방식
- 신뢰성 보장을 할 수 없음
    - TCP는 기본적으로 양방향 연결인데 이것이 지켜지지 못함
    - 상대방에게 패킷을 전달할 수 있음을 확인해야함 (O)
    - 상대방도 나에게 패킷을 전달할 수 있음을 확인해야함 (X)
- 공격에 취약함
    - d
- Connection 실패
    - 2번 단계 메세지가 네트워크 딜레이로 인해 늦게 도착하여 Timeout이 발생하는 경우
    - 클라이언트는 서버에 SYN를 전송하지만 응답이 늦어 Timeout이 발생
    - 클라이언트는 서버에 두번째 SYN를 전송
    - 뒤늦게 첫번째 SYN에 대한 서버의 ACK가 도착
        - 2-way handshake이기 때문에 서버는 정상적으로 연결이 생성됬다고 알고있음
    - ACK를 받은 클라이언트는 자신이 보낸 SYN의 ISN값과 대치되지 않기 때문에 클라이언트는 연결 실패로 판단


## 0-RTT 와 3-way handshake
- HTTP3 부터는 TÇP가 아닌 QUIC(UDP기반)를 사용하는데, 0-RTT, 1-RTT 핸드 쉐이크를 제공함


# 4-way handshake

과정
1. FIN (클라이언트)
   - 클라이언트가 연결을 닫기위해 FIN 세그먼트를 전송
   - 클라이언트는 FIN_WAIT_1 상태가 되어 서버의 응답을 기다림
2. ACK (서버)
   - 서버는 클라이언트로 ACK 사인을 전송 
      - 클라이언트는 해당 신호 수신시 FIN_WAIT_2 상태가 됨
   - 서버는 CLOSE_WAIT 상태가 됨
3. FIN (서버)
   - 서버는 일정시간 이후에 클라이언트에게 FIN 세그먼트 전송
4. ACK (클라이언트)
   - 클라이언트는 서버로 ACK 사인을 전송
      - 서버는 해당 신호 수신시 CLOSED 상태가 됨
   - 클라이언트는 TIME_WAIT 상태가 됨
   - 클라이언트는 일정시간 이후에 CLOSED 상태가되며 연결이 해제됨

## 유령세션 - 네트워크가 종료(단절)된걸 인식하는 방법
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/fd0d6792-5e9a-42af-a43a-75f336a88d7f)

- 유령 세션
    - 제대로 연결해제가 되지 않아서, 네트워크가 단절되어있음에도 남아있는 세션
    - 할당된 리소스 등이 해제되지 못해 낭비되며, 재접속 시도시 문제를 이르킬 수 있음
        - e.g. 리니지 랜선버그
    - 발생 예시
        - ESTABLISH 상태의 클라이언트가 shutdown()되어 FIN 패킷을 전송하지 못한 경우
        - 비정상적으로 종료되었지만, 서버와 클라이언트는 모두 자신이 ESTABLISH 상태에 있다고 인지함
        - 통신 중이었다면 상대방의 ACK를 기다리고, Timeout되어 재전송 요청을 반복하게 됨
        - 이런 상황에서 다시 두 노드가 네트워크 연결이 되면 통신이 재개되는데, 이를 단순히 네트워크 환경 문제로 패킷이 지연되었다고 판단하고 그 전까지 도착하지 않은 패킷들을 지연처리 함
        - 이는 일반적으로 허용되지 않는 결과를 초래할 수 있음
- 해결 방법
    - 지속적으로 서로의 연결을 확인하는 패킷을 전송 (HeartBeat)
    - KEEP_ALIVE

## TCP 연결 요청 거부
- 클라이언트가 서버에게 SYN를 보내 연결 요청을 하였지만 서버가 연결할 준비가 안된 경우 요청을 거부한다.
- RST + ACK 패킷을 클라이언트에게 전송하여 연결 요청을 거부
- 해당 패킷을 받은 클라이언트 TCP는 CLOSED 상태가 됨

## TIME_WAIT을 하는 이유
- 지연 패킷이 발생할 경우를 대비하기 위해
    - 패킷이 뒤늦게 도달하는 경우가 있을 수 있음
    - 데이터 무결성을 위해 일정시간을 대기하며 늦게 도착하는 패킷을 챙김
- 참고로 TIME_WAIT은 보통 n분 정도 유지 (우분투에서 60초, 윈도우에서 4분 정도)



# TCP 전체 동작
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/41b44ac4-c71b-493a-836a-0d4be48d5e81)




# 참고자료
https://www.scaler.com/topics/computer-network/tcp-3-way-handshake/
https://blog.naver.com/dlansduq/221017203911
https://studyandwrite.tistory.com/404
https://www.baeldung.com/cs/handshakes
https://ozt88.tistory.com/19
https://junb51.tistory.com/4


![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/644872ee-6be4-4691-94c0-51ed69f491b2)



- 
- 두 장치가 연결이 닫혔는지 확인하기 위해
    - FIN이 실수로 2번 전송될떄????????????????
    - LAST_ACK 상태에서 연결이 닫혀버리는 경우 새로운 연결을 만들 수 없는 상태가 됨
