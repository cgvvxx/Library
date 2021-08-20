# Java - class



## 1. 객체(Object) & 클래스(Class)

### 1.1 인스턴스, 필드, 메서드, 생성자

- 인스턴스 : 메모리에 만들어진 객체
- 필드(field) : 객체가 가지고 있는 속성(Attribute)
- 메서드(method) : 입력값(매개변수 또는 인자)를 받아 결과값을 리턴
  - 매개변수(Parameter) : 전달된 인자를 받아들이는 변수
  - 인자(Argument) : 어떤 함수를 호출시에 전달되는 값
  - 오버로딩(overloading) : 매개변수의 유형(타입)과 개수가 다르게 하여 같은 이름의 메서드를 여러 개 가지게 함

- 생성자 : 객체가 될 때 필드를 초기화

  - 모든 클래스는 인서턴스화 될때 생성자를 사용
  - 생성자가 없으면 매개변수가 없는 기본생성자로 자동생성
  - 오버로딩(overloading)을 통해 여러 개의 생성자를 가질 수 있음

  ```java
  // 클래스 생성
  public class "클래스명"{    
      
      public "클래스명"("매개변수"){ // 생성자 생성
          ...
      }
      
      public "return 타입" "method명"("매개변수") { // 메서드 생성
          ... // return이 없는 경우 return type = void
          
          return ...;
      }
  }
  
  
  //EX
  public class Car{ 
      // 클래스 생성
      String name; // 필드 생성
      int number;
      
      public Car(String s){ // 생성자
          name = s
      }
      
  }
  
  public class CarEx{
      public static void main(String[] args){
      	Car c1 = new Car("소방차"); // 객체 생성 (인스턴스)
          Car c2 = new Car("구급차"); // c2라는 변수가 생성된 객체(인스턴스)를 참조함
         
          c1.number = 1234;
          c2.number = 9876;
           
      }
  }
  ```



### 1.2 상속

- 부모가 가지고 있는 것을 자식이 사용 가능

- 오버라이딩(overriding) ; 부모가 가지고 있는 메서드와 같은 메서드를 자식이 가지고 있는 것

  - 항상 자식클래스에서 정의된 메서드가 호출됨
  - 부모 메서드를 호출하려면 super 키워드를 사용

  ```java
  public class Bus extends Car{ // Car class가 Bus class의 부모 class
  	...
  }
  ```



### 1.3 this & super

- this : 객체 자신을 참조

- super : 자식 class에서 부모 class 객체를 참조, super()는 부모의 생성자

  ```java
  public class Car{
      String name;
      int number;
  
      public Car(String name){
          this.name = name;
      }
  }
  
  
  public class Car{
      public Car(String name){
          System.out.println(name + " 을 받아들이는 생성자입니다.");
      }
  }
  public Bus(){
      super("소방차"); // 문자열을 매개변수로 받는 부모 생성자를 호출하였다.
      System.out.println("Bus의 기본생성자입니다.");
  }
  ```



### 1.4 scope & static

- scope : 주어진 변수가 사용 가능한 범위
  - local variable은 선언된 중괄호에서만 유효
- static (main은 static한 메서드)
  - static한 필드(필드 앞에 static 키워드를 붙힘)나, static한 메소드는 Class가 인스턴스화 되지 않아도 사용 가능
  - static 내부에서 static하지 않은 필드 사용 가능 X
  - static한 변수는 공유됨



### 1.5 객체 참조

- 자바의 변수 타입

  - 기본형 : bolean, char, byte, short, int, long, float, double  (클래스가 아님)
  - 참조형 : 기본형이 아닌 모든 타입

  ```java
  class ReferenceTypeExam {
      public static void main(String []args) {
          ReferenceTypeExam exam = new ReferenceTypeExam();
          
          //기본형 변수value1을 addOne에 전달합니다.
          int value = 10;
          exam.addOne(value);
          System.out.println("기본형 변수의 값을 다른 메소드에서 변경한 결과: " + value);
          
          //참조형 변수arr을 addOne에 전달합니다.
          int []arr = {10};
          exam.addOne(arr);
          System.out.println("참조형 변수의 값을 다른 메소드에서 변경한 결과: " + arr[0]);
      }
      
      
      public void addOne(int value) {
          value++;
      }
      
      public void addOne(int[] arr) {
          for(int i = 0; i < arr.length; i++){
              arr[i] ++;
          }
      }
  }
  
  >>
  기본형 변수의 값을 다른 메소드에서 변경한 결과: 10
  참조형 변수의 값을 다른 메소드에서 변경한 결과: 11
  
  /* 기본형 타입은 다른 메소드에 매개변수로 전달될때, 10이라는 값이 그대로 전달됩니다.따라서 addOne에서 1을 더하더라도 value라는 변수에는 아무 영향이 없습니다.
  
  하지만 참조형 타입은 다른 메소드에 매개변수로 전달될때, 변수의 주소가 전달됩니다. 예를들어 '몇번째 박스에 값이 있다'는 식으로 값이 들어있는 주소가 전달되는겁니다. 그럼 그걸 전달받은 메소드addOne에서는 그 박스에 가서 들어있는 값에 1 더합니다. addOne을 실행하고 나서 arr[0]을 확인해 볼 때도 같은 박스에 가서 값을 확인하기 때문에 값이 11로 변해있는겁니다. */
  ```



### 1.6 etc.

- 접근제어자 : public > protected > default > private
  - public : 모든 접근 가능
  - protected : 같은 패키지 또는 상속 받은 패키지에서 접근 가능
  - private : 자기 자신만 접근 가능
  - default : 자기 자신과 같은 패키지 내에서 접근 가능, 아무것도 쓰지 않았을 때
- 추상클래스 : abstract
  - 부모 클래스의 역할은 하지만 인스턴스 생성 X
  - 추상 메서드를 가지고 있으면 항상 추상 클래스
  - 추상 클래스를 상속받은 클래스는 추상 클래스가 갖고 있는 추상 메서드를 반드시 구현해야 함
- 클래스 형변환
  - 부모타입으로 자식객체를 참조하는 경우 부모가 가지고 있는 메서드만 사용 가능 > 자식 객체가 가지고 있는 메서드나 속성을 사용하고 싶은 경우 클래스 형변환 이용
  - 상속관계에 있는 경우만 형변환 가능
  - 부모타입으로 자식타입의 객체를 참조하는 경우 묵시적 형변환
  - 자식타입으로 부모타입의 객체를 참조하는 경우 명시적 형변환 해야함

- 내부클래스 : 클래스 안에 선언된 클래스 

  - 중첩클래스
  - 정적 중첩 클래스
  - 지역 중첩 클래스
  - 익명클래스

  - TBD ....

- 열거형(enum)

  - 변수에 특정 타입만 사용하고 싶은 경우 사용

  ```java
  public class EnumExam {
      public static final String MALE = "MALE";
      public static final String FEMALE = "FEMALE";
  
      public static void main(String[] args) {
          String gender1; // 옛날엔 이렇게
  
          gender1 = EnumExam.MALE;
          gender1 = EnumExam.FEMALE;
  
          Gender gender2; // 특정 두개의 값만 사용하고 싶을 때
  
          gender2 = Gender.MALE;
          gender2 = Gender.FEMALE;
      }
  }
  enum Gender{
      MALE, FEMALE;
  }
  ```



## 2. Classes

- String
  - new 연산자를 이용하지 않아도 인스턴스 생성 가능

    - 이 경우 상수 영역에서 인스턴스를 생성
    - 다른 변수가 같은 string 인스턴스를 가르킬 경우 같은 인스턴스를 참조
    - new 연산자로 변수에 할당하는 경우 다른 인스턴스를 참조

    ```java
    String str1 = "hello"; // new 연산자 없이 생성 가능, 일반적
    String str2 = new String("hello");
    // str2의 경우 클래스를 참조하는 변수(주소 할당)
    ```

  - methods

    - length() : 주어진 string 객체의 길이
    - concat("sth") : "주어진 string" + "sth"
    - substring(idx1, (idx2)) : idx1부터 (idx2 까지)

- Random

  ```java
  import java.util.Random;
  
  Random rand = new Random();
  rand.nextInt(N) // 0 <= # < N
  ```

  



## 3. Interface

- 인터페이스 

  - 가져야할 기능들만 일단 가지고 있는.. 추상클래스?
  - abstract > interface, extends > implements
  - 인터페이스를 가지고 있는 메서드가 하나라도 구현되지 않으면 해당 클래스는 추상클래스가 됨

  ```java
  public interface TV{
      public int MAX_VOLUME = 100;
      public int MIN_VOLUME = 0;
  
      public void turnOn();
      public void turnOff();
      public void changeVolume(int volume);
      public void changeChannel(int channel);
  }
  
  public class LedTV implements TV{
      public void on(){
          System.out.println("켜다");
      }
      public void off(){
          System.out.println("끄다");   
      }
      public void volume(int value){
          System.out.println(value + "로 볼륨조정하다.");  
      }
      public void channel(int number){
          System.out.println(number + "로 채널조정하다.");         
      }
  }
  ```

  - default 를 이용하여 interface 내부에서 메서드 구현 가능
  - static한 메서드 구현 가능
    - 인터페이스명.메서드명(); 와 같은 방법으로 static한 메서드 실행

  ```java
  public interface Calculator {
      public int plus(int i, int j);
      public int multiple(int i, int j);
      default int exec(int i, int j){
          return i + j;
      }
      public static int exec2(int i, int j){ 
          return i * j;
      }
  }
  ```

  









