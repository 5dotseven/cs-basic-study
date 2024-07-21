## Casting(업캐스팅 & 다운캐스팅)

---

**캐스팅** - 하나의 데이터 타입을 다른 타입으로 바꾸는 것

- 기본형: Boolean, Numeric
- 참조형: Class, Interface, Array, Enum 등

```java
long d = (long)10.233;
```

**상속 관계에 있는 부모와 자식 클래스는 서로 간에 형변환이 가능**

- 부모에서 자식으로 형변환 - 다운캐스팅
- 자식에서 부모로 형변환 - 업캐스팅

```java
List<int> arr = new ArrayList()<>;
// ArrayList가 List를 부모 클래스로서 상속받는다, 업캐스팅
```

- 하지만 형제 관계의 클래스는 타입이 다르기 때문에 참조 형변환이 불가능

### **업캐스팅**

- 자식 클래스가 부모 클래스 타입으로 캐스팅
- 캐스팅 연산자 괄호 생략 가능
- 업캐스팅될 경우 자식 클래스만이 가지고 있는 메서드는 실행 불가 → 멤버의 개수 감소
- 업캐스팅 후 메서드 실행 시 자식 클래스에서 오버라이딩한 메서드가 있을 경우, 해당 메서드가 부모 클래스의 메서드 대신 실행됨

```java
Vehicle car = new Car(); // 참조 다형성

Car car = new Car();
Vehicle upcasting_car = car; // 업케스팅

// 참조 다형성과 큰 차이가 없다.

class Vehicle {
	public void introduce() {
		System.out.println("어떤 이동수단일까요?");
	}
}

// 업캐스팅될 경우 drive()는 부모 클래스에 없기 때문에 실행 시 에러 발생
class Car extends Vehicle {
	public void drive() {
		System.out.println("자동차 부릉부릉");
	}
	
	public void introduce() {
		System.out.println("저는 자동차입니다.");
	}
}

```

왜 업캐스팅을 사용할까?

- 공통적으로 할 수 있는 부분을 만들어 다루기 쉬워지게 함
- 위의 예시에서 자동차, 트럭, 배 등의 운송 수단이 있을 경우
    - 전부 다른 타입이기 때문에 각각의 타입을 정의해야함

        ```java
        Truck[] truck = new Truck[];
        truck[0] = new Truck();
        truck[1] = new Truck();
        
        Car[] car = new Car[];
        car[0] = new Car();
        car[1] = new Car();
        ```

    - 상속 관계를 통해 업캐스팅하면 하나의 타입으로 묶어서 관리가 가능해짐

        ```java
        Vehicle[] v = new Vehicle[];
        v[0] = new Truck();
        v[1] = new Truck();
        v[2] = new Car();
        v[3] = new Car();
        ```


### 다운캐스팅

---

- 부모 클래스가 자식 클래스 타입으로 캐스팅
- 캐스팅 연산자 괄호 생략 불가능
- 업캐스팅한 객체를 다시 자식 클래스 타입의 객체로 되돌리는데 목적

```java
// 위의 예시에서 사용한 코드
// 업캐스팅으로 인해 기존의 메서드를 사용할 수 없을 때 다운캐스팅 사용
((Car) upcasting_car).drive();
```

**주의점**

- 컴파일 에러가 아닌 런타임 에러가 발생
- 잘못된 코드임에도 프로그램이 동작하는데 문제가 없으며 빨간줄로 알리는 경우도 없다

출처:

https://inpa.tistory.com/entry/JAVA-☕-업캐스팅-다운캐스팅-한방-이해하기 [Inpa Dev 👨‍💻:티스토리]