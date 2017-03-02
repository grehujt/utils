debian 8 / ubuntu 14(16)
```sh
$ uname -r

## if kernel version < 4.9
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10/linux-image-4.10.0-041000-generic_4.10.0-041000.201702191831_amd64.deb
dpkg -i linux-image-4.10.0*.deb
update-grub
reboot

## open bbr
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
## check if open succeeded
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
lsmod | grep bbr

## edit locale if needed
vi /etc/environment
LC_ALL=en_US.UTF-8
LANG=en_US.UTF-8

## ss
curl -fsSL https://get.docker.com/ | sh
docker run -d -p 443:1984 oddrationale/docker-shadowsocks -s 0.0.0.0 -p 1984 -k PASSWD -m aes-256-cfb

## done
```
