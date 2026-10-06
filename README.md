# MINIPLE

YouTube Music을 앱 내부에서 재생하는 macOS 미니 플레이어의 개발용 다운로드 저장소입니다. Chrome 확장이나 기존 YouTube Music PWA 연결이 필요하지 않습니다. 기존 Chrome 로그인 세션은 가져오지 않습니다.

이 저장소는 앱 배포 파일과 안내를 제공합니다. 기존 소스 저장소의 공개 범위를 변경하지 않습니다.

## 지원 범위

현재 개발용 빌드는 Apple Silicon Mac용입니다. 런타임의 최소 macOS 버전 선언은 13 Ventura입니다. 실제 계정 검증은 개발자의 macOS 26.5.2 환경에서 진행했습니다. 다른 macOS 버전과 Intel Mac에서의 실행은 검증하지 않았습니다.

앱은 ad-hoc 서명한 개발용 빌드이며 Apple 공증이 없습니다. macOS가 첫 실행을 차단할 수 있습니다. Homebrew 설치는 실행 승인을 대신하지 않습니다. 보안 설정 비활성화나 격리 속성 자동 제거 기능은 넣지 않았습니다. 일반 사용자용 서명·공증 릴리스는 아직 없습니다.

## Homebrew 설치

```sh
brew install --cask GamjaIsMine02/tap/miniple
```

기본 설치 위치는 `/Applications/MINIPLE.app`입니다. 같은 이름의 기존 앱을 덮어쓰도록 `--force`를 사용하지 마세요. 기존 앱을 유지하려면 다른 설치 폴더를 선택하세요.

```sh
brew install --cask --appdir="$HOME/Applications" GamjaIsMine02/tap/miniple
```

이 경우 설치 위치는 사용자 홈 폴더의 `Applications/MINIPLE.app`입니다. 해당 위치에도 같은 이름의 앱이 있다면 별도 폴더를 사용하세요.

직접 내려받을 때는 [개발용 Releases](https://github.com/GamjaIsMine02/miniple-releases/releases)에서 `MINIPLE-…-arm64.zip`을 선택하세요. GitHub가 생성한 `Source code.zip`은 앱 설치 파일이 아닙니다. 각 Release에 SHA-256 검증 파일도 첨부합니다.

## 첫 재생

1. 설치한 MINIPLE을 실행합니다.
2. 앱 내부의 최초 로그인 창에서 Google 계정으로 로그인합니다. 인증은 사용자가 직접 완료합니다.
3. 미니 플레이어의 `로그인 완료 확인`을 누릅니다.
4. YouTube Music 웹 화면에서 첫 곡을 선택합니다.
5. 웹 창을 닫아 숨긴 뒤 미니 플레이어를 사용합니다.

미니 플레이어에는 곡 제목·표지, 재생·일시정지, 이전·다음 곡, 현재 재생목록과 내 플레이리스트 전환이 포함됩니다. 표시/숨김 단축키는 `Control+Option+N`입니다. 앱 실행만으로 자동 재생하지는 않습니다.

Google 로그인 세션은 사용자의 `Library/Application Support/MINIPLEChromium`에 보관합니다. 로그인 프로필·쿠키·개인 토큰은 배포 ZIP에 포함하지 않습니다. 같은 계정으로 다른 기기에서 재생 중이면 YouTube Music이 재생 전환을 요구할 수 있습니다.

## 확인된 범위와 제한

개발자 계정으로 로그인, 실제 음원 재생·일시정지, 이전·다음 곡, 현재 재생목록 확인, 로드된 내 플레이리스트 선택 후 전환을 확인했습니다. 로그인 완료 후 한 번 재시작했을 때 세션 유지도 확인했습니다. 자동 DOM 테스트 20개와 별도 무음 Electron 통합 검사 12개가 통과했습니다.

30분 연속 재생과 다른 Mac·계정의 로그인은 검증하지 않았습니다. 보관함과 재생목록은 웹 페이지에 로드된 항목만 표시합니다. YouTube Music의 공식 제어 API가 아닌 DOM에 의존하므로 페이지 변경으로 기능이 깨질 수 있습니다. Google 또는 YouTube의 공식 앱이 아닙니다.

## 업데이트

```sh
brew update
brew upgrade --cask GamjaIsMine02/tap/miniple
```

앱 내부 자동 업데이트는 아직 없습니다. 새 개발용 ZIP을 게시하고 Cask 버전·검증값을 변경하면 Homebrew 업데이트로 배포합니다.
