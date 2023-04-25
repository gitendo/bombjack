---
title: "Object Counter"
date: 2023-04-20T18:17:35+02:00
description: "Object counter overview."
draft: true
weight: 11
---

:warning: This needs to be verified someday unless you can provide me with better explanation.

Object counter is updated every frame with number of 32\*32 objects present in [OAM](../object_memory/).

<!--more-->

{{< tip >}}
`$9A00` (Write Only)
{{< /tip >}}

During [OAM](../object_memory/) update routine at `$0958` sorts and counts 32\*32 objects. Register `C` is used for such purpose. This is done by checking 2nd byte of every entry to see if 5th bit is set. Result is being written to `$9A00` upon routine exit. This would make perfect sense if not the fact that `C` is set to 3 before loop. So basically final result is equal to number of 32\*32 objects in [OAM](../object_memory/) + 3. If there's no 32\*32 objects then 3 is written.
