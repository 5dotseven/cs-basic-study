## 실행 컨텍스트, 렉시컬 환경

---

**Execution Context**

1. 순서
    - 함수를 호출할 때 새롭게 생성되고 execution stack의 맨 위로 이동하게 된다.
    - 함수 실행 시 Global Execution Context가 가장 먼저 생성되며, 스택에 push 된다.
    - 다음으로 bar 함수가 호출되며 context를 생성하고 push한 뒤, bar 함수 내부의 foo 함수에 대해서도 동일한 과정이 진행된다.
    - 스택의 LIFO 순서대로 함수가 실행되고 제거된다.
2. 호출할 때 생성 단계, 실행 단계가 존재한다.
    1. 생성 단계: context는 만들어졌으나 실행, 깨어있지는 않은 단계
        - var로 선언된 변수들은 “Undefined”인 상태로 존재
        - this의 값이 결정됨
    2. 실행 단계: execution context를 깨우는, 실행하는 단계
        - undefined 상태였던 변수들의 값이 할당된다.

![images_paulkim_post_b4ec2560-fee8-40ee-9f89-476aae0d539a_1](https://github.com/user-attachments/assets/a168eab3-710e-40a9-b552-9a884126c5d7)
**Lexical Environment**

environment record와 lexical environment에 대한 reference로 구성

```jsx
var x = 10;

function foo(){
  var y = 20;
 console.log(x+y); // 30
}

// Environment 은 기술적으로 두가지 main components 를 갖고 있음: 
// **environmentRecord, 와 reference to the outer environment(부모)**

// Global Context 의 Environment 
globalEnvironment = {
  environmentRecord: {
    // 이미 만들어진 
    // 우리가 갖고 있는 묶여있는 값 
    x: 10
  },
  outer: null // 부모 Environment 가 없다 라는 의미 
};

// "foo" function의 Environment
fooEnvironment = {
  environmentRecord: {
    y: 20
  },
  outer: globalEnvironment
};

// 출처: https://velog.io/@paulkim/e
```

![images_paulkim_post_d087853a-6bb2-4ddf-b6ea-dc582605a540_2](https://github.com/user-attachments/assets/39ebe5d5-3f76-443b-8822-2c7b33786ee5)
함수가 선언되는 순간의 환경을 기억해두고 접근할 수 있게 해주는 것

- 함수가 속한 lexical environment를 저장하고 환경 밖에서 호출되어도 접근이 가능하게 해준다.
- 외부 함수의 실행이 끝나고 내부 함수가 외부 함수의 변수에 접근하는 경우

```jsx
function hello() {
	const a = 10;
	const b = 5;
	function add() {
		console.log(a+b);
	}	
	return add;
}

// hello 함수를 먼저 실행시킴, 외부 함수 종료
const func = hello();
// 외부 함수가 종료되었음에도 내부 함수 add에 접근이 가능
func(); // 15
```