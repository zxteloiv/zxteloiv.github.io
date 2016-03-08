---
layout: page
title: nginx_multi_server
math_support: mathjax
---


<div>originally taken from&nbsp;<a href="http://unix.stackexchange.com/questions/134087/nginx-server-config-with-multiple-locations-does-not-work">here</a></div>

<div><div>1. same port, different server names</div></div>

```
server {
listen 80;
server_name dev.local;
root /var/www/htdocs;
index index.php index.html index.htm;

...Add more location directives, php support, etc...

}

server {
listen 80;
server_name phpmyadmin.local;
root /var/www/phpmyadmin;
index index.php index.html index.htm;

# ...Specify additional location directives, php support, etc...
}
```

of course the host file needs these lines
```
127.0.0.1 dev.local
127.0.0.1 phpmyadmin.local
```

<div><div>2. different ports, same server name</div></div>

```
server {
    listen   80;
    server_name localhost;
    root /var/www/htdocs;
    index index.php index.html index.htm;

    location /phpmyadmin {
        proxy_pass http://127.0.0.1:8080/;
    }

    # ...Add more location directives, php support, etc...
}

server {
    listen 8080;
    server_name localhost;
    root /var/www/phpmyadmin;
    index index.php index.html index.htm;

    # ...Specify additional location directives, php support, etc...
}
```






