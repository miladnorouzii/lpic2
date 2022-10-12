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

You can change kernel modules values with: (not permanent)

```bash
sudo sysctl -w vm.zone_reclaim_mode=1

vm.zone_reclaim_mode = 1
```

For use `lsdev` first install `procinfo` package.

```bash
sudo apt install procinfo
lsdev
```



