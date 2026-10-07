# MINIPLE의 GitHub·Homebrew 배포 과정

MINIPLE 앱 ZIP은 GitHub Release에 저장합니다. Homebrew에는 ZIP 다운로드 주소와 설치 방법을 적은 Cask를 제공합니다. Homebrew가 앱을 대신 빌드하거나 앱 파일을 별도 서버에 올리는 방식은 아닙니다.

## 두 저장소의 역할

[miniple-releases](https://github.com/GamjaIsMine02/miniple-releases)는 다운로드할 ZIP·SHA-256 파일과 배포 안내를 제공합니다. [homebrew-tap](https://github.com/GamjaIsMine02/homebrew-tap)은 Casks/miniple.rb라는 설치 정의를 제공합니다. tap은 Homebrew가 읽을 수 있는 외부 Git 저장소입니다. macOS GUI 앱 설치에는 Cask를 사용합니다. [공식 tap 설명](https://docs.brew.sh/Taps)

## 개발자가 새 버전을 배포하는 순서

1. 소스 버전과 배포 버전을 올립니다. 이번 앱 내부 버전은 0.4.2, 공개 개발 배포 버전은 0.4.2-dev.1입니다. 이전 ZIP을 같은 URL에서 덮어쓰지 않고 새 버전 파일을 만듭니다.
2. 앱을 빌드하고 테스트합니다. MINIPLE에서는 npm run build:v1으로 MINIPLE.app을 생성합니다. 단위 테스트, 연결 안내 렌더링, AppleScript, 코드 서명과 앱 구성을 검사합니다. Cask와 실제 실행 파일의 CPU·최소 macOS 선언도 맞춥니다.
3. npm run package:v1으로 앱만 ZIP으로 묶습니다. 로그인 프로필·쿠키·개인 토큰은 포함하지 않습니다. ZIP의 SHA-256을 계산합니다. SHA-256은 내려받은 파일이 배포한 파일과 같은지 확인할 때 사용하는 검증값입니다. 개발자 신원이나 Apple 공증을 보증하는 값은 아닙니다.
4. GitHub Release에 새 버전 ZIP과 SHA-256 파일을 올립니다. 현재는 prerelease로 올립니다. 브라우저나 Homebrew가 이 URL에서 파일을 내려받습니다.
5. homebrew-tap의 Casks/miniple.rb를 새 버전과 검증값으로 변경해 GitHub에 push합니다. version은 버전, url은 ZIP 주소, sha256은 검증값, app은 설치할 MINIPLE.app입니다. depends_on은 Apple Silicon·macOS 지원 조건이고 caveats는 설치 후 보여줄 안내입니다. 패키징 스크립트가 Cask 내용을 생성하고 공개 저장소 반영은 현재 직접 수행합니다. [공식 Cask 설명](https://docs.brew.sh/Cask-Cookbook)
6. 공개 파일을 다시 다운로드하고 Homebrew 설치를 확인합니다. 다운로드 ZIP과 원본을 비교하고 설치 앱의 버전·구성·서명을 검사합니다. 설치 성공과 macOS 실행 승인은 별도로 기록합니다.

현재는 개인 tap을 사용하므로 Homebrew 공식 cask 저장소에 PR을 제출해 등록하는 절차를 거치지 않습니다. 앱스토어 심사나 Chrome 확장 심사로 이 설치 정의를 배포하는 것은 아닙니다. 자동 GitHub Actions 배포는 아직 구성하지 않았습니다.

## 사용자가 설치하는 순서

1. brew install --cask GamjaIsMine02/tap/miniple을 실행합니다. Homebrew는 GamjaIsMine02/homebrew-tap에서 MINIPLE의 설치 정의를 읽습니다. 없는 tap은 설치 명령에서 자동으로 추가합니다.
2. Cask의 url에서 ZIP을 다운로드합니다. sha256과 실제 파일의 검증값이 맞지 않으면 설치하지 않습니다.
3. ZIP을 풀고 app에 지정된 MINIPLE.app을 앱 폴더에 설치합니다. 기본은 /Applications이며 --appdir로 다른 폴더를 지정할 수 있습니다.
4. 사용자가 앱을 실행합니다. 실행 승인과 Chrome 연결 승인은 설치와 별개입니다. MINIPLE의 세 단계 안내에서 시스템 권한을 요청하고 Chrome의 JavaScript 설정을 직접 켠 뒤 연결을 확인합니다.

## 기존 사용자의 업데이트

```sh
brew update
brew upgrade --cask GamjaIsMine02/tap/miniple
```

brew update는 Homebrew와 tap의 설치 정의를 최신으로 갱신합니다. 이것만으로 MINIPLE 앱이 교체되지는 않습니다. brew upgrade는 새 Cask 버전의 ZIP을 내려받아 설치 앱을 교체합니다. 앱 내부 자동 업데이트 기능은 아직 없습니다. [공식 tap 갱신 설명](https://docs.brew.sh/Taps)

업데이트 전에 MINIPLE을 종료하세요. 기존 YouTube Music은 종료할 필요가 없습니다. Homebrew가 관리하지 않는 같은 이름의 앱은 --force로 덮어쓰지 마세요. 단축키·창 위치 등 사용자 설정과 Chrome 로그인은 ZIP 밖에 있으며 이 Cask는 삭제하지 않습니다.

## 현재 배포 제한

Apple Silicon과 macOS 13 이상을 대상으로 빌드합니다. Intel Mac과 다른 macOS 버전·계정은 실제 검사하지 않았습니다. ad-hoc 서명한 개발 빌드이고 Apple 공증이 없습니다. Homebrew 설치는 macOS 보안 검사나 사용자 승인을 우회하지 않습니다. 보안 설정과 격리 속성을 자동 변경하지 않습니다.
