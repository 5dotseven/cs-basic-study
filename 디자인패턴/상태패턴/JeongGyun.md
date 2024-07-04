디자인패턴 - 상태패턴
> 상태(현재 진행중인 행위)를 나타낼 때 어떤 방식을 사용할 것가?

우리는 보통 상태를 나타날때 `Enum`(서로 연관된 상수들의 집합) 을 사용해서 나타냄
장점: 편하게 사용할 수 있음

하지만 단점이 있음 객체지향적이라고 보기 애매한 부분이 존재함 -> 상태를 변화 시킬때 조건문을 사용해서 바꾸는데 이게 복잡도를 오히려 올리고 유지보수를 힘들게 할 수도 있음

따라서 상태를 조건문으로 검사해서 행위를 달리하는게 아닌 `상태를 객체화`하여 상태가 행동 할 수 있도록 위임하는 패턴 -> 상태패턴

*객채 지향 프로그램은 클래스를 곡 사물/생물 표현하는게 아닌 객체 내부 상태(동작/행위)도 클래스로 표현할 수 있다*
## State Pattern
<img width="660" alt="스크린샷 2024-07-04 오후 11 54 05" src="https://github.com/5dotseven/cs-basic-study/assets/144773042/e216539c-34b4-4e5b-bacb-5bad6d338605">

1. State인터페이스: 상태를 추상화한 고수준 모듈
2. ConcreateState: 구체적인 각각의 상태를 클래스로 표현
3. Context: State를 이용한 시스템

>상태클래스는 싱글톤으로 구성

## EX) 상태패턴
노트북 전원 상태에 따른 동작을 설계
- 노트북을 On/Off 상황이 존재
- 전원을 누른다면 나타나는 상황
    - On에서 누른 다면 Off
    - Off에서 누른 다면 On
    - 절전모드에서 누른다면 On

`상태를 객체화`
노트북의 상태를 3가지를 모두 클래스로 구현 -> 인터페이스 or 추상클래스로 캡슐화해서 분리
보기에는 별로 일 수 있지만 유지보수 측면에서 이점이 많음

- PowerStste 인터페이스를 만듬
    - SavingState 절전
    - OffState Off상태
    - OnState On상태

```java
interface PowerState {
    void powerButtonPush(LaptopContext cxt);

    void typebuttonPush();
}

class OnState implements PowerState {
    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 OFF");
        cxt.changeState(new OffState());
    }

    @Override
    public void typebuttonPush() {
        System.out.println("키 입력");
    }

    @Override
    public String toString() {
        return "노트북이 전원 ON 인 상태 입니다.";
    }
}

class OffState implements PowerState {
    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 ON");
        cxt.changeState(new OnState());
    }

    @Override
    public void typebuttonPush() {
        throw new IllegalStateException("노트북이 OFF 인 상태");
    }

    @Override
    public String toString() {
        return "노트북이 전원 OFF 인 상태 입니다.";
    }
}

class SavingState implements PowerState {
    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 on");
        cxt.changeState(new OnState());
    }

    @Override
    public void typebuttonPush() {
        throw new IllegalStateException("노트북이 절전 모드인 상태");
    }

    @Override
    public String toString() {
        return "노트북이 절전 모드 인 상태 입니다.";
    }
}
```

```java
class LaptopContext {
    PowerState powerState;
    LaptopContext() {
        this.powerState = new OffState();
    }
    void changeState(PowerState state) {
        this.powerState = state;
    }
    void setSavingState() {
        System.out.println("노트북 절전 모드");
        changeState(new SavingState());
    }
    void powerButtonPush() {
        powerState.powerButtonPush(this);
    }
    void typebuttonPush() {
        powerState.typebuttonPush();
    }
    void currentStatePrint() {
        System.out.println(powerState.toString());
    }
}
```

```java
class Client {
    public static void main(String[] args) {
        LaptopContext laptop = new LaptopContext();
        laptop.currentStatePrint();
    
        // 노트북 켜기 : OffState -> OnState
        laptop.powerButtonPush();
        laptop.currentStatePrint();
        laptop.typebuttonPush();
 
        // 노트북 절전하기 : OnState -> SavingState
        laptop.setSavingState();
        laptop.currentStatePrint();
        // 노트북 다시 켜기 : SavingState -> OnState
        laptop.powerButtonPush();
        laptop.currentStatePrint();
 
        // 노트북 끄기 : OnState -> OffState
        laptop.powerButtonPush();
        laptop.currentStatePrint();
    }
}
```

-> 이것도 문제점이  있음 싱글톤이 아니라 상태를 변경할 때 마다 새로운 객체가 생성됨

수정하면

```java
class OnState implements PowerState {
	// Thread-Safe 한 싱글톤 객체화
    private OnState() {}
    private static class SingleInstanceHolder {
        private static final OnState INSTANCE = new OnState();
    }
    public static OnState getInstance() {
        return SingleInstanceHolder.INSTANCE;
    }
    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 OFF");
        cxt.changeState(OffState.getInstance()); // 싱글톤 객체 얻기
    }
    @Override
    public void typebuttonPush() {
        System.out.println("키 입력");
    }
    @Override
    public String toString() {
        return "노트북이 전원 ON 인 상태 입니다.";
    }
}
```

## VS 전략패턴
유사점
- 사용법이 비슷함 -> 클래스 다이어그램이 거의 유사
- 둘다 합성을 통해 상속의 한계를 극복
- 행동을 갭슐화하여 객체지향원칙을 준수
- 난잡한 조건 분기를 -> 상태로 객체화

차이점
- 구조는 같지만 어떤 `목적`을 위해서 사용되는가 다름
- 전략패턴
    - 알고리즘을 객체화하여 클라이언트에서 유연적으로 젼략을 제공/교체
    - 그 해당 전략만의 알고리즘 동작을 정의 및 수행
    - 전략객체는 입력값에 따라 전략 형태가 다양하게 될 수 있음 -> 인스턴스로 구성
- 상태패턴
    - 객체의 상태를 객체화하는게 목적
    - 상태객체는 상태가 적용되는 대상 객체가 할수있는 모든 행동들을 정의
    - 메모리 절약을 위해 싱글톤으로 구성


출처: [https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%83%81%ED%83%9CState-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90) [Inpa Dev 👨‍💻:티스토리]