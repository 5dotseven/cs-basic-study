## Record

---

Java 14버전부터 도입, 16버전에서 정식 스펙으로 포함된 기능

**DTO 구현**

- 기존의 DTO 구현 방식을 사용할 경우 보일러 플레이트(getter, setter, equals, hashCode, toString 등) 코드가 크다.
- 보통 lombok을 사용해 이러한 불필요한 코드를 줄일 수 있다.
- 이러한 자바의 자체적인 한계를 개선하기 위한 것이 record.
    - 데이터를 간결하게 표현
    - 불변 데이터를 모델링하는데 집중
    - 데이터 지향 메소드를 자동으로 구현

```java
public class Car {
	private String type;
	private String engine;
	private int gasQuantity;
	
	public Car() {}
	
	public Car(String type, String engine, int gasQuantity) {
		this.type = type;
		this.engine = engine;
		this.gasQuantity = gasQuantity;
	}
	
	public String getType() {
		return type;
	}
	public String setType(String type) {
		this.type = type;
	}
	
	// 생략
}
```

**Record**

- 불변 객체, abstract로 선언할 수 없으며 암시적으로 final로 선언된다.
- 값이 정해지면 setter를 통한 값 변경이 불가능하며 상속을 할 수 없다.
    - 다른 클래스를 상속 받을 수 없지만 인터페이스를 통한 구현은 가능하다.
- record 내부 필드, 헤더에 나열되는 컴포넌트들은 private final로 정의된다.
    - 내부에서 멤버 변수를 선언할 수 없으나 static 변수는 생성 가능
    - 헤더에서 정의한 멤버만을 record에서 관리하기 위함

```java
public record Car(String type, String engine, int gasQuantity) {}
```

- 컴파일러는 헤더를 통해 내부 필드를 추측
    - type, engine, gasQuantity를 확인하고 접근자, 생성자, toString, equals, hashCode를 자동으로 구현해준다.
- 컴팩트 생성자
    - 컴포넌트로 들어온 값을 불변으로 만들거나 유효성 체크 등의 작업을 할 수 있다.

```java
public record Car(String type, String engine, int gasQuantity) {
	
	public Car {
		Objects.requireNonNull(type);
		Objects.requireNonNull(engine);
		Objects.requireNonNull(gasQuantity);
	}
}
```