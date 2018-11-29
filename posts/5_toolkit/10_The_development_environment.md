# [개발 환경](https://youtu.be/5nRZ5GgcOnk?t=27m26s)

이 워크플로우는 배포에는 유용하지만 개발에는 매우 적합하지 않습니다.

Is there a way to work directly in the dev branch of the tk-config-learn repo folder on the desktop?
데스크탑에서 `tk-config-learn` 저장소 폴더를 사용해서 작업할 수 있는 방법이 있을까요?

예! `Primary` 복제하여 `User Restrictions` 필드에 사용자를 추가하겠습니다.

`descriptor` 필드를 개발 환경 폴더를 가리키게 합니다.
`sgtk:descriptor:dev?path=I:\WorkSpace\shotgun_projects\tk-config-aiw`

![Local Image](/img/5_toolkit/10_The_development_environment/1.png)

다양한 `descriptor` 유형을 설명하는 문서를 알려드리겠습니다.
<http://developer.shotgunsoftware.com/tk-core/descriptor.html#descriptor-types>

지금 부터는 샷건 사이트에 구성 파일을 다운로드 하는 대시 데스크탑에 샷건 데스크탑 구성을 가져옵니다.

개발 구성을 어떻게 선택 할까요?

샷건 데스크탑에서 풀다운 메뉴를 클릭하여 개발 구성을 선택 하면 됩니다.

![Local Image](/img/5_toolkit/10_The_development_environment/2.png)

기본 구성을 사용하지 않는 경우 멋진 파란색 배너를 얻을 수 있습니다.

`User Restrictions` 필드에 비활성화된 유저를 추가하면 모든 사용자에 대한 구성이 비활성화 됩니다. 또는 `Plugin Ids` 필드를 공백으로 바꿔도 같은 효과가 있습니다.

당신은 때때로 그것을 해야 할 필요가 있다.

하지만 어떻게 구성을 변경할 수 있을까요?