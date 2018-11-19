# 클라우드 구성 설치 및 관리

> 툴킷 파이프라인 구성을 통해 툴킷 통합 앱, 엔진 및 프레임워크를 사용할 수 있습니다.
> 이 동영상에서는 구성을 제어하는 방법, 즉 특정 버전으로 잠그는 방법, GitHub 또는 Perforce와 같은 버전 제어 환경에서 이를 변경하는 방법,
> 그리고 스튜디오 또는 원격에서 작업하는 아티스트에게 변경 사항을 배포하는 방법에 대해 설명합니다.
> 먼저 "기초" 툴킷 구성을 제거하고 고급 파일 관리 도구를 참조하는 "default2" 구성으로 전환할 것입니다.
> 또한 툴킷 앱, 엔진, 프레임워크 생성 및 관리에 대해서도 간략하게 설명합니다.

[Developer Training - Toolkit: Installing and Managing Cloud Configurations](https://www.youtube.com/watch?time_continue=1119&v=5nRZ5GgcOnk)

## 소개

Welcome to the second set of developer training videos for shotgun.
샷건을 위한 두 번째 개발자 교육 비디오에 오신 것을 환영합니다. 이는 산탄총 통합 및 툴킷 관련

Which focus on shotgun integrations and toolkit related topics.
이는 샷건 통합 및 툴킷 관련 주제에 초점을 맞춥니다.

If you're new to Python or the shotgun ecosystem of tools. I encourage you to stop this video and watch the first set of training videos before proceeding.
Python이나 숏건 에코시스템을 처음 접한 사람이라면 계속하기 전에 이 비디오를 중지하고 첫 번째 교육 비디오를 시청하십시오.

Shotgun is a powerful and secure platform that developers can build on top of by using the shotgun Python API
Shotgun은 강력하고 안전한 플랫폼입니다. 그리고 개발자가 Shotgun Python API를 사용하여  

to write standalone Python scripts action menu item scripts or shotgun event daemon plugins.
액션 메뉴 항목  또는 이벤트 데몬 플러그인을 그 위에 구축할 수 있게 해 줍니다.

The downside with those approaches is you're most likely building tools from scratch. Instead,
이러한 접근 방식의 단점은 처음부터 도구를 만들 가능성이 높다는 것입니다.

to jump start your studios pipeline and your own development projects

install shotgun desktop on your artists computers and start exploring the out-of-the-box integrations.

These include file management and communication tools that run inside popular DCC like maya nuke photoshop Houdini flame and 3ds max.
여기에는 Maya nuke 포토샵 Houdini flame 3ds 맥스와 같은 인기 DCC 내부에서 실행되는 파일 관리 및 통신 도구를 포함됩니다.

And standalone tools that users can launch directly from shotgun desktop.
그리고 유저가 바로 샷건 테스크 탑에서 독립 프로그램을 실행 시킬수 있습니다.

Like this publisher. App that lets you upload and attach files to entities in shotgun. Pretty cool, Right?
퍼블리셔 같이 말입니다. 이 앱은 파일을 업로드 시켜주고 파일과 엔티티와 연결해 줍니다. 훌륭하지 않습니까?

But I bet you want to know how to modify the standard tool set. Build new apps and distribute them to artists in your studio.
당신은 표준 도구 세트를 수정하는 방법을 알고 싶을 겁니다. 새 앱을 만들어 스튜디오의 아티스트에게 배포합니다.

How does all of that work? Well shotgun distributed tools are made available by the presence of what we call a pipeline configuration.
그 모든 게 어떻게 되는 걸 까요? Shotgun의 배포 도구는 소위 파이프라인 구성이라는 존재에 의해 이용이 가능합니다.

Essentially a collection of Python and yml settings files that control which versions of pre-built or custom
기본 적으로 파이썬과 yml 셋팅 파일 몪음은 샷건 관련 아티스트 도구가 서로 다른 상황에서 사용 가능한지를 제어 합니다.

shotgun related artist tools are available in different situations. In this video
다른 상황에서 사용 가능한지를 제어 합니다. 이 비디오에서는

I'm going to show you how to take control of the pipeline configuration. How to lock it down to a specific version.
파이프라인 구성을 제어하는 방법을 보여드리겠습니다. 특정 버전으로 잠그는 방법.

How to make changes to it in a version controlled environment like github or perforce.
Github 또는 Perforce와 같이 버전이 제어하는 환경에서 변경하는 방법.

And how to distribute those changes to artists working in your studio and artists working remotely.
그리고 이러한 변경 사항을 스튜디오에서 작업하는 아티스트와 원격으로 작업하는 아티스트에게 배포하는 방법.

Will start by ripping off the quote basic toolkit configuration and move on to our quote default to configuration later on.

Which references advanced file management tools.

I'll also cover shotgun centric version management for other types of software like Maya nuke Photoshop and shotgun desktop.
또한 Maya nuke Photoshop 및 snotwun desktop과 같은 다른 유형의 소프트웨어를 위한 산탄 중심 버전 관리에 대해서도 다룰 것입니다.

After that we'll dive a bit into custom app engine and framework creation and distribution.
그런 다음 사용자 지정 애플리케이션 엔진과 프레임워크 생성 및 배포에 대해 좀 더 자세히 알아보겠습니다.

If you've installed toolkit in the past you may already know how easy it is to get lost in the weeds.
과거에 툴킷을 설치했다면, 잡초에서 길을 잃는 것이 얼마나 쉬운지 이미 알고 있을 것이다.

Trying to determine the best install strategy.
최상의 설치 전략을 결정하려고 합니다.

Indeed there are several starting points in the advanced project setup wizard.

In addition to a handful of descriptor based cloud configuration options and other choices you make related to managing
실제로 고급 프로젝트 설정 마법사 구성에는 몇 가지 시작점이 있습니다.

configurations can result in a combinatoric explosion of possibilities. Here's a glimpse of more than

125 that I've identified.
제가 확인한 125개 이상의 사례가 있습니다.

So I hope to save you some time by guiding you through what I consider the best installation option currently available
따라서 프로젝트 전반에 걸쳐 글로벌이 가능한 파이프라인 구성을 만드는 관리 전략과

combined with a management strategy that creates pipeline configurations that
함께 현재 사용 가능한 최고의 설치 옵션을 안내하여 시간을 절약해

can be global across projects. Or project specific our version controlled accessible anywhere
드리고자 합하니다. 이러한 파이프라인은 어느 곳에서든 쉽게

secure easy to edit and distribute and that load fast. But know that the workflow
편집 및 배포할 수 있습니다. 하지만 이 비디오에서 제가 홍보하는 작업흐름이

I promote in this video may not be the best fit for all studios in all situations.
모든 상황에서 모든 스튜디오에 가장 적합한 작업흐름은 아닐 수도 있습니다.

If you have ubiquitous high-speed shared storage
유비쿼터스 고속 공유 스토리지가 있는 경우

start new projects infrequently and have limited need for off-site access. For example
새로운 프로젝트를 자주 시작하지 않고 외부 액세스가 제한적입니다.

other installation strategies may be easier to install and maintain.
예를 들어 다른 설치 전략은 설치 및 유지 관리가 더 쉬울 수 있습니다.

The strategy I promote here also relies on technology added the toolkit rather recently in 2017 and 2018.
제가 여기서 추진하고자 하는 전략은 2017년과 2018년에 이 툴킷을 추가한 기술에 달려 있습니다.

So documentation and support resources are comparatively limited. Again in my opinion
따라서 문서와 지원 리소스는 상대적으로 제한됩니다. 내 생각에

it's the most powerful and flexible approach. But you should confirm that it's right for you. At the end of the video
그것은 가장 강력하고 유연한 접근입니다. 하지만 너는 그것이 너에게 맞다는 것을 확인해야 한다. 비디오 끝에

I'll compare pros and cons with some other popular installation schemes.
나는 다른 인기 있는 설치 계획과 장단점을 비교할 것 입니다.

And if based on that discussion you instead decide to use the advanced project setup wizard to install
또한 이러한 논의를 바탕으로 고급 프로젝트 설정 마법사를 사용하여 도구 키트를 클래식 모드로

toolkit in classic mode. No worries. You'll still have learned a lot of useful stuff.
설치 할 것입니다. 걱정마세요. 여러분은 여전히 많은 유용한 것들을 배웠을 것입니다.

That is all to say this is definitely a red pill situation. You ready?
이것이 확실히 빨간 알약 상황이라고 말하는 것은 전부이다.