---
title:  "순열과 조합"
excerpt: 알고리즘 이론
categories: 
- Java
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-02-08
last_modified_at: 2021-02-08 02:14
---
## 순열

```java
public class Permutation {
static int numbers[];
static boolean isselected[];

    static void permutation(int[] n, int r, int count){
        if(count == r){
            System.out.println(Arrays.toString(numbers));
            return;
        }
        for(int per : n){
            if(isselected[per] == true){continue;}
            numbers[count] = per;
            isselected[per] = true;
            permutation(n, r, count + 1);
            isselected[per] = false;
        }
    }

    public static void main(String[] args) {
        int n[] = new int[]{1, 2, 3, 4};
        int N = n.length;
        int r = 3;
        int count = 0;
        numbers = new int[r];
        isselected = new boolean[N + 1];
        permutation(n, r, count);
    }
}
```

## 조합
```java
public class Combination {
static int numbers[];

    static void combination(int N[], int r, int count, int start){
        if(count == r){
            System.out.println(Arrays.toString(numbers));
            return;
        }
        for(int i = start; i < N.length; i++) {
            numbers[count] = N[i];
            combination(N, r,count + 1, i + 1); // i + 1 -> i로 변경하면 중복 조합 
        }
    }

    public static void main(String[] args) {
        int N[] = new int[]{1, 2, 3, 4};
        int r = 3;
        int count = 0;
        numbers = new int[r];
        combination(N, r, count, 0);
    }
}
```

주사위를 던진 횟수 N과 출력형식 M을 입력 받아서 M의 값에 따라 각각 아래와 같이 출력하는 프로그램을 작성하시오.

M = 1 : 주사위를 N번 던져서 나올 수 있는 모든 경우 : 중복순열
M = 2 : 주사위를 N번 던져서 모두 다른 수가 나올 수 있는 모든 경우(순서의미) : 순열
M = 3 : 주사위를 N번 던져서 중복이 되는 경우를 제외하고 나올 수 있는 모든 경우 : 중복 조합
M = 4 : 주사위를 N번 던져서 나온 수들의 조합(순서 무관)이 모두 다른 수가 나올 수 있는 모든 경우 : 조합

```java
public class DiceTest {
    static int N, numbers[],totalCnt;
    static boolean isSelected[];
    
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        numbers = new int[N];
        isSelected = new boolean[7];
         
        int M = sc.nextInt();
        totalCnt = 0;
        switch (M) {
        case 1:
            dice1(0); // 중복순열
            break;
        case 2:
            dice2(0); // 순열
            break;          
        case 3:
            dice3(0,1); // 중복조합
            break;
        case 4:
            dice4(0,1); // 조합
            break;
    
        }
        System.out.println("총 경우의 수 : "+totalCnt);
    }
    
    public static void print() {
        for (int i : numbers) {
            System.out.print(i+" ");
        }
        System.out.println(); 
    }
    //M = 1 : 주사위를 N번 던져서 나올 수 있는 모든 경우 : 중복순열
    // nㅠr
    // 6ㅠ3= 6^3=216
    public static void dice1(int cnt) {
        if(cnt == N) {
            ++totalCnt;
            print();
            return;
        }
        for(int i=1; i<=6; ++i) {
            numbers[cnt] = i;
            dice1(cnt+1);
        }
    }
    //M = 2 : 주사위를 N번 던져서 모두 다른 수가 나올 수 있는 모든 경우 : 순열
    // nPr
    // 6P3 = 6*5*4 = 120
    private static void dice2(int cnt) {
        if (cnt == N) {
            ++totalCnt;
            print();
            return;
        }
    for (int i = 1; i <= 6; i++) {
    if (isSelected[i]) continue;
    numbers[cnt] = i;
    isSelected[i] = true;
    dice2(cnt + 1);
    isSelected[i] = false;
    }
    }//end dice2 :주사위를 N번 던져서 모두 다른 수가 나올 수 있는 모든 경우
    
    
    //M = 3 : 주사위를 N번 던져서 중복이 되는 경우를 제외하고 나올 수 있는 모든 경우 : 중복 조합
    // nHr = n+r-1Cr
    // 6H3 = 6+3-1C3=8C3=56
    public static void dice3(int cnt,int start) {
    if(cnt == N) {
    ++totalCnt;
    print();
    return;
    }
    
        for(int i=start; i<=6; ++i) { // 112  -->중복 121 211
            numbers[cnt] = i;
            dice3(cnt+1,i);//현재수와 같은 수부터 처리하도록 전달
        }
    }
    
    //M = 4 : 주사위를 N번 던져서 나온 수들의 조합(순서 무관)이 모두 다른 수가 나올 수 있는 모든 경우 : 조합
    // nCr
    // 6C3=6!/3!3!=20
    public static void dice4(int cnt,int start) {
    
        if(cnt==N) {
            ++totalCnt;
            print();
            return;
        }
        for (int i = start; i <= 6; i++) {
            numbers[cnt]=i;
            dice4(cnt+1,i+1);//현재수 다음 수부터 처리하도록 전달
        }
    }
}
```
```java
public class 백준_15664_N과M10 {
    static int N;
    static int M;
    static int numbers[];
    static int array[];
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    static void reversecombi(int start, int count) throws IOException {
        if(count == M){
            for(int num : numbers){
                bw.write(num + " ");
            }
            bw.newLine();
            return;
        }
        int num = 0;
        for(int i = start; i < N; i++) {
            if (num != array[i]) {
                numbers[count] = array[i];
                reversecombi(i + 1, count + 1);
                num = array[i];
            }
        }
    }

    public static void main(String[] args) throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        numbers = new int[M];
        st = new StringTokenizer(br.readLine());
        array = new int[N];

        for(int i = 0; i < N; i++){
            array[i] = Integer.parseInt(st.nextToken());
        }
        Arrays.sort(array);

        reversecombi(0, 0);

        bw.flush();
        bw.close();
    }
}

public class 백준_15665_N과M11 {
    static int N;
    static int M;
    static int numbers[];
    static boolean isselected[];
    static int array[];
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    static void overlapper(int count) throws IOException {
        if(count == M){
            for(int num : numbers){
                bw.write(num + " ");
            }
            bw.newLine();
            return;
        }
        int num = 0;
        for(int i = 0; i < N; i++){
            if(num != array[i]){
                numbers[count] = array[i];
//                isselected[i] = true;
                overlapper(count + 1);
                num = array[i];
//                isselected[i] = false;
            }
        }
    }
    public static void main(String[] args) throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        numbers = new int[M];
        isselected = new boolean[N];
        array = new int[N];
        st = new StringTokenizer(br.readLine());
        for(int i = 0; i < N; i++){
            array[i] = Integer.parseInt(st.nextToken());
        }
        Arrays.sort(array);

        overlapper(0);
        bw.flush();
        bw.close();

    }
}

```