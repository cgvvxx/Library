# CSS



## 1. 정의

- CSS ; Cascading Style Sheets

  - 상속형으로 스타일, 레이아웃 등을 통해 HTML 문서 속성을 기술하는 언어
  - 선택자(Selector)를 이용하여 스타일을 지정할 HTML 요소 선택
  - 속성과 값의 쌍의 statement를 통해 스타일 지정

  ![](./IMAGE/web_selector.png)

- 정의 방법

  - 인라인 :P 개별 태그 속성으로 style 지정

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

- 선택자 우선순위

  - !important > 인라인 > 아이디선택자 > 클래스선택자 > 요소선택자
  - CSS 명시도(Specificity)
    - 계산법 !important > id [100] > class [10] > tag [1] > * [0]
    - 각 해당하는 선택자의 개수로 점수를 부여하여 우선순위 계산
    - 스타일 우선순위가 같거나, 계산 방법이 없는 경우 마지막에 지정된 스타일이 우선적용

- 상속

  - CSS는 상속을 통해 상위에서 적용한 스타일이 하위에도 반영
  - box-model 속성(ex. width, height, margin, padding, border)과 크기와 배치(ex. position, top, right) 관련 속성은 상속 X

- 선택자 종류
  - 전체 선택자(Universal Selector)
    - HTML 페이지 내부의 모든 요소(태그)에 같은 CSS 속성을 적용

  ```css
  * {margin: 0; text-decoration: none;}
  ```

  - 태그 선택자(Type Selector)
    - HTML 태그를 직접 선택

  ```css
  p {background: darkgreen; color: yellowgreen;}
  >> <p>태그선택자</p>
  ```

  - 클래스 선택자(Class Selector)
    - 선택하려는 속성값 앞에 마침표(.)를 작성
    - 주어진 값을 class 속성으로 가진 HTML 요소를 찾아 선택

  ```css
  .class1 {background: darkgreen; color: yellowgreen;}
  >> <p class="class1">클래스선택자</p>
  ```

  - 아이디 선택자(ID Selector)
    - 클래스 선택자에서 마침표(.) 대신 # 문자를, class 속성 대신 id 속성을 사용
    - 일반적으로 유일하게 적용될 스타일의 경우 아이디 선택자 사용

  ```css
  #id1 {background: darkgreen; color: yellowgreen;}
  >> <p id="id1">아이디선택자</p>
  ```

  - 속성 선택자(Attribute Selector)
    - 태그 안의 특정 속성에 따라 스타일을 지정

  ```css
  a[href] {background: darkgreen; color: yellowgreen;}
  >> <a href="a.html">속성선택자</a>
  ```

  - 복합 선택자(Combinator)
    - 두 개 이상의 선택자 요소가 모인 선택자
    - 자손 선택자(Descendant Combinator) : 부모 요소에 포함된 모든 하위 요소에 스타일 적용
    - 자식 선택자(Child Combinator) :부모의 바로 아래 자식요소에만 적용
    - 일반 형제 선택자(General Sibling Combinator) : 형제 요소 중 뒤에 위치하는 모든 요소에 적용
    - 인접 형제 선택자(Adjacent Sibling Combinator) : 형제 요소 중 바로 뒤에 위치하는 요소에만 적용

  ```css
  /*자손 선택자*/
  section ul {border: 1px dotted black;} 
  /*자식 선택자*/
  section > ul {border: 1px dotted black;}
  /*일반 형제 선택자*/
  h1 ~ ul {background: darkgreen; color: yellowgren;}
  /*인접 형제 선택자*/
  h1 + ul {background: darkgreen; color: yellowgren;}
  ```

  - 가상 클래스 선택자(Pseudo-classes Selector) - **TBD ...**

    - 링크 선택자(Link Pseudo-classes Selector)
    - 동적 선택자(User Action Pseudo-classes Selector)
    - 구조적 가상 클래스 선택자(Structural Pseudo-classes Selector)



## 3. 단위

- 절대 단위 : cm, mm, in, px
  - in : 인치 ( lin = 96 px = 2.54 cm)
  - px : 픽셀 ( 1 px = 1/96th of lin )
- 상대 단위 : em, rem, %
  - em : 부모 요소의 폰트 사이즈에 대한 상대값. 부모요소가 정해져 있지 않으면 1em = 16px
  - rem(root em) : 최상위 요소(html)의 폰트 사이즈에 대한 상대값 
  - % : 부모 요소를 기준으로 한 비율로 표현
- Viewport 단위 : vh, vw, vmin, vmax
  - vh : viewport 높이 값의 1/100, (height:100vh이 풀화면)
  - vw : viewport 너비 값의 1/100
  - vmin : 웹 브라우저 너비와 높이 중에 더 작은 값을 기준으로 1/100
  - vmax : 웹 브라우저 너비와 높이 중에 더 큰 값을 기준으로 1/100 
- 색상 단위 : 색상 키워드, RGB, HSL
  - 색상 키워드 : 대소문자 구분 X. red, blue 등과 같이 특정 색을 표기
  - RGB : 16진수 표기법 혹은 함수형 표기법을 사용하여 특정 색을 표현
  - HSL : 색상, 채도, 명도 등을 통해 특정 색을 표현



## 4. Box Model, Float

![](./IMAGE/css_boxmodel.png)

- 구성 요소
  - 내용(content) : 박스의 실질적인 내용 부분, 텍스트나 이미지
  - 패딩(padding) : 내용과 테두리 사이의 간격
  - 테두리(border) : 내용과 패딩 주변을 감싸는 테두리
  - 마진(margin) : 테두리와 이웃하는 요소 사이의 간격, 배경색 지정 X
- margin / padding / morder shorthand
  - margin: 25px ; 상하좌우 25px
  - margin: 25px 50px ; 상하 25px, 좌우 50px
  - margin: 25px 50px 75px ; 상 25px, 좌우 50px, 하 75px
  - margin: 25px 50px 75px 100px ; 상 25px, 우 50px, 하 75px, 좌 100px
  - margin 0 auto ; 자동 중앙 정렬(좌우)
  - border: 5px solid red ; width & style & color
- box-sizing
  - content-box : width, height의 값이 content 영역을 의미 (기본값)
  - border-box : width, height의 값이 content, padding, border가 포함된 값
- magin collapsing
  - 두 박스가 온전한 block-level 요소일 경우 적용
  - 좌우 마진은 겹치더라도 상쇄 X
  - 마진 값이 0이더라도 상쇄 규칙 적용

