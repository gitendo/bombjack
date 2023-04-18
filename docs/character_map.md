---
title: "Character Map"
date: 2023-04-17T09:50:57+02:00
description: "BG_1 character map description."
draft: true
weight: 8
---

Character map combined with [attribute map](../attribute_map/) creates BG_1 - second of the three layers of graphics.

<!--more-->

{{< tip >}}
`$9000 - $93FF` (Read/Write)

:bulb: Columns are being stored in RAM from right to left. First visible column starts at `$93A0` and ends at `$93BF`. Last visible column starts at `$9040` and ends at `$905F` respectively. These are also addresses for top left, bottom left, top right and bottom right characters visible on the layer.
{{< /tip >}}

BG_1 is used for text, bottom part of title screen logo, HUD, stage borders, platforms and bombs. It has priority above BG_0 but below [OBJ](../object_memory/). Main features are:
- array of 32 columns, 32 rows each
- each entry is 1 byte and represents 1 character code (8x8 pixel block also called tile) from bank 0
- up to 512 unique characters is possible thanks to corresponding [attribute map](../attribute_map/) entry
- first and last two columns remain off screen (screen resolution is 224*256)
