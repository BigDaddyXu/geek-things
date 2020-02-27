# 在WSL中开启Adb调试

无法直接在wsl中开启adb连接usb，需要在windows中也安装一个adb，两个共享后台进程。

## 安装Adb

分别在windows中和wsl中安装adb，注意，必须是同版本的adb。

windows可使用Android Studio来安装Android-SDK, wsl中可以下载android-sdk-tools包来安装（见[在WSL下安装Android-Sdk-Tools](https://github.com/OceanXuY/geek-things/blob/master/install-android-sdk-tools-on-wsl.md)）

```
# Windows Powershell
> adb version
Android Debug Bridge version 1.0.41
Version 29.0.6-6198805
Installed as D:\develop\android\sdk\platform-tools\adb.exe

# wsl
> adb version
Android Debug Bridge version 1.0.41
Version 29.0.6-6198805
Installed as /mnt/d/develop/wsl-android-sdk/platform-tools/adb
```

## 在windows中启动adb

```
# Windows
> adb kill-server
> adb start-server
```

## 在wsl中查看adb shell

```
# wsl
> adb shell
```

注意：若是在Linux子系统中不小心执行adb kill-server，在执行adb start-server是不会启动adb daemon程序的,必须按照此步骤启动adb服务.

```
# 第一步.win+R调出dos终端
> adb kill-server
> adb shell //dos进入adb shell

# 第二步：打开Linux子系统
> adb shell
```

参考文献：
1. https://mp.weixin.qq.com/s/36lyCdNERa8jcd1XWjNW0A
2. https://blog.csdn.net/weixin_42600072/article/details/103129441
