# 디자인패턴 - 브리지 패턴
브리지 패턴이란?
: 객체의 확장성을 향상하기 위한 패턴 -> 객체에서 동적을 처리하는 구현부와 확장을 위한 추상부를 분리해서 사용

## 정의
- 브리지 패턴은 기능을 처리하는 클래스와 구현을 담당하는 추상클래스로 구별
- 구현뿐만 아니라 추상화도 독립적 변경이 필요할 때 사용
- 2개의 객체는 추상화를 구현에서 분리하여 매우 독립적으로 사용할 수 있어야 함
- 부수적인 새로운 기능을 지속적으로 추가할때 유용
- 새로운 인터페이스를 정의하여 기존 코드에 변경 없이 기능을 확장

<img width="713" alt="스크린샷 2024-07-06 오후 10 48 33" src="https://github.com/5dotseven/cs-basic-study/assets/144773042/8c75347c-72b5-4bfb-9d3a-68745e830ab8">


### 예시1
- 컵을 판매하는 회사임
- 컵의 재질
    - 플라스틱
    - 유리
    - 종이
- 컵의 색상
    - 빨간색
    - 파란색
    - 노란색
    - 초록색
- 컵을 클래스화 시켜서 자바프로그램 녹여넣고 싶은데 어떻게 할까
> 이때 사용하는게 브리지 패턴

상속
- 먼저 `Cup` 클래스를 만들었다.
- 컵의 재질을 표현하기 위해서 `Material` 인터페이스를 추가했다.
- `Material` 을 상속하는 `Plastic`, `Glass`, `Paper` 등을 만들었다.
- 이를 이용해 `Cup` 에 `Material` 을 상속시켜 `PlasticCup`, `GlassCup`, `PaperCup` 등을 만들었다.
- 이번엔 컵의 색상을 표현하기 위해서 `Color` 인터페이스를 추가했다.
- `Color` 를 상속시켜 `Red`, `Green`, `Blue` 등을 만들었다.
- `PlasticCup` 에 `Red` 를 상속시킨 `RedPlasticCup`, `Green` 을 상속시킨 `GreenPaperCup` 등을 성공적으로 만들어냈다.
- 시간이 지날수록 새로운 색이 추가되고 새로운 재질의 컵이 탄생했다.
- 상속 클래스가 수십개가 탄생했고, 새로운 재질과 색상이 나올 때마다 컵을 추가해야 한다.

하지만

사용(composition)
- `Cup` 클래스를 만드는 것까지는 동일하다.
- `Cup` 에 `Material` 과 `Color` 인터페이스를 필드로 추가했다.
- `Material` 과 `Color` 를 인자로 받는 생성자만 만들어주었다.
- 이제 어떤 재질의 어떤 색상의 컵을 사용하더라도 더 이상 클래스를 추가할 필요는 없다.

>사용에 개념을 사용하는게 바로 브리지 패턴
>클래스를 별도의 계층으로 추출함으로써 원래 클래스가 한 클래스 내의 모든 상태 및 동작을 갖지않고 새 계층의 객체를 참조

### 코드 예제
- 멀티플랫폼을 지원하는 GUI 도구
- Mac 운영체제와 Windows 운영체제의 버튼을 만들어야 한다.
- 버튼의 요구사항은 아래와 같다.

    - 버튼에는 색상이 들어갈 수 있다.
    - 버튼을 클릭하면 실행될 이벤트를 줄 수 있다.


### Button
```java
public interface Button {
    void onClick();
}
```

- `Button` 은 인터페이스로 클릭하면 실행되는 `onclick` 이벤트를 가지고 있다.
### Color

```java
public interface Color {
    String getColorCode();
}
```

- `Color` 도 인터페이스로 색상의 색상코드를 이용해 버튼의 색을 칠한다.

### [`Red`](#Red)

```java
public class Red implements Color {
    @Override
    public String getColorCode() {
        return "FF0000";
    }
}
```

- 빨간색을 구현한 것이다.

### [`Blue`](#Blue)

```java
public class Blue implements Color {
    @Override
    public String getColorCode() {
        return "0000FF";
    }
}
```

- 파란색을 구현한 것이다.

### [`MacButton`](#MacButton)

```java
public class MacButton implements Button{
    private final Color color;

    public MacButton(Color color) {
        this.color = color;
    }

    @Override
    public void onClick() {
        System.out.println("맥 버튼 클릭, 색상 컬러: " + color.getColorCode());
    }
}
```

- 맥OS 에 사용되는 버튼을 만들었다.

    - 생성자에서 `Color` 를 받고, `onClick` 이벤트는 직접 구현하면 된다.


### [`WindowsButton`](#WindowsButton)

```java
public class WindowsButton implements Button{
    private final Color color;

    public WindowsButton(Color color) {
        this.color = color;
    }

    @Override
    public void onClick() {
        System.out.println("윈도우즈 버튼 클릭, 색상 컬러: " + color.getColorCode());
    }
}
```

- windows 에도 동일한 버튼을 만들어보았다.

### [클라이언트에서 사용하기](#%ED%--%B-%EB%-D%BC%EC%-D%B-%EC%--%B-%ED%-A%B-%EC%--%--%EC%--%-C%--%EC%--%AC%EC%-A%A-%ED%--%--%EA%B-%B-)

```haxe
public class Client {
    public static void main(String[] args) {
        Button macButton = new MacButton(new Red());
        Button windowsButton = new WindowsButton(new Blue());

        macButton.onClick(); // 맥 버튼 클릭, 색상 컬러: FF0000
        windowsButton.onClick(); // 윈도우즈 버튼 클릭, 색상 컬러: 0000FF
    }
}
```

출처: [https://jake-seo-dev.tistory.com/397](https://jake-seo-dev.tistory.com/397) [제이크서 위키 블로그:티스토리]

