# Orca Slicer Setup for the Qidi Q2

This guide covers the Orca Slicer-specific steps to get the best experience with the Q2. If you are using Qidi Slicer exclusively, you can skip this section.

---

## Table of Contents

- [Printer Profile Machine Gcodes](#printer-profile-machine-gcodes)
- [Integrating Qidi Box With Orca Slicer](#integrating-qidi-box-with-orca-slicer)

---

## Printer Profile Machine Gcodes

The printer profile gcodes (machine start, filament change, etc.) in Qidi Slicer seem to be more up to date than the ones bundled with Orca Slicer. I have no hard evidence of this other than the fact they work better — I was having extruder jamming issues with the Orca filament change gcode that completely went away once I swapped in the Qidi Slicer versions.

**Recommended steps:**

1. Download Qidi Slicer if you don't already have it.
2. Open your printer profile in Qidi Slicer and copy the machine start gcode, filament change gcode, and any other profile gcodes.
3. Paste them into the corresponding fields in your Orca Slicer printer profile.
4. Check back in Qidi Slicer occasionally to see if they've been updated.

---

## Integrating Qidi Box With Orca Slicer

> **Note:** If your printer firmware is older than version 1.1.0, the Qidi Box may work out of the box with Orca Slicer (with a few minor limitations listed in the Reddit post below). If you are on firmware 1.1.0 or newer, follow this guide. I have not personally tested this distinction yet.

### Before You Start

Go into your printer profile > **Multimaterial** > **Single Extruder Multimaterial Parameters** and make these two changes:

- Set **Cooling Tube Position** to `5 mm`
- Make sure **Manual Filament Change** is checked

The Manual Filament Change option seems counter-intuitive, but it fixes an issue where the print head unnecessarily returns to the purge bucket mid-print as if attempting a filament change, and also eliminates extra movements.

### Follow This Guide

This Reddit post does a better job of explaining the full integration than I can. Follow it and Orca will work with the Qidi Box:

[https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new](https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new)

### Known Limitations

The biggest limitation is that setting filament in Orca Slicer does **not** transfer to Klipper or the printer. Filament must be set separately through the Klipper dashboard or directly on the printer screen.

---

## Sources

- [https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new](https://www.reddit.com/r/QidiTech3D/comments/1pnplo6/qidi_box_with_q2_on_orca_slicer_heres_how_to_get/?sort=new)

