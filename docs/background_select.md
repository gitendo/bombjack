---
title: "Background Picture Select"
date: 2023-05-20T14:54:02+02:00
description: "BG_0 character, attribute map and selection overview."
draft: true
weight: 13
---

Background picture select register simplifies setting of character and attribute maps for BG_0.

<!--more-->

{{< tip >}}
`$9E00` (Write Only)

:bulb: Value to be written should have 4th bit set to change the background. It's currently unknown if 3rd bit is also used by the hardware.
{{< /tip >}}

BG_0 and BG_1 share the common concept of [character][1] and [attribute maps][2]. They're both handled in a different way though.
BG_0 holds up to 256 characters, 16\*16 pixels each. Character data is divided into 3 bitplanes and stored in the following files:  
`06_l08t.bin`  
`07_n08t.bin`  
`08_r08t.bin`  

Game utilizes 8 backgrounds and the same amount of [character maps][1] is stored in `02_p04t.bin` file. Each [character map][1] is followed by related [attribute map][2], they're
256 bytes each. The order of background maps is as follows:

{{< block "grid-2" >}}

{{< column >}}
| ID  | Offset | Background     | 
|----:|-------:|---------------:|
| $00 | $0000  | Rome           |
| $01 | $0200  | California     |
| $02 | $0400  | Egypt          |
| $03 | $0600  | Germany        |
| $04 | $0800  | New York       |
| $0D | $0A00  | Empty          |
| $0E | $0C00  | Bonus (gold)   |
| $0F | $0E00  | Bonus (silver) |
{{< /column >}}

{{< column "text-justify">}}
The IDs are used in game data structures and code. Shift of the last three (`$0D-$08=$05`) is made to distinguish these maps by the procedure at `$6326`. They require just one palette 
slot while others need two, differently located. This seems to be code not hardware dependent but needs futher analysis.
{{< /column >}}

{{< /block >}}

All attribute maps use just two color palette slots: `$08` and `$0A` which restricts background pictures to 16 colors. Only horizontal fliping is possible - attribute's 7th bit is used
for such purpose. The process of setting chosen background is as simple as writing palette data to correct palette slot(s) and then use one of eight possible IDs. Before writing
to register 4th bit of the ID must be set. Everything else is taken care of by hardware itself. Please refer to original code, located at `$639A`:

```
set_bg_picture:
&nbsp;	ld      a, ($8985)  	; check flag (update bg picture and palette)
&nbsp;	or      a
&nbsp;	ret     z               ; quit
&nbsp;	ld      a, ($8986)	; get background picture id
&nbsp;	cp      $10		; leftover code?
&nbsp;	jp      z, _skip    	; 
&nbsp;	push    af		; keep id
&nbsp;	ld      hl, $8C70       ; RAM: palette data src
&nbsp;	ld      de, $9C70       ; CRAM: palette data dst
&nbsp;	ld      bc, $40         ; 4 palettes (bonus box, bg picture bottom half, platforms and border, bg picture top half)
&nbsp;	ldir                    ; copy from (hl) to (de)
&nbsp;	pop     af		; restore id

_skip:
&nbsp;	set     4, a            ; 
&nbsp;	ld      ($9E00), a  	; background select register (write only)
&nbsp;	ld      a, 0            ; update finished
&nbsp;	ld      ($8985), a	; reset flag
&nbsp;	ret
```


While definately convenient, this unfortunately comes at a price of static and limited backgrounds - it's not possible to modify BG_0 just like BG_1. There's no vertical flipping
which could help to conserve character data memory.

[1]: ../character_map/
[2]: ../attribute_map/
