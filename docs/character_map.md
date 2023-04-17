---
title: "Character Map"
date: 2023-04-17T09:50:57+02:00
description: "BG_1 character map description."
draft: true
weight: 8
---

Bomb Jack effectively uses 3 layers of graphic: BG_0, BG_1 and OBJ. BG_1 is explained here.

<!--more-->

{{< tip >}}
`$9000 - $93FF` (Read/Write)

:bulb: Columns are being organized from right to left. First visible column starts at `$93A0` and ends at `$93BF`. Respectively, last visible column starts at `$9040` and ends at `$905F`. These are also addresses for top left, bottom left, top right and bottom right characters visible on the layer.
{{< /tip >}}

BG_1 is used for text, bottom part of title screen logo, HUD, stage borders, platforms and bombs. Main features are:
- array of 32 columns, 32 rows each
- each entry is 1 byte and represents character code (8x8 pixel block also called tile) from bank 0
- up to 512 unique characters is possible thanks to 5th bit of corresponding attribute map entry
- first and last two columns remain off screen (Bomb Jack screen resolution is 224*256)


