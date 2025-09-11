# 1. 输入法安装和开机自启动问题
https://zhuanlan.zhihu.com/p/508797663

前言
书接上回，一时兴起将主力机的 Ubuntu 20.04 LTS 升级至了刚刚发布的 22.04 LTS。从 X 切换到 Wayland 、GNOME 从 3.36 升级至 42、Python 默认为 3.10 等等……使用太新的软件包反而暂时带来了麻烦，部分原有的软件和插件都不可用了。这其中就包括已经很久没有更新的百度输入法。故需要寻找新的中文拼音输入法。经简单浏览对比，选择了 Fcitx 5。


小企鹅输入法
本文适用于 Ubuntu 22.04 及以上版本，20.04 可以参考：

Ubuntu20.04 安装fcitx5输入法 - 纯白的小站
ouyen.github.io/fcitx5-ubuntu/
安装
检查系统中文环境
在 Ubuntu 设置中打开「区域与语言」—— 「管理已安装的语言」，然后会自动检查已安装语言是否完整。若不完整，根据提示安装即可。


检查可用的语言支持
最小安装
为使用 Fcitx 5，需要安装三部分基本内容：

Fcitx 5 主程序
中文输入法引擎
图形界面相关
按照这个思路，可以直接使用 apt 进行安装：

sudo apt install fcitx5 \
fcitx5-chinese-addons \
fcitx5-frontend-gtk4 fcitx5-frontend-gtk3 fcitx5-frontend-gtk2 \
fcitx5-frontend-qt5
安装中文词库
在 GitHub 打开维基百科中文拼音词库的 Releases 界面，下载最新版的 .dict 文件。按照 README 的指导，将其复制到 ~/.local/share/fcitx5/pinyin/dictionaries/ 文件夹下即可。

# 下载词库文件
wget https://github.com/felixonmars/fcitx5-pinyin-zhwiki/releases/download/0.2.4/zhwiki-20220416.dict
# 创建存储目录
mkdir -p ~/.local/share/fcitx5/pinyin/dictionaries/
# 移动词库文件至该目录
mv zhwiki-20220416.dict ~/.local/share/fcitx5/pinyin/dictionaries/
配置
设置为默认输入法
使用 im-config 工具可以配置首选输入法，在任意命令行输入：

im-config
根据弹出窗口的提示，将首选输入法设置为 Fcitx 5 即可。

环境变量
需要为桌面会话设置环境变量，即将以下配置项写入某一配置文件中：

export XMODIFIERS=@im=fcitx
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
如果使用 Bash 作为 shell，则建议写入至 ~/.bash_profile ，这样只对当前用户生效，而不影响其他用户。

另一个可以写入此配置的文件为系统级的 /etc/profile 。


将配置写入到/etc/profile文件末尾
开机自启动
安装 Fcitx 5 后并没有自动添加到开机自启动中，每次开机后需要手动在应用程序中找到并启动，非常繁琐。

解决方案非常简单，在 Tweaks（sudo apt install gnome-tweaks）中将 Fcitx 5 添加到「开机启动程序」列表中即可。


将Fcitx5添加到开机启动程序列表中
Fcitx 配置
Fcitx 5 提供了一个基于 Qt 的强大易用的 GUI 配置工具，可以对输入法功能进行配置。有多种启动该配置工具的方法：

在应用程序列表中打开「Fcitx 配置」
在 Fcitx 托盘上右键打开「设置」
命令行命令 fcitx5-configtool
根据个人偏好进行设置即可。需要注意的是「输入法」标签页下，应将「键盘 - 英语」放在首位，拼音（或其他中文输入法）放在后面的位置。


Fcitx5 configtool
自定义主题
Fcitx 5 默认的外观比较朴素，用户可以根据喜好使用自定义主题。

第一种方式为使用经典用户界面，可以在 GitHub 搜索主题，然后在 Fcitx5 configtool —— 「附加组件」 —— 「经典用户界面」中设置即可。

第二种方式为使用 Kim面板，一种基于 DBus 接口的用户界面。此处安装了 Input Method Panel 这个 GNOME 扩展，黑色的风格与正在使用的 GNOME 主题 Orchis-dark 非常搭配。


Input Method Panel 效果


已知问题
修复 JetBrains 系 IDE 显示问题
在 JetBrains 系 IDE（如 PyCharm）中，输入法选择框的位置始终固定于屏幕左下角，而非随输入光标移动，在中文输入很不方便。该问题为 IDE 的 JetBrainsRuntime 缺陷所致。可尝试使用 RikudouPatrickstar/JetBrainsRuntime-for-Linux-x64 这个仓库发布的 JBR 文件解决。

卸载 iBus 影响 Fcitx 5 正常使用
出于精简空间和减少冲突干扰之考虑，使用

sudo apt remove ibus
卸载了 iBus，但重启（使生效）之后发现 Fcitx 5 受到了影响。具体表现为：除在终端中之外，其他输入场景无法切换至中文输入。使用 apt 装回 iBus，再次重启即又恢复正常。

检查包依赖关系，卸载 ibus 包后会自动移除 ibus-data、ibus-gtk4、python3-ibus-1.0 三个包，似乎都只是与 iBus 紧密联系的。暂为不解之谜。






# 2. 输入法在其他软件不能使用问题
https://zhuanlan.zhihu.com/p/597764721

① 下载安装相应软件
使用Linux中的软件管理工具下载fcitx5输入法相应的依赖软件：

在Debian系统及其衍生发行版中安装：
$ sudo apt-get install  fcitx5-frontend-qt5 fcitx5-frontend-gtk2 fcitx5-frontend-gtk3 fcitx5-pinyin fcitx5-chinese-addons fcitx5-chewing fcitx5-module-lua fcitx5-module-lua-common fcitx5-modules unicode-cldr-core
在Arch Linux系统及其衍生发行版中安装：
$ sudo pacman -S fcitx5-im fcitx5-qt fcitx5-gtk fcitx5-chinese-addons fcitx5-lua unicode-cldr
……
依赖解释：
fcitx5-frontend-qt5/fcitx5-qt - 为fcitx5输入法提供Qt5 IM模块。

fcitx5-frontend-gtk2/fcitx5-gkt - 为fcitx5输入法提供GTK2 IM模块。

fcitx5-frontend-gtk3/fcitx5-gtk - 为fcitx5输入法提供GTK3 IM模块。

fcitx5-pinyin - 为fcitx5输入法框架提供拼音支持。

fcitx5-chinese-addons - 为fcitx5输入法提供中文相关插件。

fcitx5-chewing - 为fcitx5输入法提供chewing繁体中文输入引擎。

fcitx5-module-lua/fcitx5-lua - 为fcitx5输入法提供lua支持。

fcitx5-module-lua-common/fcitx5-lua - 为fcitx5输入法提供lua支持的通用文件。

fcitx5-modules - fcitx5输入法框架的核心模块。

unicode-cldr-core/unicode-cldr - 来自Unicode CLDR的核心通用数据。

② 配置相应文件
配置/etc/environment文件或~/.pam_environment文件。
XIM=fcitx5
XIM_PROGRAM=fcitx5
GTK_IM_MODULE=fcitx5
QT_IM_MODULE=fcitx5
XMODIFIERS=@im=fcitx5
SDL_IM_MODULE=fcitx5
GLFW_IM_MODULE=fcitx5
若配置~/.pam_environment文件，则在该文件中输入如下内容：
export XIM=fcitx5
export XIM_PROGRAM=fcitx5
export GTK_IM_MODULE=fcitx5
export QT_IM_MODULE=fcitx5
export XMODIFIERS=@im=fcitx5
export SDL_IM_MODULE=fcitx5
export GLFW_IM_MODULE=fcitx5
警告：
读取 ~/.pam_environment 已被弃用，不再起作用。
③ 使得配置文件生效：
使用如下命令使得刚刚配置的文件生效：

若配置的/etc/environment文件，则在root用户下使用如下命令：

# source /etc/environment
若配置的~/.pam_environment文件，则使用如下命令：

$ source ~/.pam_environment
④ 配置完成，尽情享用吧~
重启计算机后即可享用fcitx5输入法。

提示：
fcitx5-material-color - 为fcitx5输入法提供类微软输入法主题的material color主题。
