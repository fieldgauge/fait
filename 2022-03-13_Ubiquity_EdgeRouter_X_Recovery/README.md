---
title: Ubiquity EdgeRouter X ER-X Recovery
---

## Initial situation
A friend offered me a used
[Ubiquity EdgeRouter X ER-X](https://www.ui.com/download/edgemax/edgerouter-x/er-x)
in excellent condition,
except that when powered on, the only sign of life was the `PWR` led.
No other lights, no network activity, so obviously not quite ready for use.

The recommendation was to flash a new firmware via tftp,
which offered a good opportunity to revisit my Linux basic networking skills.

## The idea
https://help.ui.com/hc/en-us/articles/360019289113-EdgeRouter-TFTP-Recovery

https://superuser.com/questions/581812/put-file-with-tftp-client-in-linux

```bash
tftp -v 192.168.1.20 -m binary -c put ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed
```

## Not successful at first attempt
```
$ tftp -v 192.168.1.20 -m binary
mode set to octet
Connected to 192.168.1.20 (192.168.1.20), port 69
tftp> put ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed
putting ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed to 192.168.1.20:ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed [octet]
Transfer timed out.
```
[Power-on reset as described in the quick start guide](https://dl.ui.com/guides/edgemax/EdgeRouter_ER-X_QSG.pdf) is available afterwards, but has no effect.
Accessible neither as `192.168.1.1` on `eth0` nor gets IP via DHCP on `eth1`.

## Multiple cycles of flash, reset, reboot
Same as above, just more of it. Also runtime resets once it becomes available.
```
$ tftp -v 192.168.1.20 -m binary -c put ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed
mode set to octet
Connected to 192.168.1.20 (192.168.1.20), port 69
putting ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed to 192.168.1.20:ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed [octet]
Sent 107022404 bytes in 43.7 seconds [19592193 bit/s]
```

[Runtime reset as described in the quick start guide](https://dl.ui.com/guides/edgemax/EdgeRouter_ER-X_QSG.pdf) is now available, afterwards router gets IP via DHCP on `eth1` and is available via web interface.

## Follow up steps
1. firmware update via web interface, uploading the latest from https://www.ui.com/download/edgemax/edgerouter-x/er-x
1. runtime reset to drop any settings inherited from previous version
1. upgrade boot image via ssh
```
$ ssh ubnt@EdgeRouter-X-5-Port
...
ubnt@edgerouter-x-5-port's password: 
Linux EdgeRouter-X-5-Port 4.14.54-UBNT #1 SMP Tue May 11 13:23:28 UTC 2021 mips
Boot image can be upgraded to version [ e50_002_4c817 ].
Run "add system boot-image" to upgrade boot image.
ubnt@EdgeRouter-X-5-Port:~$ add system boot-image
Uboot version [e50_001_1e49c] is about to be replaced
Warning: Don't turn off the power or reboot during the upgrade!
Are you sure you want to replace old version? (Yes/No) [Yes]: yes
Preparing to upgrade...Done
Copying upgrade boot image...Done
Checking boot version: Current is e50_001_1e49c; new is e50_002_4c817 ...Done
Checking upgrade image...Done
Writing image...Boot image has been upgraded.
Reboot is needed in order to apply changes!
Done
Upgrade boot completed
```

## Final status
https://help.ui.com/hc/en-us/articles/360046967773-EdgeSwitch-CLI-Reference-Guide
```
ubnt@EdgeRouter-X-5-Port:~$ show version
Version:      v2.0.9-hotfix.2
Build ID:     5402463
Build on:     05/11/21 13:17
Copyright:    2012-2020 Ubiquiti Networks, Inc.
HW model:     EdgeRouter X 5-Port
HW S/N:       <masked>
Uptime:       13:05:41 up 11 min,  1 user,  load average: 1.19, 1.03, 0.65
```

Now the router is ready for normal configuration via https://edgerouter-x-5-port, not covered here.
