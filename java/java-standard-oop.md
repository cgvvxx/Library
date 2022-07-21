# 자바의 정석 - 객체지향 프로그래밍

> "Java의 정석(3rd Edition) - 남궁성"을 공부하면서 정리한 내용입니다.

<br>
<br>

**Contents**

- [클래스와 객체](#1-클래스와-객체)

- [변수와 메서드](#2-변수와-메서드)

- [오버로딩](#3-오버로딩)

- [생성자](#4-생성자)

- [변수의 초기화](#5-변수의-초기화)

<br>
<br>

## 1. 클래스와 객체

### 1.1. 클래스와 객체, 인스턴스

- 클래스(Class) = 객체 (또는 객체를 정의해놓은 것, 객체의 틀)

  - 클래스는 객체를 생성하는데 사용되며, 객체는 클래스에 정의된 대로 생성됨
- 객체(Object) = 실제로 존재하는 것, 사물 또는 개념
- 예를 들어, 붕어빵 기계를 클래스라고 하면 붕어빵 기계로 만들어내는 붕어빵을 객체라고 볼 수 있다
- 또는 다음과 같이 Audi, Nissan, Volvo는 Car라는 클래스의 객체들이라고 볼 수 있다

<p align="center">
<img src="https://i.stack.imgur.com/lNUAA.png">
</p>

- 인스턴스(Instance) = 어떤 클래스로부터 만들어진 객체

  - 객체는 모든 인스턴스를 대표하는 포괄적인 의미이고, 인스턴스는 어떤 클래스로부터 만들어진 구체적인 객체 중 하나
  - "책상은 인스턴스다"보다는 "책상은 객체다"라는 표현이, "책상은 책상 클래스의 객체이다" 보다는 "책상은 책상 클래스의 인스턴스이다"라는 표현이 옳다

### 1.2. 객체의 구성요소 - 속성과 기능

- 속성(property) = 멤버변수(member variable), 특성(attribute) ..

- 기능(function) = 메서드(method), 함수(function) ...
  - 예를 들어, TV의 속성은 크기, 색상, 전원, 채널 ...  <=> int size
  - TV의 기능은 전원 온오프, 볼륨높이기, 볼륨줄이기 ...  <=> volumeUp() {...}
  - 이를 표현하는 소스 예제는 다음과 같다.

```java
class TV {
    
    int size;
    String color;
    boolean power;
    int channel;
    
    void power() {
        power = !power; 
    }
    
    void channelUp() {
        channel++;
    }
    
    void channelDown() {
        channel--;
    }
}

class TvTest {
    public static void main(String args[]) {
        Tv t = new Tv(); // Tv인스턴스 생성
        t.channel = 7;  //Tv변수 설정
        t.channelDown();  //Tv메서드 호출
        System.out.println("현재 채널은 " + t.channel + " 입니다.");
    }
}
```

- **235p ~ 237p 그림**

- 객체의 배열을 다루는 경우 다음과 같이 초기화하여야 한다.

```java
Tv[] tvArr = new Tv[100];

for(int i=0; i<tvArr.length; i++) {
    tvArr[i] = new Tv();
}
```

### 1.3. 클래스 - 사용자 정의 타입

- 만약 사용자가 시, 분, 초를 사용하여 여러 개의 시간을 표현하고자 한다면 다음과 같이 여러 개의 변수를 정의해 주어야 한다.

```java
int hour1, hour2, hour3;
int minute1, minute2, minute3;
float second1, second2, second3;
```

- 다루어야 하는 시간이 많아지는 경우 배열을 생성할 수도 있지만, 각각의 시, 분, 초가 서로 분리되어 있기 때문에 사용하기 어려워지게 된다. 이 때 다음과 같은 사용자 정의 타입의 클래스를 활용할 수 있다.

```java
class Time {
    int hour;
    int minute;
    float second;
}

Time[] t = new Time[3];
for(int i=0; i<t.length; i++) {
    t[i] = new Time();
} // Time 클래스를 통해 여러 개의 시간에 대해 시, 분, 초를 묶어서 관리할 수 있다
```

- 만약 시간에 대하여 다음과 같은 제약시간이 있다면 제어자와 메서드를 이용하여 코드를 작성할 수 있다.

  - 시, 분, 초는 모두 0보다 크거나 같다
  - 시는 24보다 작다
  - 분, 초는 60보다 작다

```java
public class Time {
    private int hour;
    private int minute;
    private float second;
    
    public int getHour(){
        return hour;
    }
    
    public int getMinute(){
        return minute;
    }
    
    public int getSecond(){
        return second;
    }
    
    public void setHour(int h){
        if (h < 0 || h >= 24) return;
        hour = h;
    }
    
    public void setMinute(int m){
        if (m < 0 || m >= 60) return;
        minute = m;
    }
    
    public void setSecond(float s){
        if (s < 0.0f || s >= 60.0f) return;
		second = s;
    }
}
```

<br>

## 2. 변수와 메서드

### 2.1. 변수의 종류

- 클래스변수 : 멤버변수 중 static이 붙은 변수, 클래스가 메모리에 올라갈 때 생성

  - 모든 인스턴스가 공통된 값(공통된 저장공간)을 가짐
  - 클래스가 메모리에 로딩 때 생성되므로, 인스턴스를 생성하지 않고도 사용 가능
  - public을 앞에 붙이면 같은 프로그램 내에서 어디서나 접근 가능한 전역변수로 사용 가능
- 인스턴스변수 : 멤버변수 중 static이 붙지 않은 변수, 인스턴스가 선언될 때 생성
  - 각각의 인스턴스마다 독립적인 값을 가짐
- 지역변수 : 특정 메서드 내부(메서드 영역)에 선언된 변수, 변수 선언문이 수행될 때 생성
  - 지역변수가 선언된 블럭(for문, while문 등) 내에서만 사용 가능하며, 블럭을 벗어나면 소멸됨

```java
class Variables {
    static int classVariable; // 클래스변수
    int instanceVariable; // 인스턴스변수
    
    void method() {
        int localVariable; // 지역변수
    }
}
```

### 2.2. 메서드

- 메서드(Method) : 특정 작업을 수행하는 일련의 문장을 하나로 묶은 것, 내부가 보이지 않는 블랙박스

- 메서드의 장점은 다음과 같다
  - 높은 재사용성
  - 중복된 코드의 제거
  - 프로그램의 구조화
- 반환타입, 메서드 이름, 매개변수 선언을 통하여 메서드를 선언하고 해당 블럭 내에 메서드 호출 시 수행할 코드를 작성하여 구현한다
  - 반환 값이 없는 경우 반환 타입은 void
  - 반환 타입이 void인 경우 "return;" 생략 가능 (컴파일러가 자동 생성해줌)
  - 반환 타입과 같은 타입(또는 자동 형변환이 가능한 타입)을 return 해야 함

```java
/* 반환타입 메서드이름 (타입 변수명1, 타입 변수명2, ...) {
    메서드 호출 시 수행할 코드
}*/

float divide(int x, int y) {
    if (y==0) { // 매개변수의 유효성 검사를 충분히 해주어야 함
        System.out.println("0으로 나눌 수 없음");
        return 0;
    }
    return x / (float)y;
}
```

- 클래스 메서드 : 메서드 중 static이 붙어있는 메서드

  - 객체를 생성하지 않고 호출 가능
  - 인스턴스와 관계없는 메서드(인스턴스 변수를 사용할 수 없다)
  - 인스턴스 메서드를 호출할 수 없고, 인스턴스 변수도 사용할 수 없음(클래스 메서드 생성 시점에 인스턴스 멤버가 존재하지 않을 수도 있기 때문이다)
- 인스턴스 메서드 : 메서드 중 static이 붙어있지 않은 메서드
  - 인스턴스 변수를 필요로 하는 메서드
  - 항상 클래스 메서드를 호출할 수 있고, 인스턴스 변수도 사용할 수 있음


```java
class MyMath {
    long a, b; // 인스턴스 변수
    
    long add(){ // 인스턴스 메서드
        return a+b;
    }
    
    static long add(long a, long b){ 
        // 클래스 메서드, 인스턴스 선언 없이 매개변수 만으로 호출 가능
        return a+b;
    }
}

class MyMathTest {
    public static void main(String args[]){
        System.out.println(MyMath.add(200L, 100L)); 
        // 인스턴스 생성 없이 클래스 메서드 호출 가능
        
        MyMath mm = new MyMath();
        mm.a = 200L;
        mm.b = 100L;
        System.out.println(mm.add());
        // 인스턴스 메서드이므로 인스턴스 변수 생성 이후에 호출 가능
    }
}
```

<br>

## 3. 오버로딩

- 오버로딩(Overloading) : 한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것

  - 메서드 이름이 같아야하고, 매개변수의 개수 또는 타입이 달라야 함 (반환 타입은 오버로딩에 관계 없음)
  - 예를 들어 println의 경우, 다음과 같이 오버로딩된 메서드들 중의 하나가 실행되는 것

    - void println()
    - void println(char x)
    - void println(char[] x)
    - void println(float x)
  - 매개변수에 가변인자를 사용하는 경우, 가변인자는 매개변수 중 가장 마지막에 선언해야 함 (가변인자는 내부적으로 배열을 사용)
    - 위의 함수를 아래 함수와 같이 대체할 수 있다
    - String concatenate(String ch, String s1, String s2, String s3) { ... }
    - String concatenate(String ch, String... args) { ... }

<br>

## 4. 생성자

- 생성자(Constructor) : 인스턴스가 생성될 때 호출되는 인스턴스 초기화 메서드

  - 생성자의 이름은 클래스의 이름과 같아야 하고, 생성자는 리턴 값이 없어야 함
  - 클래스에 생성자가 정의되지 않은 경우 컴파일러는 자동적으로 기본 생성자 "클래스이름() {}"을 추가하여 컴파일한다

```java
class Data1 {
    int value;
}

class Data2 {
    int value;
    
    Data2(int x){
        value = x;
    }
}

class Test {
    public static void main(String[] args) {
     	Data d1 = new Data1(); // 기본 생성자에 의해 인스턴스를 생성할 수 있다
        Data d2 = new Data2(10); // 정의된 생성자에 의해 매개변수를 넣어주어야 인스턴스가 생성된다
    }
}
```

- this(), this

  - 같은 클래스 내의 생성자 간의 호출 시 this를 사용

    - 생성자의 이름으로 클래스 이름 대신 this를 사용
    - 한 생성자에서 다른 생성자를 호출할 때는 반드시 첫 줄에서만 호출해야 함
  - this는 참조 변수로 인스턴스 자신을 가리킴
    - this를 활용하여 인스턴스 변수에 접근할 수 있음
    - static 메서드의 경우 this를 사용할 수 없음(인스턴스가 생성되지 않았을 수도 있기 때문이다)
    - 생성자를 포함한 모든 인스턴스메서드에는 자신이 관련된 인스턴스를 가리키는 참조변수 this가 지역변수로 숨겨진 채 존재함
  - 아래 소스 예제는 생성자에서 this를 활용하는 경우이다
    - 첫번째 생성자, 두번째 생성자는 모두 네번째 생성자를 호출하고 이때 this를 사용
    - 네번째 생성자는 다섯번째 생성자와 같다
    - 다섯번째 생성자의 경우 매개변수로 선언된 지역변수 c의 값을 인스턴스 변수 color에 저장한다
    - 이 때, 네번째 생성자와 같이 매개변수로 선언된 지역변수 color가 인스턴스 변수 color와 같은 경우에는 this.color = color와 같이 인스턴스 변수(this.color)와 매개변수(color)를 구분한다
    - 세번째 생성자는 생성된 인스턴스와 같은 상태를 갖는 인스턴스를 추가하고자 할 때 사용하는 생성자이다

```java
class Car {
    String color;
    int price;
    
    Car() { //(1) 세 번째 생성자 Car(String color, int price)를 호출
        this("white", 100); 
    }
    
    Car(String color) { //(2) 세 번째 생성자 Car(String color, int price)를 호출
        this(color, 100);  
    }
    
    Car(Car c) { //(3)
        this(c.color, c.price);
    }
    
    Car(String color, int price) { //(4)
        this.color = color;
        this.price = price;
    }
    
    /* (5)
    Car(String c, int p) {
    	color = c;
    	price = p;
    }
    */
}
```

<br>

## 5. 변수의 초기화

- 멤버변수(클래스변수 또는 인스턴스변수)는 초기화를 하지 않아도 자동적으로 변수의 자료형에 맞는 기본값으로 초기화가 이루어지지만, 지역변수는 사용하기 전에 반드시 초기화해야함

  - 예를 들어, boolean의 경우 false
  - byte, short, int의 경우 0
  - 참조형 변수의 경우 null 등으로 초기화
- 초기화 블럭 (static {} 또는 {})를 이용하여 인스턴스 생성시 반복적으로 수행되어야 하는 작업의 중복을 제거할 수 있다
  - 클래스 변수 초기화 순서 : 기본값 > 명시적초기화 > 클래스 초기화 블럭
  - 인스턴스 변수 초기화 순서 : 기본 > 명시적초기화 > 인스턴스 초기화 블럭 > 생성자

<br>
<br>

**Reference**

- 자바의 정석 3판 - 남궁성

  - Chapter 06. 객체지향 프로그래밍 l

  - Chapter 07. 객체지향 프로그래밍 ll

