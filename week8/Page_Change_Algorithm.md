# 📑 페이지 교체 알고리즘: 메모리 페이지 교체 전략

## ✅ 페이지 교체 알고리즘

- 프로세스가 요구한 페이지가 현재 메모리에 없는 경우, 페이지 부재(page fault) 발생
- 페이지 부재 발생 시, 스왑 영역에서 메모리로 페이지를 가져옴.
    - 메모리가 꽉 찬 경우
        - 메모리에 있는 페이지 → 스왑 영역으로 보내야 함.
- 페이지 교체 알고리즘
    - 스왑 영역으로 보낼 페이지를 결정하는 알고리즘
    - 메모리에서 앞으로 사용할 가능성이 적은 페이지를 **대상 페이지**로 선정하여 페이지 부재를 줄이고 시스템 성능을 향상함.
    - 페이지 부재율을 최소화하는 것이 목표

### 🥎 FIFO(First In First Out) 알고리즘

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bGqpiD/btrpdovhuvd/9JIyEkk7DUmK6uCeO2ud40/img.png)

- 가장 먼저 메모리에 올라온 페이지를 먼저 내보내는 알고리즘
- 구현이 간단하지만 성능은 좋지 않음.
- 들어온 시간을 저장하거나 올라온 순서를 `큐`를 이용하여 저장 가능
- `Belady`s Anomaly` 현상이 발생 가능
    - 프레임의 수가 많아져도 페이지 부재율이 줄어들지 않고 늘어나는 현상
    - 직관적으로는 프레임의 개수가 많아지면 페이지 부재율이 줄어들어야 하지만, 그와 반대로 프레임 개수가 증가함에 따라 페이지 부재율도 늘어나는 현상을 의미함.
    

### 🥎 OPT(Optimal) 알고리즘

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/etkbwZ/btro8wOpRuo/yrVmZdpsBI8PienpO92bc0/img.png)

- 앞으로 가장 오랫동안 사용하지 않을 페이지를 교체하는 알고리즘
    - 앞으로 사용할 페이지를 미리 알아야 함 → 사실상 불가능

- 모든 페이지 교체 알고리즘 중 페이지 부재율이 가장 낮음.
- Belady`s Anomaly 현상 발생 ❌
- 실제 구현 가능성이 가장 낮은 알고리즘 (실현 거의 불가능)
    - 실제 사용보다는 연구 목적으로 사용됨.

### 🥎 LRU(Least Recently Used) 알고리즘

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/4z1Wa/btro38mZn1V/2k9TKkjOJiGm7A9LkCndR1/img.png)

- 가장 오랫동안 사용하지 않은 페이지를 교체하는 알고리즘
    - 가정: 가장 오랫동안 사용하지 않았던 데이터라면 앞으로도 사용할 확률이 적을 것이다.
    - `시간 지역성(temporal locality)` 성질을 고려함.
        - **시간 지역성(temporal locality)**: 최근 참조된 페이지가 미래에 다시 참조될 가능성이 높은 성질
- 사용된 시간을 알 수 있는 부분 저장 → 가장 오랫동안 참조되지 않는 페이지 제거(페이지마다 `카운터` 필요)
    - **카운터**: 각 페이지 별로 존재
        - 해당 페이지가 사용될 때마다 0으로 클리어 → 시간 증가 → 시간이 가장 오래된 페이지 교체
- 큐로 구현 가능
    - 사용한 데이터를 큐에서 제거 → 맨 위로 올림 → 프레임이 모자른 경우 맨 아래 데이터 삭제
- OPT 알고리즘(최적 알고리즘)과 비슷한 효과를 낼 수 있음.
- 많은 운영체제가 체택하는 알고리즘
- 단점
    - 프로세스가 주기억장치에 접근할 때마다 참조된 페이지 시간을 기록해야 함.
        - 막대한 오버헤드 발생
        - 카운터나 큐, 스택 같은 별도의 하드웨어 필요

### 🥎 LFU(Least Frequently Used) 알고리즘

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/k3rlo/btro8xT5B3y/DU9CSseZMbyWRwuo6X230K/img.png)

- 참조횟수가 가장 적은 페이지를 교체하는 알고리즘
- 교체 대상이 여러 개인 경우, 가장 오랫동안 사용하지 않은 페이지를 교체함.
- 단점
    - 가장 최근에 불러온 페이지가 교체될 수 있음.
    - 막대한 오버헤드 발생

### 🥎 MFU(Most Frequently Used) 알고리즘

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bcvcNG/btroVqoH7Ic/RFGqK4Lsiqls1FfXGN4Fs1/img.png)

- 가장 참조 횟수가 많은 페이지를 교체하는 알고리즘
    - 가정: 가장 많이 사용된 페이지가 앞으로는 사용되지 않을 것이다.

### 🥎 NUR(Not Used Recently) 알고리즘/ 클럭(Clock) 알고리즘

- NRU(Not Recently Used)라고도 함.
- 하드웨어적인 자원을 통해 기존 (LRU, LFU)알고리즘의 소프트웨어적인 운영 오버헤드를 줄인 방식
- LRU 알고리즘과 공통점
    - 가장 최근에 참조되지 않은 페이지를 대상으로 선정
- LRU 알고리즘과 차이점
    - 교체되는 페이지의 참조 시점이 가장 오래 되었다는 것을 보장하지 않음.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bFLfwl/btrmGg7P1GN/RhZ8a263lfv7OxMiWytbw1/img.png)

- 참조 비트를 순차적으로 조사하며 동작함.
    1. 프레임 내의 페이지가 참조될 때, 하드웨어에 의해 1로 자동 세팅됨.
    2. 한 바퀴를 돌면서 참조되지 않은 페이지의 참조 비트 값을 0으로 바꿈.
    3. 참조 비트가 0인 페이지를 방문하면 해당 페이지를 교체함.
- 페이지가 참조되어 1이 되고, 한 바퀴 도는 동안 사용되지 않으면 0이 되며, 다시 한 바퀴를 도는 동안 사용되지 않는 페이지(계속 0인 페이지)는 참조되지 않았음을 의미 → 교체 대상 페이지로 선정되는 알고리즘
- 시계 바늘이 한 바퀴를 도는 동안 걸리는 시간만큼 페이지를 메모리에 유지시켜 페이지 부재율을 줄이도록 설계된 알고리즘
    - 2차 기회 알고리즘이라고 부르기도 함.