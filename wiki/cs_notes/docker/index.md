---
layout: page
title: docker
math_support: mathjax
---


## basic config

~~~ bash
# run interactively, then type ``exit''
sudo docker run [--name gundam] -i -t ubuntu /bin/bash # create tty and capture STDIN

# start again
sudo docker start gundam # or using an ID

# attach to a running container
sudo docker attach gundam # or using an ID

# run a command as daemon
sudo docker run --name gundam_daemon -d ubuntu /bin/bash -c "while true; do echo papapa; sleep 3; done"
# do not attach to the above container, no way to exit except kill

# show logs (for daemon) with time
sudo docker logs [--tail 10] [-f] -t gundam_daemon # just behave like ``tail -f''

# show processes in a container
sudo docker top gundam

# execute a command in a container
sudo docker exec -d gundam_daemon touch /etc/something
sudo docker exec -t -i gundam_daemon /bin/bash

# stop a daemon
sudo docker stop gundam # SIGTERM
sudo docker kill gundam # SIGKILL

# restart a container
sudo docker run --restart=always --name gundam -d ubuntu /bin/bash -c "while true; do echo papapa; sleep 3; done"
# or --restart=on-failure:5 which means restart on exit code > 0 at most 5 times

# show more info about a container
sudo docker inspect gundam_daemon

# remove container
sudo docker rm gundam # which must be stopped
~~~


