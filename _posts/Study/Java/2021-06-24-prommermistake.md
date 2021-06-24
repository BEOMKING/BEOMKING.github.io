---
title:  "자바 프로그래머들이 자주하는 실수"
excerpt: 알면 좋은 지식들
categories: 
- Java
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-06-24
last_modified_at: 2021-06-24 21:37
---

### #1. 일반 배열을 ArrayList로 변환하기
보통 많은 개발자가 다음과 같이 일반 배열을 `ArrayList`로 변환한다:

```java
List<String> list = Arrays.asList(arr);
```

`Arrays.asList()`는 `Arrays`의 private 정적 클래스인 `ArrayList`를 리턴한다.
`java.util.ArrayList` 클래스와는 다른 클래스이다.
`java.util.Arrays.ArrayList` 클래스는 `set()`, `get()`, `contains()` 매서드를 가지고 있지만 원소를 추가하는 매서드는 가지고 있지 않기 때문에 사이즈를 바꿀 수 없다.
진짜 `ArrayList`를 받기 위해서는 다음과 같이 변환하면 된다:

```java
ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(arr));
```

`ArrayList`의 생성자는 `java.util.Arrays.ArrayList`의 상위(super) 클래스인 Collection type도 받아들일 수 있다.

### #2. 일반 배열에 특정 값이 들어있는지 확인하기
보통 이렇게 많이 확인한다:

```java
Set<String> set = new HashSet<String>(Arrays.asList(arr));
return set.contains(targetValue);
```

이 코드는 동작하지만 list를 set으로 변환하는 것은 시간도 더 걸릴뿐더러 사실 할 필요가 없다. 대신에 다음과 같이 처리할 수 있다:

```java
Arrays.asList(arr).contains(targetValue);

// OR

for(String s: arr){
	if(s.equals(targetValue))
		return true;
}
return false;
```

첫번째 솔루션이 훨씬 읽기 편하다.

### #3. Loop에서 list의 원소를 제거하기
다음과 같이 Loop 안에서 원소를 제거한다고 하자:

```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a", "b", "c", "d"));
for (int i = 0; i < list.size(); i++) {
	list.remove(i);
}
System.out.println(list);

// output
// [b, d]
```

위의 코드에는 아주 심각한 문제가 있다.
원소가 삭제될 때, list의 사이즈가 줄어들면서 다른 원소들의 index도 바뀌어 버린다.
그래서 만약 loop 내에서 다수의 원소를 index를 사용해 삭제한다면 생각한대로 동작하지 않을 것이다.

아마 반복자(iterator)를 사용하는 것이 바른 방법이고, foreach loop가 내부적으로 반복자를 사용한다는 것을 알고 있을지도 모른다.
하지만 사실 다음의 foreach loop에서도 올바르게 동작하지 않는다.

```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a", "b", "c", "d"));
 
for (String s : list) {
	if (s.equals("a"))
		list.remove(s);
}
```

위의 코드는 `ConcurrentModificationException`을 발생시킬 것이다.

다음의 코드는 제대로 동작한다:

```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a", "b", "c", "d"));
Iterator<String> iter = list.iterator();
while (iter.hasNext()) {
	String s = iter.next();
 
	if (s.equals("a")) {
		iter.remove();
	}
}
```

반드시 `.remove()`전에 `.next()`가 호출되어야 한다.
만약 foreach loop안에서 원소가 삭제된 뒤에 `.next()`가 호출된다면 컴파일러는 `ConcurrentModificationException`을 발생시킬 것이다.
`ArrayList.iterator()`의 코드가 깊이 이해하는 데 도움이 될 것이다.

### #4. Hashtable vs HashMap
알고리즘적으로 봤을 때 Hashtable은 자료구조 이름이지만 Java에서의 이름은 사실 `HashMap`이다.
`Hashtable`이 `HashMap`과 가장 다른 점은 바로 동기화(synchronized)라는 것이다.
그래서 대부분 `Hashtable`보다는 `HashMap`을 사용하는 것이 좋다.

- [HashMap vs. TreeMap vs. Hashtable vs. LinkedHashMap](http://www.programcreek.com/2013/03/hashmap-vs-treemap-vs-hashtable-vs-linkedhashmap/)
- [Top 10 questions about Map](http://www.programcreek.com/2013/09/top-9-questions-for-java-map/)

### #5. Collection의 Raw Type 사용
Java에서는, _raw type_과 _unbounded wildcard type_이 쉽게 섞여서 함께 사용된다.
Set을 예로 들어보면, `Set`은 raw type이고 `Set<?>`은 unbounded wildcard type이다.

다음과 같은 raw type `List`를 파라미터로 사용하는 코드가 있다고 하자:

```java
public static void add(List list, Object o){
	list.add(o);
}
public static void main(String[] args){
	List<String> list = new ArrayList<String>();
	add(list, 10);
	String s = list.get(0);
}

// Exception will be thrown..
// Exception in thread "main" java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String
// 	at ...
```

raw type collection을 사용하는 것은 타입 체크를 건너뛰기 때문에 안전하지 않다.
`Set`, `Set<?>`, `Set<Object>` 사이에는 아주 큰 차이가 있다. 다음 글들을 읽는 걸 추천한다.

- [Raw type vs. Unbounded wildcard](http://www.programcreek.com/2013/12/raw-type-set-vs-unbounded-wildcard-set/)
- [Type Erasure](http://www.programcreek.com/2011/12/java-type-erasure-mechanism-example/)

### #6. 접근 레벨
개발자들은 꽤 자주 public 클래스 필드를 사용한다.
외부에서 아주 간단하게 필드 값에 접근을 할 수 있지만, 이건 아주 안 좋은 디자인이다.
제대로 된 디자인은 각 멤버들에게 가능한한 낮은 접근 레벨을 주는 것이다.

[public, default, protected, and protected](http://www.programcreek.com/2011/11/java-access-level-public-protected-private/)

<!-- more -->
### #7. ArrayList vs. Linked List
`ArrayList`와 `LinkedList`의 차이를 모를 때 종종 그냥 더 익숙해 보이는 `ArrayList`를 사용하곤 한다.
하지만, 이 선택은 아주 큰 성능 차이를 불러온다.
간단히 말해서, `LinkedList`는 임의 접근(Random Access)이 별로 없고 값의 추가/삭제가 많을 때 사용하는 것이 적당하다.
이 자세한 내용은 [ArrayList vs. LinkedList](http://www.programcreek.com/2013/03/arraylist-vs-linkedlist-vs-vector/)에서 알 수 있다.

### #8. Mutable vs. Immutable
Immutable 객체는 심플함, 안전성 등에서 많은 장점을 가지고 있다.
하지만 각각 다른 값을 위해 새로운 객체를 생성해야 하고, 그 때문에 가비지컬렉션(garbage collection)에 부하를 줄 가능성이 있다.
mutable과 immutable중에서 선택할 때는 필요해 따라 잘 선택해야 한다.

일반적으로, mutable 객체는 하나의 객체를 만들기 위해 값을 많이 바꿀 필요가 있을 경우에 사용한다.
예를 들어, 많은 문자열(String)을 이어붙여야 할 경우, 만약 immutable 문자열을 사용한다면 매번 문자열을 이을 때마다 가비지컬렉터로 처리되어야 할 필요없는 객체가 생성 될 것이다.
이것은 쓸데 없이 CPU의 시간과 에너지를 소비시킨다. 이런 곳에서는 mutable 객체를 사용하는 것이 바른 해결책이다 (예: `StringBuilder`)

```java
String result="";
for(String s: arr){
	result = result + s;
}
```

mutable 객체를 사용하는 것이 필요한 상황들이 있다.
예를 들면, mutable 객체들을 매서드에 매개변수로 넘겨주면 고생할 필요없이 한 번에 다수의 결과를 돌려받을 수 있다.
또 다른 예로는, 정렬과 필터링이다.
당연히 일반적인 collection을 받고 정렬된 새로운 collection을 리턴하도록 메서드를 만들 수도 있지만, 아주 큰 collection이라면 아주 낭비일 것이다.
(Stack Overflow [dasblinkenlight의 답변](http://stackoverflow.com/questions/23616211/why-we-need-mutable-classes))

[Why String is Immutable?](http://www.programcreek.com/2013/04/why-string-is-immutable-in-java/)

### #9. Super와 Sub의 생성자(Constructor)

```java
class Super {
  String s;
  
  public Super(String s) {
    this.s = s;
  }
}

public class Sub extends Super {
  int x = 200;
  public Sub(String s) { // error here
    
  }
  
  public Sub() { // error here
    System.out.println("Sub");
  }
  
  public static void main(String[] args) {
    Sub s = new Sub();
  }
}
```

위의 컴파일 에러는 Super의 기본 생성자가 정의되어 있지 않기 때문이다.
자바에서는, 생성자가 없는 클래스가 있다면 컴파일러가 자동으로 아무런 매개변수를 받지않는 디폴트 생성자를 생성해준다.
만약 어떤 생성자든 Super 클래스에 정의되어 있다면 -- 지금의 경우 Super(String s) -- 디폴트 생성자를 자동으로 만들어 주지 않는다. 
이것이 바로 위의 Super 클래스의 상황이다.

위의 두 Sub 클래스의 생성자는 Super 클래스의 디폴트 생성자를 호출할 것이다.
컴파일러는 Sub 클래스의 생성자 내부에 `super()`를 자동으로 추가하지만 Super의 디폴트 생성자는 존재하지 않기 때문에 에러가 나게된다.

이 문제를 해결하기 위해서는, 1) 아래와 같이 `Super()` 생성자를 Super 클래스에 추가하거나

```java
public Super() {
  System.out.println("Super");
}
```

, 또는 2) 정의 되어 있는 Super 클래스의 생성자를 없애거나, 혹은 `super(value)`를 Sub 클래스의 각 생성자에 추가해주면 된다.

### #10. "" 아니면 생성자?
문자열은 두 방법으로 생성할 수 있다:

```java
//1. use double quotes
String x = "abc";
//2. use constructor
String y = new String("abc");
```

여기서의 차이는 무엇일까?

다음의 예제에서 쉽게 확인할 수 있다.

```java
String a = "abcd";
String b = "abcd";
System.out.println(a == b);  // True
System.out.println(a.equals(b)); // True
 
String c = new String("abcd");
String d = new String("abcd");
System.out.println(c == d);  // False
System.out.println(c.equals(d)); // True
```

문자열이 메모리에 어떻게 저장되는지에 대한 더욱 자세한 내용은 [Create Java String Using "" or Constructor?](http://www.programcreek.com/2014/03/create-java-string-by-double-quotes-vs-by-constructor/)에서 볼 수 있다.
출처 : https://bestalign.github.io/top-10-mistakes-java-developers-make-1/