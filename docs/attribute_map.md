---
title: "Attribute Map"
date: 2023-04-17T12:10:43+02:00
description: "BG_1 attribute map description."
draft: true
weight: 9
---

Attribute map is strictly related to [character map](../character_map/), both are used to create BG_1 layer.

<!--more-->

{{< tip >}}
`$9400 - $97FF` (Read/Write)

:bulb: Columns are being stored in RAM from right to left. First visible column starts at `$97A0` and ends at `$97BF`. Last visible column starts at `$9440` and ends at `$945F` respectively. These are also addresses for top left, bottom left, top right and bottom right attributes of characters visible on the layer.
{{< /tip >}}

Main purpose of attribute map is to provide additional features to [character map](../character_map/). While [character map](../character_map/) entry defines character (tile) code to be put on the BG_1 layer, related attribute map entry does the following:
- bits `0` to `3` define palette (`0`-`15`) to be used with such character
- bit `4` selects character slot to choose from: (`0`) lower (characters from 0 to 255) or (`1`) upper (characters from 256 to 511)
- bits from `5` to `7` seem to be unused

Other than that it has features identical to [character map](../character_map/):
- array of 32 columns, 32 rows each
- each entry is 1 byte and corresponds to related [character map](../character_map/) entry
- first and last two columns remain off screen (screen resolution is 224*256)
