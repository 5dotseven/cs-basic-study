# JAVA - Casting
자바에서 캐스팅(Casting)은 객체의 타입을 변환하는 과정임. 캐스팅은 업캐스팅(Upcasting)과 다운캐스팅(Downcasting)으로 나뉨.

### 업캐스팅 (Upcasting)
업캐스팅은 하위 클래스의 객체를 상위 클래스 타입으로 변환하는 것을 의미함. 자바에서 모든 클래스는 `Object` 클래스를 상속받기 때문에 모든 객체는 `Object` 타입으로 업캐스팅될 수 있음.

업캐스팅의 예:
```java
class Animal {     
	public void makeSound() {         
		System.out.println("Animal sound");     
	} 
}

class Dog extends Animal {     
	public void makeSound() {         
		System.out.println("Bark");     
	}
}  

public class Main {     
	public static void main(String[] args) {         
		Dog dog = new Dog();         
		Animal animal = dog; // 업캐스팅         
		animal.makeSound(); // "Bark" 출력     
	} 
}
```
위 예제에서 `Dog` 객체 `dog`는 `Animal` 타입 변수 `animal`에 할당됨. `animal` 변수는 `Dog` 객체를 참조하지만, 타입은 `Animal`임. 업캐스팅은 명시적인 타입 변환 없이도 자동으로 이루어짐.

### 다운캐스팅 (Downcasting)
다운캐스팅은 상위 클래스 타입을 하위 클래스 타입으로 변환하는 것을 의미함. 이는 업캐스팅과 달리 명시적으로 타입을 변환해야 하며, 클래스가 실제로 그 타입인지 확인해야 함. 그렇지 않으면 `ClassCastException`이 발생할 수 있음.

```java
class Animal {
    public void makeSound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    public void makeSound() {
        System.out.println("Bark");
    }

    public void play() {
        System.out.println("Dog is playing");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog(); // 업캐스팅
        Dog dog = (Dog) animal; // 다운캐스팅
        dog.makeSound(); // "Bark" 출력
        dog.play(); // "Dog is playing" 출력
    }
}
```
위 예제에서 `Animal` 타입 변수 `animal`은 `Dog` 객체를 참조하고 있음. 이 변수를 `Dog` 타입으로 다운캐스팅하여 `Dog` 클래스의 메서드 `play()`를 호출할 수 있음. 다운캐스팅을 할 때는 명시적으로 `(Dog)`와 같은 형식으로 타입을 변환해야 함.


다운캐스팅을 안전하게 수행하기 위해 `instanceof` 연산자를 사용할 수 있음:
```java
if (animal instanceof Dog) {
    Dog dog = (Dog) animal;
    dog.play();
}
```


이렇게 하면 `animal`이 실제로 `Dog` 객체인지 확인할 수 있어 `ClassCastException`을 피할 수 있음.

요약하면
- **업캐스팅**은 하위 클래스 객체를 상위 클래스 타입으로 변환하는 것이며, 자동으로 이루어짐.
- **다운캐스팅**은 상위 클래스 타입을 하위 클래스 타입으로 변환하는 것이며, 명시적으로 타입을 변환해야 하고 `instanceof` 연산자를 사용하여 안전하게 수행할 수 있음