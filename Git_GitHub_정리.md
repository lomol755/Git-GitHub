# Git / GitHub 정리

## 1. Git / GitHub

### 왜 써야 하나

- `보고서_최종.docx` → `보고서_진짜최종.docx` → `보고서_진짜최종_수정.docx` 이런 식으로 파일을 복사해서 관리하면 금방 꼬임. 이걸 해결해주는 게 **버전 관리**
- 이전 상태로 되돌릴 수 있음. 누가, 언제, 뭘 바꿨는지도 다 기록됨
- 여러 명이 같은 프로젝트를 동시에 작업할 수 있음 (각자 브랜치에서 작업하고 나중에 합침)
- GitHub에 올려두면 백업도 되고, 팀원이랑 코드 공유하기 쉽고, 포트폴리오로도 쓸 수 있음

### Git과 GitHub의 차이

- **Git**: 버전 관리 프로그램 (분산 버전 관리 시스템). 내 컴퓨터에 설치해서 씀. 인터넷 없이도 사용 가능. 리누스 토르발스가 리눅스 커널 관리하려고 만들었음
- **GitHub**: Git 저장소를 인터넷에 올려두고 공유하는 **서비스(호스팅 사이트)**. 같은 종류로 GitLab, Bitbucket이 있음
- 정리하면 Git = 도구, GitHub = 그 도구로 만든 저장소를 올려두는 곳
- GitHub 없이 Git만 써도 되는데, Git 없이 GitHub는 못 씀
- 비유하면 Git은 문서 편집 + 저장 기능이고 GitHub는 구글 드라이브 같은 클라우드 (거기에 협업 기능 PR, Issue 같은 게 더 붙어 있음)

### Repository (저장소)

- 프로젝트 파일 + 변경 이력을 담아두는 공간. 줄여서 **repo**
- 실제로 이력은 프로젝트 폴더 안의 `.git` 폴더에 들어 있음 (이걸 지우면 이력도 날아감)
- 로컬 저장소: 내 컴퓨터에 있는 것 / 원격 저장소: GitHub에 있는 것 (보통 이름이 `origin`)
- `git init`으로 새로 만들거나 `git clone 주소`로 원격 저장소를 통째로 복제

Git이 파일을 관리하는 영역이 3개 있음 (이거 중요)

```
작업 디렉터리  --git add-->  스테이징 영역  --git commit-->  로컬 저장소  --git push-->  원격 저장소(GitHub)
(Working Dir)               (Staging Area)                 (Local Repo)                (Remote Repo)
```

- 작업 디렉터리: 내가 실제로 파일 고치는 곳
- 스테이징 영역: 다음 커밋에 넣을 변경사항을 골라 올려두는 대기 공간
- `git pull`은 반대 방향 (원격 → 로컬)

### Commit

- 현재 프로젝트 상태를 **스냅샷으로 저장**하는 것. 게임 세이브 포인트 같은 느낌
- 커밋마다 고유한 해시값(40자리), 작성자, 날짜, 메시지가 붙음
- 나중에 이 시점으로 돌아갈 수 있음
- 커밋 메시지는 뭘 바꿨는지 알아볼 수 있게 쓰기. 예) `feat: 로그인 기능 추가`, `fix: 오타 수정`
- 한 커밋에 이것저것 다 넣지 말고, 하나의 변경 단위로 작게 자주 하는 게 좋음

### Branch

- 작업 흐름을 **갈라서 독립적으로 작업**할 수 있게 해주는 것
- 메인 코드는 건드리지 않고 새 기능을 따로 만들어보고, 다 되면 합침(merge)
- 내부적으로는 특정 커밋을 가리키는 포인터라서 만들기가 가벼움
- 기본 브랜치 이름은 `main` (예전에는 `master`)
- `HEAD`는 지금 내가 있는 브랜치(커밋)를 가리키는 포인터

---

## 2. git 기본 명령어

### git config

사용자 정보 설정. 커밋할 때 작성자로 이 정보가 들어감.

```bash
git config --global user.name "이름"
git config --global user.email "메일주소"
git config --list          # 설정 확인
```

- `--global`은 이 컴퓨터 전체에 적용, 옵션 없이 쓰면 현재 저장소에만 적용
- 이메일은 GitHub 계정 이메일이랑 맞춰야 내 커밋으로 인식됨

### git init

현재 폴더를 git 저장소로 만듦. `.git` 폴더가 생김.

```bash
git init
```

### git status

지금 상태를 확인. 제일 자주 치게 되는 명령어.

- `Untracked files`: git이 아직 추적 안 하는 새 파일
- `Changes not staged for commit`: 수정했는데 add 안 한 파일
- `Changes to be committed`: add 해서 커밋 대기 중인 파일

### git add

변경한 파일을 **스테이징 영역에 올림**.

```bash
git add index.html     # 특정 파일
git add .              # 현재 폴더의 변경사항 전부
```

### git commit

스테이징 영역에 있는 내용을 저장소에 **커밋**.

```bash
git commit -m "커밋 메시지"
```

- add 안 한 건 커밋에 안 들어감
- `-m` 안 쓰면 에디터(vim)가 열리는데, 당황하지 말고 `-m` 쓰기
- `git commit -am "메시지"`는 이미 추적 중인 파일의 수정분을 add + commit 한 번에 함 (새 파일은 안 됨)

### git push

내 로컬 커밋을 **원격 저장소에 올림**.

```bash
git remote add origin https://github.com/아이디/레포이름.git   # 원격 저장소 연결 (처음 한 번)
git push -u origin main    # 처음에는 -u 로 기본 연결 설정
git push                   # 이후에는 이것만
```

- push는 커밋한 것만 올라감. add / commit 안 하면 아무 일도 안 일어남

### git pull

원격 저장소의 변경사항을 **가져와서 내 브랜치에 합침**.

```bash
git pull origin main
```

- `git fetch`(가져오기만) + `git merge`(합치기)를 한 번에 하는 것
- 내가 모르는 사이에 팀원이 push 했을 수 있어서, 작업 시작 전이랑 push 전에 pull 하는 습관 들이기
- 내 로컬 작업이랑 겹치면 충돌(conflict) 날 수 있음

기본 흐름 (처음 올릴 때)

```bash
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/아이디/레포이름.git
git branch -M main
git push -u origin main
```

---

## 3. Branch

### git branch 명령어

```bash
git branch                  # 브랜치 목록 (현재 브랜치 앞에 * 표시)
git branch -a               # 원격 브랜치까지 보기
git branch feature/login    # 브랜치 생성 (이동은 안 함)
git switch feature/login    # 브랜치 이동
git switch -c feature/login # 생성 + 이동 한 번에
git merge feature/login     # 현재 브랜치에 feature/login 합치기
git branch -d feature/login # 삭제 (merge 된 브랜치만 삭제 가능)
git branch -D feature/login # 강제 삭제
```

- `git checkout 브랜치명`, `git checkout -b 브랜치명`도 같은 기능. checkout은 하는 일이 너무 많아서 브랜치 이동만 담당하는 `switch`가 따로 생김
- **merge는 합쳐지는 쪽이 아니라 받는 쪽에서 실행함.** main에 합치려면 main으로 이동한 다음에 `git merge feature/login` (이거 처음에 헷갈림)

### 브랜치 관리 (main, develop, feature, hotfix, release)

Git Flow라고 부르는 브랜치 전략.

- **main**: 실제 배포되는 코드. 항상 안정적인 상태여야 함
- **develop**: 개발 중인 코드를 모아두는 통합 브랜치. 다음 배포를 준비하는 곳
- **feature**: 기능 하나를 개발하는 브랜치. `feature/login` 처럼 이름 붙임
  - develop에서 갈라져 나오고, 다 되면 develop으로 merge
- **release**: 배포 직전 준비 브랜치 (최종 테스트, 버그 수정, 버전 정리)
  - develop에서 갈라져 나오고, 끝나면 main과 develop 양쪽에 merge
- **hotfix**: 배포한 뒤에 나온 급한 버그를 바로 고치는 브랜치
  - main에서 갈라져 나오고, 끝나면 main과 develop 양쪽에 merge

```
feature :  develop  ->  feature/*  ->  develop
release :  develop  ->  release/*  ->  main + develop
hotfix  :  main     ->  hotfix/*   ->  main + develop
```

- hotfix를 develop에도 merge 해야 하는 이유: 안 하면 다음 배포 때 이미 고친 버그가 다시 살아남
- 작은 프로젝트에서는 이렇게까지 안 하고 main + feature 브랜치만 쓰는 방식(GitHub Flow)도 많이 씀

### Fast-forward

합치려는 브랜치가 현재 브랜치에서 **일직선으로 앞서 있을 때** (현재 브랜치에 그 사이 새 커밋이 없을 때) 일어나는 병합.

- 새 커밋을 만들지 않고 **포인터만 앞으로 이동**함
- 이력이 일직선으로 깔끔하게 남음

```
merge 전
A - B            (main)
     \
      C - D      (feature)

git switch main
git merge feature

merge 후
A - B - C - D    (main, feature)
```

- 브랜치를 합쳤다는 흔적을 남기고 싶으면 `git merge --no-ff feature` (강제로 merge 커밋 생성)

### 3-way merge

두 브랜치가 갈라진 뒤에 **양쪽 모두 새 커밋이 생긴 경우**의 병합. fast-forward가 안 됨.

- 세 개를 비교해서 합침: 두 브랜치의 공통 조상 + 각 브랜치의 끝 커밋 (그래서 3-way)
- 합친 결과로 **merge 커밋**이 새로 생김 (부모가 2개)

```
merge 전
A - B - E          (main)
     \
      C - D        (feature)

merge 후
A - B - E - - M    (main)      M = merge 커밋
     \        /
      C ---- D     (feature)
```

- 서로 다른 파일이나 다른 부분을 고쳤으면 git이 알아서 합쳐줌

### Merge conflict

두 브랜치가 **같은 파일의 같은 부분을 다르게 수정**해서 git이 어느 쪽이 맞는지 판단 못 하는 상황. 3-way merge 중에 생김.

충돌 나면 파일에 이렇게 표시됨

```
<<<<<<< HEAD
현재 브랜치(내 쪽)에서 수정한 내용
=======
합치려는 브랜치에서 수정한 내용
>>>>>>> feature/login
```

해결 순서

1. 충돌난 파일을 열어서 어떤 내용을 남길지 직접 결정 (한쪽만 쓰거나 둘을 섞음)
2. `<<<<<<<`, `=======`, `>>>>>>>` 표시를 전부 지움
3. `git add 파일명`
4. `git commit` (merge 마무리)

```bash
git status            # 충돌난 파일 확인 (both modified)
git merge --abort     # 합치기 자체를 취소하고 merge 이전 상태로 돌아감
```

- 충돌 표시를 안 지우고 커밋하면 코드에 그대로 박힘 (주의)
- 예방하는 법: 자주 pull 하기, 같은 파일을 여럿이 동시에 건드리지 않기, 브랜치를 오래 끌지 않기
