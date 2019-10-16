### Linux常用命令

- ls显示目录内容， list directory contents

  - -a，显示隐藏文件
  - -l，使用长格式显示文件内容
  - -d，显示目录属性
  - -R，显示目录及下级子目录结构
  - -F，区分文件类型。 /表示目录，@符号链接，*表示可执行文件

- wc，文件内容统计，统计文件中的行数，字数，字节数

  - -l，统计行数line
  - -w，统计字数(以字符串为单位)
  - -c，统计字节数bytes

- ps，进程管理，查看正在运行的进程

  PID：进程编号，TTY：进程所运行的位置，TIME：进程占用CPU的处理时间，CMD：进程所运行的命令

  - -aux，列出所有正在运行的进程
  - pstree，显示进程的树形结构
  - -u username，查看指定用户进程

- tar，压缩，对文件和目录进行打包

  格式： tar [选项]  生成文件名 目标文件名/目录

  - -c，产生.tar打包文件
  - -v，显示详细信息
  - -f，指定压缩后的文件名
  - -x，解包
  - -z，利用gzip进行压缩/解压缩
  - eg：tar -zcvf  dir.tar.gz dir 

- chmod，改变文件或目录存取权限，change file access permissions

  有两种使用方法：使用字符串设置权限

  ![1571212273616](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571212273616.png)

  - eg：chmod ugo+r file
  - eg：chmod a-w file
  - eg：chmod uo+rwx file

  使用八进制数设置权限

  - eg：chmod 754 file
  - eg：chmod 777 file

- who，列出正在使用系统的用户信息

  - who am i

- find，查找文件和目录

  - -name，根据文件名查找
  - -size，根据文件大小
  - -user，根据文件所有者
  - -type，根据文件类型
  - eg：find / -name hosts
  - eg：find . -name "h*"
  - eg：find /home -size 100b

- rm，删除文件

  - -i，交互式的删除
  - -f，强制删除文件，没有提示
  - -r，递归的删除指定目录及其子目录和文件
  - eg：rm -rf dir1

- mv，移动文件（目录）或给文件改名

  - eg：mv a.txt ../
  - eg：mv a.txt b.txt
  - eg：mv dir2 ../

- mkdir，创建目录

  - -m 数字，对新建目录设置存取权限
  - -p，一次建立多级目录
  - eg：mkdir dir1
  - eg：mkdir /root/dir2
  - eg：mkdir -p bin/dir3



### vim文本编辑

#### 命令方式（默认）

#### 编辑方式（提示insert）

#### ex转义方式（以：开头）

**文本输入**

- i，在光标前输入
- I，在光标所在行首输入
- a，在光标后输入
- A，在光标所在的行尾输入
- o，在光标下插入新的空行
- O，在光标上插入新的空行

**退出VI**

- ：wq保存退出
- ：x保存退出
- ZZ保存退出
- ：w保存但不退出
- ：q退出但不保存
- ：q！强制退出，不保存

**命令模式的操作:**

- 移动光标hlkj
- 0，移动到行首
- $，移动到行尾
- gg，移动到第一行
- G，移动到最后一行
- yy，复制光标所在行
- x删除光标位置的一个字符
- dd，删除光标所在行
- u，撤销上一次操作
- U撤销光标所在行的所有操作

**ex模式的操作**

- w命令，可把编辑缓冲区中的全部或部分内容写到指定文件中
  - ：w 文件名
  - ：w！文件名，强制覆盖已有文件
  - ：w>>文件名，将当前缓冲区中的内容附加到指定文件末尾
- r命令，吧指定文本读入编辑缓冲区
  - ：r，将当前文件读入光标的位置
  - ：r 文件名，将指定的文件放入光标的位置
- e命令，在编辑当前文件时编辑其他文件
  - ：e 文件名
  - ：e！文件名 忽略当前文件所做的修改，编辑指定文件
- 定位命令
  - ：n光标移动到第n行
  - ：+n，下移n行
  - ：-n，上移n行
  - ：0，光标定位到第一行





