# अपना ऐप टेस्ट करें

[English](../../../docs/configuration.md) · [简体中文](../../zh-CN/docs/configuration.md) · [日本語](../../ja/docs/configuration.md) · [Español](../../es/docs/configuration.md) · [Português](../../pt-BR/docs/configuration.md) · [Tiếng Việt](../../vi/docs/configuration.md) · [Türkçe](../../tr/docs/configuration.md) · [Deutsch](../../de/docs/configuration.md) · हिन्दी · [한국어](../../ko/docs/configuration.md)

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

| इनपुट | |
|---|---|
| `source_repo`, `ref` | ऐप का रिपो और branch (खाली = यही रिपो)। निजी रिपो: केवल-पढ़ने वाला `SOURCE_REPO_TOKEN` secret जोड़ें |
| `app_dir`, `app_id` | ऐप का फ़ोल्डर, Android package / iOS bundle id (दूसरा platform दूसरा id रखता है तो वह उसके बने ऐप से पढ़ा जाता है) |
| `android_personas`, `ios_personas` | [personas.json](../../../personas.json) से ids, हर फ़ोन पर एक, या `all`, या कोई platform छोड़ना हो तो `none`। 20 तैयार हैं: 14 भाषाएँ, दाएँ से बाएँ, टैबलेट, छोटे से सबसे बड़े accessibility साइज़ तक टेक्स्ट, बढ़ा contrast, घटा motion, रंग-अंधता का filter और बिगड़ता नेटवर्क |
| `brief` | एजेंट के निर्देश: घूमने के लिए [AGENT_BRIEF.md](../../../AGENT_BRIEF.md), सुधार रिपोर्ट के लिए [briefs/improve.md](../../../briefs/improve.md), [डैशबोर्ड](../../../docs/dashboard.md) के नक्शे के लिए हर स्क्रीन ढूँढने वाला [briefs/map.md](../../../briefs/map.md), या किसी एक फ़ीचर के लिए अपना |
| `bug_report` | घूमने की जगह दोहराने को एक बग: एक issue (`owner/repo#123` या उसका URL) या चरणों का टेक्स्ट। हर एजेंट उसे अपने ही डिवाइस पर चलाता है ([briefs/reproduce.md](../../../briefs/reproduce.md)) |
| `web_personas`, `web_url` | web personas की ids (`tom`, `amelie`, `lena`, `arjun`), `all`, या `none` (डिफ़ॉल्ट), और जिस साइट को टेस्ट करना है उसका पता। बाकी platforms अपने डिफ़ॉल्ट पर रहते हैं, इसलिए सिर्फ़ web वाले रन के लिए `android_personas=none` और `ios_personas=none` रखें। |
| `model`, `llm_base_url` | डिफ़ॉल्ट `sonnet`, कम खर्च के लिए `haiku`, या कोई भी Anthropic-compatible API जिसकी key `LLM_API_KEY` secret में हो ([docs/cheaper-models.md](../../../docs/cheaper-models.md)) |
| `framework` | `auto` (डिफ़ॉल्ट) या नीचे के frameworks में से एक ज़बरदस्ती |
| `android_build` + `apk_path`, `ios_build` + `ios_app_path` | आपका अपना build कमांड, या पहले से बने ऐप का download (रास्ते `app_dir` से गिने जाते हैं) |
| `build_args` | दोनों platforms की स्वचालित build के लिए अतिरिक्त arguments, जैसे Flutter के लिए `--dart-define=API_URL=https://staging.example.com` (बाकी के लिए Gradle और xcodebuild arguments)। Keys एक `BUILD_ARGS` secret में रखें: सार्वजनिक रिपो पर run inputs सबको दिखते हैं। ऐप की अपनी build कोड भी वही secret पढ़ सकती है, इसलिए थोड़े समय चलने वाली या staging key ही दें, कभी लंबी नहीं |
| `block_hosts` | टेस्ट के दौरान बंद किए जाने वाले analytics hosts |
| `record_video` | हर फ़ोन की स्क्रीन रिकॉर्डिंग, रिपोर्टों के साथ encrypted |
| `replay_path` | प्रयोगात्मक, Android: हर एजेंट के बाद उसका रास्ता उसी फ़ोन पर बिना मॉडल दोहराया जाता है ([docs/replay.md](../../../docs/replay.md)) |
| `replay_traces`, `replay_times` | प्रयोगात्मक, Android: कोई एजेंट नहीं। रिपो की एक फ़ोल्डर में रखे traces हर एक को `replay_times` बार (डिफ़ॉल्ट 20) हर persona के फ़ोन पर, बिना मॉडल दोहराया जाता है ([docs/replay.md](../../../docs/replay.md#replay-kept-traces-many-times)) |
| `map` | प्रयोगात्मक, Android: पहले हर स्क्रीन का नक्शा। `robot`: बिना मॉडल का रोबोट, मुफ़्त। `robot+agent`: रोबोट, फिर एजेंट सिर्फ़ उन फ़ॉर्मों के लिए जो वह पार नहीं कर पाया। एक persona दें ([docs/dashboard.md](../../../docs/dashboard.md)) |

**बिना काम के चलता है** (अपने आप पहचाना जाता है):

| Framework | Android | iOS Simulator |
|---|---|---|
| Flutter | `flutter build apk --debug` | `flutter build ios --simulator` |
| React Native / Expo | Gradle release | Xcode, arm64 |
| नेटिव Kotlin / Java, Swift / Objective-C | Gradle debug | Xcode, arm64 |
| Kotlin Multiplatform | Gradle debug | iOS ऐप का Xcode project |
| Capacitor / Ionic | `cap sync` के बाद Gradle debug | `cap sync` के बाद Xcode |
| NativeScript (beta) | `ns build android` (debug) | `ns build ios` (simulator) |
| .NET MAUI (beta) | `dotnet publish` | `iossimulator-arm64` के लिए `dotnet build` |
| कुछ और | `android_build` + `apk_path` | `ios_build` + `ios_app_path` |

एजेंट स्क्रीनशॉट और accessibility tree से स्क्रीन पढ़ते हैं, इसलिए कोई भी UI toolkit चलता है। नाम वाले widgets (Flutter `Semantics`, `contentDescription`, `accessibilityLabel`) उन्हें ज़्यादा सटीक टैप निशाने देते हैं।
