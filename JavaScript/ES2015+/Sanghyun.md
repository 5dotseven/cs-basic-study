## ES2015+

---

노드6 버전부터 사용할 수 있는 문법으로 많은 편의성 개선이 있었다.

1. Const, let

기존의 경우 var로 변수를 선언했던 것을 대체

var는 함수 스코프를 가지지만 const와 let은 블록 스코프를 가진다.

- 스코프: 함수가 실행될 때 변수에 대한 접근 가능 범위를 표현
    - 함수 스코프: 새로운 함수가 생성될 때마다 새로운 스코프가 발생

        ```jsx
        if(5 > 4) {
        	var secret = '12345';
        }
        secret // '12345'
        // 이 경우 함수 선언이 되어 있지않기 때문에 새로운 스코프가 형성되지 않음
        // 스코프가 형성되지 않기 때문에 동일한 컨텍스트 안에 존재, sercret에 어디에서나 접근이 가능
        
        function a() {
        	var secret = '12345';
        }
        secret; //ReferenceError
        // 반면 var 변수가 함수 안에 있기 때문에 함수가 실행되자마자 새로운 실행 컨텍스트가 생성
        // 컨텍스트 안에 변수환경이 생기고 secret 변수가 저장됨, 외부에서는 secret에 접근이 불가능
        ```

    - 블록 스코프: if, while, for, function 등에서 사용하는 중괄호( {} )에 속하는 스코프

const와 let의 주요 차이는 초기화 가능 여부, let은 대입한 값을 계속해서 수정할 수 있지만 const는 한 번 대입하면 다른 값을 대입할 수 없다

1. 템플릿 문자열

백틱(`)을 사용해 문자열을 생성할 수 있다. 백틱을 활용해 문자열 안에도 간편하게 변수를 넣어 작성이 가능해진다.

```jsx
var string = num1 +'+'+num2+'='+result;

const string = `${num1} + ${num2} = ${result}`
```

1. 객체 리터럴
- 객체의 메소드에 함수를 연결할 때 ‘:’ 콜론, function을 연결하지 않아도 가능
- 함수의 이름이 중복되는 변수의 경우 하나만 작성하면 된다.
- 객체의 속성명을 리터럴 안에서 생성할 수 있게 됨.

```jsx
var oldFunc = function() {
	console.log('Old Function');
}
var oldEs = 'ES';
var oldObj = {
	sayJs : function() {
		console.log('JS');
	},
	oldFunc: oldFunc
};
oldObj[oldEs+6] = 'Wow';
oldObj.oldFunc();	// Old Function
oldObj.sayJs();		// JS
console.log(oldObj.ES6);	// Wow

---------------------------------------

const newFunc = function() {
	console.log('New Function');
}
const newEs = 'ES';
const newObj = {
	sayJs() {			// sayJs 객체의 메서드에 함수 연결 시 콜론과 function를 붙이지 않음
		console.log('JS');
	},
	newFunc,			// newFunc: newFunc 처럼 속명명과 변수명이 동일하면 한 번만 써도 됨
	[newEs+6]: 'Wow'	// 객체의 속성명을 동적으로 생성
};
newObj.newFunc();	// New Function
newObj.sayJs();		// JS
console.log(newObj.ES6);	// Wow

// 출처: https://assu10.github.io/dev/2021/08/02/js-basic/
```

1. 화살표 함수
- 함수의 return문을 생략할 수 있는 장점

    ```jsx
    function addOld(a, b) {
    	return a+b;
    }
    
    const addNew = (a, b) => a+b;
    ```

- function과 스코프가 다르다
    - 바로 바깥 스코프의 this를 그대로 사용할 수 있음

    ```jsx
    var showFriendsOld = {
    	name = "elice",
    	friends = ['a', 'b', 'c'],
    	showFriends: function() {
    		var that = this;
    		this.friends.forEach(function(friend)) {
    			console.log(that.name, friend);
    		});
    	},
    };
    
    var showFriendsNew = {
    	name = "elice",
    	friends = ['a', 'b', 'c'],
    	showFriends: function() {
    		this.friends.forEach(function(friend)) {
    			console.log(that.name, friend);
    		});
    	},
    };
    ```


1. 비구조화 할당

객체, 배열에서 속성이나 요소를 꺼내올 때 사용

기존의 경우 한 줄 씩 가져오고, count처럼 여러 단계 안에 있는 속성도 한 줄로 조회가 가능해짐

```jsx
var gumMachine = {
	status: {
		name: 'node',
		count: 10,
	},
	getGum: function() {
		this.status.count--;		
		return this.status.count;
	}
};

var getGum = gumMachine.getGum;
var count = gumMachine.status.count;

-----------------------------------------

var gumMachine = {
	status: {
		name: 'node',
		count: 10,
	},
	getGum: function() {
		this.status.count--;		
		return this.status.count;
	}
};

const {getGum, status: {count}} = gumMachine;
```

1. 프로미스, promise

자바스크립트, 노드는 비동기 프로그래밍, 이벤트 주도 방식을 활용하면서 콜백 함수를 많이 사용

콜백 함수 자체의 어려움과 이해 문제를 개선하기 위해 프로미스 기반으로 재구성

- resolve 호출 시 .then이 실행, reject 호출 시 .catch가 실행됨

```jsx
const condition = true;

const promise = new Promise((resolve, reject) => {
	if(condition) {
		resolve('success');
	} else {
		reject('fail');
	}
});

promise
	.then((message) => {
		console.log(message);
	})
	.catch((error) => {
		console.log(error);
	});
```

1. async/await

ES2017에 추가된 최신기능, 프로미스를 조금 더 깔끔하게 사용할 수 있게 해준다.

- 콜백 지옥은 해결했지만 .then, .catch로 코드가 길어지는 것을 방지

```jsx
function findAndSaveUser(Users) {
    Users.findOne({})           // findOne(), save() 가 내부적으로 프로미스 객체를 가지고 있어야 함 (=new Promise 가 내부에 구현되어 있어야 함)
        .then((user) => {
            user.name = 'zero';
            return user.save();
        })
        .then((user) => {
            return Users.findOne( { gender: 'm'});
        })
        .then((user) => {
            // 생략
        })
        .catch((error) => {
            console.error(error);
        })
}

// 프로미스로 구성되어 있는 위 코드를 async/await 문법을 사용하여 바꿔보자.
async function findAndSaveUser(Users) {
    try {
        let user = await Users.findOne({});
        user.name = 'zero';
        user = await user.save();
        user = await User.findOne( { gender: 'm' });
        // 생략
    } catch (error) {
        console.error(error);
    }
}
```