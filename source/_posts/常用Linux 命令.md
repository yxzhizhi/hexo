---
title: Linux和Docker常用命令
date: 2020-04-05
categories:
  - linux
tags:
  - linux
---

## Linux常用命令
<!--more-->
```
ls 　 -a(同时列出隐含文件),　　-l（输出一个比较完整的格式，除每个文件名外，增加显示文件
类型、权限、硬链接数、所有者名、组名、大小（byte）、及时间信息-----简化为 ll）

mkdir 　 新建目录　例：mkdir test 命令会在当前目录下建立一个名为“test”的新目录
touch 　　创建文件 例：touch test/readme.txt 在 test 目录下创建 readme.txt 文件
cd 　　切换目录 cd /. 到根目录 cd .. 上一级目录 cd /hahaha/hahaha 到指定目录
pwd 　　显示当前目录
mv 　　移动/重命名（加上 -i 参数询问是否覆盖） 　 mv hello rock/ 移动到 rock 目录下
mv hello rock 重命名为 rock
cp 　　拷贝 （加上 -i 参数询问是否覆盖，-r 参数递归调用）
cp -ir test/ workspace＂（递归复制 test 目录到 workpace 目录下并在覆盖时提示）
rm 　　删除 （加上 -i 参数确认提示，-r 参数递归调用）
rm -ir test/ 递归删除 test 目录及其子目录并询问
wget url 　　下载文件到当前目录
sudo 暂时获取超级用户权限（有默认时长）加上 -i 参数 没有时间限制,输入 exit 或 logout 退出
su 账户名 　　切换到某某用户模式，没有时间限制
```
### ZIP 工具：
```
压缩文件　　 zip target.zip filename
压缩文件夹　　 zip -r target.zip dir 　　 -r 参数表示递归压缩子目录
解压　　 unzip target.zip
```
### tar 工具：
```
-c: 建立压缩档案
-x：解压
-t：查看内容
-r：向压缩归档文件末尾追加文件
-u：更新原压缩包中的文件
这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需在压缩或解压档案时可选的。
-z：有 gzip 属性的
-j：有 bz2 属性的
-Z：有 compress 属性的
-v：显示所有过程
-O：将文件解开到标准输出
下面的参数-f 是必须的
-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名
例：tar -xvf file.tar //解压 tar 包
tar -xzvf jdk-8u131-linux-x64.tar.gz -C /usr/local/java //解压 jdk 到指定文件夹
tar -cZf jpg.tar.Z \*.jpg //将目录里所有 jpg 文件打包成 jpg.tar 后，并且将其用
compress 压缩，生成一个 umcompress 压缩过的包，命名为 jpg.tar.Z
```
### vim 编辑器：　　 vim test.cpp
```
vim 有两种模式，一种是**普通模式**，另一种是**插入模式**。执行上述命令以后进入普通模式。
按下字母键“i”进入插入模式，使用方向键移动光标到需要插入的位置，然后输入想要插入
的内容。编辑完成后按键“Esc”退出回到普通模式,在普通模式下输入冒号“:”，然后输入
w 回车，保存更改。接着输入“:q”退出。也可以直接输入“:wq”保存并退出（注意 w 一定要
在 q 之前，先保存再退出）。
**查找**：在普通模式下输入“/查找内容”，回车，即可定位到第一个匹配项。接着按下字母
键“n”可以查找下一个。
**撤销**：普通模式下输入“:u”并回车，实现撤销。
```
