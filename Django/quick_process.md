# basic procedure



## 1. 가상환경

- 가상환경 설정
  - 현재 폴더의 하위 폴더로 venv_name의 가상환경 생성
  - 가상환경 실행

```powershell
$ python -m venv [venv_name]
$ source [venv_name]/Scripts/activate
```

- django 설치

```powershell
# ipython의 경우 django DB API 구문 테스트를 위해 설치
$ pip install django django_extensions ipython
```

- 기타 설정
  - README.md
  - gitignore
    - gitignore.io에서 Keyword로 검색해서 추가
    - Ex) Python, Django, venv, Windows, VisualStudioCode
  - requirements.txt
    - requirements.txt로 패키지 관리

```powershell
$ touch README.md .gitignore

$ pip freeze > requirements.txt
$ pip install -r requirements.txt   # requirements의 패키지 설치
```



## 2. Django Project

- 프로젝트 생성
  - 프로젝트 이름에는 python이나 django에서 사용 중인 키워드 및 "-" 사용 불가

```powershell
$ django-admin startproject [pjt_name]
$ django-admin startproject [pjt_name] .   # 현재 폴더에서 프로젝트 생성
```

- 앱 생성
  - 일반적으로 앱이름은 복수형으로 생성

```powershell
$ python manage.py startapp [app_name]
```

- settings.py
  - INSTALLED_APPS에 'app_name' & 'django_extensions' 추가
  - 최상단에 templates/base.html 생성 후 TEMPLATES의 APP_DIRS에 [BASE_DIR / 'templates'] 추가

- urls.py
  - urls.py의 urlpatterns에 app_url 추가

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('polls/', include('polls.urls')),
]
```

- 서버 실행

```powershell
$ python manage.py runserver
```



## 3. Django Apps

- 일반적으로 model > urls > views > templates의 html 순으로 작성

- models.py

  - model class 생성

  ```python
  # EX. 
  from django.db import models
  
  class Poll(models.Model):
      title = models.CharField(max_length=100)
      ...
  ```

  - migration

  ```powershell
  $ python manage.py makemigrations
  $ python manage.py migrate
  ```

  - django-extensions
    - django extensions을 등록하여 직접 db에 접근 가능

  ```powershell
  $ python manage.py shell_plus
  ```

- urls.py

  - urlpatterns 내에서 해당하는 view로 할당

  ```python
  # EX.
  from django.urls import path
  from . import views
  
  app_name = 'polls'
  
  urlpatterns = [
      path('index/', views.index, name='index'),
      path('<int:pk>/', views.detail, name='detail'),
      ...
  ]
  ```

- views.py

  - 각 view에 해당하는 templates에 전송

  ```python
  # EX.
  from django.shortcuts import render, redirect
  from .models import Poll
  
  def new(request):
      return render(request, 'polls/new.html')
  
  def create(request):
      poll = Poll()
      poll.title = request.POST.get('title')
      poll.content = request.POST.get('content') 
      poll.save()
      return redirect('polls:detail', poll.pk)
      
  def index(request):
      polls = Poll.objects.all()
      context = {
          'polls': polls,
      }
      return render(request, 'polls/index.html', context)
  ```

- template/[app_name]/[sth].html

  - 실제 화면을 구성하는 html 구조 작성

  ```django
  # EX.
  {% extends 'base.html' %}
  
  {% block content %}
  <ul>
    {% for poll in polls %}
    <li><a href="{% url 'polls:detail' poll.pk %}">{{ poll.pk }}-{{ poll.title }}</a></li>
    {% empty %}
    <li>No polls</li>
    {% endfor %}
  </ul>
  {% endblock %}
  ```


