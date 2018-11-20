# [Python API—업로드/다운로드](https://youtu.be/oNGbOzksVtI?t=1)

> 이 Shotgun Python API 동영상에서는 Shotgun으로 저장, 트랜스코딩 및 트래킹하는 미디어를 중심으로 API를 이용한 업로드 및 다운로드와 관련된 일반 명령들을 살펴봅니다.

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/1.png)

## GUI에서 파일 업로드

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/2.png)

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/3.png)

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/4.png)

파일이 업로드되고 나면 파일 엔티티가 생성되고 해당 파일 필드가 업로드된 파일을 가리킵니다.

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/5.png)

참고로, 업로드된 파일은 Shotgun의 "file_serve" 영역에 임시로 저장되어 있습니다.

새로 생성된 파일 엔티티 레코드에서 "this_file" 키를 가져와서 이를 확인할 수 있습니다.

```python
import shotgun_api3
from config import *
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
f = sg.find_one("Attachment", [["id", "is", 111339]], ["this_file", "file_size", "filename", "file_extension"])

print pprint.pformat(f)

'''
{'file_extension': 'png',
 'file_size': 94714,
 'filename': '1_j_f-fL9EHgaFn4IRi7NZ7g.png',
 'id': 111339,
 'this_file': {'content_type': 'image/png',
               'id': 111339,
               'link_type': 'upload',
               'name': '1_j_f-fL9EHgaFn4IRi7NZ7g.png',
               'type': 'Attachment',
               'url': 'https://sg-media-tokyo.s3-accelerate.amazonaws.com/5b02f17a4d270281fa3a569116e4482cf329ee73/a7c938afa67c9374c74f758c3acee119ad75db2b/1_j_f-fL9EHgaFn4IRi7NZ7g.png?AWSAccessKeyId=AKIAI3ABMYXRXQDDDLYQ&Expires=1542664054&Signature=P0Zz5YXPfCtUl7QxEXMdTqcnLA8%3D&response-content-disposition=filename%3D%221_j_f-fL9EHgaFn4IRi7NZ7g.png%22&x-amz-meta-user-id=169&x-amz-meta-user-type=HumanUser'},
 'type': 'Attachment'}
'''
```

필드를 추가 합니다.

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/6.png)

모두 쿼리해 두면 시간을 절약할 수 있는 좋은 정보입니다.
파일 엔티티는 파일 자체가 아니라는 점도 짚어드리고 싶습니다.
파일 엔티티를 삭제해도 15분 동안 액세스가 가능합니다.

즉, Shotgun에 업로드하는 모든 항목은 삭제할 수 없다는 것을 의미합니다.
이를 위해, Shotgun 사이트에는 저장소 제한이 없습니다.

## 파일을 엔티티에 링크

방금 업로드한 파일 엔티티는 프로젝트를 제외하고는 Shotgun의 어느 항목과도 링크되어 있지 않습니다.
업로드된 파일을 엔티티와 연결하는 것이 더 일반적입니다.
에셋에 대한 참조 아트를 업로드하거나 샷 또는 버전과 관련된 동영상을 업로드하는 것이 훨씬 더 유용할 것입니다.
이러한 연결은 "링크(Links)" 필드를 통해 설정합니다.

링크(Links) 다중 엔티티 필드를 살펴보면 이 필드가 특정 엔티티 유형 즉 에셋(Asset), 노트(Note), 리비전(Revision), 시퀀스(Sequence) 만 받도록 구성된 것을 볼 수 있습니다.

## 이미지 및 동영상 트랜스코딩

버전 엔티티의 "업로드된 영상(Uploaded Movie)" 필드에서도 멋진 동작이 수행됩니다.
버전 엔티티는 웹 앱, RV 및 통합 제품에서 리뷰 워크플로우를 지원하기 위해 사용하는 특수한 엔티티입니다.

해당 필드는 보기 및 재생 가능한 미디어를 설명하며 S3의 미디어 파일은 파일 엔티티를 통해 "sg_uploaded_movie"라는 파일/링크 필드를 거쳐 링크됩니다.

Shotgun 개발 작업을 많이 수행하면 이 sg_uploaded_movie 필드에 아주 익숙해질 것입니다.

버전 엔티티를 이용해 파일을 업로드 해봅니다.

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/7.png)

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/8.png)

자동으로 MOV(image) 필드에 프리뷰가 생성 됩니다.
지원되는 파일 형식을 기록한 문서 입니다.

<https://support.shotgunsoftware.com/hc/ko/articles/115000067353-%ED%8A%B8%EB%9E%9C%EC%8A%A4%EC%BD%94%EB%94%A9-%EC%84%9C%EB%B9%84%EC%8A%A4>

만들어진 프리뷰 영상은 웹에서 재생될 수 있도록 트랜스코딩되고 최적화된 것입니다.

파일 엔티티 페이지를 확인 해보면 새로 만들어진 엔티티들이 보입니다.

하지만 미디어 복사본처럼 보이는 동일한 버전에 링크된 추가 파일 엔티티도 두 개 있습니다.

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/9.png)

이 동영상 파일은 Shotgun 서버의 트랜스코딩 서비스에서 자동으로 생성되었으며 Shotgun 웹 GUI와 다른 위치의 오버레이 플레이어에서 사용합니다.
RV에서는 mp4 파일이 webm 파일보다 우선하며 웹 기반 동영상 플레이어에서도 대체로 그렇습니다.

그런데 정적 이미지가 sg_uploaded_movie 필드에 업로드되면
트랜스코더가 Shotgun 웹 플레이어에서 사용하는 jpeg도 생성합니다.

정리하자면 업로드된 미디어 파일이 이처럼 트랜스코딩되고 복제되는 유일한 경우는 버전의 sg_uploaded_movie 필드가 대상인 경우입니다.

API에서 버전의 필드를 살펴보고 이름에 "upload"가 포함된 모든 필드를 검색하면 웹 GUI에서는 볼 수 없는 많은 필드가 검색됩니다.

```python
import shotgun_api3
from config import *

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)
project = sg.find_one("Project", [["name", "is", "project X"]])

version_schema = sg.schema_field_read("Version", project_entity=project)

for field in version_schema.keys():
    if "uploaded" in field.lower():
        print field

'''
sg_uploaded_movie_webm
sg_uploaded_movie_mp4
sg_uploaded_movie
sg_uploaded_movie_frame_rate
sg_uploaded_movie_image
sg_uploaded_movie_transcoding_status
uploaded_movie_duration
'''
```

Shotgun에서 영상 업로드 및 트랜스코딩과 관련하여 모든 필드를 보여 주는 것이 너무 혼란스러울 거라 생각되어 일부를 숨겼습니다.

## API를 이용한 업로드

업로드 메서드에는 네 가지 인자, 즉 유형과 링크할 대상 엔티티의 ID, 업로드할 파일 경로 그리고 field_name 사용됩니다.

```python
import shotgun_api3
from config import *
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)

project = sg.find_one("Project", [["name", "is", "project X"]])
print pprint.pformat(project)

# 버전 엔티티를 만든다
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
file_path = "C:/Users/geunmo_kim/Pictures/1_j_f-fL9EHgaFn4IRi7NZ7g.png"
attachment_id = sg.upload("Version", version["id"], file_path, field_name="sg_uploaded_movie")
print attachment_id

# 파일 확인
my_file = sg.find_one("Attachment", [["id", "is", attachment_id]], ["this_file"])
print my_file["this_file"]["url"]
```

## API를 이용한 다운로드

다운로드 메서드에는 두 가지 인자, 파일 엔티티(Attachment)의 id와 파일경로가 사용됩니다.

```python
# coding:utf-8

import shotgun_api3
from config import *
import pprint

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)

project = sg.find_one("Project", [["name", "is", "project X"]])

version = sg.find_one("Version", [["id", "is", 23426]], ["sg_uploaded_movie"])
attachment = version["sg_uploaded_movie"]
print pprint.pformat(attachment)

download_file = sg.download_attachment(attachment["id"], file_path="d:/{}".format(attachment["name"]))
print download_file
```

## 미디어 다운로드

위의 방법은 원본 파일을 다운 합니다.
하지만 트랜스코딩된 mp4 파일이 필요할 수도 있겠죠.
다운로드 속도를 높이거나 로컬 디스크 공간 사용량을 줄이거나 로컬 도구의 재생 성능을 향상시키려는 경우에 그렇습니다.
그런 경우 sg_uploaded_movie_mp4 필드를 대상으로 지정 합니다.
여기서 주의할 사항은 파일 이름 앞에 해시가 추가되어 있다는 점입니다.

```python
# coding:utf-8

import shotgun_api3
from config import *
import pprint
import re

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)

project = sg.find_one("Project", [["name", "is", "project X"]])

# 원본파일
version = sg.find_one("Version", [["id", "is", 23425]], ["sg_uploaded_movie"])
attachment = version["sg_uploaded_movie"]
print pprint.pformat(attachment)

download_file = sg.download_attachment(attachment["id"], file_path="d:/{}".format(attachment["name"]))
print download_file

# 트렌스코딩된 파일
version = sg.find_one("Version", [["id", "is", 23425]], ["sg_uploaded_movie_mp4"])
attachment = version["sg_uploaded_movie_mp4"]
print pprint.pformat(attachment)

download_file = sg.download_attachment(
    attachment["id"],
    file_path="d:/{}".format(
        re.sub("^[0-9a-z]+_", "transcoded_", attachment["name"])
    )
)
print download_file
```

## 썸네일 및 영사 슬라이드 다운로드

먼저, 썸네일 이미지에 대한 URL을 가져오겠습니다.
이제 이 URL을 download_attachment 메서드에 드롭하되 이처럼 "url" 키가 있는 작은 사전 내부에 드롭할 수 있습니다.
다음으로, 이전과 같이 파일 경로를 제공합니다.
그런데 이번에는 파일의 이름을 무엇으로 지정해야 할까요?
image 키에 멋진 name 키가 없으므로 엔티티를 기준으로 이름을 만들어야 합니다. 이와 같이 말이죠.

```python
# coding:utf-8

import shotgun_api3
from config import *
import pprint
import re

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)

project = sg.find_one("Project", [["name", "is", "project X"]])

# 썸네일
version = sg.find_one("Version", [["id", "is", 23425]], ["image"])
url = version["image"]
file_path = re.search(".*\/(\w+_t.jpg)\?AWSAccessKeyId.*", url).group(1)
thumbnail = sg.download_attachment(
    {"url": url},
    file_path="d:/{}".format(file_path)
)
print thumbnail
```

썸네일 고해상도 다운로드

```python
# 원본 이미지
import shotgun_api3
from config import *
import imghdr
import os

version = sg.find_one("Version", [["id", "is", 23425]], ["image"])
downloaded_file = sg.download_attachment(
    {"url": "{}/thumbnail/full/Version/{}".format(sg.base_url, version["id"])},
    file_path="d:/{}_thumb_orig".format(version["id"])
)

new_file_path = "".join([downloaded_file, ".", imghdr.what(downloaded_file)])
os.rename(downloaded_file, new_file_path)
print new_file_path
```

## Shotgun 외부 파일 다운로드

```python
download_img = sg.download_attachment(
    {"url": "https://upload.wikimedia.org/wikipedia/commons/5/53/Wikipedia-logo-en-big.png"},
    file_path="d:/wiki.png"
)

print download_img
````

## 로컬 파일 링크

로컬 파일 링크가 작동하려면 하나 이상의 로컬 파일 저장소 엔티티를 설정해야 합니다.
이러한 엔티티는 주로 툴킷 통합에서 사용되며 루트 저장소 폴더를 정의합니다.
공유 파일 시스템의 위치로 "primary"를 가리키는 것이 좋습니다.

![Local Image](/img/2_api/4_The_Python_API_Upload_Download/10.png)

작업을 마치면 기존 엔티티의 파일/링크 필드를 업데이트할 수 있습니다.
로컬 파일 링크를 할때 파일의 경로가 프로젝트 로컬 파일 시스템에 등록된 하위 경로여야 만 링크를 할수 있습니다.

```python
sg = shotgun

project = sg.find_one("Project", [["name", "is", "project X"]])

# 버전 엔티티를 만든다
version = sg.create("Version", {"project": project})
version["id"]

sg.update("Version", version["id"], {"sg_uploaded_movie": {"local_path": "\\\\ISSAC\\Project\\Shotgun\\projects\\project_x\\athena.mp4"}})
```

## 동영상/프레임 경로

아쉽지만 로컬 파일 링크는 묘책이 아닙니다.
특히, Shotgun RV 통합에서는 현재 작동하지 않습니다.
RV는 여러 다른 필드, 즉 sg_path_to_frames, sg_path_to_movie, sg_uploaded_movie_mp4에서 데이터를 참조합니다.
그렇다면 RV가 디스크에서 직접 원래 동영상 파일을 재생하게 하려면 어떻게 하면 될까요?
그럴 경우 **sg_path_to_movie** 필드를 작성하면 됩니다.
로컬 파일 링크와 달리 path_to_movie 필드와 관련한 한 가지 멋진 기능은 파일 경로에서 어느 위치나 가리킬 수 있으며 로컬 파일 저장소 엔티티에 의해 정의된 저장소 영역 아래에 있을 필요가 없습니다.

mp4 파일을 좋아하지 않는다면 **sg_path_to_frames** 필드에 원래 로컬 프레임 경로를 제공하면 됩니다.
제가 재생하는 동영상 파일은 Nuke에서 생성된 이 이미지 시퀀스를 바탕으로 생성되었습니다.

Windows 및 Linux 경로는 어떻게 처리하는지 궁금하시다면 이 RV 문서를 확인하시기 바랍니다.

<https://support.shotgunsoftware.com/hc/ko/articles/219042298-RV-OS-PATH-%EB%98%90%EB%8A%94-RV-PATHSWAP%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%AF%B8%EB%94%94%EC%96%B4-%EA%B2%BD%EB%A1%9C-%ED%98%84%EC%A7%80%ED%99%94>

스테레오 경로를 처리하는 방법이 궁금하고 지원되는 경로 유형에 대한 전체 목록을 보려면 이 문서를 확인하시기 바랍니다.

<http://www.tweaksoftware.com/static/documentation/rv/current/html/rv_manual.html#Command_Line_Options_Image_Sequence_Notation>