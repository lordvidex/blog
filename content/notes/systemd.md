---
title: systemd
date: 2022-10-27T07:51:00+03:00
draft: false
weight: 10
summary: managing services and changing default targets
author: Evans Owamoyo
categories:
  - Notes
tags:
  - linux
  - rhel
comments: false
slug: systemd
---
# systemd
- it is a convention for daemon progams to end in the letter **d**

* systemctl command is used to manage units e.g. display available units `systemctl -t help`

## Listing service units
```bash
systemctl list-units --type=service [--all]
systemctl list-unit-files --type=service # + installed but not enabled
```

## Viewing service states
systemctl status <name.type>  
`systemctl status sshd.service`

## Verifying the status of a service
`systemctl is-active sshd.service`  
`... is-enabled ... `  
`... is-failed ...`  

## Service controls
```bash
systemctl start ...
systemctl stop ...
systemctl restart ...
systemctl reload ... # just reloads config files without restart. This does not change pid
systemctl reload-or-restart ... # reloads if possible, otherwise restarts
systemctl list-dependencies ... # displays hierarchy mapping 
```

## Masking and Unmasking
* This prevents a service from starting by crating a symlink to /dev/null.
```bash
systemctl mask ...
systemctl unmask ...
```

## Enabling services to start / stop at boot
* starting / stopping on a running system does not guarantee that the service auto starts/stops when the system reboots.

* GRUB2 - GRand Unified Bootloader v2

## selecting a target to boot at runtime
```bash
# systemctl isolate <target>
# e.g.
systemctl isolate multi-user.target
```

### get and set the default target (persistent)
```bash
systemctl get-default
# systemctl set-default <target> e.g.
systemctl set-default multi-user.target # graphical.target
systemctl reboot # reboots to cli interface instead of gui
```
