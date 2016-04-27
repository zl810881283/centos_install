# init centos for web and prxoy

`yum -y install vim`
`systemctl stop firewalld.service`
`systemctl disable firewalld.service`

## none password login

`echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDvOyyqLgEJv/hfWpAs81V/sYmEEvHTbOOeccKh3JOko+zwahc74m/cLNWGBmyrssssOzLwGop2xbxIN1xM1N0i1SGQk1gmtR+BRLGln8+tORAZ+8o4Maw006BGu3oHYbAdJj2M/H5+WDZLtzYZwtTdjgKOygASB4TGhv3FyPu4JkReXvCu1YTLDpPFj7+RgYfi4naibKKpaEtvuDWnMjmiKBDH4cbFsqSXWMo9oI1t8JLrFd/pvKUiUjK6mu2FsuYQ/ytWJ7IyRHTr50eMomfp7hcXsebsFS+kuwTd6vC9lsFYXKXpiCmwMuOdy434bfgwxfFT0P4vPWdzXoSS128t zl@zl.local' >> ~/.ssh/authorized_keys`

## install nginx
- `wget http://nginx.org/download/nginx-1.9.12.tar.gz`
- `tar -zxvf nginx-1.9.12.tar.gz`
- `cd nginx-1.9.12`
- `yum -y install gcc-c++ zlib zlib-devel pcre-devel pcre openssl openssl--devel`
- `./configure`
- `make`
- `make install`
- `chmod +x /etc/rc.d/rc.local`
- `vi /etc/rc.d/rc.local`
```
/usr/local/nginx/sbin/nginx
```

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

## install pptpd

- `yum -y install ppp pptpd`
- `vi /etc/pptpd.conf`
```
localip 192.168.0.1
remoteip 192.168.0.234-238,192.168.0.245
```
- `vi /etc/ppp/options.pptpd`
```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```
- `vi /etc/ppp/chap-secrets`
```
用户名 pptpd 密码 *
```
- `vi /etc/sysctl.conf`
```
net.ipv4.ip_forward=1
```
- `sysctl -p`
- `iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE`
- `chmod +x /etc/rc.d/rc.local`
- `vi /etc/rc.d/rc.local`
```
iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE
```
- `pptpd start`
- `chkconfig pptpd on`

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

### nginx

nothing need run

### shadowsocks

- `ss-server`

### pptpd

nothing need run
