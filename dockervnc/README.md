# Docker-Ubuntu-Unity-noVNC中文桌面版

## 更新特色:
- 直接通过ip登陆
- 移动端及PC端打开后屏幕分辨率自适应调整
- 增加中文显示支持,可添加搜狗输入法

Dockfile for Ubuntu with Unity desktop environment and noVNC. 

This **Image/Dockerfile** aims to create a container for **Ubuntu 16.04** with **Unity Desktop** and using **TightVNCServer**, **noVNC** which allow user use browser to log in into this container.


## How to use?

You can build this **Dockerfile** yourself:

```
sudo docker build -t "yuquant/ubuntu-unity-novnc" .
```

Or, just pull my **image**:

```
sudo docker pull yuquant/ubuntu-unity-novnc
```

The default usage of this image is:

```
sudo docker run -itd -p 80:6080 yuquant/ubuntu-unity-novnc
```

Wait for a few second, you can access http://localhost/vnc.html and see this screen:

![alt text](https://github.com/yuquant/Docker-Ubuntu-Unity-noVNC/raw/master/noVNC.png "vnc.html")


### Password

In default, the **password** will create randomly, to find the password, please using the following command:

```
sudo docker exec $CONTAINER_ID cat /home/ubuntu/password.txt
```

And you can use this password to log in into this container.

After log in, you can see this screen:

![alt text](https://github.com/yuquant/Docker-Ubuntu-Unity-noVNC/raw/master/desktop.png "Unity desktop")


## Arguments

This image contains 3 input argument:

1. Password

   You can set your own user password as you like:
   ```
   sudo docker run -itd -p 80:6080 -e PASSWORD=$YOUR_PASSWORD yuquant/ubuntu-unity-novnc
   ```
   Now, you can user your own password to log in.

2. Sudo

   In default, the user **ubuntu** will not be the sudoer, but if you need, you can use this command:
   ```
   sudo docker run -itd -p 80:6080 -e SUDO=yes yuquant/ubuntu-unity-novnc
   ```

   This command will grant the **sudo** to user **ubuntu**.

   And use **SUDO=YES**, **SUDO=Yes**, **SUDO=Y**, **SUDO=y** are also supported.

   To check the sudo is work , when you open **xTerm** it should show following message:
   ```
   To run a command as administrator (user "root"), use "sudo <command>".
   See "man sudo_root" for details.
   ```
   **Caution!!** allow your user as sudoer may cause security issues, use it carefully.
   如果想禁止使用su用户,可执行以下命令
   ```
   sudo docker run -itd -p 80:6080 -e SUDO=no yuquant/ubuntu-unity-novnc
   ```

## Screen size
默认情况下分辨率自动调整,以下仅供参考
The default setting of screen siz is 1600x900.

You can change screen by using following command, this will change screen size to 1024x768:

```
sudo docker exec $CONTAINER_ID sed -i "s|-geometry 1600x900|-geometry 1024x768|g" /etc/supervisor/conf.d/supervisor.conf
sudo docker restart $CONTAINER_ID
```

## 搜狗输入法安装
[Ubuntu16.04 安装搜狗输入法](https://blog.csdn.net/Nicky_lyu/article/details/53225399)

## Issues
TODO:部分docker cpu占用缓慢增长的情况.    
Can't work properly with gnome-terminal, use XTerm to place it.

Some components of Unity may not work properly with vncserver.
