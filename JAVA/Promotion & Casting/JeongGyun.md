# JAVA - Promotion & Casting
> 데이터 타입을 변화하는 방법

이것을 하기 전 `자료형의 크기`에 대해 알고 있어야함
`자료형의 크기`와 `자료형이 선언했을 때 부여되는 메모리 공간`은 별개의 문제이다.

이 표는 자료형이 표현할 수 있는 숫자의 범위를 기준으로 나열한 것

byte < short < int < long < float < double < String
<br/>
<------정수형변수----><---- 실수형 변수---->

![스크린샷 2024-07-24 오후 11 15 13](https://github.com/user-attachments/assets/bf6fae0f-dea8-4f99-9e9a-f3e50690d7cc)
long 변수는 8바이트, float 변수는 4바이트의 공간을 할당 받지만 float 변수가 더 크다고 되어있음

이는 기본적으로 표현할 수 있는 숫자의 범위가 `실수형 변수가 정수형 변수보다 크다고 정의하기 때문임`

따라서 `long 데이터 타입에서 float 데이터 타입으로 자동 형변환이 가능`하지만 float -> long 자동형변환 불가

## 프로모션 (자동 형 변환)
> 작은 데이터 타입의 값을 더 큰 데이터 타입으로 자동 변환하는 과정

풀어서 얘기하면 크기가 작은 자료형을 더 큰 자료형에 대입할 때 자동으로 작은 자료형이 큰 자료형으로 변환되는 현상

float형 변수에 int형 데이터를 집어넣으려고 하는 상황
(따라서 int < float 이기 때문에 자동으로 변환됨)
```java
public class Main {
	public static void main(String[] args) {
		int a = 10;
		float b = a;
		System.out.println(a);
		System.out.println(b);
		//10
		//10.0
	}
}
```
`정수형에서 실수형으로 변환 된 것을 확인할 수 있음`

## 캐스팅(명시적 형 변환)
> 큰 데이터 타입의 값을 작은 데이터 타입으로 변환하거나, 서로 다른 데이터 타입 간의 변환을 수행 할 때 사용

풀어서 얘기하면 크기가 더 큰 자료형을 더 작은 자료형에 대입할 때 `자료형을 명시해서 강제로 집어넣는 것을 말함`

비유하자면 야구공만 들어 갈 수 있는 글러브에 럭비공을 억지로 쑤셔 넣는 것 따라서 `데이터의 손실이나 변형이 발생 할 수 있음`

int형 변수에 float형 변수를 넣으려고 하는 상황
```java
public class Main {
	public static void main(String[] args) {
		float a = 10;
		int b = a;
		System.out.println(a);
		System.out.println(b);
		// 에러 발생
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		float a = 10;
		int b = (int) a;
		System.out.println(a);
		System.out.println(b);
		// 10.0
		// 10
		// 소수점 데이터 손실 발생
	}
}
```
`실수형에서 정수형으로 변환 된 것을 확인가능 하지만 데이터가 손실 됨`

