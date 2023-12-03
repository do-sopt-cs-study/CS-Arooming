# 📑 세마포어(Semaphore)와 뮤텍스(Mutex): 공유 자원 동기화 메커니즘

<br />

## 🔥 임계 영역(Critical Section)

- 공유 자원에 여러 프로세스가 동시에 접근하면서 문제가 발생할 수 있음.
- 공유 자원의 데이터는 한 번에 하나의 프로세스만 접근할 수 있도록 제한을 둬야 함.
- `임계 영역`: 각 프로세스가 접근하는 공유 데이터 부분

<br />

## 🔥 세마포어(Semaphore)

- 멀티 프로세스 환경에서 공유 자원에 대한 접근을 제한하는 방법
- 세마 포어의 P, V 연산
    - `P` 연산: Wait 동작/ 임계 영역에 `들어가기 전`에 수행(프로세스 진입 여부를 자원의 개수 S를 통해 결정)
    - `V` 연산: Signal 동작/ 임계 영역에서 `나올 때`  수행(자원 반납 알림, 대기 중인 프로세스를 깨우는 신호)

     <br />

     ```tsx
    procedure P(S)   --> 최초 S값은 1임
        while S=0 do wait  --> S가 0면 1이 될때까지 기다려야 함
        S := S-1   --> S를 0로 만들어 다른 프로세스가 들어 오지 못하도록 함
    end P
    
    --- 임계 구역 ---
    
    procedure V(S) --> 현재상태는 S가 0임
        S := S+1   --> S를 1로 원위치시켜 해제하는 과정
    end V
    ```
    
    - 한 프로세스가 P/ V를 수행하고 있는 동안 프로세스 인터럽트 방지 가능
    - P와 V를 사용하여 임계 영역에 대한 상호 배제 구현 가능

    <br />
    
    ![스크린샷 2023-12-03 오후 7.36.25.png](https://velog.velcdn.com/images/aroo_ming/post/57dcfa96-3538-43d3-8852-c9004b071fc8/image.png)
    
    - 초기 상태 (빈 방 3개)

     <br />

    
    ![스크린샷 2023-12-03 오후 7.37.30.png](https://velog.velcdn.com/images/aroo_ming/post/a4f8a464-c10a-4791-ad1d-62581b1ddce0/image.png)
    
    - P 연산 후, Toilet(임계 영역 이용)

     <br />
    
    ![스크린샷 2023-12-03 오후 7.38.16.png](https://velog.velcdn.com/images/aroo_ming/post/cc754dd4-2905-4c60-b582-294393ed4854/image.png)
    
    - 모든 Toilet이 사용중 → 대기자 발생

     <br />
    
    ![스크린샷 2023-12-03 오후 7.38.53.png](https://velog.velcdn.com/images/aroo_ming/post/b0fc1341-2427-4852-9762-9740c3d66de5/image.png)
    
    - 세 번째 Toilet 사용을 끝내고 V연산 → 대기하던 사람이 P연산 수행 후, Toilet 사용

     <br />

## 🔥 뮤텍스

- 멀티 프로그래밍에서 공유 불가능한 자원의 동시 사용을 피하기 위해 사용하는 알고리즘
- 임계구역을 가진 스레드들의 실행 시간(Running Time)이 서로 겹치지 않고 각각 단독으로 실행되게 하는 기술
    - Mutual Exclustion(상호 배제)의 약자
    - 한 프로세스에 의해 소유될 수 있는 key를 기반으로 한 상호배제 기법
        - key에 해당하는 어떤 객체(object)가 있으며, 이 객체를 소유한 스레드/ 프로세스만이 공유 자원에 접근 가능함.
        - 즉, 뮤텍스 객체를 두 스레드가 동시에 사용할 수 없음.
- 해당 접근을 조율하기 위해 lock과 unlock을 사용함.
    - lock: 현재 임계 구역에 들어갈 권한을 얻어옴. (이미 점유 중이라면 종료할 때까지 대기)
    - unlock: 현재 임계 구역을 모두 사용했음을 알림.
- 뮤텍스의 상태는 0, 1로 **이진 세마포어**로 부르기도 함.

<br />

![스크린샷 2023-12-03 오후 7.41.32.png](https://velog.velcdn.com/images/aroo_ming/post/ee994fbb-1a29-4d62-bb6f-d9be1e7b1fd3/image.png)

- Toilet(임계 영역)을 이미 점유하고 있기 때문에 임계 영역 사용자는 lock 상태가 됨.

<br />

![스크린샷 2023-12-03 오후 7.42.34.png](https://velog.velcdn.com/images/aroo_ming/post/e718bbbd-1e55-4881-8b6d-af7c1c114fcf/image.png)

- Toilet을 사용하고 싶은 사람 증가.
- lock 상태이기 때문에 unlock 상태로 바뀔 때까지 기다려야 함.

<br />

![스크린샷 2023-12-03 오후 7.44.01.png](https://velog.velcdn.com/images/aroo_ming/post/57989413-c2e4-4a53-8cba-6f0e5ceae36e/image.png)

- unlock 상태가 되면, 다은 차례가 lock을 한 다음, Toilet을 사용함.