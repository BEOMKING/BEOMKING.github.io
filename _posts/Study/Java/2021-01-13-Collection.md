---
title:  "Collection & Map"
excerpt: 자바 자료구조
categories: 
- Java
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-01-31
last_modified_at: 2021-07-01 03:29
---
## Collection
Java에서 컬렉션(Collection)이란 객체나 데이터들을 효율적으로 관리할 수 있는 자료구조
JCF(Java Collections Framework)는 이러한 자료구조들의 라이브러리를 의미

- Java 콜렉션 프레임워크의 상속구조
![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/collection.png)

- Array(배열)

- List (순서, 중복이 존재)
  - LinkedList
    - 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰임
  - Stack
  - ArrayList
    - 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어남
  
- Set (순서, 중복이 존재 X)
  - HashSet
    - 가장빠른 임의 접근 속도
    - 순서를 예측할 수 없음 
    - 중복 여부를 체크할 때 List 계열 보다 엄격
    - hashCode()를 이용하여 그 값이 같은 경우 같은 객체로 인식
    - 객체가 다르면 다른 hashCode()를 리턴하도록 오버라이딩해서 해결
  
```java
public class HashSet{
    private Set<classname> name;
}

public class anyway{
    @Override
    public int hashCode() {
        int hash = 7;
        hash = 31 * hash + this.getName().hashCode();
        hash = 31 * hash + this.getPhone().hashCode();
        return hash;
    }
}

public class Main{
    public static void main(String[] args) {
        Set<classname> name = new HashSet<classname>();
    }
}
```

  - TreeSet
    - 정렬방법을 지정할 수 있음
    
- Queue (순서, 중복이 존재)
  - LinkedList
  - PriorityQueue
    PriorityQueue를 통해 객체를 정렬할 때, 그 객체는 Comparable Interface를 구현해야함
    Comparable Interface에는 compareTo()라는 우선순위 메소드가 존재
    
- Map (Key, Value 쌍 순서 존재 X, Key는 중복 존재, Value는 중복 존재 X)
  - HashTable
    - HashMap보다는 느리지만 동기화 지원
    - null불가
  - HashMap
    - 중복과 순서가 허용되지 않으며 null값이 올 수 있다.
  - TreeMap
    - 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠름

- 관련 method
  - boolean add(E e)
  - boolean remove(Object o)
  - boolean isEmpty()
  - void clear()
  - int size
  - boolean contains(Object o)
  - Object[] toArray()
  - Iterator<E> iterator()
 
Class의 객체를 정렬하는 두 가지 방법

1. 정렬될 객체의 class가 comparable interface를 구현하는 방법
2. collection.sort()를 호출하면서 comparator interface로 정렬 기준을 동적으로 제공하는 방법
Collection.sort(first, second)
첫 번째 파라미터는 대상 Collection
두 번째 파라미터는 Comparator Interface 구현체, 이전 Comparable처럼 비교해서 처리하는 compare() 존재
Anonymous Class
lambda
   
## 출력
```java
for(int i = 0; i < result.size(); i++) { // 통과한 동아리의 개인별 능력치 출력
    System.out.print(result.get(i) + " ");
}

for(int a : result) {
    System.out.print(a + " ");
}

Iterator<Integer> re = result.iterator();
while(re.hasNext()){
    System.out.print(re.next() + " ");
}
```