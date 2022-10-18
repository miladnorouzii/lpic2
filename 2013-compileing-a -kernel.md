### Compiling a kernel

Description: Candidates should be able to properly configure a kernel to include or disable specific features of the Linux kernel as necessary. This objective includes compiling and recompiling the Linux kernel as needed, updating and noting changes in a new kernel, creating an initrd image and installing new kernels.

### Kernel

Upgrading kernel and compiling it is very distro specific. The way that we traverse to achive that might be different in Ubuntu, Open Suse or cent OS. Upgrading kernel include compiling, so here we start with downloading new version of kernel and compiling it, but as an administrator you might recompile current version of your linux inorder to add / remove some modules and futures.

### Upgrading kernel
First go to https://kernel.org inorder to get the desired kernel version:

Do not forget we have to be in /usr/src/ and from now on work in this directory only, in this demonstartion we use CentOS7 machine:

```bash
[root@MiWiFi-R4AC-srv ~]# cd /usr/src/
[root@MiWiFi-R4AC-srv src]# 
[root@MiWiFi-R4AC-srv src]# 
[root@MiWiFi-R4AC-srv src]# ls 
debug  kernels  linux  linux-4.14.3  linux-4.14.3.tar.xz
[root@MiWiFi-R4AC-srv src]# 
```

Download your kernel

```bash
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.14.3.tar.xz
```

Now lets extract .xz file. Also we should make a symbolic link as linux which point to downloaded kernel:

```bash
[root@MiWiFi-R4AC-srv src]# tar -Jxvf linux-4.14.3.tar.xz 

[root@MiWiFi-R4AC-srv src]# ln -s linux-4.14.3 linux

```

Before continue there is document folder inside downloaded kernel, There are amazing help text file:

```bash
root@MiWiFi-R4AC-srv linux]# ls Documentation/
00-INDEX                    gpio                         parport-lowlevel.txt
ABI                         gpu                          PCI
accounting                  hid                          pcmcia
acpi                        highuid.txt                  percpu-rw-semaphore.txt
admin-guide                 hwmon                        perf
aoe                         hw_random.txt                phy
arm                         hwspinlock.txt               phy.txt
arm64                       i2c                          pi-futex.txt
atomic_bitops.txt           ia64                         platform
atomic_t.txt                ide                          pnp.txt
auxdisplay                  iio                          power
backlight                   index.rst                    powerpc
bcache.txt                  infiniband                   pps
blackfin                    input                        preempt-locking.txt
block                       Intel-IOMMU.txt              printk-formats.txt
blockdev                    intel_txt.txt                process
bt8xxgpio.txt               ioctl                        pti
btmrvl.txt                  io-mapping.txt               ptp
bus-devices                 io_ordering.txt              pwm.txt
bus-virt-phys-mapping.txt

```

Before continue there are some Development tools which is need to compile the kernel lets install them first:

```bash
[root@MiWiFi-R4AC-srv ~]# yum groupinstall "Development tools"
[root@MiWiFi-R4AC-srv ~]# yum install  elfutils-libelf-devel
[root@MiWiFi-R4AC-srv ~]# yum install openssl-devel
[root@MiWiFi-R4AC-srv ~]# yum install ncurses-devel
[root@MiWiFi-R4AC-srv ~]# yum install qt-devel --> for xconfig
```

Next step is making kernel with modules that we like, compile it and make other kernel required components.

```bash
[root@MiWiFi-R4AC-srv ~]# cd /usr/src/linux
[root@MiWiFi-R4AC-srv ~]# make help
```

```bash
clean --> Remove partially Generated Files From Previous compile and refresh the code but keeps config file

mrproper --> clean everything + config + backup

config --> line oriented program, take days (very hard tool)

nconfig --> ncurses based menu program (hard tool) [need to nstall ncurses-devel]

menuconfig --> menu base program (recommanded tool)

xconfig --> QT-Based menu program, like menu config [need to install qt-devel]

oldconfig --> use the current setting of current kernel for the next one
```

The thing we are going to do is making kernel image (zImage / BigzImage if you remember :) )

We are happy with current kernel configuration so lets try:

```bash
[root@MiWiFi-R4AC-srv ~]# make menuconfig
```
### Make bzImage

```bash
[root@MiWiFi-R4AC-srv ~]# make bzImage
```

### make modules
Modules need to be compiled too, so do the same thing for them:

```bash
[root@MiWiFi-R4AC-srv ~]# make modules
```

### Make modules_install
Now that every thing is prepared, its time to put all part to the their right places, First let put compiled modules to `/lib/modules/`

```bash
[root@MiWiFi-R4AC-srv ~]# make modules_install
```

to confirm what has been done:

```bash
[root@MiWiFi-R4AC-srv ~]# ls /lib/modules
```

# Make install
make install write required files into the /boot and modified grub and set everything for us

```bash
[root@MiWiFi-R4AC-srv linux]# make install
```

and reboot and enjoy your new kernel!

```bash
[root@MiWiFi-R4AC-srv linux]# uname -r
```

