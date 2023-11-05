# Blocking, Non-blocking I/O: 동기식과 비동기식 I/O의 차이와 활용

## 👀 I/O

- 입력(Input)과 출력(Output)을 함께 일컫는 말.
- 파일 I/O 뿐만 아니라, 디바이스를 통해 입력과 출력이 이뤄지는 모든 작업.
- 네트워크를 통해 다른 서버로 데이터를 전송하거나, 다른 서버로부터 데이터를 전송받는 것 모두 포함.
- 심지어 콘솔을 찍는 것도 스트림을 통해 출력하는 것이기 때문에 I/O에 속함.
- I/O가 많아진다는 것 → 애플리케이션이 연산을 처리할 때까지 CPU가 아무것도 하지 못하고 대기하는 시간 길어짐 → 애플리케이션 처리 속도 느려짐.
    - 따라서 높은 성능을 보장해야하는 어플리케이션의 입장에서 I/O는 큰 장애물이 될 수 있음.

<br />

## 👀 Blocking I/O & Non Blocking I/O

> **Blocking I/O, Non Blocking I/O의 관심사: 호출되는 함수가 바로 리턴을 하는지 여부**
> 

<br />

### 🚧 Blocking I/O

- I/O 작업이 진행되는 동안 유저 프로세스(thread)가 자신의 작업을 중단한 채, I/O가 끝날 때까지 대기하는 방식.
    - 즉, I/O가 호출되면 제어권을 가져가서 애플리케이션이 멈춤.
- I/O 작업이 CPU 자원을 거의 사용하지 않기 때문에, resource 낭비가 심함.


![](https://velog.velcdn.com/images/aroo_ming/post/28084026-256b-46c3-91eb-cefd7c985717/image.png)

1. process(thread)가 커널에 I/O 요청하는 함수 호출
2. 커널 작업 완료 시, 작업 결과 반환 받음.

<br />

### 🚧 Non Blocking I/O
> **EWOULBLOCK**: 데이터가 없다는 메시지
**커널**: 컴퓨터 운영 체제의 핵심이 되는 컴퓨터 프로그램/ 시스템의 모든 것을 완전히 제어함.
> 

- A 함수가 I/O 작업 호출 시, 작업이 완료될 때까지 A 함수의 작업을 중단하지 않고, I/O 호출에 대해 즉시 리턴 → 해당 함수가 이어서 다른 일을 수행할 수 있도록 하는 방식.
- 즉, I/O가 호출되면 결과를 즉시 리턴하고, I/O가 완료될 때까지 대기하지 않음.
    - 애플리케이션이 제어권을 가진 채 계속 동작함. (필요한 경우 polling과 같은 상태확인 가능)

![](https://velog.velcdn.com/images/aroo_ming/post/df061073-0449-449c-a7f0-4319a7e5338e/image.png)

1. read I/O를 위해 system call 수행.
2. 커널의 I/O 작업 완료 여부에 무관하게 즉시 응답.
3. 즉, 커널이 시스템 콜을 받자마자 CPU 제어권을 다시 애플리케이션에 넘겨주고, 애플리케이션은 I/O 작업이 완료되기 전에 다른 작업 수행 가능.
4. 애플리케이션은 다른 작업들을 수행하다가 중간중간 시스템 콜을 보내서 I/O가 완료됐는지 커널에 물어보고 완료되면 I/O 작업 완료.

<br />


## 👀 Synchronous & Asynchronous

> **Synchronous & Asynchronous의 관심사: 호출되는 함수의 작업 완료 여부를 누가 신경쓰고 있는지**
> 

<br />

### 🚧 Synchronous (동기)

- 호출하는 함수(A)가 호출되는 함수(B)의 작업 완료 후 리턴을 기다리는 경우
- 호출되는 함수(B)로부터 바로 리턴을 받더라도, 호출하는 함수(A) 스스로 작업 완료 여부를 계속 확인하며 신경쓰는 경우


<br />

### 🚧 Asynchronous (비동기)

- 호출되는 함수(B)에게 callback 전달 → 호출되는 함수(B)의 작업 완료 → 호출되는 함수(B)가 전달받은 callback 실행 → 호출하는 함수(A)는 작업 완료 여부 신경쓰지 않는 경우

<br />

## 👀 Sync-NonBlocking

![](https://velog.velcdn.com/images/aroo_ming/post/40148203-cc0d-4ab8-9db4-d5919964e4a0/image.png)


- 앞서 살펴본 개념에 의하면, 호출되는 함수는 바로 리턴하고, 호출하는 함수는 작업완료 여부를 신경 씀.
- NonBlocking 메소드 호출 후 바로 반환 받아서 다른 작업을 할 수 있음.
- 그러나 메소드 호출에 의해 수행되는 작업이 완료된 것은 아니며, 호출하는 메소드가 호출되는 메소드 쪽에 작업 완료 여부 계속 문의.

<br />

## 👀 Async-Blocking

![](https://velog.velcdn.com/images/aroo_ming/post/f6acc4d3-71ae-4b60-b653-d0165dd245fd/image.png)

- 어차피 blocking 되어서 대기해야 하는데, 비동기적으로 처리하는 것의 이점을 가져갈 수 있을까?
    - Async-Blocking은 이점이 별로 없어서 이 방식을 사용할 필요가 크게 없긴 하지만, 의도치 않게 Async-Blocking 방식으로 사용하게 되는 경우가 있다고 함.
        - Node.js와 MySQL 조합의 경우, 원래는 Async-NonBlocking을 추구하다가 의도치 않게 Async-Blocking이 되어버리곤 함 ..!
- Async-Blocking은 별다른 장점이 없어서 일부러 사용할 필요는 없지만, **Async-NonBlocking 방식 사용 중 하나라도 Blocking으로 동작하는 아이가 포함되어 있다면 의도치 않게 Async-Blocking으로 동작**할 수 있음 !!

<br />

## 📍 정리

1. Blocking/ NonBlocking은 호출되는 함수가 바로 리턴 하는지 여부가 관심사
    - 바로 리턴하지 않으면 Blocking
    - 바로 리턴하면 NonBlocking
2. Synchronous/ Asynchronous는 호출되는 함수의 작업 완료 여부를 누가 신경쓰느냐가 관심사
    - 호출되는 함수의 작업 완료 여부를 호출한 함수가 신경쓰면 Synchronous
    - 호출되는 함수의 작업 완료 여부를 호출된 함수가 신경쓰면 Asynchronous
3. 성능과 자원의 효율적 사용 관점에서 가장 유리한 모델은 Async-NonBlocking 모델임!
    
    ![](https://velog.velcdn.com/images/aroo_ming/post/79d7edb0-cb6e-41e2-a59a-e1c5fb106742/image.png)
