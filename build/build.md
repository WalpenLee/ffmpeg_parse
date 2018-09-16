win10+vs2013+ffmpeg
步骤如下：
1,下载FFMPEG源码，下载地址：https://ffmpeg.org/download.html； 
我用的是当前最新的版本FFmpeg-3.4

2，下载MinGW安装器，官方地址：http://www.mingw.org/； 
我用的是Mercurial-3.9-x64.exe，下载完成后安装在任意一个盘里（比如E），安装完成后点运行，标记上以下几项： 
这里写图片描述 
然后点击apply all changes，等待它配置完成。

3，下载yasm，下载地址： http://yasm.tortall.net/ ； 
我用的是vsyasm-1.3.0-win64，下载后改名为yasm.exe，复制到E:/MinGW/msys/1.0/bin目录下（根据你安装MinGW的位置改变，后面一样），将该文件的路径添加到环境变量中。

4， 配置D:/MinGW/msys/1.0/msys.bat，右击后点击“编辑”，在此文件的最前面(@echo off之后)添加一行如下内容： 
Call”C:\Program Files(x86)\Microsoft Visual Studio 12.0\VC\bin\vcvars32.bat” (依VS2013实际安装路径修改路径)。

5，重命名 E:/MinGW/msys/1.0/bin/link.exe 为link_renamed.exe (依实际安装选择路径)，这一步是防止这个link.exe与VC的link.exe发生冲突，编译完成后可修改回来。

6，下载pkg-config-0.23-2.zip及glib_2.18.4-1_win32.zip 
http://ftp.gnome.org/pub/gnome/binaries/win32/dependencies/pkg-config_0.23-3_win32.zip ,主要用到里面的pkg-config.exe； 
http://ftp.gnome.org/pub/gnome/binaries/win32/glib/2.18/glib_2.18.4-1_win32.zip ，主要用到里面的libglib-2.0-0.dll； 
把pkg-config.exe解压到E:/MinGW/bin/，把libglib-2.0-0.dll放在与之相同目录下。

7，配置pkg-config 
可以用notepad++打开E:/MinGW/msys/1.0/etc/profile文件

在 
if [ $MSYSTEM == MINGW32 ]; then 
… 
fi

后面加上下面的环境变量设置

if [ -z “$PKG_CONFIG” ]; then 
export PKG_CONFIG=E:/MinGW/bin/pkg-config.exe 
fi

//（注意这个地址要看你的MinGW安装位置） 
if [ -z “$PKG_CONFIG_PATH” ]; then

export PKG_CONFIG_PATH=MinGW/lib/pkgconfig:/usr/local/lib/pkgconfig 
fi

8, 配置编译，双击E:/MinGW/msys/1.0中的msys.bat，此时会打开一个命令行页面，最上面显示MinGW32:，此时应该转到你下载的FFmpeg-3.4所在的位置。 
命令行操作教程：

http://blog.csdn.net/rOokieMonkey/article/details/78668888

这里写图片描述
转到ffmpeg-3.4下后，即可输入配置命令： 
（1）静态库： 
./configure --enable-static --prefix=./vs2013_build 
（2）动态库： 
./configure --enable-shared --prefix=./vs2013_build 
等待配置完成。

9，编译 
输入命令：make all （这个过程要等待）

10,安装 
输入命令：make install