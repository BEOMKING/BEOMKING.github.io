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
  }
}
// 실행 결과 오류 Duplicate Case

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

- **for-each with Array**
  -가독성이 개선된 반복문으로, 배열 및 Collections에서 사용
  -index 대신 직접 요소에 접근하는 변수 제공
  -naturally read only 직접 요소를 변경 x
  -: 사용
  -파이썬의 반복문과 비슷

- **배열은 불변**
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
- **`Call by reference`**

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

구획 당 하나의 빌딩을 세울 수 있고, 빌딩을 세울 수 있는 구획은 B 로 표시, 공원 조성단지는 G 로 표시되어 있다.
빌딩을 세울 때 인접한 구획에 공원 조성 단지 G 가 있다면 2 층 높이로 세울 수 있고,
인접한 구획에 공원 조성 단지 G 가 없다면 현 위치의 가로 위치에 있는 빌딩구획 B 와 세로 위치의 빌딩 구획 B 의 수를 더한 크기만큼 빌딩을 세울 수 있다.
가장 높이 세울 수 있는 빌딩은 몇 층인가?
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