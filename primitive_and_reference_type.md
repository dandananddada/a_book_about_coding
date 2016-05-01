# 基础类型和引用类型

一般数据类型都会分为两种：基础类型和引用类型。不同语言中可能术语不同（C#中叫做值类型），但实际上是同一个概念。

**基础类型**
1. 基础类型存储的是直接值。
2. 基础类型的值存储在栈中。
3. 基础类型是不可修改的（不可变的）。
4. 基础类型进行比较时，直接值相等就相等。
5. 基础类型在作为参数传递时，传递的是值。

**引用类型**
1. 引用类型存储的是地址。
2. 引用类型的值存储在堆中。
3. 引用类型是可修改的（可变的）。
4. 引用类型进行比较时，值相等，且地址相等才相等。
5. 引用类型作为参数传递时，传递的是引用。

接下来对上面的五点分别举例说明。

**值的存储**

基础类型在栈中存储直接值，而引用类型在栈中存储引用的地址，真正的值存储在堆中。对于编译型语言而言栈是在编译时处理内存的分配的，在运行时处理堆的内存分配，所以栈的内存分配是静态的，堆的内存分配是动态的，栈中存储的数据是固定大小的，而堆中的数据是可变大小的。同时编译型语言在编译型处理了基础类型的内存分配，相比解释型语言在运行时的处理速度效率自然要高。
对于基础类型和引用类型的存储情况可以参考下图。
![](images/primitive_and_reference_type.png)
**是否可变**

基础类型是不可变的。