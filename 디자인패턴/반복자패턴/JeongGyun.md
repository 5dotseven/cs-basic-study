# 디자인패 - 반복자 패턴
## Iterator Pattern
반복자 패턴 -> `일련의 데이터 집합에 대하여 순차적인 접근을 지원하는 패턴`

데이터 집합 -> 자료의 구조를 취하는 걸렉션을 말함 ex)트리, 그래프, 리스트

![스크린샷 2024-07-07 오후 10 14 53](https://github.com/5dotseven/cs-basic-study/assets/144773042/e4f88cdb-bb6d-4aae-8a24-6a82ba67149f)
만약 데이터에 접근한다면 배열, 리스트는 for문을 통해 순회할거임 하지만 해시 트리는? 애매함

따라서
![스크린샷 2024-07-07 오후 10 15 25](https://github.com/5dotseven/cs-basic-study/assets/144773042/57df3ae9-c022-4107-b97f-115a2a953332)
이처럼 복잡하게 얽혀있는 자료 걸렉션을 순회하는 알고리즘 전략을 정의한것이 `이터레이터 패턴`

*자바의 컬렉션 프레임워크 또한 내부에서 이터레이터 패턴이 적용 된 것*

![스크린샷 2024-07-07 오후 10 15 43](https://github.com/5dotseven/cs-basic-study/assets/144773042/2ff093da-a705-401e-aa94-d264efa35b21)
## 흐름
```java
// 집합체 객체 (컬렉션)
interface Aggregate {
    Iterator iterator();
}

class ConcreteAggregate implements Aggregate {
    Object[] arr; // 데이터 집합 (컬렉션)
    int index = 0;

    public ConcreteAggregate(int size) {
        this.arr = new Object[size];
    }

    public void add(Object o) {
        if(index < arr.length) {
            arr[index] = o;
            index++;
        }
    }

    // 내부 컬렉션을 인자로 넣어 이터레이터 구현체를 클라이언트에 반환
    @Override
    public Iterator iterator() {
        return new ConcreteIterator(arr);
    }
}
```

```java
// 반복체 객체
interface Iterator {
    boolean hasNext();
    Object next();
}
class ConcreteIterator implements Iterator {
    Object[] arr;
    private int nextIndex = 0; // 커서 (for문의 i 변수 역할)
    // 생성자로 순회할 컬렉션을 받아 필드에 참조 시킴
    public ConcreteIterator(Object[] arr) {
        this.arr = arr;
    }
    // 순회할 다음 요소가 있는지 true / false
    @Override
    public boolean hasNext() {
        return nextIndex < arr.length;
    }
    // 다음 요소를 반환하고 커서를 증가시켜 다음 요소를 바라보도록 한다.
    @Override
    public Object next() {
        return arr[nextIndex++];
    }
}
```

```java
public static void main(String[] args) {
    // 1. 집합체 생성
    ConcreteAggregate aggregate = new ConcreteAggregate(5);
    aggregate.add(1);
    aggregate.add(2);
    aggregate.add(3);
    aggregate.add(4);
    aggregate.add(5);
    // 2. 집합체에서 이터레이터 객체 반환
    Iterator iter = aggregate.iterator();
    // 3. 이터레이터 내부 커서를 통해 순회
    while(iter.hasNext()) {
        System.out.printf("%s → ", iter.next());
    }
}
```
## 장단점
장점
- 일괄된 이터레이터 인터페이스를 사용해서 동일한 순회 방법을 제공
- 클래스의 책임을 분리하여 -> SRP 준수
- 컬렉션 종류가 변경되도 클라이언트 구현 코드는 손상 X -> OCP 준수

단점
- 복잡도가 증가한다
- 구현 잘못하면 -> 캡슐화 위배



출처: [https://inpa.tistory.com/entry/GOF-💠-반복자Iterator-패턴-완벽-마스터하기](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%B0%98%EB%B3%B5%EC%9E%90Iterator-%ED%8C%A8%ED%84%B4-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0) [Inpa Dev 👨‍💻:티스토리]
