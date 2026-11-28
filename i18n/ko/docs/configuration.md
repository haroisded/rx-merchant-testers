# 자신의 앱 테스트하기

[English](../../../docs/configuration.md) · [简体中文](../../zh-CN/docs/configuration.md) · [日本語](../../ja/docs/configuration.md) · [Español](../../es/docs/configuration.md) · [Português](../../pt-BR/docs/configuration.md) · [Tiếng Việt](../../vi/docs/configuration.md) · [Türkçe](../../tr/docs/configuration.md) · [Deutsch](../../de/docs/configuration.md) · [हिन्दी](../../hi/docs/configuration.md) · 한국어

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

| 입력 | |
|---|---|
| `source_repo`, `ref` | 앱 저장소와 브랜치(비워 두면 이 저장소). 비공개 저장소: 읽기 전용 `SOURCE_REPO_TOKEN` 시크릿 추가 |
| `app_dir`, `app_id` | 앱 폴더, Android 패키지 / iOS 번들 ID(다른 플랫폼이 다른 ID를 쓰면 빌드된 앱에서 읽음) |
| `android_personas`, `ios_personas` | [personas.json](../../../personas.json)의 ID, 각각 휴대폰 한 대, `all`, 또는 플랫폼을 건너뛰려면 `none`. 20개 준비됨: 언어 14개, 오른쪽에서 왼쪽으로 쓰는 언어, 태블릿, 가장 작은 글자부터 가장 큰 접근성 크기까지, 높은 대비, 동작 줄이기, 색각 이상 필터, 불안정한 네트워크 |
| `brief` | 에이전트 지침: 탐색에는 [AGENT_BRIEF.md](../../../AGENT_BRIEF.md), 개선 보고서에는 [briefs/improve.md](../../../briefs/improve.md), [대시보드](../../../docs/dashboard.md) 지도에 모든 화면을 담으려면 [briefs/map.md](../../../briefs/map.md), 또는 한 기능에 대한 직접 작성한 지침 |
| `bug_report` | 탐색 대신 재현할 버그: 이슈(`owner/repo#123` 또는 그 URL) 또는 단계를 텍스트로. 각 에이전트가 자신의 기기에서 이를 따릅니다([briefs/reproduce.md](../../../briefs/reproduce.md)) |
| `web_personas`, `web_url` | 웹 페르소나 ID(`tom`, `amelie`, `lena`, `arjun`), `all`, 또는 `none`(기본값), 그리고 테스트할 사이트 주소. 다른 플랫폼은 기본값을 유지하므로 웹만 실행하려면 `android_personas=none`, `ios_personas=none`으로 설정하세요. |
| `model`, `llm_base_url` | 기본값은 `sonnet`, 비용을 줄이려면 `haiku`, 또는 `LLM_API_KEY` 시크릿에 키를 넣은 Anthropic 호환 API([docs/cheaper-models.md](../../../docs/cheaper-models.md)) |
| `framework` | `auto`(기본값) 또는 아래 프레임워크 중 하나를 강제로 사용 |
| `android_build` + `apk_path`, `ios_build` + `ios_app_path` | 직접 작성한 빌드 명령, 또는 미리 빌드된 앱 다운로드(경로는 `app_dir` 기준) |
| `build_args` | 두 플랫폼의 자동 빌드에 추가할 인수. Flutter라면 `--dart-define=API_URL=https://staging.example.com`처럼, 다른 프레임워크는 Gradle 및 xcodebuild 인수입니다. 키는 `BUILD_ARGS` 시크릿에 넣으세요. 공개 저장소에서는 실행 입력이 공개됩니다. 앱 자체의 빌드 코드도 이 시크릿을 읽을 수 있으므로 수명이 짧거나 스테이징용 키를 사용하고, 장기 키는 절대 사용하지 마세요 |
| `block_hosts` | 테스트 중 차단할 분석 호스트 |
| `record_video` | 각 휴대폰의 화면 녹화, 보고서와 함께 암호화됨 |
| `replay_path` | 실험적, Android: 각 에이전트가 끝난 뒤 같은 휴대폰에서 모델 없이 그 경로를 재생([docs/replay.md](../../../docs/replay.md)) |
| `replay_traces`, `replay_times` | 실험적, Android: 에이전트를 실행하지 않음. 저장소의 한 폴더에 보관된 트레이스를 각 페르소나의 휴대폰에서 `replay_times`번(기본 20번) 모델 없이 재생([docs/replay.md](../../../docs/replay.md#replay-kept-traces-many-times)) |
| `map` | 실험적, Android: 먼저 모든 화면을 지도로 만듦. `robot`: 모델 없는 로봇, 무료. `robot+agent`: 로봇을 먼저 실행한 뒤 로봇이 통과하지 못한 폼에만 에이전트를 사용. 페르소나 하나를 사용하세요([docs/dashboard.md](../../../docs/dashboard.md)) |

**별도 설정 없이 지원**(자동 감지):

| 프레임워크 | Android | iOS 시뮬레이터 |
|---|---|---|
| Flutter | `flutter build apk --debug` | `flutter build ios --simulator` |
| React Native / Expo | Gradle release | Xcode, arm64 |
| 네이티브 Kotlin / Java, Swift / Objective-C | Gradle debug | Xcode, arm64 |
| Kotlin Multiplatform | Gradle debug | iOS 앱의 Xcode 프로젝트 |
| Capacitor / Ionic | `cap sync` 후 Gradle debug | `cap sync` 후 Xcode |
| NativeScript(베타) | `ns build android`(debug) | `ns build ios`(시뮬레이터) |
| .NET MAUI(베타) | `dotnet publish` | `iossimulator-arm64`용 `dotnet build` |
| 그 외 | `android_build` + `apk_path` | `ios_build` + `ios_app_path` |

에이전트는 스크린샷과 접근성 트리에서 화면을 읽으므로 어떤 UI 툴킷이든 작동합니다.
레이블이 붙은 위젯(Flutter `Semantics`, `contentDescription`,
`accessibilityLabel`)은 더 정확한 탭 대상을 제공합니다.
