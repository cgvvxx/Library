# CSS



## 1. 정의

- CSS ; Cascading Style Sheets

  - 상속형으로 스타일, 레이아웃 등을 통해 HTML 문서 속성을 기술하는 언어
  - 선택자(Selector)를 이용하여 스타일을 지정할 HTML 요소 선택
  - 속성과 값의 쌍의 statement를 통해 스타일 지정

  ![](./IMAGE/web_selector.png)

- 정의 방법

  - 인라인 ; 개별 태그 속성으로 style 지정

  ```html
  <span style="font-size:12px;font-family:Helvetica;">text</span>
  ```

  - 내부 참조 ; head 태그 내에 style 지정

  ```html
  <head>
      <style>
          .h4{
              font-size:12px;
              font-family:Helvetica;
          }
      </style>
  </head>
  ```

  - 외부 참조 ; 별도의 파일로 CSS를 정의해 head내에 link로 호출해서 style 지정, 일반적으로 사용

  ```html
  <head>
      <link rel="stylesheet" href="./style.css">
  </head>
  ```



## 2. 선택자(Selector)

- 전체 선택자(Universal Selector)

  - HTML 페이지 내부의 모든 요소(태그)에 같은 CSS 속성을 적용

  ```css
  * {margin: 0; text-decoration: none;}
  ```

- 태그 선택자(Type Selector)

  - HTML 태그를 직접 선택

  ```css
  p {background: yellowgreen; color: darkgreen;}
  >> <p>태그선택자</p>
  ```

- 클래스 선택자(Class Selector)

  - 주어진 값을 class 속성으로 가진 HTML 요소를 찾아 선택

  ```css
  .class1 {background: darkgreen; color: yellowgreen;}
  >> <p class="class1">클래스선택자</p>
  ```

- 아이디 선택자(ID Selector)

  - 

- 속성 선택자(Attribute Selector)

- 복합 선택자(Combinator)

  - 자손 선택자(Descendant Combinator)
  - 자식 선택자(Child Combinator)
  - 일반 형제 선택자(General Sibling Combinator)
  - 인접 형제 선택자(Adjacent Sibling Combinator)

- 가상 클래스 선택자(Pseudo-classes Selector)

  - 링크 선택자(Link Pseudo-classes Selector)
  - 동적 선택자(User Action Pseudo-classes Selector)
  - 구조적 가상 클래스 선택자(Structural Pseudo-classes Selector)
