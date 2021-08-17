# HTML



## 1. 정의

- HTML ; Hyper Text Markup Language
  - Hyper text ; 하이퍼링크를 통해 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
  - 하이퍼 텍스트는 문서를 구성하는 내용들이 하나의 독립된 파일이 모두 들어있는 것이 아니라 서로 분산된 문서, 또는 내용들이 하이퍼 링크를 통해 하나의 전체 문서를 구성하는 구조
  - 웹 페이지를 구조를 정의하기 위한 언어
  - WHATWG와 W3C에서 발표한 가장 최신 버전은 HTML5



## 2. 구조

- 기본구조

  ```html
  <!DOCTYPE html>
  <html lang="ko">
      <head>
          <meta charset="UTF-8">
          <title>Basic Structure</title>
      </head>
      <body>
          <h1> Large sentence </h1>
  		<div>
              <p> Basic example </p>
          </div>
      </body>    
  </html>
  ```

- DOCTYPE
  - HTML 태그는 아니지만 선언된 페이지의 HTML 버전이 무엇인지를 웹 브라우저에 알려주는 역할을 하는 선언문
  - <html> 태그를 정의하기 전에 가장 먼저 선언되어야 함
  - 대소문자 구분 X
- HEAD
  - 문서 정보를 담고 있으며 웹브라우저에 내용으로 출력되지 않음
  
  - title, css 링크, favicon, 또는 메타데이터 등의 내용을 포함
  
  - Open Graph Protocol 
    - 페이스북에서 정의된 메타 태그 프로토콜
    - HTML 문서의 메타 데이터를 통해 문서의 정보 전달
    - ex. 링크 삽입 시 해당 사이트의 메타태그를 읽어 사이트 정보를 미리 보여줌
    
    ```html
    <!-- EX -->
    <meta property="og:title" content="영화 그녀에게 - 사랑에 대한 집착 혹은 헌신?">
    <meta property="og:image" content="http://blogthumb2.naver.net/20160724_225/dslpob_14693203455573tHdz_PNG/영화그녀에게2002.png?type=w2">
    <meta property="og:description" content="2003년 혹은 2004년? 너무 오래되어서 정확한 년도를 지금은 기억 못하지만 영화의 내용만큼은 시간이 지나 ...">
    <meta property="og:url" content="http://blog.naver.com/dslpob/2204737453">      
    ```

- BODY
  - HTML 문서의 텍스트, 하이퍼링크, 이미지, 리스트 등과 같은 모든 콘텐츠를 포함하는 영역
  - HTML 문서에는 단 하나의 <body> 요소만 존재
  - DOM(Document Object Model, 문서 객체 모델)
    - HTML 문서에 접근하기 위한 인터페이스
    - 객체 모델로써 문서 내의 모든 요소를 정의하고, 각각의 요소에 접근하는 방법을 제공

- Tag, Element, Attribute
  - Tag
    - Elements의 일부로 시작 태그와, 종료 태그 두 종류
    - 일부 태그는 종료 태그 X, 내용 X
    
  - Sementic Tag
    - HTML 요소의 의미를 보다 명확히 나타내는 태그
    - ex. header, main, nav, section, article, aside, footer etc.
    
  - Elements
    - 시작 태그와 종료 태그로 이루어진 모든 명령어
    - 중첩을 통해 문서를 구조화 할 수 있음
    - 공통 속성 ; id, class, hidden, style etc.
    
  - Attributes
    - 태그 내부에서 사용하고, 속성을 통해 태그의 부가적인 정보 설정
    
    ```html
    <!-- EX -->
    <p align="center"> Paragraph example </p>
    <!-- 위의 요소는 p태그로 이루어져 있고 align이라는 attribute를 통해 가운데 정렬을 함 -->
    ```
    
    

## 3. HTML 중요 태그

- Block 요소
  - div ; 블록 요소, 컨텐츠를 한 블럭으로 묶을 때 사용, 내부에 다른 블럭요소 포함 가능
  - p ; 블록 요소, 컨텐츠를 한 블럭으로 묶을 때 사용, 내부에 다른 블럭요소 포함 X
  - ul, ol, li ; ul = unordered list, ol = ordered list, ul과 ol 내부에 항목 나열시 li 사용
- Text 요소
  - span ; 인라인 요소, 컨텐츠를 하나로 묶을 때 사용
  - a ; 웹 페이지나 외부 사이트 연결, href 속성 사용
  - b, strong ; 볼드체(non-semantic vs. semantic)
  - i, em ; 기울임(non-semantic vs. semantic)
  - img ; 이미지 삽입
  - br ; 줄 바꿈, 닫는 태그 X
  - h1 ~ h6 ; 제목, h1이 가장 크고 h6이 가장 작음
- Table 요소
  - tr, td, th
  - thead, tbody, tfoot
  - caption
  - colspan, rowspan
  - scope
  - col, colgroup
- Form
  - 웹 페이지에서의 입력 양식 전체를 감싸는 태그
  - action, name, accept-charset, target, method 등의 태그 속성을 가짐
  - Input
    - Form 태그 내부에서 사용되는 태그로 입력 형식을 지정
    - type, name, required, autofocust, placeholder 등의 태그 속성을 가짐
