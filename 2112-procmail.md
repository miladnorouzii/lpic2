Procmail: local email handler
maildir: directory based email storage format (/home/user/mail/inbox/)
mbox: old file based email storage format (/home/user/mail/inbox)

postfix: /var/spool/mail/user/ --> Every emails is added to a file

procmail: /home/user/mail/ --> save email in user directory - no system directory used


Install procmail

```
apt install procmail -y
```
Cd /etc/postfix/

Add procmail to postfix

```
postconf -e "mailbox_command=procmail"

cat /etc/postfix/main.cf | grep mailbox
```

For global config

```
vim /etc/procmailrc

MAILDIR=$HOME/email/  # add / to end of email
DEFAULT=$HOME/email/inbox

```

Create directory in all home directory

```
mkdir email
```
send email from root to other user

```

mail milad@localhost

```

for see email

```
cat /home/user/email/inbox

OR

mail -f /home/user/email/inbox

```

Set config per user:

```
vim /home/.procmailrc

#add REGEX

* ^Subject: test
reports

```



