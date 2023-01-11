
apt install slapd ldap-utils

set administrator password --> directory service

For change domain name

```
dpkg-reconfigure slapd

set domain name sematec.local

set orgainzation

enter admin pass
```

slapcat -b cn=config 

vim myfile.ldif
```
#dn:ou=users,dc=sematec,dc=local
#objectClass: organizationalUnit
#
#dn:uid=hossein,ou=users,dc=sematec,dc=local
#objectClass: inetOrgPerson
#objectClass: posixAccount
#objectClass: shadowAccount
#uid: hossein
#sn: ahmadi
#giveName: hossein
#cn: hossein
#uidNumber: 2500
#gidNumbe: 2500
#userPassword: P@ssw0rd
#loginShell: /bin/bash
#homeDirectory: /home/hessein

###################################
dn: uid=reza,ou=user,dc=sematec,dc=local
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
cn: ICP Test User
uid: reza
givenName: ICP Test User
sn: reza
userPassword: password

```

To add user:

```
slapadd -l myfile.ldif

slapcat
```

database address

```
cd /etc/ldap
```


############# ldap client

```
ldapsearch search and view
ldappasswd change password
ldapadd add user
ldapdelete delete user
```

```
ldapsearch --help
-h host
-x simple auth
-b define base
-D define distinguished name
-W request password
-w define password
-f define file
-S define user
```

For add with ldapadd:

```
dn: ou=users,dc=sematec,dc=local
ou: users
description : Ordinary users in the company
objectclass: organizationalunit

### Add Devices OU

dn: ou=sales,dc=sematec,dc=local
ou: sales
description: Sales group OU
objectclass: organizationalunit
```

```
ldapsearch -h localhost -x -b dc=sematec,dc=local
ldapsearch -h localhost -x -D cn=admin,dc=sematec,dc=local -W -b dc=sematec,dc=local

enter password

ldapsearch -h localhost -x -D cn=admin,dc=sematec,dc=local -w 123456 -b dc=sematec,dc=local

ldappasswd -h localhost -x -D cn=admin,dc=sematec,dc=local -w 123456 -S uid=reza,ou=user,dc=sematec,dc=local


ldapadd -h localhost -x -D cn=admin,dc=sematec,dc=local -w 123456 -f test.ldif


ldapdelete -h localhost -x -D cn=admin,dc=sematec,dc=local -W ou=sales,dc=sematec,dc=local
```




https://www.ibm.com/docs/en/noi/1.6.0?topic=ldap-creating-user-adding-user-group
