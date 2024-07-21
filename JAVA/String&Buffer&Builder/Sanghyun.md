## String & StringBuffer & StringBuilder

---

**StringBuffer/ StringBuilder**

- 문자열 연산을 효율적으로 실행하게 해주는 자료형
- String 자료형으로도 +, concat을 통해 문자열을 붙일수 있지만 속도가 느려 효율이 떨어진다.
- 기본적으로 Buffer라는 독립적인 공간을 가지기 때문에 공간의 낭비 없이 빠른 문자열 연산이 가능하다.
- 기본적으로 16개의 문자열을 저장할 수 있는 공간이 제공되지만 연산 중 초과할 경우 자동으로 버프를 증강 시킨다.
- 내장 메서드 - StringBuffer와 StringBuilder는 동일한 메서드를 가진다

    ```java
    String str = "Hello World";
    StringBuffer sb = new StringBuffer(str);
    
    System.out.println("문자열 String 변환 : " + sb.toString()); // StringBuffer를 String으로 변환하기
    
    System.out.println("문자열 추출 : " + sb.substring(2,4)); // 문자열 추출하기
    
    System.out.println("문자열 추가 : " + sb.insert(2,"추가")); // 문자열 추가하기
    
    System.out.println("문자열 삭제 : " + sb.delete(2,4)); // 문자열 삭제하기
    
    System.out.println("문자열 연결 : " + sb.append("hijk")); // 문자열 붙이기
    
    System.out.println("문자열의 길이 : " + sb.length()); // 문자열의 길이구하기
    
    System.out.println("용량의 크기 : " + sb.capacity()); // 용량의 크기 구하기
    
    System.out.println("문자열 역순 변경 : " + sb.reverse()); // 문자열 뒤집기
    ```

- 가변형이다. 동일 객체 내에서 문자열 크기를 변경하기 때문에 String 보다 연산이 빠르다.

**String**

- 불변 자료형, 값의 변경이 불가능하다, 실제로 String 객체는 final 형으로 선언되어 있다.

    ```java
    String str = "hello";
    str = str + " world";
    
    System.out.println(str); // hello world
    ```

- 위의 코드를 실행하게 되면 str은 “hello”가 담긴 메모리 공간이 아닌 새롭게 생성된 “hello world”가 담긴 메모리 공간을 가리키게 된다.
- 기존의 “hello”는 가비지 컬렉터가 처리하며 toUppercase, trim 등의 메서드를 사용해도 동일하게 새로운 String 객체를 생성하여 리턴하는 것이며 기존의 String 객체는 유지된다.

**값의 비교**

- .equals를 사용하여 값을 비교하는 경우 String과 다르게 StringBuffer, StringBuilder는 .equals() 메서드를 오버라이드 하지 않기 때문에 ==으로 비교한 것과 같은 결과가 나온다.
- 따라서 toString() 메서드를 통해 String 객체로 변환하고 난 다음 equals를 사용해야 한다.

**StringBuilder vs StringBuffer**

- 멀티 쓰레드에서 안전한가의 차이
- StringBuilder는 tread unsafe, StringBuffer는 thread safe
- StringBuilder는 다른 쓰레드의 작업을 기다리지 않기 때문에 요청이 제대로 수행되지 않는 경우가 발생, 비동기로 동작하는 경우는 StringBuffer를 사용하는 것이 안전하다.
- 하지만, 성능이 우선시되는 경우 쓰레드 안정성을 고려하지 않을 경우에는 String Builder를 사용하는 것이 좋다.
    - 현재 현업에서는 대부분의 환경이 멀티 쓰레드 환경이므로 StringBuffer를 쓰는 것이 좋다.
    - 실제 성능도 미미한 차이

출처:

[https://inpa.tistory.com/entry/JAVA-☕-String-StringBuffer-StringBuilder-차이점-성능-비교](https://inpa.tistory.com/entry/JAVA-%E2%98%95-String-StringBuffer-StringBuilder-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%84%B1%EB%8A%A5-%EB%B9%84%EA%B5%90)