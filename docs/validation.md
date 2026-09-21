# 검증 범위 / Validation scope

[한국어 소개](../README.md) · [English overview](../README.en.md)

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
