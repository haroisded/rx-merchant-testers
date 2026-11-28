<div align="center">

<img src="../../assets/logo.svg" alt="" width="96" height="96">

# mobile-agent-testers

**20명의 AI 테스터가 GitHub의 무료 러너에서 20대의 서로 다른 휴대폰으로 동시에 테스트합니다.**

[![Tests](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml/badge.svg)](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml)
[![Stars](https://img.shields.io/github/stars/pierridotite/mobile-agent-testers?style=flat)](https://github.com/pierridotite/mobile-agent-testers/stargazers)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](../../LICENSE)
![iOS + Android](https://img.shields.io/badge/platforms-iOS%20%2B%20Android-blue)
![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-orange)

**[예시](../../docs/examples.md)** · **[대시보드 데모](https://pierridotite.github.io/mobile-agent-testers/)** · **[비교](../../docs/compare.md)** · **[FAQ](../../docs/faq.md)** · **[페르소나](../../personas.json)** · **[기여하기](../../CONTRIBUTING.md)**

<sub>[English](../../README.md) · [简体中文](../zh-CN/README.md) · [日本語](../ja/README.md) · [Español](../es/README.md) · [Português](../pt-BR/README.md) · [Tiếng Việt](../vi/README.md) · [Türkçe](../tr/README.md) · [Deutsch](../de/README.md) · [हिन्दी](../hi/README.md) · 한국어</sub>

<a href="https://www.linkedin.com/in/pierre-raffalli/"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/linkedin-follow-dark.svg"><img src="../../assets/linkedin-follow-light.svg" alt="LinkedIn에서 Pierre Raffalli 팔로우" width="160" height="54"></picture></a>
<a href="https://discord.gg/WuaCANscr8"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/discord-join-dark.svg"><img src="../../assets/discord-join-light.svg" alt="Discord에서 함께하기" width="160" height="54"></picture></a><br>
<a href="https://trendshift.io/repositories/371956?utm_source=trendshift-badge&amp;utm_medium=badge&amp;utm_campaign=badge-trendshift-371956" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/trendshift/repositories/371956/daily?language=Python" alt="pierridotite%2Fmobile-agent-testers | Trendshift" width="250" height="55"/></a>
<a href="https://trendshift.io/repositories/371956?utm_source=trendshift-badge&amp;utm_medium=badge&amp;utm_campaign=badge-trendshift-371956" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/trendshift/repositories/371956/daily" alt="pierridotite%2Fmobile-agent-testers | Trendshift" width="250" height="55"/></a>

</div>

<p align="center"><a href="../../assets/launch-video.mp4"><img src="../../assets/launch-video.gif" alt="런칭 영상, 26초. 먼저 20명의 AI 테스터가 LocalSend를 동시에 사용하는 실제 화면 녹화: 14개 언어의 Android 에뮬레이터 10대와 iOS 시뮬레이터 10대, 각 휴대폰에 보고한 이슈 수가 표시되며 총 103개. 그다음 대시보드의 데모 앱 지도: 모든 화면, 테스터별 색상 레인, 테스트로 바뀐 버그, 모델 없이 재생되는 경로. 소리와 함께 영상이 열립니다." width="100%"></a><br>
<sub>휴대폰 화면은 LocalSend 실행의 실제 화면 녹화이고, 지도는 <a href="../../demo-app/">데모 앱</a>의 대시보드입니다. <a href="../../assets/launch-video.mp4">소리 포함 영상(MP4)</a> · <a href="../../assets/localsend-20.mp4">20대 휴대폰 전체 녹화</a> · <a href="../../docs/runs/localsend-20.md">기기별 실행 기록</a></sub></p>

테스트 스크립트를 작성할 필요가 없습니다. 각 테스터는 GitHub Actions에서 자체 iOS
시뮬레이터 또는 Android 에뮬레이터를 가진 Claude Code 에이전트입니다. 페르소나를 연기하며(최대 글자 크기의 스웨덴어 iPhone SE,
200%의 Pixel 7, 독일어 iPad, 오른쪽에서 왼쪽으로 쓰는 아랍어...), 신규 사용자처럼 앱을 사용하고(또는 지정한 기능을 실행하고) 스크린샷이 포함된 버그 보고서를 작성합니다. Flutter, React Native, Expo, 네이티브 Kotlin 및 Swift, Kotlin
Multiplatform, Capacitor, .NET MAUI를 지원합니다.

GitHub의 러너는 공개 저장소에서 무료입니다. 모델 비용만 Claude 구독 또는 API 키로 지불하면 됩니다:
[이 실행들](../../docs/runs/)에서 휴대폰당 약 $0.15~$0.55입니다.

## 빠른 시작

```bash
gh repo create my-testers --public --template pierridotite/mobile-agent-testers --clone
cd my-testers && ./setup.sh           # 키 쌍 + 시크릿 + Claude 토큰 (Windows: .\setup.ps1)
gh workflow run agent-testers.yml     # 데모: Android 2대 + iOS 2대 테스터, 약 20분
bash scripts/collect.sh <run id>      # 보고서 복호화, runs/<id>/SUMMARY.md 작성
```

그다음 자신의 앱에서:

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

또는 Claude가 설정하도록 할 수 있습니다: `/plugin marketplace add pierridotite/mobile-agent-testers`, 그다음 `/plugin install mobile-agent-testers@mobile-agent-testers`.

모든 입력(빌드, 브리프, 버그 보고서, 더 저렴한 모델, 빌드 인수, 영상)과 지원 프레임워크: [docs/configuration.md](../../docs/configuration.md).

<details>
<summary>데모 실행에서 발견되는 것</summary>

데모 앱에는 [의도적으로 심어 둔 버그 11개](../../demo-app/BUGS.md)가 있습니다. [한 번의 실행](../../docs/runs/)에서 데모 테스터 4명이 그중 9개를 찾았고(에이전트당 2~7분, 빌드 포함 전체 실행 약 21분), 아무도 심지 않은 실제 문제도 발견했습니다(Android 내비게이션 바 아래에 가려진 버튼, 앱을 다시 실행하면 데이터가 유지되지 않음). 모델 사용량: 4명 합계 $0.87.

</details>

## 무엇을 하는가

각 페르소나는 신규 사용자처럼 앱을 탐색하고 방해되는 요소를 번호가 매겨진 스크린샷과 함께 보고합니다. 한 번의 실행으로 하나의 기능을 처음부터 끝까지 따라가거나, 버그 보고서를 재현하거나, 개선 보고서를 작성하거나, 두 버전을 비교하거나, 휴대폰에서 웹사이트를 테스트할 수도 있습니다. 실행당 한 가지 용도를 선택하며 위의 입력으로 설정합니다.

<details>
<summary>여섯 가지 용도, 하나씩</summary>

| 용도 | 얻을 수 있는 것 |
|---|---|
| **신규 사용자처럼 탐색** (기본) | 각 페르소나가 목표 달성을 시도하고 방해되는 요소를 번호가 매겨진 스크린샷과 함께 보고합니다. |
| **하나의 기능을 처음부터 끝까지 실행** | 브리프가 모든 에이전트를 하나의 전체 메커니즘으로 안내합니다: 탭, 스와이프, 입력, 재실행, 딥 링크. [AnkiDroid의 브리프](../../briefs/ankidroid.md)는 새 카드의 Back 필드가 키보드 뒤에 숨겨져 있는 것을 발견했습니다. |
| **버그 보고서 재현** | 각 휴대폰이 예, 아니오 또는 불분명으로 답하며 스크린샷을 첨부합니다([브리프](../../briefs/reproduce.md)). |
| **개선 보고서 받기** | 각 페르소나가 망설인 지점, 기대한 것, 먼저 바꿔야 할 것을 알려줍니다([브리프](../../briefs/improve.md)). |
| **두 버전 비교** | 같은 페르소나를 두 브랜치 또는 빌드에서 실행한 뒤 목표, 점수, 이슈를 하나의 표로 정리합니다([방법](../../docs/compare-versions.md)). |
| **휴대폰에서 웹사이트 테스트** | 같은 페르소나가 휴대폰 브라우저에서 모든 사이트나 웹 앱을 사용합니다. 무료 Linux 러너에서 실행됩니다([구성](../../docs/configuration.md)). |

</details>

Claude Haiku부터 OpenRouter, DeepSeek, Kimi, GLM, MiniMax, Qwen까지 Anthropic 호환 모델이라면 무엇이든 작동합니다. 결과물의 품질은 모델이 스크린샷을 읽는 능력에 달려 있습니다([docs/cheaper-models.md](../../docs/cheaper-models.md)). Maestro나 Appium은 작성한 테스트를 실행합니다. 이 에이전트들은 아무도 테스트할 생각을 하지 못한 것을 찾습니다([비교](../../docs/compare.md)).

## 모든 화면을 하나의 지도에서 보기

<a href="https://pierridotite.github.io/mobile-agent-testers/"><img src="../../assets/dashboard.png" alt="LocalSend 지도: 모든 화면이 카드로 표시되고, 실행부터 왼쪽에서 오른쪽으로 읽으며, 테스터마다 색상 선, 로봇만 간 곳은 점선, 오른쪽에 개요" width="100%"></a>

테스터가 도달한 모든 화면이 각자의 색상으로 표시된 경로와 탭한 위치의 히트맵과 함께 있습니다. Device 메뉴에서 기기를 선택하면 해당 기기 모양(휴대폰 또는 태블릿)과 자체 히트맵으로 화면을 볼 수 있습니다. 화면에 댓글을 달거나, 화면에서 잘못된 부분을 체크로 바꿀 수 있습니다.

<p align="center"><a href="https://pierridotite.github.io/mobile-agent-testers/"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/demo-try-dark.svg"><img src="../../assets/demo-try-light.svg" alt="브라우저에서 라이브 데모 사용해 보기" width="190" height="54"></picture></a></p>

자신의 실행 결과: `python3 scripts/dashboard.py runs/<id>`. 내 컴퓨터에서 실행되며 결과는 컴퓨터를 떠나지 않습니다([docs/dashboard.md](../../docs/dashboard.md)).

## 모든 버그가 테스트가 됩니다

버그를 발견한 테스터는 어떤 스크립트도 작성하지 않았을 경로를 지나갔습니다. 각 Android 테스터는 그 경로를 트레이스로 저장하며, 각 탭은 손가락 아래 요소의 이름을 따서 명명됩니다. `-f replay_path=true`를 사용하면 에이전트 직후 같은 휴대폰에서 모델 없이 경로가 재생됩니다. 지도에 추가한 체크는 버그를 테스트의 일부로 만듭니다: 버그가 있는 동안 재생은 `bug`를, 사라지면 `fixed`를 표시합니다([docs/replay.md](../../docs/replay.md)).

트레이스는 에이전트의 경로를 [Maestro](https://maestro.dev) 플로로 컴파일하는 제 오픈 형식인 Explore-Compile-Replay를 따릅니다. 현재 Android만 지원하며 ECR은 초기 초안입니다.

<p align="center"><a href="https://github.com/pierridotite/explore-compile-replay"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/ecr-button-dark.svg"><img src="../../assets/ecr-button-light.svg" alt="Explore-Compile-Replay: 모든 버그가 테스트가 됩니다" width="320" height="54"></picture></a></p>

## 실제 실행

아무것도 심지 않은 오픈 소스 앱에서의 실행입니다. [LocalSend](https://github.com/localsend/localsend)(Flutter, 100k 스타)에서 14개 언어의 휴대폰 20대가 69분 동안 서로 다른 버그 46개를 찾았으며 모델 사용량은 $4.62였습니다. 그중 하나는 차단 수준입니다: 최대 글자 크기의 스웨덴어 iPhone SE에서는 문자 메시지를 전혀 보낼 수 없습니다.

<details>
<summary>세 번의 실행: LocalSend, AnkiDroid, Flutter 애니메이션 샘플</summary>

| 앱 | 휴대폰 | 에이전트가 찾은 것 | 시간, 모델 사용량 |
|---|---|---|---|
| [LocalSend](https://github.com/localsend/localsend) (Flutter, 100k 스타) | 20대: Android 10대, iOS 10대, 14개 언어 | 서로 다른 버그 46개, 차단 1개: 최대 글자 크기의 스웨덴어 iPhone SE에서 문자 메시지를 전혀 보낼 수 없음 | 69분, $4.62 |
| [AnkiDroid](https://github.com/ankidroid/Anki-Android) (Kotlin, 12k 스타, 릴리스 APK) | Android 10대 | 서로 다른 이슈 43개, 그중 200% 글꼴에서 답변 버튼이 반으로 잘리고 "Get started"가 세 언어에서 영어로 남아 있음 | 10분 미만, $4.69 |
| [Flutter 애니메이션 샘플](https://github.com/flutter/packages/tree/main/packages/animations/example) | Android 1대, iOS 1대 | 두 에이전트가 각자 보고한 동일한 오타, 다크 모드를 무시하는 앱 | 19분, $0.47 |

</details>

LocalSend의 버그 46개 중 34개는 글자 크기나 접근성과 무관했습니다: 끊긴 흐름, 조용한 실패, 누락된 번역, 태블릿 레이아웃. 스크린샷, 모든 발견 사항, 다시 실행하는 명령: [docs/examples.md](../../docs/examples.md).

## 기여하기

[가져갈 수 있는 이슈들이 준비되어 있습니다](https://github.com/pierridotite/mobile-agent-testers/issues/9), 일부는 [good first issue](https://github.com/pierridotite/mobile-agent-testers/labels/good%20first%20issue)입니다: 새 페르소나, 다른 에이전트 CLI, GitLab CI, 번역. 두 사람이 같은 것을 만들지 않도록 가져갈 이슈에 댓글을 남겨 주세요. [CONTRIBUTING.md](../../CONTRIBUTING.md)에 각 작업이 어디서 시작되고 오프라인에서 테스트하는 방법이 나와 있습니다. PR이 병합되었거나 자신의 앱에서 실행해 보셨나요? [작업을 공유해 주세요](../../docs/share.md). 질문: [Discord](https://discord.gg/WuaCANscr8).

## 팀을 위한 호스팅 버전(탐색 중)

여기 워크플로는 무료, 오픈 소스, 완전한 상태로 유지됩니다. 비공개 앱을 쓰는 팀을 위한 호스팅 버전을 탐색하고 있습니다: macOS 러너 포함(비공개 저장소에서는 GitHub이 macOS 1분을 10분으로 계산합니다) 및 보고서를 제품 관리자를 포함한 팀 전체와 공유하는 웹 대시보드.

팀에 도움이 될까요? [세 가지 질문에 답하기](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=teams.yml) 또는 [LinkedIn으로 메시지 보내기](https://www.linkedin.com/in/pierre-raffalli/). 이미 자신의 앱에서 실행해 보셨나요? [무엇을 찾았고 무엇이 방해되었는지 알려 주세요](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=usage-feedback.yml).

## 스타 기록

[![Star History Chart](https://api.star-history.com/svg?repos=pierridotite/mobile-agent-testers&type=Date)](https://star-history.com/#pierridotite/mobile-agent-testers&Date)

## 크레딧

[Claude Code](https://docs.claude.com/en/docs/claude-code) ·
[AXe](https://github.com/cameroncooke/axe) ·
[android-emulator-runner](https://github.com/ReactiveCircus/android-emulator-runner)

## 기여자

기여해 주신 모든 분께 감사드립니다. 돕고 싶으신가요? [여기서 시작하세요](../../CONTRIBUTING.md).

<a href="https://github.com/pierridotite/mobile-agent-testers/graphs/contributors"><img src="https://contrib.rocks/image?repo=pierridotite/mobile-agent-testers" alt="기여자" /></a>

Pierre Raffalli 제작 · [LinkedIn](https://www.linkedin.com/in/pierre-raffalli/) · [GitHub](https://github.com/pierridotite) · [Discord](https://discord.gg/WuaCANscr8) · MIT 라이선스
