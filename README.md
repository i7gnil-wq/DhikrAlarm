# منبه الذكر — تطبيق Android (Kotlin)

منبه لا يتوقف رنينه حتى يُنطق الذكر (أو يُكتب بدقة كبديل احتياطي).

## هيكل المشروع

```
DhikrAlarm/
├── settings.gradle.kts / build.gradle.kts / gradle.properties
├── .github/workflows/build-apk.yml      ← بناء APK تلقائياً على GitHub
└── app/
    ├── build.gradle.kts
    └── src/main/
        ├── AndroidManifest.xml          ← الأذونات والشاشات والخدمة والمستقبِلات
        ├── java/com/dhikralarm/app/
        │   ├── DhikrApp.kt              ← تثبيت العربية/RTL
        │   ├── MainActivity.kt          ← قائمة المنبهات + الأذونات + تجربة المنبه
        │   ├── AlarmAdapter.kt          ← عرض بطاقات المنبهات
        │   ├── AlarmItem.kt             ← نموذج المنبه وحساب الموعد القادم
        │   ├── AlarmStore.kt            ← التخزين (SharedPreferences/JSON)
        │   ├── AlarmScheduler.kt        ← الجدولة عبر AlarmManager.setAlarmClock
        │   ├── AlarmReceiver.kt         ← يستقبل الموعد ويشغّل الخدمة
        │   ├── BootReceiver.kt          ← إعادة الجدولة بعد إعادة التشغيل
        │   ├── AlarmService.kt          ← الرنين الدائم (نغمة + اهتزاز + فرض أقصى صوت)
        │   ├── RingActivity.kt          ← شاشة الذكر: تعرف صوتي + كتابة احتياطية
        │   └── DhikrMatcher.kt          ← نص الذكر ومحرك المطابقة (عدّل الإعدادات هنا)
        └── res/
            ├── raw/alarm_tone.mp3       ← نغمتك (حُوّلت من m4a إلى mp3)
            ├── layout/  values/  drawable/  mipmap-anydpi-v26/
```

## الحصول على ملف APK

### الطريقة 1: GitHub Actions (بدون تثبيت شيء)
1. أنشئ مستودعاً على GitHub وارفع محتوى هذا المجلد.
2. افتح تبويب **Actions** ← **Build APK** ← **Run workflow**.
3. بعد انتهاء التشغيل نزّل `app-debug.apk` من قسم **Artifacts**.

### الطريقة 2: Android Studio
1. افتح المجلد بـ Android Studio (إصدار Koala أو أحدث) ودع Gradle يزامن.
2. **Build ← Build Bundle(s) / APK(s) ← Build APK(s)**.
3. الملف في `app/build/outputs/apk/debug/app-debug.apk`.

### لنسخة الإصدار (release)
Build ← Generate Signed Bundle / APK ← APK، وأنشئ ملف توقيع (keystore) واحتفظ به.

## ضبط الهاتف بعد التثبيت
1. افتح التطبيق واضغط **منح الأذونات** حتى يختفي الشريط (الإشعارات، الميكروفون، العرض فوق القفل، استثناء البطارية).
2. اضغط **تجربة المنبه الآن** وجرّب النطق.
3. في بعض الهواتف (Xiaomi, Huawei, Oppo, Samsung) فعّل «التشغيل التلقائي» وأزل قيود البطارية للتطبيق من إعدادات النظام.

## تخصيص
- نص الذكر ونسب القبول: `DhikrMatcher.kt` (كائن `DhikrConfig`).
- مستوى خفض الصوت أثناء الاستماع: `DUCK_VOLUME` في `AlarmService.kt`.
- الألوان: `res/values/colors.xml`.
