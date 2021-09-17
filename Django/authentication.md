# Authentication



## 1. Django Authentcation system

### 1.1 구조

- Django 인증 시스템은 django.contrib.auth에 Django contrib module로 제공
- 인증(authentication)과 권한부여(authorization)을 함께 제공
- settings.py의 INSTALLED_APPS
  - django.contrib.auth ; 인증 프레임워크의 핵심과 기본 모델을 포함
  - django.contrib.contenttypes ; 사용자가 생성한 모델과 권한을 연결할 수 있음
- app이름을 일반적으로 "accounts"로 설정



### 1.2 세션 관리

- Django의 세션은 미들웨어를 통해 구현
  - MIDDLEWARE(미들웨어)
    - http 요청과 응답 처리 중간에서 작동하는 시스템
    - django는 http 요청이 들어오면 미들웨어를 거쳐 해당 URL에 등록되어 있는 view로 연결해주고, http 응답 역시 미들웨어를 거쳐서 내보냄
    - 주로 데이터 관리, 어플리케이션 서비스, 메시징, 인증 및 API 관리를 담당
  - SessionMiddleware ; 요청 전반에 걸쳐 세션을 관리
  - AuthenticationMiddleware ; 세션을 사용하여 사용자를 요청과 연결
- Django는 database-backed sessions 저장 방식을 기본 값으로 사용
- 특정 session id를 포함하는 쿠키를 사용해서 각각의 브랑추저와 사이트가 연결된 세션을 알아냄
- 세션 정보는 Django DB의 django_session 테이블에 저장



### 1.3 로그인

- AuthenticationForm
  - from django.contrib.auth.forms import AuthenticationForm
  - get_user()
    - user_cache는 인스턴스 생성 시에 None으로 할당되며, 유효성 검사를 통과했을 경우 로그인 한 사용자 객체로 할당 됨
    - 인스턴스의 유효성을 먼저 확인하고, 인스턴스가 유효할 때만 user를 제공하려는 구조
- login(request, user, backend=None)
  - from django.contrib.auth import login as auth_login
  - 현재 세션에 연결하려는 인증된 사용자가 있는 경우 활용
  - HttpRequest 객체와 User 객체가 필요
  - Django의 session framework를 사용하여 세션에 user의 ID를 저장



### 1.4 로그아웃

- logout(request)

  - HttpRequest 객체를 인자로 받고 반환 값이 없음
  - 사용자가 로그인하지 않은 경우도 오류를 발생시키지 않음
  - 현재 요청에 대한 session data를 DB에서 완전히 삭제하고, 클라이언트의 쿠키에서도 session id가 삭제됨

  ```python
  # EX.
  
  from django.contrib.auth import login as auth_login, logout as auth_logout
  from django.contrib.auth.forms import AuthenticationForm
  
  @reuire_http_methods(['GET', 'POST'])
  def login(request):
      if request.method == 'POST':
          form = AuthenticationForm(request, request.POST)
          if form.is_valid():
              auth_login(request, form.get_user())
              return redirect('articles:index')
      else:
          form = AuthenticationForm()
      context = { 'form': form }
      return render(request, 'accounts/login.html', context)
  
  @require_POST
  def logout(request):
  	auth_logout(request)
  	return redirect('articles:index')
  ```



### 1.5 로그인 사용자에 대한 접근 제한

- is_authenticated 속성(attribute)

  - user model의 속성(attributes) 중 하나
  - 모든 user 인스턴스에 대해 항상 True인 읽기 전용 속성(AnonymousUser에 대해서는 항상 False)
  - 일반적으로 request.user에서 이 속성을 사용하여, 미들웨어의 django.contrib.auth.middleware.AuthenticationMiddleware를 통과했는지 확인
  - 권한과는 관련이 없으며 사용자가 활성화 상태이거나 유효한 세션을 가지고 있는지도 확인하지 않음

  ```django
  ...
  {% if request.user.is_authenticated %}
  ...}
  ```

- login_required 데코레이터(decorator)

  - 사용자가 로그인되어 있지 않으면 settings.LOGIN_URL에 설정된 문자열 기반 절대 경로로 redirect
    - LOGIN_URL의 기본값은 '/accounts/login/'
  - 사용자가 로그인되어 있으면 정상적으로 view 함수 실행
  - 인증 성공시 사용자가 redirect 되어아하는 경로는 next라는 쿼리 문자열 매개 변수에 저장됨
    - "next" query string ; 로그인이 정상적으로 진행되면 기존에 요청했던 주소로 redirect 하기 위해 주소를 저장해주는 것

  ```python
  ...
  return redirect(request.GET.get('next') or 'articles:index')
  ```

  - @require_POST와 @login_required를 함께 사용하는 경우 에러 발생
    - 로그인 이후 next 매개변수를 따라 해당 함수로 다시 redirect 되는데 이 때 @require_POST 떄문에 405 에러 발생
    - redirect 요청은 POST 방식이 불가능 하기 때문에 GET 방식으로 요청
    - is_authenticated 속성을 활용해야함
    - @login_required는 GET method request를 처리할 수 있는 view 함수에서만 활용



## 2. Authentication ModelForm

- UserCreationForm
  - username과 password로 권한이 없는 새 user를 생성하는 ModelForm
  - 3개의 필드(username, password1, password2)

- UserChangeForm

  - 사용자의 정보 및 권한을 변경하기 위해 admin 인터페이스에서 사용되는 ModelForm
  - UserChangeForm을 상속받는 CustomUserChangeForm이라는 서브클래스를 작성해 접근 가능한 필드를 조정해야 함
    - get_user_model() ; 현재 활성화된 사용자 모델을 반환

  ```python
  from django.contrib.auth.forms import UserChangeForm
  from django.contrib.auth import get_user_model
  
  class CustomUserChangeForm(UserChangeForm):
      
      class Meta:
          model = get_user_model()
          fields = ('email', 'first_name', 'last_name')
  ```

- PasswordChangeForm

  - 사용자가 비밀번호를 변경할 수 있도록 하는 ModelForm
  - update_session_auth_hash(request, user)
    - 현재 요청과 새 session hash가 파생 될 업데이트 된 사용자 객체를 가져오고, session hash를 적절하게 업데이트
    - 비밀번호가 변경되면 기존 세션과의 회원 인증 정보가 일치하지 않게 되어 로그인 상태를 유지할 수 없기 때문에 암호가 변경되어 로그아웃되지 않도록 새로운 password hash로 session을 업데이트 함

