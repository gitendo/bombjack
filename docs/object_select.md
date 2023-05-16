---
title: "Object Size Select"
date: 2023-04-20T18:17:35+02:00
description: "Object size select overview."
draft: false
weight: 11
---

Size select registers define how many and which entries in [OAM][1] are displayed as 32\*32 objects.

:warning:  The following information is partly based on the research done by [Martin Piper][2] and it's not properly emulated by MAME as of the time of writing.

<!--more-->

{{< tip >}}
`$9A00` (Write Only)  
`$9A01` (Write Only)

:bulb: Both registers accept values from 0 to 15 however values from 0 to 3 do not provide any result.  
During initial setup memory from `$9A00` to `$9AC8` is zero filled by code at `$015F`. It's currently unknown why such large block is used.
{{< /tip >}}

Even though single [OAM][1] entry contains a lot of information it doesn't specify the object size.
All entries are assumed to be 16\*16 objects by default and no additional action is required.
To display one or more of 32\*32 objects the forementioned registers are used. Four combinations are possible:


| Mode                                | Requirements              |
|:------------------------------------|:--------------|
| 16\*16 objects only                 | Set both registers to `$00` value. This causes every [OAM][1] entry out of 24 available to be displayed as 16\*16. |
| 32\*32 objects only                 | Set `$9A00` to `$0F` and `$9A01` to `$00`. Every [OAM][1] entry out of 12 possible is displayed as 32\*32. |
| 16\*16 and 32\*32 objects (ordered) | Set `$9A00` register with number of 32\*32 objects increased by 3. Write all 32\*32 entries before 16\*16 ones during [OAM][1] update. If the number of 32\*32 objects changes dynamically also update `$9A00` register. This is roughly what game code does at `$0958`.
| 16\*16 and 32\*32 objects (mixed)   | Use `$9A00` as *start* and `$9A01` as *end* parameter. *start* points to the first [OAM][1] entry to be displayed as 32\*32 object while *end* points to entry following the last one to be displayed as such. Both parameters must be in 1-12 range and then increased by 3 before writing.

While first two modes are straightforward let's review the original code located at `$0958`:
```
oam_update:
&nbsp;	ld      ix, $9878	; VRAM: OAM last entry (dst for 16*16 obj)
&nbsp;	ld      iy, $9820	; VRAM: OAM first entry (dst for 32*32 obj)
&nbsp;	ld      hl, $8500	; RAM: OAM mirror (src)
&nbsp;	ld      b, $0C          ; 12 objects * 8 (4 byte entry followed by 4 zeroes ) = OAM size
&nbsp;	ld      c, 3		; 32*32 obj counter (starts from 3 because values 0-3 have no effect on $9A00)

_next:
&nbsp;	ld      a, b		; all 12 objects processed?
&nbsp;	cp      0
&nbsp;	jr      z, _done        ; yes
&nbsp;	inc     hl              ; skip 1st byte of OAM entry (obj id)
&nbsp;	ld      a, (hl)         ; get 2nd byte (obj attribute)
&nbsp;	dec     hl              ; move back to 1st byte (obj id)
&nbsp;	bit     5, a            ; it's 32*32 object if 5th bit is set
&nbsp;	jr      nz, _obj32
_obj16:				; 16*16
&nbsp;	call    $0982		; de = ix, copy 8 bytes from (hl) to (de), ix - 8, b - 1
&nbsp;	jr      _next

_obj32:				; 32*32
&nbsp;	call    $0996		; de = iy, copy 8 bytes from (hl) to (de), iy + 8, b - 1, c + 1
&nbsp;	jr      _next

_done:				; sorting & update finished
&nbsp;	ld      a, c		; 32*32 object count
&nbsp;	ld      ($9A00), a      ; tell the hardware how many 32*32 objects are in OAM, starting from 1st entry
&nbsp;	ret
```

This approach simply limits the total number of objects being used in game to 12, avoiding the trouble of using `$9A01` register. 
Each [OAM][1] entry is aligned to 8 byte boundary. Additional 4 bytes are usually zero filled (empty entry) however backup of x/y coordinates related to given
entry might be found sometimes there. This has no visible effect anyway. 32\*32 entries are stored (forward) from the beginning and 16\*16 entries (backward) from the end of [OAM][1].
At exit `$9A00` register is updated with value equal to number of 32\*32 entries increased by 3.

Using mixed method is somewhat trickier to understand. It doesn't require 32\*32 entries to be stored at the beginning of the OAM, they must form continuous
block nonetheless. Single 16\*16 and 32\*32 entries can't be stored interchangeably - please refer to examples below. While it might seem confusing the whole *start* / *end* assignment
is only for explanation purpose. As long as both values are right it doesn't matter which register is loaded with which value, because result is the same. Only equal values have no effect.
All possible register combinations have been tested by [Martin Piper][3] and can be viewed [here][4].


{{< block "grid-3" >}}

{{< column >}}
`(1,2) $9A00 = $04, $9A01 = $05`
| OAM Address | 16\*16 OBJ | 32\*32 OBJ |
|:------------|:-----------|:-----------|
| $9820       |  0         |            |
| $9824       |  1         |            |
| $9828       |            |  1         |
| $982C       |            |            |
| $9830       |  4         |            |
| $9834       |  5         |            |
| $9838       |  6         |            |
| $983C       |  7         |            |
| $9840       |  8         |            |
| $9844       |  9         |            |
| $9848       | 10         |            |
| $984C       | 11         |            |
| $9850       | 12         |            |
| $9854       | 13         |            |
| $9858       | 14         |            |
| $985C       | 15         |            |
| $9860       | 16         |            |
| $9864       | 17         |            |
| $9868       | 18         |            |
| $986C       | 19         |            |
| $9870       | 20         |            |
| $9874       | 21         |            |
| $9878       | 22         |            |
| $987C       | 23         |            |
{{< /column >}}

{{< column >}}
`(9,3) $9A00 = $0C, $9A01 = $06`
| OAM Address | 16\*16 OBJ | 32\*32 OBJ |
|:--------|:-----------|:-----------|
| $9820   |  0 |    |
| $9824   |  1 |    |
| $9828   |  2 |    |
| $982C   |  3 |    |
| $9830   |  4 |    |
| $9834   |  5 |    |
| $9838   |    |  3 |
| $983C   |    |    |
| $9840   |    |  4 |
| $9844   |    |    |
| $9848   |    |  5 |
| $984C   |    |    |
| $9850   |    |  6 |
| $9854   |    |    |
| $9858   |    |  7 |
| $985C   |    |    |
| $9860   |    |  8 |
| $9864   |    |    |
| $9868   | 18 |    |
| $986C   | 19 |    |
| $9870   | 20 |    |
| $9874   | 21 |    |
| $9878   | 22 |    |
| $987C   | 23 |    |
{{< /column >}}

{{< column >}}
`(4,12) $9A00=$07, $9A01=$0F`
| OAM Address | 16\*16 OBJ | 32\*32 OBJ |
|:--------|:-----------|:-----------|
| $9820   |  0 |    |
| $9824   |  1 |    |
| $9828   |  2 |    |
| $982C   |  3 |    |
| $9830   |  4 |    |
| $9834   |  5 |    |
| $9838   |  6 |    |
| $983C   |  7 |    |
| $9840   |    |  4 |
| $9844   |    |    |
| $9848   |    |  5 |
| $984C   |    |    |
| $9850   |    |  6 |
| $9854   |    |    |
| $9858   |    |  7 |
| $985C   |    |    |
| $9860   |    |  8 |
| $9864   |    |    |
| $9868   |    |  9 |
| $986C   |    |    |
| $9870   |    | 10 |
| $9874   |    |    |
| $9878   |    | 11 |
| $987C   |    |    |

{{< /column >}}

{{< /block >}}

This makes it quite flexible solution which can be used in a few different ways. As in previous cases it is recommended to update [OAM][1] first, then registers if there were
any changes in 32\*32 objects number.


[1]: ../object_memory/
[2]: https://github.com/martinpiper/BombJack/
[3]: https://github.com/martinpiper/
[4]: https://github.com/martinpiper/BombJack/blob/master/output/V2.0%20-%20Sprite%20size%20debug.gif
