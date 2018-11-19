# [Processing events (이벤트 처리)](https://youtu.be/YPZ1FWvhf7k?t=13m15s)

Next let's look at the demo function that are registered callback method references.
이제 콜백 메서드에 등록된 demo 함수를 살펴보겠습니다.

We already know that this function is run when an asset status field is changed but we haven't yet looked at the available`sg`, `logger`, `event` and `args` argument variables.
어셋 상태 필드가 변경될 때 이 기능이 실행된다는 것을 이미 알고 있지만 사용 가능한 `sg`, `logger`, `event`, `args` 인수 변수는 아직 확인하지 않았습니다.

The `sg` object is fairly straightforward.
`sg` 오브젝트는 꽤 간단합니다.

It's an authenticated shotgun a connection object the daemon creates with the script key credentials that are passed into the register callback function.
이것은 로그인 대몬이 레지스터 콜백 함수에 전달되는 스크립트 키 자격 증명을 사용하여 생성한 인증된 샷건 연결 개체입니다.

Up here we can use it to run shotgun Python API commands.
여기서는 Shotgun Python API 명령을 실행하는 데 사용할 수 있습니다.

Cool. Next we've got a logger instance this is what's sending info to standard output in the terminal and to our log files.
좋습니다. 다음은 로거 인스턴스입니다. 터미널의 표준 출력과 로그 파일로 정보를 전송하는 것입니다.

Let's test these two together.
이 두 가지를 함께 테스트해 봅시다.

I'll search for a project on our site and spit it out with a logger.
나는 우리 사이트에서 프로젝트를 찾겠습니다. 그리고 로거로 출력 됩니다.

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

Great it works.
잘 됍니다.

Remember to use this logger instance when you want to report anything don't use print statements or create your own logger instant.
기억 하세요. 이 로거 인스턴스를 사용해서 보고를 받으세요. `print`문을 사용하거나 직접 만든 로거를 사용 하면 안됩니다.

Next up we have the event dictionary.
다음은 이벤트 사전입니다.

Let's send it out to the terminal to see what's in it.
터미널로 뭐가 보내지는지 봅시다.

I like to copy the output back into the plug-in I'm working on.
작업 중인 플러그인에 출력을 다시 복사하고 싶습니다.

For reference if you take a close look at the output here you'll realize we've already got enough data to take action.
참고를 위해 여기의 출력을 자세히 살펴보면 이미 조치를 취할 수 있는 데이터가 충분하다는 것을 알 수 있습니다.

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
    # attribute_name, sg_status_list
    # event_type, Shotgun_Asset_Change
    # created_at, 2018 - 11 - 14
    # 04:04:13 + 0
    # 9:00
    # entity, {'type': 'Asset', 'id': 3302, 'name': 'AAA'}
    # project, {'type': 'Project', 'id': 336, 'name': 'Project X'}
    # meta, {'entity_id': 3302, 'attribute_name': 'sg_status_list', 'entity_type': 'Asset', 'old_value': 'omt',
    #        'field_data_type': 'status_list', 'new_value': 'ip', 'type': 'attribute_change'}
    # user, {'type': 'HumanUser', 'id': 169, 'name': '\xea\xb7\xbc\xeb\xaa\xa8 \xea\xb9\x80'}
    # session_uuid, 560803
    # a4 - e76e - 11e8 - b528 - 0242
    # ac110004
    # type, EventLogEntry
    # id, 2445666
    for k, v in event.iteritems():
        logger.info("%s, %s" % (k, v))
```

Let's write a simple bit of code that updates an assets description field when it's status field is set to on hold.
상태 필드가 보류 상태로 설정된 경우 자산 설명 필드를 업데이트하는 간단한 코드를 작성하겠습니다.

I'll use the register callback methods last argument to pass in a setting called target status.
마지막 인수를 사용하여 목표 상태라는 설정을 전달합니다.

I like to define static variable settings in the register callbacks function instead of cluttering the function body.
저는 기능 본문을 복잡하게 만드는 대신 레지스터 콜백 기능에 정적 변수 설정을 정의하는 것을 좋아합니다.

Pretty straightforward right.
아주 간단하죠.

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
        {"target_status": "hld"},
    )

    reg.logger.setLevel(logging.DEBUG)


def demo(sg, logger, event, args):
    # attribute_name, sg_status_list
    # event_type, Shotgun_Asset_Change
    # created_at, 2018 - 11 - 14
    # 04:04:13 + 0
    # 9:00
    # entity, {'type': 'Asset', 'id': 3302, 'name': 'AAA'}
    # project, {'type': 'Project', 'id': 336, 'name': 'Project X'}
    # meta, {'entity_id': 3302, 'attribute_name': 'sg_status_list', 'entity_type': 'Asset', 'old_value': 'omt',
    #        'field_data_type': 'status_list', 'new_value': 'ip', 'type': 'attribute_change'}
    # user, {'type': 'HumanUser', 'id': 169, 'name': '\xea\xb7\xbc\xeb\xaa\xa8 \xea\xb9\x80'}
    # session_uuid, 560803
    # a4 - e76e - 11e8 - b528 - 0242
    # ac110004
    # type, EventLogEntry
    # id, 2445666
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

Let's see if it works.
효과가 있는지 봅시다.

Amazing.
신기하네요.

What's that feeling in your chest you ask. That's adrenaline. Your heart is racing.
가슴 속의 그 느낌이 어때요? 아드레날린이야. 심장이 뛰어요.

You're starting to realize the Damon's potential you're itching to write thousands of lines of code to  impress your boss your family and friends.
당신은 당신의 상사에게 당신의 가족과 친구들을 감동시키기 위해 수천줄의 코드를 쓰고 싶어하는 데몬의 잠재력을 깨닫기 시작했다.

That's great now take a deep breath try to calm down and finish watching this video.
이제 심호흡을 해서 진정하고 비디오 보는 것을 끝내세요.

If you stop now you run the risk of wielding great power irresponsibly.
당신이 지금 멈추면 당신은 무책임하게 권력을 휘두르는 위험을 무릅쓰게 됩니다.