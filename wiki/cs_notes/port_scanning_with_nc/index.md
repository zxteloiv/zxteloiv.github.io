---
layout: page
title: port scanning with nc
math_support: mathjax
---


```bash
## syntax ##
nc -z -v {host-name-here} {port-range-here}
nc -z -v host-name-here ssh
nc -z -v host-name-here 22
nc -w 1 -z -v server-name-here port-Number-her
 
## scan 1 to 1023 ports ##
nc -zv vip-1.vsnl.nixcraft.in 1-1023

nc -zv v.txvip1 443
nc -zv v.txvip1 80
nc -zv v.txvip1 22
nc -zv v.txvip1 21
nc -zv v.txvip1 smtp
nc -zvn v.txvip1 ftp
 
## really fast scanner with 1 timeout value ##
netcat -v -z -n -w 1 v.txvip1 1-1023

$ netcat -z -vv www.cyberciti.biz http
www.cyberciti.biz [75.126.153.206] 80 (http) open
 sent 0, rcvd 0
$ netcat -z -vv google.com https
DNS fwd/rev mismatch: google.com != maa03s16-in-f2.1e100.net
DNS fwd/rev mismatch: google.com != maa03s16-in-f6.1e100.net
DNS fwd/rev mismatch: google.com != maa03s16-in-f5.1e100.net
DNS fwd/rev mismatch: google.com != maa03s16-in-f3.1e100.net
DNS fwd/rev mismatch: google.com != maa03s16-in-f8.1e100.net
DNS fwd/rev mismatch: google.com != maa03s16-in-f0.1e100.net
DNS fwd/rev mismatch: google.com != maa03s16-in-f7.1e100.net
DNS fwd/rev mismatch: google.com != maa03s16-in-f4.1e100.net
google.com [74.125.236.162] 443 (https) open
 sent 0, rcvd 0
$ netcat -v -z -n -w 1 192.168.1.254 1-1023
(UNKNOWN) [192.168.1.254] 989 (ftps-data) open
(UNKNOWN) [192.168.1.254] 443 (https) open
(UNKNOWN) [192.168.1.254] 53 (domain) open

```


