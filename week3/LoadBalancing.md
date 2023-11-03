# 📑 로드 밸런싱(Load Balancing): 서버 부하 분산 기술
## 👀 부하 분산(Load Balancing)

- 애플리케이션을 지원하는 리소스 풀 전체에 네트워크 트래픽을 균등하게 배포하는 방식.

> **컴퓨터공학에서의 로드 밸런싱**
> 
> - 부하분산 또는 로드 밸런싱(Load balancing)은 컴퓨터 네트워크 기술의 일종으로 둘 혹은 셋 이상의 중앙처리장치 혹은 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것을 의미한다. 이로써 가용성 및 답 시간을 최적화시킬 수 있다.

<br />

## 👀 서버 부하 분산(Server Load Balancing)

![로드 밸런싱](https://velog.velcdn.com/images/aroo_ming/post/573d58a6-014e-48da-8696-22d1c157646f/image.png)

- 외부의 사용자로부터 들어오는 다수의 요청을 서버들에 적절히 배분하여 각각의 서버들이 요청을 처리하게 하는 것.
- 즉, 서버가 처리해야 할 업무 혹은 요청(load)을 여러 대의 서버로 나눠(balancing) 처리하는 것을 의미함.
- 한 대의 서버로 부하가 집중되지 않도록 트래픽 관리 → 각각의 서버가 최적의 성능을 내게 하는 목적 !

> **Load Balancer**: cloud 상에서 서버 부하 분산을 담당하는 Network Switch를 부르는 말.
**LB, Load Balancing**: 현업에서 서버 부하 분산을 칭하는 말.
> 

<br />

## 👀 서버 부하 분산 방법(Load Balancing Method)

: 서버의 능력을 고려하여 분배해야 서버가 죽지 않기 때문에, 서버의 상황에 맞춰 적절한 방법을 선택해야 함.

### 🥽 Round Robin
![Round Robin](https://velog.velcdn.com/images/aroo_ming/post/1dfd463b-17a3-4550-addb-7b2624ab24aa/image.png)

- 로드 밸런서가 다수의 서버에게 순서대로 요청을 할당하는 방법.
- 가장 단순한 방법으로, 서버군에 차례대로 요청을 할당하여 분산함.

### 🥽 Least Connection
![Least Connection](https://velog.velcdn.com/images/aroo_ming/post/7ed6f699-c327-4874-b10a-c436cb3a3bde/image.png)

- 로드 밸런서가 서버에게 요청을 전달한 뒤, 사용자와 서버가 정상적인 연결을 맺으면 connection 생성
- 로드 밸런서 또한 중간자로서 connection 정보를 갖고 있음
- connection 수 정보를 기반으로 서버에게 요청 전달.
- 이때, connection이 가장 적은 서버 === 부하가 가장 덜한 서버를 의미, 부하가 가장 덜한 서버에 요청을 전달함 !

### 🥽 Ratio(가중치)
![Ratio](https://velog.velcdn.com/images/aroo_ming/post/d80e65fb-5720-4170-926a-b8ab113a185b/image.png)

- 서버의 처리 능력을 고려하여 할당 가능한 connection 비율을 미리 정해둠.
- 서버 부하 분산 비율이 100%라고 가정했을 때, 성능이 가장 떨어지는 서버에게 10%를 나머지 서버에게 각각 30%를 할당함.

### 🥽 Fastest(Response Time)

- 응답속도가 가장 빠른 서버에게 우선적으로 할당하는 방식.
    - 서버에 할당된 connection이 5개, 서버의 response도 5개인 경우
        - connection에 대해 모두 응답 → 성능이 충분하다고 판단 → 서버에 추가 요청 보냄.
    - 서버에 할당된 connection이 10개, 서버의 response는 5개인 경우
        - 현재 성능이 충분하지 않아 제대로 답변하지 못한다고 판단 → 서버에 추가 요청 보내지 않음.