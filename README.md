# Xiaomi Mi A1 (Tissot) Full System Restoration Guide

**By l1rox3 and adrian-refck**

> The ultimate restoration and rescue documentation for the Xiaomi Mi A1 (Tissot). Fix hard bricks (QFIL), solve bootloops, and restore a stable, rooted Android 8.1 system with custom optimizations.

**Estimated Time:** 30–60 minutes (depending on your scenario and experience level)

## Background
We wrote this guide because we spent a year working and trying to restore this device. At times, the phone was completely dead and had to be re-flashed from scratch, but there was no proper comprehensive guide available. Once we successfully finished the project (and the phone is now fully functional with root again), we decided to publish this guide along with the necessary files on GitHub to save others the time and hard work.

> **⚠️ IMPORTANT WARNING:** Please do NOT download the newest Fastboot ROM! Flashing the newest ROM will cause the device to fall into a bootloop.

---

This guide provides a step-by-step walkthrough to completely restore your Xiaomi Mi A1 (Tissot). 

* **Scenario 1:** The device does not turn on and provides no image -> **Go to Step 1**.
* **Scenario 2:** The device shows an image but is in a bootloop or similar state -> **Go to Step 2**.

> **Prerequisite:** Ensure your bootloader is unlocked before starting. If it is locked, please refer to a video tutorial on how to unlock it.

---

## ⬇ Downloads

### 1. Standard Restoration (No Root)
* **Tissot Fastboot ROM V9.6.4.0:** [Download Global Fastboot](https://xiaomirom.com/en/download/mi-a1-tissot-stable-V9.6.4.0.ODHMIFE/#global-fastboot)

### 2. Restoration with Root
* **Tissot Fastboot ROM V9.6.4.0:** [Download Global Fastboot](https://xiaomirom.com/en/download/mi-a1-tissot-stable-V9.6.4.0.ODHMIFE/#global-fastboot)
* **TWRP v3.7.0_9.0-tissot:** [Download Recovery](https://dl.twrp.me/tissot/twrp-3.7.0_9-0-tissot.img.html)
* **Magisk v17.3:** [Download Zip](https://github.com/topjohnwu/Magisk/releases/tag/v17.3)
* **Modified System Image:** [Download system_root.img (3GB via MEGA)](https://mega.nz/file/dNkEzCya#3Y6HVq-rnXxLFKOE7UB5qVDvA-w4-SwS9RP1uCzHpQQ)
* **Required Apps (Attached in Realeses):** `System Root Info.apk`, `Tissot Optimizer.apk`, `Root Terminal.apk`

---

## ⚙ Step 1: Emergency Flash via QFIL
**Author: adrian-refck**

If your device is "dead" and shows no signs of life, use QFIL to make it bootable again.

1.  **Open the device:** Watch tutorials if needed (e.g., [YouTube](https://www.youtube.com/watch?v=J6M-WBbPAZM)), but only open the housing—do not replace the battery.
2.  **Locate Test Points:** Find the specific test points for Tissot online.
3.  **Preparation:** Download the firmware (Android 8 is recommended).
4.  **Flashing:** Start QFIL. Bridge the two test points while plugging the USB cable into the phone. The device should now be selectable in QFIL.

> **Disclaimer from Adrian:** I accept no liability. Opening the device voids the warranty. These instructions are based on personal experience.

* **Troubleshooting:** If you encounter a "Sahara Protocol Error", try different cables or USB ports; QFIL is very sensitive.
* **Battery Note:** If a light illuminates after flashing while the battery is connected, try leaving the battery disconnected until the system starts.

> ✅ **Once the device has been successfully flashed via QFIL and shows signs of life, proceed to Step 2 to complete the full system restoration.**

---

## 🔁 Step 2: System Restoration
**Author: l1rox3**

### Option A: Clean System (No Root)
1.  Navigate to the extracted Fastboot ROM folder.
2.  Boot the device into **Fastboot Mode** (Hold Volume Down during startup).
3.  Run `flash_all.bat` (Windows) or `flash_all.sh` (Linux/macOS).
4.  **Important:** After the script finishes, return to Fastboot mode immediately. The `persist.img` is often skipped.
5.  Flash the persist image manually from the `/images` subfolder:
    ```bash
    fastboot flash persist images/persist.img
    fastboot reboot
    ```
6.  **Setup:** During initial setup, **skip Wi-Fi**. Downloading the latest updates immediately will cause a bootloop. Disable automatic software updates in settings once the setup is complete.

### Option B: System with Root
*Follow these steps exactly to avoid issues.*

1.  **Flash ROM:** Enter Fastboot and run `flash_all.bat` or `flash_all.sh`.
2.  **Flash Persist:** Once the script reboots the phone, return to Fastboot and flash the `persist.img` to prevent crashes:
    ```bash
    fastboot flash persist images/persist.img
    fastboot reboot
    ```
3.  **Initial Setup:** Wait 5-8 minutes for the first boot. Select "US English" and **skip everything** (SIM, Wi-Fi, Password). Disable all Google data collection and Xiaomi services.
4.  **Enable Debugging:** Go to `Settings` -> `About Phone` -> Tap `Build Number` until Developer Mode is active. In `Developer Options`, enable **USB Debugging** and **Enable view attribute inspection**.
5.  **Prepare Magisk:**
    ```bash
    adb devices
    adb push Magisk-v17.3.zip /sdcard/
    ```
    *Note: Use only version 17.3 for Android 8.1 Oreo compatibility.*
6.  **Boot TWRP:**
    ```bash
    adb reboot bootloader
    fastboot boot twrp-3.7.0_9-0-tissot.img
    ```
7.  **Install Magisk:** In TWRP, go to `Mount` and ensure `Data` is checked. Go to `Install`, select the Magisk zip, and swipe to flash. (Ignore `/vendor` messages). Reboot to System.
8.  **App Installation:** Once the Magisk app appears in the menu, install the provided APKs:
    ```bash
    adb install System_Root_Info.apk
    adb install Tissot_Optimizer.apk
    adb install Root_Terminal.apk
    ```
9.  **Final Security Patch:**
10.  ```bash
    adb reboot bootloader
    fastboot flash system_a system_root.img
    fastboot reboot
    ```
11. **Activation:** Open **System Root Info**, select "Accept & Apply Root Changes", and grant permanent Root access. Repeat for the other two apps to finalize the configuration.

---

## Credits
* **Tools:** TWRP, QFIL, MAGISK (https://github.com/topjohnwu/Magisk)
* **Graphics:** [Android Logo](https://www.vecteezy.com/free-png/android-logo) and [Lock Icon](https://de.vecteezy.com/kostenloses-png/schloss-offen) via Vecteezy

**Happy Modding!**
