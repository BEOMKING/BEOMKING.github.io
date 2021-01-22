---
title:  "자바 공부"
categories: 
- Java
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-01-13
last_modified_at: 2021-01-08 23:30
---
자바 복습할 겸 나중에 간편하게 보기 위해

## IDE 단축키

- **인텔리제이 단축키**
    - `[Ctrl+D]` 라인을 아래로 복제한다. 
    - `[Ctrl+/]` 해당 라인을 주석처리한다. 한번 더 누르면 주석을 해제한다.
    - `[Ctrl+Shift+/]` 선택영역을 블록 주석처리한다.
    - `[Ctrl+Shift+Enter]` 현재 구문을 완성한다. 보통 괄호나 세미콜론을 추가해준다.
    - `[Alt+Enter]` 해당 오류 위에 커서를 놓고 Alt+Enter를 누르면 문제에 대한 수정 제안 목록이 표시됩니다.
    - `[F2]` 에디터에서 오류와 경고 사이를 이동할 때 마우스를 사용하고 싶지 않다면, F2를 사용하여 다음 오류, 경고 또는 제안으로 점프할 수 있습니다.
    - `[Alt+1]` 도구 창을 열 때도 Alt+1를 사용하면 프로젝트 창이 열리고 거기에 포커스가 위치합니다.
    - `[Ctrl+E]` 사용해 최근 실행했던 파일을 확인할 수 있습니다. 그러면, 방향 키를 사용해 이동할 수 있는 최근 파일 상자가 표시됩니다.
    - `[Ctrl+B]` 필드 위에서 이 단축키를 누르면 커서가 해당 필드의 선언으로 이동합니다. 클래스 이름 위에서 누르면 해당 클래스 파일로 이동합니다.
    - `[Alt+F7]` 선언을 검색하는 대신 어떤 항목이 사용된 위치를 검색하고 싶을 때가 자주 있습니다.
    - `[Ctrl Ctrl]` 어디에서든 Ctrl 키를 두 번 눌러서 어떤 항목이든 실행할 수 있습니다. 
    - `[Ctrl+Alt+L]` 사용하여 해당 프로젝트의 표준에 맞도록 현재 파일의 서식을 지정할 수 있습니다.
    - `[Ctrl + Shift + Alt + T]` Refactor this
    - `[Alt + Shift + 왼클릭]` 멀티 커서
    
- **이클립스 단축키**
  - `["main" + Ctrl + Space]`  public static void main
  - `["sysout" + Ctrl + Space]`  system.out.println
  - `[Ctrl + Shift + "O"]` 라이브러리 임포트
  - `[Alt + Shift + "S"]` getters & setters
  - `[Alt + Shift + "R"]` 같은 단어 커서

**JVM** 

자바의 가상 머신, 자바가 어느 곳에서든 실행되게 해주는 것을 보장

코드를 배포하려 할 때 서버는 JRE가 설치 되어있으면 된다. JDK는 필요 X

**CLI 환경 자바 실행**

`C:\SSAFY\test> javac -d . HelloWorld.java`

`C:\SSAFY\test> java com.ssafy.HelloWorld`

**IDE를 사용하는 이유** 

컴파일 과정이 편리해지고 하나의 개발 툴로 여러 프로젝트를 관리할 수 있다.

10001001 -> -128 + 8 + 1 = -119

## 문법

```java
public class Test {
  public static void main(String[] args) {
    final int i = 0;
    System.out.println(i);

    i = 10;
    System.out.println(i);
  }
}
// 에러 상수는 변하지 않는다

public class Test {
  public static void main(String[] args) {
    // A         형 변환 에러 발생
    {
      int i = 10;
      byte b = i;
    }

    // B                 
    {
      byte b = 10;
      int i = b;
    }
  }
}

public class Test {
  public static void main(String[] args) {

    int k = 66;
    char c = (char) k;
    System.out.println(c);         // B

    c = 'A';
    k = c;
    System.out.println(k);         // 65

    int i = 10 / 3;
    System.out.println(i);         // 3

    float f = 10 / 3;
    System.out.println(f);         // 3.0

    float f2 = 10f / 3F;
    System.out.println(f2);        // 3.3333333

    double d = 10d / 3D;
    System.out.println(d);         // 3.33333333333333335

    System.out.println( ( 10 / 3 ) * 3 );  // 9
  }
}

public class Test {
  public static void main(String[] args) {

    // bit and
    System.out.println("3 & 4 = " + (3 & 4));
    //   0011
    // & 0100
    // ------
    //   0000

    // bit or
    System.out.println("3 | 4 = " + (3 | 4));
    //   0011
    // | 0100
    // ------
    //   0111

    // bit exclusive
    System.out.println("3 ^ 4 = " + (3 ^ 4));
    //   0011
    // ^ 0100
    // ------
    //   0111

    // bit not
    System.out.println(" ~ 4 = " + (~ 4));
    //   0100
    // ~
    // ------
    //   1011
  }
}

public class Test {
  public static void main(String[] args) {

    // <<
    System.out.println("1 << 2 = " + (1 << 2));
    //    .... 0000 0001
    // << .... 0000 0100

    System.out.println("3 << 3 = " + (3 << 3));
    //    .... 0000 0011
    // << .... 0001 1000


    // >>
    System.out.println("1 >> 2 = " + (1 >> 2));
    //	  .... 0000 0001
    // >> .... 0000 0000

    System.out.println("-16 >> 2 = " + (-16 >> 2));
    //	  .... 1111 0000
    // >> .... 1111 1100

    // >>>
    System.out.println("7 >>> 2 = " + (7 >>> 2));
    //	   0000 .... 0000 0111
    // >>> 0000 .... 0000 0001

    System.out.println("-5 >>> 24 = " + (-5 >>> 24));
    //	   1111 1111 1111 1111 1111 1111 1111 1001
    // >>> 0000 0000 0000 0000 0000 0000 1111 1111
  }
}

// 자바의 랜덤 이용

public class Test {
  public static void main(String[] args) {
    int N = 6;
    // Math.random() 0.0 < ? < 1.0
    System.out.printf( "%3d", (int) (Math.random()*N) + 1 );
    
    // java.util.Random 0 <= ? <= N - 1
    java.util.Random generator = new java.util.Random();
    System.out.printf( "%3d", generator.nextInt(N) + 1 );
    
    // %
    System.out.printf( "%3d", ( (int) (Math.random()*100) % N ) + 1 );
  }
}

public class Test {
  public static void main(String[] args) {
    char C  = 'A';

    switch( C ) {
      case 'A' :  // do nothing
        break;
      case 'B' :  // do nothing
        break;
      case 65  :  // do nothing
        break;
    }
  } // 실행 결과 오류 Duplicate Case
}

public class Test {
  public static void main(String[] args) {
    int num = 4;
    // 앞에 것이 틀려도 다음 것을 확인
    if( num == 3 & isEven(num) ) {
      System.out.println("3 !!");
    }
  }
  
  static boolean isEven(int num) {
    if( num % 2 == 0 ) {
      System.out.println("Even !!");
      return true;
    }else {
      return false;
    }
  }
}

public class Test {
  public static void main(String[] args) {
    int num = 4;
    // 앞에 것이 틀리면 다음 것을 진행 x
    if( num == 3 && isEven(num) ) {
      System.out.println("3 !!");
    }
  }
  // 위 메인이 false여서 아래가 실행 되지 않음
  static boolean isEven(int num) {
    if( num % 2 == 0 ) {
      System.out.println("Even !!");
      return true;
    }else {
      return false;
    }
  }
}
```
`&, | 끝까지 확인`<br>
`A & B & C   A B C 모두 판단`<br>
`A | B | C   A B C 모두 판단`<br>

`&&, || 판단하면 멈춤 `<br>
`A && B && C   A B C 순으로 판단, 하나라도 거짓이면 중단`<br>
`A || B || C   A B C 하나라도 참이면 중단`<br>
```java
public class JavaStudy {
  public static void main(String[] args) {
    boolean ss = true;
    if (ss) {
        System.out.println("됨"); // 세미 부울린 지원 x (0 거짓 그외 참), 부울린 값만 가능
    }

    short num1 = 2;
    short num2 = 3;
    // num1 +num2 = 에러  -> 자바 기본 연산의 최소 단위는 int

    // 자바는 짧은 단위 논리 연산을 진행
    int i1 = 3;
    int i2 = 4;
    boolean flagi2 = i1 > 4 && ++i2 > 3; // i2는 5가 되어야 할 것 같지만 실제로는 4이다. 앞의 값이 이미 false이기 때문에 뒤는 연산하지 않는다.
    boolean flagi3 = i1 > 4 || ++i2 > 3; // 이건 5가 된다. or 연산이기 때문
  
    String str = "ssafy";
    str += "java"; // != "java" + str

    // shift 연산자
    // i1 = i1 * 8
    // 00000011 -> 00011000
    
    // i1 <<= 35; 오버플로우가 난다고 생각 할 수 있으나 % 32를 해서 모듈러 연산을 시킨다 결과는 그대로

    // i1 = i1 * 1024; // i1을 1024번 실행
    // i1 = 1024 * i1; // 1024를 i1번 실행
    // 자바 컴파일러가 큰 수를 작은 수만큼 반복 더하기한다. 그래서 실제로 속도 차이는 안나지만 계산 방식은 저렇다.

    Scanner sc = new Scanner(System.in); // 자바의 입력 방법 스캐너
    int num1 = sc.nextInt();
    String s1 = sc.next(); // python의 input().split()이 필요없음

    String s = null;
    s = "hawidd";
    if(s != null && s.length() > 5) {
        System.out.println("됨");
    }
    System.out.println("항상 됨");

    Scanner sc = new Scanner(System.in);
    int num = sc.nextInt();
    while(true) {
        if(num > 30) {   // 30 이상 값오면 종료되는 while 문
            break;
        }
        num += sc.nextInt();
    }
    System.out.println(num);
    
    Scanner sc = new Scanner(System.in);
    int size = sc.nextInt();
    int[] map = new int[size];
    System.out.println(map.length);
    // 배열은 생성과 함께 초기값이 할당
    
    System.out.println(map[2]);
    
    //자바로 무한히 큰 수 입력 받고 합 출력하기 BigInteger
    Scanner sc = new Scanner(System.in);
    BigInteger A = new BigInteger(sc.next());
    BigInteger B = new BigInteger(sc.next());

    BigInteger C = A.add(B);
    System.out.println(C);
  }
}
```
- `str.charAt(i)` str의 i번째 인덱스 리턴
- `Arrays.toString(arrayname)` 배열 내용 출력

- `**for-each with Array**`
  -가독성이 개선된 반복문으로, 배열 및 Collections에서 사용
  -index 대신 직접 요소에 접근하는 변수 제공
  -naturally read only 직접 요소를 변경 x
  -: 사용
  -파이썬의 반복문과 비슷

- `**배열은 불변**`
  -최초 메모리 할당 이후 변경 x
  -개별 요소를 다른 값으로 변경은 가능, 삭제는 x
  -크기 변경 x
```java
public class Test {
  public static void main(String[] args) {
    int N = 6;

    int [] resultArray = new int[5];

    for( int i=0; i<resultArray.length; i++ ) {
        resultArray[i] = (int)(Math.random()*N) + 1;
    }

    for( int x : resultArray ) {
        System.out.println(x);
    }
  }
}

public class Test {
  public static void main(String[] args) {
    int[] intArray = { 3, 27, 13, 8, 235, 7, 22, 9, 435, 31, 54 };

    int min = Integer.MAX_VALUE;
    int max = Integer.MIN_VALUE;

    for (int i = 0; i < intArray.length; i++) {
        min = Math.min(min, intArray[i]);
        max = Math.max(max, intArray[i]);
    }

    System.out.println("Min : " + min + " Max : " + max);
  }
}

public class Test {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    char[][] grid = new char[4][4];

    int sum = 0;

    for( int i=0; i<4; i++ )
        for( int j=0; j<4; j++ )
            grid[i][j] = sc.next().charAt(0);

    for( int i=0; i<4; i++ )
        for( int j=0; j<4; j++ )
            if( grid[i][j] == 'X') {
                if( j-1 >= 0 && grid[i][j-1] != 'X' ) sum += grid[i][j-1] - '0';
                if( j+1 <  4 && grid[i][j+1] != 'X' ) sum += grid[i][j+1] - '0';
            }

    System.out.println(sum);
    sc.close();
  }
}

public class Test {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    char[][] grid = new char[4][4];
    boolean[][] used = new boolean[4][4];

    int sum = 0;

    for( int i=0; i<4; i++ )
      for( int j=0; j<4; j++ )
          grid[i][j] = sc.next().charAt(0);

    for( int i=0; i<4; i++ )
      for( int j=0; j<4; j++ )
        if( grid[i][j] == 'X') {
          if( i-1 >= 0 && grid[i-1][j] != 'X' && ! used[i-1][j] ) {
              sum += grid[i-1][j] - '0';
              used[i-1][j] = true;
          }
          if( i+1 < 4 && grid[i+1][j] != 'X' && ! used[i+1][j] ) {
              sum += grid[i+1][j] - '0';
              used[i+1][j] = true;
          }
          if( j-1 >= 0 && grid[i][j-1] != 'X' && ! used[i][j-1] ) {
              sum += grid[i][j-1] - '0';
              used[i][j-1] = true;
          }
          if( j+1 < 4 && grid[i][j+1] != 'X' && ! used[i][j+1] ) {
              sum += grid[i][j+1] - '0';
              used[i][j+1] = true;
          }
        }
    System.out.println(sum);
    sc.close();
  }
}

// 100미만 양의 정수들 중 0이 입력 되면 0을 제외하고 입력된 정수의 십의 자리 숫자 개수 작은수 부터 출력
public class DigitTest1 {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    ArrayList<Integer> array = new ArrayList<>(); // 몇 개의 정수가 주어지는지 알 수 없음
    int[] tenarray = new int[10]; // 십의 자리 숫자를 저장할 배열

    while(true) {
        int num = sc.nextInt();

        if(num == 0) break;
        array.add(num);
    }

    for(int i: array) {
        int ten = i / 10;
        tenarray[ten] += 1;
    }

    for(int i = 0; i < tenarray.length; i++) {
        if(tenarray[i] != 0) System.out.println(i + ": " + tenarray[i] + "개");
    }
  }
}
```
- char to int에서 숫자를 넘기는 경우 '0'을 해야 한다. (아스키코드값을 넘기기 때문)

- char to string에서 char배열인 경우 `String.valueOf`를 사용하면 char배열 글자가 서로 붙어서 문자열로 넘어간다.

- String to char에서 문자 길이가 1이라면 `charAt`으로 넘기면 된다.

- String to char에서 문자 길이가 2이상이라면 `toCharArray`를 이용해 char배열로 넘긴다.

- **java.util.Scanner**
  - `.hasNext()`
  - `.next()`
  - `.hasNextXXX()`
  - `.nextXXX()`
  - `.nextLine()`
  - `.next().charAt(i)`

- **`Call by value`**
- **`Call by reference`** pass by value

```java
public class Call {
  public static void main(String[] args) {
    int a = 10;
    f1(a);                          // 20
    System.out.println(a);          // 10
    
    int[] arr = new int[]{1, 2, 3, 4, };
    f2(arr);
    System.out.println(Array.toString(arr)); // 200 변경   주소 참조
  }
  static  void f2(int[] brr){
      brr[2] = 200;
      System.out.println(Array.toString(brr)); // 200
  }
  static void f1(int a){              // 값만 전달
      a = 20;
      System.out.println(a);     
  }
  
  // 문자를 숫자로
  ch = input.CharAt(i);
  idx = Integer.parseInt(new Character(ch).toString());
  idx = ch - '0';
  idx = ch - 48;                     
}
```
구획 당 하나의 빌딩을 세울 수 있고, 빌딩 B, 공원 조성단지는 G<br>
인접한 구획에 공원 조성 단지 G가 있다면 2 층 높이로 세울 수 있고,<br>
인접한 구획에 공원 조성 단지 G가 없다면 현 위치의 가로 위치에 있는 빌딩구획 B 와 세로 위치의 빌딩 구획 B 의 수를 더한 크기<br>
가장 높이 세울 수 있는 빌딩은 몇 층인가?<br>
```java
public class Solution {
  public static void main(String[] args){
    Scanner sc = new Scanner(System.in);
    int N = sc.nextInt();
    char[][] city = new char[N][N];
    int[][] height = new int[N][N];
    int Max = 0;

    for(int i = 0; i < N; i++) {
      for(int j = 0; j < N; j++) {
          // char 형으로 입력 받기 (자바는 입출력이 어려운듯..)
        city[i][j] = sc.next().charAt(0);
      }
    }

    for(int i = 0; i < N; i++) {   
        // 위에 작성한 &&과 &에 차이를 공부했는데 여기서 반대로 써서 시간을 오래 썼다.
      for(int j = 0; j < N; j++) { 
          // 반대로 작성하면 city[0][0]의 경우 city[-1][-1]이 인덱스 에러가 나서 다음 것들이 진행 되지 않는다.
        if(city[i][j] == 'B') {
          int count = 0;
          if(i - 1 >= 0 && j - 1 >= 0 && city[i - 1][j - 1] == 'G') {
            height[i][j] = 2;
            continue;
          }
          else if(i - 1 >= 0  && city[i - 1][j] == 'G') {
            height[i][j] = 2;
            continue;
          }
          else if(i - 1 >= 0 && j + 1 < N  && city[i - 1][j + 1] == 'G') {
            height[i][j] = 2;
            continue;
          }
          else if(j - 1 >= 0  && city[i][j - 1] == 'G') {
            height[i][j] = 2;
            continue;
          }
          else if(j + 1 < N  && city[i][j + 1] == 'G') {
            height[i][j] = 2;
            continue;
          }
          else if(i + 1 < N && j - 1 >= 0  && city[i + 1][j - 1] == 'G') {
            height[i][j] = 2;
            continue;
          }
          else if(i + 1 < N  && city[i + 1][j] == 'G') {
            height[i][j] = 2;
            continue;
          }
          else if(i + 1 < N && j + 1 < N  && city[i + 1][j + 1] == 'G') {
            height[i][j] = 2;
            continue;
          }
          for(int k = 0; k < N; k++) {
            if(city[i][k] == 'B') {
              count += 1;
            }
            if(city[k][j] == 'B') {
              count += 1;
            }
          }
          count -= 1;
          if(Max < count) {
            Max = count;
          }
        }
      }
    }
    System.out.println(Max);
  }
}
```

## 클래스

- **Object-Oriented Programming (OOP)**
  객체 지향 프로그래밍
  프로그램을 단순히 데이터와 처리 방법으로 나누는 것이 아니라, 프로그램을 수많은 '객체(object)'라는 기본 단위로 나누고 이들의 상호작용으로 서술하는 방식
  
- **캡슐화** 하나의 클래스 안에 데이터와 기능을 담아 정의하고, 중요한 데이터나 복잡한 기능 등을 숨기고, 외부에서 사용에 필요한 기능만을 공개하는 것
- **상속** 객체 정의 시 기존에 존재하는 객체의 속성과 기능을 상속받아 정의하는 것
- **다형성** 같은 타입, 같은 기능의 호출로 다양한 효과를 가져오는 것
- **추상화** 현실 세계에 존재하는 객체의 주요특징을 추출하는 과정

- **클래스**
  - 구체적인 객체들을 분석해 보고 공통적인 내용들을 추상화해서 표현한 것
  - 객체를 만들어 내기 위한 설계도 혹은 틀 
  - 연관되어 있는 Variable와 Method의 집합
  
- **객체(Object)** 
  - 실세계에 존재하는 사물
  - 상태(state)와 행동(behavior)을 가짐
  - 객체의 상태(속성, 특성)을 표현할때 멤버변수라고 표현할 수 있으며, 객체의 행동(기능)을 메소드라고 표현

- **인스턴스**
  - 설계도를 바탕으로 소프트웨어 세계에 구현된 구체적인 실
  - 실체화된 Instance는 메모리에 할당
  
- `클래스` 내부
  휴대폰(Object = 사물)
  명사 : 이름, 색상, 제조사, 가격 등등
  동사 : 켜다, 크다, 할부원금() + set, getter 메소드, 생성자 등등
  
- `this` 자기 자신의 객체를 나타냄

- **생성자(Constructor)**
 - 객체가 생성되면서 선행되어야 될 초기화 작업을 실시
 - 특수 메소드, 반환타입이 없고, 클래스 이름과 철자가 같은 메소드를 사용할 경우 new 키워드 다음에 사용
 - 선언하지 않을 경우 디폴트 생성자가 컴파일러에 의해 생성
 - String 같은 클래스도 사용자가 정의하지 않아도 컴파일러에 의해 자동 주입

- 멤버 변수
  - Ex) String name, int age
  - 정적
  
- 메소드
  - 수행 또는 처리되는 code block을 가질 수 있고 내부, 외부에서 호출 가능
  - 동적
  
- **캡슐화**
  - Setters
    변수 값을 외부에서 마음대로 접근한다면 문제가 발생, 그렇기 때문에 메소드를 통해 데이터를 변경하는 방법을 선호
```java
public void setName(String name) {
        this.name = name;
    }
```
  - Getters
    외부에서 값을 읽고자 할 때 직접 읽지 않고 별도의 메소드를 사용하는 방식
```java
public String getName() {
        return name;
    }
```
  Setters & Getters 통해 외부에서 모든 멤버변수에 접근할 수 있기 때문에 직접 노출하는 것과 동일해 보인다.
  복잡한 클래스의 경우 외부에 노출하지 않는 멤버변수와 특정한 경우를 설정할 수 있습니다.

  Setters & Getters 구조를 가져가야만, 이후 처리 로직이 변경되는 경우 쉽게 대응할 수 있습니다.
  
  - 접근제어(Access Modifier
  ![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/accessmodifier.png){: .align-center}

- **객체의 배열**
```java
public class PhoneArrayTest {
  public static void main(String[] args) {
    Phone [] phoneArray = new Phone[5];
    
    for( int i=0; i<phoneArray.length; i++ ) {
        phoneArray[i] = new Phone(); // 각 배열에 객체를 할당
        phoneArray[i].setPrice(i*2000);
    }
            
    for( Phone phone : phoneArray ) {
        System.out.println( phone.getPrice() );
    }
  }
}
```
- **JVM 메모리**
  개념적으로 Class Area, Heap, Stack으로 나누어짐
  - Class Area for Class, Static, Method
  - Heap for Objects
  - Stack for Call
  
- **Garbage Collection**
  new 연산을 통해 메모리 할당을 수행하지만, 메모리 제거는 JVM이 알아서 처리
  Heap에 만들어진 객체 중 더 이상 참조되지 않은 것들을 대상으로 적절한 시첨에 GC 작업을 수행
  - 프로그래머는 GC에 직접 관여할 수 없다.
  - 자동으로 처리되는 점은 프로그래머 입장에서는 장점이지만 운영에서는 단점이 된다.
  - 불필요한 객체 생성을 지양한다.
  
- **String Class**

```java
public class StringTest {
  public static void main(String[] args) {
    int i1 = 10;
    int i2 = 10;
    
    String s1 = "Hello"; // 객체 상수로서 String Constant poll에 관리, 재사용됨 
    String s2 = "Hello"; // 재사용 되기 때문에 같은 공간을 가르킴
    String s3 = new String("Hello"); // 각각 new에 의해 Heap에 서로 다른 객체를 생성
    String s4 = new String("Hello");
    
    if( i1 == i2 ) { System.out.println("i1 i2 Same"); } // true
    if( s1 == s2 ) { System.out.println("s1 s2 Same"); } // true
    if( s3 == s4 ) { System.out.println("s3 s4 Same"); }
    if( s3.equals(s4) ) { System.out.println("s3 s4 Same"); } // true
	}
}
```

- **toString**
  - `+` 연산자 사용
    + 연산 개수당 String 객체가 새로 만들어져서 성능에 영향을 미침
    간단한 +의 나열은 컴파일러가 내부적으로 StirngBuilder를 사용해 처리하지만 Loop에서는 그렇지 않다.
      
  - StringBuilder 사용
    toString 메소드를 재정의해서 사용
  
- **final**
  - 클래스 앞에 선언
  - 메소드 앞에 선언
    인자는 전달 받되 가공, 변경 되지 않는다.
  - 변수 앞에 선언
    최종 변수를 의미함으로써 변경이 되지 않게 합니다.
    final 변수는 모두 대문자를 관습적으로 사용 (두 단어로 구성된 변수는 사이 _ 사용)
    멤버 변수로 사용할 때는 값을 초기화 해야함
```java
class Name {
  public static void main(String[] args) {
    final BJP bjp = new BJP();
    bjp.name = "BJP";
    bjp.name = "범진"; //o 내용 변경은 가능

    BJP bjp1 = new BJP();
    bjp = bjp1; // x 객체가 가르키는 장소는 변경 불가
  }
}
```
- **static**
  - 클래스(Inner, nested) 앞에 선언
```java
public class BJP{
    public static void main(String[] args) {
      bjp bjp1 = new bjp();
    }
  
    static class bjp{ // static이 없으면 위에서 못 사용
        
    }
}
```
  - 메소드 앞에 선언
    
  - 멤버변수 앞에 선언

```java
public class STATIC{
  public static void main(String[] args) {
      // 2            1
    SData s1 = new SData();
    SData s2 = new SData();
    SData s3 = new SData();//3 각 객체는 stack 메모리에 저장
    s1.a++; 
    s1.b++; 
    s2.a++;
    s2.b++;
    s3.a++;
    s3.b++;

    System.out.println("s1 a : " + s1.a + ", b : " + s1.b);
    System.out.println("s2 a : " + s2.a + ", b : " + s2.b);
    System.out.println("s3 a : " + s3.a + ", b : " + s3.b);
    System.out.println(s1 == s2);
    System.out.println(s1.b == s2.b);
    // 결과 3, 1
  }
}
class SData{
  static int a; // class 메모리에 저장
  int b; // heap 메모리에 각 객체마다 저장
  void bb(){
      
  } // 메소드는 class 메모리에 저장, 객체마다 사용한다해도 새로운 메소드가 아니라
      // class 메모리에 있는 메소드를 사용하고 인자만 heap 메모리에서 받아옴
}
```

```java
public class StaticMethodTest {
  int a = 10;
  public static void main(String[] args) {
    a = 40; // 불가능 
    SSData.aa(); // aa는 어느 객체에 사용해도 같으므로 클래스 이름. 을 사용 (인스턴스화 되지 않고 미리 사용 가능)
  
    SSData ssdata1 = new SSData();
    SSData ssdata2 = new SSData();
    SSData ssdata3 = new SSData();
    
    ssdata1.num = 1;
    ssdata2.num = 1;
    ssdata3.num = 10;

    System.out.println(ssdata1.num == ssdata2.num); //true
    System.out.println(ssdata1 == ssdata2);
    
    ssdata1.bb();
    ssdata2.bb();
    ssdata3.bb();
  }
}
class SSData{
  int num; // 2 (메모리 생성 순서)
  static int su; // 1
  static void aa() { // 1
    su = 1; // 가능 스태틱이니까
    num = 9; // 불가능
    ff(); // 가능
    bb(); // 불가능
  }
  static void ff() { // 1
      
  }
  void bb() { // 2
    su = 1; // 가능
    num = 9; // 가능
    System.out.println(this.num);
  }
}

public class FinalTest {
  public static void main(String[] args) {
    Car car1 = new Car();
    Car car2 = new Car();
    Car car3 = new Car();

    System.out.println(Car.getNum());
    System.out.println(car1.getSerialNumber());
    System.out.println(car2.getSerialNumber());
    System.out.println(car3.getSerialNumber());

    System.out.println(Car.getNum());
    System.out.println(Car.getNum());
  }
}
class Car{
  private static int num = 0;
  private int serialNumber;

  public Car() {
    num++;
    serialNumber = num;
  }

  public int getSerialNumber() {
    return serialNumber;
  }

  public void setSerialNumber(int serialNumber) {
    this.serialNumber = serialNumber;
  }

  public static int getNum() {
    return num;
  }

}

public class StaticTest1 {
  public static void main(String[] args) {
    Data1 data1 = new Data1();
    data1 = new Data1(1);
    data1 = new Data1();
  }
}
class Data1{
  int num1 = 10;
  static int num2 = 20;
  static void print() {
    System.out.println("print");
  }
  static{
    // 클래스가 메모리에 로딩되면 한번만 실행되는 블럭
    System.out.println("클래스 블럭");
  }
  {
    // 어떠한 인스턴스던지 무조건 실행 -> 인스턴스 블럭
    System.out.println("하위");
  }

  Data1(){
    System.out.println("셍성자1 " + num1 + " " + num2);
  }

  Data1(int num1){
    this.num1 = num1;
    System.out.println("셍성자2 " + num1 + " " + num2);
  }
}

public class StaticTest2 {
  public static void main(String[] args) {
    DataSam ds1 = new DataSam();
    DataSam ds2 = new DataSam();
    DataSam ds3 = new DataSam();

    ds1.a = 10;
    ds2.a = 20;
    ds3.a = 30;

    // 인스턴스 변수
    // ds3이 ds1의 a를 변화시키는 방법은 없다.

    ds1.b = 100;
    ds2.b = 200;
    ds3.b = 300;

    // static 변수
    // ds3이 ds1의 b를 변화시키는 방법이 있다.
    // 같은 변수 공유
  }

}
class DataSam{
  int a;
  static int b;

}
```
