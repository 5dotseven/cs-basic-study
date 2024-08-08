## 고유 락

---

- 자바의 모든 객체는 lock을 가지고 있으며 이 때문에 고유 락이라고 부름
- 모니터처럼 동작하기 때문에 모니터 락, 모니터라고도 부름
- 자바 멀티 스레드 환경에서는 스레드들이 Heap, Static 영역을 공유한다.
    - Static: 메소드 내에 기본형 지역 변수, 메소드들의 콜스택을 저장
    - Heap: 참조형 데이터 타입을 갖는 객체, 배열 등을 저장

```java
public class Count {
	private int num;
	
	public int plusOne {
		return ++num;
	}
}
```

이러한 코드가 있을 경우 num 값을 읽는 쓰레드, 값을 수정하는 쓰레드, 값을 저장하는 쓰레드가 동시에 num에 접근할 경우 동시성 문제가 발생한다.

- 아래와 같은 방식으로 스레드가 lock을 획득하게 하여 다른 쓰레드의 접근을 제어 가능

```java
// Object 인스턴스를 사용하여 count 변수에 대한 접근을 제어
public class Count {
	private Object lock = new Object();
	private int num;
	
	public int plusOne {
		synchronized(lock) {
			return ++num;
		}
	}
}

// 아래와 같이 한 줄로 대체 가능
	public synchronized int plusOne() {
		return ++num;
	}
```

**고유락 재진입**

- 락의 획득은 스레드 단위로 일어난다.
- 이미 락을 획득한 스레드는 같은 락을 얻기 위해 대기하지 않아도 된다.
- 즉, 이미 락을 가지고 있기 때문에 같은 락에 대한 Synchronized 블록을 만나도 대기 없이 통과 가능

```java
public class Reentrancy {
  public synchronized void a() {
    System.out.println("a");
    // 같은 락 내에서 다른 synchronized 호출
    // 두 개의 메서드는 같은 락으로 동기화된다.
    b();
  }
  public synchronized void b() {
    System.out.println("b");
  }
  public static void main(String[] args) {
    new Reentrancy().a();
  }
}
```

**가시성**

- 한 스레드가 수정한 내용이 다른 스레드에도 보이도록 함
- 자바는 쓰레드가 락을 획득하게 되면 이전에 사용되던 값들에 대한 가시성을 보장
- A, B 쓰레드가 있고 A가 먼저 리소스를 사용 후 B가 사용할 때 변경된 값에 대한 접근이 즉각 가능해야 한다.

**구조적인 락, Structured lock**

- 고유락을 이용한 동기화
- synchronized한 블록 단위로 락의 획득과 해제가 일어나기 때문에 구조적
- 블록을 진입할 때 락을 획득, 벗어나면 락을 해제
- 따라서 A획득 → B획득 → A해제 → B해제는 불가능
    - A획득 → B획득 → B해제 → A해제
    - 코드의 구조에 따라 획득과 해제가 자동으로 이루어짐, 개발자가 명시적으로 처리할 필요가 없음

**명시적 락**

- 위의 고유락에서 불가능했던 A획득 → B획득 → A해제 → B해제를 가능하게 해줌
- 락을 확보하기 위해 대기중인 스레드에 인터럽트를 걸거나, 대기 상태에 들어가지 않은 상태에서 락의 확보가 필요한 경우 사용
- 블록의 실행이 끝나도 락을 자동해제하지 않기 때문에 항상 finally를 사용해 락을 해제 시켜야함

    ```java
    public class ReentrantLockExample {
        private int count = 0;
        private Lock lock = new ReentrantLock(); // 명시적인 락 생성
        
        public void increment() {
            lock.lock(); // 락 획득
            try {
                count++;
            } finally {
                lock.unlock(); // 락 해제
            }
        }
    }
    ```


**Read/ Write Lock**

- 읽기, 쓰기 락을 따로 관리
- 여러 쓰레드가 읽기 락을 획득이 가능, 하지만 읽기 락을 획득한 상태에서는 쓰기 락 획득 불가
    - 쓰기 락은 한번에 한 쓰레드만 획득이 가능
    - 데이터의 일관성을 보장
- 쓰기가 길어지거나 빈번하게 일어나는 작업일 경우 성능저하가 발생할 수 있다.

출처:

[https://velog.io/@kimmy/CS-지식-Java-고유락-Intrinsic-Lock](https://velog.io/@kimmy/CS-%EC%A7%80%EC%8B%9D-Java-%EA%B3%A0%EC%9C%A0%EB%9D%BD-Intrinsic-Lock)