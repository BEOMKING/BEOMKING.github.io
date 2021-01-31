---
title:  "Exception"
excerpt: 예외 처리
categories: 
- Java
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-01-31
last_modified_at: 2021-01-31 02:09
---
## Exception
![image.png]({{ site.url }}{{ site.baseurl }}/assets/image/ExceptionClassHierarchy.png)
try - catch - finally
throws - 예외 발생 시 호출자에게 throw
try - catch - throw 예외 발생시 처리 코드를 작성하되, 새로운 예외 발생 후 전달
컴파일러는 if문을 빼고 컴파일
변수가 try와 catch 모두 영향을 미치면 지역 변수 초기화
try 문을 컴파일하지 않기때문에 지역 변수를 초기화 시키지않으면 에러

- Autocloseable interface
IO 관련 Class는 IO관련 예외가 발생하므로 try-catch를 자주 사용
이 때 작업이 끝나면 close를 해야하는데 보통 finally로 구현합니다. 이런 불필요한 구문을 제거하면서
try(close관련 코드){}식으로 자동 close 인터페이스를 활용할 수 있습니다.
  
```java
public class ExceptionTest {
  public static void main(String[] args) {
    FileInputStream fins = new FileInputStream("hello.txt");
    // #1 try-catch-finally
    FileInputStream fis = null;
    try {
      fis = new FileInputStream("hello.txt");
      System.out.println("오류가 발생하면 이 줄은 실행되지 않는다.");
    } catch (FileNotFoundException e) {
      System.out.println("Handing Exception : " + e.getMessage());
      e.printStackTrace();
    } finally {
          try {
            fis.close();
          }catch (Exception e) {
            e.printStackTrace();
          }
    }
  }
    // #2 throws
  public static void main(String[] args) throws FileNotFoundException {
    FileInputStream fins = new FileInputStream("hello.txt");
  }
    // #3 throw e
  public static void main(String[] args) throws IOException {
    FileInputStream fis = null;
    try {
      fis = new FileInputStream("hello.txt");
    }catch(FileNotFoundException e) {
      System.out.println("Handing Exception : " + e.getMessage());
      e.printStackTrace();
      throw new IOException(); // throw IOException 객체 to main()
    }finally {
      try {
        fis.close();
      }catch(Exception e) {
        e.printStackTrace();
      }
    }
  }
    //#4 use AutoCloseable
  public static void main(String[] args){
    try( FileInputStream fis  = new FileInputStream("hello.txt"); ) {
      // fis code
    }catch(IOException e) {
      System.out.println("Handing Exception : " + e.getMessage());
      e.printStackTrace();
    }
  }
}
```
- 상속 관계의 예외처리
어떤 코드 블럭 안에 상위 class의 예외와 하의 class의 예외가 동시에 발생했을때
```java
public class ExceptionTest4 {
    public static void main(String[] args) {
        // Not Runtime Exception
        FileInputStream fis = null;
        try {
            fis = new FileInputStream("hello.txt");			
        }catch(Exception e) {
            System.out.println("Handing Exception : " + e.getMessage());
        }catch(IOException e) {
            System.out.println("Handing Exception : " + e.getMessage());
        }catch(FileNotFoundException e) {
            System.out.println("Handing Exception : " + e.getMessage());
        } // FileNotFoundException 구문이 실행되야하나 Exception 구문이 포함하기 때문에 Exception구문을 실행하고 종료된다.
         // 그렇기 때문에 순서를 반대로 바꿔줘야한다.
        // Runtime Exception
        try {
            String s = null;
            s.length();
        }catch(RuntimeException e) {
            System.out.println("Handing Exception : " + e.getMessage());
        }catch(NullPointerException e) {
            System.out.println("Handing Exception : " + e.getMessage());
        }
    }
}
```
- 사용자 정의 예외

```java
public class NotCoronaException extends Exception {
    public NotCoronaException(String msg) {
        super(msg);
    }
}

public class Hospital extends Organization implements MedicalAction {
    @Override
    public void addPatient(Patient p) throws NotCoronaException {
        if (!p.isCorona()) throw new NotCoronaException("NotConora");
        cdc.getPatientList().add(p);
    }
}

public interface MedicalAction {
    void addPatient(Patient p) throws NotCoronaException;
    void removePatient(Patient p);
}

public class MainTest {
    public static void main(String[] args) {
        // 예외 체크 check
        Patient p3 = new Patient("환자3", 33, "010-3333-3333", "고열", "001", false);
        try {
            univHospital.addPatient(p3);
        }catch(NotCoronaException e) {
            System.out.println("등록하려는 환자는 Corona 환자가 아닙니다.");
        }
        cdc.printPatientList();
    }
}
```