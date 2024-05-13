---
title: Oracle 19c Installation on RHEL.
next: Home
---

## A step-by-step process to installing Oracle 19c Software for Single Instance Database on Red Hat Enterprise Linux 7.9 (RHEL)

{{< callout type="info" >}}
  This Guide is Production Ready.
{{< /callout >}}


# Prerequisites

## Install required X11 packages

Install X11 packages with following command based on your operating system release and version:

```bash
yum install xorg-x11-xauth -y
```

## Configure X11 forwarding

To enable X11 Forwarding, change the "X11Forwarding" parameter using vi or nano editor to ```yes``` in the ```/etc/ssh/sshd_config``` file if either commented out or set to no.

