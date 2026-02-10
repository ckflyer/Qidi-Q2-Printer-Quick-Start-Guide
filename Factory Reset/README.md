# How to Factory Reset the Qidi Q2

The Q2 is different from previous Qidi generations that used an EMMC module — on this printer, factory reset is done via USB-C.

> Only do this if you need to. A factory reset will wipe all your config changes. Make sure you have backups of `printer.cfg`, `gcode_macros.cfg`, and any other files you've modified before proceeding.

---

## What You'll Need

- A USB-C cable
- **RKDevTool** — download link below
- **Factory firmware image (.img file)** — download link below

Both can be downloaded from here:
[https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr](https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr)

---

## Step 1 — Set RKDevTool to English

RKDevTool installs in Chinese by default. To switch it to English:

1. Open the `config.ini` file in the RKDevTool directory.
2. Find this line:
   ```ini
   Lang1File=Chinese.ini
   ```
3. Change it to:
   ```ini
   Lang1File=English.ini
   ```
4. Save the file and restart RKDevTool.

---

## Step 2 — Access the USB-C Port

The USB-C port is inside the print chamber, behind a small cover on the **left wall**. Remove the two screws to access it.

---

## Step 3 — Flash the Firmware

1. Connect the USB-C cable between your computer and the printer.
2. Open RKDevTool and load the `.img` firmware file.
3. Go to **Upgrade Firmware**.
4. Press **Switch** and let it run.
5. Press **Upgrade**.

> Honestly, I am not 100% certain on the exact steps after loading the image — the tool is a bit unusual. The sequence above is my best recollection of what worked. If you run into trouble, check the Qidi community for more detailed walkthroughs.

---

## Sources

- [https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr](https://drive.google.com/drive/folders/1DSWuDp-SBexRj-f-FzzQahNxTc9IDZGr)
- [https://github.com/qidi-community](https://github.com/qidi-community)
