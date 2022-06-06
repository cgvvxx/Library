# Static Files

- 사용자의 요청에 따라 내용이 바뀌는 것이 아닌 별도의 처리 없이 파일 내용을 그대로 보여주는 파일

- 구성

  - django.contrib.staticfiles가 INSTALLED_APPS에 포함되어 있는지 확인
  - 앱의 static/[app_name] 폴더 내에 static file 저장
  - STATIC_URL
    - app/static/.. 경로 외의 추가적인 정적 파일 경로 목록을 정의하는 리스트
    - 추가 파일 디렉토리에 대한 전체 경로를 포함하는 문자열 목록으로 작성

  ```python
  STATICFILES_DIRS = [
      BASE_DIR / 'static',
  ]
  ```

  - static templates

  ```django
  {% load static %}
  <img src="{% static 'my_app/example.jpg' %}" alt='my_app_img'>
  ```

  - STATIC_ROOT
    - collectstatic이 배포를 위해 정적 파일을 수집하는 디렉토리의 절대 경로
    - django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아 놓는 경로
    - 실제 배포 서비스 환경에서 django의 모든 정적 파일을 다른 웹 서버가 직접 제공하기 위해 사용

  ```python
  STATIC_ROOT = BASE_DIR / 'staticfiles'
  
  >> $ python manage.py collectstatic
  ```

  

  

  

  

  

  

