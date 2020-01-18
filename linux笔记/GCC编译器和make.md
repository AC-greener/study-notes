### GCC编译器和make

#### GCC

​	一个工具集合，包含预处理器，编译器，汇编器，链接器等

文件名后缀

|   文件名后缀   |   文件类型   |
| :------------: | :----------: |
|       .c       |   c源文件    |
| .cpp .c++ .cxx |  c++源文件   |
|       .h       |    头文件    |
|       .i       |  预处理文件  |
|       .s       | 汇编程序文件 |
|       .o       |   目标文件   |
|       .a       |  静态链接库  |

##### gcc的编译过程

![1571229511202](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571229511202.png)

##### gcc编译选项

|       编译选项       |                  说明                  |
| :------------------: | :------------------------------------: |
|       -E file        |    只进行预处理，生成预处理文件(.i)    |
|       -S file        |    只进行编译，生成汇编代码文件(.s)    |
|       -c file        |      只吧源文件编译成目标代码(.o)      |
|       -o file        |      吧输出写入到指定的文件file中      |
| -o file1.out file1.c | 吧文件file1.c编译成可执行文件file1.out |
|        -I dir        |          指定头文件的路径dir           |
|   **优化程序选项**   |                **说明**                |
|         -O1          |                一级优化                |
|         -O2          |                二级优化                |
|         -O3          |             最高级别的优化             |

说明：当不用任何选项时，gcc将生成一个a.out的可执行文件

gcc多文件的编译：

```
gcc -o meng meng1.c meng2.c
```



#### make

makefile：定义了多个文件之间的依赖关系，编译的规则和步骤

make的工作机制：

​	根据makefile中的内容，完成编译工作

​	可以识别makefile中被修改的文件，并在再次编译时只编译这些文件，提高编译效率

makefile的规则：

1. 定义要创建的目标文件
2. 指出目标文件的依赖文件
3. 写出根据依赖文件创建目标文件的编译命令

eg：有三个源文件program.c，pro1.c，pro2.c，program.c文件包含有自定义的头文件lib.h，要求生成可执行文件program

makefile文件如下：

```shell
program: program.o pro1.o pro2.o
	gcc -o program.o pro1.o pro2.o
program.o: lib.h program.c
	gcc -c program.c 
pro1.o: pro1.c
	gcc -c pro1.c
pro2.o: pro2.c
	gcc -c pro2.c
		
```

makefile的默认文件名：

1. GNUmakefile
2. makefile
3. Makefile

如果要使用其他文件作为makefile，需要在make时使用-f选项

eg：

```shell
make	默认生成第一个目标文件
make prog1.o	生成指定目标文件prog1.o
make -f makefile1 读取指定的makefile文件
make clean
```

使用变量

eg：

```shell
object=program.o pro1.o pro2.o
program: $(object)
	gcc -o program $(object)
```

