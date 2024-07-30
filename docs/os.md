# OS

- [little OS book](https://littleosbook.github.io/)
- Operating System: Design and Implementation by A.S. Tanenbaum and A.S. Woodhull

## MINIX

```sh
pkgin update
pkgin_sets
cd /usr/pkg/etc/rc.d/
rm kadmind kcm kdc kpasswdd # these files may cause a bunch of errors, see https://groups.google.com/forum/#!topic/minix3/HrQe7xJbnDQ
# git clone git://git.minix3.org/minix /usr/src # https://groups.google.com/g/minix3/c/av5gZMLO6y8

# Have issues with git clone, remote hang up unexpectedly. Use scp from local instead
git clone git@github.com:Stichting-MINIX-Research-Foundation/minix.git /usr/src # You'll have to set up a ssh key and add the public key to github: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
cd /usr/src
# Do this on the host if git doesn't work normally
git checkout v3.3.0 # we do not want to mess with bleeding edge code (do we?)
```

- [minix3](https://www.minix3.org)
- [code](https://github.com/Stichting-MINIX-Research-Foundation/minix)
- [wiki](https://en.wikipedia.org/wiki/Minix)
- <https://gist.github.com/Drowze/2f7cbce35ade1fa94b2511f4138a32c2>
- [build in Minix](https://wiki.minix3.org/doku.php?id=releases:3.2.1:developersguide:trackingcurrent)
- [issue with git clone](https://groups.google.com/g/minix3/c/av5gZMLO6y8)

## macOS

```sh
sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist
```

- [remote login issue resolution](https://apple.stackexchange.com/questions/442379/cant-enable-ssh-on-catalina-system-preferences-keeps-saying-remote-login-sta/442380#442380)
- [remote login issue](https://discussions.apple.com/thread/253932000?sortBy=rank)
