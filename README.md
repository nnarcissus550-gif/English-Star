# English-Star
تطبيق عن اللغة الإنجليزية لفض الاحرف من A-Z والقواعد اللغة جمل أضف جميع قواعد اللغة الانجليزية مثل هذا بس ناطق يعني اذا ادوس كلمة Fish تلفض باللغة الإنجليزية مثل هذا بس ناطق يعني اذا ادوس كلمة سمك تلفض باللغة الإنجليزية 
import os, zipfile, textwrap

root="/mnt/data/EnglishStar_Android_Project"
if os.path.exists(root):
    import shutil
    shutil.rmtree(root)

files = {
"settings.gradle": """pluginManagement { repositories { google(); mavenCentral(); gradlePluginPortal() } }
dependencyResolutionManagement { repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS); repositories { google(); mavenCentral() } }
rootProject.name = "EnglishStar"
include(":app")
""",
"build.gradle": """plugins {
    id 'com.android.application' version '8.7.3' apply false
}
""",
"gradle.properties": """org.gradle.jvmargs=-Xmx2048m -Dfile.encoding=UTF-8
android.useAndroidX=true
""",
"app/build.gradle": """plugins { id 'com.android.application' }

android {
    namespace 'com.englishstar.app'
    compileSdk 35

    defaultConfig {
        applicationId 'com.englishstar.app'
        minSdk 23
        targetSdk 35
        versionCode 1
        versionName '1.0'
    }
}

dependencies {
    implementation 'androidx.appcompat:appcompat:1.7.0'
}
""",
"app/src/main/AndroidManifest.xml": """<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:theme="@style/AppTheme"
        android:label="English Star ⭐"
        android:allowBackup="true"
        android:supportsRtl="true">
        <activity android:name=".MainActivity" android:screenOrientation="portrait" android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
""",
"app/src/main/res/values/styles.xml": """<resources>
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="android:fontFamily">sans</item>
        <item name="android:colorAccent">#5B5BD6</item>
        <item name="android:navigationBarColor">#F5F7FF</item>
        <item name="android:statusBarColor">#5B5BD6</item>
        <item name="android:windowLightStatusBar">false</item>
    </style>
</resources>
""",
"app/src/main/java/com/englishstar/app/MainActivity.java": """package com.englishstar.app;

import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        WebView web = new WebView(this);
        WebSettings s = web.getSettings();
        s.setJavaScriptEnabled(true);
        s.setDomStorageEnabled(true);
        s.setBuiltInZoomControls(false);
        web.setVerticalScrollBarEnabled(false);
        web.loadUrl("file:///android_asset/index.html");
        setContentView(web);
    }
    @Override public void onBackPressed() {
        WebView web = (WebView) findViewById(android.R.id.content);
        super.onBackPressed();
    }
}
""",
"app/src/main/assets/index.html": """<!doctype html>
<html lang="ar" dir="rtl"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>English Star ⭐</title>
<style>
body{font-family:Arial,sans-serif;margin:0;background:#f5f7ff;color:#172033}header{background:#5b5bd6;color:#fff;padding:22px;text-align:center}
main{max-width:850px;margin:auto;padding:18px}.card{background:#fff;border-radius:18px;padding:18px;margin:12px 0;box-shadow:0 3px 12px #0001}
.grid{display:grid;grid-template-columns:repeat(6,1fr);gap:8px}button{border:0;border-radius:12px;padding:12px;background:#ececff;font-size:16px}
input{width:100%;box-sizing:border-box;padding:13px;border:1px solid #ddd;border-radius:12px;font-size:16px}
.word{display:flex;justify-content:space-between;align-items:center}.big{font-size:30px;font-weight:bold}
</style></head><body><header><h1>English Star ⭐</h1><p>تعلم الإنجليزية خطوة بخطوة</p></header><main>
<div class="card"><h2>🔤 الحروف A–Z</h2><div id="letters" class="grid"></div></div>
<div class="card"><h2>📚 الكلمات</h2><input id="search" placeholder="ابحث عن كلمة..."><div id="words"></div></div>
<div class="card"><h2>📖 قواعد أساسية</h2><p><b>Pronouns:</b> I, you, he, she, it, we, they</p><p><b>Verb to be:</b> I am / You are / He-She-It is</p><p><b>Present Simple:</b> I play. / She plays.</p><p><b>Questions:</b> Do you...? / Does he...?</p></div>
<div class="card"><h2>📝 اختبار سريع</h2><p>ما معنى <b>fish</b>؟</p><button onclick="quiz('سمك')">سمك</button> <button onclick="quiz('كتاب')">كتاب</button><p id="result"></p></div>
</main><script>
const data=[['fish','سمك'],['book','كتاب'],['house','بيت'],['school','مدرسة'],['future','مستقبل'],['picture','صورة'],['culture','ثقافة'],['nature','طبيعة'],['safe','آمن'],['save','ينقذ/يحفظ'],['life','حياة'],['live','يعيش'],['great','رائع'],['giant','عملاق'],['general','عام'],['gym','صالة رياضية'],['energy','طاقة'],['night','ليل'],['light','ضوء'],['right','صحيح/يمين']];
function speak(t){speechSynthesis.cancel();let u=new SpeechSynthesisUtterance(t);u.lang='en-US';speechSynthesis.speak(u)}
const letters='ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('');letters.forEach(l=>{let b=document.createElement('button');b.textContent=l;b.onclick=()=>speak(l);document.getElementById('letters').appendChild(b)})
function render(q=''){let box=document.getElementById('words');box.innerHTML='';data.filter(x=>(x[0]+' '+x[1]).toLowerCase().includes(q.toLowerCase())).forEach(x=>{let d=document.createElement('div');d.className='word card';d.innerHTML='<span><span class="big">'+x[0]+'</span><br>'+x[1]+'</span><button>🔊</button>';d.querySelector('button').onclick=()=>speak(x[0]);box.appendChild(d)})}
document.getElementById('search').oninput=e=>render(e.target.value);render();function quiz(a){document.getElementById('result').textContent=a==='سمك'?'✅ صحيح!':'❌ جرّبي مرة ثانية.'}
</script></body></html>""",
"README_AR.txt": """English Star ⭐ — مشروع Android جاهز للبناء

المشروع عبارة عن تطبيق Android بسيط يستخدم WebView لعرض نسخة English Star داخل التطبيق.

طريقة البناء:
1) افتح المجلد في Android Studio.
2) انتظر Gradle Sync.
3) من Build اختر Build APK(s).
4) ستجد ملف APK داخل app/build/outputs/apk/.

ملاحظة: هذا مشروع مصدر (Source Project)، وليس APK جاهزاً. يحتاج Android Studio/SDK على الكمبيوتر للبناء.
"""
}

for path, data in files.items():
    p=os.path.join(root,path)
    os.makedirs(os.path.dirname(p), exist_ok=True)
    with open(p,"w",encoding="utf-8") as f: f.write(data)

zip_path="/mnt/data/EnglishStar_Android_Project.zip"
with zipfile.ZipFile(zip_path,"w",zipfile.ZIP_DEFLATED) as z:
    for dp,_,fn in os.walk(root):
        for name in fn:
            p=os.path.join(dp,name)
            z.write(p,os.path.relpath(p,root))

print(zip_path)
