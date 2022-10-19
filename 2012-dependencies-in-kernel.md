#### Manipulating kernel modules

```bash
/lib/modules -->  location of installed and available modules
uname -r --> viewing version of current kernel
insmod --> installing kernel modules with absolute path
rmmod --> removing kernel modules
modprobe --> managing kernel modules with dependencies
depmod --> crates and updates modules.dep in /lib/modules/
modinfo --> information about a module
```

Use `lsmod` to check installed modules:

```bash
lsmod


Module                  Size  Used by
xt_connmark            16384  2
xt_mark                16384  1
iptable_mangle         16384  1
xt_comment             16384  3
iptable_raw            16384  1
wireguard              94208  0
curve25519_x86_64      49152  1 wireguard
nf_conntrack          172032  5 xt_conntrack,nf_nat,nf_conntrack_netlink,xt_connmark,xt_MASQUERADE
```

Use `rmmod` and `insmod` to remove and install modules.(in `rmmod` modules should not used by andy modules. `insmod` cannot resolve dependency)

For install modules use `modprobe`. before using `modeprobe` run `depmod -a`

```bash
depmod -a
modprobe -r video --> for remove modules
modprobe video --> for install modules

lsmod | grep parport

modprobe -r parport

[error madule in use]
modprobe -r ppdev
modprobe -r parport_pc

morprobe -r parport
modprobe parport
modprobe ppdev
modprobe parport_pc
```

```bash
modinfo modules --> information abute modeules
```


#### Kernel configuration

```bash

/etc/sysctl.conf --> permanent configuratin file for loadin in kernel

/proc/sys/kernel --> current live kernel modules

/etc/sysctl.d --> package loaded on kernel modules(sysctl.conf has higher priority and override this)

/sbin/sysctl --> command for current kernel modules

udev --> monitoring adding and removing kernel modules

lsusb --> list connected usb device

lspci --> list connected pci device

lsdev --> list all connected device

dmesg --> running log of system device

udevadm monitor --> real time monitoring of kernel modules

```
### sysctl
We discussed that by manipulating /proc/sys/kernel we can change some current running kernel parameters. But as we said these settings are not permanent and they all gone after reboot. sysctl lets us make permanent settings.
sysctl -a to show all tuneables.
`root@server1:~# sysctl -a | wc`

To see curennt kernel module run:

```bash

sysctl -a

vm.numa_stat = 1
vm.numa_zonelist_order = Node
vm.oom_dump_tasks = 1
vm.oom_kill_allocating_task = 0
vm.overcommit_kbytes = 0
vm.overcommit_memory = 0
vm.overcommit_ratio = 50
vm.page-cluster = 3
vm.page_lock_unfairness = 5
vm.panic_on_oom = 0
vm.percpu_pagelist_high_fraction = 0
vm.stat_interval = 1
sysctl: permission denied on key 'vm.stat_refresh'
vm.swappiness = 60
vm.unprivileged_userfaultfd = 0
vm.user_reserve_kbytes = 131072
vm.vfs_cache_pressure = 100
vm.watermark_boost_factor = 15000
vm.watermark_scale_factor = 10
vm.zone_reclaim_mode = 0

```
that is a large file you can see.To set a new value to a parameter use sysctl -w :

You can change kernel modules values with: (not permanent)

```bash
sudo sysctl -w vm.zone_reclaim_mode=1

vm.zone_reclaim_mode = 1
```
as you can see includes /etc/sysctl.d/ directory setting files but don't for get that setting in sysctl.conf file always over write /etc/sysctl.d/ directory and Win :) You can uncomment or add setting weather in sysctl.conf or go and create custom file in /etc/sysctl.d/ directory but Take care of contrasts.

### udev
During boot process when kernel is loaded and Device Driver start setting up hardware, next step of hardware initializing is udev. So kernel initial device loading and send uevents to udev daemon. udev keep these event and handle them base on attributes it has received in the event.

### udevadm monitor , udevadm
udev has a monitoring tool, and it monitor and shows event received from the kernel after hardware initializing and uevent which are udev events after processing udev rules, lets attach a usb and monitor what it shows:

```bash
root@server1:~# udevadm monitor 
monitor will print the received events for:
UDEV - the event which udev sends out after rule processing
KERNEL - the kernel uevent

KERNEL[3520.586975] add      /devices/pci0000:00/0000:00:11.0/0000:02:03.0/usb1/1-1 (usb)
KERNEL[3520.589831] add      /devices/pci0000:00/0000:00:11.0/0000:02:03.0/usb1/1-1/1-1:1.0 (usb)
UDEV  [3520.610064] add      /devices/pci0000:00/0000:00:11.0/0000:02:03.0/usb1/1-1 (usb)
KERNEL[3520.653543] add      /module/usb_storage (module)
```
You can see both kernel messages and udev events. And finally our usb flash is mounted as sdb. For having more readable data:

```bash
root@server1:~# udevadm info --query=all --name=/dev/sdb
P: /devices/pci0000:00/0000:00:11.0/0000:02:03.0/usb1/1-1/1-1:1.0/host33/target33:0:0/33:0:0:0/block/sdb
N: sdb
S: disk/by-id/usb-Kingston_DT_101_II_0013729982D5B97196320049-0:0
S: disk/by-label/MYLINUXLIVE
S: disk/by-path/pci-0000:02:03.0-usb-0:1:1.0-scsi-0:0:0:0
S: disk/by-uuid/3879-A38A
E: DEVLINKS=/dev/disk/by-uuid/3879-A38A /dev/disk/by-label/MYLINUXLIVE /dev/disk/by-id/usb-Kingston_DT_101_II_0013729982D5B97196320049-0:0 /dev/disk/by-path/pci-0000:02:03.0-usb-0:1:1.0-scsi-0:0:0:0
E: DEVNAME=/dev/sdb
```
use udevadm info attribute-walk --name=/dev/sdb | less to have summary of device attributes:

```bash
root@server1:~# udevadm info --attribute-walk --name=/dev/sdb
```

For use `lsdev` first install `procinfo` package.

```bash
sudo apt install procinfo
lsdev
```



