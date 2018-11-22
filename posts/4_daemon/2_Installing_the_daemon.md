# [Installing the daemon (데몬 설치)](https://youtu.be/YPZ1FWvhf7k?t=1m28s)

데몬을 다운로드하려면 github에서 샷건 소프트웨어를 방문하십시오.
<https://github.com/shotgunsoftware?utf8=%E2%9C%93&q=&type=&language=>

`shotgun events`를 검색하여 데몬을 실행할 컴퓨터에 복제합니다.

제 경우에는 제 노트북에서 설치 할 거에요.

```console
git clone https://github.com/shotgunsoftware/shotgunEvents.git
```

![Local Image](/img/4_daemon/2_Installing_the_daemon/1.png)

But in production you'll want to choose a reliable server that you can access easily and that will fingers crossed be up all the time.
하지만 프로덕션 환경에서는 쉽게 액세스할 수 있고 항상 서로 교차하는 신뢰할 수 있는 서버를 선택해야 합니다.

`shotgunEventDaemon.conf.example`을 `shotgunEventDaemon.conf`로 복사 합니다.

사용자에게 폴더와 로그 생성 권한이 없는 것 같습니다.

이 로그 파일과 ID 파일이 서버에 저장될 위치를 결정하기 위해 잠시 시간을 할애해 보겠습니다. PID 파일과 이벤트 ID 파일이 나중에 어떤 것인지는 걱정하지 마십시오.

다음으로 로깅 수준을 10으로 변경합니다.

이렇게 하면 데몬을 개발하거나 학습하는 데 유용한 보다 많은 로그 출력이 제공됩니다.

```python
# src/shotgunEventDaemon.conf

pidFile: I:/WorkSpace/shotgun_projects/shotgun_daemon/shotgunEventDaemon.pid
eventIdFile: I:/WorkSpace/shotgun_projects/shotgun_daemon/shotgunEventDaemon.id
logPath: I:/WorkSpace/shotgun_projects/shotgun_daemon/logs
logging: 10
```

데몬과 데몬의 기능에 대해 더 잘 알게 되면 나중에 20개로 바꿀 수 있습니다.

다음으로 중요한 것은 샷건 연결 정보입니다.

사이트를 스와핑하고 스크립트 이름과 액세스 키를 제공합니다.

샷건 페이지에서 스크립트 키를 생성하고 그 키값을 넣습니다.

![Local Image](/img/4_daemon/2_Installing_the_daemon/2.png)

![Local Image](/img/4_daemon/2_Installing_the_daemon/3.png)

```python
server: https://aiw.shotgunstudio.com
key: **************************
```

이 작업이 완료되면 플러그인 폴더의 경로를 설정합니다.

```python
paths: I:/WorkSpace/shotgun_projects/shotgun_daemon/plugins
```

제 경우에는 바탕 화면을 가리키지만, 이 폴더를 사용할 위치를 결정하십시오.

어떤 사람들은 그것을 샷건에 다시 넣지만 나는 보통 사람들에게 그것을 다른 곳에 두라고 말합니다.

이렇게 하면 Git 작업과 샥건 이벤트 및 플러그인 폴더가 Git Reo가 될 수 있습니다.

이것이 시작하기 위한 최소 요구 사항입니다.

나중에 돌아와서 다른 설정을 확인하겠습니다. 하지만 지금은 데몬을 작동시켜 어떻게 되는지 보겠습니다.

`src` 폴더로 이동 한후 `shotgunEventDaemon.py`를 실행 시킵니다.

좋습니다. 몇 가지 옵션이 있습니다.

데몬을 시작 하면 로그 파일에 출력을 보내지만 터미널에는 아무 것도 보내지 않습니다.

대신 개발할 때 `foreground` 옵션을 사용하는 것이 좋습니다.

여전히 로그로 이동하지만 메시지는 표준 출력으로 전송되어 터미널에 표시됩니다.

먼저 바탕 화면에 해당 플러그인 폴더를 만들어야 합니다.

다시 해 봅시다. 이제 Python API에 대해 불평하고 있습니다.

만약 우리가 Github repo로 돌아가서 Wiki로 간다면, 우리는 정말로 python api가 필요한 것을 알 수 있다.

><https://github.com/shotgunsoftware/shotgunEvents/wiki/Installation>
> `pip install pypiwin32`

샷건 이벤트 데몬을 위한 새로운 가상 환경을 만들어 Python API를 설치하겠습니다.

이제 데몬을 다시 시작하겠습니다.

플러그 인의 빈 폴더와 관련된 경고가 표시되는 것 같습니다.

자 이제 샷건 이벤트 원본 폴더에 있는 다른 플러그 인이 필요합니다.

지금까지 `shotgunEventDaemon.conf`을 살펴보고 `shotgunEventDaemon.py` 파일을 실행하여 데몬을 시작했습니다.

```python
(venv) python shotgunEventDaemon.py foreground
```

보세요! 몇 가지 예제 플러그가 있습니다. 도움이 되네요.

![Local Image](/img/4_daemon/2_Installing_the_daemon/4.png)

'logArg.py'를 플러그인 폴더에 복사 합니다.

이제 데몬을 다시 시작하겠습니다.

플러그인이 로드된 것처럼 보입니다.

이 시점에서 샷건 이벤트 데몬이 성공적으로 설치되었음을 고려할 수 있습니다.

이제 플러그인을 쓰는 방법에 대해 알아보겠습니다.
