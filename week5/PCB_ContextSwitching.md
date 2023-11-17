# 📑 PCB와 Context Switching: 프로세스 정보 저장 및 전환

## 🚧 프로세스 관리

- 구독중인 프로세스가 여러 개일 때, CPU 스케줄링을 통해 프로세스를 관리하는 것
- CPU가 각 프로세스에 대해 구분할 수 있어야 관리가 가능하기 때문에, 프로세스 별 본연의 특징을 담고 있는 **Process Metadata**를 활용함.
    - Process Metadata가 담고있는 정보
        - 프로세스 고유 ID (PID)
        - 프로세스 상태
        - 프로세스 우선순위
        - Program Counter (PC)
        - CPU 레지스터
        - Owner
        - Memory Limit
        - etc ..

        <br />

## 🚧 PCB (Process Control Block)

![](https://velog.velcdn.com/images%2Fhaero_kim%2Fpost%2Fdd3d9ba6-bb62-4ade-bce7-341a2e1a3751%2F25673A5058F211C224.png)

- 프로세스 생성 시, 프로세스의 메타데이터를 저장하는 곳
- 하나의 PCB → 한 프로세스의 정보가 담김.
- 프로그램 실행 → 메모리 적재 시, 프로세스 생성 → 프로세스 주소공간에 코드, 데이터, 스택 생성 → 해당 프로세스의 메타데이터가 PCB에 저장됨.

<br />

### 👀 PCB가 필요한 경우

- 프로세스 상태에 따른 `CPU 교체 작업 발생` 시, 앞으로 `다시 수행할 block 상태의 프로세스 값을 저장`해두기 위해 PCB가 필요함.
    - **CPU 교체 작업이 발생**한다는 것?
        - 인터럽트 발생 → 할당받은 프로세스가 block 상태로 변경 → 다른 프로세스가 running으로 바뀌는 경우

<br />

### 👀 PCB의 관리 방식

- Linked List 방식
    - Array List와 달리, 엘리먼트와 엘리먼트 간의 연결(link)를 통해서 리스트를 구현한 것
        
        ![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20200922124319/Singly-Linked-List1.png)
        
- PCB Linked Head에 추가 생성된 PCB들이 붙음
    - 주소 값 연결이 이루어져있는 형태이기 때문에, 삽입/ 삭제에 용이
- 즉, 프로세스 생성 시, PCB 생성 → 프로세스 완료 시, 해당 PCB 제거

<br />

## 🚧 Context Switching

![](https://images.velog.io/images/haero_kim/post/bbc56488-d299-4c5d-9f1c-c9808b6f016b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-08-03%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.06.12.png)

- 수행중인 Task(Process/ Thread) 변경 시, CPU의 레지스터 정보가 변경되는 것.
- 이전 프로세스 상태를 PCB에 보관 → 다른 프로세스 정보를 PCB에서 읽어옴 → CPU 레지스터에 적재하는 과정

<br />

### 👀 Context Switching이 발생하는 경우

- 인터럽트 발생
- 실행 중인 CPU의 사용 허가시간을 모두 소모한 경우
- 입출력을 위해 대기해야하는 경우
- 즉, `프로세스의 상태 변경 시`(Ready → Running, Running → Ready, Running → Waiting 등) 발생!

<br />

### 👀 Context Switching Overhead

- Context Switching 기본 플로우
    - P0 수행 → P1 → P1 수행 → P0 수행 …
    - 계속해서 프로세스 교체가 발생함.
        - 현재 수행되던 프로세스 메모리에 저장 → 다음 수행할 프로세스 CPU에 넣는 과정이 계속 반복됨.
        - 이런 복잡한 과정을 왜 하는 걸까? 한 번에 P0 수행 → 나머지 P1 수행 이렇게 하면 되는 걸까?

- 결과부터 말하자면, 이는 CPU가 계속 일을 해서 사용자에게 빠르게 일처리를 해주기 위한 과정임.
    - 프로세스 수행 중 입출력 이벤트가 발생하게 되면 해당 CPU를 사용하지 않게 됨 → CPU 낭비
    - 차라리 overhead(번거로운 일)가 발생하더라도 Context Switching을 통해 다른 프로세스를 실행시키자 !
    - 효율성을 위해 overhead를 감수하고 Context Switching 수행하게 됨.