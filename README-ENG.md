# 🎧 Retro Music Player – Android 14 Port

## 🇪🇸 Read in Spanish → README.md

This repository contains a compatibility patch for a classic Android 4.4.2 music player with a KitKat-era (Holo UI) design.
The goal of this project is to restore the application so it runs properly on modern hardware and Android 14 (including HyperOS), overcoming security restrictions and internal API changes.

---

## 📸 Before / After

<p align="center">
  <img src="assetsC/old.jpg" width="300"/>
  <img src="assetsC/new.jpg" width="300"/>
</p>

---

## 📱 Screenshots

<p align="center">
  <img src="assetsC/cap1.jpg" width="250"/>
  <img src="assetsC/cap3.jpg" width="250"/>
  <img src="assetsC/cap4.jpg" width="250"/>
</p>

---

## 🚀 Technical Challenges & Solutions

The process involved direct reverse engineering using Smali code and AndroidManifest modifications.

---

### 🔧 Modified Code Sections

* **Security Patch:**
  Fixed a `SecurityException` crash by changing the SharedPreferences mode to `0x0 (MODE_PRIVATE)` at line 5778 in `MediaPlaybackService.smali`.

* **Database Patch:**
  Fixed playback crash by removing the deprecated `audio.` prefix from the `_id` column at line 242 in the same file.

* **System Compatibility:**
  Updated `targetSdkVersion` to **24** and added modern permissions (`READ_MEDIA_AUDIO`) in `AndroidManifest.xml` to prevent blocking on Android 10+.

---

### 1. 🔐 Security & SharedPreferences

**Problem:**
The app used the deprecated flag `MODE_WORLD_READABLE (0x1)` when accessing preferences, which causes a `SecurityException` on modern Android versions.

**Solution:**
Modified `MediaPlaybackService.smali` to replace it with:

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

Modern `ContentProviders` no longer accept table prefixes (`audio.`) in queries.

**Solution:**
Patched the string in `MediaPlaybackService.smali` (around line 242):

```
audio._id AS _id  →  _id
```

---

### 3. ⚙️ Installation Restrictions (Target SDK)

**Problem:**
Android 14 blocks apps with `targetSdkVersion < 23`.

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

* Disable battery restrictions
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

This project is experimental and focused on reverse engineering and compatibility learning.
Behavior may vary depending on the device or system customization layer.

---

## 😈 Bonus

This project is part of a broader focus on:

* Legacy software restoration
* Android reverse engineering
* Optimization of classic apps on modern systems

---
