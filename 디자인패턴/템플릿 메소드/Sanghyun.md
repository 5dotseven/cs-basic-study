## 템플릿 메소드

---

공통적으로 사용하는 메서드를 템플릿화, 하위 클래스에서 세부 동작 사항을 다르게 구현하는 패턴

- 상속을 극대화하는 기술
- 알고리즘의 뼈대를 구축하는 것에 초점을 둠

**구조**
- AbstractClass: 하위 클래스인 ConcreteClass에서 구체적으로 구현할 추상 메서드를 선언
- ConcreteClass: AbstractClass를 상속하고 추상 메서드의 내용을 작성한다.
- hook 메서드: 추상 메서드가 아닌 일반 메서드로 작성, 자식 클래스에서 선택적으로 오버라이드하여 메서드의 결괏값에 따라 분기점을 적용할 수 있게 한다. 더 유연한 템플릿 메서드 알고리즘을 작성할 수 있다.

- **Abstract Class**

    ```java
    abstract class AbstractTemplate {
    
        // 템플릿 메소드 : 메서드 앞에 final 키워드를 붙이면 자식 클래스에서 오버라이딩이 불가능함.
    	// 자식 클래스에서 상위 템플릿을 오버라이딩해서 자기마음대로 바꾸도록 하는 행위를 원천 봉쇄
        public final void templateMethod() {
            // 상속하여 구현되면 실행될 메소드들
            step1();
            step2();
            
            if(hook()) { // 안의 로직을 실행하거나 실행하지 않음
                // ...
            }
            
            step3();
        }
    
    // hook 메서드 오버라이딩해서 자식이 마음대로 작성 가능, 코드 로직에 유연성을 줄 수 있음
        boolean hook() {
            return true;
        }
    
        // 상속하여 사용할 것이기 때문에 protected 접근제어자 설정
        protected abstract void step1();
        protected abstract void step2();
        protected abstract void step3();
    }
    ```

- **Concrete Class**

    ```java
    class ImplementationA extends AbstractTemplate {
    
        @Override
        protected void step1() {}
    
        @Override
        protected void step2() {}
    
        @Override
        protected void step3() {}
    }
    
    class ImplementationB extends AbstractTemplate {
    
        @Override
        protected void step1() {}
    
        @Override
        protected void step2() {}
    
        @Override
        protected void step3() {}
    
        // hook 메소드를 오버라이드 해서 false로 하여 템플릿에서 마지막 로직이 실행되지 않도록 설정
        @Override
        protected boolean hook() {
            return false;
        }
    }
    ```


```java
class Client {
   public static void main(String[] args) {
       // 1. 템플릿 메서드가 실행할 구현화한 하위 알고리즘 클래스 생성
       AbstractTemplate templateA = new ImplementationA();

       // 2. 템플릿 실행
       templateA.templateMethod();
   }
}
```

**언제 사용하는가**

- 사용자가 전체 알고리즘을 확장할 필요 없이 특정한 단계만 손 보면 되는 경우
- 동일한 기능은 상위 클래스에서 정의하고 구체적으로 작성이 필요한 부분만 하위에서 구현할 때

**장점**

- 전체 로직이 알고리즘의 변경으로 인해 받는 영향을 적게 할 수 있다.
- 중복되는 로직은 상위 클래스에서 정의해 코드의 양을 줄이고 효율적으로 바꿀 수 있다.

**단점**

- 확장 가능한 정도가 제한되기 때문에 유연성이 떨어질 수 있다.
- 추상 메소드가 많아지면 하위 클래스의 관리가 어려워진다.
- 상위 메서드가 변경되면 모든 하위 클래스에서도 변경이 필요해진다.

**출처:**

[https://inpa.tistory.com/entry/GOF-💠-템플릿-메소드Template-Method-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-💠-템플릿-메소드Template-Method-패턴-제대로-배워보자) [Inpa Dev 👨‍💻:티스토리]