#### Ubuntu14 安装搜狗输入法

安装 fcitx：  https://blog.csdn.net/witnessai1/article/details/78380153

下载搜狗输入法 for linux官网：https://pinyin.sogou.com/linux/?r=pinyin

* 在 Ubuntu14 下可以直接点击下载的文件 deb 进入软件中心进行安装

* 接下来就是在终端中输入 `im-config` ，这时会出现一个对话框，点击OK，有一个对话框，点击Yes，你会看到下面的对话框。如果上面是 fcitx ，就 不用管，直接关闭；如果不是，就修改上面的ibus为fcitx.点击OK即可。又会出现一个对话框，接着就是OK，最后重启电脑。
* 在终端中输入：`fcitx-config-gtk3` 出现对话框。点击对话框左下角的（+）按钮，弹出另一个对话框。然后，取消 Only Show Current Language（很重要，否则不能找到刚安装过的搜狗输入法最后，在输入框中输入sogou，选中点击OK即可。添加完后将其放置到列表的最下方， 注意，是最下方！！！然后默认输入法就是搜狗输入法了。 
