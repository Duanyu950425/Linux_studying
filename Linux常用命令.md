# 目录

[Linux常用命令](https://github.com/Duanyu950425/Linux_studying/blob/master/Linux%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4.md#linux%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)

#  Linux常用命令

### 1. ls——查看文件信息

| 参数 |                     含义                     |
| :--: | :------------------------------------------: |
|  -a  | 显示指定目录下所有子目录与文件，包括隐藏文件 |
|  -l  |          以长格式显示文件的详细信息          |

![1557393966009](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557393966009.png)

文件类型："-"表示常规文件；

​					"d"表示目录；

​					“c”表 示字符设备文件

​					“b”表示设备文件；

​					 “s”表示管道文件；

​				      “l”表示链接文件。

文件存取权限：从左到右每3位为一组，依次代表文件拥有者、同组      用户和其他用户的存取权限。

​						   通常文件共有3个权限，“r”表示只读；“w”表示可写；“x”表示可执行；“-”表示未设置。

​                           文件的第一列如为-rw-r--r--，可知其为一个普通文件，文件所有者的权限是rw-，可读可写不可执行，文件所属组群的权限是r--，表示可读不可写不可执行，其他人的属性是r--，表示可读不可写不可执行。

​                            只有文件的拥有者或超级用户才能设置文件的属性。

### 2. more命令

​	如果使用ls命令来查看其内容，在信息过长无法在一屏上显示时，使用more命令，每次只显示一页；按“空格键”显示下一页，“Enter”显示下一行；按“q”退出显示；按“h”获取帮助。

例如：`ls -al | more`

### 3. cd——切换工作目录

（1）直接执行cd，切换回当前目录的宿主目录；

（2）cd ..  //返回上层目录

​          cd .   //返回当前目录

（3）进入根目录：相对路径：cd /

​                                                    cd ./etc

​                                 绝对路径：cd /etc

### 4. pwd——显示当前路径

### 5. mkdir——新建目录

创建一个新的**目录**，当权限不够不足以创建新目录时，我们可以采用sudo命令。

即`sudo mkdir aaa`。

### 6. cat命令

——查看**文件**内容：`cat 文件名`

——文件很多需要分屏显示：`cat 文件名 | more`

### 7. grep——指定文件中搜索指定字符内容

![1557395663929](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557395663929.png)

（1）在a.txt这和个文件中找所有拥有a的文件

a.txt的内容为：![1557481227545](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557481227545.png)

```
~/test$ grep a a.txt
```

![1557481187325](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557481187325.png)

（2）-n的用法——显示匹配行及行号

```
~/test$ grep -n a a.txt
```

![1557481348567](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557481348567.png)

（3）-v的用法——显示不包含匹配文本的所有行

```
~/test$ grep -v a a.txt
```

![1557481386633](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557481386633.png)

### 8.find——查找文件命令

![1557481395530](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557481395530.png)

例如：

```
~$ find / -name inittab    //从根目录下找文件名为inittab的文件
~$ find /etc -name inittab  //从etc目录路下找文件名为inittab的文件
```

### 9.rm——删除文件或目录

![1557481671493](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557481671493.png)

### 10.cp——复制文件

——cp命令功能是将给出的文件或目录复制到另一个文件或目录中。

——cp [参数] 源文件或目录 目标文件或目录

![1557481714507](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557481714507.png)

例如：我们将文件a.txt复制到目录day01下（已知a.txt/home/duanyu/test1下的文件，而test2与test1在同一个目录下）：

```
~$ cp /home/duanyu/test1/a.txt test2
```

### 11.mv——移动或重命名文件

![1557482033564](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557482033564.png)

例如：重命名test2目录下的a.txt文件为b.txt：

```
~test2$ mv a.txt b.txt
```

将目录test2下的文件b.txt移至/home/duanyu/test1目录下：

```
~test2$ mv /home/duanyu/test2/b.txt /home/duanyu/test1
```

### 12.clear——清除屏幕命令

### 13.ps——查看进程信息

### ![1557738336205](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557738336205.png)

```
~$ ps -u root  //看root用户的进程
~$ ps -xw      //显示终端进程并展示更多信息
```

### 14.top命令

——top命令是Linux下常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况。top命令能够在运行后，在指定时间间隔更新显示信息

——可以在使用top命令时加上-d <interval>来制定显示信息更新的时间间隔

top命令执行后，可以按下按键得到对显示的结果进行排序：

![1557738429713](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557738429713.png)

### 15.whoami——我是谁

### 16.who命令

——查看当前所有登录系统的用户信息：who [选项]

![1557738952998](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557738952998.png)

### 17.w命令

——该命令也可以查看登录当前系统的用户信息。与who命令相比，w命令的功能更强大，它不但可以显示当前有哪些用户登录到系统，还可以显示浙西而用户正在进行的操作，病给出更加详细和科学的统计数据。w [选项] [用户名]

![1557738997991](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557738997991.png)

### 18.tar——归档管理

——tar [参数] 打包文件名 文件

![1557739068165](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557739068165.png)

tar只打包不压缩，把多个文件打包成一个文件。

```
//我们将my.sql,my1.sql,my2.sql文件打包
~$ tar -cvf sql文件.tar my.sql my1.sql my2.sql
//释放sql文件.tar
~$ tar -xvf sql文件.tar my.sql my1.sql my2.sql
```

### 19.gzip——文件压缩命令

——tar和gzip命令结合使用时限文件打包，归档：gzip [选项] 被压缩文件

![img](file:///C:\Users\12201\AppData\Local\Temp\ksohtml17388\wps1.jpg)

压缩：

```
//将sql文件.tar压缩
~$ gzip sql文件.tar
//磁盘出现sql文件.tar.gz压缩文件
```

可以使用```ls -al```来查看压缩前后容量之差。

解压：

```
//解压sql文件.tar.gz文件
~$ gzip -d sql文件.tar.gz
```

### 20.man——联机帮助命令

——man [选项] 命令名

# Linux系统管理

### 1.df命令——查看磁盘使用情况

——用于检测**文件系统的磁盘空间占用和空域情况**，可以显示所有文件系统对接点和磁盘块的使用情况。命令格式：df [选项]

-h单元为根据大小适当显示

-m单位为M

### 2.du命令——查看指定目录的文件大小

```
//查看当前目录的总大小
duanyu@duanyu-VirtualBox:~$ du -sh
```

```
//查看当前目录下子目录分别的大小
duanyu@duanyu-VirtualBox:~$ du -h
```

```
//指定目录查看大小，例如指定看01.朱景瑶
duanyu@duanyu-VirtualBox:~$ du -h 01.朱景瑶
```

```
//查看指定文件大小，例如查看文件my2.sql的大小
duanyu@duanyu-VirtualBox:~$ du -h my2.sql
```

### 3.sudo命令——权限管理机制

​	忘记root用户密码，可以使用sudo passwd来重置密码。

### 4.mkfs命令

——格式化命令，用于创建指定的文件系统。使用格式：

​      mkfs [选项] 设备文件名 [blocks]

![img](file:///C:\Users\12201\AppData\Local\Temp\ksohtml17388\wps2.jpg)

### 5.rpm工具安装应用程序

——rpm  [选项]  [软件包名]

rpm常用参数：

![img](file:///C:\Users\12201\AppData\Local\Temp\ksohtml17388\wps3.jpg) 

安装软件：rpm -ivh 安装包的名称

### 6.添加用户账号——useradd或adduser

——useradd [参数] 新建用户账号

![img](file:///C:\Users\12201\AppData\Local\Temp\ksohtml17388\wps4.jpg) 

添加用户，指定用户的宿主目录为/home/test：

​		sudo useradd test -m（亲测准确）

​		useradd -d /home/test test

### 7.passwd——设置用户密码：passwd [参数] 用户名

### 8.userdel——删除用户：userdel [-r] [用户名]

——使用参数-r，则表示在删除用户的同时，将该用户的宿主目录一并删除

——使用参数-f，直接删除

### 9.Linux创建用户、设置密码、修改用户、删除用户命令

（1）useradd testuser  //创建用户testuser  或者useradd -d                /home testuser。新创建的用户在/home下创建一个用户目录testuser。

（2）passwd testuser   //给自己创建的用户testuser设置密码

（3）Usermod --help  //修改用户这个命令的相关参数

（4）userdel testuser   //删除用户testuser

​          rm -rf testuser    //输出用户testuser所在目录

​     或合并成一句：userdel -r testuser

#### Question：解决新建的用户只有$的办法：

①　cat /etc/passwd，查看新建的用户是不是在bash下，

②　如果不是通过vi /etc/passwd更改其成为bash下的。

### 10.su——命令切换用户

——exit返回上一个用户。

——如果需要进入别的普通用户账号，可在su命令后直接加上其他账号，然后输入密码。

——如果su命令后没有携带用户名，系统默认从当且用户切换到超级用户，并提示用户输入超级用户口令。

```
su 用户名  //代表的是只是切换用户

su - 用户名 //代表的是会将当前工作目录自动切换到切换后的用户的宿主目录下
```

### 11.路径表示

——Windows路径表示方法：d:\abc\a.txt

——Linux路径表示方法：/abc/a.txt  Linux没有d盘的概念，只有根目录

![img](file:///C:\Users\12201\AppData\Local\Temp\ksohtml17388\wps5.jpg) 

### 12.通配符

例如：

在etc目录下找所有l开头的文件：

```
duanyu@duanyu-VirtualBox:~$ cd etc
duanyu@duanyu-VirtualBox:/etc$ ls l*
```

在etc目录下找所有l结尾的文件：

```
duanyu@duanyu-VirtualBox:/etc$ ls *l
```

在etc目录下找?abc的文件。?是那**一个**不确定的字符：

```
duanyu@duanyu-VirtualBox:/etc$ ls ?abc
```

找开头是a、b、c、d、e、f的文件：

```
duanyu@duanyu-VirtualBox:/etc$ ls [a-f]*
```

查询文件名中有*的文件：

```
duanyu@duanyu-VirtualBox:/etc$ ls \*
```

### 13.linux文件系统

![1557741949893](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557741949893.png)

![1557741959839](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557741959839.png)

### 14.文件权限——读、写、可执行权限

读权限（r）：允许用户读取文件内容或者列目录。

写权限（w）：允许用户修改文件内容或者创建、删除文件

可执行权限（x）：允许用户执行文件或者运行使用cd命令进入目录。

注意：Linux系统仅允许文件的宿主或者超级用户改变文件的读写权限。

### 15.chmod——修改文件访问权限

——chmod [参数] 文件或目录名

#### （1）用户对象

u：属主，即文件或目录的所有者，拥有对文件最大的读写权限

g：属组，即与文件属组有相同组ID的所有用户

o：表示其他用户，通常只具有浏览权限

a：表示以上所有用户

修改权限：chmod g+r test 代表给test增加组内用户可读权限：

```
//例如文件abc.tar.gz的权限为-rw-r--r--，现在我们给其文件所有者增加可执行的权限
root@duanyu-Virtual:~# chmod u+x abc.tar.gz
```

#### （2）修改文件访问权限——chmod [参数] 文件或目录名

+：添加某个权限

-：取消某个权限

#### （3）用八进制表示权限：（0666）=（110110110）二进制。

```
root@duanyu-Virtual:~# chmod 0666 abc.tar.gz
//abc.tar.gz的权限变为-rw-rw-rw-
root@duanyu-Virtual:~# chmod 0777 abc.tar.gz
//abc.tar.gz的权限变为-rwxrwxrwx
```

# 实用工具——vi

​	   vi有输入和命令两种工作模式。输入模式用于输入；命令模式则是用来运行一些编排文件、存档以及离开vi等操作命令。当执行vi后，首先进入命令模式，此时输入的任何字符都被视为命令。

——进入文本输入模式

-a：在当前的光标前面添加文本

-i：在当前的光标后面添加文本

——通过Esc建切换文本输入模式和命令模式

（1） 使用vi新建一个文档：vi 文件名

（2） 退出vi命令：

​                      :w 代表保存文件

​                      :q 代表退出

​                      :q! 代表不保存退出

​                      :wq 代表保存退出

（3） vi撤销功能

——用户在命令模式下输入“:u”后按Enter键，就可以撤销上一次的操作。

——在vi中，撤销功能每一次撤销的是自上次存盘到现在输入的内容，因此撤销能够恢复到最原始的状态，但是此时用户不能使用“:q”命令来退出vi，因为此时用户已经修改了缓冲区的内容。如果确实需要退出vi程序，可以使用在命令模式下“:q!”。

（4） vi的删除功能

——使用Backspace来删除**光标前面**的内容；

——使用delete键来删除**当前的字符**；

![1557744350004](C:\Users\12201\AppData\Roaming\Typora\typora-user-images\1557744350004.png)

X 代表删除当前光标所在的字符

dd代表删除当前光标所在的行

r代表替换当前光标所在字符

:h 代表帮助