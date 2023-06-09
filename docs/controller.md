---
title: "Controller Inputs and NMI"
date: 2023-06-06T13:56:23+02:00
description: "Joystick, buttons, coin acceptor state and NMI."
draft: true
weight: 13
---

Use the following ports to read coin acceptor, joystick and button state.

<!--more-->

{{< tip >}}
`$B000` (Read) - Joystick 1  
`$B001` (Read) - Joystick 2  
`$B002` (Read) - Other  
{{< /tip >}}


{{< block "grid-3" >}}

{{< column >}}
{{< block "fake_th" >}}
Joystick 1
{{< /block >}}
|       |               |
|:------|--------------:|
| Right | %0000000**1** |
| Left  | %000000**1**0 |
| Up    | %00000**1**00 |
| Down  | %0000**1**000 |
| Jump  | %000**1**0000 |
{{< /column >}}

{{< column >}}
{{< block "fake_th" >}}
Joystick 2
{{< /block >}}
|       |               |
|:------|--------------:|
| Right | %0000000**1** |
| Left  | %000000**1**0 |
| Up    | %00000**1**00 |
| Down  | %0000**1**000 |
| Jump  | %000**1**0000 |
{{< /column >}}

{{< column >}}
{{< block "fake_th" >}}
Other
{{< /block >}}
|          |               |
|:---------|--------------:|
| P1 Coin  | %0000000**1** |
| P2 Coin  | %000000**1**0 |
| P1 Start | %00000**1**00 |
| P2 Start | %0000**1**000 |
{{< /column >}}

{{< /block >}}

Joystick 1 port is also used to enable / disable Non Maskable Interrupt.

:warning: This is just a guess and has not been verified at the moment of writing.

{{< tip >}}
`$B000` (Write) - NMI On / Off
{{< /tip >}}

When NMI happens Z80 starts executing code at `$0066` address which writes `0` to the register and jumps to VBLANK procedure. At VBLANK exit it is written with `1` value.
This seems to be solution to disable / enable NMI from code. As result another NMI is prevented from happening while VBLANK procedure is being processed. 
