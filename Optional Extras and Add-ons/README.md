
# Optional Extras and Add-ons

None of these are required, but they are all improvements I've found worth making. Pick and choose whatever sounds useful to you.

---

## Table of Contents

- [Better Purge Line](#better-purge-line)
- [Control Circulation Fan Through Slicer](#control-circulation-fan-through-slicer)
- [Acceleration Speed Test Macro](#acceleration-speed-test-macro)
- [Qidi Q2 Tuning Tweaks and Mods](#qidi-q2-tuning-tweaks-and-mods)

---

## Better Purge Line

I hate the stock purge line — I can never get it off the bed cleanly in one go. This replacement looks very similar and sits in the same spot, but is much easier to remove. Trust me on this one.

In your slicer, go to the printer profile settings and find your **Machine Start Gcode**. Replace only the purge line section (not the entire start gcode) with the one found here:

https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Macros%20and%20Configs

> **Note:** Only replace the specific section shown in the file above — do not replace your entire Machine Start Gcode. Alternatively, this could be converted into a Klipper macro.

---

## Control Circulation Fan Through Slicer

By default the circulation fan is hardcoded on in the `PRINT_START` macro. This change lets you control it from your filament profile in the slicer instead.

> This may have just been a me issue — I haven't seen others report this problem. But if you're experiencing it, here's the fix.

1. Open your `gcode_macro` config file.
2. Find the `PRINT_START` macro.
3. Find the line that reads `M106 P3 S255` and comment it out by adding a `#` at the start:
   ```
   #M106 P3 S255
   ```
4. You can now control the circulation fan in your filament profile under **Cooling** > **Exhaust Fan**.

---

## Acceleration Speed Test Macro

This macro lets you tune your stepper motor current and find the maximum acceleration for a given current. I tried the macro from Ellis' guide but it kept crashing hard on the Q2, so I used AI to build a version that works. I've confirmed it works on this printer.

Add this to your custom config file as described in the [Essentials guide](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/tree/main/Essentials).

- [Speed Test Macro](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Speed_Test%20Macro)
- [Ellis' Print Tuning Guide — Determining Motor Currents](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_motor_currents.html)

---

## Qidi Q2 Tuning Tweaks and Mods

This repository by bluedrool has some excellent additional guides. I've only personally tried the TMC autotune (couldn't get it working properly), but the other guides have a good reputation:

[https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods/blob/main/docs/autotune.md)

---

## Sources

- [https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Better%20Purge%20Line](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Better%20Purge%20Line)
- [https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Speed_Test%20Macro](https://github.com/ckflyer/Qidi-Q2-Printer-Quick-Start-Guide/blob/main/Speed_Test%20Macro)
- [https://ellis3dp.com/Print-Tuning-Guide/articles/determining_motor_currents.html](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_motor_currents.html)
- [https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods](https://github.com/bluedrool/Qidi-Q2-tuning-tweaks-and-mods)
