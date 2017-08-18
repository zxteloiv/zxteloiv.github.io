---
layout: post
title:  "Python 开发常用实践"
date:   2017-08-18 20:00:00 +0800
tags: python
categories: chn
math_support: 
---

本文是给组里 wiki 写的一点分享资料，面向 python 刚入门不久的新人。也放在这里分享一下，希望能帮助到人。

# 背景

Python 是一门语言的总称，一般我们下载的所谓“官方版”是 CPython 发行版。顾名思义，它里面的 python 执行文件是用C语言写的。
它是事实上的权威版本，功能支持也是最多的。

除了官方的 CPython，日常在 Windows 上还会有 Anaconda，它有自己单独的版本号，但一般是用 CPython 的某个大版本为基础，然后加入平时常用或不常用的上百个包，打在一起作为一个 Anaconda 而发布。在 Windows 上由于编译环境不太一致，有很多安装包很难使用 pip 处理，因此 Anaconda 作为一个省心的安装环境而被广泛使用。但有人不喜欢 Anaconda 默认安装了那么多包，因此也有一个轻量化的版本 miniconda，差别就是默认安装的包极少。

为什么它们的名字叫 conda，而不是 AnaPython？因为它们都集成了 [conda](https://conda.io/docs/intro.html) 这个较为方便的包，下文会说它干嘛的。

## Windows 上没有包管理器的困难

对一个没有包管理器的发行版来说，用起来会比较困难。首先在 Windows 平台上没有方便的系统级包管理器，这样有的需要编译的 python 库就会较难安装（需要 Windows 上已有标准的 C Compiler 和所有编译所需的 C 环境）。事实上，在一个干净的 ubuntu 环境下（没有安装 gcc 等开发工具之前），（跳过包管理器而直接二进制分发的） python 在用 pip 安装第三方库时同样也会报错。

所以我们很少见到在 Windows 上用 pip 安装的库，大多数库会自己提供 python-xxx.msi 安装文件直接提供面向 windows 的二进制的分发。
同样由于这个原因，无论是自带的 conda 还是 pip，虽然它们被设计用来作为 CPython 平台的包管理器，但是若发行版将其打包进来（放到 anaconda 里），依然很难实现 windows 上的包安装。

类似的问题其实并不少见，TeX Live / MacTeX 就是一个 linux / mac 上的发行版，它同样：没有包管理器、打包了一大堆库体积巨大、安装后就基本覆盖了一切常用需求。但是，如果需要装一个 latex 的新依赖库或者模板，我们都只能下载下来放到编译文件夹下，然后再编辑自己的文档引用这个模板，最后一起编译。

因此，推荐在 windows 上安装标准的 CPython 2/3、或者用 anaconda 提供安装足够多的依赖库，但在 windows 上原生运行时加倍小心。

## 不同的发行版路径问题

为了方便，把一个 python 执行文件及所有依赖库，及所有第三方 python 库，称为一个 python 发行版。当有多个 python 发行版存在于不同文件夹下时，以系统环境变量 \$PATH 从左到右的顺序为准进行寻找，无论是 python、pip、或者 python 编写的系统工具，均以第一个找到的为准。但可以通过带绝对路径的方式去调用任何一个 python。

现代 linux 默认会用包管理器安装一个 python 作为系统级工具（如 RH 和 CentOS 的一些系统管理工具本身就是用 python 写的），这个可以看做是系统提供的发行版。一般执行文件是 /usr/bin/python。由于是系统级的，系统会对它提供良好的工具支持并保证稳定，因此想要修改这个 python 的发行版时（例如安装新的库）一概需要 sudo 来给予系统权限。例如 `sudo pip install tensorflow` 或者 `sudo apt-get install python-tornado`。后一种由于发行版收录时间问题，大多库会比较老，但是保持稳定。这些库一般会被安装到 /usr/lib/pythonX.X/site-packages 之类的地方。

如果自己编译安装一个 python，可以在 configure 时指定 prefix（比如 /home/cipir/mypython）。安装完后执行文件是 /home/cipir/mypython/bin/python。这时如果执行 pip 则将第三方库安装到 /home/cipir/mypython/lib/pythonX.X/site-packages 之类的地方。

# 最佳实践

## 项目依赖隔离

最常见的问题是，一个人或一个团队里每个人用 python 写了不同的项目，但是这些项目要求依赖的库却由于历史原因有不同的版本，例如一个需要 TensorFlow 0.9 而另一个要 TensorFlow 1.3，最佳的方式并不是强制要求低版本的代码去兼容新的，这样浪费不少人力，而是去同时支持不同项目所需版本。

python 2 并不包含这个功能，需要先安装 virtualenv 包，即 `pip2 install virtualenv`。然后就可以用 `virtualenv myproj_env` 建立一个新的环境。而 python 3 自带了这个功能，直接使用 `python3 -m env myproj_env` 即可建立一个新的环境。

新的环境是什么意思呢？其实它们的工作方式是在当前目录下新建一个 `myproj_env` 文件夹，然后将当前调用它的 python 执行文件本身，以及对应的 pip、setuptools 等极少数包拷贝到这个文件夹下。这样相当于有了一个全新的python发行版。同时环境本身提供了一个激活功能，`source myproj_env/bin/activate` 即可激活当前环境（其实就是修改 PATH、PYTHONPATH 等等环境变量），这时执行 python 会首先找到这个被拷贝过来的 python，而依赖库也会从刚刚新建的目录下面去找。再敲 `deactive` 可以取消激活，恢复到正常的 python 中。

但是，当一个项目发布出去以后，怎么让别人迅速装上对应的依赖库（无论对方直接用全局安装，或是也想用 virtualenv）？一般的实践是用 `pip freeze > requirements.txt` 将当前 env（此环境包含且仅包含项目所需库文件）中所有依赖库全部导入到一个 requirements.txt 文件里保存，并加入 git 版本控制。别人拿到源代码之后，可以直接建立一个当前项目中的 env，同时从此文件里安装依赖库：

```
git clone http://example.com/somebody/your-project.git here
cd here
virtualenv env
source env/bin/activate
pip install -r requirements.txt
```

### 怎么管理 virtualenv

GitHub 默认的 .gitignore 文件中会有一行 `env` 即忽略当前项目中 env 这个文件夹，不加入 git 版本控制。因此，对于一个一般的项目，就直接在当前文件夹下建一个 env，只对当前项目使用，无疑是最方便、最好记的。甚至可以搜到一些自动化脚本，当一 cd 到这个目录下时自动激活对应环境，而 cd 出去以后自动取消激活。

但是不推荐使用这种自动化脚本。为什么？上面说了 env 其实就是 python 发行版的拷贝。没有任何规定说必须一个 env 对应一个项目，当有一批项目和另一批项目要做依赖隔离，而每一方项目其实共享一个版本时，怎么办？
于是有一些自动化脚本会选择将 env 建立到 ~/.some-arbitary-path/env-type1 即输入 `virtualenv ~/.myenvs/env-type1` 建立一个 type1 的 env，只要自己知道这个是干什么的，那自然所有可以共存的项目就能都在激活它的情况下使用了。

因此，env 可以说是一个最小粒度的配置，并不需要更多自动化脚本来增加无谓的便利：有的 env 可以选择放在 home 目录下面的某个地方，用来跨项目共享（从而省硬盘空间），而有的 env 可以选择放在项目目录下，只要自己知道每个 env 是干嘛的就行。

上面提到的 conda 其实就是 virtualenv + pip 的合体，直接 conda 可以新建独立 env 以及 pip。

## python 发行版隔离

同样，如果有一份代码是 python 3.4 的，另一份需要 python 2.7，还有一份需要 python 3.5，而 env 是拷贝对应的 python，实际拷过来的都是 2.7，因此我们还迫切需要用不同的 python 版本。
这时推荐使用 [pyenv](https://github.com/pyenv/pyenv) 工具。具体的使用方式可以看文档，但主要 `pyenv install 3.5.1` 即可安装一个版本，`pyenv global 3.5.1` 则可以将全局默认的 python 切换为3.5.1版本，同时 pyenv 还可以安装大量的其他发行版，如 pypy、cython、anaconda、miniconda 等等。

pyenv 实际上是编译安装，但它管理好了所有目录设置相关的事情（全都在它安装目录下，如~/.pyenv），只需要 git clone 一下项目目录就可以。pyenv 一般会设置在 PATH 的第一位，然后将 python 版本的寻找职责从系统那里抢过来，再按 pyenv 自己配置去找到对应的 python 位置（而不再是 PATH 变量里从左到右的顺序）。

有了不同的 pyenv 的 python 之后，还可以直接将其配置到 pycharm 中作为远程解释器，例如 2.7.13 可以直接在 pycharm 里填 python 执行文件为：\$PYENV_ROOT/versions/2.7.13/bin/python2.7。

而 pyenv 也有一个 virtualenv 的插件，会将所有 env 放在 \$PYENV_ROOT 下面的某个地方。同样由于上面的理由，也不推荐使用自动化 pyenv 和 virtualenv 的插件。直接用 source 激活、deactive 取消激活是最方便的。

# FAQ

\1. Q: 为什么我配好了 pyenv，运行 ipython 时依然调用的是系统的 python？  
A: 可能由于命令行环境设置的问题，直接断开连接，重新连接服务器即可。

\2. Q: 为什么我配好了 pyenv 导致其他软件（如 vim）安装失败？  
A: pyenv 是另一套编译安装的 python，例如 UCS 编译选项默认为4，曾经导致 vim 在带上 python 扩展支持进行编译安装时提示找不到一些符号而失败。已经由 vim 方面更新了编译脚本后解决。若其他软件依然遇到此类问题，将 pyenv 临时切换为 system 即可。

\3. Q: 为什么在用了 pyenv 以后有的脚本跑不起来，提示找不到 sqlite3、pysqlite2 或者 bz2 库？  
A: pyenv 是编译安装，而 python 编译时会自动检测系统中有没有对应的 C 依赖库，若不存在，则自动关闭 python 中 bz2 和 sqlite3 的对应编译选项（这点非常令人无语，也就是说虽然这是 python 标准库里的东西但仍可能是机器上缺少的）。因此遇到找不到库之前一般先要搜索一下对应系统的 C 依赖库的包，例如 ubuntu 找不到 sqlite3 要先安装 `sudo apt install libsqlite3-dev`，然后用 `pyenv uninstall 2.7.13 && pyenv install 2.7.13` 重新安装 python（此时依赖库会全部丢失，需要全部重新安装）。







