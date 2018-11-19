# [이벤트 데몬 소개](https://www.youtube.com/watch?time_continue=764&v=YPZ1FWvhf7k)

> Shotgun은 태스크 템플릿과 종속성 등 프로덕션 가속화를 위해 마련된 기본 제공 워크플로우 자동화 기능과 함께 제공되지만 원한다면 커스텀 자동화 기능을 생성할 수도 있습니다.
>
> 이 동영상에서는 Shotgun 이벤트 데몬, 그리고 커스텀 트리거를 생성하는 방법을 살펴봅니다.
>
> **Python 또는 Shotgun 초보자를 위한 동영상이 아닙니다. 이벤트 데몬 플러그인을 잘못 구현하는 경우에는 Shotgun 사이트의 성능이 저하될 수 있으므로 먼저 개발자 교육 시리즈의 동영상을 시청하는 것이 좋습니다.**
>
> 윈도우 환경일 경우 <https://github.com/shotgunsoftware/shotgunEvents/wiki/Installation> 를 참조 하세요.
>
> `pip install pypiwin32`

Shotgun ships with built-in workflow automations designed to speed up production.
샷건에는 생산 속도를 높일 수 있도록 설계된 기본 제공 워크플로 자동화가 포함되어 있습니다.

For example once task templates are defined they can be applied to many assets with a single operation saving enormous amounts of time.
예를 들어 테스크 템플릿이 정의되면 한 번의 작업으로 많은 어셋에 적용할 수 있으므로 엄청난 시간이 절약됩니다.

And if dependencies are defined in those templates adjustments to one tasks schedule carry through to dependent tasks again reducing frustrating and time-consuming human labor.
또한 이러한 템플릿 조정에서 하나의 작업에 대한 의존성이 정의되면 다시 종속 작업에 착수하여 좌절과 시간이 많이 소요되는 인력을 줄입니다.

Automations like these are super helpful.
이런 자동화는 매우 유용합니다.

But every studio is unique and can benefit from additional custom automations.
그러나 모든 스튜디오는 독특하며 추가 사용자 지정 자동화의 혜택을 받을 수 있습니다.

For example maybe the modeling department would like to run the process on an asset when its status is set to done.
예를 들어 모델링 부서는 상태가 완료로 설정된 경우 어셋에 대해 프로세스를 실행할 수 있습니다.

Or maybe you'd like changes to an assets cost to automatically bubble up to a global budget report.
또는 어셋 비용을 변경하여 자동으로 글로벌 예산 보고서로 이동하기를 원할 수 있습니다.

There are an infinite number of useful automations that can be imagined across unique industries and Studios.
독특한 산업과 스튜디오에서 상상 가능한 수많은 유용한 자동화가 있습니다.

So how do we extend shotgun and create these custom automations.
그러면 우리는 어떻게 샷건을 확장하고 이러한 맞춤 제작을 할 수 있을까요?

Well there are several ways to do this.
이런 일을 하는 데는 여러 가지 방법이 있습니다.

With AMI toolkit apps scripts.
AMI으로 가능합니다.

But in this video we'll be focusing on what the shotgun event daemon brings to the table.
하지만 이 비디오에서는 샷건 이벤트 데몬이 테이블에서 데이터를 얻어오는데 초점을 맞출 것입니다.

Before we dig in you should know that this is not a video for beginners to Python or shotgun.
시작전에 이 비디오는 파이썬 또는 샷건 초보자 용이 아님을 알아야 합니다.

It's a good idea to watch the other videos in this training series first unless you're already an experienced chuck and developer who knows how to write efficient code.
당신이 이미 경험이 많고 효율적인 코드를 작성하는 법을 아는 개발자가 아니라면  이 교육 이전의 다른 비디오를 먼저 보는 것을 추천 합니다.

Otherwise you could create slow event daemon plugins that decrease performance on your shotgun site.
그렇지 않으면 느린 이벤트 데몬 플러그인을 생성하여 샷건 사이트의 성능을 저하시킬 수 있습니다.