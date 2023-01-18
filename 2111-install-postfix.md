```
apt install postfix mailutils
```
If you get a error code 1 run
```
sudo apt-get install -f
```

Send email to account.

```
mail milad.norouzi1370@gmail.com
```
Condifuration file is:

```
cd /etc/postfix

vim main.cf
```

Mail to othe user in linux

```
mail -s "subject of email" user@localhost

for quit ctrl+d
```

for forward email to another person

```
vim /etc/aliases

postmaster: root

# and then

newaliases

```

For check log

```
tail -f /var/log/mail.log
```

For check outbok and see send mail.

```
sendmail -bp
```