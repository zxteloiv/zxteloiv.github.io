---
layout: page
title: ubuntu_14.04_vps
math_support: mathjax
---


### connection

with local proxy:

~~~bash
ssh  -o "ProxyCommand /usr/bin/nc -X 5 -x 127.0.0.1:1080 %h %p" -p 22 root@$someip
~~~

### locale

~~~bash
sudo locale-gen "en_US.UTF-8"
sudo dpkg-reconfigure locales
~~~

### user

create acount

~~~bash
useradd --create-home --system --shell /bin/bash demo

# if created error
sudo usermod -d /new/place/will/be/created -m zxteloiv
~~~

password

~~~bash
passwd demo
~~~

use visudo to give permission

~~~
demo ALL=(ALL:ALL) ALL
~~~

### sshd port

change sshd config

~~~bash
vim /etc/ssh/sshd_config # modify the options as below

reload ssh
~~~

maybe reference

~~~
Port 25000 # new port
Protocol 2 # 
PermitRootLogin no # no root login in the future

UseDNS no
AllowUsers demo # only the new user
~~~

### old version python curl

for python 2.7 older than 2.7.9

`pip install --upgrade ndg-httpsclient`

### copy pub key

~~~bash
mkdir -p ~/.ssh
touch ~/.ssh/authorized_keys

# then copy the id_rsa.pub into it
~~~

### basic building tools

~~~bash
sudo apt-get install gcc g++ autoconf make libssl-dev openssl 
~~~

### timezone

~~~ bash
sudo dpkg-reconfigure tzdata
~~~

### reference
- init ubuntu 14.04 [tutorial](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)
- init ubuntu [tutorial](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04)
- add user [tutorial](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps)
- additional [config](https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers)



