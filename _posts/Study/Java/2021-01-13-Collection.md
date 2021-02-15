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
last_modified_at: 2021-01-31 22:59
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

//        for(int i = 0; i < result.size(); i++) { // 통과한 동아리의 개인별 능력치 출력
//            System.out.print(result.get(i) + " ");
//
//        }
//        for(int a : result) {
//            System.out.print(a + " ");
//        }
//        Iterator<Integer> re = result.iterator();
//        while(re.hasNext()){
//            System.out.print(re.next() + " ");
//        }

암호문1
I 1 5 400905 139831 966064 336948 119288
I 8 6 436704 702451 762737 557561 810021 771706
I 3 8 389953 706628 552108 238749 661021 498160 493414 377808
for (int tal = 0; tal < talk; tal++) { // 명령어 개수만큼 반복
String I = st2.nextToken(); // 처음 입력 문자 제거
int start = Integer.parseInt(st2.nextToken()); // 시작
int end = Integer.parseInt(st2.nextToken()); // 끝
for (int i = start; i < start + end; i++) { // 시작점에서 한칸씩 증가하면서 삽입
List.add(i, Integer.parseInt(st2.nextToken()));
}
}
String Bulider,
위에서 보는바와 같이 생성된 클래스의 주소값이 다른 것을 볼 수 있다. String은 새로운 값을 할당할 때마다 새로 생성되기 때문이다. 이와 달리 StringBuffer는 값은 memory에 append하는 방식으로 클래스를 직접생성하지 않는다. 논리적으로 따져보면 클래스가 생성될 때 method들과 variable도 같이 생성되는데, StringBuffer는 이런 시간을 사용하지 않는다.

또한 지금은 한 번만 생성되었지만 수십번 String이 더해지는 경우에는 각 String의 주소값이 stack에 쌓이고 클래스들은 Garbage Collector가 호출되기 전까지 heap에 지속적으로 쌓이게 된다. 메모리 관리적인 측면에서는 치명적이라고 볼 수 있다.
그럼 String class의 내부는 어떤 구조로 되어 있기에 새로 생성될까.

아래 이미지를 보면 value[]라는 char형의 배열이 보인다. 여기서 힌트를 찾을 수 있다. private final char형이라는 것을 눈여겨 보아야 한다.

String에서 저장되는 문자열은 알고보면 char의 배열형태로 저장되며 이 값들은 외부에서 접근할 수 없도록 private으로 보호된다. 또한 final형이기 때문에 초기값으로 주어진 String의 값은 불변으로 바뀔 수가 없게 되는 것이다.
String의 특징을 알아봤으니 memory에 값을 append하는 StringBuilder와 StringBuffer에 대해서 알아보자. api는 아래와 같다.

해석해보면 StringBuilder는 변경가능한 문자열이지만 synchronization이 적용되지 않았다. 하지만 StringBuffer는 thread-safe라는 말에서처럼 변경가능하지만 multiple thread환경에서 안전한 클래스라고 한다. 이것이 StringBuilder와 StringBuffer의 가장 큰 차이점이다.
StringBuilder와 StringBuffer를 테스트 해보자. 아래의 결과를 보면 다른 값이 나온 것을 볼 수 있다. StringBuilder의 값이 더 작은 것을 볼 수 있는데 이는 쓰레드들이 동시에 StringBuilder클래스에 접근할 수 있기 때문에 일어난 결과다. 이와 달리 StringBuffer는 multi thread환경에서 다른 값을 변경하지 못하도록 하므로 web이나 소켓환경과 같이 비동기로 동작하는 경우가 많을 때는 StringBuffer를 사용하는 것이 안전할 것이다.

bufferedwriter

package 구현;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class 백준_20299_3대측정 {
public static void main(String[] args) throws Exception{
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
StringTokenizer st = new StringTokenizer(br.readLine());
StringBuilder sb = new StringBuilder();

        int N = Integer.parseInt(st.nextToken()); // 신청한 동아리 수
        int S = Integer.parseInt(st.nextToken()); // 팀원 능력치 합 제한
        int M = Integer.parseInt(st.nextToken()); // 팀원 개인 능력치 제한
        int count = 0;

        for(int n = 0; n < N; n++){ // 신청한 팀 순서대로
            StringTokenizer st2 = new StringTokenizer(br.readLine());
            int x1 = Integer.parseInt(st2.nextToken()); // 팀원 1
            int x2 = Integer.parseInt(st2.nextToken()); // 팀원 2
            int x3 = Integer.parseInt(st2.nextToken()); // 팀원 3

            int s = x1 + x2 + x3; // 팀원 능력치 합
            if(s >= S && x1 >= M && x2 >= M && x3 >= M) {
                count += 1;
                sb.append(x1 + " "); // StringBuilder로 답을 저장시키면서 속도의 이득을 가져온다.
                sb.append(x2 + " ");
                sb.append(x3 + " ");
            }
        }
        System.out.println(count); // 통과한 동아리 수
        bw.write(sb.toString()); // BufferedWriter로 한번에 정답을 출력함으로 for문을 사용할 때보다 빠르게 출력할 수 있다.
        bw.flush();
        bw.close();
    }
}

