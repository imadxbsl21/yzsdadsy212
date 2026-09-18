# نشر DZ LIFE من الهاتف فقط

هذا المشروع مهيأ ليتم بناؤه في GitHub Actions من الهاتف، بدون الحاجة إلى Android Studio على هاتفك.

## 1) ارفع المشروع إلى GitHub

- أنشئ مستودعًا جديدًا باسم `DZ-LIFE`.
- ارفع كل ملفات هذا المجلد.
- لا ترفع أي ملف `.jks` إلى GitHub. ملف الـ keystore يبقى فقط عندك أو داخل GitHub Secrets بصيغة Base64.

## 2) أنشئ مفتاح توقيع التطبيق

تحتاج إلى Keystore واحد وتحتفظ به للأبد. لا تضِع الملف أو كلمات المرور داخل الكود.

إذا كنت تستخدم Termux، بعد تثبيت Java يمكنك إنشاء المفتاح مثلًا:

```bash
keytool -genkeypair -v -keystore upload-key.jks -alias dzlife -keyalg RSA -keysize 2048 -validity 10000
```

ثم حوّل الملف إلى Base64:

```bash
base64 -w 0 upload-key.jks
```

انسخ الناتج واحفظه بأمان.

## 3) أضف GitHub Secrets

داخل المستودع:
`Settings → Secrets and variables → Actions → New repository secret`

أنشئ:

- `KEYSTORE_BASE64` = نص Base64 للـ keystore
- `KEYSTORE_PASSWORD` = كلمة مرور الـ keystore
- `KEY_ALIAS` = `dzlife`
- `KEY_PASSWORD` = كلمة مرور المفتاح

## 4) ابنِ ملف AAB من الهاتف

افتح:
`Actions → Build signed Google Play AAB → Run workflow`

الـ workflow يثبت Android SDK 36، ينشئ مشروع Capacitor Android، يزامن اللعبة، ثم يبني AAB موقّع.

بعد نجاح العملية:
`Actions → آخر تشغيل ناجح → Artifacts → DZ-LIFE-release-AAB`

نزّل ملف `.aab` إلى الهاتف.

## 5) Google Play

ارفع الـ AAB في Play Console كنسخة اختبار أولًا. لا تغيّر package ID `com.dzlife.algerianlifesimulator` بعد أول نشر.

## مهم

- احتفظ بنسخة احتياطية من `upload-key.jks` وكلمات المرور.
- لا ترفع keystore إلى GitHub.
- لا تضع مفاتيح API سرية داخل JavaScript في التطبيق.
- اسم التطبيق: DZ LIFE
- Package ID: `com.dzlife.algerianlifesimulator`
- Target SDK: 36
