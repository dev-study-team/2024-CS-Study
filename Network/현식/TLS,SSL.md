

# TLS(SSL)
![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/d1c78201-599a-45ef-9228-37994c382b84)


- SSL은 웹 표준 암호화 통신으로, 웹 클라이언트와 서버 사이에 정보를 암호화해주는 방식
- TLS는 SSL의 향상된 버전이나, 일반적으로 SSL이라고 부른다.
    - SecureSocketsLayer, TransportLayerSecurity
- SSL 및 SSL 디지털 인증서 사용시 장점 (=HTTPS)
    - 통신 내용이 노출되는 것을 막을 수 있음
    - 클라이언트가 접속하려는 서버가 신뢰할 수 있는 서버인지 판단할 수 있음
    - 통신 내용의 악의적인 변경을 방지할 수 있음
- SSL 인증서 역할
    - 클라이언트가 접속한 서버가 신뢰 할 수 있는 서버임을 보장
    - SSL 통신에 사용할 공개키를 클라이언트에게 제공

<br/>

## SSL 인증서
- 클라이언트와 서버간의 통신을 제3자가 보증해주는 전자화된 문서
- 인증서 내용
    - 서비스의 정보 (인증서를 발급한 CA(기관), 서비스의 도메인 등)
    - 서버 측 공개키 (공개키의 내용, 공개키의 암호화 방법)
- 인증서를 발행하기 위해서 서비스의 도메인, 공개키 같은 정보를 제출해야 함

### CA (CertificateAuthority)
- SSL 디지털 인증서를 발급하는 기관
- CA는 엄격하게 공인된 기업들만이 참여할 수 있음
    - 대표기업: IdenTrust, DigiCert, Sectigo, GlobalSign, Let'sEncrypt, GoDaddy
    - 공인된 기업에서 발행한 인증서가 아닌 경우 브라우저에서 경고가 발생함
- 브라우저는 신뢰할 수 있는 CA 목록을 가지고 있음
- Root Certificate라고도 부름

### 브라우저가 'SSL 인증서'를 활용하는 법
브라우저는 SSL 인증서를 신분증처럼 확인하여, 신뢰할 수 있는 서버인지 확인함

순서
1. 브라우저(=클라)는 서버에 인증서 요청
2. 브라우저는 인증서를 발급한 CA가 자신이 가지고있는 '신뢰할 수 있는 CA 리스트'에 적혀있는지 확인
3. 신뢰할 수 있다면, '해당 CA의 공개키'를 이용해 인증서를 복호화
4. 복호화가 성공한 경우 해당 서버를 신뢰할 수 있다고 판단
    1. 비공개키는 오직 해당 CA만 소지하도록 관리되는데,
    2. 정상적으로 복호화가 된다는 것은 CA가 비공개키를 통해 해당 인증서를 암호화하 하였다는 사실

<br/>
<br/>
<br/>

# TLS Handshake 
TLS 암호화를 사용하는 세션을 만드는 과정으로, 서버와 클라이언트가 서로를 검증하고 사용할 암호화 알고리즘을 선택하여 세션키(암호화용 새 키)를 정의합니다. (TLS Handshake는 비대칭 암호화를 사용)

![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/b2e9fc3d-aa4d-47d8-95bf-2831fcaa5daa)


<br/>

### 언제 발생하는가
- 클라이언트가 HTTPS를 통해 새로운 사이트에 접속한 경우에 발생
- TCP Handshake가 진행된 이후에 TLS Handshake가 진행 됨

<br/>


### 어떤 일을 하는가
- 사용할 TLS 버전(TLS 1.0, 1.2, 1.3)을 지정
- 사용할 암호 알고리즘 결정
- 서버의 공개키와 SSL 인증서를 기반으로 신뢰할 수 있는 서버인지 확인
- 대칭 암호화를 사용하기 위한 세션키 발급 (이후 이를 통해 암호화 통신 진행)

![image](https://github.com/dev-study-team/2024-CS-Study/assets/10378777/77f3fb6b-8aba-47c9-8ee4-c68cc9e644d7)


<br/>



### TLS 1.3 이전 Handshake 

1. Client Hello
   - 클라이언트가 서버로 보내는 Handshake 요청 메세지
   - 클라이언트가 지원하는 TLS 버전, 지원 암호 알고리즘 목록, 무작위 바이트 배열이 메세지에 포함됨
2. Server Hello
   - 클라이언트 헬로 메세지에 대한 응답 메세지
   - 서버의 SSL 인증서, 서버에서 선택한 암호 알고리즘, 무작위 바이트 배열이 메세지에 포함됨
   - Handshake 방식에 따라, 이후 메세지에 디지털 서명을 전달하여 클라이언트가 확인할 수 있도록 함
3. 인증
   - 클라이언트는 서버가 전달해준 SSL 인증서를 검증 (신뢰할 수 있는 서버인지 확인)
   - 서버의 SSL 인증서를 통해 공개키를 획득
4. 예비 마스터 암호
   - 클라이언트는 공개키로 암호화한 무작위 바이트 문자열을 서버로 전송 (예비 마스터 암호라고 부름)
5. 개인 키 사용
   - 서버는 예비 마스터 암호를 개인키로 복호화
6. 세션 키 생성
   - 클라이언트와 서버는 통신을 통해 얻은 아래 3가지를 이용해 세션키를 생성
      - Client Hello시 생성된 무작위 바이트 배열
      - Server Hello시 생성된 무작위 바이트 배열
      - 예비 마스터 암호
   - 클라이언트와 서버가 생성한 세션키는 동일해야 함
7. Client Finished (클라이언트 준비 완료)
   - 클라이언트는 세션키로 암호화된 "완료" 메세지를 전송
8. Server Finished (서버 준비 완료)
   - 서버는 세션키로 암호화된 "완료" 메세지를 전송
9. 안전한 대칭 암호화 성공

*RSA 키 교환 알고리즘은 안전하지 않으나 1.3이전 버전의 TLS에서는 사용되었음*

<br/>


### TLS 1.3 Handshake 
공격에 취약한 RSA 등의 암호 제품군 및 매개변수를 지원하지 않으며, 핸드쉐이크 과정이 단축되어 더 빠르고 안전함

1. Client Hello 
   - 클라이언트가 서버로 보내는 Handshake 요청 메세지
   - 클라이언트가 지원하는 TLS 버전, 지원 암호 알고리즘 목록, 무작위 바이트 배열이 메세지에 포함됨
   - 예비 마스터 암호도 메세지에 포함됨
2. 서버가 세션 키를 생성
   - 서버 측 무작위 바이트 배열을 생성
   - 클라이언트에게 전달 받은 무작위 바이트 배열, 예비 마스터 암호를 이용하여 세션키 생성
3. Server Hello 및 Server Finished (서버 준비 완료)
   - 서버가 클라이언트로 보내는 메세지
   - 서버의 SSL 인증서, 디지털 서명, 서버에서 선택한 암호 알고리즘, 무작위 바이트 배열이 메세지에 포함됨
   - 서버는 세션키를 생성하였으므로 암호화된 "완료" 메세지를 전송
4. Client Finished (클라이언트 준비 완료)
   - 클라이언트는 전달받은 정보를 통해 세션키를 생성
   - 클라이언트는 세션키로 암호화된 "완료" 메세지를 전송
5. 안전한 대칭 암호화 성공

<br/>



### 0-RTT 모드 (세션 재개시 사용되는 모드)
- TLS 1.3에서는 클라이언트와 서버가 이전에 서로 연결된 적이 있는 경우, 첫 번째 세션에서 "재개 주 암호"라고 하는 또 다른 공유 암호를 각각 도출할 수 있음
- 또한 서버에서는 이 첫 번째 세션 중에 세션 티켓이라고 불리는 것을 클라이언트에게 보냄
- 클라이언트는 이 공유 암호를 사용하여 다음 세션의 첫 번째 메시지에서 해당 세션 티켓과 함께 암호화된 데이터를 서버로 전송할 수 있음
- 이를 통해 클라이언트와 서버 간에 TLS가 재개

<br/>



# 참고자료
https://www.youtube.com/watch?v=j9QmMEWmcfo&ab_channel=ByteByteGo

https://opentutorials.org/course/228/4894

https://en.wikipedia.org/wiki/Certificate_authority

https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/

https://www.cloudflare.com/ko-kr/learning/ssl/how-does-ssl-work/

https://luavis.me/server/tls-1.3
https://www.a10networks.com/glossary/key-differences-between-tls-1-2-and-tls-1-3/
