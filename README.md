# Qidi Q2 Printer Quick Start Guide

A quick start guide to QOL improvements and integrations for the Qidi Q2.

## Introduction

Let me start by saying I am not a pro when it comes to code, 3D printers, or most things for that matter. I just like to tinker which has led me to this guide, which I originally created for myself. Through my tinkering I had to reinstall factory firmware 4 or 5 times after messing with things and not remembering how to back track — I wanted a way to keep track of what I had done so I could quickly get back to the state I was in before I goofed something up.

Again, not a pro, so take all of this with a grain of salt. This guide is primarily for those new to 3D printers, new to Klipper, or new to Qidi, to help them get up and running fairly quickly. Hopefully there is a tip or two in here some of the more experienced will find helpful as well.

Thank you to all of you, who are much smarter than I am, for putting together the guides and repositories compiled here. This is not an all inclusive list, obviously, but as I find more helpful guides I will continue to add them here for the future tinkers.

---

## Quick Notes Before You Start

- All sources for guides and tips that I didn't come up with have links at the bottom of each doc to the original sources.
- **Before you start**, back up the config files you will be editing — primarily `printer.cfg` and `gcode_macros.cfg`.
- Run `SAVE_CONFIG` in your Klipper console regularly.
- Things in this guide are subject to change with new methods and updates from Qidi. I will do my best to keep it updated.
- None of these items are required. You can do all, none, or some. They are also in no particular order.
- The slicer specifics in this guide are primarily geared towards Qidi Slicer/Studio and Orca Slicer.
- **While everything in here should be safe, I am not responsible for what you do to your printer.** Take everything in here as if it were your 89 year old grandmother writing this.

---

## Contents

| Guide | Description |
|---|---|
| [Essentials](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Essentials) | Core setup steps to get your Q2 running well — calibrations, Klipper config, KAMP, Octoeverywhere, and more |
| [Orca Slicer](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Orca%20Slicer) | Orca Slicer-specific setup including machine gcodes and Qidi Box integration |
| [Optional Extras and Add-ons](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Optional%20Extras%20and%20Add-ons) | Nice-to-have improvements — better purge line, fan control, speed test macro, and more |
| [Factory Reset](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Factory%20Reset) | Step-by-step instructions for flashing factory firmware via USB-C |

---

## Sources

- [https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main)
- [https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new](https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new)
- [https://ellis3dp.com/Print-Tuning-Guide/](https://ellis3dp.com/Print-Tuning-Guide/)
- [https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md)
- [https://github.com/qidi-community](https://github.com/qidi-community)
- [https://wiki.qidi3d.com/en/Memo/octoeverywhere](https://wiki.qidi3d.com/en/Memo/octoeverywhere)
