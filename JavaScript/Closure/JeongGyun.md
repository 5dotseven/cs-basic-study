# JS - 클로져
## 클로저 함수란 ?
클로저 함수(Closure function)란, 자신이 생성될 때의 환경을 기억하고 이 환경에서 생성될 때의 데이터를 계속 유지하는 함수를 말함

이는 자바스크립트에서 함수가 일급 객체(first-class object)이기 때문에 가능

클로저 함수는 외부 함수 안에 내부 함수를 정의하고, 내부 함수가 외부 함수의 지역 변수를 참조하거나 외부 함수의 매개변수를 활용하는 경우에 생성

이때 외부 함수가 실행을 마치고 반환되어도, 내부 함수가 이용하는 외부 함수의 지역 변수나 매개변수는 메모리에 유지됨

클로저 함수를 사용하면, 내부 함수에서 외부 함수의 지역 변수에 접근하여 이용할 수 있으므로, 정보의 은닉, 캡슐화, 함수 팩토리 등의 다양한 디자인 패턴을 구현할 수 있슴

### 클로저 함수의 장점
1. 데이터 및 함수를 캡슐화하여 코드를 이해하기 쉽게 만들고 이름 충돌 가능성을 줄여줌
2. 성능을 개선하고 반복 계산의 필요성을 줄일 수 있는 영구 데이터 및 상태를 만듬
3. 재사용 가능한 코드를 만드는 데 유용할 수 있는 함수를 생성함

### 클로저 함수의 단점
1. 적절하게 관리되지 않으면 메모리 누수를 일으킬 수 있으며, 이는 애플리케이션의 성능을 저하시킬 수 있슴
2. 클로저 함수는 코드를 읽고 이해하기 어렵게 만들어 버그와 오류를 유발할 수 있슴
3. 경우에 따라 클로저 함수는 성능에 부정적인 영향을 미칠 수 있지만 이는 일반적으로 성능에 매우 민감한 애플리케이션에서만 문제가 됨

> 전반적으로 클로저 함수는 코드 구성을 개선하고 중복성을 줄이며 효율적이고 재사용 가능한 코드를 더 쉽게 작성할 수 있다는 매우 유용한 점이  있지만 잠재적인 단점을 인식하고 신중하게 사용하는 것이 중요

## 예시코드
```javascript
function outerFunction() {
  let outerVariable = 10;
  
  function innerFunction() {
    return outerVariable;
  }
  
  return innerFunction;
}

let closure = outerFunction(); // closure는 innerFunction을 참조
console.log(closure()); // 10
```

1. outerFunction 함수는 값이 10으로 설정된 로컬 변수 outerVariable로 정의
2. outerFunction 함수는 outerVariable의 값을 반환하는 innerFunction이라는 내부 함수도 정의
3. outerFunction 함수는 innerFunction 함수를 반환
4. outerFunction 함수가 호출되고 반환 값(innerFunction)이 closure 변수에 할당
5. closure 변수는 클로저를 통해 outerVariable 변수에 액세스할 수 있는 innerFunction 함수에 대한 참조
6. closure 함수는 outerVariable의 값을 반환하는 closure() 구문을 사용하여 호출
7. outerVariable의 반환 값(10)은 console.log(closure())를 사용하여 콘솔에 출력

요약하면 코드는 외부 함수의 범위에 정의된 로컬 변수(outerVariable)의 값을 반환하는 내부 함수를 생성하는 클로저 함수를 정의한다. 클로저를 사용하면 내부 함수가 더 높은 범위에서 정의된 'outerVariable' 변수의 값에 액세스하고 값을 반환할 수 있음

또 다른 예를 보면

```javascript
function counter() {
  let count = 0;
  function increment() {
    count++;
    console.log(count);
  }
  function decrement() {
    count--;
    console.log(count);
  }
  return {
    increment,
    decrement
  };
}

const myCounter = counter();
myCounter.increment(); // 1
myCounter.increment(); // 2
myCounter.decrement(); // 1
```

1. 'counter' 함수는 0으로 설정된 로컬 변수 'count'로 정의
2. counter 함수는 또한 클로저를 통해 count 변수에 액세스할 수 있는 increment 및 decrement의 두 내부 함수를 정의
3. increment 함수는 count 변수를 1씩 증가시키고 console.log(count)를 사용하여 콘솔에 출력
4. decrement 함수는 count 변수를 1씩 감소시키고 console.log(count)를 사용하여 콘솔에 출력
5. 'counter' 함수는 해당 내부 함수로 설정된 '증가' 및 '감소'의 두 가지 속성을 가진 객체를 반환
6. counter 함수가 호출되고 결과 개체가 myCounter 변수에 할당
7. myCounter.increment() 메서드가 두 번 호출되어 count 변수를 매번 1씩 증가시키고 console.log(count)를 사용하여 새 값을 콘솔에 출력
8. myCounter.decrement() 메서드가 한 번 호출되어 count 변수를 1씩 줄이고 console.log(count)를 사용하여 새 값을 콘솔에 출력

요약하면 이 코드는 함수 호출 간에 유지되는 카운터 변수를 증가 및 감소시키는 데 사용할 수 있는 두 가지 메서드로 객체를 생성하는 클로저 함수를 정의함

클로저를 사용하면 increment 및 decrement 함수가 더 높은 범위에서 정의된 count 변수에 액세스하고 조작함

## Reference & Additional Resources
- https://growing-jiwoo.tistory.com/84