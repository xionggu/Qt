之前在windows下Qt的可执行程序，附带对应的库文件即可，到了linux下面，原理一样。

Qt发布程序，一般默认缺少libQtCore.so.4和libQtGui.so.4两个文件，需要从/qtsdk2010/qt/lib下面找到这两个文件，
而不是/qtsdk2010/lib下面的文件，将libQtCore.so.4.7.0和libQtGui.so.4.7.0复制到与可执行文件同一目录，
将文件名称改为libQtCore.so.4和libQtGui.so.4，然后编写同名shell文件，内容如下：
```
#!/bin/sh
appname=`basename$0 | sed s,\.sh$,,`

dirname=`dirname$0`
tmp="${dirname#?}"

if[ "${dirname%$tmp}" != "/" ]; then
dirname=$PWD/$dirname
fi

export LD_LIBRARY_PATH=$dirname/lib
export QT_PLUGIN_PATH=$dirname/plugin
export QT_QWS_FONTDIR=$dirname/fonts

$dirname/$appname$*

```

主要是：libQtCore.so.4（大概22MB）libQtGui.so.4（大概102.MB）这两个库，四个文件放到同一目录，拷贝到其他linux系统下面即可运行。


如果有些程序不确定需要哪些库，可以用ldd命令查看，例如ldd hello，下面会显示对应依赖的库文件名称以及路径，对应拷贝过来即可。

## 注意：
+ 有些库是快捷方式，不要忘记拷贝真实库，可通过ls -l查看并拷贝到lib路径上
+ 脚本名字必须和程序名字一样，例如hello可执行文件，对应shell文件为hello.sh，
