# 디자인패턴 - 프록시패턴
## 프록시 패턴 (Proxy Pattern) 이란?

---

**프록시 패턴 (Proxy Pattern)은 객체 지향 디자인 패턴 중 하나로,객체 간의 간접적인 접근을 가능하게 하는 구조를 제공하는 패턴**이다.

여기서 프록시 (Proxy)란 대리자, 대변인의 의미를 가지고 있다. 말 그대로 원본 객체를 바로 호출하는 것이 아니라, 원본 객체에 접근할 수 있는 대리자를 호출하는 패턴이다.

어떤 객체를 호출할 때, 객체를 직접 호출하는 것이 아니라 대리자 객체를 호출하는 방식을 사용하면 해당 객체가 메모리에 존재하지 않아도 기본적인 정보를 참조하거나 설정할 수 있다. 또, 실제 객체의 필요 시점까지 객체 생성을 미루는 지연 초기화 (Lazy Initializing)이 가능하다.

## 프록시 패턴의 장단점

---
- 장점
    - **보안성 향상**
        - 원본 객체에 직접 접근하지 않기 때문에 보안성이 향상된다.
        - 프록시 객체를 사용하여 원본 객체 접근 권한을 제한할 수 있다.
        - 접근 로그를 남기는 등 보안적인 추가 기능을 적용할 수 있다.
    - **유연성 향상**
        - 원본 객체에 간접적으로 접근하므로, 객체 간의 결합도를 줄일 수 있다.
    - **성능 향상**
        - 지연 초기화를 통해 객체가 필요한 순간에 필요한 객체만 초기화하여 사용하므로, 불필요한 객체 생성을 줄일 수 있다.
- 단점
    - **복잡성 증가**
        - 객체 간의 중간 계층이 추가되어 코드의 복잡성이 증가한다.
    - **성능 저하**
        - 프록시 객체를 추가로 사용하므로, 객체 생성이 빈번한 경우 성능이 저하될 수 있다.
## **프록시 패턴의 종류**
---
프록시 패턴은 원격 프록시, 가상 프록시, 보호 프록시 등 다양한 형태로 사용된다.
- **원격 프록시**
    - 원격 객체에 대한 대변자 역할을 하여 다른 공간에 있더라도 마치 같은 공간에 있는 객체에 접근한 것 처럼 동작한다.
- **가상 프록시**
    - 꼭 필요로 하는 시점까지 객체의 생성을 연기하고, 해당 객체가 생성된 것 처럼 동작한다.
- **보호 프록시**
    - 객체에 대한 접근을 제어하거나 권한을 달리 하고 싶은 경우 사용한다.
## 프록시 패턴 구현
---

인터페이스를 정의

```java
public interface Image {
	void display();
}
```

위 인터페이스를 구현하는 클래스를 정의한다.

```java
public class RealImage implements Image {
	private String filename;
 
	public RealImage(String filename) {
		this.filename = filename;
		loadFromDisk();
	}

	@Override
	public void display() {
		System.out.println("Displaying " + filename);
	}

	private void loadFromDisk() {
		System.out.println("Loading " + filename);
	}
}
```

이 클래스는 이미지 파일을 불러오는 데 시간이 걸리기 때문에, 이미지를 표시하는 display() 메소드를 호출하기 전에 loadFromDisk() 메소드를 호출하여 이미지를 로드한다.

이제 이 클래스를 프록시 클래스로 래핑한다.

```java
public class ProxyImage implements Image {
	private RealImage realImage;
	private String filename;

	public ProxyImage(String filename) {
		this.filename = filename;
	}

	@Override
	public void display() {
		if (realImage == null) {
			realImage = new RealImage(filename);
		}
		realImage.display();
	}
}
```

이 클래스는 RealImage 객체를 감싸고 있으며, display() 메소드를 호출할 때 RealImage 객체를 생성하고, 이미지를 로드한 후 이미지를 표시한다.

이제 클라이언트는 프록시 객체를 사용하여 이미지를 표시할 수 있다.

```java
public class Client {
	public static void main(String[] args) {
		Image image = new ProxyImage("test.jpg");
 
		// 이미지를 표시한다.
		image.display();
		System.out.println("");

		// 이미지를 다시 표시한다.
		// 이번에는 이미지를 로드하지 않는다.
		image.display();
	}
}
```

RealImage 객체에 직접 접근하지 않고 대리자를 통해 동작을 수행하는 것을 알 수 있다.






출처: [https://jangjjolkit.tistory.com/59](https://jangjjolkit.tistory.com/59) [장쫄깃 기술블로그:티스토리]