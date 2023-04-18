---
title: "Object Attribute Memory"
date: 2023-04-17T18:54:39+02:00
description: "Object attribute map overview."
draft: true
weight: 10
---

Object attribute memory holds the objects to be displayed (also reffered as sprites) and their attributes. It creates OBJ - last of the three layers of graphics.

<!--more-->

{{< tip >}}
`$9820 - $987F` (Write Only)

:bulb:
OAM mirror is stored in RAM at `$8500` where it's being freely manipulated. OAM update is done during VBLANK - this prevents glitches and is commonly used method. During update objects are sorted, 32\*32 ones are stored from the beginning of OAM and 16\*16 from the end. Finally `$9A00` register is updated with value equal to sum of 32\*32 objects + 3 (refer to routine at `$0958`).
{{< /tip >}}

The hardware allows 24 independent objects 16\*16 or 32\*32 pixels each with dedicated palette (0-15). OAM effectively creates 3rd layer of graphic which has the highest priority - OBJ is displayed over BG_1 which is displayed over BG_0. Each each of 24 entries in the map takes 4 bytes with the following meaning:


| Bits  | Description                                                     |
|:------|:----------------------------------------------------------------|
| 0 - 6 | object id (0-127 in case of 16\*16 and 0-31 for 32\*32 objects) |
| 7     | toggles between 16\*16 (0) and 32\*32 (1) objects               |
|       | &nbsp;                                                          |
| 0 - 3 | object palette id (0-15)                                        |
| 4     | seems unused                                                    |
| 5     | not used by hardware, game uses it to sort objects by size which hardly makes any sense |
| 6     | flips object vertically                                         |
| 7     | flips object horizontally                                       |
|       | &nbsp;                                                          |
| 0 - 7 | object x coordinate                                             |
|       | &nbsp;                                                          |
| 0 - 7 | object y coordinate                                             |
