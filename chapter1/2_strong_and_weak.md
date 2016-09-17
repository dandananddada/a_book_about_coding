## 1.2 强类型和弱类型

这里的强与弱是对类型检查严格程度的描述，强类型语言中对类型转换的要求相对与弱类型语言来说要更加严格。



我想通过对比几种语言的类型转换来说明强弱类型的区别，而不同数据类型之间的运算最能体现不同语言对类型转换的不同处理方式。

	下面的例子中会用到Java、JavaScript、C++和Ruby，它们分别是强类型、弱类型、弱类型、强类型。


### 类型转换

在强类型语言中，不同类型的变量是无法直接运算的，而弱类型语言在处理这种运算时不会抛出异常，来看下面几个例子。
    
在Java中int类型不能和String类型直接运算，在加法运算中int类型会被隐式转换为String类型，然后执行String + String的字符串合并运算。

而在减法运算中，因为字符串不存在减法运算，所以int类型不会隐式转换为String类型进行String - String运算。而Java中String类型无法隐式转换为int类型，所以int与String的减法运算在编译时会报错。

```java
//Java 隐式转换下整型与字符型加减运算

int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + string);
//1this is a string

System.out.println(number + stringNumber);
//11

System.out.println(number - string);
//error: bad operand types for binary operator '-'
```

接下来我们看下Java显示转换下会如何处理上述运算。

```java
//Java 显示转换下整型与字符型加减运算

int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + Integer.parseInt(stringNumber));
//2

System.out.println(number -  Integer.parseInt(stringNumber));
//0

System.out.println(number - Integer.parseInt(string));
//Exception in thread "main" Java.lang.NumberFormatException: For input string: "this is a string"
```

可以看出当我们显示的把一个内容为数字的String转换为int时，会按照两个int类型的运算规则进行加减运算。

而当我们显示的把一个内容为非数字的String转换为int时会抛出一个运行时异常：字符串无法格式化为数字。


同样在Ruby中对String和Number进行加法运算也会报运行时错误。

```ruby
#Ruby 整型与字符型加减运算

1 + "1"
#String can't be coerced into Fixnum

1 + "this is a string"
#String can't be coerced into Fixnum
```

接下来我们看下弱类型的C++和JavaScript是如何处理两种不同数据类型的运算的。

```c++
//C++ 整型与字符型加减运算

#include <iostream>
using namespace std;

int main()
{
	int number = 1;
	char character1 = '1';
	char characterA = 'A';

	cout << number + character1 << endl;    
	//50

	cout << number + characterA << endl;    
	//66

	cout << number - character1 << endl;    
	//-48
	
	return 0;
}
```
可以看到无论char是字母还是数字在C++中都会转换为表示这个字符的ASCII码（数字表示），然后与int进行运算。

接下来看下JavaScript中是如何处理的。

```javascript
//JavaScript 整型与字符型加减运算

var number = 1
,   string = "this is a string"
,   stringNumber = "1";

console.log(number + string);       
//1this is a string

console.log(number + stringNumber); 
//11 

console.log(number - string);       
//NaN

console.log(number - stringNumber); 
//0

```
可以看出JavaScript的运算结果是在Java的基础上进行了变化，number + string 仍旧是做字符串合并运算，而 number - string 则变成了数字减法运算，而对无法转换为数字的字符串进行数学运算也不会返回错误，取而代之的是返回NaN（Not a Number，是JavaScript内置的一个特殊值）。

值得一提的是，在大多数语言中除零运算都会报错，比如Java和Ruby中会报运行错误，而C++中会报编译错误，但是在JavaScript中除零会返回另外一个特殊值 Infinity（表示无限大的值）。

也许通过上面的几个例子，你会总结出：可以用 int - string 来判断一个语言是强类型还是弱类型，确实这个看上去很取巧，但是事实总在意料之外，来看下面这段Clojure代码。
    
```clojure
;Clojure 整型与字符型加减运算

;Clojure是前置运算符，(+ 1 "1")即1 + "1"
;后续章节会单独介绍，这里暂时不用理会。
(+ 1 "1")   ;=>"11"
(- 1 "1")   ;=>0
```
Clojure是强类型语言，但是可以看出其 number + string 返回的是string，而 number - string 返回的是int，所以判断一门语言是强类型还是弱类型，最好还是看看官方定位。


究竟一个语言是强类型还是弱类型还是取决于语言设计者所做出的偏好，弱类型语言可以让你的代码更简单明了，逻辑更加清晰易懂。但为此付出的代价就是你可能会因为一时的疏忽而导致一些难以发现的问题（trap）。

通常我在用JavaScript处理字符串与数字转换时我完全可以避免语言繁琐的API，而只用 + 或 - 操作符。

```javascript
//JavaScript 弱类型语言代码更加简洁

"1" - 0;
//将字符串转换为数字

1 + "0"
//将数字转换为字符串
```

如我之前所说的，这样的隐式转换会隐藏着一些小的陷阱。

```javascript
"1" == 1;           //=> true
```
所以在一些if判断时你不得不用这样的操作符。
```javascript
"1" === 1;           //=> false
```

总之强弱类型决定着你是用简短的方式编码，还是扎扎实实的处理变量类型避免一些不必要的问题。不过，既然你选择了要使用的语言，或者你被迫要使用这样的语言，那么你只能遵从语言设计时所采取的折中，你在只能知道这样做有什么好处又什么坏处，而不是能决定不这么做。

好，强/弱类型就到此为止吧，接下来说一说动/静类型，猜一猜动/静类型将会在哪一方面做出折中，而这种折中又会影响到语言在哪方面的表现呢。
