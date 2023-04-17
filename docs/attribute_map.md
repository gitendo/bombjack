---
title: "Attribute Map"
date: 2023-04-17T12:10:43+02:00
description: "BG_1 attribute map description."
draft: true
weight: 9
---

Attribute map is strictly related to character map, both are used by BG_1 layer.

<!--more-->

{{< tip >}}
`$9400 - $97FF` (Read/Write)
{{< /tip >}}

The main purpose of attribute map is to provide additional features to character map. While character map entry defines character code to be put on the layer, related attribute map entry is used to apply:
- palette (`0`-`15`) to be used with such character - bits `0` to `3`
- bank: lower (`0`) characters from 0 to 255 or upper (`1`) characters from 256 to 511 - bit `4`
- bits from `5` to `7` seem to be unused

Other than that it has identical features to character map:
- array of 32 columns, 32 rows each
- each entry is 1 byte and corresponds to related character map entry
- first and last two columns remain off screen (Bomb Jack screen resolution is 224*256)
