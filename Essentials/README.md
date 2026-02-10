# Essentials to Get Up and Running

These are the core steps to get your Qidi Q2 set up and running well. Run all built-in calibrations and complete the initial setup wizard before starting anything here. Also make sure your firmware is up to date — if you update firmware later, it will overwrite any changes you've made to the config files (it does create a backup, but it's annoying to reintegrate).

---

## Table of Contents

- [Run All Built-in Calibrations](#run-all-built-in-calibrations)
- [Make a Custom Macro and Config File](#make-a-custom-macro-and-config-file)
- [Enable Full Display In Klipper](#enable-full-display-in-klipper)
- [Creating Buttons to Filament Change From Klipper](#creating-buttons-to-filament-change-from-klipper)
- [Install Octoeverywhere](#install-octoeverywhere)
- [Integrate KAMP Smart Bed Leveling](#integrate-kamp-smart-bed-leveling)
- [Quality Gcode Thumbnails on Q2 Screen](#quality-gcode-thumbnails-on-q2-screen)
- [Better Print Start Macro](#better-print-start-macro)

---

## Run All Built-in Calibrations

Run the initial setup wizard and complete all system calibrations before starting this guide.

The heated bed seemed slow heating up from the factory, so I ran a PID tune on the bed and hot end using these commands in the Klipper console:

```
PID_CALIBRATE HEATER=extruder TARGET=200
```
```
PID_CALIBRATE HEATER=heater_bed TARGET=60
```

Do `save_config` after each calibration if the printer doesn't do it automatically.

---

## Make a Custom Macro and Config File

This is the most important step before making any other changes. Creating your own config file keeps all your customizations in one easy-to-find place that won't get wiped by firmware updates.

In Fluidd (the Klipper web UI), go to your configuration page from the left side menu and add a new file using the plus icon at the top. Name it whatever you like — I named mine `My_Configs.cfg`. Make sure it is stored in the main directory.

Then open `printer.cfg` and add your new file to the include list at the top where the other files are referenced:

```ini
[include My_Configs.cfg]
```

When a firmware update runs, it will overwrite the stock config files and create backups of your changes. Having your own file means your customizations are separate and easy to compare against whatever Qidi changed.

> **Important:** There can only be ONE macro with any given name across all config files. If you add a custom `print_start` macro to your file, you must go into `gcode_macro.cfg`, find the existing `print_start` macro, highlight the entire thing, and use `CTRL + /` to comment it out.

---

## Enable Full Display In Klipper

Insert this code into your custom config file to see toolhead and host CPU temperatures in Klipper. Also go into Klipper settings and enable **Full Display** to see stepper driver temps.

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

Source: [bluedrool's Qidi Q2 tuning tweaks and mods](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md) — several other useful mods are listed in that repository as well.

---

## Creating Buttons to Filament Change From Klipper

A QOL improvement that gives you Load and Unload buttons right on the Klipper dashboard so you don't have to walk to the printer.

1. Go to Klipper settings and click **Macros** in the sidebar.
2. Click **Add Category** and name it whatever you like (e.g. "Filament Change").
3. Go into **Uncategorized** and find `M603` (unload) and `M604` (load) and move them to your new category.
4. Go into the new category, click each macro, and give them a friendly alias. They will now appear on the main Klipper dashboard.

---

## Install Octoeverywhere

The Octoeverywhere add-on lets you monitor your print remotely through the stock camera with AI spaghetti detection — for free. You install it by SSHing into your printer, which sounds scary but is straightforward.

1. Find your printer's IP address from the interface on the printer's screen.
2. Open a terminal (cmd, PowerShell, Putty, etc).
3. Connect with username `mks` and password `makerbase`:
   ```bash
   ssh mks@<your printer's IP>
   ```
4. Enter the password (`makerbase`) when prompted.
5. Paste and run this command:
   ```bash
   bash <(curl -s https://octoeverywhere.com/install.sh)
   ```
6. Follow any prompts. At the end you will be given a link and code to add the printer to your Octoeverywhere account.

Official Qidi guide: [https://wiki.qidi3d.com/en/Memo/octoeverywhere](https://wiki.qidi3d.com/en/Memo/octoeverywhere)

---

## Integrate KAMP Smart Bed Leveling

KAMP is a smart bed probing integration that only probes the area of the bed your print will actually use, rather than the whole surface. The print_start macro linked in the next section integrates KAMP, but there are a few additional setup steps. This guide covers it better than I could:

[https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated](https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated?tab=readme-ov-file)

---

## Quality Gcode Thumbnails on Q2 Screen

If your thumbnails look pixelated on the printer screen, this will fix it. Works for both Orca Slicer and Qidi Slicer.

Go to your printer profile in your slicer > **Basic Information** > **Advanced** > **Gcode Thumbnails** and insert:

```
300x300/PNG, 380x380/PNG
```

---

## Better Print Start Macro

This `print_start` macro was built on top of [Catsarecool700's QIDI Q2 optimised start macro](https://github.com/Catsarecool700/QIDI-Q2-optimised-start-macro) with a few minor changes that made it run more reliably with my setup. It is annotated with notes on what was changed and works with the Qidi Box.

The macro is linked in the main repository:
[https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Essentials](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Essentials)

Remember to comment out the existing `print_start` macro in `gcode_macro.cfg` before adding this one, as described in the [Make a Custom Macro and Config File](#make-a-custom-macro-and-config-file) section above.

---

## Sources

- [https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md)
- [https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated](https://github.com/Heiko910/Qidi-Q2-with-KAMP-fully-integrated?tab=readme-ov-file)
- [https://github.com/Catsarecool700/QIDI-Q2-optimised-start-macro](https://github.com/Catsarecool700/QIDI-Q2-optimised-start-macro)
- [https://wiki.qidi3d.com/en/Memo/octoeverywhere](https://wiki.qidi3d.com/en/Memo/octoeverywhere)

