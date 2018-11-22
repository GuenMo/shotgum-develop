# [Logging Events (이벤트 로깅)](https://youtu.be/YPZ1FWvhf7k?t=5m11s)

이제 `logArgs.py` 플러그인이 로드되었습니다.

Let's pop it open and take a look.
`logArgs.py`을 열어 봅니다.

먼저 두 가지 환경 변수를 짚어 보겠습니다.

![Local Image](/img/4_daemon/3_Logging_events/5.png)

데몬 자체는 스크립트 키를 의미합니다. 이 스크립트 키는 `EventLog` 변경 사항을 쿼리할 수 있습니다.

하지만 개별 플러그인은 이러한 이벤트의 결과로 동일한 스크립트 키를 사용할 필요가 없습니다.

실제로 각 플러그인은 자체 스크립트 키를 참조해야 합니다.

이러한 방식으로 샷건의 이벤트 로그 항목은 특정 플러그인에 기인할 수 있으며 불량 행위자는 제외되고 디버그될 수 있습니다.

이제 샷건 스크립트 키를 만들어야 겠어요.

![Local Image](/img/4_daemon/3_Logging_events/6.png)

![Local Image](/img/4_daemon/3_Logging_events/7.png)

그런 다음 환경 변수를 내 쉘로 내보냅니다.

```console
setx /m SGDAEMON_LOGARGS_NAME "logArgs.py"
setx /m SGDAEMON_LOGARGS_KEY "#Onslgpcxmfop3kbsuvmioduf"
```

```python
import os
import logging


def registerCallbacks(reg):
    eventFilter = None
    reg.registerCallback(
        os.environ["SGDAEMON_LOGARGS_NAME"],
        os.environ["SGDAEMON_LOGARGS_KEY"],
        logArgs,
        eventFilter,
        None,
    )

    reg.logger.setLevel(logging.DEBUG)


def logArgs(sg, logger, event, args):
    logger.info("%s" % str(event))
```

데모에서는 주석을 제거 하겠습니다.

데몬은 지정된 플러그인 폴더에 있는 파일을 확인하는데, 이 플러그인은 우리가 config 파일에 정의한 것 입니다.

그리고 만약 그 파일들 중 하나에  `registerCallbacks(reg)` 함수가 있다면, 그 기능을 실행할 것입니다.

그래서 그 기능은 뭘까요?

음, 그것의 주된 목적은 실행하는 것 뿐입니다.

`reg.registerCallback` 함수를 보면 4개의 변수를 사용 됐습니다.

1. 인증 자격 증명 집합(스크립트 키 이름, 키값)
2. function 이름(플러그인 이름)
3. 이벤트 필터(eventFilter = None)
4. 마지막으로 추가 인수(선택 사항)입니다.

플러그인에서 조금 더 아래쪽에 레지스터 콜백 호출에서 참조된 로그 args 기능이 표시됩니다.

따라서 본질적으로 이 플러그인 레지스터 콜백 기능은 "야 데몬 내가 스크립트 키가 있는데 이 권한으로 샷건에서 무슨 일이 이어나고 있는지 로고로 남겨줘" 입니다.

이벤트 필터가 없기 때문에 그게 전부 입니다.

데몬을 다시 실행 하고 샷건 웹에서 어셋의 정보를 변경해 봅니다.

```python
python shotgunEventDaemon.py foreground
```

터미널을 확인하여 플러그가 다시 로드되었는지 확인하고 샷건에서 무슨 일이 일어나는지 알아보겠습니다.

로그 디렉토리에 있는 로그 파일을 확인 해봅니다.

이 디렉터리는 conf 파일에서 지정한 디렉터리입니다.

여기서 데몬이 만든 로그 파일을 볼 수 있습니다.

하나는 logArg 플러그인에서 생성된 메시지와 다른 하나는 데몬에서 오는 메시지에 대한 것입니다.

실제로 지금처럼 전경 모드에서 데몬을 사용할 때는 이러한 로그를 거의 확인하지 않지만 나중에 데몬을 시작 모드로 시작하면 이 로그만이 출력되는 유일한 장소 메시지입니다.
