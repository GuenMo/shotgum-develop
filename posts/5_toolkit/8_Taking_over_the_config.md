# [구성 인계받기](https://youtu.be/5nRZ5GgcOnk?t=21m3s)

So far we've been using the `app store descriptor` type to instruct chalk and desktop to download and load specific versions of TK config basic from the shotgun app store.
지금까지 저희는 앱 스토어 설명자 유형을 사용하여 분필과 데스크톱이 산탄 애플리케이션 스토어에서 기본 TK 구성의 특정 버전을 다운로드하고 로드하도록 지시했습니다.

Now the shotgun app store is actually just a shotgun site that we here at shotgun manage internally it's where we store the configs engines apps and frameworks that you as our clients.
사실 샷건 앱 스토어는 샷건에서 내부적으로 관리하는 샷건 사이트입니다. 고객으로서 구성 엔진 앱과 프레임워크를 저장하는 곳입니다.

But you can't upload anything to the App Store.
하지만 앱스토어에 아무것도 업로드할 수 없습니다.

You have no control and can't change anything.
당신은 통제할 수 없고 아무것도 바꿀 수 없다.

Bummer right but hold on.

You can use your own site the same way as a sort of private App Store that only users of your shotgun site can access.
사용자는 자신의 사이트를 샷건 사이트 사용자만 액세스할 수 있는 개인 App Store와 같은 방식으로 사용할 수 있습니다.

And that you can upload new configs engines apps and frameworks too.
또한 새로운 구성 엔진 애플리케이션 및 프레임워크를 업로드할 수 있습니다.

That's all made possible with the shotgun descriptor type.
이 모든 것이 샷건 `설명자 descriptor` 유형으로 가능합니다.

Let's get our hands dirty by changing this epithet.

Let's distribute the TK can fake basic payload to our artists from our own shotgun site instead of the shotgun app store.
TK는 샷건 앱스토어가 아닌 우리 자신의 샷건 사이트에서 우리 아티스트들에게 기본 페이로드로 위장할 수 있습니다.

First download the version of TK config basic that you're interested in from shotgun software's github site.
먼저 샷건 소프트웨어의 Github 사이트에서 관심 있는 TK 구성 기본 버전을 다운로드하십시오.

Next add a file link field through the pipeline configuration entity called uploaded config if someone hasn't already created.
그런 다음 업로드된 구성을 호출한 파이프라인 구성 엔티티를 통해 파일 링크 필드를 추가합니다(다른 사용자가 아직 생성되지 않은 경우).

It upload the TK config basic zip file the one that you just downloaded to the SG uploaded config field.
방금 SG 업로드 구성 필드에 다운로드한 TK 구성 기본 zip 파일을 업로드합니다.

Done! Now when I reload shotgun desktop it'll download the config from my shotgun site instead of the shotgun app store.
완료! 이제 샷건 데스크톱을 다시 로드하면 산탄총 앱 스토어 대신 내 산탄총 사이트에서 구성을 다운로드합니다.

And if I want I can see the download in the bundle cache by navigating to the shotgun descriptor folder.
원하는 경우 shogun descriptor 폴더로 이동하여 번들 캐시에서 다운로드를 볼 수 있습니다.

Here we have the ID of the pipeline configuration entity and the ID of the attachment that's storing the configuration payload as responsible.
여기 파이프라인 구성 엔티티의 ID와 구성 페이로드를 책임지는 대로 저장하는 첨부 파일의 ID가 있습니다.
