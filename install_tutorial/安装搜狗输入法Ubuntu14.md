#### Ubuntu14 安装搜狗输入法

安装 fcitx：  https://blog.csdn.net/witnessai1/article/details/78380153

第一步： 添加 fcitx 键盘输入法系统

sudo add-apt-repository ppa:fcitx-team/nightly

更新一下： sudo apt-get update

安装 fcitx： sudo apt-get install fcitx

安装 fcitx 的配置工具：sudo apt-get install fcitx-config-gtk

安装 fcitx 的 table-all 软件包：sudo apt-get install fcitx-table-all

安装 im-switch 切换工具：sudo apt-get install im-switch

第二步： 安装搜狗输入法

下载搜狗输入法 for linux官网：https://pinyin.sogou.com/linux/?r=pinyin

sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb

如果抱错， 安装依赖： sudo apt-get install -f

第三步： 设置语言选项

系统设置->语言设置, 将键盘输入法系统由默认的 iBus 改为 fcitx

接下来就是在终端中输入 `im-config` ，这时会出现一个对话框，点击OK，有一个对话框，点击Yes，你会看到下面的对话框。如果上面是 fcitx ，就 不用管，直接关闭；如果不是，就修改上面的ibus为fcitx.点击OK即可。又会出现一个对话框，接着就是OK，最后重启电脑。

在终端中输入：`fcitx-config-gtk3` 出现对话框。点击对话框左下角的（+）按钮，弹出另一个对话框。然后，取消 Only Show Current Language（很重要，否则不能找到刚安装过的搜狗输入法最后，在输入框中输入sogou，选中点击OK即可。添加完后将其放置到列表的最下方， 注意，是最下方！！！然后默认输入法就是搜狗输入法了。 


Ubuntu20.04 搜狗输入法打不出中文解决方案:

sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2

sudo apt install libgsettings-qt1
