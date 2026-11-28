# FAQ

[English](../../../docs/faq.md) · [简体中文](../../zh-CN/docs/faq.md) · [日本語](../../ja/docs/faq.md) · [Español](../../es/docs/faq.md) · [Português](../../pt-BR/docs/faq.md) · [Tiếng Việt](../../vi/docs/faq.md) · [Türkçe](../../tr/docs/faq.md) · Deutsch · [हिन्दी](../../hi/docs/faq.md) · [한국어](../../ko/docs/faq.md)

## Finden 20 Agenten denselben Bug einfach 20 Mal?

Nein. Bei LocalSend entsprachen die 103 Berichte 46 verschiedenen Bugs, und 29 davon wurden nur von einem Agenten gefunden, der Blocker eingeschlossen ([Diagramm](examples.md#localsend-20-agenten-20-geräte-14-sprachen)). Bugs mit Rechts-nach-links-Schrift kamen nur von den beiden arabischen Personas, Tablet-Bugs nur von den vier Tablets. Die Duplikate waren appweite Zustände, die alle treffen, wie die leere Geräteliste.

## Was kostet das?

Die Runner von GitHub sind für öffentliche Repos kostenlos, macOS eingeschlossen. Die Modellnutzung lag in [diesen Läufen](../../../docs/runs/) bei 0,15 bis 0,55 $ pro Agent, also etwa 5 $ für 20 Smartphones. Mit einem Claude-Abonnement zählen Läufe gegen das Nutzungskontingent deines Plans statt gegen eine Rechnung. Um weniger auszugeben, nutze `model=haiku` oder das Modell eines anderen Anbieters: [docs/cheaper-models.md](../../../docs/cheaper-models.md).

## Wie lange dauert ein Lauf?

Jeder Agent erkundet 1 bis 18 Minuten. AnkiDroids 10 Android-Agenten brauchten unter 10 Minuten vom Start bis zum letzten Bericht. LocalSends 20 Geräte brauchten 69 Minuten inklusive Builds, weil der Free-Plan 5 macOS-Jobs gleichzeitig ausführt.

## Brauche ich einen Mac?

Nein. Alles läuft auf den Runnern von GitHub: Linux für Android, macOS für iOS. Du brauchst die `gh`-CLI und ein Claude-Abonnement oder einen API-Key, und `./setup.sh` (`.\\setup.ps1` unter Windows) erledigt den Rest.

## Werden zwei Läufe dasselbe Ergebnis liefern?

Nein. Jeder Agent erkundet, zwei Läufe nehmen also nie denselben Weg: Erwarte dieselben Arten von Bugs, nicht dieselbe Liste. Behalte skriptierte Tests (Maestro, Appium) für Regressionen, siehe [Vergleich](../../../docs/compare.md).

## Sind meine Berichte öffentlich?

Nein. Die Logs eines öffentlichen Repos sind öffentlich, also werden Berichte, Screenshots und Videos mit deinem Key verschlüsselt, bevor sie den Runner verlassen, und nur du kannst sie entschlüsseln. Analytics-Hosts sind innerhalb der Smartphones blockiert. Nutze Testkonten, niemals echte. Mehr in [PRIVACY.md](../../../PRIVACY.md).

## Kann ich eine Closed-Source-App testen?

Ja. Führe den Workflow aus einem privaten Repo aus, mit einem Nur-Lese-`SOURCE_REPO_TOKEN` für das Repo deiner App, oder richte `android_build` und `ios_build` auf ein vorab gebautes APK oder .app. Private Repos verbrauchen deine Actions-Minuten, und macOS-Minuten zehnfach.

## Kann ich eigene Personas machen?

Ja. Eine Persona sind ein paar Zeilen in [personas.json](../../../personas.json): Gerät, OS-Version, Sprache, Textgröße, Dunkelmodus, Standort, wer sie sind und was sie zu tun versuchen. Optionale Einstellungen in `device`: `\"increase_contrast\": true` (Increase Contrast auf iOS, High-Contrast-Text auf Android), `\"reduce_motion\": true` (iOS und Web: Android-Emulatoren laufen bereits mit ausgeschalteten Animationen), `\"color_filter\"` (`\"protanopia\"`, `\"deuteranopia\"` oder `\"tritanopia\"`, angewandt auf Screenshots unter Android, iOS und Web) und `\"network\"` (`\"slow\"` oder `\"flaky\"`, nur Android-Emulator).

## Was läuft falsch?

Manchmal wird ein Bildschirm falsch gelesen: 3 von LocalSends 20 Agenten hielten die englische Tastatur des Simulators für einen Teil der App. Ein paar Minuten pro Agent decken die Hauptwege ab, nicht jede Ecke einer großen App. Jeder Fund kommt mit nummerierten Screenshots, prüfe sie also, bevor du etwas meldest.

## Grenzen

- GitHub-Runner sind für öffentliche Repos kostenlos: etwa 20 Jobs gleichzeitig, 5 auf macOS.
- Bei Plänen mit 5-Stunden-Nutzungslimit können 20 Agenten es in ~15 Min. aufbrauchen: fahre in Chargen von 6–10.
- Gehostete Runner sind für das eigene Projekt des Repos gedacht. Für eine Closed-Source-App führe den Workflow in deinem privaten Repo oder auf deinem eigenen Mac aus.

## Wie es funktioniert

<p align="center"><img src="../../../assets/how-it-works.svg" alt="Wie es funktioniert: ein Workflow baut die App, dann ein Emulator oder Simulator mit einem Agenten pro Persona, verschlüsselte Berichte, Zusammenfassung auf deinem Rechner" width="100%"></p>

- **Personas** legen das Gerät fest: Modell, OS, Schriftgröße, Dunkelmodus, Sprache, Standort.
- **Drei Werkzeuge pro Agent**: den Bildschirm sehen (mit Tippkoordinaten), Gesten, Eingeben.
- **Barrierefreiheits-Check** auf jedem Bildschirm: Bedienelemente ohne Namen und Touch-Ziele unter 48 dp (Android) oder 44 pt (iOS), die der Agent am Screenshot bestätigt.
- **Verschlüsselte Ergebnisse**: Logs öffentlicher Repos sind öffentlich, also wird alles mit deinem Key verschlüsselt.
- **Analytics blockiert** innerhalb der Smartphones, damit Tests deine echten Statistiken nie berühren.
- **Eine Zusammenfassung** auf deinem Rechner. Mit `SUMMARY_GROUPING=1` gruppiert Claude die Berichte, die denselben Bug beschreiben (ein Aufruf über deinen Claude-Login), sodass du siehst, wie viele Personas auf jeden gestoßen sind und welche Personas welche Art von Problem fanden. Mit gesetztem `SUMMARY_WEBHOOK_URL` geht eine Kurzfassung zusätzlich an deinen Slack- oder Discord-Webhook, nur von deinem Rechner aus.
- **Entwürfe für Issues** auf deinem Rechner: `python3 scripts/draft_issues.py runs/<id>` zeigt ein GitHub-Issue pro Fund aus `SUMMARY.md` als Vorschau, und `--create --repo OWNER/REPO` öffnet die von dir bestätigten mit `gh`.
- **Tests** bei jedem Push, offline und ohne Smartphone: Barrierefreiheits-Check, Persona-Matrix, Zusammenfassung, verschlüsselte Ergebnisse ([tests/](../../../tests/)).

## Warum nicht Maestro, Appium oder agent-device?

Sie führen die Tests aus, die du schreibst. Diese Agenten suchen, was niemand zu testen gedacht hat.

| | mobile-agent-testers | Maestro, Appium, Detox, e2e |
|---|---|---|
| Du schreibst | Nichts: Personas aus der Liste wählen | Flows oder Testcode |
| Es findet | Unterbrochene Abläufe, stille Fehler, fehlende Übersetzungen, Layout bei großem Text, Dunkelmodus, verwirrende Texte | Was jeder Test prüft |
| Bei jedem Lauf dasselbe Ergebnis | Nein: jeder Lauf erkundet, prüfe also die Screenshots | Ja |

[agent-device](https://github.com/callstackincubator/agent-device) gibt einem Coding-Agenten die Hände auf einen Simulator. Hier läuft eine ganze Sitzung: Es baut die App, gibt jeder Persona ihr eigenes Gerät und sammelt verschlüsselte Berichte. Nutze Agenten vor einem Release und skriptierte Flows für Regressionen.

Vollständiger Vergleich, mit Firebase Test Lab Robo, mobile-mcp, phonedriver und menschlichen Beta-Testern: [docs/compare.md](../../../docs/compare.md).
