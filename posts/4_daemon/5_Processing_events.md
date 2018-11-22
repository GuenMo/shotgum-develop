# [Processing events (이벤트 처리)](https://youtu.be/YPZ1FWvhf7k?t=13m15s)

이제 콜백 메서드에 등록된 demo 함수를 살펴보겠습니다.

어셋 상태 필드가 변경될 때 이 기능이 실행된다는 것을 이미 알고 있지만 사용 가능한 `sg`, `logger`, `event`, `args` 인수 변수는 아직 확인하지 않았습니다.

`sg`는 키 자격 증명을 사용하여 생성한 샷건 연결 개체입니다. 여기서는 Shotgun Python API 명령을 실행하는 데 사용할 수 있습니다.

`logger`: 터미널의 출력과 로그 파일로 정보를 전송하는 것입니다.

사이트에서 프로젝트를 찾아서 로거로 출력 하겠습니다.

```python
import logging

SGDAEMON_LOGARGS_NAME = "demo.py"
SGDAEMON_LOGARGS_KEY = "xvlx2-okVdinoaxbltqempnjw"

def registerCallbacks(reg):
    eventFilter = {
        "Shotgun_Asset_Change": "sg_status_list"
    }
    reg.registerCallback(
        SGDAEMON_LOGARGS_NAME,
        SGDAEMON_LOGARGS_KEY,
        demo,
        eventFilter,
        None,
    )

    reg.logger.setLevel(logging.DEBUG)


def demo(sg, logger, event, args):
    logger.info(sg.find_one("Project", []))

```

잘 됍니다. 기억 하세요. 이 로거 인스턴스를 사용해서 보고를 받으세요. `print`문을 사용하거나 직접 만든 로거를 사용 하면 안됩니다.

다음은 이벤트 사전입니다. 터미널로 뭐가 보내지는지 봅시다. 플러그인에 출력을 보겠습니다.

```python
'attribute_name': 'sg_status_list'
'created_at': datetime.datetime(2018, 11, 23, 6, 35, 23)
'entity': {'id': 3302, 'name': 'test_asset', 'type': 'Asset'}
'event_type': 'Shotgun_Asset_Change'
'id': 2461595
'meta': {'attribute_name': 'sg_status_list',
            'entity_id': 3302,
            'entity_type': 'Asset',
            'field_data_type': 'status_list',
            'new_value': 'wtg',
            'old_value': 'ip',
            'type': 'attribute_change'},
'project': {'id': 336, 'name': 'Project X', 'type': 'Project'},
'session_uuid': '0b672a96-ee9e-11e8-b79d-0242ac110004',
'type': 'EventLogEntry',
'user': {'id': 169,
         'name': '\xea\xb9\x80 \xea\xb7\xbc\xeb\xaa\xa8',
         'type': 'HumanUser'},
```

참고용 으로 취할 수 있는 데이터가 충분하다는 것을 알 수 있습니다.

어셋의 `sg_status_list` 필드가 `hold`로 설정될 경우 `description` 필드를 업데이트하는 간단한 코드를 작성하겠습니다.

마지막 인수를 사용하여 목표 상태라는 설정을 전달합니다.

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
        {"target_status": "hld"},
    )

    reg.logger.setLevel(logging.DEBUG)


def demo(sg, logger, event, args):
    if event["meta"]["new_value"] == args["target_status"]:
        sg.update(
            event["entity"]["type"],
            event["entity"]["id"],
            {"description": "{} set this {} to {}.".format(
                event["user"]["name"],
                event["entity"]["type"],
                args["target_status"]
            )}
        )
```