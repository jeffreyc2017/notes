# MINIX

## A workable setup for building MINIX3 on MINIX itself

1. Download the iso compressed file then check the md5sum.

   ```sh
   $ md5sum minix_R3.3.0-588a35b.iso.bz2
   3234ffcebfb2a28069cf3def41c95dec
   ```

2. Virtual Box 7.0.10 on Ubuntu 22.04

3. Post-installation of Minix3

   - <https://wiki.minix3.org/doku.php?id=usersguide:postinstallation>

   switch to NAT on Virtual Box or pkgin will be stalled

   ```sh

   pkgin update
   pkgin install openssh
   cp /usr/pkg/etc/rc.d/sshd /etc/rc.d/
   printf 'sshd=YES\n' >> /etc/rc.conf
   /etc/rc.d/sshd start
   pkgin install curl binutils bmake gmake clang
   ```

   - <https://wiki.minix3.org/doku.php?id=developersguide:rebuildingsystem>

   switch to Bridged Adaptor to scp the minix source code from the host:

   ```sh
   scp -r usr@192.168.1.100:~/minix /usr/src
   cd /usr/src/minix
   make build

   ```

   ```sh
   # This will install git-base which has issues as below
   # pkgin_sets

   # cd /usr/pkg/etc/rc.d/
   # rm kadmind kcm kdc kpasswdd # these files may cause a bunch of errors, see https://groups.google.com/forum/#!topic/minix3/HrQe7xJbnDQ
   # git clone git://git.minix3.org/minix /usr/src # https://groups.google.com/g/minix3/c/av5gZMLO6y8

   # Have issues with git clone, remote hang up unexpectedly. Use scp from local instead
   # git clone git@github.com:Stichting-MINIX-Research-Foundation/minix.git /usr/src # You'll have to set up a ssh key and add the public key to github: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
   cd /usr/src
   # Do this on the host if git doesn't work normally
   # git checkout v3.3.0 # we do not want to mess with bleeding edge code (do we?)
   ```

## Cross-compile on Ubuntu 22.04 LTS

It works with this env:

```sh
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.4 LTS
Release:        22.04
Codename:       jammy
$ gcc --version
gcc (Ubuntu 9.5.0-1ubuntu1~22.04) 9.5.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

$ g++ --version
g++ (Ubuntu 9.5.0-1ubuntu1~22.04) 9.5.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

```sh
sudo apt install gcc-10
sudo apt install g++-10
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 10
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 10
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 11
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-11 11
sudo apt install gcc-12
sudo apt install g++-12
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 12
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 12
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
```

```sh
$ sudo update-alternatives --config gcc
There are 6 choices for the alternative gcc (providing /usr/bin/gcc).

  Selection    Path             Priority   Status
------------------------------------------------------------
  0            /usr/bin/gcc-12   12        auto mode
  1            /usr/bin/gcc-10   10        manual mode
  2            /usr/bin/gcc-11   11        manual mode
  3            /usr/bin/gcc-12   12        manual mode
  4            /usr/bin/gcc-7    7         manual mode
  5            /usr/bin/gcc-8    8         manual mode
* 6            /usr/bin/gcc-9    9         manual mode

Press <enter> to keep the current choice[*], or type selection number:
$ sudo update-alternatives --config g++
There are 4 choices for the alternative g++ (providing /usr/bin/g++).

  Selection    Path             Priority   Status
------------------------------------------------------------
  0            /usr/bin/g++-12   12        auto mode
  1            /usr/bin/g++-10   10        manual mode
  2            /usr/bin/g++-11   11        manual mode
  3            /usr/bin/g++-12   12        manual mode
* 4            /usr/bin/g++-9    9         manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

- <https://groups.google.com/g/minix3/c/A_VKJdj_Pa8>
- <https://linuxconfig.org/how-to-switch-between-multiple-gcc-and-g-compiler-versions-on-ubuntu-20-04-lts-focal-fossa>

## On macOS

Not successful with macOS. Only note the setup of gcc-9 below:

1. Install the GNU gcc compiler:

   ```sh
   brew install gcc@9
   ```

2. After installation:

   ```sh
   % which gcc-9
   /usr/local/bin/gcc-9
   % which g++-9
   /usr/local/bin/g++-9
   ```

3. Edit the shell config file to change the default compiler:

   ```sh
   vim ~/.zshrc
   ```

   ```sh
   % head ~/.zshrc
   # For GNU gcc & g++
   # start >>>
   alias gcc="gcc-9"
   alias g++="g++-9"
   alias cc="gcc-9"
   alias c++="g++-9"
   alias clang="gcc-9"
   alias clang++="g++-9"
   # end <<<
   ```

   Enable the changes:

   ```sh
   source ~/.zshrc
   ```

## Patches

- <https://gist.github.com/petershh/1973b8f723fb36242c9558a23408d469>

## References

- [minix3](https://www.minix3.org)
- [code](https://github.com/Stichting-MINIX-Research-Foundation/minix)
- [wiki](https://en.wikipedia.org/wiki/Minix)
- <https://gist.github.com/Drowze/2f7cbce35ade1fa94b2511f4138a32c2>
- [build in Minix](https://wiki.minix3.org/doku.php?id=releases:3.2.1:developersguide:trackingcurrent)
- [issue with git clone](https://groups.google.com/g/minix3/c/av5gZMLO6y8)
- [The future of MINIX](https://groups.google.com/g/minix3/c/nUG1NwxXXkg)

## Code understanding

- [ITEC856 Self-study Units](http://web.science.mq.edu.au/~mike/itec856/os/selfstud.html)
- [MINIX 2](https://minix1.woodhull.com/docs.html)
- [MINIX 3](https://wiki.minix3.org/doku.php?id=developersguide:start)

## Code structure

> [!NOTE]
> Use 8 spaces for tabs when reading the MINIX code.

```text
.
├── LICENSE
├── Makefile
├── Makefile.inc
├── bin
│   ├── Makefile
│   ├── Makefile.inc
│   ├── cat
│   ├── chmod
│   ├── cp
│   ├── date
│   ├── df
│   ├── echo
│   ├── ed
│   ├── expr
│   ├── hostname
│   ├── kill
│   ├── ksh
│   ├── ln
│   ├── ls
│   ├── mkdir
│   ├── mv
│   ├── pax
│   ├── ps
│   ├── pwd
│   ├── rm
│   ├── rmdir
│   ├── sh
│   ├── sleep
│   ├── stty
│   ├── sync
│   └── test
├── build.sh
├── common
│   ├── dist
│   ├── include
│   └── lib
├── distrib
│   ├── Makefile
│   ├── Makefile.inc
│   ├── common
│   └── sets
├── docs
│   ├── UPDATING
│   └── profiling.txt
├── etc
│   ├── Makefile
│   ├── Makefile.params
│   ├── ast
│   ├── boot.cfg.default
│   ├── crontab
│   ├── defaults
│   ├── devmand
│   ├── fonts
│   ├── gettytab
│   ├── group
│   ├── hostname.file
│   ├── inet.conf
│   ├── man.conf
│   ├── master.passwd
│   ├── mk.conf
│   ├── motd
│   ├── mtree
│   ├── newfstab.sh
│   ├── profile
│   ├── protocols
│   ├── rc
│   ├── rc.capes
│   ├── rc.cd
│   ├── rc.conf
│   ├── rc.daemons.dist
│   ├── rc.shutdown
│   ├── rc.subr
│   ├── resolv.conf
│   ├── rs.inet
│   ├── rs.single
│   ├── services
│   ├── shells
│   ├── syslog.conf
│   ├── system.conf
│   ├── termcap
│   ├── termcap.big
│   ├── ttys
│   ├── usr
│   └── utmp
├── external
│   ├── Makefile
│   ├── README
│   ├── bsd
│   ├── gpl3
│   ├── historical
│   ├── lgpl3
│   ├── mit
│   └── public-domain
├── games
│   ├── Makefile
│   ├── Makefile.inc
│   ├── adventure
│   ├── arithmetic
│   ├── bcd
│   ├── colorbars
│   ├── factor
│   ├── fortune
│   ├── monop
│   ├── morse
│   ├── number
│   ├── pig
│   ├── ppt
│   ├── primes
│   ├── random
│   ├── tetris
│   └── wargames
├── gnu
│   ├── Makefile
│   ├── README
│   ├── dist
│   └── usr.bin
├── include
│   ├── Makefile
│   ├── a.out.h
│   ├── aio.h
│   ├── ar.h
│   ├── arpa
│   ├── assert.h
│   ├── atomic.h
│   ├── bitstring.h
│   ├── bm.h
│   ├── cdbr.h
│   ├── cdbw.h
│   ├── complex.h
│   ├── cpio.h
│   ├── ctype.h
│   ├── db.h
│   ├── dirent.h
│   ├── disktab.h
│   ├── dlfcn.h
│   ├── err.h
│   ├── errno.h
│   ├── fenv.h
│   ├── fmtmsg.h
│   ├── fnmatch.h
│   ├── fstab.h
│   ├── fts.h
│   ├── ftw.h
│   ├── getopt.h
│   ├── glob.h
│   ├── grp.h
│   ├── hesiod.h
│   ├── iconv.h
│   ├── ieeefp.h
│   ├── ifaddrs.h
│   ├── inttypes.h
│   ├── iso646.h
│   ├── kvm.h
│   ├── langinfo.h
│   ├── libgen.h
│   ├── limits.h
│   ├── link.h
│   ├── link_aout.h
│   ├── link_elf.h
│   ├── locale.h
│   ├── login_cap.h
│   ├── lwp.h
│   ├── malloc.h
│   ├── math.h
│   ├── md2.h
│   ├── memory.h
│   ├── mntopts.h
│   ├── monetary.h
│   ├── mpool.h
│   ├── mqueue.h
│   ├── ndbm.h
│   ├── netconfig.h
│   ├── netdb.h
│   ├── netgroup.h
│   ├── nl_types.h
│   ├── nlist.h
│   ├── nsswitch.h
│   ├── paths.h
│   ├── protocols
│   ├── pwd.h
│   ├── quota.h
│   ├── randomid.h
│   ├── ranlib.h
│   ├── re_comp.h
│   ├── regex.h
│   ├── regexp.h
│   ├── res_update.h
│   ├── resolv.h
│   ├── rmt.h
│   ├── rpc
│   ├── rpcsvc
│   ├── sched.h
│   ├── search.h
│   ├── semaphore.h
│   ├── setjmp.h
│   ├── sgtty.h
│   ├── signal.h
│   ├── spawn.h
│   ├── ssp
│   ├── stab.h
│   ├── stdbool.h
│   ├── stddef.h
│   ├── stdio.h
│   ├── stdlib.h
│   ├── string.h
│   ├── stringlist.h
│   ├── strings.h
│   ├── struct.h
│   ├── sysexits.h
│   ├── tar.h
│   ├── tgmath.h
│   ├── time.h
│   ├── ttyent.h
│   ├── tzfile.h
│   ├── ucontext.h
│   ├── ulimit.h
│   ├── unistd.h
│   ├── util.h
│   ├── utime.h
│   ├── utmp.h
│   ├── utmpx.h
│   ├── uuid.h
│   ├── vis.h
│   ├── wchar.h
│   ├── wctype.h
│   └── wordexp.h
├── lib
│   ├── Makefile
│   ├── Makefile.inc
│   ├── bumpversion
│   ├── checkoldver
│   ├── checkver
│   ├── checkvers
│   ├── csu
│   ├── libbz2
│   ├── libc
│   ├── libc_vfp
│   ├── libcrypt
│   ├── libcurses
│   ├── libedit
│   ├── libexecinfo
│   ├── libform
│   ├── libm
│   ├── libmenu
│   ├── libprop
│   ├── libpuffs
│   ├── librefuse
│   ├── librmt
│   ├── libterminfo
│   ├── libutil
│   ├── libz
│   └── lua
├── libexec
│   ├── Makefile
│   ├── Makefile.inc
│   ├── fingerd
│   ├── ftpd
│   ├── getty
│   ├── ld.elf_so
│   └── makewhatis
├── minix
│   ├── Makefile
│   ├── Makefile.inc
│   ├── benchmarks
│   ├── bin
│   ├── commands
│   ├── drivers
│   ├── fs
│   ├── include
│   ├── kernel
│   ├── lib
│   ├── llvm
│   ├── man
│   ├── net
│   ├── sbin
│   ├── servers
│   ├── share
│   ├── tests
│   ├── usr.bin
│   └── usr.sbin
├── releasetools
│   ├── Makefile
│   ├── arm_sdimage.sh
│   ├── fetch_u-boot.sh
│   ├── gen_uEnv.txt.sh
│   ├── issue.install
│   ├── mkboot
│   ├── packages.install
│   ├── release
│   ├── release.functions
│   ├── release.sh
│   ├── sort_set.sh
│   └── x86_hdimage.sh
├── sbin
│   ├── Makefile
│   ├── Makefile.inc
│   ├── chown
│   ├── fsck
│   ├── fsck_ext2fs
│   ├── init
│   ├── mknod
│   ├── newfs_ext2fs
│   ├── nologin
│   ├── ping
│   ├── reboot
│   └── shutdown
├── share
│   ├── Makefile
│   ├── Makefile.inc
│   ├── misc
│   ├── mk
│   ├── terminfo
│   └── zoneinfo
├── sys
│   ├── Makefile
│   ├── arch
│   ├── compat
│   ├── conf
│   ├── dev
│   ├── external
│   ├── fs
│   ├── lib
│   ├── net
│   ├── netinet
│   ├── netinet6
│   ├── sys
│   ├── ufs
│   └── uvm
├── tests
│   ├── Makefile
│   ├── Makefile.inc
│   ├── README
│   ├── bin
│   ├── crypto
│   ├── dev
│   ├── fs
│   ├── games
│   ├── h_macros.h
│   ├── include
│   ├── ipf
│   ├── kernel
│   ├── lib
│   ├── libexec
│   ├── modules
│   ├── net
│   ├── rump
│   ├── sbin
│   ├── share
│   ├── sys
│   ├── usr.bin
│   └── usr.sbin
├── tools
│   ├── Makefile
│   ├── Makefile.gmakehost
│   ├── Makefile.gnuhost
│   ├── Makefile.gnuwrap
│   ├── Makefile.host
│   ├── Makefile.nbincludes
│   ├── awk
│   ├── binstall
│   ├── binutils
│   ├── cat
│   ├── cksum
│   ├── compat
│   ├── file
│   ├── gcc
│   ├── genassym
│   ├── gmake
│   ├── gmp
│   ├── headerlist
│   ├── host-mkdep
│   ├── installboot
│   ├── join
│   ├── lex
│   ├── llvm
│   ├── llvm-clang
│   ├── llvm-clang-tblgen
│   ├── llvm-include
│   ├── llvm-lib
│   ├── llvm-librt
│   ├── llvm-lld
│   ├── llvm-mcld
│   ├── llvm-tblgen
│   ├── lorder
│   ├── m4
│   ├── make
│   ├── makewhatis
│   ├── mandoc
│   ├── mkdep
│   ├── mkfs.mfs
│   ├── mkheaderlist.sh
│   ├── mknod
│   ├── mkproto
│   ├── mktemp
│   ├── mpc
│   ├── mpfr
│   ├── mtree
│   ├── nbperf
│   ├── partition
│   ├── pax
│   ├── pwd_mkdb
│   ├── sed
│   ├── stat
│   ├── strfile
│   ├── texinfo
│   ├── tic
│   ├── toproto
│   ├── tsort
│   ├── writeisofs
│   ├── yacc
│   └── zic
├── usr.bin
│   ├── Makefile
│   ├── Makefile.inc
│   ├── apropos
│   ├── asa
│   ├── banner
│   ├── basename
│   ├── bdes
│   ├── bzip2
│   ├── bzip2recover
│   ├── cal
│   ├── calendar
│   ├── checknr
│   ├── chpass
│   ├── cksum
│   ├── col
│   ├── colcrt
│   ├── colrm
│   ├── column
│   ├── comm
│   ├── csplit
│   ├── ctags
│   ├── cut
│   ├── deroff
│   ├── dirname
│   ├── du
│   ├── env
│   ├── expand
│   ├── false
│   ├── finger
│   ├── fold
│   ├── fpr
│   ├── from
│   ├── fsplit
│   ├── ftp
│   ├── genassym
│   ├── getopt
│   ├── gzip
│   ├── head
│   ├── hexdump
│   ├── id
│   ├── indent
│   ├── infocmp
│   ├── join
│   ├── jot
│   ├── lam
│   ├── last
│   ├── ldd
│   ├── leave
│   ├── lock
│   ├── login
│   ├── logname
│   ├── lorder
│   ├── m4
│   ├── machine
│   ├── make
│   ├── man
│   ├── menuc
│   ├── mesg
│   ├── mkdep
│   ├── mkfifo
│   ├── mkstr
│   ├── mktemp
│   ├── msgc
│   ├── nbperf
│   ├── newgrp
│   ├── nice
│   ├── nl
│   ├── nohup
│   ├── passwd
│   ├── paste
│   ├── patch
│   ├── pathchk
│   ├── pr
│   ├── printenv
│   ├── printf
│   ├── pwhash
│   ├── renice
│   ├── rev
│   ├── sdiff
│   ├── sed
│   ├── seq
│   ├── shar
│   ├── shlock
│   ├── shuffle
│   ├── soelim
│   ├── sort
│   ├── split
│   ├── stat
│   ├── su
│   ├── tail
│   ├── tee
│   ├── tic
│   ├── touch
│   ├── tput
│   ├── tr
│   ├── true
│   ├── tsort
│   ├── tty
│   ├── ul
│   ├── uname
│   ├── unexpand
│   ├── unifdef
│   ├── uniq
│   ├── units
│   ├── unvis
│   ├── unzip
│   ├── users
│   ├── uudecode
│   ├── uuencode
│   ├── uuidgen
│   ├── vis
│   ├── w
│   ├── wall
│   ├── wc
│   ├── what
│   ├── whatis
│   ├── whereis
│   ├── who
│   ├── whois
│   ├── write
│   ├── xargs
│   ├── xinstall
│   ├── xstr
│   └── yes
└── usr.sbin
    ├── Makefile
    ├── Makefile.inc
    ├── chroot
    ├── i2cscan
    ├── installboot
    ├── link
    ├── mtree
    ├── postinstall
    ├── pwd_mkdb
    ├── rdate
    ├── traceroute
    ├── unlink
    ├── user
    ├── vipw
    ├── vnconfig
    └── zic

360 directories, 194 files
```

minix:

```text
.
├── Makefile
├── Makefile.inc
├── benchmarks
│   ├── Makefile
│   ├── Makefile.inc
│   ├── run
│   └── unixbench-5.1.2
├── bin
│   ├── Makefile
│   └── Makefile.inc
├── commands
│   ├── DESCRIBE
│   ├── MAKEDEV
│   ├── Makefile
│   ├── Makefile.inc
│   ├── add_route
│   ├── arp
│   ├── at
│   ├── atnormalize
│   ├── autopart
│   ├── backup
│   ├── btrace
│   ├── cawf
│   ├── cdprobe
│   ├── ci
│   ├── cleantmp
│   ├── cmp
│   ├── co
│   ├── compress
│   ├── crc
│   ├── cron
│   ├── crontab
│   ├── dd
│   ├── decomp16
│   ├── devmand
│   ├── devsize
│   ├── dhcpd
│   ├── dhrystone
│   ├── diff
│   ├── diskctl
│   ├── dosread
│   ├── eject
│   ├── fbdctl
│   ├── fdisk
│   ├── fetch
│   ├── find
│   ├── fix
│   ├── format
│   ├── fsck.mfs
│   ├── gcov-pull
│   ├── grep
│   ├── host
│   ├── hostaddr
│   ├── ifconfig
│   ├── ifdef
│   ├── intr
│   ├── ipcrm
│   ├── ipcs
│   ├── irdpd
│   ├── isoread
│   ├── loadfont
│   ├── loadkeys
│   ├── loadramdisk
│   ├── logger
│   ├── look
│   ├── lp
│   ├── lpd
│   ├── lspci
│   ├── mail
│   ├── mined
│   ├── mount
│   ├── mt
│   ├── netconf
│   ├── nonamed
│   ├── part
│   ├── partition
│   ├── pkgin_all
│   ├── pkgin_cd
│   ├── pkgin_sets
│   ├── playwave
│   ├── postinstall
│   ├── pr_routes
│   ├── prep
│   ├── printroot
│   ├── profile
│   ├── progressbar
│   ├── ps
│   ├── pwdauth
│   ├── ramdisk
│   ├── rarpd
│   ├── rawspeed
│   ├── rcp
│   ├── readclock
│   ├── recwave
│   ├── remsync
│   ├── repartition
│   ├── rget
│   ├── rlogin
│   ├── rotate
│   ├── rsh
│   ├── rshd
│   ├── screendump
│   ├── service
│   ├── setup
│   ├── slip
│   ├── spell
│   ├── sprofalyze
│   ├── sprofdiff
│   ├── srccrc
│   ├── svclog
│   ├── svrctl
│   ├── swifi
│   ├── synctree
│   ├── sysenv
│   ├── syslogd
│   ├── tcpd
│   ├── tcpdp
│   ├── tcpstat
│   ├── telnet
│   ├── telnetd
│   ├── term
│   ├── termcap
│   ├── tget
│   ├── time
│   ├── truncate
│   ├── udpstat
│   ├── umount
│   ├── update
│   ├── update_bootcfg
│   ├── updateboot
│   ├── version
│   ├── vol
│   ├── worldstone
│   ├── writeisofs
│   ├── zdump
│   └── zmodem
├── drivers
│   ├── Makefile
│   ├── Makefile.inc
│   ├── audio
│   ├── bus
│   ├── clock
│   ├── eeprom
│   ├── examples
│   ├── hid
│   ├── iommu
│   ├── net
│   ├── power
│   ├── printer
│   ├── sensors
│   ├── storage
│   ├── system
│   ├── tty
│   ├── usb
│   ├── video
│   └── vmm_guest
├── fs
│   ├── Makefile
│   ├── Makefile.inc
│   ├── ext2
│   ├── hgfs
│   ├── iso9660fs
│   ├── mfs
│   ├── pfs
│   ├── procfs
│   └── vbfs
├── include
│   ├── Makefile
│   ├── arch
│   ├── configfile.h
│   ├── ddekit
│   ├── env.h
│   ├── fetch.h
│   ├── lib.h
│   ├── libdde
│   ├── libutil.h
│   ├── minix
│   ├── net
│   ├── sys
│   └── varargs.h
├── kernel
│   ├── Makefile
│   ├── arch
│   ├── clock.c
│   ├── clock.h
│   ├── config.h
│   ├── const.h
│   ├── cpulocals.c
│   ├── cpulocals.h
│   ├── debug.c
│   ├── debug.h
│   ├── extract-errno.sh
│   ├── extract-mfield.sh
│   ├── extract-mtype.sh
│   ├── glo.h
│   ├── interrupt.c
│   ├── interrupt.h
│   ├── ipc.h
│   ├── kernel.h
│   ├── main.c
│   ├── priv.h
│   ├── proc.c
│   ├── proc.h
│   ├── profile.c
│   ├── profile.h
│   ├── proto.h
│   ├── smp.c
│   ├── smp.h
│   ├── spinlock.h
│   ├── system
│   ├── system.c
│   ├── system.h
│   ├── table.c
│   ├── type.h
│   ├── usermapped_data.c
│   ├── utility.c
│   ├── vm.h
│   ├── watchdog.c
│   └── watchdog.h
├── lib
│   ├── Makefile
│   ├── Makefile.inc
│   ├── libasyn
│   ├── libaudiodriver
│   ├── libbdev
│   ├── libblockdriver
│   ├── libc
│   ├── libchardriver
│   ├── libclkconf
│   ├── libddekit
│   ├── libdevman
│   ├── libexec
│   ├── libfetch
│   ├── libgcc_s_empty
│   ├── libgpio
│   ├── libhgfs
│   ├── libi2cdriver
│   ├── libinputdriver
│   ├── liblwip
│   ├── libminc
│   ├── libminixfs
│   ├── libmthread
│   ├── libnetdriver
│   ├── libnetsock
│   ├── libsffs
│   ├── libsys
│   ├── libtimers
│   ├── libusb
│   ├── libvassert
│   ├── libvboxfs
│   ├── libvirtio
│   └── libvtreefs
├── llvm
│   ├── Makefile
│   ├── Makefile.inc
│   ├── build.llvm
│   ├── clientctl
│   ├── common.inc.default
│   ├── configure.llvm
│   ├── generate_gold_plugin.sh
│   ├── minix.inc
│   ├── passes
│   └── relink.llvm
├── man
│   ├── Makefile
│   ├── Makefile.inc
│   ├── man1
│   ├── man1x
│   ├── man2
│   ├── man4
│   ├── man5
│   ├── man7
│   ├── man8
│   └── man9
├── net
│   ├── Makefile
│   ├── Makefile.inc
│   ├── inet
│   └── lwip
├── sbin
│   ├── Makefile
│   └── Makefile.inc
├── servers
│   ├── Makefile
│   ├── Makefile.inc
│   ├── devman
│   ├── ds
│   ├── input
│   ├── ipc
│   ├── is
│   ├── pm           # process manager
│   ├── rs
│   ├── sched
│   ├── vfs
│   └── vm
├── share
│   ├── Makefile
│   ├── Makefile.inc
│   └── beaglebone
├── tests
│   ├── Makefile
│   ├── Makefile.inc
│   ├── blocktest
│   ├── check-install
│   ├── common.c
│   ├── common.h
│   ├── ddekit
│   ├── ds
│   ├── fbdtest
│   ├── ipc
│   ├── kernel
│   ├── magic.h
│   ├── mod.c
│   ├── run
│   ├── safecopy
│   ├── t10a.c
│   ├── t11a.c
│   ├── t11b.c
│   ├── t40a.c
│   ├── t40b.c
│   ├── t40c.c
│   ├── t40d.c
│   ├── t40e.c
│   ├── t40f.c
│   ├── t40g.c
│   ├── t60a.c
│   ├── t60b.c
│   ├── t67a.c
│   ├── t67b.c
│   ├── t68a.c
│   ├── t68b.c
│   ├── test1.c
│   ├── test10.c
│   ├── test11.c
│   ├── test12.c
│   ├── test13.c
│   ├── test14.c
│   ├── test15.c
│   ├── test16.c
│   ├── test17.c
│   ├── test18.c
│   ├── test19.c
│   ├── test2.c
│   ├── test20.c
│   ├── test21.c
│   ├── test22.c
│   ├── test23.c
│   ├── test24.c
│   ├── test25.c
│   ├── test26.c
│   ├── test27.c
│   ├── test28.c
│   ├── test29.c
│   ├── test3.c
│   ├── test30.c
│   ├── test31.c
│   ├── test32.c
│   ├── test33.c
│   ├── test34.c
│   ├── test35.c
│   ├── test36.c
│   ├── test37.c
│   ├── test38.c
│   ├── test39.c
│   ├── test4.c
│   ├── test40.c
│   ├── test41.c
│   ├── test42.c
│   ├── test43.c
│   ├── test44.c
│   ├── test45.c
│   ├── test45.h
│   ├── test46.c
│   ├── test47.c
│   ├── test48.c
│   ├── test49.c
│   ├── test5.c
│   ├── test50.c
│   ├── test51.c
│   ├── test52.c
│   ├── test53.c
│   ├── test54.c
│   ├── test55.c
│   ├── test56.c
│   ├── test57.c
│   ├── test57loop.S
│   ├── test58.c
│   ├── test59.c
│   ├── test6.c
│   ├── test60.c
│   ├── test61.c
│   ├── test62.c
│   ├── test63.c
│   ├── test64.c
│   ├── test65.c
│   ├── test66.c
│   ├── test66expected.h
│   ├── test67.c
│   ├── test68.c
│   ├── test69.c
│   ├── test7.c
│   ├── test70.c
│   ├── test71.c
│   ├── test72.c
│   ├── test73.c
│   ├── test74.c
│   ├── test75.c
│   ├── test76.c
│   ├── test77.c
│   ├── test78.c
│   ├── test79.c
│   ├── test8.c
│   ├── test9.c
│   ├── testcache.c
│   ├── testcache.h
│   ├── testinterp.sh
│   ├── testisofs.sh
│   ├── testkyua.sh
│   ├── testmfs.sh
│   ├── testsh1.sh
│   ├── testsh2.sh
│   ├── testvm.c
│   ├── testvm.conf
│   ├── testvm.h
│   ├── testvnd.sh
│   └── tvnd.c
├── usr.bin
│   ├── Makefile
│   ├── Makefile.inc
│   ├── eepromread
│   ├── ministat
│   ├── top
│   ├── toproto
│   └── w
└── usr.sbin
    ├── Makefile
    ├── Makefile.inc
    ├── mkfs.mfs
    └── mkproto

240 directories, 200 files
```

## Book content

1. INTRODUCTION

   ```text
   1.1 WHAT IS AN OPERATING SYSTEM?
   1.1.1 The Operating System as an Extended Machine
   1.1.2 The Operating System as a Resource Manager
   1.2 HISTORY OF OPERATING SYSTEMS 6
   1.2.1 The First Generation (1945–55) Vacuum Tubes and Plugboards 7
   1.2.2 The Second Generation (1955–65) Transistors and Batch Systems 7
   1.2.3 The Third Generation (1965–1980) ICs and Multiprogramming 9
   1.2.4 The Fourth Generation (1980–Present) Personal Computers 14
   1.2.5 History of MINIX 3 16
   1.3 OPERATING SYSTEM CONCEPTS 19
   1.3.1 Processes 20
   1.3.2 Files 22
   1.3.3 The Shell 25
   1.4 SYSTEM CALLS 26
   1.4.1 System Calls for Process Management 27
   1.4.2 System Calls for Signaling 31
   1.4.3 System Calls for File Management 33
   1.4.4 System Calls for Directory Management
   1.4.5 System Calls for Protection 40
   1.4.6 System Calls for Time Management 42
   1.5 OPERATING SYSTEM STRUCTURE 42
   1.5.1 Monolithic Systems 42
   1.5.2 Layered Systems 45
   1.5.3 Virtual Machines 46
   1.5.4 Exokernels 49
   1.5.5 Client-Server Model 49
   1.6 OUTLINE OF THE REST OF THIS BOOK 51
   1.7 SUMMARY 51
   ```

2. PROCESSES

   ```text
   2.1 INTRODUCTION TO PROCESSES 55
   2.1.1 The Process Model 56
   2.1.2 Process Creation 57
   2.1.3 Process Termination 59
   2.1.4 Process Hierarchies 60
   2.1.5 Process States 60
   2.1.6 Implementation of Processes 62
   2.1.7 Threads 64
   2.2 INTERPROCESS COMMUNICATION
   2.2.1 Race Conditions 69
   2.2.2 Critical Sections 70
   2.2.3 Mutual Exclusion with Busy Waiting
   2.2.4 Sleep and Wakeup 76
   2.2.5 Semaphores 78
   2.2.6 Mutexes 81
   2.2.7 Monitors 81
   2.2.8 Message Passing 85
   2.3 CLASSICAL IPC PROBLEMS 88
   2.3.1 The Dining Philosophers Problem 89
   2.3.2 The Readers and Writers Problem 92
   2.4 SCHEDULING 93
   2.4.1 Introduction to Scheduling 94
   2.4.2 Scheduling in Batch Systems 99
   2.4.3 Scheduling in Interactive Systems 102
   2.4.4 Scheduling in Real-Time Systems 109
   2.4.5 Policy versus Mechanism 110
   2.4.6 Thread Scheduling 110
   2.5 OVERVIEW OF PROCESSES IN MINIX 3 112
   2.5.1 The Internal Structure of MINIX 3 112
   2.5.2 Process Management in MINIX 3 116
   2.5.3 Interprocess Communication in MINIX 3 120
   2.5.4 Process Scheduling in MINIX 3 122
   2.6 IMPLEMENTATION OF PROCESSES IN MINIX 3
   2.6.1 Organization of the MINIX 3 Source Code 125
   2.6.2 Compiling and Runniing MINIX 3 128
   2.6.3 The Common Header Files 130
   2.6.4 The MINIX 3 Header Files 138
   2.6.5 Process Data Structures and Header Files 146
   2.6.6 Bootstrapping MINIX 3 156
   2.6.7 System Initialization 160
   2.6.8 Interrupt Handling in MINIX 3 167
   2.6.9 Interprocess Communication in MINIX 3
   2.6.10 Scheduling in MINIX 3 182
   2.6.11 Hardware-Dependent Kernel Support 185
   2.6.12 Utilities and the Kernel Library 190
   2.7 THE SYSTEM TASK IN MINIX 3 192
   2.7.1 Overview of the System Task 194
   2.7.2 Implementation of the System Task 197
   2.7.3 Implementation of the System Libarary 200
   2.9 THE CLOCK TASK IN MINIX 3 204
   2.8.1 Clock Hardware 204
   2.8.2 Clock Software 206
   2.8.3 Overview of the Clock Driver in MINIX 3 208
   2.8.4 Implementation of the Clock Driver in MINIX 3 212
   2.9 SUMMARY 214
   ```

3. INPUT/OUTPUT

   ```text
   3.1 PRINCIPLES OF I/O HARDWARE
   3.1.1 I/O Devices 223
   3.1.2 Device Controllers
   3.1.3 Memory-Mapped I/O 225
   3.1.4 Interrupts 226
   3.1.5 Direct Memory Access 227
   3.2 PRINCIPLES OF I/O SOFTWARE 229
   3.2.1 Goals of the I/O Software 229
   3.2.2 Interrupt Handlers 231
   3.2.3 Device Drivers 231
   3.2.4 Device-Independent I/O Software 233
   3.2.5 User-Space I/O Software 236
   3.3 DEADLOCKS 237
   3.3.1 Resources 238
   3.3.2 Principles of Deadlock 239
   3.3.3 The Ostrich Algorithm 242
   3.3.4 Detection and Recovery 244
   3.3.5 Deadlock Prevention 245
   3.3.6 Deadlock Avoidance 247
   3.4 OVERVIEW OF I/O IN MINIX 3 252
   3.4.1 Interrupt Handlers in MINIX 3 252
   3.4.2 Device Drivers in MINIX 3 256
   3.4.3 Device-Independent I/O Software in MINIX 3 259
   3.4.4 User-level I/O Software in MINIX 3 260
   3.4.5 Deadlock Handling in MINIX 3 260
   3.5 BLOCK DEVICES IN MINIX 3 261
   3.5.1 Overview of Block Device Drivers in MINIX 3 262
   3.5.2 Common Block Device Driver Software 265
   3.5.3 The Driver Library 269
   3.6 RAM DISKS 271
   3.6.1 RAM Disk Hardware and Software 271
   3.6.2 Overview of the RAM Disk Driver in MINIX 3 273
   3.6.3 Implementation of the RAM Disk Driver in MINIX 3 274
   3.7 DISKS 278
   3.7.1 Disk Hardware 278
   3.7.2 RAID 280
   3.7.3 Disk Software 281
   3.7.4 Overview of the Hard Disk Driver in MINIX 3 287
   3.7.5 Implementation of the Hard Disk Driver in MINIX 3 290
   3.7.6 Floppy Disk Handling 300
   3.8 TERMINALS 302
   3.8.1 Terminal Hardware 303
   3.8.2 Terminal Software 307
   3.8.3 Overview of the Terminal Driver in MINIX 3 316
   3.8.4 Implementation of the Device-Independent Terminal Driver 331
   3.8.5 Implementation of the Keyboard Driver 350
   3.8.6 Implementation of the Display Driver 357
   3.9 SUMMARY 366
   ```

4. MEMORY MANAGEMENT

   ```text
   4.1 BASIC MEMORY MANAGEMENT 374
   4.1.1 Monoprogramming without Swapping or Paging
   4.1.2 Multiprogramming with Fixed Partitions 375
   4.1.3 Relocation and Protection 377
   4.2 SWAPPING 378
   4.2.1 Memory Management with Bitmaps 380
   4.2.2 Memory Management with Linked Lists
   4.3 VIRTUAL MEMORY 383
   4.3.1 Paging 384
   4.3.2 Page Tables 388
   4.3.3 TLBs—Translation Lookaside Buffers 392
   4.3.4 Inverted Page Tables 395
   4.4 PAGE REPLACEMENT ALGORITHMS 396
   4.4.1 The Optimal Page Replacement Algorithm 397
   4.4.2 The Not Recently Used Page Replacement Algorithm
   4.4.3 The First-In, First-Out (FIFO) Page Replacement Algorithm 399
   4.4.4 The Second Chance Page Replacement Algorithm 399
   4.4.5 The Clock Page Replacement Algorithm 400
   4.4.6 The Least Recently Used (LRU) Page Replacement Algorithm 401
   4.4.7 Simulating LRU in Software 401
   4.5 DESIGN ISSUES FOR PAGING SYSTEMS 404
   4.5.1 The Working Set Model 404
   4.5.2 Local versus Global Allocation Policies 406
   4.5.3 Page Size 408
   4.5.4 Virtual Memory Interface 410
   4.6 SEGMENTATION 410
   4.6.1 Implementation of Pure Segmentation 414
   4.6.2 Segmentation with Paging: The Intel Pentium 415
   4.7 OVERVIEW OF THE MINIX 3 PROCESS MANAGER 420
   4.7.1 Memory Layout 422
   4.7.2 Message Handling 425
   4.7.3 Process Manager Data Structures and Algorithms 426
   4.7.4 The FORK, EXIT, and WAIT System Calls 432
   4.7.5 The EXEC System Call 433 4.7.6 The BRK System Call 437
   4.7.7 Signal Handling 438
   4.7.8 Other System Calls 446
   4.8 IMPLEMENTATION OF THE MINIX 3 PROCESS MANAGER 447
   4.8.1 The Header Files and Data Structures 447
   4.8.2 The Main Program 450
   4.8.3 Implementation of FORK, EXIT, and WAIT
   4.8.4 Implementation of EXEC 458
   4.8.5 Implementation of BRK 461
   4.8.6 Implementation of Signal Handling 462
   4.8.7 Implementation of Other System Calls 471
   4.8.8 Memory Management Utilities
   4.9 SUMMARY 475
   ```

5. FILE SYSTEMS

   ```text
   5.1 FILES 482
   5.1.1 File Naming 482
   5.1.2 File Structure 484
   5.1.3 File Types 485
   5.1.4 File Access 488
   5.1.5 File Attributes 488
   5.1.6 File Operations 490
   5.2 DIRECTORIES 491
   5.2.1 Simple Directories 491
   5.2.2 Hierarchical Directory Systems
   5.2.3 Path Names 493
   5.2.4 Directory Operations 496
   5.3 FILE SYSTEM IMPLEMENTATION
   5.3.1 File System Layout 497
   5.3.2 Implementing Files 499
   5.3.3 Implementing Directories 502
   5.3.4 Disk Space Management 509
   5.3.5 File System Reliability 512
   5.3.6 File System Performance 519
   5.3.7 Log-Structured File Systems 524
   5.4 SECURITY 526
   5.4.1 The Security Environment 526
   5.4.2 Generic Security Attacks 531
   5.4.3 Design Principles for Security 532
   5.4.4 User Authentication 533
   5.5 PROTECTION MECHANISMS 537
   5.5.1 Protection Domains 537
   5.5.2 Access Control Lists
   5.5.3 Capabilities 542
   5.5.4 Covert Channels 545
   5.6 OVERVIEW OF THE MINIX 3 FILE SYSTEM 548
   5.6.1 Messages 549
   5.6.2 File System Layout 549
   5.6.3 Bitmaps 553
   5.6.4 I-Nodes 555
   5.6.5 The Block Cache 557
   5.6.6 Directories and Paths 559
   5.6.7 File Descriptors 561
   5.6.8 File Locking 563
   5.6.9 Pipes and Special Files 563
   5.6.10 An Example: The READ System Call 565
   5.7 IMPLEMENTATION OF THE MINIX 3 FILE SYSTEM
   5.7.1 Header Files and Global Data Structures 566
   5.7.2 Table Management 570
   5.7.3 The Main Program 579
   5.7.4 Operations on Individual Files 583
   5.7.5 Directories and Paths 591
   5.7.6 Other System Calls 596
   5.7.7 The I/O Device Interface 597
   5.7.8 Additional System Call Support 603
   5.7.9 File System Utilities 605
   5.7.10 Other MINIX 3 Components 606
   SUMMARY 606
   ```
