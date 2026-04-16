# 🎧 Retro Music Player – Android 14 Port

This repository contains a compatibility patch for a classic Android music player with a KitKat-era (Holo UI) design.
The goal of this project is to restore the application so it runs properly on modern hardware and Android 14 (including HyperOS), overcoming security restrictions and changes in internal system APIs.

---

## 🚀 Technical Challenges & Solutions

The restoration process involved direct reverse engineering using Smali code and AndroidManifest modifications.

---

### 1. 🔐 Security & SharedPreferences

**Problem:**
The app attempted to access shared preferences using the deprecated flag `MODE_WORLD_READABLE (0x1)`, which triggers an immediate `SecurityException` on modern Android versions.

**Solution:**
`MediaPlaybackService.smali` was modified to replace the access mode with:

```
MODE_PRIVATE (0x0)
```

---

### 2. 🗄️ SQL Query Incompatibility

**Problem:**
Runtime error:

```
IllegalArgumentException: Invalid column audio._id
```

Modern `ContentProviders` no longer accept table prefixes (`audio.`) in query projections.

**Solution:**
The string constant in `MediaPlaybackService.smali` (around line 242) was patched:

```
audio._id AS _id  →  _id
```

---

### 3. ⚙️ Installation Restrictions (Target SDK)

**Problem:**
Android 14 blocks installation of apps targeting SDK versions lower than 23.

**Solution:**

* Updated `targetSdkVersion` to **24**
* Added modern permissions:

  * `READ_MEDIA_AUDIO`
  * `POST_NOTIFICATIONS`

This ensures background playback works correctly on newer systems.

---

## 🛠️ Tools Used

* **Apktool** → APK decompilation and recompilation
* **uber-apk-signer** → APK signing and zip alignment
* **ADB** → Installation with bypass flag:

```
adb install --bypass-low-target-sdk-block app.apk
```

---

## 📱 Performance Notes (HyperOS / Xiaomi)

To ensure stable playback:

* Disable battery restrictions for the app
* Allow background execution
* Avoid aggressive system optimizations

---

## 🎯 Project Goal

Preserve legacy Android applications while maintaining:

* Original Holo UI aesthetics
* Core functionality
* Compatibility with modern Android systems

---

## ⚠️ Disclaimer

This project is experimental and intended for learning purposes in reverse engineering and compatibility patching.
Behavior may vary depending on device and system customization layer.

---

## 🙌 Credits

* Original application developers (unknown)
* Reverse engineering & patching by repository author
* English translation assisted by ChatGPT (OpenAI)

---
