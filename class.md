# 类

在说明类之前我想先聊一聊结构体。

####结构体
结构体在C语言中用来打包存储多种数据类型的数据类型，它可以在一块内存区存储多种变量，并通过一个名字来访问这些变量。
```c
/*  定义一个结构体  */
struct account {
   int account_number;
   char *first_name;
   char *last_name;
   float balance;
};
//引用结构体
struct account s;
```
