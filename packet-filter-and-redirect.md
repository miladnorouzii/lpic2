Filter packet

```
iptables -A OUTPUT -d 8.8.8.8 -j DROP --> drop destination trafic
iptables -A INPUT src 192.168.1.100 -j DROP --> drop from src address
```

For list tables rule

```
iptables -L
iptables -L -t nat
iptables -L --line-numbers
```

For delete rule

```
iptables -D OUTPUT -d 8.8.8.8 -j DROP
iptables -D OUTPUT 2
```

Redirect port

```
iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 8000 -j REDIRECT --to-port 80
```

Persist iptables

```
apt install iptables-persistent
```

