# Git_기초정리

Git은 `리누스 토르발즈`가 자기가 쓰던 분산 버전 관리 시스템인 `BitKeeper`가 유료화가 되서 빡쳐서 2주 만에 만든 분산 버전 관리 시스템이다.

쉽게 얘기하면 리모트 서버에 있는 파일을 내 컴퓨터로 복사한 후, 수정해서 다시 리모트 서버로 업데이트하는 것!

<br>

### Github 용어 정리

#### -Remote / Origin

 `Remote`는 말 그대로 리모트 서버 자체를 의미합니다. 우리가 자주 사용하는 G드라이브, N드라이브처럼 전 세계 어딘가에 있는 서버를 의미하고 Github은 `Git` 이라는 시스템에 필요한 리모트 서버와 `Git`을 조금 더 편리하게 사용할 수 있는 기능을 제공하는 친구입니다.

 `Git`을 사용할 때는 내가 어떤 리모트 서버에 변경 사항을 업로드 할 것인지 정해야하는데, 관례적인 이름이 `Origin` 입니다. 보통 한 개의 리모트 서버만 운용하는 경우가 대다수이지만 `Remote`와 `Origin`은 다른 말 입니다. 

#### -Repository

Remote Repository(원격 저장소) 와 Local Repository(로컬 저장소) 로 나눌 수 있다. 로컬 저장소는 말 그대로 내 컴퓨터에 있는 저장소를 의미하고, 원격 저장소는 서버에 있는 저장소를 의미한다. 기본적으로 로컬 저장소에서 작업을 수행하고 그 결과를 원격 저장소에 저장해 버전을 관리한다. 일반적으로 한 개의 Repository는 하나의 프로젝트를 의미하지만, Repository는 서버 내에서 구분되는 프로젝트 단위로 하나의 Repository에 여러 개의 프로젝트를 구성할 수도 있다.

#### -Branch

 `Branch`는 독립된 작업을 진행하기 위한 작업 공간을 의미한다. 맨 처음 Git을 초기화했을 때 기본적으로 `master` ( 요즘은 `main`) 이라는 이름의 브랜치가 하나 생성된다. 그 후, 개발하는 기능 또는 버그 픽스에 따라 브랜치를 새로 생성하고 작업한 후에 나중에 `master` 에 합칠 수 있다.

- Git을 초기화하면 생기는 `master` 혹은 `main` 브랜치가 메인 브랜치다
- 브랜치는 어떤 브랜치에서 분리시킨 것으로, 부모 브랜치가 항상 존재한다. 부모 브랜치의 상태를 그대로 가지고 분리된다.

#### -clone

 `clone`은 리모트 서버의 Repository에서 클라이언트로 파일을 복사하는 행위를 말합니다. 어떤 레파지토리에서 파일을 가져올 것인지에 대한 정보를 URL로 표현해주면 됩니다.

```bash
git clone https://github.com/travelbeeee/Tech_Knowledge
```

 `clone` 후에는 복사된 소스를 맘대로 수정하거나 파괴해도 리모트 서버에 업로드만 하지 않는다면, 영향을 주지 않는다.

#### -pull

 `pull` 명령어는 리모트 서버의 최신 소스를 가져와서 로컬 소스에 `병합(Merge)` 해주는 명령어입니다. 리모트 서버에서 소스를 클론 후에 다른 사람이 리모트 서버 소스를 갱신했다면, 변경된 사항을 알려주지 않기 때문에 직접 `pull` 명령어를 통해 병합을 해줘야합니다.

 `pull`은 단순히 가져온다의 개념이 아니라 가져와서 합친다(병합)는 개념이기 때문에 브랜치끼리도 `pull`을 통해 합칠 수 있습니다.

```bash
git pull # 현재 내 로컬 브랜치와 같은 이름을 가진 리모트 서버 브랜치가 타겟
git pull origin master # origin 리모트 서버의 master 브랜치가 타겟
```

#### -fetch

 `fetch` 명령어는 리모트 서버의 최신 이력을 가져오고 `pull`과 달리 병합은 하지 않는 명령어입니다.

 `fetch` 명령어를 사용해 리모트 서버의 새로 업데이트된 모든 내역을 받아온 후, 내역을 보고 `pull` 명렁어를 사용해 내 로컬 저장소의 소스 코드를 갱신할 수 있습니다. `fetch` 후 `merge` 를 수행하면 `pull` 명령을 실행했을 때와 같은 이력이 만들어집니다.

```bash
git fetch	
```

#### -add

 `add` 명령어는 리모트 서버에 어떤 변경 사항을 반영할지 정하는 명령어입니다. 물건을 배송 보내기 전에 어떤 물건들을 포장할 것인지 고르는 과정입니다.

```bash
git add . # 현재 디렉토리의 모든 변경사항을 스테이지에 올린다
git add ./src/controller # controller 디렉토리의 모든 변경사항을 스테이지에 올린다
git add ./src/controller/MainController.java # 특정 파일의 변경사항만 스테이지에 올린다
git add -p # 변경된 사항을 하나하나 살펴보면서 스테이지에 올린다
```

 이때 선택된 변경 사항들은 `스테이지(Stage)`라고 불리는 공간으로 이동하게 된다. `git status` 명령어를 사용하여 스테이지에 담긴 변경 사항들을 확인해볼 수 있고, `status` 명령어에 추가적으로 `-v`옵션을 사용하면 어떤 파일의 변경되었는지도 함께 볼 수 있다.

#### -commit

 `commit` 명령어는 스테이지에 있는 변경 사항들을 포장하는 명령어입니다. `commit`은 `Git`에서 하나의 버전으로 정의하기 때문에 매우 중요한 개념입니다. 변경을 기록하는 개념으로 파일 및 폴더의 추가/변경 사항을 저장소에 기록하려면 `commit`을 해줘야하빈다. `commit`을 하면 이전 `commit` 상태부터 현재 상태까지의 변경 이력이 기록된 `commit`이 만들어집니다.

```bash
git commit -m "커밋메시지"
```

#### -push

 `push` 명령어는 `commit`을 통해 포장된 변경 사항들을 리모트 서버로 업로드하는 명령어입니다. 커밋된 변경 사항들을 실제 리모트 서버에 전송하는 것이기 때문에 네트워크 연결이 되어있어야 하고, 어떤ㅉ 리모트 서버의 어떤 브랜치로 푸쉬할 것인지도 알려줘야 합니다.

```bash
git push origin master # origin 리모트 서버의 master 브랜치로 푸쉬
```

#### -fork

 `fork` 는 다른 사람의 Github repository에서 내가 어떤 부분을 수정하거나 추가 기능을 넣고 싶을 때 해당 respository를 내 Github repository로 그대로 복제하는 기능이다. fork한 저장소는 원본(다른 사람의 github repository)와 연결되어 있다. 여기서 연결 되어 있다는 의미는 original repository에 어떤 변화가 생기면(새로운 commit) 이는 그대로 forked된 repository로 반영할 수 있다. 이 때 fetch나 rebase의 과정이 필요하다.

 그 후 original repository에 변경 사항을 적용하고 싶으면 해당 저장소에 pull request를 해야한다. pull request가 original repository의 관리자로 부터 승인 되었으면 내가 만든 코드가 commit, merge되어 original 에 반영된다. pull request 하기 전까지는 내 github에 있는 forked repository에만 change가 적용된다.

<br>

참고

https://evan-moon.github.io/2019/07/25/git-tutorial/

https://tagilog.tistory.com/377

