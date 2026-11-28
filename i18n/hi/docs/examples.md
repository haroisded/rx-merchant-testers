# लागू किए हुए उदाहरण

[English](../../../docs/examples.md) · [简体中文](../../zh-CN/docs/examples.md) · [日本語](../../ja/docs/examples.md) · [Español](../../es/docs/examples.md) · [Português](../../pt-BR/docs/examples.md) · [Tiếng Việt](../../vi/docs/examples.md) · [Türkçe](../../tr/docs/examples.md) · [Deutsch](../../de/docs/examples.md) · हिन्दी · [한국어](../../ko/docs/examples.md)

खुले स्रोत के ऐप पर असली रन, कुछ भी जानबूझकर नहीं रखा गया। हर आँकड़ा [रनों](../../../docs/runs/) से है, जहाँ हर रिपोर्ट डिवाइस दर डिवाइस सूचीबद्ध है।

## सिर्फ़ accessibility नहीं

बड़ा टेक्स्ट सबसे चौंकाने वाले स्क्रीनशॉट देता है, पर वह एजेंटों की रिपोर्ट का छोटा हिस्सा है। हर persona का एक लक्ष्य है ("अंग्रेज़ी छुए बिना एक काम निपटाना", "हर टेक्स्ट फ़ील्ड में तुर्की अक्षर, emoji और बहुत लंबा टेक्स्ट भरना", "ऐप काम के बीच में बंद करके चले जाना, फिर लौट आना") और वह नए उपयोगकर्ता की तरह उसे पाने की कोशिश करता है, इसलिए एजेंट जो भी रास्ते में आया वही रिपोर्ट करते हैं:

- **टूटे flows और चुपचाप नाकाम होने**: डिवाइस सूची हमेशा के लिए लोड होती रहे और कभी न कहे कि कुछ नहीं मिला (20 में से 12 फ़ोन), नाकाम भेजने पर सिर्फ़ "Error", कोई पता जवाब न दे तो डायलॉग हमेशा के लिए इंतज़ार करे, एक बेतुके टैप के बाद टेक्स्ट डायलॉग आपका लिखा हुआ फेंक दे।
- **Onboarding और permissions**: AnkiDroid सेटअप तब तक रोके रखता है जब तक आप सिस्टम पेज पर सब-फ़ाइलें access न दें, बिना कोई समझाए (उसके 10 में से 8 एजेंट)।
- **अनुवाद**: पुर्तगाली, स्पेनिश और जापानी फ़ोनों पर "Get started" अंग्रेज़ी में ही, पुर्तगाली में # टैब पर "Jogo da velha" (tic-tac-toe), अरबी में साइज़ "B 14" लिखे, फ़ोन के फ़ॉर्मैट की जगह ISO तारीखें, privacy policy सिर्फ़ अंग्रेज़ी में।
- **टेक्स्ट**: Flutter के सरकारी नमूने में एक टाइपो, एक संदेश की गिनती "Files: 1", होम स्क्रीन पर बिना समझाए "#16 #15"।
- **डिवाइस**: टैबलेट जो फ़ोन layout को तंग कॉलम में दिखाएँ, Cancel और Confirm के ऊपर कीबोर्ड, सूची के आख़िरी चीज़ों के ऊपर Android की navigation bar, dark mode को अनदेखा करने वाला ऐप।
- **आँकड़े**: ऐप दोबारा खोलने पर कुछ भी सुरक्षित नहीं (डेमो ऐप में)।

LocalSend पर 46 में से 34 बगों का टेक्स्ट साइज़ या accessibility से कोई लेना-देना नहीं था। हर रिपोर्ट persona की राय और 10 में से एक स्कोर से ख़त्म होती है (क्या वह ऐप चलाता रहेगा?), इसलिए एक रन दिखाता है कि नए उपयोगकर्ता कहाँ अटकते हैं, सिर्फ़ यह नहीं कि क्या टूटा है।

## LocalSend: 20 एजेंट, 20 डिवाइस, 14 भाषाएँ

<p align="center"><img src="../../../assets/real-findings-localsend-20.png" alt="चार LocalSend स्क्रीन, Flutter की overflow पट्टियों के साथ, हर एक लाल डिब्बे में: 200% टेक्स्ट पर एक Pixel 7 जहाँ सभी छह Send बटन overflow करते हैं, सबसे बड़े टेक्स्ट साइज़ पर स्वीडिश भाषा वाला iPhone SE जहाँ message डायलॉग 149 pixel बाहर निकलकर text फ़ील्ड और Confirm छिपा देता है, बड़े टेक्स्ट पर जर्मन iPad Air जहाँ Add बटन अपने card से 114 pixel बाहर निकलता है, और डिफ़ॉल्ट साइज़ पर फ़्रेंच iPhone SE जहाँ एक row 28 pixel बाहर निकलती है" width="100%"></p>

[LocalSend](https://github.com/localsend/localsend) 100k-star वाला Flutter ऐप है। एजेंटों ने क्या पाया:

- iOS के सबसे बड़े टेक्स्ट साइज़ पर स्वीडिश भाषा वाला iPhone SE: message डायलॉग 149 px बाहर निकलता है, text फ़ील्ड और Confirm स्क्रीन से बाहर चले जाते हैं और कोई संदेश भेज ही नहीं पाता।
- Android के 200% फ़ॉन्ट पर Pixel 7: सभी छह Send बटन overflow करके अपने लेबल आधे कर देते हैं। वही पहले accessibility साइज़ पर इतालवी भाषा वाले iPhone 16 Plus पर।
- बड़े टेक्स्ट पर जर्मन iPad Air: "Hinzufügen" अपने card से 114 px बाहर निकलता है, Settings की values शब्द के बीच से टूटती हैं।
- डिफ़ॉल्ट साइज़ पर फ़्रेंच iPhone SE: "Recevoir via un lien" 28 px बाहर निकलकर एक checkbox छिपा देता है।
- स्पेनिश Pixel Tablet: keyboard पते के डायलॉग के Cancel और Confirm को ढक लेता है।
- नाकाम हाथ से भेजने पर सिर्फ़ "Error", "Erreur", "Fehler", "エラー", "Hata" या "Erro" दिखता है, कोई कारण नहीं। iOS पर जो पता जवाब न दे, डायलॉग बिना प्रगति दिखाए इंतज़ार करता रहता है (तीन एजेंट)।

कुल 103 रिपोर्ट (1 blocker, 15 major)। हाथ से समूहबद्ध करने पर 46 अलग-अलग बग बनते हैं, और ज़्यादातर कोई एक ही फ़ोन ने देखे:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../../assets/bugs-per-phone-dark.svg">
  <img src="../../../assets/bugs-per-phone-light.svg" alt="LocalSend के 46 में से 29 बग कोई एक ही फ़ोन ने देखे, इकलौता blocker समेत। 6 बग 2 फ़ोनों ने देखे, 6 को 3, और एक एक को 4, 5, 6, 10 और 12 फ़ोनों ने: बड़े टेक्स्ट पर overflow करते Send बटन, बिना accessibility label वाले icon बटन, सिर्फ़ Error कहने वाला नाकाम भेजना, शब्द के बीच कटती settings values, और हमेशा के लिए लोड होती डिवाइस सूची। किसी बग को 20 में से 12 से ज़्यादा फ़ोनों ने नहीं देखा।" width="100%">
</picture>

शुरू से आख़िरी रिपोर्ट तक 69 मिनट (Free plan एक वक़्त में 5 macOS jobs चलाता है), 20 के लिए $4.62 मॉडल खर्चा। हर रिपोर्ट, डिवाइस दर डिवाइस: [docs/runs/localsend-20.md](../../../docs/runs/localsend-20.md)। खुद चलाएँ:

```bash
gh workflow run agent-testers.yml -f source_repo=localsend/localsend -f app_dir=app \
  -f app_id=org.localsend.localsend_app -f android_personas=all -f ios_personas=all -f record_video=true
```

## AnkiDroid: 10 एजेंट, सीधे release APK से

<p align="center"><img src="../../../assets/real-findings-ankidroid.png" alt="चार AnkiDroid स्क्रीन, हर समस्या लाल डिब्बे में: 200% टेक्स्ट पर Pixel 7 जहाँ Again, Hard, Good और Easy बटन आधे कटे (बड़ा करके दिखाए), उसी फ़ोन का Statistics पेज जहाँ लाइनें दाएँ किनारे से बाहर निकलती हैं, और पुर्तगाली व जापानी में स्वागत स्क्रीन जहाँ Get started बटन अंग्रेज़ी में ही है" width="100%"></p>

[AnkiDroid](https://github.com/ankidroid/Anki-Android) 12k-star वाला Kotlin ऐप है। 10 Android एमुलेटर पर 10 एजेंट, उनकी GitHub release के APK से, कोई build नहीं:

- Android के 200% फ़ॉन्ट पर Pixel 7: Again / Hard / Good / Easy बटन आधे कटे, और Statistics का टेक्स्ट दाएँ किनारे से बाहर। दूसरे एजेंट ने वही बटन उसके 2.26 alpha पर भी कटे देखे, "Show large answer buttons" चालू होने पर दोनों rows में।
- पुर्तगाली, स्पेनिश और जापानी सेट डिवाइसों पर स्वागत स्क्रीन "Get started" अंग्रेज़ी में कहती है, जबकि स्क्रीन का बाकी हिस्सा अनुवादित है (तीन एजेंट, हर एक अपने सिर से)।

83 रिपोर्ट, जो 43 अलग सुधारों में समूहबद्ध होती हैं (उनमें दो Android 10 और 11 के एमुलेटर images के पुराने system WebView से आए, ऐप से नहीं)। शुरू से आख़िरी रिपोर्ट तक 10 मिनट से कम, 10 के लिए $4.69 मॉडल खर्चा। हर रिपोर्ट: [docs/runs/ankidroid-10.md](../../../docs/runs/ankidroid-10.md)। खुद चलाएँ:

```bash
gh workflow run agent-testers.yml -f app_dir=. -f app_id=com.ichi2.anki -f brief=briefs/ankidroid.md \
  -f android_build="curl -fsSL -o anki.apk https://github.com/ankidroid/Anki-Android/releases/download/v2.25.3/AnkiDroid-2.25.3-full-universal.apk" \
  -f apk_path=anki.apk -f android_personas=all -f ios_personas=none -f block_hosts="ankiweb.net www.google-analytics.com"
```

## Flutter का सरकारी animations नमूना

<p align="center"><img src="../../../assets/real-finding-flutter.png" alt="Flutter के animations उदाहरण ऐप की एक ही स्क्रीन पर दो फ़ोन, एक Pixel 7 और फ़्रेंच भाषा वाला iPhone SE, शीर्षक Streamling your courses घेरा हुआ: दोनों एजेंटों ने यह टाइपो अपने-अपने सिर से रिपोर्ट किया" width="100%"></p>

[flutter/packages के उदाहरण ऐप](https://github.com/flutter/packages/tree/main/packages/animations/example) में Pixel 7 पर एक एजेंट और फ़्रेंच भाषा वाले iPhone SE पर दूसरे ने एक ही टाइपो रिपोर्ट किया, "Streamling your courses" ([line 110](https://github.com/flutter/packages/blob/main/packages/animations/example/lib/shared_axis_transition.dart#L110)), और Android वाले ने देखा कि ऐप dark mode को अनदेखा करता है। 19 मिनट, $0.47 मॉडल खर्चा।

अपने ऐप पर चलाया? [जो मिला वह साझा करें](https://github.com/pierridotite/mobile-agent-testers/issues/new?title=Finding:%20) और वह [रनों](../../../docs/runs/) में जुड़ता है। अगर आप इनमें से किसी ऐप के मालिक हैं और कोई नतीजा ग़लत लगे, issue खोलें; उसे ठीक करके दोबारा चलाया जाता है।
