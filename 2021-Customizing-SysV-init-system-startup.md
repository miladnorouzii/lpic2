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
