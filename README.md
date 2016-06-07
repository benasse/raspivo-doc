the following information are valid for raspberry3

## raspivo install

###Install a raspbian image
```
wget https://downloads.raspberrypi.org/raspbian_lite_latest
unzip *raspbian-jessie-lite.zip
dd bs=4M if=*raspbian-jessie-lite.img of=/dev/sdc
```

###set default locale in utf8 (example : en_US.UTF8) and configure timezone
```
dpkg-reconfigure locales tzdata
```

###install some utilities
```
apt install vim tmux htop mlocate git
cp /usr/share/doc/tmux/examples/screen-keys.conf /root/.tmux.conf
```

###install some apt requirement
```
apt -y install debian-archive-keyring apt-transport-https debian-keyring 
```

###update 
```
apt upgrade -y
apt dist-upgrade -y
reboot
```

###add the Raspberry 3 repo
```
cat > /etc/apt/sources.list.d/raspivo.list << EOF
deb http://www.iris-network.fr/raspivo/raspberrypi3/16.06 jessie main
deb http://ftp.fr.debian.org/debian/ stable main contrib non-free
EOF
```
###add the repo keys
```
gpg --keyserver pgpkeys.mit.edu --recv-key  3FC6A9B2ACDD4CF3 && gpg -a --export 3FC6A9B2ACDD4CF3 | apt-key add -
gpg --keyserver pgpkeys.mit.edu --recv-key  8B48AD6246925553 && gpg -a --export 8B48AD6246925553 | apt-key add -
gpg --keyserver pgpkeys.mit.edu --recv-key  7638D0442B90D010 && gpg -a --export 7638D0442B90D010 | apt-key add -
gpg --keyserver pgpkeys.mit.edu --recv-key  CBF8D6FD518E17E1 && gpg -a --export CBF8D6FD518E17E1 | apt-key add -
```

###update and upgrade
```
apt update
apt upgrade -y
```

###disable XiVO during the installation
```
mkdir /var/lib/xivo/
touch /var/lib/xivo/disabled
```

###install XiVO
```
apt install -y xivo
```

###disable dahdi
```
sed -i 's/  status)/  status)\n\texit 0/g' /etc/init.d/dahdi
```

###reboot
```
reboot
```

##raspivo update

###xivo-dist
You should not use xivo-dist, and may have to comment the xivo repo in `/etc/apt/sources.list.d/xivo-dist.list` like this :
```
# xivo-five
# deb http://mirror.xivo.io/debian/ xivo-five main
# deb-src http://mirror.xivo.io/debian/ xivo-five main

```
###change the repository link
edit `/etc/apt/sources.list.d/raspivo.list` and change the repository link with the desired version

by example from 16.06 :

```
deb http://www.iris-network.fr/raspivo/raspberrypi3/16.06 jessie main
deb http://ftp.fr.debian.org/debian/ stable main contrib non-free
```
to 16.07 :
```
deb http://www.iris-network.fr/raspivo/raspberrypi3/16.07 jessie main
deb http://ftp.fr.debian.org/debian/ stable main contrib non-free
```

###run xivo-upgrade
```
xivo-upgrade
```
