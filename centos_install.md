# init centos for web and prxoy
`yum -y install vim`
`systemctl stop firewalld.service`
## none password login

## install nginx
- `wget http://nginx.org/download/nginx-1.9.12.tar.gz`
- `tar -zxvf nginx-1.9.12.tar.gz`
- `cd nginx-1.9.12.tar.gz`
- `yum -y install gcc-c++ zlib zlib-devel pcre-devel pcre openssl openssl--devel`
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
- `mv librehat-shadowsocks-epel-7.repo /etc/yum.repo.d/`
- `yum update`
- `yum install shadowsocks-libev`
- `vim /etc/shadowsocks-libev/config.json`
```
ase-256-cbf
```
- `ss-server`

## when reboot

- `systemctl stop firewalld.service`
- `/usr/local/nginx/sbin/nginx`
- `ss-server`

