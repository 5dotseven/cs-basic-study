# 자바스크립트 - 호이스팅
자바스크립트
- 프로토타입언어에서는 '분류'를 우선하지 않음
    - 생성된 객체 위주로 유사성을 정의
- 어휘, 쓰임새는 맥락(context)에 의해 평가됨
    - 실행 컨텍스트, 스코프체인이 여기서 파생
    - 클로져, this, 호이스팅 등은 프로토타입의 '맥락'을 표현하기 위한 것

## 호이스팅(Hoisting)
> 자바스크립트 호이스팅은 인터프리터가 코드를 실행하기 전에 함수, 변수, 클래스 또는 임포트 `선언문을 해당 범위에 맨 위로 끌어올리는 것처럼 보이는 현상`

```js
console.log(a); // Undefined
var a = 3;
```

코드는 위에서 아래로 읽어 내려감 그렇다면 위에 코드는 에러가 발생할거 같지만 JS 'Undefined'를 내보냄

>이걸 바로 `호이스팅`이라고 부른다.

```js
var a;
console.log(a); // Undefined
var a = 3;
```
마치 위에 코드처럼

하지만 let과 const로 값을 선언한다면 위에 코드처럼 성공하는게 아닌 오류가 발생함

```js
console.log(variable); // ReferenceError: Cannot access 'variable' before initialization
console.log(constance); // ReferenceError: Cannot access 'constance' before initialization

let variable = 3;
const constance = 3;
```

그러면 의문점이 든다.  let과 const는 var와는 다르게 호이스팅이 발생하지 않는건가?

- 호이스팅 끌어올리는 거라고 했는데? 왜 let과 const 안될까?
- `let`과 `const`는 정말로 호이스팅이 안된건가?

```js
// 1
console.log(a); // ReferenceError: a is not defined

// 2
console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 3;
```

상황 1번과 상황 2번은 둘다 에러코드가 나오지만 상황이다르고 에러 내용도 다르다
- 상황 1번은 변수가 선언 조차 되지 않았을때
- 상황 2번은 호출이 선언본다 먼저 되었을때

에러 내용을 보면
1번은 `a가 정의되지 않았다는 에러가 나옴` 2번은 `초기화전에 b에 접근할 수 없다`라고 얘기하고 있음

이것을 보면 만약 2번상황에 let b가 호이스팅이 되지 않았다면 1번 상황 처럼 에러가 나와야하지만 에러가 다르게 나오는걸 확인할 수 있다

변수할당 과정

![](https://velog.velcdn.com/images/sisofiy626/post/ff0e987d-b28c-4d60-a7d6-ed8fb82d875a/image.png)

- 선언: 번수가 scope에 등록되는 것
- 초기화 : 변수가 메모리에 적재되며 `Undefined`값을 가지게 된는 것
- 할당: 실제 값을 변수에 할당 하는것

따라서 변수 a는 선언자체가 되지 않는 상태, 변수 b는 아직 초기화가 되지 않은 상태라는 것

### TDZ(Temporal Dead Zon)
> 일시적으로 특정 변수에 접근을 할수 없는 구간을 말함

```js
// TDZ
console.log(temp); // ReferenceError: Cannot access 'temp' before initialization
// TDZ
let temp = 0;
{
    // TDZ
	console.log(temp); // ReferenceError: Cannot access 'temp' before initialization
    // TDZ
    let temp = 5;
}
```

위에 코드도 아까와 같은 상황으로 이해한다면

첫번째 콘솔로그 에러는 전역변수 temp에 값이 아직 초기화되지 않은 상태여서 나타는 현상
그렇다면 두번째 콘솔로그는 전역변수 temp에 접근한것이므로 에러가 나지 않아야 할거 같지만 에러가 발생함 이것은 두번째 콘솔로그는 지역변수 temp에 접근했다는 증거가 됨

따라서 let, const는 es2015+에 등장했던 것들이고 얘내들이 등장한 이유는
함수스코프 대신 블록스코프 사용함으로써 호스팅 문제를 해결

블로스코프를 사용함으로써 블록 밖에서는 변수에 접근을 할수 없다는걸 얘기하는 거고 호이스팅이 되어도 안된것처럼 보이는 이유

## 호이스팅 발생 이유 + **실행 컨텍스트**(Execution Context)
```js
console.log(a);
console.log(b);
console.log(c);

var a = 1;
let b = 2;
const c = 3;
```

자바스크립트는 프로토타입언어이므로 맥락이 중요 따라서 스크립트 엔진은 코드가 실행되기전에 원할하게 실행될 수 있도록 실행 환경을 만들어줌

그리고 그게 `실행 컨텍스트`

실행컨텍스트는 생성단게 + 실행단계로 구성
생성단계는 코드를 읽기전 실행단계는 실제코드를 읽는 단계를 말함

따라서 선언보다 호출이 먼저 있더라고 정의가되지 않았다는 에러가 아니라 `var`의 경우 `Undefined`, `let`과`const`는 초기화하기전에 사용할 수 없다는 에러가 출력되는것이며  
`var`은 이 실행컨텍스트에 등록될 때 초기화과정까지 진행되기 때문에 `Undefined`라는 값을 갖게 되는것

![스크린샷 2024-09-02 오후 11 40 04](https://github.com/user-attachments/assets/2e1806f5-fa33-423c-b21b-ec86f7b384d9)

## Reference & Additional Resources
- https://developer.mozilla.org/ko/docs/Glossary/Hoisting
- https://velog.io/@sisofiy626/JavaScript-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85%EC%9D%98-%EC%9B%90%EB%A6%AC%EC%99%80-TDZ%EA%B0%80-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94-%EC%9B%90%EC%9D%B8
		