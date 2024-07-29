# JAVA - 프리미티브타입 & 레퍼런스 타입
> 프리티브타입과 레퍼런스타입에 차이점에 대해 정리해서 자바의 메로기관리 방식과 겍체지향을 이해

## Primitive Type
> 가장 기본적인 데이터 타입으로 정수, 실수, 불리언과 같은 간단한 값을 직접 저장함
- byte
- short
- int
- long
- double
- char
- boolean

이렇게 총 8가지가 존재함. 프리미티브타입의 가장 큰 특징은 `값이 생성될 때 메모리의 스택 영역에 직접 할당됨, 함수 호출이 끝나면 자동으로 사라지는 특성을 가짐`
```java  
int number = 10;  
```  
이는 정수 10을 스텍 메모리에 저장한다는 뜻 따라서 프리미티브 타입 변수가 직접 값을 가지고 있음을 의미함 이렇게 사용하는 이유는 `스택 메모리는 실행 속도가 빠르고 관리가 슆기 때문`
## Reference Type
> 객체의 메모리 주소를 저장하는 타입
- 클래스
- 배열
- 인터페이스

등이 여기에 속해 있음 이 타입의 변수는 `실제 데이터가 아닌 데이터가 저장된 메모리의 주소를 가리킴`

## 기본형의 한계
자바는 객체지향언어 기본형을 사용하면 장점을 살리지못함 왜냐하면 `객체가 아니니까`
- 객체가 아님
    - 객체지향 장점 살리기 힘듬
    - 제네릭 사용불가
    - 겍체 참조가 필요한 프레임워크 사용 불가
- Null 값을 가질 수 없음
    - 데이터가 없음이라는 상태가 필요한경유 표현 할 수 없음

```java
public class Primitivetype{
	public static void main(String[] args) {
		int value = 10;
		int i1 = compareTo(value, 5);
        int i2 = compareTo(value, 10);
        int i3 = compareTo(value, 20);
        System.out.println("i1 = " + i1);
        System.out.println("i2 = " + i2);
        System.out.println("i3 = " + i3);
	}

	public static int compareTo(int value, int target) {
		if (value < target) {
			return -1;
		} else if (value > target) {
             return 1;
        } else {
			return 0;
		}
	}
}
```

-> value와 비교대상 값을 비교하는 copareTo() 외부 메소드를 사용할 떄 자기자신인 value를 사용함 따라서 객체로서 비교를하는게 좀더 좋음

```java
public class MyInteger {

	private final int value;
    
    public MyInteger(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }

    public int compareTo(int target) {
        if (value < target) {
            return -1;
        } else if (value > target) {
            return 1;
        } else {
	        return 0; 
	    }
	}

    @Override    
   public String toString() {
		return String.valueOf(value); //숫자를 문자로 변경 
	}
}

public class dMain {
	public static void main(String[] args) {
	MyInteger myInteger = new MyInteger(10);
		int i1 = myInteger.compareTo(5);
        int i2 = myInteger.compareTo(10);
        int i3 = myInteger.compareTo(20);
        System.out.println("i1 = " + i1);
        System.out.println("i2 = " + i2);
        System.out.println("i3 = " + i3);
    } 
}
```

### Null 표현
```java
public class MyIntegerNullMain0 {
    public static void main(String[] args) {
	    int[] intArr = {-1, 0, 1, 2, 3};
        System.out.println(findValue(intArr, -1)); //-1
        System.out.println(findValue(intArr, 0));
        System.out.println(findValue(intArr, 1));
        System.out.println(findValue(intArr, 100)); //-1
    }
	private static int findValue(int[] intArr, int target) {
		for (int value : intArr) {
            if (value == target) {
                return value;
            }
        }
		return -1; 
	}
}
```
- int는 기본형임 따라서 값을 항상 내보내야함
- 배열에 타켓 숫자가 있는지 보는 메소드
    - 해당 값이 있다면 자기자신을
    - 해당 값이 없다면 -1로 표현

`여기서 문제가 발생 입력값으로 -1을 넣었을때 저 값이 없어서 -1인지 아니면 배열에 있어서 -1이 있어서 나온건지 구분이 안됨

따라서 객체로써 가지고 있어야 null을 표현할 수 있다.

```java
public class MyIntegerNullMain1 {

    public static void main(String[] args) {
        MyInteger[] intArr = {
	        new MyInteger(-1), new MyInteger(0), new MyInteger(1)
	    };
        
        System.out.println(findValue(intArr, -1));
        System.out.println(findValue(intArr, 0));
        System.out.println(findValue(intArr, 1));
        System.out.println(findValue(intArr, 100)); //-1
}

    private static MyInteger findValue(MyInteger[] intArr, int target) {
		for (MyInteger myInteger : intArr) {
	        if (myInteger.getValue() == target) {
	            return myInteger;
	        }
	    }
        return null;
     }
}
```

이렇게 표현 한다면 참조형(객체)이므로 null을 표현할 수 있음 -> NullPointerException 주의

>따라서 이런식으로 표현되는 것이 래퍼클래스

자바에서는 기본적으로 래퍼클래스를 제공함
- byte -> Byte
- Short -> Short
- int -> Integer
- long -> Long
- float -> Float
- double -> Double
- char -> Char
- boolean -> Boolean

## Reference & Additional Resources
- https://medium.com/@s23051/%EB%9E%98%ED%8D%BC-%ED%81%B4%EB%9E%98%EC%8A%A4%EB%9E%80-wrapper-class-cc5aa6f7cdd1
- [https://www.inflearn.com/course/lecture?courseSlug=김영한의-실전-자바-중급-1&unitId=212224](https://www.inflearn.com/course/lecture?courseSlug=%EA%B9%80%EC%98%81%ED%95%9C%EC%9D%98-%EC%8B%A4%EC%A0%84-%EC%9E%90%EB%B0%94-%EC%A4%91%EA%B8%89-1&unitId=212224 "https://www.inflearn.com/course/lecture?courseSlug=김영한의-실전-자바-중급-1&unitId=212224")