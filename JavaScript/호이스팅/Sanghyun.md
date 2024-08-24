## 호이스팅

---

영어로 끌어올리기, 인터프리터가 변수와 함수의 메모리 공간을 선언 전 미리 할당하는 것

```jsx
func();
// Hello World

function func() {
	console.log("Hello World");
}

func();
// Hello World
```

- 위 코드에서 첫 번째 func의 경우 이론적으로 생각해볼 때 실행이 안될 것 같다.
- 하지만 Javascript는 끌어올리기, 호이스팅 덕에 맨 위의 func도 문제 없이 실행된다.

**var 변수 호이스팅**

- var 변수의 경우 선언과 초기화만 이루어진 상태로 호이스팅
- 할당되기 전에 사용할 경우 undefined를 출력하게 된다.

```jsx
console.log(number);
// undefined
var number = "100";
console.log(number);
// 100
```

**const, let 호이스팅**

- const와 let은 var의 모호성과 과도한 유연성을 대체하기 위해 등장
- var와 다르게 선언하기 전에 참조할 경우 참조 오류가 발생한다.

```jsx
console.log(name); 
console.log(number);
// 참조오류
let name = "Michael";
const number = "1000";
```

- 하지만 호이스팅이 아예 안된 것이 아닌 접근만 불가능한 상태이다.

**TDZ**

- Temporal Dead Zone, 일시적으로 죽은 구역
- 위의 코드를 예시로 들면 name, number를 출력하는 두 줄에 해당한다.
- name과 number가 선언되기 전까지 해당 구역은 죽은 구역으로 접근이 불가능하다는 의미

```jsx
console.log(name); 
console.log(number);
// 참조오류
let name = "Michael";
const number = "1000";
```