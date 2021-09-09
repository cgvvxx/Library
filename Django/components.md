# Components



## 1. Urls

- 웹 어플리케이션은 URL을 통한 클라이언트의 요청에서부터 시작

- 기존 프로젝트의 urls.py에 각각의 앱의 urls.py 추가

  - include() - 다른 URLconf들을 참조할 수 있도록 하는 함수

  ```python
  from django.urls import path, include
  
  urlpatterns = [
      ...
      path('polls/', include('polls.urls')),
  ]
  ```

- Variable Routing

  - URL 주소를 변수로 사용
  - view 함수의 인자로써 전달
  - URL path converters
    - str : 기본값, '/'를 제외한 비어 있지 않은 모든 문자열과 매치
    - int : 0 또는 양의 정수와 매치
    - slug : 모든 슬러그 문자열과 매치

  ```python
  # EX. 
  path('hi/<str:name>/', views.hi)
  path('detail/<int:pk>/', views.detail)
  ```

  - app의 view함수가 많아지면서 프로젝트 유지보수를 위해 각 app에 urls.py 작성

- Naming URL patterns

  - path() 함수의 name 인자를 이용하여 naming
  - DTL의 url 태그를 이용하여 path() 함수에 작성한 name 사용 가능 > 특정 경로 의존성 제거

  ```python
  path('index/', views.index, name='index')
  ```

- DTL - url : URL patterns 이름 및 선택적 매개 변수와 일치하는 절대 경로 주소 반환

  ```django
  <a href="{% url 'index' %}">INDEX</a>
  ```

- URL Namespace

  - urls.py에 "app_name" attribute 값 작성
  - 이를 통해 서로 다른 앱에서 동일한 URL 이름을 사용하는 경우에도 지정된 이름의 URL을 고유하게 사용 가능

  ```python
  app_name = 'polls'
  urlpatterns = [
      path('index/', views.index, name='index'),
  ]
  ```

  ```django
  <a href="{% url 'polls:index' %}">INDEX</a>
  ```

  

## 2. View

- render, redirect
  - return은 render ; 새로 다른 html로 갈때, redirect ; 이미 만들어진 view로 갈때
  - render는 보낼때 context도 같이 보냄
  - redirect할때 인자 있으면 인자도 같이
  - redirect()
    - 새 URL로 돌림
    - 브라우저는 현재 경로에 따라 전체 URL 자체를 재구성

- request.method
  - GET
    - 특정 리소스를 가져오도록 요청할 때 사용
    - DB에 변화를 주지 않음
  - POST
    - 서버로 데이터를 전송할 때 사용
    - 리소스를 생성/변경하기 위해 데이터를 HTTP body에 담아 전송
    - 서버에 변경사항 생성가능
- CRUD
  - Model의 DB API를 이용하여 주어진 DB의 객체를 생성하여 CRUD 작업 실행



## 3. Templates

- Template Namespace
  - Django는 기본적으로 app_name/templates/ 경로에 있는 templates 파일들만 찾을 수 있으며, INSTALLED_APPS에 작성한 app 순서로 templates을 검색 후 렌더링 함
  - 따라서 templates의 폴더 구조를 app_name/templates/app_name 형태로 변경해 임이의 이름 공간을 생성 후 변경된 추가 경로로 수정

- Inheritance

  - 코드의 재사용성을 위하여 템플릿 상속 이용
  - 사이트의 모든 공통 요소를 포함하고 하위 템플릿이 재정의해 사용할 수 있는 블록을 정의하는 기본 skeleton 템플릿을 생성
  - extends
    - 자식템플릿이 부모 템플릿을 확장한다는 것을 알림
    - 반드시 템플릿의 최상단에 작성되어야 함
  - block
    - 자식템플릿에서 재지정되는 블록 부분을 정의

  ```django
  {% extends '' %}
  {% block content %}{% block %}
  ```

- CSRF(Cross-Site Request Forgery) token

  - 웹 어플리케이션 취약점 중 하나로 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법
  - Security Token 방식의 CSRF token 사용
  - input type이 hidden으로 작성되며 value는 django에서 생성한 hash 값으로 설정
  - 해당 태그 없이 요청을 보내면 django 서버는 403 forbidden으로 응답

  ```django
  {% csrf_token %}
  ```

  
