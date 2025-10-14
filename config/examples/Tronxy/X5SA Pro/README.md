# Table of Contents

- [Configure Marlin for Your Printer](#configure-marlin-for-your-printer)
  - [Other Marlin Config](#other-marlin-config)
- [Flash Marlin Using SD Card](#flash-marlin-using-sd-card)
- [Known Issues](#known-issues)
- [Suggested Printing Workflow](#suggested-printing-workflow)
- [Bed Tramming and Z Offset](#bed-tramming-and-z-offset)
- [Auto Bed Levelling](#auto-bed-levelling)

---

## See X5SA Readme

For general information about the X5SA printer and its predecessor, see the
[X5SA Readme on GitHub](//github.com/MarlinFirmware/Configurations/tree/import-2.1.x/config/examples/Tronxy/X5SA).

---

## Configure Marlin for Your Printer

These configurations arre designed to use the `chitu_f103` environment in PlatformIO for the CXY‑V6‑191017 Chitu V6 board. If you have another board version in your printer, please update your configuration to suit, and tell the community what needed to change and what board you have.


### Other Marlin Config

Feel free to tweak these configuration files for your own setup.

---

## Flash Marlin Using SD Card

You can now update Marlin directly from an SD card.

1. Compile Marlin with the settings above. The build output will be `YOUR-MARLIN-DIR/.pio/build/chitu_f103/update.cbd`.
2. Power off the printer.
3. Copy `update.cbd` to an SD card and insert it.
4. Turn the printer on. You'll hear a series of beeps, then Marlin will begin the update.

That's all—no need to open the case or use a programmer.

If you previously flashed Marlin the old way, restore your Chitu backup before using this method; it will simplify future updates.


---

## Known Issues

The pull request [28059](//github.com/MarlinFirmware/Marlin/pull/28059) has only recently merged. If you are using Marlin version 2.1.3-beta3 or older, you must manually override the Z‑stop pin in `pins_CHITU3D_V6.h` because the CXY‑V6‑191017 board uses PG9 instead of PA14.

In `pins_CHITU3D_V6.h` replace:

```cpp
#define Z_STOP_PIN PA14
```

with:

```cpp
#ifndef Z_STOP_PIN
  #define Z_STOP_PIN PA14
#endif
```
---

## Bed Tramming and Z Offset

1. Preheat the bed and nozzle.
2. Run “Probe and Level → Tramming Wizard” from the menu:
   - Measure the front‑left corner and confirm a 0.0 reading.
   - Repeat for the remaining corners, adjusting the bed screws until the readings are within –0.05mm to +0.05mm.
3. Use “Probe and Level → Z Probe Wizard” to set the zero height.
4. Save the values to EEPROM with “Configuration → Store Settings.” (You may need to confirm the Z‑offset babysteps and save again.)


---

## Auto Bed Levelling

Because the bed support structure can wobble, These configs are designed to run the UBL automatic bed probing for every print. This is why the UBL mesh is limited to a 3×3 grid to save time.

Replace your slicer's G28 command in your machine‑start G‑code with:

   ```text
   G28 ; home all axes and clear ABL map
   G29 P1 ; Probe the bed
   G29 A F10.0 ; activate UBL and set fade height to 10mm
   ```

   (Do not save the mesh; the support structure's wobble makes it unreliable.)

Enjoy a smoother printing experience!
