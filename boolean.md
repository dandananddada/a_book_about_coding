# 布尔类型

布尔类型用于逻辑真/假判断，值为`true`和`false`。
```Java
//java
if(true)        System.out.println("true");
```
一般弱类型语言中也会可以用数字类型代替同样的逻辑判断。
```javascript
//javascript
if(1)           console.log("true");
```
其原理是弱类型语言会将数字类型隐式转换为布尔值。
我准备用JavaScript为例来说明boolean类型和其他类型的转化。
```javascript
1   == true;            //=> true,  [1]
0   == false;           //=> true,  [1]
1   === true;           //=>false,  [2]
''  == true;            //=>false,  [3]
''  == false;           //=> true,  [3]
's' == true;            //=>fasle,  [4]
's' == false;           //=>false,  [4]
!'' == true;            //=>true,   [5]
!'s' == true;           //=>false,  [5]
!!'s' == true;          //=>true,   [5]
Boolean('s') == true;   //=>true,   [6]
Boolean('') == true;    //=>true,   [6]
```


Java 字符串转 boolean

Java boolean 0 1

Ruby Scala 0 1

JavasCript 0 1

