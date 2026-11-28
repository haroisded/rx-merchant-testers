<div align="center">

<img src="../../assets/logo.svg" alt="" width="96" height="96">

# mobile-agent-testers

**GitHub के मुफ़्त runners पर 20 अलग-अलग फ़ोन पर 20 AI टेस्टर, समानांतर में।**

[![Tests](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml/badge.svg)](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml)
[![Stars](https://img.shields.io/github/stars/pierridotite/mobile-agent-testers?style=flat)](https://github.com/pierridotite/mobile-agent-testers/stargazers)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](../../LICENSE)
![iOS + Android](https://img.shields.io/badge/platforms-iOS%20%2B%20Android-blue)
![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-orange)

**[उदाहरण](../../docs/examples.md)** · **[डैशबोर्ड डेमो](https://pierridotite.github.io/mobile-agent-testers/)** · **[तुलना](../../docs/compare.md)** · **[अक्सर पूछे जाने वाले सवाल](../../docs/faq.md)** · **[personas](../../personas.json)** · **[योगदान](../../CONTRIBUTING.md)**

<sub>[English](../../README.md) · [简体中文](../zh-CN/README.md) · [日本語](../ja/README.md) · [Español](../es/README.md) · [Português](../pt-BR/README.md) · [Tiếng Việt](../vi/README.md) · [Türkçe](../tr/README.md) · [Deutsch](../de/README.md) · हिन्दी · [한국어](../ko/README.md)</sub>

<a href="https://www.linkedin.com/in/pierre-raffalli/"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/linkedin-follow-dark.svg"><img src="../../assets/linkedin-follow-light.svg" alt="LinkedIn पर Pierre Raffalli को फ़ॉलो करें" width="160" height="54"></picture></a>
<a href="https://discord.gg/WuaCANscr8"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/discord-join-dark.svg"><img src="../../assets/discord-join-light.svg" alt="Discord पर जुड़ें" width="160" height="54"></picture></a><br>
<a href="https://trendshift.io/repositories/371956?utm_source=trendshift-badge&amp;utm_medium=badge&amp;utm_campaign=badge-trendshift-371956" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/trendshift/repositories/371956/daily?language=Python" alt="pierridotite%2Fmobile-agent-testers | Trendshift" width="250" height="55"/></a>
<a href="https://trendshift.io/repositories/371956?utm_source=trendshift-badge&amp;utm_medium=badge&amp;utm_campaign=badge-trendshift-371956" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/trendshift/repositories/371956/daily" alt="pierridotite%2Fmobile-agent-testers | Trendshift" width="250" height="55"/></a>

<sub>यह पृष्ठ अंग्रेज़ी संस्करण का अनुवाद है, कुछ पीछे हो सकता है; [README.md](../../README.md) को मानक मानें।</sub>

</div>

<p align="center"><a href="../../assets/launch-video.mp4"><img src="../../assets/launch-video.gif" alt="लॉन्च वीडियो, 26 सेकंड। पहले 20 AI टेस्टर के एक साथ LocalSend इस्तेमाल करते हुए असली स्क्रीन रिकॉर्डिंग: 14 भाषाओं में 10 Android एमुलेटर और 10 iOS सिम्युलेटर, हर फ़ोन पर उसके रिपोर्ट किए सुधारों की संख्या, कुल 103। फिर डैशबोर्ड का डेमो ऐप का नक्शा: हर स्क्रीन, हर टेस्टर की एक रंगीन लेन, एक बग जो टेस्ट बन गया, और बिना मॉडल के दोबारा चलाया गया एक रास्ता। आवाज़ के साथ वीडियो खुलता है।" width="100%"></a><br>
<sub>फ़ोन की तस्वीरें LocalSend रन की असली स्क्रीन रिकॉर्डिंग हैं; नक्शा <a href="../../demo-app/">डेमो ऐप</a> पर डैशबोर्ड का है। <a href="../../assets/launch-video.mp4">आवाज़ के साथ वीडियो (MP4)</a> · <a href="../../assets/localsend-20.mp4">20 फ़ोन, पूरी रिकॉर्डिंग</a> · <a href="../../docs/runs/localsend-20.md">वही रन, डिवाइस दर डिवाइस</a></sub></p>

कोई टेस्ट स्क्रिप्ट लिखनी नहीं पड़ती। हर टेस्टर GitHub Actions पर अपना iOS
सिम्युलेटर या Android एमुलेटर रखने वाला एक Claude Code एजेंट है। वह एक persona निभाता
है (सबसे बड़े टेक्स्ट साइज़ पर स्वीडिश भाषा वाला iPhone SE, 200% पर Pixel 7, जर्मन
iPad, दाएँ से बाएँ अरबी...), आपके ऐप को एक नए उपयोगकर्ता की तरह चलाता है (या आपके
बताए फ़ीचर से गुज़रता है) और स्क्रीनशॉट के साथ बग रिपोर्ट लिखता है। Flutter, React Native, Expo, नेटिव Kotlin और Swift, Kotlin
Multiplatform, Capacitor और .NET MAUI।

सार्वजनिक रिपो के लिए GitHub के runners मुफ़्त हैं। आप सिर्फ़ मॉडल का किराया देते हैं, अपनी
Claude सब्सक्रिप्शन या API key से: [इन रन](../../docs/runs/) में हर फ़ोन का खर्चा लगभग $0.15 से $0.55 रहा।

## तेज़ शुरुआत

```bash
gh repo create my-testers --public --template pierridotite/mobile-agent-testers --clone
cd my-testers && ./setup.sh           # key pair + secrets + Claude token (Windows: .\setup.ps1)
gh workflow run agent-testers.yml     # डेमो: 2 Android + 2 iOS टेस्टर, लगभग 20 मिनट
bash scripts/collect.sh <run id>      # रिपोर्ट decrypt करता है, runs/<id>/SUMMARY.md लिखता है
```

फिर अपने ऐप पर:

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

या Claude से खुद सेटअप करवाएँ: `/plugin marketplace add pierridotite/mobile-agent-testers`, फिर `/plugin install mobile-agent-testers@mobile-agent-testers`।

हर इनपुट (बिल्ड, briefs, बग रिपोर्ट, सस्ते मॉडल, build arguments, वीडियो) और समर्थित frameworks: [docs/configuration.md](../../docs/configuration.md)।

<details>
<summary>डेमो रन क्या पाता है</summary>

डेमो ऐप में [11 जानबूझकर रखे बग](../../demo-app/BUGS.md) हैं। [एक रन](../../docs/runs/) में 4 डेमो टेस्टरों ने उनमें से 9 पाए (हर एजेंट को 2 से 7 मिनट, पूरे रन को बिल्ड समेत लगभग 21 मिनट), साथ में ऐसे असली सुधार भी जो किसी ने नहीं रखे थे (Android की nav bar के नीचे दबा एक बटन, ऐप दोबारा खोलने पर कुछ भी सुरक्षित न रहना)। मॉडल खर्चा: 4 के लिए $0.87।

</details>

## यह क्या करता है

हर persona आपके ऐप को एक नए उपयोगकर्ता की तरह घुमाता है और रिपोर्ट करता है कि क्या रास्ते में आया, क्रमांकित स्क्रीनशॉट के साथ। एक रन किसी एक फ़ीचर को शुरू से आख़िर तक चला सकता है, बग रिपोर्ट दोहरा सकता है, सुधार रिपोर्ट लिख सकता है, दो वर्ज़न तुलना सकता है या फ़ोन पर वेबसाइट चला सकता है। हर रन में एक ही काम, ऊपर के इनपुट से तय।

<details>
<summary>छह काम, एक एक करके</summary>

| काम | क्या मिलता है |
|---|---|
| **नए उपयोगकर्ता की तरह घूमना** (डिफ़ॉल्ट) | हर persona अपना लक्ष्य पाने की कोशिश करता है और रास्ते की रुकावटें क्रमांकित स्क्रीनशॉट के साथ रिपोर्ट करता है। |
| **एक फ़ीचर शुरू से आख़िर तक** | आपका brief हर एजेंट को पूरे मैकेनिक से गुज़ारता है: टैप, स्वाइप, टाइपिंग, ऐप दोबारा खोलना, deep links। [AnkiDroid का](../../briefs/ankidroid.md) रिपोर्ट: नए कार्ड का Back फ़ील्ड कीबोर्ड के पीछे छिपा था। |
| **बग रिपोर्ट दोहराना** | हर फ़ोन हाँ, ना या अस्पष्ट कहता है, स्क्रीनशॉट के साथ ([brief](../../briefs/reproduce.md))। |
| **सुधार रिपोर्ट** | हर persona कहाँ अटका, क्या उम्मीद थी, सबसे पहले क्या बदलना चाहिए ([brief](../../briefs/improve.md))। |
| **दो वर्ज़न की तुलना** | वही personas दो branches या builds पर, फिर लक्ष्य, स्कोर और सुधारों की एक तालिका ([कैसे](../../docs/compare-versions.md))। |
| **फ़ोन पर वेबसाइट टेस्ट** | वही personas फ़ोन के ब्राउज़र में, किसी भी साइट या वेब ऐप पर। मुफ़्त Linux runners पर चलता है ([configuration](../../docs/configuration.md))। |

</details>

कोई भी Anthropic-compatible मॉडल चलता है, Claude Haiku से OpenRouter, DeepSeek, Kimi, GLM, MiniMax या Qwen तक; नतीजे उतने ही अच्छे हैं जितना मॉडल स्क्रीनशॉट पढ़ पाता है ([docs/cheaper-models.md](../../docs/cheaper-models.md))। Maestro या Appium आपकी लिखी टेस्ट चलाते हैं; ये एजेंट वह ढूँढते हैं जिसे किसी ने टेस्ट करने की सोची ही नहीं ([तुलना](../../docs/compare.md))।

## हर स्क्रीन एक नक्शे पर

<a href="https://pierridotite.github.io/mobile-agent-testers/"><img src="../../assets/dashboard.png" alt="LocalSend का नक्शा: हर स्क्रीन एक कार्ड, लॉन्च से बाएँ से दाएँ पढ़ा जाता है, हर टेस्टर की अपनी रंगीन लाइन, डैश वाली लाइन जहाँ सिर्फ़ रोबोट गया, दाईं ओर एक overview" width="100%"></a>

टेस्टरों ने जो-जो स्क्रीन छुई, सब एक जगह, हर एक का रास्ता अपने रंग में और जहाँ-जहाँ टैप हुआ उसका heat map। Device मेनू से कोई डिवाइस चुनें तो स्क्रीन उसी आकार (फ़ोन या टैबलेट) में और उसी का heat map दिखता है। किसी स्क्रीन पर टिप्पणी करें, या उस पर गलत दिखने वाली चीज़ को एक check बना दें।

<p align="center"><a href="https://pierridotite.github.io/mobile-agent-testers/"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/demo-try-dark.svg"><img src="../../assets/demo-try-light.svg" alt="ब्राउज़र में लाइव डेमो आज़माएँ" width="190" height="54"></picture></a></p>

अपने रन पर: `python3 scripts/dashboard.py runs/<id>`। यह आपकी मशीन पर चलता है और नतीजे वहाँ से बाहर नहीं जाते ([docs/dashboard.md](../../docs/dashboard.md))।

## हर बग एक टेस्ट बन जाता है

बग पाने वाला टेस्टर ऐसा रास्ता ले गया जो कोई स्क्रिप्ट नहीं लिखती। हर Android टेस्टर वह रास्ता trace के रूप में सँभालता है, हर टैप का नाम उस एलिमेंट पर रखा जो उँगली के नीचे था। `-f replay_path=true` देने पर वही रास्ता एजेंट के तुरंत बाद उसी फ़ोन पर बिना मॉडल दोहराया जाता है। नक्शे पर जोड़ा गया check उस बग को टेस्ट का हिस्सा बना देता है: replay जब तक बग है तब तक `bug` कहता है और ठीक होने पर `fixed` ([docs/replay.md](../../docs/replay.md))।

traces Explore-Compile-Replay का पालन करते हैं, मेरा खुला फ़ॉर्मैट जो एजेंट के रास्ते को [Maestro](https://maestro.dev) flow में बदलता है। अभी सिर्फ़ Android; ECR का शुरुआती draft है।

<p align="center"><a href="https://github.com/pierridotite/explore-compile-replay"><picture><source media="(prefers-color-scheme: dark)" srcset="../../assets/ecr-button-dark.svg"><img src="../../assets/ecr-button-light.svg" alt="Explore-Compile-Replay: हर बग एक टेस्ट बनता है" width="320" height="54"></picture></a></p>

## असली रन

खुले स्रोत के ऐप, कुछ भी जानबूझकर नहीं रखा गया। [LocalSend](https://github.com/localsend/localsend) (Flutter, 100k stars) पर 14 भाषाओं के 20 फ़ोनों ने 69 मिनट में 46 अलग-अलग बग पाए, $4.62 के मॉडल खर्चे में। उनमें एक blocker है: सबसे बड़े टेक्स्ट साइज़ पर स्वीडिश भाषा वाले iPhone SE पर कोई संदेश भेज ही नहीं पाता।

<details>
<summary>तीन रन: LocalSend, AnkiDroid, Flutter के animations का नमूना</summary>

| ऐप | फ़ोन | एजेंटों ने क्या पाया | समय, मॉडल खर्चा |
|---|---|---|---|
| [LocalSend](https://github.com/localsend/localsend) (Flutter, 100k stars) | 20: 10 Android, 10 iOS, 14 भाषाएँ | 46 अलग बग, 1 blocker: सबसे बड़े टेक्स्ट साइज़ पर स्वीडिश iPhone SE पर संदेश भेजना असंभव | 69 मिनट, $4.62 |
| [AnkiDroid](https://github.com/ankidroid/Anki-Android) (Kotlin, 12k stars, release APK) | 10 Android | 43 अलग सुधार, जिनमें 200% फ़ॉन्ट पर आधे कटे answer buttons और तीन भाषाओं में "Get started" अंग्रेज़ी में ही छूटा | 10 मिनट से कम, $4.69 |
| [Flutter के animations का नमूना](https://github.com/flutter/packages/tree/main/packages/animations/example) | 1 Android, 1 iOS | वही टाइपो, दोनों एजेंटों ने अपने-अपने सिर से पाया, और dark mode को अनदेखा करने वाला ऐप | 19 मिनट, $0.47 |

</details>

LocalSend के 46 में से 34 बगों का टेक्स्ट साइज़ या accessibility से कोई लेना-देना नहीं था: टूटे flows, चुपचाप नाकाम होने वाले काम, गायब अनुवाद, टैबलेट layouts। स्क्रीनशॉट, हर नतीजा और उन्हें दोबारा चलाने के कमांड: [docs/examples.md](../../docs/examples.md)।

## योगदान

[Issues लेने को तैयार हैं](https://github.com/pierridotite/mobile-agent-testers/issues/9), कुछ [good first issues](https://github.com/pierridotite/mobile-agent-testers/labels/good%20first%20issue) भी: नए personas, दूसरा agent CLI, GitLab CI, अनुवाद। जो लें उस पर टिप्पणी कर दें ताकि दो लोग एक ही चीज़ न बनाएँ। [CONTRIBUTING.md](../../CONTRIBUTING.md) बताता है कि कहाँ से शुरू करें और बिना नेटवर्क कैसे जाँचें। Pull request merge हुआ, या अपने ऐप पर चलाया? [अपना काम दिखाएँ](../../docs/share.md)। सवाल: [Discord](https://discord.gg/WuaCANscr8)।

## टीमों के लिए hosted संस्करण (सोच के दौर में)

यहाँ का workflow मुफ़्त, खुला और पूरा रहेगा। मैं टीमों के लिए एक hosted संस्करण पर विचार कर रहा हूँ, निजी ऐपों के साथ: macOS runners शामिल (निजी रिपो पर GitHub हर macOS मिनट को दस गिनता है) और एक web डैशबोर्ड जिससे रिपोर्ट पूरी टीम, प्रोडक्ट मैनेजर समेत, देख सके।

क्या यह आपकी टीम के काम आएगा? [तीन सवालों के जवाब दें](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=teams.yml) या [LinkedIn पर लिखें](https://www.linkedin.com/in/pierre-raffalli/)। अपने ऐप पर चला चुके हैं? [बताएँ क्या मिला और क्या रुकावट बना](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=usage-feedback.yml)।

## Star history

[![Star History Chart](https://api.star-history.com/svg?repos=pierridotite/mobile-agent-testers&type=Date)](https://star-history.com/#pierridotite/mobile-agent-testers&Date)

## आभार

[Claude Code](https://docs.claude.com/en/docs/claude-code) ·
[AXe](https://github.com/cameroncooke/axe) ·
[android-emulator-runner](https://github.com/ReactiveCircus/android-emulator-runner)

## योगदानकर्ता

जिन सबने योगदान दिया, उन सबका धन्यवाद। मदद करनी है? [यहाँ से शुरू करें](../../CONTRIBUTING.md)।

<a href="https://github.com/pierridotite/mobile-agent-testers/graphs/contributors"><img src="https://contrib.rocks/image?repo=pierridotite/mobile-agent-testers" alt="Contributors" /></a>

Pierre Raffalli ने बनाया · [LinkedIn](https://www.linkedin.com/in/pierre-raffalli/) · [GitHub](https://github.com/pierridotite) · [Discord](https://discord.gg/WuaCANscr8) · MIT license
