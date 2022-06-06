# Model

- Django에서의 Model

  - 저장된 데이터베이스의 구조로 단일한 데이터에 대한 정보를 가짐
  - django는 model을 통해 데이터에 접속하고 관리
  - 일반적으로 model은 하나의 데이터베이스 테이블에 매핑 됨

  - 웹 어플리케이션의 데이터를 구조화하고 조작하기 위한 도구

- ORM(Object-Relational-Mapping)
  - 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에(ex. Django - SQL) 데이터를 변환하는 프로그래밍 기술
  - OOP 프로그래밍에서 RDBMS을 연동할 때, 데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법
  - Django는 내장 Django ORM을 사용
  - DB를 객체(Object)로 조작하기 위해 ORM을 사용

- models.py 작성

  - 각 모델은 django.db.models.Model 클래스의 서브 클래스로 표현
  - 각 필드는 클래스 속성으로 지정되며, 각 속성은 데이터베이스의 column에 매핑
  - CharField(), TextField(), DateTimeField(), EmailField() etc.
  - str method를 통해 각 object를 설명할 수 있는 문자열을 반환하도록 설정 가능

  ```python
  from django.db import models
  
  class Poll(models.Model):
      title = models.CharField(max_length=100)
      content = models.TextField()
      created_at = models.DateTimeField(auto_now_add=True)
      
      def __str__(self):
          return self.title
  ```

- Migrations

  - django에서 model에 생긴 변화를 반영하는 방법
  - makemigrations, migrate 명령어를 통해 migration 실행
    - makemigrations - migration 파일 생성
    - migrate - DB에 migration 적용, 모델에서의 변경 사항들과 DB의 스키마의 동기화
    - sqlmigrate - migration에 대한 sql 구문 확인
    - showmigrations - 프로젝트 전체의 migration 상태 확인

  ```powershell
  $ python manage.py makemigrations
  $ python manage.py migrate
  $ python manage.py showmigrations   # migrations 적용 현황
  $ python manage.py sqlmigrate [app_name] [migration_name]   # 지정된 migrations의 sql 내역
  ```

- DB API

  - django에서 기본적으로 ORM을 제공함으로써 DB를 편하게 조작 가능
  - model 생성시 django는 객체들을 만들고 읽고 수정하고 지울 수 있는 database-abstarct API를 자동으로 생성
  - Manager - django 모델에 데이터베이스 query 작업이 제공되는 인터페이스
  - QuerySet - 데이터베이스로부터 전달받은 객체 목록
  - 생성(Create)

  ```powershell
  # 1.
  >>> poll = Poll()
  >>> poll.title = 'title1'
  >>> poll.content = 'content1'
  >>> poll.save()
  
  # 2.
  >>> poll = Poll(title='title2', content='content2')
  >>> poll.save()
  
  # 3.
  >>> Poll.objects.create(title='title3', content='content3')
  ```

  - 조회(Read)

  ```powershell
  # 1. 현재 QuerySet의 복사본을 반환
  >>> Poll.objects.all()
  
  # 2. 주어진 변수에 일치하는 하나의 객체만 반환
  # 객체가 존재하지 않으면 DoewNotExist 에외를, 둘 이상의 객체를 찾으면 MultipleObjectReturned 예외를 발생
  >>> Poll.objects.get(pk=1) 
  
  # 3. 해당하는 객체를 포함하는 새 QuerySet 반환
  >>> Poll.objects.filter(content='title')
  ```

  - 수정(Update)

  ```powershell
  >>> poll = Poll.objects.get(pk=2)
  >>> poll.title = 'title updated'
  >>> poll.save()
  ```

  - 삭제(Delete)

  ```powershell
  # 삭제된 개체 수와 객체 유형당 삭제 수가 포함된 딕셔너리 반환
  >>> poll = Poll.objects.get(pk=3)
  >>> poll.delete()
  ```

- Admin

  - 서버의 관리자가 활용하기 위한 페이지
  - Model class를 admin.py에 등록하고 관리

  ```python
  from django.contrib import admin
  from .models import Poll
  
  admin.site.register(Poll)
  ```

  - 관리자 계정 생성 후 /admin에서 관리자 페이지 로그인을 통해 관리

  ```powershell
  $ python manage.py createsuperuser
  ```

  
