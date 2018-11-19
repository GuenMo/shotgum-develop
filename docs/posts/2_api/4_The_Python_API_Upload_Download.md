# Python API—업로드/다운로드

> 이 Shotgun Python API 동영상에서는 Shotgun으로 저장, 트랜스코딩 및 트래킹하는 미디어를 중심으로 API를 이용한 업로드 및 다운로드와 관련된 일반 명령들을 살펴봅니다.

[Developer Training - Shotgun: The Python API—Upload/Download](https://www.youtube.com/watch?time_continue=354&v=oNGbOzksVtI)

![Local Image](/img/10/1.png)

## GUI에서 파일 업로드

![Local Image](/img/10/2.png)

![Local Image](/img/10/3.png)

![Local Image](/img/10/4.png)

파일이 업로드되고 나면 파일 엔티티가 생성되고 해당 파일 필드가 업로드된 파일을 가리킵니다.

![Local Image](/img/10/5.png)

참고로, 업로드된 파일은 Shotgun의 "file_serve" 영역에 임시로 저장되어 있습니다.

새로 생성된 파일 엔티티 레코드에서 "this_file" 키를 가져와서 이를 확인할 수 있습니다.

```python
import shotgun_api3
from config import *
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
f = sg.find_one("Attachment", [["id", "is", 111258]], ["this_file"])

print pprint.pformat(f)
```

필드를 추가 합니다.

![Local Image](/img/10/6.png)

모두 쿼리해 두면 시간을 절약할 수 있는 좋은 정보입니다.
파일 엔티티는 파일 자체가 아니라는 점도 짚어드리고 싶습니다.
파일 엔티티를 삭제해도 15분 동안 액세스가 가능합니다.

즉, Shotgun에 업로드하는 모든 항목은 삭제할 수 없다는 것을 의미합니다.
이를 위해, Shotgun 사이트에는 저장소 제한이 없습니다.

## 파일을 엔티티에 링크

이 엔티티는 프로젝트를 제외하고는 Shotgun의 어느 항목과도 링크되어 있지 않습니다.
업로드된 파일을 엔티티와 연결하는 것이 더 일반적입니다.
에셋에 대한 참조 아트를 업로드하거나 샷 또는 버전과 관련된 동영상을 업로드하는 것이 훨씬 더 유용할 것입니다.
이러한 연결은 "링크(Links)" 필드를 통해 설정합니다.

링크(Links) 다중 엔티티 필드를 살펴보면 이 필드가 특정 엔티티 유형 즉 에셋(Asset), 노트(Note), 리비전(Revision), 시퀀스(Sequence) 만 받도록 구성된 것을 볼 수 있습니다.

## 이미지 및 동영상 트랜스코딩

## API를 이용한 업로드

업로드 메서드에는 네 가지 인자, 즉 유형과 링크할 대상 엔티티의 ID, 업로드할 파일 경로 그리고 field_name 사용됩니다.

```python
import shotgun_api3
from config import *
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)

project = sg.find_one("Project", [["name", "is", "sg_integrations"]])
print pprint.pformat(project)

# Create Version
# version = sg.create("Version", {"project": project})
# print pprint.pformat(version)

# Read Version
version = sg.find_one("Version",
                      [
                          ["project", "is", project],
                          ["id", "is", 23404]
                      ])
print pprint.pformat(version)

# Upload File
attachment_id = sg.upload("Version",
                          version["id"],
                          "C:/Users/geunmo_kim/Pictures/1_j_f-fL9EHgaFn4IRi7NZ7g.png",
                          field_name="sg_uploaded_movie"
                          )

```

## API를 이용한 다운로드

## 미디어 다운로드

## 썸네일 및 영사 슬라이드 다운로드

## Shotgun 외부 파일 다운로드

## 전송 안 함 선택

## 로컬 파일 링크

## 동영상/프레임 경로
