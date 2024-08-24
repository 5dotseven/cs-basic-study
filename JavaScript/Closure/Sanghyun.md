## Closure

---

### 클로저의 활용

1. **상태유지**

현재 상태, 변경된 최신 상태를 유지할 때

```jsx
var box = document.querySelector('.box');
var toggleBtn = document.querySelector('.toggle');

var toggle = (function () {
  var isShow = false;

  // ① 클로저를 반환
  return function () {
    box.style.display = isShow ? 'block' : 'none';
    // ③ 상태 변경
    isShow = !isShow;
  };
})();

// ② 이벤트 프로퍼티에 클로저를 할당
toggleBtn.onclick = toggle;

//출처: https://poiemaweb.com/js-closure
```

1. **정보 은닉, private 구현**

```jsx
var incleaseBtn = document.getElementById('inclease');
var count = document.getElementById('count');

var increase = (function () {
  // 카운트 상태를 유지하기 위한 자유 변수
  var counter = 0;
  // 클로저를 반환
  return function () {
    return ++counter;
  };
}());

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};

----------------------------------------------------------------

function makeCounter() {
    this.publicCounter = 0;
}

makeCounter.prototype = {
    changeBy : function(val) {
        this.publicCounter += val;
    },
    increment : function() {
        this.changeBy(1);
    },
    decrement : function() {
        this.changeBy(-1);
    },
    value : function() {
        return this.publicCounter;
    }
}
var counter1 = new makeCounter();
var counter2 = new makeCounter();

alert(counter1.value());  // 0.

counter1.increment();
counter1.increment();
alert(counter1.value()); // 2.

counter1.decrement();
alert(counter1.value()); // 1.
alert(counter2.value()); // 0.
```

**자주 발생하는 실수**

```jsx
var arr = [];

for (var i = 0; i < 5; i++) {
  arr[i] = function () {
    return i;
  };
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}

// 0 1 2 3 4 를 반환 X
// for loop에 사용된 i는 전역 변수이기 때문
----------------------------------------------
var arr = [];

for (var i = 0; i < 5; i++){
  arr[i] = (function (id) { // ②
    return function () {
      return id; // ③
    };
  }(i)); // ①
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}

// function(id)는 즉시실행함수, i를 전달받고 이를 id에 저장 후 내부 함수를 반환
// 배열에는 즉시실행함수의 결과인 내부함수가 담겨져 있으며
// arr에 할당된 함수 id를 반환, id는 상위 스코프의 자유변수이므로 값이 유지된다.
```