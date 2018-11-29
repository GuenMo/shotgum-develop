# [Shotgun 데스크톱 설치 및 배포](https://youtu.be/5nRZ5GgcOnk?t=10m32s)

먼저 스튜디오에 있는 모든 사용자를 위해 shotgun 데스크톱 애플리케이션 버전을 관리하는 방법에 대해 알아보겠습니다.

안타깝게도 여기 클릭할 수 있는 아티스트보다 더 편리한 링크가 있어 최신 버전을 다운로드할 수 있습니다.

![Local Image](/img/5_toolkit/1.png)

문제는 아티스트가 다른 시간에 설치 프로그램을 다운로드하면 스튜디오와 다른 버전 설치 할수 있다는 것입니다.

![Local Image](/img/5_toolkit/2.png)

대신 스튜디오에 관리자가 설치 프로그램을 다운로드하고 배포 설치를 하게 합니다.

![Local Image](/img/5_toolkit/3.png)

This is a common best practice with most desktop software installers but again you'll have to work against the easy discoverability of the download link here to make it happen.
이는 대부분의 데스크톱 소프트웨어 설치 프로그램에서 일반적으로 사용하는 모범 사례이지만 다운로드 링크가 쉽게 검색될 수 있도록 하려면 여기에서 작업해야 합니다.

샷건 데스크탑 앱 자체는 현재 자동 업데이트 메커니즘이 없습니다.

That said it's really just a shell that hosts other toolkit components that do auto update by default so when should you distribute updates.
즉, 기본적으로 자동 업데이트를 수행하는 다른 툴킷 구성 요소를 호스팅하는 셸 뿐이므로 업데이트를 배포해야 합니다.

업데이트가 있는지 어떻게 알 수 있을까요?

숏건 데스크톱의 릴리스 노트 페이지를 방문한 다음 로그인하면 페이지 하단에 있는 다음 업데이트 버튼을 클릭할 수 있습니다.

이제 새 버전의 숏건 데스크톱이 출시될 때마다 알림이 표시됩니다.

그런 다음 릴리스 노트를 읽고 해당 노트를 다운로드하여 아티스트에게 배포할 가치가 있는지 확인할 수 있습니다.

새로운 버전의 숏건 데스크톱은 보통으로 자주 배포되지 않으므로 관리 작업에는 큰 문제가 없어야 합니다.

이제 데스크톱을 설치하고 실행 해 보겠습니다.

오른쪽 아래 메뉴에서 모자 아이콘을 클릭하면 버전 정보를 알수 있습니다.

![Local Image](/img/5_toolkit/4.png)

![Local Image](/img/5_toolkit/5.png)

이 모든 구성 요소는 숏건 데스크톱 애플리케이션이 현재 사용 중인 사이트 환경을 로드하는 데 필요한 구성 요소로, 기본적으로 새 버전으로 자동 업데이트되거나 새 버전을 잠그고 중앙 위치에서 관리할 수 있습니다.

![Local Image](/img/5_toolkit/6.png)

데스크톱 설치 프로그램과 달리 수동으로 아티스트 컴퓨터에 업데이트를 배포 할 필요가 없습니다.