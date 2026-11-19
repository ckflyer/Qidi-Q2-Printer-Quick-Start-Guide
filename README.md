# Qidi Q2 Printer Quick Start Guide

A quick start guide to QOL improvements and integrations for the Qidi Q2.

## Contents

- [Introduction](#introduction)
- [Disclaimer and quick notes](#disclaimer-and-quick-notes)
- [How to use this guide](#how-to-use-this-guide)
- [Transparency](#transparency)
- [1. Before You Start](#1-before-you-start)
  - [Back up your config files](#back-up-your-config-files)
  - [Update firmware first](#update-firmware-first)
  - [Run built-in calibrations and PID tune](#run-built-in-calibrations-and-pid-tune)
  - [Create your custom config file](#create-your-custom-config-file)
- [2. Klipper and Fluidd Setup](#2-klipper-and-fluidd-setup)
  - [Show toolhead and host CPU temps](#show-toolhead-and-host-cpu-temps)
  - [Load and unload filament buttons](#load-and-unload-filament-buttons)
  - [Better PRINT_START macro](#better-print_start-macro)
  - [KAMP adaptive bed mesh](#kamp-adaptive-bed-mesh)
  - [Control circulation fan from slicer](#control-circulation-fan-from-slicer)
- [3. Slicer Setup](#3-slicer-setup)
  - [Sharp Gcode thumbnails](#sharp-gcode-thumbnails)
  - [Better purge line](#better-purge-line)
  - [Orca: Qidi Box integration](#orca-qidi-box-integration)
  - [ORCA_QIDI_BOX macro](#orca_qidi_box-macro)
- [4. Remote Monitoring](#4-remote-monitoring)
  - [Install Octoeverywhere](#install-octoeverywhere)
  - [Mobileraker phone app](#mobileraker-phone-app)
- [5. Advanced Tuning, Extras, and Tips](#5-advanced-tuning-extras-and-tips)
  - [Acceleration speed test macro](#acceleration-speed-test-macro)
  - [Lubricating and cleaning your linear rods and screws](#lubricating-and-cleaning-your-linear-rods-and-screws)
  - [Cleaning your nozzle](#cleaning-your-nozzle)
  - [Spoolman with the Qidi Box (q2-spoolbridge)](#spoolman-with-the-qidi-box-q2-spoolbridge)
  - [Ellis print tuning guide](#ellis-print-tuning-guide)
  - [bluedrool tuning tweaks and mods](#bluedrool-tuning-tweaks-and-mods)
- [6. Recovery and Downgrading Firmware](#6-recovery-and-downgrading-firmware)
  - [Factory reset via USB-C](#factory-reset-via-usb-c)
  - [Downgrading firmware to v1.1.1](#downgrading-firmware-to-v111)
- [7. Reference](#7-reference)
  - [Macro and config files](#macro-and-config-files)
  - [Sources](#sources)
  - [PDF version](#pdf-version)

---

## Introduction

Let me start by saying I am not a pro when it comes to code, 3D printers, or most things for that matter. I just like to tinker, which has led me to this guide. I originally created it for myself. Through my tinkering I had to reinstall factory firmware 4 or 5 times after messing with things and not remembering how to backtrack, so I wanted a way to keep track of what I had done and quickly get back to where I was before I goofed something up.

Again, not a pro, so take all of this with a grain of salt. This guide is primarily for those new to 3D printers, new to Klipper, or new to Qidi, to help them get up and running fairly quickly. Hopefully there is a tip or two in here the more experienced will find helpful as well. I know there are wikis and pinned posts and all that, but as a newbie those tutorials can be hard to find, so I hope this shortens the learning curve at least a little bit.

Thank you to all of you, who are much smarter than I am, for putting together the guides and repositories compiled here. This is not an all-inclusive list, obviously, but as I find more helpful guides I will continue to add them here for future tinkerers.

## Disclaimer and quick notes

- **While everything in here should be safe, I am not responsible for what you do to your printer.** I am not a pro. Take everything in here as if it were your 89 year old grandmother writing it.
- **Back up first.** Before you change anything, back up the config files you will be editing, primarily `printer.cfg` and `gcode_macro.cfg`. It would suck to have to start all over a few times, like I did. See [Back up your config files](#back-up-your-config-files).
- Run `SAVE_CONFIG` in your Klipper console regularly.
- **I'm running firmware 1.1.1.** Later versions, while they look cool, kinda suck and are buggy. 1.1.1 is the most recent version I'd consider stable. Most of this guide should work on other versions, but not all of it may. See [Update firmware first](#update-firmware-first), or [Downgrading firmware to v1.1.1](#downgrading-firmware-to-v111) if you're already on something newer.
- Things in this guide are subject to change with new methods and updates from Qidi. I will do my best to keep it updated.
- The slicer specifics in this guide are geared towards Qidi Slicer/Studio and Orca Slicer.
- Anything I didn't come up with myself links to its original source, and everything is collected in [Sources](#sources). A few of those sources include extra guides and mods I haven't tried myself.

## How to use this guide

None of this is required. You can do all of it, none of it, or just the parts that sound useful.

That said, **do section 1 first.** Backing up and creating your own config file is what makes everything after it safe and easy to undo. After that, sections 2 through 5 can be done in any order. Section 6 is there for when things go wrong.

Any macro or code file mentioned in the guide lives in the [Resources](Resources) folder and is linked where it's used.

## Transparency

All of the README markdown in this repository was created using AI, as was the SPEED_TEST macro. I fully wrote the original PDF guide, but I simply don't have the know-how or patience to code these readme files. Despite me asking it not to, the AI changed some of the wording and formatting between the PDF and the readme. This version of the README was also reorganized and rewritten with AI, using my original guide as the source.

---

## 1. Before You Start

### Back up your config files

Before you edit anything, save copies of the files you'll be changing, mainly `printer.cfg` and `gcode_macro.cfg`.

In Fluidd, go to the configuration page from the left side menu, right-click a file and choose **Download** to save a copy to your computer. Do this again any time you have a setup you're happy with.

You'll want these if you ever need to [factory reset](#factory-reset-via-usb-c), since a reset wipes all your config changes.

### Update firmware first

> **Version note:** I'm running firmware **1.1.1**, and that's what this guide was written on. Later versions, while they look cool, kinda suck and are buggy. 1.1.1 is the most recent version I'd consider stable. Most of this guide should work on other versions, but not all of it may. Already on a newer version? See [Downgrading firmware to v1.1.1](#downgrading-firmware-to-v111).

Get your firmware to the version you want to stay on before starting this guide. If you update the firmware later on, it will overwrite your changes to the config files. That isn't a huge deal since it creates a backup of them, but it's annoying to go back in and reintegrate everything. Keeping your changes in [your own config file](#create-your-custom-config-file) makes this much easier.

### Run built-in calibrations and PID tune

This one is pretty straightforward. Run the initial setup wizard and complete all the system calibrations.

The heated bed seemed slow to heat up from the factory, so I also ran a PID tune on the bed and hot end using these commands in the Klipper console:

```
PID_CALIBRATE HEATER=extruder TARGET=200
```

```
PID_CALIBRATE HEATER=heater_bed TARGET=60
```

Run `SAVE_CONFIG` after each calibration if the printer doesn't do it automatically.

### Create your custom config file

This is the most important step before making any other changes. Your own config file keeps all your custom macros and configs in one easy-to-find place.

1. In Fluidd (the web UI used by Klipper), go to the configuration page from the left side menu.
2. Add a new file using the plus icon at the top of your files. Name it whatever you like. I named mine `My_Configs.cfg`. Make sure it's stored in the main directory.
3. Open `printer.cfg` and add your new file at the top, where the other files are included:

   ```ini
   [include My_Configs.cfg]
   ```

A starter version of this file is in [Resources/My_Configs.cfg](Resources/My_Configs.cfg).

**Why bother?** Any time you update your printer, it overwrites your changes to the stock config files and creates a backup of what you changed. With your own file, your changes are separate and easy to find. When you add custom macros, add comments on what you changed. Then when the printer updates, it's easy to compare against what Qidi changed and decide whether to use their new version, your custom one, or a mix of both.

> **Important:** There can only be ONE macro with any given name across all your config files. So if you add a custom `PRINT_START` macro to your file, you need to go into `gcode_macro.cfg`, find the existing `PRINT_START` macro, highlight the whole thing, and press `CTRL + /` to comment it out.

---

## 2. Klipper and Fluidd Setup

### Show toolhead and host CPU temps

Add this to your custom config file to see toolhead and host CPU temperatures in Klipper. (It works in any config file, as long as `printer.cfg` includes that file.) Also go into Klipper settings and enable **Full Display** to see stepper driver temps.

```ini
[temperature_sensor Host_CPU]
sensor_type: temperature_host
min_temp: 10
max_temp: 100

[temperature_sensor toolhead]
sensor_type: temperature_mcu
sensor_mcu: THR
min_temp: 0
max_temp: 100
```

This is already included in [Resources/My_Configs.cfg](Resources/My_Configs.cfg).

Source: [bluedrool's Qidi Q2 tuning tweaks and mods](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md). Several other useful mods are listed in that repository too.

### Load and unload filament buttons

This one is really just a QOL improvement so I don't have to spin my chair around and walk three feet to the printer. It puts Load and Unload buttons right on the Klipper dashboard. It's mainly useful if you don't have the Qidi Box or another AMS unit.

1. Go to Klipper settings and click **Macros** in the sidebar.
2. Click **Add Category** and name it whatever you like (for example, "Filament Change").
3. Go into **Uncategorized**, find `M603` (unload) and `M604` (load), and add them to your new category.
4. Back out, go into the new category, click each macro, and give it an alias to rename it however you like. They'll now show on the main dashboard.

### Better PRINT_START macro

**File:** [Resources/PRINT_START.cfg](Resources/PRINT_START.cfg)

This `PRINT_START` macro is built on [Catsarecool700's QIDI Q2 optimised start macro](https://github.com/Catsarecool700/QIDI-Q2-optimised-start-macro). I made a few minor changes that made it run more reliably with my setup, and each one is noted in parentheses in the macro's comments. It works with the Qidi Box.

The main changes:

- `CG28` changed to `G28`. `CG28` was moving straight to the left side of the bed and crashing into the corner of the purge parking area.
- `NOZZLE_WIPE` changed to `CLEAR_NOZZLE`, since `NOZZLE_WIPE` didn't exist in my macros.
- `CUT_FILAMENT_1` disabled.
- `SMART_PARK` added (though it doesn't seem to be working for me right now).
- The circulation fan line is commented out so the slicer can control it. See [Control circulation fan from slicer](#control-circulation-fan-from-slicer).

To use it, paste it into your custom config file. Remember to comment out the stock `PRINT_START` in `gcode_macro.cfg` first, as described in [Create your custom config file](#create-your-custom-config-file).

### KAMP adaptive bed mesh

KAMP is a smart bed probing integration that only probes and levels the part of the bed you'll actually be printing on for that specific print, rather than the whole surface. The [PRINT_START macro](#better-print_start-macro) above already integrates KAMP, but there are a few other setup steps. I won't go into much detail here, because this guide does a much better job than I would:

[Heiko910's Qidi Q2 with KAMP fully integrated](https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated?tab=readme-ov-file)

### Control circulation fan from slicer

By default, the circulation fan is hardcoded on in the stock `PRINT_START` macro. This change lets you control it from your filament profile in the slicer instead.

> **Already using my [PRINT_START macro](#better-print_start-macro)?** This change is already made. Skip ahead.

If you're using the stock macro:

1. Open your `gcode_macro.cfg` file.
2. Find the `PRINT_START` macro.
3. Find the line `M106 P3 S255` and comment it out by adding a `#` at the start:

   ```
   #M106 P3 S255
   ```

4. You can now control the circulation fan in your filament profile under **Cooling** > **Exhaust Fan**.

This may have just been a me issue. I haven't heard or seen others having it, but if you do, this is your fix.

---

## 3. Slicer Setup

### Sharp Gcode thumbnails

My thumbnails were very pixelated on the printer screen when I got my printer. This fixes it in both Orca Slicer and Qidi Slicer.

Go to your printer profile in your slicer > **Basic Information** > **Advanced** > **Gcode Thumbnails** and enter:

```
300x300/PNG, 380x380/PNG
```

### Better purge line

**File:** [Resources/Better_Purge_Line.gcode](Resources/Better_Purge_Line.gcode)

I hate the stock purge line. I can never get it off the bed in one go. This replacement looks very similar and sits in the same spot, but it's much easier to remove. I don't have a picture of this one, but trust me.

In your slicer, go to the printer profile settings and find your **Machine Start Gcode**. The file above shows the stock purge section at the top and the replacement at the bottom. Swap only that section.

> **Note:** Do not replace the entire Machine Start Gcode, only the section shown in the file. Alternatively, you could probably turn this into a Klipper macro.

### Orca: Qidi Box integration

*If you only use Qidi Slicer, you can skip the Orca items.*

> **Firmware note:** These steps are what I needed on firmware **1.1.1**. Newer versions may work without this, but I'm not sure. I've also been told the Qidi Box works with Orca Slicer out of the box on firmware older than 1.1.0 (with the few limitations listed in the Reddit post below), but I haven't tested that.

**Before you start**, go into your printer profile > **Multimaterial** > **Single Extruder Multimaterial Parameters** and make these two changes:

- Set **Cooling Tube Position** to `5 mm`.
- Check **Manual Filament Change**.

Manual Filament Change seems counterintuitive, but it fixed an issue for me where the print head went back to the purge bucket mid-print as if it were going to attempt a filament change. It also removed some extra movements.

**Then follow this guide.** This Reddit post does a better job of explaining the full integration than I can, and once you follow it, Orca will work with the Qidi Box:

[Qidi Box with Q2 on Orca Slicer - here's how to get it working (r/QidiTech3D)](https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new)

**Importing filaments from the printer:** On Orca Slicer version 2.5.0 or later, you can import and send the filaments loaded on your printer. Go to **Settings** > **Developer** > **Experimental features** and check **Use printer agents instead of printer hosts**.

### ORCA_QIDI_BOX macro

**File:** [Resources/ORCA_QIDI_BOX.cfg](Resources/ORCA_QIDI_BOX.cfg)

This macro goes with the Orca + Qidi Box setup above. It tells the printer which Qidi Box slot to start from, based on the first tool your Orca print uses.

1. Paste the macro into your custom config file.
2. Add this line to your Machine Start Gcode in Orca:

   ```
   ORCA_QIDI_BOX T=[initial_tool]
   ```

Tools `T0` to `T3` map to Box slots 1A to 1D. If more boxes are connected, the extra tools continue in the same order. A tool number higher than your available Box slots, or `-1`, disables the Box and uses the external spool on the rack instead.

---

## 4. Remote Monitoring

### Install Octoeverywhere

I'm not a fan of the Qidi mobile app. In case you don't know, the Octoeverywhere add-on lets you watch your print through the stock camera from anywhere, with AI spaghetti detection, for free. To install it you need to SSH into your printer. Sounds scary, but it's not that hard.

1. Find your printer's IP address from the interface on the printer's screen.
2. Open a terminal (cmd, PowerShell, PuTTY, or whatever terminal app you use).
3. Connect with username `mks` and password `makerbase`:

   ```bash
   ssh mks@<your printer's IP>
   ```

4. Enter the password (`makerbase`) when prompted.
5. Paste this command and hit enter:

   ```bash
   bash <(curl -s https://octoeverywhere.com/install.sh)
   ```

6. Let the script run and follow any prompts. At the end you'll get a link and a code to add the printer to your Octoeverywhere account.

Qidi's official guide: [wiki.qidi3d.com/en/Memo/octoeverywhere](https://wiki.qidi3d.com/en/Memo/octoeverywhere)

### Mobileraker phone app

Mobileraker is a free phone app (iOS and Android) that works as a Klipper dashboard on your phone. You can see temps and progress, pause, resume or stop a print, run macros, and watch the camera. It's a lot nicer than the Qidi app.

1. Install **Mobileraker** from the App Store or Google Play.
2. Open the app and tap **Add Machine**, then **Simple**.
3. Enter your printer's IP address (the same one you used for [Octoeverywhere](#install-octoeverywhere)) and save.

That gets you control on your home network.

**Remote access:** If you've set up [Octoeverywhere](#install-octoeverywhere), Mobileraker can connect through it, so you can use the app away from home too. Look for the remote connection options in the printer's settings in the app.

**Push notifications** (print done, progress, errors) need a small add-on called Mobileraker Companion installed on the printer. SSH in the same way as the [Octoeverywhere steps](#install-octoeverywhere) and run:

```bash
git clone https://github.com/Clon1998/mobileraker_companion.git
./mobileraker_companion/scripts/install.sh
```

Then allow notifications in the app under **App Settings**.

More info: [Mobileraker](https://github.com/Clon1998/mobileraker) and [Mobileraker Companion](https://github.com/Clon1998/mobileraker_companion)

---

## 5. Advanced Tuning, Extras, and Tips

### Acceleration speed test macro

**File:** [Resources/SPEED_TEST.cfg](Resources/SPEED_TEST.cfg)

Using this macro along with Ellis' guide on [determining motor currents](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_motor_currents.html), you can tune your stepper motor current and find the max acceleration for a given current. I tried the macro from Ellis' guide, but it kept crashing pretty hard on the Q2, so I used AI to help make this one. I can confirm it works on the Q2.

Add it to your custom config file as described in [Create your custom config file](#create-your-custom-config-file), then run it from the Klipper console:

```
SPEED_TEST SPEED=300 ACCEL=5000 BOUND=30 ITERATIONS=5
```

It homes, runs a test pattern, then homes again. Compare the two `GET_POSITION` outputs: if the MCU positions differ by more than one full step, the motors skipped.

### Lubricating and cleaning your linear rods and screws

#### Linear rods

Use a clean cloth to wipe down the rods. Apply a small amount of Super Lube (I've read to use Super Lube without syncolon, but not 100%) and apply by wiping the rods once more. You don't need a lot. Do not use grease!

#### Leadscrews

Use the [Rod Sloth](https://www.printables.com/model/605305-rod-sloth-fits-most-3d-printers-prusa-bambu-creali) to clean out the screws with a thin rag and some IPA. Raise or lower your bed all the way and apply the grease that came with the printer, or white lithium grease.

#### Spreading the lube

**File:** [Resources/LUBE_IT.cfg](Resources/LUBE_IT.cfg)

Once your screws and rods have been lubed, you can add this macro to your custom config file and run it to spread the lube. Clear the bed (above and below) first, then run this in the Klipper console:

```
LUBE_IT
```

It homes (if needed), runs X and Y back and forth across their full travel and diagonally, then runs the bed up and down the full length of the leadscrews. It does 5 passes by default and stays 20mm away from the edges. You can change that with `LUBE_IT PASSES=10` in the console. The other options are listed at the top of the file.

THIS IS UNTESTED BE CAREFUL!!!!!!

### Cleaning your nozzle

This took me way too long to figure out, so I figured I'd throw it in here.

You can use an old cotton rag to clean oozing filament off your nozzle. It works amazingly. I use old socks or a cut-up T-shirt. Just make sure it's **100% cotton**. I like socks because they're usually thicker, so it's harder to burn yourself through them.

### Spoolman with the Qidi Box (q2-spoolbridge)

[q2-spoolbridge](https://github.com/Warre118/q2-spoolbridge) by Warre118 connects the Qidi Box to [Spoolman](https://github.com/Donkie/Spoolman) and keeps track of how much filament is left on each spool. You assign a spool to each Box slot in Fluidd, and it automatically switches the active spool to whatever slot the Box has loaded. It also corrects the usage counts that Moonraker gets wrong during tool changes and purges. This is built for version 01.01.03.x, so I haven't personally tested it, but looks like a pretty cool project.

### Ellis print tuning guide

[Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide/) is the go-to reference for dialing in your prints. The motor current article linked above comes from here.

### bluedrool tuning tweaks and mods

[bluedrool's Qidi Q2 tuning tweaks and mods](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods) has some great extra guides. I've only tried the [TMC autotune](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md) and couldn't get it working properly, but I've heard good things about the other guides.

---

## 6. Recovery and Downgrading Firmware

### Factory reset via USB-C

> **Only do this if you need to.** A factory reset wipes all your config changes. Make sure you have [backups](#back-up-your-config-files) of `printer.cfg`, `gcode_macro.cfg`, and any other files you've changed.

This printer isn't like previous Qidi generations that used an EMMC module. On the Q2, you factory reset over USB-C.

**What you'll need:**

- A USB-C cable
- **RKDevTool**
- **The factory firmware image** (`.img` file)
- [7-Zip](https://www.7-zip.org/) (free) to extract the downloads

You can download RKDevTool and the firmware from either of these:

1. **From this repo:** download [Recovery.7z](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/releases/download/Recovery/Recovery.7z) (about 1.9 GB) from the [Update and Recovery release](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/releases/tag/Recovery). These are the files I downloaded and used. It contains:
   - `KLP_IMG_QD_Q2_V1.0.1_20250505_Release.7z`: the factory firmware (v1.0.1). It's another archive, so extract it too to get the `.img` file.
   - `RKDevTool_v3.19_for_window.rar`: RKDevTool for Windows.
   - `re-flash emmc English version.docx`: Qidi's translated instructions.
2. **From Google Drive:** [Qidi Q2 factory firmware and RKDevTool (Google Drive)](https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr)

**Step 1: Set RKDevTool to English.** It's in Chinese when you first install it. To fix that:

1. Open `config.ini` in the RKDevTool folder.
2. Change this line:

   ```ini
   Lang1File=Chinese.ini
   ```

   to:

   ```ini
   Lang1File=English.ini
   ```

3. Save the file and restart RKDevTool.

**Step 2: Get to the USB-C port.** It's inside the print chamber, under a small cover on the **left wall**. Remove the two screws to get to it.

**Step 3: Flash the firmware.**

1. Connect the USB-C cable between your computer and the printer.
2. Open RKDevTool and load the `.img` firmware file.
3. Go to **Upgrade Firmware**.
4. Press **Switch** and let it run.
5. Press **Upgrade**.

> Honestly, I'm not 100% sure of the exact steps after going to Upgrade Firmware. The program is a little weird, and this is my best recollection of what worked. If you run into trouble, check the [Qidi community](https://github.com/qidi-community) for more detailed walkthroughs. I also have the "translated" instructions from Qidi in the Recovery folder: [re-flash emmc English version.docx](<Resources/Recovery/re-flash emmc English version.docx>).

### Downgrading firmware to v1.1.1

You'll need to do the [factory reset steps above](#factory-reset-via-usb-c) first. If you've done that, you're most of the way there.

1. If you're able to, [back up](#back-up-your-config-files) any config files you don't want to lose before starting.
2. Download [QD_Update.7z](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/releases/download/Recovery/QD_Update.7z) from the [Update and Recovery release](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/releases/tag/Recovery). This will update you to v1.1.1.
3. Right-click the file and choose **7-Zip** > **Extract to "QD_Update\"**. This creates a `QD_Update` folder with three files in it (`QD_Q2_MCU`, `QD_Q2_SOC_V1.1.1_20260109_Release`, and `QD_Q2_THR`).
4. Copy the whole `QD_Update` folder (not just the files inside it) to the root of a USB stick. The stick should be formatted to FAT32 and at least 8GB.
5. Plug the USB stick into your printer, power on, go to **Offline Update** and run the update.
6. You'll need to rerun all your calibrations and set up your config files again.

---

## 7. Reference

### Macro and config files

Everything lives in the [Resources](Resources) folder:

| File | What it is | Used in |
|---|---|---|
| [My_Configs.cfg](Resources/My_Configs.cfg) | Starter custom config file, with the temp sensors | [Create your custom config file](#create-your-custom-config-file), [Show toolhead and host CPU temps](#show-toolhead-and-host-cpu-temps) |
| [PRINT_START.cfg](Resources/PRINT_START.cfg) | Better print start macro, with KAMP and Qidi Box support | [Better PRINT_START macro](#better-print_start-macro) |
| [Better_Purge_Line.gcode](Resources/Better_Purge_Line.gcode) | Replacement purge line for your slicer's start gcode | [Better purge line](#better-purge-line) |
| [ORCA_QIDI_BOX.cfg](Resources/ORCA_QIDI_BOX.cfg) | Macro for using the Qidi Box with Orca Slicer | [ORCA_QIDI_BOX macro](#orca_qidi_box-macro) |
| [SPEED_TEST.cfg](Resources/SPEED_TEST.cfg) | Acceleration and motor current test macro | [Acceleration speed test macro](#acceleration-speed-test-macro) |
| [LUBE_IT.cfg](Resources/LUBE_IT.cfg) | Spreads lube along the rods and leadscrews | [Lubricating and cleaning your linear rods and screws](#lubricating-and-cleaning-your-linear-rods-and-screws) |
| [re-flash emmc English version.docx](<Resources/Recovery/re-flash emmc English version.docx>) | Qidi's translated re-flash instructions | [Factory reset via USB-C](#factory-reset-via-usb-c) |
| [Update and Recovery release](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/releases/tag/Recovery) | `Recovery.7z` (factory image, RKDevTool, re-flash docx) and `QD_Update.7z` (v1.1.1 update) | [Factory reset via USB-C](#factory-reset-via-usb-c), [Downgrading firmware to v1.1.1](#downgrading-firmware-to-v111) |
| [Original PDF guide](<Resources/Qidi Q2 Printer Quick Start Guide (original).pdf>) | The first version of this guide | [PDF version](#pdf-version) |

### Sources

- [Catsarecool700: QIDI Q2 optimised start macro](https://github.com/Catsarecool700/QIDI-Q2-optimised-start-macro)
- [Heiko910: Qidi Q2 with KAMP fully integrated](https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated?tab=readme-ov-file)
- [bluedrool: Qidi Q2 tuning tweaks and mods](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods) ([TMC autotune guide](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md))
- [r/QidiTech3D: Qidi Box with Q2 on Orca Slicer](https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new)
- [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide/) ([Determining motor currents](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_motor_currents.html))
- [Qidi Wiki: Octoeverywhere](https://wiki.qidi3d.com/en/Memo/octoeverywhere)
- [Mobileraker](https://github.com/Clon1998/mobileraker) and [Mobileraker Companion](https://github.com/Clon1998/mobileraker_companion)
- [Qidi community on GitHub](https://github.com/qidi-community)
- [Rod Sloth (Printables)](https://www.printables.com/model/605305-rod-sloth-fits-most-3d-printers-prusa-bambu-creali)
- [Warre118: q2-spoolbridge](https://github.com/Warre118/q2-spoolbridge)
- [Qidi Q2 factory firmware and RKDevTool (Google Drive)](https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr)
- [This guide on GitHub](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide)

### PDF version

The [original PDF guide](<Resources/Qidi Q2 Printer Quick Start Guide (original).pdf>) is kept for reference, but it's the older version. This README is the current one.
