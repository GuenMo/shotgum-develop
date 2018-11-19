# [Validation (유효성 확인)](https://youtu.be/YPZ1FWvhf7k?t=16m10s)

Let's take a closer look at the event dictionary.
이벤트 사전을 좀 더 자세히 살펴봅시다.

We've got the field that the user changed.
사용자가 변경한 필드가 있습니다.

The event type when the event happened the entity and project.
이벤트가 발생한 경우의 이벤트 유형입니다.

The event is linked to some super useful albeit somewhat redundant metadata info the user responsible for the event a session UUID and the ID of the event log entry entity.
이 이벤트는 세션 UUID 및 이벤트 로그 항목 엔티티의 ID를 담당하는 사용자가 다소 중복되지만 매우 유용한 메타데이터 정보에 연결됩니다.

Pretty much everything we could possibly want to know about event.
우리가 사건에 대해 알고 싶어할 수 있는 거의 모든 것.

But there is one bit of missing info.
하지만 한 가지 부족한 정보가 있습니다.

The `EventLogEntity` description which we can pull down easily enough by querying the event.
이벤트 로그 엔티티 설명입니다.

```python
sg_event = sg.fine_one(
        "EventLogEntry",
        [["id", "is", event["id"]]],
        ["description"]
    )
```

There's an added bonus to doing that.
그렇게 하면 보너스가 더 생긴다.

We can verify that the `EventLogEntry` entity still exists in shotgun and not just in the daemon's cached event backlog.
`EventLogEntry` 엔티티가 데몬의 캐시된 이벤트 백로그에서  존재 하는지 아닌지 확인할 수 있습니다.

So I'll do that here and bail if the event doesn't exist in shotgun.
그래서 만약 그 사건이 샷건에서 일어나지 않는다면 나는 여기서 보석으로 풀려날 것이다.

```python
if not sg_event:
    logger.warning("Could not find event with id {}, skipping".format(envet_id))
    return
```

But how would that happen.
하지만 어떻게 그런 일이 일어날까요.

Shouldn't the event log entry always exist?
이벤트 로그 항목이 항상 존재해야 하지 않습니까?

Well the event may not exist in shotgun in rare cases.
음, 그 사건은 드문 경우에 산탄총에서 일어나지 않을 수도 있습니다.

If the site is restored from a backup for example or if an admin goes out of their way its blow away event log entry entity records.
예를 들어 사이트에서 백업본이 복원되거나 관리자가 삭제 이벤트 로그 항목 엔티티가 기록하는 경우 입니다.

What I'm doing here might seem overly cautious to you.
내가 여기서 하는 일은 너에게 지나치게 신중한 것 같다.

But since the daemon literally scans for everything that happens in shotgun every day forever into the future.
하지만 데몬은 문자 그대로 샷건에서 일어나는 모든 일을 앞으로 영원히 스캔하기 때문입니다.

It does a pretty good job of running into any and every bad Apple edge case.
애플사의 모든 나쁜 상황에 맞서는 것은 꽤 잘한 일이다.

So we should try to account for them.
그래서 우리는 그들을 설명하려고 노력해야합니다.

You might want to think of this extra layer of paranoia as the first rule of event club.
이벤트 클럽의 첫 번째 규칙으로 편집증의 추가 레이어를 생각할 수도 있습니다.

Don't take anything for granted.
당연하게 여겨서는 안됩니다.

Just because the event dictionary came back nice and clean when we tested the name in earlier that doesn't mean and will come back clean each and every time the daemon processes an event.
이벤트 사전이 훌륭하고 깨끗해 졌기 때문에 이전에 이름을 테스트했을 때 데몬이 이벤트를 처리 할 때마다 항상 깨끗하게 돌아올 것입니다.

Thousands, hundreds of thousands and potentially millions of events are processed monthly.
수천, 수만 건의 이벤트들이 매달 처리된다.

Fan can hit the paddle without a creak.
팬은 삐걱거리지 않고 노를 칠 수 있다.

In the spirit of event club you can see that I've used at the dictionary `get` method to query for event keys in case they're missing.
이벤트 클럽의 정신에서 여러분은 제가 "Dictionary Get" 방법으로 이벤트 키를 쿼리하는 것을 볼 수 있습니다.

And I've also made sure that if event ID ends up being none that there will be no repercussions in the code.
또한 이벤트 ID가 `None`이라도 코드에 영향을 미치지 않을 것이라고 확신합니다. 실종되었을 경우를 대비해서요

```python
event_id = event.get("id")
sg_event = sg.fine_one(
    "EventLogEntry",
    [["id", "is", event_id]],
    ["description"]
)

if not sg_event:
    logger.warning("Could not find event with id {}, skipping".format(event_id))
    return

logger.info("Running this plugin because {}".format(sg_event["description"]))
```

For example here if the ID is none the API will simply return none and in that case if the SG event variable ends up being none then we bail.
예를 들어 여기서 `event_id`가 none이면 `sg_event`는 none을 반환하고, `sg_event` 변수가 none이 되면 진행은 멈춥니다.

Any ideas about other info and the event dictionary we should be paranoid about?
우리가 편집증적으로 생각해야 할 다른 정보와 사건 사전에 대한 생각은 없나요?

Let's go down the list.
목록을 살펴 봅시다.

First are we sure all the keys were expecting exist.
첫째, 모든 키가 존재한다고 확신하는 것입니다.

Let's check anything we might use.
우리가 사용할 수 있는 것은 모두 확인해 봅시다.

And make some convenient variables while we're at it.
우리가 하는 동안 편리한 변수를 만듭니다.

Okay those are bits of data I want to use. If any of them come back as none we bail.
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

Next are we sure the `sg_status_list` field is still in the asset schema.
다음으로 SG 상태 목록 필드가 여전히 자산 스키마에 있음을 확인합니다.

Surely I'm joking. Right? Yes.
물론 농담입니다. 그렇죠? 맞습니다.

That's a pretty extreme example of paranoia.
그것은 편집증의 매우 극단적인 예이다.

But all added in a check. Anyway for kicks.
그렇지만 모두 첵크리트에 추가하세요.

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

Cool.

Next are we sure our entity still exists.
다음은 엔티티가 존재한다는 것을 확인 했습니다.

It may have been trashed.
그것은 폐기되었을지도 모른다.

So let's query that again and bail if it's missing.
그러니 다시 한번 물어보고 만약 없다면 종료 합니다.

We could also check whether or not it's been moved to a different project if that matters.
만약 그것이 중요하다면 다른 프로젝트로 옮겨졌는지도 확인할 수 있습니다.

But I'll skip that one got to draw the line somewhere.
하지만 어디선가 선을 그어야 할 사람은 건너뛰겠습니다.

```python
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

Next what about the current value of the attribute for example this events metadata tells us that the status field was changed from ready to waiting.
다음으로, 이 이벤트 메타데이터와 같은 속성의 현재 값은 어떻게 됩니까? 상태 필드가 준비 상태에서 대기 중으로 변경되었음을 나타냅니다.

But that happened at a specific time in the past and things may have changed since then seconds minutes or even hours can pass if a backlog of events builds up.
하지만 그것은 과거에 특정한 시간에 일어났고 그 이후로 상황이 바뀌었을 수도 있고 심지어 사건의 잔고가 쌓이면 몇 초가 지나거나 심지어 몇 시간이 지나도 바뀔 수도 있다.

Remember the daemon processes events sequentially.
데몬은 이벤트를 순차적으로 처리합니다.

And if the daemon was accidentally shut down we could be talking days.
그리고 만약 데몬이 실수로 셧다운되었다면 우리는 며칠 동안 이야기를 하고 있을 수 있습니다.

In any of those cases the value of the status field might not be waiting anymore.
이러한 경우 상태 필드의 값이 더 이상 대기 중이 아닐 수 있습니다.

So our code should not assume it is and we should require a shotgun and verify any info we plan to act on.
그러므로 우리의 코드는 그것을 가정해서는 안 되며 우리는 샷건을 필요로 하고 우리가 행동하려는 어떤 정보라도 확인해야 한다.

So I'll add a check for that here.
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

Okay. I'm starting to feel a little better.
좋아, 좀 나아지기 시작했습니다.

We've built a little paranoid rampart to protect us from the uncouth maelstrom of events boiling in our shotgun sights database.
우리는 우리를 보호하기 위해 편집증적인 경적을 만들었습니다.

Might as well clean up our original call to update the assets description field.
`assets description field`를 업데이트하기 위해 원래 호출를 정리하는 것이 좋습니다.

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

I'll point out a couple strategies in the code here before we move on.
이 코드에서 몇 가지 전략을 짚고 넘어가겠습니다.

You will remember my copious use of the get method up here to avoid key errors.
당신은 내가 핵심 오류를 피하기 위해 여기서 얻는 방법을 많이 사용한 것을 기억할 것이다.

Take a look at the way we used it with `new_value`.
`new_value`와 함께 사용한 방식을 살펴보십시오.

If you're going after nested keys remember to default the parent keys to an empty dictionary or you'll get an attribute error.
중첩된 키를 찾는 경우 상위 키를 빈 사전으로 기본 설정할 것을 기억하거나 특성 오류가 발생합니다.

Finally you may wonder what the performance impact of these validators is.
마지막으로 이러한 검증자가 성능에 미치는 영향이 궁금할 수 있습니다.

I encourage you to test and time your plugin to make sure the benefits of the validators aren't outweighed by performance hits.
플러그인을 테스트하여 검증자의 이점이 성능 저하에 비해 크게 다르지 않은지 확인하시기 바랍니다.

For what it's worth I'd stand by the code I've written here unless someone proved to me this plug-in was running literally all the time and these checks affected performance.
이 플러그인은 말 그대로 실행되고 있고 이 점검이 성능에 영향을 미친다는 것을 누군가가 증명하지 않는 한, 나는 여기서 작성한 코드를 고수할 가치가 있다.

Then I might remove them to speed things along.
그럼 내가 그것들을 제거해서 속도를 높일지도 몰라.

Or I might try to move some of validators to the register callbacks function.
또는 일부 검증자를 콜백 레지스터 기능으로 이동할 수도 있습니다.

That way they'd only run once when the plugin was loaded.
이렇게 하면 플러그인이 로드될 때 한 번만 실행됩니다.

Instead of every time the plugin was triggered by an event can we do that with any of the checks I just wrote.
플러그 인이 이벤트에 의해 트리거될 때마다 대신 내가 방금 작성한 어떤 수표도 사용할 수 있습니다.

Aha! Let's try it with the asset status fueled schema check.
아하! 자산 상태에 따라 스키마 점검을 해보자.

A cut and paste it out into its own validation function called is valid and then call it from register callbacks with a dedicated shotgun handle.
잘라낸 후 이를 소위 말하는 자체 검증 기능에 붙여넣은 다음 전용 산탄총 손잡이를 사용하여 레지스터 콜백에서 호출합니다.

Here's an additional benefit of putting validators in the register callbacks function.
다음은 검증자를 레지스터 콜백 함수에 넣는 추가 이점입니다.

They run when you load the plug-in.
플러그 인을 로드할 때 실행됩니다.

So issues with the plug-in crop up right away.
따라서 플러그인의 문제가 즉시 발생합니다.

And we can even decide not to register the callback at all if the validation fails.
그리고 검증에 실패해도 콜백을 전혀 등록하지 않기로 결정할 수도 있습니다.

I'm also going to take the opportunity to refactor a bit here.
저는 또한 여기서 조금 반박할 기회를 가질 것입니다.

Don't worry if you don't follow all of this.
이 모든 것을 따르지 않더라도 걱정하지 마세요.

The code in this `demo.py` plug-in is available for download.
이 `demo.py` 플러그인을 다운로드할 수 있습니다.

So you can take a closer look.
그래서 좀 더 자세히 볼 수 있습니다.

<https://github.com/shotgunsoftware/shotgunEvents/blob/master/src/examplePlugins/entity_status_update_task_status.py>
