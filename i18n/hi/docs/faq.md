# अक्सर पूछे जाने वाले सवाल

[English](../../../docs/faq.md) · [简体中文](../../zh-CN/docs/faq.md) · [日本語](../../ja/docs/faq.md) · [Español](../../es/docs/faq.md) · [Português](../../pt-BR/docs/faq.md) · [Tiếng Việt](../../vi/docs/faq.md) · [Türkçe](../../tr/docs/faq.md) · [Deutsch](../../de/docs/faq.md) · हिन्दी · [한국어](../../ko/docs/faq.md)

## क्या 20 एजेंट एक ही बग 20 बार पाते हैं?

नहीं। LocalSend पर 103 रिपोर्टें 46 अलग-अलग बग थीं, और उनमें 29 कोई एक ही एजेंट ने पाए, blocker समेत ([चार्ट](examples.md#localsend-20-agents-20-devices-14-languages))। दाएँ से बाएँ वाले बग सिर्फ़ दो अरबी personas से आए और टैबलेट वाले चार टैबलेटों से। दोहराए गए सब वे हर-ऐप वाले हाल थे जिनसे सब गुज़रते हैं, जैसे खाली डिवाइस सूची।

## कितना खर्च आता है?

सार्वजनिक रिपो के लिए GitHub के runners मुफ़्त हैं, macOS समेत। [इन रनों](../../../docs/runs/) में हर एजेंट का मॉडल खर्चा $0.15 से $0.55 रहा, यानी 20 फ़ोनों का लगभग $5। Claude सब्सक्रिप्शन पर रन आपके प्लान के इस्तेमाल में गिने जाते हैं, बिल में नहीं। कम खर्च के लिए `model=haiku` या किसी और सेवा देने वाले का मॉडल चलाएँ: [docs/cheaper-models.md](../../../docs/cheaper-models.md)।

## एक रन में कितना समय लगता है?

हर एजेंट 1 से 18 मिनट घूमता है। AnkiDroid के 10 Android एजेंट शुरू से आख़िरी रिपोर्ट तक 10 मिनट से कम में निपटे। LocalSend के 20 डिवाइसों को 69 मिनट लगे, बिल्ड समेत, क्योंकि Free plan एक वक़्त में 5 macOS jobs चलाता है।

## मुझे Mac चाहिए?

नहीं। सब GitHub के runners पर चलता है: Android के लिए Linux, iOS के लिए macOS। आपको `gh` CLI और Claude सब्सक्रिप्शन या API key चाहिए, और `./setup.sh` (Windows पर `.\setup.ps1`) बाकी कर देता है।

## क्या दो रन एक ही नतीजा देंगे?

नहीं। हर एजेंट खुद घूमता है, इसलिए दो रन कभी एक ही रास्ता नहीं लेते: बगों की वही क़िस्में मिलेंगी, वही सूची नहीं। दोबारा होने वाले सुधारों के लिए लिखी टेस्ट (Maestro, Appium) अपने पास रखें, [तुलना देखें](../../../docs/compare.md)।

## क्या मेरी रिपोर्टें सबको दिखती हैं?

नहीं। सार्वजनिक रिपो के logs सबको दिखते हैं, इसलिए रिपोर्टें, स्क्रीनशॉट और वीडियो runner से निकलने से पहले आपकी key से encrypt किए जाते हैं और decrypt सिर्फ़ आप कर सकते हैं। फ़ोनों के भीतर analytics hosts बंद रहते हैं। टेस्ट खाते ही चलाएँ, असली कभी नहीं। और [PRIVACY.md](../../../PRIVACY.md) में।

## क्या बंद स्रोत का ऐप टेस्ट कर सकता हूँ?

हाँ। workflow निजी रिपो से चलाएँ, अपने ऐप के रिपो के लिए केवल-पढ़ने वाला `SOURCE_REPO_TOKEN` दें, या `android_build` और `ios_build` को पहले से बने APK या .app पर मोड़ें। निजी रिपो आपके Actions मिनट खर्च करती है और macOS के मिनट दस गुना गिने जाते हैं।

## क्या अपने personas बना सकता हूँ?

हाँ। persona [personas.json](../../../personas.json) की कुछ पंक्तियाँ है: डिवाइस, OS संस्करण, भाषा, टेक्स्ट साइज़, dark mode, जगह, वह कौन है और क्या करने की कोशिश करता है। `device` में वैकल्पिक सेटिंग्स: `"increase_contrast": true` (iOS पर Increase Contrast, Android पर high contrast text), `"reduce_motion": true` (iOS और web: Android एमुलेटर पहले से animations बंद चलते हैं), `"color_filter"` (`"protanopia"`, `"deuteranopia"` या `"tritanopia"`, Android, iOS और web के स्क्रीनशॉट पर लागू) और `"network"` (`"slow"` या `"flaky"`, सिर्फ़ Android एमुलेटर)।

## वह क्या ग़लत कर देता है?

कभी-कभी स्क्रीन ग़लत पढ़ता है: LocalSend के 20 में से 3 एजेंटों ने Simulator का अंग्रेज़ी keyboard ऐप का हिस्सा समझ लिया। हर एजेंट के कुछ मिनट मुख्य रास्ते ढकते हैं, बड़े ऐप का हर कोना नहीं। हर नतीजे के साथ क्रमांकित स्क्रीनशॉट आते हैं, इसलिए कुछ दर्ज करने से पहले उन्हें देख लें।

## सीमाएँ

- सार्वजनिक रिपो के लिए GitHub runners मुफ़्त: एक वक़्त में लगभग 20 jobs, macOS पर 5।
- 5 घंटे की इस्तेमाल सीमा वाले प्लान पर 20 एजेंट उसे लगभग 15 मिनट में पूरा खा सकते हैं: 6 से 10 के बैच चलाएँ।
- Hosted runners रिपो के अपने project के लिए बने हैं। बंद स्रोत के ऐप के लिए workflow अपनी निजी रिपो में या अपने Mac पर चलाएँ।

## यह काम कैसे करता है

<p align="center"><img src="../../../assets/how-it-works.svg" alt="काम कैसे करता है: एक workflow ऐप बनाता है, फिर हर persona के लिए एक एजेंट वाला एक एमुलेटर या सिम्युलेटर, encrypted रिपोर्टें, आपकी मशीन पर summary" width="100%"></p>

- **Personas** डिवाइस तय करते हैं: मॉडल, OS, फ़ॉन्ट साइज़, dark mode, भाषा, जगह।
- **हर एजेंट के तीन औज़ार**: स्क्रीन देखना (टैप coordinates समेत), gestures, टाइपिंग।
- **हर स्क्रीन पर accessibility जाँच**: बिना नाम के controls और 48 dp (Android) या 44 pt (iOS) से छोटे touch निशाने, एजेंट स्क्रीनशॉट पर पक्का करता है।
- **Encrypted नतीजे**: सार्वजनिक रिपो के logs सबको दिखते हैं, इसलिए सब कुछ आपकी key से encrypt होता है।
- **फ़ोनों के भीतर analytics बंद**, इसलिए टेस्ट आपके असली आँकड़ों को कभी छूते नहीं।
- **आपकी मशीन पर एक summary**। `SUMMARY_GROUPING=1` रखने पर Claude एक ही बग बताने वाली रिपोर्टें समूहबद्ध करता है (आपकी Claude login पर एक call), इसलिए आप देखते हैं किस बग पर कितने personas अटके और किस क़िस्म का सुधार किसने पाया। `SUMMARY_WEBHOOK_URL` रखने पर एक छोटा संस्करण आपके Slack या Discord webhook पर भी जाता है, आपकी ही मशीन से।
- **आपकी मशीन पर draft issues**: `python3 scripts/draft_issues.py runs/<id>` `SUMMARY.md` से हर नतीजे का एक GitHub issue प्रीव्यू करता है, और `--create --repo OWNER/REPO` आपके पक्के किए `gh` से खोलता है।
- **हर push पर टेस्ट**, बिना नेटवर्क और बिना फ़ोन: accessibility जाँच, persona matrix, summary, encrypted नतीजे ([tests/](../../../tests/))।

## Maestro, Appium या agent-device क्यों नहीं?

वे आपकी लिखी टेस्ट चलाते हैं। ये एजेंट वह ढूँढते हैं जिसे किसी ने टेस्ट करने की सोची ही नहीं।

| | mobile-agent-testers | Maestro, Appium, Detox, e2e |
|---|---|---|
| आप लिखते हैं | कुछ नहीं: सूची से personas चुनें | Flows या टेस्ट कोड |
| वह पकड़ता है | टूटे flows, चुपचाप नाकाम होने, गायब अनुवाद, बड़े टेक्स्ट पर layout, dark mode, उलझाने वाला टेक्स्ट | हर टेस्ट जो जाँचता है |
| हर रन में एक ही नतीजा | नहीं: हर रन खुद घूमता है, इसलिए स्क्रीनशॉट देखें | हाँ |

[agent-device](https://github.com/callstackincubator/agent-device) एक coding एजेंट को एक सिम्युलेटर पर हाथ देता है। यह पूरा सेशन चलाता है: ऐप बनाता है, हर persona को अपना डिवाइस देता है और encrypted रिपोर्टें जमा करता है। Release से पहले एजेंट चलाएँ, और दोबारा होने वाले सुधारों के लिए लिखी flows।

पूरी तुलना, Firebase Test Lab Robo, mobile-mcp, phonedriver और इंसानी beta testers समेत: [docs/compare.md](../../../docs/compare.md)।
