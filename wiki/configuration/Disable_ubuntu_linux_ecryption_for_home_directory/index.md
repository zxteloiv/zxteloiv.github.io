---
layout: page
title: Disable ubuntu linux ecryption for home directory
math_support: mathjax
---


`$ ecryptfs-setup-private --undo`

then follow
```
In the event that you want to remove your eCryptfs Private Directory setup, you will need to very carefully perform the following actions manually:

    Obtain your Private directory mountpoint

    $ PRIVATE=`cat ~/.ecryptfs/Private.mnt 2>/dev/null || echo $HOME/Private`

    Ensure that you have moved all relevant data out of your $PRIVATE directory

    Unmount your encrypted private directory

    $ ecryptfs-umount-private

    Make your Private directory writable again

    $ chmod 700 $PRIVATE

    Remove $PRIVATE, ~/.Private, ~/.ecryptfs

    Note: THIS IS VERY PERMANENT, BE VERY CAREFUL

    $ rm -rf $PRIVATE ~/.Private ~/.ecryptfs

    Uninstall the utilities (this is specific to your Linux distribution)

    $ sudo apt-get remove ecryptfs-utils libecryptfs0

```



