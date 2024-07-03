# 디자인패턴 - 추상팩토리
추상팩토리 -> 팩토리 메서트 패턴과 비슷
하지만 분명히 차이점이 존재 팩토리 메서드 패턴은 `어떤 객체를 생성 할지에 집중`하고 추상 팩토리에 패턴은 `연관된 객체들을 모아둔다는 것에 집중`

## Abstract Method
<img width="559" alt="스크린샷 2024-07-03 오후 11 29 21" src="https://github.com/5dotseven/cs-basic-study/assets/144773042/829397a0-6a69-4667-82dc-d0f79c75b9e9">

추상 팩토리 태펀은 -> 연관된 객체들의 생성을 하나의 팩토리에서 담당
## Example
```java
// 스포츠팀을 만듬
// 각 팀의 매니저
public interface Manager { } 

public class SoccerManager implements Manager { } 

public class TennisManager implements Manager { }
```

```java
// 각 팀의 선수
public interface Player { } 

public class SoccerPlayer implements Player { } 

public class TennisPlayer implements Player { }
```

팩토리 구성

```java
public interface StaffFactory { 
	Manager createManager(); 
	Player createPlayer(); 
} 

public class SoccerStaffFactory implements StaffFactory {
	
	@Override
	public Manager createManager() {return new SoccerManager();} 
	
	@Override 
	public Player createPlayer() {return new SoccerPlayer();} 
}

public class TennisStaffFactory implements StaffFactory {
	
	@Override 
	public Manager createManager() { return new TennisManager(); }
	@Override 
	public Player createPlayer() { return new TennisPlayer(); } 
}
```

> 공통된 집합을 모아두는 것이 추상팩토리

```java
public class AbstractFactoryApp { 
	public static void main(String[] args) { 
		use(new SoccerStaffFactory()); 
		use(new TennisStaffFactory()); 
	} 
	private static void use(StaffFactory factory) { 
		Manager manager = factory.createManager(); 
		Player player = factory.createPlayer(); 
	}
}
```