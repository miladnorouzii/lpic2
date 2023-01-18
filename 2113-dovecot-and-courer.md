Install courier 

```
apt-cache search courier | grep courier

apt install courier-imap courier-pop -y 
```

cd /etc/courier

```
vim imapd

MAILDIEPATH=mail/inbox
```

```
vim pop3d

MAILDIRPATH=mail/inbox
```

```
service courier-imap restart
service courier-pop restart
```

```
ss -ntlp | grep 110
ss -ntlp | grep 143
```

For install dovecot:

```
apt purge courier*
apt-get install dovecot-imapd dovecot-pop3d -y
```

All config file save in `/etc/dovecot/conf.d/`

```
cd /etc/dovecot/conf.d/

ls -l
```

```
vim 10-mail.conf

mail_location = ~/mail/inbox

service dovecot restart
```
```
ss -ntlp | grep 110
ss -ntlp | grep 143
```
