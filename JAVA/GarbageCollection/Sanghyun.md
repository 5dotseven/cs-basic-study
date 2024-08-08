## Garbage Collection

---

유효하지 않은 메모리, 가비지를 알아서 정리해주는 프로세스

- 개발자가 메모리를 직접 관리하고 누수에 대응할 필요 없이 개발에만 집중이 가능
- 자바외에도 파이썬, 자바스크립트, Go 등의 언어에도 탑재되어 있는 기능

**단점**

- 자동으로 처리해주지만 정확히 언제 메모리에서 해제되는지 알 수 없어 제어가 어렵다.
- 가비지 컬렉션이 동작 중일 때 다른 동작이 중지되어 오버헤드가 발생할 수 있다.

    <aside>
    💡 **stop-the-world**
    가비지 컬렉션을 수행하기 위해 JVM이 프로그램의 실행을 멈추는 현상
    GC를 제외한 스레드가 전부 멈추기에 서비스에 차질이 생길 수 있어 이 시간을 최소화 시켜야 한다.
    → GC 튜닝

    </aside>


**GC 대상**

- 도달능력으로 가비지를 판단
- 객체가 참조되고 있다면 Reachable, 아니라면 Unreachable
- JVM의 경우 HEAP 영역에 객체들이 생성되며 stack, method 영역에서 이들을 참조하지 않으면 Unreachable로 판단되며 주기적으로 GC가 처리해준다.

    ![img1 daumcdn](https://github.com/user-attachments/assets/18ebe5ed-ce75-470f-bcee-f51cea9b1bf6)

**청소방식**

- Mark and Sweep
    - 처리할 대상을 식별하고 제거하고 파편화된 메모리 영역을 채워나가는 작업을 수행
    - Mark: Root Space부터 **그래프 순회**를 하면서 연결된 객체들을 조회하면서 참조하고 있는 객체를 찾아서 마킹
    - Sweep: 참조하고 있지 않은 객체를 Heap에서 제거
    - Compact: Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축

**Heap 메모리 구조**

- 동적으로 레퍼런스 데이터가 저장되는 공간, 가비지 컬렉션의 대상
- Heap 영역의 설계 전제
    - 대부분의 객체는 금방 접근 불가능한 상태가 된다.
    - 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재
- 영역
    - Young
        - 새로운 객체들이 할당
        - 대부분의 객체는 금방 Unreachable 상태가 되기때문에 금방 사라짐
        - 해당 영역에 대한 GC는 Minor GC라고 불림
    - Old
        - Young에서 오래 살아남은 객체들이 존재, 더 큰 공간이다.
        - 가비지가 적게 발생하며, 발생할 경우 Major GC 또는 Full GC라고 부름

**Young 영역**

- 더 효율적인 GC를 위해 young 영역을 3가지로 나눔
- young 영역은 크기가 작기 때문에 객체를 검색하고 제거하는데 걸리는 시간이 짧다 - **Minor** GC
- Eden
    - new 를 통해 생성된 객체가 위치
    - 여기서 살아남은 경우 survivor 영역으로 이동
- Survivor 0/ 1
    - 최소 1번 이상의 GC를 살아남은 경우 위치
    - 둘 중 하나는 꼭 비어있어야 한다는 규칙이 있다.

> **영역을 세부적으로 나누어 객체의 생존기간을 면밀하게 제어 가능**
>

**Minor GC**

1. new로 처음 생성된 객체는 Eden에 위치
2. Eden이 꽉 차게 되면 GC를 실행하고 Mark로 reachable 객체를 찾아 Survivor 영역으로 이동
3. 이후 Sweep으로 unreachable 객체의 메모리를 해제
4. 살아남아 survivor에 있는 객체들은 age 값을 1 증가시킨다.
    1. age의 임계값은 31, 기록하는 부분이 6bit로 되어있기 때문
    2. 반드시 1개의 Survivor만이 사용되어야 함, 이것을 통해 현재 시스템이 정상인지 확인 가능
5. Eden이 다시 가득 차게 되면 다시 minor gc를 발생
    1. survivor 영역에서도 마킹이 이루어짐
6. 마킹된 객체들을 모두 비어있는 survivor로 이동시키고 살아남은 객체들의 age 값을 1 증가

**Major GC**

- age가 임계값을 넘은 객체들이 존재하는 Old 영역이 메모리가 부족해질 경우 실행
    - Old로 이동하는 것을 promotion이라고 부름
- Old가 꽉 차게 되면 Mark and Sweep을 통해 필요없는 객체들을 한번에 삭제하게 됨
- 하지만 공간이 크기 때문에 매우 큰 시간을 소요하며 여기서 Stop-the-world 문제가 발생한다.

**가비지 컬렉션 알고리즘**

Stop-the-world 현상을 방지하고 최적화를 위해 다양한 GC 알고리즘이 개발되었으며 설정을 통해 가장 적합한 알고리즘을 선택할 수 있다.

- Serial GC
    - 서버의 CPU 코어가 1개일 때 사용하는 가장 단순한 GC
    - GC 처리 스레드가 하나라서 가장 Stop-the-world 시간이 김
    - Minor에 Mark-Sweep, Major는 여기에 compact까지 사용
    - 성능이 안 좋기 때문에 디바이스 성능이 나쁜 경우에만 사용 → 실무에서는 쓰이지 않는다
- Parallel GC
    - Java의 기본 GC
    - Minor GC를 멀티 쓰레드로 수행, Old는 그대로 싱글 쓰레드
    - Serial에 비해 Stop-the-world 시간 감소
- Parallel Old GC
    - Parallel GC의 개선, Old 영역에서도 멀티 쓰레드로 수행
    - Mark-Summary-Compact 방식을 이용한다.

    <aside>
    💡 Mark-Summary-Compact
    Mark: 영역 별로 나누고 자주 참조되는 객체들을 식별
    Summary: 영역 별 통계정보를 토대로 살아남은 객체들의 밀도가 높은 부분이 어디까지인지 dense prefix로 정함
    오래 살아남은 객체는 이후로도 사용될 가능성이 높기 때문에 dense prefix를 기준으로 compact 영역을 줄임
    Compact: compact 영역을 destination과 source로 나누고 살아남은 객체는 destination으로 그렇지 못한 객체는 제거

    </aside>

- CMS GC
    - 애플리케이션 쓰레드와 GC 쓰레드가 동시에 실행, stop-the-world를 최대한 줄이기 위함
    - GC 과정이 매우 복잡해지며, GC 대상을 파악하는 과정이 여러 단계로 수행되기 때문에 CPU 사용량이 매우 높음
    - 현재 JAVA14부터 사용이 중지됨
- G1 GC
    - 4GB 이상의 힙 메모리, stop-the-world 시간이 0.5초 정도 필요한 경우 사용
    - young, old 영역이 아닌 region이라는 개념을 도입
    - 체스판 형태로 구역을 나누고 각 구역의 역할을 동적으로 부여
    - 가비지가 가득 찬 영역을 빠르게 회수하여 공간을 확보하기 대문에 GC 빈도가 줄어듬
- Shenandoah GC
    - 레드 햇에서 개발
    - 기존 CMS가 가진 단편화, G1이 가진 pause의 이슈를 해결
    - 강력한 Concurrency와 가벼운 GC 로직으로 heap 사이즈에 영향을 받지 않고 일정한 pause 시간이 소요가 특징
- ZGC
    - G1의 Region 처럼,  ZGC는 ZPage라는 영역을 사용, G1의 Region은 크기가 고정인데 비해, ZPage는 2mb 배수로 동적으로 운영됨. (큰 객체가 들어오면 2^ 로 영역을 구성해서 처리)
    - ZGC가 내세우는 최대 장점 중 하나는 힙 크기가 증가하더도 'stop-the-world'의 시간이 절대 10ms를 넘지 않는다는 것

출처:

[https://inpa.tistory.com/entry/JAVA-☕-가비지-컬렉션GC-동작-원리-알고리즘-💯-총정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC)

[Inpa Dev 👨‍💻:티스토리]