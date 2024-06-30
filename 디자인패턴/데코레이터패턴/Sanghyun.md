## 데코레이터 패턴

---

객체에 동적으로 기능을 추가, 확장할 수 있는 구조 패턴

기본 기능에 추가할 기능을 Decorator 클래스로 정의, Decorator 객체를 조합하여 추가 기능의 조합을 설계하는 방식

- Concrete Component: 기본 기능을 구현한 원본 객체
- Decorator: 기본 기능(Decorator)에 추가되는 개별적인 기능을 구현
- Component: 기본 기능과 추가 기능을 묶는 클래스로 사용자가 실제로 사용하는 객체
- Concrete Decorator: 여러 Decorator들의 공통적인 기능을 정의하는 클래스

```java
// 원본 객체와 장식된 객체 모두를 묶는 인터페이스
interface IComponent {
    void operation();
}

// 장식될 원본 객체
class ConcreteComponent implements IComponent {
    public void operation() {
    }
}

// 장식자 추상 클래스
abstract class Decorator implements IComponent {
    IComponent wrappee; // 원본 객체를 composition

    Decorator(IComponent component) {
        this.wrappee = component;
    }

    public void operation() {
        wrappee.operation(); // 위임
    }
}

// 장식자 클래스
class ComponentDecorator1 extends Decorator {

    ComponentDecorator1(IComponent component) {
        super(component);
    }

    public void operation() {
        super.operation(); // 원본 객체를 상위 클래스의 위임을 통해 실행하고
        extraOperation(); // 장식 클래스만의 메소드를 실행한다.
    }

    void extraOperation() {
    }
}

class ComponentDecorator2 extends Decorator {

    ComponentDecorator2(IComponent component) {
        super(component);
    }

    public void operation() {
        super.operation(); // 원본 객체를 상위 클래스의 위임을 통해 실행하고
        extraOperation(); // 장식 클래스만의 메소드를 실행한다.
    }

    void extraOperation() {
    }
}

public class Client {
    public static void main(String[] args) {
        // 1. 원본 객체 생성
        IComponent obj = new ConcreteComponent();

        // 2. 장식 1 하기
        IComponent deco1 = new ComponentDecorator1(obj);
        deco1.operation(); // 장식된 객체의 장식된 기능 실행

        // 3. 장식 2 하기
        IComponent deco2 = new ComponentDecorator2(obj);
        deco2.operation(); // 장식된 객체의 장식된 기능 실행

        // 4. 장식 1 + 2 하기
        IComponent deco3 = new ComponentDecorator1(new ComponentDecorator2(obj));
    }
}
```

- Decorator 사용하지 않는 경우

    ```java
    // 음료수
    public interface Drinks {
    	String getName();
    	int price;
    }
    
    // 사이다
    public class Sprite implements Drinks {
    	@Override
    	public String getName() {
    		return "사이다";
    	}
    	
    	@Override 
    	public int price() {
    		return 2000;
    	}
    }
    
    // 제로사이다
    public class ZeroSprite implements Drinks {
    	private Drinks sprite;
    	
    	public ZeroSprite(Drinks sprite) {
    		this.sprite = sprite;
    	}
    	
    	@Override
    	public String getName() {
    		return "제로사이다";
    		// return "제로" + sprite.getName();
    	}
    	
    	@Override 
    	public int price() {
    		return 1500;
    	}	
    }
    
    // 칠성사이다
    public class KoreaSprite implements Drinks {
    	private Drinks sprite;
    	
    	public KoreaSprite(Drinks sprite) {
    		this.sprite = sprite;
    	}
    	
    	@Override
    	public String getName() {
    		return "칠성사이다";
    		// return "칠성" + sprite.getName();
    	}
    	
    	@Override 
    	public int price() {
    		return 2000;
    	}	
    }
    
    public class Main {
    	public static void main(String[] args) {
    		Drinks drinks = new Drinks();
    		
    		Drinks zeroSprite = new ZeroSprite(drinks);
    		
    		Drinks koreaSprite = new KoreaSprite(drinks)
    	}
    }
    ```

- Decorator 사용 예시

    ```java
    // 사이다 decorator
    public abstract class SpriteDecorator implements Drinks {
    	private Drinks drinks;
    	
    	public SpriteDecorator(Drinks drinks) {
    		this.drinks = drinks;
    	}
    	
    	@Override
    	public String getName() {
    		return drinks.getName();
    	}
    	
    	@Override 
    	public int price() {
    		return drinks.getPrice();
    	}
    }
    
    // ConcreteDecorator
    public class SugarDecorator extends SpriteDecorator {
    	public SugarDecorator(Drinks drinks) {
    		super(drinks);
    	}
    	
    	@Override String getName() {
    		return "제로" + super.getName();
    	}
    	
    	@Override int getPrice() {
    		return super.getPrice();
    	}
    }
    ```


https://inpa.tistory.com/entry/GOF-💠-데코레이터Decorator-패턴-제대로-배워보자#패턴_사용_시기

**언제 사용하는 것이 좋을까**

- 객체의 정보와 역할이 동적으로 변하는 상황, 기능의 변경이 자주 일어나는 경우
- 객체 결합을 통해 기능이 생성될 수 없는 경우
- 객체 코드를 수정하지 않고 런타임에서 추가적인 기능 실행이 필요한 경우
- 상속을 통해 객체의 기능을 추가하는 것이 어색하거나 불가능 할 때

**장점**

- 유연한 확장이 가능하다.
- 코드 수정 없이도 새로운 기능을 추가할 수 있어 코드 재사용성이 좋으며 다양한 조합이 가능해진다.
- 단일 책임 원칙 준수: 데코레이터는 기능의 추가와 수정에만 집중하면 된다.
- 개방 폐쇄 원칙 준수: 기존의 코드 변경 없이도 데코레이터 클래스 추가를 통해 새 기능을 만들어 줄 수 있다.
- 의존성 역전 원칙 준수: 컴포넌트와 데코레이터가 모두 동일한 인터페이스나 추상 클래스를 공유하므로 데코레이터가 컴포넌트에 의존하지 않는다.

**단점**

- 객체를 감싸는 층이 많아지기 때문에 클래스 수가 많아지면서 복잡성도 증가한다.
- 데코레이터를 적용하는 순서에 따라 최종적으로 생성되는 객체의 동작이나 상태가 달라진다.
- 모든 데코레이터는 원본 객체의 인스턴스를 소유, 데코레이터를 여러 개 사용할 수록 불필요한 객체가 복사될 수 있다.

출처:

[https://ittrue.tistory.com/558](https://ittrue.tistory.com/558)

[https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)