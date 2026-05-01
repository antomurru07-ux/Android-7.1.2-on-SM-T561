# LineageOS 14.1 on Samsung Galaxy Tab E SM-T561

> Flash Android 7.1.2 on a device stuck on Android 4.4.4 — and bring it back to life.

---

## Why this exists

The Samsung Galaxy Tab E (SM-T561) ships with Android 4.4.4 KitKat. Google dropped support for devices this old, meaning you can't log into Google accounts, modern websites don't render, and most apps refuse to install. This guide walks through replacing the stock ROM with **LineageOS 14.1 (Android 7.1.2)** to restore full usability.

---

## Device info

| Field | Value |
|-------|-------|
| Model | Samsung Galaxy Tab E SM-T561 |
| Variant | 3G |
| Codename | `gtel3g` |
| Stock Android | 4.4.4 (KitKat) |
| Target Android | 7.1.2 (LineageOS 14.1) |

---

## ⚠️ Disclaimer

> This process **wipes all data** on the device. Back up anything important before starting.
> Proceed at your own risk. Bricking the device is possible if steps are not followed correctly.

---

## Requirements

### Hardware
- Samsung Galaxy Tab E SM-T561
- Windows PC
- USB cable
- microSD card (to transfer files to the tablet)

### Files to download on your PC

| File | Where to get it |
|------|----------------|
| Odin 3.13.1 | sammobile.com — search "Odin 3.13.1" |
| Samsung USB Drivers | samsung.com — "USB Driver for Mobile Phones" |
| TWRP 3.5.2 | xdaforums.com — search "TWRP 3.5.2 SM-T561" — file: `twrp_3.5.2_gtel3g.tar` |
| LineageOS 14.1 for SM-T561 | xdaforums.com — search "LineageOS 14.1 SM-T561" |
| GApps pico | opengapps.org → ARM → 7.1 → pico |

Those files are the one that i included so either pick those up or look them for yourself

> **Why Odin 3.13.1?** Newer Odin versions can be unstable with older Samsung devices. 3.13.1 is the most reliable choice for the T561.

> **Why GApps pico?** The nano variant throws Error 20 on this device. Pico is the lightest variant that works reliably.

---

## Step 1 — Prepare your PC

1. Install the **Samsung USB Drivers** on your PC
2. Extract **Odin 3.13.1** (no installation needed — just run the `.exe`)
3. Copy **LineageOS** and **GApps** zip files onto the microSD card

---

## Step 2 — Flash TWRP via Odin

TWRP is a custom recovery that allows installing unofficial ROMs.

### Enter Download Mode on the tablet

1. Power off the tablet completely
2. Hold **Volume Down + Home + Power** simultaneously
3. A screen with a yellow triangle will appear
4. Press **Volume Up** to confirm
5. The screen turns blue with "Downloading..." text

### Flash with Odin

1. Open **Odin 3.13.1** on your PC
2. Connect the tablet via USB
3. Odin should show **"0:[number] Added"** in the bottom-left — this means the device is recognized
4. Click the **AP** button and select `twrp_3.5.2_gtel3g.tar`
5. Make sure **BL, CP, CSC** slots are all empty
6. In options, only **F. Reset Time** and **Auto Reboot** should be checked (if you have problems catching the right time uncheck the *Auto Reboot* option, in this way you directly skip to step 3.2)
7. Click **Start**

The flash takes about 30–60 seconds. When Odin shows **"PASS"**, you're done.

---

## Step 3 — Boot into TWRP

> ⚡ **Timing is critical here.** Samsung's bootloader will overwrite TWRP if Android boots even once after flashing. You must catch the device before it boots.

1. As soon as the tablet restarts and you see the Samsung logo, **immediately power it off** (hold Power for a few seconds)
2. With the device off, hold **Volume Up + Home + Power**
3. TWRP's blue/black interface should appear

### If the tablet gets stuck in Download Mode
Hold **Power + Volume Down** for 20–30 seconds to force a shutdown, then retry with Volume Up + Home + Power.

### First thing to do in TWRP
When TWRP asks **"Swipe to Allow Modifications"**, swipe to allow — otherwise you won't be able to install anything.

---

## Step 4 — Wipe and install

### Insert the microSD
Insert the microSD card with the LineageOS and GApps zip files before proceeding.

### Wipe the old system

1. In TWRP tap **Wipe**
2. Tap **Advanced Wipe**
3. Check: **Dalvik/ART Cache**, **System**, **Data**, **Cache**
4. ⚠️ **Do NOT check Internal Storage**
5. Swipe to confirm

### Install LineageOS

1. Go back to the TWRP main menu
2. Tap **Install**
3. Navigate to the microSD
4. Select the **LineageOS 14.1** zip file
5. Swipe to confirm installation
6. Wait for "Success"

### Install GApps — do NOT reboot first!

1. Tap **Install** again
2. Select the **GApps pico** zip file
3. Swipe to confirm
4. Wait for "Success"

### Reboot

1. Go back to the TWRP main menu
2. Tap **Reboot → System**

> The first boot takes several minutes. This is normal — LineageOS is setting up for the first time.

---

## Step 5 — Initial setup

1. Follow the LineageOS setup wizard
2. Sign in with your Google account when prompted
3. Open the **Play Store** and install your apps

---

## Performance tips

The SM-T561 has 1.5GB of RAM and a 2015-era processor. LineageOS runs, but it's not fast. Here's how to make it feel snappier:

### Reduce animation speed (biggest impact)
1. Go to **Settings → About device**
2. Tap **Build number** 7 times to unlock Developer Options
3. Go back → **Developer Options**
4. Set **Window animation scale**, **Transition animation scale**, and **Animator duration scale** all to **0.5x** or **Off**

### Clear system cache via TWRP
1. Boot into TWRP (Vol Up + Home + Power)
2. Tap **Wipe → Wipe Cache/Dalvik**
3. Reboot

### Use a lightweight browser
Chrome works but is heavy. For light browsing, **Firefox** (installable from Play Store) uses less RAM and leaves more headroom for the rest of the system.

### About swap/zRAM
Swap on physical storage (microSD or internal) is **not recommended** — it wears out flash memory quickly and is often slower than just having less RAM. zRAM (RAM compression) may already be active in LineageOS by default and is the safer option if you want to stretch available memory.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Odin doesn't recognize the tablet | Reinstall Samsung USB Drivers, try a different USB port |
| TWRP doesn't appear after flashing | Timing issue — power off immediately at the Samsung logo, then hold Vol Up + Home + Power |
| Error: "Can't install this package on top of incompatible data" | Do a full Wipe (Advanced Wipe) before installing the ROM |
| GApps Error 20 | Use **pico** variant instead of nano |
| Tablet stuck in Download Mode | Hold Power + Vol Down for 20–30 seconds to force shutdown |
| Still on Android 4.4.4 after reboot | Re-enter TWRP, redo the full Wipe, reinstall the ROM |
| Websites still not loading | Install Chrome from Play Store and use it instead of the stock browser |
| App stuck on loading screen | Try accessing the service via Chrome browser instead of the app |

---

## Result

After completing this guide you will have:

- Android 7.1.2 (LineageOS 14.1)
- Working Google Play Store
- Ability to install modern apps
- Chrome for browsing modern websites
- A tablet that was otherwise headed for the recycling bin, back in action

---

*Tested on SM-T561 running stock Android 4.4.4 — April 2026*
