## **직렬화 Serialization**

---

자바 언어에서 사용되는 Object나 Data를 다른 컴퓨터의 자바 시스템에서 사용 가능하게 바이트 스트림 형태로 연속적인 데이터로 변환하는 포맷 변환 기술

- 힙, 스택 영역의 데이터를 직렬화하여 DB에 저장
- 다른 컴퓨터에서 파일들을 가져와 역직렬화하여 자바 객체로 변환 및 사용

    ![img1 daumcdn](https://github.com/user-attachments/assets/35be5a75-2007-41bc-b94c-244f7a5450dc)

**사용처**

- JVM의 객체 데이터를 영속화할 경우
- 서블릿 세션
    - 세션 데이터를 저장, 공유할 경우
- 캐시
    - 조회한 객체 데이터를 다른 모듈에서도 필요할 때 다시 DB를 조회할 필요 없이 메모리나 외부 파일에 저장해 공유 → 캐시 데이터로 활용이 가능
    - 자바 시스템에서 구현이 가장 간편해 많이 사용됨
    - 최근에는 Redis, Memcached같은 캐시 DB를 사용
- 자바 RMI
    - 원격 시스템 간의 메시지 교환을 위해 자바에서 지원하는 기술
    - 메시지를 직렬화하여 송신, 최근에는 소켓의 사용으로 인해 잘 사용되지 않음

**왜 JSON 대신 직렬화를?**

- 직렬화는 자바 고유의 기술인 만큼 자바 시스템에 최적화됨
- 자바의 레퍼런스 타입에 대한 제약 없이 외부로 내보낼 수 있다.
    - 컬렉션, 클래스, 인터페이스 타입, 사용자 커스텀 자료형 등의 경우 파일 포맷만으로는 한계가 존재하며 별도의 파싱 과정이 필요
    - 직렬화를 사용하면 다른 언어 시스템에서는 사용이 불가하더라도 같은 자바 시스템 내에서는 역직렬화로 데이터 타입이 자동으로 맞추어지기 때문에 곧바로 사용이 가능해 편리

**직렬화 방법**

- java.io.Serializable 인터페이스 implement

```java
import java.io.Serializable;

class Customer implements Serializable {
    int id; // 고객 아이디
    String name; // 고객 닉네임
    String password; // 고객 비밀번호
    int age; // 고객 나이

    public Customer(int id, String name, String password, int age) {
        this.id = id;
        this.name = name;
        this.password = password;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Customer{" +
                "id=" + id +
                ", password='" + password + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

- ObjectOutputStream을 사용해 직렬화
- 직렬화 시 객체의 인스턴스 필드값 만을 저장, static 필드나 메서드는 저장하지 않음
- 실행하면 customer.ser 파일이 생성, .txt 확장명도 가능하지만 직렬화된 파일임을 명시하기 위해 .ser, .obj를 자주 사용한다.

```java
public static void main(String[] args) {
    // 직렬화할 고객 객체
    Customer customer = new Customer(1, "홍길동", "123123", 40);

    // 외부 파일명
    String fileName = "Customer.ser";

    // 파일 스트림 객체 생성 (try with resource)
    try (
            FileOutputStream fos = new FileOutputStream(fileName);
            ObjectOutputStream out = new ObjectOutputStream(fos)
    ) {
        // 직렬화 가능 객체를 바이트 스트림으로 변환하고 파일에 저장
        out.writeObject(customer);

    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

**역직렬화 방법**

- ObjectInputStream을 사용
    - 직렬화된 대상이 된 객체의 클래스가 외부 클래스라면, 클래스 경로에 존재해야하며 import 된 상태여야 한다.
- 역직렬화를 하면 생성자로 객체 초기화 없이 바로 객체 정보를 가져오고 인스턴스화 가능

```java
public static void main(String[] args) {
    // 외부 파일명
    String fileName = "Customer.ser";

    // 파일 스트림 객체 생성 (try with resource)
    try(
            FileInputStream fis = new FileInputStream(fileName);
            ObjectInputStream in = new ObjectInputStream(fis)
    ) {
        // 바이트 스트림을 다시 자바 객체로 변환 (이때 캐스팅이 필요)
        Customer deserializedCustomer = (Customer) in.readObject();
        System.out.println(deserializedCustomer);

    } catch (IOException | ClassNotFoundException e) {
        e.printStackTrace();
    }
}
```

출처:

https://inpa.tistory.com/entry/JAVA-☕-직렬화Serializable-완벽-마스터하기 [Inpa Dev 👨‍💻:티스토리]