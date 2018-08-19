# BootRepair 教程

安装完双系统，如果在使用过程中不小心删除了Ubuntu引导向，则会导致开机后无法选择进入Ubuntu系统。或者当我们重装了windows系统后，也会发现原来的Ubuntu引导不见了，当出现这两种情况之一时，最好的解决办法不是重新把Ubuntu系统装一遍，我们只需要冲洗修复一下Ubuntu引导文件，就可以把问题解决了。 
不过首先你需要Ubuntu U盘启动盘，来进入Ubuntu系统来修复引导问题。

第一步：

还是需要进入Ubuntu界面，但是并不需要安装（如果直接安装的话，以前在Ubuntu里面的文件可全部都没有了，所以万不得已，千 
万别这样做）。

第二步： 
选择TRY Ubuntu选项，进入U盘的Ubuntu 试用系统，并连接好网络（因为后续工作需要用到网络）。

第三步： 
打开终端，终端快捷键是Ctrl+Alt+T，输入命令，添加boot-repair所在的源： 
sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt-get update

第四步： 
待上面命令执行完毕后，继续输入以下命令，安装boot-repair并且开启boot-repair： 
sudo apt-get install -y boot-repair && boot-repair

等一会，会出现如下的界面： 
这里写图片描述

就会出现这个，点击Recommended repair，过几分钟重启就行了。

第五步： 
如果上面已经执行成功了，可以跳过此部，否则，我们可以自己输入命令进行修复：

　　sudo recommended repair

　 成功后，就会弹出我们的盘的各种信息以及引导的信息。 
　 如果有些人不小心点击了Create a BootInfo summary的话，那你的开机启动界面将会出来一大堆你以前没见过的东西。 
那样的话，你可以输入名令：cd /boot/grub

接着输入sudo gedit grub.cfg,打开grub.cfg文件后，通过搜索找到windows，然后把下面这些删去就和原来一样了。

BEGIN /etc/grub.d/25_custom

menuentry “efi/EFI/Boot/bootx64.efi” { 
search –fs-uuid –no-floppy –set=root d000ed6a-5303-40aa-a517-af50e807c0e9 
chainloader (${root})/efi/EFI/Boot/bootx64.efi 
}

menuentry “efi/EFI/ubuntu/MokManager.efi” { 
search –fs-uuid –no-floppy –set=root d000ed6a-5303-40aa-a517-af50e807c0e9 
chainloader (${root})/efi/EFI/ubuntu/MokManager.efi 
}

menuentry “Windows UEFI recovery bootmgfw.efi” { 
search –fs-uuid –no-floppy –set=root A603-846C 
chainloader (${root})/EFI/Microsoft/Boot/bootmgfw.efi 
}

menuentry “Windows Boot UEFI recovery” { 
search –fs-uuid –no-floppy –set=root A603-846C 
chainloader (${root})/EFI/Boot/bkpbootx64.efi 
}

menuentry “EFI/ubuntu/MokManager.efi sda2” { 
search –fs-uuid –no-floppy –set=root A603-846C 
chainloader (${root})/EFI/ubuntu/MokManager.efi 
}

menuentry “Windows UEFI recovery LrsBootmgr.efi” { 
search –fs-uuid –no-floppy –set=root 7607-5674 
chainloader (${root})/efi/Microsoft/Boot/LrsBootmgr.efi 
}

menuentry “Windows Boot UEFI recovery bkpbootx64.efi” { 
search –fs-uuid –no-floppy –set=root 7607-5674 
chainloader (${root})/efi/Boot/bkpbootx64.efi 
}

END /etc/grub.d/25_custom

参考博文：http://m.blog.csdn.net/article/details?id=50589667
         https://blog.csdn.net/u012260238/article/details/52713724
