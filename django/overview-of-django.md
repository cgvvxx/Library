# Overview of Django

## 1. Django란

### 1.1. 정의

- Django : high-level Python Web Framework
  - Web Framework : 웹 페이지를 개발하는 과정 framework 제공. 데이터베이스 연동, 템플릿 형태의 표준, 세션 관리, 코드 재사용 등의 기능 포함

- Django의 MTV(Model-Templates-View) Pattern
  - Framework Architecture - MVC(Model-View-Controller) Design Pattern
    - Model : 응용프로그램의 데이터 구조를 정의하고 데이터베이스의 기록 관리(추가, 수정, 삭제)
    - Template(<=> MVC의 view) : 파일의 구조나 레이아웃 정의, 실제 내용을 보여주는 부분
    - View(<=> MVC의 controller) : HTTP 요청 수신 및 응답 반환, Model을 통해 받은 요청을 위해 필요한 데이터 접근, Template에게 응답 전달
  
  <p align="center">
  	<img src="http://wiki.hash.kr/images/5/59/%EC%9E%A5%EA%B3%A0%EA%B5%AC%EC%A1%B0.png">
  </p>

### 1.2. Django의 구조

#### 1.2.1. Project 구조

- 기본 구조

  ```
  mysites
  ├───manage.py
  └───mysite
  		__init__.py
          asgi.py
          settings.py
          urls.py
          wsgi.py

- asgi.py
  - Asynchronous Server Gateway Interface
  - django 어플리케이션이 비동기식 웹 서버와 연결 및 소통하는 것을 도움
- settings.py : 어플리케이션의 모든 설정 포함

- urls.py

  - 사이트의 url과 적절한 views의 연결 지정
  - http 요청을 알맞은 view로 전달

- wsgi.py

  - Web Server Gateway Interface
  - django 어플리케이션이 웹서버와 연결 및 소통하는 것을 도움

- manage.py ; django 프로젝트와 상호작용 하는 커맨드라인 유틸리티

  ```powershell
  $ python manage.py <command> [options]
  ```

#### 1.3.1. Applications 구조

- 일반적으로 application 명은 복수형

- 기본구조

  ```	
  polls
  ├── migrations
  |       __init__.py
  ├── __init__.py
  ├── apps.py
  ├── admin.py
  ├── models.py
  ├── tests.py
  └── views.py
  ```

- admin.py : 관리자용 페이지 설정

- apps.py : 앱의 정보 작성

- models.py : 앱에서 사용하는 Model 정의

- tests.py : 프로젝트의 테스트 코드 작성

- views.py 

  - view 함수들의 정의
  - http 요청을 수신하고 응답을 반환하는 함수 작성
  - Model을 통해 요청에 맞는 필요 데이터에 접근
  - HTTP 응답서식은 Templates를 통해 

- Templates/[app_name]/ : 새로 생성해야 함
  - 실제 내용을 보여주는데 사용되는 파일
  - 파일의 구조나 레이아웃 정의(Ex. html)
  - 일반적으로 최상위 폴더에 templates의 base.html을 상속하여 작성

### 1.4. 기본 사항

- 프로젝트는 어플리케이션의 집합 (collections of apps)

- 어플리케이션(앱)은 실제 요청을 처리하고 페이지를 보여주고 하는 등의 역할을 담당

- 하나의 프로젝트는 여러 앱을 가짐

- 일반적으로 앱은 하나의 역할 및 기능 단위로 작성

- INSTALLED_APPS

  - 항상 APPS 먼저 생성 후 settings에 등록
  - Local apps > Third party apps > Django apps 순서로 등록


### 1.5. DTL(Django Template Language)

- django template에서 사용하는 built-in template system

- Variable, Filters, Tags, Comments 등으로 구성

  ```django
  {{ variables }}   # varialbes
  {{ variables|lower }}   # filters
  {% if %}{% endif %}   # tags
  {% for %}{% empty %}{% endfor %}   # tags2
  {# comments #}   # comments
  {% comments %}{% endcommnets %}   # comments2
  
  # EX.
  {{ name.0 }}
  {{ title|truncatewords:5 }}
  ```

## 2. Django의 구성 요소

### 2.1. Urls

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


### 2.2. View

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

### 2.3. Templates

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
