# 目录

# [gcc](https://github.com/Duanyu950425/Linux_studying/blob/master/gcc-gdb-make.md#gcc)

## [gcc常用选项](https://github.com/Duanyu950425/Linux_studying/blob/master/gcc-gdb-make.md#gcc%E5%B8%B8%E7%94%A8%E9%80%89%E9%A1%B9)

# [make](https://github.com/Duanyu950425/Linux_studying/blob/master/gcc-gdb-make.md#make)

# [gdb——调试](https://github.com/Duanyu950425/Linux_studying/blob/master/gcc-gdb-make.md#gdb%E8%B0%83%E8%AF%95)









# gcc

编译过程3个阶段：预处理、汇编、连接。

![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%871) 

在命令行键入：gcc -o hello hello.c    

​                          这行命令是告诉gcc对源程序hello.c进行编译和链接，并使用参数-o创建名为hello的可执行文件。

这一命令分三步进行：预编译源代码、编译目标代码、链接程序

（1）预编译：在命令行键入：gcc -o hello1.c -E hello.c

​            	         这个命令的含义是：gcc预编译hello.c文件，预编译完以后生成hello1.c的文件。

​                         -E代表只预编译不编译

预编译就是将头文件内容全部放到代码中去。

（2）编译：在命令行键入：gcc -o hello.o -c hello1.c

​                     这个命令的含义是：告诉gcc对源程序hello1.c进行编译但不链接，编译结果输出到hello.o文件中。

​                    -c代表只编译不连接

（3）链接：在命令行键入：gcc -o hello hello.o

​                     这个命令的含义是：告诉gcc对源程序hello.o进行链接,生成可执行程序hello。

​		我们一般不干涉预编译的过程，因为仅是将头文件添加进来，一般不会出错。而一般干预的是编译和链接的过程。

```
gcc -o hello.o -c hello.c  //将hello.c文件编译输出到hello.o文件

gcc -o hello hello.o   //将hello.o文件进行链接输出到hello文件中
```

## gcc常用选项：

| 参  数      | 含  义                                      |
| ----------- | ------------------------------------------- |
| -o filename | 输出文件名，如果没指定filename，默认为a.out |
| -c          | 只编译，不链接                              |
| -E          | 预编译                                      |
| -g          | 包含调试信息                                |
| -l          | 链接指定的库文件                            |
| -O          | 优化编译后的代码                            |
| -w          | 关闭所有告警信息                            |
| -Wall       | 开启所有告警信息                            |

gcc是如何文件类型？——拖文件扩展名判断文件类型

| 扩展名 | 含  义               |
| ------ | -------------------- |
| c      | C语言源文件          |
| cpp    | C++源文件            |
| s      | 汇编语言源文件       |
| o      | 编译后的目标代码文件 |
| a，so  | 编译后的库文件       |

​	若将.c改为.cpp，在链接时会出错（编译不出错）。gcc默认只链接c的标准库，并不链接c++标准库。

​	如何让gcc可以链接C++的库文件，只需要在链接时加入-lstdc++。在命令行键入命令：gcc –lstdc++ -o hello hello.cpp

​	nm命令主要是用来列出某些文件中的符号（说白了就是一些函数和全局变量等）

​	在命令行输入：nm hello，可以发现如果是C++语言，会改变函数名，即函数重载。

# make

​	make是一种控制编译或者重复编译软件的工具。

​	make可以自动管理软件的编译内容、方式和时机，从而使程序员把更多的精力集中在编写代码上。

- make是怎么完成工作的呢？

  makefile是一个文本形式的脚本文件，其中包含一些规则告诉make编译哪些文件，怎么样编译以及在什么条件下编译。

- makefile规则遵循以下通用形式

  target:dependency [dependency[…]]

  ​	command

  ​	command

  ​	[…]

- 每个command第一个字符必须是tab键，而不是空格键，不然make会报错并停止。

- 用vi编辑一个简单的makefile，

  ```
  duanyu@duanyu-VirtualBox:~$ vi makefile
  //进入编辑页面
  1 start:
  2	gcc -o hello hello.c
  //编辑完成返回命令行
  duanyu@duanyu-VirtualBox:~$ make
  gcc -o hello hello.c
  ```

  随后生成了hello文件，则以后再编译时，就不用手动输入gcc -o hello hello.c，直接键入make即可。

img 

- 稍微复杂的makefile

  ![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%872)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片2) 

![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%873)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片3)

 ![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%874)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片4) 

​	target start后面的hello.o代表其下的command依赖与hello.o这个target。所以make先执行了hello.o这个target下的command。

![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%875)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片5)当没有.o文件时，先执行编译，后执行链接；但是如果有了.o文件，则直接执行链接。

​	在makefile文件中如果有多个标号，只执行第一个标号（除非有依赖关系），若想要执行其他标号，只有指明其标号才行，则需要键入：make 标号

- 进一步完善的makefile

  ![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%876)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片6) 

![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%877)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片7) 

1）增加了target clean。

2）输入make clean,make会直接执行clean其下的command。

- 在makefile执行shell命令：

![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%879)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片9)

 ![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%8710)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片10) 

​	增加了@echo，显示编译成功语句，为了不将语句本身输出，所以前面加@符号。

- 为了简化编辑和维护makefile，可以在makefile中使用变量。

varname=some_text

- 把变量用括号括起来，前面加$就可以引用该变量的值。

$(varname)

- 按照惯例makefile的变量都是大写（只是习惯而已，不是必须的）。

- 在makefile使用变量：

（1）单个变量

![avatar](https://github.com/Duanyu950425/Linux_studying/blob/master/%E5%9B%BE%E7%89%87/gcc%E5%9B%BE%E7%89%8711)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片11) 

![img](D:\03 GitHub\Linux_studying\图片\gcc图片12) 

增加变量CC，每个引用变量CC的地方的展开成变量的值。

（2）多个变量

![img](D:\03 GitHub\Linux_studying\图片\gcc图片13) 

![img](D:\03 GitHub\Linux_studying\图片\gcc图片14) 

（3）更简略的写法

![img](D:\03 GitHub\Linux_studying\图片\gcc图片15) 

OBJS=$(SRCS:.c=.o),意思是将SRCS变量中的.c替换为.o。

（4）再次修改makefile

![img](D:\03 GitHub\Linux_studying\图片\gcc图片16) 

make定义了一些有用的预定义变量

| 变量名 | 含  义                   |
| ------ | ------------------------ |
| $@     | 规则的目标所对应的文件名 |
| $<     | 规则中的第一个相关文件名 |

$@：代表.c和.o关联的时候目标对应一个的文件名，这里即代表.o文件

$<：代表.c和.o关联的时候第一相关的文件，即.c文件

这样做的原因是：当.c文件很多时，无需一个一个添加执行语句到makefile文件中，只需要增加.c文件名即可。例如有相关的两个.c文件——add.c和hello.c，可以这样写makefile：

![img](D:\03 GitHub\Linux_studying\图片\gcc图片17) 

![img](D:\03 GitHub\Linux_studying\图片\gcc图片18)会自动执行

- 常见的make出错信息：

（1）No rule to make target ‘target’.Stop

—makefile中没有包含创建指定target所需要的规则，而且也没有默认规则可用。

（2）target’ is up to date 

—指定的target相关文件没有变化。

（3）command：Command not found

—make找不到命令，通常是因为命令被拼写错误或者不在$PATH路径下。

- 如果有很多个.c文件，现在修改其中一个.c文件，在重新编译时，只编译被修改的.c文件。make是通过时间来判断的，如果.c文件的时间晚于.o文件，则说明则.c文件被修改过。

# gdb——调试

1. 如果要用gdb调试，gcc编译的时候一定要加-g选项。

![img](D:\03 GitHub\Linux_studying\图片\gcc图片19) 

2. core文件是unix/linux通用的出错内存印象文件。但是默认情况下unix/linux时不生成core文件的。

   Linux默认是不生成corefile的，所以需要在.bashrc文件中添加

ulimit -c unlimited。(修改完.bashrc文件后记得. .bashrc让修改生效)

![img](D:\03 GitHub\Linux_studying\图片\gcc图片20) 

使用gdb生成core文件调试案例：

```c
//一个有错的程序
#include<stdio.h>
void test(void)
{
    int *i = NULL;
    *i = 2;
}
itn main(void)
{
    printf("hello world\n");
    test();
    return 0;
}
```

![img](D:\03 GitHub\Linux_studying\图片\gcc图片21) 

![img](D:\03 GitHub\Linux_studying\图片\gcc图片22) 

