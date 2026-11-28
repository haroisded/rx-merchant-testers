# Praxisbeispiele

[English](../../../docs/examples.md) · [简体中文](../../zh-CN/docs/examples.md) · [日本語](../../ja/docs/examples.md) · [Español](../../es/docs/examples.md) · [Português](../../pt-BR/docs/examples.md) · [Tiếng Việt](../../vi/docs/examples.md) · [Türkçe](../../tr/docs/examples.md) · Deutsch · [हिन्दी](../../hi/docs/examples.md) · [한국어](../../ko/docs/examples.md)

Echte Läufe auf Open-Source-Apps, ohne eingepflanzte Fehler. Jede Zahl stammt aus [den Läufen](../../../docs/runs/), in denen jeder Bericht Gerät für Gerät aufgeführt ist.

## Nicht nur Barrierefreiheit

Großer Text liefert die eindrucksvollsten Screenshots, aber er ist nur ein kleiner Teil dessen, was die Agenten melden. Jede Persona hat ein Ziel („eine Aufgabe ohne Englisch abschließen“, „jedes Textfeld mit türkischen Zeichen, Emoji und sehr langem Text füllen“, „die App mitten drin verlassen und dann zurückkommen“) und versucht, es wie ein neuer Nutzer zu erreichen. Also melden die Agenten alles, was im Weg steht:

- **Unterbrochene Abläufe und stille Fehlschläge**: eine Geräteliste, die endlos lädt und nie sagt, dass nichts gefunden wurde (12 von 20 Smartphones), ein fehlgeschlagener Versand, der nur „Error“ sagt, ein Dialog, der endlos wartet, wenn eine Adresse nicht antwortet, ein Textdialog, der nach einem falschen Tippen wegwirft, was du eingegeben hast.
- **Onboarding und Berechtigungen**: AnkiDroid blockiert das Setup, bis du auf einer Systemseite Vollzugriff auf Dateien erteilst, ohne Erklärung (8 seiner 10 Agenten).
- **Übersetzungen**: „Get started“ auf Portugiesisch-, Spanisch- und Japanisch-Smartphones auf Englisch, der #-Tab auf Portugiesisch mit „Jogo da velha“ (Tic-Tac-Toe) beschriftet, Größen in Arabisch als „B 14“ geschrieben, ISO-Daten statt des Formats des Smartphones, eine Datenschutzerklärung nur auf Englisch.
- **Texte**: ein Tippfehler in Flutters offiziellem Beispiel, eine Textnachricht als „Files: 1“ gezählt, „#16 #15“ auf dem Startbildschirm ohne Erklärung.
- **Geräte**: Tablets, die das Telefon-Layout in einer schmalen Spalte zeigen, die Tastatur über Abbrechen und Bestätigen, die Android-Navigationsleiste über den letzten Einträgen einer Liste, eine App, die den Dunkelmodus ignoriert.
- **Daten**: nach einem Neustart bleibt nichts erhalten (in der Demo-App).

Bei LocalSend hatten 34 der 46 Bugs nichts mit Schriftgröße oder Barrierefreiheit zu tun. Jeder Bericht endet auch mit der Meinung der Persona und einer Bewertung von 10 (würden sie die App behalten?), ein Lauf zeigt also, wo neue Nutzer sich verirren, nicht nur, was kaputt ist.

## LocalSend: 20 Agenten, 20 Geräte, 14 Sprachen

<p align="center"><img src="../../../assets/real-findings-localsend-20.png" alt="Vier LocalSend-Bildschirme mit Flutters Überlauf-Streifen, jeder rot umrahmt: ein Pixel 7 bei 200 % Text, wo alle sechs Senden-Buttons überlaufen, ein iPhone SE auf Schwedisch mit größter Schriftgröße, wo der Nachrichten-Dialog um 149 Pixel überläuft und das Textfeld sowie Bestätigen verdeckt, ein iPad Air auf Deutsch, wo der Hinzufügen-Button um 114 Pixel über seine Karte hinausläuft, und ein iPhone SE auf Französisch mit Standard-Schriftgröße, wo eine Zeile um 28 Pixel überläuft" width="100%"></p>

[LocalSend](https://github.com/localsend/localsend) ist eine Flutter-App mit 100k Sternen. Was die Agenten fanden:

- iPhone SE auf Schwedisch mit iOS’ größter Schriftgröße: der Nachrichten-Dialog läuft um 149 px über, sodass das Textfeld und Bestätigen außerhalb des Bildschirms liegen und du überhaupt keinen Text senden kannst.
- Pixel 7 mit Androids 200 % Schrift: alle sechs Senden-Buttons laufen über und schneiden ihre Beschriftungen halbiert ab. Dasselbe auf einem iPhone 16 Plus auf Italienisch mit der ersten Barrierefreiheitsgröße.
- iPad Air auf Deutsch mit großem Text: „Hinzufügen“ läuft um 114 px über seine Karte hinaus, und Einstellungswerte brechen mitten im Wort um.
- iPhone SE auf Französisch mit Standard-Schriftgröße: „Recevoir via un lien“ läuft um 28 px über und verdeckt eine Checkbox.
- Pixel Tablet auf Spanisch: die Tastatur verdeckt Abbrechen und Bestätigen des Adress-Dialogs.
- Ein fehlgeschlagener manueller Versand sagt nur „Error“, „Erreur“, „Fehler“, „エラー“, „Hata“ oder „Erro“, ohne Grund. Auf iOS lässt eine nicht antwortende Adresse den Dialog ohne Fortschrittsanzeige warten (drei Agenten).

103 Berichte insgesamt (1 Blocker, 15 schwerwiegende). Von Hand gruppiert sind es 46 verschiedene Bugs, und die meisten wurden nur von einem Smartphone gesehen:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../../assets/bugs-per-phone-dark.svg">
  <img src="../../../assets/bugs-per-phone-light.svg" alt="29 von LocalSends 46 Bugs wurden nur von einem Smartphone gesehen, der einzige Blocker eingeschlossen. 6 Bugs wurden von 2 Smartphones gesehen, 6 von 3, und je einer von 4, 5, 6, 10 und 12 Smartphones: die überlaufenden Senden-Buttons bei großem Text, Icon-Buttons ohne Accessibility-Label, ein fehlgeschlagener Versand, der nur Error sagt, abgeschnittene Einstellungswerte mitten im Wort und eine Geräteliste, die endlos lädt. Kein Bug wurde von mehr als 12 der 20 gesehen." width="100%">
</picture>

69 Minuten vom Start bis zum letzten Bericht (der Free-Plan führt 5 macOS-Jobs gleichzeitig aus), 4,62 $ Modellnutzung für die 20. Jeder Bericht, Gerät für Gerät: [docs/runs/localsend-20.md](../../../docs/runs/localsend-20.md). Selbst laufen lassen:

```bash
gh workflow run agent-testers.yml -f source_repo=localsend/localsend -f app_dir=app \
  -f app_id=org.localsend.localsend_app -f android_personas=all -f ios_personas=all -f record_video=true
```

## AnkiDroid: 10 Agenten, direkt aus dem Release-APK

<p align="center"><img src="../../../assets/real-findings-ankidroid.png" alt="Vier AnkiDroid-Bildschirme, jedes Problem rot umrahmt: ein Pixel 7 bei 200 % Text, wo die Buttons Again, Hard, Good und Easy halbiert abgeschnitten sind (vergrößert gezeigt), dieselbe Telefon-Statistikseite, wo die Linien über den rechten Rand hinauslaufen, und der Willkommensbildschirm auf Portugiesisch und Japanisch, wo der Get-started-Button noch auf Englisch steht" width="100%"></p>

[AnkiDroid](https://github.com/ankidroid/Anki-Android) ist eine Kotlin-App mit 12k Sternen. 10 Agenten auf 10 Android-Emulatoren, aus dem APK ihres GitHub-Releases, ohne Build:

- Pixel 7 mit Androids 200 % Schrift: die Buttons Again / Hard / Good / Easy werden halbiert abgeschnitten, und der Statistiktext läuft über den rechten Rand hinaus. Ein zweiter Agent fand die Buttons auch auf ihrem 2.26-alpha noch abgeschnitten, in beiden Zeilen, wenn „Show large answer buttons“ aktiviert ist.
- Der Willkommensbildschirm sagt „Get started“ auf Englisch, obwohl die Geräte auf Portugiesisch, Spanisch und Japanisch eingestellt sind, während der Rest des Bildschirms übersetzt ist (drei Agenten, jeder für sich).

83 Berichte, die sich zu 43 verschiedenen Problemen gruppieren (zwei davon stammen vom veralteten System-WebView der Android-10- und 11-Emulator-Images, nicht von der App). Unter 10 Minuten vom Start bis zum letzten Bericht, 4,69 $ Modellnutzung für die 10. Jeder Bericht: [docs/runs/ankidroid-10.md](../../../docs/runs/ankidroid-10.md). Selbst laufen lassen:

```bash
gh workflow run agent-testers.yml -f app_dir=. -f app_id=com.ichi2.anki -f brief=briefs/ankidroid.md \
  -f android_build="curl -fsSL -o anki.apk https://github.com/ankidroid/Anki-Android/releases/download/v2.25.3/AnkiDroid-2.25.3-full-universal.apk" \
  -f apk_path=anki.apk -f android_personas=all -f ios_personas=none -f block_hosts="ankiweb.net www.google-analytics.com"
```

## Flutters offizielles Animations-Beispiel

<p align="center"><img src="../../../assets/real-finding-flutter.png" alt="Zwei Smartphones auf demselben Bildschirm von Flutters Animations-Beispiel-App, ein Pixel 7 und ein iPhone SE auf Französisch, mit dem umkreisten Titel Streamling your courses: beide Agenten meldeten diesen Tippfehler unabhängig voneinander" width="100%"></p>

In der [Beispiel-App von flutter/packages](https://github.com/flutter/packages/tree/main/packages/animations/example) meldeten ein Agent auf einem Pixel 7 und ein anderer auf einem iPhone SE auf Französisch jeweils denselben Tippfehler, „Streamling your courses“ ([Zeile 110](https://github.com/flutter/packages/blob/main/packages/animations/example/lib/shared_axis_transition.dart#L110)), und der Android-Agent stellte fest, dass die App den Dunkelmodus ignoriert. 19 Minuten, 0,47 $ Modellnutzung.

Auf deiner App gelaufen? [Teile, was es fand](https://github.com/pierridotite/mobile-agent-testers/issues/new?title=Finding:%20), und es landet in [den Läufen](../../../docs/runs/). Wenn du eine dieser Apps betreust und ein Fund falsch ist, eröffne ein Issue; dann wird er korrigiert und erneut gelaufen.
