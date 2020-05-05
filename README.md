# 前言

1. 刚刚接触 Git 时，通篇阅读了官方的 [Pro Git Book](https://git-scm.com/book/zh/v2) ，实际操作时命令行一个也没记住，就直接使用了Git-GUI。

2. 官网下载的 [Git](https://git-scm.com/downloads) 安装包，只有英文界面，没有任何其它语言。网上能找到的中文语言文件大概是10多年前的了。

4. 语言文件本身内容是纯文本的，格式是符合某规范的，工具在源码里也有。在规范不了解，工具不会用的情况下完全纯手工打造应该更快一些。

5. [Git 官网](https://git-scm.com/downloads) 下载有时很慢，[镜像站](https://npm.taobao.org/mirrors/git-for-windows/) 下载很快，但是新版本只有安装包没有源码。

# 工具软件

1. [notepad++](https://notepad-plus-plus.org/)
2. [金山词霸](http://www.iciba.com/)
3. [Typora](https://typora.io/)
4. [BC](http://www.scootersoftware.com/)

# 文件说明

* 如果 $GITROOT/mingw64/share/git-gui/lib/msgs/zh_cn.msg 文件存在，在中文系统中 Git-GUI 就会自动显示中文界面。
* 可以从[源码](https://github.com/git/git)或[分支](https://github.com/git-for-windows/git)中获得需要翻译的内容。
* $git-master\git-gui\po\git-gui.pot 是模板文件，包含了需要翻译的英文。
* $git-master\git-gui\po\zh_cn.po 是中文源文件（已经10多年没有更新了，包含了某些翻译内容）。
* $git-master\git-gui\po\glossary\zh_cn.po 是中文术语表，翻译时尽可能参照这个。

# 文件格式说明
* 制作语言文件就是把 zh\_cn.po 参照 git-gui.pot 转换成 zh_cn.msg。
* po、pot、msg 文件中，以井号(#)开头的行为注释行，不起作用。
* zh\_cn.po 或 git-gui.pot 文件：
    * \# 开头的行为注释行，不起作用。
    * msgid 开始的多行为原文，即英文界面显示的内容。
    * msgstr 开始的多行为翻译后的文本，即中文界面显示的内容。
    * 文件中 msgid 为空的行及其相关的 msgstr 是提供给工具使用的。
* zh\_cn.msg 文件：
    * 由若干行文本组成，每行为一项翻译内容。
    * 以井号(#)开头的行为注释行，不起作用。
    * 有效行格式固定为：**::msgcat::mcset zh_cn "msgid" "msgstr"** 。
    * 其中，**msgid **为英文原文，**msgstr** 为翻译后的中文。

# 制作过程详述
1. 使用软件 notepad++ 打开源文件 zh\_cn.po 和 git-gui.pot  。
2. 删除文件中以 **#** 开头的注释行。

   1. 使用 “Ctrl+H” 快捷键，调出“替换”对话框；
   2. “查找目标：”框中输入 `^#.*$\n` ；
   3. 删除“替换为：”框中的所有内容，使其为空；
   4. 勾选“循环查找”；
   5. “查找模式”选择“正则表达式”；
   6. 点击“替换所有打开文件”。
3. 合并文件中 **msgid** 和 **msgstr** 后面的多行字符串为1行。
   1. 方法参考第 2 步；
   2. “查找目标：”框中输入 `"\n"` 即可。
4. 合并 **msgstr** 行的内容到 **msgid** 行后面，同时去除字符 **msgstr** ，保留空白符和字符串。
   1. 方法参考第 2 步；
   2. “查找目标：”框中输入 `\nmsgstr` 即可。
5. 替换字符 **msgid** 为 **::msgcat::mcset zh_cn** ，保留空白字符。
   1. 方法参考第 2 步；
   2. “查找目标：”框中输入`msgid` ；
   3. “替换为：”框中输入 `::msgcat::mcset zh_cn` 即可。
6. 删除空白行，此时文件内容格式与 zh_cn.msg 一致。
   1. 方法参考第 2 步；
   2. “查找目标：”框中输入 `^\s+` 即可。
7. 对文件 zh\_cn.po 和 git-gui.pot  **分别进行排序**，以便于合并文件。
   1. 依次选择菜单 “编辑” -> “行操作” -> “升序排列文本行”；
   2. 排序号首行会变为空白行，删除即可；
   3. 此时文件 git-gui.pot 为 580 行，文件 zh_cn.po 为 391 行，可知有好多行未翻译。
8. 使用比较软件合并 zh\_cn.po 文件内容到 git-gui.pot 文件中。
9. 合并原  zh_cn.msg 文件内容 到 git-gui.pot  文件中，生成新的 zh_cn.msg 文件。
10. 格式为：**::msgcat::mcset zh_cn "msgid" ""** 的行需要在末尾的引号中间添加相应的翻译内容；否则，Git-GUI 界面上原来显示 **msgid** 的地方将显示为空白。
11. 可以在行首添加字符 **#** 注释掉不需要翻译的行，则界面上将显示原文 **msgid**。
12. 双引号中的 `"$[]` 等符号需要在前面添加转义字符 `\` ，否则 git-gui 会报错，运行失败。
13. 将文件 zh\_cn.msg 拷贝到前述位置即可。

# 更多工作

1. git-gui 中 “图形化显示分支” 的菜单会调用 gitk 工具，gitk 是英文界面。
2. 源码中也提供了 gitk 的 zh_cn.po 文件，可参照上述过程制作 zh_cn.msg 文件。
3. 因为 gitk 不支持 msgid 为空的翻译，所以要删除该行，也就是 zh_cn.msg 文件的首行。
4. 拷贝到 $GITROOT/mingw64/share/gitk/lib/msgs/zh_cn.msg 即可。