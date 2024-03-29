
## Make and install programs from source

As you know any modern linux distribution has its own repository, where we can easily search for a program , get info and install from it. Although installing a program from repository is easy and make kind of security, but it limits that program source to a specific distribution. So many developers prefer to publish a program source code it self, and let other people to configure and compile it base on their distribution and needs.

3 basic steps to install a program from source code are:

![](./pic/make.jpg)

before beginning We need to install some developers tool. in redhat use`yum groupinstall "Development Tools"` and in debian based and ubuntu run `apt-get install build-essentia` :

```
root@server1:~# apt install build-essential 
Reading package lists... Done
Building dependency tree
Reading state information... Done
build-essential is already the newest version (12.1ubuntu2).
0 upgraded, 0 newly installed, 0 to remove and 172 not upgraded.
```

For demonstration lets have fun and mine bitcoin with cpuminer. First download the source:

```
root@server1:~# wget https://sourceforge.net/projects/cpuminer/files/pooler-cpuminer-2.5.0.tar.gz
--2018-02-04 02:37:14--  https://sourceforge.net/projects/cpuminer/files/pooler-cpuminer-2.5.0.tar.gz
Resolving sourceforge.net (sourceforge.net)... 216.34.181.60
Connecting to sourceforge.net (sourceforge.net)|216.34.181.60|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://sourceforge.net/projects/cpuminer/files/pooler-cpuminer-2.5.0.tar.gz/download [following]
--2018-02-04 02:37:19--  https://sourceforge.net/projects/cpuminer/files/pooler-cpuminer-2.5.0.tar.gz/download
Connecting to sourceforge.net (sourceforge.net)|216.34.181.60|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://downloads.sourceforge.net/project/cpuminer/pooler-cpuminer-2.5.0.tar.gz?r=&ts=1517740642&use_mirror=jaist [following]
--2018-02-04 02:37:22--  https://downloads.sourceforge.net/project/cpuminer/pooler-cpuminer-2.5.0.tar.gz?r=&ts=1517740642&use_mirror=jaist
Resolving downloads.sourceforge.net (downloads.sourceforge.net)... 216.34.181.59
Connecting to downloads.sourceforge.net (downloads.sourceforge.net)|216.34.181.59|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://jaist.dl.sourceforge.net/project/cpuminer/pooler-cpuminer-2.5.0.tar.gz [following]
--2018-02-04 02:37:28--  https://jaist.dl.sourceforge.net/project/cpuminer/pooler-cpuminer-2.5.0.tar.gz
Resolving jaist.dl.sourceforge.net (jaist.dl.sourceforge.net)... 150.65.7.130, 2001:df0:2ed:feed::feed
Connecting to jaist.dl.sourceforge.net (jaist.dl.sourceforge.net)|150.65.7.130|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 257872 (252K) [application/x-gzip]
Saving to: ‘pooler-cpuminer-2.5.0.tar.gz’

pooler-cpuminer-2.5 100%[===================>] 251.83K  21.3KB/s    in 12s     

2018-02-04 02:37:43 (21.3 KB/s) - ‘pooler-cpuminer-2.5.0.tar.gz’ saved [257872/257872]

root@server1:~# ls -l
total 252
drwxr-xr-x 2 root root      0 Feb  3 23:53 mynfs
-rw-r--r-- 1 root root 257872 Jun 22  2017 pooler-cpuminer-2.5.0.tar.gz
```

and now lets uncompress the source, as a quick review:

| uncompress commands       | Description           |
| ------------------------- | --------------------- |
| tar -xvf filename.tar     | uncompress  tar file  |
| tar -xvf filename.tar.gz  | uncompress gzip file  |
| tar -xvf filename.tar.bz2 | uncompress bzip2 file |
| gunzip filename.gz        | uncompress gzip file  |

okey:

```
root@server1:~# mkdir cpuminer
root@server1:~# cd cpuminer/
root@server1:~/cpuminer# tar -zxvf ../pooler-cpuminer-2.5.0.tar.gz 
cpuminer-2.5.0/
cpuminer-2.5.0/compat.h
cpuminer-2.5.0/ChangeLog
cpuminer-2.5.0/INSTALL
cpuminer-2.5.0/compat/
cpuminer-2.5.0/compat/jansson/
cpuminer-2.5.0/compat/jansson/hashtable.c
cpuminer-2.5.0/compat/jansson/strbuffer.h
cpuminer-2.5.0/compat/jansson/jansson_private.h
cpuminer-2.5.0/compat/jansson/hashtable.h
cpuminer-2.5.0/compat/jansson/Makefile.in
cpuminer-2.5.0/compat/jansson/dump.c
cpuminer-2.5.0/compat/jansson/jansson.h
cpuminer-2.5.0/compat/jansson/utf.h
cpuminer-2.5.0/compat/jansson/Makefile.am
cpuminer-2.5.0/compat/jansson/strbuffer.c
cpuminer-2.5.0/compat/jansson/config.h
cpuminer-2.5.0/compat/jansson/load.c
cpuminer-2.5.0/compat/jansson/util.h
cpuminer-2.5.0/compat/jansson/value.c
cpuminer-2.5.0/compat/jansson/utf.c
cpuminer-2.5.0/compat/Makefile.in
cpuminer-2.5.0/compat/Makefile.am
cpuminer-2.5.0/config.sub
cpuminer-2.5.0/miner.h
cpuminer-2.5.0/sha2.c
cpuminer-2.5.0/minerd.1
cpuminer-2.5.0/aclocal.m4
cpuminer-2.5.0/cpu-miner.c
cpuminer-2.5.0/Makefile.in
cpuminer-2.5.0/missing
cpuminer-2.5.0/depcomp
cpuminer-2.5.0/example-cfg.json
cpuminer-2.5.0/util.c
cpuminer-2.5.0/scrypt-x64.S
cpuminer-2.5.0/AUTHORS
cpuminer-2.5.0/compile
cpuminer-2.5.0/elist.h
cpuminer-2.5.0/README
cpuminer-2.5.0/sha2-x64.S
cpuminer-2.5.0/configure
cpuminer-2.5.0/Makefile.am
cpuminer-2.5.0/configure.ac
cpuminer-2.5.0/cpuminer-config.h.in
cpuminer-2.5.0/NEWS
cpuminer-2.5.0/install-sh
cpuminer-2.5.0/scrypt-x86.S
cpuminer-2.5.0/sha2-arm.S
cpuminer-2.5.0/config.guess
cpuminer-2.5.0/sha2-x86.S
cpuminer-2.5.0/COPYING
cpuminer-2.5.0/nomacro.pl
cpuminer-2.5.0/scrypt-ppc.S
cpuminer-2.5.0/scrypt.c
cpuminer-2.5.0/scrypt-arm.S
cpuminer-2.5.0/sha2-ppc.S
```

Before being able to compile our source code, we need to prepare all its requirements and dependencies. To do that use configure command. The configure command is NOT a standard Linux command. configure is a script that is generally provided with the source of most standardized type Linux packages.

```
root@server1:~/cpuminer# ls
cpuminer-2.5.0
root@server1:~/cpuminer# cd cpuminer-2.5.0/
root@server1:~/cpuminer/cpuminer-2.5.0# ls
aclocal.m4    configure             INSTALL      nomacro.pl    sha2.c
AUTHORS       configure.ac          install-sh   README        sha2-ppc.S
ChangeLog     COPYING               Makefile.am  scrypt-arm.S  sha2-x64.S
compat        cpu-miner.c           Makefile.in  scrypt.c      sha2-x86.S
compat.h      cpuminer-config.h.in  minerd.1     scrypt-ppc.S  util.c
compile       depcomp               miner.h      scrypt-x64.S
config.guess  elist.h               missing      scrypt-x86.S
config.sub    example-cfg.json      NEWS         sha2-arm.S

root@server1:~/cpuminer/cpuminer-2.5.0# ./configure 
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking target system type... x86_64-unknown-linux-gnu
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking whether to enable maintainer-specific portions of Makefiles... no
checking for style of include used by make... GNU
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking whether gcc understands -c and -o together... yes
checking dependency style of gcc... gcc3
checking for gcc option to accept ISO C99... none needed
checking how to run the C preprocessor... gcc -E
checking for grep that handles long lines and -e... /bin/grep
checking for egrep... /bin/grep -E
checking whether gcc needs -traditional... no
checking dependency style of gcc... gcc3
checking for ranlib... ranlib
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking sys/endian.h usability... no
checking sys/endian.h presence... no
checking for sys/endian.h... no
checking sys/param.h usability... yes
checking sys/param.h presence... yes
checking for sys/param.h... yes
checking syslog.h usability... yes
checking syslog.h presence... yes
checking for syslog.h... yes
checking for sys/sysctl.h... yes
checking whether be32dec is declared... no
checking whether le32dec is declared... no
checking whether be32enc is declared... no
checking whether le32enc is declared... no
checking for size_t... yes
checking for working alloca.h... yes
checking for alloca... yes
checking for getopt_long... yes
checking whether we can compile AVX code... yes
checking whether we can compile XOP code... yes
checking whether we can compile AVX2 code... yes
checking for json_loads in -ljansson... no
checking for pthread_create in -lpthread... yes
checking for gawk... (cached) mawk
checking for curl-config... no
checking whether libcurl is usable... no
configure: error: Missing required libcurl >= 7.15.2
```

Well obviously it needs libcurl, but if we have had read README file it was mentioned there:

```
Dependencies:
        libcurl                 http://curl.haxx.se/libcurl/
        jansson                 http://www.digip.org/jansson/
                (jansson is included in-tree)
```

so its recommended to read README file, any how lets try to search and install libcurl:

```
root@server1:~/cpuminer/cpuminer-2.5.0# apt search libcurl
Sorting... Done
Full Text Search... Done
fp-units-net/xenial 3.0.0+dfsg-2 amd64
  Free Pascal - networking units dependency package

fp-units-net-3.0.0/xenial 3.0.0+dfsg-2 amd64
  Free Pascal - networking units

gnupg/xenial-updates,xenial-security,now 1.4.20-1ubuntu3.1 amd64 [installed]
  GNU privacy guard - a free PGP replacement

gnupg-curl/xenial-updates,xenial-security 1.4.20-1ubuntu3.1 amd64
  GNU privacy guard - a free PGP replacement (cURL)

libcupt4-1-downloadmethod-curl/xenial 2.9.4ubuntu1 amd64
  flexible package manager -- libcurl download method

libcurl-ocaml/xenial 0.5.3-2build5 amd64
  OCaml curl bindings (Runtime Library)

libcurl-ocaml-dev/xenial 0.5.3-2build5 amd64
  OCaml libcurl bindings (Development package)

libcurl3/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64 [upgradable from: 7.47.0-1ubuntu2.5]
  easy-to-use client-side URL transfer library (OpenSSL flavour)

libcurl3-dbg/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  debugging symbols for libcurl (OpenSSL, GnuTLS and NSS flavours)

libcurl3-gnutls/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64 [upgradable from: 7.47.0-1ubuntu2.5]
  easy-to-use client-side URL transfer library (GnuTLS flavour)

libcurl3-nss/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  easy-to-use client-side URL transfer library (NSS flavour)

libcurl4-doc/xenial-updates,xenial-updates,xenial-security,xenial-security 7.47.0-1ubuntu2.6 all
  documentation for libcurl

libcurl4-gnutls-dev/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  development files and documentation for libcurl (GnuTLS flavour)

libcurl4-nss-dev/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  development files and documentation for libcurl (NSS flavour)

libcurl4-openssl-dev/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  development files and documentation for libcurl (OpenSSL flavour)

libcurlpp-dev/xenial 0.7.3-6 amd64
  c++ wrapper for libcurl (development files)

libcurlpp0/xenial 0.7.3-6 amd64
  c++ wrapper for libcurl

libghc-curl-dev/xenial 1.3.8-7 amd64
  GHC libraries for the libcurl Haskell bindings

libghc-curl-doc/xenial,xenial 1.3.8-7 all
  Documentation for the libcurl Haskell bindings; documentation

libghc-curl-prof/xenial 1.3.8-7 amd64
  Profiling libraries for the libcurl Haskell bindings; profiling libraries

libghc-hxt-curl-dev/xenial 9.1.1.1-4build1 amd64
  LibCurl interface for HXT

libghc-hxt-curl-doc/xenial,xenial 9.1.1.1-4build1 all
  LibCurl interface for HXT; documentation

libghc-hxt-curl-prof/xenial 9.1.1.1-4build1 amd64
  LibCurl interface for HXT; profiling libraries

libjsonrpccpp-client0/xenial 0.6.0-2build1 amd64
  library implementing json-rpc C++ clients

libjsonrpccpp-client0-dbg/xenial 0.6.0-2build1 amd64
  debugging symbols for libjsonrpccpp-client0

libresource-retriever-dev/xenial 1.11.6-2 amd64
  Robot OS resource_retriever library - development files

libresource-retriever0d/xenial 1.11.6-2 amd64
  Robot OS resource_retriever library

libstrongswan-extra-plugins/xenial-updates 5.3.5-1ubuntu3.5 amd64
  strongSwan utility and crypto library (extra plugins)

libwww-curl-perl/xenial 4.17-2build1 amd64
  Perl bindings to libcurl

lua-curl/xenial 0.3.0-9 amd64
  libcURL bindings for the Lua language

lua-curl-dev/xenial 0.3.0-9 amd64
  libcURL development files for the Lua language

python-pycurl/xenial 7.43.0-1ubuntu1 amd64
  Python bindings to libcurl

python-pycurl-dbg/xenial 7.43.0-1ubuntu1 amd64
  Python bindings to libcurl (debug extension)

python-pycurl-doc/xenial,xenial 7.43.0-1ubuntu1 all
  Python bindings to libcurl (documentation)

python-resource-retriever/xenial,xenial 1.11.6-2 all
  Robot OS resource_retriever library - Python

python3-pycurl/xenial,now 7.43.0-1ubuntu1 amd64 [installed]
  Python bindings to libcurl (Python 3)

python3-pycurl-dbg/xenial 7.43.0-1ubuntu1 amd64
  Python bindings to libcurl (debug extension, Python 3)

ruby-curb/xenial 0.8.8-1build5 amd64
  Ruby libcurl bindings

ruby-ethon/xenial,xenial 0.8.0-1 all
  libcurl wrapper using ffi

ruby-typhoeus/xenial,xenial 0.8.0-2 all
  parallel HTTP library on top of ethon

strongswan-plugin-curl/xenial-updates,xenial-updates 5.3.5-1ubuntu3.5 all
  strongSwan plugin for the libcurl based HTTP/FTP fetcher

tclcurl/xenial 7.22.0+hg20151017-1 amd64
  Tcl bindings to libcurl

wmget/xenial 0.6.1-1 amd64
  Background download manager in a Window Maker dock app
```

Wow. That is a long list , Usually libraries with "-dev" on their tails are what we want, so we chose one and try:

```
root@server1:~/cpuminer/cpuminer-2.5.0# apt search libcurl | grep dev

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

libcurl-ocaml-dev/xenial 0.5.3-2build5 amd64
libcurl4-gnutls-dev/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  development files and documentation for libcurl (GnuTLS flavour)
libcurl4-nss-dev/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  development files and documentation for libcurl (NSS flavour)
libcurl4-openssl-dev/xenial-updates,xenial-security 7.47.0-1ubuntu2.6 amd64
  development files and documentation for libcurl (OpenSSL flavour)
libcurlpp-dev/xenial 0.7.3-6 amd64
  c++ wrapper for libcurl (development files)
libghc-curl-dev/xenial 1.3.8-7 amd64
libghc-hxt-curl-dev/xenial 9.1.1.1-4build1 amd64
libresource-retriever-dev/xenial 1.11.6-2 amd64
  Robot OS resource_retriever library - development files
lua-curl-dev/xenial 0.3.0-9 amd64
  libcURL development files for the Lua language

root@server1:~/cpuminer/cpuminer-2.5.0# apt install libcurl4-openssl-dev
root@server1:~/cpuminer/cpuminer-2.5.0# apt install build-essential
```

Now run configure again and watch for any problems:

```
root@server1:~/cpuminer/cpuminer-2.5.0# ./configure 
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking target system type... x86_64-unknown-linux-gnu
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking whether to enable maintainer-specific portions of Makefiles... no
checking for style of include used by make... GNU
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking whether gcc understands -c and -o together... yes
checking dependency style of gcc... gcc3
checking for gcc option to accept ISO C99... none needed
checking how to run the C preprocessor... gcc -E
checking for grep that handles long lines and -e... /bin/grep
checking for egrep... /bin/grep -E
checking whether gcc needs -traditional... no
checking dependency style of gcc... gcc3
checking for ranlib... ranlib
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking sys/endian.h usability... no
checking sys/endian.h presence... no
checking for sys/endian.h... no
checking sys/param.h usability... yes
checking sys/param.h presence... yes
checking for sys/param.h... yes
checking syslog.h usability... yes
checking syslog.h presence... yes
checking for syslog.h... yes
checking for sys/sysctl.h... yes
checking whether be32dec is declared... no
checking whether le32dec is declared... no
checking whether be32enc is declared... no
checking whether le32enc is declared... no
checking for size_t... yes
checking for working alloca.h... yes
checking for alloca... yes
checking for getopt_long... yes
checking whether we can compile AVX code... yes
checking whether we can compile XOP code... yes
checking whether we can compile AVX2 code... yes
checking for json_loads in -ljansson... no
checking for pthread_create in -lpthread... yes
checking for gawk... (cached) mawk
checking for curl-config... /usr/bin/curl-config
checking for the version of libcurl... 7.47.0
checking for libcurl >= version 7.15.2... yes
checking whether libcurl is usable... yes
checking for curl_free... yes
checking that generated files are newer than configure... done
configure: creating ./config.status
config.status: creating Makefile
config.status: creating compat/Makefile
config.status: creating compat/jansson/Makefile
config.status: creating cpuminer-config.h
config.status: executing depfiles commands
```

it seems every thing is okey, and it generates some file for compiling :

```
-rwxr-xr-x 1 root  root   35K Feb  4 04:08 config.status
-rw-r--r-- 1 root  root   52K Feb  4 04:08 Makefile
drwxr-xr-x 3 USER1 USER1 4.0K Feb  4 04:08 compat
-rw-r--r-- 1 root  root  5.3K Feb  4 04:08 cpuminer-config.h
-rw-r--r-- 1 root  root    32 Feb  4 04:08 stamp-h1
-rw-r--r-- 1 root  root   33K Feb  4 04:08 config.log
```

time to compile:

```
root@server1:~/cpuminer/cpuminer-2.5.0# make
make  all-recursive
make[1]: Entering directory '/root/cpuminer/cpuminer-2.5.0'
Making all in compat
make[2]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat'
Making all in jansson
make[3]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat/jansson'
gcc -DHAVE_CONFIG_H -I. -I../..     -g -O2 -MT dump.o -MD -MP -MF .deps/dump.Tpo -c -o dump.o dump.c
mv -f .deps/dump.Tpo .deps/dump.Po
gcc -DHAVE_CONFIG_H -I. -I../..     -g -O2 -MT hashtable.o -MD -MP -MF .deps/hashtable.Tpo -c -o hashtable.o hashtable.c
mv -f .deps/hashtable.Tpo .deps/hashtable.Po
gcc -DHAVE_CONFIG_H -I. -I../..     -g -O2 -MT load.o -MD -MP -MF .deps/load.Tpo -c -o load.o load.c
mv -f .deps/load.Tpo .deps/load.Po
gcc -DHAVE_CONFIG_H -I. -I../..     -g -O2 -MT strbuffer.o -MD -MP -MF .deps/strbuffer.Tpo -c -o strbuffer.o strbuffer.c
mv -f .deps/strbuffer.Tpo .deps/strbuffer.Po
gcc -DHAVE_CONFIG_H -I. -I../..     -g -O2 -MT utf.o -MD -MP -MF .deps/utf.Tpo -c -o utf.o utf.c
mv -f .deps/utf.Tpo .deps/utf.Po
gcc -DHAVE_CONFIG_H -I. -I../..     -g -O2 -MT value.o -MD -MP -MF .deps/value.Tpo -c -o value.o value.c
mv -f .deps/value.Tpo .deps/value.Po
rm -f libjansson.a
ar cru libjansson.a dump.o hashtable.o load.o strbuffer.o utf.o value.o 
ar: `u' modifier ignored since `D' is the default (see `U')
ranlib libjansson.a
make[3]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat/jansson'
make[3]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[3]: Nothing to be done for 'all-am'.
make[3]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[2]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[2]: Entering directory '/root/cpuminer/cpuminer-2.5.0'
gcc -DHAVE_CONFIG_H -I.  -I./compat/jansson -pthread  -fno-strict-aliasing -g -O2 -MT minerd-cpu-miner.o -MD -MP -MF .deps/minerd-cpu-miner.Tpo -c -o minerd-cpu-miner.o `test -f 'cpu-miner.c' || echo './'`cpu-miner.c
mv -f .deps/minerd-cpu-miner.Tpo .deps/minerd-cpu-miner.Po
gcc -DHAVE_CONFIG_H -I.  -I./compat/jansson -pthread  -fno-strict-aliasing -g -O2 -MT minerd-util.o -MD -MP -MF .deps/minerd-util.Tpo -c -o minerd-util.o `test -f 'util.c' || echo './'`util.c
mv -f .deps/minerd-util.Tpo .deps/minerd-util.Po
gcc -DHAVE_CONFIG_H -I.  -I./compat/jansson -pthread  -fno-strict-aliasing -g -O2 -MT minerd-sha2.o -MD -MP -MF .deps/minerd-sha2.Tpo -c -o minerd-sha2.o `test -f 'sha2.c' || echo './'`sha2.c
mv -f .deps/minerd-sha2.Tpo .deps/minerd-sha2.Po
gcc -DHAVE_CONFIG_H -I.  -I./compat/jansson -pthread  -fno-strict-aliasing -g -O2 -MT minerd-scrypt.o -MD -MP -MF .deps/minerd-scrypt.Tpo -c -o minerd-scrypt.o `test -f 'scrypt.c' || echo './'`scrypt.c
mv -f .deps/minerd-scrypt.Tpo .deps/minerd-scrypt.Po
gcc -DHAVE_CONFIG_H -I.  -I./compat/jansson -pthread   -g -O2 -MT minerd-sha2-x64.o -MD -MP -MF .deps/minerd-sha2-x64.Tpo -c -o minerd-sha2-x64.o `test -f 'sha2-x64.S' || echo './'`sha2-x64.S
mv -f .deps/minerd-sha2-x64.Tpo .deps/minerd-sha2-x64.Po
gcc -DHAVE_CONFIG_H -I.  -I./compat/jansson -pthread   -g -O2 -MT minerd-scrypt-x64.o -MD -MP -MF .deps/minerd-scrypt-x64.Tpo -c -o minerd-scrypt-x64.o `test -f 'scrypt-x64.S' || echo './'`scrypt-x64.S
mv -f .deps/minerd-scrypt-x64.Tpo .deps/minerd-scrypt-x64.Po
gcc -fno-strict-aliasing -g -O2 -pthread  -o minerd minerd-cpu-miner.o minerd-util.o minerd-sha2.o minerd-scrypt.o  minerd-sha2-x64.o minerd-scrypt-x64.o   -L/usr/lib/x86_64-linux-gnu -lcurl compat/jansson/libjansson.a -lpthread  
make[2]: Leaving directory '/root/cpuminer/cpuminer-2.5.0'
make[1]: Leaving directory '/root/cpuminer/cpuminer-2.5.0'
```

All required binary files have been created:

```
-rw-r--r-- 1 root  root  261K Feb  4 04:18 minerd-cpu-miner.o
-rw-r--r-- 1 root  root  164K Feb  4 04:18 minerd-util.o
-rw-r--r-- 1 root  root   43K Feb  4 04:18 minerd-sha2.o
-rw-r--r-- 1 root  root  112K Feb  4 04:18 minerd-scrypt.o
-rw-r--r-- 1 root  root  138K Feb  4 04:18 minerd-sha2-x64.o
-rw-r--r-- 1 root  root   71K Feb  4 04:18 minerd-scrypt-x64.o
-rwxr-xr-x 1 root  root  664K Feb  4 04:18 minerd
```

see minerd, it works but its not in the path:

```
root@server1:~/cpuminer/cpuminer-2.5.0# ./minerd
./minerd: no URL supplied
Try `minerd --help' for more information.

root@server1:~/cpuminer/cpuminer-2.5.0# minerd
minerd: command not found
```

Next step is installing compiled program. In this step PATHis modified and generated files are placed in /usr/local/bin

```
root@server1:~/cpuminer/cpuminer-2.5.0# make install
Making install in compat
make[1]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat'
Making install in jansson
make[2]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat/jansson'
make[3]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat/jansson'
make[3]: Nothing to be done for 'install-exec-am'.
make[3]: Nothing to be done for 'install-data-am'.
make[3]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat/jansson'
make[2]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat/jansson'
make[2]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[3]: Entering directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[3]: Nothing to be done for 'install-exec-am'.
make[3]: Nothing to be done for 'install-data-am'.
make[3]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[2]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[1]: Leaving directory '/root/cpuminer/cpuminer-2.5.0/compat'
make[1]: Entering directory '/root/cpuminer/cpuminer-2.5.0'
make[2]: Entering directory '/root/cpuminer/cpuminer-2.5.0'
 /bin/mkdir -p '/usr/local/bin'
  /usr/bin/install -c minerd '/usr/local/bin'
 /bin/mkdir -p '/usr/local/share/man/man1'
 /usr/bin/install -c -m 644 minerd.1 '/usr/local/share/man/man1'
make[2]: Leaving directory '/root/cpuminer/cpuminer-2.5.0'
make[1]: Leaving directory '/root/cpuminer/cpuminer-2.5.0'

root@server1:~/cpuminer/cpuminer-2.5.0# minerd
minerd: no URL supplied
Try `minerd --help' for more information.
```

Okey lets start bitcoin mining:

```
root@server1:~/cpuminer/cpuminer-2.5.0# minerd -a sha256d -o stratum+tcp://stratum.antpool.com:3333 -u MyName.1 -p 123
[2018-02-04 04:44:03] Starting Stratum on stratum+tcp://stratum.antpool.com:3333
[2018-02-04 04:44:03] Binding thread 0 to cpu 0
[2018-02-04 04:44:03] 4 miner threads started, using 'sha256d' algorithm.
[2018-02-04 04:44:03] Binding thread 1 to cpu 1
[2018-02-04 04:44:03] Binding thread 3 to cpu 3
[2018-02-04 04:44:03] Binding thread 2 to cpu 2
```
