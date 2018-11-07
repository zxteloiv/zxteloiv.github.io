---
layout: post
title:  "macOS上启用2K显示器的HiDPI支持"
date:   2018-11-07 20:05 +0800
tags: macos display_manager hidpi 2k
categories: chn
---

最近购入了一台 AOC 的 2K 显示器，收到才发现不能直接在系统里开启 HiDPI 功能，即用4个物理像素显示1个点。
在 retina MacBook Pro 上，这样的效果能让眼睛更舒服，一旦体验就再也回不去了。
这里记录一下在 2K 显示器上开启此功能的过程，并总结一下搜到的其他帖子。

### 1. 缘起

2K 默认的分辨率为 2560 X 1440，这样一算，默认能实现的最大 HiDPI 模式下得分辨率为（宽高各除以二）1280 X 720。这台机器可以通过两边留黑边的方式变为16:10模式，即 2560 X 1600 从而得到 1280 X 800 的 HiDPI。

这时只有如下几种选择：
- 退货买 4K, 如果你已经有4K外接显示器，macOS 可以自己适配，并支持 1080p 级别的 HiDPI，可以不用看下文
- 坚持使用1280 X 800 的分辨率，然而高宽在如今都偏小，有的网页浏览可能非常困难，日常工作区也捉襟见肘，不方便
- 想办法在 2K 显示器下支持 HiDPI，穷人的选择

虽然通过一番计算好像并不可行，但不知是显示器隐藏了更高分辨率的支持，或是系统有软件的方式，总之其实可以实现。

### 2. 基本的思想

最关键的目标其实是在系统的 `/System/Library/Displays/Contents/Resources/Overrides` 目录下写入对应的显卡适配文件。此目录下有各个显卡制造商的子文件夹，每个制造商文件夹下再放各款型号的配置。如路径所暗示，这里其实只需要添加并覆盖（Override）显示器配置信息即可，并不需要列出所有的显示器支持的分辨率。所以其实我们的目的就是在这里写入开启 HiDPI 所需要的分辨率即可。

> **容易发现，因为这个目录是 /System 的系统路径，macOS 的 SIP 功能会阻止一切修改，哪怕 sudo 执行也一样，因此需要先重启关闭 SIP 等一切结束后再重新打开，无论使用任何方法此步骤都不可省略。**

已经有很多工具可以帮我们做这件事：

- [SwitchResX](http://www.madrau.com/index.html)，收费软件，可以免费试用7天。其实也是帮助创建此文件，但有很多别的显示功能，与 HiDPI 无关。
- [enable-HiDPI](https://github.com/syscl/Enable-HiDPI-OSX)，一个 shell 脚本，在命令行中输入想要的分辨率后即可。

但其实手动处理也非常容易且干净，下面几节简单记录一下过程，帮助大家省下付费软件和4K显示器的费用。

### 3. 构造显示器配置文件

首先关闭 SIP，不再赘述。

之后，需要在系统配置里打开 HiDPI 的开关。在终端中执行以下命令即可。

```bash
sudo defaults write /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled -bool true
```

和 macOS 的其他系统选项一样，可以使用如下命令查看一下修改效果，返回为1的话表示这一项已经设好。

```bash
sudo defaults read /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled 
```

然后就需要随便在任意地方新建一个文件，用于记录我们想要的分辨率。如上节所述，需要先获得显示器制造商编号（DisplayVendorID）和设备编号(DisplayProductID)，可以通过系统命令 `ioreg` 获得：

```bash
ioreg -lw0 | ggrep IODisplayPrefsKey
ioreg -l | grep DisplayVendorID
ioreg -l | grep DisplayProductID
```

这两个值的十进制和十六进制形式都需要用到，方便起见可以直接使用第一个命令，得到16进制表示。例如，我的机器上有如下返回值

```
.... IOService:/.........   /AppleDisplay-5e3-2490
```

注意最后的一段，AppleDisplay-5e3-2490 即可以得到制造商 ID 为5e3，设备 ID 为2490，均为16进制。而使用后两个命令，也可以得到这两个值的十进制，即 9360 和 1507，需要先自己转换为16进制。

这两个值需要写入即将创建的显示器配置文件中，也会确定该配置文件所放的路径。
使用[这个在线工具](https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/)会比较方便，也能直观地看到最终的XML文件。

在右侧填入制造商和设备ID的16进制，再添加 Scale Resolutions 即可。注意，这个工具中的 ProductName 是可以随便填的，起任何个性的名称均可，只要保证两个ID准确即可。

> 如果使用 SwitchResX 工具会发现，可添加的分辨率有很多种，本文只关注开启 HiDPI 的方法，所以只添加一种分辨率即 Scale Resolution

默认这个工具提供了很多分辨率设置，可以按自己需要添加分辨率。常见的可能设置的分辨率如下：

- 2560 x 1440：是显示器的16:9默认值，勾选 HiDPI 的话可以得到 2560 x 1440 以及 1280 x 720 (HiDPI) 两个分辨率
- 3840 x 2160：为了实现 1080p 即 1920 x 1080 的 HiDPI 效果，需要添加这个分辨率，宽高都是 1080p 的两倍，即4K分辨率大小
- 3200 x 1800: 如果觉得 1080p 显示内容太小，显示器尺寸或者距离不习惯，想要得到 1600 x 900 (HiDPI) 的话，可以使用这个配置，会比我们折腾之前的 1280 x 800 好一点。建议添加，多一个选择。

如果有比 1080p 更高的要求，可以做类似的设置。每设置一个分辨率，工具会给出具体形式，方便我们知道配置文件中每一项是如何得来的。

如果外接显示器是可以旋转的话，还需要添加对应的旋转后的分辨率，对应上面三种分别为：

- 1440 x 2560
- 2160 x 3840
- 1800 x 3200

否则在旋转模式下显示器会失去响应，重新接上的话会使用显示器最初默认的 1440 x 2560 模式。

综上，我们得到了一个对应的 XML 文件，可以自己复制出来或点击下载。本文中的 xml 文件如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>DisplayProductName</key>
  <string>Q2490PXG</string>
  <key>DisplayProductID</key>
  <integer>1507</integer>
  <key>DisplayVendorID</key>
  <integer>9360</integer>
  <key>scale-resolutions</key>
  <array>
    <data>AAAPAAAACHAAAAABACAAAA==</data>
    <data>AAAIcAAADwAAAAABACAAAA==</data>
    <data>AAAMgAAABwgAAAABACAAAA==</data>
    <data>AAAHCAAADIAAAAABACAAAA==</data>
  </array>
</dict>
</plist>
```

> 若使用下载得到 plist 文件，其实该文件依然是一个 xml，但只需要拷贝到对应目录即可，文件名本身没有影响，重要的是文件中的制造商ID和设备ID。

此工具上方会给出此文件的预期目录，本例中为 `/System/Library/Displays/Contents/Resources/Overrides/DisplayVendorID-5e3/DisplayProductID-2490`。将包含上述 XML 内容的文件放到此处即可。

### 4. 应用分辨率

由于系统目录下的东西已经开机加载完成，为了让修改生效必须重启。

仅仅将文件放进去后，在系统的显示设置里依然不能调整出需要的分辨率，如果设置成 1080p 其实还是拉伸放大的 1080p，显示非常虚，并没有开启 HiDPI 功能。为了应用，比较方便的做法是使用 Retina Display Manager(RDM) 开源工具。

如果本机装了开发工具链，可以[使用源码](https://github.com/avibrazil/RDM)自己编译，若没有编译器，也可以使用现成的[二进制包](http://avi.alkalay.net/software/RDM/)。

如下图所示，此时可以看到带闪电的分辨率就是我们开启的 HiDPI 分辨率。而如果没有经过这个设置，这里只会列出一系列设备支持的分辨率，以及最高 1280 x 800 的 HiDPI 分辨率。

![](https://cloud.githubusercontent.com/assets/3484242/7100316/255a7d74-dff0-11e4-9bf9-16e726336e29.png)

需要注意的是在竖屏模式下，旋转功能需要首先在系统的显示设置中开启，然后再使用 RDM 选择合适的竖屏 HiDPI 分辨率。
如果在设置旋转后显示器发生黑屏，需要拔下外接插头再插回去，此时再用 RDM 调整即可。
由于 rMBP 两个 thunderbolt2 口会分别记录上一次的配置，一般可以一个口用竖屏，一个口插横屏，切换的时候换一下口即可，非常方便，就可以避免每次旋转屏幕时遇到变黑的情况。

弄完以后再重启打开 SIP 功能即可。


### 参考

1. v2ex 的一个索引 https://www.v2ex.com/t/464426
2. BENQ 官方写的知乎专栏文章 https://zhuanlan.zhihu.com/p/36913571
3. 使用 SwitchResX 实现的知乎答案 https://www.zhihu.com/question/35300978
4. 上述提到的 enable-HiDPI.sh 工具脚本和显示配置的网页生成工具



