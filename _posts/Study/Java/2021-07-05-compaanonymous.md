---
title:  "Comparable vs Comparator (Anonymous)"
excerpt: 
categories: 
- Java
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-07-05
last_modified_at: 2021-07-05 21:37
---

## Comparable Comparator API 확인하기

Comparable과 Comparator는 모두 인터페이스(interface)  

즉, Comparable 혹은 Comparator을 사용하고자 한다면 인터페이스 내에 선언된 메소드를 '반드시 구현'

[Comparable 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#method.summary)

보면 Comparable 인터페이스에는 compareTo(T o) 메소드 하나가 선언되어있는 것을 볼 수 있다.  

우리가 만약 Comparable을 사용하고자 한다면 compareTo 메소드를 재정의(Override/구현)을 해주어야 해야한다.  

[Comparator 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#method.summary)

Comparator를 보면 선언 된 메소드가 많지만, 우리가 실질적으로 구현해야 하는 것은 단 하나다.  

바로 compare(T o1, T o2)다.  

인터페이스라면 선언된 메소드를 모두 구현해야하지만 Comparator는 그러지 않아도 문제가 없다.  

이유는 Java8부터는 Interface에서도 일반 메소드(함수)를 구현할 수 있도록 변경되었기 때문이다.  

![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/comparator.JPG)

거의 대부분이 default 로 선언된 메소드나, static으로 선언된 메소드인 것을 볼 수 있다.  

그리고 default 나 static으로 선언된 메소드들을 보면 함수를 구현하고 있다.  

보다시피 기존 구현된 함수를 반환하거나 등, 함수의 내용이 { ... } 블럭 안에 구현이 되어있는 것을 볼 수 있다.  

이 말은, default나 static으로 선언된 메소드가 아니면 이는 추상메소드라는 의미로 반드시 재정의를 해주어야 한다는 것이다.  

default와 static의 차이라면 default로 선언된 메소드는 재정의(Override)를 할 수 있고, static은 재정의를 할 수 없다는 차이다.  


참고로 bool equals(Object obj) 메소드는 default나 static이 안붙어있음에도 구현이 강제되지 않는 이유는 모든 객체의 최상위 타입(객체)인 Object 클래스에서 정의되어있기 때문이다.
{: .notice--warning}

이제 두 인터페이스의 차이점을 알 수 있다.  

Comparable 인터페이스를 쓰려면 compareTo 메소드를 구현해야하고, Comparator 인터페이스를 쓰려면 compare 메소드를 구현해야 한다는 점이다.  

## Comparable과 Comparator란?

### 필요한 이유
보통 많은 사람들의 경우 객체를 정렬을 하기 위해 쓴다고 한다만, 정확히 말하자면 그 건 용도에 불과하다.  

여러분이 생각해야 할 것은 딱 하나다.  

"객체를 비교할 수 있도록 만든다."  

왜 객체를 비교할 수 있도록 한다는 것일까?  

우리는 primitive 타입의 실수 변수(byte, int, double 등등..)의 경우 부등호를 갖고 쉽게 변수를 비교할 수 있었다.  

```java
public class Test {
    public static void main(String[] args)  {
        int a = 1;
        int b = 2;
        
        if(a > b) {
            System.out.println("a가 b보다 큽니다.");
        }
        else if(a == b) {
            System.out.println("a와 b는 같습니다.");
        }
        else {
            System.out.println("b가 a보다 큽니다. ");
        }
    }
}

```
이런식으로 primitive type은 자바 자체에서 제공되기에 별다른 처리 없이 비교가 가능하다.   

즉, 기본 자료형이기 때문에 부등호로 쉽게 비교가 가능하다.  

하지만, 여러분들이 새로운 클래스 객체를 만들어 비교하고자 한다면 어떻게 될까?  

예로들어 학생의 나이와 학급 정보를 갖고있는 클래스를 만든다고 가정해보자.

```java
public class Test {
    public static void main(String[] args)  {
		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		/*
		   어떻게 비교..
		   if(a > b) ..?
		 */
	}
}
class Student {
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
}
```

자, a학생과 b학생 두 객체를 생성했다.  

그럼 두 객체(a, b)를 어떻게 비교할 것인가?  

부등호로 비교하려 하면, 나이(age)를 기준으로 비교되는 건가? 아니면 학급(classNumber)을 기준으로 비교되는 건가?  

이 부분이 포인트다. 본질적으로 객체는 사용자가 기준을 정해주지 않는 이상 어떤 객체가 더 높은 우선순위를 갖는지 판단 할 수가 없다.    

어떤 사람은 나이를 기준으로 판단할테고, 또 다른 사람은 학급을 기준으로 판단하는 등 그 기준이 중구난방일 것이다.  

그래서 이러한 문제점을 해결하기 위해 바로 Comparable 또는 Comparator가 쓰인다는 것이다.  

### Comparable과 Comparator의 역할은 비슷한 것 같은데 무슨 차이인 것일까?

왜 Comparable의 compareTo(T o) 메소드는 파라미터(매개변수)가 한 개이고, Comparator의 compare(T o1, T o2) 메소드는 파라미터가 왜 두 개인 것일까?  

Comparable은 자기 자신과 파라미터로 들어오는 객체를 비교하는 것이고, Comparator는 자기 자신의 상태가 어떻던 상관없이 파라미터로 들어오는 두 객체를 비교하는 것이다.  

즉, 본질적으로 비교한다는 것 자체는 같지만, 비교 대상이 다르다는 것이다.  

또 다른 차이점이라면 Comparable은 lang패키지에 있기 때문에 import 를 해줄 필요가 없지만, Comparator는 util패키지에 있다.  

그럼 한 번 Comparable과 Comparator을 각각 알아보도록 해보자.  

## Comparable

Comparable은 무엇이라고 했는가?  

"자기 자신과 매개변수 객체를 비교"한다고 했다. 이 것을 기억해두고 한 번 사용법을 알아보자.  

즉, 여러분이 클래스를 만들 때, 기본적으로 사용 방법은 이렇다.  
```java
public class ClassName implements Comparable<Type> {
	// 필수 구현 부분
	@Override
	public int compareTo(Type o) {
		/*
		 비교 구현
		 */
	}
}
```

이 때, 필수 구현 부분인 compareTo() 메소드가 바로 우리가 객체를 비교할 기준을 정의해주는 부분이 된다.  

아까 Comparable은 자기 자신과 매개변수 객체를 비교한다고 했다.  

즉, 자기자신은 ClassName으로 생성한 객체 자신이 되고, 매개변수 객체는 ClassName.compareTo(o); 를 통해 들어온 파라미터 o가 비교 할 객체가 되는 것이다.  

예로 들어보자. 아까 Student클래스를 비교하고자 했으니 이를 위 방법에 맞게 적용하려면 어떻게 해야할까?  

일단, Student 클래스에 Comparable 을 implements 해야한다. 그리고 <> 사이에 들어갈 타입은 무엇일까?  

Student 객체와 또 다른 Student 객체를 비교하고 싶다면, <> 사이에 들어갈 타입 또한 Student가 되어야하지 않겠는가?  

즉, Type 은 Student로 바뀌게 된다.  

이제 compareTo 메소드를 구현해야 할 것이다. 만약 나이를 기준으로 비교(대소 관계)를 하고자 한다면 어떻게 하면 될까?  
자기 자신의 age(나이)와 매개변수로 들어온 o의 age(나이)의 값을 비교하면 된다.  

```java
class Student implements Comparable<Student> {
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
	
	@Override
	public int compareTo(Student o) {
		// 자기자신의 age가 o의 age보다 크다면 양수
		if(this.age > o.age) {
			return 1; // 12345 같이 양수면 다 가능 (물론 굳이 하진 않는다)
		}
		// 자기 자신의 age와 o의 age가 같다면 0
		else if(this.age == o.age) {
			return 0;
		}
		// 자기 자신의 age가 o의 age보다 작다면 음수
		else {
			return -1;
		}
	}
}
```
compareTo 메소드를 보면 int값을 반환하도록 되어있다.  

'값'을 비교해서 정수를 반환해야 한다는 것이다.  

그럼 이러한 의문이 나올 것이다. 무슨 기준으로 양수, 0, 음수를 반환하는 건가요?  

우리는 "자기 자신"과 "상대방"을 비교하는 것이다. 즉, 자기 자신을 기준으로 삼아 대소관계를 파악해야 한다.  

만약 내가 갖고 있는 값이 7라고 가정해보자. 그리고 상대방은 3이라고 가정한다면, 나 자신은 상대방보다 값이 4만큼 크다.  

반대로 상대방이 9을 갖고 있다고 가정하면, 나는 상대방보다 2만큼 작다. 즉, -2 만큼 크다는 것이다.  

한 마디로 자기 자신을 기준으로 상대방과의 차이가 얼마나 나느냐다.

그래서 필자가 위 코드에서 1, 0, -1을 반환했지만, 주석으로 '양수', '음수'라고 표현한 것도 이 때문이다.  

사실 1, 0, -1이 이해하기는 쉬울테고 아마 많은 분들도 1, 0, -1 을 반환값으로 썼거나 그렇게 배웠을 것이다.  

하지만, 꼭 1, 0, -1 이 아니라 양수, 0, 음수로 표현해도 된다는 것이다.  

```java
class Student implements Comparable<Student> {
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
	@Override
	public int compareTo(Student o) {
		/*
		 * 만약 자신의 age가 o의 age보다 크다면 양수가 반환 될 것이고,
		 * 같다면 0을, 작다면 음수를 반환할 것이다.
		 */
		return this.age - o.age;
	}
}
```

위와같이 두 값의 차를 반환해버리면 번거로운 조건식 없이 한방에 3개의 조건을 만족할 수 있다.  

## Comparator

두 번쨰로 Comparator다.  

객체를 비교하는 것 자체는 Comparable과 비슷하면서도 다르기 때문에 자주 두 개를 헷갈리곤 한다.   

"두 매개변수 객체를 비교"  

이 말은 자기 자신이 아니라 파라미터(매개 변수)로 들어오는 두 객체를 비교하는 것이다.  

그럼 Comparator는 어떤 형식일까?  

Comparable과 인터페이스 형식이 유사하게 interface Comparator<T> { ... } 라고 되어있다.  

즉, 여러분이 클래스를 만들 때, 기본적으로 사용 방법은 이렇다.  
```java

import java.util.Comparator;	// import 필요
    public class ClassName implements Comparator<Type> {
	// 필수 구현 부분
	@Override
	public int compare(Type o1, Type o2) {
		/*
		 비교 구현
		 */
	}
}
```

이 때, 필수 구현 부분인 compare() 메소드가 바로 우리가 객체를 비교할 기준을 정의해주는 부분이 된다.  

앞서 말했듯, Comparable과 다르게 Comparator는 매개변수로 들어오는 두 객체를 비교하는 것이기 때문에 당연히 매개변수가 두 개가 되는 것이다.  

그러면 이제 compare 메소드를 구현해야 할 것이다.  

기본적으로 compare메소드 매커니즘 자체는 compareTo와 같다.  

다만, 자기 자신과 비교되느냐 안되느냐의 차이일 뿐이다.  

일단 코드를 보면서 이해해보자. 이 번엔 한 번 학급을 기준으로 정의를 해보도록 하겠다.  

```java
import java.util.Comparator;	// import 필요
class Student implements Comparator<Student> {
	int age;			// 나이
	int classNumber;	// 학급

	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
	
	@Override
	public int compare(Student o1, Student o2) {
		// o1의 학급이 o2의 학급보다 크다면 양수
		if(o1.classNumber > o2.classNumber) {
			return 1;
		}
		// o1의 학급이 o2의 학급과 같다면 0
		else if(o1.classNumber == o2.classNumber) {
			return 0;
		}
		// o1의 학급이 o2의 학급보다 작다면 음수
		else {
			return -1;
		}
		// return o1.classNumber - o2.classNumber; 가능
	}
}
```
앞서 Comparable의 compareTo()와는 다르게, 두 객체를 비교하는 것이기 때문에 파라미터로 들어오는 o1과 o2의 classNumber을 비교해주는 것이다.  

좀 더 구체적으로 말하자면 Comparable의 compareTo는 선행 원소가 자기 자신이 되고, 후행 원소가 매개 변수로 들어오는 o 가 되는 반면에, Comparator의 compare는 선행 원소가 o1이 되고, 후행 원소가 o2가 된다.  

이 말은, o1과 o2를 비교함에 있어 자기 자신은 두 객체 비교에 영향이 없다는 뜻이다.  

a.compare(b, c); 의 경우 b, c로만 비교한다.  

a객체의 compare 메소드를 통해 비교하지만, 그 내부에선 두 매개변수인 b(o1)과 c(o2) 가 비교되는 것이기 때문에 a객체와는 관련 없이 두 객체의 비교 값을 반환하게 되는 것이다.  

만약에 a.compare 메소드에서 a와 비교하고 싶다면 다음과 같이 해주면 되는 것이다.  

a.compare(a, b);  

즉, 객체 자체와는 상관 없이 독립적으로 매개변수로 넘겨진 두 객체를 비교하는 것이 포인트다.  

## Comparator 활용편

위에서 보았듯이 Comparator를 통해 compare 메소드를 사용려면 결국에는 compare메소드를 활용하기 위한 객체가 필요하게 된다.  

무슨 말인가 하면, a, b, c 객체가 생성되어있고, 이들을 비교를 하고 싶다면 어느 한 객체를 통해 compare메소드를 사용해야한다는 것이다.  

즉, 다음과 같은 상황이 온다는 것이다.  

```java

public class Test {
    public static void main(String[] args)  {
		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		Student c = new Student(15, 3);	// 15살 3반
		//          ⋁
		int isBig = a.compare(a, b);
        //           ⋁
		int isBig2 = a.compare(b, c);
		//           ⋁
		int isBig3 = b.compare(a, c);
	}
}
```

보면 메소드를 호출하기 위한 대상(⋁ 표시 된 부분)은 사실 a이건, b이건, c이건 어떤 객체를 통해 호출하던 상관이 없다.  

이 말을 조금 돌려서 생각해보면, 일관성이 떨어진다는 것이다.  

즉, 우리가 원하는 것은 Comparator 비교 기능만 따로 두고 싶은 것이다.  

Comparator 기능만 따로 두고싶다면 어떻게 해야할까?  

답은 매우 간단하다. "익명 객체(클래스)를 활용한다"  

### Anonymous

익명 객체는 쉽게 말해서 '이름이 정의되지 않은 객체'를 의미한다.  

자바는 객체지향 언어다. 그래서 여러분이 어떠한 객체를 만든다고 한다면 class를 생성하여 이름을 정의한다.  

그동안 우리가 예시로 들었던 Student 또한 Student라는 이름으로 정의된 객체다.  

그럼 이름이 정의되지 않는다는 것은 무엇일까?  

우리가 클래스를 생성할 때 class 키워드 다음에 이름을 정의했다.  

하지만, 이름 없이 class를 정의할 수 있는가? 불가능 하다.  

하지만, 우리의 고민처럼 특정 구현 부분만 따로 사용한다거나, 부분적으로 기능을 일시적으로 바꿔야 할 경우가 생길 때가 있다.  

이럴 때 사용할 수 있는 것이 바로 익명객체인데, 일단 코드를 먼저 보도록 하자.  

```java
public class Anonymous {
	public static void main(String[] args) {
		Rectangle a = new Rectangle();
		// 익명 객체 1 
		Rectangle anonymous1 = new Rectangle() {
			@Override
			int get() {
				return width;
			}
		};
		
		System.out.println(a.get());
		System.out.println(anonymous1.get());
		System.out.println(anonymous2.get());
	}
	// 익명 객체 2
	static Rectangle anonymous2 = new Rectangle() {
		int depth = 30;
		@Override
		int get() {
			return width * height * depth;
		}
	};
}
class Rectangle {
	int width = 10;
	int height = 20;
	
	int get() {	
		return height;
	}
}
```
보면 우리가 일반적으로 객체 생성방식과는 조금 다르다.  

보통의 경우 다음과 같이 생성할 것이다.  

Rectangle a = new Rectangle();  

하지만, 익명객체의 경우는 다음과 같이 생성된다.  

Rectangle a = new Rectangle() { //...구현부...// };  

왜 익명 객체인 것일까? 얼핏보면 같은 객체 생성 방식인 것 같지만, 우리가 보아야 할 것은 { } 블럭 안의 구현부이다.  

우리가 객체를 구현한다는 것은 무엇일까?   

바로 변수를 선언하고, 메소드를 정의하며 하나의 클래스(객체)로 만든다는 것을 의미한다.  

말 자체는 어렵지만 쉽게 생각해보면 위 Rectangle 클래스처럼 일반적인 클래스 구현 방식과, interface 클래스를 implements 하여 interface의 메소드를 재정의하거나, class 를 상속(extends)하여 부모의 메소드, 필드를 사용 또는 재정의 하는 것들 모두 객체를 구현하는 것이다.  

이 때, 구현을 하는 클래스들은 모두 '이름'이 존재한다.  

그러나 한 번 Rectangle anonymous2 = new Rectangle() {...} 이 부분을 한 번 봐보자.  

구현부에서 분명히 변수를 선언하기도 하고, Rectangle 클래스의 메소드 get()을 '재정의(Override)'를 했다.  

즉, 쉽게 생각하여 'Rectangle을 상속받은 하나의 새로운 class라는 것이다.  

분명 새로운 class인데 이름이 정의되지 않고 있다.  

이는 annoymous1 객체 또한 마찬가지다.  

음? 이름은 Rectangle이 아닌가요? 라고 생각할 수 있지만 아니다. 한 번 두 코드를 비교해보자.  

```java
public class Anonymous {
  public static void main(String[] args) {
          Rectangle a = new Rectangle();
          ChildRectangle child = new ChildRectangle();
   
          System.out.println(a.get());		// 20
          System.out.println(child.get());	// 10 * 20 * 40
      }
  }

class ChildRectangle extends Rectangle {
	int depth = 40;
	
	@Override
	int get() {
		return width * height * depth;
	}
}

class Rectangle {
	int width = 10;
	int height = 20;
 
	int get() {
		return height;
	}
}
```

위 코드는 Rectangle 이라는 클래스를 상속받아 ChildeRectangle 이라는 이름으로 정의 된 자식 클래스가 있다.  

그리고 그 자식 클래스에서는 depth란 필드(변수)도 새로 생성했고, get() 메소드를 가로 세로 높이의 곱을 반환하도록 재정의되었다.  

그리고 각 클래스는 a와 child 란 변수 명으로 객체를 담고 있다.  

그 다음 익명 객체를 사용한 코드를 한 번 보자.  

```java

public class Anonymous {
	public static void main(String[] args) {
		Rectangle a = new Rectangle();
 
		Rectangle anonymous = new Rectangle() {
			int depth = 40;
			@Override
			int get() {
				return width * height * depth;
			}
		};
 
		System.out.println(a.get());			// 20 
		System.out.println(anonymous.get());	// 10 * 20 * 40
	}
}
class Rectangle {
	int width = 10;
	int height = 20;
 
	int get() {
		return height;
	}
}
```

분명 앞서 본 상속받아 ChildRectangle 클래스를 만든 것과 같지만, 이 코드는 이름이 정의되어있지 않고, anonymous라는 이름으로 객체만 생성되어 있다.  

이렇게 클래스 이름으로 정의되지 않는 객체를 바로 익명 객체라 하는 것이다.  

이는 거꾸로 말하면, 이름이 정의되지 않기 때문에 특정 타입이 존재하는 것이 아니기 때문에 반드시 익명 객체의 경우는 상속할 대상이 있어야 한다는 것이다.  

이 때, 상속이라 함은 class의 extends 뿐만 아니라 interface의 implements 또한 마찬가지다.  

```java
public class Anonymous {
public static void main(String[] args) {
		Rectangle a = new Rectangle();
		
		Shape anonymous = new Shape() {
			int depth = 40;
			
			@Override
			public int get() {
				return width * height * depth;
			}
		};
 
		System.out.println(a.get());			// Shape 인터페이스를 구현한 Rectangle
		System.out.println(anonymous.get());	// Shape 인터페이스를 구현한 익명 객체
	}

}

class Rectangle implements Shape {
int depth = 40;

	@Override
	public int get() {
		return width * height * depth;
	}
}

interface Shape {
	int width = 10;
	int height = 20;
 
	int get();
}
```

우리가 원하는 것은 무엇이었을까? 바로 Comparator 의 기능만 사용하고 싶은 것이다.  

즉, Comparator의 구현을 통해 compare만 사용하고 싶은 것이라는 뜻이다.  

앞서 익명객체에서 설명한 것을 적용해보자.  

분명히 Comparator라는 interface는 존재한다. 이는 구현(상속)할 대상이 존재한다는 것이다. 이는 익명객체로 만들 수 있다는 것이다.  

즉, 이름은 정의 되지 않지만, Comparator을 구현하는 익명객체를 생성하면 되는 것이다.  

이 때, Comparator 구현은 이 전에 class Student implements Comparator { ... } 에서 구현했던 방식을 그대로 차용하면 된다.  

```java
import java.util.Comparator;

public class Test {
    public static void main(String[] args) {
		// 익명 객체 구현방법 1
		Comparator<Student> comp1 = new Comparator<Student>() {
			@Override
			public int compare(Student o1, Student o2) {
				return o1.classNumber - o2.classNumber;
			}
		};
	}
	// 익명 객체 구현 2
	public static Comparator<Student> comp2 = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.classNumber - o2.classNumber;
		}
	};
}
// 외부에서 익명 객체로 Comparator가 생성되기 때문에 클래스에서 Comparator을 구현 할 필요가 없어진다.
class Student {
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}

}
```

익명 객체의 경우 필요에 따라 main함수 밖에 정적(static) 타입으로 선언해도 되고, main안에 지역변수처럼 non-static으로 생성해도 된다.  

자. 이렇게 외부에서 Comparator을 구현하는 익명객체가 생성되었기 때문에, Student 클래스 내부에서 우린 Comparator을 구현해줄 필요가 없어졌다.  

즉, 이 전에 a.compare(b, c) 이런식이 아니라, 위에서 생성한 익명객체를 가리키는 comp 를 통해 comp.compare(b, c) 이런 식으로 해주면 된다는 것이다.

익명 객체는 이름이 정의되지 않은 하나의 새로운 클래스와 같다고 보면 된다.  

클래스를 상속(구현)할 때, 이름만 다르게 하면 몇 개던 여러개를 생성할 수 있듯이, 익명 객체 또한 마찬가지다. 다만, 이름이 없을 뿐이라는 것이다.  

즉, 익명 객체를 가리키는 변수명만 달리하면 몇 개든 자유롭게 생성할 수 있다.  

위 예제에서는 학급을 기준으로 대소 비교를 했지만, 만약 나이를 기준으로도 대소 비교를 하고 싶다면 다음과 같이 하나의 또다른 익명 객체를 생성 할 수 있다는 것이다.  

```java
import java.util.Comparator;

public class Test {
    public static void main(String[] args)  {
		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		Student c = new Student(15, 3);	// 15살 3반
			
		// 학급 기준 익명객체를 통해 b와 c객체를 비교한다.
		int classBig = comp.compare(b, c);
		
		if(classBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(classBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}
		
		// 나이 기준 익명객체를 통해 b와 c객체를 비교한다.
		int ageBig = comp2.compare(b, c);
		
		if(ageBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(ageBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}
		
	}
	
	// 학급 대소 비교 익명 객체
	public static Comparator<Student> comp = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.classNumber - o2.classNumber;
		}
	};
	
	// 나이 대소 비교 익명 객체
	public static Comparator<Student> comp2 = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.age - o2.age;
		}
	};
}

class Student {
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
}
```

즉, 익명객체를 통해 여러가지 비교 기준을 정의할 수 있다는 것이 큰 장점인 것이다.  

Comparable도 익명객체로 할 수 있지 않나요?라고 물을 수 있다. 물론 생성이 가능은 하다.  

하지만 좀만 고민해보면 굳이 Comparable을 익명객체로 생성 할 필요도 없고 오히려 복잡해진다. 이유는 단순하다.  

Comparable과 Comparator의 차이는 계속 말했듯이 "자기 자신"과 하나의 매개변수를 비교하느냐, 두 개의 매개변수를 비교하느냐의 차이다.

만약 Comparable을 익명객체로 다음과 같이 생성했다고 가정해보자.

```java
public static Comparable<Student> comp = new Comparable<Student>() {
    @Override
    public int compareTo(Student o1) {
    // 구현부
    }
};
```

그러면 Comparable에서 자기 자신은 무엇인가?  

익명 객체가 될 것이다. Student객체가 아니라는 것이다.  

즉, 익명의 객체와 Student가 비교하는 것이지, Student와 Student가 비교되는 것이 아니라는 것이다.  

```java
public class Test {
    public static void main(String[] args)  {
		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		/*
		 * Stduent b 객체는 comp의 30이랑 비교되는 것이다.
		 * 즉, a.compareTo(b) 처럼 서로 다른 객체에 대한 비교가 불가능하다.
		 */
		int classBig = comp.compareTo(b);
	}
	// 학급 대소 비교 익명 객체
	public static Comparable<Student> comp = new Comparable<Student>() {
		int a = 30;
		@Override
		public int compareTo(Student o) {
			return a - o.classNumber;
		}
	};
}
// Student 클래스 생략
```

한 마디로 여러분이 자기 동일한 타입의 자신의 객체와 어떤 객체를 비교하고자 하면 Comparable을 익명객체로 선언한다고 한들, 동일한 타입 비교는 불가능하다는 것이다.  

## Comparable, Comparator 와 정렬의 관계

객체를 비교하기 위해 Comparable 또는 Comparator을 쓴다는 것은 곧 사용자가 정의한 기준을 토대로 비교를 하여 양수, 0, 음수 중 하나가 반환된다는 것이다.  

여기서 정렬과의 관계를 알아보기 전에 Java의 일반적인 정렬기준에 대해 알고가야 할 필요가 있다.  

대부분의 언어도 마찬가지지만, Java에서의 정렬은 특별한 정의가 되어있지 않는 한 '오름차순'을 기준으로 한다.  

우리가 흔히 쓰는 Arrays.sort(), Collections.sort() 모두 오름차순을 기준으로 정렬이 된다는 것이다.  

오름차순으로 정렬이 된다는 것은 무엇일까?  

예로들어 {1, 3, 2} 배열이 있다고 가정해보자.  

그럼 우리가 최종적으로 얻어야 할 배열 {1, 2, 3} 을 얻기 위해 정렬 알고리즘을 사용하게 될 것이다.  

이 때, 정렬을 하기 위해 두 원소를 비교 하게 될 것 아닌가?   

정렬 메소드에서 두 수를 비교하기 위해 index 0 원소와 index 1 원소를 비교한다고 가정해보자.  

그럼 선행 원소인 1과 후행 원소인 3의 경우 대소관계는 어떻게 되는가? 1이 3보다 작다.  

앞서 선행 원소와 후행 원소를 비교 할 때, 얼마큼 차이가 나는지를 반환한다고 헀다.  

return o1 - o2; 를 한다면, 1-3 = -2로 '음수'가 나올 것이다.  

이 때, 자바에서는 오름차순을 디폴트 기준으로 삼고 있다고 했다.  

이 말은 선행 원소가 후행 원소보다 '작다'는 뜻이다.  

즉, compare 혹은 compareTo를 사용하여 객체를 비교 할 경우 음수가 나오면 두 원소의 위치를 바꾸지 않는다는 것이다.  

그 다음 정렬 알고리즘에 의해 index 1 원소와 index 2 원소를 비교한다고 해보자.  

선행 원소인 3이 2보다 크다.  

compare 혹은 compareTo를 사용하여 index 1 원소와 index 2 원소를 비교한다면 '양수'가 나올 것이다.  

(3-2 = 1) 이는 곧 이러면 선행 원소가 후행 원소보다 크다는 뜻이라는 것이다.  

즉, compare 혹은 compareTo를 사용하여 객체를 비교 할 경우 양수가 나오면 두 원소의 위치를 바꾼다는 것이다.  

그러면 {1, 2, 3} 으로 오름차순으로 정렬 될 것이다.  

그럼 규칙을 일반화 할 수 있다.  

- 두 수의 비교 결과에 따른 작동 방식
    - 음수일 경우 : 두 원소의 위치를 교환 안함
    - 양수일 경우 : 두 원소의 위치를 교환 함
    
정렬을 구현해보면 알겠지만 Counting Sort 같은 특수한 경우를 제외하고 Insertion, Quick, Merge 등 다양한 정렬 알고리즘은 '두 데이터(요소)의 비교'를 통해 두 원소를 교환할지 말지를 정하게 된다.  

앞서 primitive type의 경우 이미 대소 비교가 가능하지만, 객체를 정렬하고자 한다면 너무나 당연히도 두 요소를 비교하기 위해서는 Comparable을 통한 compareTo() 혹은, Comparator을 통한 compare() 메소드를 활용하여 두 객체의 대소 비교를 한다는 것이다.  

진짜로 그럴까?  

Arrays.sort 메소드에서의 Merge Sort 중 일부분은 다음과 같다.  

보면 Comparator c라는 매개변수를 통해 c.compare을 호출하고 각 두 요소를 비교한다는 것을 볼 수 있다.  

또한 Comparator가 아닌 Comparable을 통한 compareTo() 메소드를 활용하는 Merge Sort 또한 존재한다.  

위 코드에서도 일관되게 오름차순으로 정렬하기 때문에 두 원소중 선행 원소가 작으면 교환을 안하고, 그 반대라면 교환하는 메커니즘은 같다.  

예로들어 다음과 같은 객체를 만든 뒤 이를 배열로 만들어서 생성 된 객체 배열을 정렬하고자 한다.   

앞서 배웠던 비교 기준을 생성하면 되는 것이다.  

우리는 비교 기준을 설정하는 방법 두 가지를 배웠다. Comparable과 Comparator다.  

Comparable을 사용한다면 MyInteger 클래스에 구현(implements)을 해야 할 것이다.

그리고 나서 정렬 메소드로 가장 자주 쓰이는 Arrays.sort()메소드에 한 번 돌려서 테스트를 해보자. 다음과 같이 말이다.
```java
import java.util.Arrays;

public class Test {
	public static void main(String[] args) {
		MyInteger[] arr = new MyInteger[10];
		// 객체 배열 초기화 (랜덤 값으로) 
		for(int i = 0; i < 10; i++) {
			arr[i] = new MyInteger((int)(Math.random() * 100));
		}
		// 정렬 이전
		System.out.print("정렬 전 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
		
		Arrays.sort(arr);
        
		// 정렬 이후
		System.out.print("정렬 후 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
	}

}

class MyInteger implements Comparable<MyInteger> {
int value;

	public MyInteger(int value) {
		this.value = value;
	}
	
	@Override
	public int compareTo(MyInteger o) {
		return this.value - o.value;
	}

}
```
만약에 Comparable을 구현하지 않고 그냥 정렬했다면 어떻게 될까?  

예외가 던져지면서 프로그램이 종료된다.  

MyInteger 클래스를 Arrays.sort 안에서 정렬을 하면서 원소를 비교하려 하는데, 해당 클래스가 비교할 수 있는 기준이 정의되어있지 않아서 정렬 자체가 불가능한 것이다.  

만약 Comparable 대신 Comparator을 쓴다면 어떻게 해야할까?  

앞서 배운 것 처럼 익명객체를 생성하여 MyInteger에 대한 Comparator를 구현해주는 것이다.  

위를 적용하면 전체적인 코드는 다음과 같을 것이다.  

```java
import java.util.Arrays;
import java.util.Comparator;

public class Test {
	public static void main(String[] args) {
		MyInteger[] arr = new MyInteger[10];
		
		// 객체 배열 초기화 (랜덤 값으로) 
		for(int i = 0; i < 10; i++) {
			arr[i] = new MyInteger((int)(Math.random() * 100));
		}
	}
	static Comparator<MyInteger> comp = new Comparator<MyInteger>() {
		@Override
		public int compare(MyInteger o1, MyInteger o2) {
			return o1.value - o2.value;
		}
	};
}

class MyInteger {
    int value;

	public MyInteger(int value) {
		this.value = value;
	}

}
```

(필자는 정적 변수로 main메소드 밖에 선언해줄 것이기 때문에 static을 붙여 사용할 것이니 참고하시길 바란다.)  

그러면 이제 우리가 만든 comp 익명 객체를 사용하여 정렬할 수 있도록 해야한다.  

"그러면 Arrays.sort()에 어떻게 Comparator 익명객체를 기준으로 정렬을 시키는 것이죠?" 라고 물을 수 있다.  

이 부분은 걱정 할 것이 없다. Arrays.sort()에는 단순히 배열만 파라미터로 받는 것이 아니라 Comparator 또한 파라미터로 받기도 한다.  

즉, 우리가 쓸 메소드는 다음과 같다.  

```java
public static <T> void sort(T[] a, Comparator<? super T> c){
    if(c == null){
        sort(a);
    } else {
        if(LegacyMergeSort.userRequested)
            legacyMergeSort(a, c);
        else
            TimSort.sort(a, 0, a.length, c, null, 0, 0);
    }
}
```
이 메소드에 대한 내용은 다음과 같다.  

sort  
public static <T> void sort(T[] a, Comparator<? super T> c)  
Sorts the specified array of objects according to the order induced by the specified comparator.  
All elements in the array must be mutually comparable by the specified comparator (that is, c.compare(e1, e2) must not throw a ClassCastException for any elements e1 and e2 in the array).  
This sort is guaranteed to be stable: equal elements will not be reordered as a result of the sort.
{: .notice--warning}

간략하게 요약하자면, Comparator 파라미터로 넘어온 c의 비교 기준을 갖고 파라미터로 넘어온 객체배열 a을 정렬하겠다는 의미다.  

우리가 그동안 Arrays.sort()를 쓸 때 Arrays.sort(array); 이런식으로 배열만 넘겨주었지만 사실은 Comparator로 구현된 객체를 파라미터로 같이 넘겨주어 Arrays.sort(array, comp); 로도 쓸 수 있다는 것이다.  

즉, 우리가 구현한 Comparator 익명객체를 이용하여 정렬을 하고싶다면 아래와 같이 작성을 해주면 된다는 것이다.  

```java
import java.util.Arrays;
import java.util.Comparator;

public class Test {
	public static void main(String[] args) {
		MyInteger[] arr = new MyInteger[10];
		// 객체 배열 초기화 (랜덤 값으로) 
		for(int i = 0; i < 10; i++) {
			arr[i] = new MyInteger((int)(Math.random() * 100));
		}
		// 정렬 이전
		System.out.print("정렬 전 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
		
		Arrays.sort(arr, comp);		// MyInteger에 대한 Comparator을 구현한 익명객체를 넘겨줌
		// 정렬 이후
		System.out.print("정렬 후 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
	}
	
	static Comparator<MyInteger> comp = new Comparator<MyInteger>() {
		@Override
		public int compare(MyInteger o1, MyInteger o2) {
			return o1.value - o2.value;
		}
	};
}

class MyInteger {
    int value;

	public MyInteger(int value) {
		this.value = value;
	}


}
```

여기까지는 쉽게 이해했을 것이다.   

사용자 클래스 객체 배열을 정렬하고자 하면 Comparable 혹은 Comparator을 통해 해당 클래스가 비교가 될 수 있도록 해야한다는 것이다.  

그리고 Java에서 sort 알고리즘은 오름차순을 기준으로 구현되어있다.  

근데, 만약 오름차순이 아닌 내림차순으로 구현하고 싶다면 어떻게 해야할까?  

sort메소드를 새로 구현하는 것도 방법이긴 하나 그 과정이 너무 길고 복잡해져버린다.  

그렇다고 기존 Arrays.class 파일을 수정 할 수도 없다.  

이에 대한 해답은 의외로 쉽다.  

```
// Comparable
public int compareTo(MyClass o) {
return this.value - o.value;
}

// Comparator
public int compare(Myclass o1, MyClass o2) {
return o1.value - o2.value;
}
```

즉, 정렬 알고리즘에서는 두 원소를 compare 혹은 compareTo 를 써서 양수값이 나오냐, 음수값이 나오냐에 따라 판단을 한다는 것이다.  

위 방법이 오름차순이라면 내림차순으로 정렬하고 싶은 경우 두 원소를 비교한 반환값을 반대로 해주면 되는 것 아닌가?  

쉽게 말해 두 값의 차가 양수가 된다면 이를 음수로 바꿔 반환해주고, 만약 음수가 된다면 그 값을 양수로 바꾸어 반환해주면 된다는 것이다.  

```
// Comparable
public int compareTo(MyClass o) {
return o.value - this.value;	// == -(this.value - o.value);
}

// Comparator
public int compare(Myclass o1, MyClass o2) {
return o2.value - o1.value;		// == -(o1.value - o2.value);
}
```

위와 같이 반환값의 부호(sign)을 바꿔주는 것이다.  

그러면 위에서 배운 내용을 토대로 한 번 내림차순으로 정렬되도록 해보자.

[Comparable]
```java
import java.util.Arrays;
import java.util.Comparator;

public class Test {
	public static void main(String[] args) {
		MyInteger[] arr = new MyInteger[10];
		// 객체 배열 초기화 (랜덤 값으로) 
		for(int i = 0; i < 10; i++) {
			arr[i] = new MyInteger((int)(Math.random() * 100));
		}
		// 정렬 이전
		System.out.print("정렬 전 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
		
		Arrays.sort(arr);		// MyInteger에 대한 Comparable을 사용하여 정렬
		// 정렬 이후
		System.out.print("정렬 후 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
	}
}

class MyInteger implements Comparable<MyInteger> {
    int value;

	public MyInteger(int value) {
		this.value = value;
	}
	
	@Override
	public int compareTo(MyInteger o) {
		return o.value - this.value;
	}

}
```
[Comparator]
```java

import java.util.Arrays;
import java.util.Comparator;

public class Test {
	public static void main(String[] args) {
		MyInteger[] arr = new MyInteger[10];
		// 객체 배열 초기화 (랜덤 값으로) 
		for(int i = 0; i < 10; i++) {
			arr[i] = new MyInteger((int)(Math.random() * 100));
		}
		// 정렬 이전
		System.out.print("정렬 전 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
		
		Arrays.sort(arr, comp);		// MyInteger에 대한 Comparator을 구현한 익명객체를 넘겨줌
		// 정렬 이후
		System.out.print("정렬 후 : ");
		for(int i = 0; i < 10; i++) {
			System.out.print(arr[i].value + " ");
		}
		System.out.println();
	}
	
	static Comparator<MyInteger> comp = new Comparator<MyInteger>() {
		@Override
		public int compare(MyInteger o1, MyInteger o2) {
			return o2.value-  o1.value;
		}
	};
}

class MyInteger {
    int value;

	public MyInteger(int value) {
		this.value = value;
	}
}
```

보면 알겠지만, Comparator는 익명객체로 여러개를 생성할 수 있지만, Comparable의 경우 compareTo 하나 밖에 구현할 수 없다.  

그렇다보니, 보통은 Comparable은 여러분이 비교하고자 하는 가장 기본적인 설정(보통은 오름차순)으로 구현하는 경우가 많고, Comparator는 여러개를 생성할 수 있다보니 특별한 정렬을 원할 때 많이 쓰인다.  

쉽게 말해 Comparable은 기본(default) 순서를 정의하는데 사용되며, Comparator은 특별한(specific) 기준의 순서를 정의할 때 사용된다는 것이다.  

이를 이용하여 다음과 같이 복합적으로도 구현 하여 각기 다른 정렬을 할 수도 있다.  

```java
import java.util.Arrays;
import java.util.Comparator;

public class Test {
	public static void main(String[] args) {
		Student[] arr = new Student[9];
		
		arr[0] = new Student(3, 70);	// 3반 70점
		arr[1] = new Student(1, 70);	// 1반 70점
		arr[2] = new Student(1, 50);	// 1반 50점
		arr[3] = new Student(2, 60);	// 2반 60점
		arr[4] = new Student(2, 80);	// 2반 80점
		arr[5] = new Student(1, 30);	// 1반 30점
		arr[6] = new Student(2, 70);	// 2반 70점
		arr[7] = new Student(3, 90);	// 3반 90점
		arr[8] = new Student(3, 60);	// 3반 60점
		
		Student[] arr2 = arr.clone();	// 정렬 테스트를 위한 arr 객체 복사
		Student[] arr3 = arr.clone();	// 정렬 테스트를 위한 arr 객체 복사
		
		System.out.println("(c, s) -> (classNum, score)");
		// 정렬 이전
		System.out.print("정렬 전 : ");
		for(Student v : arr) {
			System.out.print(v);
		}
		System.out.println();
 
		Arrays.sort(arr);	// Comparable 사용
      
		System.out.print("\n학급 오름차순 정렬(같을 경우 성적 내림차순) : ");
		for(Student v : arr) {
			System.out.print(v);
		}
		System.out.println();
		
		Arrays.sort(arr2, comp1);	// Comparator 사용 
		
		System.out.print("\n학급 오름차순 정렬(같을 경우 성적 오름차순) : ");
		for(Student v : arr2) {
			System.out.print(v);
		}
		System.out.println();
		
		Arrays.sort(arr3, comp2);	// Comparator 사용
		
		System.out.print("\n성적 내림차순 정렬(같을 경우 학급 오름차순) : ");
		for(Student v : arr3) {
			System.out.print(v);
		}
		System.out.println();
		
	}
	
	static Comparator<Student> comp1 = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			
			// 만약 학급이 같다면 성적을 기준으로 "오름차순"으로 정렬한다.
			if(o1.classNum == o2.classNum) {
				return o1.score - o2.score;
			}
			return o1.classNum - o2.classNum;	// 학급 기준 오름차순으로 정렬한다.
		}
	};
	
	static Comparator<Student> comp2 = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			
			// 만약 성적이 같다면 학급을 "오름차순"으로 정렬한다.
			if(o1.score == o2.score) {
				return o1.classNum - o2.classNum;
			}
			return o2.score - o1.score;	// 성적을 내림차순으로 정렬한다.
		}
	};
}

class Student implements Comparable<Student> {
	int classNum;
	int score;
	
	public Student(int classNum, int score) {
		this.classNum = classNum;
		this.score = score;
	}
	
	@Override
	public int compareTo(Student o) {
		// 만약 학급이 같다면 성적을 기준으로 "내림차순"으로 정렬한다.
		if(this.classNum == o.classNum) {
			return o.score - this.score;
		}
		return this.classNum - o.classNum;	// 학급 기준 오름차순으로 정렬한다.
	}
	
	@Override
	public String toString() {
		return "("+classNum + ", " + score + ")  ";
	}

}
```

출처 : https://st-lab.tistory.com/243