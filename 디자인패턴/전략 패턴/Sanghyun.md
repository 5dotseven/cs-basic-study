## 전략 패턴

---

실행 중에 알고리즘 전략을 선택, 객체의 동작을 실시간으로 바뀌도록 해주는 행위 디자인 패턴

- 알고리즘 변형이 빈번하기 필요한 경우 적합

**구조**
- Concrete Strategy: 알고리즘, 행위, 동작을 객체로 정의
- Strategy: 전략을 구현하기 위해 사용되는 공용 인터페이스
- Context: 알고리즘 실행 중 연결된 전략을 호출하는 역할

- **Strategy**

    ```java
    // 전략(추상화된 알고리즘)
    interface IStrategy {
        void doSomething();
    }
    
    // 전략 알고리즘 A
    class ConcreteStrateyA implements IStrategy {
        public void doSomething() {}
    }
    
    // 전략 알고리즘 B
    class ConcreteStrateyB implements IStrategy {
        public void doSomething() {}
    }
    ```

- **Context**

    ```java
    // 컨텍스트(전략 등록/실행)
    class Context {
        IStrategy Strategy; // 전략 인터페이스를 합성(composition)
    	
        // 전략 교체 메소드
        void setStrategy(IStrategy Strategy) {
            this.Strategy = Strategy;
        }
    	
        // 전략 실행 메소드
        void doSomething() {
            this.Strategy.doSomething();
        }
    }
    ```

- **Client**

    ```java
    // 클라이언트(전략 교체/전략 실행한 결과를 얻음)
    class Client {
        public static void main(String[] args) {
            // 1. 컨텍스트 생성
            Context c = new Context();
    
            // 2. 전략 설정
            c.setStrategy(new ConcreteStrateyA());
    
            // 3. 전략 실행
            c.doSomething();
    
            // 4. 다른 전략 설정
            c.setStrategy(new ConcreteStrateyB());
    
            // 5. 다른 전략 시행
            c.doSomething();
        }
    }
    ```


**전략 패턴은 언제 쓸까**

- 버전이나 변형이 필요할 때 관리 용이 위해
- 알고리즘 코드가 민감한 정보를 다루는 경우
- 알고리즘의 동작이 런타임에서 변경되어야 할 때

**장단점**

- 알고리즘의 대체와 변경이 간편해 다양한 상황에 유연하게 대응할 수 있다
- 인터페이스의
- 알고리즘이 많아질수록 관리 용이성이 감소한다.
- 알고리즘의 변경이 자주 일어나지 않고 상황에 따라 변경이 필요하지 않다면 사용하지 않는 것이 좋다

### 면접 질문 예상

---

1. 템플릿 메소드 패턴과 전략 패턴의 차이점은 무엇인가

상속을 사용하지 않는다는 점에서 차이가 있다.
탬플릿 패턴은 상속을 사용하기 때문에 부모나 상위 클래스에서 코드의 수정, 로직의 추가가 발생하면 모든 자식 클래스를 수정해야 하는 번거로움이 발생할 수 있다.

전략 패턴은 인터페이스를 사용하기 때문에 부모와 자식 간의 결합도를 낮출 수 있다.

1. 실무에서 템플릿 메서드/ 전략 패턴이 사용되는 예시

Spring 프레임워크에서 WebSecurityConfiguererAdapter의 경우 configure를 override 함으로 Config의 일부를 직접 용도에 맞게 바꿀 수 있다.

출처:

[https://inpa.tistory.com/entry/GOF-💠-전략Strategy-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-💠-전략Strategy-패턴-제대로-배워보자)

[Inpa Dev 👨‍💻:티스토리]