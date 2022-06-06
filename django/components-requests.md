# Requests

## HTTP requests

- HTTP 요청에 따라 적절한 예외처리, 데코레이터를 통해 서버를 보호하고 클라이언트에게 정확한 상황을 제공
- django.shortcut
  - render()
  - redirect()
  - get_object_or_404()
    - get() 메서드를 호출할 때 해당 객체가 없을 경우 DoesNotExist 예외 대신 HTTP 404를 raise
  - get_list_or_404()
- View decorators
  - Decorator : 어떤 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능을 연장 해주는 함수
  - django.views.decorators.http
    - require_http_methods() : view 함수가 특정한 method 요청에 대해서만 허용하도록 하는 데코레이터
    - require_POST() : view 함수가 POST method 요청만 승인하도록 하는 데코레이터
    - require_safe()
    - require_GET()

``` python
# EX.
from django.shortcut import redirect, get_object_or_404
from django.views.decorators.http import require_POST

@require_POST
def delete(request, pk):
    article = get_object_or_404(Article, pk=pk)
    article.delete()
    return redirect('articles:index')
