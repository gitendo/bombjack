---
title: "Dip Switch Settings"
date: 2023-03-16T21:09:42+01:00
description: "Bomb Jack dip switch settings."
draft: false
weight: 3
---

Bomb Jack PCB has two dip switches to configure game settings. How the game accesses and makes use of them is explained right here.

<!--more-->

{{< block "spacer" >}}
{{< /block >}}

## DS_1
{{< tip >}}
`$B004` (Read/Write)

DS_1 value can be read from `$B004`. It's being mirrored in RAM at `$8013` and used accordingly. Factory DS_1 settings are: `%11000000` which translates to demo sound on and upright mode.

:bulb: While `$B004` seems like read only it actually is writable. Function at `$4587` checks for two player game and when the mode is set to cocktail it writes `1` to flip the screen for player two and `0` back for player one.
{{< /tip >}}

{{< block "grid-3" >}}

{{< column >}}
{{< block "fake_th" >}}
Demo sound
{{< /block >}}
|            |               |
|:-----------|--------------:|
| Off        | %**0**0000000 |
| On         | %**1**0000000 |
{{< /column >}}

{{< column >}}
{{< block "fake_th" >}}
Mode
{{< /block >}}
|            |               |
|:-----------|--------------:|
| Cocktail   | %0**0**000000 |
| Upright    | %0**1**000000 |
{{< /column >}}

{{< column >}}
{{< block "fake_th" >}}
Lives
{{< /block >}}
|            |               |
|:-----------|--------------:|
| 3          | %00**00**0000 |
| 4          | %00**01**0000 |
| 5          | %00**10**0000 |
| 2          | %00**11**0000 |
{{< /column >}}

{{< /block >}}

{{< block "grid-2" >}}

{{< column >}}
{{< block "fake_th" >}}
Coin 2
{{< /block >}}
|                    |               |
|:-------------------|--------------:|
| 1 Coin - 1 Credit  | %0000**00**00 |
| 2 Coins - 1 Credit | %0000**01**00 |
| 1 Coin - 2 Credits | %0000**10**00 |
| 1 Coin - 3 Credits | %0000**11**00 |
{{< /column >}}

{{< column >}}
{{< block "fake_th" >}}
Coin 1
{{< /block >}}
|                    |               |
|:-------------------|--------------:|
| 1 Coin - 1 Credit  | %000000**00** |
| 1 Coin - 2 Credits | %000000**01** |
| 1 Coin - 3 Credits | %000000**10** |
| 1 Coin - 6 Credits | %000000**11** |
{{< /column >}}

{{< /block >}}

{{< block "spacer" >}}
{{< /block >}}

## DS_2

{{< tip >}}
`$B005` (Read Only)

DS_2 value can be read from `$B005`. It's being mirrored in RAM at `$8014` and used accordingly. Factory defaults for DS_2 are: `%01010000` which translates to high special coin ratio, hard difficulty and bird moving fast.

:bulb: On the contrary to DS_1 it is read only however it holds its own secret. According to manual, first three bits / switches are unused and should be turned off. But if you enable function at `$5A07` by replacing `$C9 (ret)` with `$F5 (push af)` you'll get access to unused "Extra lives" setting.
{{< /tip >}}

{{< block "grid-3" >}}

{{< column >}}

{{< block "fake_th" >}}
Special coin appearance ratio
{{< /block >}}
|            |               |
|:-----------|--------------:|
| High       | %**0**0000000 |
| Low        | %**1**0000000 |
{{< /column >}}

{{< column >}}
{{< block "fake_th" >}}
Enemy numbers and speed
{{< /block >}}
|         |               |
|:--------|--------------:|
| Easy    | %0**01**00000 |
| Medium  | %0**00**00000 |
| Hard    | %0**10**00000 |
| Hardest | %0**11**00000 |

{{< block "fake_th" >}}
Extra lives
{{< /block >}}
|                    |               |
|:-------------------|--------------:|
| None               | %00000**000** |
| 100K Only          | %00000**100** |
| 100K and 300K      | %00000**110** |
| 50K Only           | %00000**011** |
| 50K and 100K       | %00000**101** |
| 50K, 100K and 300K | %00000**111** |
| Every 100K         | %00000**001** |
| Every 30K          | %00000**010** |

{{< /column >}}

{{< column >}}
{{< block "fake_th" >}}
Bird speed
{{< /block >}}
|         |               |
|:--------|--------------:|
| Slow    | %000**00**000 |
| Normal  | %000**01**000 |
| Fast    | %000**10**000 |
| Fastest | %000**11**000 |
{{< /column >}}

{{< /block >}}
