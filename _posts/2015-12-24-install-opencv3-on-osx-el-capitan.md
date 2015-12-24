---
layout: post
title:  "Install OpenCV 3 on OSX El Capitan"
date:   2015-12-24 19:50
tags: opencv osx
categories: main learning
---

### Preliminaries 

#### 1. Python Binding for OpenCV

Up to now, most of the python wrappers for OpenCV in PyPI seem dead. The better way is to use the Python binding that comes with OpenCV source. The source firstly provide a traditional C/C++ interface because it is itself written in C/C++.

#### 2. OpenCV

On OS X we have several ways to build OpenCV. For example

- download the tarball source file from [opencv.org](http://opencv.org), and build it on one's own
- use `brew tap homebrew/science`, and `brew install opencv3` 

I choose the 2nd method because OpenCV relies on some other packages like eigen, libpng and so on. Homebrew helps a lot when I deal with the dependencies.

#### 3. Python Distributions

On OSX we may have many python distributions.

- Python 2.7.10 shipped with OS X El Capitan in `/System/Library/Frameworks/Python`
- Python 2.7.11 downloaded from [python.org](http://www.python.org) in `/Library/Frameworks/Python.framework/`
- Brewed python 2.7.11 in `/usr/local/`
- Python compiled from source with a designated `--prefix`

Each of them may have a corresponding pip util. Since the first one is reserved by OS X and save package in root folder, I don't want to use it and battle with the new SIP feature. I previously installed python in the second way, so now I have to use pip with `--user` option to install all packages in my user folder. But the latter two distribution have the right directory permission, pip will work well.

> Note if you want to use GUI function in python, like the matplotlib package, you may have to install packages pyobjc-framework-*. Check it with brew or your other distribution.

### Procedure

Now let's start to brew opencv3. There're several points that may concern. And in China we may suffer the bad network connection.

#### 1. numpy

You could tap homebrew/python to get python packages, but I like to use pip instead. Installing OpenCV3 by default needs the numpy in `homebrew/python/numpy`. If you have your own numpy package at hand, you may add "--without-numpy" option.

#### 2. python

If you don't want to use python binding in OpenCV and only need C/C++ interface, you may add "--without-python" option. But I need it and I want to use my own distribution residing in `/Library/Frameworks` so we have to edit the formula: `brew edit opencv3`

Search "python" in the document we may find a ruby block

``` ruby
if build.with? "python"
  py_prefix = `python-config --prefix`.chomp
  py_lib = OS.linux? ? `python-config --configdir`.chomp : "#{py_prefix}/lib"
  args << "-DPYTHON2_EXECUTABLE=#{which "python"}"
  args << "-DPYTHON2_LIBRARY=#{py_lib}/libpython2.7.#{dylib}"
  args << "-DPYTHON2_INCLUDE_DIR=#{py_prefix}/include/python2.7"
end
```

If compiled by default, opencv will use the python-config shipped with OS X. So change it to other binary as you like, mine, for example, is `/Library/Frameworks/Python.framework/Versions/2.7/bin/python-config`.

Then save and exit. Correct build options will be passed to CMake.

#### 3. download

In homebrew formula for OpenCV3 there's a dependency of [ippicv_macosx_20141027.tgz](https://downloads.sourceforge.net/project/opencvlibrary/3rdparty/ippicv/ippicv_macosx_20141027.tgz). It's hard to download it using my network because of the bad connection with sourceforge.net. But brew will download and save it in `/Library/Caches/Homebrew` by default. If I download it using other ways like proxy/relay server/Cloud Storage/VPN, and copy it to the folder, homebrew will recognize it correctly.

But here comes the weird thing, CMake has found the ippicv version is not newest, and try to download another file *ippicv_macosx_20151201.tgz*. If you google this file, you will find the cmake config from GitHub. After analyzing that file we can reconstruct the download URL as "https://raw.githubusercontent.com/Itseez/opencv_3rdparty/81a676001ca8075ada498583e4166079e5744668/ippicv/ippicv_macosx_20151201.tgz".

But even if I use brew with *http_proxy* variable defined, CMake didn't use my proxy. It's hard for me to access the file directly, too.

We can't skip the download phase because homebrew uncompress the source file each time we install OpenCV, and if I changed the CMake config in the source tarball the MD5 digest will be different and homebrew will refuse to use it to install.

The crazy idea comes to my head. 

Firstly I signed a SSL certificate for myself. Following this [guide](http://pankajmalhotra.com/Simple-HTTPS-Server-In-Python-Using-Self-Signed-Certs/) it's easy to create a certificate file. But enter raw.githubusercontent.com when it is asking for the common name.

``` bash
openssl genrsa -des3 -out server.key 1024

openssl req -new -key server.key -out server.csr
// when Common Name (e.g. server FQDN or YOUR name) []:raw.githubusercontent.com

openssl x509 -req -days 1024 -in server.csr -signkey server.key -out server.crt

cat server.crt server.key > server.pem
```

Then I import the private key into System Keychain following this [guide](https://www.digicert.com/ssl-support/p12-import-export-mac-mavericks-server.htm). Since I use python directly to start a simple https server there's no need to assign the certificate to the website as in the tutorial. 

Then put the ippicv_macosx_20151201.tgz file to correct path.

``` bash
dir=Itseez/opencv_3rdparty/81a676001ca8075ada498583e4166079e5744668/ippicv
mkdir -p $dir
mv ippicv_macosx_20151201.tgz $dir/
```

Run a https server in python as 

``` python
#!/usr/bin/python

import BaseHTTPServer, SimpleHTTPServer
import ssl

httpd = BaseHTTPServer.HTTPServer(('localhost', 443), SimpleHTTPServer.SimpleHTTPRequestHandler)
httpd.socket = ssl.wrap_socket (httpd.socket, certfile='/path/to/server.pem', server_side=True)
httpd.serve_forever()
```

Since 443, 80 and other small ports are reserved by OS X, we have to run the script with *sudo*.

Finally `sudo edit /etc/hosts` file and append this line

``` 
127.0.0.1 raw.githubusercontent.com
```

Now we finished hijacking the *raw.githubusercontent.com* domain locally, run the `brew install opencv3 --without-numpy --verbose` again we can successfully brewed it from source.

### Conclusion

Maybe things remaining are all related to python packages. I dismiss them here but you can find them easily by google. This post finished the compilation and some installation of OpenCV3 I found hard to complete.