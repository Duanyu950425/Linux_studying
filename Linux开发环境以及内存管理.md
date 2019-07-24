# Linux开发环境以及内存管理

## 1. 堆和栈

### （1）内存段

​		一个进程被操作系统加载到内存后并不是加载到一段连续的内存空间，而是至少分为三段或多段内存空间。

![1557837262774](D:\03 GitHub\Linux_studying\图片\linux环境开发1)

### （2）栈（stack）

​		是一种先进后出的结构。就像往一个管子里放东西，最后放进去的先拿出来，最先放进去的最后拿出来。栈空间由系统自动维护，不需要通过代码申请或者释放。

![1557837336560](D:\03 GitHub\Linux_studying\图片\linux环境开发2)

### （3）堆（heap）

​		是一种动态的结构。对在存取上没有栈那样严格的先进后出的顺序。堆就像一个大容器，随时拿进来，随时拿出去。堆空间需要malloc或者new申请或者释放。

```c++
int abc()
{
    int *p = new int;//对中申请了一块int型的内存
    delete p;//释放堆中申请的内存
    return a+b;
}
```

### （4）代码空间（Code）

​		程序中可执行代码部分。代码空间操作系统加载进程后就不能改变。如果尝试改变，进程会出现段错误异常而被操作系统终止。

# Linux文件和输入输出目录操作

## 一、Linux文件操作——系统调用

### 1. 文件描述符：就是一个程序打开文件的编号，是个整数

——文件描述符是个很小的正整数，它是一个索引值，指向内核为每个进程所维	护的该进程打开文件的记录表。 

——例如：每个进程启动时都打开3个文件：

​					标准输入文件（stdin）

​					标准输出(stdout)

​					标准出错(stderr)

——这三个文件分别对应文件描述符0、1、2

——实际上，你可能已经用过基于描述的I/O操作，但却没有意识到。

——编程中应该使用<unistd.h>中定义的STDIN_FILENO、 STDOUT_FILENO、 STDERR_FILENO代替数字0、1、2。

 

#### （1） open函数返回的整数就是一个文件描述符。

   文件描述符一般都是从3开始的一个整数，一个程序当中同时打开多个文件时，从3开始累加1。

int open(const char *pathname, int flags);

- open试图打开参数pathname中的一个文件。

- 参数flags指定访问该文件的方式。

- 第一个参数是要打开文件的文件名。

- 必须把flags设置为O_RDONLY、O_WRONLY、O_RDWR、O_CREAT、O_APPEND分别表示只读、只写、读写、如果文件不存在就创建、追加。

- Open成功后会返回一个文件描述符。

- Open失败后会返回-1，并设置errno变量。

 

打开文件就要关掉：

```c++
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<fcntl.h>

int main(int arg, char *args[])
{
    int fd1 = open("a.txt", O_RDONLY);
    int fd2 = open("b.txt", O_RDONLY);
    printf("%d\n", fd1, fd2);
    
    cloe(fd1);
    close(fd2);
    
    return EXIT_FAILURE;
}
```

#### （2）errno的使用方法

——包含头文件：在Linux输入 man errno

errno只是告知错误数，其返回的是一个整数。

——使用strerrno，包含头文件 man strerrno

strerrno可以告诉错误信息。使用：strerrno(errno)  //返回错误描述

```c++
int main(int arg, char *args[])
{
    int fd1 = open("a.txt", O_RDONLY);
    if(fd1 = -1)
    {
    	//失败就打印错误原因
        printf("%s\n", strerror(errno));
    }
    else
    {
        //成功才关闭文件
        close(fd1);
    }
    
    return EXIT_FAILURE;
}
```

#### （3）read读取文件内容

读写文件描述符：

ssize_t read(int fd, void *buf, size_t count);

ssize_t write(int fd, void *bug, size_t count);

```C++
int main(int arg, char *args[])
{
    int fd1 = open("a.txt", O_RDONLY);
    char buf[1024];
    memset(buf, 0, sizeof(buf));//对buf初始化
    
    if(fd1 = -1)
    {
    	//失败就打印错误原因
        printf("%s\n", strerror(errno));
    }
    else
    {
        read(fd1, buf, sizeof(buf));//读取a.txt文件内容
        printf("%s\n", buf);
        //成功才关闭文件
        close(fd1);
    }
    
    return EXIT_FAILURE;
}
```

#### （4）优化程序，读取大文件内容

```c++
int main(int arg, char *args[])
{
    if(arg < 2)
    {
        printf("错误，没有参数，程序退出\n");
        return -1;
    }
    int fd1 = open("a.txt", O_RDONLY);
    char buf[1024];
    
    if(fd1 = -1)
    {
    	//失败就打印错误原因
        printf("%s\n", strerror(errno));
    }
    else
    {
        while(1)//增加循环，可以任意读取很大的文件的内容
        {
             memset(buf, 0, sizeof(buf));//对buf初始化
            //放在循环中，每次循环清空上次读的垃圾
            if( read(fd1, buf, sizeof(buf)) <= 0)//读取a.txt文件内容
                break;
            //fd1是要打开的文件，buf就是读取的内容，sizeof(buf)是读取的大小
            printf("%s\n", buf);
        }
        //成功才关闭文件
        close(fd1);
    }
    
    return EXIT_FAILURE;
}
```

#### （5）0号文件描述符==标准输入

```C++
//文件描述符0的用法
int main(int arg, char *args[])
{
    char buf[1024];
    memset(buf, 0, sizeof(buf));
    read(fd1, buf, sizeof(buf);//读编号为0的文件描述符
    
    printf("%s\n", buf);
    return EXIT_FAILURE;
}
```

在Linux下执行后就会让你输入，回车后得到你所输入的内容。

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发3)

（6） 标准输入描述符的用法——0

不建议写0，建议写成STDIN_FILENO

| 小知识：                                 |
| ---------------------------------------- |
| man：什么也没有，代表查询的是Linux命令。 |
| man 2：代表查询的是系统调用              |
| man 3：代表C函数库调用                   |
| man 5：代表第三方库函数                  |

### （7） 改写文件内容write

| 改写文件内容write                                            |
| ------------------------------------------------------------ |
| int fd1=open(args[1],OWRONLY); //代表往已经存在的文件中写数据，只是把写进去的数据添加在开头，不删除原有数据，如果不存在就open失败 |
| int fd1=open(args[1],O_WRONLYO_CREAT): // 代表新建一个文件并写数据 |

```c++
//通过命令行参数写文件
int main(int arg, char *args[])
{
    if(arg < 2)
    {
        printf("错误，没有参数，程序退出\n");
        return -1;
    }
    //只写方式打开命令行参数指定的文件，O_CREAT若没有此文件则建立
    //如果只写O_WRONLY,代表往已经存在的文件中写数据，如果不存在就open失败
    int fd1 = open(args[1], O_WRONLY | O_CREAT);
    char buf[1024];
    
    if(fd1 = -1)
    {
    	//失败就打印错误原因
        printf("%s\n", strerror(errno));
    }
    else
    {
        memset(buf, 0, sizeof(buf));
        strcpy(buf, "hello world");
        //只把字符串“hello world”写进去，而不是把整个buf（1024位）写进去
        write(fd1, buf, strlen(buf));
    }
    
    return EXIT_FAILURE;
}
```

#### （8） 标准输出描述符的用法——1（STDOUT_FILENO）

```c++
//文件描述符1（STDOUT_FILENO）的用法：标准输出的用法
void printf1(const char *s)
{
    write(STDOUT_FILENO, s, strlen(s));//写STDOUT_FILENO
}
int main(int arg, char *args[])
{
    printf1("hello world\n");
    return EXIT_FAILURE;
}
```

## 二、系统调用和C库调用的区别

### 1. 系统调用：

——缺点

​		（1）不兼容

​		（2）没有文件缓冲操作。对硬盘来讲寿命不是他工作多长时间，是硬盘磁头寻磁道的次数。如果系统调用就没有文件缓冲，调用一次就读写一次磁盘，效率低下，同时严重影响磁盘寿命。

——优点

​			直接调用操作系统底层的功能可以通过底层来做更加细腻的控制。例如标准输入，标准输出。

### 2. c库函数：

​			普通文件操作都是调用的c库函数，它是带缓冲的调用。带缓冲的函数并不是立刻直接把数据写入磁盘，而是等到

内存的缓冲区满了，才一次性的往磁盘写入。

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发4)

注意：在Linux下输入*作为四则运算乘法时，要注意增加转移符\，才能进行运算，否则，在Linux中，*代表的是通配符。

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发5)

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发6)

### 3. stat函数的用法

- 使用fstat获取文件信息。

```c++
int fstat(int fd, struct stat *buf)//fstat第一个命令是文件描述符，而stat是文件名
```

参数fd必须是用open调用返回的有效文件描述符。

- 使用stat获取文件信息。

```C++
int stat(const char *path, struct stat *buf);
```

```c++
//stat函数的用法
int main(int arg, char *args[])
{
    if(arg < 2)
    {
        return 0;
    }
    struct stat st;//定义一个struct stat结构的变量
    if(stat(args[1], &st) == -1)
    {
        printf("error is %s\n", strerror(errno));
        return 0;
    }
    printf("文件大小是%lu\n", st.st_size);
    return 0;
}
```

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发7)

#### (1)用stat实现对文件的copy：但以下代码一次性拷贝，但是不能拷贝大文件

```c++
//用stat实现对文件的copy
int main33(int arg, char *args[])
{ 
	if (arg < 3)
		return 0;

	const char *srcfilename = args[1];//得到源文件名
	const char *descfilename = args[2];//得到目标文件名

	FILE *psrc = fopen(srcfilename, "r");//用只读方式打开源文件
	if (psrc == NULL)
	{
		printf("%s open failed, %s\n", srcfilename, strerror(errno));
		return 0;
	}

	FILE *pdesc = fopen(descfilename, "w");//用只写方式打开目标文件
	if (pdesc == NULL)
	{
		printf("%s open failed, %s\n", descfilename, strerror(errno));
		return 0;
	}

	//copy files
	struct stat st;
	memset(&st, 0, sizeof(st));
	if (stat(srcfilename, &st) == -1)//如果源文件stat失败
	{
		printf("%s stat failed, %s\n", srcfilename, strerror(errno));
		return 0;
	}

	//st.st_size读到的文件有多大
	char *buf = malloc(st.st_size);//根据源文件的实际大小动态分配一块内存空间

	if (buf == NULL)
	{
		printf("%out of mem\n");
		return 0;
	}

	size_t rc = fread(buf, 1, st.st_size, psrc);//将源文件一次性地读入到buf中

	fwrite(buf, 1, rc, pdesc);//将源文件的内容一次性写入目标文件

	fclose(psrc);
	fclose(pdesc);

	return EXIT_FAILURE;
}
```

调用：

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发8)

#### （2）可以拷贝大文件——通过循环

```c++
//通过固定大小的buf，循环读取文件实现拷贝，及时大文件也可以
int main(int arg, char *args[])
{
	if (arg < 3)
		return 0;

	const char *srcfilename = args[1];//得到源文件名
	const char *descfilename = args[2];//得到目标文件名

	FILE *psrc = fopen(srcfilename, "r");//用只读方式打开源文件
	if (psrc == NULL)
	{
		printf("%s open failed, %s\n", srcfilename, strerror(errno));
		return 0;
	}

	FILE *pdesc = fopen(descfilename, "w");//用只写方式打开目标文件
	if (pdesc == NULL)
	{
		printf("%s open failed, %s\n", descfilename, strerror(errno));
		return 0;
	}

	char buf[1024];
	while (1)
	{
		size_t rc = fread(buf, 1, sizeof(buf), psrc);//读取源文件，一次读1024字节buf当中
		if (rc <= 0)
		{
			break;
		}
		fwrite(buf, 1, rc, pdesc);//读多大写多大，读到源文件的最后，循环退出
	}

	fclose(psrc);
	fclose(pdesc);

	return EXIT_FAILURE;
}
```

#### （3）fread函数使用注意事项：

1）对于fread函数，第一个参数是读取文件的buf，第二个参数是一次读取最小结构的字节大小，第三个参数是结构的数量，第四个参数是源文件。

2）如果fread读取到的内容小于第二个参数的字节数大小，那么fread返回0。

3）对于fread函数，返回的不是字节数，返回的是第二个参数的结构的数量。

#### （4）如何让fread能返回读取的字节数？

​		fread第二个参数写成1，代表结构大小就是一个字节，第三个参数写buf的大小：

```c++
size_t rc = fread(nuf, 1, sizeof(buf), p);
```

### 4. getpass——读写用户输入，屏幕不回显

```c++
char *getpass( const char *prompt); 
```

- 参数prompt为屏幕提示字符。

- 函数返回值为用户键盘输入的字符串。

```C++
int main(int arg, char *args[])
{
    //使用getpass使输入密码不回显
    char name[];
    memset(name, 0, sizeof(name));
	printf("请输入用户名：");
	scanf("%s",name);
	char *passwd = getpass("请输入密码：");

	if((strncmp(name, "root",4) == 0)&&		(strncmp(passwd,"123456",6)) ==0)
	{
   		printf("登录成功\n");
	}
	else
	{
    	printf("登录失败\n");
	}
	return 0;
}
```

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发9)

## 三、文件和目录操作

读取目录的方式：

第一步、打开目录，调用函数（opendir）

第二步、读目录，调用函数（readdir）

第三步、关闭目录，调用函数（closedir）

```
DIR *opendir(const char *pathname);  //打开——opendir
struct dirent *readdir(DIR *dir);  //读取——readdir
int closedir(DIR *dir);  //关闭——closedir
```

### 1. 读取当前目录所有文件

```c++
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<errno.h>
#include <sys/types.h>
#include <dirent.h>

//读目录的例子
int main01(void)
{
	DIR *dir = opendir("./");//打开当前目录
	if (dir == NULL)
	{
		printf("打开目录失败，%s\n", strerror(errno));
		return -1;
	}
	
	while (1)
	{
		struct dirent *dent = readdir(dir);
		if (dent == NULL)//代表读到了这个目录的最后了，没有可读的文件了
		{
			break;
		}
		else
		{
			printf("文件名称：%s\n", dent->d_name);//打印目录下的文件名
		}
	}
	
	closedir(dir);
	return EXIT_SUCCESS;
}
```

![img](D:\03 GitHub\Linux_studying\图片\linux环境开发10)

### 2. 打开指定的目录例子

```c++
//读参数指定的目录例子
int main(int arg, char *args[])
{
	if (arg < 2)//代表没有参数
	{
		return 0;
	}
	DIR *dir = opendir(args[1]);//打开参数1指定的目录
	if (dir == NULL)
	{
		printf("打开目录失败，%s\n", strerror(errno));
		return -1;
	}

	while (1)
	{
		struct dirent *dent = readdir(dir);
		if (dent == NULL)//代表读到了这个目录的最后了，没有可读的文件了
		{
			break;
		}
		else
		{
			printf("文件名称：%s\n", dent->d_name);//打印目录下的文件名
		}
	}

	closedir(dir);
	return EXIT_SUCCESS;
}
```

