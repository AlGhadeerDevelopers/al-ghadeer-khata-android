# Al Ghadeer Khata Pro — Android & Ionic Capacitor Architecture Guide

**App Name:** Al Ghadeer Khata Pro  
**Package ID:** `com.alghadeer.khatapro`  
**Official Email:** `info.alghadeer@yahoo.com`  
**Platforms Supported:** Android 7.0+ (API Level 24 through Android 15 / API 35)

---

## 1. Project Directory Overview

```text
AlGhadeerKhataProAndroid/
├── package.json                   # Capacitor 6 core, Android runtime & native plugins
├── capacitor.config.json          # Package ID, status bar #052359, orientation
├── www/
│   ├── index.html                 # Mobile-optimized HTML5 suite with Native Bottom Nav
│   └── icon (2)_2.jpg             # High-res official brand asset
├── resources/                     # Pre-scaled Android launcher icons (mdpi to xxxhdpi)
│   ├── mipmap-mdpi/
│   ├── mipmap-hdpi/
│   ├── mipmap-xhdpi/
│   ├── mipmap-xxhdpi/
│   └── mipmap-xxxhdpi/
└── .github/
    └── workflows/
        └── build-apk.yml          # GitHub Actions automated cloud build (Zero setup required!)
```

---

## 2. Automated APK Build: Method B (Zero Local Setup via GitHub Actions)

**Best for users who do not want to download heavy 10GB Android Studio:**

1. Create a free GitHub repository (public or private) named `al-ghadeer-khata-pro`.
2. Extract `Al_Ghadeer_Khata_Pro_Android_Capacitor_Project.zip` and upload all files (including the `.github` folder) to your repository.
3. In your GitHub repository, click on the **Actions** tab.
4. The workflow **"Build Al Ghadeer Khata Pro Android APK & AAB"** will automatically trigger.
5. In ~3 minutes, click on the finished workflow run and download **`Al-Ghadeer-Khata-Pro-Debug-APK`** directly to your phone to install immediately!

---

## 3. Local APK Build: Method A (Via Terminal / Android Studio)

If you have Node.js and Java JDK 17 installed on your PC:

### Step 1: Install Dependencies
```bash
npm install
```

### Step 2: Initialize Android Native Project
```bash
npx cap add android
npx cap sync android
```

### Step 3: Copy Brand Launcher Icons
```bash
cp -r resources/mipmap-* android/app/src/main/res/
```

### Step 4: Build Signed / Debug APK
```bash
cd android
./gradlew assembleDebug
```
*Your installable `.apk` will be generated at:*
```text
android/app/build/outputs/apk/debug/app-debug.apk
```

### Step 5: (Optional) Open in Android Studio
```bash
npx cap open android
```
From Android Studio, click **Build > Generate Signed Bundle / APK** to generate `.aab` for Google Play Store.

---

## 4. Native Permissions (`AndroidManifest.xml`)

Capacitor automatically manages these permissions, but you can inspect them inside `android/app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

---

## 5. Mobile Capabilities Verified in `index.html`

- **Hardware Back Button**: Dismisses open modals (Account, Transaction, Cashbook, WhatsApp, Print Preview) before exiting the app.
- **Native Camera & Gallery**: Instant capture of bill/receipt photos using `@capacitor/camera`.
- **Phone Downloads Folder**: Exports `.xlsx` multi-sheet workbooks and `.json` backups via `@capacitor/filesystem` and triggers the native Android Share sheet (`@capacitor/share`).
- **Direct WhatsApp Intent**: Uses `whatsapp://send?phone=...&text=...` to open WhatsApp / WhatsApp Business immediately without slow browser redirects.
- **Fixed Bottom Navigation Bar**: Ergonomic 6-tab thumb navigation for one-handed operation on mobile screens.
