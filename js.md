# JavaScript

## JavaScript Objects

1. ### Create an object:

   var car = {type:"Fiat", model:"500", color:"white"};

   The **name:values** pairs in JavaScript objects are called **Property:Property Value**



2. ### Accessing Object Properties

   *objectName.propertyName*

   or

   *objectName["propertyName"]*

3. ### Object Methods

   ```js
   var person = {
     firstName: "John",
     lastName : "Doe",
     id       : 5566,
     fullName : function() {
       return this.firstName + " " + this.lastName;
     }
   };
   ```

4. ### Accessing Object Methods

   *objectName.methodName()*



## JavaScript Arrays

1. ### Creating an Array

   var cars = ["Saab", "Volvo", "BMW"];

   var person = [];
   person[0] = "John";
   person[1] = "Doe";
   person[2] = 46;

2. ### Using the JavaScript Keyword new

   var cars = new Array("Saab", "Volvo", "BMW");

3. ### Access the Elements of an Array

   var name = cars[0];

4. ### Changing an Array Element

   cars[0] = "Opel";

5. ### Arrays are Objects

   Arrays use **numbers** to access its "elements".

6. ### Array Properties and Methods

   ```js
   var x = cars.length;   // The length property returns the number of elements
   var y = cars.sort();   // The sort() method sorts arrays
   ```

7. ### Looping Array Elements

   ```js
   for (i = 0; i < cars.length; i++) {
     text += "<li>" + cars[i] + "</li>";
   }
   ```

   ```js
   You can also use the Array.forEach() function:
   
   cars.forEach(function (value) {
       text += "<li>" + value + "</li>";
   });
   ```

8. ### Adding Array Elements

   ```js
   The easiest way to add a new element to an array is using the push() method:
   
   var fruits = ["Banana", "Orange", "Apple", "Mango"];
   fruits.push("Lemon");    // adds a new element (Lemon) to fruits
   ```

9. ### The Difference Between Arrays and Objects

   In JavaScript, **arrays** use **numbered indexes**.  

   In JavaScript, **objects** use **named indexes**.

10. ### Avoid new Array()

    ```js
    Use [] instead.
    
    These two different statements both create a new empty array named points:
    
    var points = new Array();     // Bad
    var points = [];              // Good
    ```



## Event

- click 事件

  当点击元素时，会发生 click 事件



- blur 事件

  当元素失去焦点时发生 blur 事件



- focus 事件

  当元素获得焦点时，发生 focus 事件



- change 事件

  当元素的值发生改变时，会发生 change 事件。

  该事件仅适用于文本域（text field），以及 textarea 和 select 元素。



- submit 事件

  当提交表单时，会发生 submit 事件



- 完整的 key press 过程分为两个部分：1. 按键被按下；2. 按键被松开。

  当按钮被按下时，发生 keydown 事件。

  当按键(键盘)被松开时，发生 keyup 事件。它发生在当前获得焦点的元素上。



- 当鼠标指针从元素上移开时，发生 mouseout 事件。

  当鼠标指针位于元素上方时，会发生 mouseover 事件。

  大多数时候这两个事件一起使用。



## Functions

- parseFloat() 函数可解析一个字符串，并返回一个浮点数。



- indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。

对大小写敏感！如果要检索的字符串值没有出现，则该方法返回 -1。





# JavaScript RegExp 对象

## RegExp 对象

RegExp 对象表示正则表达式，它是对字符串执行模式匹配的强大工具。

### 直接量语法

```
/pattern/attributes
```

### 创建 RegExp 对象的语法：

```
new RegExp(pattern, attributes);
```

### 参数

参数 *pattern* 是一个字符串，指定了正则表达式的模式或其他正则表达式。

参数 *attributes* 是一个可选的字符串，包含属性 "g"、"i" 和 "m"，分别用于指定全局匹配、区分大小写的匹配和多行匹配。ECMAScript 标准化之前，不支持 m 属性。如果 *pattern* 是正则表达式，而不是字符串，则必须省略该参数。

### 返回值

一个新的 RegExp 对象，具有指定的模式和标志。如果参数 *pattern* 是正则表达式而不是字符串，那么 RegExp() 构造函数将用与指定的 RegExp 相同的模式和标志创建一个新的 RegExp 对象。

如果不用 new 运算符，而将 RegExp() 作为函数调用，那么它的行为与用 new 运算符调用时一样，只是当 *pattern* 是正则表达式时，它只返回 *pattern*，而不再创建一个新的 RegExp 对象。

### 抛出

SyntaxError - 如果 *pattern* 不是合法的正则表达式，或 *attributes* 含有 "g"、"i" 和 "m" 之外的字符，抛出该异常。

TypeError - 如果 *pattern* 是 RegExp 对象，但没有省略 *attributes* 参数，抛出该异常。

## 修饰符

| 修饰符                                                   | 描述                                                     |
| -------------------------------------------------------- | -------------------------------------------------------- |
| [i](http://www.w3school.com.cn/jsref/jsref_regexp_i.asp) | 执行对大小写不敏感的匹配。                               |
| [g](http://www.w3school.com.cn/jsref/jsref_regexp_g.asp) | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m                                                        | 执行多行匹配。                                           |

## 方括号

方括号用于查找某个范围内的字符：

| 表达式                                                       | 描述                               |
| ------------------------------------------------------------ | ---------------------------------- |
| [[abc\]](http://www.w3school.com.cn/jsref/jsref_regexp_charset.asp) | 查找方括号之间的任何字符。         |
| [[^abc\]](http://www.w3school.com.cn/jsref/jsref_regexp_charset_not.asp) | 查找任何不在方括号之间的字符。     |
| [0-9]                                                        | 查找任何从 0 至 9 的数字。         |
| [a-z]                                                        | 查找任何从小写 a 到小写 z 的字符。 |
| [A-Z]                                                        | 查找任何从大写 A 到大写 Z 的字符。 |
| [A-z]                                                        | 查找任何从大写 A 到小写 z 的字符。 |
| [adgk]                                                       | 查找给定集合内的任何字符。         |
| [^adgk]                                                      | 查找给定集合外的任何字符。         |
| (red\|blue\|green)                                           | 查找任何指定的选项。               |

## 元字符

元字符（Metacharacter）是拥有特殊含义的字符：

| 元字符                                                       | 描述                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| [.](http://www.w3school.com.cn/jsref/jsref_regexp_dot.asp)   | 查找单个字符，除了换行和行结束符。                      |
| [\w](http://www.w3school.com.cn/jsref/jsref_regexp_wordchar.asp) | 查找单词字符。                                          |
| [\W](http://www.w3school.com.cn/jsref/jsref_regexp_wordchar_non.asp) | 查找非单词字符。                                        |
| [\d](http://www.w3school.com.cn/jsref/jsref_regexp_digit.asp) | 查找数字。                                              |
| [\D](http://www.w3school.com.cn/jsref/jsref_regexp_digit_non.asp) | 查找非数字字符。                                        |
| [\s](http://www.w3school.com.cn/jsref/jsref_regexp_whitespace.asp) | 查找空白字符。                                          |
| [\S](http://www.w3school.com.cn/jsref/jsref_regexp_whitespace_non.asp) | 查找非空白字符。                                        |
| [\b](http://www.w3school.com.cn/jsref/jsref_regexp_begin.asp) | 匹配单词边界。                  单词中存在,并且从头匹配 |
| [\B](http://www.w3school.com.cn/jsref/jsref_regexp_begin_not.asp) | 匹配非单词边界。              单词中存在,并且从非头匹配 |
| \0                                                           | 查找 NUL 字符。                                         |
| [\n](http://www.w3school.com.cn/jsref/jsref_regexp_newline.asp) | 查找换行符。                                            |
| \f                                                           | 查找换页符。                                            |
| \r                                                           | 查找回车符。                                            |
| \t                                                           | 查找制表符。                                            |
| \v                                                           | 查找垂直制表符。                                        |
| [\xxx](http://www.w3school.com.cn/jsref/jsref_regexp_octal.asp) | 查找以八进制数 xxx 规定的字符。                         |
| [\xdd](http://www.w3school.com.cn/jsref/jsref_regexp_hex.asp) | 查找以十六进制数 dd 规定的字符。                        |
| [\uxxxx](http://www.w3school.com.cn/jsref/jsref_regexp_unicode_hex.asp) | 查找以十六进制数 xxxx 规定的 Unicode 字符。             |

## 量词

| 量词                                                         | 描述                                        |
| ------------------------------------------------------------ | ------------------------------------------- |
| [n+](http://www.w3school.com.cn/jsref/jsref_regexp_onemore.asp) | 匹配任何包含至少一个 n 的字符串。           |
| [n*](http://www.w3school.com.cn/jsref/jsref_regexp_zeromore.asp) | 匹配任何包含零个或多个 n 的字符串。         |
| [n?](http://www.w3school.com.cn/jsref/jsref_regexp_zeroone.asp) | 匹配任何包含零个或一个 n 的字符串。         |
| [n{X}](http://www.w3school.com.cn/jsref/jsref_regexp_nx.asp) | 匹配包含 X 个 n 的序列的字符串。            |
| [n{X,Y}](http://www.w3school.com.cn/jsref/jsref_regexp_nxy.asp) | 匹配包含 X 至 Y 个 n 的序列的字符串。       |
| [n{X,}](http://www.w3school.com.cn/jsref/jsref_regexp_nxcomma.asp) | 匹配包含至少 X 个 n 的序列的字符串。        |
| [n$](http://www.w3school.com.cn/jsref/jsref_regexp_ndollar.asp) | 匹配任何结尾为 n 的字符串。                 |
| [^n](http://www.w3school.com.cn/jsref/jsref_regexp_ncaret.asp) | 匹配任何开头为 n 的字符串。                 |
| [?=n](http://www.w3school.com.cn/jsref/jsref_regexp_nfollow.asp) | 匹配任何其后紧接指定字符串 n 的字符串。     |
| [?!n](http://www.w3school.com.cn/jsref/jsref_regexp_nfollow_not.asp) | 匹配任何其后没有紧接指定字符串 n 的字符串。 |

## RegExp 对象属性

| 属性                                                         | 描述                                     | FF   | IE   |
| ------------------------------------------------------------ | ---------------------------------------- | ---- | ---- |
| [global](http://www.w3school.com.cn/jsref/jsref_regexp_global.asp) | RegExp 对象是否具有标志 g。              | 1    | 4    |
| [ignoreCase](http://www.w3school.com.cn/jsref/jsref_regexp_ignorecase.asp) | RegExp 对象是否具有标志 i。              | 1    | 4    |
| [lastIndex](http://www.w3school.com.cn/jsref/jsref_lastindex_regexp.asp) | 一个整数，标示开始下一次匹配的字符位置。 | 1    | 4    |
| [multiline](http://www.w3school.com.cn/jsref/jsref_multiline_regexp.asp) | RegExp 对象是否具有标志 m。              | 1    | 4    |
| [source](http://www.w3school.com.cn/jsref/jsref_source_regexp.asp) | 正则表达式的源文本。                     | 1    | 4    |

## RegExp 对象方法

| 方法                                                         | 描述                                               | FF   | IE   |
| ------------------------------------------------------------ | -------------------------------------------------- | ---- | ---- |
| [compile](http://www.w3school.com.cn/jsref/jsref_regexp_compile.asp) | 编译正则表达式。                                   | 1    | 4    |
| [exec](http://www.w3school.com.cn/jsref/jsref_exec_regexp.asp) | 检索字符串中指定的值。返回找到的值，并确定其位置。 | 1    | 4    |
| [test](http://www.w3school.com.cn/jsref/jsref_test_regexp.asp) | 检索字符串中指定的值。返回 true 或 false。         | 1    | 4    |

## 支持正则表达式的 String 对象的方法

| 方法                                                         | 描述                             | FF   | IE   |
| ------------------------------------------------------------ | -------------------------------- | ---- | ---- |
| [search](http://www.w3school.com.cn/jsref/jsref_search.asp)  | 检索与正则表达式相匹配的值。     | 1    | 4    |
| [match](http://www.w3school.com.cn/jsref/jsref_match.asp)    | 找到一个或多个正则表达式的匹配。 | 1    | 4    |
| [replace](http://www.w3school.com.cn/jsref/jsref_replace.asp) | 替换与正则表达式匹配的子串。     | 1    | 4    |
| [split](http://www.w3school.com.cn/jsref/jsref_split.asp)    | 把字符串分割为字符串数组。       | 1    | 4    |



test() 方法用于检测一个字符串是否匹配某个模式.

### 语法

```
RegExpObject.test(string)
```

| 参数   | 描述                   |
| ------ | ---------------------- |
| string | 必需。要检测的字符串。 |

### 返回值

如果字符串 string 中含有与 RegExpObject 匹配的文本，则返回 true，否则返回 false。





^(\d)$就是0-9的任意一个数字，^表示以...开头，\d表示0-9的数字，$表示以...结尾，所以这个就是表示单个数字了