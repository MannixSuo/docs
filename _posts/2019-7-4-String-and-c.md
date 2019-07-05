---
title: 字符串和格式化输入输出
tags: C语言
layout: posts
---

"a",'a'不一样字符串后面会带有"\0"代表空字符(null),"a"在内存中实际上是`|a|\0|`而'a'是`|a|`

strlen();表示字符串中字符的个数
sizeof str;表示数组str的大小.
scanf();只会读取输入的第一个单词 空格以后的单词不会读取

`#define NAME value` 编译时替换. 吧程序中出现的任何NAME替换成value甚至是一行代码...

`const int MONTHS = 12;` const 关键字修饰的变量只能读不能改。

|printf转换说明|输出|
|--|--|
|%a|浮点数十六进制数和p计数法|
|%A|浮点数十六进制数和p计数法|
|%c|单个字符|
|%d|有符号十进制整数|
|%e|浮点数，e计数法|
|%E|浮点数，e计数法|
|%f|浮点数，十进制计数法|
|%g|根据值的不同自动选择%f或%e，%e格式用于指数小于-4或者大于等于精度时|
|%G|根据值的不同自动选择%f或%e，%E格式用于指数小于-4或者大于等于精度时|
|%i|有符号十进制整数和%d相同|
|%o|无符号八进制整数|
|%p|指针|
|%s|字符串|
|%u|无符号十进制整数|
|%x|无符号十六进制整数，使用十六进制整数0f|
|%X|无符号十六进制整数，使用十六进制整数0F|
|%%|打印一个百分号|


### 复习题

1. 再次运行程序清单4.1，但是在要求输入名字时，请输入名和姓（根据英文书写习惯，名称和姓之间有一个空格），看看会发生什么？为什么？

    ```c
    #include <stdio.h>
    #include <string.h>
    #define DENSITY 62.4
    int main(void)
    {
        float weight,volume;
        int size,letters;
        char name[40];

        printf("Hi! What's your first name?\n");
        scanf("%s",name);
        printf("%s, what's your weight in ponds?\n",name);
        scanf("%f",&weight);
        size = sizeof name;
        letters = strlen(name);
        volume = weight/DENSITY;
        printf("Well, %s ,your volume is %2.2f cubic feet.\n",
        name,volume);
        printf("Also, your first name has %d letters,\n",letters);
        printf("and we have %d bytes to store it.\n",size);
        return 0;
    }
    ```

    现在只能获取到名因为空格后面的字符scanf不会读取。

2. 假设下列示例都是完整程序中的一部分，他们的打印结果分别是什么？
    1. printf("He sold the painting for $%2.2f.\n",2.345e2); // He sold the painting for $234.50
    2. printf("%c%c%c\n",'H',105,'\41'); // Hi)
    3. #define Q "His Hamlet was funny without being vulgar."
        printf("%s\nhas  %d characters.\n",Q,strlen(Q));
        //His Hamlet was funny without being vulgar.
        //has 42 characters.
    4. printf("Is %2.2e the same as %2.2f?\n",1201.0,1201.0);
        //Is 1.20e+003 the same as 1201.00?

3. 在第二题的3中，要输出包含双引号的字符串Q，应该如何修改？
    #define Q "\"His Hamlet was funny without being vulgar.\""

4. 找出下列程序中的错误

    ```c
    #define B "booboo"
    #define X 10
    int main(void)
    {
        int age;
        char name[40];
        int xp;
        printf("Please enter your first name.");
        scanf("%s",name);
        printf("All right, %s , what's your age?\n",name);
        scanf("%d",age);
        xp = age + X;
        printf("That's a %s! You must be at least %d.\n",B,xp);
        return 0;
    }
    ```

5. 假设一个程序的开头是这样

    ```c
    #define BOOK "War and Peace"
    int main(void)
    {
        float cost = 12.99;
        float percent = 80.0;
        printf("This copy of \"%s\" sells for $%f \n",BOOK,cost);
        printf("That is %f%% of list.",percent)
    }
    ```

    请构造一个使用BOOK cost 和percent的printf（）语句打印如下内容
    This copy of "War and Peace" sells for $12.99
    That is 80% of list.

6. 打印下列各项内容要分别使用什么转换说明？
    1. 一个字段宽度与位数相同的十进制整数  //%d
    2. 一个形如8A，字段宽度为4的十六进制整数 //%4x
    3. 一个形如232.346，字段宽度为10的浮点数 //%10f
    4. 一个形如2.33e+002字段宽度为12的浮点数 //%12e
    5. 一个字段宽度为30，左对齐的字符串 //%-30s

7. 打印下面各项内容分别使用什么转换说明？
    1. 字段宽度为15的unsigned long类型整数 //%15ul
    2. 一个形如0x8a字段宽度为4的十六进制整数 //%#4x
    3. 一个形如2.33E+02 字段宽度为12 左对齐的浮点数 //%-12E
    4. 一个形如+232.346 字段宽度为10的浮点数//%+10f
    5. 一个字段宽度为8的字符串的前8个字符//%8s

8. 打印下面各项内容分别使用什么转换说明？
    1. 一个字段宽度为6最少有4位数字的十进制整数//%6d
    2. 一个再参数列表中给定字段宽度的八进制整数//%*o
    3. 一个字段宽度为2的字符//%2s
    4. 一个形如+3.13字段宽度等于数字中字符数的浮点数//%+f
    5. 一个字段宽度为7左对齐字符串的前5个字符//%5s

9. 分别写出读取下列各输入行的scanf语句,并声明语句中用到的变量和数组
    1. 101  //scanf("%d",&value)
    2. 22.32 8.34E-09//scanf("%f %E",&value,&value2)
    3. linguini//scanf("%s",value)
    4. catch 22//scanf("%s %d",value,&value2)
    5. catch 22(跳过catch)//scanf("catch %d",&value)

10. 什么是空白
    \0

11. 下面的语句有什么问题如何修正?
    printf("The double type is %z bytes..\n",sizeof(double));

    printf("The double type is %zd bytes..\n",sizeof(double));

12. 假设要在程序中用圆括号代替花括号以下方法是否可行？
    #define ( {
    #define ) }

    不可行，会把方法后的圆括号也替换掉
