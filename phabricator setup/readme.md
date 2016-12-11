# Notes on setup local phabricator server
- git
```sh
$ sudo yum update && sudo yum install git
```
- ssl cert via letsencrypt
```sh
$ git clone https://github.com/Neilpang/acme.sh.git
$ cd ./acme.sh && ./acme.sh --install
$ crontab -e
$ ./acme.sh --issue --dns -d xxxxxxx.com
(after adding TXT record)
$ ./acme.sh --renew -d xxxxxxx.com
```
- DNS setup
  - add A record for xxxxxxx.com
  - add TXT record for letsencrypt dns challenge
- docker env setup
```sh
$ sudo yum update
$ curl -fsSL https://get.docker.com/ | sh
$ sudo systemctl enable docker.service
$ sudo systemctl start docker
$ sudo yum install python-pip
$ sudo pip install docker-compose
```
- adjust contains' confs
   - ports
   - domain name
   - storage type/location
   - db confs
```sh
$ git clone https://github.com/hach-que-docker/phabricator
$ cd phabricator
$ vi docker-compose.yml
version: '2'
services:
  phabricator:
    restart: always
    ports:
     - "443:443"
     - "80:80"
     - "13022:22"
    volumes:
     - /srv/docker/phabricator/repos:/repos
     - /srv/docker/phabricator/extensions:/srv/phabricator/phabricator/src/extensions
     - /srv/docker/phabricator/sshhostkey:/sshhostkey
     - /home/kris/.acme.sh/xxxxxxx.com:/ssl
    depends_on:
     - mysql
    links:
     - mysql
    environment:
     - MYSQL_HOST=mysql
     - MYSQL_USER=root
     - MYSQL_PASS=phabricator
     - PHABRICATOR_REPOSITORY_PATH=/repos
     - PHABRICATOR_HOST=pha2.compathnion.com
     - PHABRICATOR_VCS_PORT=13022
     - PHABRICATOR_STORAGE_TYPE=disk
     - PHABRICATOR_STORAGE_PATH=/repos/files/
     - PHABRICATOR_HOST_KEYS_PATH=/sshhostkey
     - SSL_TYPE=manual
     - SSL_CERTIFICATE=/ssl/fullchain.cer
     - SSL_PRIVATE_KEY=/ssl/xxxxxxx.com.key
    image: hachque/phabricator
  mysql:
    restart: always
    volumes:
     - /srv/docker/phabricator/mysql:/var/lib/mysql
    image: mysql:5.7.14
    environment:
     - MYSQL_ROOT_PASSWORD=phabricator
```
- firewall setup
```sh
$ sudo firewall-cmd --zone=public --permanent --remove-port=13023/tcp
$ sudo firewall-cmd --reload
```
- containers setup
```sh
$ sudo docker exec -ti phabricator_phabricator_1 bash
# cd /srv/phabricator/phabricator/bin/
# ./config set environment.append-paths '["/usr/local/bin","/usr/bin:/sbin","/usr/sbin","/bin","/usr/lib/git"]'
# ./config set security.require-https true
# visudo
nginx ALL=(ALL) SETENV: NOPASSWD: /usr/lib/git/git-http-backend
# zypper addrepo http://download.opensuse.org/repositories/server:php:extensions/php_openSUSE_13.1/server:php:extensions.repo
# zypper refresh
# zypper install php5-APCu
(crtl-d)
$ sudo docker exec -ti phabricator_mysql_1 bash
# vi /etc/mysql/mysql.conf.d/mysqld.cnf
max_allowed_packet = 64M
sql_mode=STRICT_ALL_TABLES
innodb_buffer_pool_size=3200M
(ctrl-d)
$ sudo docker-compose restart
```
