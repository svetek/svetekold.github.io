---
layout: default
title: Domotz agent 
nav_order: 1
nav_exclude: false
has_children: false
parent: Troubleshooting
---
## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
   
## Domotz agent go offline when VPN is activated
Domotz lost DNS settings when VPN is activated, after that on app Domotz Pro Desktop agent go offlie.  
You need login to Domotz agent use ssh client, you can see is in system logs like bellow:  
```yaml
Oct  5 11:45:22 monitor systemd-resolved[683]: message repeated 433 times: [ Got packet on unexpected IP range, refusing.]
Oct  5 11:45:25 monitor systemd[1]: Started Session 3 of user svetek.
Oct  5 11:45:27 monitor systemd-resolved[683]: Got packet on unexpected IP range, refusing.
Oct  5 11:45:34 monitor systemd-resolved[683]: message repeated 18 times: [ Got packet on unexpected IP range, refusing.]
Oct  5 11:45:37 monitor systemd-resolved[683]: Got packet on unexpected IP range, refusing.
Oct  5 11:45:41 monitor systemd-resolved[683]: message repeated 40 times: [ Got packet on unexpected IP range, refusing.]
Oct  5 11:45:42 monitor systemd-resolved[683]: Got packet on unexpected IP range, refusing.
Oct  5 11:45:45 monitor systemd-resolved[683]: Got packet on unexpected IP range, refusing.
Oct  5 11:45:55 monitor systemd-resolved[683]: message repeated 52 times: [ Got packet on unexpected IP range, refusing.]
Oct  5 11:45:57 monitor systemd-resolved[683]: Got packet on unexpected IP range, refusing.
```
For fix that issue need run two commands:  
```bash
mv /etc/resolv.conf /etc/resolv.conf.old
ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```
