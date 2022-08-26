### Kernel Version Numbering
Linux Kernel as heart of Linux Operating System was numbered from its beginning. The rule rule of version numbering was like this:

|2|5|23|3|
|:---:|:----:|:---:|:---:|
|Version Number|Major Revision|Minor Revision|Correction/Patch|

where the "odd" or "even" revision number had different concepts. "odd" numbers was used for "development versions" and this showed they where some how unstable and weren't suitable for production environment. On the other hand "even" reversion number showed "stable" version and so it gave hope for Reliability. But this story is for past. When Kernel 2.6 arrived they decided to release just "stable" kernels and so they stick to version 2.6.X. So all versions after 2.6 , weather they have "odd" or "even" numbers, they are all "stable. Kernel 2.6.X was alive till version number 2.6.39.4 , that is a long number huh ? Finally Linus Trovalds accepted to using version 3 and so on . and changed the rule:

|3|0|X|
|:---:|:----:|:---:|
|Version Number|Major Revision|Minor Revision|

and we don't need to be worried about correction/patch number. Lets Check :
```bash
root@ubuntu:~#
root@ubuntu:~# uname -a
Linux ubuntu 5.4.0-124-generic #140-Ubuntu SMP Thu Aug 4 02:23:37 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
root@ubuntu:~#
root@ubuntu:~# uname -r
5.4.0-124-generic
root@ubuntu:~#
```
as you can see we are on version "5", Major release "4" and ubuntu specified patch "0-124-generic"
This way you will have enough insight to work with kernel, upgrade it and troubleshoot linux system. Lets start learning theme little by little:

### initial Ram Disk / initial Ram File System (/boot/)
After Bios/UEFI done his job and pass the control to boot loader, boot loader tries to load the kernel. But loading kernel is not that much easy. Kernel need some drivers to be loaded. How Linux achieve that ? initrd. initrd (initial ramdisk)/initramfs is a scheme for loading a temporary root file system into memory, which may be used as part of the Linux startup process. initrd and initramfs refer to two different methods of achieving this but do the same thing. Both are commonly used to make preparations before the real root file system can be mounted.

```bash

root@ubuntu:~# cd /boot/
root@ubuntu:/boot#
root@ubuntu:/boot#
root@ubuntu:/boot# ls -l
total 101064
-rw-r--r-- 1 root root   237947 Aug  4 01:48 config-5.4.0-124-generic
drwxr-xr-x 4 root root     4096 Aug 26 17:04 grub
lrwxrwxrwx 1 root root       28 Aug 26 17:03 initrd.img -> initrd.img-5.4.0-124-generic
-rw-r--r-- 1 root root 84801138 Aug 26 17:14 initrd.img-5.4.0-124-generic
lrwxrwxrwx 1 root root       28 Aug 26 17:03 initrd.img.old -> initrd.img-5.4.0-124-generic
drwx------ 2 root root    16384 Aug 26 17:00 lost+found
-rw------- 1 root root  4758850 Aug  4 01:48 System.map-5.4.0-124-generic
lrwxrwxrwx 1 root root       25 Aug 26 17:03 vmlinuz -> vmlinuz-5.4.0-124-generic
-rw------- 1 root root 13660416 Aug  4 01:54 vmlinuz-5.4.0-124-generic
lrwxrwxrwx 1 root root       25 Aug 26 17:03 vmlinuz.old -> vmlinuz-5.4.0-124-generic
root@ubuntu:/boot#
root@ubuntu:/boot#
root@ubuntu:/boot#
```
What lets our kernel to boot up is `initrd.img-5.4.0-124-generic` which populate RAM during boot process.do not forget that each kernel version requires its initrd with the same version. Now it is Kernel turn to be loaded. But it has its own story, kernel must be some how compressed inorder to fit in ram. first lets start with some terminology lesson:

### Kernel Modules (/lib/modules/)
in /lib/modules/ you can see Folders contains each kernel's modules:

```bash

root@ubuntu:/boot#
root@ubuntu:/boot# cd /lib/modules
root@ubuntu:/lib/modules# ls -l
total 4
drwxr-xr-x 5 root root 4096 Aug 26 17:03 5.4.0-124-generic
root@ubuntu:/lib/modules#
root@ubuntu:/lib/modules#
root@ubuntu:/lib/modules# cd 5.4.0-124-generic/
root@ubuntu:/lib/modules/5.4.0-124-generic#
root@ubuntu:/lib/modules/5.4.0-124-generic# ls -l
total 5844
lrwxrwxrwx  1 root root      40 Aug  4 01:48 build -> /usr/src/linux-headers-5.4.0-124-generic
drwxr-xr-x  2 root root    4096 Aug  4 01:48 initrd
drwxr-xr-x 17 root root    4096 Aug 26 17:03 kernel
-rw-r--r--  1 root root 1404552 Aug 26 17:03 modules.alias
-rw-r--r--  1 root root 1381603 Aug 26 17:03 modules.alias.bin
-rw-r--r--  1 root root    8105 Aug  4 01:48 modules.builtin
-rw-r--r--  1 root root   25036 Aug 26 17:03 modules.builtin.alias.bin
-rw-r--r--  1 root root   10257 Aug 26 17:03 modules.builtin.bin
-rw-r--r--  1 root root   63668 Aug  4 01:48 modules.builtin.modinfo
-rw-r--r--  1 root root  610977 Aug 26 17:03 modules.dep
-rw-r--r--  1 root root  853885 Aug 26 17:03 modules.dep.bin
-rw-r--r--  1 root root     330 Aug 26 17:03 modules.devname
-rw-r--r--  1 root root  220047 Aug  4 01:48 modules.order
-rw-r--r--  1 root root     947 Aug 26 17:03 modules.softdep
-rw-r--r--  1 root root  615580 Aug 26 17:03 modules.symbols
-rw-r--r--  1 root root  748689 Aug 26 17:03 modules.symbols.bin
drwxr-xr-x  3 root root    4096 Aug 26 17:03 vdso
root@ubuntu:/lib/modules/5.4.0-124-generic#
root@ubuntu:/lib/modules/5.4.0-124-generic#

```

### Kernel (/usr/src/)
And Finally where is the Kernel itself ?

```bash

root@ubuntu:/usr/src#
root@ubuntu:/usr/src# ls -l
total 8
drwxr-xr-x 24 root root 4096 Aug 26 17:03 linux-headers-5.4.0-124
drwxr-xr-x  7 root root 4096 Aug 26 17:03 linux-headers-5.4.0-124-generic
root@ubuntu:/usr/src#
root@ubuntu:/usr/src#
root@ubuntu:/usr/src#
```

