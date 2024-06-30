## **팩토리 메소드**

---

객체 생성을 캡슐화 처리한 공장 클래스가 대신 생성하게 하는 디자인 패턴

- 클라이언트에서 직접 new 연산자를 통해 객체를 생성하지 않음
- 객체를 도맡아 생성하는 공장 클래스를 생성하고 이를 상속하는 서브 공장 클래스에서 여러 객체를 생성

**구조**
- Creator: 최상위 공장 클래스, 팩토리 메서드 추상화
- ConcreteCreator: 서브 공장 클래스, 객체 하나를 책임지는 생산 공장
- Product: 제품 구현체 추상화
- ConcreteProduct: 공장 객체를 통해 생성된 제품 구현체

- **제품 클래스**

    ```java
    // 제품 객체 추상화 (인터페이스)
    interface IProduct {
        void setting();
    }
    
    // 제품 구현체
    class ConcreteProductA implements IProduct {
        public void setting() {
        }
    }
    
    class ConcreteProductB implements IProduct {
        public void setting() {
        }
    }
    ```

- **공장 클래스**

    ```java
    // 공장 객체 추상화 (추상 클래스)
    abstract class AbstractFactory {
    
        // 객체 생성 전처리 후처리 메소드 (final로 오버라이딩 방지, 템플릿화)
        final IProduct createOperation() {
            IProduct product = createProduct(); // 서브 클래스에서 구체화한 팩토리 메서드 실행
            product.setting(); // .. 이밖의 객체 생성에 가미할 로직 실행
            return product; // 제품 객체를 생성하고 추가 설정하고 완성된 제품을 반환
        }
    
        // 팩토리 메소드 : 구체적인 객체 생성 종류는 각 서브 클래스에 위임
        // protected 이기 때문에 외부에 노출이 안됨
        abstract protected IProduct createProduct();
    }
    
    // 공장 객체 A (ProductA를 생성하여 반환)
    class ConcreteFactoryA extends AbstractFactory {
        @Override
        public IProduct createProduct() {
            return new ConcreteProductA();
        }
    }
    
    // 공장 객체 B (ProductB를 생성하여 반환)
    class ConcreteFactoryB extends AbstractFactory {
        @Override
        public IProduct createProduct() {
            return new ConcreteProductB();
        }
    }
    ```


**언제 사용하는가**

- 생성과 사용 로직을 분리하여 결합도 낮추고자 할 때
- 객체의 유형과 종속성을 캡슐화하여 은닉 처리 할 경우
- 인터페이스 사용자가 직접 제품의 추가나 수정할 수 있는 방법이 필요할 때
- 원본 객체의 수정 없이 재사용을 통해 리소스를 절약하고자 하는 경우

**장점, 단점**

- 생성자와 구현 객체의 결합도 감소
- 객체 생성 후 공통으로 할 일을 지정 가능하며 구체적인 타입을 은닉할 수 있다.
- 단일 책인 원칙, 객체를 모아두는 패키지를 생성하여 한 곳에서 관리할 수 있다.
- 개방/ 폐쇄 원칙 준수, Creator 객체의 수정 없이도 새로운 제품의 추가나 수정이 가능하다.
- 하지만, 제품 하나하나마다 팩토리 개체가 필요하기 때문에 서브 클래스가 많아질 수 있으며 코드의 복잡성이 증가할 수 있다.