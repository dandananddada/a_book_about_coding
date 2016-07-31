We don't have to think about types belonging to a big hierarchy of types. Instead, we think about what the types can act like and then connect them with the appropriate typeclasses.

It kind of applies the function to the element inside the box.

回调后面的元素

multBy4 \(x:xs\) = times4 x : multBy4 xs

code is data too

momod combine two things to a new things such as f·g is new function

\(x.y\).z = x.\(y.z\),

functor
fmap::\(a-&gt;b\)-&gt;f a-&gt;f b,  f is a datatype such as Maybe

functor like if else.

1. make datatype a functor
2. implements the fmap of dataype

\/\/perfect number

\(function\(\){
  var factors = \[\]
  ,   number
  ,   sum = 0
  ,   result;

number = 496;

getFactors\(\);  
  aliquoSum\(\);
  result = isPerfect\(\);

if\(result\)   console.log\(number + " is perfcet"\);
  else                  console.log\(number + " is not perfect"\);

function getFactors\(\){
    for\(i = 1;  i &lt; number; i = i+1\){
       if\(isFactor\(i\)\)  factors.push\(i\); 
    }
  }

function isFactor\(pontential\){
    return number % pontential === 0;
  }

function aliquoSum\(\){
    for\(i = 0; i &lt; factors.length; i = i+1\){
      sum = sum + factors\[i\];
    }
  }

function isPerfect\(\){
    return sum === number;
  }

function isAbundant\(\){
    return aliquSum &gt; number;
  }

function isDeficient\(\){
    return aliquSum &lt; number;
  }
}\)\(\)

console.log\(
  _.isEqual\(300,\(
    _.reduce\(
      _.filter\(
        _.range\(1, 300\)
        , function\(n\){ return 300%n == 0 }\)
      , function\(p,n\){ return p+n }
      , 0\)
    \)
  \)
\);

var a = _.chain\(_.range\(1, 300\)\)
  .filter\(function\(n\){ return 300%n == 0 }\)
  .reduce\(function\(p,n\){ return p+n }, 0\)
  .value\(\);
console.log\(\_.isEqual\(a, 300\)\)

编程范式用来区分不同程序语言的编程风格

In these languages the computer is told what the problem is, not how to solve the problem




as




