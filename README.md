## Silent EAC3 Playback on Windows 11 24H2

---

### Problem Description

Following a clean installation of Windows 11, Version 24H2, the Plex for Windows and Plex HTPC desktop applications failed to produce sound during video playback. The issue was specific to files containing EAC3 (Dolby Digital Plus) 5.1 audio tracks; music files and videos with other audio codecs like DTS or AAC played correctly. A key diagnostic symptom was the complete absence of the Plex application from the Windows Volume Mixer during the silent playback, indicating the app was failing to initialize an audio stream with the operating system.

---

### Symptoms

* After a fresh install of **Windows 11 Version 24H2**, videos with **EAC3 5.1 audio** were silent in both `Plex for Windows` and `Plex HTPC`.
* Music and videos with other audio codecs (DTS, AAC) worked correctly.
* The Plex app would not appear in the Windows Volume Mixer during silent playback.

---

### Root Cause

* Microsoft **removed native Dolby codecs** (including `DolbyDecMFT.dll`) from clean installations of **Windows 11 Version 24H2** due to a licensing change.
* The issue was not a bug, but a deliberate OS change.

---

### Summary of Failed Fixes

1.  **Plex Settings:** Toggling `Exclusive Audio` and other player settings had no effect.
2.  **System File Checks:** `sfc /scannow` and `DISM` found no errors because the file was missing by design, not corrupted.
3.  **Manual DLL Restore:** This was the critical failure point.
    * We confirmed `DolbyDecMFT.dll` was missing using `regsvr32`.
    * We extracted the DLL from an older **Windows 11 22H2 ISO**.
    * Manually installing this older DLL on the 24H2 system caused **catastrophic application crashes** due to binary incompatibility.
    * **Action:** The incompatible DLLs had to be manually unregistered and deleted to restore system stability.

---

### ✅ Final Solution

* The issue was resolved by installing the **"Dolby Digital Plus decoder for PC OEMs"** app from the **Microsoft Store**.
* This app is the official Microsoft-sanctioned method for restoring the EAC3 codec on Windows 11 24H2, providing the correct, version-compatible files.
