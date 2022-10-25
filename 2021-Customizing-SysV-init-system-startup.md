### Over View
During the previous lessons we talked about initrd/initramfs. When the kernel completly loaded it searchs for init process to start it. init process can be init, upstart or systemd. Traditionally System v init is used to start other services but it has some short comings. So other solutions invented like upstart and systemd.

/sbin/init can be linked to upstart or systemd. to check which system you are running, check each directory existence:

| Directpry      | Description [if exist]|
| ----------- | ----------- |
| /etc/init.d      | Shows you have SysV in your linux box |
|/usr/share/upstart |You are on a Upstart Based system |
|/usr/lib/systemd |You are using Systemd-Based system |