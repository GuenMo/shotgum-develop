# [Validation (유효성 확인)](https://youtu.be/YPZ1FWvhf7k?t=16m10s)

이벤트 사전을 좀 더 자세히 살펴봅시다.

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

우리가 발생한 이벤트에 대해 알고 싶어할 수 있는 거의 모든 것이 있습니다.

하지만 한 가지 부족한 정보가 있습니다.

이벤트 로그 엔티티 설명입니다.

```python
sg_event = sg.fine_one(
        "EventLogEntry",
        [["id", "is", event["id"]]],
        ["description"]
    )
```

There's an added bonus to doing that.
그렇게 하면 보너스가 `EventLogEntry`의 `description` 필드 값을 얻었습니다.

We can verify that the `EventLogEntry` entity still exists in shotgun and not just in the daemon's cached event backlog.
`EventLogEntry` 엔티티가 데몬의 캐시된 이벤트 백로그에서  존재 하는지 아닌지 확인할 수 있습니다.

만약 이벤트가 일어나지 않는다면 나는 여기서 작업을 중지 합니다(return).

```python
if not sg_event:
    logger.warning("Could not find event with id {}, skipping".format(envet_id))
    return
```

하지만 어떻게 그런 일이 일어날까요?.

이벤트 로그 항목이 항상 존재해야 하지 않을까요?

음, 그 사건은 드문 경우에 샷건에서 일어나지 않을 수도 있습니다.

예를 들어 사이트에서 백업본이 복원되거나 관리자가 삭제 이벤트 로그 항목 엔티티가 기록하는 경우 입니다.

너무 신중하다고 생각 할수 있습니다.

하지만 데몬은 문자 그대로 샷건에서 일어나는 모든 일을 영원히 스캔하기 때문입니다.

그래서 우리는 이런 상황도 준비해야 합니다.

You might want to think of this extra layer of paranoia as the first rule of event club.
이벤트 클럽의 첫 번째 규칙으로 편집증의 추가 레이어를 생각할 수도 있습니다.

수천, 수만 건의 이벤트들이 매달 처리됩니다.

이벤트 클럽의 정신에서 여러분은 제가 "Dictionary Get" 방법으로 이벤트 키를 쿼리하는 것을 볼 수 있습니다.

이벤트 ID값을 가져올때 키값을 사용하지 않고 `get`을 사용합니다. 값이 없을 경어 에러가 아니라 `None` 값을 반환 하기 때문 입니다.

```python
event_id = event.get("id")
sg_event = sg.find_one(
    "EventLogEntry",
    [["id", "is", event_id]],
    ["description"]
)

if not sg_event:
    logger.warning("Could not find event with id {}, skipping".format(event_id))
    return

logger.info("Running this plugin because {}".format(sg_event["description"]))
```

예를 들어 여기서 `event_id`가 none이면 `sg_event`는 none을 반환하고, `sg_event` 변수가 none이 되면 진행은 멈춥니다.

우리가 편집증적으로 생각해야 할 다른 정보와 사건 사전에 대한 생각은 없나요?

목록을 살펴 봅시다.

첫째, 모든 키가 존재 하는지 확인하는 것입니다. 편리한 변수를 만듭니다.

좋습니다. 이것들은 제가 사용하고자 하는 데이터들입니다. 이들중 `None`이 있으면 진행은 멈춥니다.

```python
event_id = event.get("id")
field_name = event.get("attribute_name")
entity = event.get("entity")
project = event.get("project")
new_value = event.get("meta", {}).get("new_value")
user = event.get("user")

if None in [event_id, field_name, entity, project, new_value, user]:
    logger.warning("Missing info in event dictionary, skipping.")
    return
```

다음으로 `sg_status_list` 필드가 에셋 스키마에 있음을 확인 하는것 입니다..

그것은 편집증의 매우 극단적인 예이다. 그렇지만 모두 첵크리트에 추가하세요.

The schema check can be especially useful if your plugin acts on a custom field across multiple entities.
스키마 검사는 플러그인이 여러 엔티티에서 사용자 지정 필드에서 작동하는 경우에 특히 유용할 수 있습니다.

It's quite possible an issue with the schema will crop up.
키마에 문제가 생길 가능성이 아주 높습니다.

And now that we've got the schema we can check to make sure the hold status is a valid option.
이제 스키마를 확보했으므로 보류 상태가 유효한 옵션인지 확인할 수 있습니다.

```python
# Make sure field_name is still in our entity schema.
try:
    entity_schema = sg.schema_field_read(
        entity["type"],
        project_entity=project,
        field_name=field_name
    )
except Exception, e:
    logger.warning(
        "{} does not exist in % schema, skipping: {}".forma(
            field_name,
            entity["type"],
            e.message
        )
    )
    return
# Make sure args["target_status"] is in the entity schema.
if args["target_status"] not in entity_schema["sg_status_list"]["properties"]["valid_values"]["value"]:
    logger.warning(
        "{} is not in %s schema, plugin will never execute.".format(
            args["target_status"],
            entity["type"]
        )
    )
    return
```

다음은 엔티티가 존재한다는 것을 확인 했습니다.

그것은 폐기되었을지도 모른다.

그러니 다시 한번 물어보고 만약 없다면 종료 합니다.

```python
# Re-query our entity for updated filed values.
entity = sg.find_one(entity["type"], entity["id"], [field_name])

# Bail if our entity doesn't exist.
if not entity:
    logger.warning(
        "{} with id {} does not exits, skipping.".format(
            entity["type"],
            entity["id"]
        )
    )
    return
```

다음으로, 이벤트 메타데이터 안에 속성의 값은 어떻게 됩니까? 상태 필드가 준비 상태에서 대기 중으로 변경되었음을 나타냅니다.

하지만 그것은 과거에 특정한 시간에 일어났고 그 이후로 상황이 바뀌었을 수도 있고 심지어 이벤트가 많이 쌓이면 몇 초가 지나거나 심지어 몇 시간이 지나도 바뀔 수도 있다.

데몬은 이벤트를 순차적으로 처리합니다.

And if the daemon was accidentally shut down we could be talking days.
그리고 만약 데몬이 실수로 셧다운되었다면 우리는 며칠 동안 이야기를 하고 있을 수 있습니다.

이러한 경우 상태 필드의 값이 더 이상 대기 중이 아닐 수 있습니다.

So our code should not assume it is and we should require a shotgun and verify any info we plan to act on.
그러므로 우리의 코드는 그것을 가정해서는 안 되며 우리는 샷건을 필요로 하고 우리가 행동하려는 어떤 정보라도 확인해야 한다.

첵크 기능을 추가 하겠습니다.

```python
# Bail if our entity's field value has changed (is not new_value).
if not entity[field_name] == new_value:
    logger.warning(
        "{} with id {}'s {} has changed from {} since event inception.".format(
            entity["type"],
            entity["id"],
            field_name,
            new_value
        )
    )
    return

# Todo: this check should really go at the top.
if not new_value == args["target_status"]:
    logger.debug("new_value is not {}, skipping.".format(args["target_status"]))
    return
```

우리는 우리를 보호하기 위해 편집증적인 검사 코드를 만들었습니다.

Might as well clean up our original call to update the assets description field.
에셋의 `description` 필드 에 내용이 없을 때만 업데이트 하게 만듭니다. 

```python
# Add a description if our entity's new_value is "hld" and description is None.
if not entity["description"]:
    if event["meta"]["new_value"] == args["target_status"]:
        logger.info("Updating {} with {}...".format(entity["type"], entity["id"]))

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

이 코드에서 몇 가지 전략을 짚고 넘어가겠습니다.

You will remember my copious use of the get method up here to avoid key errors.
당신은 내가 핵심 오류를 피하기 위해 여기서 얻는 방법을 많이 사용한 것을 기억할 것이다.

`new_value`와 함께 사용한 방식을 살펴보십시오.

중첩된 키를 찾는 경우 상위 키를 빈 사전으로 기본 설정할 것을 기억하거나 특성 오류가 발생합니다.

마지막으로 이러한 검증자가 성능에 미치는 영향이 궁금할 수 있습니다.

플러그인을 테스트하여 검증자의 이점이 성능 저하에 비해 크게 다르지 않은지 확인하시기 바랍니다.

이 플러그인은 말 그대로 실행되고 있고 이 점검이 성능에 영향을 미친다는 것을 누군가가 증명하지 않는 한, 나는 여기서 작성한 코드를 고수할 가치가 있다.

또는 일부 검증자를 콜백 레지스터 기능으로 이동할 수도 있습니다.

이렇게 하면 플러그인이 로드될 때 한 번만 실행됩니다.

플러그 인이 이벤트에 의해 트리거될 때마다 대신 내가 방금 작성한 어떤 수표도 사용할 수 있습니다.

<https://github.com/shotgunsoftware/shotgunEvents/blob/master/src/examplePlugins/entity_status_update_task_status.py>
