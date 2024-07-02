# 디자인패턴 - 팩토리 메서드 패턴
토리패턴은 객체 생성과 관련된 디자인 패턴
Simple Factory는 객채 생성 역할을 담당하면서 각 클라이언트에서 구현 클래스에 직접의존하지 않도록 하는 것
하지만 `새로운 클래스가 추가되면 Factory 클래스를 수정해야하는 한계`에 직면

>따라서 기존 코드의 변경없이 확장하기 위한 디자인 패턴이 Factory Method 패턴이다.

## Factory Method
<img width="438" alt="스크린샷 2024-07-02 오후 11 44 05" src="https://github.com/5dotseven/cs-basic-study/assets/144773042/28e0ce9e-2f76-4b15-82f8-73c4a21f641f">
생성 패턴중 하나로 `객채를 생성할 때 어떤 클래스의 인스턴스를 만들 지 서브 클래스에서 결정`

>인스턴스 생성을 서브 클래스에게 위힘

부모 추상 클래스는 인터페이스만 의존하고 실제 구현 클래스 호출은 서브 클래스에서 구현
따라서 새로운 구현 클래스가 추가되어도 기존 Factory코드는 수정없이 새로운 Factory를 추가하면 됨

## Example
```java
// 유저 인터페이스 정의
public interface User { 
	void signup(); 
}
```

```java
// 구현체 정의
// 오버라이드 메서드
public class NaverUser implements User {
	@Override 
	public void signup() {
		System.out.println("네이버 아이디로 가입"); 
	} 
}
```

```java
// 추상클래스로 정의
// 외부에서 User 객체를 생성할때 정의한 메소드 newInstance() 호출
// 실제로 어떤 객체를 생성할지는 추상메서드로 정의해서 -> 하위클래스에서 정의
public abstract class UserFactory {
	public User newInstance() { 
		User user = createUser(); 
		user.signup(); return user; 
	} 
	protected abstract User createUser(); 
}
```

```java
// UserFactory 상속 받고 NaverUserFactory 정의
// 오버라디으 해서 반환
public class NaverUserFactory extends UserFactory {
	@Override 
	protected User createUser() {
		return new NaverUser(); 
	} 
}
```
```java
UserFactory userFactory = new NaverUserFactory(); 
User user = userFactory.newInstance();
```

새로운 Product 추가

```java
public class KakaoUser implements User {
	@Override 
	public void signup() {
		System.out.println("카카오 아이디로 가입"); 
	} 
}
```

```java
public class KakaoUserFactory extends UserFactory {
	@Override 
	protected User createUser() {
		return new KakaoUser(); 
	}
}
```

