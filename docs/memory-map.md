---
title: "Memory Map"
date: 2023-03-14T11:46:26+01:00
description: "Bomb Jack memory map overview."
draft: false
weight: 2
---

The following table shows a short overview of Bomb Jack memory map as seen and used by Z80.

<!--more-->

{{< block "spacer" >}}
{{< /block >}}

| Address | Size  | Type | Access | Description               |
|:--------|:------|:-----|:------:|:--------------------------|
| $0000   | $1FFF | ROM  | RO     | 09_j01b.bin               |
| $2000   | $3FFF | ROM  | RO     | 10_101b.bin               |
| $4000   | $5FFF | ROM  | RO     | 11_m01b.bin               |
| $6000   | $7FFF | ROM  | RO     | 12_n01b.bin               |
| $8000   | $0FFF | RAM  | RW     | general purpose RAM       |
| $9000   | $03FF | VRAM | RW     | tile map |
| $9400   | $03FF | VRAM | RW     | tile attribute map |
| $9820   | $005F | VRAM | RW     | sprite / attribute map |
| $9C00   | $00FF | CRAM | WO     | palette data |
| $9E00   | -     | I/O  | WO     | background picture select |
| $B000   | -     | I/O  | RW     | joystick 1 and NMI mask |
| $B001   | -     | I/O  | RO     | joystick 2 |
| $B002   | -     | I/O  | RO     | coins and start button |
| $B003   | -     | I/O  | RW     | watchdog reset |
| $B004   | -     | I/O  | RW     | [dip switch 1 and screen flip](../dip-switch/#ds_1) |
| $B005   | -     | I/O  | RO     | [dip switch 2](../dip-switch/#ds_2) |
| $B006   | -     | I/O  | RW     | sound latch |
| $C000   | $1FFF | ROM  | RO     | 13.1r |