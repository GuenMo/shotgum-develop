# [Shotgun API 및 CRUD와 상호 작용](https://www.youtube.com/watch?v=f5UWy3d-4ZI)

> Shotgun Python API에는 여러 메서드 세트가 포함되어 있습니다. 이 동영상에서는 CRUD 세트에서 가장 유용한 메서드를 중점적으로 다룹니다.
> Shotgun에서 샷을 생성하고, 기존 샷과 그 상세 정보를 찾고, 샷을 업데이트하고, 샷을 삭제하는 등의 모든 작업을 API를 통해 처리하는 방법을 배워 보십시오.

## Create

`create`

1. "엔티티 타입"
2. [[검색 조건]]

```python
import shotgun_api3
from config import URL, USER, PASSWORD

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'crud']], ['name'])

# CREATE
shot = sg.create('Shot', {'code': 'sh003', 'project': project})
```

## Read

`read`

1. "엔티티 타입"
2. [[검색 조건]]
3. [검색할 필드 키(들)]

```python
import shotgun_api3
from config import URL, USER, PASSWORD

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one("Project", [['name', 'is', 'Project X']])

# READ
shot = sg.find_one('Shot', [['project', 'is', project], ['code', 'is', 'sh001']])
print shot

shots = sg.find('Shot', [['project', 'is', project]], ['code', 'sg_status_list'])
for s in shots:
    print s
```

## Update

`update`

1. "엔티티 타입"
2. 엔티티 id
3. {업데이트 할 필드 키: 필드 값}

```python
import shotgun_api3
from config import URL, USER, PASSWORD

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'Project X']])

sequence = sg.create('Sequence', {'project': project, 'code': 'seq001'})

shots = sg.find('Shot', [['project', 'is', project]])

for shot in shots:
    sg.update('Shot', shot['id'], {'sg_status_list': 'apr', 'sg_sequence': sequence})
```

## Delete

`delete`

1. "엔티티 타입"
2. 엔티티 id

```python
import shotgun_api3
from config import URL, USER, PASSWORD

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'Project X']])
print(project)

sequence = sg.find_one('Sequence', [["project", "is", project], ["code", "is", "seq001"]], ["shots"])
print(sequence)

if sequence["shots"]:
    for shot in sequence["shots"]:
        sg.delete(shot["type"], shot["id"])

sg.delete(sequence["type"], sequence["id"])
```

## revive

`revive`

1. "엔티티 타입"
2. 엔티티 id

## Document

<http://developer.shotgunsoftware.com/python-api/reference.html#crud-methods>