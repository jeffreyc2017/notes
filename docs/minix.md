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
