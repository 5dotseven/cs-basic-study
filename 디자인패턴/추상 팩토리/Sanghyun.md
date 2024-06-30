## **추상 팩토리**

---

팩토리 패턴과 비슷하지만 여러 객체 군을 생성할 수 있다는 점에서 차이가 존재

- AbstractFactory: 최상위 공장 클래스, 제품을 생성하는 메소드들을 추상화
- ConcreteFacotory: 서브 공장 클래스, 여러 개의 제품들에 대한 메소드들을 재정의
- AbstractProduct: 각 제품들을 추상화한 인터페이스
- ConcreteProduct: 각 제품들의 구현체, 자신을 생성한 공장에 대해서는 모름
- 공장 별로 같은 Product 일지라도 반환되는 제품군이 다르다.
- 윈도우와 맥 환경에서 버튼, 체크박스 등의 UI 구성이 다른 경우 기본적인 틀을 AbstractFactory로 제공하고 각각 맥과 윈도우 공장을 생성하여 버튼의 타입을 다르게 지정할 수 있다.
    - 기존의 팩토리 메서드와 다르게 분기점을 작성해야 할 필요가 없다.
    - 새로운 환경이 추가될 시 각 팩토리 별로 있는 분기점을 수정해야 하기 때문에 OCP 원칙을 위배하는 경우가 생긴다.
- 리눅스 환경을 추가로 생성하는 팩토리가 필요할 경우에도 코드 수정없이 리눅스 전용 버튼, 체크박스 UI 구현체와 리눅스 팩토리 클래스의 추가만으로 새로운 팩토리를 만들어 제공할 수 있다. - OCP 원칙 준수
- 하지만 새로운 UI, 마우스 커서 같은 것을 추가하고자 할 경우에는 모든 서브 클래스마다 이를 적용하는 코드를 작성해야한다.

**팩토리 패턴과의 차이점**

- 팩토리 패턴은 한 객체 당 하나의 팩토리
- 메소드 정의, 사용법에 초점을 맞춤으로 생성과 구성에 대한 의존성을 감소시킨다.
- 반면 추상 팩토리는 팩토리에서 제품을 생성하는 방법에 더 초점을 둠

- 제품 클래스

    ```java
    // Product A 제품군
    interface AbstractProductA {
    }
    
    // Product A - 1
    class ConcreteProductA1 implements AbstractProductA {
    }
    
    // Product A - 2
    class ConcreteProductA2 implements AbstractProductA {
    }
    
    // Product B 제품군
    interface AbstractProductB {
    }
    
    // Product B - 1
    class ConcreteProductB1 implements AbstractProductB {
    }
    
    // Product B - 2
    class ConcreteProductB2 implements AbstractProductB {
    }
    ```

- 공장 클래스

    ```java
    interface AbstractFactory {
        AbstractProductA createProductA();
        AbstractProductB createProductB();
    }
    
    // Product A1와 B1 제품군을 생산하는 공장군 1 
    class ConcreteFactory1 implements AbstractFactory {
        public AbstractProductA createProductA() {
            return new ConcreteProductA1();
        }
        public AbstractProductB createProductB() {
            return new ConcreteProductB1();
        }
    }
    
    // Product A2와 B2 제품군을 생산하는 공장군 2
    class ConcreteFactory2 implements AbstractFactory {
        public AbstractProductA createProductA() {
            return new ConcreteProductA2();
        }
        public AbstractProductB createProductB() {
            return new ConcreteProductB2();
        }
    }
    ```

- 사용 예시

    ```java
    class Client {
        public static void main(String[] args) {
        	AbstractFactory factory = null;
            
            // 1. 공장군 1을 가동시킨다.
            factory = new ConcreteFactory1();
    
            // 2. 공장군 1을 통해 제품군 A1를 생성하도록 한다 (클라이언트는 구체적인 구현은 모르고 인터페이스에 의존한다)
            AbstractProductA product_A1 = factory.createProductA();
            System.out.println(product_A1.getClass().getName()); // ConcreteProductA1
    
            // 3. 공장군 2를 가동시킨다.
            factory = new ConcreteFactory2();
    
            // 4. 공장군 2를 통해 제품군 A2를 생성하도록 한다 (클라이언트는 구체적인 구현은 모르고 인터페이스에 의존한다)
            AbstractProductA product_A2 = factory.createProductA();
            System.out.println(product_A2.getClass().getName()); // ConcreteProductA2
        }
    }
    ```


**언제 사용하는가**

- 하나의 제품이 다른 제품들과 연관되어 작동해야 할 경우
- 구체적인 클래스에 의존하지 않은 상태로 제품을 사용하고 싶은 경우
- 여러 제품들 중 하나를 선택하여 사용하고 다른 제품으로 대체가 가능하게 하고 싶을 때

**장단점**

- 클라이언트 코드와 객체 생성 코드의 결합도를 낮춤
- 간편하게 제품 군을 교체하고 조합할 수 있다.
- 단일 책임 원칙, 개방/폐쇄 원칙 준수
- 팩토리 패턴의 공통적인 문제로 구현체마다 팩토리 객체들의 구현이 필요, 객체가 증가할수록 코드의 양과 복잡성이 증가한다.
- 새로운 제품의 추가, 추상 팩토리의 구성 변경이 있을 경우 하위의 모든 팩토리와 서브클래스의 수정이 필요하다.

출처

출처: [https://inpa.tistory.com/entry/GOF-💠-추상-팩토리Abstract-Factory-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%ACAbstract-Factory-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90) [Inpa Dev 👨‍💻:티스토리]