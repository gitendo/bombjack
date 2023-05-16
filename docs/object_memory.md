---
title: "Object Attribute Memory"
date: 2023-04-17T18:54:39+02:00
description: "Object attribute map overview."
draft: false
weight: 10
---

Object attribute memory holds the information about objects (also reffered as sprites) to be displayed. It creates last of the three layers of graphics.

<!--more-->

{{< tip >}}
`$9820 - $987F` (Write Only)

:bulb:
OAM mirror is stored in RAM at `$8500` where it's being freely manipulated. OAM update is done during VBLANK - this prevents glitches and is commonly used method. 
{{< /tip >}}

Objects are stored in character format but can be moved independently and displayed anywhere on the screen. 
They have the highest priority and are displayed over BG_1 which is displayed over BG_0.
Hardware supports two object sizes: 16\*16 and 32\*32 pixels.
Each object has its own palette applied and might be flipped horizontally or vertically.
Obviously, there's display and storage limit:

- as for display, it's [384][1] pixels per scan line which translates to 24 objects (16\*16 pixels) or 12 objects (32\*32 pixels).
It's possible to have objects of mixed sizes but the limit still applies (consider one 32\*32 as two 16\*16 objects to make things easier).
Multiplexing also works (this has been already [proven][2] by Martin Piper) although there's no register that provides information about current scan line being processed.

- there're 1024 characters to be used for storage of object graphics. One 16\*16 object uses four of them and 32\*32 uses sixteen. This gives room to 256 objects of 16\*16
size or 64 objects of 32\*32 size. Different sizes could be stored interchangeably but this requires some planning. Object character data is seen by the hardware as 2 banks
which affects directly how they're bing selected.

{{< block "grid-2 mt-1" >}}

{{< column "text-justify">}}

To create an object there must be valid entry in OAM - please refer to the table on the right. The order is related to display priority: low to high.

As you can tell, 16\*16 object entries must be aligned to 4 byte boundary while 32\*32 objects to 8 byte boundary.
In the latter case each OAM entry should be followed by four zero bytes. 

Since OAM entry doesn't contain any information about object size another thing specific to 32\*32 objects is usage of [size select registers](../object_select/). 
This is explained in details in it's own chapter.

{{< /column >}}

{{< column >}}

| OAM Address | 16\*16 OBJ | 32\*32 OBJ |
|:--------|:-----------|:-----------|
| $9820   |  1 |  1 |
| $9824   |  2 |    |
| $9828   |  3 |  2 |
| $982C   |  4 |    |
| $9830   |  5 |  3 |
| $9834   |  6 |    |
|   -     |  - |  - |
| $9878   | 23 | 12 |
| $987C   | 24 |    |

{{< /column >}}

{{< /block >}}


Finally here's the entry itself. It consists of 4 bytes with their bits having the following meaning:

| Byte | Bits  | Description                                                                  |
|:-----|:------|:-----------------------------------------------------------------------------|
| 1    | 7     | toggles between lower (0) and upper (1) bank where object graphics is stored |
|      | 6 - 0 | object id (0-127) in case of 16\*16 and (0-31) for 32\*32 objects            |
|      |       | &nbsp;                                                                       |
| 2    | 7     | flips object horizontally                                                    |
|      | 6     | flips object vertically                                                      |
|      | 5     | not used by hardware (used by software to tag 32\*32 objects)                |
|      | 4     | not used by hardware                                                         |
|      | 3 - 0 | object palette id (0-15)                                                     |
|      |       | &nbsp;                                                                       |
| 3    | 7 - 0 | object x coordinate                                                          |
|      |       | &nbsp;                                                                       |
| 4    | 7 - 0 | object y coordinate                                                          |


[1]: https://github.com/martinpiper/BombJack/issues/6#issuecomment-1521667462
[2]: https://youtu.be/vDuSqMVrGko?t=187
