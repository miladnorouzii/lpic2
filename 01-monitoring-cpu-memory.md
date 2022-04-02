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

shift + > for memory sort
shift + < for cpu sort

```
```
stress-ng -c 1 # for overload on 1 core
stress-ng -m 10 # for overload on memory

```


