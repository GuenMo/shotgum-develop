# 버전 제어

[Version control](https://youtu.be/5nRZ5GgcOnk?t=22m53s)

As responsible citizens of the galaxy we should always encourage each other to version control critical files like a complex code or settings files.
책임있는 은하계 시민으로서 우리는 항상 서로에게 복잡한 코드나 설정 파일과 같은 중요한 파일들을 제어하도록 격려해야 한다.

This is one of those times.
지금이 바로 그런 때야.

I'm going to strongly encourage you to version control your config.
저는 당신이 당신의 구성을 제어할 것을 강력히 권할 것입니다.

It's up to you how you want to do it with perforce or git or CBS or SVN etc.
당신이 그것을 어떻게 할 것인가는 당신에게 달려있다.

If your studio already uses version control technology you should probably just use that.
스튜디오에서 이미 버전 제어 기술을 사용하고 있다면, 아마 그냥 사용해야 할 것입니다.

If you're a games company for example you'll likely want to use perforce.
예를 들어 게임 회사일 경우 Perforce를 사용할 수 있습니다.

Here at shotgun software we use github to manage and collaborate on code so I'll use github for this demo.
여기 샷건 소프트웨어에서는 Github를 사용하여 코드를 관리하고 공동작업을 하므로 이 데모에서는 github를 사용할 것입니다.

As an aside if you haven't chosen a version control technology or service at your studio yet I highly recommend github.
별개로, 만약 당신이 당신의 스튜디오에서 버전 제어 기술이나 서비스를 선택하지 않았다면 나는 Github를 강력히 추천한다.

It's got a snappy intuitive interface you can access it from anywhere as apposed for example to a git or perforce server running on a local network it's used and trusted by thousands of companies worldwide and you can easily migrate to a local git server if necessary.
이 인터페이스에는 간편한 인터페이스를 통해 어디서나 액세스할 수 있습니다. 예를 들어, 전세계 수천 개의 회사에서 사용하고 신뢰하는 Git 또는 Perforce 서버에 액세스할 수 있으며 필요할 경우 쉽게 로컬 서버로 마이그레이션할 수 있습니다.

Bitbucket is another good yet based solution for large corporate style studios.
Bitbucket은 대기업 스타일 스튜디오를 위한 또 다른 우수하지만 기반 솔루션입니다.

So if I'm going to keep track of changes I make to teacake and vague basic I need to initialize a github repository.
그래서 만약 내가 찻잔에 대한 변경사항과 모호한 기본사항을 추적하려면, 나는 Github 저장소를 초기화해야 한다.

My shotgun site is called learn so I'll call my repo TK config learn.
내 샷건 사이트는 `aiw`라고 부르기 때문에 나는 내 `tk-config-aiw`에 전화할 것이다.

Of course you could call it whatever you want.
물론 너는 그것을 네가 원하는 대로 부를 수 있다.

I'll clone that to my desktop dump in the contents of TK config basic.
TK 구성 기본 컨텐츠에 있는 데스크톱 덤프에 복제하겠습니다.

While being careful to preserve the docket ignore file a very easy thing to overlook.
도킷을 보존하기 위해 조심하는 동안 무시되는 것은 간과하기 쉬운 일이다.

And post that to a branch named after the version of TK config basic that I started with.
그리고 제가 시작한 TK config basic의 버전 이름을 따서 지었습니다.

Next I'll start a development dev branch which is where I'll post work-in-progress changes.
다음으로, 진행 중인 변경 사항을 게시하는 개발 개발 개발 지점을 시작합니다.

And a master branch which is where I'll tag and release code from actually why don't I do that right now.
그리고 제가 태그를 붙이고 코드를 풀 수 있는 지점인 마스터 분기는 왜 지금 당장 그렇게 하지 않는가?

I'll push up my master branch draft the initial release download the zip file and then upload it to our primary configuration entity.
마스터 버전 초안으로 zip 파일을 다운로드한 후 기본 구성 엔티티에 업로드하겠습니다.

See how the github version number is visible here.
Github 버전 번호를 보려면 여기를 클릭하십시오.

Cool right?
멋지지?

So we know exactly where it came from.
그래서 우리는 그것이 어디서 왔는지 정확히 안다.

Let's release and upload a new version to test that out.
새 버전을 릴리스하고 업로드하여 테스트해 봅시다.

Sweet but what if there's something wrong with our new version?
좋아요, 하지만 새 버전에 문제가 생기면 어쩌죠?

How do we know which version to roll back to?
어떤 버전으로 롤백할지 어떻게 알 수 있습니까?

Well you can right-click on the little attachment icon choose view uploaded config history and then filter for change event types.
작은 첨부 파일 아이콘을 마우스 오른쪽 단추로 클릭하면 업로드된 구성 기록 보기를 선택한 다음 이벤트 유형을 변경할 수 있습니다.

Now I know exactly what config I should roll back to if there are issues.
이제 문제가 있을 경우 어떤 구성을 롤백해야 하는지 정확히 알고 있습니다.

It's also handy to set up a permanent pull request against the original code you downloaded from shotgun software.
또한 샷건 소프트웨어에서 다운로드한 원래 코드에 대해 영구 당기기 요청을 설정하는 것도 편리합니다.

That way you can see the sum of all the changes you've made in one place.
이렇게 하면 한 곳에서 수행한 모든 변경사항의 합계를 볼 수 있습니다.

You could even load up older versions of our releases to really get a clearer view of what we've changed on our end.
이전 버전의 릴리즈를 로드하여 최종 단계에서 변경된 사항을 보다 명확하게 파악할 수 있습니다.

Finally let's talk about the master branch and per project branches.
마지막으로 마스터 분기와 프로젝트 분기에 대해 살펴보겠습니다.

After chatting internally we think it's a good idea to use the master branch to release to the global site config.
내부적으로 채팅한 후 마스터 분기를 사용하여 글로벌 사이트 구성에 릴리스하는 것이 좋습니다.

But you can also host project specific branches in the same repo.
그러나 프로젝트 특정 분기를 동일한 방식으로 호스트할 수도 있습니다.

For example if I wanted the artists working on the hyperspace madness project to get some extra special apps
I could create a hyperspace madness branch tag a release from there something like this
예를 들어, 만약 내가 하이퍼스페이스 광기 프로젝트를 하는 아티스트들이 몇 개의 특별한 애플리케이션을
초공간 광기의 분기 태그를 만들 수 있습니다.

And post it to a primary config linked to the hyperspace madness project.
그리고 그것을 하이퍼스페이스 광기 프로젝트와 연결된 일차 구성에 게시한다.

Get it!

Here's a picture in case your brain is filling up.
여기 당신의 뇌가 가득 찰 경우를 대비한 사진이 있습니다.

Awesome now not only are config payloads coming from our own shotgun sights app store
they're hooked up to specific released versions of github repo site or per project branches
that we control.
멋진 구성 페이로드가 우리의 숏건 관광 앱 스토어에서 오는 것뿐만 아니라
Gitub repo 사이트 또는 프로젝트 지점별로 릴리스된 특정 버전에 연결됩니다.
우리가 통제하는

This repo should be private and shared with other developers who might make changes to configs.
이 재작성은 비공개이어야 하며 구성을 변경할 수 있는 다른 개발자와 공유해야 합니다.