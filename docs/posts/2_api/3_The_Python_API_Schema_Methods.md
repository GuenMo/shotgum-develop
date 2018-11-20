# [Python API—스키마 메서드](https://www.youtube.com/watch?v=pjnaIHKzX8k)

> Shotgun의 Python API를 스키마 메서드에 중점을 두고 보다 자세히 살펴봅니다.
> 이 동영상에서는 스키마 정의, 스키마가 Shotgun과 작동하는 방식, 알려진 한계 등의 주제를 다룹니다.

## 소개

Shotgun Python API에는 여러 메서드 세트가 있습니다.
이번 동영상에서는 스키마 세트에서 가장 유용한 메서드에 대해 집중적으로 펴보겠습니다.

![Local Image](/img/2_api/3_The_Python_API_Schema_Methods/1.png)

## 스키마란 무엇인가

![Local Image](/img/2_api/3_The_Python_API_Schema_Methods/2.png)

여기서 살펴보는 모든 항목이 스키마이지만 "샷 스키마"와 같은 특정 엔티티 스키마에 대해 얘기할 때도 있을 것입니다. 그럴 때 스키마는 샷을 정의하는 `필드와 필드 값으로 구성된 집합만 지칭`합니다.

단일 엔티티 필드 스키마를 지칭하여 훨씬 더 세부적으로 설명할 수도 있습니다.
예를 들어, 샷 엔티티의 "코드 필드 스키마"에 대해 설명할 수 있으며 이 경우에는 데이터 유형 및 가시성 같은 코드 필드의 특성 값에 대한 이야기가 됩니다.

이번 동영상에서는 Shotgun Python API를 통해 이 세 가지 관점 모두에서 "스키마"라는 개념에 대해 살펴볼 것입니다.

참고로, Shotgun 에코시스템에서 "스키마"라는 용어는 Shotgun 툴킷에서 템플릿과 상호 작용하는 파일 시스템 폴더의 계층을 나타낼 수도 있습니다.

## 커넥션 엔티티

두 엔티티가 연결될 때 자동으로 생기는 엔티티 이다.

<https://support.shotgunsoftware.com/hc/ko/articles/114094036254-%EC%97%B0%EA%B2%B0-%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%82%AC%EC%9A%A9>

## API 스키마 메서드를 이용한 CRUD

필드 엔티티는 표준 CRUD API 메소드와 호환되지 않든다.

```python
import shotgun_api3
from config import URL, USER, PASSWORD

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
files = sg.find_one('Field', [])

# ERROR
```

대신 특수한 스키마의 CRUD 메소드가 필드 엔티티에 대해 존재하고 이러한 메서드는 Python API 문서에 상세히 설명되어 있습니다.

![Local Image](/img/2_api/3_The_Python_API_Schema_Methods/3.png)

## schema_entity_read

이 메서드는 모든 활성화된 에티티와 두가지 정보(visible, name) 요소를 제공하며, 이 둘 모두 API에서는 변경할 수 없습니다.

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'crud']])

# 모든 활성화된 entity_types
schema = sg.schema_entity_read(project_entity=project)
for entity in schema:
    print entity

shot = schema['Shot']
print pprint.pformat(shot)

# {'name': {'editable': False, 'value': 'Shot'},
# 'visible': {'editable': False, 'value': True}}
```

물론, 이 정보로 많은 일을 할 수 있다고 상상하실 수 있습니다.
이 정보를 사용하여 커스텀 도구에서 사용 가능한 엔티티 목록을 반환하고 엔티티가 존재하고 관련 프로젝트에서 표시되는지 검증하고 GUI 또는 보고서에 사용할 엔티티 표시 이름을 가져올 수도 있습니다.

## schema_field_read

![Local Image](/img/2_api/3_The_Python_API_Schema_Methods/4.png)

schema_field_read 메서드는 엔티티 유형을 하나의 필수 인자로 받습니다.

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'project X']])

asset_schema = sg.schema_field_read('Asset', project_entity=project)

# Asset 엔티티의 모든 필드 스키마
print "--All Fields of Asset--"
for field in sorted(asset_schema.keys()):
    print field
```

모든 에셋 엔티티 필드의 내부 코드를 파악했으므로 이러한 필드의 스키마 중 하나를 더 자세히 살펴볼 수 있습니다.

에셋 엔티티의 "type" 필드 스키마는 어떨까요?

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'project X']])

asset_schema = sg.schema_field_read('Asset', project_entity=project)

# Asset 엔티티의 Type(sg_asset_type) 필드 스키마
print "--Asset 엔티티의 Type(sg_asset_type) 필드 스키마--"
for k, v in enumerate(sorted(asset_schema["sg_asset_type"].items())):
    print k, v

'''
--Asset 엔티티의 Type(sg_asset_type) 필드 스키마--
0 ('data_type', {'editable': False, 'value': 'list'})
1 ('description', {'editable': True, 'value': ''})
2 ('editable', {'editable': False, 'value': True})
3 ('entity_type', {'editable': False, 'value': 'Asset'})
4 ('mandatory', {'editable': False, 'value': False})
5 ('name', {'editable': True, 'value': 'Type'})
6 ('properties', {'default_value': {'editable': True, 'value': None}, 'valid_values': {'editable': True, 'value': ['Character', 'Set/Env', 'Prop', 'FX_Asset', 'In House Tool', 'Outsourcing Tool', 'Format', 'Guide', 'Poster', 'Character Design', 'Research', 'Plan', 'Matte']}, 'summary_default': {'editable': True, 'value': 'none'}})
7 ('ui_value_displayable', {'editable': False, 'value': True})
8 ('unique', {'editable': False, 'value': False})
9 ('visible', {'editable': False, 'value': True})
'''
```

이제 sg_asset_type 필드 엔티티, 실제로 Shotgun의 모든 필드 엔티티를 설명하는 필드 목록이 반환되었습니다.

더 깊이 들어가서 이러한 필드 중 하나인 "name" 필드가 어떤지 살펴보겠습니다.

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'project X']])

asset_schema = sg.schema_field_read('Asset', project_entity=project)

# Asset 엔티티의 Type(sg_asset_type) 필드의 name 스키마
print "--Asset 엔티티의 Type(sg_asset_type) 필드의 name 스키마--"
print asset_schema["sg_asset_type"]["name"]

#{'editable': True, 'value': 'Type'}
```

Shotgun으로 돌아가서 필드 이름의 내부 이름을 확인하면 "name"이 아니라 "human_name"인 것을 볼 수 있습니다.

![Local Image](/img/2_api/3_The_Python_API_Schema_Methods/5.png)

Shotgun의 데이터와 API에서 반환되는 데이터는 서로 유사하지만 구성 방식은 각각 다릅니다.

일반 API CRUD 메서드와 다르게 스키마 명령을 사용하여 동일한 식으로 반환되는 필드(Fields) 페이지의 모든 정보는 완전히 믿을 수 없습니다.

Shotgun에는 Id 필드와 필드 유형(Field Type) 필드가 있는데 이 두 필드 모두 API에서는 반환되지 않습니다.

API에서는 "editable", "mandatory", "properties", "unique", "visible" 키가 반환됩니다.

여기서 반환되는 데이터를 살펴보는 것은 여러분 몫입니다.

하지만 각 필드 엔티티에서 가장 흥미로운 사항은 "properties" 키입니다.

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'project X']])

asset_schema = sg.schema_field_read('Asset', project_entity=project)

print '--["sg_asset_type"]["properties"]--'
for k, v in enumerate(sorted(asset_schema["sg_asset_type"]["properties"].iteritems())):
    print k, v

print '--["sg_asset_type"]["properties"]["valid_values"]--'
for k, v in enumerate(sorted(asset_schema["sg_asset_type"]["properties"]["valid_values"].iteritems())):
    print k, v

print '--["sg_asset_type"]["properties"]["valid_values"]["value"]--'
for k, v in enumerate(sorted(asset_schema["sg_asset_type"]["properties"]["valid_values"]["value"])):
    print k, v

'''
--["sg_asset_type"]["properties"]--
0 ('default_value', {'editable': True, 'value': None})
1 ('summary_default', {'editable': True, 'value': 'none'})
2 ('valid_values', {'editable': True, 'value': ['Character', 'Set/Env', 'Prop', 'FX_Asset', 'In House Tool', 'Outsourcing Tool', 'Format', 'Guide', 'Poster', 'Character Design', 'Research', 'Plan', 'Matte']})
--["sg_asset_type"]["properties"]["valid_values"]--
0 ('editable', True)
1 ('value', ['Character', 'Set/Env', 'Prop', 'FX_Asset', 'In House Tool', 'Outsourcing Tool', 'Format', 'Guide', 'Poster', 'Character Design', 'Research', 'Plan', 'Matte'])
--["sg_asset_type"]["properties"]["valid_values"]["value"]--
0 Character
1 Character Design
2 FX_Asset
3 Format
4 Guide
5 In House Tool
6 Matte
7 Outsourcing Tool
8 Plan
9 Poster
10 Prop
11 Research
12 Set/Env
'''
```

이 경우 "유형(Type)" 목록 필드에 Character, Environment, FX 등의 값이 미리 입력되어 있는 것을 볼 수 있습니다.

schema_field_read에서 반환되는 데이터는 어떠한 용도로든 사용할 수 있습니다.

커스텀 앱에서 엔티티 필드를 동적으로 나열해야 하거나 작업을 진행하기 전에

특정 필드가 존재하고 올바른 데이터 유형인지를 검증해야 할 수 있습니다.

아니면 사용자 입력이 목록 필드 값에 표시된 옵션 중 하나와 일치하는지를 확인해야 할 수도 있습니다.

모두 유용한 기능입니다.

## schema_field_create

스크립트가 처리하는 모든 엔티티에 고유한 이동 경로 식별자를 유지하는 스크립트를 작성한다고 가정합니다.

예를 들어, 다른 데이터베이스에서 데이터를 가져오는 경우 소스 id를 이어서 유지하려고 한다면 프로그램이 검색하고 입력할 수 있는 모든 엔티티에 대해 소스 id 필드를 수동으로 생성할 수 있으며 schema_field_create 메서드를 사용하여 이 작업을 프로그래밍 방식으로 수행할 수도 있습니다.

엔티티 유형 목록을 통해 실제 활용 사례를 살펴보겠습니다.

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one('Project', [['name', 'is', 'project X']])

entity_types = ['Asset', 'Shot', 'Camera']
entity_schema = sg.schema_entity_read(project_entity=project)

for entity_type in entity_types:
    if entity_type not in entity_schema.keys():
        print '{} is not an enabled entity_type.'.format(entity_type)
        continue
    if 'sg_source_id' in sg.schema_field_read(entity_type):
        print 'sg_source_id is already in schema for entoty type {}'.format(entity_type)
        continue

    print 'Did not find sg_source_id {}, adding in to the schema...'.format(entity_type)
    sg.schema_field_create(entity_type, 'number', 'Source ID', {'description': 'geunmo_kim created field using API, delete at will.'})
```

## schema_field_update

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one("Project", [["name", "is", "SG_Intigration"]])

properties = {"name": "Original ID", "description": "geunmo_kim updated field using API, delete at will."}
sg.schema_field_update("Asset", "sg_source_id", properties)
```

## schema_field_delete

```python
# coding:utf-8

import shotgun_api3
from config import URL, USER, PASSWORD

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)

sg.schema_field_read("Shot", "KGM_TXT")
```

## 한계

API를 사용해서는 필드를 영구 삭제할 수 없습니다.
API에서는 쿼리 또는 파일/링크 필드를 생성할 수도 없으며 필드 가시성 설정을 제어할 수도 없다는 점에 유의하시기 바랍니다.
이는 권한 프레임워크에 속하므로 보안 문제로 인해 제한이 있기 때문입니다.
사이트에 대한 엔티티 유형 스키마, 해당 가시성, 필드, 그리고 필드 값을 관리하는 방법에 대해 알아보았으며
스키마 명령을 함께 사용하여 스키마를 즉시 변경하거나 유효성 검사를 수행할 수 있는 전략도 살펴보았습니다.

전체 스키마를 가져오는 **schema_read**라는 다른 유용한 스키마 메서드도 있다는 점을 간단히 알려드립니다.
이 메서드는 기본적으로 schema_entity_read와 schema_field_read 메서드가 결합된 형태입니다.
이 메서드를 사용하면 너무 많은 데이터가 전송되므로 자주 사용하지는 마시기 바랍니다.
**단, 전체 사이트 스키마를 캐시해야 할 경우 이 메서드가 있다는 사실을 알고 계시기 바랍니다.**