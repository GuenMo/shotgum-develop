# [액션 메뉴 항목 생성, 테스팅 및 디버깅](https://www.youtube.com/watch?v=u3u8pL9o-as&feature=youtu.be&t=0s)

> 액션 메뉴 항목 또는 줄여서 AMI는 툴킷 액션의 경량 버전입니다.
> AMI를 이용하면 HTTP 또는 HTTPS 같은 지정된 프로토콜, 아니면 커스텀 프로토콜을 통해 Shotgun 페이지에서 스크립트로 정보를 전송할 수 있습니다.
> 이 동영상에서는 여러 AMI 예와 그 구현 결과를 살펴봅니다.

## 소개

![Local Image](/img/8/1.png)

AMI는 툴킷 액션 대신 사용할 수 있는 경량 옵션입니다.
AMI를 사용하면 http 또는 https와 같은 특정 프로토콜이나 커스텀 프로토콜을 통해 Shotgun 페이지의 정보를 스크립트로 보낼 수 있습니다.

![Local Image](/img/8/2.png)

AMI에 대해 알아야 할 중요한 사항은 정보를 보내기는 하지만 받을 수는 없다는 점입니다.

AMI가 데이터를 보내는 방법에는 POST와 GET, 두 가지가 있습니다.

![Local Image](/img/8/3.png)

![Local Image](/img/8/4.png)

## AMI 등록

![Local Image](/img/8/5.png)

![Local Image](/img/8/6.png)

## AMI와 Django

Django는 완벽한 기능을 갖춘 Python 기반의 오픈 소스 웹 프레임워크이며 데이터베이스 및 관리 웹 포털과 함께 패키징된 상태로 제공됩니다.

폴더와 가상환경을 만듭니다.

![Local Image](/img/8/8.png)

`Django` 와 `Shotgun Python API`를 설치 합니다.

```console
pip install django
pip install git+git://github.com/shotgunsoftware/python-api.git@v3.0.26
```

**프로젝트와 앱을 생성 합니다.**

```console
django-admin startproject mysite .
python manage.py migrate
python manage.py createsuperuser
python manage.py startapp ami
```

**작동 하는지 확인 합니다.**

```python
python manage.py runserver
```

![Local Image](/img/8/9.png)

**설정 파일을 수정 합니다.**

```python
# mysite/settings.py

ALLOWED_HOSTS = ['*']

INSTALLED_APPS = [
    'ami',
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
```

```console
python manage.py makemigrations
python manage.py migrate
```

**Template 파일을 만듭니다.**

```html
<!-- ami/templates/ami/ami_for_asset.html -->

<h1>Hello</h1>
```

**urls 설정 합니다.**

```python
# mysite/urls.py

from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^ami/', include('ami.urls', namespace='ami')),
    url(r'^admin/', admin.site.urls),
]
```

```python
# ami/urls.py

from django.conf.urls import url

from .views import AmiView

app_name = 'ami'

urlpatterns = [
    url(r'^$', AmiView.as_view(), name='index'),
]
```

**view를 만듭니다.**

```python
# ami/views.py
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.shortcuts import render
from django.views.generic.base import TemplateView


class AmiView(TemplateView):
    template_name = 'ami/ami_for_asset.html'
```

**url이 작동하는지 확인 합니다.**

```console
python manage.py runserver
```

<http://127.0.0.1:8000/ami/>

![Local Image](/img/8/10.png)

**POST GET 구현.**

```python
# ami/views.py

# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.shortcuts import render
from django.views.generic.base import View, ContextMixin, TemplateResponseMixin


class AmiView(ContextMixin, TemplateResponseMixin, View):
    template_name = 'ami/ami_for_asset.html'

    def get(self, request, *args, **kwargs):
        context = self.get_context_data(*args, **kwargs)
        context['method'] = 'get'
        return render(request, self.template_name, context)

    def post(self, request, *args, **kwargs):
        context = self.get_context_data(*args, **kwargs)
        context['method'] = 'post'
        return render(request, self.template_name, context)
```

```html
<!-- ami/templates/ami/ami_for_asset.html -->
<h1>Hello</h1>

{{ method }}
```

<http://127.0.0.1:8000/ami/>

![Local Image](/img/8/11.png)

**Shotgun API 연동.**

```python
# ami/views.py

# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.shortcuts import render
from django.views.generic.base import View, ContextMixin, TemplateResponseMixin
import shotgun_api3
from .config import *


class AmiView(ContextMixin, TemplateResponseMixin, View):
    template_name = 'ami/ami_for_asset.html'

    def shotgun(self):
        sg = shotgun_api3.Shotgun(URL, script_name=SCRIPT_NAME, api_key=API_KEY)
        return sg

    def get(self, request, *args, **kwargs):
        context = self.get_context_data(*args, **kwargs)
        sg = self.shotgun()
        projects = sg.find(
            'Project', [
                ['is_template', 'is', False],
                ['name', 'not_contains', 'demo'],
            ],
            ['name']
        )
        context['projects'] = projects
        return render(request, self.template_name, context)

    def post(self, request, *args, **kwargs):
        context = self.get_context_data(*args, **kwargs)
        context['method'] = 'post'
        return render(request, self.template_name, context)
```

```html
<!-- ami/templates/ami/ami_for_asset.html -->
<h1>Project List</h1>

<ul>
{% for project in projects %}
    <li>{{ project }}</li>
{% endfor %}
</ul>
```

<http://127.0.0.1:8000/ami/>

![Local Image](/img/8/12.png)

`demo` script key를 사용해서 사용자 인증을 하겠습니다.

**CSRF 처리.**

```python
# mysite/mixins.py

from django.utils.decorators import method_decorator
from django.views.decorators.csrf import csrf_exempt

class CsrfExemptMixin(object):
    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return super(CsrfExemptMixin, self).dispatch(request, *args, **kwargs)
```

```python
# ami/views.py

# -*- coding: utf-8 -*-
from __future__ import unicode_literals

import shotgun_api3
from django.shortcuts import render
from django.views.generic.base import View, ContextMixin, TemplateResponseMixin

from mysite.mixins import CsrfExemptMixin
from .config import *


class AmiView(ContextMixin, TemplateResponseMixin, CsrfExemptMixin, View):
    template_name = 'ami/ami_for_asset.html'

    def shotgun(self):
        sg = shotgun_api3.Shotgun(URL, script_name=SCRIPT_NAME, api_key=API_KEY)
        return sg

    def get(self, request, *args, **kwargs):
        context = self.get_context_data(*args, **kwargs)
        sg = self.shotgun()
        projects = sg.find(
            'Project', [
                ['is_template', 'is', False],
                ['name', 'not_contains', 'demo'],
            ],
            ['name']
        )
        context['projects'] = projects
        return render(request, self.template_name, context)

    def post(self, request, *args, **kwargs):
        sg = self.shotgun()
        projects = sg.find_one('Project', [])
        return JsonResponse(projects)
```

![Local Image](/img/8/13.png)

Shotgun AMI 클릭 한 데이터를 Django 에서 처리 하도록 합니다.

```python
# ami/views.py

# -*- coding: utf-8 -*-
from __future__ import unicode_literals

import shotgun_api3
from django.shortcuts import render
from django.views.generic.base import View, ContextMixin, TemplateResponseMixin
from django.http import JsonResponse

from mysite.mixins import CsrfExemptMixin
from .config import *


class AmiView(ContextMixin, TemplateResponseMixin, CsrfExemptMixin, View):
    template_name = 'ami/ami_for_asset.html'

    def shotgun(self):
        sg = shotgun_api3.Shotgun(URL, script_name=SCRIPT_NAME, api_key=API_KEY)
        return sg

    def post(self, request, *args, **kwargs):
        sg = self.shotgun()
        projects = sg.find_one('Project', [])
        post_data = dict(request.POST.iterlists())
        if len(post_data['selected_ids']) != 1:
            message = 'Error: Select one entity!'
        else:
            asset = sg.find_one(
                post_data['entity_type'][0],
                [
                    ['id', 'is', int(post_data['selected_ids'][0])]
                ],
                ['shots', 'code'],
            )
            message = '{} is used in the following shots: {}'.format(
                asset['code'], [x['name'] for x in asset['shots']]
            )
        return render(request, self.template_name, {'message': message})
        #return JsonResponse(post_data, json_dumps_params={'indent': 4})
```

Shotgun의 데이터를 사용하여 커스텀 뷰를 만들 수 있는 정말로 멋진 솔루션입니다.
보고서를 실행하고, 그래프를 만들고, 통찰력 있는 정보를 아티스트, 프로듀서, 감독에게 제공하는 등 원하는 어떤 작업이든 가능합니다.

![Local Image](/img/8/14.png)

![Local Image](/img/8/15.png)

두 번째 문제는 공유 파일 시스템의 Flask 또는 Django 웹 서버에서 수행하는 액션이 서버 사용자 및 권한 규칙 세트를 기반으로 수행된다는 점입니다.

또한, Shotgun에서 데이터를 만들거나 변경하기 위해 Shotgun Python API에서 수행하는 액션이 스크립트 사용자로부터 발생하는 것으로 기록됩니다.

하지만 AMI가 Shotgun의 정보를 아티스트의 로컬 컴퓨터로 전달하고 거기서 해당 사용자의 Shotgun 자격 증명을 사용하여 프로세스를 트리거할 수 있다면 좋지 않을까요?

## AMIs with custom protocol handlers

Custom protocol handlers는 웹 앱에 의해 로컬 컴퓨터가 어떤 작업을 하게 하는 것입니다. 여기서 어떤 작업이라 하면 python script를 실행 하게 하는 것을 포함한 모든 작업이 가능 합니다.

간단한 예로, AMI를 설정하고 컴퓨터에 설치한 Skype를 가리키도록 할 경우
AMI를 실행하면 Skype가 시작됩니다.

![Local Image](/img/8/16.png)

Skype는 설치시 OS에서 호출 가능 하도록 되어 있기 때문에 바로 호출이 가능 하지만 개발자가 만든 app이나 script는 호출 가능 하도록 설정을 해줘야 합니다.
이는 각 OS마다 설정 방법이 다릅니다.

[OS 별 커스텀 브라우저 프로토콜 사용 방법](https://support.shotgunsoftware.com/hc/ko/articles/219031308-%EC%BB%A4%EC%8A%A4%ED%85%80-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EC%9D%91%EC%9A%A9%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%EC%8B%A4%ED%96%89)

## Windows

**레지스트리 편집.**

`Windows`키 + R 클릭한 후 그림과 같이 레지스트리 편집기를 엽니다.

![Local Image](/img/8/17.png)

`HKEY_CLASSES_ROOT`에 키를 생성 합니다. 키 값은 `shotgun_ami` 합니다.

![Local Image](/img/8/18.png)

![Local Image](/img/8/26.png)

`shotgun`키에 문자열을 추가합니다. 문자열 값은 `URL_protocol` 입니다.

![Local Image](/img/8/27.png)

![Local Image](/img/8/28.png)

같은 방법으로 `shell`, `open`, `command`를 그림과 같이 생성해 줍니다.

![Local Image](/img/8/29.png)

마지막으로 `command`키 값 안에 문자열 편집 합니다.

![Local Image](/img/8/30.png)

> `응용프로그램 경로` `%1`

여기서 응용프로그램은 `python` 됩니다. `python`대신 `pythonw`를 사용하면 콘솔창이 생기지 않습니다.
주의 할점은 python이 Shotgun Python API를 사용 해야 하기 때문에 따로 `vertualenv`를 만들어 놓은 후 vertualenv의 python을 사용 했습니다.
`%1`은 인자 값을 받기 위함이고 인자값을 받아야 하는 부분이 있다면 추가 합니다.

예제 에서는 응용프로그램을 `D:\shotgun_venv\Scripts\python`으로 하고 첫번째 인자는 `D:\ami_test\sgTriggerScript.py`, 두번째 인자는 url에서 받아야 하므로 `%1`로 설정 했습니다.

레지스트리 등록이 완료 됩습니다.
이제 웹브라우저에서 `shotgun://`을 호출 해봅니다.

**Python Script 생성.**

```python
# coding:utf-8

import json
import shotgun_api3


with open('D:/ami_test/result.txt', 'w') as result:
    sg = shotgun_api3.Shotgun(
        'https://aiw.shotgunstudio.com',
        login='geunmo_kim',
        password='**********'
    )
    project =  sg.find_one('Project', [])

    try:
        result.write(json.dumps(project, indent=4))
    except:
        result.write(u'Failed.')
```

**AMI 등록.**

Shotgun AMI를 등록 합니다.

`URL`를 `shotgun://`로 수정 합니다.

![Local Image](/img/8/31.png)

스크립트 결과로 `result.txt` 파일이 생성 됩니다.

**라우팅.**

이 스크립트는 모든 AMI 요청을 특정 함수로 라우팅하므로 특정 액션을 식별할 방법이 필요합니다.

액션 메뉴를 두개 추가 합니다.

| Name           | URL                 |
| -------------- | ------------------- |
| Launch Python1 | shotgun://ami_test1 |
| Launch Python2 | shotgun://ami_test2 |

![Local Image](/img/8/33.png)

**Python Script 수정.**

```python
# coding:utf-8

import json
import sys
import urllib

def main():
    arg = sys.argv[1]
    _, fullPath = arg.split(':', 1)
    path, fullArgs = fullPath.split('?', 1)
    action = path.strip('/')
    args = fullArgs.split('&')

    get_dict = {}
    for arg in args:
        key, value = map(urllib.unquote, arg.split('=', 1))
        get_dict[key] = value

    if action == 'ami_test1':
        with open('D:/ami_test/result.txt', 'w') as result:
            result.write(action)
            result.write(json.dumps(get_dict))

    if action == 'ami_test2':
        with open('D:/ami_test/result.txt', 'w') as result:
            result.write(action)
            result.write(json.dumps(get_dict))

if __name__ == '__main__':
    sys.exit(main())
```

`Launch Python1`와 `Launch Python2`를 시행 시켰을 때 다른 결과를 같습니다.

**가능한 변형.**

AMI에서 URL의 // 부분 뒤에 오는 키워드를 변경함으로써 스크립트 내 action 변수의 내용을 변경할 수 있습니다. 이때 같은 shotgun:// 프로토콜이 유지되고, 커스텀 프로토콜 하나만 등록됩니다. 그러면 action 변수의 내용과 매개변수의 내용을 통해 의도된 동작이 무엇인지 스크립트가 파악할 수 있습니다.

이 방법을 사용하여 응용프로그램을 열고, FTP 같은 서비스를 통해 컨텐츠를 업로드하고, 데이터를 보관하고, 이메일을 보내거나, PDF 보고서를 생성할 수 있습니다.

## AMI 디버깅

개발 도중에 AMI 스크립트에서 오류를 쉽게 포착하거나 프로덕션 단계에서 디버그하려면 브라우저의 디버깅 도구에 익숙해져야 합니다.

크롬 shotgun 페이지 에서 계발자 도구를 클릭합니다.
`Nsetwork` 선택 하면 shotgun 페이지에서 보내는 정보드를 확인 할수 있습니다.

custom protocol 같은 경우 내부에서 생기는 문제는 개발자 도구에서 확인 할 수가 없습니다.
이런 경우 python script에 logging 모듈을 사용해 로그 파일로 남깁니다.

**logging 모듈 예제.**

```python
# coding:utf-8

import logging
import logging.handlers

if __name__ == '__main__':
    # looger 인스턴스를 생성, 로그 레벨, 포멧 설정
    logger = logging.getLogger('my')
    logger.setLevel(logging.DEBUG)
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    file_max_bytes = 10 * 1024 * 1024 # 10MB

    # 화면 출력 로그 생성
    stream_handler = logging.StreamHandler()
    stream_handler.setFormatter(formatter)
    logger.addHandler(stream_handler)

    # 파일 출력 로그 생성
    file_handler = logging.handlers.RotatingFileHandler(
        filename='./my.log',
        maxBytes=file_max_bytes,
        backupCount=10
        )
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)

    # Test
    logger.debug('Debug')
    logger.info('Info')
    logger.warning('Warning')
    logger.error('Error')
    logger.critical('Critical')
```

```python
# coding:utf-8

import json
import sys
import urllib

import shotgun_api3

import logging
import logging.handlers

# 로그인 인스턴스 생성
logger = logging.getLogger('shotgun_ami_log')
logger.setLevel(logging.DEBUG)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
file_max_bytes = 10 * 1024 * 1024 # 10MB

# 화면 출력 로그 생성
stream_handler = logging.StreamHandler()
stream_handler.setFormatter(formatter)
logger.addHandler(stream_handler)

# 파일 출력 로그 생성
file_handler = logging.handlers.RotatingFileHandler(
    filename='D:/ami_test/my.log',
    maxBytes=file_max_bytes,
    backupCount=10
    )
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)

arg = sys.argv[1]
handler, fullPath = arg.split(':', 1)
path, fullArgs = fullPath.split('?', 1)
action = path.strip('/')
args = fullArgs.split('&')
logger.debug('Action is ' + action)
logger.debug('Full arg is ' + fullArgs)
for a in args:
    logger.debug(a)

get_dict = {}
for arg in args:
    key, value = map(urllib.unquote, arg.split('=', 1))
    get_dict[key] = value
    logger.debug(key + ' = ' + value)

if action == 'ami_test1':
    logger.debug(action)
    with open('D:/ami_test/result.txt', 'w') as result:
        sg = shotgun_api3.Shotgun(
            'https://aiw.shotgunstudio.com',
            login='geunmo_kim',
            password='1004kyjkesphw44^^'
        )
        project =  sg.find_one('Project', [])

        try:
            result.write(json.dumps(project, indent=4))
        except:
            result.write(u'Failed.')

if action == 'ami_test2':
    logger.debug(action)
    with open('D:/ami_test/result.txt', 'w') as result:
        result.write(action)
```

## Custom protocol handlers 단점

`custom protocol handlers`를 통한 응용프램 호출에 몇가지 단점이 있습니다.
레지스트리에 호출되는 이름이 고유의 값이야 한다는 것입니다.
만약 다른 응용프로그램과 호출 이름이 같다면 둘 중에 어느것이 호출될지 모릅니다.
또한 브라우저에서 호출시 클라이언트 팝업차에서도 확인 할 수 있듯이 호출 내용을 확인할 수 있습니다. 이런 점은 공격 당하기 쉬운 부분이 될 수 있습니다.

추가적으로 nsis 인스톨러를 사용하시는 분들은 인스톨시에 아래와 같은 방법을 참고하시어 레지스트리에 등록하시면 도움이 될 것같습니다.

WriteRegStr HKCR "&#36;{Appname}" "URL protocl" ""
>WriteRegStr HKCR "&#36;{Appname}&#92;shell" "" "open"
>WriteRegStr HKCR "&#36;{Appname}&#92;shell&#92;open" "" "command"
>WriteRegStr HKCR "&#36;>{Appname}&#92;shell&#92;oepn&#92;command" "" '"path&#92;Appendname.exe" "&#37;1"'