---
title:  "자바 배열을 복사하는 방법들"
excerpt: 얕은 복사, 깊은 복사
categories: 
- Java
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-06-24
last_modified_at: 2021-06-24 20:37
---
알고리즘 문제를 풀다보면 배열을 복사해서 변경하고 그 값을 인자로 전달해야하는 경우가 있습니다.
간단하게 작성해보았습니다.

## 얕은 복사
얕은 복사(Shallow Copy) : 복사된 배열이나 원본배열이 변경될 때 서로 간의 값이 같이 변경됩니다.

```java
public class Shallow{
    public static void main(String[] args)  {
        int a[] = { 1, 2, 3, 4 };
        int b[] = a;
    }
}
```

a의 배열을 b배열에 대입하면 깊은 복사가 되지 않고 얕은 복사가 됩니다.
그렇기에 b배열의 값을 수정하여도 a배열까지 같이 수정되어버리는 상황이 나옵니다.

## 깊은 복사
깊은 복사(Deep Copy) : 복사된 배열이나 원본배열이 변경될 때 서로 간의 값은 바뀌지 않습니다.

```java
public class Deep{
    public static void main(String[] args)  {
        int a[] = {1, 2, 3, 4};
        int b[] = new int[a.length]; 
        for (int i = 0; i < a.length; i++) {
            b[i] = a[i];
        }
    }
}
```

for문을 이용해 일일히 값을 옮기는 방식을 주로 사용하였는데 간단하게 복사할 수 있는 여러 메소드가 있었습니다.

## 배열을 복사하는 여러가지 메서드

### Object.clone()

```java
public class clone{
    public static void main(String[] args)  {
        int a[] = {1, 2, 3, 4};
        int b[] = a.clone();
    }
}
```

Array.clone()을 사용하면 배열을 쉽게 복사할 수 있습니다. (깊은 복사) 가장 보편적인 방법입니다.

### Arrays.copyOf()

```java
public class copyOf{
    public static void main(String[] args)  {
        int a[] = {1, 2, 3, 4};
        int b[] = Arrays.copyOf(a, a.length);
    }
}
```

Arrays클래스는 배열을 조작할 수 있는 메소드를 가진 클래스입니다.
이 클래스 안에 있는 Arrays.copyOf()를 사용하면 배열의 시작점 ~ 원하는 length까지 배열의 깊은 복사를 할 수 있습니다.

### Arrays.copyOfRange()

```java
public class copyOfRange{
    public static void main(String[] args)  {
        int a[] = {1, 2, 3, 4};
        int b[] = Arrays.copyOfRange(a, 1, 3);
    }
}
```

Arrays.copyOf()는 배열의 처음~지정한 length까지 복사하는 메서드였다면 Arrays.copyOfRange() 메서드는 복사할 배열의 시작점도 지정할 수 있습니다.

### System.arraycopy()

```java
public class arraycopy{
    public static void main(String[] args)  {
        int[] a = { 1, 2, 3, 4 };
        int[] b = new int[a.length];
        System.arraycopy(a, 0, b, 0, a.length);
    }
}
```

`System.arraycopy()` 메서드는 지정된 배열을 대상 배열의 지정된 위치에 복사합니다.

## 2차원 배열의 깊은 복사

1차원 배열의 깊은 복사의 경우 위에서 소개한 메서드를 사용하면 쉽게 복사가 가능합니다.
하지만 2차원 배열의 경우 위의 메서드를 활용해도 깊은 복사가 되지 않습니다.

그 이유는 위와 같은 2차원 배열의 구조 a[x][y]에서 배열을 복사하는 메서드를 사용하게 되면 y좌표를 가리키는 주소 값만 있는 a[x] 부분만 깊은 복사가 되고 값이 있는 a[x][y]는 깊은 복사가 되지 않습니다.
그렇기에 2차원 배열을 복사하기 위해서는 for문을 돌리면서 값이 있는 a[x][y]를 일일이 복사해주어야 합니다.

### 이중 for문 활용

```java
public class Array_Copy{
    public static void main(String[] args)  {
        int a[][] = new int[3][3];
        int b[][] = new int[a.length][a[0].length];
	    
        for(int i=0; i<a.length; i++) {
            for(int j=0; j<a[i].length; j++) {
                b[i][j] = a[i][j];  
            }
        }
    }
}
```

2차원 객체 배열의 복사를 할 경우 arraycopy나 clone을 이용해서 복사할 수가 없습니다.
그렇기에 이중 포문을 복사하시려면 이중 for문을 돌면서 값을 일일이 옮겨주셔야 합니다.

### System.arraycopy 활용

```java
public class Array_Copy{
    public static void main(String[] args)  {
        int a[][] = new int[3][3];
        int b[][] = new int[a.length][a[0].length];
	    
        for(int i=0; i<b.length; i++){
            System.arraycopy(a[i], 0, b[i], 0, a[0].length);
        }
    }
}
```

이중 for문이 싫으시다면 for문을 돌려 System.arraycopy 메서드를 이용해 2차원 배열을 복사할 수 있습니다.
1차원 배열을 2차원 배열의 row 길이만큼 복사합니다.

출처 : https://coding-factory.tistory.com/548?category=758267