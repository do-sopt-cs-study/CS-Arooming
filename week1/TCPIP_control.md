# 📑 TCP/IP 흐름제어와 혼잡제어: 데이터 전송 관리

## 🌟 TCP/IP 란?

컴퓨터와 컴퓨터 간의 LAN 또는 WAN에서 원활한 통신을 가능하게 하기 위한 통신 규약이다.

<br />

`TCP 통신`은 클라이언트와 서버가 연결된 상태에서 데이터를 주고 받는 연결 지향적 프로토콜로, 가장 큰 특징은 `신뢰성`이다.

신뢰성이란, 통신 중간에 데이터가 유실되지 않는 것을 뜻한다. 

신뢰성을 구성하는 대표적인 방법으로는 `흐름제어`와 `혼잡제어`가 있다.

<br />

---

## 🌟 흐름 제어 (End to End)

흐름 제어는 송신 측과 수신 측의 `데이터 처리 속도를 일치`시키는 기법이다.

수신 측이 송신 측보다 데이터 처리 속도가 느릴 경우, 데이터를 손실할 위험이 존재하게 된다. 흐름 제어는 송신 측에서 전송하는 데이터의 양을 제어하는 방식으로 데이터 처리속도를 일치시킨다. 

즉, 흐름 제어는 `송신 측과 수신 측 사이의 **패킷** 수를 제어`하는 기능이라고 할 수 있다.

→ `패킷` (Packet): 인터넷에서 라우팅을 효과적으로 하기 위해 나눈 데이터 조각

<br />

**[흐름 제어가 필요한 상황]**

- 송신 측 데이터를 전송량 > 수신 측 데이터 처리량
    - 수신자의 버퍼에 미처 처리하지 못한 데이터가 쌓임
    - 수신자의 버퍼 가득 참 → 버퍼가 가득 차있는 동안 전달된 데이터 손실 위험

<br />

**[흐름 제어의 해결 방식]**

- 송신자의 데이터 전송 속도 조절
    - 수신자가 데이터를 처리하는 속도만큼 송신자가 데이터를 천천히 보내게 하여 문제 해결

<br />

### 👣 Stop and Wait (→ 비효율적)

- 매번 전송한 패킷에 대해 확인 응답(ACK)을 받으면 다음 패킷을 전송하는 방법.
- 패킷을 하나씩 보내기 때문에, `비효율적인 방식`!


![](https://velog.velcdn.com/images/aroo_ming/post/6826249c-04d8-4a9d-b61f-7eace2f9d4d9/image.png)


💦 **데이터 보낼 때마다 아래 과정 반복** 💦

1. 송신자 → [데이터] → 수신자
2. 수신자 → [남은 버퍼 공간] → 송신자 (데이터 받을 때마다 알려줌)
3. 송신자 → [수신자가 보낸 정보] → 자신의 데이터 전송 속도 조절

⇒ 매번 전송한 패킷에 대한 확인 응답을 받아야 다음 패킷 전송 가능한 구조.

⇒ 구현은 간단하지만 매우 비효율적 !!

<br />

### 👣 Sliding Window 방식 (→ Stop and Wait 방식 개선)

- 데이터를 하나만 보내고 기다리는 것이 아닌, 한 번에 여러 개의 데이터 전송 → 수신자의 응답 기다림
- 송신 측이 수신 측에서 받은 **윈도우 크기**를 참고해서 데이터의 흐름을 제어하는 방식
    - `윈도우 크기`: 단위 시간 내에 보내는 패킷 수


![](https://velog.velcdn.com/images/aroo_ming/post/8486af38-b720-4596-b2b4-73e1f0831d8d/image.png)


💦 **데이터를 다 받을 때까지 아래 과정 반복** 💦

1. 수신자 → 최초 윈도우 크기를 7로 설정

1. 송신자 → 수신자의 확인 응답(ACK)을 받기 전까지 데이터 전송
    - 재전송
        - 일정 시간동안 수신 측으로부터 확인 응답(ACK)을 받지 못하면 패킷 재전송
        - 송신 측 재전송 → 수신 측 버퍼의 남은 공간 없는 경우 → 문제 발생
            - 이를 해결하기 위해, 확인 응답(ACK)을 보내면서 남은 버퍼의 크기(윈도우 크기)도 함께 전송

1. 수신자 → 확인 응답(ACK) → 송신자에게 전송 → 윈도우 크기를 충족할 수 있도록 윈도우를 옆으로 이동 시킴.

<br />

---

## 🌟 혼잡 제어 (End to Network)

혼잡 제어는 송신 측의 데이터 전달과 `네트워크의 데이터 처리 속도 차이를 해결`하기 위한 기법이다.

데이터의 양이 라우터가 처리할 수 있는 양을 초과할 경우, 라우터는 데이터를 처리하지 못하고 송신 측에서는 해당 데이터를 손실 데이터로 간주하게 된다. 혼잡 제어는 송신 측의 데이터 전송 속도를 적절히 조절하여 데이터 처리 속도 차이를 해결한다.

즉, 혼잡 제어는 `네트워크 내의 패킷 수를 조절`하여 네트워크의 오버 플로우를 방지하는 기능이라고 할 수 있다.

<br />

**[혼잡 제어가 필요한 상황]**

- 송신자가 사용하는 네트워크에 엄청나게 많은 인원이 데이터를 전송하는 경우
    - 라우터가 데이터를 처리하는 속도보다 많은 양의 데이터가 들어오면 문제 발생
    - 송신자 → **라우터**가 처리하지 못한 데이터를 잃어버린 것으로 간주 → 계속해서 데이터 재전송
        - `라우터`(router): 네트워크에서 송신자가 보낸 데이터를 알맞은 수신자에게 보내주는 장치/ 데이터 처리 속도와 처리할 데이터를 보관해두는 버퍼 존재
    - 이러한 과정이 반복되면 네트워크는 점점 혼잡해짐.

<br />

**[혼잡 제어 해결 방식]**

- 송신자의 데이터 전송 속도 조절
    - 네트워크에 전송되는 데이터가 과도하게 증가하는 현상 방지

<br />

### 👣 AIMD(Additive Increase/ Multiplicative Decrease)

- 패킷을 하나씩 전송하다가 문제없이 도착 시, 윈도우 크기 1씩 증가시켜가며 전송하는 방법
- 패킷 전송에 실패 시, 네트워크가 혼잡하다 판단 → 패킷 전송 속도 절반으로 줄임.

![](https://velog.velcdn.com/images/aroo_ming/post/11ae571d-a22b-42bf-abbe-894eebec3585/image.png)


💦 **특징 💦**

- 공평한 방식
- 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 불리하지만, 시간이지나면 평형상태로 수렴.

💦 **문제점 💦**

- 초기 네트워크의 높은 대역폭을 사용하지 못함 → 오랜 시간 소요 → 네트워크가 혼잡해지는 상황을 미리 감지하지 못함.
- 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식

<br />

### 👣 Slow Start (느린 시작)

- AIMD 방식: 네트워크의 수용량 주변에서는 효율적으로 작동하지만, 초기 전송 속도를 올리는데 오랜 시간이 소요되는 단점 존재.
- 패킷을 하나씩 전송하다가 문제없이 도착 시, 각각의 ACK 패킷마다 윈도우 크기 1씩 증가시킴.
    - 한 주기가 지나면 윈도우 크기는 2배가 됨.
- 전송 속도는 AIMD에 반해 지수 함수 꼴로 증가
    - 단, 혼잡 현상 발생 시, 윈도우 크기를 1로 떨어뜨림.

![](https://velog.velcdn.com/images/aroo_ming/post/a3cba096-073b-44eb-bf3e-a6e70e628bfa/image.png)


💦 **특징 💦**

- 초기 네트워크의 수용량을 예상할 수 있는 정보 없음.
- 그러나 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느정도 예상 가능
- ACK가 도착할 때마다 윈도우 크기를 증가 시킴
    - 초기 윈도우 크기가 느리게 증가할 지 라도, 시간이 지날 수록 윈도우 크기가 점점 빠르게 증가!
    - 혼잡 현상이 발생했던 윈도우 크기의 절반까지는 지수 함수 꼴로 창 크기 증가 → 완만하게 1씩 증가

<br />

### 👣 Fast Retransmit (빠른 재전송)

- TCP의 혼합 조절에 추가된 정책.
- 패킷을 받는 쪽에서 먼저 도착 해야할 패킷이 도착하기 전, 다음 패킷이 도착한 경우에도 ACK 패킷 보냄.
- 순서대로 잘 도착한 마지막 패킷의 다음 패킷 순번을 ACK 패킷에 실어서 보냄.
    - 중간에 하나의 패킷 손실 → 송신 측: 순번이 중복된 ACK 패킷 받음.
    - 이를 감지하는 순간, 문제가 되는 순번의 패킷 재전송.
- 중복된 순번의 패킷을 3개 받으면 재전송.
    - 혼잡한 상황 감지 → 윈도우 크기 줄임.

![](https://velog.velcdn.com/images/aroo_ming/post/857562eb-ffa7-4a91-b242-2657880eb878/image.png)

💦 **특징 💦**

- 송신 측은 자신이 설정한 타임아웃 시간이 지나지 않았어도 바로 손실 패킷 재전송 가능
    - 보다 빠른 재전송률 유지 가능

<br />

### 👣 Fast Recovery (빠른 회복)

- 혼잡한 상태 → 윈도우 크기를 1이 아니라 반으로 줄이고, 선형 증가시킴.
- 혼잡 상황을 한 번 겪은 이후로는 AIMD 방식으로 동작.

<br />

 

---

## 🌟 마무리

흐름 제어와 혼잡 제어는 모두 `TCP의 신뢰성있는 데이터 전송`을 위한 기능이다.

두 기능의 공통점은 송신자의 전송 속도를 제어함으로써 데이터 전송 시의 오류를 줄이는 기능이라는 것이다.

두 기능의 차이점은 **흐름 제어**는 **송신 측과 수신 측**의 패킷 수를 제어하는 기능이고, **혼잡 제어**는 **네트워크 내**의 패킷 수를 조절하는 기능이라는 것이다.