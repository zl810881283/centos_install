# init centos for web and prxoy

`yum -y install vim`

`systemctl stop firewalld.service`

## none password login

`echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDvOyyqLgEJv/hfWpAs81V/sYmEEvHTbOOeccKh3JOko+zwahc74m/cLNWGBmyrssssOzLwGop2xbxIN1xM1N0i1SGQk1gmtR+BRLGln8+tORAZ+8o4Maw006BGu3oHYbAdJj2M/H5+WDZLtzYZwtTdjgKOygASB4TGhv3FyPu4JkReXvCu1YTLDpPFj7+RgYfi4naibKKpaEtvuDWnMjmiKBDH4cbFsqSXWMo9oI1t8JLrFd/pvKUiUjK6mu2FsuYQ/ytWJ7IyRHTr50eMomfp7hcXsebsFS+kuwTd6vC9lsFYXKXpiCmwMuOdy434bfgwxfFT0P4vPWdzXoSS128t zl@zl.local' >> ~/.ssh/authorized_keys`

## install nginx
- `wget http://nginx.org/download/nginx-1.9.12.tar.gz`
- `tar -zxvf nginx-1.9.12.tar.gz`
- `cd nginx-1.9.12`
- `yum -y install gcc-c++ zlib zlib-devel pcre-devel pcre openssl openssl--devel`
- `./configure`
-  `make`
-  `make install`

### test download speed

- `vi /usr/local/nginx/conf/nginx.conf`
```
multi_accept on;  
use epoll;
```
- `cd && mkdir www && cd www`
- `echo 'test for web site' >> index.html`
- `dd if=/dev/zero of=500m.tmp bs=1M count=500`
- `/usr/local/nginx/sbin/nginx`

## install shadowsocks

https://github.com/shadowsocks/shadowsocks-libev

- `cd`
- `wget https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo`
- `mv librehat-shadowsocks-epel-7.repo /etc/yum.repos.d/`
- `yum -y update`
- `yum -y install shadowsocks-libev`
- `vim /etc/shadowsocks-libev/config.json`
```
aes-256-cfb
```
- `ss-server`

## after reboot

- `systemctl stop firewalld.service`
- `/usr/local/nginx/sbin/nginx`
- `ss-server`

