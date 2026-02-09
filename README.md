# Qidi Q2 Printer Quick Start Guide

A quick start guide to QOL improvements and integrations

## Table of Contents

- [Introduction](#introduction)
- [Essential Setup](#essential-setup)
  - [Run All Built-in Calibrations](#run-all-built-in-calibrations)
  - [Enable Full Display In Klipper](#enable-full-display-in-klipper)
  - [Creating Buttons to Filament Change From Klipper](#creating-buttons-to-filament-change-from-klipper)
  - [Install Octoeverywhere](#install-octoeverywhere)
  - [Integrate KAMP Smart Bed Leveling](#integrate-kamp-smart-bed-leveling)
  - [Quality Gcode Thumbnails on Q2 Screen](#quality-gcode-thumbnails-on-q2-screen)
  - [Better Print Start Macro](#better-print-start-macro)
- [Essentials for Orca Slicer Users](#essentials-for-orca-slicer-users)
  - [Printer Profile Machine Gcodes](#printer-profile-machine-gcodes)
  - [Integrating Qidi Box With Orca Slicer](#integrating-qidi-box-with-orca-slicer)
- [Optional Add-Ons and Extras](#optional-add-ons-and-extras)
  - [Better Purge Line](#better-purge-line)
  - [Control Circulation Fan Through Slicer](#control-circulation-fan-through-slicer)
  - [Acceleration Speed Test Macro](#acceleration-speed-test-macro)
  - [How to Factory Reset](#how-to-factory-reset)
- [Sources](#sources)

## Introduction

Let me start by saying I am not a pro when it comes to code, 3D printers, or most things for that matter. I just like to tinker which has led me to this guide, which I originally created for myself. Through my tinkering I had to reinstall factory firmware 4 or 5 times after messing with things and not remembering how to back track and wanted a way to keep track of what I had done so I could quickly get back to the state I was before I goofed something up.

Again, not a pro, so take all of this with a grain of salt. This guide is primarily for those new to 3D printers, new to Klipper, or new to Qidi, to help them get up and running fairly quickly. Hopefully there is a tip or two in here some of the more experienced will find helpful as well. I know there are Wikis and pinned posts and all that. As newbie, these tutorials can sometimes be hard to find, so I hope this shortens the learning curve at least a little bit.

Thank you to all of you, who are much smarter than I am, for putting together the guides and repositories compiled here. This is not an all inclusive list, obviously, but as I find more helpful guides, I will continue to add them here for the future tinkers.

### Quick Notes and Things to Aid in Your Journey

- All the sources of these guides and tips that I didn't come up with will have a link at the bottom to find the original sources. A few of them include extra guides and mods that I haven't done myself.
- **Before you start**, I recommend creating backup of the config files you will be editing. Primarily `printer.cfg` and `gcode_macros.cfg`. It would suck to have to start all over a few times, as I did.
- Do `SAVE_CONFIG` in your Klipper console regularly.
- Things in this guide are subject to change with new methods and changes made from Qidi themselves. I will do my best to keep it updated as previously mentioned.
- None of these items are required for others. You can do all, none, or some. They are also in no particular order.
- This guide is primarily geared towards Qidi Slicer/Studio and Orca Slicer
- You can add the configs and macros in this guide directly to `printer.cfg` or `gcode_macros.cfg`. If you do this I recommend putting them at the top or bottom in a marked off area so they can easily be found. **HOWEVER**, a better way of doing this is to create your own config file in your printers directory, name is whatever you want (`My_config.cfg`, or something similar), and set your `printer.cfg` to include it with:
  ```
  [include My_Configs.cfg]
  ```
  - There should be a list at the top of `printer.cfg` with all the other included files. Add it there.
  - Keep in mind that if you add a macro with the same name as a pre-existing macro, the printer will not boot. Go in to the original macro and comment out the entire macro by highlighting it and use the keyboard shortcut `CTRL + /` to comment it out in one go.
- **While everything in here should be safe, I am not responsible for what you do to your printer.** I am not a pro. Take everything in here as if it were your 89 year old grandmother writing this.

## Essential Setup

### Run All Built-in Calibrations

This is pretty straight forward. Run the initial setup wizard, complete all the system calibrations, and ensure your firmware is up to date prior to beginning this guide. If you update firmware later on, the system will overwrite your changes to the config files. This isn't a big deal as it creates a backup of them, but annoying to go back in and reintegrate.

The heated bed seemed slow heating up from the factory, so I ran a PID tune on bed and hot end using these commands in the Klipper console:

```
PID_CALIBRATE HEATER=extruder TARGET=200
PID_CALIBRATE HEATER=heater_bed TARGET=60
```

To avoid loosing any progress, I recommend typing `save_config` into the Klipper console often throughout this guide.

### Enable Full Display In Klipper

Insert this code into any of your printer config files (`printer.cfg`, `gcode_macros.cfg`, etc) to see tool head and host CPU temperatures. It should work as long as the main `printer.cfg` file references the file you insert the code into. Also, go into Klipper settings and enable Full Display to see stepper driver temps.

This is from [bluedrool's Qidi Q2 tuning tweaks and mods](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md) and there are several other mods listed in this repository.

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

### Creating Buttons to Filament Change From Klipper

This one is really just a QOL improvement so I don't have to spin my chair around and walk three feet to the printer. It lets you separate the Load and Unload macros into their own, easily accessible category in Klipper.

1. Go to Klipper settings and click Macros in the side bar.
2. Click 'Add Category' and name it whatever you would like.
3. Then go into 'uncategorized' and find M603 and M604 and add them to your new Filament Change category. M603 being unload and M604 being load.
4. Once you do that, you can back out, go into the new Filament Change category, click on each M603 and M604 and give them an alias and rename them however you like. They should now show on the main dashboard.

### Install Octoeverywhere

I am not a fan of the Qidi Mobile App. In case you don't know, the Octoeverywhere add-on lets you view your print through the stock camera anywhere with AI spaghetti detection anywhere for free. To install it, you need to SSH into your printer. Sounds scary, but it's not that hard.

1. Find your printers IP address from the interface on the printer's screen.
2. Open cmd, putty, powershell or whatever terminal app you use.
3. The user name is `mks` and the password is `makerbase`
4. Type `ssh mks@<your printer's IP>` and hit enter
5. Type in the password (`makerbase`)
6. Paste this command into the terminal and hit enter:
   ```bash
   bash <(curl -s https://octoeverywhere.com/install.sh)
   ```
7. Let the scripts run, follow any prompts it give you, and eventually you be given a link to add the printer to your Octoeverywhere account with a code

Here is Qidi's official guide: [https://wiki.qidi3d.com/en/Memo/octoeverywhere](https://wiki.qidi3d.com/en/Memo/octoeverywhere)

### Integrate KAMP Smart Bed Leveling

I won't go into really any detail on this one, as this guide did a much better job than I would:

[https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated](https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated?tab=readme-ov-file)

### Quality Gcode Thumbnails on Q2 Screen

Go to your printer profile in your slicer > Basic Information > Advanced > Gcode Thumbnails. Insert this into the Gcode Thumbnails box:

```
300x300/PNG, 380x380/PNG
```

My thumbnails were very pixelated when I got my printer. This should fix this issue.

### Better Print Start Macro

This print_start macro was built off of [Catsarecool700's QIDI Q2 optimised start macro](https://github.com/Catsarecool700/QIDI-Q2-optimised-start-macro). I made a few minor changes that made it run a little more reliably with my setup. Mine is linked here with annotations in parenthesis on the things I changed.

## Essentials for Orca Slicer Users

### Printer Profile Machine Gcodes

The printer profile gcodes (machine start, filament change, etc) in Qidi Slicer seem to be more updated than the ones in Orca Slicer. I have no evidence that they are newer other than the fact they seem to work better. Before you get too deep into your printing, I recommend you download Qidi Slicer, and copy those gcodes over to your Orca profile, and maybe occasionally check in with Qidi slicer to see if they have changed. I was having extruder jamming issues with the Orca filament change gcode. That all went away once I grabbed the Qidi slicer gcode.

### Integrating Qidi Box With Orca Slicer

Before you start here, go into your printer profile > multimaterial > single extruder multimaterial parameters > set cooling tube position to 5 mm's. While in here, also check 'Manual Filament Change'. This seems counter intuitive, but helped me solve an issue with the print head going back to the purge bucket, acting like it was going to attempt a filament change and also removed extra movements.

Again, this post does a better job at explaining than I will. Follow this guide and Orca will work with the Qidi box. Keep in mind there are a couple minor limitations. The biggest being setting filament in Orca does not transfer to Klipper or the printer. This needs to be done through the Klipper dashboard or on the printer itself.

[https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new](https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new)

## Optional Add-Ons and Extras

### Better Purge Line

I hate the stock purge line. I can never get it all up in one go. Don't have a picture of this one, but it looks very similar and is in the same spot. Trust me on this one.

In your slicer, go to the printer profile settings and find your Machine start gcode. Replace the gcode shown at the top with the one at the bottom here:

[https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Better%20Purge%20Line](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Better%20Purge%20Line)

Note that you should not replace the entirety of the gcode in the Machine start gcode box. Only replace what is specified above. Alternatively, you could probably turn this into a macro.

### Control Circulation Fan Through Slicer

To enable fan control via your slicer, go into `gcode_macro` config file and find the `PRINT_START` macro. Find the line in that macro that says `M106 P3 S255` and comment that out with a hashtag (`#M106 P3 S255`). You can now control the circulation fan in your filament profile > Cooling > Exhaust Fan.

### Acceleration Speed Test Macro

Using this macro with this guide, you can tune your steppers current and find your steppers max acceleration for a given current. I attempted to use the macro from Ellis' guide, but it kept crashing pretty hard. I used AI to help make this one and can confirm it works. I added this to my custom config file as I described in the introduction section above.

- [Speed Test Macro](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Speed_Test%20Macro)
- [Ellis' Print Tuning Guide - Determining Motor Currents](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_motor_currents.html)

### How to Factory Reset

This printer isn't like previous generations of Qidi where they use the EMMC module. Instead you can do it with a USB-C cable. The USB port is under the small cover on left wall inside the print chamber. You will need to remove two screws to access it. Other than a USB, you will need the RKDevTool and the factory firmware which can both be downloaded here:

[https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr](https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr)

The DevTool is in chinese when you first install it. To fix that, open the `config.ini` file and change:

```ini
Lang1File=Chinese.ini
```

to

```ini
Lang1File=English.ini
```

Restart the app and you should be good. The program is a little weird, and honestly, I am not 100% how to run it. I just know to plug in the USB to your computer and printer > load the img file > go to Upgrade Firmware (after this point is when I get lost, but I think it is as follows) > press Switch, let it run > then Upgrade.

## Sources

- [https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main)
- [https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new](https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new)
- [https://ellis3dp.com/Print-Tuning-Guide/](https://ellis3dp.com/Print-Tuning-Guide/)
- [https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md)
- [https://github.com/qidi-community](https://github.com/qidi-community)
- [https://wiki.qidi3d.com/en/Memo/octoeverywhere](https://wiki.qidi3d.com/en/Memo/octoeverywhere)
