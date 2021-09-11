# Form



- Django form

  - 데이터의 유효성을 검증하고, 반복되는 코드 작성을 효율화 시킴
  - 공격 및 데이터 손상에 대한 방어 작업 시행
  - 역할
    - 렌더링을 위한 데이터 준비 및 재구성
    - 데이터에 대한 HTML forms 생성
    - 클라이언트로부터 받은 데이터 수신 및 처리

- Form

  - Model의 선언과 유사한 필드 타입사용
  - rendering - as_p, as_ul, as_table
  - widgets : 웹 페이지의 HTML input 요소 렌더링

  ```python
  # EX.
  from django import forms
  
  class ArticleForm(forms.Form):
      title = forms.CharField(max_length=20)
      content = forms.CharField(widget=forms.Textarea)
  ```

- ModelForm

  - Model을 통해 Form class를 생성할 수 있는 헬퍼의 기능
  - 일반 Form class와 완전히 같은 방식(객체 생성)으로 view에서 사용 가능
  - Meta Class : Model의 정보를 작성
  - widgets : HTML input element 표현 방식

  ```python
  # EX.
  from django import forms
  from .models import Article
  
  class ArticleForm(forms.ModelForm):
      
      title = forms.CharField(
      	label = '제목',
          widget = forms.TextInput(
          	attrs = {
                  'class': 'my_title',
                  'placeholder': 'Enter the title',
              }
          )
      )
      
      class Meta:
          model = Article
          fields = '__all__'
  ```

  - method
    - is_valid() : 주어진 데이터가 특정 조건을 충족하는지 유효성 검사 실행, boolean으로 반환
    - save() : Form에 바인딩 된 데이터에서 데이터베이스 객체를 만들고 저장
  - Form vs. ModelForm
    - Form은 어떤 model에 저장해야 하는지 지정되어 있지 않으므로 유효성 검사 후 cleaned_data 딕셔너리 생성
    - Form은 model에 연관되지 않은 데이터를 받을 때 사용
    - ModelForm은 django가 해당 model에서 양식에 필요한 대부분의 정보를 미리 정의
    - ModelForm의 경우 .save() 호출 가능

