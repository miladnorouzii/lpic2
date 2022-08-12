install ```strees-ng```

```bash
apt install stress-ng
apt install sysstat
apt install iotop
apt install nload iftop  net-tools -y 
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











