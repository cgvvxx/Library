# Java - grammar



## 1. Data Type

- Data type : 기본형(Primitive type), 참조형(Reference type)

- Primitive type - 8개의 기본 변수 타입(클래스가 아님)

  - 논리형 - boolean
  - 문자형
    - char ; 16bit, ASCII코드
  - 정수형 - byte, short, int, long

    - byte ; 8bit
    - short ; 16bit
    - int ; 32bit (~2,147,483,647, 약 20억), default(기본형)
    - long ; 64bit
  - 실수형 - float, double

    - float ; 32bit
    - double ; 64bit, default(기본형)

  ```java
  // ... 
  float f = 10 / 3;
  System.out.print(f); // float
  System.out.print((10/3)*3); // int
  
  >> 3.0
  9
  ```

  

- Reference type

  - 미리 만들 수 없는 데이터를 별도의 공간(heap)에 표현하고 그 공간의 주소를 저장

- Type casting

  - primitive는 primitive끼리, reference는 reference끼리 형 변환 가능
  - implicit type casting ; 작은 크기의 타입은 큰 크기의 타입으로 자동으로 형변환
  - explicit type casting ; 큰 크기의 타입을 작은 크기의 타입으로 변경할 경우 직접 선언
    - 값 손실이 발생할 수 있음
    - byte < short < int < long < float < double
  - 연산시 두 개의 피연산자 중 큰 타입으로 형 변환 후 연산 진행
  
  ```java
  long var= 100;
  float fvar= var;
  long var= (long) favr;
  ```



## 2. Arrays

- 배열 선언, 배열 생성, 값 할당 

  ```java
  int[] arr;
  int arr[];
  
  // 객체이므로 new을 이용하여 생성, 0으로 초기화
  arr = new int[3];
  
  arr[0]=10;
  arr[1]=20;
  arr[2]=30;
  
  // 선언과 생성을 동시에 ******
  int[] arr=new int[5];
  //선언과 생성, 값할당을 동시에
  int[] arr=new int[] {1, 2, 3, 4, 5};
  ```

- 2차원 배열

  ```java
  int[][] arr=new int[3][4];
  
  // 각 배열의 리스트 크기를 지정안해도
  // 각 배열안에 리스트 할당시 항상 new로 생성!
  int[][] arr=new int[3][]; 
  arr[0] = new int[1];
  arr[0][0] = 10;
  
  // 선언과 생성, 값 할당을 동시에
  int[][] arr={{1},{1,2},{1,2,3}}; 
  ```




## 3. Grammars

- if

  ```java
  if (boolean_expression){
      statements;
  } else if(bolean_expression){
      statements;
  } else{
      statements;
  }
  ```

  - 3항 연산식

  ```java
  type var=(statements)? val1 : val2;
  ```

- switch

  - 해당하는 case부터 break를 만날 때 까지 모두 실행

  ```java
  switch (expr1) {
      case val1:
          statements;
          break;
      case val2:
          statements;
          break;
      default:
          statements;           
  }
  
  // 여러 케이스를 한 번에 설정 가능
  switch (expr1) {
      case val1:
      case val2:
          statements;
          break;
      case val3:
      case val4:
      case val5:        
          statements;
          break;
  }
  ```

- for

  ```java
  for (init_expr; boolean_textexpr; alter_expr){
      statements;
  }
  ```

  - for each : 배열에서 원소 순회

  ```java
  for (type var: vars){
      statements;
  }
  ```

- while

  ```java
  while (boolean_expr){
      statements
  }
  ```

  - do ~ while : 무조건 한번은 실행

  ```java
  do {
      statements
  } while (boolean_expr);
  ```



## 4. Exception

- try ~ catch ~ (finally)

- exception ; catch 대신 모든 예외를 받는 경우 또는 catch(Exception e)

  ```JAVA
  public class ExceptionExam {
      public static void main(String[] args) {
          int i = 10;
          int j = 0;
          try{
              int k = i / j;
              System.out.println(k);
          }catch(ArithmeticException e){
              System.out.println("0으로 나누기" + e.toString());
          }finally {
              System.out.println("무조건 실행.");
          }
      }
  }

- throw, throws

  - throws ; 메서드에서 오류가 발생했을 때 예외를 호출한 쪽에서 처리하도록 함
  - throw ; 강제로 오류 발생

  ```java
  public class ExceptionExam {   
      public static void main(String[] args) {
          int i = 10;
          int j = 0;
          try{
              int k = divide(i, j);
              System.out.println(k);
          }catch(IllegalArgumentException e){
              System.out.println("0으로 나누기");
          }           
      }
  
      public static int divide(int i, int j) throws IllegalArgumentException{
          if(j == 0){
              throw new IllegalArgumentException("0으로 나누기");
          }
          int k = i / j;
          return k;
      }   
  }
  ```

- 사용자 정의 Exception

  ```java
  public class "클래스이름" extends Exception {}
  ```

