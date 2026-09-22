# 검증 범위 / Validation scope

[한국어 소개](../README.md) · [English overview](../README.en.md)

## 1.0.1 정식 연결·업데이트 튜닝 / Stable connection and update tuning

초기 연결 응답을 화면 스레드 밖에서 기다리고, 종료한 앱의 대기 중인 연결을 취소합니다. 수동 재시도는 이전 예약을 취소하며 자동 재시도 간격은 최대 30초입니다. 업데이트 확인 실패는 성공으로 기록하지 않고, 5분 뒤 다음 시작·복귀 시 다시 확인할 수 있습니다. 정상 확인은 6시간 간격이며 수동 확인은 즉시 가능합니다. 시각·날짜·온도·미디어 문구가 같으면 반복해서 설정하지 않습니다. 아래 RC의 물리 키보드 포커스, Back 제한, 분할 크기 복구도 포함합니다.

기존 서명의 비디버그 release APK, versionCode 47입니다. SHA-256: `32292e9eda3e5d00a8aebeec1dfeca1c604c8f31f97132d7c165ad77be609b78` (20,678,200 bytes). **단위 검사 42개와 동일한 최종 APK의 통합 검사 22개가 통과했습니다.** Release Lint는 오류 0개, 경고 43개입니다. 통합 검사는 기능 확인과 Android ANR·충돌·6,000ms 감시 기준을 모두 통과해야 성공으로 집계합니다.

| 최종 APK 검사 / Exact-APK checks | 개수 / Count | 결과 / Result |
|---|---:|---|
| 느린 연결 중 화면 응답, 재시도 예약 교체, 오프라인 업데이트 재확인, 진행 중·대기 중 연결 종료 / Slow handshake responsiveness, retry timer replacement, offline update retry, closing during active and queued handshakes | 5 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 자체 연결: Back 30회 연타·지연 Back 만료, 생성 중 배율 변경·원격 크기 복구, 설정 복귀·입력 대체 경로·음소거 뒤 타이핑, 연결 프로세스 재시작, Settings/Clock 좌우 실행, release 설정 / Built-in connection: Back burst and expiry, creation and remote geometry recovery, keyboard focus and mute, process reconnect, two installed apps, release configuration | 10 | 통과 / Pass |
| Root 실행 어댑터: 연결 프로세스 재시작과 설정 복귀 후 타이핑 / Root launch adapter: reconnect and typing after settings | 2 | 통과 / Pass |
| 공식 Shizuku: 재연결, 지연 Back 만료, 입력 대체 경로 / Official Shizuku: reconnect, Back expiry and keyboard fallback | 3 | 통과 / Pass |
| 공개 1.0.0 및 1.0.1-rc5에서 실제 덮어쓰기 설치, 앱·분할 비율·배율·테마·즐겨찾기·바 위치·입력 설정 유지와 앱 터치 / Install over published 1.0.0 and 1.0.1-rc5, preserve settings and interact with the embedded app | 2 | 통과 / Pass |

공개 rc5에 새 회귀 검사를 실행하여 느린 연결의 화면 스레드 차단, 오래된 재시도 예약, 실패한 업데이트 확인의 성공 기록을 재현했습니다. 첫 정식 후보에서는 종료 후 대기 연결이 실행되는 경우를 추가로 재현하여 보완했습니다. 해당 실패 기록은 보존하며 통과 수에 포함하지 않습니다. 첫 후보의 기존 기능 검사 17개도 위 최종 APK 22개와 별도로 보관합니다.

두 번째 후보에서는 대체 입력 경로의 음량 내리기 후 다음 글자가 막혔습니다. 입력 기록에는 앱에 포커스가 돌아온 상태에서 display 0 대상 키 놓기 이벤트와 이후 글자가 대기 중이었습니다. 음량 키를 누르는 도중에는 포커스를 옮기지 않고 정상적인 키 놓기 뒤에 복구하도록 수정했습니다. 최종 검사는 자체 연결과 Shizuku에서 음량·음소거 8회 연속 조작 직후 타이핑을 포함하며, 단일 음량 명령이 5초 이상 걸려도 실패합니다. 수정 전 실패를 재시도 성공으로 덮어쓰지 않았습니다.

세 번째 후보의 Shizuku 검사에서는 포커스 복구 시간 초과와 글자 순서 변경을 함께 관찰했습니다. 같은 키 놓기에 일반 복구를 먼저 예약한 뒤 대체 입력 복구를 기다리는 중복 경로를 없앴습니다. 최종 후보에서는 두 연결 방식 모두 강화한 검사를 통과했으며, 입력·포커스 응답 대기 한도 500ms는 늘리지 않았습니다.

LGPL 자료의 앱 클래스 105개(생성 리소스 포함 139개)와 런타임 파일 30개가 최종 빌드 입력과 바이트 단위로 일치합니다. 이 자료만으로 재구성·서명·설치한 별도 APK도 자체 연결 프로세스 종료 후 재연결 검사 1개를 기능·시스템 상태 모두 통과했습니다. 배포 APK를 다시 설치하고 해시가 일치하는지 확인했습니다. 재구성 검사는 원본 22개와 별도입니다. 자료는 `DriveDeck-1.0.1-relink.zip`입니다.

전용 Android 13 에뮬레이터, 1600×900, density 160에서 확인했습니다. 자체 연결은 무선 디버깅·UID 2000·Shizuku 중지 상태, Shizuku는 공식 서버, Root는 AOSP 실행 어댑터입니다. 키보드는 Linux uinput 가상 USB/블루투스 장치입니다. 업그레이드는 ADB의 실제 패키지 덮어쓰기 설치이며 차량 설치 창이나 Magisk 승인 과정을 검증한 것은 아닙니다. 실제 차량·물리 키보드·한국어 입력기·다른 OEM·장시간 주행은 확인하지 않았습니다. `ro.adb.secure=0`인 AOSP 이미지여서 차량별 인증 정책도 별도 확인이 필요합니다. 아래 rc5의 144개 배치·13단계 배율 검사는 해당 RC의 기록이며 이번 최종 APK에서 다시 실행한 수치가 아닙니다. 이전에 관찰한 간헐적 시작 렌더링 지연이 모두 해결됐다고 주장하지 않습니다. 정식·실험 채널 모두 최종 1.0.1을 제공합니다.

This non-debuggable versionCode 47 final release waits for bridge handshakes off the UI thread, cancels queued work after closing, replaces old retry timers and caps backoff at 30 seconds. Failed update checks become eligible again after five minutes on the next start/resume; successful checks keep their six-hour interval. Unchanged clock/date/temperature/media text is no longer reapplied. RC keyboard, Back and pane recovery fixes are included.

All 42 unit tests and 22 checks on the exact final APK passed. Release Lint reports 0 errors and 43 warnings. System health is checked independently of assertions: Android ANRs, crashes and unexpected 6,000ms watchdog stalls fail the run. Published rc5 reproduced three new scheduling/update regressions; the first final candidate reproduced a queued handshake running after closure. Failed evidence is retained and excluded. The earlier candidate's 17 passing regression checks are also separate from the final 22.

The second candidate failed fallback typing after volume-down: a display-0 key release and later text remained pending while focus was already back on the app display. The final candidate restores focus only after a normal release. Built-in connection and Shizuku checks include eight consecutive targeted volume/mute commands, each immediately followed by typing; any individual command taking five seconds also fails. The failed candidate and its evidence remain separate.

The third candidate's Shizuku check observed a focus-restoration timeout and reordered text. The final candidate removes the duplicate ordinary restoration queued before the bounded fallback restoration for the same key release. Both connection paths passed the stronger check; the 500ms input/focus response wait was not increased.

All LGPL relink object inputs match the final build. A reconstructed APK separately passed one built-in bridge process reconnect check; the original APK was then restored and hash-verified. Tests cover Android 13 at 1600×900, native wireless debugging with Shizuku stopped, official Shizuku, an AOSP root adapter, virtual USB/Bluetooth keyboards, and ADB package upgrades from the two published versions. Actual vehicles, physical keyboards, Korean IMEs, other OEMs, Magisk approval and vehicle installer UI remain untested. AOSP uses `ro.adb.secure=0`. The 144 layouts and 13 scale steps below are historical rc5 results, not a rerun on this final APK. Previously observed intermittent startup renderer stalls remain unverified. Both update channels now offer final 1.0.1.

## 1.0.1-rc5 뒤로가기·구역 크기 복구 / Back and pane geometry recovery

사이드바 뒤로가기의 중복 요청·지연 입력을 제한하고, 앱 생성 중 바뀐 구역 크기와 배율을 실행 전에 적용합니다. 실행 중 실제 디스플레이 크기가 현재 구역과 다르면 주기적으로 다시 맞춥니다. 렌더링 크기는 실제 구역 픽셀에 맞추며, 글자 배율 기본값 160dpi와 저장한 배율은 유지합니다.

비디버그 release APK, versionCode 46, 기존 서명입니다. SHA-256: `c2642cc3b1d2745030d086927adfb645d1869f238fa07df3ab9337383ef1f104` (20,678,200 bytes). 단위 검사 39개 통과, Release Lint 오류 0개·경고 43개입니다. **아래 17개 검사는 동일한 최종 APK에서 기능과 시스템 상태가 모두 통과했습니다.**

| 최종 APK 검사 / Exact-APK checks | 개수 / Count | 결과 / Result |
|---|---:|---|
| 자체 연결: 사이드바 30회 연타, 입력 대기열 1.3초 지연 후 오래된 Back 폐기와 새 Back 전달, 생성 중 크기·배율 변경, 실행 중 원격 크기 변경의 자동 복구·실제 터치 / Built-in connection: 30 sidebar taps, delayed-input expiry and fresh Back delivery, creation-time size/scale changes, automatic repair and touch after remote geometry drift | 4 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 분할·전체화면·비율·바 위치·테마·즐겨찾기 조합 144개 및 120–240dpi의 13단계 터치·입력·Back으로 키보드 닫기 / 144 layout combinations and 13 scale steps with touch, typing and Back to dismiss the IME | 2 | 통과 / Pass |
| 가상 USB 키보드: 내비·음소거 후 입력, 바 터치 후 입력, 설정·앱 목록 복귀 / Virtual USB keyboard: navigation/mute, bar touches, settings/picker return | 3 | 통과 / Pass |
| 앱 교체 후 이전 세대 입력 거부, Settings·Clock 실제 터치, 단독 실행 후 복귀, release 설정, 내비 전체화면 Back 대상 / Stale input after app replacement, actual Settings/Clock interaction, standalone return, release configuration, fullscreen navigation Back target | 5 | 통과 / Pass |
| 루트 실행 어댑터: 사이드바 30회 연타 후 구역 사용 / Root launch adapter: pane remains usable after 30 sidebar Back taps | 1 | 통과 / Pass |
| 공식 Shizuku: 생성 중 크기·배율 변경과 지연 Back 폐기 / Official Shizuku: creation-time geometry changes and delayed Back expiry | 2 | 통과 / Pass |

기존 rc4에서는 생성 중 크기·배율을 바꾸면 오래된 크기가 남는 오류를 재현했습니다. 입력 대기열을 1.3초 막은 뒤에는 지난 Back 12회가 24개 DOWN/UP 이벤트로 뒤늦게 전달됐습니다. 단순 30회 연타만으로 차량의 오류 창 자체를 재현하지는 못했습니다. 첫 rc5 후보의 화면 크기는 맞았지만, 새 앱의 로딩 카드가 닫히기 전 터치를 보내 실패한 검사는 앱 준비 완료를 확인한 뒤 터치하도록 수정했습니다. 실패 자료는 보존하며 최종 결과에 포함하지 않습니다. 복구 뒤 앱이 이미 보이면 임시 복구 카드도 닫도록 보완했습니다.

LGPL 자료의 앱 클래스 105개(생성 리소스 포함 139개)와 런타임 파일 30개는 최종 빌드 입력과 바이트 단위로 일치합니다. 자료만으로 APK를 재구성·서명·설치한 뒤 생성 중 크기·배율 변경과 터치 검사 1개도 기능·시스템 상태 모두 통과했습니다. 재구성 APK 검사는 위 원본 17개와 별도입니다. 자료는 `DriveDeck-1.0.1-rc5-relink.zip`에 제공합니다.

검증 환경은 전용 Android 13 에뮬레이터 1600×900, density 160입니다. 자체 연결은 공식 무선 디버깅·셸 UID 2000·Shizuku 중지 상태, Shizuku는 공식 서버, 루트는 AOSP 실행 어댑터를 사용했습니다. 키보드는 Linux uinput 가상 USB 장치입니다. 실제 차량·물리 키보드·한국어 입력기·다른 OEM·장시간 주행은 확인하지 않았습니다. `ro.adb.secure=0`인 AOSP 이미지라 실제 폰의 인증 정책도 별도 확인이 필요합니다. 앞선 버전에서 관찰한 간헐적 시작 렌더링 지연이 해결됐다고 주장하지 않습니다. Android ANR과 6,000ms 감시 기준은 유지했습니다. 정식 채널은 1.0.0, 시험 채널은 1.0.1-rc5입니다.

This non-debuggable versionCode 46 preview limits duplicate and expired sidebar Back input, applies current size and density before launching an app, and repairs running display geometry without reapplying the layout. Rendering follows the actual pane pixel dimensions; the default 160dpi and saved scale are unchanged. All 39 unit tests and the 17 exact-APK checks above passed, including system health. Release Lint reports 0 errors and 43 warnings.

On published rc4, changing geometry during display creation reproduced stale dimensions. Blocking the input queue for 1.3 seconds delivered all 12 old Back taps later as 24 DOWN/UP events. A simple 30-tap burst did not reproduce the vehicle's error dialog. The first rc5 candidate corrected dimensions but a touch check ran before the new app's loading card closed; the final check waits for launch readiness before tapping. Those failed results are retained and excluded from final passes. Healthy visible apps now also dismiss a temporary Back-recovery card.

All relink object inputs match the final build byte for byte. A reconstructed APK separately passed one creation-time size/scale and touch check with system health. Tests use Android 13 at 1600×900, actual wireless debugging with Shizuku stopped, the official Shizuku server, an AOSP root launch adapter and a virtual USB keyboard. These do not establish actual vehicle, physical-keyboard, Korean-IME, OEM or prolonged-driving compatibility. AOSP uses `ro.adb.secure=0`. Previously observed intermittent startup renderer stalls remain unverified; Android ANR and the 6,000ms watchdog threshold were not relaxed. Stable remains 1.0.0.

## 1.0.1-rc4 바의 키보드 포커스 차단 / Control-bar keyboard focus

**사이드바와 즐겨찾기 바는 메뉴가 열려 있어도 키보드 포커스를 받지 않습니다.** 터치·길게 누르기는 유지합니다. 보조 키 연결이 끊긴 경우의 입력 경로와 음소거·볼륨 직후 첫 글자 순서, 메뉴 입력 분리도 보완했습니다. 정식 채널은 1.0.0, 시험 채널은 1.0.1-rc4입니다.

APK는 versionCode 45, 기존 서명, 비디버그 release 빌드입니다. SHA-256: `19b431b771f09fa5ed1d86334a2be5cd0302578959f96cbe2eae104bba268360` (20,678,200 bytes). 단위 검사 39개 통과, Release Lint 오류 0개·경고 43개입니다. 아래 통합 검사 **15개는 같은 최종 APK에서 기능·시스템 상태가 모두 통과**했습니다.

| 동일 APK 검사 / Checks on this exact APK | 개수 / Count | 결과 / Result |
|---|---:|---|
| 자체 연결·가상 USB 키보드: 분할/전체화면 복귀, 커서/삭제/단축키, 내비·음소거 후 입력, 메뉴 복귀, 바 터치 후 입력, 메뉴에서 Tab 32회씩, 보조 키 연결 중단 후 음소거·볼륨 직후 타이핑 및 메뉴 입력 분리 / Built-in connection and virtual USB keyboard: layouts, cursor/deletion/shortcuts, navigation/mute then typing, menu return, bar touches, 32 Tabs per menu, helper failure with immediate audio-key/typing and menu isolation | 8 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 배포 설정·테스트 진입 차단 / Release configuration and disabled test entry | 1 | 통과 / Pass |
| 앱 길게 누르기 메뉴·즐겨찾기 추가/삭제 / App long-press actions and explicit favorite add/remove | 1 | 통과 / Pass |
| Settings·Clock 좌우 실행 및 실제 터치·스톱워치 시작/정지 / Settings and Clock side by side, actual touches and stopwatch start/pause | 1 | 통과 / Pass |
| 선택·미선택 앱 단독 열기와 구역 복귀 / Assigned and unassigned standalone apps and pane return | 1 | 통과 / Pass |
| 루트 실행 어댑터: 설정·앱 목록에서 보조 입력 창 포커스 해제와 키보드 복귀 / Root launch adapter: key helper yields focus to settings/picker and restores editor input | 1 | 통과 / Pass |
| 공식 Shizuku·가상 Bluetooth 키보드·화면 키보드 동시 활성화: 메뉴 포커스 및 복귀, 보조 연결 중단 뒤 입력·음소거·볼륨 직후 타이핑·메뉴 입력 분리 / Official Shizuku, virtual Bluetooth keyboard and on-screen IME: menu focus/return, fallback typing, mute, immediate volume/typing and menu isolation | 2 | 통과 / Pass |

재링크 자료의 애플리케이션 클래스 105개(자동 생성 리소스 포함 139개), 런타임 파일 30개를 최종 빌드 입력과 바이트 단위로 대조했습니다. 자료로 APK를 재구성·서명·설치한 뒤 바 터치 후 물리 키보드 입력 검사 1개도 기능·시스템 상태 모두 통과했습니다. 재구성 APK 검사는 위 원본 APK 15개와 별도입니다. 자료는 `DriveDeck-1.0.1-rc4-relink.zip`에 제공합니다.

전용 Android 13 에뮬레이터(1600×900, density 160)에서 검증했습니다. 키보드는 Linux uinput 가상 장치이며 InputReader를 거치는 입력과 수신 장치 ID를 확인합니다. 실제 USB 장치나 Bluetooth 무선 통신 검사가 아닙니다. 자체 연결은 Shizuku 서버를 중지하고 실제 무선 디버깅 연결·셸 UID 2000을 확인했습니다. Shizuku 검사는 공식 서버를 사용했습니다. 루트 검사는 AOSP 실행 어댑터를 사용하며 실제 Magisk 승인 검사는 아닙니다.

개발 중 재현한 첫 글자 누락·순서 뒤바뀜·메뉴 입력 유입과 실패 자료는 보존합니다. 한 후보에서는 설정 닫기 터치 중 보조 입력 창이 포커스를 가져와 메뉴가 남았습니다. 최종 후보는 메뉴가 열린 동안 보조 입력 창의 포커스 가능 여부와 주기적 포커스 요청을 함께 끄며, 세 연결 방식에서 실제 해제를 검사합니다. 다른 후보에서는 창이 포커스를 받자마자 앱 1로 복원해 음소거 키를 놓쳤습니다. 자동 창 포커스 콜백의 복원을 제거하고 실제 키 처리 후 복원하도록 정리했습니다. 앞선 후보에서 시작 시 `HardwareRenderer.nSetStopped`의 6,000ms 이상 지연도 관찰했습니다. **최종 검사 통과가 이 간헐적 시작 지연의 해결을 입증하지는 않습니다.** 시스템 ANR과 6,000ms 상태 판정 기준을 완화하지 않았습니다.

일반 보조 창은 해당 화면이 최상위가 아닐 때 시스템 포커스 목록에서 제외될 수 있어, display 0 포커스 목록의 단일 스냅샷 대신 실제 음소거 DOWN/UP 수신·6.5초 대기·정확한 후속 입력을 확인합니다. 보조 프로세스 실행을 지연시키는 검사는 해당 실행 어댑터가 있는 루트 경로에만 적용했습니다. 최종 후보 이전의 반복 음량 검사 등은 위 15개에 포함하지 않습니다.

사용자 폰·실제 차량, 물리 USB/Bluetooth 키보드, 한국어 입력기·한영 전환, 다른 Android/OEM, 장치 재연결·시동 전원 차단·장시간 주행은 확인 전입니다. 이 AOSP 이미지의 `ro.adb.secure=0` 때문에 실제 폰의 ADB 인증 정책도 별도 확인이 필요합니다.

**Neither the sidebar nor favorites bar accepts keyboard focus, including while menus are open.** Touch and long-press actions remain available. This preview also changes fallback input after the optional key helper fails, preserves letter order immediately after mute/volume, and isolates menu input. Stable remains 1.0.0. This non-debuggable versionCode 45 APK retains the existing certificate. All 39 unit tests and the 15 exact-APK integration checks above passed; Release Lint reports 0 errors and 43 warnings.

All 105 application classes (139 including generated resources) and 30 runtime files in the LGPL relink materials match the final build inputs byte for byte. A reconstructed, signed and installed APK separately passed one bar-touch/physical-keyboard check with system health. That check is excluded from the 15 original-APK checks.

Tests use a dedicated Android 13 emulator at 1600×900, density 160. Virtual USB/Bluetooth keyboards send Linux uinput events through InputReader, with receiving-device IDs checked. These do not test physical USB devices or Bluetooth radio. Built-in connection checks use actual wireless debugging at shell UID 2000 with Shizuku stopped; Shizuku checks use the official server. Root checks use an AOSP launch adapter, not actual Magisk approval.

Development failures remain recorded, including lost/reordered first letters, menu input leakage and startup renderer stalls of at least 6,000 ms. An earlier candidate also left settings open when the key helper reclaimed focus during the close-button touch. The final candidate disables both that window's focus eligibility and periodic focus requests during menus; actual focus release is checked on all three connection paths. Another candidate restored the editor from a window-focus callback before receiving a mute key. That callback restoration was removed, retaining restoration after actual key handling. Passing final checks does not establish that the intermittent startup stall is fixed. System ANR and the 6,000 ms health threshold remain unchanged. A single display-0 focus snapshot was replaced with actual mute DOWN/UP receipt, a 6.5-second wait and exact subsequent text, since an ordinary window's focus eligibility depends on the top display. The delayed-launch adapter test runs only on its applicable root path. Earlier-candidate stress and recovery tests are not counted in the final 15.

The user's phone/car, physical keyboards, Korean IMEs and language switching, other Android/OEM versions, hotplugging, ignition power loss and prolonged driving remain unverified. Actual-phone ADB authentication also remains outside this AOSP image's `ro.adb.secure=0` scope.

## 1.0.1-rc3 물리 키보드 시험판 / Physical keyboard preview

앱 1의 물리 키보드 입력 경로, 내비 터치 뒤 입력 대상 유지, 설정·앱 목록 복귀 시 첫 글자와 메뉴 입력 분리를 수정했습니다. 자체 무선 디버깅·루트·Shizuku 연결을 유지합니다. 정식 채널은 1.0.0, 시험 채널은 1.0.1-rc3입니다.

APK는 versionCode 44, 기존 서명, 비디버그 release 빌드입니다. SHA-256: `9ecd731724df199ecac2f5485b3a42cf508e912193ed0892542812bfe51e0c12` (20,678,200 bytes). 단위 검사 39개 통과, Release Lint 오류 0개·경고 43개입니다. 아래 통합 검사 17개는 같은 APK에서 기능·시스템 상태가 모두 통과했습니다.

| 동일 APK에서 확인한 검사 / Checks on this exact APK | 개수 / Count | 결과 / Result |
|---|---:|---|
| 가상 USB 키보드: 분할·전체화면·복귀, 방향키·Home/End·삭제, Ctrl+A·Shift 선택, 내비·음소거 직후 입력, 설정·앱 목록 복귀 / Virtual USB keyboard: split/fullscreen/return, cursor/Home/End/deletion, Ctrl+A/Shift selection, navigation/mute then typing, settings/picker return | 5 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 메뉴 입력 분리, 런처 UI 일시 정지 중 별도 입력 전달, 음소거·볼륨 입력 180회 사이 글자 순서 / Overlay isolation, forwarding while launcher UI is paused, text order between 180 mute/volume inputs | 3 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 한 구역·두 구역 음소거 / Mute input with one and two panes | 2 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 실제 Settings 화면 조작·Clock 스톱워치 시작/정지 / Actual Settings interaction and Clock stopwatch start/pause | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 배포 설정·테스트 진입 차단 / Release configuration and disabled test entry | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 미선택·선택 앱 단독 실행과 구역 복귀 / Assigned and unassigned standalone apps and pane return | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 가상 Bluetooth 키보드와 화면 키보드 함께 사용: 메뉴 복귀·커서·삭제 / Virtual Bluetooth keyboard with on-screen IME enabled: menu return, cursor and deletion | 2 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 루트 경로: 실제 메뉴 버튼 터치·물리 키보드 복귀 / Root path: actual menu-button touches and physical keyboard return | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 공식 Shizuku 경로: 실제 메뉴 버튼 터치·물리 키보드 복귀 / Official Shizuku path: actual menu-button touches and physical keyboard return | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |

별도로 제공 재링크 자료의 애플리케이션 클래스 103개와 런타임 파일 30개를 빌드 입력과 대조했습니다. 자료만으로 APK를 재구성·서명·설치하고 자체 연결에서 메뉴 터치 후 물리 키보드 복귀 검사 1개가 기능·시스템 상태 모두 통과했습니다. 이 재구성 APK 검사는 위 원본 APK 17개 검사 수에 포함하지 않습니다.

Separately, all 103 application classes and 30 runtime inputs in the relink materials were compared with the build inputs. An APK rebuilt from those materials was signed and installed; one built-in-connection physical keyboard/menu-touch check passed functionality and system health. This rebuilt-APK check is separate from the 17 original-APK checks above.

대상은 전용 Android 13 에뮬레이터(1600×900, density 160)입니다. 물리 입력 검사는 Linux uinput에 가상 키보드를 등록하고 InputReader를 거쳐 입력하며, 앱이 실제 해당 장치 ID의 키를 받는지 확인합니다. 테스트용 장치 등록에만 에뮬레이터의 루트 권한을 사용합니다. 자체 연결 검사는 공식 Shizuku 서버를 중지한 상태에서 실제 무선 디버깅 연결과 셸 UID 2000을 확인합니다. 실제 USB 하드웨어나 Bluetooth 무선 통신을 검사한 것은 아닙니다.

개발 중 첫 글자 순서, 메뉴 입력 지연 전달, 입력 대기 실패를 재현한 기록을 보존합니다. 최종 후보의 초기 반복 검사 한 건은 매 글자를 display 0에 강제 지정해 Android가 매번 입력 화면을 바꾸도록 만들었고, 외부 편집기 ANR을 일으켰습니다. Android 13 소스의 해당 정책을 확인한 뒤 일반 키보드와 같이 화면을 지정하지 않는 방식으로 검사 입력을 수정했습니다. 단일 display-0 입력 전달 검사는 유지합니다. 제조사가 특정 화면에 묶은 키보드의 연속 입력은 이 결과로 검증되지 않습니다.

루트 회귀 검사는 실제 Magisk 승인을 대신하는 AOSP 실행 어댑터를 사용합니다. 메뉴를 내부 메서드로 직접 연 직후 입력 정책 값만 보고 타이핑한 검사에서는 첫 글자 하나가 앱 1에 남았습니다. 실제 사용자의 메뉴 버튼 터치가 수행하는 Android 포커스 전환을 포함하도록 검사를 바꾸었고 위 표의 루트 검사는 이 경로를 사용합니다. 코드로 메뉴가 열리는 순간의 입력 경합까지 해결됐다는 의미는 아닙니다.

최종 후보에서 화면 키보드를 함께 켠 메뉴 복귀 검사는 기능상 통과했으나, 시작 시 `HardwareRenderer.nSetStopped`에서 6,005ms 지연을 기록해 전체 통과로 세지 않았습니다. 초기 개발 후보에서도 렌더링 지연이 관찰됐습니다. 실패 자료와 6,000ms 시스템 상태 기준을 그대로 유지하며, 이후 통과 기록만으로 이 지연이 해결됐다고 단정하지 않습니다.

사용자 폰·실제 차량, 실제 USB/Bluetooth 키보드, 한국어 입력기·한영 전환 조합, 다른 Android·제조사, 장치 재연결·시동 전원 차단·장시간 주행은 확인 전입니다. 이 AOSP 이미지의 `ro.adb.secure=0` 때문에 실제 폰의 ADB 인증 정책도 별도 확인이 필요합니다. 이 버전의 LGPL 소스·애플리케이션 오브젝트·교체 절차는 `DriveDeck-1.0.1-rc3-relink.zip`에 제공합니다.

This preview changes App 1 physical-keyboard forwarding, preserves its input destination during navigation touches, and restores input after settings/picker transitions without delivering old menu keystrokes to the editor. Built-in wireless debugging, root and Shizuku remain available. Stable stays on 1.0.0. The APK uses versionCode 44, the existing certificate and a non-debuggable release build. All 39 unit tests pass; Release Lint reports 0 errors and 43 warnings. The table lists checks that passed both functional assertions and system health on this exact APK.

The dedicated Android 13 emulator runs at 1600×900, density 160. Physical-input tests register a virtual Linux uinput keyboard, send events through InputReader, and verify the keyboard's device ID in the receiving app. Emulator root is used only to create the test input device. Built-in connection tests stop the official Shizuku server and verify the real wireless-debugging connection at shell UID 2000. These are not physical USB or Bluetooth-radio tests.

Development failures remain recorded. An early repeated-input test on the final candidate forced every letter to display 0; Android then repeatedly moved that display to the front, competing with forwarded editor events and causing an external-editor ANR. After checking Android 13's policy source, the overlay test was changed to unassigned-display input, matching a normal keyboard. The single-key display-0 forwarding test remains. These results do not validate continuous typing from manufacturer keyboards explicitly bound to one display.

Root regression uses an AOSP launch adapter, not actual Magisk approval. Directly invoking a private menu method and immediately typing after the routing-policy flag changed left one letter in App 1. The test now includes the Android focus change caused by an actual user touching the menu button; the table's root result uses that path. It does not establish that every race during programmatic menu opening is resolved.

A final-candidate menu-return run with the on-screen keyboard also enabled passed its text assertions but recorded a 6,005 ms startup stall in `HardwareRenderer.nSetStopped`, so it is not counted as an overall pass. Earlier candidates also recorded renderer stalls. The failure records and 6,000 ms health threshold remain; later passing runs do not establish that these delays are fixed.

The user's phone, actual vehicles, physical USB/Bluetooth devices, Korean IMEs and language-switch combinations, other Android/OEM versions, hotplugging, ignition power loss and prolonged driving remain unverified. This AOSP image has `ro.adb.secure=0`, so actual-phone ADB authentication policy is also outside the tested scope. The companion relink ZIP contains the corresponding LGPL source, application objects and replacement procedure.

## 1.0.1-rc2 자체 연결 시험판 / Built-in connection preview

Shizuku 설치 없이 Android 무선 디버깅에 직접 연결하는 기능을 추가합니다. Android 13 이상이며 처음에는 시스템이 보여 주는 6자리 코드로 페어링해야 합니다. 루트·Shizuku 선택은 유지하고 기존 설치의 연결 방식도 보존합니다. 정식 채널은 1.0.0, 시험 채널은 1.0.1-rc2입니다.

APK는 versionCode 43, 기존 서명, 비디버그 release 빌드입니다. SHA-256: `7ffda0f21af6d82024c304d038f774a241c0e479aacd0a7cc7b750a93d594600` (20,629,048 bytes). 단위 검사 39개 통과, Release Lint 오류 0개·경고 44개입니다. 아래 15개 검사는 동일 APK에서 기능·시스템 상태가 모두 통과했습니다.

| 검사 / Check | 개수 / Count | 결과 / Result |
|---|---:|---|
| 배포 설정·테스트 진입 차단 / Release configuration and disabled test entry | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 잘못된 코드·취소, 실제 알림 페어링, 저장된 연결, 자동 복구, 한영 설정 화면 / Incorrect code and cancellation, real notification pairing, saved connection, bridge recovery, Korean/English setup | 5 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 실제 Settings·Clock 좌우 실행 / Actual Settings and Clock side by side | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 한 구역·두 구역 음소거 / Mute input with one and two panes | 2 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 키보드 입력 대상 유지 / Keyboard input routing | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 미선택·선택 앱 단독 실행과 복귀 / Assigned and unassigned standalone apps and return | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 실제 Settings 화면 조작·Clock 스톱워치 시작/정지 / Physical touches in Settings and Clock stopwatch start/pause | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 기존 루트 거부 처리·재연결 / Existing root denial and reconnect paths | 2 | 기능·시스템 상태 통과 / Functional and system-health pass |
| 공식 Shizuku 좌우 앱 실행·터치 / Official Shizuku panes and touch | 1 | 기능·시스템 상태 통과 / Functional and system-health pass |

전용 Android 13 에뮬레이터(1600×900, density 160)에서 Shizuku 서버를 중지한 상태로 자체 연결과 관련 기능을 검증했습니다. 실제 Android 코드와 알림의 RemoteInput/PendingIntent 경로를 사용하며, 루트 실행 어댑터로 자체 연결을 대신하지 않았습니다. 셸 UID 2000을 확인했습니다. 별도 루트 복구 검사는 AOSP 실행 어댑터, Shizuku 회귀 검사는 공식 서버를 사용했습니다. 실제 Magisk 승인을 검증한 것은 아닙니다. 이 AOSP 이미지의 `ro.adb.secure=0` 설정 때문에 실제 폰의 ADB 인증 정책까지 확인한 것도 아닙니다.

초기 구현에서는 mDNS 전환과 이전 ADB 스트림의 늦은 종료 응답을 처리하지 못해 연결이 실패했습니다. 수정 후 자체 연결 검사 5개가 모두 통과했습니다. 초기 개발 검사에서 6,101ms 렌더링 지연도 관찰됐으며 그 기록을 보존합니다. 최종 APK의 앞선 검사에서는 Settings를 강제 종료해 남긴 페어링 스레드·실패 창과 재시작 직후 설정 항목 탐색 실패 때문에 후속 검사가 실패했습니다. 정상적인 코드 창 취소 순서로 시험을 고치고 같은 APK에서 다시 검증했습니다. 실패 기록을 삭제하거나 시스템 상태 판정 기준을 완화하지 않았습니다.

사용자 폰·실제 차량, 다른 Android 버전·제조사, Wi-Fi 변경·기기 재부팅 후 복구, 시동 전원 차단과 장시간 주행은 확인 전입니다. 연결 재시도 시 무선 디버깅이 켜져 있어야 합니다. LGPL 라이브러리 소스·애플리케이션 오브젝트·교체 절차는 같은 배포의 `DriveDeck-1.0.1-rc2-relink.zip`에 제공합니다.

This preview adds direct wireless-debugging pairing without installing Shizuku. It requires Android 13+ and the six-digit code from Android Settings. Root and Shizuku remain available; upgrades retain the previous connection method. Stable stays on 1.0.0. The APK uses versionCode 43, the existing certificate and a non-debuggable release build. All 39 unit tests pass; Release Lint reports 0 errors and 44 warnings. All 15 checks in the table passed functionality and system health on this exact APK.

Built-in connection and related feature checks used a dedicated Android 13 emulator at 1600×900, density 160, with the Shizuku server stopped. They exercised real Android pairing and the notification's RemoteInput/PendingIntent route, then verified shell UID 2000 without a root-launch adapter. Separate root recovery used an AOSP launch adapter; the Shizuku regression used the official server. Actual Magisk approval was not tested. This AOSP image has `ro.adb.secure=0`, so the results do not validate a real phone's ADB authentication policy.

Early connection failures exposed mDNS handoff and late ADB stream-close handling; those were fixed. An early 6,101ms renderer stall remains in the records. Earlier checks of the final APK also failed after force-stopping Settings left pairing state behind, and when a settings row was not found immediately after an emulator restart. The test now cancels the code dialog normally, and the same APK was tested again. Failures remain recorded and health thresholds were not relaxed.

The user's phone, actual vehicles, other Android versions/manufacturers, Wi-Fi changes, device-reboot recovery, ignition power loss and prolonged driving remain unverified. Wireless debugging must be enabled when reconnecting. The companion relink ZIP supplies LGPL source, application object code and replacement instructions.

## 1.0.1-rc1 Shizuku 시험판 / Shizuku preview

루팅하지 않은 기기에서 사용할 Shizuku 연결을 추가했습니다. **Android 13 이상**, 별도로 설치·시작한 공식 **Shizuku 13 이상**이 필요합니다. 정식 채널은 1.0.0을 유지하며 1.0.1-rc1은 시험 채널에서 제공합니다. APK는 versionCode 42, 기존 서명, 디버깅이 꺼진 배포용 빌드입니다. SHA-256: `0b9e62f1463fa7dacbadb7fd8e1c58515ce00ddb43c47cf3a3fc806ded1f3eb7` (4,799,510 bytes).

최종 APK에서 **기능 검사 10개와 각 묶음의 시스템 상태 검사가 모두 통과**했습니다. 단위 검사 33개 통과, Release Lint 오류 0개·경고 37개입니다. 대상은 Android 13 전용 에뮬레이터, 1600×900·density 160입니다. 공식 Shizuku 13.6.0을 ADB 방식으로 실행했고 서버와 화면 프로세스의 UID 2000을 확인했습니다. 아래 Shizuku 검사에는 루트 실행 어댑터나 가짜 승인 응답을 사용하지 않았습니다.

| 최종 APK 검사 / Final APK checks | 개수 / Count | 결과 / Result |
|---|---:|---|
| 배포 설정·테스트 진입 차단 / Release configuration and disabled test entry | 1 | 통과 / Pass |
| 실제 Shizuku 좌우 실행·터치, 서버 종료 후 자동 복구, 연결 방식·한영 전환 및 설정 보존 / Real Shizuku panes and touch, server restart recovery, method/language switching and saved settings | 3 | 통과 / Pass |
| Settings·Clock 좌우 실행, 첫 알림 권한 창, 실제 터치·스톱워치 / Settings and Clock, first notification prompt, physical taps and stopwatch | 1 | 통과 / Pass |
| 닫히는 앱 목록과 테마 갱신 / Theme refresh while closing the picker | 1 | 통과 / Pass |
| 미선택·선택 앱 단독 실행, 터치, 구역 복귀 / Assigned and unassigned standalone apps, touch and pane return | 1 | 통과 / Pass |
| 키보드 입력 대상 유지 / Keyboard input routing | 1 | 통과 / Pass |
| 한 구역·두 구역에서 음소거 입력 / Mute input with one and two panes | 2 | 통과 / Pass |

개발 중 실제 Shizuku 승인창에서 거부→재승인을 확인했습니다. 최종 실행은 승인된 상태에서 시작했습니다. 처음에는 앱 내부 로그 파일을 셸로 넘기는 방식이 SELinux에 거부돼, 익명 파이프로 로그를 전달하도록 수정했습니다. 시스템 보안 정책을 바꾸지 않았습니다.

기존 루트 경로도 별도 2개 검사에서 루트 거부 시 설정 조작과 연결 종료 후 복구가 기능·시스템 상태 모두 통과했습니다. 복구 검사는 에뮬레이터의 AOSP 실행 어댑터를 사용하므로 실제 기기의 Magisk 승인 검증을 대신하지 않습니다. 위 10개와 합쳐 최종 APK의 통합 검사 12개가 통과했습니다.

Clock 첫 권한 창이 실행 대기 안내에 가려지는 실패도 발견했습니다. 해당 구역의 시스템 권한 창이 준비되면 대기 안내를 해제하도록 수정했고, 최종 APK에서는 권한 상태를 초기화해 실제 창부터 다시 확인했습니다. 오전 7시 테마 전환이 앱 목록 닫기와 겹쳐 목록을 다시 여는 문제도 수정하고 별도 재현 검사를 통과했습니다.

초기 후보 검사에서는 기능 성공과 별개로 약 6초의 화면 렌더링 지연을 기록했습니다. 재시작 후 이전 후보의 800×480 검사 5개와 최종 APK의 위 1600×900 검사 모두 통과했지만, 이 사실로 초기 실패 기록을 지우거나 지연 원인이 해결됐다고 단정하지 않습니다. 판정 기준은 완화하지 않았습니다. 사용자 폰·실제 차량, 다른 Android 버전과 제조사, 시동 전원 차단, 장시간 주행은 아직 확인 전입니다. 기기 재부팅 후에는 Shizuku를 다시 시작해야 합니다.

This preview adds a Shizuku connection for unrooted devices. It requires **Android 13+** and official **Shizuku 13+**, installed and started separately. Stable stays on 1.0.0; 1.0.1-rc1 is preview-only. The APK uses versionCode 42, the existing certificate, and a non-debuggable release build; its hash and size appear above.

All **10 functional checks on the final APK and every group's system-health check passed**, as listed above. All 33 unit tests passed; Release Lint reported 0 errors and 37 warnings. Checks used a dedicated Android 13 emulator at 1600×900, density 160. Official Shizuku 13.6.0 ran through ADB with server/bridge UID 2000. These Shizuku checks used neither a root-launch adapter nor a fake permission response. Denial followed by approval was exercised through the real Shizuku dialog during development; the final run started authorized.

An initial attempt to pass an app-private log file to the shell was blocked by SELinux. Logs now travel through an anonymous pipe without changing system security policy. A separate failure left Clock's first notification prompt behind the launch overlay. The overlay now clears when the system permission activity is ready; the final check reset notification permission and answered the actual prompt before interacting with both apps. Theme refresh could also reopen a closing picker; a dedicated regression check now passes.

Two separate checks of the existing root path also passed functionality and system health: usable settings after root denial, and recovery after a dropped connection. Recovery used the emulator's AOSP launch adapter and does not validate actual Magisk approval. This brings the final APK's passing integration checks to 12.

Initial candidates recorded roughly six-second rendering delays despite some passing functional assertions. Five checks on an earlier candidate at 800×480 after restart and the final 1600×900 checks passed, but this does not erase those failures or establish that their cause is resolved. Health thresholds were not relaxed. The user's phone, an actual vehicle, other Android versions/manufacturers, ignition power loss and prolonged driving remain unverified. Start Shizuku again after rebooting the device.

## 1.0.0 출시 검증 / Release validation

1.0.0은 GitHub 정식 배포이며, 아래 RC8 기록은 과거 시험 결과로 보존합니다. 배포용 APK는 디버깅이 꺼져 있고 테스트 전용 연결 진입점을 거부합니다. 기존 RC와 같은 서명으로 업데이트 호환성을 유지합니다. 단위 검사 33개 통과, Release Lint 오류 0개·경고 37개입니다.

첫 release 후보에서 단독 실행 앱이 전면에 떠도 입력이 꺼진 가상 디스플레이에 남는 실패를 두 번 재현했습니다. 수정본은 단독 실행 전에 입력 복원을 취소하고 분할 화면을 숨긴 뒤 외부 앱을 열도록 순서를 정리했습니다. 큐에 남은 focus 요청도 실행 시 현재 화면 상태를 다시 확인합니다. 실패 이력은 보존합니다.

최종 배포 APK의 **핵심 검사 7개가 기능·시스템 상태 모두 통과**했습니다. 배포 설정 1개, 음소거 입력 2개, 자동 재연결 1개, 미선택/선택 앱의 단독 실행·실제 터치·구역 복귀 1개, 실제 Settings/Clock 좌우 실행·터치 1개, 한국어↔영어 전환·설정 보존 1개입니다. 단독 실행 검사는 앞선 수정 확인 실행에서도 통과했습니다. 최종 검사 중 새 Android ANR이나 watchdog 지연은 관찰되지 않았습니다.

게시 후 RC8에서 정식 채널을 통해 1.0.0을 실제 다운로드·검증하고, 새 프로세스에서 설치 파일을 복구해 Android 확인창으로 설치했습니다. 두 단계의 기능·시스템 상태가 통과했고, 설치된 versionCode 41·APK 해시·기존 설정 XML 전체 보존을 확인했습니다. 정식·시험 채널 모두 같은 APK를 가리킵니다.

검사는 실제 비디버그 release APK를 설치한 Android 13 전용 에뮬레이터에서 수행합니다. 에뮬레이터의 AOSP root 실행 어댑터는 계측 앱에만 들어 있으며, 실제 차량의 Magisk 승인 절차를 대신 검증한 것은 아닙니다. 실차 음소거·연결 끊김의 해결, 장시간 주행, 모든 앱·제조사 조합은 확인 완료가 아닙니다.

Version 1.0.0 is the official GitHub release; the RC8 results below are retained as historical evidence. The release APK disables debugging and rejects its test-only connection entry point. It retains the existing certificate for RC upgrade compatibility. All 33 unit tests passed; Release Lint reported 0 errors and 37 warnings.

The first release candidate reproduced a standalone-launch failure twice: the external app was in front, but input remained on an off virtual display. The revised candidate cancels input restoration and hides the panes before opening the external app. Queued focus requests also recheck the current screen state. Failure evidence is retained.

All **seven core checks on the final release APK passed both functional and system-health checks**: release configuration (1), mute input (2), automatic reconnection (1), standalone launch/touch/pane return for assigned and unassigned apps (1), actual side-by-side Settings/Clock interaction (1), and Korean/English switching with settings preservation (1). Standalone launching also passed the preceding fix-verification run. No new Android ANR or watchdog delay was observed during the final suite.

After publication, RC8 downloaded and verified 1.0.0 through the stable channel. A new process recovered the file and opened Android's installation confirmation. Both instrumented phases passed functional and system-health checks. Installation completed with versionCode 41, the matching APK hash, and the entire launcher settings XML preserved. At 1.0.0 publication, stable and preview pointed to the same APK. The Shizuku preview is now provided separately on the preview channel.

Checks run against the actual non-debuggable release APK on a dedicated Android 13 emulator. The AOSP root-launch adapter exists only in the instrumentation app; these checks do not validate the vehicle's Magisk approval flow. Vehicle mute/disconnection resolution, prolonged driving, and every app/manufacturer combination remain unverified.

## RC8 기록 · 한국어

**RC8은 시험판이며 정식 1.0.0 출시 판정은 미완료입니다.** 이 페이지는 기능 성공과 응답성 문제를 함께 기록합니다.

| 확인 항목 | 결과 |
|---|---|
| 단위 검사 | 33개 통과 |
| Android Lint | 오류 0개, 경고 37개 |
| 한국어 ↔ 영어 | 앱 내 선택과 화면 재생성 확인 |
| 설정 보존 | 좌우 앱 선택, 즐겨찾기, 분할 비율, 조작 바 위치 유지 확인 |
| 연속 언어 변경 | 한국어·영어·기기 설정 따르기를 반복해도 잘못된 안전 모드 진입 없음 |
| 재실행 | 새 앱 프로세스에서 영어 선택과 앱 선택 유지 확인 |
| 작은 화면 | 800×480에서 언어 선택과 길게 누르기 메뉴 6개 도달성·48dp 이상 조작 영역 확인 |
| RC7 → RC8 업데이트 | GitHub 실제 다운로드, 해시·서명 검증, 새 프로세스에서 파일 복구, Android 설치 확인, versionCode 40·설정 보존 확인. 두 단계의 기능·시스템 상태 통과 |
| 실제 화면 | Android 13 에뮬레이터에서 캡처. 별도 앱은 Organic Maps와 Android 시계 |

언어·설정 보존 기능 검사의 단언은 통과했지만, 한 실행에서 메인 스레드 응답 감시가 **6,076ms 지연**을 기록해 그 묶음의 시스템 상태 검사는 미통과로 남겼습니다. 당시 스택은 Android의 대기 경로인 `MessageQueue.nativePollOnce`였고 원인은 단정하지 않았습니다. 새 시스템 ANR 기록은 없었습니다. 이후 연속 전환·문서 화면·재실행 검사에서 시스템 상태 검사가 통과한 사실로 이 기록을 지우지 않습니다.

첫 언어 검사는 다른 앱이 제공하는 이름인 ‘테스트 지도’까지 번역 대상이라고 잘못 검사해 실패했습니다. 앱 이름은 원래 유지하는 동작이므로 검사에서 설치된 앱 이름을 구분하도록 수정했습니다. 초기 문서 촬영은 앱 로딩 중 화면이 찍혀, 실제 앱이 열린 것을 확인하고 다시 촬영했습니다. 이 화면을 준비된 앱 화면으로 소개하지 않았습니다.

이후 예시 지도 위치를 준비한 촬영 실행에서는 Organic Maps 시작 화면의 입력 응답 시간 초과(ANR)가 기록됐습니다. 당시 지도 앱은 Android의 TTS 엔진 조회를 기다리는 스택이었습니다. 해당 묶음은 기능 단언 성공과 별개로 시스템 상태 미통과이며, 기록을 보존하고 전용 에뮬레이터를 재부팅했습니다. 원인을 DriveDeck 또는 지도 앱으로 단정하지 않습니다.

재부팅 중에도 System UI·Bluetooth·미디어 저장소·권한 관리 서비스의 ANR이 기록돼 후속 검사의 시작 조건 확인에서 중단했습니다. 부팅 완료 후 System UI의 대기를 선택했지만 이후 촬영에서도 지도 앱 ANR과 시스템 오류창이 반복됐습니다. 반복 오류창에서 닫기를 선택했으며 모든 실패 기록을 보존했습니다. 이 테스트 환경의 응답성 제한까지 포함해 정식 출시 가능 상태로 판단하지 않습니다. 공개 문서는 앞선 화면과 별도로 다시 촬영한 앱 목록을 사용하며, 수동 촬영을 전체 시스템 검증 통과로 계산하지 않습니다.

이전 RC7에서는 실제 GitHub 다운로드, 해시·서명 검증, 프로세스 재시작 후 파일 복구, Android 설치와 설정 보존을 확인했습니다. 별도 RC7 검사에서는 화면 종료 중 약 6.8초 지연을 기록했습니다.

차량 음소거 버튼, Bluetooth·후방카메라 복귀, 시동 전원 차단, 제조사별 루트·입력 처리, 장시간 반복 단독 실행을 포함한 모든 차량 조합은 검증 완료가 아닙니다. 차량의 기존 멈춤·연결 끊김이 해결됐다고 주장하지 않습니다.

## RC8 history · English

**RC8 is a preview, not a stable 1.0.0 release.** This page records both functional results and responsiveness issues.

| Check | Result |
|---|---|
| Unit tests | 33 passed |
| Android Lint | 0 errors, 37 warnings |
| Korean ↔ English | In-app selection and screen recreation verified |
| Settings preservation | Pane choices, favorites, split ratio, and control-bar side retained |
| Repeated language changes | Korean, English, and device-language selection did not incorrectly trigger Safe mode |
| Process restart | English selection and app selection persisted in a new app process |
| Small display | At 800×480, language choices and all six app actions were reachable, with action targets at least 48dp high |
| RC7 → RC8 update | Actual GitHub download, hash/signature checks, recovery in a new process, Android installation confirmation, versionCode 40, and settings preservation verified. Both instrumented phases passed assertions and system-health checks |
| Screenshots | Captured on an Android 13 emulator with Organic Maps and Android Clock as separate apps |

The language/settings assertions passed, but one run recorded a **6,076ms responsiveness delay**, so that run's system-health result remains failed. Its sampled stack was in Android's `MessageQueue.nativePollOnce` wait path; no cause was established. No new system ANR was recorded. Later passing health checks for repeated switching, documentation capture, and process restart do not erase this result.

The first translation check incorrectly treated another app's Korean label as DriveDeck text. App labels are intentionally preserved, so the check was corrected to identify installed app names. Initial documentation captures showed apps still loading; captures were repeated after confirming the apps had opened, rather than presenting loading screens as ready workspaces.

A later capture run with an example map location recorded an input-timeout ANR in Organic Maps' startup screen. Its sampled stack was waiting for Android's TTS engine lookup. That run remains a system-health failure despite passing its functional assertions. Evidence was retained and the dedicated emulator was rebooted. No cause is attributed to DriveDeck or the map app.

The reboot also recorded ANRs in System UI, Bluetooth, media storage, and permission services, so the next test stopped at its precondition check. Wait was selected after boot, but a later capture again encountered a map-app ANR and the system error dialog. Close app was selected on the recurring system dialog, and all failure records were retained. These test-environment responsiveness limits are another reason this is not considered ready for stable release. Published documentation uses earlier captures and separately recaptured app choosers; manual capture is not counted as a passing system-health test.

Earlier RC7 validation covered actual GitHub downloading, hash/signature checks, saved-file recovery after process restart, Android installation, and settings preservation. A separate RC7 run recorded a roughly 6.8-second screen-stop delay.

Vehicle mute buttons, Bluetooth/reverse-camera transitions, ignition power loss, manufacturer-specific root/input behavior, prolonged standalone launching, and every vehicle/app combination have not all been verified. Previously reported vehicle freezes and lost connections are not confirmed resolved.
