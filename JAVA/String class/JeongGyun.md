# JAVA - String class
Java에서 문자열을 다루는 클래스들이 있음
- String
- StringBuffer
- StringBuilder

### 1. String
- **불변성 (Immutable)**: `String` 객체는 한 번 생성되면 변경할 수 없슴. 즉, 문자열을 변경하려면 새로운 문자열 객체가 생성됨
```java
class Test{
	public static void main(String[] args) {
		String str = "Hello";
		System.out.print(str);
		str = str + " World"; // 새로운 String 객체가 생성됨
		System.out.print(str);
	}
}
```
- **장점**: 불변성이 있어 멀티스레드 환경에서 안전
- **단점**: 문자열을 자주 변경해야 하는 경우 메모리와 성능에 비효율적

### 2. StringBuffer
- **가변성 (Mutable)**: `StringBuffer` 객체는 변경 가능합니다. 즉, 동일한 객체 내에서 문자열을 변경할 수 있슴
- **스레드 안전성 (Thread-Safety)**: 모든 메서드가 synchronized로 선언되어 있어 멀티스레드 환경에서 안
```java
  StringBuffer sb = new StringBuffer("Hello"); 
  sb.append(" World"); // 같은 객체 내에서 문자열이 변경됨
```
- **장점**: 문자열을 자주 변경해야 하는 경우 메모리와 성능이 효율적
- **단점**: synchronized로 인해 단일 스레드 환경에서는 약간의 성능 저하가 있을 수 있슴

### 3. StringBuilder
- **가변성 (Mutable)**: `StringBuilder` 객체도 변경 가능합
- **스레드 안전성 없음 (Non-Thread-Safety)**: 메서드가 synchronized로 선언되지 않았기 때문에 단일 스레드 환경에서 빠른 성능을 제공
```java
class Test{
	public static void main(String[] args) {
		StringBuilder sb = new StringBuilder("Hello"); 
	    sb.append(" World"); // 같은 객체 내에서 문자열이 변경됨`
	    System.out.print(sb);
	}
}
```

- **장점**: 단일 스레드 환경에서 `StringBuffer`보다 빠름
- **단점**: 멀티스레드 환경에서 안전하지 않음

### 요약
- **String**: 불변, 멀티스레드 안전, 문자열 변경 시 새로운 객체 생성
- **StringBuffer**: 가변, 멀티스레드 안전, 문자열 변경 시 동일 객체 사용
- **StringBuilder**: 가변, 멀티스레드 안전하지 않음, 단일 스레드 환경에서 빠름

이러한 특성을 이해하고 필요에 따라 적절한 클래스를 선택하는 것이 중요
