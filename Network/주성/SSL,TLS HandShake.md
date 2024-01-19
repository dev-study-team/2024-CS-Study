## SSL부터 1.2까지 통신과정통신 과정(RSA,DH)

![Pasted image 20240118225817](https://github.com/dev-study-team/2024-CS-Study/assets/53209324/718e2323-7f30-4a10-83fa-3e34bbd3f0ad)

<img width="420" alt="Pasted image 20240118234639" src="https://github.com/dev-study-team/2024-CS-Study/assets/53209324/2f1bba04-e225-43f8-a904-898ab0ac4ac4">


RSA,DH는 키교환 알고리즘 중 하나입니다
### RSA 통신과정

1. **Client Hello** : 클라이언트측에서 생성한 랜덤 데이터, 클라이언트가 지원하는 암호화 방식들(chipher suites),클라이언트가 지원하는 TLS 버전 ,세션 ID(이전에 연결됬다면 존재) 등 포함한 패킷을 Client -> Server로 전송한다.
2.  **Sever Hello** : 서버측에서 생성한 랜덤 데이터, 서버가 선택한 클라이언트의 암호화 방식(chipher suites) 등 포함한 패킷을 Server -> Client로 전송한다.
3.   **인증 :** SSL인증서를 서버의 개인키로 암호화 하여 클라이언트로 전송합니다. 클라이언트는 브라우저 CA 리스트에 있는 공개키를 바탕으로 이를 복호화하여 서버의 공개키를 얻고 자신이 접속한 서버가 맞는지 즉 서버 인증에 대해 확인합니다.
4. **Server Key Exchange/Server Hello Done** : 서버 공개키 포함한 패킷을 Server -> Client로 전송한다. (서버 공개키는 Ceritificate에 포함되기도 하고 포함되지 않을 수도 있음. 포함되지 않았다면 이때 전송) 이후 서버 헬로우가 끝났다는것을 알린다. 
5. **Client Key Exchange/Change Cipher Spec/Encryted Handshake Message**
	1. Client Key Exchange : 클라이언트는 세션 키를 공유하기 위한 프리마스터 시크릿을 서버의 공개키로 암호화하여 전송합니다.
	2. "Change Cipher Spec" 메시지는 핸드셰이크 단계 중에서 보내는 마지막 메시지로, 클라이언트는 서버에게 이 메시지를 통해 암호화된 통신을 시작할 것임을 알립니다.
	3. "Encrypted Handshake Message"는 실제 핸드셰이크 메시지를 암호화하여 전송하는 역할을 합니다. 암호화된 핸드셰이크 메시지에는 세션 키 협상, 인증 등의 핸드셰이크에서 중요한 정보들이 포함됩니다. (앞 Change Cipher 부터 전까지 모두 담긴다)
	4. 이때 클라이언트는 클라이언트 헬로우 피니쉬 메시지를 같이 보낸다.
	5. 예상이지만 이때  클라이언트는 대칭키를 생성해놓는 것 같다.
6. Change Cipher Spec/Encryted Handshake Message 
	1. "Change Cipher Spec" 메시지는 핸드셰이크 단계 중에서 보내는 마지막 메시지로, 서버는 클라이언트에게 이 메시지를 통해 암호화된 통신을 시작할 것임을 알립니다.
	2. Encryted Handshake Message : Version(TCP1.2)등 위에설명한대로 핸드셰이크 내용을 담는 것 같다.
	3. 서버 헬로우 피니쉬 메시지를 보낸다
	4. 이때 서버는 대칭키를 생성해놓는것 같다.
7. 이후 대칭키를 이용해 데이터를 교환하게 됩니다.

> **세션 키 생성:** 클라이언트와 서버가 모두 클라이언트 무작위, 서버 무작위, 예비 마스터 암호를 이용해 세션 키를 생성합니다. 모두 같은 결과가 나와야 합니다.

### DH 이용 통신과정

4. **Server Key Exchange/Server Hello Done** : 원래 내용에 추가로 메시지에 DH에서 사용되는 소수(p)와 원시근(g) 등을 포함한다. 이때 서버는 Diffie-Hellman 그룹에서의 개인 키를 생성하고, 이를 클라이언트에게 전송한다.
5. **Client Key Exchange/Change Cipher Spec/Encryted Handshake Message**
	1. Client Key Exchange : 클라이언트는 마찬가지로 자신의 Diffie-Hellman 개인 키를 생성하고, 이를 서버에게 전송합니다.

클라이언트 공개키 + 서버 비공개키 = 클라이언트 비공개키 + 서버 공개키 = 세션키를 생성한다라고 생각하면 좋다. 자세한건 [링크](https://blog.naver.com/wellconn/221550406582)

#### DH를 사용함으로써 장점
순방향 비밀성, 완벽한 순방향 비밀성을 가질 수 있다. 쉽게 말해 개인 키가 노출된다고 하더라도 암호화된 데이터가 암호화된 상태로 유지되도록 하여 더 높은 보안을 챙기는 것이다.
##### FS(Forward Secrecy, 순방향 비밀성)란 무엇일까요? PFS(Perfect Forward Secrecy, 완벽한 순방향 비밀성)란 무엇일까요?

FS(Forward Secrecy, 순방향 비밀성)는 개인 키가 노출된다고 하더라도 암호화된 데이터가 암호화된 상태로 유지되도록 합니다. 이것을 "PFS(Perfect Forward Secrecy, 완벽한 순방향 비밀성)"라고도 합니다. FS(Forward Secrecy, 순방향 비밀성)는 각각의 통신 세션마다 고유한 세션 키를 사용하거나 세션 키를 개인 키와 별도로 생성하는 경우에 가능합니다. 어느 하나의 세션 키가 손상되더라도 공격자가 해당 세션만 해독할 수 있을 뿐 다른 세션은 모두 암호화된 상태로 유지됩니다.

순방향 비밀성(forward secrecy)을 위해 설정된 프로토콜에서는 개인 키가 최초 핸드셰이크 프로세스 중 인증용으로 사용될 뿐 다른 목적으로는 사용되지 않습니다. 임시 Diffie-Hellman 핸드셰이크는 개인 키와 별도로 세션 키를 생성하므로 순방향 비밀성을 갖추었다고 할 수 있습니다.

대조적으로, RSA는 순방향 비밀성이 없습니다. 개인 키가 손상되면 공격자가 예비 마스터 암호를 해독할 수 있고 클라이언트 무작위와 서버 무작위는 일반 텍스트이므로 과거의 대화에 대한 세션 키를 결정할 수 있습니다. 공격자는 이 셋을 조합하면 어떠한 세션 키든 도출할 수 있습니다


## TLS 1.3 통신과정

![Pasted image 20240118235904](https://github.com/dev-study-team/2024-CS-Study/assets/53209324/9ecc3635-2e62-42c6-a180-ee7cb6590a5a)

<img width="427" alt="Pasted image 20240118235938" src="https://github.com/dev-study-team/2024-CS-Study/assets/53209324/2369a87b-c5c4-46fb-b8fb-64b43c21cdae">

TLS1.3 핸드쉐이크는 전과 달리 단 1회의 오아복만 포함됩니다. 이로인해 대기 시간이 단축 되는 것!

### 통신 과정
1. **Client Hello** : 클라이언트 공개키들(extension : key share), 클라이언트가 지원하는 인증/해쉬 알고리즘 방식들(extension : signature algorithms), 클라이언트가 지원하는 암호화 방식들(chipher suites), 세션 ID 등 포함한 패킷을 Client -> Server로 전송한다.
2. **Server Hello**/**Change Cipher Spec****** : 서버가 선택한 클라이언트의 암호화 방식(chipher suites), 서버 공개키(extension : key share)등 포함한 패킷을 Server -> Client로 전송한다.
   이때 **pre_shared_key**라는 것을 같이 보내 **세션 재 연결 시 라운드 트립을 줄이기 위해 사용**한다.**(TLS1.3 단축 협상 0-RTT)**
3. 클라이언트는 서버 인증서를 확인하고 서버의 키 공유를 가지고 있기 때문에 키를 생성한 후 "Client Finished" 메시지를 보냅니다.
여기서부터 데이터 암호화가 시작되는 것이죠.

### TLS 장점
TLS Handshake가 생성한 잠재적인 지연을 완화하는 것을 돕는 기술이 있습니다. 하나는 TLS Handshake가 완료되기 전에 서버와 클라이언트가 데이터 전송을 시작하도록 하는 TLS False Start입니다. TLS를 빠르게 하기 위한 또 다른 기술은 이전에 커뮤니케이션한 적이 있는 서버와 클라이언트가 간략화된 Handshake를 사용하도록 허용하는 TLS Session Resumption입니다.
https://www.cloudflare.com/ko-kr/learning/ssl/transport-layer-security-tls/
#### 세션 재개를 위한 0-RTT 모드
![[Pasted image 20240119002111.png]]
https://www.fasterize.com/en/blog/tls-1-3-the-new-and-more-efficient-version-of-secure-web/

또한 TLS 1.3은 클라이언트와 서버 간의 왕복 또는 주고받는 통신이 전혀 필요 없는 더욱 빠른 버전의 TLS 핸드셰이크를 지원합니다. 클라이언트와 서버가 이전에 서로 연결된 적이 있는 경우(예: 사용자가 이전에 웹 사이트를 방문한 적이 있는 경우) 첫 번째 세션에서 "재개 주 암호"라고 하는 또 다른 공유 암호를 각각 도출할 수 있습니다. 또한 서버에서는 이 첫 번째 세션 중에 세션 티켓이라고 불리는 것을 클라이언트에게 보냅니다. 클라이언트는 이 공유 암호를 사용하여 다음 세션의 첫 번째 메시지에서 해당 세션 티켓과 함께 암호화된 데이터를 서버로 전송할 수 있습니다. 
