# MINIPLE v1

기존 Chrome YouTube Music 앱/PWA 또는 탭을 조작하는 경량 macOS 미니 플레이어입니다. 음악은 기존 YouTube Music에서 재생합니다. MINIPLE에는 Chrome 확장이나 Chromium 런타임을 설치하지 않습니다. 기존 Chrome의 로그인 상태를 사용합니다.

현재 배포는 `0.4.2-dev.1`입니다. 세 단계 연결 안내와 버튼 지연 개선을 포함합니다. v1은 제품 방식의 이름이며 정식 1.0 출시를 뜻하지 않습니다. 소스 저장소의 공개 범위는 변경하지 않습니다.

## 설치

```sh
brew install --cask GamjaIsMine02/tap/miniple
open /Applications/MINIPLE.app
```

Apple Silicon, macOS 13 Ventura 이상을 대상으로 빌드했습니다. Intel Mac과 다른 Mac은 검증하지 않았습니다. ad-hoc 서명한 개발 빌드이며 Apple 공증이 없습니다. macOS가 실행을 차단할 수 있습니다. Homebrew는 실행 승인을 대신하지 않습니다. 보안 설정과 격리 속성을 자동 변경하지 않습니다.

기본 설치 위치는 `/Applications/MINIPLE.app`입니다. Homebrew가 관리하지 않는 같은 이름의 앱이 있다면 다른 폴더를 선택하세요. `--force`로 덮어쓰지 마세요.

```sh
brew install --cask --appdir="$HOME/Applications" GamjaIsMine02/tap/miniple
```

이 위치에도 같은 이름의 앱이 있다면 다른 빈 앱 폴더를 사용하세요. Homebrew 설치는 앱을 자동 실행하지 않습니다.

## 처음 연결

1. MINIPLE을 실행합니다. 첫 연결을 완료하기 전에는 연결 안내가 자동으로 열립니다.
2. 기존 YouTube Music을 사용하는 Chrome 프로필에서 YouTube Music 앱/PWA 또는 `https://music.youtube.com/`을 엽니다. 로그인되지 않았다면 기존 YouTube Music에서 로그인합니다.
3. MINIPLE 안내의 `시스템 권한 요청`을 누릅니다. macOS의 MINIPLE → Chrome 제어 요청을 사용자가 허용합니다.
4. Chrome 상단 메뉴에서 `보기 → 개발자 → Allow JavaScript from Apple Events`를 켭니다. 한국어 메뉴에서는 Apple Events의 JavaScript 허용 항목을 찾으세요. 설정 페이지나 확장 관리 페이지의 옵션이 아닙니다.
5. MINIPLE의 `연결 확인`을 누릅니다. 곡을 선택하면 제목·표지와 재생 상태가 표시됩니다.

권한과 Chrome 설정은 사용자 승인이 필요합니다. 앱이나 Homebrew가 대신 켜지 않습니다. macOS 자동화 권한과 Chrome JavaScript 허용은 Chrome 단위이며 YouTube Music 전용 권한이 아닙니다. 앱 코드는 YouTube Music URL과 정확한 origin을 확인한 뒤 읽기와 명령을 실행합니다. [Chrome 공식 안내](https://www.chromium.org/developers/applescript/)

한 번 연결되면 이후 실행에서는 안내를 생략하고 자동 연결을 시도합니다. 재설치·빌드 변경으로 macOS가 권한을 다시 요청할 수 있습니다. 문제가 있으면 플레이어 `… → YouTube Music 연결`에서 안내를 다시 여세요. 하위 메뉴 없이 바로 연결 창이 열립니다. 권한을 거부했다면 `시스템 설정 → 개인정보 보호 및 보안 → 자동화`에서 MINIPLE의 Google Chrome 항목을 확인하세요.

연결 창은 하늘색·둥근 패널에 시스템 권한 요청, JavaScript 허용 안내, 연결 확인을 순서대로 표시합니다. 링크 복사·외부 문서 열기 버튼은 없습니다. 플레이어 메뉴는 단축키 변경, 투명도 조절, 연결, YouTube Music 열기, 종료만 표시합니다.

## 사용과 제한

곡 제목·아티스트·표지, 재생/일시정지, 이전/다음 곡, 볼륨, 좋아요, 셔플, 반복과 현재 재생목록을 표시·조작하도록 구현했습니다. LP는 재생 중 회전합니다. 투명도와 단축키를 변경할 수 있습니다. 기본 표시/숨김 단축키는 `Control + Option + M`입니다. 다른 내 플레이리스트로 전환하는 기능은 없습니다. 검색과 새 곡 선택은 기존 YouTube Music을 사용하세요.

기존 YouTube Music 창이나 탭을 닫으면 제어할 수 없습니다. 최소화는 가능합니다. 앱이 음악을 자동 시작하거나 Chrome을 강제 재시작하지 않습니다. 응답 실패 후 재생 명령을 자동 재실행하지 않습니다. 현재 재생목록은 페이지에 로드된 최대 100곡 범위입니다.

YouTube Music DOM을 사용하며 공식 제어 API는 아닙니다. 페이지 구조가 바뀌면 기능이 깨질 수 있습니다. Safari 웹 앱은 대상이 아닙니다. Google 또는 YouTube의 공식 앱이 아닙니다. 비밀번호·쿠키·로그인 프로필·개인 토큰은 앱 ZIP에 포함하지 않습니다.

개발용 검사에서 단위 테스트 23개, 생성된 AppleScript 컴파일, 모의 DOM의 빠른 읽기·전체 목록 읽기·비동기 상태 관찰·origin 제한, 최초 안내 정책·렌더링·메뉴와 앱 서명·구성 검사가 통과했습니다. 실행 파일 최소 대상도 Cask와 같은 macOS 13.0으로 검사합니다. 다른 Mac·계정에서의 실행은 미검증입니다. 서명 무결성과 macOS 실행 승인은 별개의 검사입니다.

초기 연결만 탭 주소를 일괄 검색하고, 이후에는 연결된 창·탭 ID를 직접 사용합니다. 명령은 추가 조회보다 먼저 처리합니다. 명령 뒤 실제 상태를 짧은 간격으로 확인하며 명령을 재실행하지 않습니다. 재생목록 전체 읽기는 목록 패널을 열었을 때만 합니다. 동일 코드를 적용한 이전 로컬 수정본에서 요청부터 재생 상태 렌더링까지 두 번 각각 233ms·270ms를 측정하고 원래 상태로 복원했습니다. 모든 환경의 지연 보장과 소리 출력 지연 측정은 아닙니다.

## 업데이트와 직접 다운로드

2026-10-07 공개 ZIP을 재다운로드해 원본과 일치하는지 확인했습니다. Homebrew로 이전 독립형 설치를 `0.4.1-dev.1`로 업데이트했고, 설치 앱의 버전·arm64·서명·구성·실행 파일 일치 검사가 통과했습니다. 설치 앱은 약 632KB입니다. macOS 실행 평가는 `rejected`였습니다. 격리 속성과 보안 설정은 변경하지 않았으며 배포 앱의 실제 재생과 최초 설치 GUI 검증은 완료하지 않았습니다.

이전 독립형 `v0.1.0-dev.1` Release를 제거했습니다. 배포 파일과 교체 전 앱의 복구용 로컬 백업은 보존했습니다.

```sh
brew update
brew upgrade --cask GamjaIsMine02/tap/miniple
```

직접 다운로드는 [개발용 Releases](https://github.com/GamjaIsMine02/miniple-releases/releases)에서 `MINIPLE-0.4.2-dev.1-arm64.zip`을 선택하세요. 각 ZIP에 SHA-256 파일을 제공합니다. GitHub의 `Source code.zip`은 앱 설치 파일이 아닙니다. 이전 독립형 앱의 로그인 프로필은 이 앱에서 사용하지 않으며 삭제하지 않습니다.

앱 파일이 GitHub와 Homebrew를 통해 전달되는 구조는 [Homebrew 배포 과정](DISTRIBUTION.md)에 정리했습니다.
