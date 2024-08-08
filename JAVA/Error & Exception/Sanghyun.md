## **Error & Exception**

---

프로그래밍 오류의 종류, 자바는 런타임 실행 중 발생할 수 있는 오류를 2가지로 구분

- 에러 - 프로그램 코드에서 수습될 수 없는 심각한 오류
    - 메모리 부족, 스택 오버플로우와 같이 발생하면 복구할 수 없는 심각한 경우
    - 예측이 불가능하며 JVM 실행에 문제가 발생한 것으로 개발자가 대처 불가능
- 예외 - 프로그램 코드에서 수습이 가능한 경미한 오류
    - 발생하더라도 프로그램이 죽지는 않는 경우
    - 하지만 적절한 대응 즉, 예외 처리를 하지 않을 경우 큰 문제로 이어질 수 있다.

<aside>
💡 **에러의 종류**

- **논리 에러**: 프로그램이 정상적으로 실행되지만 개발자의 의도와 다르게 동작하는 경우
    - 흔히 버그라고 부르는 것으로 음료수 1개가 나와야 할게 3개가 나오는 것
    - 컴퓨터 입장에서는 아무 문제가 없기 때문에 에러 메시지가 뜨지 않는다
    - 전반적인 코드와 알고리즘에 대한 검토가 필요
- **컴파일 에러**: 문법 구문 오류로 인해 컴파일 단계에서 발생하는 오류
    - 컴파일이 안되기 때문에 프로그램 실행 자체가 안되는 경우
    - 심각하지 않은 에러로 개발자가 코드를 수정하면 해결이 가능
- **런타임 에러:** 프로그램 실행 중에 발생하는 에러
    - 에러 발생 시 개발자가 직접 역추적으로 통해 원인을 확인해야 함
    - 실행 중 발생할 수 있는 경우를 최대한 고려하여 예외에 대비해야 한다.
</aside>

**자바의 예외 클래스**

- Error 클래스는 개발자의 대처가 불가능한 영역
- Exception에 집중하고 대처를 해야한다.
- Excpetion 및 하위 클래스
    - 사용자의 실수로 인해 발생하는 컴파일 예외
- RuntimeException 클래스
    - 프로그래머의 실수로 인해 발생

![img1 daumcdn](https://github.com/user-attachments/assets/a5d34408-16e5-4d1b-8e31-e8fb049d00be)
**대표적인 예시**

- ArrayIndexOutOfBoundsException
    - 배열의 범위를 넘어선 인덱스를 참조할 경우 발생

    ```java
    int[] arr = {1, 2, 3, 4};
    System.out.println(arr[4]);
    ```

- ArithmeticException
    - 정수를 0으로 나누는 경우 발생
- NullPointException
    - null 객체에 접근하여 메서드를 호출할 경우 발생, 가장 빈번하게 발생

    ```java
    String no = null;
    no.length();
    ```

- NumberFormatException
    - 정수가 아닌 문자열을 정수로 변환하려고 할 때 발생

    ```java
    String num = "3.1415";
    int num = Integer.parseInt(num);
    // num은 float 이므로 Float.parseFloat을 해야한다.
    ```


**Checked Exception/ Unchecked Exception**

![img1 daumcdn 1](https://github.com/user-attachments/assets/d48be005-4bb4-43d3-be9c-9350ce64fc4f)

- 전자는 컴파일 예외 클래스들, 후자는 런타임 예외 클래스들
- 전자는 반드시 예외처리를 해야하며 후자는 명시적인 처리를 하지 않아도 된다.
    - Checked는 명시적인 예외처리를 하지 않을 시 컴파일 자체가 되지 않음
    - try - catch를 통해 로직을 감싸거나 throws를 던져 처리를 해야한다.
- Unchecked의 경우 개발자가 신경을 쓰면 회피할 수 있기 때문에 예외처리가 강제되지 않는다.

**Checked를 Unchecked로 변환**

- 코드마다 하나하나 하는 것이 가독성을 해치거나 번거로울 경우 사용
- Chained Exception을 통해 변환하면 예외처리를 하지 않아도 된다.
    - 예외를 다른 예외로 감싸는 것
    - A예외를 B예외로 감싸 던질 경우 A를 B의 원인 예외라고 부른다.

```java
class MyCheckedException extends Exception { ... } // checked excpetion

public class Main {
    public static void main(String[] args) {
            install();
    }

    public static void install() {
        throw new RuntimeException(new IOException("설치할 공간이 부족합니다."));
        // Checked 예외인 IOException을 Unchecked 예외인 RuntimeException으로 감싸 Unchecked 예외로 변신 시킨다
    }
}
```

**throw & thorws**

- throw는 강제로 예외를 발생시킬수 있다.
    - 개발자는 자신이 원하는 부분에서 에러를 직접 발생시켜 로그에 기록이 가능
- throws는 예외를 메서드에서 처리하지 않고 메서드를 호출한 곳에서 처리하게 한다.

    ```java
    public class Main {
        public static void main(String[] args) {
            method1();
            method2();
            method3();
        }
    
        public static void method1() {
            try {
                throw new ClassNotFoundException("에러이지롱");
            } catch (ClassNotFoundException e) {
                System.out.println(e.getMessage());
            }
        }
    
        public static void method2() {
            try {
                throw new ArithmeticException("에러이지롱");
            } catch (ArithmeticException e) {
                System.out.println(e.getMessage());
            }
        }
    
        public static void method3() {
            try {
                throw new NullPointerException("에러이지롱");
            } catch (NullPointerException e) {
                System.out.println(e.getMessage());
            }
        }
    }
    
    // throws 사용
    public class Main {
        public static void main(String[] args) {
            try {
                method1();
                method2();
                method3();
            } catch (ClassNotFoundException | ArithmeticException | NullPointerException e) {
                System.out.println(e.getMessage());
            }
        }
    
        public static void method1() throws ClassNotFoundException {
            throw new ClassNotFoundException("에러이지롱");
        }
    
        public static void method2() throws ArithmeticException {
            throw new ArithmeticException("에러이지롱");
        }
    
        public static void method3() throws NullPointerException {
            throw new NullPointerException("에러이지롱");
        }
    }
    ```


출처:

https://inpa.tistory.com/entry/JAVA-☕-에러Error-와-예외-클래스Exception-💯-총정리 [Inpa Dev 👨‍💻:티스토리]