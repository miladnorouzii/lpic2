install ```strees-ng```

```bash
apt install stress-ng

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

shift + < or > for memory sort
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


