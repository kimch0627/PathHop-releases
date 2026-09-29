# PathHop

PathHop은 Windows에서 자주 가는 폴더로 바로 이동하게 해 주는 작은 프로그램입니다. 단축키(기본 Ctrl+/)나 마우스로 메뉴를 열어 즐겨찾기, 최근 폴더, 탐색기에 열려 있는 폴더로 이동하고, 파일 열기·저장 대화상자도 그 폴더로 바로 옮깁니다. 글자를 입력하면 한글 초성으로도 바로 찾습니다.

이 저장소에는 설치 파일과 릴리스 설명만 있습니다.

## 내려받기

[Releases](../../releases/latest)에서 최신 버전을 내려받으세요.

| 파일 | 용도 |
|---|---|
| `PathHop-<버전>-x64.msi` | 설치 프로그램. `C:\Program Files\PathHop`에 모든 사용자용으로 설치하고, 로그인할 때 자동으로 실행합니다. 새 버전 MSI를 실행하면 그대로 업그레이드합니다. |
| `PathHop-<버전>-x64.zip` | 설치 없이 쓰는 포터블 버전. 압축을 풀고 `PathHop.exe`를 실행합니다. 실행 파일 옆에 `config.json`을 두면 설정도 그 폴더에 저장합니다. |
| `SHA256SUMS.txt` | 파일 확인용 SHA-256 값 |

내려받은 파일은 PowerShell에서 `Get-FileHash .\PathHop-<버전>-x64.msi`로 확인하고, 결과를 `SHA256SUMS.txt`의 값과 비교할 수 있습니다.

아직 코드 서명 전이라 처음 실행할 때 Windows SmartScreen 경고가 나올 수 있습니다. "추가 정보"를 누른 뒤 "실행"을 누르면 됩니다.

## 기업 배포

GPO, Intune 같은 도구로 무인 설치·제거할 수 있습니다.

```powershell
msiexec /i PathHop-<버전>-x64.msi /qn                  # 설치, 모든 사용자 자동 실행
msiexec /i PathHop-<버전>-x64.msi /qn AUTOSTART=0      # 자동 실행 없이 설치
msiexec /x PathHop-<버전>-x64.msi /qn                  # 제거, 사용자 설정은 남김
msiexec /x PathHop-<버전>-x64.msi /qn REMOVEUSERDATA=1 # 제거하는 사용자의 설정·기록·로그까지 삭제
```

## 지원 환경

- Windows 11 23H2 이상, x64. Windows 10에서도 동작하지만 공식 지원 대상은 아닙니다.
- 한국어, 영어

## 개인정보

PathHop은 어떤 데이터도 외부로 보내지 않습니다. 설정은 `%APPDATA%\PathHop`, 최근 폴더 기록과 로그, 비정상 종료 진단 파일은 `%LOCALAPPDATA%\PathHop`에만 저장합니다.

## QuickNav 사용자

0.3.0까지의 이름은 QuickNav였습니다. PathHop MSI를 실행하면 QuickNav를 PathHop으로 바꿔 설치하고, 처음 실행할 때 설정, 최근 폴더 기록, 자동 실행 설정을 그대로 옮깁니다.

## English

PathHop is a small Windows utility that jumps to your folders from a popup menu (Ctrl+/ by default): favorites, recent folders, folders open in File Explorer, and file dialogs. Download the MSI (per-machine install) or the portable ZIP from [Releases](../../releases/latest). It runs on Windows 11 23H2 or later (x64), has a Korean and English UI, and sends no data anywhere.
