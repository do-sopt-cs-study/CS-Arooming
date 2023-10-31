# 📑 TLS/SSL 프로토콜의 역할과 handshake: 암호화된 통신 설정 과정

## 💻 TLS (Transport Layer Security)

- 전송 계층 보안으로, 인터넷 상의 커뮤니케이션을 위한 개인 정보와 데이터 보안을 용이하게 하기 위해 설계된 보안 프로토콜
- 네트워크 상에서 보안을 제공하는 암호화 프로토콜을 사용해 네트워크에 보안을 제공함으로써, 개인정보 보호를 보장하는 클라이언트/ 서버 프로토콜
- 전송 중인 정보를 보호함으로써 안전하지 않은 인프라를 통해 통신을 보호함.
    - 웹 사이트를 로드하는 웹 브라우저와 같이 웹 응용 프로그램과 서버 간의 커뮤니케이션을 암호화에 사용됨.
    - 이메일, 메시지, 보이스오버 IP(VoIP) 등 다른 커뮤니케이션을 암호화하기 위해 사용
- TLS 버전 1.3은 최근에 활발히 사용되는 암호화 프로토콜 버전으로, 이제는 사용하지 않는 SSL 버전 3.0 프로토콜보다 향상된 보안 기능과 성능을 다수 제공함.

<br />

### 🌟 TLS 프로토콜 역할

- TLS 프로토콜은 주로 암호화, 인증, 무결성이라는 세 가지 역할을 수행함.
    - `암호화`: 제 3자로부터 전송되는 데이터를 숨김.
    - `인증`: 정보를 교환하는 당사자가 요청된 당사자임을 보장함.
    - `무결성`: 데이터가 위조되거나 변조되지 않았는지 확인함.
- 서버와 클라이언트가 TLS로 통신을 할 때, 어떠한 제 3자도 메시지를 변형하거나 감청할 수 없도록 함.

---

<br />

## 💻 SSL (Secure Sockets Layer)

- 클라이언트와 서버  간의 안전한 링크를 통해 송수신되는 모든 데이터를 안전하게 보장하는 과거의 보안 표준 기술이었음.
- 보안 취약점으로 인해 현재는 사용하지 않는 암호화 프로토콜

<br />

### 🌟 SSL 프로토콜 역할

- SSL이 적용되지 않은 통신의 경우, 평문(plain-text)이 그대로 전송됨.
    - 만약 제 3자가 통신 패킷을 탈취할 경우, 그 내용을 쉽게 확인할 수 있음.
- SSL을 적용한다면 요청을 암호화해서 보내기 때문에, 통신 패킷이 탈취되어도 복호화 키가 없으면 원래 내용을 알 수 없게되어 평문이 노출되는 문제를 기술적으로 해결 가능함.

---

<br />

## 💻 ****TLS/SSL HandShake****

> handshake: 클라이언트와 서버 간의 요청/ 응답을 반복하며 통신에 필요한 사전 작업을 하는게 마치 악수 같아 이러한 용어가 붙여졌다고 한다.
> 

> HTTPS에서 클라이언트와 서버 간 통신 전, SSL 인증서로 신뢰성 여부를 판단하기 위해 연결하는 방식
> 

1. **`ClientHello 요청`**
    - 클라이언트가 특정 주소에 접근하면, 해당하는 서버에 요청을 보냄.
        - ex. NAVER에 접근 시, 네이버 서버에 요청 전송
    - 클라이언트의 주요 정보를 서버에 전송.
        - 난수 데이터
        - 암호화 프로토콜 정보
        - 클라이언트가 사용 가능한 암호화 기법
        - 세션 ID
        - 기타 확장 정보
    - 해당 클라이언트를 식별, 어떤 암호화를 사용할 수 있는지 등의 정보를 서버가 인지하도록 함.

2. **`ServerHello 응답`**
    - 서버가 ClientHello 요청을 받으면, 아래의 정보를 담아 클라이언트에게 응답을 보냄.
        - 난수 데이터(ClientHello의 난수 데이터와 다름)
        - 서버가 사용할 암호화 기법
        - 인증서
            - CA, 도메인, 공개 키
    - 클라이언트에서 사용 가능한 암호화 기법 중 서버에서 활용할 암호화 기법을 전달하여 동일한 암호화 기법으로 송수신할 수 있도록 선언.
    - 인증서 정보와 함께, 서버와의 암호화 통신을 위한 서버 공개키가 전달됨.
    - 서버의 공개키로 데이터를 암호화 → 서버는 개인 키로 복호화 → 요청 분석

3. **`인증서 검토`**
    - 서버가 전달한 인증서가 실제 해당 서버의 인증서인지, 신뢰할 수 있는 CA에서 발급된 것인지, 실제 해당 CA에서 발급받았는지 등 인증서 검토
        - 인증서에 이상이 없다면, `연결이 안전`하다는 창을 볼 수 있음.
        - 인증서에 문제가 있다면, `비공개 연결이 아닙니다.` 와 같은 경고문을 볼 수 있음.
            - 이는 브라우저가 사용자에게 보내는 경고문으로, 해당 사이트의 인증서가 올바르지 않음을 의미.

4. **`Premaster Secret 송수신`**
    - ClientHello, ServerHello에서 송수신한 난수 데이터를 조합하여 Premaster Secret 생성.
    - Premaster Secret을 ServerHello에서 전달받았던 공개 키로 암호화.
    - 해당 데이터는 서버가 가진 개인 키로만 복호화 가능 → 데이터 탈취 시에도 내용 보호 가능.
    - 서버: 수신된 데이터 복호화 → 클라이언트와 동일한 Premaster Secret 저장 가능.

5. **`통신 키 생성`**
    - 보유한 Premaster Secret을 기반으로 Master Secret, Session Key 생성.
    - 이를 통해, 클라이언트와 서버가 동일한 키를 보유하게됨.
    - 클-서 암호화 통신 가능.
    
6. **`데이터 송수신`**
    - 저장된 Session Key를 통한 대칭 키 암호화 방식으로 암/복호화하여 통신.

7. **`세션 종료`**
    - 클라이언트와의 연결이 끝났을 경우, Session Key 폐기.