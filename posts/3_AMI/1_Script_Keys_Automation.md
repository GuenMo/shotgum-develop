# [스크립트 키 및 자동화](https://www.youtube.com/watch?v=7HetJL17qJM)

> 이 동영상에서는 Shotgun의 스크립트 엔티티를 살펴보고, 자동화와 함께 사용할 때 제약이 심할 수 있는 사용자 기반 자격 증명 대신 스크립트 엔티티의 응용프로그램 키를 사용하여 Shotgun Python API 세션을 인증하는 방법도 알아봅니다.

사용자가 아이디와 패스워들 이용해 샷건 웹앱이나 API에 접근 합니다.
그렇지만 사용자의 권한에 따라 할수 있는 일이 제한 적입니다.
이때 `Script Key`를 발급 하여 잠시 권한을 `admin`으로 승급 시켜줍니다.

`demo.py` 만듭니다.

```python
import shotgun_api3
from config import PASSWORD, URL, USER

sg = shotgun_api3.Shotgun(URL, login=USER, password=PASSWORD)

project = sg.find_one("Project", [["name", "is", "Project X"]])
print(project)

assets = sg.find("Asset", [["project", "is", project]], ["code", "sg_status_list"])
for asset in assets:
    print(asset)
```

## Permissions in Shotgun

Shotgun 계정이 있는 모든 사용자에게 Shotgun 데이터베이스에 대한 액세스 권한을 제공합니다.
하지만 이 액세스 권한은 인증된 사용자나 스크립트 사용자에게 연결된 권한 규칙 세트에 따라 조정되고 제한됩니다.
아티스트에게 부여되는 Shotgun에 대한 액세스 권한은 API를 사용하는 경우에도 이와 동일하게 제한적입니다.

## Permissions in the Python API

프로젝트의 설명 필드를 변경할 수 있는 스크립트나 응용프로그램을 작성한다고 상상해 봅시다.
이 앱이 관리자(Admin)에게는 잘 작동하겠지만 아티스트로 로그인한 경우에는 예외가 있을 수 있습니다.

![Local Image](/img/7/1.png)

스크립트 키를 사용하여 문제를 해결하는 더 나은 방법이 있습니다.

![Local Image](/img/7/2.png)

스크립트 키를 만듭니다.

![Local Image](/img/7/3.png)

![Local Image](/img/7/4.png)

새 스크립트 키를 만들면 "API 관리자(API Admin)" 권한 그룹에 할당됩니다.
사용자 권한 그룹은 스크립트 엔티티에 할당될 수 없습니다.
반대로 말하면 API 권한 그룹은 인간 사용자에게 할당될 수 없습니다.
기본적으로 API 관리자(API Admin) 그룹은 Shotgun 사이트에 대한 거의 모든 액세스 권한을 갖는 상승된 권한 규칙 세트를 나타냅니다.

`키 변경`을 클릭 합니다.

![Local Image](/img/7/5.png)

`api-key`를 복사 하고 demo.py에 적용 합니다.

```python
import shotgun_api3

SCRIPT_NAME = os.environ["SG_DEMO_SCRIPT_NAME"]
SCRIPT_KEY = os.environ["SG_DEMO_SCRIPT_KEY"]

sg = shotgun_api3.Shotgun(URL, script_name=SCRIPT_NAME, api_key=SCRIPT_KEY)

project = sg.find_one("Project", [["name", "is", "Project X"]])
print(project)

assets = sg.find("Asset", [["project", "is", project]], ["code", "sg_status_list"])
for asset in assets:
    print(asset)
```

스크립트 키 값을 주기적으로 바꿔줘야 합니다.
