---
layout: post
title:  "用 zotero 管理文献和个人知识库"
date:   2015-01-03 23:09:00
tags: zotero
categories: chn
---

这周开始用了一下 [zotero](https://www.zotero.org/)。  
这是一款非常好用的文献管理工具，但它更像是一个多功能的收藏夹，或者说是一个知识、笔记、文献的综合管理工具。  
入门的时候搜到了阳志平老师的几个图文并茂的教程，这里也推荐一下：http://www.yangzhiping.com/tech/zotero1.html

### zotero concepts

zotero 最初是一个文献工具，但它管理的最小单位是 item，每个 item 可以是网页、书等，逐渐可以扩展为一个全能的知识工具。  
一个单独的 item 可以嵌入另一个 item 的下面，但已经有嵌入条目的是不可以再嵌入别的条目下的。

实际的使用例子：

1. 一个 blog 网页可以作为单独的 item
2. 一个豆瓣图书网页可以作为一个 item，并且嵌入任意多个 URI 或者本地的 PDF 文件，可以是本书的全文或摘要或者习题答案
3. 一个 Google Scholar 搜索结果的目标页可以作为一个 item
4. 一本单独的书籍，或许在互联网上没有对应的描述URI，也可以建立一个 item，并填上对应的作者、出版社、年份等等

本文并不包含怎么用 zotero 比较好的讨论，只是一些软件使用细节上的介绍，避免后来人踩坑。

### install

zotero 官网提供了各个平台的下载链接，它可以运行于各操作系统甚至浏览器中。但是推荐使用独立安装(standalone)版本。  
arch linux 下可以直接从 AUR 安装。

为了便于收藏网页，一般除了 standalone 版本，还要再装一个浏览器插件，但是根据我自己实验的结果， Firefox 版本要特别说明一下。

以 Firefox 方式安装，是可以拥有 zotero 的全部功能的。  
standalone 方式安装后，如果再装一个 firefox xpi 插件，实际上也会同样装上全功能的 firefox 版本，其他浏览器插件由于没有原生的 zotero，所以不用担心这里的问题。

通过下一节的恰当设置，可以让 firefox 版本和 standalone 版本和谐共处。

### configure

#### zotero 用户文件内容

zotero 的用户数据一般来说包括几个主要部分：

1. 系统自己的配置，如首选项设置的保存文件
2. 用户的文献库的索引和结构等数据，保存在用户文件夹里
3. 用户所有嵌入的 PDF 全文等文件，存放再上面的用户文件夹下的 storage 文件夹中

第1个一般不用管理，哪怕重装电脑丢了也不心疼。下面两个需要做一些处理。

一般来说，为了方便配合各人电脑已有的分区管理，我们应该将用户文件夹移动到某个位置，而不是默认的`C:/Users/xxx/Documents/`或者`~/.zotero/blabla`下面。

首先进入 zotero 的 Preference 中，切换到 Advanced - Files and Folders 下面。直接修改 Data Directory Location 为 Custom，并指定另一个位置，比如 `~/data/zotero`。

这样我们单机版的 zotero 就完成了，之后再正常使用即可。如果你需要多台电脑同步，再看下面。

#### zotero 同步设置

zotero 有 `sync` 功能，可以同步上述2、3两个文件夹。注册一个官网帐号，然后到 Preference - Sync - Settings 中填上即可。

但是我们一般不希望 zotero 将上述3类 storage 文件夹也做同步。因为大量的 PDF 文件应该很大，而且 zotero 也没有那么大的云空间，于是我们就有两种处理方式了。

一种方法是，关闭 zotero 对附加文件夹的同步：回到上述 Sync - Settings 中取消下面的打勾。然后我们另外使用其他国内的云盘，如百度云、坚果云等，将对应的 storage 文件夹纳入同步就可以了。  
在 *nix 系统下面有一个 trick，即可以将 storage 文件夹删除，用一个 symbolic link 来替代它，这样我们还可以进一步地自定义 storage 文件夹的位置和名称，也照顾了云盘的使用、方便管理。  
当然，如果在多台电脑同步，那就在多台电脑上都需要对云盘做类似设置。

另一种方法是，使用 zotero 的 WebDAV 方式同步。只需提供其他支持 WebDAV 的网盘即可，如坚果云，在坚果云后台 "设置" - "安全" 中可以看到 WebDAV 的路径，甚至可以设置一个单独密码，以便不用暴露坚果云的密码到 zotero 中。  
我们回到 zotero 中 Preference - Sync - Settings 下面下拉框中将 "Zotero" 改成 "WebDAV"，然后选默认 https，再填 "dav.jianguoyun.com/dav" 到后面的框中。下面的用户名是坚果云的邮箱名，密码是坚果云的密码或者独立密码。使用 Verify Server 功能可以验证是否成功。

这样之后，zotero 会在坚果云账户里建立一个 zotero 的子文件夹（放心，不会包含URL路径的dav字样）。所有的 storage 内容就会放到这里了，并且云盘上存的是经过压缩后的，而不像本地一样是原始 PDF，不会占用太多云盘上的空间。同时，在坚果云客户端中可以关闭对此文件夹的同步，因为我们并不关心此文件夹的结构和内容，它就是一个给 zotero 专用的区域。

官方也有一些同步的建议: https://www.zotero.org/support/sync

#### firefox 插件

其他系统的插件如果没有像 firefox 这样功能完整的话，就避免了此处的问题，可以跳过此节。

首先需要关闭 zotero standalone 版本，再打开 firefox，按 Ctrl + Shift + Z 或者点插件图标，会在火狐中打开一个类似 standalone 版本的下边栏。

firefox 此时并不知道 standalone 版本的存在，需要点击齿轮图标，进入 Preference，可以看到插件本身的设置。然后将 Advanced - Files and Folders 下面的内容，以及 Sync - Settings 下面的内容，设置成和 standalone 版本完全一致。这样二者就共用一个用户文件夹了。

今后，如果 standalone 版本已经打开，是无法启动火狐插件的侧边栏的（估计是因为 zotero 对文件夹加了只读锁，不会允许多个 zotero 同时访问以免数据库被写坏）。

然后照阳志平老师的文章里一样，可以直接点导航栏上的图标，或者右键点击网页，在所有支持了添加 zotero 条目的页面上都可以将内容添加到 zotero 库了。

### backup and recovery

虽然有 sync 功能，但是 zotero 官方还是推荐了采用原始方法进行备份，这也是对重要资料一种稳妥的保护办法。

备份时，只需定时拷贝整个用户文件夹到别的地方，就可以了。

恢复时，关闭同步功能并退出程序，将备份文件夹内容全部拷贝回来，再重新打开程序，就可以原样恢复了。

但是，如果是彻底以本地备份为准，可能需要到 sync - reset 里抹去 zotero 服务器的备份。如果只是部分文件被修改，可以先在本地 duplicate item 部分条目，然后开启 sync，服务器的版本会同步下来把本地的覆盖掉，但是新 duplicate 出来的 item 是不会受影响的。这样可以删了被 sync 修改的，而新的 item 会自动 sync 回去。

具体可以看官方关于备份一节: https://www.zotero.org/support/zotero_data

