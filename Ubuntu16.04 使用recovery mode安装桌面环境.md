# Ubuntu16.04 使用recovery mode安装桌面环境

## 故障描述：

ubuntu16.04 系统开机后，鼠标键盘失灵，没有任何反应，操作不了，也进不了系统，重启也不行。



## 故障原因：

在系统上安装了类似桌面主题或者格式的软件，导致ubuntu的桌面环境被毁掉了，即系统的ubuntu-desktop找不到路径了。



## 解决方法：

#### 1. 强制重启电脑（按电源键），同时一直按“F12”键，进入GRUB界面（不同电脑进入GRUB按键不一样，其他的电脑请百度查找）。

#### 2. 选择recovery mode，按Enter进入。**各菜单命令：** 

 **<1> resume**

```
resume --- Resume normal boot
继续正常启动，这个模式是提供给那些不小心误入此选项的使用者使用，可以使他们继续之前正常的启动模式
```

**<2> clean**

```
clean --- Try to make free space
清理软件包，尝试清除硬盘中不必要的档案存放空间。
```

**<3> dpkg**

```
dpkg --- Repair broken packages
修护损害的安装包
```

**<4> failsafeX**

```
failsafeX --- Run in failsafe graphic mode
以安全的图形模式运行
```

**<5> fsck**

```
fsck --- Check all file system
硬盘检查与修护坏道/逻辑坏道，进行磁盘/硬盘扫描修复，适合不正常的开机使用
```

**<6> grub**

```
grub --- Update grub bootloader
更新grub引导
```

**<7> network**

```
network --- Enable networking
带网络连接的shell界面，直接以文字模式启动，并且开启网络连线支援
```

**<8> root**

```
root --- Drop to root shell prompt
最高的管理员的shell界面
```

**<9> system-summary**

```
system-summary --- System summary
查看系统的信息/资料
```

 #### 3. 选择resume --- Resume normal boot选项进入(Enter)。

#### 4. 进入后，按Ctrl + Alt + F1进入 tty 命令模式；界面提示用户登陆，输入用户名和密码。

#### 5. 连接网络，以wifi为例，输入命令：

~~~
nmcli dev wifi connect 网络名称 password 密码
~~~

#### 6. 安装ubuntu-desktop，执行命令：

```
sudo apt-get install ubuntu-desktop
```

#### 7. 安装完后，执行重启命令

```
sudo shutdown -r now
或
sudo reroot
```



## 参考文献

https://www.jianshu.com/p/53cc63693cdc

https://blog.csdn.net/chichoxian/article/details/72873746

https://blog.csdn.net/zjwcdd/article/details/51496573

 

