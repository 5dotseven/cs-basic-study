ㅛ# 데코레이터 패턴
>객체 지향 프로그래밍에서 사용되는 구조적 디자인 패턴

이 패턴은 지족 객체의 기능을 동적으로 확장할 수 있게 해줌 -> 상속보다 유연한 방식으로 객체의 책임을 추가할 수 있는 방식

## 구조패턴
: 구조 패턴이란 작은 클랙스들을 상속과 합섭을 이용하여 더 큰 클래스를 생성하는 방법을 제공하는 패턴
이 패턴을 사용하면 서로 독립적으로 개발한 클래스 라이브버리를 마치 하나인 양 사용 가능 또한 여러 인터페이스를 합성하여 서로 다른 인터페이스들의 통일된 추상을 제공

중요포인트: 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공하는 것 이는 `컴파일 단계에서가 아닌 런타임 단계에서 복합 방법이나 대상을 변경할 수 있다는 점에서 유연성을 갖는다.`

![스크린샷 2024-06-19 오후 11 54 38](https://github.com/5dotseven/MurheHelp/assets/144773042/cf38685b-12f2-4ae2-8cf4-59f0991b591d)


```java
public interface Car {
	public void assemble();
}
```

```java
public class BasicCar implements Car {
 
	@Override
	public void assemble() {
		System.out.print("Basic Car.");
	}
}
```

```java
public class CarDecorator implements Car {
 
	protected Car car;
	
	public CarDecorator(Car c){
		this.car=c;
	}
	
	@Override
	public void assemble() {
		this.car.assemble();
	}
}
```

```java
public class LuxuryCar extends CarDecorator {
 
	public LuxuryCar(Car c) {
		super(c);
	}
	
	@Override
	public void assemble(){
		super.assemble();
		System.out.print(" Adding features of Luxury Car.");
	}
}
```