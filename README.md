# PathHop

PathHop은 Windows에서 자주 가는 폴더로 바로 이동하게 해 주는 작은 프로그램입니다. 단축키(기본 Windows 키+/)나 마우스로 메뉴를 열어 즐겨찾기, 최근 폴더, 탐색기에 열려 있는 폴더로 이동하고, 파일 열기·저장 대화상자도 그 폴더로 바로 옮깁니다. 글자를 입력하면 한글 초성으로도 바로 찾고, `C:\`처럼 경로를 입력하면 하위 폴더를 따라 들어갑니다.

탐색기뿐 아니라 터미널(명령 프롬프트, PowerShell, Windows Terminal)에서는 그 폴더로 이동하는 명령을 입력하고, Total Commander·XYplorer·Files·Q-Dir에서는 보고 있는 패널을 옮깁니다. 지금 폴더를 다른 프로그램(VS Code 등)으로 여는 메뉴 항목과, 팀이 함께 쓰는 공유 메뉴도 만들 수 있습니다.

이 저장소에는 설치 파일과 릴리스 설명만 있습니다.

## 내려받기

[Releases](../../releases/latest)에서 최신 버전을 내려받으세요.

| 파일 | 용도 |
|---|---|
| `PathHop-<버전>-x64.msi` | 설치 프로그램. `C:\Program Files\PathHop`에 모든 사용자용으로 설치하고, 로그인할 때 자동으로 실행합니다. 새 버전 MSI를 실행하면 그대로 업그레이드합니다. |
| `PathHop-<버전>-x64.zip` | 설치 없이 쓰는 포터블 버전. 압축을 풀고 `PathHop.exe`를 실행합니다. 실행 파일 옆에 `config.json`을 두면 설정도 그 폴더에 저장합니다. |
| `SHA256SUMS.txt` | 파일 확인용 SHA-256 값 |
| `SHA256SUMS.txt.sig` | `SHA256SUMS.txt`의 서명. PathHop은 이 서명이 맞는 파일만 자동으로 설치합니다. |
| `latest.json` | PathHop이 새 버전을 확인할 때 읽는 버전 정보 |

내려받은 파일은 PowerShell에서 `Get-FileHash .\PathHop-<버전>-x64.msi`로 확인하고, 결과를 `SHA256SUMS.txt`의 값과 비교할 수 있습니다.

아직 코드 서명 전이라 처음 실행할 때 Windows SmartScreen 경고가 나올 수 있습니다. "추가 정보"를 누른 뒤 "실행"을 누르면 됩니다.

## 업데이트

PathHop은 하루에 한 번 이 페이지에서 새 버전을 확인합니다. 0.4.9부터는 새 버전을 미리 내려받아 서명과 SHA-256 값을 확인해 두고, 알림이나 트레이 메뉴의 "새 버전 설치하고 다시 시작"을 누르면 설치한 뒤 다시 실행합니다. MSI로 설치했다면 이때 Windows가 관리자 확인을 한 번 묻고, 포터블 버전은 실행 파일을 새 버전으로 바꿉니다. 설정 창 고급 페이지에서 끌 수 있습니다.

## 기업 배포

GPO, Intune 같은 도구로 무인 설치·제거할 수 있습니다.

```powershell
msiexec /i PathHop-<버전>-x64.msi /qn                  # 설치, 모든 사용자 자동 실행
msiexec /i PathHop-<버전>-x64.msi /qn AUTOSTART=0      # 자동 실행 없이 설치
msiexec /i PathHop-<버전>-x64.msi /passive LAUNCHAPP=0 # 설치가 끝난 뒤 바로 실행하지 않음
msiexec /x PathHop-<버전>-x64.msi /qn                  # 제거, 사용자 설정은 남김
msiexec /x PathHop-<버전>-x64.msi /qn REMOVEUSERDATA=1 # 제거하는 사용자의 설정·기록·로그까지 삭제
msiexec /i PathHop-<버전>-x64.msi /qn UPDATECHECK=0    # 새 버전 확인을 모든 사용자에게 끔
msiexec /i PathHop-<버전>-x64.msi /qn UPDATEURL=https://intranet/pathhop.json  # 사내 주소에서 새 버전 확인
msiexec /i PathHop-<버전>-x64.msi /qn SHAREDMENU=\\server\share\pathhop-menu.json  # 모든 사용자 메뉴 맨 위에 팀 공유 메뉴
```

사내 주소는 `{"version": "1.2.0", "url": "https://내려받는 페이지", "files": "https://사내 폴더/1.2.0/"}`를 돌려주면 됩니다. `files` 폴더에 이 페이지의 MSI, ZIP, `SHA256SUMS.txt`, `SHA256SUMS.txt.sig`를 그대로 두면 PathHop이 그곳에서 받아 설치하고, `files`가 없으면 새 버전을 알리기만 합니다. 자동 업데이트는 처음 설치할 때 준 `AUTOSTART`, `UPDATEURL`, `SHAREDMENU`를 그대로 유지합니다.

## 지원 환경

- Windows 11 23H2 이상, x64. Windows 10에서도 동작하지만 공식 지원 대상은 아닙니다.
- 한국어, 영어

## 개인정보

PathHop은 하루에 한 번 이 저장소에서 새 버전이 있는지 확인하고 새 버전의 설치 파일을 내려받는 것 외에는 어떤 데이터도 외부로 보내지 않습니다. 이 요청에는 제품 이름과 버전만 실리며, 설정 파일의 `update_check`나 MSI의 `UPDATECHECK=0`으로 끌 수 있습니다. 공유 메뉴를 정했다면 그 주소에서 메뉴 파일을 읽어 오기만 합니다. 설정은 `%APPDATA%\PathHop`, 최근 폴더 기록과 로그, 비정상 종료 진단 파일은 `%LOCALAPPDATA%\PathHop`에만 저장합니다.

## QuickNav 사용자

0.3.0까지의 이름은 QuickNav였습니다. PathHop MSI를 실행하면 QuickNav를 PathHop으로 바꿔 설치하고, 처음 실행할 때 설정, 최근 폴더 기록, 자동 실행 설정을 그대로 옮깁니다.

## English

PathHop is a small Windows utility that jumps to your folders from a popup menu (Windows key + / by default): favorites, recent folders, folders open in File Explorer, and file dialogs. It also changes the folder of terminals (Command Prompt, PowerShell, Windows Terminal) and of Total Commander, XYplorer, Files and Q-Dir, browses into folders as you type a path, opens folders in other programs, and can show a menu shared by your team. Download the MSI (per-machine install) or the portable ZIP from [Releases](../../releases/latest). It runs on Windows 11 23H2 or later (x64), has a Korean and English UI, and sends nothing anywhere except a daily check of this page for a new version (product name and version only; it can be turned off). From 0.4.9 it downloads a new version, verifies its signature and SHA-256, and installs it and restarts when you click the notification or the tray menu entry.
