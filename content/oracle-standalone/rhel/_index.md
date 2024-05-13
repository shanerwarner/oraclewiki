---
title: Oracle 19c Installation on RHEL.
next: Home
---

#### A step-by-step process to installing Oracle 19c Software for Single Instance Database on Red Hat Enterprise Linux 7.9 (RHEL)

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

```bash
vi /etc/ssh/sshd_config
```
or 

```bash
nano /etc/ssh/sshd_config
```

You should see similar output as the following:

`X11Forwarding yes`

### Install the Dependencies

Install the Following

```bash
yum install libnsl* -y
```

```bash
yum update -y
```
Check if Development Tools are installed

``` bash
yum grouplist
```
 If They are not installed, then execute the Following Command
```bash
yum group install "Development Tools"

```
