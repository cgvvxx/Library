# Bootstrap



## 1. 정의

-  웹사이트를 쉽게 만들 수 있게 도와주는 HTML, CSS, JS 프레임워크
- 각종 레이아웃, 버튼, 입력창 등의 디자인을 CSS와 Javascript로 만들어 놓음

- CDN(Content Delivery Network)

  - 서버와 사용자 사이의 물리적 거리를 줄여 웹 페이지 콘텐츠 로드 지연을 최소화하는, 촘촘히 분산된 서버로 이루어진 플랫폼
  - 아래와 같이 부트스트랩 CDN 링크를 head 태그 내부에 삽입

  ```html
  <!DOCTYPE html> 
  <html lang="ko"> 
      <head> 
          <meta charset="utf-8"> 
          <meta http-equiv="X-UA-Compatible" content="IE=edge"> 
          <meta name="viewport" content="width=device-width, initial-scale=1"> 
  
          <title>Bootstrap</title> 
  
          <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" crossorigin="anonymous"> 
          <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" crossorigin="anonymous"> 
      </head> 
      <body > 
          <div class="container"> 
              <h1>Hello, world!</h1> 
              <p>This is Bootstrap sample page!</p> 
          </div> 
  
          <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script> 
          <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" crossorigin="anonymous"></script> 
      </body> 
  </html>
  ```

  

## 2. Spacing

- m : margin, p : padding

- t : top,  b : bottom, s : left, e : right, x : left & right, y : top & bottom

- 0 : 0 rem = 0px, 1 : 0.25 rem = 4px, 2 : 0.5 rem = 8px, 3 : 1 rem = 16px, 4 : 1.5 rem = 24px, 5 : 3 rem : 48px

  - 브라우저 HTML의 root 글꼴 크기 = 16px

  ```html
  <!-- EX -->
  <div class "box mt-3 ms-5">margin</div>
  <div class "box mx-auto">mx auto</div>
  ```



## 3. Grid system

- 반응형 웹 디자인(Responsive Web Design)
  - 다양한 화면 크기를 가진 디바이스에 맞춘 웹 디자인
- Container, Row, Column으로 구성됨
  - 12개의 columns, 6개의 grid breakpoints(xs, sm, md, lg, xl, xxl)
  - 576px, 768px, 992px, 1200px, 1400px
- Example
  - .container-fluid : 좌우로 꽉 찬 레이아웃
  - offset : 지정된 블록만큼의 간격을 두고 배치
    - class="col-md-offset-1 col-md-3"
  - visible : 지정된 디바이스의 크기의 경우에만 컨텐츠를 표시
    - class="visible-md-block"

```html
<!-- EX -->
<div class="container">
    <div class="row">
        <div class="col-lg-2 col-md-3 col-sm-4 col-xs-12">col1</div>
        <div class="col-lg-10 col-md-9 col-sm-8 col-xs-12">col2</div>
    </div>
</div>
```



