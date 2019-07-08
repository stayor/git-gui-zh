# 制作Git GUI中文语言文件
* 如果$GITROOT/share/git-gui/lib/msgs/zh_cn.msg文件存在，在中文系统中Git-GUI就会自动显示中文界面。
* 可以从[源码](https://github.com/git/git)或[镜像](https://npm.taobao.org/mirrors/git-for-windows/)中获得已经部分翻译的语言源文件：$git-2.22.0.windows\git-gui\po\zh_cn.po。
* $git-2.22.0.windows\git-gui\po\git-gui.pot是模板文件，包含了所有需要翻译的原文。
* 制作语言文件就是把zh\_cn.po或git-gui.pot转换成zh_cn.msg。
* po、pot、msg文件中，以井号(#)开头的行为注释行，不起作用。
* zh\_cn.po或git-gui.pot文件中：
    * \# 开头的行位注释行
    * msgid 开头的行为原文
    * msgstr 开头的行为翻译后的文本
* zh\_cn.msg文件由若干行文本组成，以井号(#)开头的行为注释行，不起作用。
* zh\_cn.msg文件有效行格式固定为：**::msgcat::mcset zh_cn "msgid" "msgstr"** 。
* 其中，**msgid**为英文原文，**msgstr**为翻译后的中文。

# 制作过程概述：
0. 打开源文件zh\_cn.po或git-gui.pot，另存为zh\_cn.msg。

1. 删除文件中以 **#** 开头的注释行。

2. 合并文件中 **msgid** 和 **msgstr** 后面的多行字符串为1行。

3. 合并 **msgstr** 行的内容到 **msgid** 行后面，并去除字符 **msgstr** ，保留空白符和字符串。

4. 替换字符 **msgid** 为 **::msgcat::mcset zh_cn** ，保留空白字符。

5. 格式为：**::msgcat::mcset zh_cn "msgid" ""** 的行需要添加相应的翻译内容；否则，Git-GUI 界面上原来显示 **msgid** 的地方将显示为空白。

6. 也可以用在行首添加字符 **#** ，注释掉此行，则界面上将显示原文 **msgid**。

7. 将文件zh\_cn.msg拷贝到上述位置即可。

# 更多工作
1. 经测试，Gui中的部分英文信息不会被翻译成中文。

2. git-gui.pot并没有包括全部界面信息。

3. 根据GUI上的英文信息，手动添加翻译内容。

   
