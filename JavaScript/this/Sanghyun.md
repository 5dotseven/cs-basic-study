## **javascript this**

---

```jsx
this;

var global = 'Global';
console.log(this.global);
// window.global

function temp() {
	console.log(this);
}

temp();
// Window
```

this는 기본적으로 window, 함수 내에서 this를 선언하면 해당 this는 window 객체를 가리킨다.

<aside>
💡 **Window 객체**

- 브라우저 안의 모든 요소들이 소속된 객체, 어디서든 접근이 가능해 전역객체라고도 부른다.
- 브라우저의 창을 의미하며 이를 제어하는 다양한 메서드를 제공
</aside>

**객체 메서드의 경우**

- obj 안의 객체 메서드 func의 this는 obj를 가리킨다.
- 내부적으로 this가 바뀌었기 때문
- temp의 경우 obj메서드가 아닌 변수에 담긴 일반 함수가 되기 때문에 window가 찍힌다.

```jsx
var obj = {
	func: function() {
		console.log(this);
	},
};

obj.func();
// obj

var temp = obj.func;
temp();
// Window
```

자바와 다르게 자바스크립트 this에 바인딩되는 객체는 한 가지 고정이 아니며 함수 호출 방식에 따라 바인딩되는 객체가 달라진다

**함수 호출 방식**

1. **함수 호출**
- 기본적으로 this는 전역객체에 바인딩

```jsx
function foo() {
  console.log("foo's this: ",  this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

- 객체 메소드의 내부함수, 콜백함수의 경우에도 this는 전역객체에 바인딩된다.

```jsx
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() { /* 내부함수 */
      console.log("bar's this: ",  this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    bar();
  }
};

obj.foo();

var obj2 = {
  value: 100,
  foo: function() {
    setTimeout(function() {  /* 콜백함수 */
      console.log("callback's this: ",  this);  // window
      console.log("callback's this.value: ",  this.value); // 1
    }, 100);
  }
};

obj2.foo();
```

이러한 경우를 회피하기 위해서는

- var that = this;

```jsx
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    var that = this;  // Workaround : this === obj

    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() {
      //console.log("bar's this: ",  this); // window
     // console.log("bar's this.value: ", this.value); // 1

      console.log("bar's that: ",  that); // obj
      console.log("bar's that.value: ", that.value); // 100
    }
    bar();
  }
};
```

- call, bind, apply로 this를 설정하거나
- 화살표 함수 =>를 사용할 수 있다.

1. **메소드 호출**
- 함수가 객체의 프로퍼티 값일 경우 메소드로서 호출되며 this는 호출한 객체에 바인딩

```jsx
var name1 = {
	name: "James",
	introduce: function() {
		console.log(this.name);
	}
}

var name2 = {
	name: "John"
}

name2.introduce = name1.introduce;

name1.introduce(); // James
name2.introduce(); // John
```

1. **생성자 함수 호출**
- 자바스크립트는 함수에 new를 붙이면 생성자 함수로 동작하게 된다.
- 일반 함수에도 적용되기 때문에 생성자 함수와 일반 함수의 구분이 필요

```jsx
function Animal(species) {
	this.species = species;
}

var newAnimal = new Animal("Dog");
console.log(newAnimal); // Dog
```

1. **콜백 호출**
- javascript는 call by value, userdata.setName을 넘겨줄 때 함수는 복사되어 넘어가게 된다.
- 복사된 함수의 전역객체는 window이기 때문에 원하는 결과가 나오지 않는다.
- callback.call을 통해 해결이 가능

```jsx
let userData = {
    signUp: '2020-10-06 15:00:00',
    id: 'minidoo',
    name: 'Not Set',
    setName: function(firstName, lastName) {
        this.name = firstName + ' ' + lastName;
    }
}

function getUserName(firstName, lastName, callback) {
    callback(firstName, lastName);
}

getUserName('PARK', 'MINIDDO', userData.setName);

console.log('1: ', userData.name); // Not Set
console.log('2: ', window.name); // PARK MINIDDO

-----------------------------------------------------
function getUserName(firstName, lastName, callback) {
    callback.call(userData, firstName, lastName);
}

getUserName('PARK', 'MINIDDO', userData.setName);

console.log('1: ', userData.name); // PARK MINIDDO
console.log('2: ', window.name); // 빈칸. callback.call로 this가 가리키던 window를 대체
```

1. **apply/call/bind 호출**
- 콜백함수 내부의 this는 window를 가리킨다.
- 콜백함수의 this를 호출하는 함수 내부의 this와 일치 시켜주는 과정이 필요하다

```jsx
var name = "window name";

function Person(name) {
  this.name = name;
}

Person.prototype.doSomething = function(callback) {
  if(typeof callback == 'function') {
	  // callback();
    callback.bind(this);
  }
};

function foo() {
  console.log(this.name);

var p = new Person('Lee');
p.doSomething(foo);
```

출처:

[https://inpa.tistory.com/entry/JS-📚-this-총정리#5._콜백_함수_호출](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-this-%EC%B4%9D%EC%A0%95%EB%A6%AC#5._%EC%BD%9C%EB%B0%B1_%ED%95%A8%EC%88%98_%ED%98%B8%EC%B6%9C)