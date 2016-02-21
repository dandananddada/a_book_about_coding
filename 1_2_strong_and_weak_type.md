## 1.2 强类型和弱类型

这里的强与弱是对数据类型检查的严格程度的描述，强类型语言中对类型转换的要求相对要更严格些。所以在参数的类型匹配上要求也会更加严格。

    以下例子会用到Java、JavaScript、C++和Ruby，它们分别是强类型、弱类型、弱类型、强类型。

用不同类型变量间的运算说明类型转换是最为方便的。

在强类型语言中，不同类型的变量是无法直接运算的，而弱类型语言在处理这种运算时不会抛出异常，来看下面几个例子。
    
在Java中`int`类型不能和`String`类型直接运算，在加法运算中`int`类型会被隐式转换为`String`，然后进行`String + String`的字符串合并运算。

而在减法运算中，首先字符串不存在减法运算，所以`int`类型无法隐式转换为`String`类型进行`String - String`运算，其次`String`类型无法隐式转换为`int`类型，所以`int`与`String`的减法运算在**编译时**会报错。

```java
int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + string);
//=> 1this is a string

System.out.println(number + stringNumber);
//=> 11

System.out.println(number - string);
//=> error: bad operand types for binary operator '-'
```
接下来我们看下Java中显示转换下会如何。

```java
int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + Integer.parseInt(stringNumber));
//=> 2

System.out.println(number -  Integer.parseInt(stringNumber));
//=> 0

System.out.println(number - Integer.parseInt(string));
//=> Exception in thread "main" Java.lang.NumberFormatException: For input string: "this is a string"
```

可以看出当我们显示的把一个内容为数字的`String`转换为`int`时，会按照两个`int`类型的规则进行加减运算。

而当我们显示的把一个内容为非数字的`String`转换为`int`时会抛出一个**运行时**异常：字符串无法格式化为数字。

另外如果你在Ruby中对`String`和`Number`进行加法运算的话，也会报运行时错误。

```ruby
1 + "1"
#=> String can't be coerced into Fixnum

1 + "this is a string"
#=> String can't be coerced into Fixnum
```

接下来我们看下弱类型的C++和JavaScript是如何处理两种不同数据类型的运算的。

```c++
#include <iostream>

using namespace std;

int main()
{
    int number = 1;
    char character1 = '1';
    char characterA = 'A';
    
    cout << number + character1 << endl;    //=> 50
    cout << number + characterA << endl;    //=> 66
    cout << number - character1 << endl;    //=> -48
   return 0;
}
```
可以看到无论`char`是字母还是数字在C++中都会转换为表示这个字符的ASCII码，然后与`int`进行运算。

接下来看下JavaScript中是如何处理的。

```javascript
var number = 1
,   string = "this is a string"
,   stringNumber = "1";

console.log(number + string);       //=> 1this is a string

console.log(number + stringNumber); //=> 11 

console.log(number - string);       //=> NaN

console.log(number - stringNumber); //=> 0

```
可以看出JavaScript的运算结果是在Java的基础上有所变化，`number` + `string` 仍旧是做字符串合并运算，而`number` - `string`则变成了数字减法运算，而对无法转换为数字的字符串进行数学运算也不会返回错误，取而代之的是返回`NaN`（Not a Number，是JavaScript内置的一个特殊值）。

值得一提的是，在大多数语言中除零运算都会报错，比如Java和Ruby中会报运行错误，而C++中会报编译错误，但是在JavaScript中除零会返回另外一个特殊值`Infinity`（表示无限大的值）。

当然通过上述的说明想判断一门语言是否为强类型你可以用`int - string`试一试，不过千万别把这个作为判断准则，我只是说这个规则有时候还是挺靠谱，但只是有些时候。

究竟一个语言是强类型还是弱类型还是取决于语言设计是所做出的偏好，弱类型语言可以让你的代码更简单明了，逻辑更�加清晰易懂。但为此付出的代价就是你可能会因为一时的疏忽而导致一些难以发现的trap。

```javascript
"1" - 0;
//将字符串转换为数字

//将数字转换为字符串
1 + "0"
```
如我之前所说的，这样的隐式转换会隐藏着一些小的陷阱。
```javascript
"1" == 1;           //=> true
```
所以在一些if判断时你不得不用这样的操作符。
```javascript
"1" === 1;           //=> false
```

总之强/弱类型影响着你的是是用简短的方式编码，还是扎扎实实的处理变量类型避免一些不必要的trap。不过，既然你选择了自己要使用的语言，或者你被迫要使用这样的语言，那么你只能遵从语言设计时所采取的折中，你在这里只是能知道这样做有什么好处又什么坏处，而不是能不能不这么做。

通常我在用JavaScript处理字符串与数字转换时我完全可以避免语言繁琐的API，而只用`+`或`-`操作符。

好，强/弱类型就到此为止吧，接下来说一说动/静类型，猜一猜动/静类型是在哪方面进行折中的考核，而这种折中又会影响到语言在哪方面的表现呢。
