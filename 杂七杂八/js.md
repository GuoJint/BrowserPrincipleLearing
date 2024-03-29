# 强弱类型
  - 强类型语言对类型的要求很严格，很强势，当遇到参与运算的类型不同或不符合规则时，往往会编译失败
    Python:
    `'x' + 3`
    执行后会报错 `TypeError: can only concatenate str (not "int") to str`，因为参与字符串连接运算的第二个操作数不符合规则

  - 而弱类型语言对类型的要求不严格，很松散，在编译阶段时遇到数据类型有问题时，往往会进行隐式转换（type coercion），这使得开发者在编写代码时对数据类型的处理可以更随意  
    弱类型语言对于变量类型的检查比较宽松，容忍隐式类型转换这种事情的发生
    js:
    `console.log("x" + 3);`
    执行后控制台会打印 x3，因为编译器将 3 隐式转换成了 '3'
# 静态动态类型
  - 静态类型语言变量的类型必须先声明，即在创建的那一刻就已经确定好变量的类型
  - 动态类型则没有这样的限制，你将什么类型的数据赋值给变量，这个变量就是什么类型

# js弱类型体现
  - 宽松相等（Loose Equality）  *NaN 与自己不相等*`
  JavaScript 中的 == 与大部分语言中的 == 不同，其 === 才是大部分语言中的 ==。JavaScript 中的 == 被称为宽松相等，它会尝试将两边的操作数转化为同一类型后再进行严格比较。 这里出现了隐式转换，所以我认为 == 是 JavaScript 弱类型的体现。

  - 算术运算
  算术运算中的 + 和 - 在一些情况下是 JavaScript 的弱类型的体现：  
  ```
    "1" + 2; // '12'
    1 + "2"; // '12'
    "a" + 1; // 'a1'

    "1" - 2; // -1
    1 - "2"; // -1
    "a" - 1; // NaN
  ```
  + 会将数字转化为字符串并进行字符串连接，- 会将字符串转化为数字并进行算术运算。
