# 힙

## **힙이란?**

: 완전 이진 트리 형태이며, 우선순위 큐를 위해서 만들어진 자료구조

**완전 이진 트리(complete binary tree)**

: 이진 트리 종류 중 하나로, 마지막 level을 제외하고 모든 level이 **두개의** 자식 노드로 완전히 채워져 있는 이진 트리

**사용할 때**

: 우선순위 큐와 같이 **최대/최소값을 빠르게 찾아야하는 자료구조 및 알고리즘 구현에 사용**된다. (시간 복잡도 **O(N)** ⇒ **O(log N)**)

## **힙의 종류**

**1. 최대 힙** : 부모노드가 자식노드 보다 크거나 같다 (루트가 최대값)

**2. 최소 힙** : 부모노드가 자식노드 보다 작거나 같다 (루트가 최소값)

![image](https://blog.kakaocdn.net/dn/wmK5g/btrVnZd2shQ/VGLRoBQ8nH1zDftBgR7K50/img.png)

## **힙 VS 이진 탐색 트리**

**공통점 :** 이진 트리 구조

**차이점**

- 이진 탐색 트리:
    - 노드 값이 오른쪽 자식 노드 > 부모 노드 > 왼쪽 자식 노드 순
- 힙
    - 각 노드의 값이 자식노드보다 크거나 같다 또는 작거나 같다.
    - 완전 이진 트리이므로, 무조건 왼쪽 자식 노드부터 데이터를 삽입한다.
    - 데이터의 중복이 허용된다.

이진 탐색 트리는 탐색에 유리하고

힙은 최대/최소값에 접근할 때 유리하다.

## **힙의 시간복잡도**

데이터 삽입과 삭제의 연산 자체는 O(1)의 시간 복잡도이지만,

최대 또는 최소힙의 조건을 유지하는 과정이 필요하므로. **O(logN)**의 시간 복잡도를 가진다.