# JAVA - Garbage Collection
## GarbageCollection이란?
프로그램을 개발 하다 보면 유효하지 않은 메모리인 `Garbage`가 발생하게 됨 이때 이 Garbage를 어떻게 처리할까? C에서는 free()함수를 이용해서 직접 제거하지만 Java & Kotlin 에서는 개발자가 직접 해제 하지않는다. 그렇다면 어떻게 처리가 되는 걸끼?

바로 `JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리`

```java
Person person = new Person();
person.setName("Mang");
person = null;

// 가비지 발생
person = new Person();
person.setName("MangKyu");
```

기본의 Mang으로 생성된 person 객체는 더이상 참조를 하지 않는 상황이므로 가비지가 되었음 여기서 자바나 코틀린은 `메모리 누수를 방지하기 위해 가비지 컬렉터가 주기적으로 검사하여 메모리를 청소해줌`

### stop- the -world
GC에 대해서 알아보기 전에 알아야할 용어가 있음. 바로 'stop-the-world' GC를 실행하기 위해서 JVM이 애플리케이션의 실행을 멈추는 것을 뜻하는 말. stop-the-world가 발생하면 GC에서 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춤 GC가 완료한 이후에 중단된 작업을 다시 실행한다. 어떤 GC알고리즘도 `stop-the-word가 발생함 따라서 GC튜닝을 한다는 것은 stop-the-world에 시간을 줄이는 것`

java로 프로그램을 짤 떄 명시적으로 메모리를 지정하여 해제하지 않는다. (null로 객체를 지정하거나, Sytem.gc()메소를 사용하거나...로 사용할 수 있지만 null은 크게 문제가 되지 않지만  Sytem.gc()은 성능을 건드릴수 있으므로 절대 사용 X)

## Minor GC 와 Major GC
Java에서 개발자가 프로그램 코드에 명시적으로 메모리를 해제하지 않기 떄문에 GC가 가비지 객체를 찾아 지우는 작업을 함 여기서 GC는 두가지 전제하에 만들어졌다.
- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉  `대부분의 겍체는 일회성이며, 메모리에 오랫동안 남아 있는 경우가 드물다는 것` 따라서 객체의 생존 기간에 따라 물리적인 Heap영역을 Young, Old 총 2가지로 설계함
![스크린샷 2024-07-25 오후 10 35 08](https://github.com/user-attachments/assets/1cba49e0-af8d-4b55-b686-cc045bc007e1)

- Young 영역
    - 새롭게 생성된 객체의 대부분이 여기에 위치함
    - 대부분의 객체가 금방 접근 불가능 상태가 되기 떄문에 많은 객체들이 Young영역에서 생성되거나 사라짐
    - Young 영역에서 객체가 사라질때 Minor GC가 발생
- Old 영역
    - Young영역에서 Reachable(접근불가능 상태로 되지 않아) 상태를 유지하여 살아남은 객체가 복사되는 영역
    - Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가비지는 Young영역에 비해 적게 발생함
    - Old 영역에 대한 가비지 컬렉션(Garbage Collection)을 Major GC가 발생

Old 영역이 Young 영역 보다 크게 할당 되는 이유는 Young 영역에 수명이 짧은 객체들은 큰공간이 필요하지 않음 만약 큰 공간이 필요한 객체면 바로 Old 영역에 들어감

![스크린샷 2024-07-25 오후 10 35 21](https://github.com/user-attachments/assets/5157d4c7-a185-4422-acf2-c2f5ba483620)

반대로 Old에서 Youg으로 가는 상황도 존재

![스크린샷 2024-07-25 오후 10 35 34](https://github.com/user-attachments/assets/0be26b15-a722-4d98-8a7a-df88449df334)
카드테이블 도입 이유
- GC를 진행 할때 Young에서 모든 Old영역에서 존재하는 객체를 검사해서 참조 되지않는 Young영역에 객체를 찾는건 매우 비효율적
- 따라서 Young영역에서 GC를 진행할 때 `카드 테이블만 조회하면 GC의 대상인지 식별이 가능`

## GC동작방식
Old 영역과 Young 영역은 서로 다른 메모리 구조이기때문에 세부적인 동작 방식은 다르지만 기본적인 틀은 똑같음. 앞에서 얘기했던 Stop-the-world와 추가로 Mark-and-Sweep

### Mark-and-Sweep
- Mark: 사용되는 메모리인지 사용되지 않는 메모리인지 식별하는 작업
- Sweep: Mark에서 사용되지 않는걸로 식별된 메모리를 해제하는 작업

Stop-the-world (멈추고) -> GC가 어떤객체를 참고하고있는지 탐색 -> 메모리를 식별함(Mark) -> Mark가 되지 않은 객체들을 메모리에서 제거(Sweep)



## Reference & Additional Resources
- [https://mangkyu.tistory.com/118](https://mangkyu.tistory.com/118) [MangKyu's Diary:티스토리]
- https://advenoh.tistory.com/14
- https://d2.naver.com/helloworld/1329