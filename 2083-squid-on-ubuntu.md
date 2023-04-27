Install squid on ubuntu

```
apt install squid -y
```

```
cd /etc/squid/
vim squid.conf

### uncomment

cache_mem 256 MB
maximum_object_size_in_memory 512 KB
cache_dir  ufs /var/spool/squid 100 16 256 # 100MB cache / 16 dir /256 sub dir
```
Restart squid service

```
systemctl restart squid

```

Set proxy on your browser, in firefox go to setting > general > network setting > manual proxy setting

```
http proxy 127.0.0.1 port 3128
```

Open your browser and serch for wallpaper and cleare your browser cache.


### ACL

Acl has to object `ACL Element` and `ACL list`

##### ACL Element

- src ip
- dst ip
- time
- dst domain
- port

##### ACL list

- http_access
- always_direct
- log_access
- cache
- always_redirect

Ok let to config acl element and access

```
vim /etc/squid/squid.conf

acl localnet src 0.0.0.1-0.255.255.255  # RFC 1122 "this" network (LAN)
acl localnet src 10.0.0.0/8             # RFC 1918 local private network (LAN)
acl localnet src 100.64.0.0/10          # RFC 6598 shared address space (CGN)
acl localnet src 169.254.0.0/16         # RFC 3927 link-local (directly plugged) machines
acl localnet src 172.16.0.0/12          # RFC 1918 local private network (LAN)
acl localnet src 192.168.0.0/16         # RFC 1918 local private network (LAN)
acl localnet src fc00::/7               # RFC 4193 local private network range
acl localnet src fe80::/10              # RFC 4291 link-local (directly plugged) machines

####################################
acl GOOGLE dstdomain .google.com # dot for all subdomain

acl PRIVATE src 192.168.1.0/24
#################################################
acl SSL_ports port 443
acl Safe_ports port 80          # http
acl Safe_ports port 21          # ftp
acl Safe_ports port 443         # https
acl Safe_ports port 70          # gopher
acl Safe_ports port 210         # wais
acl Safe_ports port 1025-65535  # unregistered ports
acl Safe_ports port 280         # http-mgmt
acl Safe_ports port 488         # gss-http
acl Safe_ports port 591         # filemaker
acl Safe_ports port 777         # multiling http
acl CONNECT method CONNECT
```

```
vim /etc/squid/squid.conf

#
# Recommended minimum Access Permission configuration:
#
# Deny requests to certain unsafe ports
http_access deny !Safe_ports

# Deny CONNECT to other than secure SSL ports
http_access deny CONNECT !SSL_ports

# Only allow cachemgr access from localhost


####################################################
http_access deny GOOGLE

####################################################

http_access allow localhost manager
http_access deny manager

# We strongly recommend the following be uncommented to protect innocent
# web applications running on the proxy server who think the only
# one who can access services on "localhost" is a local user
#http_access deny to_localhost

```
Dont forget to set proxy on browser and set for HTTPS 


#### Basic authentication

```
locate ncsa

/usr/lib/squid/basic_ncsa_auth
/usr/share/man/man8/basic_ncsa_auth.8.gz
```

```
vim /etc/squid/squid.conf

##auth_param basic program <uncomment and complete this line>
##auth_param basic children 5 startup=5 idle=1
##auth_param basic realm Squid proxy-caching web server
##auth_param basic credentialsttl 2 hours
#Default:
# none
############################################################################
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwords

############################################################################



acl localnet src 172.16.0.0/12          # RFC 1918 local private network (LAN)
acl localnet src 192.168.0.0/16         # RFC 1918 local private network (LAN)
acl localnet src fc00::/7               # RFC 4193 local private network range
acl localnet src fe80::/10              # RFC 4291 link-local (directly plugged) machines

#############################################################################
acl AUTHENTICATED proxy_auth REQUIERD

#############################################################################
acl GOOGLE dstdomain .google.com # dot for all subdomain

# Only allow cachemgr access from localhost


####################################################
http_access allow AUTHENTICATED
http_access deny GOOGLE

####################################################

http_access allow localhost manager
```

Create user

```
apt install apache2-utils -y

root@desktop:/etc/squid# htpasswd -c /etc/squid/passwords user1
New password: 
Re-type new password: 
Adding password for user user1
root@desktop:/etc/squid# 


systemctl restart squid.service
```

Open your browser :)






