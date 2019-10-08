---
title: 数据和C
layout: posts
tags: C语音
---

|最初K&R给出的关键字|C90标准添加的关键字|C99标准添加的关键字|
|------------------|------------|--------------------|
|int|signed|_Bool|
|long|void|_Complex|
|short| | |
|unsigned| | |
|char| | |
|float | | |
|double| | |

_Bool 类型表示布尔变量 true或false

_Complex和_Imaginary分别表示复数和虚数（？？）。

位，字节和字

位，字节和字是描述计算机数据单元或存储单元的术语。这里主要指存储单元。
最小的存储单元是位（bit）可以存储0或1。
字节（byte）是常用的计算机存储单位。对于几乎所有机器一字节均为8位。这是字节的标准定义，至少在衡量存储单位时是这样（但是C语言对此有不同的定义）
字（word）是设计计算机时给定的自然存储单位，对于8位的微型计算机一字长只有8位。现在个人计算机的字长增长到32位以及64位。计算机的字长越大，其数据转移越快，允许的内存访问也就越多。

|+|.314159|1|
|--|--|--|
|符号|小数|指数|

浮点数通常只是实际值得近似值，例如7.0可能被存储为6.99999

C语言的基本数据类型

一般而言存储一个int要用一个机器字长。早期的16位IBM PC兼容机使用16位来存储一个int，目前个人计算机是32位，因此使用32位来存储一个int

声明变量

int a；
int a,b,c;
int a=10;
int a=10,b;这样声明很不好

16进制数的每一位的数恰好由4位2进制数表示。例如3用4位2进制数表示为0011,5是0101。因此16进制数35的位组合是0011 0101。十六进制数53的位组合是0101 0011.

0X 或0x表示16进制数如0x10,0X10

0前缀表示8进制。

不同的进制要使用不同的转换说明：

|进制|转换说明|显示前缀(0,0x,0X)|
|--|--|--|--|
|十进制|%d|无|
|八进制|%o|%#o|
|十六进制|%x|%#x，或%#X|

C语言提供三个附属关键字修饰基本整数类型：short，long，和unsigned

* short int (或者简写为short)类型占用的存储空间可能比int类型少，常用于较小数值的场合以节省空间。与int类似，short是有符号类型。

* long int或long 占用的存储空间可能比int多，适用于较大数值的场合。long是有符号类型。

* long long int 或long long (C99标准加入)占用的存储空间可能比long多，该类型最少占用64位。有符号。

* unsigned int 或unsigned 只适用于非负值的场合，这种类型与有符号类型表示的范围不同。例如16位unsigned int 允许的取值范围是0~65535而不是-32768~32767.用于表示正负号的位现在用来表示另一个二进制位，所以无符号整形可以表示更大的数。

* 在C90标准中添加了unsigned long int或unsigned long，和unsigned int 或unsigned short类型。C99标准又添加了unsigned long long int 或unsigned long long。

* 在任何有符号类型前面添加关键字signed，可强调使用有符号类型的意图。

在C语言中可以把多个连续的字符串合成为一个。

### 复习题

1. 指出下面各种数据合适的数据类型
    1. East Simpleton的人口(int)
    2. DVD影碟的价格(unsigned float)
    3. 本章出现次数最多的字母(char)
    4. 本章出现次数最多的字母次数(unsigned int)

2. 在什么情况下要用long类型的变量代替int类型的变量？
    > 数字超出int的范围
    > 如果要处理更大的值那么使用一种在所有系统上都保证至少32位的类型可以提高程序的可移植性。

3. 使用那些可移植的数据类型可以获得32位有符号整数？选择的理由是什么？
    > 如果要正好获得32位的整数，可以使用int32_t,如果要获取最少可存储32位整数的最小类型，可以使用int_least_32_t类型，如果要为32位提供最快的计算速度，可以选用int_fast32_t类型。

4. 指出下列常量的类型和含义
    1. '\b'  char(存储为int类型) 转义字符 backspace
    2. 106  int 106
    3. 99.44    double 99.44
    4. 0XAA int 16进制
    5. 2.0e30   double 2*10^30

5. Dottie Cawm编写了一个程序找出程序中的错误。

    ```c
    #include <stdio.h>
    int main(void)
    {
        float g,h;
        float tax,rate;

        g = 1.0e21;
        tax = rate*g;
        return 0;
    }
    ```

6. 写出下列常量在声明中使用的数据类型在printf()中对应的转换说明。

    |常量|类型|转换说明|
    |--|--|--|
    |12|int|%d|
    |0X3|int|%#X|
    |'C'|char|%c|
    |2.34E07|double|%e|
    |'\040'|char|%c|
    |7.0|double|%f|
    |6L|long|%ld|
    |6.0f|float|%f|
    |0x5.b6p12|float|%a|(5+11/16+6/256)*2^12

7. 写出下列常量在声明中使用的数据类型和在printf()中对应的转换说明。(假设int为16位)

    |常量|类型|转换说明|
    |--|--|--|
    |012|int|%#o|
    |2.9e05L|long double|%le|
    |'s'|char|%c|
    |100000|long|%ld|
    |'\n'|char|%c|
    |20.0f|float|%f|
    |0x44|int|%x|
    |-40|int|%d|

8. 假设程序的开头有下列声明：

    ```c
    int imate = 2;
    long shot = 53456;
    char grade = 'A';
    float log = 2.71828;
    ```

    把下面的printf()语句中的转换字符补充完整

    ```c
    printf("The odds against the %d were %ld to 1.\n",imate,shot)
    printf("A score of %f is not an %c grade.\n",log,grade)
    ```

9. 假设ch是char类型的变量，分别使用转义序列，十进制值，八进制字符常量和十六进制字符常量把回车字符赋给ch(假设使用ASCII码值)

    转义序列：'\r',十进制值：13，八进制符号常量：'\015',十六进制符号常量：'0XD'

10. 修正下面的程序。

    ```C
    #include <stdio.h>
    int main(void) /* This program is perfect*/
    {
        int cows,legs;
        printf("How many cow legs did you count?\n");
        scanf("%d",&legs);
        cows = legs/4;
        printf("That implies there are %d cows.\n",cows);
        return 0;
    }
    ```

11. 支持下列转义序列的含义
    1. \n  换行
    2. \\  \
    3. \"  双引号
    4. \t  tab

### 编程练习

1. 通过实验即编写带有此类问题的程序验证整数上溢，浮点数上溢和浮点数下溢。

    ```C
    #include <stdio.h>
    int main(void)
    {
        int a = 4294967295;
        printf("%d\n",a); // -1
        float toobig = 3.4E38 * 100.0f;
        printf("%e\n",toobig);// inf
        float down = 0.123456;
        printf("%f,%f",down,down/100000);// 0.123456,0.00001
        return 0;
    }
    ```

2. 编写一个程序，要求提示输入一个ASCII码值，然后打印输入的字符。

    ```C
    #include <stdio.h>
    int main(void)
    {
        int ascii = 0;
        printf("Enter a ASCII number: ");
        scanf("%d",&ascii);
        printf("The character of this ASCII is %c .\n",ascii);
    }
    ```

3. 编写一个程序，发出一声警报，然后打印下面的文本。
    > Startled by the sudden sound, Sally shouted,
    > "By the Great Pumpkin, what was that!"

    ```C
    #include <stdio.h>
    int main(void)
    {
        printf("\a");
        printf("Startled by the sudden sound, Sally shouted,\n");
        printf("\"By the Great Pumpkin, what was that!\"");
    }
    ```

4. 编写一个程序，读取一个浮点数，先打印成小数点的形式，在打印成指数形式，然后如果系统支持在打印成p计数法，按以下格式输出。
    > Enter a floating-point value :64.25
    >
    >fixed-point notation:64.25000
    >
    >exponential notation:6.425000E+01
    >
    >p notation 0x1.01p+6

    ```C
    #include <stdio.h>
    int main(void)
    {
        float value = 0.0;
        printf("Enter a floating-point value :");
        scanf("%f",&value);
        printf("fixed-point notation: %f\n",value);
        printf("exponential notation: %e\n",value);
        printf("fixed-point notation: %a\n",value);
    }
    ```

5. 一年大约有3.156X10^7秒。编写一个程序提示用户输入年龄，然后显示该年龄对应的秒数。

    ```C
    #include <stdio.h>
    int main(void)
    {
        long secondsAnYear = 3.156E7;
        short age;
        printf("Your age:");
        scanf("%hd",&age);
        printf("Seconds for your age: %ld \n",age * secondsAnYear);
        return 0;
    }
    ```

