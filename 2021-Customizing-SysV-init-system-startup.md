### Over View
During the previous lessons we talked about initrd/initramfs. When the kernel completly loaded it searchs for init process to start it. init process can be init, upstart or systemd. Traditionally System v init is used to start other services but it has some short comings. So other solutions invented like upstart and systemd.

/sbin/init can be linked to upstart or systemd. to check which system you are running, check each directory existence:

| Directpry      | Description [if exist]|
| ----------- | ----------- |
| /etc/init.d      | Shows you have SysV in your linux box |
|/usr/share/upstart |You are on a Upstart Based system |
|/usr/lib/systemd |You are using Systemd-Based system |

also try stat /proc/1/exe

| link      | Description |
| ----------- | ----------- |
|File: ‘/proc/1/exe’ -> ‘/sbin/init’ |in SysV and upstart system |
|File: '/proc/1/exe' -> '/lib/systemd/systemd' |in Systemd box |

### Sys v

System "5" or Sys "v" is an ancient method of handling system services from unix world back to 1980s. SysV uses serial loading of services, in another word each service must be loaded in sequence (after each other). SysV uses runlevels concept to define which stat the server should boot in. In each runlevel specific amount of shell scripts is processed to reach the state we desire.
runlevels start from 0 upto 6 and they are different in Redhat based and Debian based systems.

| runlevel  | Redhat | Debian |
| ----------- | ----------- | ----------- |
|0 |System Halt (do not set as default) |System Halt (do not set as default) |
|1 |Single User Mode |Single User Mode |
|2 |Multi User without NFS |Full multi User mode with GUI(default)|
|3 |Full Multi User Mode |--same as 2-- --unused-- |
|4 |---unused-- |--same as 2-- --unused-- |
|5 |X11/Full Multi user Mode(default) |--same as 2-- --unused-- |
|6 |reboot (Do not set as initdefault) |rebbot (do not set as default) |

/etc/inittab is SysV configuration file where default runlevel can be set, We use CentOS 5 for demonstration :

```bash
#
# inittab       This file describes how the INIT process should set up
#               the system in a certain run-level.
#
# Author:       Miquel van Smoorenburg, <miquels@drinkel.nl.mugnet.org>
#               Modified for RHS Linux by Marc Ewing and Donnie Barnes
#

# Default runlevel. The runlevels used by RHS are:
#   0 - halt (Do NOT set initdefault to this)
#   1 - Single user mode
#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
#   3 - Full multiuser mode
#   4 - unused
#   5 - X11
#   6 - reboot (Do NOT set initdefault to this)
# 
id:5:initdefault:

# System initialization.
si::sysinit:/etc/rc.d/rc.sysinit

l0:0:wait:/etc/rc.d/rc 0
l1:1:wait:/etc/rc.d/rc 1
l2:2:wait:/etc/rc.d/rc 2
l3:3:wait:/etc/rc.d/rc 3
l4:4:wait:/etc/rc.d/rc 4
l5:5:wait:/etc/rc.d/rc 5
l6:6:wait:/etc/rc.d/rc 6

# Trap CTRL-ALT-DELETE
ca::ctrlaltdel:/sbin/shutdown -t3 -r now

# When our UPS tells us power has failed, assume we have a few minutes
# of power left.  Schedule a shutdown for 2 minutes from now.
# This does, of course, assume you have powerd installed and your
# UPS connected and working correctly.  
pf::powerfail:/sbin/shutdown -f -h +2 "Power Failure; System Shutting Down"

# If power was restored before the shutdown kicked in, cancel it.
pr:12345:powerokwait:/sbin/shutdown -c "Power Restored; Shutdown Cancelled"


# Run gettys in standard runlevels
1:2345:respawn:/sbin/mingetty tty1
2:2345:respawn:/sbin/mingetty tty2
3:2345:respawn:/sbin/mingetty tty3
4:2345:respawn:/sbin/mingetty tty4
5:2345:respawn:/sbin/mingetty tty5
6:2345:respawn:/sbin/mingetty tty6

# Run xdm in runlevel 5
x:5:respawn:/etc/X11/prefdm -nodaemon
```

By modifying id:5:initdefault: we can change the run level for the next time boot but there is another proper way :

### init / telinit

init and telinit commands are the same.how ever telinit is recommended. They are both used to change current system runlevel

```bash
[root@centos5 ~]# telinit 3
```
and for come back to previous run level:

```bash
[root@centos5 ~]# telinit 5
```
and to see previous runlevel and current runlevel use runlevel command:

```bash
[root@centos5 ~]# runlevel
3 5
```
### /etc/init.d and /etc/rc.d

as we said SysV runs scripts in sequence to start services. But how and where they are managed? its simple but implementation is some how complicated.

all scripts are inside /etc/rc.d/init.d but there are symbolic links to desired rc folder. each rc folder specify one runlevel. so if you want to manually start a service in a run level you can create a symbolic link inside desired rc folder from init.d folder and put a name with sequence for that. "K" for Kill the service and "S" to Start it.

### chkconfig

Exploring rc folders, creating symbolic links is a hard job. chkconfig is a great tool which let us turn on or off specific service or services in desired runlevel.lets start:

```bash
[root@centos5 ~]# chkconfig 
chkconfig version 1.3.30.2 - Copyright (C) 1997-2000 Red Hat, Inc.
This may be freely redistributed under the terms of the GNU Public License.

usage:   chkconfig --list [name]
         chkconfig --add <name>
         chkconfig --del <name>
         chkconfig [--level <levels>] <name> <on|off|reset|resetpriorities>
```

```bash
[root@centos5 ~]# chkconfig --list
NetworkManager  0:off   1:off   2:off   3:off   4:off   5:off   6:off
acpid           0:off   1:off   2:on    3:on    4:on    5:on    6:off
anacron         0:off   1:off   2:on    3:on    4:on    5:on    6:off
atd             0:off   1:off   2:off   3:on    4:on    5:on    6:off
auditd          0:off   1:off   2:on    3:on    4:on    5:on    6:off
autofs          0:off   1:off   2:off   3:on    4:on    5:on    6:off
avahi-daemon    0:off   1:off   2:off   3:on    4:on    5:on    6:off
avahi-dnsconfd  0:off   1:off   2:off   3:off   4:off   5:off   6:off
```
to change see some usefull examples:

| command  | description |
| ----------- | ----------- |
|chkconfig <service name> on |start service on any unspecial run level(3,4,5 centOS) |
|chkconfig <service name> off |
turn service off in all runlevels |
|chkconfig  --list <service name> |List service condition in all unlevels |
|chkconfig --level  345 <service name> on/off |
Turn serve on/off in runlevel 3,4 and 5 |

### update-rc.d
in Debian based systems like ubuntu update-rc.d used as a command instead of chkconfig command

```bash
root@ubuntu:/etc# update-rc.d
usage: update-rc.d [-n] [-f] <basename> remove
       update-rc.d [-n] <basename> defaults [NN | SS KK]
       update-rc.d [-n] <basename> start|stop NN runlvl [runlvl] [...] .
       update-rc.d [-n] <basename> disable|enable [S|2|3|4|5]
        -n: not really
        -f: force

The disable|enable API is not stable and might change in the future.

```
```bash
root@ubuntu:/etc# update-rc.d apparmor disable
update-rc.d: warning: apparmor start runlevel arguments (none) do not match LSB Default-Start values (S)
 Disabling system startup links for /etc/init.d/apparmor ...
 Removing any system startup links for /etc/init.d/apparmor ...
   /etc/rc0.d/K63apparmor
 Adding system startup for /etc/init.d/apparmor ...
   /etc/rc0.d/K63apparmor -> ../init.d/apparmor
```

### upstart

The first serious attempt to replace systemV was upstart, created by ubuntu. upstart is reactionary, means it takes events and based on them run jobs. In comparison with SysV upstart is more flexible but still it uses scripts and like SysV has some shortages. Although upstart is backward compatible and lets us to use SysV commands. if your system has /etc/init directory it us using upstart.
upstart keeps all previous SysV Folders and uses it, We use Ubuntu 14 machine :

```bash
root@server1:/etc# cd /etc/init.d/ && ls
acpid          dns-clean          procps      single
anacron        friendly-recovery  pulseaudio  skeleton
apparmor       grub-common        rc          speech-dispatcher
apport         halt               rc.local    sudo
avahi-daemon   irqbalance         rcS         thermald
bluetooth      kerneloops         README      udev
brltty         killprocs          reboot      umountfs
console-setup  kmod               resolvconf  umountnfs.sh
cron           lightdm            rsync       umountroot
cups           networking         rsyslog     unattended-upgrades
cups-browsed   ondemand           saned       urandom
dbus           pppd-dns           sendsigs    x11-common

```

```bash
root@server1:~# cd /etc/rc6.d/ && ls -l
total 4
lrwxrwxrwx 1 root root  29 Dec  9 02:07 K10unattended-upgrades -> ../init.d/unattended-upgrades
lrwxrwxrwx 1 root root  20 Dec  9 02:07 K20kerneloops -> ../init.d/kerneloops
lrwxrwxrwx 1 root root  15 Dec  9 02:07 K20rsync -> ../init.d/rsync
lrwxrwxrwx 1 root root  27 Dec  9 02:07 K20speech-dispatcher -> ../init.d/speech-dispatcher
-rw-r--r-- 1 root root 351 Mar 12  2014 README
lrwxrwxrwx 1 root root  18 Dec  9 02:07 S20sendsigs -> ../init.d/sendsigs
lrwxrwxrwx 1 root root  17 Dec  9 02:07 S30urandom -> ../init.d/urandom
lrwxrwxrwx 1 root root  22 Dec  9 02:07 S31umountnfs.sh -> ../init.d/umountnfs.sh
lrwxrwxrwx 1 root root  18 Dec  9 02:07 S40umountfs -> ../init.d/umountfs
lrwxrwxrwx 1 root root  20 Dec  9 02:07 S60umountroot -> ../init.d/umountroot
lrwxrwxrwx 1 root root  16 Dec  9 02:07 S90reboot -> ../init.d/reboot
```
The configuration files of native upstart services are in /etc/init/ directory :

```bash
root@server1:/etc/init# ls 
acpid.conf                   mtab.sh.conf
alsa-restore.conf            networking.conf
alsa-state.conf              network-interface.conf
alsa-store.conf              network-interface-container.conf
anacron.conf                 network-interface-security.conf
apport.conf                  network-manager.conf
avahi-cups-reload.conf       passwd.conf
avahi-daemon.conf            plymouth.conf
bluetooth.conf               plymouth-log.conf
```

### How upstart keeps backward compatibility and live beside old SysV? the secret is inside /etc/init/rcS.conf :

```bash
# rcS - System V single-user mode compatibility
#
# This task handles the old System V-style single-user mode, this is
# distinct from the other runlevels since running the rc script would
# be bad.

description    "System V single-user mode compatibility"
author        "Scott James Remnant <scott@netsplit.com>"

start on runlevel S
stop on runlevel [!S]

console owner
exec /sbin/sulogin

post-stop script
    # Don't switch runlevels if we were stopped by an event, since that
    # means we're already switching runlevels
    if [ -n "${UPSTART_STOP_EVENTS}" ]
    then
    exit 0
    fi

    # Switch, passing a magic flag
    start --no-wait rc-sysinit FROM_SINGLE_USER_MODE=y
end script
```
bu using rcS.conf upstart can run SysV scripts which haven't been developed for upstart natively .

### Systemd
Systemd is used in all modern linuxes. Its a new way of starting linux services but that is not all. The idea of Systemd project is scary! Systemd developers have this idea to create Systemd OS which runs on linux OS. So it seems usual if we know it has its own tool which work like cron, or fstab, rsyslog ... .

In Systemd world we have tragets and unit files, targets is like our goal which we want to reach. But for reaching targets we need to specified what ever is needed to be load in unit files. As Systemd doing variety of thing, there for different types of unit files exists:

service : unit file to start a service considering its dependencies

mount : replace the mount in /etc/fstab

timer : replacement for cron

automount : mount a directory when needed

target : as we said a bunch of unit files :), target is an end point, tragets can be used as runlevel .

path :observ activities on a path and start a service associated with that
...

### /usr/lib/systemd/system and /etc/systemd/system
Orginal systemd ubit files are in /usr/lib/systemd/system directory but they should not be modified by administrators. Modification should be done in /etc/systemd/system and its good to know that they are linked. Inorde to do any modification we should copy service files from /usr/lib/systemd/system to /etc/systemd/system and then set our settings, we use Ubuntu 16:

```bash
[root@server1 system]# cd /usr/lib/systemd/system && ls 
abrt-ccpp.service                        plymouth-poweroff.service
abrtd.service                            plymouth-quit.service
abrt-oops.service                        plymouth-quit-wait.service
abrt-pstoreoops.service                  plymouth-read-write.service
abrt-vmcore.service                      plymouth-reboot.service
abrt-xorg.service                        plymouth-start.service
accounts-daemon.service                  plymouth-switch-root.service
alsa-restore.service                     polkit.service
```

```bash
[root@server1 system]# cd /etc/systemd/system/ && ls -l
total 4
drwxr-xr-x. 2 root root   31 Oct 28 11:21 basic.target.wants
drwxr-xr-x. 2 root root   31 Oct 28 11:20 bluetooth.target.wants
lrwxrwxrwx. 1 root root   41 Oct 28 11:20 dbus-org.bluez.service -> /usr/lib/systemd/system/bluetooth.service
lrwxrwxrwx. 1 root root   41 Oct 28 11:20 dbus-org.fedoraproject.FirewallD1.service -> /usr/lib/systemd/system/firewalld.service
lrwxrwxrwx. 1 root root   44 Oct 28 11:21 dbus-org.freedesktop.Avahi.service -> /usr/lib/systemd/system/avahi-daemon.service
lrwxrwxrwx. 1 root root   44 Oct 28 11:21 dbus-org.freedesktop.ModemManager1.service -> /usr/lib/systemd/system/ModemManager.service
lrwxrwxrwx. 1 root root   46 Oct 28 11:20 dbus-org.freedesktop.NetworkManager.service -> /usr/lib/systemd/system/NetworkManager.service
lrwxrwxrwx. 1 root root   57 Oct 28 11:20 dbus-org.freedesktop.nm-dispatcher.service -> /usr/lib/systemd/system/NetworkManager-dispatcher.service
lrwxrwxrwx. 1 root root   36 Oct 28 11:25 default.target -> /lib/systemd/system/graphical.target
drwxr-xr-x. 2 root root   87 Oct 28 11:20 default.target.wants
```
As you have probably seen Systemd works with different dependencies:

| Dependency  | description |
| ----------- | ----------- |
|Requires |define unit files must beloaded, if not load it |
|wants |[seen in targets], specify unit files which must be loaded but if not, takes it easy and continue |
|requisite |must be already loaded, if not loaded fails |
|confilicts |
unit files which never be activated with current unit file |
|before |current unit files activated before listed unit files |
|after |current unit file activated after listed unit file |

### systemctl
systemctl is systemd magic command to work with services:

| systemctl useful commands | description |
| ----------- | ----------- |
|systemctl start <ServiceName> |to start a service |
|systemctl stop <ServiceName> |to stop service |
|systemctl disable <ServiceName> |disable it, won't be activated even after reboot |
|systemctl enable <ServiceName> |enable a service |
|systemctl relaod <ServiceName> |reload a service by reading its conf file,[might not work]|
|systemctl restart <ServiceName> |stop and start a service |
|systemctl list-unit-files --type=service |same as chkconf --list but in systemd environment |
|systemctl daemon-reload |Reload systemd Daemon, used after unit files modification |
|systemctl list-depencencies |List targets and services dependencies |

### For the History:

|Distro |Pre 2006|2006 (14.04)-2019 |2015(15.10)-???? |
|---|---|---|---|
|ubuntu |SysV |Upstart |Systemd |

|Distro |2007 |2011-2020 |2014-???? |
|---|---|---|---|
|centOS |SysV (centOS 5 |Upstart (centOS6)|systemd (centOS 7) |