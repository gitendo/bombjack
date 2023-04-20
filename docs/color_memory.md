---
title: "Color Palette Memory"
date: 2023-04-19T14:00:28+02:00
description: "Color palette memory overview."
draft: true
weight: 11
---

Color palette memory holds user defined palettes. They can be applied to characters and objects used to create graphics layers.

<!--more-->

{{< tip >}}
`$9C00 - $9CFF` (Write Only)

:bulb: CRAM has its mirror located at `$8C00` where it's freely modified. Just like with [OAM](../object_memory/), update is recommended during VBLANK to prevent any glitches.
{{< /tip >}}

{{< block "grid-2" >}}

{{< column >}}
Color palette memory is organized into 16 slots.

Each palette occupies one slot. It has 8 colors and each color entry takes 2 bytes, being represented by 12-bit BGR value. Unused bits are not set. 
{{< /column >}}

{{< column >}}
| Address | Description |
|:--------|:------------|
| $9C00   | palette $00 |
| $9C10   | palette $01 |
| . . .   | . . . . . . |
| $9CF0   | palette $0F |
{{< /column >}}

{{< /block >}}

Color 0 of each palette is displayed as transparent for BG_1 and OBJ - they effectively use one color less than BG_0.

Palettes are independent from graphics data which means any object or character can be assigned any palette and have it modified any time.

Couple examples:
- white (R:255, G:255, B:255) equals to `$0FFF`
- orange (R:255, G:119, B:00) equals to `$007F`.
- blue (R:0, G:170, B:255) equals to `$0FA0`.
