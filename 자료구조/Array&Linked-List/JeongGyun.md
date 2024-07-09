# 자료구조 - Array & LinkedList
## Array (배열)
: 배열은 동일한 타입의 요소들이 연속적으로 저장된 자료구조. 배얄의 크기는 고정되어 있으며, 각 요소는 인덱스를 통해 접은할 수 있음
#### 장점
1. **빠른 접근 속도**: 인덱스를 사용하여 O(1) 시간 복잡도로 요소에 접근할 수 있음.
2. **메모리 효율성**: 요소들이 연속된 메모리 공간에 저장되므로 메모리 사용이 효율적임.
#### 단점
1. **고정 크기**: 배열의 크기는 선언 시에 정해지며, 나중에 변경할 수 없음.
2. **비효율적인 삽입 및 삭제**: 요소를 삽입하거나 삭제할 때, 모든 후속 요소들을 이동시켜야 하므로 O(n) 시간이 걸림.

```java
class ArrayDemo {
    public static void main(String[] args) {
        // declares an array of integers
        int[] anArray;

        // allocates memory for 10 integers
        anArray = new int[10];
           
        // initialize first element
        anArray[0] = 100;
        // initialize second element
        anArray[1] = 200;
        // and so forth
        anArray[2] = 300;
        anArray[3] = 400;
        anArray[4] = 500;
        anArray[5] = 600;
        anArray[6] = 700;
        anArray[7] = 800;
        anArray[8] = 900;
        anArray[9] = 1000;

        System.out.println("Element at index 0: "
                           + anArray[0]);
        System.out.println("Element at index 1: "
                           + anArray[1]);
        System.out.println("Element at index 2: "
                           + anArray[2]);
        System.out.println("Element at index 3: "
                           + anArray[3]);
        System.out.println("Element at index 4: "
                           + anArray[4]);
        System.out.println("Element at index 5: "
                           + anArray[5]);
        System.out.println("Element at index 6: "
                           + anArray[6]);
        System.out.println("Element at index 7: "
                           + anArray[7]);
        System.out.println("Element at index 8: "
                           + anArray[8]);
        System.out.println("Element at index 9: "
                           + anArray[9]);
    }
} 

The output from this program is:

Element at index 0: 100
Element at index 1: 200
Element at index 2: 300
Element at index 3: 400
Element at index 4: 500
Element at index 5: 600
Element at index 6: 700
Element at index 7: 800
Element at index 8: 900
Element at index 9: 1000
```

## 연결 리스트 (Linked List)
: 연결 리스트는 각 요소가 데이터와 다음 요소를 가리키는 포인터를 포함하는 노드로 이루어진 자료구조임. 연결 리스트의 크기는 동적으로 조정될 수 있음.
#### 장점
1. **동적 크기 조정**: 연결 리스트의 크기는 필요에 따라 조정될 수 있어 메모리 낭비를 피할 수 있음.
2. **효율적인 삽입 및 삭제**: 특정 위치에 요소를 삽입하거나 삭제할 때 포인터만 업데이트하면 되므로 O(1) 또는 O(n) 시간이 걸림(위치에 따라 다름).
#### 단점
1. **느린 접근 속도**: 요소에 접근하려면 처음부터 리스트를 순회해야 하므로 O(n) 시간이 걸림.
2. **높은 메모리 사용**: 각 노드가 데이터와 포인터를 저장해야 하므로 배열에 비해 메모리 사용량이 많음.
### 요약
- **배열**은 인덱스를 통해 빠르게 접근할 수 있지만, 크기가 고정되어 있고 삽입 및 삭제가 비효율적임.
- **연결 리스트**는 동적으로 크기를 조정할 수 있고 삽입 및 삭제가 효율적이지만, 접근 속도가 느리고 메모리 사용량이 많음.


참고: https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html



