## Composition

---

상속과 합성은 코드 재사용 기법의 일종

| 상속 | 합성 |
| --- | --- |
| 부모, 자식 사이의 의존성은 컴파일 타임에 해결 | 객체 사이의 의존성을 런타임 단계에 해결 |
| is-a | has-a |
| 부모 클래스의 구현에 의존도가 높다. | 구현에 의존하지 않으며
내부에 포함되는 객체의 구현이 아닌 인터페이스에 의존 |
| 클래스 사이의 정적 관계 | 클래스 사이 동적인 관계 |
| 부모 클래스에 구현된 코드 자체를 물려받아 재사용 | 포함되는 객체의 public 인터페이스를 재사용 |

상속은 코드 재사용보다는 코드의 확장에 초점이 맞추어져 있다.

- 상위 클래스가 확장을 목적으로 설계되었고 문서화가 잘 된 경우 사용에 용이
- 이로 인해 부모와 자식 간의 결합도가 매우 커지게 된다.
- 만약 여러 기능을 조합해야 할 경우 상속을 사용하게 되면 클래스 폭발 문제가 발생할 수 있다.
    - 조합별로 필요한 클래스 여러 개를 추가해야하는 경우 발생

**합성**

- 필드로 클래스의 인스턴스를 참조하게 만드는 설계
- new(new) 형태로 생성자에 생성자를 받는 방식 - 포워딩
    - Has-A 관계라고 불리는 이유

```java
class Car {
	Engine engine;
	
	Car(Engine engine) {
		this.engine = engine;
	}
	void drive() {
		System.out.printf("%s 엔진으로 드라이브\n", engine.EngineType);
	}
	void breaks() {
		System.out.printf("%s 엔진으로 브레이크\n", engine.EngineType);
	}
}

class Engine {
	String EngineType;
	
	Engine(String type) {
		this.EngineType = type;
	}
}

public class Main {
    public static void main(String[] args) {
        Car digelCar = new Car(new Engine("디젤"));
        digelCar.drive(); // 디젤엔진으로 드라이브~

        Car electroCar = new Car(new Engine("전기"));
        electroCar.drive(); // 전기엔진으로 드라이브~
    }
}
```

- 합성은 실행 시점 동적으로 관계 변환이 가능하다.
    - 전략 패턴이 예시
- 불필요한 인터페이스 상속으로 인한 문제 해결

    ```java
    public class Stack<E> {
        private Vector<E> elements = new Vector<>(); // 합성
        
        public E push(E item) {
            elements.addElement(item);
            return item;
        }
    
        public E pop() {
            if (elements.isEmpty()) {
                throw new EmptyStackException();
            }
            return elements.remove(elements.size() -1);
        }
    }
    ```

- 메서드 오버라이딩 문제 해결

    ```java
    class CustomSet<E> {
        private int addCount = 0; // 자료형에 몇번 추가되었는지 세는 카운트 변수
        private Set<E> set = new HashSet<>(); // 합성
    
        public boolean add(E e) {
            addCount++;
            return set.add(e); // 합성된 객체의 메서드를 실행
        }
    
        public boolean addAll(Collection<? extends E> c) {
            addCount += c.size();
            return set.addAll(c); // 합성된 객체의 메서드를 실행
        }
    
        public int getAddCount() {
            return addCount;
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            CustomSet<String> mySet = new CustomSet<>();
    
            mySet.addAll(Arrays.asList("가", "나", "다", "라", "마"));
            mySet.add("바");
    
            System.out.println(mySet.getAddCount()); // 6
        }
    }
    ```


**상속이 가지는 문제점**

- 결합도가 높아진다.
    - 컴파일 시점에 관계가 결정, 실행 시점에서 객체의 종류 변경이 불가능해짐
    - 런타임에서 A 상속을 C 상속으로 바꾸는 행위가 불가능
- 불필요한 기능을 상속
    - 동물 클래스에 날다라는 메서드가 있을 경우 육상동물 자식에 이러한 기능이 상속
    - 구현은 하고 빈칸으로 냅두거나, 클래스를 분리하여 해결 가능하지만 결국 복잡해진다.
- 부모 클래스의 결함을 그대로 이어받게됨.
- 부모 클래스, 자식 클래스의 동시 수정 문제
    - 부모 클래스를 변경하게 되면 자식도 함께 변경해야한다.
- 메서드 오버라이딩 오동작
    - 부모의 public 메서드 이용 시 의도하지 않은 동작을 수반할 수 있다.
    - 캡슐화를 위반하게 되는 것 - 내부 동작을 알 필요없이 메서드만 사용하면 된다는 신뢰가 깨짐

    ```java
    public class CustomHashSet<E> extends HashSet {
        private int count = 0;
    
        public CustomHashSet(){}
    
        public CustomHashSet(int initCap, float loadFactor){
            super(initCap,loadFactor);
        }
    
        @Override
        public boolean add(Object o) {
            count++;
            return super.add(o);
        }
    
        @Override
        public boolean addAll(Collection c) {
            count += c.size();
            return super.addAll(c);
        }
    
        public int getCount() {
            return count;
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            CustomSet<String> mySet = new CustomSet<>();
    
            mySet.addAll(Arrays.asList("가", "나", "다", "라", "마"));
            mySet.add("바");
    
            System.out.println(mySet.getAddCount()); // ! 6이 나와야 정상이지만 11이 나오게 된다.
        }
    }
    ```

    ```java
    public boolean addAll(Collection<? extends E> c) {
    	boolean modified = false;
    	for(E e: c)
    		if(add(e)) // 내부에서 add()를 자체적으로 계속 호출 중인 상황
    			modified = true;
    	return modified;
    }
    ```

- 블필요한 인터페이스 상속
    - java.util.Stack은 Vector 클래스를 상속하는데 이때 add 메서드도 상속을 받게 된다.
    - 사용자는 add 메서드를 실수로 사용할 수 있게 되며 의도치 않은 동작을 유발할 수 있다.

    ```java
    Stack<String> stack = new Stack<>();
    stack.push("one");
    stack.push("two");
    stack.push("three");
    
    stack.add(0, "four"); // add 메소드를 호출함으로써 stack의 의미와는 다르게 특정 인덱스의 값이 추가
    
    String str = stack.pop(); // three
    System.out.println(str.equals("four")); // false
    ```


출처:

https://inpa.tistory.com/entry/OOP-💠-객체-지향의-상속-문제점과-합성Composition-이해하기 [Inpa Dev 👨‍💻:티스토리]