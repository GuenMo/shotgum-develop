# 기초 구성을 잠가야 합니까

[Should I lock the basic configuration?](https://youtu.be/5nRZ5GgcOnk?t=13m5s)

Here's more good news.
더 좋은 소식이 있어요.

Desktops basic configuration also serves as the fallback configuration for all your shotgun projects.
데스크톱 기본 구성은 모든 샷건 프로젝트에 대한 예비 구성 역할도 합니다.

For example if I visit the hyperspace madness projects project environment, desktop checks the App Store for updates to the basic configuration and if there is an update its downloaded along with any app engine and framework versions that config references that don't already exist on a local disk.
예를 들어 하이퍼스페이스 광기 프로젝트 환경을 방문하는 경우 데스크톱은 App Store에서 기본 구성에 대한 업데이트를 확인하고, 아직 로컬에 없는 디스크를 구성하는 앱 엔진 및 프레임워크 버전과 함께 다운로드한 업데이트가 있는지 확인합니다.

![Local Image](/img/13/11.png)

After that I'm presented with a few apps if I visit another project I get the same thing.
그 후 다른 프로젝트를 방문하면 똑같은 것을 얻을 수 있습니다.

Later I'll show you how to override the config per project so we can make different sets of pipeline and artist tools available to different projects.
나중에 프로젝트당 구성을 재정의하는 방법을 보여 드리겠습니다. 그러면 다른 프로젝트에서 다양한 파이프라인 및 아티스트 도구를 사용할 수 있습니다.

But for now Desktop falls back to the same basic config that it checks for and downloads every time it's launched and that it uses for its site environment.
하지만 지금은 데스크톱이 실행될 때마다 그리고 사이트 환경에 사용할 때마다 확인하고 다운로드하는 동일한 기본 구성으로 돌아갑니다.

So do you want to take control of the basic configuration?
기본 구성을 제어하려고합니까?

Let's take a look at our handy decision-making spreadsheet again from all the screen you can tell that I think it's a good idea.
모든 화면에서 편리한 의사 결정 스프레드시트를 다시 살펴보도록 하겠습니다.

You'll have the flexibility to lock down config versions for your entire studio and/or lock specific projects to different configurations which empowers you to surgically distribute updates to pipeline tools instantly anyone who has access to your shotgun site on your schedule and rollback instantly if issues crop up.
유연하게 전체 스튜디오의 구성 버전을 잠그거나 특정 프로젝트를 다른 구성으로 잠그면 숏건 사이트에 즉시 액세스할 수 있는 경우 즉시 파이프라인 도구에 업데이트를 배포할 수 있습니다.

There's really no downside except for the obvious fact that some management is required.
일부 관리가 필요하다는 분명한 사실 외에는 어떤 단점도 없습니다.

But if you hope to unlock the raw power of toolkit and gain access it's amazeballs development platform then you'll have to lock down the config version anyway there's really no way around it.
하지만 툴킷의 원시 파워를 잠금 해제하고 amazeballs 개발 플랫폼에 접근하려면 구성 버전을 잠가야 합니다.

So are you a noodle er you have development resources to spare want to kick some ass at your studio does that sound like you cool and let's take a look at managing all this stuff.
자, 여러분은 여분의 개발 자원을 가지고 있는 라면 인가요? 여러분의 스튜디오에서 멋있게 보이고 이 모든 것들을 관리하는 것을 살펴봅시다.
