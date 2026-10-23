# M350 Metric and Inch Units — Quick User Guide

Applies to software version **2026-10-23-05**, for both **V1 and V2**.

**Parameter 299 controls the units used by the interface and manual input. G20/G21 control the units used by the machining program. These settings are independent.**

## 1. Interface and manual input: Parameter 299

1. Open System Parameters and find parameter **299 (Length Unit Selection)**.
2. Set it to **0** for metric units (**mm**) or **1** for inch units (**in**).
3. Confirm and save the setting, then **restart the system**. The new units take effect only after restarting. Until then, the display and input continue to use the previous units.
4. After restarting, use the units shown on the interface when reading coordinates, entering length parameters, and entering a custom jog distance.

For example, a custom XYZ jog distance of `0.01` means **0.01 mm** in metric mode or **0.01 in (0.254 mm)** in inch mode.

Changing the display units preserves the physical lengths represented by existing parameters. You do not need to convert and re-enter all existing parameters.

## 2. Machining programs: G20 and G21

- **G20:** Interpret program lengths in inches.
- **G21:** Interpret program lengths in millimeters.

G20/G21 take effect from the block in which they appear, including XYZ dimensions in that same block. The selected units remain active for subsequent blocks until changed again. No system restart is required.

A newly initialized program defaults to G21. Explicitly specify G20 or G21 at the beginning of every program, and specify the units when entering code manually.

Changing parameter 299 does not automatically convert a millimeter program into an inch program. G20/G21 do not change the interface units.

## 3. Common combinations

| Parameter 299 (after restart) | Program command | Interface units | Program units |
| --- | --- | --- | --- |
| 0 | G21 | Millimeters | Millimeters |
| 0 | G20 | Millimeters | Inches |
| 1 | G21 | Inches | Millimeters |
| 1 | G20 | Inches | Inches |

## 4. Program examples

The following examples are equivalent when using the same work coordinate system and **G90 absolute programming**.

### Metric example

```gcode
G21 G90
G01 X25.4 F254
```

### Inch example

```gcode
G20 G90
G01 X1.0 F10
```

Both examples specify an X target position of **25.4 mm** and a feed rate of **254 mm/min**: **1.0 in = 25.4 mm**, and **10 in/min = 254 mm/min**.

These examples only illustrate unit handling. For actual machining, prepare a complete program suited to the work coordinate system and machining requirements.

## 5. Usage notes

1. Metric/inch conversion in this version applies to **XYZ only**. The fourth and fifth axes retain their original units, even when configured as linear axes.
2. For normal XYZ feed moves, a newly specified F value is interpreted as **mm/min under G21** or **in/min under G20**. When moving only the fourth or fifth axis, feed uses that axis's original units.
3. After switching between G20 and G21, specify the F value again. If no new F value is specified, the previous physical feed rate is retained.
4. Spindle speed, time, identification numbers, and similar values are unaffected by length unit changes. Macros that directly read or write system parameters continue to use the original units; do not directly substitute values converted for display on the interface.
5. Before starting machining from the middle of a program, confirm that the active program units match the code at the starting block.

## Documentation updates

Future documentation will be published at [https://docs.foinnccnc.com/](https://docs.foinnccnc.com/).
