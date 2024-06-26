# 캐시의 지역성

- **RAM**(주기억장치)에서 자주 사용하는 프로그램과 데이터를 미리 저장해 둔 고속 메모리 공간
- 속도가 빠른 장치(**CPU** 연산)와 느린 장치(**메모리** 접근) 간의 속도 차에 따른 **병목 현상**을 줄이기 위한 범용 메모리
- 메인 메모리와 CPU 사이에 위치
- 캐시를 사용하면 메모리에 접근하는 횟수가 줄어 컴퓨터 처리 속도 향상
- 캐시가 효율적으로 동작하기 위해서는 CPU가 참조할 정보에 대해 잘 예측해야 함
    - 캐시 적중률(Hit Rate) 극대화
    - 캐시의 지역성(Locality) 고려


❓ ※ **적중률** = 적중 횟수 / 총 접근 횟수
(적중률은 캐시 기억장치가 있는 컴퓨터의 성능을 나타내는 척도로 이용되며, 적중률이 0.95~0.99일 때 우수하다고 한다.)

</aside>

### 캐시의 지역성(locality)

- 데이터에 대한 접근이 시간적/공간적으로 가깝게 발생하는 것
- 기억장치 내의 정보를 균등하게 엑세스하는 것이 아닌 어느 한 순간에 특정 부분을 집중적으로 참조하는 특성
- 캐시 적중률(Hit Rate)을 위해 저장할 데이터는 지역성을 가져야 함

### 지역성의 종류

- **시간적 지역성**
    - 특정 데이터가 한번 참조된 경우, 가까운 미래에 또 한번 참조될 가능성이 높은 경우
    - 메모리 상의 같은 주소에 여러 차례 읽기 쓰기를 수행할 경우, 캐시를 사용해 효율성을 높임
- **공간적 지역성**
    - 특정 데이터와 인접한 주소의 데이터가 참조될 가능성이 높은 것
    - CPU 캐시나 디스크 캐시의 경우 한 메모리 주소에 접근할 때 그 주소뿐 아니라 해당 블록을 전부 캐시에 가져오도록 함
        - 메모리 주소를 오름차순이나 내림차순으로 순서대로 접근한다면 이미 캐시에 저장되어 있기 때문에 효율성이 높음

### 캐싱라인

- 캐시에서 원하는 데이터를 바로 접근하기 위해 특정 자료구조를 통해 묶음으로 캐싱하는 것
- 캐시에서 데이터를 바로 찾을 수 있어야 성능이 올라감
- 캐시 메모리의 매핑 프로세스

### 캐싱라인의 종류

- **직접 매핑(Direct mapping)**
    - 메모리 주소와 캐시의 순서를 일치시켜 지정된 캐싱 라인으로만 매핑
    - 메모리의 특정 블럭은 특정 캐싱 라인에만 저장
    - 구현이 간단하고 빠르지만, 충돌로 인한 잦은 스와핑이 발생할 가능성이 높아 성능이 제한될 수 있음
- **연관 매핑(Associative Mapping)**
    - 순서를 일치시키지 않고 필요한 메모리 값(데이터)을 캐시의 어디든지 저장
    - 찾는 과정이 복잡하고 느리지만 충돌이 적으며 필요한 캐시 위주로 저장하기에 적중률 높음
- **집합 연관 매핑(Set-Associative Mapping)**
    - 집합 간의 순서는 일치시키는 반면 집합 내에서는 순서를 일치시키지 않고 편하게 저장
    - 직접 매핑과 연관 매핑의 장점만 취함
