install ```strees-ng```

```bash
apt install stress-ng
apt install sysstat
apt install iotop
apt install nload iftop iperf net-tools iptraf-ng -y 
```

```bash
vmstat - Report virtual memory statistics

```
```
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 1162320  27124 535700    0    0    24    60   28   38  0  0 100  0  0
```

```bash
free - Display amount of free and used memory in the system

free -m
free -h

```
```
top - display Linux processes

1 for show cpu core

shift + < or > sort top based on different culoms
shift + p # sort based CPU usage
shift + m # sort based memory usage
press c   # shows absolute patch of process
press z   # will running process in color
press d + number # delay number. default 3 sec
press k + PID   # Kill the process by using PID
press r + PID   # to renice a process
press q     # exit

```
```
us:% CPU time spent in user space #####Linux has two Spaces from OS point of view, User space and Kernel space
sy:% CPU time spent in kernel space####
ni:% CPU time spent on low priority processes
id:% CPU time spent idle
wa:io wait cpu time (or) % CPU time spent in wait (on disk)#######  IO WAIT ######
hi: % CPU time spent servicing/handling hardware interrupts
si:% CPU time spent servicing/handling software interrupts
st:% CPU time in involuntary wait by virtual cpu while hypervisor is servicing another processor
```

```
stress-ng -c 1 # for overload on 1 core
stress-ng -m 10 # for overload on memory

```

###### Uptime

```bash
uptime
12:10:01 up 45 min,  2 users,  load average: 0.08, 0.07, 0.12
```
0.08 load average of last 1 minute
0.07 load average of last 5 minute
0.12 load average of last 15 minute

Load avarage based on cpu and io

For example if you use 1 cpu:

load avrage 0.5 means => half used :)

load avrage 1.0 means => fully used :|

load avrage 1.5 means => overused :(

##### Disk i/o

```bash
strees-ng --hdd 3
```
Use `iotop` for check i/o

You can use `iostat` for check i/o but it's not a real time. Also you can use `sar 1 10` (every 1 second and repet 10) 

```
sar 1 10


Linux 5.4.0-107-generic (ubuntu-srv) 	08/12/2022 	_x86_64_	(4 CPU)

12:31:41 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
12:31:42 PM     all      0.00      0.00      0.00      0.00      0.00    100.00
12:31:43 PM     all      0.00      0.00      0.00      0.00      0.00    100.00
12:31:44 PM     all      0.00      0.00      0.25      0.00      0.00     99.75
12:31:45 PM     all      0.00      0.00      0.00      0.00      0.00    100.00
12:31:46 PM     all      0.00      0.00      0.00      0.00      0.00    100.00
12:31:47 PM     all      0.00      0.00      0.00      0.00      0.00    100.00
12:31:48 PM     all      0.00      0.00     16.67     36.72      0.00     46.61
12:31:49 PM     all      0.00      0.00      3.38     75.32      0.00     21.30
12:31:50 PM     all      0.00      0.00      2.84     76.23      0.00     20.93
12:31:51 PM     all      0.00      0.00      5.48     68.41      0.00     26.11
Average:        all      0.00      0.00      2.79     25.09      0.00     72.12
```

### Network Monitoring

Install `iftop & nload & net-tools` 

Use `ifconfig` command to see TX and RX

```bash 
ifconfig 
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.137.128  netmask 255.255.255.0  broadcast 192.168.137.255
        inet6 fe80::20c:29ff:fefc:9072  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:fc:90:72  txqueuelen 1000  (Ethernet)
        RX packets 286858  bytes 398213903 (398.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 104170  bytes 7084606 (7.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

If you dont have `ifconfig` commadn you can use `netstat -ie` to see your interface status and information.

```bash
 netstat -ie 
Kernel Interface table
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.137.128  netmask 255.255.255.0  broadcast 192.168.137.255
        inet6 fe80::20c:29ff:fefc:9072  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:fc:90:72  txqueuelen 1000  (Ethernet)
        RX packets 286919  bytes 398219397 (398.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 104220  bytes 7090426 (7.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Use `netstat -s` for details of send and recive packet per interface.

```bash
 netstat -s 
Ip:
    Forwarding: 2
    205098 total packets received
    7 with invalid addresses
    0 forwarded
    0 incoming packets discarded
    205091 incoming packets delivered
    104654 requests sent out
    20 outgoing packets dropped
    4 dropped because of missing route
Icmp:
    45 ICMP messages received
    0 input ICMP message failed
    ICMP input histogram:
        destination unreachable: 42
        echo requests: 1
        echo replies: 2
    47 ICMP messages sent
    0 ICMP messages failed
    ICMP output histogram:
        destination unreachable: 42
        echo requests: 4
        echo replies: 1
IcmpMsg:
        InType0: 2
        InType3: 42
        InType8: 1
        OutType0: 1
        OutType3: 42
        OutType8: 4
Tcp:
    152 active connection openings
    2 passive connection openings
    0 failed connection attempts
    3 connection resets received
    2 connections established
    204082 segments received
    103923 segments sent out
    0 segments retransmitted
    0 bad segments received
    112 resets sent
Udp:
    731 packets received
    42 packets to unknown port received
    0 packet receive errors
    775 packets sent
    0 receive buffer errors
    0 send buffer errors
    IgnoredMulti: 189
UdpLite:
TcpExt:
    30 TCP sockets finished time wait in fast timer
    83 delayed acks sent
    194271 packet headers predicted
    1781 acknowledgments not containing data payload received
    2235 predicted acknowledgments
    TCPBacklogCoalesce: 3928
    2 connections reset due to early user close
    TCPRcvCoalesce: 82761
    TCPAutoCorking: 110
    TCPOrigDataSent: 5666
    TCPDelivered: 5719
IpExt:
    InBcastPkts: 189
    InOctets: 390976800
    OutOctets: 5106653
    InBcastOctets: 35622
    InNoECTPkts: 287468
```
Check number of packet with `ifconfig`, after than `ping 8.8.8.8` and recheck `ifconfig` output. You can see the value of ifconfig changed.

##### Live network monitoring

###### iftop

Use `iftop` to check live monitoring

###### nload
Use `nload` to check interface send and recive network traffic.

###### Speedtest

Firest of all install `iperf` on linux system.

For this test you should have tow linux system and install `iperf` on both of them.

Download `speedtest` on bellow link.

```bash
wget https://github.com/sivel/speedtest-cli/raw/master/speedtest.py
```

Edit script and replace `python3` on first line.

```bash 
chmod +x speedtest.py
./speedtest.py --simple

Ping: 138.215 ms
Download: 19.20 Mbit/s
Upload: 3.27 Mbit/s
```
To check list servers

```bash
./speedtest.py --list

37703) Abramad (Tehran, Iran) [6.53 km]
32500) PISHGAMAN (Tehran, Iran) [8.94 km]
37820) Sindad (Tehran, Iran) [8.94 km]
45095) GreenWeb (Pardis, Iran) [40.40 km]
37535) MTNIrancell (Mazandaran, Iran) [121.28 km]
 7667) ATINET (Hamedan, Iran) [275.73 km]
 9795) MTNIrancell (Isfahan, Iran) [340.98 km]
22243) MCI (Hamrahe Avval) (Tabriz, Iran) [519.78 km]
 9890) MTNIrancell (Tabriz, Iran) [519.78 km]
 9888) Maxnet (Tabriz, Iran) [519.78 km]
```
Every server has an uniq id and you can use id for test with specific server.

```bash 
./speedtest.py --server 37703
```
Use `--share` option for generate link to see graphical modeles

###### iperf

`iperf` is a clinet server tools for checking speed test between tow linux system.

On server side run 

```bash
iperf -s
```

On client side run:

```bash
iperf -c SERVER_IP
```
```bash
iperf -c 192.168.137.128
------------------------------------------------------------
Client connecting to 192.168.137.128, TCP port 5001
TCP window size: 2.50 MByte (default)
------------------------------------------------------------
[  3] local 192.168.137.128 port 48418 connected with 192.168.137.128 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  31.7 GBytes  27.3 Gbits/sec
```
###### iptraf

install `iptraf-ng`

```bash
iptraf-ng
iptraf-ng -i ens33
```
### Monitoring tools

prtg
cacti
zabbix
netdata
collectd

###### collectd

Install `collectd` on linux server.

```bash
apt install collectd
```
After install `collectd` you should enable plugin to monitor your system.

```bash
vim /etc/collectd/collectd.conf
```
Uncomment the plugin you want to monitor

```

##############################################################################
# LoadPlugin section                                                         #
#----------------------------------------------------------------------------#
# Specify what features to activate.                                         #
##############################################################################

#LoadPlugin aggregation
#LoadPlugin amqp
#LoadPlugin apache
#LoadPlugin apcups
#LoadPlugin ascent
#LoadPlugin barometer
LoadPlugin battery
#LoadPlugin bind
#LoadPlugin ceph
#LoadPlugin cgroups
#LoadPlugin chrony
#LoadPlugin conntrack
#LoadPlugin contextswitch
LoadPlugin cpu
#LoadPlugin cpufreq
#LoadPlugin cpusleep
#LoadPlugin csv
```
Restart `collectd` service.

```bash
service collectd restart
```

#####  Install Collectd-Web and Dependencies

```bash
apt install  librrds-perl libjson-perl libhtml-parser-perl libcgi-pm-perl git
```
```bash
cd /usr/local/
git clone https://github.com/httpdss/collectd-web.git
```
Go to directory and change:

```bash
cd collectd-web/
ls
chmod +x cgi-bin/graphdefs.cgi
```
Collectd-web standalone Python server script is configured by default to run and bind only on loopback address (127.0.0.1).
You need to edit the runserver.py script and change the 127.0.1.1 IP Address to 0.0.0.0, in order to bind on all network interfaces IP Addresses.

```bash
vim runserver.py
```
```bash
python runserver.py &
```
If you dont have python package you can install it with:

```bash
apt install python
```
To visit Collectd-web interface and display statistics about your host, open a browser and point the URL at your server IP Address and port 8888 using HTTP protocol.

How does it work? Collectd uses data sets which are defines in types.db:

```bash
ls /usr/share/collectd/
types.db

root@ubuntu-srv:~#  cat /usr/share/collectd/types.db | grep cpu
cpu                     value:DERIVE:0:U
cpu_affinity            value:GAUGE:0:1
cpufreq                 value:GAUGE:0:U
ps_cputime              user:DERIVE:0:U, syst:DERIVE:0:U
redis_command_cputime   value:DERIVE:0:U
vcpu                    value:GAUGE:0:U
virt_cpu_total          value:DERIVE:0:U
virt_vcpu               value:DERIVE:0:U
```

to gather data in a special format(Round Robin DataBase ) in /var/lib/directory/rrd directory which we previously enabled in collectd.conf:

```bash
root@ubuntu-srv:~# cat /etc/collectd/collectd.conf | grep rrd
#LoadPlugin rrdcached
LoadPlugin rrdtool
#<Plugin rrdcached>
#	DaemonAddress "unix:/var/run/rrdcached.sock"
#	DataDir "/var/lib/rrdcached/db/collectd"
<Plugin rrdtool>
	DataDir "/var/lib/collectd/rrd"
```
```bash
root@ubuntu-srv:~#  tree /var/lib/collectd/rrd
/var/lib/collectd/rrd
└── ubuntu-srv
    ├── cpu-0
    │   ├── cpu-idle.rrd
    │   ├── cpu-interrupt.rrd
    │   ├── cpu-nice.rrd
    │   ├── cpu-softirq.rrd
    │   ├── cpu-steal.rrd
    │   ├── cpu-system.rrd
    │   ├── cpu-user.rrd
    │   └── cpu-wait.rrd
    ├── cpu-1
    │   ├── cpu-idle.rrd
    │   ├── cpu-interrupt.rrd
    │   ├── cpu-nice.rrd
    │   ├── cpu-softirq.rrd
    │   ├── cpu-steal.rrd
    │   ├── cpu-system.rrd
    │   ├── cpu-user.rrd
    │   └── cpu-wait.rrd
    ├── cpu-2
    │   ├── cpu-idle.rrd
    │   ├── cpu-interrupt.rrd
    │   ├── cpu-nice.rrd
    │   ├── cpu-softirq.rrd
    │   ├── cpu-steal.rrd
    │   ├── cpu-system.rrd
    │   ├── cpu-user.rrd
    │   └── cpu-wait.rrd
    ├── cpu-3
    │   ├── cpu-idle.rrd
    │   ├── cpu-interrupt.rrd
    │   ├── cpu-nice.rrd
    │   ├── cpu-softirq.rrd
    │   ├── cpu-steal.rrd
    │   ├── cpu-system.rrd
    │   ├── cpu-user.rrd
    │   └── cpu-wait.rrd
    ├── df-boot
    │   ├── df_complex-free.rrd
    │   ├── df_complex-reserved.rrd
    │   └── df_complex-used.rrd
    ├── df-root
    │   ├── df_complex-free.rrd
    │   ├── df_complex-reserved.rrd
    │   └── df_complex-used.rrd
    ├── df-snap-core20-1328
    │   ├── df_complex-free.rrd
    │   ├── df_complex-reserved.rrd
    │   └── df_complex-used.rrd
    ├── df-snap-lxd-21835
    │   ├── df_complex-free.rrd
    │   ├── df_complex-reserved.rrd
    │   └── df_complex-used.rrd
    ├── df-snap-snapd-14978
    │   ├── df_complex-free.rrd
    │   ├── df_complex-reserved.rrd
    │   └── df_complex-used.rrd
    ├── disk-dm-0
    │   ├── disk_io_time.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-loop0
    │   ├── disk_io_time.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-loop1
    │   ├── disk_io_time.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-loop2
    │   ├── disk_io_time.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-loop3
    │   ├── disk_io_time.rrd
    │   ├── disk_octets.rrd
    │   └── disk_ops.rrd
    ├── disk-sda
    │   ├── disk_io_time.rrd
    │   ├── disk_merged.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-sda1
    │   ├── disk_io_time.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-sda2
    │   ├── disk_io_time.rrd
    │   ├── disk_merged.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-sda3
    │   ├── disk_io_time.rrd
    │   ├── disk_merged.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── disk-sr0
    │   ├── disk_io_time.rrd
    │   ├── disk_octets.rrd
    │   ├── disk_ops.rrd
    │   └── disk_time.rrd
    ├── entropy
    │   └── entropy.rrd
    ├── interface-ens33
    │   ├── if_dropped.rrd
    │   ├── if_errors.rrd
    │   ├── if_octets.rrd
    │   └── if_packets.rrd
    ├── interface-lo
    │   ├── if_dropped.rrd
    │   ├── if_errors.rrd
    │   ├── if_octets.rrd
    │   └── if_packets.rrd
    ├── irq
    │   ├── irq-0.rrd
    │   ├── irq-12.rrd
    │   ├── irq-14.rrd
    │   ├── irq-15.rrd
    │   ├── irq-16.rrd
    │   ├── irq-17.rrd
    │   ├── irq-18.rrd
    │   ├── irq-19.rrd
    │   ├── irq-1.rrd
    │   ├── irq-24.rrd
    │   ├── irq-25.rrd
    │   ├── irq-26.rrd
    │   ├── irq-27.rrd
    │   ├── irq-28.rrd
    │   ├── irq-29.rrd
    │   ├── irq-30.rrd
    │   ├── irq-31.rrd
    │   ├── irq-32.rrd
    │   ├── irq-33.rrd
    │   ├── irq-34.rrd
    │   ├── irq-35.rrd
    │   ├── irq-36.rrd
    │   ├── irq-37.rrd
    │   ├── irq-38.rrd
    │   ├── irq-39.rrd
    │   ├── irq-40.rrd
    │   ├── irq-41.rrd
    │   ├── irq-42.rrd
    │   ├── irq-43.rrd
    │   ├── irq-44.rrd
    │   ├── irq-45.rrd
    │   ├── irq-46.rrd
    │   ├── irq-47.rrd
    │   ├── irq-48.rrd
    │   ├── irq-49.rrd
    │   ├── irq-50.rrd
    │   ├── irq-51.rrd
    │   ├── irq-52.rrd
    │   ├── irq-53.rrd
    │   ├── irq-54.rrd
    │   ├── irq-55.rrd
    │   ├── irq-56.rrd
    │   ├── irq-57.rrd
    │   ├── irq-58.rrd
    │   ├── irq-8.rrd
    │   ├── irq-9.rrd
    │   ├── irq-CAL.rrd
    │   ├── irq-DFR.rrd
    │   ├── irq-ERR.rrd
    │   ├── irq-IWI.rrd
    │   ├── irq-LOC.rrd
    │   ├── irq-MCE.rrd
    │   ├── irq-MCP.rrd
    │   ├── irq-MIS.rrd
    │   ├── irq-NMI.rrd
    │   ├── irq-NPI.rrd
    │   ├── irq-PIN.rrd
    │   ├── irq-PIW.rrd
    │   ├── irq-PMI.rrd
    │   ├── irq-RES.rrd
    │   ├── irq-RTR.rrd
    │   ├── irq-SPU.rrd
    │   ├── irq-THR.rrd
    │   ├── irq-TLB.rrd
    │   └── irq-TRM.rrd
    ├── load
    │   └── load.rrd
    ├── memory
    │   ├── memory-buffered.rrd
    │   ├── memory-cached.rrd
    │   ├── memory-free.rrd
    │   ├── memory-slab_recl.rrd
    │   ├── memory-slab_unrecl.rrd
    │   └── memory-used.rrd
    ├── processes
    │   ├── fork_rate.rrd
    │   ├── ps_state-blocked.rrd
    │   ├── ps_state-paging.rrd
    │   ├── ps_state-running.rrd
    │   ├── ps_state-sleeping.rrd
    │   ├── ps_state-stopped.rrd
    │   └── ps_state-zombies.rrd
    ├── swap
    │   ├── swap-cached.rrd
    │   ├── swap-free.rrd
    │   ├── swap_io-in.rrd
    │   ├── swap_io-out.rrd
    │   └── swap-used.rrd
    └── users
        └── users.rrd

29 directories, 183 files
```

Install netdata

```bash
wget -O /tmp/netdata-kickstart.sh https://my-netdata.io/kickstart.sh && sh /tmp/netdata-kickstart.sh
```

To visit netdata interface and display statistics about your host, open a browser and point the URL at your server IP Address and port 19999 using HTTP protocol.

Okey do not forget that the installation process is not part of LPIC exam and based on your distribution and version the installation steps might be different from what we have done here




source:


https://www.tecmint.com/install-collectd-and-collectd-web-to-monitor-server-resources-in-linux/

https://zoomadmin.com/HowToInstall/UbuntuPackage/libcgi-pm-perl

https://www.linux.com/training-tutorials/installation-guide-collectd-and-collectd-web-monitor-server-resources-linux/

https://www.linuxsysadmins.com/install-collectd-monitoring-on-linux/
