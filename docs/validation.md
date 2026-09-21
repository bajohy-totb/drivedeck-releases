# 검증 범위 / Validation scope

[한국어 소개](../README.md) · [English overview](../README.en.md)

## 1.0.0 출시 검증 / Release validation

1.0.0은 GitHub 정식 배포이며, 아래 RC8 기록은 과거 시험 결과로 보존합니다. 배포용 APK는 디버깅이 꺼져 있고 테스트 전용 연결 진입점을 거부합니다. 기존 RC와 같은 서명으로 업데이트 호환성을 유지합니다. 단위 검사 33개 통과, Release Lint 오류 0개·경고 37개입니다.

첫 release 후보에서 단독 실행 앱이 전면에 떠도 입력이 꺼진 가상 디스플레이에 남는 실패를 두 번 재현했습니다. 수정본은 단독 실행 전에 입력 복원을 취소하고 분할 화면을 숨긴 뒤 외부 앱을 열도록 순서를 정리했습니다. 큐에 남은 focus 요청도 실행 시 현재 화면 상태를 다시 확인합니다. 실패 이력은 보존합니다.

최종 배포 APK의 **핵심 검사 7개가 기능·시스템 상태 모두 통과**했습니다. 배포 설정 1개, 음소거 입력 2개, 자동 재연결 1개, 미선택/선택 앱의 단독 실행·실제 터치·구역 복귀 1개, 실제 Settings/Clock 좌우 실행·터치 1개, 한국어↔영어 전환·설정 보존 1개입니다. 단독 실행 검사는 앞선 수정 확인 실행에서도 통과했습니다. 최종 검사 중 새 Android ANR이나 watchdog 지연은 관찰되지 않았습니다.

검사는 실제 비디버그 release APK를 설치한 Android 13 전용 에뮬레이터에서 수행합니다. 에뮬레이터의 AOSP root 실행 어댑터는 계측 앱에만 들어 있으며, 실제 차량의 Magisk 승인 절차를 대신 검증한 것은 아닙니다. 실차 음소거·연결 끊김의 해결, 장시간 주행, 모든 앱·제조사 조합은 확인 완료가 아닙니다.

Version 1.0.0 is the official GitHub release; the RC8 results below are retained as historical evidence. The release APK disables debugging and rejects its test-only connection entry point. It retains the existing certificate for RC upgrade compatibility. All 33 unit tests passed; Release Lint reported 0 errors and 37 warnings.

The first release candidate reproduced a standalone-launch failure twice: the external app was in front, but input remained on an off virtual display. The revised candidate cancels input restoration and hides the panes before opening the external app. Queued focus requests also recheck the current screen state. Failure evidence is retained.

All **seven core checks on the final release APK passed both functional and system-health checks**: release configuration (1), mute input (2), automatic reconnection (1), standalone launch/touch/pane return for assigned and unassigned apps (1), actual side-by-side Settings/Clock interaction (1), and Korean/English switching with settings preservation (1). Standalone launching also passed the preceding fix-verification run. No new Android ANR or watchdog delay was observed during the final suite.

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
