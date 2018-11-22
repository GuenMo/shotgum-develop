# [Filering for events (이벤트 필터링)](https://youtu.be/YPZ1FWvhf7k?t=8m9s)

이 시점에서 데몬을 다운로드하고 방법, 설정하는 방법, 시작하는 방법 그리고 `logArgs.py` 플러그인을 추가하고 테스트하는 방법을 이해해야 합니다.

다음으로 `logArgs.py` 아주 단순고 필요없는 플러그인 입니다.

이 플러그인은 샷건에서 무슨 일이 일어날 때마다 작동하며 실제로 아무것도 하지 않습니다.

그러면 샷건에서 특별한 일이 일어날 때만 플러그 인이 작동하도록 하려면 어떻게 해야 할까요?

누군가 새 샷을 만들거나 어셋 상태에 바꾸거나 하는 경우와 같이 말이죠.

`reg.registerCallback` 함수에 네번째 인수인 `eventFilter` 수정하면 됩니다.

그렇게 하기 전에 이 플러그인의 복사본을 만들어 `demo.py`라고 부르는 게 어때요?
그 다음 `logArgs` 함수를 `demo`로 바꿉니다.

![Local Image](/img/4_daemon/4_Filtering_for_events/8.png)

샷건에서 새 스크립트 키를 만들고 환경 변수에 추가 합니다.

![Local Image](/img/4_daemon/4_Filtering_for_events/9.png)

`eventFilter`는 키와 값으로된 포함된 사전입니다. 여기서 키는 이벤트의 유형이고 값은 엔터티의 필드 또는 필드입니다.

데몬이 기존에 어셋에 어떤 변화가 발생 할 때마다 실행 한다고 가정 해봅시다.

`eventFilter`에 "Shotgun_Asset_Change" 라고 설정만 하면 됩니다.

```python
import os
import logging

def registerCallbacks(reg):
    eventFilter = {
        "Shotgun_Asset_Change": None
    }
    reg.registerCallback(
        os.environ["SGDAEMON_DEMO_NAME"],
        os.environ["SGDAEMON_DEMO_KEY"],
        demo,
        eventFilter,
        None,
    )
    reg.logger.setLevel(logging.DEBUG)

def demo(sg, logger, event, args):
    logger.info("%s" % str(event))
```

어떻게 어셋에 대한 변경을 `Shotgun_Asset_Change`이라고 부를지 알았을까요?

그 질문에 답하기 위해 우리 샷건 사이트의 `All Event Log Entries`라는 페이지를 봅시다.

아직 `All Event Log Entries` 페이지를 작성하지 않은 경우, 이벤트 로그 항목으로 엔티티 유형을 설정하고 프로젝트 필드를 비워 두면 됩니다.

![Local Image](/img/4_daemon/4_Filtering_for_events/11.png)

이제 우리는 샷건 내부에 했던 모든 변화를 알수 있습니다.

여기에 변화, 사용자 또는 스크립트가 기록됩니다.

![Local Image](/img/4_daemon/4_Filtering_for_events/12.png)

Let's test this I'll open the shots page in my project and make a change.
이 테스트를 해보겠습니다. 프로젝트의 샷 페이지를 열고 변경하겠습니다.

`logArgs.py` 플러그인이 여전히 활성 상태입니다. 플러그인이 변화를 보고했다.

데몬을 종료하고 `disabled` 폴더를 만들고 `logArgs.py`와 `logArgs.pyc`를 이동 시킵니다.

![Local Image](/img/4_daemon/4_Filtering_for_events/13.png)

이제 `demo.py` 플러그인만 로드한 데몬을 실행 시킵니다. 그리고 샷 엔티티를 다시 변경 합니다.

좋아요. 아무 일도 일어나지 않아요. 하지만 어셋 실체를 변경하면 log가 기록 됩니다.

제가 방금 한 것처럼 플러그 인을 폴더에 넣은 다음 나중에 꺼내면 데몬이 혼동될 수 있습니다.

여기서 `shotgunEventDaemon.id` 파일을 빨리 삭제합니다.

물론 그것은 당신의 이벤트 큐를 날려버릴 것이지만 그것은 대개 개발 중에 문제가 되지 않는다.

![Local Image](/img/4_daemon/4_Filtering_for_events/14.png)

어셋 엔티티의 상태 필드가 변경될 때만 플러그인이 실행되도록 하려면 여기에 해당 필드의 내부 코드를 추가 하면 됩니다.

무슨 일이 일어나고 있는지 좀 더 자세한 정보를 주기 위해 로거 메시지를 바꾸겠습니다.

```python
import logging

def registerCallbacks(reg):
    eventFilter = {
        "Shotgun_Asset_Change": "sg_status_list"
    }
    reg.registerCallback(
        os.environ["SGDAEMON_DEMO_NAME"],
        os.environ["SGDAEMON_DEMO_KEY"],
        demo,
        eventFilter,
        None,
    )

    reg.logger.setLevel(logging.DEBUG)


def demo(sg, logger, event, args):
    logger.info(
            sg.find_one(
                "EventLogEntry",
                [["id", "is", event["id"]]],
                ["description"]
            )["description"]
    )
```

![Local Image](/img/4_daemon/4_Filtering_for_events/15.png)

여러 개의 필드를 대상으로 지정하려면 이 필드를 목록으로 변경할 수 있습니다.

```python
eventFilter = {
        "Shotgun_Asset_Change": ["sg_status_list", "description"]
    }
```

이제 상태 또는 설명 필드가 변경되면 플러그인이 실행됩니다.

다른 이벤트 유형에 대해 여기에 다른 항목을 추가할 수도 있습니다.

```python
eventFilter = {
        "Shotgun_Asset_Change": ["sg_status_list", "description"],
        "Shotgun_Shot_Change": ["code", "sg_status_list"]
    }
```

이벤트에 대해 필터링하는 방법에 대해 궁금한 경우, GUI에서 사용자가 이벤트 로그 항목을 생성한 다음 모든 이벤트 페이지에서 이벤트 유형 구문을 확인합니다.

이 데몬의 나머지 부분에서는 `sg_status_list` 변경 사항에 초점을 맞추기 위해 코드를 되돌리겠습니다.

```python
eventFilter = {
        "Shotgun_Asset_Change": "sg_status_list"
    }
```

다음 단계로 넘어가기 전에 마지막으로.

여기에서 레지스터 콜백 함수를 두 개 이상 등록할 수 있습니다.

예를 들어 이 블록 변경을 이벤트 필터 위로 복사한 다음 파일에서 다른 기능을 가리킬 수 있습니다.

```python
def registerCallbacks(reg):
    eventFilter = {
        "Shotgun_Asset_Change": "sg_status_list"
    }

    reg.registerCallback(
        os.environ["SGDAEMON_DEMO_NAME"],
        os.environ["SGDAEMON_DEMO_KEY"],
        demo,
        eventFilter,
        None,
    )

    reg.registerCallback(
        os.environ["SGDAEMON_DEMO_NAME"],
        os.environ["SGDAEMON_DEMO_KEY"],
        demo,
        {"Shotgun_Shot_Change": "code"},
        None,
    )

    reg.logger.setLevel(logging.DEBUG)
```

왜 그렇게 할까요?

Well if you've got very similar plugins and you want the share code without importing modules you could consolidate them this way.
매우 비슷한 플러그인있고 코드를 공유 하고 싶다면 이러한 방식으로 통합할 수 있습니다.

실제로 그렇게 자주 나오는 것은 아니다.

Don't be surprised if you only ever use a single register call back call per register callbacks function.
레지스터 콜백당 하나의 레지스터 콜백 기능만 사용하더라도 놀라지 마십시오.

```python
import os
import logging


def registerCallbacks(reg):
    eventFilter = {
        "Shotgun_Asset_Change": "sg_status_list"
    }

    reg.registerCallback(
        os.environ["SGDAEMON_DEMO_NAME"],
        os.environ["SGDAEMON_DEMO_KEY"],
        demo,
        eventFilter,
        None,
    )

    reg.registerCallback(
        os.environ["SGDAEMON_DEMO_NAME"],
        os.environ["SGDAEMON_DEMO_KEY"],
        demo2,
        {"Shotgun_Shot_Change": "code"},
        None,
    )

    reg.logger.setLevel(logging.DEBUG)


def demo(sg, logger, event, args):
    logger.info(
        sg.find_one(
            "EventLogEntry",
            [["id", "is", event["id"]]],
            ["description"]
        )["description"]
    )


def demo2(sg, logger, event, args):
    logger.info(
        sg.find_one(
            "EventLogEntry",
            [["id", "is", event["id"]]],
            ["description"]
        )["description"]
    )
```