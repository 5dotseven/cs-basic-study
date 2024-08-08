## 문자열 클래스

---

- Java에서 String은 기본형이 아닌,참조형. int, char 등의 변수와 다르다.
- 스택 영역이 아닌, Heap 영역에서 다루어진다.
    - 따라서 String은 같은 문자열을 다루는 StringBuffer, StringBuilder와 다르게 불변하다
    - 한 번 선언되면 메모리 공간 안에서 값이 변하지 않는다.
    - concat, + 연산 또는 새로운 문장을 할당하는 경우와 같이 변화가 있을 경우 새로운 메모리 공간을 할당하여 String 객체를 생성하고 주소값을 바꾼다.

  ![img1 daumcdn](https://github.com/user-attachments/assets/66ce792d-e268-4a40-a46e-35f8587c968c)

- 따라서 동기화에 신경 쓸 필요가 없고 단순 조회가 빠르다.
- 하지만, 연산이 일어날 경우 기존 객체는 GC로 처리하고 새로운 메모리 공간을 할당하기 때문에 성능이 나쁘다.

**왜 불변일까?**

- JVM에서 String Constant Pool 이라는 독립적인 영역을 생성, 문자열을 constant화
    - 데이터 캐싱이 일어나기 때문에 성능적인 이득
    - new를 사용해 String을 생성하게 되면 heap 공간을 따로 차지하게 되므로 new 없이 사용되는 것이 좋다
    - s1, s2는 같은 메모리 주소를 가리키고 있음, String Constant Pool에 있는 String을 재사용

  ![img1 daumcdn 1](https://github.com/user-attachments/assets/768288c2-1b62-4de9-b304-82b4c55c4d35)

- 데이터가 불변하면 멀티 쓰레드 환경에서 동기화로 인한 문제가 발생하지 않기 때문에 안전하다.
- 보안적인 측면, 문자열 값을 불변으로 만들어 외부에서 악의적인 변경을 통한 피해를 줄일 수 있다.

**==, .equals()**

```java
String str1 = "Hello"; // 문자열 리터럴을 이용한 방식
String str2 = "Hello";

String str3 = new String("Hello"); // new 연산자를 이용한 방식
String str4 = new String("Hello");

// 리터럴 문자열 비교
System.out.println(str1 == str2); // true

// 객체 문자열 비교
System.out.println(str3 == str4); // false
System.out.println(str3.equals(str4)); // true

// 리터럴과 객체 문자열 비교
System.out.println(str1 == str3); // false
System.out.println(str3.equals(str1)); // true
```

- ==은 주소값을 비교하게 된다.
    - 따라서 같은 String Constant Pool에 있는 str1과 str2는 true
    - 다른 Heap 영역에 있는 str3, str4 와는 false 값이 나오게 된다.
    - str3와 str4도 new를 통해 생성, 다른 영역에 있다.
    - 즉 현재 3개의 별개의 공간이 존재하는 것
- .equals는 대상의 값 자체를 비교한다.
    - 따라서 공간이 달라도 주소값에 있는 값 자체를 비교하기 때문에 결과가 정확한다.

**intern()**

- String 리터럴을 생성할 때 내부적으로 실행되는 메서드
- pool안에 해당 값이 존재하는지 확인하고 있다면 리터럴을 리턴 없으면 리터럴을 넣고 주소값을 반환한다.

    ```java
    String s1 = "Hello"; // 문자열 리터럴을 이용한 방식
    String s2 = "Hello";
    
    String s3 = new String("Hi"); // new 연산자를 이용한 방식
    String s4 = "Hi";
    
    s3 = s3.intern();
    // s3.intern() 을 통해 String pool 안에 있는 Hi 주소값으로 변경된다.
    
    System.out.println(s4 == s3);  // true
    ```

    ![img1 daumcdn 2](https://github.com/user-attachments/assets/ba17ef80-4ac7-4588-88ec-cb4ebd5d7e2c)