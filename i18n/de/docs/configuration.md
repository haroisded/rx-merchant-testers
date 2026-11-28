# Teste deine eigene App

[English](../../../docs/configuration.md) · [简体中文](../../zh-CN/docs/configuration.md) · [日本語](../../ja/docs/configuration.md) · [Español](../../es/docs/configuration.md) · [Português](../../pt-BR/docs/configuration.md) · [Tiếng Việt](../../vi/docs/configuration.md) · [Türkçe](../../tr/docs/configuration.md) · Deutsch · [हिन्दी](../../hi/docs/configuration.md) · [한국어](../../ko/docs/configuration.md)

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

| Eingabe | |
|---|---|
| `source_repo`, `ref` | App-Repo und Branch (leer = dieses Repo). Privates Repo: füge ein Nur-Lese-`SOURCE_REPO_TOKEN`-Secret hinzu |
| `app_dir`, `app_id` | App-Ordner, Android-Paket / iOS-Bundle-ID (nutzt die andere Plattform eine andere ID, wird sie aus ihrer gebauten App gelesen) |
| `android_personas`, `ios_personas` | IDs aus [personas.json](../../../personas.json), jeweils ein Smartphone, `all` oder `none` zum Überspringen einer Plattform. 20 fertig: 14 Sprachen, rechts nach links, Tablets, Text von klein bis zur größten Barrierefreiheitsgröße, erhöhter Kontrast, reduzierte Bewegung, ein Farbenblindheitsfilter und ein wackeliges Netzwerk |
| `brief` | Agenten-Anweisungen: [AGENT_BRIEF.md](../../../AGENT_BRIEF.md) zum Erkunden, [briefs/improve.md](../../../briefs/improve.md) für einen Verbesserungsbericht, [briefs/map.md](../../../briefs/map.md), um jeden Bildschirm für die [Dashboard](../../../docs/dashboard.md)-Karte zu finden, oder eigene für ein Feature |
| `bug_report` | Ein Bug zum Reproduzieren statt Erkunden: ein Issue (`owner/repo#123` oder seine URL) oder die Schritte als Text. Jeder Agent folgt ihm auf seinem eigenen Gerät ([briefs/reproduce.md](../../../briefs/reproduce.md)) |
| `web_personas`, `web_url` | IDs der Web-Personas (`tom`, `amelie`, `lena`, `arjun`), `all` oder `none` (Standard), und die Adresse der zu testenden Website. Die anderen Plattformen behalten ihre Standards, also setze `android_personas=none` und `ios_personas=none` für einen reinen Web-Lauf. |
| `model`, `llm_base_url` | Standard `sonnet`, `haiku` zum Sparen, oder jede Anthropic-kompatible API mit ihrem Key in einem `LLM_API_KEY`-Secret ([docs/cheaper-models.md](../../../docs/cheaper-models.md)) |
| `framework` | `auto` (Standard) oder erzwinge eines der Frameworks unten |
| `android_build` + `apk_path`, `ios_build` + `ios_app_path` | Dein eigener Build-Befehl oder der Download einer vorab gebauten App (Pfade relativ zu `app_dir`) |
| `build_args` | Zusätzliche Argumente für den automatischen Build beider Plattformen, wie `--dart-define=API_URL=https://staging.example.com` für Flutter (Gradle- und xcodebuild-Argumente für die anderen). Keys gehören in ein `BUILD_ARGS`-Secret: Run-Eingaben sind in einem öffentlichen Repo öffentlich. Der Build-Code der App kann dieses Secret auch lesen: nimm einen kurzlebigen oder Staging-Key, nie einen dauerhaften |
| `block_hosts` | Während der Tests blockierte Analytics-Hosts |
| `record_video` | Bildschirmaufnahme jedes Smartphones, verschlüsselt mit den Berichten |
| `replay_path` | Experimentell, Android: Nach jedem Agenten wird sein Pfad auf demselben Smartphone ohne Modell abgespielt ([docs/replay.md](../../../docs/replay.md)) |
| `replay_traces`, `replay_times` | Experimentell, Android: kein Agent. Die in einem Ordner des Repos aufbewahrten Traces werden je `replay_times`-mal (Standard 20) auf dem Smartphone jeder Persona abgespielt, ohne Modell ([docs/replay.md](../../../docs/replay.md#replay-kept-traces-many-times)) |
| `map` | Experimentell, Android: zuerst jeden Bildschirm kartieren. `robot`: ein Roboter ohne Modell, kostenlos. `robot+agent`: der Roboter, dann ein Agent nur für die Formulare, die er nicht durchkam. Eine Persona verwenden ([docs/dashboard.md](../../../docs/dashboard.md)) |

**Direkt unterstützt** (automatisch erkannt):

| Framework | Android | iOS-Simulator |
|---|---|---|
| Flutter | `flutter build apk --debug` | `flutter build ios --simulator` |
| React Native / Expo | Gradle release | Xcode, arm64 |
| Native Kotlin / Java, Swift / Objective-C | Gradle debug | Xcode, arm64 |
| Kotlin Multiplatform | Gradle debug | Xcode-Projekt der iOS-App |
| Capacitor / Ionic | Gradle debug nach `cap sync` | Xcode nach `cap sync` |
| NativeScript (beta) | `ns build android` (debug) | `ns build ios` (simulator) |
| .NET MAUI (beta) | `dotnet publish` | `dotnet build` für `iossimulator-arm64` |
| Alles andere | `android_build` + `apk_path` | `ios_build` + `ios_app_path` |

Agenten lesen den Bildschirm aus Screenshots und dem Accessibility-Baum, also funktioniert
jedes UI-Toolkit. Beschriftete Widgets (Flutter `Semantics`, `contentDescription`,
`accessibilityLabel`) geben ihnen präzisere Tipppunkte.
