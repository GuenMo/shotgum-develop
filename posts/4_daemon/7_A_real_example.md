# [A real example (실제 예시)](https://youtu.be/YPZ1FWvhf7k?t=23m6s)

Okay. Now let's create something that might actually be useful.
좋습니다. 그럼 이제 실제로 유용할 수 있는 것을 만들어봅시다.

We've already got a plug-in that fills in an assets description field when that  assets status field is set to you on hold.
어셋 상태 필드가 보류 상태로 설정되면 어셋 설명 필드를 채우는 플러그인이 이미 있습니다.

Let's extend our plug-in.
플러그인을 확장해 봅시다.

Let's get all the tasks linked to that asset and set their statuses to on hold - unless they're already set to final or n/a.
해당 어셋에 연결된 모든 태스크를 가져와 상태를 보류 상태로 설정하겠습니다(최종 또는 n/a로 설정되지 않은 경우).

I'll need to run multiple update commands to accomplish this.
이 작업을 수행하려면 여러 개의 업데이트 명령을 실행해야 합니다.

So I'm going to initialize a batch data command list and change this update call to be part of it.
따라서 배치 데이터 명령 목록을 초기화하고 업데이트 호출을 변경해 보겠습니다.

```python
batch_data = []
update_message = []
```

Next if we're going to update tasks status fields we should make sure the SG status list field is in the schema.
다음으로 작업 상태 필드를 업데이트하려면 `sg_status_lsts` 필드가 스키마에 있는지 확인해야 합니다.

Again this is super paranoid.
다시 이것은 극도로 편집증적이다.

Especially since it's impossible to delete this field from the GUI but I'll do it  here anyway as an exercise.
특히 GUI에서 이 필드를 삭제하는 것이 불가능하기 때문에 연습 삼아 여기서 하겠습니다.

If it was a custom field that didn't exist on all entity types this check would make more sense.
모든 엔티티 유형에 존재하지 않는 사용자 지정 필드였다면 이 점검은 더 의미가 있을 것입니다.

After that we can pull down all the tasks linked to the entity and log and update requests with our batch data list.
그런 다음 엔티티에 연결된 모든 작업을 종료하고 일괄 데이터 목록으로 요청을 로깅 및 업데이트할 수 있습니다.

```python
# Make sure the Task schema has an args["target_status"] field.
task_schema = sg.schema_field_read(
    "Task",
    field_name="sg_status_list",
)
if args["target_status"] not in task_schema["sg_status_list"]["properties"]["valid_values"]["value"]:
    logger.warning("%s is not in Task schema, plugin will never execute." % args["task_status"])
    return

return True
```

Remember we don't want to change tasks at the final or n/a so I'll add another setting for that maybe I'll call it skip statuses and then skip tasks set to those statuses.
마지막이나 n/a에서 작업을 변경하지 않으니 상태를 건너뛰고 해당 상태로 설정된 작업을 건너뜁니다.

```python
# Find all the Tasks linked to our entity.
tasks = sg.find(
    "Task",
    [["entity", "is", entity]],
    ["sg_status_list"],
)
```

Cool almost done.

Finally we can pass our batch data list to an SG batch command and tell the logger about it.

Let's see.

If it works great just what we expected.

Let's check the output in the terminal.

Cool.

So now we've got a plugin that filters for specific event types validates relevant schema fields and incoming events checks field data for expected values based on the event and finally if everything looks right it performs a little automation  with the shotgun Python API.

I'll leave it up to you to judge whether what we've created here would be helpful in your production.

Personally I would only use a plugin like this if I had a lot of small assets in flux whose many tasks were distributed across remote artists and my producers were spread thin.

If instead assets at my studio are infrequently put on hold I'd prefer to trust my  producer to manage the statuses and message out changes personally.

But that's just me and of course you're in complete control and free to decide what your custom automation should or shouldn't do.

Writing daemon plugins is a bit of an art and the best way to learn is to write a few.

Finally I'd like to say that I believe I'm overly paranoid for a good reason.

After writing a large number of event daemon plugins I've seen about everything go wrong that can go wrong.

I wouldn't necessarily run so many checks in say.

Toolkit app or standalone script.

But the daemon related coding habits I'm passing on here are hard one.

Of course maybe you think this plug-in is overly complex and every developer is free to walk his or her own path of many lessons far be it for me to keep those  lessons from you.
