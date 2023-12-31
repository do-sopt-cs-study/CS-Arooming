# 📑 인터럽트(Interrupt)와 시스템 콜(System Call): 입출력 및 예외 처리

## 🚧 운영체제의 동작 과정 - Dual Mode

- 운영체제가 서로 다른 두 개의 모드로 돌아가는 것.
    - 현대 운영체제의 대부분은 Dual Mode로 동작함.
- 사용자 모드 (User Mode): 일반적인 응용 프로그램이 구동되는 환경
- 커널 모드 (Kernel Mode): 커널이 구동되는 환경/ OS 시스템에 영향을 주는 명령어를 실행 가능한 환경

![](https://velog.velcdn.com/images/klm03025/post/cb461772-62ff-4ac5-ac24-ccf13f37843b/image.png)

<br />

## 🚧 인터럽트(Interrupt)

- 프로그램 실행 중 예기치 않은 상황이 발생한 경우
    - 현재 실행 중인 작업을 즉시 중단
    - 예외 상황 우선 처리
    - 실행 중이던 작업으로 복귀
    - 원래 실행 중이던 작업 계속 처리

<br />

### 👀 인터럽트 동작 과정

1. 현재 실행 중이던 명령어까지 수행 후, 프로그램 실행 중단
2. PC(Program Counter) 값을 스택에 저장 → 현재 프로그램의 상태 보존
    - PC(Program Counter)
        - 메모리에서 실행할 다음 명령어의 주소를 저장하는 레지스터
3. 인터럽트 처리루틴 실행
4. 인터럽트 서비스 루틴(ISR) 실행
    - 인터럽트의 원인을 파악하고 실질적인 작업 수행
    - 서비스 루틴 수행 중 우선 순위가 더 높은 인터럽트 발생 시, 재귀적으로 위의 과정(1~4) 수행
5. 인터럽트 발생 당시 저장해둔 PC 값 복구
6. 복구된 PC 값을 이용하여 이전에 수행 중이던 프로그램 재개 → 중단된 프로그램의 실행 재개

<br />

### 👀 인터럽트 종류

- 외부 인터럽트
    - 전원 이상 인터럽트
    - 기계 착오 인터럽트
    - 외부 신호 인터럽트
        - 타이머에 의한 인터럽트
        - 키보드로 인터럽트 키를 누른 경우 (ex. Control + Alt + Delete)
        - 외부 장치로부터의 인터럽트 요청
    - 입출력 인터럽트
        - 입출력 장치가 데이터 전송을 요구하거나 전송이 끝나 다음 동작이 수행되어야 할 경우
        - 입출력 데이터에 이상이 있는 경우
- 내부 인터럽트(트랩)
    - 잘못된 명령이나 데이터를 사용했을 때 발생(Program check interrupt 라고도 함)
        - Division by zero
        - Overflow/Underflow
        - 기타 프로그램 Exception

<br />


## 🚧 시스템 콜 (System Call)

- 프로세스가 시스템의 자원이나 서비스를 필요로 할 경우, 커널 모드로 접근하기 위한 인터페이스
    - 운영체제에게 커널 모드로의 접근을 요청하는 것
- 이 또한 인터럽트의 일종이며, 해당 인터럽트를 처리하기 위해 OS로 CPU 제어권이 넘어가고 OS 영역의 코드가 실행됨.
- 인터럽트 라인을 읽어서 커널 함수가 실행될 뿐, 사용자 프로그램에서 직접 OS 영역을 호출하진 않음 !

<br />

### 👀 시스템 콜 동작과정

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/c0ykkj/btsirSksbKj/p6Hjnf21IB07KzktzzRl50/img.png)

1. 사용자 프로세스가 시스템 콜 호출 (Kernel Mode 진입)
2. 커널은 내부적으로 시스템 콜을 구분하기 위해, 기능 별 고유번호 할당 → 할당 번호 별 제어 루틴 정의
3. 커널은 요청받은 시스템 콜에 대응하는 고유번호 확인 → 번호에 맞는 서비스 루틴 호출
4. 커널은 서비스 루틴 처리 → 사용자 모드(User Mode)로 전환

<br />

### 👀 시스템 콜 유형

- 프로세스 제어
    - 끝내기(exit), 중지(abort)
    - 적재(load), 실행(execute)
    - 프로세스 생성(create process) - fork
- 파일 조작
    - 읽기(read), 쓰기(write)
    - 열기(open), 닫기(close)
    - 파일 생성(create file), 파일 삭제(delete file)
- 장치 관리
    - 하드웨어의 제어와 상태 정보 얻음
    - 장치 요구, 장치 방출
    - 읽기(read), 쓰기(write)
- 정보 유지
    - getpid(), alrarm(), sleep()
- 통신
    - 통신 연결의 생성 및 제거
    - 메시지의 송신 및 수신