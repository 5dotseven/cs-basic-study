## Call by Value vs Call by Reference

---

메서드를 호출할 때 파라미터를 전달하는 방법

```java
public class main
{
    public static void main(String[] args)
    {
        Sample sample = new Sample();

        int var = 1; // primitive 타입 변수 int
        int[] arr = { 1 }; // reference 타입 변수 int[] 배열

        // 변수 자체를 보냄 (call by value)
        add_value(var);
        System.out.println(var); // 1 : 값 변화가 없음

        // 배열 자체를 보냄 (call by reference)
        add_reference(arr);
        System.out.println(arr[0]); // 101 : 값이 변화함
    }

    static void add_value(int var_arg) {
        var_arg += 100;
    }

    static void add_reference(int[] arr_arg) {
        arr_arg[0] += 100;
    }
}
```

### Call by value

![img1 daumcdn 2](https://github.com/user-attachments/assets/cd95a717-c75a-4dbd-a897-c6aad09cc676)

- primitive 변수 int인 var은 1의 값을 그대로 가지고, reference 타입인 arr의 실제 데이터는 heap 영역에 저장
- 스택 영역에는 arr의 값이 아닌 주소값을 저장한다.

![img1 daumcdn 3](https://github.com/user-attachments/assets/0006d2f9-b994-411e-84ee-3454e74cf3da)

- add_value 호출 시 새 스택 프레임이 생성
- 지역 변수 var_arg가 var의 값 1을 받은 상태로 생성된다.
    - 로직으로 인해 var_arg는 101로 변하지만 main 스택 프레임 안에 잇는 1의 값은 영향을 받지 않았다.

### Call by reference

![img1 daumcdn 4](https://github.com/user-attachments/assets/9efc1ca3-e419-427f-85ac-0a913699297b)

- arr_arg는 동일하게 새로운 스택 프레임 안에 생성되지만 복사된 값은 arr의 주소값이다.
- 두 주소는 동일한 메모리를 가리키고 있기 대문에 하나의 데이터를 동시에 참조하고 있다고 볼 수 있다.
- 따라서 따로 return을 하지 않아도 Heap 영역의 주소에 있는 데이터가 변경되기 때문에 출력값은 101이 된다.

<aside>
💡 Java는 C와 다르게 포인터가 철저하게 숨겨져있다.
C는 *( 포인터 ), & ( 주소연산자 )를 통해 주소값을 직접 사용하여 메모리 참조가 가능
따라서 엄밀히 따진다면 자바는 call by value만 가능하며 value가 원시값인지 주소값인지의 차이가 있을 뿐이다.

</aside>

출처:

https://inpa.tistory.com/entry/JAVA-☕-자바는-Call-by-reference-개념이-없다-❓ [Inpa Dev 👨‍💻:티스토리]