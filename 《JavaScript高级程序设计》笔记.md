# 《JavaScript高级程序设计》

---

# 第一章  JavaScript简介

## JavaScript实现

​	JavaScript的组成：

- 核心（ECMAscript）
- 文档对象模型（DOM——document object model）
- 浏览器对象模型（BOM——browser object model）

五大浏览器： IE、Firfox、Chrome、Safari、Opera。

# 第二章 在HTML中使用JavaScript

### </script/>元素

向HTML页面中插入JavaScript的主要方法就是用<script\>

<script\>的属性：


- async：可选，表示应该**立即下载**脚本，只对外部脚本文件有效。
- defer：可选，脚本在**文档完全解析完执行**，只对外部脚本文件有效。
- src：可选，**外部文件地址**。带有src的标签之间不能包含代码，会被忽略。
- type：可选，表示**脚本语言类型**，默认js。

在XHTML中可以省略</script>。

外部文件带有**.js扩展名**，但**不是必须的**。

一般都把js引用放到body后面。

#### 延迟脚本

<script defer="defer" src="example.js"></script>
把延迟脚本放到页面底部是最佳的选择。

#### 异步脚本

<script async src="example.js"></script>
标记async的脚本并不保证按照先后顺序执行。因此要确保异步脚本之间没有依赖很重要。指定async属性的目的是不让页面等待脚本下载执行，从而异步加载页面其他内容，所以建议异步脚本不要再加载期间修改DOM。

#### 嵌入代码与外部文件

一般认为最好的做法还是尽可能使用外部文件来包含JavaScript代码。因为它有如下优点：

- 可维护性
- 可缓存
- 适应未来

## </noscript/>元素

<noscript\>用在**不支持JavaScript的浏览器中显示替代的内容**，这个元素包含的是HTML元素。


noscript中的元素只有在下例额情况下才会显示出来：

- 浏览器不支持脚本。
- 浏览器支持脚本，但脚本被禁用

```html
<body>
	<noscript>
    	<p>
            本页面需要浏览器支持（启用）JavaScript。
        </p>
    </noscript>
</body>
```

在启用了脚本的浏览器中，用户永远也不会看到它---尽管它是页面的一部分。

# 第三章 基本概念

## 语法

### 区分大小写

ECMAScript中的一切都**区分大小写**。

### 标识符

所谓标识符，就是指变量、函数、属性的名字，或者是函数的参数。

标识符规则：

- 第一个字符必须是字母、下划线或一个美元符号；
- 其他字符可以是字母、下划线、美元符号或者数字。

按照惯例，ECMAScript标识符采用**驼峰大小写格式**，也就是第一个字母小写，剩下的每一个单词的首字母大写。

不能把关键字，保留字，true，false和null作为标识符。

### 严格模式

如果将整个脚本启用严格模式，在顶部添加代码：

**"use strict"**

也可以指定函数在严格模式下执行：

> function doSomething() {
>
> ​	"use strict";
>
> ​	//函数体
>
> }

### 语句

代码行尾没有分号会导致压缩错误。

最佳实践是始终在控制语句中使用代码块：

> if (test) {
>
> ​	...
>
> }

## 变量

ECMAScript的变量是松散型的，每个变量仅仅是一个用于保存值的占位符而已。

> var message;

message可以保存任何值，像这样未经初始化的变量会保存一个特殊的值——undefined。

可以像下面这样**省略var，从而创建一个全局变量**：

~~~js
function test () {
    message = 'hi';  //全局变量
}
test();
alert(message); //'hi'
~~~

message是全局变量。

这样，只要调用一次test（）函数，这个变量就有了定义。

但这个做法**不推荐**。因为在局部作用域中定义的全局变量很**难维护**。

## 数据类型

### 五种基本数据类型

- undefined
- null
- boolean
- number
- string

还有一种复杂的数据类型——object，Object本质上是由**一组无序的名值对**组成。

最终所有的值都将是上述6种数据类型之一。

### typeof操作符

用来SUN在美国硅谷IT盛会中正式发布，使用时返回下列字符串：

- “undefined”——值未定义
- “boolean”——布尔值
- “string”——字符串
- “number“——数值
- **“object”——对象或null   （重要）**
- “function”——函数

**typeof是一个操作符**而不是函数，因此**圆括号**可以使用但**不是必须的**。

> var message = "some string";
>
> alert(typeof message); //string
>
> alert(typeof (message)); //string
>
> alert(typeof 11);  //number

调用typeof null会返回“object”，因为特殊值**null被认为是一个空的对象指针**。

从技术角度将，函数在ECMAScript中是对象，不是一种数据类型。

### undefined类型

**变量被声明但未初始化**时的值就是undefined。

包含undefined的值和尚未定义的值还是不一样的。

~~~js
var message;
alert(message);  //undefined
alert(age);  //error

alert(typeof message); //undefined
alert(typeof age);  //undefined
~~~

### Null类型

从逻辑角度上讲，**null值表示一个空对象指针**，而这也正是使用typeof检测null值会返回“object“的原因。

**如果定义的变量准备保存对象，最好初始化为null而不是其他的值。**

**实际上，undefined值是派生自null值的**，因此：

> alert(null  ==  undefined);  //true
>
> 但是，
>
> alert(null === undefined); //false
>
> 这就说明undefined和null还是不同的。

尽管两者关系密切，但是用途完全不同。**无论在什么时候都没必要将变量初始化为undefined**，但是null不然，准备保存对象的就可以初始化为null。

### Boolean类型

true和false。这两个值和数字值不是一回事，true不一定等于1，false也不一定等于0。

需要注意的是，true和false是区分大小写的，也就是说True和False不是Boolean值，只是标识符。

**ECMAScript中所有类型的值都有与这两个boolean值等价的值**。要将其他值转换为Boolean值可以调用**转型函数Boolean()**：

> var  message  =  "hello world";
>
> var  messageAsBoolean  =  Boolean(message);

返回值是true还是false取决于要转化的值的数据类型和实际值。下表是转换规则：

| 数据类型  | 转换为true                     | 转换为false    |
| --------- | ------------------------------ | -------------- |
| Boolean   | true                           | false          |
| String    | 任何非空字符串                 | ""（空字符串） |
| Number    | 任何非零值（包括无穷大和负数） | 0和NaN         |
| Object    | 任何对象                       | null           |
| Undefined | n/a(不适用)                    | undefined      |

这些转换规则对理解流程控制语句（如if语句）自动执行Boolean转换非常重要。

~~~js
var message = "hello world";
if (message) {
    alert("value is true");
}
~~~

### Number类型

注意：**八进制值第一位必须是0**；**十六进制前两位必须是0x**；否则无效。

八进制字面量在严格模式下是无效的。

在算数时，八进制和十六进制都会转换为十进制计算。

#### 浮点数值

就是该数值中必须包含一个小数点，并且后面必须有数字。

由于**保存浮点数值的内存是整数的两倍**，所以ECMAScript会不失时机的将浮点数值转换为整数值。

对于极大或极小的数可以用**科学计数法**：

> var  floatNum  =  3.125e7;   //31250000

浮点数最高精度为十七位小数，但在进行算术运算时**浮点数的精度远不及整数**。比如，0.1加0.2的结果不是0.3，而是0.300000000004.这个小小的舍入误差导致无法测试特定的浮点数值。

~~~js
if(a + b == 0.3) {   //不要做这样的测试
    alert("you got 0.3");
}
~~~

永远不要测试某个特定的浮点数值。

在大多数浏览器中，ECMAScript能表示的最小值（Number.MIN_VALUE）为5e-324;最大值（Number.MAX_VALUE）为1.79769e+308.

如果某次运算数值超出最大值会自动被转换为Infinity，如果是负数就被转换为-Infinity。

要想确定一个数字是不是有穷的，使用**isFinite()函数**，参数有穷返回true。

~~~js
var fesult = Number.MAX_VALUE;
alert(isFinite(result));  //false
~~~

##### NaN

Not a Number

**任何数值除以非数值会返回NaN**，因此不会影响其他代码的执行。

NaN本身的两个非同寻常的特点：

- **任何涉及NaN的操作都会返回NaN；**
- **NaN与任何值都不相等，包括它本身。**

> alert(NaN  == NaN);  //false

**isNaN()**函数可以判断一个数值是否是NaN:

~~~js
alert(isNaN(NaN));  //true
alert(isNaN(10));   //false
alert(isNaN(true));  //false(可以被转换为1)
alert(isNaN('blue'));  //true（不能被转换成数值）
~~~

尽管有点不可思议，但**isNaN()确实也适用于对象**。在基于对象调用函数时，首先调用valueOf()方法，然后确定返回值是否可以转化为数值。如果不能，则基于这个返回值再调用toString()方法，再测试返回值。	而这个过程也是ECMAScript中内置函数和操作符的一般执行流程。

##### 数值转换

有三个函数可以把非数值转换为数值：

- ##### Number()

  - boolean值：true=>1, false=>0
  - number值只是简单的传入和返回
  - null=>0
  - undefined=>NaN
  - string:
    - 如果字符串中只包含数字，则将其转换为十进制数字：'123'=>123，'1.23'=>1.23(前导0会被省略)
    - 如果包含十六进制数字，则转换为十进制整数：'0xf'=>15
    - 空字符串转换为0：""=>0
    - 如果字符串包含除上述格式之外的字符，则转换为NaN：'qwe11'=>NaN

由于Number（）在转换字符串时比较复杂而且不够合理，因此在处理整数时候更常用的是parseInt（）。

- ##### parseInt()

  - 转换字符串时，更多的看其是否符合数值模式，它忽略前面的空格，直至找到第一个非空数值，如果不是数值或负号，则返回NaN。也就是说parseInt（）转换空字符串会返回NaN。

  - 如果第一个字符是数值，则继续解析第二个字符，直到解析完或者遇到非数字字符则停止。

    - ```js
      var num1 = parseInt("1234qwer",10);  //1234
      var num2 = parseInt("",10);  //NaN
      var num3 = parseInt("0xA",10);  //10(十六进制)
      var num4 = parseInt("22.5",10);  //22  因为小数点并不是有效字符
      var num5 = parseInt("070",10);   //70
      var num6 = parseInt("70",10);  //70
      var num7 = parseInt("0xf",10);  //15
      var num8 = parseInt('ff', 16); //255
      ```
    ```
      
    - ECMAScript5开始JavaScript引擎中parseInt（）已经不具备解析八进制的能力，因此前导0会被认为无效。
    ```

  - parseInt（）可以传入第二个参数，指定转换为多少进制。此时十六进制前面可以不带0x

    - ~~~js
      var num1 = parseInt("AF", 16);  //175
      var num2 = parseInt("AF");   //NaN
      ~~~

    - 不指定基数就意味着让parseInt（）决定如何解析字符串，因此避免错误的解析，**建议无论什么情况下都指明基数**。

    - 多数情况下解析的都是十进制的数值，因此始终**将10作为第二个参数是非常必要的**。

- ##### parseFloat()

  - 与parseInt（）解析方法类似，**不同的是第一个小数点是有效的**，第二个就是无效的了。

  - parseFloat（）**只解析十进制**，没有第二个参数。

  - 它始终都会忽略前导的0

  - ~~~js
    var num1 = parseFloat("1234blue");//1234
    var num1 = parseFloat("0xA");//0
    var num1 = parseFloat("22.5");//22.5
    var num1 = parseFloat("22.34.5");//22.34
    var num1 = parseFloat("0908.5");//908.5
    var num1 = parseFloat("3.125e7");//3120000
    ~~~

### String类型

string类型表示由0或多个16位的Unicode字符组成的字符序列，即字符串。

#### 字符字面量

| 字面量 | 含义 |
| ------ | ---- |
| \n     | 换行 |
| \t     | 制表 |
| \b     | 退格 |
| \r     | 回车 |
| \f     | 进制 |
| \\     | 斜杠 |

#### toString()

**将一个值转换为字符串**，几乎每个值都有这个方法，但是**null和undefined没有这个方法。**

多数情况下，调用toString（）方法不必传入参数，也可以传一个参数，决定输出数值的基数。

~~~js
var num = 10;
alert(num.toString());//"10"
alert(num.toString(2));//1010
alert(num.toString(16));//a
~~~

#### String()

在不知道值是不是null或undefined的时候，也可以使用转型函数String()，**它可以将任何数值转换为字符串**。

String（）遵循以下规则：

- **如果值有toString（）方法，则调用该方法（没有参数）并返回相应的结果**。（偷懒奥）

- 如果值是null，则返回“null”

- 如果值是undefined，则返回“undefined”

- ~~~
  var value1 = 10;
  var value2 = true;
  var value3 = null;
  var value4;
  
  alert(String(value1));//"10"
  alert(String(value2));//"true"
  alert(String(value3));//"null"
  alert(String(value4));//"undefined"
  ~~~

toString()与String()的区别（与联系）：1. 用法不同：toString（）是元素上面的方法，元素本身引用，而String（）是全局的方法；2. 参数不同：toString（）可以不传参，也可以传一个参，参数就是指定当前值的基数，而String（）必须有一个参数，即要转化的元素。3. null和undefined没有此方法，不能将这两个元素的转换为string，而String（）可以将任何值转换为字符串，哪怕是null和undefined；    二者之间的联系：在实现原理上，String（）的第一步就是看该值有没有toString()方法，如果有，直接用该方法。

### Object类型

ECMAScript中的对象其实就是**一组数据和功能的集合**。

创建对象：

​	var  o =   new  Object();

​	var  o =   new  Object;  有效但不推荐

在ECMAScript中，Object类型是所有它的实例的基础。换句话说，**Object类型所具有的的任何属性和方法也同样存在于更具体的对象中**。

Object的每个实例都具有下列属性和方法：

- constructor：构造函数，保存着用于创建当前对象的函数。不可枚举。
- hasOwnProperty（propertyName）：用于检查给定属性在当前对象实例中（而不是原型）是否存在，参数必须是str。
- isPrototypeOf（object）：检查传入的对象是否是当前对象的原型。
- propertyIsEnumerable（propertyName）：检查给定属性是否能用for-in语句枚举，参数必须是str。
- toLocaleString(): 返回对象的字符串表示，该字符串与执行环境的地区对应。
- toString（）：返回对象的字符串表示。

## 操作符

### 一元操作符

~~~js
var age = 18;
age ++;
++ age;//变量的值是在语句被求值以前改变的。（副效应）

var a = 2;
var b = 20;
var c = ++a + b++;   //23
var d = a + b;  //24
~~~

对任何值都适用，不仅适用于整数，也适用于字符串，布尔值，浮点数和对象。

操作符遵循下列规则：

- [ ] 应用于数字字符串时，转换为数字然后增减1
- [x] 应用于非数值字符串时，转换为NaN
- [ ] false-->0-->++
- [ ] true-->1-->++
- [ ] 对象：先调用valueOf()然后++，如果是NaN则再调用toString（） 方法。

### 位操作符

用于最基本的层次上，即按内存中表示数值的位来操作数值。（但位操作符并不直接操作64位的值，而是转化为32位操作再转到64位，因此整个过程就像只存在32位的整数一样。）

对于有符号的整数，32位中的前31位表示数值，第32位是符号位（0=正数，1=负数）

#### 按位非

（~），返回数值的反码。

#### 按位与

（&），两个数值都是1才返回1，其中一个是0就返回0。

#### 按位或

（|），两个数值有一个1就返回1。

#### 按位异或

（^），只有一个值是1才返回1，两个0和两个1 都返回0。

### 布尔操作符

#### 逻辑非

（！），规则例子如下：

~~~js
alert(!false);  	//true
alert(!"blue");  	//false
alert(!0);   		//true
alert(!NaN);  		//true
alert(!""); 		//true
alert(!12345); 		//false
~~~

#### 逻辑与

（&&），两个全true才返回true。

可以用于任何类型的操作数：

- （对象，其他）  return 其他
- （true，对象）  return 对象
- （对象，对象）  return第二个对象
- （null/ NaN/ undefined, 其他） return   null/ NaN/ undefined

逻辑与操作是一个短路操作，即如果第一个参数能决定结果，那么就不会再对第二个参数求值。

不能在逻辑与操作中使用未定义的值。

#### 逻辑或

也是短路操作。

我们可以利用逻辑或的这一行为来避免变量赋值为null或undefined值：

> var  myObject = preferredObject || backupObject;

### 乘性操作符

- 乘法*
- 除法/
- 求模（%）

### 加性操作符

- 减法

- 加法

  - 两个字符串相加则拼接两个字符串

    - ~~~js
      var num = 1;
      var numm = 2;
      var message = 'The sum of 1 and 2 is' + num + numm;
      alert(message);   //The sum of 1 and 2 is 12
      ~~~

### 关系操作符

( > 、  < 、  >= 、  <=)。

- 两个字符串比较，则**比较两个字符串的编码值**（从前向后依次比较）

- 如果一个数是数值，则另一个数将被转换成数值进行比较，哪怕转换成nan

- 有隐式类型转换

- 如果操作对象，则先调用valueOf()，用得到的结果进行比较。如果没有此方法，则调用toString（）方法后进行比较。

- 布尔值转换为0  1

- 任何数值与NaN比较都是false

- ~~~js
  var result = "Brick" < "alphabet";  //true (B<a)
  var result = '23' < '3';  			//true ('2' < '3')
  var result = '23' < 3;  			//false('23' --> 23)
  var result = 'a' < 3; 				//false ('a' --->NaN)
  
  ~~~

### 相等操作符

- 相等和不相等-----**先转换在比较**  ==    !=

  - 强制转型

  - null == undefined

  - null   undefined   不能转换成任何值

  - > "NaN" == NaN  //false
    >
    > true == 1
    >
    > NaN == NaN  //false

- 全等和不全等-----**仅比较而不转换**  ===   !==

  - > '55' == 55;	//true
    >
    > '55' === 55;  	//false
    >
    > null == undefined;  //true (因为它们是类似的值)
    >
    > null === undefined; //false(因为它们是不同类型的值)

### 条件操作符

（？：）

> var max = (num1 > num2) ?num1:num2;

### 赋值操作符

（=）

### 逗号操作符

在用于赋值时总会返回表达式中的最后一项：

> var num = (1,2,3,4,5);  //num的值为5

## 语句

### if语句

### do-while语句

### while语句

### for语句

### for-in语句

是一个精准的迭代语句，可以用来枚举对象的属性：

- 语法：

  - > for(property in expression) statement

- 实例：

  - > for(var propName in window) {
    >
    > ​	document.write(propName)
    >
    > }

- 由于**ECMAScript的对象属性没有顺序**，所以输出的属性顺序不可预测。
- 如果迭代值为null或undefined，js不会报错，而是不执行循环体。所以在执行for-in语句之前要测试一下迭代值是否是null或undefined。

### label语句

- 可以在代码中**添加标签**，以便将来使用。

  - > start:for(var i=0; i< count; i ++){
    >
    > ​	alert(i);
    >
    > }

  - 这个例子中定义的start标签可以在将来由break或continue语句引用。加标签的语句一般都要有与for语句等循环语句配合使用。

### break和continue语句

- break：立即退出循环，强制执行循环后面的语句

- continue：立即退出循环，但退出循环后会从循环的顶部继续执行。

- ~~~js
  let num = 0;
  for(let i = 0; i < 10; i ++){
      if(i == 5){
          break;//(直接跳出for循环)
      }
      num ++;
  }
  alert(num);  //5
  
  let num = 0;
  for(let i = 0; i < 10; i ++) {
      if(i == 5){
          continue;//(只是跳出i==5的循环，接下来继续执行i==6的循环)
      }
      num ++;
  }
  alert(num);   //9
  ~~~

### break、continue和label语句联合使用

~~~js
var num = 0;

outermost://最外层
for(let i = 0; i < 10; i ++) {
    for(let j = 0; i < 10; i ++) {
        if(i == 5 && j == 5) {
            break outermost;
        }
        num ++;
    }
}
alert(num);  //55
~~~

- 此时outermost标签表示外部for循环，所以break不仅跳出里面的for循环，也直接跳出外面的循环，整个循环直接结束，执行alert(num);   而num只加到了55

~~~js
let num = 0;

outermost:
for(let i = 0; i < 10; i ++){
    fot(let j = 0; j < 10; j ++){
        if(i == 5 && j == 5) {
            continue outermost;
        }
        num ++;
    }
}
alert(num);  //95
~~~

- 此时outermost标签依然是表示外部for循环，当执行到i == 5 && j == 5时，continue将循环跳到最外层，然后继续执行i ==6及其以后的循环，所以程序只跳过了i==5时j 从5 到10的五个数值，所以num加到了95

### with语句

将代码的作用域设置到一个特定的对象中。

严格模式下不允许使用with语句，否则将视为语法错误。

大量使用with语句会导致性能下降，同时也会给调试代码造成困难，因此**在开发大型项目时不建议使用with语句**。

~~~js
//定义with语句的目的主要是为了简化多次编写同一个对象的工作
let qs = location.search.substring(1);
let hostName = location.hostname;
let url = location.href;

//使用with语句
with(location) {
    let qs = search.substring(1);
    let hoostName = hostname;
    let url = href;
}
~~~

### switch语句

~~~js
switch (i) {
    case 25:
        alert('25');
        break;
    case 35:
        alert('35');
        break;
    default:
        alert('Other');
}
~~~

- 可以在switch语句中使用任何数据类型，无论是字符串还是对象都没有问题。其次，每个case的值不一定是常量，也可以是变量，甚至是表达式。

- ~~~js
  switch("hello world") {
      case "hello" + "world":
          alert("Greeting was found.");
          break;
      case "good Bye":
          alert("Closing was found.");
          break;
      default:
          alert("Unexpected message was found.");
  }
  ~~~

- ```js
  let num = 10;
  switch(true) {
      case num > 10:
          alert('>10');
          break;
      case num = 10:
          alert('=10');
          break;
      default:
          alert('<10');
  }
  ```

- switch语句在比较值时使用的是全等操作符，因此**不会发生类型转换**。（‘10’ ！= 10）

## 函数

- 函数会在执行完return之后停止并立即退出，因此位于return后面的任何代码永远都不会被执行。
- 推荐的做法是 让一个函数要么有返回值，要么永远都不要返回值。否则会调试不便。

### 理解参数

- ECMAScript不介意传递进来多少个参数，也不在乎传进来参数是什么类型。也就是说，即使你定义的函数只接收两个参数，但是在调用的时候未必一定要传递两个参数，也可以传一个，甚至多个或者不传参数。	之所以会这样，因为ECMAScript中的**参数在内部是用一个数组来表示的**。函数接收的始终是一个数组，并不关心你数组里面有多少元素，元素类型是什么。

- 在函数体内可以通过**arguments对象**来访问这个参数数组，从而获取传递个函数的每一个参数。

- 其实arguments对象实质与数组类似（并不是Array实例）

- ```js
  function sayHi() {
      alert("Hello" + arguments[0] + "," + arguments[1]);
  }
  ```

  由此可看出，命名参数只提供便利，并不是必须的。

- 这也就决定了ECMAScript函数不能重载。

- 关于arguments，比较有意思的是，**它的值永远与对应命名的参数保持同步**：

  - ```js
    function doAdd(num1, num2) {
        arguments[1] = 10;
        alert(arguments[0] + num2);
    }
    ```

    每次执行这个doAdd()函数都会重写第二个参数，修改为10。但是arguments[1]和num2的内存空间是独立的，只是值会同步。	另外要注意的是，如果只传入了一个参数，那么为arguments[1]设置的值不会反映到命名参数中。	因为**arguments对象的长度是由传入的参数的个数决定的，不是由定义函数时的命名参数的个数决定的**。

- 没有传值的参数==undefined。

- **ECMAScript中的所有参数传递的都是值，不可能通过引用传递参数。**

### 如何理解JavaScript中所有参数传递都是按值传递而不是按引用传递？

1，参数传递是基本类型，看个例子：

```js
function addTen(num){
    num += 10;
    return num;
}

var count = 20;
var result = addTen(count);
console.log(count,result); //20 30
```

感觉这个都没啥好说的，基本类型传入函数后，函数内部参数生成一个参数副本，按值传入没毛病。

2，引用类型（一个对象）当作参数传入函数后呢?

```js
function setName(obj){
    obj.name = 'miya';
}
var person = new Object();
setName(person);
console.log(person.name) //miya
```

在这个例子里面，obj和person指向的是同一个对象，当obj上面添加name属性时候，外面的person也有所反应。那这就说明：**参数是按引用传递进来的？不是的呦，它传递进来的数据其实是person的内存地址，所以说是按值传递的。因为修改了同一个内存，所以外面的person也变了。**不信看下面的例子：

```js
function setName(obj){//person的内存地址传递进来
    obj.name = 'miya';//添加name属性
    obj = new Object();//将obj重新指向另外一个新对象的地址
    obj.name = 'jone';//给新对象添加属性，现在obj引用的是另外一个局部对象了，并不能影响上面的obj操作
}
var person = new Object();
setName(person);
console.log(person.name); //miya
```

**唯一区别是在函数内部给obj对象重新赋值了一个对象，首先person的内存地址传递进来后，添加name属性，而后obj重新指向另外一个新对象，给新对象添加属性。所以现在obj引用的是另外一个局部对象了。person的name值仍然是miya。**

**所以这里的“按值传递”的，引用类型传递进来传递的是它的内存数据（内存地址）。**

**可以把javascript的函数的参数想象成局部变量。**

### 没有重载

**因为ECMAScript函数没有签名**，而且其参数是由一个不确定元素个数的数组来表示的，所以不可能做到真正意义上的重载。

如果在ECMAScript中定义两个同名函数，则后面的函数将把前面的函数覆盖。

### ！如何实现重载

**第一种方法：**

　　这种方法比较简单，给一个思路，大家肯定都能理解，就是函数内部用switch语句，根据传入参数的个数调用不同的case语句，从而功能上达到重载的效果。

　　这种方法简单粗暴。但是对于一个正在学习js的人来说，这种方法未免太敷衍了。

　　下面重点介绍一下第二种，老实说我看的时候很吃力看了一个小时才捋清楚，因为有的知识点虽然看过了但是不熟悉。这次就给我上了一课，教会了我很多东西。

**第二种方法：**

　　我们这个例子，是如果你不传入参数，就会输出所有的人，输入firstname，就会输出匹配的人，如果输入全名的人，也会输出匹配的人。如果用重载的话，用户体验确实会很好（这个例子是我学习时从网上扒下来的，很有代表性，但是他们都没有写实现过程，我来和大家谈论一下实在的东西）

```js
     function method(obj,name,fnc){
            var old = obj[name];
            console.log(old instanceof Function);
            obj[name] = function(){//此时people有了find属性（一个函数）
                console.log(arguments.length+" "+fnc.length);
                if(arguments.length === fnc.length){//3 === 0
                    return fnc.apply(this,arguments);
                }else if(typeof old === "function"){
                    return old.apply(this,arguments);
                }
            }
        }
        var people = {
            values:["Zhang san","Li si"]
        };
        method(people,"find",function(){
            console.log("无参数");
            return this.values;
        })
        method(people,"find",function(firstname){
            console.log("一个参数");
            var ret = [];
            for(var i = 0;i < this.values.length;i++){
                if(this.values[i].indexOf(firstname) === 0){
                    ret.push(this.values[i])
                }
            }
            return ret;
        })
        method(people,"find",function(firstname,lastname){
            console.log("两个参数");
            var ret = [];
            for(var i = 0;i < this.values.length;i++){
                if(this.values[i] == firstname + " " + lastname){
                    ret.push(this.values[i])
                }
            }
            return ret;
        })
        console.log(people.find());
        console.log(people.find("Zhang"));
```

 

思路：这段代码第一眼看到我是懵逼的，再看有点思路，再看又懵了。这种方法巧妙的运用了闭包原理，既然js后面的函数会覆盖前面的同名函数，我就强行让所有的函数都留在内存里，等我需要的时候再去找它。有了这个想法，是不是就想到了闭包，函数外访问函数内的变量，从而使函数留在内存中不被删除。就是闭包。

**实现过程**：我们看一下上面这段代码，最重要的是method方法的定义：这个方法中最重要的一点就是这个old，这个old真的很巧妙。它的作用相当于一个指针，指向上一次被调用的method函数，这样说可能有点不太懂，我们根据代码来说，js的解析顺序从上到下为。

　　1.解析method（先不管里面的东西）

　　2.method(people,"find",function()  执行这句的时候，它就回去执行上面定义的方法，然后此时old的值为空，因为你还没有定义过这个函数，所以它此时是undefined，然后继续执行，这是我们才定义 obj[name] = function()，然后js解析的时候发现返回了fnc函数，更重要的是fnc函数里面还调用了method里面的变量，这不就是闭包了，因为fnc函数的实现是在调用时候才会去实现，所以js就想，这我执行完也不能删除啊，要不外面那个用啥，就留着吧先（此处用call函数改变了fnc函数内部的this指向）

　　3.好了第一次method的使用结束了，开始了第二句，method(people,"find",function(firstname) 然后这次使用的时候，又要执行old = obj[name]此时的old是什么，是函数了，因为上一条语句定义过了，而且没有删除，那我这次的old实际上指向的是上次定义的方法，它起的作用好像一个指针，指向了上一次定义的 obj[name]。然后继续往下解析，又是闭包，还得留着。

　　4.第三此的method调用开始了，同理old指向的是上次定义的 obj[name] 同样也还是闭包，还得留着。

　　5.到这里，内存中实际上有三个 obj[name]，因为三次method的内存都没有删除，这是不是实现了三个函数共存，同时还可以用old将它们联系起来是不是很巧妙

　　6.我们 people.find() 的时候，就会最先调用最后一次调用method时定义的function，如果参数个数相同 也就是  arguments.length === fnc.length 那么就执行就好了，也不用找别的函数了，如果不相同的话，那就得用到old了  return old.apply(this,arguments); old指向的是上次method调用时定义的函数，所以我们就去上一次的找，如果找到了，继续执行 arguments.length === fnc.length  如果找不到，再次调用old 继续向上找，只要你定义过，肯定能找到的，对吧！

　　总结：运用闭包的原理使三个函数共存于内存中，old相当于一个指针，指向上一次定义的function，每次调用的时候，决定是否需要寻找。

![img](https://images2017.cnblogs.com/blog/1019408/201712/1019408-20171207102842113-807434197.png)

执行过程很容易说明这一点：首先第一次调用的时候 old肯定不是函数，所以instance判断是false，继续调用的话就会为true。然后，我们调用method的顺序，是从没有参数到两个参数，所以我们最先调用find方法，是最后一次method调用时定义的，所以fnc的length长度是2.然后向上找，length为1，最后终于找到了length为0的然后执行，输出。

# 第四章 作用域和内存问题

## 基本类型和引用类型的值

ECMAScript变量可能包含两种不同数据类型的值：基本类型值和引用类型值。

- 基本类型值值的是简单的数据段。
  - 五种基本数据类型：undefined、null、boolean、number、string。

- 引用类型值值那些能由多个值构成的对象。
  - 引用类型的值是保存在内存中的对象。
  - 与其他语言不同，JavaScript不允许直接访问内存中的位置，也就是**不能直接操作对象的内存空间**。在操作对象的时候，实际上是在操作对象的引用而不是实际的对象。

- **基本类型值在内存中占据固定大小的空间，因此被保存在栈内存中**。
- **引用类型值是对象，保存在堆内存中**。
- 包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针。因此复制的时候其实是复制的指针，因此两个变量最终都指向同一个对象。

### 堆和栈的区别

**1、堆栈空间分配区别**

栈（操作系统）：由操作系统（编译器）自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

堆（操作系统）： 一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回收，分配方式倒是类似于链表。

**2、堆栈缓存方式区别**

栈使用的是一级缓存， 它们通常都是被调用时处于存储空间中，调用完毕立即释放。

堆则是存放在二级缓存中，生命周期由虚拟机的垃圾回收算法来决定（并不是一旦成为孤儿对象就能被回收）。所以调用这些对象的速度要相对来得低一些。

**3、堆栈数据结构区别**

堆（数据结构）：堆可以被看成是一棵树，如：堆排序。

栈（数据结构）：一种先进后出的数据结构。

### 动态的属性

引用类型值可以动态添加属性和方法。

但是不能给基本类型值添加属性和方法，尽管这样不会导致任何错误：

~~~js
let name = "jingwei";
name.ago = 18;
alert(name.ago);  //undefined(但不报错)
~~~

### 赋值变量值

- 基本类型值

  - ```js
    let num1 = 10;
    let num2 = num1;
    alert(num2); //10
    num2 += 5;
    alert(num1);  //10
    alert(num2);  //15
    ```

  - num2中的10只是num1的一个副本，初始化之后二者相互独立，互不影响。

- 引用类型值

  - 复制另一个引用类型值的时候实际上是引用到了一个**指针**，指针指向存储在堆中的一个对象。

  - 改变其中一个另一个也会随着改变

  - ```js
    let obj1 = new Object();
    let obj2 = obj1;
    obj1.name = "jingwei";
    alert(obj2.name);   //jingwei
    ```

### 传递参数

ECMAScript中所有参数都是按值传递的。

访问变量有按值和按引用两种，而**参数只能按值传递**。

- 基本类型值的传递和复制一样，将一个值复制给参数，二者互不影响。

  - ```js
    let num = 10;
    function addTen(a){
        a += 10;
        return a;
    }
    let num1 = addTen(num);
    alert(num);	//10(不变)
    alert(num1);//20
    ```

- 而**引用类型值的传参是将引用类型值的地址复制给参数**（局部变量），因此这个局部变量的变化会反应在外部。

  - ```js
    function setName(obj) {
        obj.name = "Nicholas";
    }
    let person = new Object();
    alert(person.name);  //"Nicholas"(函数外部也将改变)
    //person指向的对象在堆内存中只有一个，而且是全局对象
    ```

- （重中之重）：如何理解引用类型值是按值传递，如下有个例子：

  - ```js
    function setName(obj) {
        obj.name = "Nicholas";
        var obj = new Object();
        obj.name = "Greg";
    }
    
    var person = new Object();
    setName(person);
    alert(person.name);        //"Nicholas"
    ```

    定义了一个person对象，然后作为参数传入setName函数，其name = "Nicholas",然后又将一个新的对象赋给obj并添加属性name=“Greg”。如果person是按引用传递的，那么person就会被修改为指向name为Greg的新对象。	但是，接下来访问person.name 的时候，显示的仍然是“Nicholas"。这说明即使在函数内部修改参数的值，但原始的引用仍然保持未变。	实际上，在函数内部重写obj 的时候，这个变量引用的就是一个局部对象了。而这个局部对象在函数执行完立即被销毁。

  - 

### 检测类型

检测一个变量是不是基本数据类型时，最好的方法就是typeof操作符。

但是**检测引用类型值时，用instanceof操作符**。

```js
alert(person instanceof Object);  //变量person是Object实例吗？
alert(colors instanceof Array);   //变量colors是Array实例吗？
alert(pattern instanceof RegExp); //变量pattern是RegExp实例吗？
```

根据规定，所有的引用类型的值都是Object的实例。

instanceof检测引用类型值始终返回true，检测到基本数据类型值始终返回false。

## 执行环境及作用域

执行环境定义了变量或函数有权按访问的其他数据，决定了它们各自的行为。	每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。虽然我们无法访问它，但是编译器在后台可以访问。

在Web浏览器中，全局执行环境（最外围的环境）被认为是windows对象，因此所有的全局变量和函数都适合作为window对象的属性和方法创建的。

某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（全局环境直到应用程序退出----例如关闭网页或浏览器----时才会被销毁）

每个函数都有自己的执行环境。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。

当代码在一个环境中执行时，会创建变量对象的一个作用域链。

### 延长作用域链

- [ ] try-catch语句的catch块
- [ ] with语句

```js
function buildUrl() {
    let qs = "?debug = true";
    
    with(location) {
        let url = href + qs;
    }
    return url;
}
```

### 没有块级作用域

```js
for(let i = 0; i < 10; i ++) {};

alert(i);   //10
```

在其它语言中外部是访问不到i 的。

查找变量的时候是从内向外一层一层查找，内部作用域有就不会查到外部作用域。

## 垃圾收集

javascript具有自动垃圾收集机制，也就是说执行环境会负责管理代码执行过程中使用的内存。

垃圾收集器会按照固定的时间间隔周期性地清除内存。

垃圾收集的两种策略：

### 标记清除

JavaScript中**最常用**的垃圾收集方式。

（执行环境有全局执行环境和函数执行环境之分）。

垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记（任何标记方式）。然后去掉环境（如window)中的变量以及被环境中的变量所引用的变量的标记，然后清除身上还有标记的变量并释放内存，原因是环境中的变量已经无法访问到这些变量（如函数中的局部变量且在外部没有被引用）。回收后将所有变量标记全部清除，以便于下次垃圾收集。

### 引用计数

不太常见。

跟踪记录每个变量被引用的次数，执行加一减一的操作，为0的时候则说明没有办法再引用到这个值，然后就回收回来。

##### 缺点是有循环引用的问题

比如：

```js
function problem() {
    var obj1 = new Object();
    var obj2 = new Object();
    
    obj1.someOtherObject = obj2;
    obj2.anotherObject = obj1;
}
```

例子中的obj1和obj2循环引用，如果用引用计数来垃圾回收，二者引用次数都是2，假如这个函数被多次调用，就会导致大量内存得不到回收，就会导致性能问题。

### 性能问题

经典案例就是IE因此而名声狼藉的性能问题，IE当时的垃圾收集器是根据内存分配量运行的，256个变量，4096个对象或数组或者是64KB的字符串，达到上述任何一个临界值，垃圾收集器就会运行，但是如果一个脚本中包含那么多变量，垃圾收集器就会频繁运行，结果引发的严重性能问题促使IE7重写了其垃圾收集例程。

IE7的js引擎改变了工作方式：临界值调整为动态修正，如果回收量小于15%，那么临界值翻倍；如果大于85%，那么临界值恢复。 这一调整极大的提升了IE的性能。

有的浏览器可以触发垃圾回收机制，但不建议这样做。

### 管理内存

确保占用最少的内存可以让页面性能更好。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再用，最好设置为null来释放其引用——这个做法叫**解除引用**。适用于大多数全局变量或全局对象的属性。

```js
function createPerson(name) {
    var localPerson = new Object();
    localPerson.name = name;
    return localPerson;
    //局部变量执行后会被自动回收
}

var globalPerson = createPerson("Nicholas");
//手动解除引用
globalPerson = null;
```

解除引用的真正作用是让值脱离执行环境，以便垃圾收集器下次运行时将其回收。

# 第五章 引用类型

对象是引用类型的一个实例。

在ECMAScript中，引用类型是一种数据结构，用于将数据和功能组织在一起。它也常被称为类，但这种称呼并不妥当。

尽管ECMAScript从技术上讲是一门面向对象的语言，但它不具备传统面向对象语言所支持的类和接口等基本结构。

虽然引用类型与类看起来相似，但并不同。

## valueOf()&toString()&toLocaleString()

### valueOf()

**返回对象的原始值。**

### toString()

**返回对象的字符串表示**。可以传参决定返回基数的数值字符串。

### toLocaleString()

调用每个元素的toLocaleString()方法，然后使用地区特定的分隔符（格式）将生成的字符串连接起来，形成一个字符串。

（虽然经常和toString（）返回同一个值）

### toString() vs toLocaleString()

- toString()方法调用的是toString(传统字符串)，而toLocaleString()方法调用的是toLocaleString(本地环境字符串)。
- 如果你开发的脚本在世界范围内都有人使用，那么将对象转换为字符串请用toString()方法来完成，它可以保证在不同的地区返回的值是相同的。
- toLocaleString()会根据你机器的本地环境来返回字符串，它和toString()的返回值在不同的本地环境下使用的符号会有微妙的变化。
- 所以使用toString()是保险的，它的返回值是唯一的，它的返回值不会因为本地环境变化而改变。
- **如果是返回时间类型的数据，推荐使用toLocaleString()。若是后台处理字符串，请务必使用toString()。**

## Object类型

我们看到的大多数引用类型值都是Object类型的实例。

### 创建Object实例的两种方式：

- ```js
  //操作符加构造函数
  var person = new Objct();
  person.name = "Nicholas";
  person.age = 18;
  ```

- ```js
  //使用对象字面量表示
  var person = {
      name : "Nicholas",
      age : 18
  };
  //属性名也可以是字符串
  var person = {
      "name" : "Nicholas",
      "age" : 18
  };
  ```

- ````js
  var person = {};    //与new Object()相同
  person.name = "Nicholas";
  person.age = 18
  ````

- 在通过对象字面量定义对象时，实际上不会调用Object构造函数。

- 程序员们更青睐对象字面量语法，因为代码量少而且给人封装数据的感觉。实际上，对象字面量也是向函数传递大量参数的首选方式。

- 举例：

  - ```js
    function displayInfo(args) {
        var output = "";
        
        if(typeof args.name == "string") {
            output += "Name:" + args.name + "\n";
        }
        if(typeof args.age == "number") {
            output += "Age:" + args.age + "\n";
        }    
        alert(output);
    }
    
    displayInfo({
        name : "Nicholas",
        age : 18
    });
    
    displayInfo({
        name : "Jingwei",
        age :17
    })
    ```

- 最好的做法是，对那些必须值使用命名参数，而使用对象字面量来封装多个可选参数。

一般来说，访问对象属性时使用的都是点表示法（person.name），这也是很多面向对象语言中通用的语法。不过，在JavaScript中也可以使用**方括号表示法**来访问对象的属性，不过要将属性名以字符串的形式放在方括号中：

```js
alert(person["name"]);
alert(perosn.name);
```

**方括号主要的优点是可以通过变量来访问属性。**

```js
var person = new Object();
person.name = "Nicholas";

var propertyName = "name";
console.log(person[propertyName]);  //Nicholas
```

如果属性名中包括会导致语法错误的字符，或者关键字、保留字，也可以使用方括号表示法：

```js
perosn["first name"] = "Nicholas";
```

**由于有空格，所以不能用点语法表示。**

通常除非必须使用变量来访问属性，否则**建议使用点表示法**。 

## Array类型

与其他语言不同的是，ECMAScript数组的每一项可以保存任何类型的数据。而且数组大小可以动态调整的。

创建数组：

- 第一种方法：

  - ```js
    //用构造方法
    var arr = new Array(20);//长度为20
    
    var color = new Array("red", "yellow", "green");
    
    //也可以省略new
    
    var arr = Array('a', 'b', 'c');
    ```

- 第二种方法

  - ```js
    //使用数组字面量表示
    var color = ['blue', 'green', 'red'];
    ```

数组的length不是只读的，可以通过设置这个属性在数组的末尾删除或添加新项（undefined）；

灵活使用：

```js
var colors = ['red', 'blue', 'green'];
colors[colors.length] = 'black';  //在末尾添加一项
colors[colors.length] = 'yellow'; //在末尾再添加一项
```

因为数组最后一项是length-1，所以在length位添加就是最后一位添加。

### 检测数组

使用instanceof操作符：

```js
if (value instanceof Arrray) {
    //对数组执行某些操作
}
```

新增**Array.isArray()**方法来确定某个值到底是不是数组，而不管它是在哪个全局执行环境中创建的。

```js
if (Array.isArray(value)) {
    //对数组执行某些操作
}
```

### 转换方法

如前所述，所有对象都具有toLocaleString()、toString()和valueOf()方法。

#### toString()

返回对象的字符串形式，（作用于数组会返回每个值的形式拼接而成 的一个以逗号分隔的字符串。）

当对象被表示为文本值时或者当以期望字符串的方式引用对象时，该方法被自动调用。

可以自己定义一个对象的toString()方法来覆盖它原来的方法。这个方法不能含有参数，方法里必须return一个值。

#### valueOf()

返回指定对象的原始值。

JavaScript调用valueOf（）方法用来将对象转换成原始类型的值（数字，字符串和布尔值）。

如果对象没有原始值，就会返回对象本身。

可以自己定义一个对象的valueOf()方法来覆盖它原来的方法。这个方法不能含有参数，方法里必须return一个值。

#### toLocaleString()

经常与以上两种方法返回相同的值。当调用数组的toLocaleString()的方法时，他也会创建一个数组值的一个以逗号分隔的字符串，但不同的是，每一项的值调用的是toLocaleString()方法，而不是toString()，请看下面的例子：



```js
//通过重写两个对象的两个方法，测试两种方法触发的时候数组中的每个元素调用的是那种方法
var person1 = {
    toLocaleString : function () {
        return "Nicholas";
    },
    toString : function () {
        return "Nicholas";
    }
}

var person2 = {
    toLocaleString : function () {
        return "Grigorios";
    },
    toString : function () {
        return "Greg";
    }
}

var people = [person1, person2];
alert(people);  //Nicholas,Greg
alert(people.toString());  //Nicholas,Greg
alert(people.toLocaleString()); //Nicholas,Grigorios
```

#### join()

**数组转换为字符串**，指定分隔符。

```js
var colors = ['red', 'green', 'blue'];
alert(colors.join("-"));  //red-green-blue
alert(colors.join("!"));  //red!green!blue
alert(colors.join(" "));   //red green blue
```

如果不传值或传undefined，则默认逗号分隔。

### 栈方法

栈是一种LIFO（Last-In-First-Out，后进先出）的数据结构，也就是新添加的项最早被移除。而栈中项的插入和移除只发生在栈的顶部。

#### push()

接受任意数量的参数，逐个**添加到数组末尾**，并返回数组长度。

#### pop()

从数组末尾**移除最后一项**，减少数组length的值，然后返回移除的项。

```js
var colors = new Array();
var item = colors.push("red", "green", "yellow");
alert(item);		//3
alert(colors);		//'red','green','yellow'

var item2 = colors.pop();
alert(item2);		//"yellow"
alert(colors);  	//"red","green"
```

### 队列方法

队列是一种FIFO（First-In-First-Out，先进先出）的数据结构。也就是在末端添加项在顶端移除项。

#### shift()

**移除数组的第一项**并返回该项，同时数组长度减一。

#### unshift()

在数组**前端添加任意个项**并返回数组长度。

```js
var colors = new Object();
var item1 = colors.push("red","green","yellow");
alert(item1);			//3
alert(colors);			//"red","green","yellow"

var item2 = colors.pop();
alert(item2);			//"yellow"
alert(colors);			//"red","green"

var item3 = colors.shift();
alert(item3);			//"red"
alert(colors);			//"green"

var item4 = colors.unshift("black","blue");
alert(item4);			//3
alert(colors);			//"black","blue","green"s
```

### 重排序方法

#### reverse()

**反转数组**项的顺序

```js
var arr = [1,2,3,4,5,6,7];
arr.reverse();//改变原数组的
console.log(arr);	//,7,6,5,4,3,2,1
```

#### sort()

给数组项**排序**。

由于sort()排序的原理是将每个项都调用toString()转型方法，然后比较得到的字符串，以确定排序顺序。即使每项都是数值，sort()方法比较的也是字符串。

默认为升序排列，最小值放到最前面。

```js
var arr = [1,5,7,11,23,56];
arr.sort();
alert(arr);     //1,11,23,5,56,7
```

然而很多时候这种结果并不是我们想要的，所以sort()方法可以接收一个比较函数作为参数。

比较函数：接收两个参数，如果第一个参数应该位于第二个之前，则返回一个负数；反之返回正数，相等返回0。

```js
//比较函数
function compare(num1,num2) {
    return num1 - num2;		//(升序)但是只有数字数组才可以这样写，字符串相减会返回NaN，但是可以比较大小，所以可以用书上的方法。
}

//sort()
var arr = [1,3,6,8,12,34,67,78];
arr.sort(compare);
alert(arr)
```

### 操作方法

#### concat()

**复制一个新数组**并且向里面添加元素，原数组保持不变。

```js
var colors = ["red", "yellow","green"];
var colors2 = colors.concat("blue", ["black", "white"]);
alert(colors); 		//"red", "yellow","green"
alert(colors2);		//"red", "yellow","green","blue","black","white"
```

#### slice()

**切割数组**并返回一个新数组，原数组保持不变。

接收两个参数  **[** 起始位置，结束位置**)**，如果只有一个参数就是起始位置到最后所有项。

```js
var colors = ["red", "yellow","green"];
var colors2 = colors.slice(1,2);
alert(colors2);		//"yellow"
var colors3 = colors.slice(1);
alert(colors3);		//"yellow","green"
```

如果参数有负数，则用数组长度加上该负数以确定相应的位置。

#### splice()---import

**最强大的数组方法**。它改变原数组。

它有三种用途：

- [ ] 删除：可以删除任意数量项，需两个参数（起始位置，要删除的项数），返回删除项。

  - [ ] ```js
    var colors = ["red", "green", "yellow"];
    var colors2 = colors.splice(1,2);
    alert(colors);  	//"red"
    alert(colors2);  //"green","yellow"
    ```

- [ ] 插入：指定位置插入项，需三个参数，（起始位置，0（要删除的项数），要插入的项），插入项可以是任意数量。，返回删除项或空数组；

  - [ ] ```js
    var colors = ["red", "green", "yellow"];
    var colors2 = colors.splice(1,0,"black");
    alert(colors);		//"red","green","black","yellow"
    alert(colors2);		//[]
    ```

- [ ] 替换：指定位置删除项然后在插入项，二者不必相等。（起始位置，2（要删除的项数），要插入的项），返回删除项或空数组。

  - [ ] ```js
    var colors = ["red", "green", "yellow"];
    var colors2 = colors.splice(1,2,"black","white");
    alert(colors);		//"red","black","white"
    alert(colors2);		//"green","yellow"
    ```

### 查找方法

#### indexOf()

**用来查找数组元素位置**。从数组开头开始向后查找，接收两个参数（要查找的项，（可选）起始位置索引）。

返回数组元素的位置，如果没找到返回-1。

在比较第一个参数与数组中的每一项时，使用的是全等操作符（===）。

#### lastIndexOf()

与indexOf()唯一不同的是它**从末尾开始查找元素**。

而且二者都是找到一个该元素就停止查找，所以如果数组里面有不止一个要查找的元素该方法也只会返回它找到的第一个元素。

```js
var arr = [1,6,2,7,2,5,7];
var a = arr.indexOf(2);
console.log(a);		//2
var b = arr.lastIndexOf(2,5);
console.log(b);		//4
var c = arr.indexOf(3);
console.log(c);		//-1
```

### 迭代方法

ECMAScript为数组定义了5个迭代方法。每个方法都接收两个参数：要在每一项上面运行的函数和（可选）运行该函数的作用域对象——影响this的值。

传入的函数接收三个参数（数组项的值（item）、该项在数组中的位置（index）、数组对象本身（array））。

**这些方法都不会修改原数组**。

#### every()

对数组中的每一项运行给定的函数，如果该函数对每一项都返回true，则返回true。——  &&

```js
var arr = [1,2,3,4,5];
var everyResult = arr.every(function (item,index,arr) {
    return (item>2);
})
alert(everyResult);		//false
```

#### some()

对数组中的每一项运行给定的函数，如果该函数对任一项返回true，则返回true。——  ||

````js
var arr = [1,2,3,4,5];
var someResult = arr.some(function (item,index,array) {
    return (item>2);
})
alert(someResult);		//true
````

#### filter()

对数组中每一项运行给定的函数，返回该函数会返回true的项组成的新数组。

```js
var arr = [1,2,3,4,5];
var filterResult = arr.filter(function (item,index,array) {
    return (item>2);
})
alert(filterResult);	//[3,4,5]
```

#### forEach()

对数组中每一项运行给定的函数，这个方法**没有返回值**，但也不改变原数组，只是用来遍历数组。本质上与for循环迭代数组一样。

```js
var arr = [1,2,3,4,5];
var num = 0;
arr.forEach(function (item) {
    num += item;
})

alert(num);			   //15
```

#### map()

对数组中每一项运行给定的函数，返回每次函数调用的结果组成的新数组。

```js
var arr = [1,2,3,4,5];
var mapResult = arr.map(function (item) {
    return item + 1;
})
alert(mapResult);		//[2,3,4,5,6]
```

### 归并方法

#### reduce()

迭代数组中所有项，从数组第一项开始，方法接收两个参数：一个在每一项上调用的函数，和（可选的）作为归并基础的初始值。

函数接收四个参数：前一个值，当前值，（可选）项的索引，（可选）数组对象。

这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组第二项上，因此最开始第一个参数为数组的第一项，第二个参数为数组的第二项。

reduce（）方法适用于数组求和求积的运算：

```js
var arr = [1,2,3,4,5];
var reduceResult = arr.reduce(function (prev,cur,index,array) {
    return prev + cur;
})
alert(reduceResult); 	//15
```

#### reduceRight()

与上一方法唯一不同的就是此方法在数组最后开始向第一个项遍历，除此之外完全相同。

## Date类型

ECMAScript中的Date类型是在早期Java中的java.util.Date类基础上构建的。为此，Date类型使用自UTC（国际协调时间）**1970年1月1日零时开始经过的毫秒数来保存日期**。Date可以精确到1970年1月1日前后的100 000 000年。

这里为什么是197 0年呢？这就涉及到了计算机操作系统的东西了。

### UNIX系统简介

1969年8月由贝尔实验室的肯 · 汤普森和丹尼斯 · 里奇用B语言编写发明Unics系统，是UNIX系统的原型，它的部分技术来源可追溯到1965年开始的Multics工程计划，由贝尔实验室、美国麻省理工学院和通用电气公司联合发起，目的是开发一个优秀的分时操作系统，以取代当时的批处理操作系统。

但1969年发布的只是一个UNIX的雏形，UNIX真正意义上的诞生是1970年，所以**1970年是UNIX世界的纪元**。所以UNIX时间戳定义为从格林尼治时间1970年1月1日00时00分00秒起到现在的秒数。

当时的操作系统是32位的，32位能表示的最长时间为68年，也就是说到2038年1月19日便会出现时间回归现象，很多软件便会出现异常。	由于现在操作系统都是64位的，所以这个问题已经得到了解决，64位操作系统能表示的世间毫不夸张的说可以表示到世界末日。

1970年加以优化Unics系统，并更名为UNIX系统。所以1970年是UNIX元年。

1973年，丹尼斯 · 里奇改良了B语言，这就是今天大名鼎鼎的C语言。	于是肯 · 汤普森和丹尼斯 · 里奇成功地用C语言重写了UNIX系统的第三版内核。

1974年UNIX上发布第一篇文章，于是UNIX第五版开始广泛流行起来，用于教育以及科研。

在目前主流的服务器端操作系统中，UNIX诞生于20世纪60年代末，Windows诞生于20世纪80年代中期，Linux诞生有20世纪90年代初，可以说UNIX是操作系统的老大哥，后来的windows和Linux都参考了UNIX。

iOS和Linux系统都是基于UNIX系统衍生而来。

好的回归主题：

### Date对象的创建

> var now = new Date();

不传参新对象自动获得当前时间日期。

如果想创建特定时间的Date对象，必须传入表示该日期的毫秒数，ECMAScript提供了两种方法：

#### Date.parse()

接收一个表示日期的字符串，然后根据字符串**返回日期的毫秒数**。

美国浏览器通常都接受下列日期格式：

- [ ] 月/日/年   6/13/2019

- [ ] January 3,2019

- [ ] Tue May 25 1019 00:00:00 GMT-700

- [ ] ISO 8601扩展格式

  如：

  ```js
  var someDate = new Date(Date.parse("May 26,2019"));
  alert(someDate); //Thu May 26 2019 00:00:00 GMT+0800 (中国标准时间)
  ```

如果传入的字符串不能表示日期，则返回NaN。

实际上直接将字符串传递给Date构造函数，也会在后台调用Date.parse()方法，或者说，下面的代码和上面的是等价的：

```js
var someDate = new Date("May 26,2019");
alert(someDate); 	//Thu May 26 2019 00:00:00 GMT+0800 (中国标准时间)
```

#### Date.UTC()

同样返回日期的毫秒数，但参数不同：（年份、基于0的月份（一月是0）、日、小时、分钟、秒、毫秒）。只有前两个是必须的。天数默认为1，其他的默认为0：

```js
var someDate = new Date(Date.UTC(2019, 2, 13, 15, 02, 00));
alert(someDate);   	// Wed Mar 13 2019 23:02:00 GMT+0800 (中国标准时间)
```

两个方法有区别，**前者是返回本地时间，后者是返回UTC时间**，一般情况下，两者相差8小时，也就是说UTC时间比中国标准时间快8小时。

Date构造函数也会模仿Date.UTC()方法。但有一点不同，时间和日期都是基于本地时区而非GMT创建。 也就解决了8小时的时差问题。

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);
alert(someDate);   	//Wed Mar 13 2019 15:02:00 GMT+0800 (中国标准时间)
```

Date构造函数会判断，第一个数是数值，就调用Date.UTC()方法，但是是基于本地时间的；而如果第一个元素时字符串，就调用Date.parse()方法。

#### Date.now()

**返回表示调用这个方法时候的日期和时间的毫秒数。**

```js
//取得开始时间
var start = Date.now();

//执行函数
doSomething();

//取得结束时间
var stop = Date.now();

//计算函数所执行的时间
var time = stop - start;
alert(time);
```

在不支持Date.now()的浏览器中使用+操作符也可以达到同样的目的。

```js
//取得开始时间
var start = +new Date();

//执行函数
doSomething();

//取得结束时间
var stop = +new Date();

//计算函数所执行的时间
var time = stop - start;
alert(time);
```

### 继承的方法

与其他引用类型一样，Date类型也写了以下方法：

#### toLocaleString()

按照与浏览器设置的地区相适应的格式返回日期和时间，也就包含AM和PM但不包含时区信息（具体格式因浏览器而异）。

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

alert(someDate.toLocaleString());		"2019/3/13 下午3:02:00"
```

#### toString()

返回带有时区信息的日期和时间。

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

someDate.toString();
"Wed Mar 13 2019 15:02:00 GMT+0800 (中国标准时间)"
```

#### valueOf()

根本不返回字符串了，而是返回日期的毫秒显示。

因此方便比较两个日期值。(隐式转换)

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

someDate.valueOf();
1552460520000

var date1 = new Date(2018, 11,1);
var date2 = new Date(2019, 1,1);
alert(date1 > date2);  //false
```

### 日期格式化方法

将日期格式化为字符串的方法：(返回格式化的日期副本，原日期不变)

#### toDateString()

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

someDate.toDateString()
"Wed Mar 13 2019"

someDate
Wed Mar 13 2019 15:02:00 GMT+0800 (中国标准时间)
```

#### toTimeString()

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

someDate.toTimeString()
"15:02:00 GMT+0800 (中国标准时间)"
```

#### toLocaleDateString()

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

someDate.toLocaleDateString()
"2019/3/13"
```

#### toLocaleTimeString()

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

someDate.toLocaleTimeString()
"下午3:02:00"
```

#### toUTCString()

```js
var someDate = new Date(2019, 2, 13, 15, 02, 00);

someDate.toUTCString()
"Wed, 13 Mar 2019 07:02:00 GMT"
```

以上这些字符串格式方法的输出也是因浏览器而异的。

## RegExp类型

### 	3个标志

#### 		g：全局模式

#### 		i：忽略大小写模式

#### 		m：多行模式



### 元字符必须转义

​	正则里的元字符有：

​		(  [  \  ^  $  |  )  ?  *  +  .   ]  }

### 定义正则表达式

1. 字面量形式：var regexp = /asdf/i
2. 构造函数：var regexp = new RegExp("asdf", 'i');           参数都是字符串。

### RegExp实例属性

RegExp的每个实例都有下列属性：

- global：布尔值，表示是否设置了g属性。
- ignoreCase：布尔值，表示是否设置了i属性。
- multiline：布尔值，表示是否设置了m属性。
- lastIndex：整数，表示开始搜索 的下一个匹配项的位置，从0算起。
- source：正则表达式的字符串表示，

### RegExp实例方法

#### exec()

RegExp的主要方法是exec()，该方法是专门为捕获组设计的。exec()接收一个参数，就是要应用模式的字符串，然后返回包含第一个匹配项信息的数组；没有匹配项的情况下返回null。返回的虽然是array实例，但包含额外的两个属性：index和input。其中，index表示匹配项在字符串中的位置，而input表示应用了正则表达式的字符串。如果没有捕获组，则该数组只包含一项。

捕获组就是把正则表达式中子表达式匹配的内容，保存到内存中以数字编号或显式命名的组里，方便后面引用。

```js
var text "mom and dad and baby";
var pattern = /mom (and dade( and baby)?)?/gi;

var matches = pattern.exec(text);
alert(matches.index);		//0
alert(matches.input);		//mom and dad and baby
alert(matches[0]);			//mom and dad and baby
alert(matches[1]);			//and dad and baby
alert(matches[2]);			//and baby
```

```js
var str = "wang jing wei shi ge da shuai ge";

var pattern = /jing wei( shi ge(da)?)?/gi;

var matches = pattern.exec(str);

matches
(3) ["jing wei shi ge", " shi ge", undefined, index: 5, input: "wang jing wei shi ge da shuai ge", groups: undefined]
```

此例中"shi ge"就是一个捕获组，在数组的第二项中。

对于exec（）而言，无论有没有g都只返回一个匹配项，但是如果没有g的情况下多次调用同一个字符串将始终返回第一个匹配项信息；如果有g的情况下同一个字符串多次调用exec（）都会在字符串中查找下一个匹配项然后返回：

```js
var text = "cat, bat, sat, fat";
undefined
var pattern = /.at/;
undefined
var matches = pattern.exec(text);
undefined
matches
["cat", index: 0, input: "cat, bat, sat, fat", groups: undefined]
pattern.lastIndex
0
matches = pattern.exec(text);
["cat", index: 0, input: "cat, bat, sat, fat", groups: undefined]
var pattern2 = /.at/g;
undefined
var matches = pattern2.exec(text);
undefined
matches
["cat", index: 0, input: "cat, bat, sat, fat", groups: undefined]
pattern2.lastIndex
3
matches=pattern2.exec(text);
["bat", index: 5, input: "cat, bat, sat, fat", groups: undefined]
pattern2.lastIndex
8
```

#### lastIndex属性

lastIndex从字面上来讲是最后一个索引，实际上它的意思是正则下一次开始查找的索引位置，第一次的时候总为0，第一次查找完之后会把lastIndex值设为匹配到的字符串的最后一个字符的位置加1，然后第二次查找就在此开始查，后面的以此类推。	如果没找到则lastIndex为0。

要注意的是，此属性只有在全局正则上面有用。g

#### test()

它接收一个参数，即要匹配的字符串。在模式与该参数匹配的情况下返回=true，否则返回false。因此经常被用在if语句中。

在只想知道目标字符串与某个模式是否匹配，但不需要知道其内容的情况下是用这个方法非常方便。

```js
var text = "123-45-6789";
var pattern = /\d{3}-\d{2}-\d{4}/;
undefined

if(pattern.test(text)){console.log("The pattern sas matched.")};
The pattern sas matched.
undefined
```

这种做法经常用在验证用户输入的情况下，因为我们只想知道输入的是不是有效，至于它为什么无效就无关紧要了。

#### toString()

#### toLocaleString()

这两个方法显示正则的字符串表示。

#### valueOf()

返回正则表达式本身。

```js
var pattern = /\d{2}-\d{3}/;
undefined
pattern.toString();
"/\d{2}-\d{3}/"
pattern.toLocaleString();
"/\d{2}-\d{3}/"
pattern.valueOf();
/\d{2}-\d{3}/
```

### RegExp构造函数属性

这些是RegExp构造函数的属性，作用在RegExp上。适用于作用域中的所有正则表达式，并且基于所执行的最近一次正则表达式操作而变化。

| 长属性名     | 短属性名 | 说明                             |
| ------------ | -------- | -------------------------------- |
| input        | $_       | 返回最近一次要匹配的字符串       |
| lastMatch    | $&       | 最近一次匹配的匹配项             |
| lastParen    | $+       | 最近一次匹配的捕获组             |
| leftContext  | $`       | input字符串中lastMatch之前的文本 |
| rightContext | $'       | input字符串中lastMatch之后的文本 |

```js
var text = "this has been a short summer";
	undefined
var pattern = /(.)hort/g;
	undefined
pattern.test(text);
	true
RegExp.input
	"this has been a short summer"
RegExp;
	ƒ RegExp() { [native code] }
RegExp.lastMatch;
	"short"
RegExp.lastParen;
	"s"
RegExp.leftContext;
	"this has been a "
RegExp.rightContext;
	" summer"
RegExp.multiline;
	undefined
RegExp.$_;
	"this has been a short summer"
RegExp.$&;
	VM974:1 Uncaught SyntaxError: Unexpected token ;
RegExp["$&"];
	"short"
RegExp["$+"];
	"s"
RegExp["$`"];
	"this has been a "
RegExp["$*"];
	undefined
RegExp["$'"];
	" summer"
pattern = /(.)hort/gm;
	/(.)hort/gm
pattern.test(text);
	true
RegExp["$*"];
	undefined
```

```js
//接上面
pattern.multiline;
true
var pattern = /(.)hort/g;
undefined
pattern.test(text);
true
pattern.multiline;
false
pattern.ignoreCase;
false
pattern.global;
true
```

这里的是RegExp实例属性作用在pattern这个正则实例上。

除了这些，还有多达九个用于存储捕获组的构造函数属性。访问这些属性的语法是：

RexExp.$1  RexExp.$2    ...   RexExp.$9

分别用来存储第一、第二 ...  ... 第九个匹配的捕获组。

```js
var text = "this has been a short summer";
undefined
var pattern = /(..)or(.)/g;
undefined

pattern.test(text);
true
RegExp.$1
"sh"
RegExp.$2
"t"
RegExp.$3
""
RegExp.$4
""
```

要区分RegExp实例属性和RegExp构造函数属性! ! !

## Function类型

函数实际上是对象，每个函数 都是Function类型的实例，而且都具有属性和方法。

由于函数是对象，因此函数名就是一个指向函数对象的指针，不会与某个函数绑定。

### 函数定义的3种方法

- 函数声明语法定义

  - ```js
    function sum (num1, num2) {
        return num1 + num2;
    }
    ```

    - 最常用

- 函数表达式定义

  - ```js
    var sum = function (num1, num2) {
        return num1 + num2;
    }
    ```

- Function构造函数定义

  - ```js
    var sum = new Function("num1", "num2", "return num1 + num2");  //不推荐
    ```

    - 这种语法会导致解析两次代码（解析一次js代码，解析一次传入的字符串代码），从而影响性能。
    - 不过这种语法对于“函数是对象，函数名是指针”的概念倒是很直观。

由于函数名仅仅是指针，所以函数名与包含函数的对象的变量名没有什么不同，或者说，一个函数可以有多个名字：

```js
function sum (num1, num2) {return num1 + num2;};
undefined
console.log(sum(10,20));
30
var anotherSum = sum;
undefined
anotherSum(10,30);
40
sum = null;
null
anotherSum(10,10);
20
```

这个过程很重要，首先定义一个名叫sum的函数，此时sum这个变量指向这个求和函数的函数体，然后运行没有问题，然后将sum赋值给anotherSum变量，就是把函数体地址给了anotherSum，此时两个变量指向同一个函数体，然后将sum变量清空，但这只是把sum和函数体的连接掐断了，并没有将函数体删除，所以anotherSum依然可以正常使用。

### 没有重载（深入理解）

在js中如果两个函数名相同，则后面的函数会将前面的函数覆盖掉，尽管它们参数不同。

### 函数声明与函数表达式

实际上，js解析器对待函数声明和函数表达式并非一视同仁。解析器会率先读取函数声明，并使其在执行任何代码前可用；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。：

```js
alert(sum(10, 10));		//20
function sum(num1, num2) {
    return num1 + num2;
}
```

以上代码完全可以正常运行。因为代码再开始执行前，解析器就已经通过函数声明提升的过程读取并将函数声明添加到执行环境中。对代码求值时，js引擎在第一遍会声明函数并将它们放到源代码树的顶部。

```js
alert(sum(10,10));  //Error
var sum = function (num1, num2) {
    return num1 + num2;
}
```

以上代码会导致错误。在第一行就导致错误，实际上也执行不到下一行。

### 作为值的函数

因为js中的函数名本身就是一个变量，所以函数也可以作为值来用。可以将一个函数当做参数传递给另一函数，也可以将一个函数作为另一个函数的结果返回：

```js
function callSomeFunction (someFunction, someArgument) {return someFunction(someArgument)};
undefined
function add10(num){return num +10};
undefined
var result1 = callSomeFunction (add10, 10);
undefined
result1
20
function getGreeting(name){return "hello," + name;}
undefined
var result2 = callSomeFunction(getGreeting, "Nicholas");
undefined
result2
"hello,Nicholas"
```

此处要注意的是，要访问函数的指针而不执行函数的话，必须去掉()。

假设有一个对象数组，我们要根据某对象的属性对数组进行排序。要解决这个问题，可以定义一个函数，它接收一个属性名，然后根据这个属性名来创建一个比较数组。

```js
function creatComparisonFunction(propertyName){ 
    return function (object1, object2) { 
        var value1 = object1[propertyName]; 
        var value2 = object2[propertyName];	
        
        return value1 - value2;
    }
};
//这里有错误，比较字符串不能相减
undefined
var data = [{name:"Zachary", age: 28}, {name:"Nicholas", age: 29}];
undefined
data.sort(creatComparisonFunction(name));
(2) [{…}, {…}]0: {name: "Zachary", age: 28}1: {name: "Nicholas", age: 29}length: 2__proto__: Array(0)
data[0]
{name: "Zachary", age: 28}
data.sort(creatComparisonFunction("name"));
(2) [{…}, {…}]
data[0]
{name: "Zachary", age: 28}
data.sort(creatComparisonFunction("age"));
(2) [{…}, {…}]
data[0];
{name: "Zachary", age: 28}//导致和上面一样。
```

````js
function creatComparisonFunction(propertyName){ 
    return function (object1, object2) { 
        var value1 = object1[propertyName]; 
        var value2 = object2[propertyName];	
        
        if(value1 < value2){
            return -1
        } else if( value1 > value2){
            return 1
        } else {
            return 0
        };
    } 
};
undefined
var data = [{name: "Zachary",age: 28}, {name: "Nicholas",age: 29}];
undefined
data.sort(creatComparisonFunction("name"));
(2) [{…}, {…}]
data[0];
{name: "Nicholas", age: 29}
data.sort(creatComparisonFunction("age"));
(2) [{…}, {…}]
data[0]
{name: "Zachary", age: 28}
"Zachary" - "Nicholas"
NaN
"Zachary" > "Nicholas"
true
````

sort()方法返回值的规则：若第一个参数应该在第二个参数前面则返回-1。

数组的sort()方法有一个小坑，排序函数我们经常返回前一个数减去后一个数，但是这只适用于数字数组，如果数字符串就会出现问题，因为两个字符串相减会返回NaN，但是可以比较大小，通过ASII码。

### 函数的内部属性

#### arguments

它是一个类数组对象，包含传入的所有参数。它有一个属性名叫callee，该属性是一个指针，指向拥有这个arguments对象的函数。

求阶乘的例子：（递归算法）

````js
function factorial(num){
    if(num <= 1) {
        return 1;
    } else {
        return num * factorial(num-1);
    }
}
````

在函数有名字且不会改变的情况下这样写完全没有问题。但问题是这个函数名与函数体紧紧地耦合在一起，为了消除耦合，可以这样：

```js
function factorial(num) {
    if(num <= 1){
        return 1;
    } else {
        return num * arguments.callee(num - 1);
    }
}
```

此处arguments.callee就代指这个函数，也就是factorial，如果函数名变也不会影响函数体。

如果重写这个函数：

```js
var trueFactorial = factorial;	//赋值给另一个变量，此时另一个变量指向那个函数体的地址
factorial = function () {		//指向一个新的函数体地址
    return 0;
}

alert(trueFactorial(5));	//120
alert(factorial(5));		//0
```

这样，无论引用函数时候用什么名字，都可以保证正常调用。

#### this

this引用的是函数执行的环境对象——或者说是this值。

```js
window.color = "red";
"red"
var o = {color: "blue"};
undefined
function sayColor(){console.log(this.color)};
undefined
sayColor();
red
o.sayColor = sayColor;
ƒ sayColor(){console.log(this.color)}
o.sayColor();
blue
```

this会在代码执行过程中引用不同的对象。

一定要牢记，函数的名字仅仅是一个包含指针的变量而已。因此，即使是在不同的环境中执行，全局的sayColor()函数与o.sayColor()指向的仍然是同一个函数。

#### caller

这个属性保存着调用当前函数的函数引用，如果是在全局作用域中调用当前函数，它的值为null：

```js
function outer() {
    inner();
}

function inner() {
    console.log(inner.caller);
}

outer();	//function outer() {inner();}
```

为了实现更松散的耦合：

```js
function outer() {
    inner();
}

function inner() {
    console.log(arguments.callee.caller);
}

outer();	//function outer() {inner();}
```

但在严格模式下，访问arguments.callee、arguments.caller都会导致错误。

严格模式下不能为函数的caller属性赋值。

### 函数属性和方法

#### length

表示函数希望接收的命名参数个数。

#### prototype

是保存它们实例方法的真正所在。

换句话说，诸如toString()和valueOf()等方法实际上都保存在prototype名下，只不过是通过 各自对象实例访问罢了。

#### apply()

每个函数都包含两个非继承而来的方法：apply()和call()。它们用来改变this指向。

apply()接收两个参数（函数运行时的作用域——this指向，给函数的参数——可选），参数可以是数组也可以是arguments对象。

```js
function sum(num1, num2){
    return num1 + num2;
}

function callSum1(num1, num2) {
    return sum.apply(this, arguments);		//传入arguments对象
    //这里就相当于把sum函数在这运行一遍，但是运行时的this指向callSum1的this，然后运行时的参数也是由apply提供，也就是callSum1的参数列表。
}

function callSum2(num1, num2) {
    return sum.apply(this, [num1, num2]);	//传入数组
}

alert(callSum1(10,10));	//20
alert(callSum2(10,10)); //20
```

此例中this指向window。

#### call()

与apply（）作用相同，区别仅仅是传参不同。

使用call()时，传递给函数的参数必须逐个列举出来：

```js
function sum(num1, num2){
    return num1 + num2;
}

function callSum1(num1, num2) {
    return sum.apply(this, num1, num2);
}

alert(callSum1(10,10));	//20
```

它们两个真正的强大之处是可以扩充函数赖以运行的作用域——也就是改变this指向。

```js
window.color = "red";
"red"
var o = {color: "blue"};
undefined
function sayColor(){
    console.log(this.color);
}
undefined
sayColor();
red
undefined
sayColor.call(this);
VM224:2 red		//
undefined
sayColor.call(window);
VM224:2 red		//
undefined
sayColor.call(o);
VM224:2 blue	//
undefined
```

#### bind()

这个方法会创建一个函数实例，其this值会被绑定到传给bind()函数的参数的值。

```js
window.color = "red";
var o = { color: "blue"};

function sayColor() {
    console.log(this.color);
}

var objectSayColor = sayColor.bind(o);
objectSayColor();		//blue
```

#### toLocaleString()

#### toString()

两者均返回函数的字符串形式。但是返回代码的格式因浏览器而异。所以无法根据此返回的结果实现任何重要功能，但是可用来调试。

#### valueOf()

返回整个函数

```js
function sayColor() {
    console.log(this.color);
}

sayColor.toString();
"function sayColor(){
    console.log(this.color);
}"

sayColor.valueOf()
ƒ sayColor(){
    console.log(this.color);
}

sayColor.toLocaleString();
"function sayColor(){
    console.log(this.color);
}"
```

## 基本包装类型

js还提供了3个特殊的引用类型：Boolean、Number和String。

实际上，每当读取一个基本类型值时，后台就创建一个对应的基本包装类型对象，从而可以调用一些方法来操作这些数据。

引用类型和基本包装类型的主要区别就是对象的生存周期：

- 使用new操作符创建的引用类型实例，在执行流离开当前作用域之前都一直保存在内存中；
- 而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即被销毁，这意味着我们不能在运行时为基本类型值添加属性和方法。

```js
var s1 = "some text";
undefined
s1.color = "red";	 //创建s1实例-->赋值-->销毁
"red"
s1.color;			//又创建s1实例，但并没有color属性，所以为undefined
undefined	
```

用代码表示此过程即为：

```js
//第三行
var s1 = new String("red");
s1 = null;
```

Object构造函数也会像工厂函数一样：

- 传入str，就创建String实例；
- 传入num，就会创建Number实例；
- 传入布尔值，就会创建Boolean实例。

要注意的是，使用new调用基本包装类型的构造函数，与直接调用同名的转型函数是不一样的。

```js
var value = "25";
undefined
var number = Number(value);
undefined
typeof number;
"number"
var obj = new Number(value);
undefined
typeof obj;
"object"
```

### Boolean类型

创建boolean对象：

```js
var booleanObject = new Boolean(true);
```

boolean类型实例重写了valueOf（）方法，返回基本类型值true或false；重写了toString（）方法，返回字符串true和false。

但是它用途不大，容易造成误解：

```js
var falseObject = new Boolean(false);
undefined
var result = falseObject && true;//这里是对falseObject而不是对它的值（false）求值，因为布尔表达式中的所有对象都会被转换为true，因此falseObject对象在布尔表达式中代表的是true。
undefined
result;
true		//所以true && true 当然是true
var falseValue = false;
undefined
result = falseValue && true;
false
result
false		//
```

基本类型与引用类型的布尔值还有两个区别：

- typeof对基本类型返回"boolean"； 对引用类型返回"object"
- instanceof对基本类型返回"false"；对引用类型返回"true"

```js
alert(typeof falseObject);				//object
alert(typeof falseValue);				//boolean
alert(falseObject instanceof Boolean);	 //true
alert(falseValue instanceof Boolean);	 //false
```

### Number类型

创建Number对象：

```js
var numberObject = new Number(10);
```

它也重写了那三个方法：

- valueOf()返回对象表示的基本类型的数值
- toString()返回字符串形式的数值
- toLocaleString()返回字符串形式的数值

#### toFixed()

按照指定的小数位返回数值的字符串表示，并且遵循四舍五入的规则，所以很适合处理货币值。

```js
var num = 10.056;
10.056
num.toFixed(2);
"10.06"
```

#### toExponential()

返回 指数表示法 表示的数值字符串形式。也接收一个参数，并且也是小数的位数。

```js
var num = 10.056;
num.toExponential(2);
"1.01e+1"
num.toExponential(3);
"1.006e+1"
```

#### toPrecision()

根据实际情况返回格式，可能是固定大小格式，也可能是指数格式，具体看哪种格式最合适。

```js
var num = 99;
undefined
num.toPrecision(1);
"1e+2"
num.toPrecision(2)
"99"
num.toPrecision(3);
"99.0"
```

但是我们仍然不建议直接实例化Number类型，原因与Boolean一样。

### String类型

创建String对象：

```js
var StringObject = new String("hello world");
```

toString)()   toLocaleString()   valueOf()均返回str：

```js
var str = new String("hello world");
undefined
str.toString()
"hello world"
str.valueOf();
"hello world"
str.toLocaleString();
"hello world"
```

#### length属性

表示字符串中有多少字符。

#### charAr() &&  charCodeAt()

用于查找字符串中某个位置的字符。

都接收一个参数，即基于0的字符位置。

**charAt()返回所查找到的字符的字符串形式；**

**charCodeAt()返回所查找到的字符编码(数字形式)。**

ES5定义了可以用方括号加数字索引来访问字符串中的特定字符。

```js
var str = "hello world";
undefined
str.charAt(4);
"o"
str.charCodeAt(4);
111
typeof str.charCodeAt(4);
"number"
str[4];
"o"
```

#### concat()方法

将一个或多个字符串拼接起来并返回新的字符串，不改变原始值。

```js
var str = "hello";
undefined
var result = str.concat(" ", "world", "!");
undefined
result
"hello world!"
```

但在实践中使用更多的还是+操作符来拼接字符串。

#### slice() && substr() && substring()

用来截取字符串，返回被操作字符串的一个子字符串。

```js
//用例子书写规则
var str = "hello world";
undefined
str.length
11
str.slice(3)			//从3到末尾
"lo world"
str.substr(3)			//从3到末尾
"lo world"
str.substring(3)		//从3到末尾
"lo world"
str.slice(3,7);			//从3到7，包括3但不包括7
"lo w"
str.substr(3,7)			//从3开始，截取7个字符
"lo worl"
str.substring(3,7)		//从3到7，包括3但不包括7
"lo w"
str.slice(-3);			//负数加上长度，到末尾
"rld"
str.substr(-3)			//负数加上长度，到末尾
"rld"
str.substring(-3);		//负数均转换为0
"hello world"
str.slice(-3, -1)		//两个负数均加上长度然后根据得数截取[ )
"rl"
str.substr(-3,-1)		//将第一个参数加上长度，第二个参数如果是负数就转换为0，（8,0）==>""
""
str.substring(-3, -7)	//两个负数都转换为0，（0，0）==>""
""
str.substring(3,-4)		//第二个数转换为0，（3,0）==>"hel" ,0>3 不包括第三个，
"hel"
str.substring(5, 3);	//证明可以反着截取str，5>3,包括第三个不包括第五个
"lo"
```

#### indexOf() && lastIndexOf()

从str中查找子str，返回开头位置，如果没找到则返回-1。

前者是返回指定字符串首次出现的位置，后者是返回指定字符串最后一次出现的位置。

```js
var str = "hello world";
undefined
str.indexOf(o);				//参数一定加""，否则报错
VM2591:1 Uncaught ReferenceError: o is not defined
    at <anonymous>:1:13
(anonymous) @ VM2591:1
str.indexOf("o");			
4
str.lastIndexOf("o");		
7
str.indexOf("a");			
-1
str.indexOf("o", 5);		//第二个参数表示查找的起始位置
7
str.lastIndexOf("o", 5);	//第二个参数表示查找的起始位置
4
```

可以使用循环找到所有匹配的子字符串：

```js
var str = "Lorem ipsum dolor sit amet, consectetur adipisicing elit";
		var positions = new Array();
		var pos = str.indexOf("i");

		while(pos > -1) {
			positions.push(pos);
			pos = str.indexOf("i", pos+1);//注意这里不能用pos++,浏览器控制台会崩溃
		}

		console.log(positions);
```

#### trim()方法

创建一个str副本，删除前后所有空格，返回结果。不改变原str

```js
var str = "   hello world!  ";
undefined
str
"   hello world!  "
str.trim();
"hello world!"
```

#### toLowerCase() && toLocaleCase()

借鉴自java。

将大写转换成小写：

```js
var str = "HELLO WORLD";
undefined
str.toLowerCase();
"hello world"
str.toLocaleLowerCase();
"hello world"
```

#### toUpperCase() && toLocaleUpperCase()

借鉴自java。

将小写转换为大写：

```js
var str = "hello world";
undefined
str.toUpperCase();
"HELLO WORLD"
str.toLocaleUpperCase();
"HELLO WORLD"
```

toLocaleLowerCase() &&toLocaleUpperCase()两个方法均是针对特定地区的实现，有些地区，针对地区的方法与通用的方法结果相同，而少数语言（如土耳其语）会为Unicode大小写转换应用特殊的规则，这时候就必须使用针对地区的方法更稳妥些。  ----locale---当地

在不知道自己的代码在哪种语言环境中运行的情况下，还是使用针对地区的方法更稳妥些。

#### match()方法

用于在str中匹配模式。（在str中查找子str，返回子str）

在str中调用这个方法，本质上与调用RegExp的exec()方法相同。

match()只接收一个参数，要么是一个正则表达式，要么是一个RegExp对象。

```js
var text = "cat, bat, sat, fat";
undefined
var pattern = /.at/g;
undefined
var matches = text.match(pattern);
undefined
matches
(4) ["cat", "bat", "sat", "fat"]0: "cat"1: "bat"2: "sat"3: "fat"length: 4__proto__
pattern = /.at/;
/.at/
matches = text.match(pattern);
["cat", index: 0, input: "cat, bat, sat, fat", groups: undefined]
matches
["cat", index: 0, input: "cat, bat, sat, fat", groups: undefined]
```

#### search()方法

也是查找str的子str，返回第一个匹配项的索引。

没找到返回-1。

一个参数就是RegExp。

```js
var text = "cat, bat, sat, fat";
undefined
var pos = text.search(/at/);
undefined
pos
1
```

#### replace()方法

在str中查找到子str，然后将它替换掉。

两个参数：正则或str、str或函数。（要想替换所有，就必须用正则+g）

```js
var text = "cat, bat, sat, fat";
undefined
var result = text.replace("at","ond");
undefined
result
"cond, bat, sat, fat"		//如果第一个参数是str，则只替换第一个
text
"cat, bat, sat, fat"
result = text.replace(/at/g, "ond");	//reg+g替换所有
"cond, bond, sond, fond"
result
"cond, bond, sond, fond"
```

第二个参数是函数的情况下：

函数的三个参数（模式的匹配项，匹配项的位置，原始字符串）

多个捕获组的情况下：

函数可以接收多个参数（模式的匹配项，第一个捕获组匹配项，第二个捕获组匹配项...，匹配项的位置，原始字符串）

这个函数返回一个字符串，表示被替换的匹配项：

```js
function htmlEscape(text){		//创建函数
    return text.replace(/[<>"&]/g, function (match, pos, originalText){//第二个参数为函数
        switch(match) {			//根据匹配项不同，替换值也不同
            case "<":
                return "&lt;";
            case ">":
                return "&gt;";
            case "&":
                return "&amp;";
            case "\"":
                return "&quot;";
        }
    });
}
undefined
htmlEscape("<p class=\"greeting\">Hello world!</p>");		//将一行HTML代码中的符号全部替换掉
"&lt;p class=&quot;greeting&quot;&gt;Hello world!&lt;/p&gt;"
```

#### split()方法

将一个str基于指定的分隔符分隔成多个子str，并将结果放在一个数组中返回出来。

接收两个参数（分隔符，指定数组大小——可选）。

分隔符可以是str或regexp：

```js
var colorText = "red, blue, green, yellow";
undefined
colorText.split(",");
(4) ["red", " blue", " green", " yellow"]
colorText.split(",", 2);
(2) ["red", " blue"]
colorText.split(/[^\,]+/);	//这个正则表示匹配多个非逗号的字符，此处是将red  bule  green  yellow 四个单词作为分隔符来分隔这个字符串
(5) ["", ",", ",", ",", ""]
colorText.split(/[^,]+/);	//不加转义字符\也无伤大雅
(5) ["", ",", ",", ",", ""]
colorText.split(/[^,]/);	//如果没有+就是每个字母都作为一个分隔符，所以如果匹配单词一定要加+
(22) ["", "", "", ",", "", "", "", "", ",", "", "", "", "", "", ",", "", "", "", "", "", "", ""]
```

#### localeCompare()方法

比较两个字符串，并返回下列值中的一个：

- [x] 如果字符串在字母表中应该排在参数字符串之前，则返回一个负数（大多数情况是-1）；
- [x] 如果二者相等，则返回0；
- [x] 如果字符串在字母表中应该排在参数字符串之后，则返回一个正数（大多数情况是1）；

```js
var str = "yellow";
undefined
str.localeCompare("red");
1
str.localeCompare("zoo");
-1
str.localeCompare("yellow");
0
```

因为localeCompare()返回的数值取决于实现，所以最好这样用：

```js
function determineOrder(value){
    var result = stringvalue.localeCompare(value);
    if(result < 0){
        console.log("big");
    } else if(result > 0) {
        console.log("smalll");
    } else {
        console.log("equal");
    }
}
undefined

var stringvalue = "yellow";
undefined
determineOrder("brick");
VM1351:6 smalll
undefined
```

localeCompare()方法比较与众不同的地方，就是实现所支持的地区决定了这个方法的行为。

#### fromCharCode()

将编码转换成str。

接收一个或多个参数——字符编码

```js
alert(String.fromCharCode(104, 101, 108, 108, 111));  //hello
```

## 单体内置对象

ECMA-262对内置对象的定义是：“有ECMAScript实现提供的、不依赖于宿主环境的对象，这些对象在ECMAScript程序执行之前就已经存在了。”

### Global对象

可以说是ECMAScript中最特别的对象了，因为你不管从什么角度看，这个对象都是不存在的。

它是一个终极的“兜底儿对象”，因为不属于任何对象的方法和属性，最终都是它的方法和属性。

事实上，没有全局变量和全局函数，所有在全局作用域中定义的属性和函数，都是Global对象的属性。比如isNaN()   parseInt()   parseFloat()等方法。

除此之外Global对象还包含其他一些方法：

#### 插话：URL和URI

这两者有什么联系又有什么区别呢？

首先，我们看图说话：

![URL&&URI](E:\TyporaFiles\img\URL&&URI.png)



根据图可以看出URI包括URL。

然后，这三者都是什么呢？

“URI可以分为URL,URN或同时具备locators 和names特性的一个东西。URN作用就好像一个人的名字，URL就像一个人的地址。换句话说：URN确定了东西的身份，URL提供了找到它的方式。”

下面到了回答问题的时候了：

当我们替代web地址的时候，URI和URL那个更准确？

基于我读的很多的文章，包括RFC，我想说URI更准确。

别急，我有我的理由：

我们经常使用的URI不是严格技术意义上的URL。例如：你需要的文件在`files.hp.com`. 这是URI，但不是URL--系统可能会对很多协议和端口都做出正确的反应。

你去`http://files.hp.com` 和`ftp://files.hp.com`.可能得到完全不同的内容。这种情况可能更加普遍，想想不同谷歌域名上的不同服务啊。

所以，用URI吧，这样你通常技术上是正确的，URL可不一定。最后“URL”这个术语正在被弃用。所以明智吧少年！



#### encodeURI() && encodeURIComponent()方法

对URI（通用资源标识符）进行编码，将无效的字符转换掉，以便发送给浏览器。

它们的区别是：

- encodeURI()主要用于整个URI；
- 而encodeURIComponent()主要对于URI中的某一段进行编码。
- encodeURI()不会对本身属于URI的特殊字符进行编码，比如冒号、正斜杠、问号和井字号。
- 而encodeURIComponent()会对它发现的任何非标准字符进行编码。

```js
var uri = "http://www.wrox.com/illengal value.htm#start";
undefined
encodeURI(uri);
"http://www.wrox.com/illengal%20value.htm#start"				//只编码了空格
encodeURIComponent(uri)
"http%3A%2F%2Fwww.wrox.com%2Fillengal%20value.htm%23start"		//编码了所有非标准字符
```

#### decodeURI() && decodeURIComponent()方法

作用是将编码后的URI进行解码。

而且是与上面两个方法一一对应的：

- decodeURI()只能对使用encodeURI()替换的字符进行解码。例如，它可将%20解码为一个空格，但对%23不作任何处理。
- decodeURIComponent()只能对encodeURIComponent()替换的字符进行解码。

encode——编码；  decode——解码

```js
var uri = "http%3A%2F%2Fwww.wrox.com%2Fillengal%20value.htm%23start";
undefined
decodeURI(uri);
"http%3A%2F%2Fwww.wrox.com%2Fillengal value.htm%23start"
decodeURIComponent(uri);
"http://www.wrox.com/illengal value.htm#start"
```

#### Unicode编码 && ASCII编码 && utf-8编码

最早只有127个字母被编码到计算机里，也就是大小写英文字母、数字和一些符号，这个编码表被称为ASCII编码，比如大写字母A的编码是65，小写字母z的编码是122。

但是要处理中文显然一个字节是不够的，至少需要两个字节，而且还不能和ASCII编码冲突，所以，中国制定了GB2312编码，用来把中文编进去。

你可以想得到的是，全世界有上百种语言，日本把日文编到Shift_JIS里，韩国把韩文编到Euc-kr里，各国有各国的标准，就会不可避免地出现冲突，结果就是，在多语言混合的文本中，显示出来会有乱码。

因此，Unicode应运而生。Unicode把所有语言都统一到一套编码里，这样就不会再有乱码问题了。

Unicode标准也在不断发展，但最常用的是用两个字节表示一个字符（如果要用到非常偏僻的字符，就需要4个字节）。现代操作系统和大多数编程语言都直接支持Unicode。

新的问题又出现了：如果统一成Unicode编码，乱码问题从此消失了。但是，如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算。

所以，本着节约的精神，又出现了把Unicode编码转化为“可变长编码”的UTF-8编码。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间。

UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。

#### eval()方法

现在，我们介绍最后一个——大概也是整个ECMAScript语言中最强大的一个方法：eval() 。

它就像是一个完整的ECMAScript解析器，它只接收一个参数，即要执行的js代码的字符串形式。

```js
eval("alert('hi')");   //等价于  alert("hi");
```

当解析器发现eval()方法时，会将其参数当做js插入到原位置，被执行的代码具有与该执行环境相同的作用域链。这意味着通过eval()可以引用在包含环境中的变量。举个例子：

```js
var msg = "hello world";
undefined
eval("console.log(msg)");
VM1905:1 hello world		//可以访问到方法外面的变量
undefined

eval("function sayHi() { console.log('hi');}");
undefined
sayHi()		//可以访问到方法内部的函数。
VM1909:1 hi
undefined
```

但是在eval()中创建的任何变量或函数都不会被提升，因为在解析代码的时候，它们被包含在一个str中，它们只在eval()执行的时候创建。

严格模式下，在外部访问不到eval()内部的变量或函数，前面的例子会导致错误。同样，为eval赋值也会导致错误。

eval()非常强大，但也非常危险。使用时必须极为谨慎，特别是在执行用户输入的时候，要防止恶意代码注入。

#### Global对象的属性

| 属性                    | 说明     |
| ----------------------- | -------- |
| undefined               | 特殊值   |
| NaN                     | 特殊值   |
| Infinity                | 特殊值   |
| Object                  | 构造函数 |
| Array                   | 构造函数 |
| Function                | 构造函数 |
| Boolean                 | 构造函数 |
| String                  | 构造函数 |
| Number                  | 构造函数 |
| Date                    | 构造函数 |
| RegExp                  | 构造函数 |
| Error                   | 构造函数 |
| EvalError               | 构造函数 |
| RangeError_范围错误     | 构造函数 |
| ReferenceError_引用错误 | 构造函数 |
| SyntaxError_语法错误    | 构造函数 |
| TypeError               | 构造函数 |
| URIError                | 构造函数 |

#### 插话：window和Global

Global:

Global对象是单体内置对象，即不依赖于宿主环境的对象，这些对象在ECMAScript程序执行之前就已经存在了，就是一切全局里存在的变量和函数都是它的属性和方法，即“终极老祖宗”。也就是那些在宿主环境（常见的宿主环境：浏览器和nodejs）里所有的内建或自定义的变量和函数全局都是Global这个的全局对象和方法。它更像是一个抽象概念，而要指明它是什么，取决于程序在什么环境中运行。（例如：js不仅可以书写页面，在书写页面中，global相对于浏览器这个环境而言就是window，但是其他环境就不一定了）

浏览器把Global对象作为window对象的一部分实现了，因此，所有的全局属性和函数都是window对象的属性和方法。

Window:

window对象是相对于Web浏览器而言的，它并不是ECMAScript规定的内置对象，它是浏览器的Web API,是存在于浏览器之中的，也就是离开浏览器这个宿主环境的话就不存在此对象了。

所以说Global对象是包含于window对象这种情况的。

#### window对象

ECMAScript虽然没有指出如何直接访问Global对象，但web浏览器都是将这个全局对象作为window对象的一部分加以实现的。因此，所有的全局属性和函数都是window对象的属性和方法。

```js
var color = "red";
undefined
function sauyColor() {
    console.log(window.color);
}
undefined
window.sauyColor();		//说明color是window的属性，sauyColor()是window的方法
VM2151:2 red
undefined
```

另一种取得Global对象的方法：

```js
var global = function (){ return this}();//创建一个立即调用的函数，返回this的值，如果没有给函数明确指定this值的情况下，this指定的值等于Global对象。 	
undefined
global
Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}//这里面包含非常多的属性和方法

window
Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}

this
Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
```

### Math对象

#### math对象的属性

| 属性         | 说明                    |
| ------------ | ----------------------- |
| Math.E       | e的值，即自然对数的底数 |
| Math.LN10    | 10的自然对数            |
| Math.LN2     | 2的自然对数             |
| Math.LOG2E   | 以2为底e的对数          |
| Math.LOG10E  | 以10为底e的对数         |
| Math.PI      | π的值                   |
| Math.SQRT1_2 | 1/2的平方根             |
| Math.SQRT2   | 2的平方根               |

#### min() && max()方法

用于确定一组数值中的最小值和最大值。

可以接收任意多个参数。

```js
var max = Math.max(23, 34, 14, 56, 10);
undefined
max			//输出最大值
56

var min = Math.min(23, 34, 14, 56, 10);
undefined
min			//输出最小值
10
```

要想找数组中的最大值和最小值，可以这样：

```js
var arr = [12, 23, 34, 45, 46, 566];
undefined
var max = Math.max.apply(Math,arr);//apply(运行的作用域——this指向，参数数组)，从而将this指向Math，然后可以将任何数组作为第二参数
undefined
max
566
```

#### ceil() && floor() && round()方法

- [ ] Math.ceil() ——向上舍入
- [ ] Math.floor()——向下舍入
- [ ] Math.round()——四舍五入

````js
Math.ceil(25.6)
26
Math.floor(25.6)
25
Math.round(25.6);
26
Math.round(25.4)
25
````

#### random()方法

返回一个[0, 1)的随机数。

语法：

> 值 = Math.floor(Math.random() * 可能值的总数 + 第一个可能的值)

如果你想选择一个1到10之间的随机数：

```js
var num = Math.floor(Math.random() * 10 + 1);  //从1开始，十个数字，就是1~10+1-1
```

通过一个函数计算：

```js
function selectFrom(lowerValue, upperValue){
    var choies = upperValue - lowerValue + 1;
    return Math.floor(Math.random() * choies + lowerValue);
}

var num = selectFrom(2, 10);
console.log(num);		//输出一个随机数
```

随机输出数组中的某个元素：

```js
var colors = ["red", "green", "yellow", "blue", "white"];
var color = colors[selectFrom(0, colors.length - 1)];
console.log(color);
```

#### 其他方法

| 方法                | 说明                |
| ------------------- | ------------------- |
| Math.abs(num)       | 返回num的绝对值     |
| Math.exp(num)       | 返回Math.E的num次幂 |
| Math.log(num)       | 返回num的对数       |
| Math.pow(num,power) | 返回num的power次幂  |
| Math.sqrt(num)      | 返回num的平方根     |
| Math.acos(x)        | 返回x的反余弦值     |
| Math.asin(x)        | 返回x的反正弦值     |
| Math.atan(x)        | 返回x的反正切值     |
| Math.atan2(y,x)     | 返回y/x的反正切值   |
| Math.sin(x)         | 返回x的正弦值       |
| Math.cos(x)         | 返回x的余弦值       |
| Math.tan(x)         | 返回x的正切值       |

这些方法在不同的实现中可能会有不同的精度。

# 第六章 面向对象的程序设计

面向对象语言的标志就是都有类的概念。

## 理解对象

### 创建对象

```js
var person = new Object();
undefined
person.nane = "Nicholas";
"Nicholas"
person.age = 29;
29
person.job = "Software Engineer";
"Software Engineer"
person.sayName = function () {
    console.log(this.name);
}
ƒ () {
    console.log(this.name);
}
//但这样创建太麻烦了，更多的是用对象字面量的方法创建对象：
undefined
var person = {
    name : "Nicholas",
    age: 29,
    job: "Software Engneer",
    
    sayName: function () {
        console.log(this.name);
    }
}
undefined
```

### 属性类型

只有js内部才用的特性，描述了属性的各种特征，这些特征是为了实现js引擎用的，因此在js中不能直接访问它们。

为了表示特性是内部值，该规范把它们放在[ [ ] ]中。

#### 数据属性

包含一个数据的位置，它有4个特性：

- [ [ Configurable ] ]: 表示能否通过delete删除属性从而重新定义属性，能否修改属性。直接在对象上定义的属性默认值为true
- [ [ Enumerable ] ]: 表示能够通过for-in循环返回属性。直接在对象上定义的属性默认值为true
- [ [ Writable ] ]: 表示能否修改属性的值。默认为true
- [ [ Value ] ]: 包含这个属性的数据值。从这读取数据，写入数据。默认值为undefined

```js
var person = {
	name: "Nicholas"		//[[Value]]特性将被设置为"Nicholas",而对这个值的任何修改都将反应在这个位置
}
```

##### Object.defineProperty()

用来修改属性的默认特性。

接收3个参数（属性所在对象，属性名，要修改的属性及值——可以是数组的形式一次修改多个属性值）。

```js
var person = {};
undefined
Object.defineProperty(person, "name", {
    writable: false,	//表示不能修改此属性值，是只读的
    value: "Nicholas"
});
{name: "Nicholas"}
person.name
"Nicholas"
person.name = "Greg";	//此处试图修改
"Greg"
person.name
"Nicholas"				//但失败了
```

类似的：

```js
var person = {};
undefined
Object.defineProperty(person, "name", {
    configurable: false,			//设置configurable属性为false,不可配置
    value: "Nicholas"
});
{name: "Nicholas"}

person.name
"Nicholas"

person.name = "Greg";		//试图修改属性值
"Greg"

person.name					//但失败了
"Nicholas"

Object.defineProperty(person, "name", {
    configurable: true,	//试图将configurable属性设置回true
    value: "Nicholas"
});
{name: "Nicholas"}

person.name = "Greg";	//试图修改属性值
"Greg"

person.name				//依然失败了，因为configurable的值是单向设置的，一旦把属性改为不可配置的，就不能再改回去了。
"Nicholas"
```

这说明，可以多次调用Object.defineProperty()方法修改同一个属性，但是把configurable特性设置为false后就会有限制了。

多数情况下可能都没必要利用Object.defineProperty()方法提供的这些高级功能。不过理解这些概念对理解js对象很有用。

#### 访问器属性

它不包含数据值。具有以下4个特性：

- [ [ Configurable ] ]: 表示能否通过delete删除属性并重新定义，能否修改属性。直接在对象上定义的属性默认true
- [ [ Enumerable ] ]: 表示能否通过for-in循环返回属性。直接在对象上定义的属性默认true
- [ [ Get ] ]: 在读取属性时调用的函数。默认undefined
- [ [ Set ] ]: 在写入属性时调用的函数。默认undefined

访问器属性不能直接写入，必须使用Object.defineProperty()来定义。

```js
var book = {
    _year: 2018,	//_表示只能通过对象方法访问的属性
    edition: 9
};
undefined
Object.defineProperty(book, "year", {//year表示访问器属性，_year则为数据属性
    get: function () {			//get
        return this._year;
    },
    set: function (newValue) {		//set
        if(newValue > 2018){
            this._year = newValue;
            this.edition += newValue-2018;
        }
    }
});
{_year: 2018, edition: 9}
book.year = 2019;		//改变year触发set函数（此处是year而不是_year）
2019
book
{_year: 2019, edition: 10}	//对应的值随之改变
book._year = 2000;		//改变_year并不会触发set函数
2000
book
{_year:2000,edition: 10}  //对应的edition值也就不会发生改变
```

### 定义多个属性

#### Object.defineProperties()方法

用来为对象一次定义多个属性。

接收两个参数（对象，要添加或修改的属性）

```js
var book = {};
undefined
Object.defineProperties(book, {
    _year: {//数据属性
        writable: true,	//值是否可修改，默认也为true
        value: 2018
    },
    edition: {//数据属性
        writable: true,
        value: 9
    },
    year: {//访问器属性
        get: function () {
            return this._year;
        },
        set: function (newValue) {
            if(newValue > 2018){
                this._year = newValue;
                this.edition += newValue-2018;
            }
        }
    }
});
{_year: 2018, edition: 9}
book.year = 2019;//访问器属性修改
2019
book
{_year: 2019, edition: 10}//其他两个属性随之修改
```

### 读取属性的特性

#### Object.getOwnPropertyDescriptor()方法

用来取得给定属性的描述符（特性）。

接收两个参数（属性所在的对象，属性名称）。

返回一个对象，如果是访问器属性，就返回那四个属性和值所组成的对象，如果是数据属性，就返回那四个属性和值所组成的对象。

```js
var book = {};		//创建对象	
undefined
Object.defineProperties(book, {		//添加属性，属性的特性以及数据
    _year: {
        writable: true,	
        value: 2018
    },
    edition: 
        writable: true,
        value: 9
    },
    year: {
        get: function () {
            return this._year;
        },
        set: function (newValue) {
            if(newValue > 2018){
                this._year = newValue;
                this.edition += newValue-2018;
            }
        }
    }
});
{_year: 2018, edition: 9}

var descriptor = Object.getOwnPropertyDescriptor(book, "_year");//用此方法赋值给变量
undefined
descriptor
{value: 2018, writable: true, enumerable: false, configurable: false}//但此处默认的特性值都是false

descriptor = Object.getOwnPropertyDescriptor(book, "year");//其他的属性默认的特性值也都是false
{get: ƒ, set: ƒ, enumerable: false, configurable: false}

descriptor = Object.getOwnPropertyDescriptor(book, "edition");
{value: 9, writable: true, enumerable: false, configurable: false}
```

然后

```js
ar obj ={name: "Nicholas"};
undefined
Object.getOwnPropertyDescriptor(obj, "name");
{value: "Nicholas", writable: true, enumerable: true, configurable: true}//事实证明，字面量方法直接创建的属性的其三个特性值默认都是true
Object.defineProperties(obj, {//用defineProperties()方法添加属性
    age: {
        value: 18,
        writable:true
    }
});
{name: "Nicholas", age: 18}
Object.getOwnPropertyDescriptor(obj, "age");
{value: 18, writable: true, enumerable: false, configurable: false}//而用defineProperties()方法添加的属性其特性值默认为false。
```

## 创建对象

### 工厂模式

它是软件工程领域一种广为人知的设计模式，这种模式抽象了创建具体对象的过程。

由于在ECMAScript中无法创建类，开发人员就发明一种函数，用来封装以特定接口创建对象的细节。

```js
function createPerson(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function (){
        console.log(this.name);
    }
    return o;
}
undefined
var person1 = createPerson("Nicholas", 29, "Software Engineer");
undefined
var person2 = createPerson("Greg", 22, "poctor");
undefined
person1
{name: "Nicholas", age: 29, job: "Software Engineer", sayName: ƒ}age: 29job: "Software Engineer"name: "Nicholas"sayName: ƒ ()__proto__: Object
person2
{name: "Greg", age: 22, job: "poctor", sayName: ƒ}
```

工厂模式解决了创建多个相似对象的问题，但是对象识别的问题（即怎样知道一个对象的类型）。

### 构造函数模式

上例可以写成：

```js
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function () {
        console.log(this.name);
    };
};
undefined
var person1 = new Person("Nicholas", 29, "Software Engineer");
undefined
person1
Person {name: "Nicholas", age: 29, job: "Software Engineer", sayName: ƒ}

person1.constructor		//constructor属性指向Person
ƒ Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function () {
        console.log(this.name);
    };
}

person1 instanceof Object	//检测对象类型
true
person1 instanceof Person
true
```

按照惯例，**构造函数应以大写字母开头**。

构造函数本身也是函数，只不过用来创建对象了而已。

构造出来的对象都有一个constructor属性，指向构造它的函数，即Person。

**创建自定义的构造函数意味着将来可以将它的实例标识一种特定的类型；而这正是构造函数模式胜过工厂模式的地方。**

构造函数与其他函数不同的地方就是调用方式不同。

任何函数，只要通过new操作符来调用，那就可以作为构造函数。

构造函数不通过new调用，那就是普通函数。



构造函数模式缺点：**使用构造函数时每个方法都要在每个实例上重新创建一遍。**（隐形创建，明面上看不出来，也就是说实例里面的函数并不是使用的构造函数的，而是复制一份放到自己这里，所以每个实例里面的同名函数是不相等的）

ECMAScript中函数是对象，因此每定义一个函数就是实例化了一个对象。

从逻辑角度讲，此时的构造函数也可以这样定义：

```js
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = new Function ("console.log(this.name)");  //与声明函数在逻辑上是等价的
}
```

从这个角度来看构造函数，更容易明白每个Person实例都包含一个不同的Function实例的本质。

因此，**不同实例上的同名函数是不相等的。**

```js
person1.sayName == person2.sayName;
false
```

然而，创建两个同样作用的函数实例的确没必要；况且有this对象在，根本不用在执行代码之前就把函数绑定到特定的对象上面。

因此，可以这样解决这个问题：

```js
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = sayName;
}

function sayName(){			//将函数写到外面。
    console.log(this.name);
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 22, "poctor");
```

两个实例共享同一个函数。

确实解决了多个函数做同一件事的问题。可是新问题又来了：在全局作用域中的函数只能被某个对象调用，这让全局作用域有点名不副实，而且，如果对象需要定义很多方法，那就要在全局定义很多方法，于是我们自定义的引用类型就丝毫没有封装性可言了。

好在，这些问题可以通过使用原型模式来解决：

### 原型模式

我们创建的每个函数都有一个prototype（原型）属性，是一个指针，指向一个对象。

**{constructor指向它的构造函数，prototype指向它的原型}**

prototype就是通过构造函数创建的实例的原型对象，使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。	换句话说，不用再构造函数中定义对象实例的信息，而是直接添加到原型对象中。

```js
function Person() {};
undefined
Person.prototype.name = "Nicholas";
"Nicholas"
Person.prototype.age = 29;
29
Person.prototype.job = "Software Engineer";
"Software Engineer"
Person.prototype.sayName = function () {console.log(this.name);};
ƒ () {console.log(this.name);}

var person1 = new Person();
undefined

person1
Person {}__proto__: age: 29job: "Software Engineer"name: "Nicholas"sayName: ƒ ()constructor: ƒ Person()__proto__: Object

person1.age
29
```

#### 理解原型对象

创建一个新函数的时候，新函数就会有一个prototype属性，指向函数的原型对象。

**默认情况下，所有原型对象都有一个constructor属性，指向prototype所在函数（构造函数）**。 其他方法都是从Object上继承来的。

要明确的真正重要的一点是：  **prototype是连接于实例与原型对象之间的，而不是实例与构造函数之间。**

![yuanxing](E:\TyporaFiles\img\yuanxing.png)

Person的每个实例person1和person2都包含一个内部属性——[ [ prototype ] ]，该属性仅仅指向了Person.prototype；换句话说，它们与构造函数没有直接关系。

##### isPrototypeOf()

判断实例的原型。

```js
alert(Person.prototype.isPrototypeOf(person1));   //true
alert(Person.prototype.isPrototypeOf(person2));   //true
```

这里都返回true，因为它们内部都有一个指向Person.prototype的指针。

##### Object.getPrototypeOf()

返回对象的[ [ Prototype ] ]（原型），这对利用原型实现继承非常重要。

```js
alert(Object.getPrototypeOf(person1) == Person.prototype);   //true
alert(Object.getPrototypeOf(person1).name);  		//"Nicholas"
```

##### 属性搜索顺序

当代码读取某个对象的某个属性时，都会去搜索它的值。

- 首先从对象本身开始，找到了就返回，没找到就往上找原型对象
- 然后在原型对象中找到了，则返回值。

也就是说要执行两次搜索。

这正是多个对象实例共享原型所保存的属性和方法的基本原理。



虽然可以从对象实例中访问原型的值，但不可重写。

如果我们在对象实例中添加了一个和原型中重名的属性或方法，该属性或方法会屏蔽掉原型中的，也就是优先级比原型中的更高。

```js
function Person() {};
undefined
Person.prototype.name = "Nicholas";
"Nicholas"

var person1 = new Person();
undefined
person1.name
"Nicholas"

person1.name = "Greg";
"Greg"
person1.name
"Greg"

var person2 = new Person();
undefined
person2.name
"Nicholas"
```

添加这个属性只会阻止我们访问原型中的那个同名属性，但不会修改那个属性。

使用delete操作符可以完全删除实例属性，从而重新访问到原型上面的属性。

```js
//接上面代码
delete person1.name
true

person1.name
"Nicholas"
```

##### hasOwnProperty()

**检测一个属性存在实例对象中还是存在于原型中**。它是从Object中继承而来。

它只在给定属性（参数）存在于对象实例中时，才返回true。

```js
function Person () {};
undefined
Person.prototype.name = "Nicholas";
"Nicholas"
Person.prototype.age = 29;
29
var person1 = new Person();
undefined
var person2 = new Person();
undefined
person1.job = "Software Engineer";
"Software Engineer"
person1.hasOwnProperty("name");	//存在于原型对象中的属性返回false
false
person1.hasOwnProperty("job");//存在于实例对象中的属性才返回true
true
person2.hasOwnProperty("age");
false
```

##### 原型与in操作符

有两种方式使用in操作符：单独使用和for-in循环使用。

单独使用时，它会在通过对象能访问给定的属性时返回true，无论该属性存在于实例中还是原型中。

```js
function Person () {};

Person.prototype.name = "Nicholas";

Person.prototype.age = 29;

var person1 = new Person();
29
person1.hasOwnProperty("name");//用来做比较
false
"name" in person1			//来自原型的属性
true
person1.job = "Software Engineer";
"Software Engineer"
person1.hasOwnProperty("job");
true
"job" in person1			//来自实例的属性
true
```

在使用for-in循环时，返回的是所有能够通过对象访问的、可枚举的属性，包括实例中的和原型中的。

**屏蔽了原型中不可枚举属性（即将[ [ Enumerable ] ]标记为false的属性）的   <u>实例属性</u>  也会在for-in循环中返回。**

意思就是在原型中的不可枚举的属性名，在实例中也有一个同名的实例属性，这个实例属性正常返回，不受原型中的属性影响。

因为根据规定，所有开发人员定义的属性都是可枚举的——只有在IE8及更早的版本中例外。

```js
var o = {
    toString: function () {					//原型中的toString是不可枚举的，在实例中写一个重名方法
        return "My toString";
    }
}

for(var prop in o){
    if(prop == "toString"){
        console.log("Found toString");		//Found toString，说明并不受原型中的属性名影响。
    	//IE8及更早不会显示这条
    }
}
```

##### Object.key()方法

用来取得对象上所有可枚举的**实例属性**。

接收一个对象作为参数。

返回一个包含所有可枚举属性的字符串数组。

```js
function Person() {};
undefined
Person.prototype.name = "Nicholas";
"Nicholas"
Person.prototype.age=29;
29
Person.prototype.job = "Software Engineer";
"Software Engineer"
var keys = Object.keys(Person.prototype);
undefined
keys
(3) ["name", "age", "job"] 				//在原型上调用，返回原型中可枚举的属性组成的str数组

var person1 = new Person();
undefined
person1.name = "Greg";
"Greg"

person1.age = 18;
18
Object.keys(person1);				//在实例对象上调用，返回实例中可以枚举的属性组成的str数组
(2) ["name", "age"]
```

##### Object.getOwnPropertyNames()

返回实例的所有属性组成的str数组，无论它是否可枚举。

传递一个对象。

```js
//接上面
Object.getOwnPropertyNames(person1);//实例的属性
(2) ["name", "age"]
Object.getOwnPropertyNames(Person);//构造函数的属性
(5) ["length", "name", "arguments", "caller", "prototype"]
Object.getOwnPropertyNames(Person.prototype);//原型的属性，constructor属性是不可枚举的，但是也会被抓出来
(4) ["constructor", "name", "age", "job"]
```

#### 更简单的原型语法

前面每给原型添加一个属性或方法都要敲一遍Person.prototype。

为减少不必要的输入，也为了视觉上更好的封装原型的功能。

更常见的做法是用一个对象字面量来**重写**整个原型对象。

**最终结果与之前的方法相同，但有一个例外：constructor属性不再指向Person了，而是Object构造函数**。

因为这里本质上是完全**重写了原型对象**，也就改变了constructor属性的指向：

```js
function Person () {};
undefined
Person.prototype = {
    name : "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
{name: "Nicholas", age: 29, job: "Software Engineer", sayName: ƒ}

Person.prototype.constructor
ƒ Object() { [native code] }

var person1 = new Person();
undefined
person1.constructor == Object	//指向Object（从实例上访问原型的属性）
true
person1.constructor == Person	//不指向Person
false

Person.prototype.constructor == Person
false
Person.prototype.constructor == Object
true
```

如果constructor的值真的很重要，可以向这样特意设置回适当的值：

```js
function Person () {};
    
undefined
Person.prototype = {
    constructor : Person,		//在这里特意写一个constructor属性指向Person构造函数
    name : "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
{constructor: ƒ, name: "Nicholas", age: 29, job: "Software Engineer", sayName: ƒ}
var person1 = new Person();
undefined
person1.constructor == Person		//true 
true
person1.constructor					//Perosn() {}
ƒ Person () {}
```

注意：以这种方式重设constructor属性会导致它的[ [ Enumerable ] ]（是否可以for-in枚举）特性被设置为true，默认为false。

```js
//接上面
for(var prop in person1){
    console.log(prop);
}
VM1632:2 constructor		//constructor被枚举出来了
VM1632:2 name
VM1632:2 age
VM1632:2 job
VM1632:2 sayName
undefined
```

然后可以调用Object.defineProperty()方法将属性的特性设置回去：

```js
Object.defineProperty(person1,"constructor",{	//将constructor属性的特性改回去
    enumerable: false,
    value :Person
});

Person {constructor: ƒ}
for(var prop in person1){
    console.log(prop);
}
VM1803:2 name				//此处就不枚举出来constructor属性了
VM1803:2 age
VM1803:2 job
VM1803:2 sayName
undefined

Object.defineProperty(Person.prototype, "constructor",{		//和上面的方法一样，都是更改的都是同一个constructor属性
    enumerable: false,
    value: Person
});
{name: "Nicholas", age: 29, job: "Software Engineer", sayName: ƒ, constructor: ƒ}

for(var prop in person1){
    console.log(prop);
}
VM1977:2 name				//此处就不枚举出来constructor属性了
VM1977:2 age
VM1977:2 job
VM1977:2 sayName
undefined
```

**这里呢，个人感觉如果想用字面量的方法为原型中添加属性或方法，可以直接字面量表示，然后用Object.defineProperty()方法将constructor属性指向Person，同时将enumerable特性设置为false，代码如下：**

```js
function Person () {};	

Person.prototype = {
    name : "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
{name: "Nicholas", age: 29, job: "Software Engineer", sayName: ƒ}

Object.defineProperty(Person.prototype,"constructor",{
    enumerable: false,
    value: Person
});
{name: "Nicholas", age: 29, job: "Software Engineer", sayName: ƒ, constructor: ƒ}

var person1 = new Person();
undefined
person1.constructor			//constructor属性指向Person
ƒ Person () {}

person1.constructor == Person
true
person1.constructor == Object
false

for(var prop in person1){	
    console.log(prop);
}
VM2394:2 name		//而且并不能枚举出来constructor属性，说明constructor属性的enumerable特性值为false
VM2394:2 age
VM2394:2 job
VM2394:2 sayName
undefined
```

#### 原型的动态性

由于在原型中查找是一次搜索，因此我们对原型做任何修改都能立即从实例上表现出来——即使是先创建实例后修改原型。

```js
//接上面
var person2 = new Person();

Person.prototype.address  = "USA";

person2.address
"USA"
```

但是重写整个原型，情况就不一样了。

**调用构造函数创建实例时会为实例添加一个[ [ Prototype ] ]指针指向原型对象，但是重写原型就相当于将原型修改为另一个对象 ，也就切断了构造函数与最初原型之间的联系。**

请记住：**实例中的指针仅指向原型，而不指向构造函数**。

```js
function Person () {};

var friend = new Person();

Person.prototype = {
    name : "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};

friend.sayName();   //Error	  因为friend指向的原型中不包含这个属性。
```

![重写原型](E:\TyporaFiles\img\重写原型.png)

#### 原生对象的原型

所有的原生引用类型（Object、Array、String，等）都是采用这种原型模式创建的。

例如，在Array.prototype中可以找到sort()方法。

```js
typeof Array.prototype.sort
"function"
typeof String.prototype.substring
"function"
```

可以再原生对象的原型中添加新方法。

```js
String.prototype.sayName = function () {
    console.log("I'm String sayName function ");
}
ƒ () {
    console.log("I'm String sayName function ");
}

var str = new String();
undefined
str.sayName();
VM2635:2 I'm String sayName function 
undefined
```

尽管可以这样做，但不推荐。

**可能会导致命名冲突，和意外的重写原生的方法。**

#### 原型对象的问题

原型模式的最大问题是由其共享的本质所导致的。

```js
function Person () {};	

Person.prototype = {
    name : "Nicholas",
    age: 29,
    friend: ["Shelby", "Court"],	//在原型上新加一个friend属性
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
{name: "Nicholas", age: 29, friend: Array(2), job: "Software Engineer", sayName: ƒ}

var person1 = new Person();			//定义两个实例
undefined
var person2 = new Person();
undefined
person1.friend.push("Van");			//为person1添加一个朋友
3
person2.friend						//但是person2的friend属性也会改变，因为两个共享同一个原型
(3) ["Shelby", "Court", "Van"]
person1.friend == person2.friend	//两者的friend属性是同一个
true
```

实例一般都是要有属于自己的全部属性的，而这个问题正是很少有人单独使用原型模式的原因所在。

### 构造函数模式 + 原型模式

这是创建自定义类型的最常见方式。

**构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性**。

这最大限度的节省了内存。

这种混成模式还支持向构造函数传递参数，可谓是集两种模式之长。

下面的代码重写了前面的例子：

```js
function Person (name, age, job){	//实例属性写这里
    this.name = name;
    this.age = age;
    this.job = job;
    this.friend = ["Shelby", "Court"];
}
undefined
Person.prototype = {			//	方法和共享属性写这里
    constructor: Person,
    sayName: function () {
        console.log(this.name);
    }
};
{constructor: ƒ, sayName: ƒ}
var person1 = new Person("Nicholas", 29, "Software Engineer");
undefined
var person2 = new Person("Greg", 27, "Doctor");
undefined
person1.friend.push("Van");
3
person1.friend
(3) ["Shelby", "Court", "Van"]
person2.friend
(2) ["Shelby", "Court"]
person1.friend == person2.friend
false
person1.sayName === person2.sayName
true
```

**这是使用最广泛、认同度最高的一种创建自定义类型的方法。**

### 动态原型模式

用于解决独立使用构造函数和原型的问题。

它把所有的信息都封装在构造函数中，通过构造函数初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。

```js
function Person(name, age, job){
    //属性
    this.name = name;
    this.age = age;
    this.job = job;
    
    //方法   这里只有在sayName()方法不存在的情况下才会将它添加到原型中。这段代码只在初次调用函数时才会执行，此后，原型已经初始化完成，不需要再做什么修改了。不过要记住，这里对原型所做的修改，能够立即在所有实例中得到反映。
    if (typeof this.sayName != "function"){
        Person.prototype.sayName = function () {//这里不能使用对象字面量形式重写原型，如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系。
            console.log(this.name);
        }
    };
}
undefined
var friend = new Person("Nicholas", 29, "Software Engineer");
undefined

friend.sayName()
VM4061:10 Nicholas
```

### 寄生构造函数模式

通常在前述的几种模式都不适合的情况下，可以使用这种模式。

这种模式的基本思想就是创建一个函数，该函数的作用仅仅是封装创建对象的代码。然后再返回新的对象。表面上很像典型的构造函数。

```js
function Person(name, age, job){
    var o = new Object();		//创建一个对象
    o.name = name;		//添加属性
    o.age = age;
    o.job = job;
    o.sayName = function () {
        console.log(this.name);
    };
    return o;			//返回这个对象
}
undefined
var friend = new Person("Nicholas", 29, "Software Engineer");//创建一个新实例
undefined
friend.sayName();
VM4448:7 Nicholas
undefined
```

除了使用new操作符并把使用的包装函数叫构造函数之外，和工厂模式其实是一模一样的。

这个模式可以在特殊的情况下用来为对象创建构造函数。

假设我们想创建一个具有额外的方法的特殊数组。由于不能直接修改Array构造函数，因此可以使用这种方式：

```js
function SpecialArray() {
    //创建数组
    var values = new Array();
    
    //添加值
    values.push.apply(values, arguments);//修改this值指向values
    
    //添加方法
    values.toPidpedString = function () {
        return this.join("|");
    };
    
    //返回数组
    return values;
};
undefined
var colors = new SpecialArray("red", "green", "blue");
undefined
colors
(3) ["red", "green", "blue", toPidpedString: ƒ]
colors.toPidpedString();
"red|green|blue"
```

需要说明的是：

首先，**返回的对象与构造函数和构造函数的原型属性之间没有任何关系**。也就是说，构造函数返回的对象与在外面创建的对象没有什么不同。

为此，不能依赖instanceof操作符来确定对象类型。（instanceof对基本类型返回"false"；对引用类型返回"true"）

由于存在上述问题，建议能使用其它模式就不使用这种模式。

### 稳妥构造函数模式

所谓稳妥对象，指的是没有公共属性，而且其方法也不引用this对象。

最适合在一些（禁止使用this和new）安全环境中运行。

```js
function Person (name,age,job){
    
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function () {
        console.log(name);  //没有使用this
    };
    return o;
};

var friend = Person("Nicholas", 29, "Software Engineer");
undefined
friend.sayName()
VM345:8 Nicholas
undefined
```

只有sayName（）方法可以访问name的值。

即使以后向里面添加属性或方法，但也不可能有其他方法可以访问其原始数据。

这种安全性，使得它非常适合在某些安全执行环境——例如ADsafe（净网大师）和Caja（前端环境安全检测）提供的环境下使用。

与寄生构造函数模式类似，稳妥构造函数模式构造出来的对象和构造函数也没有什么关系，因此instanceof对这种对象也没有意义。

## 继承

许多语言都有两种继承方式：接口继承和实现继承。

接口继承只继承方法签名，而实现继承则继承实际的方法。

**由于ECMAScript中的函数没有签名，所以无法实现接口继承**。

**ECMAScript只支持实现继承，而且主要以原型链实现的。**

### 原型链

原型链是实现继承的主要方法。

其基本思想是利用原型让一个引用类型继承另一个引用类型的方法。

回顾一下构造函数、实例与原型的关系：

​	每个构造函数都有一个原型对象（prototype），而每个原型对象都有一个指向构造函数的指针（constructor），每个实例也都有一个指向原型对象的指针（[ [ Prototype ] ]）。

如果让一个原型指向另一个原型的实例，结果会怎样呢？

​	此时的原型对象就包含指向另一个原型对象的指针（[ [ Prororypt ] ] ），而且，另一个原型也包含着指向另一个构造函数的指针（constructor）。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条，也就是所谓的**原型链的基本概念**。

实现原型链有一个基本模式：

```js
function SuperType() {		//一个构造函数
    this.property = true;
}
undefined
SuperType.prototype.getSuperValue = function (){//一个构造函数的方法
    return this.property;
};
ƒ (){
    return this.property;
}

function SubType() {		//另一个构造函数
    this.subproperty = false;
};
undefined

SubType.prototype = new SuperType();//另一个构造函数的原型指向一个构造函数的实例，也就相当于重写了原型对象，代之以一个新类型的实例
SuperType {property: true}

SubType.prototype.getSubValue = function () {//另一个构造函数的方法
    return this.subproperty;
}
ƒ () {
    return this.subproperty;
}

var instance = new SubType();//另一个构造函数的实例
undefined
instance.getSubValue();		//调用原型上的方法
false
instance.getSuperValue();	//调用原型链上的（也就是继承来的）方法
//这里原则上会经历三个搜索步骤：搜索实施--> 搜索SubType.prototype -->搜索SuperType.prototype，这样一环一环的向上搜索。
true

instance.constructor	//要注意，这里了的constructor指向了SuperType，因为原来的SubType.prototype中的constructor被重写了的缘故。
ƒ SuperType() {
    this.property = true;
}
```

#### 别忘记默认的原型

事实上上面的搜索步骤还少一环，那就是Object.prototype，因为所有函数默认都是Object实例。

这也正是所有自定义类型都会继承toString(), valueOf())等默认方法的根本原因。

上例的完整原型链：

![原型链](E:\TyporaFiles\img\原型链.png)

一句话，SubType继承了SuperType，SuperType继承了Object。

（部分与书上不符，此图完全按照浏览器反馈制作，但是和书上的意思一样，这样看更清楚其中的关系）

#### 确定原型和实例的关系

两种方式：

- 使用instanceof操作符，只要测试的是实例与其原型链中出现的构造函数，就返回true

  - ```js
    alert(instance instanceof Object);  //true
    alert(instance instanceof SubType); //true
    alert(instance instanceof SuperType);//true
    ```

  - 由于原型链的关系，我们可以说instance是它们三个任意一个的实例。因此都返回true。

- 使用isPrototypeOf()，同样，只要测试的是实例与其原型链中出现的构造函数，就返回true。

  - ```js
    alert(Object.prototype.isPrototypeOf(instance));   //true
    alert(SubText.prototype.isPrototypeOf(instance));  //true
    alert(SuperText.prototype.isPrototypeOf(instance));//true
    ```

#### 谨慎的定义方法

子类有时候需要在超类中覆盖或者添加方法，一定要放在替换原型的语句之后，不然会随着原型被重写（继承）直接被弄丢。

```js
function SuperType() {
    this.property = true;
};

SuperType.prototype.getSuperValue = function () {
    return this.property;
};
ƒ () {
    return this.property;
}
function SubType() {
    this.subproperty = false;
}
undefined
//继承
SubType.prototype = new SuperType();
SuperType {property: true}
//添加新方法，必须在用SuperType实例替换SubType原型之后，在定义这两个方法
SubType.prototype.getSubValue = function () {
    return this.subproperty;
};
ƒ () {
    return this.subproperty;
}
//重写方法，放在继承后面
SubType.prototype.getSuperValue = function () {
    return false;
};
ƒ () {
    return false;
}
var instance = new SubType();
undefined
instance.getSuperValue();//调用的是重写的方法
false
instance.getSubValue();
false
```

还有一点需要注意，在通过原型链实现继承时，不能使用对象字面量方法创建原型对象，这样会重写原型链。

```js
function SuperType() {
    this.property = true;
};

SuperType.prototype.getSuperValue = function () {
    return this.property;
};
ƒ () {
    return this.property;
}
function SubType() {
    this.subproperty = false;
}
undefined
//继承
SubType.prototype = new SuperType();
SuperType {property: true}

//使用字面量方式创建方法，会使上一行代码无效
SubType.prototype = {
    getSubValue : function () {
        return this.subproperty;
    }
}
var instance = new SubType();
undefined
instance.getSuperValue();//因为原型链已经被重写
error!!!

```

#### 原型链的问题

最主要的问题来自包含引用类型值的原型。

```js
function SuperType () {
    this.color = ["red","green"];
}
undefined
function SubType() {};
undefined
SubType.prototype = new SuperType();
SuperType {color: Array(2)}

var instance1 = new SubType();
undefined
instance1.color.push("blue");//给instance1的color添加一个blue
3
instance1.color
(3) ["red", "green", "blue"]

var instance2 = new SubType();
undefined
instance2.color
(3) ["red", "green", "blue"]	//但是新创建的实例的color也会多个blue，因为两个实例公用的是同一个原型中的属性
```

第二个问题是，在创建子类实例时，不能向超类的构造函数中传递参数。

所以实践中很少会单独使用原型链。

### 借用构造函数

基本思想就是在子类的构造函数中调用超类的构造函数。

别忘了，函数只不过是在特定环境中执行代码的对象，因此可以通过call()和apply()方法在新创建的对象上执行构造函数。

```js
function SuperType () {
    this.color = ["red","green"];
};
undefined
function SubType() {
    SuperType.call(this);			//在SubType中执行构造函数SuperType，并且改变this指向
};
undefined
SubType.prototype = new SuperType();
SuperType {color: Array(2)}
var instance1 = new SubType();
undefined
instance1.color.push("blue");
3
instance1.color
(3) ["red", "green", "blue"]
var instance2 = new SubType();
undefined
instance2.color				//此时每个实例都有自己的color属性，互不干扰
(2) ["red", "green"]
```

而且可以传参：

```js
function SuperType(name) {
    this.name = name;//在全局定义的函数，this就是指向window，不管你在哪用，执行的时候都会到这里来执行，this依然是window，所以说下面的call()改变this指向很有必要
};
undefined
function SubType() {
    SuperType.call(this,"Nicholas");//在内部调用构造函数时，实际上是为SubType()定义了name属性。
    this.age = 29;
};
undefined
SubType.prototype = new SuperType();
SuperType {name: undefined}
var instance1 = new SubType();
undefined
instance1.name
"Nicholas"
instance1.age
29
```

### 组合继承（最常用）

有时候也叫伪经典继承，指的是将原型链和借用构造函数组合到一起，从而发挥二者之长。

**其背后的思想就是使用原型链实现对原型的属性和方法的继承，使用借用构造函数实现对实例属性的继承。**

```js
function SuperType(name) {
    this.name = name;
    this.color = ["green", "blue"];
};
undefined
SuperType.prototype.sayName = function () {
    return this.name;
};
ƒ () {
    return this.name;
}
function SubType(name, age) {
    //继承实例属性
    SuperType.call(this, name);
    //写自己的实例属性
    this.age = age;
};
undefined
//原型链继承，继承原型链的属性及方法
SubType.prototype = new SuperType();
SuperType {name: undefined, color: Array(2)}
SubType.prototype.constructor = SubType;//如果不这样，constructor就指向着SuperType
ƒ SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}
SubType.prototype.sayAge = function () {
    return this.age;
};
ƒ () {
    return this.age;
}
//创建实例
instance1 = new SubType("Nicholas", 29);
SubType {name: "Nicholas", color: Array(2), age: 29}
instance1.color
(2) ["green", "blue"]
instance1.color.push("red");
3
instance1.color
(3) ["green", "blue", "red"]
instance1.sayName();
"Nicholas"
instance1.sayAge();
29
var instance2 = new SubType("Greg", 27);
undefined
instance2.color		//证明两个实例的属性互不干扰，而且方法共用的原型链中的方法，也互不干扰
(2) ["green", "blue"]
instance2.sayName();
"Greg"
instance2.sayAge();
27
```

**使之成为了JavaScript中最常用的继承模式。**

### 原型式继承

这种方法没有使用严格意义上的构造函数。

其主要思想如下：

```js
function object(o){
   	function F() {};//创建一个临时的构造函数
    F.prototype = o;//实现继承
    return new F();//返回了一个由传入参数为原型的构造函数实例
}
```

#### Object.create()

ECMAScript5通过新增的Object.create()方法规范化了原型式继承。

接收两个参数（用作新对象原型的对象，为新对象定义额外属性的对象——可选）。而且每个属性都是通过自己的描述符定义的。

```js
var person = {
    name : "Nicholas",
    friends: ["A","B","C"]
};
undefined
var anotherPerson = Object.create(person, {
    name: {
        value: "Greg"
    },
    age: {
        value: 27
    }
});
undefined
anotherPerson
{name: "Greg", age: 27}
//展开
age: 27name: "Greg"
__proto__: friends: (3) ["A", "B", "C"]name: "Nicholas"__proto__: Object

anotherPerson.friends
(3) ["A", "B", "C"]
```

在没有必要兴师动众的创建构造函数，只是想让两个对象保持类似关系的情况下，原型式继承完全可以胜任。

不过别忘了，包含引用类型值的属性始终都会共享其相应的值，就像使用原型模式一样。

### 寄生式继承

寄生式继承是与原型式继承紧密相关的一种思路。

即创建一个仅用于封装继承过程的函数，在内部以某种方式增强对象，然后返回对象。

```js
function createAnother(original) {
    var clone = object(original);  //上面封装的原型式继承
    //clone是以original为原型的实例对象
    //object不是必须的，任何能够返回新对象的函数都适用
    clone.sayHi = function () {    //以某种方式增强这个对象
        alert("Hi");
    };
    return clone;				//返回这个对象
}

var person = {
    name: "Nicholas",
    friend: ["A","B","C"];
}

var anotherPerson = createAnother(person);	//调用函数创建一个实例对象

anotherPerson.sayHi();
"Hi"
```

在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式

### 寄生组合式继承（最理想）

前面说组合继承是最常用的继承模式；不过，他也有自己的不足，组合继承最大的问题就是无论什么情况下，都要调用两次超类的构造函数。

```js
function SuperType(name) {
    this.name = name;
    this.color = ["green", "blue"];
};
undefined
SuperType.prototype.sayName = function () {
    return this.name;
};
ƒ () {
    return this.name;
}
function SubType(name, age) {
    SuperType.call(this, name);//第二次调用SuperType()
    this.age = age;
};
undefined

SubType.prototype = new SuperType();//第一次调用SuperType()
SuperType {name: undefined, color: Array(2)}
SubType.prototype.constructor = SubType;
```

所谓寄生组合式继承，即通过借用构造函数来继承属性，也就是第二次调用不变，然后通过原型链的混成形式来继承方法。

本质上就是使用寄生式继承来继承超类的原型，然后再将结果指定给子类型的原型。

基本模式如下：

```js
//原型式继承
function object(o){
   	function F() {};//创建一个临时的构造函数
    F.prototype = o;//实现继承
    return new F();//返回了一个由传入参数为原型的构造函数实例
}

function inheritPrototype(subType, superType){//参数：（子类型构造函数，超类型构造函数）
    var prototype = object(superType.prototype);//创建一个以superType.prototype为原型的实例
    prototype.constructor = subType;//修正constructor指向
    subType.portotype = prototype;//将实例赋值给SubType的原型
    
    //个人理解也可以这样写
    subType.prototype = object(superType.prototype);
    subType.prototype.constructor = subType;
}
```

```js
//接上面
function superType (name) {
    this.name = name;
    this.color = ["red","green"];
}
superType.prototype.sayName = function () {
    alert(this.name);
}

function subType(name, age) {
    superType.call(this, name);
    this.age = age;
}

inheritPrototype(subType, superType);
//一定要记住它要放在它下面
subType.prototype.sayAge = function () {
    alert(this.age);
}

var instance = new SubType("Nicholas", 29);	//然后就可以用了
SubType {name: "Nicholas", color: Array(2), age: 29}
instance.sayName();
"Nicholas"
```

这个例子的高效率体现在它只调用了一次SuperType构造函数。

可以正常使用instanceof和isPrototypeOf()。

**开发人员普遍认为寄生组合式继承是引用类型最理想的继承模式。**

# 第七章 函数表达式

firefox  Safari   Chrome和Opera都给函数定义了一个非标准的name属性，通过这个属性可以访问到给函数指定的名字。这个属性的值永远等于跟在function关键字后面的标识符。

```js
//只有在firefox  Safari   Chrome和Opera有效
function functionName() {};
undefined
functionName.name
"functionName"
```

函数声明提升：

在执行代码之前会先读取函数声明，所以可以将函数声明放在调用它的语句后面。

```js
sayHi();
function sayHi() {
    alert("Hi");
}
```

但是用函数表达式创建的函数不能这样。因为它没有声明哈哈哈。

```js
var functionName = function (arg) {
    
};
```

这样创建的函数叫匿名函数（有时候也叫拉姆达函数），因为function关键字后面没有标识符。

函数表达式与其他表达式一样，在使用前必须先赋值。以下代码会导致错误

```js
sayHi();//导致错误，实际上执行到这就停止了
var sayHi = function() {
    alert("Hi");
}
```

理解函数提升关键，就是理解函数声明与函数表达式之间的区别。

来看以下例子：

```js
//不要这样做，这在ECMAScript中属于无效语法（2020/2/10/15:57在Chrome亲测这有效）
if(condition){
    function sayHi() {
        alert("Hi");
    }
} else {
    function sayHi() {
        alert("No");
    };
}

//可以这样做，不同的函数会根据condition被赋值给sayHi
var sayHi;

if(condition){
    sayHi = function () {
        alert("Hi");
    }
} else {
    sayHi = function () {
        alert("No");
    };
}
```

能够创建函数再赋值给变量，也能够把函数作为其他函数的值返回。

比如第五章的那个比较函数：

```js
function createComparisonFunction(propertyName){ 
    return function (object1, object2) { //这里返回的就是一个匿名函数
        var value1 = object1[propertyName]; 
        var value2 = object2[propertyName];	
        
        if(value1 < value2){
            return -1
        } else if( value1 > value2){
            return 1
        } else {
            return 0
        };
    } 
};
```

## 递归（改良）

通过函数调用自身：

```js
function factorial(num) {
 	if(num <= 1)
        return 1;
    return num * factorial(num - 1);
}
//这是一个典型的递归阶乘函数，看起来没什么问题，但是下面的代码会导致它出错：

var anotherFactorial = factorial;
factorial = null;
anotherFactorial(4);   //Error!
//由于阶乘函数中必须执行factorial()，而它现在已经不是函数了，所以就会导致错误

//这种情况下可以用arguments.callee解决
//我们知道，arguments.callee是一个指向正在执行的函数的指针
//所以可以这样重写factorial()函数：
function factorial(num) {
    if(num <= 1)
        return 1;
    return num * arguments.callee(num - 1);
}

//但在严格模式下，不能通过脚本访问arguments.callee，访问这个属性会导致错误。
//不过，可以使用命名函数表达式来达到相同的效果
var factorial = (function F(num) {
    if(num <= 1)
        return 1;
    return num * F(num - 1);
})//亲测这里不加括号也有效
//以上代码创建了一个名为F 的命名函数表达式，然后将它赋值给了变量factorial。即便把函数值赋给了另一个变量，F()仍然有效，所以递归调用照样能完成，这种方式在严格模式和非严格模式下都行得通
```

## 闭包

匿名函数和闭包容易搞混，要明白二者的区别。

**闭包是指有权访问另一个函数的作用域的变量的函数。**

创建闭包的常用方式，就是在一个函数中创建另一个函数。

依然以第五章的createComparisonFunction() 为例：

```js
function createComparisonFunction(propertyName){ 
    return function (object1, object2) { //这里返回的就是一个匿名函数
        //这两行代码访问了外部的变量porpertyName。即使这个内部函数被返回了，而且在其他地方调用了，但它仍然可以访问变量propertyName变量。 之所以能够访问到，是因为内部函数的作用域链中包含createComparisonFunction()的作用域。
        var value1 = object1[propertyName]; //
        var value2 = object2[propertyName];	
        
        if(value1 < value2){
            return -1
        } else if( value1 > value2){
            return 1
        } else {
            return 0
        };
    } 
};
```

第四章介绍了作用域链，而有关如何创建作用域链以及作用域链有什么作用的细节，对于理解闭包至关重要。

当某个函数被调用时，会创建一个执行环境及相应的作用域链。然后，使用arguments和其他命名参数的值来初始化函数的活动对象。但在作用域链中，外部函数的活动对象始终处于第二位，外部函数的外部函数的活动对象处于第三位... ... 直至作为作用域链终点的全局执行环境。

在函数执行过程中，为读取和写入变量的值，就需要在作用域链中查找变量。

其实，作用域链的本质是一个指向变量对象的指针列表，它只引用但不实际包含变量对象。

一般来讲，当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域。但是闭包的情况有所不同。

**当createComparisonFunction()函数执行完毕返回匿名函数后，其执行环境的作用域链会被销毁，但是它的活动对象仍然会留在内存中；直到匿名函数被销毁后，createComparisonFunction()函数的活动对象才会被销毁。**

````js
//创建函数
var compareNames = createComparisonFunction("name");
//调用函数
var result = compareNames({name: "Nicholas"},{name: "Greg" });
//解除对匿名函数的引用
compareNames = null
//通知垃圾回收例程将其清除，随着匿名函数的作用域链被销毁，其他作用域（除了全局作用域）也都可以安全的销毁了，所以，闭包用完之后最好手动清除其内存
````

![闭包](E:\TyporaFiles\img\闭包.png)

由于闭包会携带包含它的函数的作用域，因此会比其他的函数占用更多的内存。过度使用闭包会导致内存占用过多，所以建议只在绝对必要的地方使用闭包。

### 闭包与变量

作用域链的这种机制有一个副作用，就是闭包只会保存包含函数中任何变量的最后一个值。别忘了闭包所保存的是整个变量对象，而不是某个特殊的变量。

```js
function createFunction() {
	var result = new Array();
    
    for(var i=0; i<10; i++){
        result[i] = function () {
            return i;
        }
    }
    
    return result;
};
undefined

createFunction();
(10) [ƒ, ƒ, ƒ, ƒ, ƒ, ƒ, ƒ, ƒ, ƒ, ƒ]		//每个函数都返回10
```

用立即执行函数解决：

```js
function createFunction() {
    var result = new Array();
    
    for(var i=0; i<10; i++){
        result[i] = (function (num) {
            return i;
        })(i);
        //我们没有直接将闭包给数组，而是用一个立即执行函数执行代码，然后将结果赋值给数组
    }
    return result;
}
undefined
createFunction();
(10) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### 关于this对象

this是在运行时基于函数的运行环境绑定的。

**匿名函数的执行环境具有全局性，因此this对象通常指window。**来看下例：

```js
var name = "The Window";
undefined
var obj = {
    name: "My Object",
    getNameFunc: function () {
        return function () {
            return this.name;
        };
    }
}
undefined
obj.getNameFunc() 		//一个小错误，这样调用会把闭包函数返回出来，而不是结果
ƒ () {
            return this.name;
        }
obj.getNameFunc()()		//要两个括号
"The Window"			//在非严格模式下
```

函数在被调用的时候都会自动取得this和arguments值。内部函数在搜索这两个值时，只会搜索到其活动对象为止，永远不可能直接访问外部函数的这两个变量。（这也就是上例中的this保存的是window而不是obj的原因）

不过，如果把外部函数的this保存到闭包能访问到的变量里，就可以让闭包访问该对象了。

```js
var name = "The Window";
undefined
var obj = {
    name: "My Object",
    
    getNameFunc: function () {
        var _this = this;//将当前的this值用变量保存下来，闭包可以访问到这个变量
        return function () {
            return _this.name;
        };
    }
}
undefined
obj.getNameFunc()()
"My Object"
```

this和arguments存在着同样的问题，如果想访问作用域中的arguments对象，必须将该对象的引用保存到闭包能访问到的变量中。

一个小例子：

```js
var name = "The Window";
undefined
var obj = {
    name: "My Object",
    
    getNameFunc: function () {//这里并没有形成闭包，所以this指向正常
        return this.name;
    }
}
undefined
obj.getNameFunc()()
"My Object"
```

### 内存泄漏

什么是内存泄漏？

​	就是由于闭包引用其所在函数的活动对象，导致其所在函数中的元素一直不被销毁，导致内存一直得不到回收。

如：

```js
function assignHander() {
    var div = document.getElementById("div");
    div.click = function () {
        alert(div.id);//循环引用问题
    }
}
```

这个例子中，只要有匿名函数在，div的引用数至少也是1，所以它占用的内存永远也得不到回收。

可以修改一下代码来解决：

```js
function assignHander() {
    var div = document.getElementById("div");
    var divId = div.id;//将要用的副本保存到变量中，并且在闭包引用该变量并不会出现循环引用。
    div.click = function () {
        alert(divId);
    }
    div = null;//这里要记住：闭包会引用包含函数的整个活动对象，而其中包含着div。即使闭包不直接引用div，但其活动对象中也仍然保存着一个引用。因此，有必要把div变量设置为null。通知垃圾回收机制释放其内存。
}
```

### 模仿块级作用域

匿名函数可以模仿块级作用域。

用作块级作用域的匿名函数（通常称为私有作用域）的匿名函数的语法如下：

```js
(function () {
    //这里是块级作用域
})();
```

**将函数声明包含在一对圆括号中，表示它实际上是一个函数表达式**（这里为什么要变成函数表达式呢？因为function关键字是函数声明的开始，而函数声明后面不能跟括号）。而紧随其后的圆括号会立即调用这个函数表达式。这就是立即执行函数的原理。

变量只是值的另一种表现形式。

无论在什么地方，只要临时需要一些变量，就可以使用私有作用域。

私有作用域里面的变量执行结束后就被销毁，在外面访问不到。

这种技术经常用在全局作用域中被用在函数外部，从而限制向全局作用域中添加过多的变量和函数。

大型的程序中，过多的全局变量和函数很容易导致命名冲突。而通过创建私有作用域，每个开发人员既可以使用自己的变量，又不必担心搞乱全局作用域。

```js
(function () {
    var now = new Date();
    if(now.getMonth() == 4 && now.getDate() == 27){
        alert("Happy new Day!");
    }
})();
```

也可以减少闭包占用的内存问题。

### 私有变量

任何在函数中定义的变量都是私有变量，因为不能再外部访问到。

私有变量包括函数的参数、局部变量和在函数内部定义的其他函数。

```js
function MyObject() {
    //私有变量和私有函数
    var privateVariable = 10;
    
    function privateFunction () {
        return false;
    }
    
    //特权方法
    this.publicMethod = function () {
        privateVariable ++;
        return privateFuction ();
    }
}
//创建MyObject后，除了publicMethod()这一途径外，没有任何办法可以直接访问那两个私有变量和函数
```

**特权方法**：是有权访问私有变量和函数的公有方法称为特权方法。（通过闭包实现）

通过私有和特权成员，可以隐藏那些不应该被直接修改的数据：

```js
function Person(name) {
    this.getName = function () {
        return name;
    }
    
    this.setName = function (value) {
        value = name;
    }
}

var person = new Person("Nicholas");
person.getName()
"Nicholas"
person.setName("Greg");
person.getName()
"Greg"
```

以上定义了两个特权方法getName()  setName()，私有变量name在Person的每个实例中都不同，因为每次调用都会重建这两个方法。但是构造函数有缺点，那就是针对每个实例都会创建同样的一组新方法。

而使用静态私有变量来实现在特权方法就可以避免这个问题。

### 静态私有变量

通过私有作用域中定义私有变量或函数。

```js
(function () {
    //私有变量和私有函数
    var privateValue =10;
    
    function privateFunction() {
        return false;
    }
    
    //构造函数
    MyObject = function (){//未使用var，因此它是全局变量，能够在外面访问到（但在严格模式下导致错误）
    }
    
    //特权方法
    MyObject.prototype.publicMethod = function () {//通过原型实现方法复用，由实例共享
        privateValue ++;
        return privateFunction();
    };
})();
```

### 模块模式

为单例创建私有变量和特权方法。

单例——只有一个实例的对象：

```js
var singleton = {
    name: value,
    method: function () {
        //method
    }
}
```

在web开发中，经常需要使用一些单例来管理应用程序级的信息。

```js
//创建一个管理自检的application对象的单例
var application = function () {
    //私有变量和函数
    va components = new Array();
    
    //初始化
    compoents.push(new BaseComponent());
    
    //公共
    return {//返回包含闭包的对象
        getComponentCount: function () {
            return components.length;//返回组件数目
        },
        registerComponent: function (component) {
            if(typeof component == "object"){
                components.push(component);//注册新组件
            }
        }
    }
}();
```

简言之，如果必须创建一个对象并以某些数据对其初始化，同时还要公开一些能够访问这些私有数据的方法，那么就可以使用模块模式。

### 增强的模块模式

进一步改进的模块模式，即在返回对象之前加入对其增强的代码。

改良上例：

````js
var application = function () {
    
    var components = new Array();
    
    components.push(new BaseComponent());
    
    //创建application的一个局部副本
    var app = new BaseComponent();
    
    //公共接口
    app.getComponentCount = function () {
        return components.length;
    }
    app.registerComonent: function (component) {
        if(typeof component == "object"){
            components.push(component);
        }
    }
    
    //返回副本，给application
    return app;
}
````

# 第八章 BOM



## window对象

BOM的核心对象是window，它表示浏览器的一个实例。在浏览器中，它既是通过js访问浏览器的接口，又是ECMAScript中的Global对象。这意味着在网页中定义的任何一个对象、变量或函数，都以window作为其Global对象。因此有权访问parseInt()等方法。

### 全局作用域

由于window扮演着ECMAScript中的global对象，所以在全局中定义的变量、函数都会变成window的属性或方法。

````js
var age = 19;
undefined

function sayAge() {
    console.log(this.age);
}
undefined

sayAge()
VM534:2 19
undefined
````

定义全局变量和在window上定义属性还是有一点差别的：全局变量不能通过delete删除，而window的属性可以。

```js
var age = 29;//这样定义在window上的属性有一个名为[ [ Configurable ] ]的特性，值为false，因此不可通过delete删除
undefined
window.color = "red";
"red"

delete window.age;
false

age
29

delete age
false

age
29

delete window.color
true

window.color//尝试访问未声明的变量会抛出错误，但是通过查询window对象，可以知道某个可能未声明的变量是否存在
undefined
```

**尝试访问未声明的变量会抛出错误，但是通过查询window对象，可以知道某个可能未声明的变量是否存在。**

```js
var newValue = oldValue;//报错，因为oldValue未定义
VM828:1 Uncaught ReferenceError: oldValue is not defined
    at <anonymous>:1:16
(anonymous) @ VM828:1//不报错，这里是一次属性查询
var newValue = window.oldValue;
undefined
```

### 窗口关系及框架

如果页面中包含框架，则每个框架都有自己的window对象，并且保存在frames集合中。

在frames中可以通过数值索引（从0开始，从上到下，从左到右）或者框架名称来访问相应的window对象。

每个window对象都有name属性，其包含框架名称。

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<frameset rows="160,*">
		<frame src="frame.html" name="topFrame">
		<frameset cols="50%,50%">
				<frame src="anotherframe.html" name="leftFrame">
				<frame src="yetanotnerframe.html" name="rightFrame">	
		</frameset>
	</frameset>
</body>
</html>
```

对于这个例子而言，可以使用window.frames(0)或window.frames["topFrame"]来引用上方框架。不过最好使用top而非window来引用这些框架。

#### top

**top对象始终指向最外层框架——浏览器窗口**。使用它可以确保在一个框架中正确的访问另一个框架。因为对于框架中的代码来说，window对象指向的都是按个框架的window，而非最外层的window。

所以，可以使用top.frames(0)或top.frames["topFrame"]来引用上方框架。

#### parent

**parent对象始终指向当前框架的直接上层框架。**

在某些情况下parent=top。

在没有框架的情况下，parent == top == window。

注意，除非最高层窗口是通过window.open()打开的，否则其window的name属性不会包含任何值。

#### self

**它始终指向window**，实际上self和window可以互换使用。引用self的目的只是为了与top和parent对应起来，因此它不格外包含任何值。

所有这些对象都是window的属性，可以通过window.top  window.parent形式来访问。同时，这也意味着可以将不同层次的window对象连缀起来，例如：window.parent.parent.frames[0]。

在每个框架中定义的全局属性或函数都会成为你框架中window对象的属性或方法。

### 窗口位置

Safari、chrome、IE、Opera提供screenLeft和screenTop属性表示浏览器窗口相对于屏幕左边和上边的位置。

Firefox提供screenX和screenY属性表示浏览器窗口相对于屏幕左边和上边的位置。Safari和chrome也支持。

所以使用以下代码来解决浏览器之间的兼容性问题：

```js
var leftPos = (typeof window.screenLeft == "number") ? window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ? window.screenTop : window.screenY;
```

由于种种原因，无法在跨浏览器的情况下取得窗口左边和上边的精确坐标。

然而，使用moveTo()和moveBy()方法倒是有可能将窗口精确的移动到一个新位置。

#### moveTo() && moveBy()

**将窗口精确的移动到一个新位置。**

接收两个参数： 其中，moveTo()接收的是新位置x和y 的坐标值，而moveBy()接收的是在水平和垂直方向上移动的像素数。

```js
//将窗口移动到屏幕左上角
window.moveTo(0,0);
//将窗口向下移动100像素
window.moveBy(0,100);
//将窗口移动到（200,300）
window.moveTo(200,300);
//将窗口向左移动50px
window.moveBy(-50,0);
```

需要注意的是，这两个方法可能会被浏览器禁用。在Opera和IE7及更高版本默认就是禁用的。另外这两个方法都不适用于框架，只能对最外层的window对象使用。

### 窗口大小 

跨浏览器确定一个窗口大小不是一件简单的事。

五大浏览器均为此提供了4个属性：innerWidth、innerHeight、outerWidth、outerHeight，用来表示窗口尺寸或视口大小。

在五大浏览器中，document.documentElement.clientWidth和document.documentElement.clientheight中保存了页面视口的信息。在IE6中这些只有在标准模式下有效，如果是混杂模式，就必须通过document.body.clientWidth和document.body.clientHeight取得相同的信息。

虽然最终无法确定浏览器窗口大小，但可以取得页面视口大小：

```js
var pageWidth = window.innerWidth;
var pageHeight = window.innerHeight;
//为低版本IE做兼容
if(typeof pageWidth != "number"){//如果上述未能取得有效值
    if(document.compatMode == "CSS1Compat"){//判断是不是标准模式
        pageWidth = document.documentElement.clientWidth;
        pageHeight = document.documentElement.clientHeight;
    } else {//混杂模式
        pageWidth = document.body.clientWidth;
        pageHeight = document.body.clientHeight;
    }
}
```

由于PC设备和移动设备有差异，要先检测一下用户是否在使用移动设备，然后再决定使用哪个属性。

有关移动设备视口的话题比较复杂，如果你做移动端开发，推荐阅读：http://t.cn/zOZs0Tz  or   https://quirksmode.org/mobile/viewports2.html

#### resizeTo() && resizeBy()

调整浏览器窗口大小。

接收参数看例子：

```js
//调整到100*100
window.resizeTo(100, 100);
//调整到200*150
window.resizeBy(100, 50);
//调整到300*300
window.resizeTo(300, 300);
```

需要注意的是，这两个方法可能会被浏览器禁用。在Opera和IE7及更高版本默认就是禁用的。另外这两个方法都不适用于框架，只能对最外层的window对象使用。

### 导航和打开窗口

#### window.open()

用于导航到一个特定的URL，也可以打开一个新的浏览器窗口。

接收4个参数（要加载的URL，窗口目标，一个特性字符串，一个表示新页面是否取代当前页的布尔值）。

通常只传递第一个参数。

- 如果传递了第二个参数，而且该参数是已有窗口或框架的名称，那么就会在具有该名称的窗口或框架中加载第一个参数指定的URL。

  - ```js
    window.open("http://www.baidu.com", "topFrame");
    //等同于
    <a href="http://www.baidu.com" target="topFrame"></a>
    ```

- 第三个参数是一个逗号分隔的设置字符串，表示新窗口的特性。

  - ```js
    window.open("http://www.baidu.com", "topFrame","height=400,width=400,top=10,left=10,resizeable=yes");
    ```

    具体特性详见P200

window.open()方法会返回一个指向新窗口的引用。

```js
var wroxWin = window.open("http://www.baidu.com", "topFrame","height=400,width=400,top=10,left=10,resizeable=yes");

//调整大小
worxWin.resizeTo(500,500);

//移动位置
worxWin.moveTo(100,100);

//关闭窗口
worxWin.close();//仅适用于window.open()打开的页面。

//浏览器主窗口没有用户许可是无法关闭的，但是弹窗可以调用top.close()在不经用户允许下关闭自己。弹窗关闭后引用还在，但除了检测其closed属性外没有任何作用
console.log(worxWin.closed);
```

新创建的window对象有一个opener属性，其中保存着打开它的原始窗口对象。

```js
alert(worxWin.opener == window);  //true
```

有些浏览器会在独立的进程中运行每个标签页。如果两个标签页之间有通信，就不能各自在一个进程中运行，如果新创建的标签页的opener属性值设置为null，即表示在新进程中运行。

```js
var wroxWin = window.open("http://www.baidu.com", "topFrame","height=400,width=400,top=10,left=10,resizeable=yes");

worxWin.opener = null;//无需与原标签页通信，运行在新进程中
```

#### 弹出窗口屏蔽程序

大多数浏览器都内置弹出窗口屏蔽程序，没有内置程序的也可以安装屏蔽工具。

弹出窗口被屏蔽时有两种可能：

- 如果是浏览器内置程序屏蔽了弹窗，那么window.open()很可能返回null。
- 如果是其他程序屏蔽了弹窗，那么window.open()通常抛出错误。

因此，要想准确的检测出弹出窗口是否被屏蔽，必须在检测返回值的同时，将对window.open()的调用封装在一个try--catch块中：

````js
var blocked = false;

try{
    var worxWin = window.open("http://www.baidu.com", "_blank");
    if(worxWin == null){
        blocked = true;
    }
} catch (ex){
    blocked = true;
}

if(blocked){
    alert("The popup was blocked!");
}
````

在任何情况下，以上代码都可以显示弹窗是否被屏蔽了，但要注意，它只是检测，并不会阻止浏览器显示与被屏蔽的弹出窗口有关的消息。

### 间歇调用 && 超时调用（2种定时器）

#### setTimeout()——超时调用

过指定时间后执行一次代码。

接收两个参数：（要执行的代码，以毫秒表示的时间）。

```js
//不建议使用str
setTimeout("alert('hello world!)", 1000);

//推荐的调用方式
setTimeout(function () {
    alert("hello world!");
}, 1000);
```

如果时间是空的，代码则立即执行。

调用setTimeout()会返回一个ID，表示超时调用，可以通过它来取消超时调用，用clearTimeout()。

```js
var timeoutId = setTimeout(function () {
    alert("hello world!");
}, 1000);

//取消
clearTimeout(timeoutId);
```

#### setInterval()——间歇调用

它会按照指定的时间重复执行代码，直到被取消或关闭页面。

参数：（要执行的代码，以毫秒表示的时间）。

它同样会返回一个ID，表示间歇调用，可以通过它用clearTimeout（）来取消调用。

```js
var num = 0;
var max = 10;
var intervalId = null;

function incrementNumber() {
    num ++;
    if(num == max){
        clearInterval(intervalId);
        alert("Done");
    }
}

intervalId = setInterval(incrementNumber,1000);
```

#### 超时调用模拟间歇调用

```js
var num = 0;
var max = 10;

function incrementNumber() {
    num ++;
    
    if(num < max){
        setTimeout(incrementNumber, 500);//调用自己
    } else {
        alert("Done");
    }
}
```

一般认为，用超时调用来模拟间歇调用是一种最佳方式，在开发环境下，很少使用真正的间歇调用，原因是后一个间歇调用可能会在前一个间歇调用前启动。上例完全避免了这个问题。

但是这句话又要怎样理解呢？我们来看个例子：

```js
var aa = null;
undefined
var bb = null;
undefined
function aa() {
    console.log("aa");
};
undefined
function bb() {//此函数执行需要1000ms
    setTimeout(aa, 1000);
};
undefined
setInterval(bb, 500);//间歇调用500ms执行一次

//结果显示，aa每隔500ms就显示一个，所以说，间歇调用并没有等到里面的函数执行结束后再进行下一遍，而是依然按照500ms的顺序在执行，所以就导致了后一个间歇调用再前一个间歇调用前启动的情况。
```

### 系统对话框

显示对话框的时候代码会停止执行，关掉后继续执行。

#### alert()——警告框

一般用来向用户展示错误消息。

包含指定文本和 “ OK ” 按钮。

#### confirm()——确认框

一般用来向用户征求意见，比如用户要删除文件的时候。

包含指定文本和 “ OK ” 按钮 && “ Cancel ” 按钮。

通过检查confirm返回值确定用户单击的是哪个按钮，true---OK ，false---Cancel。

```js
if(confirm("Are you sure ?")){
    console.log("click OK");
} else {
    console.log("click Cancel");
}
VM187:4 click Cancel //我点击的是cancel
```

#### prompt()——提示框

让用户输入文本。

两个参数（要显示的文本提示信息，文本输入域的默认值）。

返回用户输入内容。

```js
var result = prompt("What's your name?", "Michael");
if(result != null){
    alert("welcome," + result);
}
```

Google浏览器还有一种新特性，如果用户有意无意的打开了多个对话框，那么在第二个对话框起，每个对话框都有一个是否阻止继续弹出对话框的复选框。

#### print() && find()

查找和打印对话框，都是异步显示的，所以不受Chrome的新特性影响，原理上来说它们可以无限弹出。

```js
window.print();

window.find();
```

## location对象

**它是最有用的BOM对象之一**，**它提供了当前窗口加载的文档有关的信息，还提供了一些导航功能。**

location是一个特别的对象，它既是window的属性，也是document的属性，或者说window.location === document.location。

location对象还将URL解析为独立的片段，可以通过不同的属性来访问这些片段。

location的属性表：（省略了每个属性前的location前缀）

| 属性名   | 例子                  | 说明                                                         |
| -------- | --------------------- | ------------------------------------------------------------ |
| hash     | "#contents"           | 返回URL中的hash（#号后跟0~多个字符）如果URL中不包含散列，则返回空字符串 |
| host     | "www.wrox.com:80"     | 返回服务请求名称和端口号（如果有）                           |
| hostname | "www.wrox.com"        | 返回不带端口号的服务器名称                                   |
| href     | "http://www.wrox.com" | 返回当前页面完整的URL。而location对象的toString()也返回这个值 |
| pathname | "/WileyCDA/"          | 返回URL中的目录或文件名                                      |
| port     | "8080"                | 返回URL中指定的端口号。若无则返空str                         |
| portocol | "http:"               | 返回页面使用的协议，通常是http:或https:                      |
| search   | "?a=javascript"       | 返回URL的查询字符串。这个字符串以问号开头                    |

```js
//在百度中搜索”location对象“
//完整URL：https://www.baidu.com/s?ie=UTF-8&wd=location%E5%AF%B9%E8%B1%A1

location.hash
""
location.host
"www.baidu.com"
location.hostname
"www.baidu.com"
location.pathname
"/s"
location.href
"https://www.baidu.com/s?ie=UTF-8&wd=location%E5%AF%B9%E8%B1%A1"
location.port
""
location.protocol
"https:"
location.search
"?ie=UTF-8&wd=location%E5%AF%B9%E8%B1%A1"
```

### location.search属性优化

location.search属性会返回从问号到URL末尾的所有内容，没办法逐个访问其属性以及参数。

可以创建个函数来解决：

```js
function getQueryStringArgs() {
    //取得查询字符串并去掉开头的问号(先判断不是空str)
    var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
        //保存数据的对象
        args = {},
        //取得每一项
        items = qs.length ? qs.split("&") : [],
        item = null,
        name = null,
        value = null,
        //在for循环中用
        i = 0,
        len = items.length;
    //逐个将每一项添加到args对象中
    for(i=0; i<len; i++){
        item = items[i].split("=");
        //因为查询str是被编码过的，所以要解码
        name = decodeURIComponent(item[0]);	//URI不是URL
        value = decodeURIComponent(item[1]);
        
        if(name.length){
            args[name] = value;
        }
    }
    return args;
}

//在百度中搜索 “有道” 
location.search			//原本的search属性值
"?ie=utf-8&f=3&rsv_bp=1&tn=baidu&wd=%E6%9C%89%E9%81%93&oq=%25E6%259C%2589%25E9%2581%2593&rsv_pq=83b6cc7f00030d59&rsv_t=19555vtYvgWZQ8tfOOjCXjzpPojdh2UaV46ETC0ZHPDnVz1PIprKl4rLapQ&rqlang=cn&rsv_enter=0&prefixsug=%25E6%259C%2589%25E9%2581%2593&rsp=0&rsv_sug=1"

getQueryStringArgs() 	//优化后的属性值。
{ie: "utf-8", f: "3", rsv_bp: "1", tn: "baidu", wd: "有道", …}
f: "3"
ie: "utf-8"
oq: "%E6%9C%89%E9%81%93"
prefixsug: "%E6%9C%89%E9%81%93"
rqlang: "cn"
rsp: "0"
rsv_bp: "1"
rsv_enter: "0"
rsv_pq: "83b6cc7f00030d59"
rsv_sug: "1"
rsv_t: "19555vtYvgWZQ8tfOOjCXjzpPojdh2UaV46ETC0ZHPDnVz1PIprKl4rLapQ"
tn: "baidu"
wd: "有道"
__proto__: Object
```

### 位置操作

使用location对象可以通过很多方式来改变浏览器的位置。

#### assign()方法

> location.assign("http://www.wrox.com");

这样就可以立即打开新URL并生成一条历史记录，用于单击 “ 后退 ” 返回前一个页面。

如果是将location.href或window.location设置一个URL值，也会调用assign()方法。

下面的代码与显示调用assign()方法效果完全一样。

```js
window.location = "http://www.wrox.com";
location.href = "http://www.worox.com";
```

另外，修改location对象的其他属性也可以改变当前加载的页面：

| 属性名   | 例子                    | 说明                                                         |
| -------- | ----------------------- | ------------------------------------------------------------ |
| hash     | "#contents"             | 返回URL中的hash（#号后跟0~多个字符）如果URL中不包含散列，则返回空字符串 |
| host     | "<www.wrox.com:80>"     | 返回服务请求名称和端口号（如果有）                           |
| hostname | "<www.wrox.com>"        | 返回不带端口号的服务器名称                                   |
| href     | "<http://www.wrox.com>" | 返回当前页面完整的URL。而location对象的toString()也返回这个值 |
| pathname | "/WileyCDA/"            | 返回URL中的目录或文件名                                      |
| port     | "8080"                  | 返回URL中指定的端口号。若无则返空str                         |
| portocol | "http:"                 | 返回页面使用的协议，通常是http:或https:                      |
| search   | "?a=javascript"         | 返回URL的查询字符串。这个字符串以问号开头                    |

```js
//假设初始URL为http://www.wrox.com/WileyCDA/

//将URL修改为"http://www.wrox.com/WileyCDA/#section1"
location.hash = "#sesction";

//将URL修改为"http://www.wrox.com/WileyCDA/?q=javascript"
location.search = "?q=javascript";

//将URL修改为"http://www.yahoo.com/WileyCDA/"
location.hostname = "www.yahoo.com";

//将URL修改为"http://www.yahoo.com/mydir/"
location.pathname = "mydir";

//将URL修改为"http://www.yahoo.com:8080/WileyCDA/"
location.prot = "8080";
```

上述方法修改URL之后，浏览器会生成一条历史记录，可以后退到上一个网页。

#### replace()方法

禁止浏览器生成历史记录，用户也就不能回退到前一个页面。

接受一个参数（要导航到的URL）。

```js
setTimeout(function () {
    location.replace("www.wrox.com/");
}, 1000);
```

#### reload()方法

作用是重新加载当前页面。

一个可有可无的参数（true）

```js
location.reload();			//重新加载（有可能从缓存中加载）
location.reload(true);		//重新加载（从服务器重新加载）
```

## navigator对象

现已成为识别客户浏览器的事实标准。

navigator对象是所有支持JavaScript的浏览器所共有的。

它有很多属性，**用来检测显示网页的浏览器类型**。

### 检测插件

检测浏览器是否安装了特定的插件是一种最常见的检测例程。

对于非IE而言，可以用plugins数组来达到这个目的。该数组的每一项都包含下列属性：

- name：插件的名字
- description：插件的描述
- filename：插件的文件名
- length：插件所处理的MIME类型数量。

检测插件的小程序（IE无效）：

```js
function hasPlugin(name){
    name = name.toLowerCase();
    for(var i=0; navigator.plugins[i].name.toLowerCase().indexOf(name) > -1){
        //indexOf()用来查找数组元素，找到返回索引，找不到返回-1
        return true;
    }
    return false;
}

//检测Flash
alert(hasPlugin("Flash"));

//检测QuickTime
alert(hasPlugin("QuickTime"));
```

在IE中检测插件的唯一方法就是使用专有的ActiveXObject类型，并尝试创建一个特定插件的实例。

IE是以COM对象实现插件的。要想检测插件，就必须知道其COM标识符。

检测IE插件：

```js
function hasIEPlugin(name){
    try{
        new ActiveXObject(name);//此处如果成功创建了实例，则证明IE中有这个插件，如果没有此处会出现错误
        return true;
    } catch (ex){
        return false;
    }
}

//检测Flash
alert(hasIEPlugin("ShockwaveFlash.ShockwaveFlash"));//Flash的标识符

//检测QuckTime
alert(hasIEPlugin("QuickTime.QuickTime"));//QuickTime标识符
```

但典型的做法是针对每个插件分别创建函数，而不是前面介绍的通用检测方法。

```js
//接上面两个代码

//检测所有浏览器中的Flash
function hasFlash(){
    var result = hasPlugin("Flash");//其他浏览器
    if(!result){//IE
        result = hasIEPlugin("ShockwaveFlash.ShockwaveFlash");
    }
    return result;//返回结果
}

//检测所有浏览器中的QuckTime
function hasQuickTime() {
    var result = hasPlugin("QuickTime");
    if(!result){
        result = hasIEPlugin("QuickTime.QuickTime");
    }
    return result;
}

//检测Flash
alert(hasFlash());

//检测QuickTime
alert(hasQuickTime());
```

## screen对象

用途不大。

screen对象基本上只用来表明客户端的能力，其中包括浏览器窗口外部显示器的信息，如像素宽度和高度等。

它有诸多属性用来返回客户端信息。

## history对象

history对象保存着用户上网的历史记录。

使用go()方法可以在用户历史记录中任意跳转。

````js
//后退一页
history.go(-1);

//前进两页
history.go(2);

//跳转到最近的wrox.com页面
history.go("Wrox.com");

//后退一页
history.back();

//前进一页
history.forward();

if(history.length == 0){//length属性，保存着历史记录的数量
    //这应该是用户打开窗口后的第一个页面
}
````

# 第九章 客户端检测

每种浏览器都有各自的优缺点。

在现实中，浏览器之间的差异以及不同的浏览器的“怪癖”多的简直不胜枚举。因此客户端检测除了是一种补救措施之外，更是一种行之有效的开发策略。

最重要的是，不到万不得已，就不要使用客户端检测。只要有更通用的方法就用更通用的方法。

## 能力检测

最常用也是最为人们广泛接受的客户端检测形式 是能力检测（又称特性检测）。

能力检测的目标不是识别特定浏览器，而是识别浏览器的能力。

举例：IE5之前的版本不支持document.getElementById()这个DOM方法，支持document.all属性实现相同的目的。

于是就有了类似下面的能力检测代码：

```js
function getElement(id){
    if(document.getElementById){
        return document.getElementById(id);
    } else if(document.all) {
        return document.all[id];
    } else {
        throw new Error("No way to retrieve element!");
    }
}
```

要理解能力检测，必须理解两个重要的概念：

- 先检测能达成目的的最常用特性。对前面的例子来说，先检测document.getElementById()，后检测document.all。因为前者更常用。**先检测最常用的特性可以保证代码最优化**，因为在多数情况下都可以避免测试多个条件。

- 必须测试实际要用到的特性。一个特性存在，不一定意味着另一个特性也存在。

  - ```js
    function getWindowWidth() {
        if(documentl.all){//假设是IE
            return document.documentElement.clientWidth;//错误的用法！！！
        } else {
            return window.innerWidth;
        }
    }
    ```

    这是个错误的例子，因为不一定使用document.all特性的就是IE，Opera支持document.all也支持window.innerWidth。

## 更可靠的能力检测

比如测试一个对象是否支持排序：

```js
function isSortable(object){
    return !!object.sort;//!!将表达式强制转换为布尔值
}
//不要这样做，这不是能力检测——只检测了是否存在相应的方法
//问题是，任何包含sort属性的对象也会返回true
```

更好的方法是检测该对象上 sort是不是一个函数。

```js
//检测sort是不是一个函数
function isSortable(object){
    return typeof object.sort == "function";
}
```

在浏览器环境下测试任何对象的某个特性是否存在，要使用下面这个函数：

```js
function isHostMethod(object, property){
    var t = typeof object[property];
    return t == 'function' || (!!(t=='object' && object[property])) || t=='unknown';
}

//可以像这样使用
result = isHostMethod(xhr, 'open');    //true
result = isHostMethod(xhr, 'foo');     //false
```

## 能力检测，不是浏览器检测

```js
//错误，还不够具体
var isFirefox = !!(navigator.vendor && navigator.vendorSub);
//因为其它浏览器也有同样的属性，比如Safari

//错误，假设过头了
var isIE = !!(document.all && document.uniqueID);
//IE的新版本并不一定会继续存在这两个属性
```

实际上，根据浏览器不同将能力组合起来是更可取的方式。

```js
//确定浏览器是否支持Netscape风格的插件
var hasNSPlugins = !!(navigator.plugins && navigator.plugins.length);

//确定浏览器是否具有DOM1级规定的能力
var hasDOM1 = !!(document.getElementById && document.createElement && document.getElementsByTagName);
```

**在实际开发中，应该将能力检测作为下一步方案的依据，而不是用它来判断用户用的是什么浏览器。**

## 怪癖检测

怪癖检测的目标是识别浏览器的特殊行为。但与能力检测的确认浏览器支持什么能力不同，怪癖检测是想要知道浏览器存在什么缺陷（也就是bug）。

例如，IE8及更早版本有一个bug，即如果某个实例属性与[ [ Enumerable ] ]标记为false的某个原型属性同名，那么该实例属性将不会出现在for-in循环中。    可以用以下代码检测该 “ 怪癖 ” ：

```js
var hasDontEnumQuirk = function () {
    var o = { toString : function () {}};
    for(var prop in o){
        if(prop == "toString"){
            return false;//在正确的ECMAScript实现中，toString应该在for-in循环中返回
        }
    }
    return true;
}();
```

## 用户代理检测

用户代理检测通过检测用户代理字符串来确定用户使用的浏览器。

在每一次HTTP请求过程中，用户代理字符串是作为响应首部发送的，而且该字符串可以通过JavaScript的navigator.userAgent属性访问。

在服务器端，通过检测用户代理字符串来确定用户使用的浏览器是一种常用而且广为接受的做法。

而在客户端，用户代理检测一般被认为是一种万不得已才用的做法，其优先级排在能力检测和怪癖检测之后。

### 电子欺骗

所谓电子欺骗，就是指浏览器通过在自己的用户代理字符串加入错误或误导性信息，来达到欺骗服务器的目的。

## 用户代理字符串检测技术

### 1.识别呈现引擎

如前所述，**确切知道浏览器名和版本号不如确切知道它使用的是什么呈现引擎**。

因为相同引擎的浏览器其都具备同样的功能及特性。

因此我们编写脚本将主要监测五大呈现引擎：IE、Gecko、WebKit、KHTML和Opera。

为了不在全局作用域添加多余的变量，我们将使用模块增强模式来封装检测脚本：

```js
var client = function () {
    var engine = {
        //呈现引擎
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        //具体的版本号
        ver:null
    };
    //在此检测呈现引擎、平台和设备
    return {
        engine: engine
    }
}();//立即执行了，engine也就成为了client的一个属性
```

如果检测到了哪个呈现引擎，那么就以浮点数值形式将该引擎的版本号写入相应的属性。

而呈现引擎的完整版本被写入ver属性。可以支持下面的代码：

```js
if(client.engine.ie){//如果是IE，client.ie的值应该大于0
    //针对IE的代码
} else if ( client.engine.gecko > 1.5){
    if(client.engine.ver == '1.8.1'){
        //针对这个版本执行某些操作
    }
}
```

要正确识别呈现引擎，检测顺序要正确。

第一步就要检测Opera，因为它的用户代理字符串有可能完全模仿其他浏览器。我们不相信Opera，是因为其用户代理字符串不会将自己标识为Opera。

要识别Opera，必须检测window.opera对象：

```js
if(window.opera){
    engine.ver = window.opera.version();
    engine.opera = parseFloat(engine.ver);
}
```

第二步检测WebKit呈现引擎，因为WebKit的用户代理字符串中包含“Gecko”和“KHTML”这两个子字符串，所以如果首先检测它们，很可能会得出错误的结论。

不过WebKit的用户代理字符串中的 “ AppleWebKit ” 是独一无二的，因此检测这个字符串最合适。

下面就是检测该字符串的实例代码：

```js
var ua = navigator.userAgent;//将用户代理字符串保存在变量ua中

if(window.opera){
    engine.ver = window.opera.version();
    engine.opera = parseFloat(engine.ver);
} else if (/AppleWebKit\/(\S+)/.test(ua)){// \S匹配任何非空白字符（此处用来匹配版本号）， 而\s匹配任何空白字符，包括空格、制表符、换页符等等
    //+ 一个或多个    
    //test()接收一个参数，即要匹配的字符串。在模式与该参数匹配的情况下返回=true，否则返回false。
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.webkit = parseFloat(engine.ver);
}
```

一般的用户代理字符串格式为：

​	Mozilla/5.0 (其他)

可以看出，\S可以匹配到5.0，而括号中的\S+就是捕获组的第一个元素，也就是版本号。

接下来要测试的呈现引擎是KHTML。由于Konqueror3.1及更早版本中不包含KHTML的版本，要用Konqueror的版本代替。代码如下：

```js
var ua = navigator.userAgent;//将用户代理字符串保存在变量ua中

if(window.opera){
    engine.ver = window.opera.version();
    engine.opera = parseFloat(engine.ver);
} else if (/AppleWebKit\/(\S+)/.test(ua)){// \S匹配任何非空白字符（此处用来匹配版本号）， 而\s匹配任何空白字符，包括空格、制表符、换页符等等
    //+ 一个或多个    
    //test()接收一个参数，即要匹配的字符串。在模式与该参数匹配的情况下返回true，否则返回false。
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.webkit = parseFloat(engine.ver);
} else if(/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){//[^;]+  匹配一个或多个非;的字符
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.khtml = parseFloat(engine.ver);
}
```

用户代理字符串示例：

​	Mozilla/5.0 (compatible; Konqueror/ 版本号；操作系统或CPU)

在排除了WebKit和KHTML后，就可以准确的检测Gecko了。

但是，在用户代理字符串中，Gecko是出现在字符串"rv:"的后面：

比如：

​	Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:0.9.4) Gecko/20011128 Netscape6/6.2.1

所以：

```js
var ua = navigator.userAgent;//将用户代理字符串保存在变量ua中

if(window.opera){
    engine.ver = window.opera.version();
    engine.opera = parseFloat(engine.ver);
} else if (/AppleWebKit\/(\S+)/.test(ua)){// \S匹配任何非空白字符（此处用来匹配版本号）， 而\s匹配任何空白字符，包括空格、制表符、换页符等等
    //+ 一个或多个    
    //test()接收一个参数，即要匹配的字符串。在模式与该参数匹配的情况下返回=true，否则返回false。
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.webkit = parseFloat(engine.ver);
} else if(/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){//[^;]+  匹配一个或多个非;的字符
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.khtml = parseFloat(engine.ver);
} else if(/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){//Gecko/ 后面的8位数字
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.gecko = parseFloat(engine.ver);    
}
```

最后一个呈现引擎就是IE了。IE版本号位于字符串 “ MSIE ” 的后面、一个分号的前面，因此相应的正则非常简单。

举例：Mozilla/4.0 (compatible; MSIE 4.0; Windows 98)

```js
var ua = navigator.userAgent;//将用户代理字符串保存在变量ua中

if(window.opera){
    engine.ver = window.opera.version();
    engine.opera = parseFloat(engine.ver);
} else if (/AppleWebKit\/(\S+)/.test(ua)){// \S匹配任何非空白字符（此处用来匹配版本号）， 而\s匹配任何空白字符，包括空格、制表符、换页符等等
    //+ 一个或多个    
    //test()接收一个参数，即要匹配的字符串。在模式与该参数匹配的情况下返回=true，否则返回false。
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.webkit = parseFloat(engine.ver);
} else if(/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){//[^;]+  匹配一个或多个非;的字符
    engine.ver = RegExp["$1"];
    engine.khtml = parseFloat(engine.ver);
} else if(/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){//Gecko/ 后面的8位数字
    engine.ver = RegExp["$1"];//正则的第一个捕获组，来去取得版本号。
    engine.gecko = parseFloat(engine.ver);    
} else if(/MSIE ([^;]+)/.test(ua)){
    engine.ver = RegExp["$1"];
    engine.ie = parseFloat(engine.ver);  
}
```

### 2.识别浏览器

大多数情况下，识别了浏览器的呈现引擎就足以为我们采取正确的操作提供依据了。可是，只有呈现引擎还不能说明存在所需的JavaScript功能。

Safari和Chrome浏览器都是用WebKit作为呈现引擎，但它们的JavaScript引擎却不一样。

对于它们，有必要再向client对象添加一些新的属性：

```js
var client = function () {
    var engine = {
        //呈现引擎
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        
        //具体的版本
        ver: null
    };
    
    var browser = {//又添加私有变量browser，用于保存每个浏览器的属性。
        //浏览器
        ie: 0,
        firefox: 0,
        safari: 0,
        konq: 0,
        opera: 0,
        chrome: 0,
        
        //具体的版本
        ver: null
    };
    
    //在此检测呈现引擎、平台和设备
    return {
        engine: engine,
        browser: browser
    };
}();
```

由于大多数浏览器与其呈现引擎密切相关，所以下面示例中检测浏览器的代码与检测呈现引擎的代码是混合在一起的。

```js
//检测呈现引擎及浏览器
var ua = navigator.userAgent;

if (window.opera){//检测opera引擎
    engine.ver = browser.ver = window.opera.version();
    engine.opera = browser.opera = parseFloat(engine.ver);
} else if (/AppleWebkit\/(\S+)/.test(ua)){//检测webkit引擎
    engine.ver = RegExp["$1"];
    engine.webkit = parseFloat(engine.ver);
    
    //确定是Chrome还是Safari
    if(/Chrome\/(\S+)/.test(ua)){
        browser.ver = RegExp["$1"];
        browser.chrome = parseFloat(browser.ver);
    } else if (/Version\/(\S+)/.test(ua)){
        browser.ver = RegExp["$1"];
        browser.safari = parseFloat(browser.ver);
    } else {
        //近似的确定版本号
        var safariVersion = 1;
        if(engine.webkit < 100){
            safariVersion = 1;
        } else if (engine.webkit < 312){
            safariVersion = 1.2;
        } else if (engine.webkit < 412){
            safariVersion = 1.3;
        } else {
            safariVersion = 2;
        }
        
        browser.safari = browser.ver = safariVersion;
    }
} else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){//检测khtml引擎
    engine.ver = browser.ver = RegExp["$1"];
    engine.khtml = browser.konq = parseFloat(engine.ver);
} else if (/rv:([^\)]+) Gecko\/d{8}/.test(ua)){//检测gecko引擎
    engine.ver = RegExp["$1"];
    engine.gecko = parseFloat(engine.ver);
    
    //确定是不是Firefox
    if(/Firefox\/(\S+)/.test(ua)){
        browser.ver = RegExp["$1"];
        browser.firefox = parseFloat(browser.ver);
    }
} else if (/MSIE ([^;]+)/.test(ua)){//检测ie引擎
    engine.ver = browser.ver = RegExp["$1"];
    engine.ie = browser.ie = parseFloat(engine.ver);
}



//有了上面这些代码之后，我们就可以编写下面的逻辑
if(client.engine.webkit){
    if(client.browser.chrome){
        //执行针对Chrome的代码
    } else if (client.browser.safari){
        //执行针对Safari的代码
    }
} else if (client.engine.gecko){
    if (client.browser.firefox){
        //执行针对Firefox的代码
    } else {
        //执行针对Gecko的代码
    }
} else if (client.engine.ie){
    //执行针对ie的代码
} else if (client.engine.opera){
    //执行针对Opera的代码
}
```

### 插话：五大主流浏览器

**五大主流浏览器（按照诞生顺序介绍）：**

1、IE（Internet Explorer）浏览器：

​     IE的诞生起源于1994年，当时微软为了对抗几乎占据市场百分之九十份额的网景Netscape Navigator（导航者），准备在windows中开发自己的浏览器，取名为Internet Explorer，意为因特网探险者，好吧，一个导航者一个探险者，从名字起火药味就很重（ps 自此也拉开了第一次浏览器大战的帷幕，结果大家都知道了，微软大获全胜，基本以98年网景将自己卖给了AOL公司暂且告终，但是还没结束，因为后来网景换了个身份，也就是Firefox火狐，又进入了大众视野，迸发了一种凤凰涅槃的快感，到今天为止Firefox也成为了五大主流浏览器之一，后面我们再说它~  话说回来，竞争才能推动技术的发展，第一次浏览器大战以微软和网景为代表，大力推动了浏览器方面技术的发展，各大公司开始着手研发自己的浏览器，有压力才有动力嘛），但是微软着急对抗网景啊，没那么多时间从零开始，于是选择和和Spyglass合作，所以IE其实从早期一款商业性的专利网页浏览器Spyglass Mosaic派生出来，虽然Spyglass Mosaic与NCSA Mosaic(1993年，美国NCS（National Center for Supercomputing Applications）也就是国家超级计算机中心，发布的世界上第一款Web浏览器取名为Mosaic，后来网景大名鼎鼎的Mozilla就来自于这里，意为Mosaic Killer（Mosaic杀手）不过事实上， Mosaic 并不是第一个具有图形界面的网页浏览器，但是， Mosaic 是第一个被人普遍接受的浏览器，它让许多人了解了Internet )甚为相似，但Spyglass Mosaic则相对地较不出名并使用了NCSA Mosaic少量的源代码~~

​     从1996年开始，微软从Spyglass手里拿到了Spyglass Mosaic的源代码和授权。从而使IE逐渐成为微软专属软件。（后来，微软以IE和操作系统捆绑的模式不断扩展其市场份额，使IE成为了浏览器市场的绝对主流~~）从那时开始，IE的呈现引擎就是Trident，这也是大家俗称的IE内核，国内的大多数浏览器都有使用IE内核，或者是IE和Chrome双内核这样的形式来提高性能。

2、Opera浏览器：

​      Opera创始于1995年4月，由挪威Opera Software ASA公司发布，2016年2月确定被奇虎360和昆仑万维收购（题外话~Opera浏览器从一开始，就在做自己的东西，无论是内核还是版本号，虽然后来为了市场份额还是弃用了曾让其达到巅峰的Presto，转向了Webkit，现在是Blink，但我还是欣赏这家公司在残酷的浏览器大战中坚持自己并存活下来的顽强精神的，它的起源时间和IE差不多，但是没有微软那样强大的后台~也许从它弃用自己内核的那时候起就决定了这个结果吧，但是不得不说，它为浏览器的发展贡献了不可或缺的一份力量，最后，希望奇虎和万维能将这样一个有骨血的浏览器继续发扬光大吧，虽然~最初的东西已经没有了）。自我感觉，Opera能从第一次浏览器大战两大霸主的交火中勉强存活下来已经是个奇迹了，毕竟后来的三大浏览器都是诞生于第一次浏览器大战之后，但是却没抵得过时间的考验，这真的是个悲伤的故事~~

​      前段括弧里面已经交代清楚了，Opera浏览器的内核最初是Presto，前几年宣布使用Google的开源项目Webkit作为自己的内核，没过多久，又跟随Google使用Blink内核~~就酱~

3、Safari浏览器：

​      第二次浏览器大战基本是从苹果公司2003年1月发布其自有浏览器Safari开始的，苹果利用自己独天得厚的手机市场份额，使Safari浏览器的用户数量不断上升。从Safari推出之时起，它的渲染引擎就是Webkit，一提到 webkit，首先想到的便是 chrome，可以说，chrome 将 Webkit内核 深入人心，殊不知，Webkit 的鼻祖其实是 Safari。现在很多人错误地把 webkit 叫做 chrome内核（即使 chrome内核已经是 blink 了），苹果都哭瞎了有木有。Safari 是苹果公司开发的浏览器，使用了KDE（Linux桌面系统）的 KHTML 作为浏览器的内核，Safari 所用浏览器内核的名称是大名鼎鼎的 WebKit。 Safari 在 2003 年 1 月 7 日首度发行测试版，并成为 Mac OS X v10.3 与之后版本的默认浏览器，也成为苹果其它系列产品的指定浏览器（也已支持 Windows 平台）。如上述可知，WebKit 前身是 KDE 小组的 KHTML 引擎，可以说 WebKit 是 KHTML 的一个开源的分支。当年苹果在比较了 Gecko 和 KHTML 后，选择了后者来做引擎开发，是因为 KHTML 拥有清晰的源码结构和极快的渲染速度。Webkit内核可以说是以硬件盈利为主的苹果公司给软件行业的最大贡献之一。随后，2008 年谷歌公司发布 chrome 浏览器，采用的 chromium 内核便fork了 Webkit。

4、Firefox浏览器：

​     前面提到过，在第一次浏览器中大败的网景公司并没有彻底烟消云散，就是几经曲折（此处省略，有兴趣查阅资料），原网景公司的人员创办了Mozilla基金会，这是一个非盈利组织，正是他们在2004年推出了自己的浏览器Firefox，并且以之前的Mosaic内核为基础，开发了Gecko引擎，这也是火狐自04年发布以来一直使用的渲染引擎~后来在2005年，又在基金会的基础上成立了Mozilla公司，其主要任务就是继续开发Firefox。Gecko是一个开源项目，代码完全公开，因此受到很多人的青睐~~对了，从Firefox问世开始，第二次浏览器大战基本算是彻底打响了，第二次浏览器大战与第一次二元鼎力的局面不同，这一次的特点就是百家争鸣，也自此打破了IE浏览器从98年网景被收购后独霸浏览器市场的局面。

5、Chrome浏览器：

​     2008年，大名鼎鼎的互联网巨头Google公司发布了它的首款浏览器Chrome浏览器。虽然在浏览器方面，Chrome算是年轻的一代了，但是没办法啊，人家是富二代官二代啊，后台太强，而且确实先天能力得天独厚，从文章最初贴的那个浏览器市场份额报告可以看出即便是在国内市场，Chrome浏览器依然占据着半壁江山。前面说的，其实Chrome浏览器的内核名为chromium，也就是现在大家习惯称的chrome内核，而且按照大家的误解，一直认为的chrome内核就是由苹果公司最先选择的算是KHTML引擎的分支-Webkit，这大概是苹果公司至今说不清道不明的伤痛吧~~chromium fork 自开源引擎 webkit，却把 WebKit 的代码梳理得可读性提高很多，所以以前可能需要一天进行编译的代码，现在只要两个小时就能搞定。因此 Chromium 引擎和其它基于 WebKit 的引擎所渲染页面的效果也是有出入的。所以有些地方会把 chromium 引擎和 webkit 区分开来单独介绍，而有的文章把 chromium 归入 webkit 引擎中，都是有一定道理的。（谷歌公司还研发了自己的 Javascript 引擎，V8，极大地提高了 Javascript 的运算速度。）chromium 问世后，带动了国产浏览器行业的发展。一些基于 chromium 的单核，双核浏览器如雨后春笋般拔地而起，例如 搜狗、360、QQ浏览器等等，无一不是套着不同的外壳用着相同的内核。

​     然而 2013 年 4 月 3 日，谷歌在 Chromium Blog 上发表 博客，称将与苹果的开源浏览器核心 Webkit 分道扬镳，在 Chromium 项目中研发 Blink 渲染引擎（即浏览器核心），内置于 Chrome 浏览器之中。其实Blink引擎也就是Webkit的分支，就像Webkit是KHTML的分支一样。Blink引擎现在是谷歌公司与Opera Software共同研发，上面提到过的，Opera弃用了自己的Presto内核，加入Google阵营，跟随谷歌一起研发Blink，套上Chromium内核后，用户体验貌似确实大不如前，鼎盛时期的Opera7.0也不复存在~~

 

​     好啦，五大主流浏览器的内核基本啰啰嗦嗦的配合着浏览器大战的一些陈年往事介绍完了~~接下来，连着国内的一些主流浏览器内核做一个详细的总结。

1、IE浏览器内核：Trident内核，也是俗称的IE内核；

2、Chrome浏览器内核：统称为Chromium内核或Chrome内核，以前是Webkit内核，现在是Blink内核；

3、Firefox浏览器内核：Gecko内核，俗称Firefox内核；

4、Safari浏览器内核：Webkit内核；

5、Opera浏览器内核：最初是自己的Presto内核，后来加入谷歌大军，从Webkit又到了Blink内核；

6、360浏览器、猎豹浏览器内核：IE+Chrome双内核；

7、搜狗、遨游、QQ浏览器内核：Trident（兼容模式）+Webkit（高速模式）；

8、百度浏览器、世界之窗内核：IE内核；

9、2345浏览器内核：好像以前是IE内核，现在也是IE+Chrome双内核了；

10、UC浏览器内核：这个众口不一，UC说是他们自己研发的U3内核，但好像还是基于Webkit和Trident，还有说是基于火狐内核。。

### 3.识别平台

在某些条件下，平台可能是必须关注的问题。那些具有各种平台版本的浏览器（如Safari、Firefox和Opera）在不同的平台下可能会有不同的问题。

目前三大主流平台是：Windows、Mac和Unix（包括Linux）。

为了检测这些平台，还需添加一个新对象：

~~~js
var client = function () {
    var engine = {
        //呈现引擎
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        
        //具体的版本
        ver: null
    };
    
    var browser = {//又添加私有变量browser，用于保存每个浏览器的属性。
        //浏览器
        ie: 0,
        firefox: 0,
        safari: 0,
        konq: 0,
        opera: 0,
        chrome: 0,
        
        //具体的版本
        ver: null
    };
    
    var system = {
        //平台
        win: false,
        mac: false,
        xll: false
    };
    
    //在此检测呈现引擎、平台和设备
    return {
        engine: engine,
        browser: browser,
        system: system
    };
}();
~~~

对于这三个平台而言，浏览器一般只报告Windows的版本，所以保存的是布尔值，而不是数字。

在确定凭条时，检测navigator.platform要比检测用户代理字符串更简单，后者会在不同浏览器中给出不同的平台信息。而前者的值包括：Win32   Win64  MacPPc   MacIntel   Xll   Linux i686

检测平台的代码非常直观：

```js
var p = navigator.platform;
system.win = p.indexOf("Win") == 0;//此处== 比= 优先级要高，所以会先计算出来后面表达式的布尔值，然后赋值给前面变量
system.mac = p.indexOf("Mac") == 0;//indexOf()用来查找数组元素的位置、查找str中子str的位置。找到返回位置，找不到返回-1
system.mac = (p.indexOf("Xll") == 0) || (p.indexOf("Linux") == 0); 	
```

### 4.识别Windows操作系统

在Windows平台下，还可以从用户代理字符串中进一步取得具体的操作系统信息。

```js
if(system.win){
    if(/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)){//?:-->非获取匹配，匹配冒号后面的内容，但不进行存储供以后使用，因此它不算是一个捕获组。    [^do]非空格字符。    嗯，其他的没什么问题
        if(Regexp["$1"] == "NT"){
            switch(RegExp["$2"]){
                case "5.0":
                    system.win = "2000";
                    break;
                case "5.1":
                    system.win = "XP";
                    break;
                case "6.0":
                    system.win = "Vista";
                    break;
                case "6.1":
                    system.win = "7";
                    break;
                default:
                    system.win = "NT";
                    break;
            }
        } else if (RegExp["$1"] == "9x"){
            system.win = "ME";
        } else {
            system.wwin = RegExp["$1"];
        }
    }
}

//用
if(client.system.win){//非空str会转换为布尔值
    if(client.system.win == "XP"){
        //XP
    } else if (client.system.win == "Vista"){
        //Vista
    }
}
```

### 5.识别移动设备

首先为要检测的所有移动设备添加属性：

```js
var client = function () {
    var engine = {
        //呈现引擎
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        
        //具体的版本
        ver: null
    };
    
    var browser = {//又添加私有变量browser，用于保存每个浏览器的属性。
        //浏览器
        ie: 0,
        firefox: 0,
        safari: 0,
        konq: 0,
        opera: 0,
        chrome: 0,
        
        //具体的版本
        ver: null
    };
    
    var system = {
        //平台
        win: false,
        mac: false,
        xll: false
        
        //移动设备
        iphone: false,
        ipod: false,
        ipad: false,
        ios: false,
        android: false,
        nokiaN: false,
        winMobile: false
    };
    
    //在此检测呈现引擎、平台和设备
    return {
        engine: engine,
        browser: browser,
        system: system
    };
}();
```

检测ios设备：

```js
system.iphone = ua.indexOf("iPhone") > -1;
system.ipod = ua.indexOf("ipod") > -1;
system.ipad = ua.indexOf("ipad") > -1;
```

检测iOS版本：

```js
if(system.mac && ua.indexOf("Mobile") > -1){//检验系统是不是Mac OS、字符串中是否存在“Mobile”，可以保证无论是什么版本，system.ios中都不会是0.
    if(/CPU (?:iPhone)?OS (\d+_\d+)/.test(ua)){
        system.ios = parseFloat(RegExp.$1.replace("_","."));
    } else {
        system.ios = 2;
    }
}
```

 检测Android版本：

```js
if(/Android (\d+\.\d+)/.test(ua)){//由于所有版本的Android都有版本值，因此这样就可以检测所有版本
    system.android = parseFloat(RegExp.$1);
}
```

检测Nokia   N系列手机：

```js
system.nokiaN = ua.indexOf("NokiaN") > -1;
```

检测及使用：

```js
if(client.engine.webkit){
    if(client.system.ios) {
        //IOS
    } else if (client.system.android){
        //Android
    } else if (client.system.nokiaN){
        //nokia N
    }
}
```

### 6.识别游戏系统

两大游戏系统：

- 任天堂Wii（Wii中的浏览器是定制版的Opera）
- PlayStation

两个用户代理字符串：

````js
Opera/9.10 (Niintendo Wii;U; ; 1621; en)
Mozilla/5.0 (PLAYSTATION 3; 2.00)
````

所以在client.system中添加适当属性：

```js
var system = {
        //平台
        win: false,
        mac: false,
        xll: false,
        
        //移动设备
        iphone: false,
        ipod: false,
        ipad: false,
        ios: false,
        android: false,
        nokiaN: false,
        winMobile: false,
    
    	//游戏系统
    	wii: false,
    	ps: false
    };
```

检测游戏系统代码：

```js
system.wii = ua.indexOf("Wii") > -1;
system.ps = /playstation/i.test(ua);
```

### 7.完整的代码

```js
var client = function () {
    //呈现引擎
    var engine = {
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        
        //完整的版本号
        ver : null
    };
    
    //浏览器
    var browser = {
        ie: 0,
        chrome: 0,
        opera: 0,
        firefox: 0,
        safari: 0,
        kong: 0,
        
        //完整的版本号
        ver : null
    };
    
    //平台、设备和（游戏）操作系统
    var system = {//由于浏览器一般只会返回Windows系统的版本，所以索性只关注它是哪个系统，暂时并不关注其版本号
        //平台
        win: false,
        ios: false,
        xll: false,
        
        //移动设备
        iphone: false,
        ipad: false,
        ipod: false,
        ios: false,
        android: false,
        nokiaN: false,
        winMoile: false,
        
        //游戏系统
        wii: false,
        ps: false
        
    };
    
    //检测呈现引擎及浏览器(先检测引擎，然后根据引擎检测浏览器)
    var ua = navigator.userAgent;//将用户代理字符串保存在ua中
    if(window.opera){//由于Opera不会将自己用户代理字符串中写Opera，所以必须检测window.opera
        engine.ver = browser.ver = window.opera.version();
        engine.opera = browser.opera = parseFloat(engine.ver);
    } else if (/Appwebkit\/(\S+)/.test(ua)){//检测WebKit引擎
        engine.ver = RegExp["$1"];
        engine.webkit = parseFloat(engine.ver);
        
        //确定是Chrome还是Safari
        if(/Chrome\/(\S+)/.test(ua)){
            browser.ver = RegExp["$1"];
            browser.chrome = parseFloat(browser.ver);
        } else if (/Version\/(\S+)/.test(ua)){
            browser.ver = RegExp["$1"];
            browser.safari = parseFloat(browser.ver);
        } else {//近似的确定版本号
            var safariVersion = 1;
            if(engine.webkit < 100){
                safariVersion = 1;
            } else if (engine.webkit < 312){
                safariVersion = 1.2
            } else if (engine.webkit < 412){
                safariVersion = 1.3
            } else {
                safariVersion = 2;
            }
            
            browser.safari = browser.ver = safariVersion;
        }
    } else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){//检测KHTML引擎
        engine.ver = browser.ver = RegExp["$1"];
        engine.khtml = browser.kong = parseFloat(engine.ver);
    } else if (/rv:([^/)]+)\) Gecko\/\d{8}/.test(ua)){//检测gecko引擎
        engine.ver = RegExp["$1"];
        engine.gecko = parseFloat(engine.ver);
        
        //确定是不是Firefox
        if(/Firefox\/(\S+)/.test(ua)){
            browser.ver = RegExp["$1"];
            browser.firefox = parseFloat(browser.ver);
        }
    } else if (/MSIE ([^;]+)/.test(ua)){//检测IE引擎
        engine.ver = browser.ver = RegExp["$1"];
        engine.ie = browser.ie = parseFloat(engine.ver);
    }
    
    //检测浏览器(个人觉得多此一举)
    browser.ie = engine.ie;
    browser.opera = engine.opera;
    
    //检测平台
    var p = navigator.platform;
    system.win = p.indexOf("Win") == 0;//判断p的开头是否是Win   dows
    system.mac = p.indexOf("Mac") == 0;
    system.xll = (p == "Xll") || (p.indexOf("Linux") == 0);
    
    //检测Windows操作系统及版本
    if(system.win){
        if(/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)){
            if(RexExp["$1"] == "NT"){
                switch(RexExp["$2"]){
                    case "5.0":
                        system.win = "2000";
                        break;
                    case "5.1":
                        system.win = "XP";
                        break;
                    case "6.0":
                        system.win = "Vista";
                        break;
                    case "6.1":
                        system.win = "7";
                        break;
                    default:
                        system.win = "NT";
                        break;
                }
            } else if (RegExp["$1"] == "9x"){
                system.win = "ME";
            } else {
                system.win = RegExp["$1"];
            }
        }
    }
    
    //检测iOS操作系统及版本
    if(system.mac && ua.indexOf("Mobile") > -1){
        if(/CPUU (?:iPhome )?OS (\d+_\d+)/.test(ua)){
            system.ios = parseFloat(RegExp.$1.replace("_","."));
        } else {
            system.ios = 2;
        }
    }
    
    //检测Android操作系统及版本
    if(/Android (\d+_\d+)/.test(ua)){
        system.android = parseFloat(RegExp["$1"]);
    }
    
    //检测移动设备
    system.iphone = ua.indexOf("iPhone") > -1;
    system.ipod = ua.indexOf("iPod") > -1;
    system.ipad = ua.indexOf("iPad") > -1;
    system.nokiaN = ua.indexOf("NokiaN") > -1;
    
    //windows mobile
    if(system.win == "CE"){
        system.winMobile = system.win;
    } else if (system.win == "Ph"){
        if(/Windows Phone OS (\d+\.\d+)/.test(ua)){
            system.win == "Phone";
            system.winMobile = parseFloat(RegExp["$1"]);
        }
    }
    
    //检测游戏系统
    system.wii = ua.indexOf("wii") > -1;
    system.ps = /playstation/i.test(ua);
    
    //最后一步，返回这些对象
    return {
        engine: engine,
        browser: browser,
        system: system
    }
}();
```

### 使用方法

强调一下，用户代理检测是客户端检测的最后一个选择。只要可能，都应该优先采用能力检测和怪癖检测。因为这种方法对用户代理字符串具有很强的依赖性。

用户代理检测一般适用于下列情形：

- 不能准确的使用能力检测和怪癖检测
- 同一款浏览器在不同平台下具有不同的能力
- 为跟踪分析等目的需要知道确切的浏览器。

## 小结

由于浏览器之间存在差别，通常需要根据不同浏览器的能力分别编写不同的代码。

最常用的三种检测方法：

- [ ] 能力检测：在编写代码之前先检测特定浏览器的能力。
- [ ] 怪癖检测：怪癖就是bug。通常涉及到运行一小段代码来确定浏览器是否存在某个怪癖。
- [ ] 用户代理检测：通过检测用户代理字符串来识别浏览器。
  - [x] 电子欺骗：浏览器提供商试图通过在用户代理字符串中添加一些欺骗性信息，欺骗网站相信自己的浏览器是另一种浏览器。
  - [ ] 用户代理检测需要特殊的技巧，特别是要注意Opera会隐瞒其用户代理字符串的情况。

# 第十章 DOM

DOM（文档对象模型）是针对HTML和XML文档的一个API（应用编程接口）。

注意，**IE中的所有DOM对象都是以COM对象的形式实现的**。这意味着IE中的DOM对象与原生的JavaScript对象的行为活动特点并不一致。

## 节点层次

DOM可以将任何HTML或XML文档描绘成一个由多层节点构成的结构。

节点分为几种不同的类型，每种类型分别表示文档中不同的信息及标记。

每个节点都拥有各自的特点、数据和方法，另外也与其他节点存在某种关系。

节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点为根节点的树形结构。

以HTML为例：

```html
<html><!--文档节点，也是每个文档的根节点。在这个例子中，<html>元素是文档节点的唯一子节点，称之为文档元素。是文档最外层元素。每个文档只有一个文档元素。在HTML页面中，文档元素始终都是<html>元素。在XML中，没有预定义的元素，因此任何元素都可能成为文档元素。-->
    <head>
        <title>Sample Page</title>
    </head>
    <body>
        <p>
            Hello World!
        </p>
    </body>
</html>
```

### Node类型



JavaScript中的所有节点类型都继承自Node类型，因此所有节点都共享着相同的基本属性和方法。

每个节点都有一个nodeType属性，用于表明节点类型，由12个数值常量表示，任何节点类型必居其一：

- [x] Node.ELEMENT_NODE(1);元素节点
- [ ] Node.ATTREBUTE_NODE(2);属性节点
- [ ] Node.TEXT_NODE(3);  文本节点
- [ ] Node.CDATA_SECTION_NODE(4);CDATA节点
- [ ] Node.ENTITY_REFEREENCE_NODE(5);实体引用名称节点
- [ ] Node.ENTITY_NODE(6);实体名称节点
- [ ] Node.PROCESSING_INSTRUCTION_NODE(7);处理指令节点
- [ ] Node.COMMENT_NODE(8);注释节点
- [ ] Node.DOCUMENT_NODE(9);文档节点
- [ ] Node.DOCUMENT_TYPE_NODE(10);文档类型节点
- [ ] Node.DOCUMENT_FRAGMENT_NODE(11);文档片段节点
- [ ] Node.NOTATION_NODE(12)。DTD声明节点

通过上面这些常量，可以容易的确定节点类型，为了确保浏览器（IE）兼容，最好将nodeType属性与数字值进行比较：

```js
if(someNode.nodeType == 1){//适用于所有浏览器
    alert("Node is an element");
}
```

- nodeName属性：元素节点中nodeName保存元素的标签名。
- nodeValue属性：元素节点的nodeValue值为null。

#### 节点关系

每个节点都有一个childNodes，保存着NodeList对象。NodeList是以追踪类数组对象，用于保存着一组有序节点，可通过位置访问这些节点。但要注意**它并不是Array实例**。

DOM结构的变化能够自动反映在NodeList对象中。 我们常说，**NodeList是有生命、有呼吸的对象，而不是某一瞬间拍下的照片。**

访问在NodeList中的节点：

```js
var firstChild = someNode.childNodes[0];//更多人用
var secondChild = someNode.childNodes.item(1);//也可以用item()
var count = someNode.ChildNodes.length;//要注意length属性表示访问NodeList的那一刻，其中包含的节点数量
```

对arguments对象使用Array.prototype.slice()方法可以将其转化为数组。而采用同样的方法，也可以将NodeList对象转换为数组。

```js
var arrayOfNodes = Array.prototype.splice.call(someNode.childNodes,0);
```

要想在IE中将NodeList转换为数组，必须手动枚举所有成员。

兼容后：

```js
function convertToArray(nodes) {
    var array = null;
    try{
        array = Array.prototype.splice.call(nodes,0);
    } catch (ex) {
        for(var i=0, len = nodes.length; i<len; i++) {
            array.push(nodes[i]);
        }
    }
    return array;
}
```

- parentNode属性：指向父节点。
- childNodes属性：指子节点，之间都是同胞节点。
  - previousSibling属性：同列表中当前节点的上一个节点，无则null。
  - nextSibling属性：同列表中当前节点的下一个节点，无则null。

```js
if(someNode.nextSibling == null){
    alert("This is last node!");
} else if (someNode.previousSibling == null){
    alert("This is first node!");
}
```

- firstChild属性：父节点的第一个子节点。
- lastChild属性：父节点的最后一个子节点。
- [x] hasChildNodes()方法：在节点包含一个或多个子节点的情况下返回true。
- ownerDocument属性：指向表示整个文档的文档节点（最外层的节点）。通过这个属性，我们不用层层回溯，可以直接访问文档节点。
- [x] appendChild()方法：用于向childNodes列表末尾添加一个节点。返回新增节点。如果传入节点已经是文档的一部分了，那么就将节点从原位置转移到新位置。

```js
var returnNode = someNode.appendChild(newNode);
alert(returnNode == newNode);  //true
alert(someNode.lastChild == newNode);  //true

//传入已有节点情况
var returnNode = someNode.appendChild(someNode.firstChild);
alert(returnNode == someNode.firstChild);   //false
alert(returnNode == someNode.lastChild);   //true
```

- [x] insertBefore()方法：把节点放在childNodes列表中的某个特定位置上。接受两个参数（要插入的节点，作为参照的节点）。插入的节点会插入到参照节点前一个位置。返回插入的节点。

```js
//插入到最后
returnNode = someNode.insertBefore(newNode, null);
alert(returnNode == someNode.lastChild); //true

//插入到第一个
returnNode = someNode.insertBefore(newNode, someNode.firstChild);
alert(returnNode == someNode.firstChild);

//插入到最后一个的前面
returnNode = someNode.insertBefore(newNode, someNode.lastChild);
alert(returnNode == someNode.childNode[someNode.childNode.length - 2]);
```

- [x] replaceChild()方法：替换节点。接受两个参数（要插入的节点，要替换掉的节点）。

  ```js
  //替换第一个子节点
  var returnNode = someNode.replaceChild(newNode, someNode.firstChild);
  ```

- [x] removeChild()方法：移除节点。接受一个参数（要移除的节点）。

  ```js
  //移除第一个子节点
  var returnNode = someNode.removeChild(someNode.firstChild);
  ```

如果在不支持子节点的节点中调用这些方法会导致错误发生。

- [x] clineNode()方法：克隆节点。接受个布尔值，表示是否深度复制。true——深复制（复制节点及其整个节点树）；false——浅复制（仅复制节点本身）。	复制后的节点由文档所有，但并没有指定父节点，因此这个节点副本就成了一个“孤儿”，除非用appendChild()、insertBefore()、replaceChid()将它添加到文档中。    它不会复制事件处理程序（添加到DOM节点中的JavaScript属性）。IE存在bug就是也复制事件处理程序。    所以建议在克隆之前先移除事件处理程序。
- [x] normalize()方法：处理文档树中的文本节点。由于某种原因，可能会出现文本节点不包含文本，或接连出现两个文本节点。  normalize()方法就会查找这两种情况，如果找到空文本节点，删除它；如果找到两个连着的文本节点，合并它。

### Document类型



JavaScript通过Document类型表示文档。在浏览器中，document是HTMLDocument（继承自Document）的一个实例，表示整个HTML页面。

Document节点由下列特征：

- nodeType的值为9；
- nodeName的值为“#document”；
- nodeValue的值为null；
- parentNode 的值为null；
- ownerDocument的值为null；

文档的子节点

- document.documentElement属性：始终指向<html>元素
- document.body属性：始终指向<body>元素
- document.doctype属性：始终指向<!DOCTYPE>元素。但浏览器对它的支持不一致，所以它用处有限。

文档的信息

- document.title属性：表示文档标题

  ```js
  //取得文档标题
  var title = document.title;
  
  //设置文档标题
  document.title = "New page Title";
  ```

- document.URL属性：包含页面完整的URL

- document.domain属性：包含页面的域名

- document.referrer属性：保存着链接到当前页面的那个页面的URL。

```js
//取得完整的URL
var url = document.URL;    			//http://www.baidu.com

//取得域名
var domain = document.domain;		//www.baidu.com

//取得来源网页的URL
var referrer = document.referrer;	//nczonline.net   出错
```

只有domain属性可设置。但有限制，只能设置为URL包含的域，比如这个例子只能讲domain值重新设置为baidu.com，但不能设置回“松散的”。

```js
document.domain = "baidu.com";

document.domain = "www.baidu.com";//出错
```

当页面中包含来自其他子域的框架或内嵌框架时，能够设置document.domain就非常方便了。由于跨域安全限制，来自不同子域的页面无法通过JavaScript通信。而通过将每个页面的document.domain设置为相同的值，就可以互相访问对方包含的JavaScript对象了。

查找元素：

- [x] getElementById()方法。（ID）

​	如果页面中多个元素ID相同，此方法只返回文档中第一次出现的元素。

​	IE7及更低版本有个怪癖，此方法会查到表单元素name的值。为了避免这个问题，就是让表单元素name值不与其他元素的ID值相同。

- [x] getElementsByTagName()方法。通过元素的标签名查找元素。返回的是0或多个元素的HTMLcollection对象，作为一个动态集合，该对象与NodeList非常相似。	可以使用方括号语法或item()来访问其中的项。	向里传入\*作为参数则可以取得文档中的所有元素。    注意，IE将注释实现为元素，所以在IE中使用此方法传入\*会将所有注释节点也返回出来。

- [x] nameItem()方法。通过元素的name取得集合中的项。

  - [ ] ```js
    <img src="myimage.gif" name="myImage">
        
    var images = document.getElementsByTagName("img");
    
    var myImage = images.nameItem("myImage");
    ```

- [x] getElementsByName()方法。返回所有给定name的所有元素。最常用的是使用它取得单选按钮。因为一组单选按钮的name特性必须相同。

#### 特殊集合

- [x] document.anchors，包含文档中所有带name特性的<a>元素。
- [x] document.forms，包含文档中所有的<form>元素。
- [x] document.images，包含文档中所有的<img>元素。
- [x] document.links，包含文档中所有带href特性的<a>元素。

集合的项也会跟着文档的内容更新而更新。

- [x] document.implementation属性用于检测浏览器实现的DOM。DOM1级只为document.implementation规定了一个方法，即hasFeature()，该方法接受两个参数（要检查的DOM功能，版本号）。

```js
var hasXmlDom = document.implementation.hasFeature("XML","1.0");
```

建议同时使用能力检测。

- [x] write()方法。向文档写入输出流。
- [x] writeln()方法。末尾带换行的向文档写入输出流。
- [x] open()方法和close()方法。分别用于打开和关闭网页的输出流。

### Element类型

Element类型用于表现XML或HTML元素，提供了对元素标签名、子节点及特性的访问。

特征：nodeType的值为1；

​	  nodeName的值为元素的标签名。

由于HTML和XML对标签名的大小写要求不统一，所以建议这样写：

```js
if(element.tagName == 'div'){//不建议，很容易出错
    
}

if(element.tagName.toLowerCase() == 'div'){//这样最好
    
}
```

#### 操作特性的DOM方法

##### getAttribute()方法——取得特性值

用来取得元素的特性，传递给getAttriute()的特性名与实际的特性名相同。(也可以取得自定义特性，自定义特性根据HTML5规范应该加上data-前缀。并且不区分大小写。)

```js
var div = document.getElementById("div");
alert(div.getAttribute("id"));				//"myDiv"
alert(div.getAttribute("class"));			//"myClass"
```

用getAttribute()方法取得style特性时会返回css文本，而通过属性访问它则返回一个对象。

用getAttribute()方法取得onclick这样的事件处理程序时，则会返回相应的字符串。而访问onclick()属性时则返回给一个函数。

由于存在这些差别，在用JavaScript操作DOM时经常不使用此方法，而是只是用对象的属性。只有在取得自定义特性值的时候才会使用它。

##### setAttribute()方法——设置特性值

用来设置特性值。接受两个参数（要设置的特性名，值）。

若属性存在则替换值，若不存在则创建属性并赋值。

```js
div.setAttribute("id", "someOntherId");
div.setAttribute("class", "ft");
```

这个方法也可以操作自定义特性。因为所有的特性都是属性，所以直接给属性赋值可以设置特性的值。

```js
div.id = "someId";
div.align = "left";
```

但是像下面这样为元素添加自定义属性，该属性不会自动成为元素的特性。

```js
div.myColor = "red";
alert(div.getAttribute("myColor"));//null,  IE除外
```

##### removeAttribute()方法——删除特性

调用该方法可以彻底从元素中清除特性。

#### attributes属性

element类型是使用attributes属性的唯一一个DOM节点类型。attributes属性包含一个NamedNodeMap，与NodeList类似，也是一个“动态的”集合。元素的每个特性都由一个Attr节点表示并保存在NameNodeMap对象中。

NameNodeMap对象拥有下列方法：

- [ ] getNamedItem(name):  返回nodeName属性等于name 的节点。
- [ ] removeNameItem(name):  从列表中移除nodeName属性等于name的节点。
- [ ] setNamedItem(node): 向列表中添加节点，以节点的nodeName属性为索引。
- [ ] item(pos)：返回位于数字pos位置处的节点。

attributes属性中包含一系列节点，每个节点的nodeName就是特性的名称，而节点的nodeValue就是特性的值。

```js
//取得元素的id特性
var id = element.attributes.getNameItem("id").nodeValue;
//简写形式
var id = element.attributes["id"].nodeValue;

//删除元素的id特性
var id = element.attributes.removeNameItem("id");

//添加新特性
element.attributes.setNamedItem(newAttr);
```

迭代元素的每一个特性，然后将他们构造成name = "value" , name = "value"这样的字符串格式：

```js
function outputAttributes(element){
    var pairs = new Array(),
        attrName,
        attrValue,
        i,
        len;
    for(i=0, len=element.attributes.length; i<len; i++){
        attrName = element.attributes[i].attrName;
        attrValue = element.attributes[i].attrValue;
        if(element.attributes[i].specified){//兼容IE，避免IE将未指定的特性返回出来，在IE中所有未指定的特性的specified值为false
            pairs.push(attrName + "=\"" + attrValue + "\");
        }
    }
    return pairs.join(",");
}
//需要注意的是：不同浏览器返回的特性顺序不同；
```

#### 创建元素

document.createElement()方法创建新元素，接受一个参数：要创建元素的标签名。在HTML中不分大小写，在XML中区分大小写。

```js
//创建
var div = document.createElement("div");

//添加特性
div.id = "newDiv";
div.className = "box";

//将新元素添加到文档树中,(可以用
	//appendChild()——向childNodes末尾添加子节点
	//insertBefore()——添加到参考节点的前面（新节点，参考节点）
	//replaceChild()——新节点替换旧节点（新节点，旧节点)
//);
document.body.appendChild(div);
```

在IE中也可以直接传入完整的标签：

```js
var div = document.createElement("<div class=\"box\" id=\"newDiv\"></div>");
//这种方式有助于避免在IE7及更早版本中动态创建元素的某些问题。
//但是这样的用法要求使用浏览器检测，所以不再必要时刻不使用这种方法。
```

#### 元素的子节点

元素的childNodes属性包含了它所有的子节点。

```html
<ul id="myList">
    <li>Item1</li>
    <li>Item2</li>
    <li>Item3</li>
</ul>
```

如果是IE解析这些代码，则ul元素包含3个子节点，即3个li元素 ；如果是其他浏览器解析，则ul包含7个子节点，即3个li，4个空白符。如果向下面这样将空白符删除，则所有浏览器都返回3个子节点：

```html
<ul id="myList"><li>Item1</li><li>Item2</li><li>Item3</li></ul>
```

如果需要通过childNodes遍历子节点，则要先检测nodeType值：

```js
for(var i=0, len = element.childNode.length; i<len; i++){
    if(element.childNodes[i].nodeType == 1){
        //执行某些操作
    }
}
```

### Text类型

文本节点特征：

- [ ] nodeType=3;
- [ ] nodeName="#text";
- [ ] nodeValue=节点所包含文本;
- [ ] parentNode=一个Element

```js
//访问文本子节点
var textNode = div.firstChild;  //或者div.childNodes[0]

//修改文本子节点
div.childNodes[0].nodeValue = "Some other message";
```

#### 创建文本节点

document.createTextNode(要插入节点中的文本)；

```js
var textNode = document.crateTextNode("<div id=\"ID\"></div>");

var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello World!");
element.appendChild(textNode);

document.body.appendChild(element);
```

在某些情况下，也可能包含多个文本节点：

```js
var textNode = document.crateTextNode("<div id=\"ID\"></div>");

var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello World!");//1
element.appendChild(textNode);

var anotherTextNode = document.createTextNode("Hello World too!");//2
element.appendChild(anotherTextNode);

document.body.appendChild(element);

alert(element.childNodes.length);//2

//由于存在相邻文本节点很容易出错，因为分不清哪个文本节点表示哪个字符串。
element.normalize();//将相邻文本合并
alert(element.childNodes.length);//1
alert(element.firstChild.nodeValue);//Hello World!Hello World too!

//分割文本节点————splitText(指定位置分割)，原来文本包含位置之前的内容，新文本包含剩余内容并返回
var newNode = element.firstChild.splitText(5);
alert(element.firstChild.nodeValue);//"Hello"
alert(newNode.nodeValue);//" World!Hello World too!"
alert(element.childNodes.length);//2
```

#### ownerDocument属性

返回元素的根节点

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
</head>
<body>
<p id="demo">单击按钮获取li元素的根文字的节点类型</p>
<button onclick="myFunction()">点我</button>
<script>
function myFunction(){
    var x=document.getElementById("demo");  
    x.innerHTML=x.ownerDocument.nodeType;
}
</script>
</body>
</html>

```

### Comment类型

注释在DOM中是通过Comment类型来表示的。它具有下列特征：

- [ ] nodeType=8;
- [ ] nodeName="%Comment";
- [ ] nodeValue=注释内容；
- [ ] parentNode可能是Document或Element；

comment类型与text类型继承自相同的基型，因此它拥有除splitText()之外的所有字符串操作方法。

注释节点可以通过父节点来访问：

```html
<div id="myDiv">
    <!--A comment -->
</div>
<script>
	var div = document.getElementById("myDiv");
    var comment = div.firstChild;
    alert(comment.data);//"A comment "   相当于comment.nodeValue
</script>
```

另外，可以通过document.createComment()并为其传递注释文本也可以创建注释节点。

```js
var comment = document.createComment("A comment");
```

显然，开发人员很少会创建和访问注释节点，因为注释节点对算法鲜有影响。

### CDATASection类型

只针对基于XML文档，表示CDATA区域。

```xml
<div id="myDiv">
	<![CDATA[This is some content.]]>
</div>
```

可是，四大主流浏览器无一能够这样解析它。

术语 CDATA 指的是不应由 XML 解析器进行解析的文本数据。

CDATA 部分中的所有内容都会被解析器忽略。

CDATA 部分由 "*<![CDATA[*" 开始，由 "*]]>*" 结束：

如果文本包含了很多的"< "字符和"&"字符——就象程序代码一样，那么最好把他们都放到CDATA部件中。

### DocumentType类型

DocumentType包含着与文档的doctype有关的所有信息。

仅有Firefox、Safari和Opera支持它。

它具有下列特征：

- [ ] nodeType=10;
- [ ] nodeName=doctype 的名称；
- [ ] nodeValue=null
- [ ] parentNode=Document。

### DocumentFragment类型

在所有节点中，只有DocumentFragment在文档中没有对应的标记。

虽然不能将文档片段直接添加到文档中，但可以将它作为一个“仓库”来使用，即可以在里面保存将来可能会添加到文档中的节点。

要创建文档片段，可以使用document.createDocumentFragment()方法：

```js
var fragment = document.createDocumentFragment();
```

```html
<ul id="myList">
    
</ul>

<script>
//假设我们要为这个ul添加3个li元素。如果逐个添加会导致浏览器反复渲染新信息。为避免这个问题，可以像下面这样使用一个文档片段来保存创建的列表项，然后再一次性将它们添加到文档中。
    
     var fragment = document.createDocumentFragment();//创建文档片段
     var ul = ducument.getElementById("myList");
     var li = null;
 
    for(var i=0; i<3; i++){
        li = document.createElement("li");
        li.appendChild(document.createTextNode("Item"+(i+1)));
        fragment.appendChild(li);
    }
    ul.appendChild(fragment);
</script>
```

### Attr类型

元素的特性在DOM中以Attr类型来表示。

从技术角度来讲，特性就是存在于元素的attributes属性中的节点。

尽管它们也是节点，但特性却不被认为是DOM文档树的一部分。

开发人员最常使用的是：

​	getAttribute()：取得特性值

​	setAttribute()：设置特性值

​	removeAttribute()：删除特性值

## DOM操作技术

### 动态脚本

动态脚本指的是在页面加载时不存在，但将来的某一时刻通过修改DOM动态添加的脚本。

两种创建动态脚本的方式：插入js文件和直接插入代码。

```js
function loadScript(url){
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = url;
    document.head.appendChild(script);
}
//就可以通过调用这个函数来加载外部文件了
loadScript("client.js");
```

另一种指定JavaScript代码的方式是行内方式：

```js
//封装一个函数来处理兼容性问题
function loadScriptString(code){
    var script = document.createElement("script");
    script.type = "text/javascript";
    try{
        script.appendChild(document.createTextNode(code));//大多数浏览器都支持
    } catch(ex) {
        script.text = code;//由于IE将script标签视为特殊元素，不允许DOM访问其子节点，所以要这样写；以及兼容Safari3之前的版本。
    }
    document.body.appendChild(script);
}

//调用这个函数
loadScriptString("function sayHi() { alert('Hi') }");
```

以这种方式加载的代码会在全局作用域中执行，而且当脚本执行后将立即可用。**实际上，这与在全局作用域中把相同的字符串传递给eval()是一样的。**

### 动态样式

```js
//第一种方式（加载外部文件）
function loadStyles(url){
    var link = document.createElement("link");
    link.rel = "stylesheet";
    link.type = "text/css";
    link.href = url;
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(link);
};

loadStyles("styles.css");

//第二种方式（加载css字符串）
function loadStyleString(css){
    var style = document.createElement("style");
    style.type = "text/css";
    try{
        style.appendChild(document.createTextNode(css));
    } catch(ex) {
        style.text = css;//由于IE也将style标签视为特殊元素，不允许DOM访问其子节点
    };
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(style);
}

loadStyleString("#div1{width: 100px; height: 100px; background: red;}");
```

必须将link标签放到head中。

加载外部样式文件的过程是异步的，也就是加载样式与执行JavaScript代码的过程没有固定的次序。

### 操作表格

为了方便构建表格，HTML DOM添加了一些属性和方法：

为<table> 元素添加属性和方法如下：

- caption：保存着对<caption>元素（如果有）的指针。
- tBodies:  是一个对<tbody>元素的HTMLCollection（表示一个包含了元素的通用集合，可用方括号访问其节点。）
- tFoot:  保存着对<tfoot>元素（如果有）的指针。
- tHead:  保存着对<thead>元素（如果有）的指针。
- rows:  是一个表格中所有行的HTMLCollection。
- createTHead():  创建<thead>元素，将其放到表格中，返回引用。
- createTFoot():   创建<tfoot>元素，将其放到表格中，返回引用。
- createCaption():  创建<caption>元素，将其放到表格中，返回引用(表格标题)。
- deleteTHead():  删除<thead>元素。
- deleteTFoot():  删除<tfoot>元素。
- deleteCaption()：删除<caption>元素。
- deleteRow(pos):  删除指定位置的行。
- insertRow(pos): 向指定位置插入行。

为<tbody>元素添加属性和方法如下：

- rows:  保存着<tbody>元素中行的HTMLCollection。
- deleteRow(pos):  删除指定位置的行。
- insertRow(pos): 向指定位置插入行，并返回引用。

为<tr>元素添加的属性和方法如下：

- cells: 保存着<tr>元素中单元格的HTMLCollection。
- deleteCell(pos):  删除指定位置的单元格。
- insertCell(pos):  向cells集合中的指定位置插入一个单元格，返回对新插入的单元格的引用。

利用这些属性和方法，可以极大地减少创建及修改表格所需的代码数量。例如：

```html
<table border="1" width="100%">
    <tbody>
        <tr>
        	<td>Cell 1,1</td>
            <td>Cell 1,2</td>
        </tr>
        <tr>
        	<td>Cell 2,1</td>
            <td>Cell 2,2</td>
        </tr>
    </tbody>
</table>
```

用上面的方法来创建这个表格：

```js
//创建table表格
var table = document.createElement("table");
table.border = "1";
table.width = "100%";

//创建tbody
 var tbody = document.createElement("tbody");
table.appendChild(tbody);

//创建第一行，以及里面的节点
tbody.insertRow(0);
tbody.rows[0].insertCell(0);
tbody.rows[0].cells[0].appendChild(createTextNode("Cell 1,1"));
tbody.rows[0].insertCell(1);
tbody.rows[0].cells[1].appendChile(createTextNode("Cell 1,2"));

//创建第二行，以及里面的节点
tbody.insertRow(1);
tbody.rows[1].insertCell(0);
tbody.rows[1].cells[0].appendChild(createTextNode("Cell 2,1"));
tbody.rows[1].insertCell(1);
tbody.rows[1].cells[1].appendChild(createTextNode("Cell 2,2"));

//将创建好的表格插入到body中
document.body.appendChild(table);
```

### 使用NodeList

**要理解NodeList及其近亲  NamedNodeMap和HTMLCollection，是从整体上理解DOM的关键之所在。**

**这三个集合都是动态的。**

换句话说，**每当文档结构发生变化时，它们都会得到更新。因此它们始终保存着最新的、最准确的信息**。

**从本质上说，所有NodeList对象都是在访问DOM文档时运行的查询。**

一般来说，**应该尽量减少访问NodeList的次数**。因为每次访问NodeList，都会运行一次基于文档结构的查询。所以，**可以考虑将从NodeList中取得 的值缓存起来**。

## 小结

理解DOM的关键，就是理解DOM对性能 的影响。DOM操作往往是JavaScript中开销最大的部分，而因访问NodeList导致的问题最多。NodeList对象都是“动态的”，这就意味着每次访问NodeList对象，都会运行一次查询。

有鉴于此，最好的办法就是尽量减少DOM操作。

# 第十一章 DOM扩展

对DOM的两个主要的扩展是Selectors API和HTML5。

## 选择符API

众多JavaScript库中最常用的一项功能。

### querySelector()方法

它接受一个CSS选择符，**返回与该模式匹配的第一个元素**，如果没找到匹配元素，返回null。

```js
//取得body元素
var body = document.querySelector("body");

//取得ID为 myDiv 的元素
var myDiv = document.querySlector("#myDiv");

//取得类为 selected 的第一个元素
var selected = document.querySlector(".selected");

//取得类为 button 的第一个图像元素
var button = document.querySelector("img.button");
```

### querySelectorAll()方法

它也接受一个CSS选择符，但**返回的是所有匹配的元素**，而不仅仅是第一个元素。这个方法返回的是一个NodeList的实例。

具体来说，返回的值实际上是带有所有属性和方法的NodeList，其底层实现则类似于一组元素的快照，而非不断对文档进行搜索的动态查询。这样做可以避免NodeList的大多数性能问题。

与querySelector()类似，能调用querySlectorAll()的类型包括document、documentFragment和Element。

```js
//取得某div中所有的em元素（类似于getElementsByTagName("em");
var ems = document.getElementById("myDiv").querySlectorAll("em");

//取得类为 selected 的所有元素
var sellecteds = document.querySelectorAlll(".selected");

//取得所有p中的所有 strong 元素
var strongs = document.querySelectorAll("p strong");
```

### matchesSelector()方法

匹配选择器

Element类型新增了一个方法matchesSelector()。

接受一个css选择符，如果调用元素与该选择符匹配，返回true；否则返回false。

```js
if(document.body.matchesSelector("body.page1")){
    //true
}

//由于各浏览器兼容性问题，最好编写一个包装函数
function matchesSlector(element, selector){
    if(element.matchesSelector){
        return element.matchesSelector(selector);
    } else if (element.msMatchesSelector){//IE 9+
        return element.msMatchesSelector(selector);
    } else if (element.mozMatchesSelector){//FireFox 3.6+
        return element.mozMatchesSelector(selector);
    } else if (element.webkitMatchesSelector){//Safari5+ && Chrome
        return element.webkitMatchesSelector(selector);
    } else {
        throw new Error("Not supported.");//throw语句创建自定义错误
    }
}

if(matchesSelector(document.body, "body.page1")){
    //执行操作
}
```

## 元素遍历

对于元素间的空格，IE9及之前的版本不会返回文本节点，而其他所有浏览器都会返回文本节点。鉴于这个问题：

Element Traversal API 为DOM元素添加了一下5个属性：

- childElementCount：返回子元素（不包括文本节点和注释节点）的个数。
- firstElementChild：指向第一个子元素； firstChild的元素版本。
- lastElementChild：指向最后一个子元素；lastChild的元素版本。
- previousElementSibling：指向前一个同辈元素；previousSibling的元素版本。
- nextElementSibling：指向后一个同辈元素；nextSibling的元素版本。

利用这些元素不必担心空白文本节点，从而更方便的查找DOM元素。

过去，跨浏览器遍历某个元素的所有子元素，需要这样编写代码：

```js
var i,
    len,
    child = element.firstChild;
while(child != element.lastChild){
    if(child.nodeType == 1){//检查是不是元素节点
            processChild(child);
    }
    child = child.nextSibling;
}
```

而使用新增元素，代码会更简洁：

```js
var i,
    len,
    child = element.firstElementChild;
while(child != element.lastElementChild){
    processChild(child);	//已知其是元素
    child = nextElementSibling;
}
```

## HTML5

HTML5规范则围绕如何使用新增标记定义了大量的JavaScriptAPI。

### 与类相关的扩充

#### getElementsByClassName()方法

接受一个包含一或多个类名的字符串作为参数，返回带有指定类的所有元素的NodeList。

```js
//取得所有类中包含 username 和current 的元素，类名的先后顺序无所谓
var allCurrentUsernames = document.getElementsByClassName("username current");

//取得ID为 myDiv 的元素中带有类名 selected 的所有元素
var selecteds = document.getELementById("myDiv").getElementsByClassName("selected");
```

因为返回的对象是NodeList（实时更新的对象），所以也会有性能问题。

#### classList属性

HTML5新增了一种操作类名的方式，那就是为所有元素添加classList属性，**表示元素的所有class值的集合**，它是新集合类型**DOMTokenList**的实例。

这个新类型有如下方法：

- add(value):  将给定的字符串值添加到列表中。如果值已经存在，则不添加。
- contains(value): 表示列表中是否存在给定的值，如果存在返回true，否则返回false。
- remove(value)：从列表中删除给定的字符串。
- toggle(value):  如果列表中有给定的值，删除它；如果没有，添加它。

```js
//删除 disable 类
div.classList.remove("disable");

//添加 current 类
div.classList.add("current");

//切换 user 类
div.classList.toggle("user");

//确定元素中是否包含既定的类名
if(div.classList.contains("bd") && !div.classList.contains("disable")){
    //执行操作
}

//迭代类名
for(var i=0, len=div.classList.length; i<len; i++){
    dosomething(div.classList[i]);
}
```

暂时支持classList 的浏览器有FireFox3.6+  和 Chrome。

### 焦点管理

#### document.activeElement属性

**这个属性始终会引用DOM中当前获得了焦点的元素**。元素获得焦点的方式有页面加载、用户输入和在代码中调用focus()方法。

```js
var button = document.getElementById("myButton");
button.focus();
alert(document.activeElement === button);  //true
```

默认情况下，文档刚刚加载完成时，document.activeElement中保存的是document.body元素的引用。

文档加载期间，document.activeElement的值为null。

#### document.hasFocus()方法

这个方法用于确定文档是否获得焦点。

```js
var button = document.getElementById("myBtn");
button.focus();
alert(document.hasFocus());		//true
```

通过检测文档是否获得了焦点，可以知道用户是不是在与页面交互。

这两个功能最重要的用途是提高Web应用的无障碍性。

### HTMLDocument的变化

#### readyState属性

它有两个值：

- loading，正在加载文档；
- complete，已经加载完文档。

使用它最恰当的方式，就是通过它来实现一个指示文档已经加载完成的指示器。

#### 兼容模式

由于IE的兼容性问题，IE给document添加了一个compatMode属性，这个属性就是为来告诉过开发人员浏览器采用了那种渲染模式。

- 标准模式下：compatMode == "CSS1Compat"
- 混杂模式下：compatMode == "BackCompat"

```js
if(document.compatMod == "CSS1Compat"){
    alert("Standards mode");
} else {
    alert("Quirks mode");
}
```

#### head属性

HTML5新增了document.head 属性，引用文档的<head>元素。

```js
var head = document.head || document.getElementsByTagName("head")[0];//优化处理，加一个后备方法
```

实现document.head 的浏览器暂时有Safari5 和Chrome。

### 字符集属性

HTML5新增属性：

- charset：它表示文档中实际使用的字符集。默认为 UTF-16。
- defaultCharset：它表示根据浏览器及操作系统的设置，当前文档默认的字符集应该是什么

```js
if(document.charset != document.defaultCharset){
    alert("字符集有误");
}
```

### 自定义数据属性

**HTML5规定可以为元素添加非标准的属性，但要添加前缀data-**，目的是为元素提供与渲染无关的信息，或者提供语义信息。

这些属性可以任意添加、随便命名，只要以data-开头即可

```html
<div id="myDiv" data-appId="21345" data-myName="Nicholas">
    
</div>
```

添加了自定义属性后，可以通过元素的**dataset**属性来访问自定义属性的值。

dataset属性的值是DOMStringMap的一个实例，也就是一个名值对儿的映射。

在这个映射中，每个data-name形式的属性都会有一个对应的属性，只不过属性名没有data-的前缀。

看一个例子：

```js
var div = document.getElementById("myDiv");

//取得自定义属性的值
var appId = div.dataset.appId;
var myName = div.dataset.myName;

//设置值
appId = "23456";
myName = "Michael";

//是否有效呢？
if(div.dataset.myName){
    alert("Hello," + div.dataset.myName);
}
```

支持这个属性的浏览器有Firefox6+ 和Chrome。

### 插入标记

#### innerHTML属性

- 在读模式下，innerHTML属性返回与调用元素**下**的所有子节点对应的HTML标记。
- 在写模式下，innerHTML属性会根据指定的值创建新的DOM树，然后这个DOM树完全替换调用元素原先的所有子节点。

使用innerHTML属性也有一些限制。

比如，在大多数浏览器中，通过innerHTML插入\<script\>元素并不会执行其中的脚本。IE8及更早版本是唯一能够在这种情况下执行脚本的浏览器。但是需要两个条件：

```js
//一是必须要有defer属性
//二是<script>必须在有作用域元素之后

div.innerHTML = "_<script defer>alert("Hi")<\/script>";
div.innerHTML = "<div>&nbsp;</div><script defer>alert("hi")<\/script>";
div.innerHTML = "<input type=\"hidden\"><script defer>alert("Hi")<\/script>";

//前两个方法会影响布局，第三个方法是首选。
```

\<style\>元素也同上。

建议读者在通过innerHTML插入代码之前，尽可能先手工检查一下其中的文本内容。

#### outerHTML属性

- 在读模式下，outerHTML属性返回调用它的元素**及**所有子节点的HTML标签。
- 在写模式下，outerHTML属性会根据指定的字符串创建新的DOM子树，然后用DOM子树完全替换**调用元素**
- 与innerHTML的区别是它包含调用它的元素本身，而非仅仅调用元素的所有子元素。

#### insertAdjacentHTML()方法

它接受两个参数：插入位置和要插入的HTML文本，第一个参数必须是下列值之一：

- beforebegin：在当前元素之前插入一个紧邻的同辈元素。
- afterbegin：在当前元素之下插入一个新的子元素或再第一个子元素之前再插入新的子元素（取决于它是否有子元素）。
- beforeend：在当前元素之下插入一个新的子元素或再最后一个子元素之前再插入新的子元素（取决于它是否有子元素）。
- afterend：在当前元素之后插入一个紧邻的同辈元素。

第二个参数是一个HTML字符串。

```js
//作为前一个同辈元素插入
element.insertAdjacentHTML("beforebegin", "<p>Hello world!</p>");

//作为第一个子元素插入
element.insertAdjacentHTML("afterbegin","<p>Hello world!</p>");

//作为最后一个子元素插入
element.insertAdjacentHTML("beforeend","<p>Hello world!</p>");

//作为后一个同辈元素插入
element.insertAdjacentHTML("afterend","<p>Hello world!</p>");
```

#### 内存与性能问题

使用本节介绍的方法替换子节点会导致浏览器内存的占用问题，尤其是在IE中，问题更加明显。

在删除带有事件处理程序或引用了其他JavaScript对象子树时，就有可能导致内存占用的问题。

**因此在使用innerHTML、outerHTML属性和insertAdjacentHTML()方法时，最好要手动删除要被替换的元素的所有事件处理程序和JavaScript对象的属性。**

最好将设置innerHTML和outerHTML的次数控制在合理的范围内。

例如：

```js
for(var i=0,len=values.length; i<len; i++){
    ul.innerHTML = "<li>" + values[i] +"</li>";//要避免这种频繁的操作
}

//改进后的代码：
var itemsHtml = "";
for(var i=0,len=values.length; i<len; i++){
    itemsHtml += "<li>" + values[i] + "</li>";
}
ul.innerHTML = itemsHtml;
```

### scrollIntoView()方法

scrollIntoView()可以在所有HTML元素上调用，**通过滚动浏览器窗口或某个容器元素，调用元素就可以出现在视口中。**

它接受 两个参数：

- true：或者不传参数，那么窗口滚动之后会让元素顶部与视口顶部尽可能平齐；
- false：调用元素会尽可能全部出现在视口中，不过顶部不一定平齐。

当页面发生变化时，一般用这个方法来吸引用户注意力。

支持scrollIntoView()方法的浏览器有IE、Firefox、Safari和Opera。（根据测试Chrome也支持此方法）

## 专有扩展

### 文档模式

IE8引入的新概念叫“文档模式”。

到了IE9，一共有4种文档模式：

- IE5：混杂模式
- IE7：IE7标准模式
- IE8：IE8标准模式
- IE9：IE9标准模式

要强制浏览器以某种模式渲染页面，可以使用HTTP头部信息X-UA-Compatible，或通过等价的\<meta\>标签来设置：

```html
<meta http-rquiv="X-UA-Compatible" centeent="IE=IEVersion"
```

这里的IE版本有以下 一些不同的值：

- Edge：始终以最新模式渲染页面，忽略文档类型声明。
- EmulateIE9：如果有文档类型声明，则以IE9标准模式渲染页面，否则将文档模式设置为IE5.
- EmulateIE8：如果有文档类型声明，则以IE8标准模式渲染页面，否则将文档模式设置为IE5.
- EmulateIE7：如果有文档类型声明，则以IE7标准模式渲染页面，否则将文档模式设置为IE5.
- 9：强制以IE9标准模式渲染页面，忽略文档类型声明。
- 8：强制以IE8标准模式渲染页面，忽略文档类型声明。
- 7：强制以IE7标准模式渲染页面，忽略文档类型声明。
- 5：强制将文档模式设置为IE5，忽略文档类型声明。

```html
<!--比如，IE7中-->
<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE7">

<!--强制为IE7标准模式-->
<meta http-equiv="X-UA-Compatible" content="IE=7"
```

IE8新增属性：通过document.documentMode属性可以知道给定的页面使用的是什么文档模式，他会返回使用的文档模式的版本号。

```js
var mode = document.documentMode;
```

### Children属性

由于IE9之前的版本与其他浏览器在处理文本节点中的空白符时有差异，因此就出现了children属性。

这个属性是HTMLCollection的实例，只包含元素中同样还是元素的子节点。

```js
var childrenCount = element.children.length;
var firstChild = element.children[0];
```

IE8及更早版本的children属性中也会包含注释节点 ，但**IE9之后的版本则只返回元素节点。**

### contains()方法

**用于检查括号中的节点是否是调用这个方法的节点的子节点**。是返回true，否则返回false。（不通过DOM）

```js
alert(document.documentElement.contains(document.body));  //true
```

### conpareDocumentPosition()方法

用来确定两个节点之间的关系。返回掩码值：

| 掩码 | 节点关系 |
| ---- | -------- |
| 1    | 无关     |
| 2    | 居前     |
| 4    | 居后     |
| 8    | 包含     |
| 16   | 被包含   |

使用一些浏览器能力检测，就可以写出如下所示的一个通用的contains函数：

```js
function contains(refNode, otnerNode){
    if(typeof refNode.contains == "function" && (!client.engine.webkit || client.engine.webkit >= 522)){
        return refNode.contains(otherNode);
    } else if (typeof refNode.compareDocumentPosition == "function"){
        return !!(refNode.compareDocumentPosition(otherNode) & 16);
    } else {
        var node = otherNode.parentNode;
        do{
            if(node === refNode){
                return true;
            } else {
                node = node.parentNode;
            }
        }while(node !== null);
        return false;
    }
}
```

## 插入文本

### innerText属性

**通过innerText属性可以操作元素中包含的所有文本**。

**textContent属性也有类似的作用。但是两个属性浏览器兼容性不同，所以有必要写两个函数来检测属性和修改属性：**

```js
function getInnerText(element){
    return (typeof element.textContent == "string") ? element.textcontent : element.innerText;
}

function setInnerText(element, text){
    if(typeof element.textContent == "string"){
        element.textContent = text;
    } else {
        element.innerText = text;
    }
}

//调用该方法
setInnerText(div, "Hello world!");
alert(getInnerText(div));		//"Hello world!"
```

### outerText属性

除了作用范围扩大到了**包含调用它的节点**之外，outerText与innerText基本上没有多大区别。

## 滚动

下面几个方法是对HTMLElement类型的扩展：

- scrollIntoViewIfNeeded(alignCenter):  只有在当前元素在视口中不可见的情况下，才滚动浏览器窗口或容器元素，最终让它可见。
- scrollByLines(lineCount)：将元素的内容滚动指定的行高，lineCount可正可负。
- scrollByPages(pageCount)：将元素的内容滚动指定的页面高度。

这三个方法都是只有Safari和Chrome实现了。

**由于scrollIntoView()是唯一一个所有浏览器都支持的方法，因此还是这个方法最常用。**

## 插话：js如何实现重载

**所谓重载，就是一组相同的函数名，有不同个数的参数，在使用时调用一个函数名，传入不同参数，根据你的参数个数，来决定使用不同的函数！但是我们知道js中是没有重载的，因为后定义的函数会覆盖前面的同名函数，但是我们又想实现函数重载该怎么办呢？**

**第一种方法：**

　　这种方法比较简单，给一个思路，大家肯定都能理解，就是函数内部用switch语句，根据传入参数的个数调用不同的case语句，从而功能上达到重载的效果。

　　这种方法简单粗暴。但是对于一个正在学习js的人来说，这种方法未免太敷衍了。

　　下面重点介绍一下第二种，老实说我看的时候很吃力看了一个小时才捋清楚，因为有的知识点虽然看过了但是不熟悉。这次就给我上了一课，教会了我很多东西。

**第二种方法：**

　　我们这个例子，是如果你不传入参数，就会输出所有的人，输入firstname，就会输出匹配的人，如果输入全名的人，也会输出匹配的人。如果用重载的话，用户体验确实会很好（这个例子是我学习时从网上扒下来的，很有代表性，但是他们都没有写实现过程，我来和大家谈论一下实在的东西）

```js
function method(obj,name,fnc){
    var oldMethod = obj[name];
    console.log('oldMethod = ', oldMethod);
    obj[name] = function () {
        console.log('fnc.length = ', fnc.length);
        console.log('arguments.length = ', arguments.length);
        if(fnc.length === arguments.length){
            return fnc.apply(this, arguments);
        } else if(typeof oldMethod === "function") {
            return oldMethod.apply(this, arguments);
        }
    }
};
//函数的长度就是定义形参的个数,我们可以利用这一点来写重载函数。
        //call、apply和bind的区别是：call第二个及以后的参数接受的是和一个参数列表，而apply接受的是参数数组。而bind()方法调用并改变函数运行时上下文后，返回一个新的函数，供我们需要时再调用。

var people = {
    values: ["zhang san", "li si"]//values在这里
};
method(people, "find", function () {
    console.log("无参数");
    return this.values;//返回全部
});
//上面method调用完成后，people.find就是一个函数了，但是两个if条件都不满足，所以是一个空函数
method(people,"find",function (firstName) {
    console.log("一个参数");
    var ret = [];
    for(var i=0; i<this.values.length; i++){
        if(this.values[i].indexOf(firstName) == 0){//indexOf()方法会返回某个字符串在大字符串中首次出现的位置
            ret.push(this.values[i]);
        }
    }
    return ret;
});
//由于已经调用过了method方法一次，所以再一次调用的时候people.find已经是一个函数，并赋值给了oldMethod，然后给people.find赋值一个新的函数，if(1 ==== 3){}else if(true){返回老函数在此执行的结果}
method(people, "find", function (firstName, lastName){
    console.log("两个参数");
    var ret = [];
    for(var i=0; i<this.values.length; i++){
        if(this.values[i] == firstName + " " + lastName){
            ret.push(this.values[i]);
        }
    }
    return ret;
});

console.log(people.find());				//["Zhang san","Li si"]
console.log(people.find("Zhang"));		//["Zhang san"]
console.log(people.find("Li"));			//["Li si"]
console.log(people.find("Zhang","Li"));	//["Zhang san","Li si"]
console.log(people.find("san"));		//[]
```

~~~js
//将method函数添加一点标记更容易理解
function method(obj,name,fnc){
    var oldMethod = obj[name];
    console.log('oldMethod = ', oldMethod);
    obj[name] = function () {
        console.log('fnc.length = ', fnc.length);
        console.log('arguments.length = ', arguments.length);
        console.log('fnc = ', fnc.toString());
        
        if(fnc.length === arguments.length){
            return fnc.apply(this, arguments);
        } else if(typeof oldMethod === "function") {
            return oldMethod.apply(this, arguments);
        }
    }
};
undefined

var people = {
    values: ["zhang san", "li si"]
};
undefined

method(people, "find", function () {
    console.log("无参数");
    return this.values;//返回全部
});
VM1566:3 oldMethod =  undefined//此时oldMethod还是undefined
undefined

method(people,"find",function (firstName) {
    console.log("一个参数");
    var ret = [];
    for(var i=0; i<this.values.length; i++){
        if(this.values[i].indexOf(firstName) == 0){
            ret.push(this.values[i]);
        }
    }
    return ret;
});
VM1566:3 oldMethod =  ƒ () {//现在oldMethod已经是一个函数，虽然看起来每个oldMethod都是相同的代码，但是其引用着保存这段代码的时候的fnc,arguments和oldMethod，也就是说，这才是闭包的所在，当时的作用域没有销毁。
        console.log('fnc.length = ', fnc.length);
        console.log('arguments.length = ', arguments.length);
        console.log('fnc = ', fnc.toString());
    
        if(fnc.length === arguments.length){
            return fnc.apply(this, arguments);
        } else if(typeof oldMethod === "function") {
            return oldMethod.apply(this, arguments);
        }
…
undefined
method(people, 'find', function (firstName, lastName) {
    console.log('两个参数');
    var ret = [];
    for(var i = 0; i < this.values.length; i ++) {
        if(this.values[i] == firstName + lastName) {
            ret.push(this.values[i]);
        }
    }
    return ret;
});
VM1566:3 oldMethod =  ƒ () {//解释同上
        console.log('fnc.length = ', fnc.length);
        console.log('arguments.length = ', arguments.length);
        console.log('fnc = ', fnc.toString());
    
        if(fnc.length === arguments.length){
            return fnc.apply(this, arguments);
        } else if(typeof oldMethod === "function") {
            return oldMethod.apply(this, arguments);
        }
…
undefined
people.find();
//第一步从上面保存的作用域栈的最顶端开始一次向下查找符合的作用域，也就是先从两个参数的那个作用域开始判断（因为它是最后一个进栈的），不符合if，走else if来翻上一个保存的作用域，也就是栈里倒数第二个进入的作用域，不符合if，继续走else if来翻上一个保存的作用域，也就是栈里倒数第三个进入的作用域，符合if条件，则执行这个作用域里所保存的fnc函数，完成。
VM1566:5 fnc.length =  2
VM1566:6 arguments.length =  0
VM1566:8 fnc =  function (firstName, lastName) {
    console.log('两个参数');
    var ret = [];
    for(var i = 0; i < this.values.length; i ++) {
        if(this.values[i] == firstName + lastName) {
            ret.push(this.values[i]);
        }
    }
    return ret;
}
VM1566:5 fnc.length =  1
VM1566:6 arguments.length =  0
VM1566:8 fnc =  function (firstName) {
    console.log("一个参数");
    var ret = [];
    for(var i=0; i<this.values.length; i++){
        if(this.values[i].indexOf(firstName) == 0){
            ret.push(this.values[i]);
        }
    }
    return ret;
}
VM1566:5 fnc.length =  0
VM1566:6 arguments.length =  0
VM1566:8 fnc =  function () {
    console.log("无参数");
    return this.values;//返回全部
}
VM1576:2 无参数
(2) ["zhang san", "li si"]

//测试了一下一个参数的情况
people.find('zhang san');
VM1566:5 fnc.length =  2
VM1566:6 arguments.length =  1
VM1566:8 fnc =  function (firstName, lastName) {
    console.log('两个参数');
    var ret = [];
    for(var i = 0; i < this.values.length; i ++) {
        if(this.values[i] == firstName + lastName) {
            ret.push(this.values[i]);
        }
    }
    return ret;
}
VM1566:5 fnc.length =  1
VM1566:6 arguments.length =  1
VM1566:8 fnc =  function (firstName) {
    console.log("一个参数");
    var ret = [];
    for(var i=0; i<this.values.length; i++){
        if(this.values[i].indexOf(firstName) == 0){
            ret.push(this.values[i]);
        }
    }
    return ret;
}
VM1582:2 一个参数
["zhang san"]
~~~



**思路**：这段代码第一眼看到我是懵逼的，再看有点思路，再看又懵了。这种方法巧妙的运用了闭包原理，既然js后面的函数会覆盖前面的同名函数，**我就强行让所有的函数都留在内存里，等我需要的时候再去找它。**有了这个想法，是不是就想到了闭包，函数外访问函数内的变量，从而使函数留在内存中不被删除。就是闭包。

**实现过程**：我们看一下上面这段代码，最重要的是method方法的定义：这个方法中最重要的一点就是这个oldMethod，这个oldMethod真的很巧妙。它的作用相当于一个指针，指向上一次被调用的method函数，这样说可能有点不太懂，我们根据代码来说，js的解析顺序从上到下为。

　　1.解析method（先不管里面的东西）

　　2.method(people,"find",function()  执行这句的时候，它就回去执行上面定义的方法，然后此时oldMethod的值为空，因为你还没有定义过这个函数，所以它此时是undefined，然后继续执行，这时我们才定义 obj[name] = function()，然后js解析的时候发现返回了fnc函数，更重要的是fnc函数里面还调用了method里面的变量，这不就是闭包了，因为fnc函数的实现是在调用时候才会去实现，所以js就想，这我执行完也不能删除啊，要不外面那个用啥，就留着吧先（此处用call函数改变了fnc函数内部的this指向）

　　3.好了第一次method的使用结束了，开始了第二句，method(people,"find",function(firstname) 然后这次使用的时候，又要执行oldMethod = obj[name]此时的oldMethod是什么，是函数了，因为上一条语句定义过了，而且没有删除，那我这次的oldMethod实际上指向的是上次定义的方法，它起的作用好像一个指针，指向了上一次定义的 obj[name]。然后继续往下解析，又是闭包，还得留着。

　　4.第三次的method调用开始了，同理oldMethod指向的是上次定义的 obj[name] 同样也还是闭包，还得留着。

　　5.到这里，内存中实际上有三个 obj[name]，因为三次method的内存都没有删除，这是不是实现了三个函数共存，同时还可以用oldMethod将它们联系起来是不是很巧妙

　　6.我们 people.find() 的时候，就会最先调用最后一次调用method时定义的function，如果参数个数相同 也就是  arguments.length === fnc.length 那么就执行就好了，也不用找别的函数了，如果不相同的话，那就得用到oldMethod了  return oldMethod.apply(this,arguments); oldMethod指向的是上次method调用时定义的函数，所以我们就去上一次的找，如果找到了，继续执行 arguments.length === fnc.length  如果找不到，再次调用oldMethod 继续向上找，只要你定义过，肯定能找到的，对吧！

　　总结：**运用闭包的原理使三个函数共存于内存中，oldMethod相当于一个指针，指向上一次定义的function，每次调用的时候，决定是否需要寻找。**

![重载](E:\TyporaFiles\img\重载.png)

执行过程很容易说明这一点：首先第一次调用的时候 oldMethod肯定不是函数，所以instance判断是false，继续调用的话就会为true。然后，我们调用method的顺序，是从没有参数到两个参数，所以我们最先调用find方法，是最后一次method调用时定义的，所以fnc的length长度是2.然后向上找，length为1，最后终于找到了length为0的然后执行，输出。

# 第十二章 DOM2和DOM3

## DOM变化

DOM有很多的API，**可以通过以下代码来确定浏览器是否支持这些DOM模块**：

```js
var supportsDOM2Core = document.implementation.hasFeature("Core", "2.0");
var supportsDOM3Core = document.implementation.hasFeature("Core", "3.0");
var supportsDOM2Views = document.implementation.hasFeature("Views", "2.0");
```

### HTML  XHTML  XML的区别

XML是可扩展标记语言，为文档的创建、结构化的存储和编码提供了规则。它是一种语法，或者说是一个编写规范，它定义如何写数据，而不是写什么数据。

HTML是超文本标记语言，设计HTML的目的是创建结构化的文档，提供文档的语义。

XHTML是基于XML的HTML，作用与HTML相同，确定文档结构的markup规则与XML相同。与HTML相比它的语法规则更加严格。

### 针对XML命名空间的变化

XML命名空间：是提供避免元素命名冲突的方法。

从技术上说，HTML不支持XML命名空间，但XHTML支持XML命名空间。

命名空间要使用xmlns特性来指定。XHTML的命名空间是http://www.w3.org.1999//xhtml，在任何格式良好的XHTML页面中，都应该将其包含在<html>元素中。

```xml
<html xmlns="http://www.w3org/1999/xhtml">
	<head>
    	<title>Example XHTML page</title>
    </head>
    <body>
    	Hello World!
    </body>
</html>
```

在只基于XML文档的情况下，命名空间实际上也没有什么用。不过，在混合使用两种语言的情况下，命名空间的用处就非常大了。

下面是XHTML和SVG语言（可缩放矢量图形）的文档：

```xml
<html xmlns="http://www.w3.org/1999/xhtml">
	<head>
    	<title>Example XHTML page</title>
    </head>
    <body>
    	<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
             viewBox="0 0 100 100" style="width:100%; height:100%">
        	<rect x="0" y="0" width="100" style="fill:red"/>
        </svg>
    </body>
</html>
```

在这个例子中，通过设置命名空间，将<svg>标识为了与包含文档无关的元素。

**此时，<svg>中的所有元素都被认为属于http://www.w3.org/2000/svg命名空间。即使这个文档从技术上说是一个XHTML文档，但因为有了命名空间，其中的SVG代码仍然也是有效的。**

#### 命名冲突

在XML中，元素名称是由开发者定义的，当两个不同的文档使用相同的元素名时，就会发生命名冲突。

```xml
<table>
	<tr>
    	 <td>Apples</td>
   		 <td>Bananas</td>
    </tr>
</table>
```

```xml
<table>
	<name>African Coffee Table</name>
    <width>80</width>
    <length>120</length>
</table>
```

假如这两个xml文档被一起使用，<table>元素就会发生命名冲突，xml解析器无法确定如何处理这类冲突。

使用前缀来避免这类冲突：

```xml
<h:table>
	<h:tr>
    	<h:td>Apples</h:td>
        <h:td>Bananas</h:td>
    </h:tr>
</h:table>

<f:table>
	<f:name>African Coffee Table</f:name>
    <f:width>80</f:width>
    <f:length>120</f:length>
</f:table>
```

通过使用前缀，我们创建了两个不同类型的table元素。

**在js中，由于一个项目都是由多个前端工程师共同完成，所以也会有命名冲突的问题，解决办法就是将每个人的变量以及方法都放在自己的对象中，引用的时候引用自己对象中的变量名以及方法名。**

#### Node类型的变化

在DOM2级中，Node类型包含下列特定于命名空间的属性：

- localeName：不带命名空间前缀的节点名称。
- namespaceURI：命名空间的URI或null。
- prefix：命名空间前缀或者null。

以下面文档为例：

```xml
<html xmlns="http://www.w3.org/1999/xhtml">
	<head>
    	<title>Example XHTML page</title>
    </head>
    <body>
    	<s:svg xmlns="http://www.w3.org/2000/svg" version="1.1"
             viewBox="0 0 100 100" style="width:100%; height:100%">
        	<s:rect x="0" y="0" width="100" style="fill:red"/>
        </s:svg>
    </body>
</html>
```

对于<html>而言：localeName=tagName="html"。    namespaceURI="http://www.w3/org/1999/xhtml"。    prefix=null。

对于<s: svg>而言:  localeName="svg"   namespaceURI="http://www.w3.org/2000/svg"    prefix="s"

DOM3在此基础上又引入了下列方法：

- isDefaultNamespace(namespaceURI):  在指定的namespaceURI是当前节点的默认命名空间的情况下返回true。
- lookupNamespaceURI(prefix):  返回给定prefix的命名空间。
- lookupPrefix(namespaceURI)：返回namespaceURI的前缀。

针对前面的例子：

```js
alert(document.body.isDefaultNamespace("http://www.w3.org/1999/xhtml"));  //true

alert(svg.lookupPrefix("http://www.w3.org/2000/svg"));  //"s"

alert(svg.lookupNamespaceURI("s"));  //"http://www.w3.org.2000.svg"
```

在取得一个节点，但不知道该节点与文档其他元素之间的关系的情况下，这些方法是很有用的。

#### Document类型的变化

DOM2级中包含下列方法：

- createElementNS(namespaceURI, tagname)：使用给定的tagName创建一个属于命名空间namespaceURI的新特性。
- cueateAttributeNS(namespaceURI, attributeName)：使给定的attributeName创建一个属于命名空间namespaceURI的新特性。
- getElementsByTagNameNS(namespaceURI, tagName):返回属于命名空间namespaceURI的tagName元素的NodeList。

使用这些方法时需要传入表示命名空间的URI，如下所示：

```js
//创建一个新的SVG元素
var svg = docuemnt.createElementNS("http://www.w3.org/2000/svg", "svg");

//创建一个属于某个命名空间的新特性
var att = document.createAttributeNS("http://www.somewhere.com", "random")

//取得所有XHTML元素
var elems = document.getElementsByTagNameNS("http://www.w3.org/2000/xhtml", "*");
```

#### Element类型的变化

DOM2新增方法如下：

- getAttributeNS(namespaceURI, localName):  取得属于命名空间namespaceURI且名为localName的特性。
- getAttributeNodeNS(namespaceURI， localName)：取得属于命名空间namespaceURI且名为localeName的特性节点。
- getElementsByTagNameNS(namespaceURI, tagName):  返回属于命名空间namespaceURI的tagName元素的NodeList。
- hasAttributeNS(namespaceURI, localName):  确定当前元素是否有一个名为localeName的特性，而且改特性的命名空间是namespaceURI。
- removeAttributeNS(namespaceURI,localName):  删除属于命名空间namespaceURI且名为localName的特性。
- setAttributeNS(namespaceURI, qualifiedName,value):  设置属于命名空间namespaceURI且名为qualifiedName的特性的值为value。
- setAttributeNodeNS(attNode):  设置属于命名空间namespaceURI的特性节点。

#### NameNodeMap类型的变化

由于特性是由NameNodeMap表示的，因此这些方法多数情况下只针对特性使用。

- getNamedItemNS(namespaceURI, localName): 取得属于命名空间namespaceURI且名为localName的项。
- removeNamedItemNS(namespaceURI, localName):移除属于命名空间namespaceURI且名为localName的项。
- setNameItemNS(node): 添加node，这个节点已经事先指定了命名空间信息。

由于一般都是通过元素访问特性，所以这些方法很少使用。

#### 其他方面的变化

##### DocumentType类型的变化

新增了3个属性：publicId、systemId和internalSubset。

举例说明：

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
	"http://www.w3.org/TR/htmlr/script.dtd">

https://wenku.baidu.com/view/bb83093c856a561253d36f7f.html
```

- publicId = "-//W3C//DTD HTML 4.01//EN"
- systemId = "http://www.w3.org/TR/htmlr/script.dtd"
- internalSubset = "<!ELEMENT name (#PCDATA)>"这是内部子集。

这三个方法很少用到。

##### Document类型的变化

新增方法：

- importNode():  从一个文档中取得一个节点，然后将其导入另一个文档，使其成为这个文档结构的一部分。接受两个参数（要复制的节点，是否符指子节点的布尔值）

```js
var newNode = doucment.importNode(oldNode, true);  	//导入节点及其所有子节点
document.body.appeendChild(newNode);
//这个在xml中用的比较多
```

新增属性：

- defaultView：保存一个指针，指向拥有给定文档的窗口（或框架）。在IE中等价的属性是parentWindow。

因此要确定文档的归属窗口，可以使用以下代码：

```js
var parentWindow = document.defaultView || document.parentWindow;
```

为document.implementation对象规定了两个新方法：

- createDocumentType():  用于创建一个新的DocumentType节点，接受3个参数（文档类型名称，publicId，systemId）

  - 创建一个新的HTML 4.01 Strict文档类型：

    - ```js
      var doctype = document.implementation.createDocumentType("html", "-//W3C//DTD HTML 4.01//EN", "http://www.w3.org/TR/htmlr/script.dtd")
      ```

- createDocument():  用于创建新文档，接受3个参数（针对文档中元素的namespaceURI，文档元素的标签名，新文档的文档类型）。

  - 创建一个空白的xml文档：

    - ```js
      var doc = document.implementation.createDocument("", "root", null);
      ```

- 创建一个XHTML文档：

  - ```js
    var doctype = document.implementation.createDocumentType("html", "-//W3C//DTD HTML 4.01//EN", "http://www.w3.org/TR/htmlr/script.dtd");
    
    var doc = document.implementation.createDocument("http://www.w3.org/1999/xhtml", "html", doctype);
    ```

- createHTMLDocument():  用于创建一个完整的HTML文档，包括<html>  <head>  <title>  <body>等元素。接受一个参数（文档标题）。

  - ```js
    var htmlDoc = document.implementation.createHTMLDocument("New Doc");
     alert(html.title);  //"New Doc"
    alert(typeof htmlDoc.body); //"object"
    ```

  - 只有Opera和Safari支持这个方法。

##### Node类型的变化

新增方法：

- isSupported()方法：用于确定当前节点具有什么能力，接受两个参数（特性名，特性版本号）。

  - ```js
    if(document.body.isSypported("HTML", "2.0")){
        //执行只有“DOM2级HTML”才支持的操作
    }
    ```

DOM3引入新方法：

- isSameNode():  判断传入的节点与引用的节点是否相同，相同返回true，接受一个参数（节点）；所谓相同，指的是两个节点引用的是同一个对象。

- isEquralNode():  判断传如的节点与引用的节点是否相等，相等返回true，接受一个参数（节点）；所谓相等，指的是两个节点是否是同一类型，具有相等的属性等。

- ```js
  var div1 = document.createElement("div");
  div1.setAttribute("class", "box");
  
  var div2 = document.createElement("div");
  div2.setAttribute("class", "box");
  
  alret(div1.isSameNode(div1));		//true
  alert(div1.isSameNode(div2)); 		//false
  alert(div1.isEqurallNode(div2));    //true
  //这两个元素相等，但不相同。
  ```

- setUserData():  将数据指定给节点，它接受3个参数：要设置的键，实际的数据，处理函数。

  - ```js
    doucment.body.setUserData("name", "Nicholas",function () {});
    ```

  - 然后使用getUserData()并传入相同的键，就可以取得该数据：

  - ```js
    var value = document.body.getUserData("name");
    ```

##### 框架的变化

- 框架：HTMLFrameElement
- 内嵌框架：HTMLIframeElement

这两个在DOM2级中都有一个新的属性：

- contentDocument：它包含一个指针，指向表示框架内容的文档对象。

  - ```js
    var iframe = document.getElementById("myIframe");
    var iframeDoc = iframe.contentDocument;//IE8之前版本无效
    ```

  - IE8以前版本支持contentWindow属性

  - 所以处理兼容性代码如下：

    - ```js
      var iframe = document.getElementById("myIframe");
      var iframeDoc = iframe.contentDocument || iframe.contentWindow.dodcument;
      ```

      所有浏览器都支持contentWindow属性。

### 插话：3个集合对象：HTMLCollection、NodeList和NameNodeMap



用例子表示三个集合对象：

```js
var divs = document.getElementsByTagName("div");
alert(divs instanceof HTMLCollection);     		//true
//这证明，HTMLCollection集合对象保存的是HTML元素的集合

var div = document.getELementById("div1");
var children = div.childNodes;  //获取div子节点的集合
alert(children instanceof NodeList);  			//true
//这证明，NodeList集合对象保存的是Node节点的集合。

var attrs = div.attributes;		//获取div元素的特性集合
alert(attrs instanceof NameNodeMap);			//true
//这证明NameNodeMap集合对象保存的是元素特性的集合
```

这三个对象都是类数组，可以获取它们的length，也可以通过attrs[i]获取数据，有点类似于函数里面的arguments。



div.attributes将返回一个NameNodeMap对象，而集合中的每个元素，都是Attr类型的对象。Attr对象有三个属性：name  value  specified。

但在日常应用中，一般会应用getAttribute()   setAttribute()    removeAttribute()来操作特性，而不直接访问特性对象。

- 它们都是类数组。
- 它们都具有 “ 动态性 ” 。
- 特性和属性是不同的。

### 插话：特性和属性

特性（Attribute）。

属性（property）。

简单理解，Attribute就是DOM节点自带的属性，例如HTML中的id，class，title等。

而property是这个DOM元素作为对象，其附加的内容，例如childNodes，firstChild等。

另外，常用的Attribute如id，class，title等已经作为property附加到DOM对象上，可以和property一样取值和赋值，但是自定义的Attribute就不会这么幸运。

```html
<div id="div1" class="divclass" title="divtitle" align="left" title1="titile1">
    
</div>
```

这里的title1就不会变成property。

即，只要是DOM标签中出现的属性，都是Attribute。然后有些常用的特性（id，class，title等），会被转化为property。可以形象的说，这些特性/属性，是脚踏两只船的，因为它们既是特性又是属性。

最后注意，class变成property后就变成了className了，因为class是js中的关键字。

```js
var className = div.className;
var className1 = div.getAttribute("class"); 
```

Attribute 的取值与赋值：

- getAttribute()
- setAttribute()

property的取值与赋值：

```js
var id = div.id;

div.id = "div1";
```

对property可以赋值任何值，对Attribute只能赋值字符串。

另外，对于属性Property的赋值在IE中可能会引起循环引用，内存泄漏。为了防止这个问题，jQuery.data()做了特殊处理，解耦了数据和DOM对象。

sytle和onclick：

与id、class、title一样，它俩也是“脚踏两只船”，但是id、class、title都是简单的字符串，用.和getAttribute()获取结果一样，但是对于style和onclick就不一样了：

- 用.获取style会返回一个CSSStyleDeclaration对象，这对象中包含着所有样式信息。
- 而用getAttribute()返回的就是“width:100%; height:100%”这样简单的字符串。

用.获取的是style属性的Property，我们可给property赋值任何值；而用getAttribute()获取到的是特性Attribute，特性Attribute中只能储存字符串，**两者的数据结构不一致，导致返回结果不一致。**

- 特性和属性两者的存储方式不同；
- 了解“脚踏两只船”的特性/属性；
- DOM属性引用可能会导致循环引用。

## 样式

DOM2为3种应用样式机制提供了一套API，要确定浏览器是否支持DOM2级定义的CSS能力，可以使用以下代码：

```js
var supportsDOM2CSS = document.implementation.hasFeature("CSS", "2.0");
var supportsDOM2CSS2 = document.implementation.hasFeature("CSS2", "2.0");
```

### 访问元素的样式

任何支持style特性的HTML元素在JavaScript中都有一个对应的style属性。

这个style对象是CSSStyleDeclaration的实例，包含着通过HTML的style特性指定的所有样式信息，但不包括外部样式表或嵌入样式表层叠而来的样式。

在style中指定的任何css属性都将成为这个style对象的相应属性。

**对于使用-分隔的css属性，必须将其转化为驼峰大小写形式，才能通过JavaScript来访问**。

如：

| CSS属性          | JavaScript属性        |
| ---------------- | --------------------- |
| background-image | style.backgroundInage |
| color            | style.color           |
| display          | style.display         |
| font-family      | style.fontFamily      |

**其中有一个不能直接转化的css属性就是float，因为float是JavaScript的保留字，因此不能用作属性名。所以将其写为 CSSFloat，IE中写为styleFloat。**

#### DOM样式属性和方法

“DOM2级样式”规范还为style对象定义了一些属性和方法：

- cssText：访问style特性中的CSS代码。赋值给cssText的值会重写整个style特性的值。

  - ```js
    myDiv.style.cssText = "width:25px; height: 25px; background-color: red;";
    alert(myDiv.style.cssText);
    ```

- length：css属性数量。

  - 设计length属性的目的是与item()方法联用，以便迭代在元素中定义的css属性。

- parentRule：表示css信息的CSSRule对象。

- getPropertyValue(propertyName): 返回给定属性的字符串值。

- getPropertyCSSValue(propertyName):  返回包含给定属性的CSSValue对象。

  - 它返回两个属性的CSSValue对象，这两个属性分别是：CSSText和CSSValueType。其中CSSText与getPropertyValue()返回值相同；CSSValueType属性则是一个数值常量，表示值的类型：0表示继承的值，1表示基本的值，2表示值列表，3表示自定义的值。

- getPropertyPriority(propertyName):  如果使用!important，则返回“important”，否则返回空字符串。

- item(index):  返回给定位置的css属性名称。

  - 可以使用方括号语法来取代item()。

- removeProperty(propertyName):  删除属性。

- setProperty(propertyName,value,priority):   将给定属性设置为特定的值并加上优先权标志。

举例：

```js
var prop,value,i,len;
for(i=0, len=myDiv.style.length; i<len; i++){
    prop = myDiv.style[i];//或者item()
    value = myDiv.style.getPropertyValue(prop);
    alert(prop + ":" + value.cssText + "(" + value.cssValueType + ")");
}
```

删除属性（也就意味着为该属性应用默认的样式）：

```js
myDiv.style.removeProperty("border");
```

#### 计算的样式

DOM2级样式  增强了document.defaultView，提供了

- getComputedStyle()方法：接收两个参数（要取得计算样式的元素，伪元素字符串（如:after）/null）。它返回一个CSSStyleDeclaration对象（与style对象的类型相同）。

举例：

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Computed Styles Example</title>
        <style>
            #myDiv{
                background-color: blue;
                width: 100px;
                height: 100px;
            }
        </style>
    </head>
    <body>
        <div id="myDiv" style="background-color: red; border: 1px solid black"></div>
        
        <script>
        	var myDiv = document.getElementById("myDiv");
            var computedStyle = myDiv.defaultView.getComputedStyle(myDiv, null);
            
            alert(computedStyle.backgroundColor);   //"red"
            alert(computedStyle.width);  //"100px"
            alert(computedStyle.border);  //在某些浏览器中是"1px solid black"
            //这里存在差别的原因是不同浏览器解释综合（rollup）属性（如border）的方式不同。   但即使border不会在所有浏览器中都返回值，但computedStyle.borderLeftWidth会返回值。
            
            //IE中不支持getComputedStyle()方法，但在IE中每个具有style属性的元素还有一个currentStyle属性，这个属性也是CSSStyleDeclaration的实例，包含当前元素全部计算后的样式。：
            var computedStyle_IE = myDiv.currentStyle;
            
            alert(computedStyle_IE.backgroundColor);  //"red"
            alert(computedStyle_IE.width);  //"100px"
            alert(computedStyle.border);    //undefined
        </script>
    </body>
</html>
```

### 操作样式表（*）

CSSStyleSheet类型表示的是样式表，包括：

- \<link\>元素定义的样式表，本身由HTMLLinkElement表示。
- \<style\>元素定义的样式表，本身由HTMLStyleElement表示。

但是CSSStyleSheet类型相对更加通用，它只表示样式表，而不管这些样式表在HTML中是如何定义的。

确定浏览器是否支持DOM2级样式表：

```js
var supportsDOM2StyleSheet = document.implementation.hasFeature("StyleSheets","2.0");
```

CSSStyleSheet继承自StyleSheet，继承而来的属性如下：

- disabled：样式是否被禁用的布尔值。可读写，将这个值设置为true可以禁用样式表。
- href:  如果样式是<link>包含的，则是URL，否则为null。
- media：当前样式表支持的所有媒体类型的集合。
- ownerNode：指向拥有当前样式表的节点的指针。
- parentStyleSheet： 在当前样式表是通过@import导入的情况下，这个属性是一个指向导入它的样式表的指针。
- title：ownerNode中的title属性值。
- type：表示样式表类型的字符串。对于css，是"type/css"；

除了disabled属性之外，其他属性都是只读的。

在支持以上属性的基础上，CSSStyleSheet还支持以下属性和方法（IE均不支持，但有的有其他的属性代替）：

- CSSRules：样式表中包含的样式规则的集合。IE中是rules属性。
- ownerRule：如果样式表是通过@import导入的，这个属性就是一个指针，指向导入的规则，否则为null。
- deleteRule(index)：删除CSSRules集合中指定位置的规则。IE中是removeRule()方法。
- insertRule(rule, index)：向CSSRules集合中指定位置插rule字符串。IE中是addRule()方法。

应用于文档的所有样式表是通过document.styleSheets集合来表示的。通过这个集合的length属性可以获知文档中样式表的数量，而通过方括号语法或item()方法可以访问每个样式表。

```js
var sheet = null;
for(var i=0, len=document.styleSheets.length; i<len; i++){
    sheet = document.styleSheets[i];
    alert(sheet.href);
}
//输出文档中使用的每一个样式表的href属性。
```

不同浏览器的document.styleSheets返回的样式表也不同。

- sheet属性：包含CSSStyleSheet对象的属性。IE中为styleSheet属性。

  要想在不同浏览器中都能取得样式表对象，可以使用以下代码：

  ```js
  function getStyleSheet(element){
      return elemet.sheet || element.styleSheet;
  }
  
  //取得第一个link元素引入的样式表
  var link = document.getElementsByTagName("link")[0];
  var sheet = getStyleSheet(link);
  ```

#### 插话：css语法与css规则

基本语法：

​	选择器{

​		样式规则

​	}

样式规则：

​	属性名1：属性值1;

​	属性名2：属性值2;

​	属性名3：属性值3;

#### CSS规则

CSSRule对象表示样式表中的每一条规则。它包含下列属性：

- CSSText：返回整条规则对应的文本。
- parentRule：如果当前规则是导入的规则，这个属性引用的就是导入规则；否则为null。
- parentStyleSheet：当前规则所属的样式表。
- selectorText： 返回当前规则的选择符文本。
- style：一个CSSStyleDeclaration对象。
- type：表示规则类型的常量值。

CSSText与style.cssText属性的区别：前者包含选择符文本和花括号，后者只包含样式信息；CSSText是只读的，而style.cssText也可以被重写。

大多数情况下，仅使用style属性就可以满足所有操作样式规则的需求了。

```html
<style>
    div.box{
        background-color: blue;
        width：100px;
        height: 100px;
    }
</style>

<script>
	var sheet = document.styleSheets[0];
    var rules = sheet.cssRules || sheet.rules;  //取得规则表
    var rule = rules[0];	//取得第一条规则
    alert(rule.selectorText);    //"div.box"
    alert(rule.style.cssText);  //完整的css代码
    alert(rule.style.backgoundColor);  //"blue"
    alert(rule.sytle.width);		//"100px"
    
    rule.style.backgroundColor = "red";	//修改规则
</script>
```

#### 创建规则

- insertRule()方法：向现有样式表中添加新规则。接受两个参数（规则文本，插入位置索引）。

  - ```js
    sheet.insertRule("body{ background-color: silver }", 0);  //插入为第一条规则。
    
    //在IE中使用addRule()方法
    sheet.addRule("body", "background-color: silver", 0);  //仅IE
    ```

跨浏览器插入规则：

```js
function insertRule(sheet, selectorText,cssText,position){
    if(sheet.insertRule){
        sheet.insertRule(selectorText + "{" + cssText + "}",position);
    } else if(sheet.addRule){
        sheet.addRule(selectorText, cssText, position);
    }
}

//调用
insertRule(document.styleSheets[0], "body", "background-color: silver", 0);
```

随着要添加的规则的增多，这种方法会变的非常繁琐，所以还是建议使用第十章介绍过的动态加载样式表技术。

#### 删除规则

- deleteRule()方法，接受一个参数（要删除规则的位置）。IE使用removeRule()方法，使用方式相同。

跨浏览器删除规则：

```js
function deleteRule(sheet, index){
    if(sheet.deleteRule){
        sheet.deleteRule(index);
    } else if(sheet.removeRule){
        sheet.removeRule(index);
    }
}

deleteRule(document.sytleSheets[0], 0);
```

考虑到CSS的层叠效果，慎用此方法。

### 元素大小

IE率先引用一些属性来操作页面中元素大小。目前所有主流浏览器都已经支持这些属性。

#### 偏移量

包括元素在屏幕上占据的所有可见的空间。

通过4个属性取得元素的偏移量：

- offsetHeight：元素在垂直方向占用的空间大小，以像素计。包括元素的高度，（可见）水平滚动条的高度、上边框高度和下边框高度。
- offsetWidth：元素水平方向占用的空间大小，以像素计。包括元素的宽度，（可见）垂直滚动条的宽度，左右边框的宽度。
- offsetLeft：元素的左外边框至包含元素的左内边框之间的像素距离。
- offsetTop：元素的上外边框至包含元素的上内边框之间的像素距离。

其中，offsetLeft和offsetTop属性与包含元素有关，包含元素的引用保存在offsetParent属性中。offsetParent不一定与parentNode的值相等，比如td元素的offsetParent是table元素，而parentNode是tr。

图解在p321。要想知道某个元素在页面上的偏移量，将这个元素的offsetTop和offsetLeft与其offsetParent的相同属性相加，如此循环直至根元素，就可以得到一个基本准确的值。

```js
function getElementLeft(element){
    var actualLeft = element.offsetLeft;
    var current = element.offsetParent;
    
    while(current !== null){
        actualLeft += current.offsetLeft;
        current = current.offsetParent;
    }
    
    return actualLeft;
}

function getElementTop(element){
    var actualTop = element.offsetTop;
    var current = element.offsetParent;
    
    while(current !== null){
        actualTop += current.offsetTop;
        current = current.offsetParent;
    }
    
    return actualTop;
}
```

对于简单的css布局而言，这可以得到非常精确的结果。

对于使用表格和内嵌框架的页面，由于不同浏览器实现这些元素的方式不同，因此得到的值就不太精确了。

#### 客户区大小

指的是元素的内容及其内边距所占的空间大小。两个属性：

- clientWidth：元素内容区宽度 + 左右内边距宽度；
- clientHeight： 元素内容区高度 + 上下内边距高度。

图解P322

客户区大小就是元素内部的空间大小，因此**滚动条的占用空间不计算在内**。

要确定浏览器视口大小：

```js
function getViewport(){
    if(document.comatMode !== "BackCompat"){//判断浏览器是否运行在混杂模式下（<=IE7）
        return{//应该先处理非IE的，这样提高性能
            width:document.documentElement.clientWidth,
            height:document.documentElement.clientHeight;
        }
    } else {
        return{//函数返回一个对象
            width:document.body.clientWidth,
            height:document.body.clientHeight;
        }
    }
}
```

#### 滚动大小

指的是包含滚动内容的元素的大小。

有些元素需要用CSS属性overflow进行设置才能滚动。4个属性：

- scrollHeight：没有滚动条时候元素内容总高度。
- scrollWidth：没有滚动条时候元素内容总高度。
- scrollLeft：被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素滚动位置。
- scrollTop：被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素滚动位置。

scrollHeight和scrollWidth主要用于元素内容的实际大小。

scrollLeft和scrollTop即可以确定当前元素的滚动状态，也可以设置元素的滚动位置。

图解P324。

由于这些属性在不同浏览器之间会发生一些不一致问题，在确定文档的总高度时，必须去的scrollWidth/clientWidth和scrollHeight/clientHeight中的最大值，才能保证跨浏览器的情况下得到精确的结果。举例：

```js
var docHeight = Math.max(document.documentElement.scrollHeight ||document.body.scrollHeight, document.documentElement.clientHeight || document.body.clientHeight);

var docWidth = Math.max(document.documentElement.scrollWidth || document.body.scrollWidth, document.documentElement.clientWidth || document.body.clientWidth);
```

重置元素滚动位置：

```js
function scrollToTop(element){
    if(element.scrollTop != 0){
        element.scrollTop = 0;
    }
}
```

#### 确定元素大小

- getBoundingClientRect()方法：返回一个矩形对象，包含4个属性：left、top、right、bottom。这些属性给出了元素在页面中相对于视口的位置。

IE8及更早版本认为文档的左上角坐标为（2,2），而其他浏览器则为（0,0）。

来看下面函数：

```js
function getBoundingClientRect(element){
    if(typeof arguments.callee.offset != "number"){
        var scrollTop = document.documentElement.scrollTop ||document.body.scrillTop;
        var temp = document.createElement("div");
        temp.style.cssText = "position: absolute; left:0; top:0;";
        document.body.appendChild(temp);
        arguments.callee.offset = -temp.getBoundingClientRect().top - scrollTop;
        document.body.removeChild(temp);
        temp = null;
    }
    
    var rect = element.getBoundingClientRect();
    var offset = arguments.callee.offset;
    
    return{
        left: rect.left + offset,
        right: rect.right + offset,
        top: rect.top + offset,
        bottom: rect.bottom + offset
    }
}
```

这里不太明白，请教老师中。

## 遍历

“DOM2级遍历和范围”模块定义了两个而用于辅助完成顺序遍历DOM结构的类型：

- NodeIterator
- TreeWalker

这两个类型能够基于给定的起点对DOM结构执行深度优先的遍历操作。

检测浏览器支持情况：

```js
var suportsTraversals = document.implementation.hasFeature("Traversale", "2.0");
var suportsNodeIterator = type document.createNodeIterator == "function");
```

DOM遍历是深度优先的DOM结构遍历，也就是说移动的方向至少有两个。

以document为根节点的遍历则可以访问到文档中的全部节点。

### 插话：深度优先遍历和广度优先遍历



**1.深度优先遍历（DFS）**——一头走到黑（Depth first traversal）

(1)从某个顶点V出发，访问顶点并标记为已访问

(2)访问V的邻接点，如果没有访问过，访问该顶点并标记为已访问，然后再访问该顶点的邻接点，递归执行

​     如果该顶点已访问过，退回上一个顶点，再检查该顶点的邻接点是否都被访问过，如果有没有访问过的继续向下访问，如果全部都访问过继续退回到上一个顶点，继续同样的步骤。



深度优先遍历类似于树的先序遍历，深度优先遍历算法结果不唯一。

![遍历](E:\TyporaFiles\img\遍历.jpg)



选择V1为出发点，访问V1，然后访问V1的邻接点，邻接点有V2 V3和V4，假设都从左边的邻接点开始访问

访问V1的最左边的邻接点V2，访问V2的邻接点V5

访问V5的邻接点V3，V3的左边邻接点是V1，V1已经访问过，所以访问V4

V4的左边邻接点V6，访问V6，V6的所有邻接点都已访问过，退回V4，V4的所有邻接点也都访问过退回V3，V3的邻接点全部访问过退回V5，

V5的邻接点全部访问过退回V2，V2的邻接点全部访问过退回到出发点V1，V1的全部邻接点访问过，遍历结束。

**遍历序列为V1→V2→V5→V3→V4→V6**





**2.广度优先遍历（BFS）**——一层一层走（Breadth first traversal）

(1)从某个顶点V出发，访问该顶点的所有邻接点V1，V2..VN

(2)从邻接点V1，V2...VN出发，再访问他们各自的所有邻接点

(3)重复上述步骤，直到所有的顶点都被访问过

![遍历](E:\TyporaFiles\img\遍历.jpg)

(1)从顶点V1出发，v1入队，访问V1，V1的邻接点有V2     V3     V4，将它们入队,v1出队

队列：V2   V3  V4

(2)访问队头V2，V2的邻接点为V1(已访问过，忽略)和V5，将V5入队，V2出队

队列：V3  V4  V5

(3)访问V3，V3的邻接点为V1(访问过，忽略)   V4（已在队列，忽略）  V6   V5（已在队列，忽略），V6入队，V3出队

队列：V4 V5  V6

(4)访问V4的邻接点为V1(访问过，忽略)   V3（已访问过，忽略）  V6 （已在队列，忽略），V4出队

队列：V5  V6

(5)访问V5的邻接点为V2(访问过，忽略)   V3 (访问过，忽略)  V6(已在队列，忽略)，V5出队

队列：V6

(6)访问V6的邻接点为V4(访问过，忽略)   V3 (访问过，忽略)   V5（访问过，忽略），V6出队

队列：空

### NodeIterator

NodeIterator类型是两者中比较简单的一个，可以使用document.createNodeIterator()方法创建它的新实例。它接受4个参数：

- root：想要作为搜索起点的树中的节点。
- whatToShow：表示要访问哪些节点的数字代码。
- filter：是一个NodeFileter对象，或者一个表示应该接受还是拒绝某种特定节点的函数。
- entityReferenceExpansion：是一个布尔值，标识是否要扩展实体引用。

whatToShow是一个位掩码，通过应用一或多个过滤器来确定要访问哪些节点。节点值（部分）如下所示：

- NodeFilter.SHOW_ALL:  显示所有类型的节点。
- NodeFilter.SHOW_ELEMEMT:  显示元素节点。
- 。。。

filter参数用来指定NodeFilter对象，或者指定一个过滤器函数。每个NodeFilter对象只有一个方法：

- acceptNode():  如果应该访问给定的节点，该方法返回NodeFilter.FILTER_ACCEPT，反之，返回NodeFilter.FILTER_SKIP。

由于NodeFilter是一个抽象的类型，因此不能直接创建实例。在必要时，只要创建一个包含acceptNode()方法的对象，然后将这个对象传入createNodeIterator()中即可：

```js
//创建一个只显示<p>元素的节点迭代器
var filter = {
    acceptNode: function (node){
        return node.tagName.toLowerCase() == "p" ? NodeFilter.FILTER_ACCCEPT : NodeFilter.FILTER_SKIP;
    };
};

var iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT,filter,false);

//第三个参数也可以是一个与acceptNode方法类似的函数：
var filter = function(node){
    return node.tagName.toLowerCase() == "p" ? NodeFilter.FILTER_ACCEPT : NodeFilter.FILTER_SKIP;
};

var iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT,filter,false);

//如果不指定过滤器，那么第三个参数位传入null
var iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT,null,false);
```

NodeIterator类型的两个主要方法是

- nextNode()：向前一步。
- previousNode()：向后一步。

在刚刚创建的NodeIterator对象中，有一个内部指针指向根节点，因此第一次调用nextNode()会返回根节点。当遍历到DOM子树的最后一个节点时，nextNode()返回null。

previousNode()方法的工作机制类似。

以下面的HTML片段为例：

```html
<div id="div1">
    <p>
        <b>Hello</b>
        World!
    </p>
    <ul>
        <li>List item 1</li>
        <li>List item 2</li>
        <li>List item 3</li>
    </ul>
</div>

<script>
	//假设我要遍历div下的所有元素：
    var div = document.getElementById("div1");
    var iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT,null,false);
    var node = iterator.nextNode();
    
    while(node !== "null"){
        alert(node.tagName);  //输出标签名
        node = itrator.nextNode();//这里非node=node.nextNode(),因为是iterator的迭代，iterator的每一次执行nextNode()都向前进一个节点，iterator不是节点的集合。
    };
</script>

执行结果：
DIV
P
B
UL
LI
LI
LI

如果指向返回li元素，只需使用一个过滤器即可：
<script>
	var div = document.getElementById("div1");
    
    var filter = function (node){
        return node.tagName.toLowerCase() == "li" ? NodeFilter.FLITER_ACCEPT : NodeFilter.FILTER_SKIP;
    };
    
    var iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT, filter,false);
    var node = iterator.nextNode();
    
    while(node !== null){
        alert(node.tagName);  //输出标签名
        node = iterator.nextNode();
    }
</script>
```

由于nextNode()和previousNode()方法都基于NodeIterator在DOM中的内部指针工作，所以DOM结构的变化会反映在遍历的结果中。

### TreeWalker

TreeWalker是NodeIterator的更高级版本。除了nextNode()   previousNode()方法之外，还提供了下列用于在不同方向上遍历DOM结构的方法。

- paprentNode():  遍历到当前节点的父节点；
- firstChild():  遍历到当前节点的第一个子节点；
- lastChild()：遍历到当前节点的最后一个子节点；
- nextSibling()：遍历到当前节点的下一个同辈节点；
- previousSibling(): 遍历到当前节点的上一个同辈节点。

创建TreeWalker对象：

- document.createTreeWalker()方法，接受4个参数与createNodeIterator()方法相同：根节点，节点类型，过滤器，是否扩展实体引用。

由于这两个创建方法很相似，所以很容易用TreeWalker来代替NodeIterator：

```js
var div = document.getElementById("div");
var filter = function (node){
    return node.tagName.toLowerCase() == "li" ? NodeFilter.FILTER_ACCEPT : NodeFilter.FLITER_SKIP;
};

var walker = document.createTreeWalker(div,NodeFilter.SHOW_ELEMENT,filter,false);

var node = walker.nextNode();
while(node !== null){
    alert(node.tagName);
    node = walker.nextNode();
}
```

filter可以返回的值有所不同：

- NodeFilter.FILTER_SKIP:  跳过相应的节点继续前进到子树中的下一个节点；
- NodeFilter.FILTER_REJECT:  跳过相应节点及该节点整个子树；
- NodeFilter.FILTER_ACCEPT:  无变化。

当然，TreeWalker真正强大的地方在于能够在DOM结构中沿着任何方向移动。使用TreeWalker遍历DOM树，即使不定义过滤器，也可以取得所有li元素：

```js
var div = document.getElementById("div1");
var walker = document.createTreeWalker(div, NodeFilter.SHOW_ELEMENT,null,false);

walker.firstChild();//p
walker.nextSibling();///ul

var node = walker.firstChild();//li
while(node !== null){
    alert(node.tagName);
    node = walker.nextSubling();
};
```

TreeWalker还有一个属性：

- currentNode：表示任何遍历方法在上一次遍历中返回的节点。通过这个属性也可以修改遍历继续进行的起点：

```js
var node = walker.nextNode();
if(node === node.currentNode){//判断是否是最后一个节点
    walker.currentNode = document.body;  //修改起点
};
```

由于IE中没有对应类型的方法，所以使用遍历的跨浏览器解决方案非常少见。

## 范围

在常规的DOM操作不能更有效的修改文档时，使用范围往往可以达到目的。

Firefox、Opera、Safari和Chrome都支持DOM范围。IE以专有方式实现了自己的范围特性。

### DOM中的范围

- createRange()方法创建范围。

确定浏览器是否支持范围。

```js
var supportsRange = document.implementation.hasFeature("Range", "2.0");
var alsoSupportsRange = (typeof document.createRange == "function");
```

创建DOM范围：

```js
var range = document.createRange();
```

与节点类似，新创建的范围也直接与创建它的文档关联在一起，不能用于其他文档。

每个范围由一个Range类型的实例表示，这个实例拥有很多属性和方法。

下列属性提供了当前范围在文档中的位置信息：

- startContainer：包含范围起点的节点（即选区中第一个节点的父节点）；
- startOffset：范围在startContainer中起点的偏移量。如果startContainer是文本节点、注释节点或CDATA节点，那么startOffset就是范围起点之前跳过的字符数量。否则，startOffset就是范围中第一个子节点的索引 。
- endContainer：包含范围终点的节点（即选区中最后一个节点的父节点）；
- endOffset：范围在endContainer中终点的偏移量。
- commonAncestorContainer：startContainer和endContainer共同的祖先节点，在文档树中位置最深的那个。

在把范围放到文档中特定的位置时，这些属性都会被赋值。

#### 用DOM范围实现简单选择

要使用范围来选择文档中的一部分，最简单的方式就是使用

- selectNode()：接受一个DOM节点参数。选择整个节点，包括子节点。
- selectNodeContents()：接受一个DOM节点参数。只选择其子节点。

举例：

```html
<!DOCTYPE>
<html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <p id="p1">
            <b>Hello</b>
            World!
        </p>
    </body>
    
    <script>
    	//创建范围并赋值
        var range1 = document.createRange(),
            range2 = document.createRange(),
            p1 = document.getElementById("p1");
        
        range1.selectNode(p1);//包括p1及其子节点
        range2.selectNodeContents(p1);//仅包括p1的子节点
    </script>
</html>
```

在这个例子中：

​	调用selectorNode()时，startContainer、endContainer和commonAncestorContainer都等于传入节点p的父节点——document.body。而startOffset属性等于给定节点在其父节点的childNodes集合中的索引（在这个例子中是1，因为兼容DOM的浏览器将空格算在做一个文本节点），endOffset等于startOffset加1（因为只选择了一个节点）

调用selectorNodeContents()时，startContainer、endContainer和commonAncestorContainer都等于传入的节点p。而startOffset属性始终等于0，因为范围从给定节点的第一个子节点开始。而endOffset等于子节点的数量（node.childNodes.length)，在这个例子中是3。

为了更精细的控制将哪些节点包含在范围中，还可以使用以下方法：

- setStartBefore(refNode)：将范围的起点设置在refNode之前，此时refNode为第一个子节点。
- setStartAfter(refNode):  将范围的起点设置在refNode之后，此时refNode的下一个同辈节点为第一个子节点。
- setEndBefore(refNode)：将范围的终点设置在refNode之前，此时refNode的上一个同辈节点为最后一个子节点。
- setEndAfter(refNode)：将范围的终点设置在refNode之后，此时refNode为最后一个子节点。

在调用这些方法时，所有属性都会自动为你设置好。不过，要想创建复杂的范围选区，也可以直接指定这些属性的值。

#### 用DOM范围实现复杂选择

要创建复杂的范围就得使用

- setStart():  接受两个参数（参照节点，偏移量）。参照节点会变成startContainer，而偏移量值会变成startOffset。
- setEnd()： 接受两个参数（参照节点，偏移量）。参照节点会变按成endContainer，而偏移量值会变成endOffset。

使用这两个方法模仿selectNode()和selectNodeContends()：

```js
var range1 = document.createRange(),
    range2 = document.createRange(),
    p1 = document.getElementById("p1"),
    p1Index = -1,
    len,i;
for(i=0,len=p1.parentNode.childNode.length; i<len; i++){//确定p在其父节点的子节点中的索引
    if(p1.parentNode.childNode[1] == p1){
        p1Index = i;
        break;
    };
};
//range1的目的就是在其父节点中将p1放进范围里。
range1.setStart(p1.parentNode, p1Index);
range1.setEnd(p1.parentNode,p1Index+1);//这里之所以是p1Index+1，是因为终止节点就是p的后一个同辈节点
//range2的目的是将p1的所有子节点放进范围里。
range2.setStart(p1,0);
range2.setEnd(p1,p1.childNode.length);
```

但这并不是它们的主要用途，它们更胜一筹的地方在于能够选择节点的一部分。

假设你只想选择前面HTML中的"Hello"中的"llo"到"world!"中的"o"：

```js
var p1 = document.getElementById("p1"),
    helloNode = p1.firstChild.firstChild,
    worldNode = p1.lastChild;

var range  = doucment.createRange();
range.setStart(helloNode, 2);//0 1 2
range.setEnd(worldNode, 3);//0 1 2 3(0=" ", 1=w,2=o,3=r)
```

#### 操作DOM范围中的内容

在创建范围时，内部会为这个范围创建一个文档片段，范围所属的全部节点都被添加到了这个文档片段中。为了创建这个文档片段，范围内容格式必须正确有效。因此，前面的例子会被范围重新构建DOM结构，范围修改后的DOM就变成了：

```html
<p><b>He</b><b>llo</b> world!</p>这里的world！也被拆分成了"wo"和"rld!"两个文本节点。
```

像这样创建了范围后，就可以进行操作了（注意，表示范围的内部文档片段中的所有节点，都只是指向文档中相应节点的指针）。

第一个方法：

- deleteContents()：从文档中删除范围所包含的内容。如：

  - ```js
    var range = document.createRange(),
        p1 = document.getElementById("p1"),
        helloNode = p1.firstChild.firstChild,
        worldNode = p1.lastChild;
    
    range.setStart(helloNode, 2);
    range.setEnd(worldNode,3);
    
    range.deleteContents();
    ```

  - 页面中显示的如下HTML代码：

  - ```html
    <p><b>He</b>rld!</p>
    ```

由于范围选区在修改底层DOM结构时能保证格式良好，因此即使内容被删除了，最终的DOM结构依旧是格式良好的。

第二个方法：

- extractContents()：也是从文档中移除范围选区，但会返回范围的文档片段，可以利用它将此范围插入到文档中的其他地方。如：

  - ```js
    var range = document.createRange(),
        p1 = document.getElementById("p1"),
        helloNode = p1.firstChild.firstChild,
        worldNode = p1.lastChild;
    
    range.setStart(helloNode, 2);
    range.setEnd(worldNode, 3);
    
    var fragment = range.extractContents();
    document.body.appendChild(fragment);
    ```

    记住，在将文档片段传入appendChild()方法中时，添加到文档中的只是片段的子节点，而非片段本身。

  - HTML结果如下：

  - ```html
    <p><b>He</b>rld!</p>
    <b>llo</b> wo
    ```

第三个方法：

- cloneContents()：创建范围对象的一个副本返回。

  - ```js
    var p1 = document.getElementById("p1"),
        range = document.createRange(),
        helloNode = p1.firstChild.fistChild,
        worldNode = p11.lastChild;
    
    range.setStart(helloNode, 2);
    range.setEnd(worldNode, 3);
    
    var fragment = range.cloneContents();
    p1.parentNode.appendChild(fragment);
    ```

  - 它与extractContents()的区别是它返回的是范围中节点的副本，而不是实际的节点。

  - 执行上面的操作后的HTML：

  - ```html
    <p><b>He</b>rld!</p>
    <b>llo</b> wo
    ```

有一点要注意，在调用上面的方法之前，拆分的节点并不会产生格式良好的文档片段。换句话说，原始的HTML在DOM被修改之前会始终不变。

#### 插入DOM范围中的内容

- insertNode():  向范围选区开始处插入一个节点。

  - 举例：在上例HTML前面插入HTML代码：

  - ```html
    <!--插入如下span-->
    <span style="color:red">Inserted text</span>
    
    <script>
    	var p1 = document.getElementById("p1"),
            helloNode = p1.firstChild.firstChild,
            worldNode = p1.lastChild,
            range = document.createRange();
        
        range.setStart(helloNode, 2);
        range.setEnd(worldNode, 3);
        
        var span = document.createElement("span");
        span.style.color = "red";
        //span.innerHTML = "Inserted text";
        span.appendChild(document.createTextNode("Inserted text"));
        range.insertNode(span);
    </script>
    ```

  - 运行上面代码得到如下HTML：

  - ```html
    <p id="p1"><b>He<span style="color:red">Inserted text</span>llo</b> world!</p>
    <!--由于这里没有适用上一节介绍的方法，所以并没有形成格式良好的DOM结构，也就是说并没有添加<b>元素。
    使用这种技术可以插入一些帮助提示的信息，例如再打开新窗口的链接旁边插入一幅图像-->
    ```

- surroundContents()方法：环绕范围插入内容，接收一个参数（环绕范围内容的节点）。在环绕范围插入内容时，后台会执行下列步骤：

  - 提取出范围中的内容（类似执行extractContents()）；
  - 将给定节点插入到文档中原来范围所在的位置上；
  - 将文档片段的内容添加到给定节点中。

通俗易懂的来说：你在床上站起来，给你铺一层毛毯，然后你在躺在毛毯上。毛毯就把你裹住了。

可以用这种技术突出显示网页中的某些词句：

```js
var p1 = document.getElementById("p1"),
 			range = document.createRange(),
 			helloNode = p1.firstChild.firstChild,
 			worldNode = p1.lastChild;

 		range.selectNode(helloNode);

 		var span = document.createElement("span");
 		span.style.backgroundColor = "yellow";

 		range.surroundContents(span);
//会给范围选区加上一个黄色背景
```

HTML结果如下：

```html
<p><b><span style="background-color: yellow">Hello</span></b> world!</p>
```

#### 折叠DOM范围

所谓折叠范围，就是指范围中未选择文档的任何部分。

- collapse()方法：用来折叠范围，接受一个布尔值参数：

  - true：折叠到范围的起点。
  - false：折叠到范围的终点。

- collapsed属性：确定范围已折叠完毕。

- ```js
  range.collapse(true);  //折叠到起点
  alert(range.collapsed);  //true
  ```

检测某个范围是否处于折叠状态，可以帮我们确定范围中的两个节点是否紧密相邻。

例如：

```html
<p id="p1">Paragraph 1</p><p id="p2">Paragraph 2</p>

<script>
	var p1 = document.getElementById("p1"),
        p2 = document.getElementById("p2"),
        range = docuent.createRange();
    
    range.setStartAfter(p1);
    range.setEndBefore(p2);
    
    alert(range.collapsed);   //true
    //在这个例子中，新创建的范围时折叠的，因为p1的后面到p2的前面什么也没有。
</script>
```

#### 比较DOM范围

- compareBoundaryPoints()方法：在多个范围的情况下确定这些范围是否有公共的起点或终点。接受两个参数：（表示比较方式的常量值，要比较的范围）。

- 常量值：

  - Range.START_TO_START(0)：比较第一个范围的起点和第二个范围的起点。
  - Range.START_TO_END(1)：比较第一个范围的起点和第二个范围的终点。
  - Range.END_TO_END(2)：比较第一个范围的终点和第二个范围的终点。
  - Range.END_TO_START(3)：比较第一个范围的终点和第二个范围的起点。

- compareBoundaryPoints()方法可能的返回值如下：

  - -1：第一个范围中的点在第二个范围中的点之前；
  - 0：两个点相等。
  - 1：第一个范围中的点在第二个范围中的点之后。

- 看下面例子：

- ```js
  var range1 = document.createRange(),
      range2 = document.createRange(),
      p1 = document.getElementById("p1");
  
  range1.selectNodeContents(p1);//选择其子节点
  range2.selectNodeContents(p1);
  range2.setEndBefore(p1.lastChild);
  
  alert(range1.compareBoundaryPoints(Range.START_TO_START, range2));  // 0
  alert(range1.compareBoundaryPoints(Range.END_TO_END,range2));  //1
  ```

#### 复制DOM范围

- cloneRange()方法：复制范围，这个方法创建一个调用它的范围的一个副本。

  - ```js
    var newRange = range.cloneRange();
    ```

- 新创建的范围与原来的范围包含相同的属性，而修改它的端点不会影响原来的范围。

#### 清理DOM范围

在使用完范围之后，最好是调用detath()方法，以便从创建范围的文档中分离出该范围。调用detath()之后，就可以放心的解除对其的引用，从而让垃圾回收机制回收其内存了。

```js
range.detath();//从文档中分离
range = null;  //解除引用
//在使用范围的最后再执行这两个步骤使我们推荐的方式。一旦分离的范围，就不能再恢复使用了。
```

### IE8及更早版本中的范围

虽然IE9+支持DOM范围，但低版本不支持DOM范围。不过IE8及早期版本只吃一种类似的概念————文本范围（IE特有）

- createTextRange()：创建文本范围

  - ```js
    var range = document.body.createTextRange();
    //这样在document上创建的范围可以在页面中的任何地方使用
    ```

#### 用IE范围实现简单的选择

- findText()方法：找到第一次出现的给定文本，并将范围一过来以环绕该文本，并且返回true，如果没找到则返回false。

  - ```html
    <p id="p1"><b>Hello</b> world!</p>
    
    <script>
    	var range = document.body.createTextRange();
        var found = range.findText("Hello");
        
        alert(found);  //true
        alert(range.text);  //"Hello"
    </script>
    ```

- findText()方法还可以传入第二个参数：一个表示继续向哪个方向继续搜索的数值。负值表示从当前位置向后搜索，正值向前搜索。    因此，要查找文档中前两个“Hello”可以这样写：

  - ```js
    var found = range.findText("Hello");
    var foundAgain = range.findText("Hello",1);
    ```

- moveToElementText()方法：与selectNode()方法最接近，接受一个DOM元素参数，并选择该元素的所有文本，包括HTML标签：

  - ```js
    var range = document.body.createTextRange();
    var p1 = document.getElementById("p1");
    
    range.moveToElementText(p1);
    ```

- htmlText属性：取得范围的全部内容：

  - ```js
    alert(range.htmlText);
    ```

- parentElement（）方法：反映文本选区的父节点。

#### 使用IE范围实现复杂的选择

方法就是一特定的增量向四周移动范围。

IE提供了4个方法：

emmm，就是换汤不换药，原理都一样，就是IE的方法和属性名称不同，如果以后用到了，去书上查一下就可以了，这里不做过多介绍了。

此内容在书上P341。

# 第十三章 事件

JavaScript与HTML之间的交互是由事件实现的。

可以使用侦听器（或处理程序）来预定事件， 以便事件发生时执行相应代码。

这种在软件工程中被称为观察员模式的模型，支持页面的行为与外观之间的松散耦合。

使用事件有时相对简单，有时则非常复杂，难易程度会因你的需求不同。不过，有关事件的一些核心概念是一定要理解的。

## 事件流

事件流描述的是从页面中接收事件的顺序。

### 事件冒泡

IE的事件流叫做事件冒泡，即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。

![事件冒泡](E:\TyporaFiles\img\事件冒泡.png)

所有现代浏览器都支持事件冒泡，但具体实现上有一定的差别。

### 事件捕获

事件捕获的思想是不太具体的节点应该更早的接收到事件，而最具体的节点应该最后接收到事件。

事件捕获的用意在于时间到达预定目标之前捕获它。

![事件捕获](E:\TyporaFiles\img\事件捕获.png)

由于老版本浏览器不支持，因此很少有人用事件捕获。我们也建议读者放心的使用事件冒泡，在有特殊需要的时候使用事件捕获。

### DOM事件流

“ DOM2级事件 ” 规定的事件流包括三个阶段：

- 事件捕获阶段
- 处于目标阶段
- 事件冒泡阶段

首先发生的是事件捕获，为截获事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。

![DOM事件流](E:\TyporaFiles\img\DOM事件流.png)

在DOM事件流中，实际的目标（Div元素）在捕获阶段不会接收到事件。这意味着在捕获阶段，事件从document到html再到body后就停止了。下一个阶段是 ” 处于目标阶段 “ ，于是事件在div上发生，并在事件处理中被看成冒泡阶段的一部分。然后冒泡阶段发生，事件又传播回文档。

即使“DOM2级事件”规范明确规定补货阶段不会涉及事件目标，但IE9、Safari、Chrome、Firefox和Opera9.5+都会在捕获阶段出发事件对象上的事件。    结果，就是有两个机会在目标对象上面操作事件。

IE8及更早版本不支持事件流

## 事件处理程序

响应某个事件的函数就是事件处理程序（或事件侦听器）。

事件处理程序与on开头如：onclick、onload等。

为事件指定处理程序的方式有好几种：

### HTML事件处理程序

某个元素支持的每种事件，都看看可以使用一个与相应事件处理程序同名的HTML特性来指定。如：

```html
<input type="button" value="Click Me" onclick="alert('Clicked')" />
```

如果要想在里面写入字符要转义。

也可以调用页面其他地方定义的脚本：

```html
<input type="button" value="Click Me" onclick="showMessage()" />

<script>
    function showMessage() {
        alert("Hello world!");
    };
</script>
```

这个函数中有一个局部变量event，也就是事件对象：

```html
<input type="button" value="Click Me" onclick="alert(event.type)">
输出click
```

在这个函数内部，this值等于事件的目标对象：

```html
<input type="button" value="Click Me" onclick="this.value">
输出   Click Me
```

可以用with语句扩展 其作用域：

```js
function () {
    with(document){
        with(this){
            //元素属性值
        };
    };
};
```

```html
<input type="button" value="Click Me" onclick="alert(value)">
```

不过，在HTML中指定事件处理程序有两个缺点：

- 存在时差问题，因为用户可能会在HTML元素一出现就点击，但当时的事件处理程序有可能尚不具备执行条件。

  - 为此，很多HTML事件处理程序都会被封装到一个try-catch块中，以便错误不会浮出水面。

  - ```html
    <input type="button" value="Click Me" onclick="try{showMesage();}catch(ex){};">
    ```

- 这样扩展事件处理程序的作用域链在不同的浏览器中导致不同的结果。

- HTML与JavaScript代码紧密耦合。

  - 这正是许多开发人员摒弃HTML事件处理程序，转而使用JavaScript指定事件处理程序的原因所在。

### DOM0级事件处理程序

通过JavaScript指定 事件处理程序 的传统方式，就是将一个函数赋值给一个事件处理程序属性。

每个元素都有自己的事件处理程序属性，这些属性通常全部小写，如onclick。

```js
var btn = document.getElementById("myBtn");
btn.onclick = function () {
    alert(this.id);
};

//这种方法只能为元素添加一个事件，如不能添加两个click

//此时的事件处理程序是在元素的作用域中运行的。
//所以可以通过this访问元素的任何属性和方法。

//以这种方式添加的事件处理程序会在事件流的冒泡阶段被处理。

//删除通过DOM0级方法指定的事件处理程序：
btn.onclick = null;
```

### DOM2级事件处理程序

- addEventListener()方法：用于指定事件处理程序。接受3个参数（事件名，事件处理函数，布尔值——true表示在捕获阶段 调用事件处理程序；false表示在冒泡阶段调用事件处理程序。）

  - ```js
    var btn = document.getElementById("myBtn");
    btn.addEventListener("click", function () {//先执行
        alert(this.id);
    }, false);
    
    //与DOM0级方法一样，这里添加的事件处理程序也是在其依附的元素的作用域中运行
    
    //而且可以添加多个事件处理程序：
    btn.addEventListener("click", function (){//后执行
        alert("Hello world!");
    }, false);
    //这两个事件处理程序会按照添加它们的顺序触发。
    ```

- removeEventListener()方法：用于删除事件处理程序。接受3个参数（事件名，事件处理函数，布尔值——true表示在捕获阶段 调用事件处理程序；false表示在冒泡阶段调用事件处理程序。）

  - 通过addEventListener()添加的事件处理程序只能使用removeEventListener()来移除；这也意味着通过addEventListener()方法添加的匿名函数将无法移除：

    - ```js
      var btn = document.getElementById("myBtn");
      btn.addEventListener("click", function () {//注意这里是click而非onclick，IE的是onclick
          alert(this.id);
      }, false);
      
      btn.removeEventListener("click", function () {//没有用！
          alert(this.id);
      }, false);
      
      //实际上，第二个参数与传入addEventListener中的那个完全不是一个函数，所以没有用
      ```

  - 所以应该这样用：

    - ```js
      var btn = document.getElementById("myBtn");
      var handler = function () {
          alert(this.id);
      };
      
      btn.addEventListener("click", handler, false);
      
      btn.removeEventListener("click", handler, false);//有效！
      ```

大多数时候，都将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度的兼容各种浏览器。

如果不是特别需要，不建议在事件捕获阶段注册事件处理程序。

### IE事件处理程序

IE实现了与DOM中类似的两个 方法：

- attachEvent()：用于指定事件处理程序。接受2个参数（事件处理程序名称，处理函数）。

  - ```js
    var btn = document.getElementById("myBtn");
    btn.attachEvent("onclick", function () {//注意这里是onclick而非click
        alert("onclick");					//后执行
        alert(this === window);   //true
    });
    
    //使用attachEvent的情况下，事件处理程序会在全局作用域中运行，而非其所属元素的作用域。
    //在编写跨浏览器的代码时，牢记这一区别非常重要。
    
    //而且，attachEvent也可以为一个元素添加多个事件，但是执行顺序是反过来的，先定义的后执行。
    btn.attachEvent("onclick", function () { //先执行
        alert("First");
    });
    ```

  - 

- detachEvent()：用于删除事件处理程序。接受2个参数（事件处理程序名称，处理函数），和DOM方法一样，这也意味着添加的匿名函数将不能被移除。所以建议这样写：

  - ```js
    var btn = document.getElementById("btn");
    var handler = function () {
        alert("clicked");
    };
    
    btn.attachEvent("onclick", handler);
    
    btn.detachEvent("onclick", handler);
    ```

由于IE8-只支持事件冒泡，所以这样添加的事件处理程序都会被添加到冒泡阶段。

支持IE事件处理程序的浏览器有IE和Opera。

### 跨浏览器的事件处理程序

自己编写一个对象，其中有各种关于解决兼容性问题的方法：

- 第一个要创建的方法是addHander()：它的职责是视情况分别使用DOM0级方法、DOM2级方法或IE方法来添加事件。接受3个参数（要操作的元素，事件处理名称，事件处理函数）
- 第二个数removeHander():移除事件处理程序，与上面接受同样的参数。

```js
var EventUtil = { 		//用来处理浏览器之间差异的对象
    addHander: function (element, type, handler) {
        if(element.addEventListener){
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent){//IE
            element.attachEvent("on" + type, handler);
        } else{//如果其他方法无效，默认采用DOM0的方法
            element["on" + type] = handler;
        }
    },
    removeHander: function (element, type,handler) {
        if(element.removeEventListener){
            element.removeEventListener(type, handler, false);
        } else if(element.detachEvent){
            element.detachEvent("on" + type, handler);
        } else {
            element.["on" + type] = null;
        }
    }
};
```

可以这样使用EventUtil对象：

```js
var btn = document.getElementById("btn");
var handler = function () {
    alert("tyle");
};

EventUtil.addHander(btn, "click", handler);
//...
EventUtil.removeHander(btn, "click", handler);
```

但上述两个方法并没有考虑到所有的浏览器问题，例如在IE中的作用域问题。DOM0级和DOM2级中的两个方法的作用域都是作用元素的作用域，而IE中的方法的作用域是全局作用域。

## 事件对象

在触发DOM上的某个事件时，会产生一个事件对象event——包含着所有与时间有关的信息。

所有浏览器都支持event，但支持的方式不同。

### DOM中的事件对象

兼容DOM的浏览器会将一个event对象传入到事件处理程序中：

```js
var btn = document.getElementById("btn");
btn.onclick= function (event) {
    alert(event.type);  //"click"
};
btn.addEventListener("click", function (event) {
    alert(event.type); //"click"
}, false);
```

event对象包含与创建它的特定事件有关的属性和方法，所有事件都会有下列属性和方法（下列方法均只读）：

| 属性/方法                  | 类型           | 说明                                                       |
| -------------------------- | -------------- | ---------------------------------------------------------- |
| bubbles                    | Boolean        | 表明事件是否冒泡                                           |
| cancelable                 | Boolean        | 表明是否可以取消事件的默认行为                             |
| currentTarget              | Element        | 其事件处理程序当前正在处理事件的那个元素                   |
| defaultPrevented           | Boolean        | 为true表示已经调用了preventDefault()                       |
| detail                     | Integer（int） | 与事件相关的细节信息                                       |
| eventPhase                 | Integer        | 1=捕获阶段，2=“处于目标”，3表示冒泡阶段                    |
| preventDefault()           | Function       | （如果cancelable=true）取消时间的默认行为。                |
| stopImmediatePropagation() | Function       | 取消事件的进一步捕获或冒泡，同时阻止任何事件处理程序被调用 |
| stopPropagation()          | Function       | （如果bubbles = true）取消事件的进一步捕获或冒泡           |
| target                     | Element        | 事件的目标                                                 |
| trusted                    | Boolean        | true=>此事件是浏览器生成的；false=>此事件是开发人员创建的  |
| type                       | String         | 被触发的事件类型                                           |
| view                       | AbstractiView  | 发生事件的window对象                                       |

在事件处理程序内部，对象this始终等于currentTaget的值，而target则只包含事件的实际目标。

如果直接将事件处理程序指定给了目标元素，则this、currentTarget和target包含相同的值。

```js
var btn = document.getElementById("btn");

btn.onclick = function (event) {
    alert(event.currentTarget === this);		//true
    alert(event.target === this);					   //true
};//此时三者是相同的
```

但如果事件处理程序存在于按钮的父节点中（如document.body），那么这些值是不同的：

```js
document.body.onclick = function (event) {
    alert(event.currentTarget);			//document.body
    alert(event.target === btn);		//true
    alert(this);						//document.body
};
//当单击这个目标时，this和currentTarget的值都是document.body，因为事件处理程序是注册到这个元素上的。然而target的值是btn，因为它是click事件真正的目标。
//由于按钮上并没有注册事件处理程序，所以click事件就冒泡到document.body，在那里事件才得到了处理。
```

在需要一个函数处理多个事件时，可以使用type属性：

```js
var btn = document.getElementById("btn");

var handler = function (event) {
    switch(event.type) {
        case "click":
            alert("Clicked!");
            break;
        case "mouseover":
            alert("mouseover");
            break;
        case "mouseout":
            alert("mouseout");
            break;
    }
};

btn.onclick = handler;
btn.onmouseover = handler;
btn.onmouseout = handler;
```

- preventDefault()方法：阻止事件的默认行为。例如阻止链接的跳转：

  - ```js
    var link = document.getElementById("link");
    
    link.onclick = function (event) {
        event.preventDefault();
    };
    //只有cancelable = true时才能使用preventDefault()方法。
    ```

- stopPropagation()方法：立即停止事件在DOM层次中的传播（捕获或冒泡泡）。例如阻止按钮点击事件传播到body：

  - ```js
    var btn = document.getElementById("btn");
    
    btn.onclick = function (event) {
        alert("btn.onclick");
        event.stopPropagation();
    };
    
    document.body.onclick = function (event) {
        alert("body.onclick");
    };
    //如果不调用stopPropagation()方法就会弹出两个警告框。
    ```

- eventPhase属性：用来确定事件正处于事件流的哪个阶段。

  - ```js
    var btn = docuemnt.getElementById("btn");
    
    btn.onclick = function (event) {
        alert(event.eventPhase);		//2
    };
    //这里要注意，尽管 “ 处于目标 ” 发生在冒泡阶段，但eventPhase仍然等于2
    //而且，当eventPhase=2时，this、target和currentTarget的值始终都是相等的。
    
    document.body.addEventListener("click", function (event) {
        alert(event.eventPhase);		//1
    }, true);//true->捕获阶段
    
    document.body.onclick = function (event) {
        alert(event.eventPhase);		//3
    };//以这种方式添加的事件处理程序会在冒泡阶段被处理
    ```

只有事件处理程序执行期间，event对象才会存在；一旦事件处理程序执行完成，event对象就会被销毁。

### IE中的事件对象

与访问DOM中的event对象不同，要访问IE中的event对象有几种不同的方法，取决于指定事件处理程序的方法。

- 在使用DOM0级方法添加事件处理程序时，event对象作为window对象的一个属性存在。

  - ```js
    var btn = document.getElementById("btn");
    
    btn.onclick = function () {
        var event = widnow.event;//取得event对象
        alert(event.type); 		//click
    };
    ```

- 在使用attachEvent()添加的，那么就会通过传参将event对象传入事件处理程序函数中。

  - ```js
    var btn documeng.getElemntById("btn");
    
    btn.atttachEvent("onclick", function (event) {
        alert(event.type);  //"click"
    });
    //这里也可以通过window对象来访问event对象。不过为了方便起见，同一个对象也会作为参数传递。	
    ```

- 如果是通过HTML特性指定的事件处理程序，可以通过event变量来访问event对象。

  - ```html
    <input  type="button" value="Click Me" onclick="alert(event.type)"/>
    ```

IE 的event对象同样也包含与创建它的事件的属性和方法：

| 属性/方法    | 类型    | 读/写 | 说明                                          |
| ------------ | ------- | ----- | --------------------------------------------- |
| cancelBubble | Boolean | 读/写 | 默认为false，修改为true可以取消事件冒泡       |
| returnValue  | Boolean | 读/写 | 默认为true，修改为false可以取消事件的默认行为 |
| srcElement   | Element | 只读  | 事件的目标（与DOM中的target相同）             |
| type         | String  | 只读  | 被触发的事件类型                              |

因为事件处理程序的作用域是根据指定它的方式来确定的，所以不能认为this会始终等于事件目标。故而，最好还是使用event.srcElement比较保险：

```js
var btn = document.getElementById("btn");

btn.onclick = function (event) {
    alert(window.event.srcElement ==== this);		//true
};

btn.attachEvent("onclick", function (event) {
    alert(event.srcElement === this);		//false，因为this为window。
});
```

- returnValue属性：取消给定事件的默认行为。只要将值设置为false即可：

  - ```js
    var link = document.getElementById("link");
    
    link.onclick = function () {
        window.event.returnValue = false;
    };
    ```

  - 但是与DOM的preventDefault()方法不同的是这里不能确定事件是否被取消。defaultPrevented=true表示已经调用过了preventDefault()方法取消了默认行为。

### 跨浏览器事件对象

IE中event对象的全部信息和方法DOM对象中都有，只不过实现方法不一样。

可以对前面介绍的EventUtil对象加以增强，添加如下方法求同存异：

```js
var EventUtil = { 		//用来处理浏览器之间差异的对象
    addHander: function (element, type, handler) {//添加事件
        if(element.addEventListener){
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent){//IE
            element.attachEvent("on" + type, handler);
        } else{//如果其他方法无效，默认采用DOM0的方法
            element["on" + type] = handler;
        }
    },
    removeHander: function (element, type,handler) {//移除事件
        if(element.removeEventListener){
            element.removeEventListener(type, handler, false);
        } else if(element.detachEvent){
            element.detachEvent("on" + type, handler);
        } else {
            element.["on" + type] = null;
        }
    },
    getEvent: function (event) {//取得event
        return event ? event : window.event;
    },
    getTarget: function (event) {//取得target
        return event.target || event.srcElement;
    },
    preventDefault: function (event) {//阻止默认事件
        if(event.preventDefatult){
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },
    stopPropagation: function (event) {//阻止事件传递（捕获和冒泡）
        if(event.stopPropagation){
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
};

//在使用这些方法时，必须有一个事件对象传入到事件处理程序中，而且要把该变量传给这个方法，如下所示：
//使用上述自定义方法：
var btn = document.getElementById("btn");
btn.onclick = function (event) {
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
};

var link = document.getElementById("link");
link.onclick = function (event) {
    event = EventUtil.getEvent(event);
    EventUtil.preventDefault(event);
};

var btn1 = document.getElementById("btn1");
btn1.onclick = function (event) {
    alert("click btn1");
    event = EventUtil.getEvent(event);
    EventUtil.stopPropagation(event);
};
document.body.onclick = function (event) {
    alert("click body");
};
//别忘了IE不支持事件捕获，因此这个方法在跨浏览器的情况下，也只能用来阻止事件冒泡。
```

## 事件类型

DOM3级事件  规定了一下几类事件：

- UI事件，当用户与页面上的元素交互时触发。
- 焦点事件
- 鼠标事件
- 滚轮事件
- 文本事件，当在文档中输入文本时触发。
- 键盘事件
- 合成事件，当为IME（输入法编辑器）输入字符时触发。
- 变动事件，当底层DOM结构发生变化时触发。
- 变动名称事件，当元素或属性名变动时触发，（此类事件已被废弃）

### UI事件

UI事件指的是不一定与用户操作有关的事件。

现有的UI事件如下：

- load：当页面完全加载完后再window上触发，当所有框架都加载完后再框架集上触发，当图片加载完时在<img>元素上触发，或者当嵌入内容加载完毕时在<object>元素上面触发。

  - ```js
    EventUtil.addHandler(window, "load", function (event){
        alert("Loaded!");
    });
    ```

  - ```html
    <html>
        <head>
            
        </head>
        <body onload="alert('Loaded!')">
            <p>
                实际上，在window上发生的任何事件都可以在body上面通过相应的特性来指定，所有浏览器也很好的支持这种方式，但这HTML与js耦合性太高，调试起来不方便。
            </p>
        </body>
    </html>
    ```

  - 建议使用JavaScript的方法（前者）

  - 图片上也可以出发load事件：

    - ```js
      //也可以在标签上定义，这里就不写了
      var img = document.getElementById("img");
      EventUtil.addHandler(img, "load", function (event){
          event = EventUtil.getEvent(event);
          alert(EventUtil.getTarget(event).src);
      });
      ```

  - 在创建新的img元素时，可以为其指定一个事件处理程序，以便图像加载完毕后给出提示。此时，最重要的是在指定src属性之前先指定事件：

    - ```js
      EventUtil.addHandler(window, "load", function () {//先为window指定了load事件的原因在于，我们是想向DOM添加新元素，所以必须先确定页面已经加载完毕————如果在页面加载前操作document.body会导致错误
          var img = document.createElement("img");
          EventUtil.addHandler(img, "load", function (event) {
              event = EventUtil.getEvent(event);
              alert(EventUtil.getTarget(event).src);
          });
          document.body.appendChild(img);
          img.src = "smile.gif";//这里有一点需格外注意：只要设置了src属性就会开始下载，所以要先给img指定load事件，后插入到页面中。
      });
      ```

  - 在不属于DOM文档的图像上触发load事件时，IE8-版本不会生成event对象。IE9修复了这个问题。

  - \<script\>元素也可以添加load事件：

    - ```js
      EventUtil.addHandler(window, "load", function () {
          var script = document.createElement("script");
          EventUtil.addHandler(script, "load", function (event) {
              alert("Loaded");
          });
          script.src = "test.js";//只有设置了script元素的src并且将其添加到文档后才会开始下载，所以此时两条语句的顺序并不重要了。
          document.body.appendChild(script);
      });
      ```

  - IE和Opera还支持\<link\>元素的load事件。（在未指定src并且插入到文档中时不会下载样式表）

- unload：当页面完全被卸载后再window上触发，当所有框架都卸载完后再框架集上触发，或者当嵌入内容都卸载完毕后再<object>上触发。

  - 只要用户从一个页面切换到另一个页面，就会发生unload事件。而利用这个事件最多的情况是清除引用，以避免内存泄漏问题：

    - ```js
      //也可以在body中添加特性来指定unload事件处理程序
      EventUtil.addHandler(window, "unload", function (event) {
          alert("unlooad");
      });
      ```

    - 要小心编写unload事件处理程序的代码。既然unload事件是在一切都被卸载之后才触发，那么在页面加载后存在的那些对象，此时就不一定存在了。  此时，操作DOM节点或者元素的样式就会导致错误。

- abort：在用户停止下载过程时，如果嵌入的内容还没加载完，则在<object>上触发。

- error：当发生JavaScript错误时在window上面触发，当无法加载图像时在<img>上触发，当无法加载嵌入内容时在<object>元素上面触发，或者当有一或多个框架无法加载时在框架集上触发。

- select：当用户选择文本框中的一或多个字符时触发。

- resize：当窗口或框架的大小变化时在window或框架上触发。

  - 还是一样推荐JavaScript方式：

    - ```js
      EventUitl.addHandler(window, "resize", function (event) {
          alert("Resize!");
      });
      ```

    - 关于何时出发resize事件，不同浏览器有不同的机制

      - IE、Safari、Chrome和Opera会在浏览器窗口变化了1px就触发resize事件，然后随着变化不断触发。
      - Firefox则只会在用户停止调整窗口大小时才会触发resize事件。
      - 由于存在这个差别，应该注意不要在这个处理程序中加入过多的代码，因为这里的代码可能会频繁的执行，从而影响性能。
      - 浏览器窗口最小化和最大化也会触发resize事件。

- scroll：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。<body>元素中包含所加载页面的滚动条。

  - ```js
    EventUtil.addHandler(window, "scoll", function (event) {
        if(document.compatMode === "CSS1Compat"){//标准模式下
         	alert(document.doocumentElement.scrollTop);
        } else {//混杂模式下 compatMode === BackCompat
            alert(document.body.scrollTop);
        }
    });
    ```

  - 与resize事件类似，scroll事件也会在文档被滚动期间重复被触发，所以有必要尽量保事件处理程序的代码简洁。

上述事件在DOM2级事件中都归为HTML事件。

要确定浏览器是否支持DOM2级事件规定的HTML事件：

```js
var isSupported = document.implementation.hasFeature("HTMLEvents", "2.0");
//确定浏览器是否支持DOM3级事件规定的事件：
var isSUpported = document.implementation.haasFeature("HTMLEvents", "3.0");
```

### 焦点事件

焦点事件会在元素获得或失去焦点时触发。

有以下几个焦点事件：

- blur：元素失去焦点时触发，不冒泡，所有浏览器都支持。
- focus：元素获得焦点时触发，不冒泡，所有浏览器都支持。
- focusin：元素获得焦点时触发，但会冒泡。IE，Safari，Opera和Chrome部分版本支持。
- focusout：元素失去焦点时触发，但会冒泡。IE，Safari，Opera和Chrome部分版本支持。

当焦点从页面中的一个元素移动到另一个元素，会依次出发下列事件：

1. focusout：失去焦点元素
2. focusin：获得焦点元素
3. blur：失去焦点元素
4. focus ：获得焦点元素

要确定浏览器是否支持这些事件：

```js
var isSupported = document.implementation.hasFeature("FocusEvent", "3.0");
```

即使focus不冒泡，也可以在捕获阶段侦听到它们。

### 鼠标与滚轮事件

DOM3级事件中定义了9个鼠标事件：

1. click：单击或按下回车时触发。（意味着onclick事件也可以通过键盘触发）
2. dbclick：双击触发。
3. mousedown：在用户按下任意鼠标按钮时触发。
4. mouseenter：光标移动到元素内时触发。不冒泡，而且光标移动到后代元素上不触发。IE、Firefox9+、Opera支持。
5. mouseleave：光标移动到元素外时触发。不冒泡，而且光标移动到后代元素上不触发。IE、Firefox9+、Opera支持。
6. mousemove：光标在元素内部移动时重复地触发。
7. mouseout：光标从一个元素移动到另一个元素时触发，另一个元素可在前一个元素的外部或内部。
8. mouseover：光标位于元素外部时，首次移入另一个元素边界之内时触发。
9. mouseup：在用户释放鼠标按钮时触发。

上述事件除了mouseenter和mouseleave之外，所有鼠标事件都会冒泡，也可被取消，而取消鼠标事件将会影响浏览器的默认行为，从而影响其他事件，因为鼠标事件与其他事件是密不可分 的。

事件触发的顺序如下：

1. mousedown（按下鼠标）
2. mouseup（释放鼠标）
3. click（才触发点击事件）
4. mousedown
5. mouseup
6. click
7. dbclick（两次快速点击事件触发双击事件）

显然，click和dbclick事件都会依赖于其他先行事件的触发。

检测浏览器是否支持DOM2级事件（除dbclick、mouseenter和mouseleave之外）：

```js
var isSupported = document.implementation.hasFeature("MouseEvents", "2.0");

//要检测是否支持上述所有事件：
var isSupported = document.implementation.hasFeature("MouseEvent", "3.0");//这里是MouseEvent而非MouseEvents
```

鼠标事件中还有一类滚轮事件：mousewheel事件。

#### 1.客户区坐标位置

鼠标事件都是在  <font color="#f40">浏览器视口</font>  中的特定位置上发生的。这个位置信息保存在事件对象的clientX和clientY属性中。所有浏览器都支持这两个属性，它们的值表示事件发生时鼠标指针在视口中的水平和垂直坐标。

取得鼠标事件在浏览器视口的坐标：

```js
var div = document.getElementById("div1");
EventUtil.addHandler(div, "click", function (event) {
    event = EventUtil.getEvent(event);
    alert("Client coordinates:" + event.clientX + "," + event.clientY);
    //注意，这些值不包括页面滚动的距离，因此这个位置并不表示鼠标在页面上的位置
});页面坐标位置
```

#### 2.页面坐标位置

页面坐标位置通过事件对象的pageX和pageY属性保存事件发生时相对于  <font color="#f00">页面的位置</font>  。因此坐标是从页面本身计算的。

取得鼠标事件在页面中的坐标：

```js
var div = document.getElementById("div2");
EventUtil.addHandler(div, "click", function (event) {
    event = EventUtil.getEvent(event);
    alert("Page coordinates:" + event.pageX + "," + event.pageY);
});
```

IE8-版本不支持事件对象上的页面坐标，不过可以用客户区坐标和滚动信息计算出来：

```js
var div = document.getElementById("div3");
EventUtil.addHandler(div, "click", function (event) {
    event = EventUtil.getEvent(event);
    var pageX = event.pageX,
        pageY = event.pageY;
    
    if(pageX === undefined){//IE8-
        //视口位置 = 客户区坐标位置 + 滚动位置。
        pageX = event.clientX + (document.body.scrollLeft || document.documentEvent.scrollLeft);
    }
    
    if(pageY === undefined){
        pageY = event.clientY + (document.body.scrollTop || document.documentElement.sscrollTop);
    }
    
    alert("Page coordinates:" + pageX + "," + pageY);
});
```

#### 3.屏幕坐标位置

通过screenX和screenY属性就可以确定鼠标事件发生时鼠标指针相对于  <font color="#f49">整个屏幕</font>  的坐标信息。

取得鼠标事件在整个屏幕中的坐标信息：

```js
var div = document.getElementById("div4");
EventUtil.addHandler(div, "click", function (event) {
    event = EventUtil.getEvent(event);
    alert("Screen coordinates:" + event.screenX + "," +event.screenY);
});
```

#### 4.修改键

- Shift
- Ctrl
- Alt
- Meta（在Windows上是Windows键，在苹果上是Cmd键）

它们经常被用来鼠标事件的行为。

当某个鼠标事件触发时，通过检测这几个属性就可以确定用户是否同时按下了其中的键：

```js
var div = document.getElementById("div5");
EventUtil.addHandler(div, "click", function (event){
    event = EventUtil.getEvent(event);
    var keys = new Array();
    
    switch(true){
        case event.shiftKey:
            keys.push("shift");
            break;
        case event.ctrlKey:
            keys.push("ctrl");
            break;
        case event.altKey:
            keys.push("alt");
            break;
        case event.metaKey://IE8-不支持此属性
            keys.push("meta");
            break;
    }
    
    alert("Keys" + keys.join(","));
});
```

#### 5.相关元素

如果鼠标指针一开始在body里的一个div上面，然后移出了这个元素，那么在div上面就会触发mouseout事件，相关元素就是body。与此同时，body元素上面会触发mouseover事件，而相关元素就是div。

DOM通过event对象的relatedTarget属性提供了相关元素的信息。但只对于mouseout和mouseover事件才包含值，对于其他事件，这个属性的值是null。

IE8-版本不支持relatedTarget属性。但提供了保存相同信息的不同属性：在mouseover事件触发时，IE的fromElement属性中保存了相关元素；在mouseout事件触发时，IE的toElement属性中保存了相关元素。

可以把下面这个跨浏览器取得相关元素的方法添加到EventUtil中：

```js
var EventUtil = {
    //省略了其他代码
    getRelatedTarget: function (event) {
        if(event.relatedTarget){
            return event.relatedTarget;
        } else if (event.toElement){
            return event.toElement;
        } else if (event.fromElement){
            return event.fromElement;
        } else {
            return null;
        }
    }
};
```

```js
//使用getRelatedTarget（）
var div = document.getElementById("div6");
EventUtil.addHandler(div, "mouseout", function (event) {
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);//先获得事件发生目标
    var relatedTarget = EventUtil.getRelatedTarget(event);//然后获得事件相关元素
    alert("Mouse out of" + target.getName + "to" + relatedTarget.getName);
});
```

#### 6.鼠标按钮

对于mousedown和mouseup事件来说，event对象存在一个button属性：0=左键（主按钮），1=中间键（滚轮按钮），2=右键（次按钮）

IE8-版本也提供了button属性 ，但这个属性的值与DOM的button属性有很大差异：

- 0：没按
- 1：按左键
- 2：按右键
- 3：按左、右键
- 4：按中间键
- 5：按左键和中间键
- 6：按右键和中间键
- 7：按左、中、右三个键

只要将IE的其它选项分别转换成如同按下这个三个按键中的一个即可（同时将转牛作为优先选取的对象）。

由于单独使用能力检测无法确定差异（因为二者都有同名的button属性），因此必须另辟蹊径。我们知道，支持DOM版鼠标事件的浏览器可以通过hasFeature()方法来检测，所以可以再EventUtil对象添加如下getButton()方法：

```js
var EventUtil = {
    //省略了其他代码
    getButton: function (event) {
        if(document.implementation.hasFeature("MouseEvents", "2.0")){
            return event.button;
        } else {
            switch(event.button){
                case 0:
                case 1:
                case 3:
                case 5:
                case 7:
                    return 0;
                case 2:
                case 6:
                    return 2;
                case 4:
                    return 1;	
            }
        }
    }
};

//使用
var div = document.getElementById("div7");
EventUitl.addHandler(div, "mousedown", function (event) {
    event = EventUtil.getEvent(event);
    alert(EventUtil.getButton(event));
});
```

此外，如果不是按下或释放主鼠标按钮，Opera不会触发mouseup和mousedown事件。

#### 7.更多的事件信息

DOM2级事件规范在event对象中还提供了了detail属性，用于给出有关事件的更多信息，对于鼠标事件来说，detail中包含了一个数值，表示在给定位置上发生了多少次单击。  如果鼠标在mousedown和mouseup之间移动了位置，则detail会被重置为0.

IE也通过下列属性为鼠标事件提供更多的信息：

- altLeft：布尔值，表示是否按下了Alt键。
- ctrlLeft：布尔值，表示是否按下了Ctrl键。
- shiftLeft：布尔值，表示是否按下了shift键。
- offsetX：光标相对于目标元素边界的x坐标。
- offsetY：光标相对于目标元素边界的y坐标。

这些属性的用处并不大，因为只有IE支持它，而且提供的信息价值不大，可以通过其他方式计算得来。

#### 8.鼠标滚轮事件

mousewheel事件：用户滚动滚轮就会触发，可以在任何元素上面触发，最终会冒泡到document（IE8）或widow（IE9、Opera、Chrome和Safari）对象。

与mousewheel事件对应的event对象还包含一个特殊的wheelDelta属性。滚轮向前（上）滚动，wheelDelta是120的倍数；滚轮向后（下）滚动，wheelDelta是-120的倍数。

举例：

```js
EventUtil.addHandler(document, "mousewheel", function (event) {
    event = EventUtil.getEvent(event);
    alert(event.wheelDelta);
});
//多数情况下只需要知道滚轮滚动的方向就够了，所以只需检测wheelDelta的正负
```

有一点要注意，Opera9.5-版本中wheelDelta值的正负号是颠倒的，如果你打算兼容此版本，那么就需要做检测：

```js
EventUtil.addHandler(ddocument, "mousewheel", function (event) {
    event = EventUtil.getEvent(event);
    var delta =  (client.engine.opera && client.engine.opera < 9.5 ? -event.wheelDelta : event.wheelDelta);
    alert(delta);
});
```

Firefox支持一个名为DOMMouseScroll的类似事件，也是在鼠标滚动的时候触发。它包含与鼠标事件有关的所有属性。而有关鼠标滚轮的讯息保存在detail属性中，当向前滚动鼠标滚轮，这个属性值是-3的倍数，当向后滚动时时3的倍数。

若要给出跨浏览器环境下的解决方案，第一步就是要创建一个能够去的鼠标滚轮增量值的方法。

下面是我们添加到EventUtil对象中的方法：

```js
var EventUtil = {
    //
    getWheelDelta: function (event) {//获取鼠标滚轮增量值
        if(event.wheelDelta){
            return (client.engine.opera && client.engine.opera < 9.5 ? -event.wheelDelta : event.wheelDelta);
        } elae {
            return -event.detail * 40;
        }
    }
};

//使用此方法
(function (){//将相关代码放到私有作用域中，从而不会让新定义的函数干扰全局作用域
    function handleMouseWheel(event){
        event = EventUtil.getEvent(event);
        var detail = EventUtil.getWheelDelta(event);
        alert(detail);
    }
    //由于这里要重复用handleMouseWheel函数，所以将此区域放到一个立即执行函数中
    EventUtil.addEvent(document, "mousewheel", handleMouseWheel);
    EventUtil.addEvent(document, "DOMMouseScroll", handleMouseWheel);
})();
```

#### 9.触摸设备

iOS和Android设备的实现非常特别，因为这些设备没有鼠标，在面向触摸设备浏览器开发时，要注意以下几点：

- 不支持dbclick。双击浏览器窗口会放大画面，而且没有办法改变该行为。
- 轻击可单击元素会触发mousemove事件。如果此操作或导致内容变化，将不再有其他事件发生；如果没变化，那么会依次发生mousedown、mouseup和click事件。轻击不可单击的元素不会触发任何事件。
- mousemove事件也会触发mouseover和mouseout事件。
- 两个手指放在屏幕上且界面随手指移动而滚动时会触发mousewheel和scroll（当用户滚动带滚动条的元素中的内容时，在该元素上面触发。）事件。

#### 10.无障碍性问题

如果你的web应用程序或网站要确保残疾人特别是那些使用屏幕阅读器的人都能访问，那么在使用鼠标事件时就要格外小心。因为click可以通过键盘回车键触发，所以不建议使用click之外的其他鼠标事件来展示功能或引发代码执行，因为这样会给盲人造成极大的不便。

### 键盘与文本事件

DOM3级事件  为键盘事件制定了规范：

有3个键盘事件，如下：

- keydown：按下任意键时触发，如果按住不放就会重复触发此事件。
- keypress：按下字符键时触发，如果按住不放就会重复触发此事件。按下ESC键也会触发这个事件。
- keyup：释放键盘上的键时触发。

只有一个文本事件：

- textInput：在文本插入文本框之间会触发textInput事件。

在用户按下了一个字符键时：keydown -->keypress  -->keyup。其中keydown和keypress都是在文本框发生变化之前被触发的；而keyup事件则是在文本框已经发生变化后被触发的。 如果用户按下一个字符键不放，就会重复触发keydown和keypress事件，直到松开为止。

在用户按下了一个非字符键时，keydown -->keyup.如果按住不放，就会重复触发keydown事件，直到松开为止，然后触发keyup事件。

键盘事件与鼠标事件一样，都支持相同的修改键。而且键盘事件的事件对象中也有shiftKey、ctrlKey、altKey和metaKey属性，IE不支持metaKey属性。

#### 1.键码

在发生keydown和keyup事件时，event对象的keyCode属性中会包含一个键码：对数字字母字符键，keyCode属性值与ASCII码中对应的编码值相同。而非字符键码，emmm，用的时候自己查吧，有点多，在P380页。

DOM和IE的event对象都支持keyCode属性：

```js
var textbox = document.getElementById("textbox");
EventUtil.addHandler(textbox, "keydown", function (event) {
    event = EventUtil.getEvent(event);
    alert(event.keyCode);
});
```

非字符键码表略。

无论keydown还是keyup事件都会存在一些特殊情况：

​	在Firefox和Opera中，分号键的keyCode是59，也就是ASCII码中的分号编码；但IE和Safari返回186，即键盘中按键的键码。

#### 2.字符编码

发生keypress事件意味着按下的键会影响到屏幕中文本的显示。在所有浏览器中，按下能够插入或删除的键都会触发keypress事件；按下其他的键是否能够触发此事件因浏览器而异。

IE9、Firefox、Chrome和Safari的event对象都支持一个charCode属性，只有在发生keypress事件时才包含值，而且这个值值是按下的那个键所代表的字符的ASCII编码。	在IE8-和Opera中则是在keyCode中保存的ASCII编码。

跨浏览器解决方案：

```js
var EventUtil = {
    //
    getCharCode: function (event) {
        if(typeof event.charCode == "number"){//在不支持这个属性的浏览器中的值为undefined
            return event.charCode;
        } else {
            return event.keyCode;
        }
    }
};

//使用这个方法
var textbox = document.getElementById("textbox");
EventUnit.addHandler(textbox, "keypress", function (event) {
   event = EventUnit.getEvent(event);
    alert(EventUtil.getCharCode(event));
});
//在取得了字符编码之后，就可以使用String.fromCharCode()将其转换成实际的字符。
```

#### 3.DOM3级变化 

DOM3级事件中的键盘事件，不再包含charCode属性，而是包含两个新属性：

- key：取代keyCode属性，它的值是一个字符串。在按下某个键时，key的值就是相应的文本（如k  或M）；在按下非字符键时，key 的值是相应的键名（如Shift或Down）。
- char：在按下字符键时与key的值相同，但按下非字符键时为null。

IE9支持key，但不支持char。

Safari和Chrome支持名为keyIdentifier属性，在按下费字符键时与key的值相同，对于字符键，它返回一个格式类似“U+0000”  的字符串，表示Unicode值。

```js
var textbox = document.getElementById("textbox");
EventUtil.addHandler(textbox, "keypress",function (event) {
    event = EventUtil.getEvent(event);
    var identifier = event.key || event.keyIdentifier;
    if(identifier){
        alert(identifier);
    }
});
```

由于存在跨浏览器问题，所以本书不推荐使用key、keyIdentifier或char。

DOM3级事件新添加一个名为location的属性，这是一个数值，表示按下了什么位置上的键（如默认键盘，左侧键盘，右侧键盘，手柄等）。

但支持location的浏览器也不多，所以在跨浏览器开发中不推荐使用。

最后给event对象添加了getModifierState()方法，接受一个str参数，表示要检测的修改键，如果指定修改键是活动的，则为true，反之为false。

但支持getModifierState()方法的只有IE9，所以也不建议使用。

#### 4.textInput事件

由DOM3级事件引入，当用户在可编辑区域中输入字符时，就会触发这个事件。

由于textInput事件主要考虑的是字符，因此它的event对象中还包含一个data属性，这个属性的值就是用户输入的字符（而非字符编码）。

举例：

```js
var textbox = document.getElementById("textbox");
EventUtil.addHandler(textbox, "textInput", function (event) {
    event = EventUtil.getEvent(event);
    alert(event.data);
});
```

另外，event对象还有一个属性，叫inputMethod，表示吧文本输入到文本框中的方式。具体的值有0~9，分别表示具体输入方法，详情请见P383。

支持textInput属性的浏览器有IE9+、Safari和Chrome。

只有IE支持inputMethod属性。

#### 5.设备中的键盘事件

任天堂Wii会在用户按下Wii遥控器上你的按键时触发键盘事件。具体按键及键码键P384，或者自行百度搜索，谢谢合作。

### 复合事件

DOM3新添加的事件，用于处理IME（Input Method Editor，输入法编辑器）的输入序列。可以让用户输入在物理键盘上好不到的字符。

有以下三种复合事件：

- compositionstart：打开IME文本复合系统时触发。
- compositionupdate：在向输入字段插入新字符时触发。
- compositionend：关闭IME文本复合系统时触发。

三种复合事件的事件对象比文本事件多一个属性date，其中包含以下几个值中的一个：

- 如果是在compsitionstart事件发生时访问，data值为正在编辑的文本（如已经选中马上就要替换的文本）。
- 如果是在compsitionupdate事件发生时访问，data值为正插入的新字符。
- 如果是在compsitionend事件发生时访问，包含此次输入的所有字符。

但IE9+是到2011年唯一支持复合事件的浏览器，由于缺少支持，所以它用处不大。

### 变动事件

DOM2级的变动事件能在DOM中的某一部分发生变化时给出提示。

DOM2级定义了如下变动事件：

- DOMSubstreeModified：在DOM结构中发生任何变化时触发。这个事件在其他任何事件触发后都会触发。
- DOMNodeInserted：在一个节点作为子节点被插入到另一个节点中时触发。
- DOMNodeRemoved：在节点从其父节点中移除时触发。
- DOMNodeInsertedIntoDocument：在一个节点被直接插入文档或通过子树间接插入文档之后触发。在DOMNodeInserted之后触发。
- DOMNodeRemovedFromDocument：在一个节点被直接或间接从文档中移除之前触发。在DOMNodeRemoved之后触发。
- DOMAttrModified：在特性被修改后触发。
- DOMCharacterDataModified：在文本节点的值发生变化时触发。

检测浏览器是否支持：

```js
var isSupport = document.implementation.hasFeature("MutationEvents", "2.0");
```

IE8-版本不支持任何变动事件。

#### 1.删除节点

在使用removeChild()——删除节点  或replaceChild()——替换节点  从DOM中删除节点时，首先触发DOMNodeRemoved事件。这个事件的目标（event.target）是被删除的节点，而event.relatedNode属性中包含着对目标节点的父节点的引用。在这个事件触发时，节点尚未从其父节点删除，因此parentNode属性仍然指向父节点。这个事件会冒泡，因而可以在DOM的任何层次上面处理它。

如果移除的节点包含子节点，那么子节点和本身会相继触发DOMNodeRemovedFromDocument事件。但不会冒泡，所以只有直接指定给其中一个子节点的事件处理程序才会被调用。这个事件的目标是相应的子节点或者那个被移除的节点。

紧随其后触发的是DOMSubtreeModified事件。这个事件的目标是被移除节点的父节点。

为了理解上述事件的触发过程，举个例子：

```html
<!DOCTYPE html>
<html>
   <head>
       <title>Node Removal Events Example</title>
    </head>
    <body>
        <ul id="myList">
            <li>Item 1</li>
            <li>Item 2</li>
            <li>Item 3</li>
        </ul>

        <p>
            假设要移除 ul 元素。此时，就会依次触发以下事件。
            1.ul 上触发DOMNodeRemoved事件。relatedNode属性等于document.body。
            2.ul 上触发DOMNodeRemoveFromDocument事件。
            3.ul 中的 li 及其文本节点上触发DOMNodeRemoveFromDocument事件。
            4.在document.body上触发DOMSubtreeModified事件，因为 ul 是document.body的直接子元素。
        </p>
	   
        <script>
            EventUtil.addHandler(window, "load", function (event){
                var list = document.getElementById("myList");
                
                EventUtil.addHandler(document, "DOMSubtreeModified", function (event) {
                    alert(event.type);
                    alert(event.target);
                });
                EventUtil.addHandler(document, "DOMSubtreeModified", function (event) {
                    alert(event.type);
                    alert(event.target);
                    alert(event.relatedNode);//返回目标节点的父节点
                });
                EventUtil.addHandler(list.firstChild, "DOMNodeRemovedFromDocument", function (event) {//此事件不会冒泡，所以添加到具体元素上
                    alert(event.type);
                    alert(event.target);
                });
                
                list.parentNode.removeChild(list);
            });
        </script>
     </body>
</html>
```

#### 2.插入节点

在使用appendChild()——添加子节点、replaceChild()——替换子节点 或 insertBefore()——把节点放在childNodes的指定位置上  向DOM中插入节点时，首先会触发DOMNodeInserted事件，这个事件的目标（event.target）是被插入的节点，而event.relatedNode属性中包含着对目标节点的父节点的引用。在这个事件触发时，节点已经插入到新的父节点中。这个事件会冒泡，因而可以在DOM的任何层次上面处理它。

紧接着，在新插入的节点上面触发DOMNodeInsertedIntoDocument事件。这个事件不会冒泡，因此必须在插入节点之前为它添加这个事件处理程序。这个事件的目标是被插入的节点。

最后一个触发的事件时DOMSubtreeModified，触发于新插入节点的 父节点。

举例说明：

```html
<!DOCTYPE html>
<html>
   <head>
       <title>Node Removal Events Example</title>
    </head>
    <body>
        <ul id="myList">
            <li>Item 1</li>
            <li>Item 2</li>
            <li>Item 3</li>
        </ul>
        <script>
            EventUtil.addHandler(window, "load", function (event){
                var list = document.getElementById("myList");
                var item = document.createElement("li");
                item.appendChild(document.creteTextNode("Item 4"));
                
                EventUtil.addHandler(document, "DOMSubtreeModified", function (event) {//冒泡
                    alert(event.type);
                    alert(event.target);
                });
                EventUtil.addHandler(document, "DOMNodeInserted", function (event) {//冒泡
                    alert(event.type);
                    alert(event.target);
                    alert(event.relateNode);
                });
                EventUtil.addHandler(item, "DOMNodeInsertedIntoDocument", function (event) {
                    alert(event.type);
                    alert(event.target);
                });
                
                list.appendChild(item);
            });
        </script>
     </body>
</html>
```

### HTML5事件

#### 1.contextmenu事件

用来表示何时显示上下文菜单，以便开发人员取消默认上下文菜单而提供自定义的菜单。

由于contextmenu事件是冒泡的，所以可以在document指定一个事件事件处理程序，用以处理页面上发生的所有此类事件。

取消这个事件：

- 在兼容DOM的浏览器中，使用event.preventDefault()；
- 在IE中，将event.returnValue的值设置为false。

举例：

```html
<html>
    <head>
        <title></title>
    </head>
    <body>
        <div id="myDiv">
            右键点击我出现上下文菜单
        </div>
        <ul id="myMenu" sytle="position:absolute; visibility:hidden; background-color:silver">
            <li><a href="">Nidhilas</a></li>
            <li><a href="">Nidhilas</a></li>
            <li><a href="">Nidhilas</a></li>
            <li><a href="">Nidhilas</a></li>
        </ul>
        
        <script>
            EventUtil.addHandler(window, "load", function (event){
                var div = document.getElementById("div");
                
                EventUtil.addHandler(document, "contextmenu", function (event){
                    event = EventUtil.getEvent(event);
                    EventUtil.preventDefault(event);//取消默认上下文
                    
                    
                    var menu = document.getElementById("myMenu");
                    menu.style.top = event.clientY + "px";
                    menu.style.left = event.clientX + "px";
                    menu.style.visbility = "visible";//设置元素是否可见		
                });
                
                EventUtil.addHandler(document,"click", function (event){
                    document.getElementById("myMenu").style.visibility = "hidden";
                });
            });
        </script>
    </body>
</html>
```

#### 2.beforeunload事件

这个事件在页面被卸载之前触发，可以通过它来取消卸载并且继续使用原有页面。

这个事件的意图是将控制权交个用户，显示消息告诉用户页面将被卸载，询问用户是否真的要关闭页面，还是希望继续留下来。

为了显示弹出对话框，必须将event.returnValue值设置为要显示给用户的字符串（对IE及Firefox而言），同时作为函数的值返回（对Safari及Chrome而言），如下所示：

```js
EventUtil.addHandler(window, "beforeunloadd", function (event) {
    event = EventUtil.getEvent(event);
    var message = "I'm really going to miss you if you go.";
    event.returnValue = message;
    return message;
});
```

Opera11-版本不支持beforeunload事件。

#### 3.DOMContentLoaded事件

window的load事件会在页面的一切都加载完毕时触发，但这个过程可能会因为外部资源加载过多而颇费周折。

而DOMContentLoaded事件则在已经形成完整的DOM树之后就会触发，不会理其他外部资源是否已经下载完毕。

与load事件不同，DOMContentLoaded事件支持在页面下载的早期添加事件处理程序，这样用户就可以尽早地与页面进行交互。

要处理DOMContentLoaded事件，可以为document或window添加相应的事件处理程序（尽管这个事件会冒泡到window，但它的目标实际上是document）。如下所示：

```js
EventUtil.addHandler(document, "DOMContentLoaded", function (event) {
    alert("Content loaded");
});
```

通常这个事件即可以添加事件处理程序，也可以执行其他DOM操作。这个事件始终都会在load事件之前触发。

对于不支持DOMContentLoaded的浏览器，我们建议在页面加载期间设置一个时间为0毫秒的超时调用，如下所示：

```js
setTimeout(function () {
    //在此添加事件处理程序
}, 0);
//为了保证这个方法有效，必须将其作为页面中的第一个超时调用
```

#### 4.readystatechange事件

这个事件提供与文档或元素的加载状态有关的信息，但这事件的行为有时候也很难预料。具体内容请参照P390.

支持readyStatechange事件的浏览器有IE、Firefox4+和Opera。

#### 5.pageshow和pagehide事件

Firefox和Opera有一个特性，叫 ” 往返缓存 “ （back-forward cache，或bfcache），可以在用户使用浏览器的  后退  和  前进  按钮时加快页面的转换速度。    这个缓存中实际上是将整个页面都保存在了内存里。如果页面位于bfcache中，那么再次打开就会更快，而且不触发load事件。

Firefox还提供了了一些新事件：

- pageshow事件：在页面显示时触发，无论页面是否来自bfcache。在重新加载的页面中，pageshow会在load事件触发后触发；而对于bfcache中的页面，pageshow会在页面状态完全恢复的那一刻触发。 另外要注意的是，虽然这个事件的目标是document，但必须将其事件处理程序添加到window。如下所示：

  - ```js
    (function () {//使用私有作用域，以防止变量showCount进入全局作用域
        var showCount = 0;
        
        EventUtil.addHandler(window, "load", function (event) {
            alert("window load");
        });
        
        EventUtil.addHandler(window, "pageshow", function (event) {
            showCount ++;
            alert("Show has been fired" + shiwCount + "times.Persisted?" + event.persisted);
            //pageshow事件的event对象上保存一个persisted布尔属性。如果页面被保存在bfcache中，persisted=true，否则为false。
        })
    })();
    ```

  - 如果离开页面后又单击后退，就会发现event.persited已经加一；但如果刷新页面，那么showCount的值就会被重置为0，因为页面已经完全重新加载了。

- pagehide事件：与pageshow事件对应，该事件会在浏览器卸载页面时候触发，而且是在unload事件触发之前触发。pagehide事件也在document上面触发，但其事件处理程序必须添加到window对象上，这个事件的event对象也包含persisted属性，不过其用途稍有不同。

  - ```js
    EventUtil.addHandler(window, "pagehide", function (event) {
        alert("Hiding. Persisted?" + event.persisted);
        //如果页面在卸载之后会被保存在bfcache中，那么persisted= true；
        //结合pageshow事件的persisted值，可以断定，当第一次打开页面时，persisted的值一定是false。
    });
    ```

支持这两个事件的浏览器有：Firefox、Safari5+、Chrome、Opera、IE10+。

指定了onunload事件（onunload 事件在用户退出页面时发生。）处理程序的页面会被自动排出在bfcache之外，即使事件处理程序是空的。原因在于，onunload最常用于撤销在onload中所执行的操作，而跳过onload后再次显示页面很可能就会导致页面不正常（因为在bfcache缓存中调用出来的页面是恢复成的，所以并不触发onload事件）。

#### 6.haschange事件

HTML5新增了haschange事件，以便在URL的参数列表发生变化时通知开发人员。

必须将haschange事件处理程序添加到window对象，然后URL参数列表只要变化就会调用它。此时event额外包含两个属性：oldURL和newURL。这两个属性分别保存着参数列表变化前后的完整URL。如下所示：

```js
EventUtil.addHandler(window, "haschange", function (event) {
    alert("Old URL:" + event.oldURL + "\nNew URL:" + event.newURL);
});
```

支持haschange事件的浏览器有IE8+、Firefox3.6、Safari5+、Chrome和Opera10.6+。在这些浏览器中，只有Firefox6.0+、Chrome和Opera支持oldURL和newURL属性。为此，最好使用location对象来确定当前的参数列表。

```js
EventUtil.addHandler(window, "haschange", function (event) {
    alert("Current hash:" + location.hash);
});
```

检测浏览器是否支持haschange事件：

```js
var isSupported = ("onhashchange" in window);//这里有bug
//如果是IE8在IE7文档模式下运行，即使功能无效它也会返回true，所以更稳妥的方式为：
var isSupported = ("onhashchange"in window) && (document.documentMode === undefined || document.documentMode > 7);
```

### 设备事件

可以让开发人员确定用户在怎样使用设备。

#### 1.orientationchange事件

苹果🍎公司为移动Safari中添加了orientationchange事件，以便开发人员能能够确定用户何时将设备由横向查看模式切换为纵向查看模式。

移动Safari的window.oriendation属性中可能包含3个值：

- 0表示肖像模式
- 90表示向左转的横向模式（“主屏幕” 按钮在右侧）
- -90表示向右转的横向模式

只要用户改变了设备的查看模式，就会触发orientationchange事件。

所有iOS设备都支持orientationchange事件和window.orientation属性。

#### 2.MozOrientation事件

Firefox3.6为检测设备的方向引入了一个名为MozOrientation的事件（前缀Moz表示这是特定于浏览器开发商的事件，不是标准事件）。    当设备的加速计检测到设备方向改变时，就会触发这个事件。

```js
EventUtil.addHandler(window, "MozOrientation", function (event) {
    var output = document.getElementById("output");
    output.innerHTML = "X=" + event.x + ",Y=" + event.y + ",Z=" + event.z + "<br>";
});//x,y,z分别表示向左、右及远离用户方向倾斜的角度数值
```

只有带加速计的设备才支持MozOrientation事件。但要注意，这是个实验性的api，将来坑你会变。

#### 3.deviceorientation事件

DeviceOrientation Event规范：DeviceOrientationEvent提供给网页开发者当设备（指手机，平板等移动设备）在浏览页面时物理旋转的信息。

此规范定义的deviceorientation事件与MozOrientation事件类似。它也是在加速计检测到设备方向变化时在window对象上触发，而且具有与MozOrientation事件相同的支持限制。

不过，deviceorientation事件的意图是告诉开发人员设备在空间中朝向哪儿，而不是如何移动。

设备在三维空间中是靠x，y，z轴来定位的。如图所示：

![设备方向](E:\TyporaFiles\img\设备方向.png)

触发deviceorientation事件时，事件对象中包含每个轴相对于设备静止状态下发生变化的信息。

事件对象包含以下5个值：

- alpha：在围绕z轴旋转时（即左右旋转时），y轴的度数差；是0~360之间的浮点数。
- beta：在围绕x轴旋转（即前后旋转时），z轴的度数差；是-180~180之间的浮点数。
- gamma：在围绕y轴旋转（即扭转设备时），x轴的度数差，是-90~90之间的浮点数。
- absolute：布尔值，表示设备是否返回一个绝对值。
- compassCalibrated：布尔值，表示设备的指南针是否校准过。

输出alpha、beta和gamma值：

```js
EventUtil.addHandler(window, "deviceorientation", function (event) {
    var output = document.getElementById("output");
    output.innerHTML = "Alpha=" + event.alpha + ", Beta=" + event.beta + ", Gamma=" + event.gamma + "<br>";
});
```

通过这些信息，可以响应设备的方向，重新排列或修改屏幕上的元素。

要相应设备方向的改变而旋转元素，可以参考如下代码：

```js
EventUtil.addHandler(window, "deviceorientation", function (event) {
    var arrow = document.getElementById("arrow");
    arrow.style.webkitTransform = "rotate(" + Math.round(event.alpha) + "deg)";
});
```

到2011年，支持deviceorientation事件的浏览器有iOS4.2+中Safari、Chrome和Android版WebKit。

#### 4.devicemotion事件

DeviceOrientation Event规范还定义了一个devicemotion（设备 运动）事件。这个事件是要告诉开发人员设备什么时候移动，而不仅仅是设备方向如何改变。

例如，通过devicemotion能够检测到设备是不是正在往下掉，或者是不是被走着的人拿在手里。

触发devicemoment事件时，事件对象包含以下属性。：

- acceleration：一个包含x、y和z属性的对象，在不考虑重力的情况下，告诉你在每个方向上的加速度。如果读取不到则值为null。
- accelerationIncludeGravity：一个包含x、y和z属性的对象，在考虑z轴重力的情况下，告诉你在每个方向上的加速度。如果读取不到则值为null。
- interval：以毫秒表示的时间值。必须在另一个devicemotion事件触发前传入。这个值在每个事件中应该是一个常量。
- rotationRate：一个包含表示方向的alpha、beta和gamma属性的对象。如果读取不到则值为null。

在使用这三个属性之前，应该先检测确定它们的值是不是null：

```js
EventUtil.addHandler(window, "deviceorientation", function (event) {
    var output = document.getElementById("output");
    if(event.rotationRage !== null){
    	output.innerHTML = "Alpha=" + event.alpha + ", Beta=" + event.beta + ", Gamma=" + event.gamma + "<br>";
    }
});
```

与deviceorientation事件类似，只有iOS4.2+中的Safari、Chrome和Android版WebKit实现devicemotion事件。

### 触摸与手势事件

W3C指定的Touch Events规范，对触摸事件的规范。

#### 1.触摸事件

以下几个触摸事件：

- touchstart：当手指触摸屏幕时触发；即使已经有一个手指放在了屏幕上也会触发。
- touchmove：当手指在屏幕上滑动时连续的触发。在这个事件发声期间，调用preventDefault()可以阻止滚动。
- touchend：当手指在屏幕上移开时触发。
- touchcancel：当系统停止跟踪触摸时触发。

上面这几个事件都会冒泡，也都可以取消。

它们都是以兼容DOM的方式实现的。因此，每个触摸事件的event对象都提供了在鼠标事件中常见的属性：

- bubbles：表示事件是否冒泡。
- cancelable：表示是否可以取消事件的默认行为。
- view：发生事件的window对象。
- clientX：鼠标事件在 浏览器视口 中的x坐标。
- clientY：鼠标事件在 浏览器视口 中的y坐标。
- screenX：鼠标事件在 屏幕视口 中的x坐标。
- screenY：鼠标事件在 屏幕视口 中的y坐标。
- detail：与事件相关的细节信息。
- altKey：是否按下Alt键。
- shiftKey：是否按下shift键。
- ctrlKey：是否按下ctrl键。
- metaKey：是否按下meta键。

移动端有四个关于触摸的事件，分别是touchstart、touchmove、touchend、touchcancel（比较少用）, 它们的触发顺序是touchstart-->touchmove-->touchend-->click，所以touch事件触发完成后会接着触发click事件，需要注意一下 ，阻止一下事件冒泡就可以了。 
touch事件可以产生一个event对象，这个event对象除基本的一些属性外还附带了三个额外的属性：

- touches：一个TouchList对象，包含当前**所有屏幕上**的触点的Touch对象，该属性不一定是当前元素的touchstart事件触发，需要注意。
- targetTouchs：也是一个TouchList对象，包含从当前元素touchstart事件触发的的触点的Touch对象，跟上面的touches区别于触发点不一样。
- changeTouches：也是一个TouchList对象，对于touchstart事件, 这个TouchList对象列出在此次事件中新增加的触点。对于touchmove事件，列出和上一次事件相比较，发生了变化的触点。对于touchend，列出离开触摸平面的触点（这些触点对应已经不接触触摸平面的手指）。

Touch对象和TouchList：

由Touch对象构成的数组，通过event.touches、event.targetTouches或者event.changedTouches取到。一个 Touch 对象代表一个触点，当有多个手指触摸屏幕时，TouchList就会存储多个Touch对象。Touch对象代表一个触点，可以通过TouchList[0]获取， 每个触点包含位置，大小，形状，压力大小，和目标 element属性。更多介绍可以查看[Touch属性介绍](https://developer.mozilla.org/zh-CN/docs/Web/API/Touch)。

每个Touch对象包含下列属性：

- clientX：触摸目标在视口中的x坐标。
- clientY：触摸目标在视口中的y坐标。
- indentifier：标识触摸的唯一ID。
- pageX：触摸目标在页面中的x坐标。
- pageY：触摸目标在页面中的y坐标。
- screenX：触摸目标在屏幕中的x坐标。
- target：触摸的DOM节点目标。

使用这些属性可以跟踪用户对屏幕的触摸操作，如下所示：

```js
fnction handleTouchEvent(event){
    //只跟踪一次触摸
    if(event.touches.length == 1){
        var output = document.getElementById("output");
        switch(event.type){
            case "touchstart"://输出触摸的位置信息
                output.innerHTML = "Touch started (" + event.touches[0].clientX + "," +event.touches[0].clientY + ")";
                break;
            case "touchend"://输出触摸的最终信息
                //注意，在touchend事件发生时，touches集合中就没有任何Touch对象了，因为不存在活动的触摸操作；此时，就必须转而使用changeTouchs集合。
                output.innerHTML = "<br>Touch ended (" + event.changeTouches[0].clientX + "," + event.changeTouches(0).clientY + ")";
                break;
            case "touchmove"://输出触摸的变化信息
                event.preventDefault();//阻止滚动（触摸移动的默认行为时滚动页面）
                output.innerHTML = "<br>Touch moved (" + event.changeTouches[0].chientX + "," + event.changeTouches(0).clientY + ")";
                break;
        }
    }
};

EventUtil.addHandler(document, "touchstart", handdleTouchEvent);
EventUtil.addHandler(document, "touchend", handdleTouchEvent);
EventUtil.addHandler(document, "touchmove", handdleTouchEvent);
```

在触摸屏幕上的元素时，这些时间（包括鼠标事件）发生的顺序如下：

1. touchstart
2. mouseover（从外界移入元素）
3. mousemove（一次）（从元素内部移动）
4. mousedown（按下鼠标按钮）
5. mouseup（释放鼠标按钮）
6. click
7. touchend

#### 2.手势事件

ios2.0中的Safari还引入了一组手势事件。当两个手指触摸屏幕时就会产生手势事件，手势通常会改变显示项的大小，护着旋转显示项。

有3个手势事件：

- gesturestart：当两个手指都触摸屏幕时触发。
- gesturechange：当触摸屏幕的任意手指的位置发生变化时触发。
- gestureend：：当任何一个手指从屏幕上面移开时触发。

由于这些事件冒泡，所以将事件处理程序放在文档上也可以处理所有手势事件。此时，事件的目标就是两个手指都位于其范围内的那个元素。

与触摸事件一样，每个手势事件的event对象都包含着标准的鼠标事件属性：

- bubbles（表示事件是否冒泡）
- cancelable（表示是否可以取消默认行为）
- view（发生事件的window对象）
- clientX（相对于浏览器视口的事件的x坐标）
- clientY（相对于浏览器视口的事件的y坐标）
- screenX（相对于屏幕视口的事件的x坐标）
- screenY（相对于屏幕视口的事件的y坐标）
- detail（事件的细节信息）
- altKey
- shiftKey
- ctrlKey
- metaKey

此外，还包含两个额外的属性：

- rotation：表示手指变化引起的旋转角度，负值表示逆时针旋转，正值表示顺时针旋转（该值从0开始）
- scale：表示两个手指间距离的变化情况（例如向内收缩会缩短距离）；该值从1开始，并随距离拉大而增长，随距离缩短而减小。

举例：

```js
function handleGestureEvent(event) {
    var output = document.getElementById("output");
    switch(event.type){
        case "gesturestart":
            output.innerHTML = "Gesture started (rotation=" + event.rotation + ", scale=" + event.scale + ")";
            break;
        case "gestureend":
            output.innerHTML = "<br>Gesture ended (rotation=" + event.rotation + ", scale=" + event.scale +")";
            break;
        case "gesturechange":
            output.innerHTMLL = "<br>Gesture change (rotation=" + event.rotation + ",scale=" + event.scale + ")";
            break;
    }
};

document.addEventListener("gesturestart", handleGestureEvent, false);
document.addEventListener("gestureend", handleGestureEvent, false);
document.addEventListener("gesturechange", handleGestureEvent, false);
```

## 内存和性能

在JavaScript中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能。

首先，每个函数都是对象，都会占用内存。

其次，必须事先指定所有事件处理程序而导致的DOM访问次数增多，会延迟整个页面的交互就绪时间。

### 事件委托

对 “ 事件处理程序过多 ” 问题的解决方案就是 事件委托。

事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。

例如，click事件会一直冒泡到document层次。也就是说，我们可以为整个页面指定一个onclick事件处理程序，而不必给每个可单击的元素分别添加事件处理程序。

以下面的HTML代码为例：

```html
<ul id="ul">
    <li id="goSomewhere">Go somewhere</li>
    <li id="doSomething">Do something</li>
    <li id="sayHi">Say hi</li>
</ul>


<script>
//传统做法：
    var item1 = document.getElementById("goSomewhere");
    var item2 = document.getElementById("doSomething");
    var item3 = document.getElementById("sayHi");
    //分别为每个元素添加一个click事件处理程序
    EventUtil.addHandler(item1, "click", function (event) {
        location.href="http://www.wrox.com";
    });
    EventUtil.addHandler(item2, "click", function (event) {
        location.href="http://www.wrox.com";
    });
    EventUtil.addHandler(item3, "click", function (event) {
        location.href="http://www.wrox.com";
    });
</script>

<script>
//利用事件委托的做法：
    //使用事件委托，只需在DOM树中尽量最高的层次上添加一个事件处理程序
    var list = document.getElementById("ul");
    
    EventUtil.addHandler(list, "click", function (event) {
        event = EventUtil.getEvent(event);
        var target = EventUtil.getTarget(event);
        //直接给它们的上级添加一个click事件处理程序，然后用switch-case语句判断当前点击目标的id，然后做出不同的回应
        switch(target.id){
            case "goSomewhere":
                location.href="http://www.wrox.com";
                break;
            case "doSomething":
                location.href="http://www.wrox.com";
                break;
            case "sayHi":
                location.href="http://www.wrox.com";
                break;
        }
    });
    //这种做法消耗更低，性能更好，因为它只取得一个DOM元素，只添加了一个事件处理程序。
</script>
```

如果可行的话，也可以靠考虑为document对象添加已给事件处理程序，用于处理页面上，发生的某种特定类型的事件。这样做与传统做法相比具有如下优点：

- document对象很快就可以访问，而且可以在页面生命周期的任何时间点上为它添加事件处理程序（无需等待DOMContentLoaded或load事件）。换句话说，只要可单击的元素呈现在页面上，就可以立即具备适当的功能。
- 在页面中设置事件处理程序的所需的时间更少。
- 整个页面占用的内存空间更少，能够提升整体性能。

最适合采用事件委托技术的事件包括：click、mousedown、mouseup、keydown、keyup和keypress（按下字符键触发）。虽然mouseover和mouseout事件也冒泡，但要是适当处理它们并不容易，而且经常需要计算元素的位置。

### 移除事件处理程序

每当事件处理程序指定给元素时，运行中的浏览器代码与js代码之间就会建立连接。这种连接越多，页面执行起来越慢。可以使用事件委托技术，另外，再不需要的时候移除事件处理程序，也是解决问题的一种方案。内存中留有不用的“空事件处理程序” 也是造成性能问题的主要原因。

在两种情况下可能会造成上述问题：

1. 用removeChild()或replaceChild()方法移除文档中带有事件处理程序的元素时。
2. 但更多的是发生在使用innerHTML替换页面中某一部分的时候，如果带有事件处理程序的元素被innerHTML删除了，那么原来添加到元素中的事件处理程序极有可能无法被当做垃圾回收。

来看下面例子：

```html
<div id="myDiv">
    <input type="button" value="Click Me" id="myBtn">
</div>
<script>
	var btn = document.getElementById("myBtn");
    btn.onclick = function (event){
      document.getElementById("myDiv").innerHTML = "Processing...";//麻烦啦！  
    };
    
    //为了避免双击，单击这个按钮时就将按钮移除并替换成一条消息；这是网站设计中非常流行的方法。
    //但问题在于，当按钮被从页面中移除时，按钮的事件处理程序仍然与按钮保持着引用关系，也依然在内存中。
    
    //如果你知道某个元素即将被移除，那么最好手工移除事件处理成程序：
    btn.onclick = function () {
        btn。onclick= null;
        document.getElementById("myDiv").innerHTML = "Processing...";
    };
</script>
```

注意，在事件处理程序中删除按钮也能阻止事件冒泡。目标元素在文档中是事件冒泡的前提。

采用事件委托也有助于解决这个问题，如果事先知道将来有可能使用innerHTML替换掉页面中的某一部分，那么就可以不直接把事件处理程序添加到该部分的元素中。而通过把事件处理程序指定给较高层次的元素，同样能够处理该区域中的事件。

导致“空事件处理程序“ 的另一种情况，就是页面卸载的时候。如果在页面被卸载之前没有清理干净事件处理程序，那它们就会滞留在内存中。

一般来说，最好的方法就是在页面被卸载之前，先通过onunload事件处理程序移除所以有事件处理程序。再次，事件委托技术再次表现出它的优势——需要跟踪的事件处理程序越少，移除它们越容易。对这种类似撤销的操作，我们可以把它想象成：只要是通过onload事件处理程序添加的东西，最后都要通过onunload事件处理程序将它们移除。

不要忘了，使用onunload事件处理程序意味着页面不会被缓存在bfcache中。如果你在意这个问题，那么就只能在IE中通过onunload来移除事件处理程序了。

## 模拟事件

事件经常有用户和浏览器来触发，但很少有人知道，其实也可以使用JavaScript在任意时刻来触发特定事件，而此时的事件就如同浏览器创建的事件一样。

在测试Web应用程序，模拟触发事件是一种极其有用的技术。

### DOM中的事件模拟

创建模拟事件的步骤：

- 创建event对象：
  - 在document上使用createEvetn()方法创建event对象。这个方法接收一个参数，即表示要创建的事件类型的字符串。
  - 这个字符串可以使以下几个字符串之一：
    - UIEvents：UI事件，鼠标事件和键盘事件都继承自UI事件，DOM3级中是UIEvent。
    - MouseEvents：鼠标事件，DOM3级中是MouseEvent。
    - MutationEvents：DOM变动事件，DOM3级中是MutationEvent。
    - HTMLEvents：HTML事件，DOM3级中是HTMLEvent。
  - 要注意的是，DOM2级事件 并没有专门规定键盘事件， 后来的DOM3级事件 中才正式将其作为一种事件给出规定。IE9是目前唯一支持DOM3级键盘事件的浏览器。
- 初始化信息：
  - 每种类型的event对象都有一个特殊的方法用来传入适量的数据来初始化该event对象。

- 触发事件：
  - 使用dispatchEvent()方法，传入一个参数，即表示要触发事件的event对象。触发的事件可以冒泡。

#### 1.模拟鼠标事件

```js
var btn = document.getElementById('btn');


//创建鼠标事件对象
var event = document.createEvent("MouseEvents");
//创建的对象有一个名为initMouseEvent()的方法，用来初始化event对象，该方法接收15个参数（type(str), bubbles(boolean), cancelable(booleana),view, deatill, screenX, screenY, clientX, clientY,ctrlKey, altKey, shiftKey, metaKey, button, relatedTarget(Object,表示与事件相关的对象，只在mousesover与mouseout中使用)）    前4个参数对正确的激发事件事关重要，因为浏览器要用到，其余的参数只有在事件处理程序中用到。

//初始化事件对象
event.initMouseEvent("click", true, true, document.defaultView, 0, 0, 0, 0, 0, false, false, false, false, 0, null);

//触发事件
btn.dispatchEvent(event);
```

在兼容DOM的浏览器中，可以用这种方式模拟其他鼠标事件（如dbClick）。

#### 2.模拟键盘事件

```js
var textbox = document.getElementById("textBox"),
    event;

//由于DOM2级事件没有规定模拟键盘事件，DOM3中有，所以要先做判断DOM级别
//而且DOM3级不提倡使用keypress事件，因此只能模拟keydown和keyup事件
//以DOM3级方式创建事件对象
if(document.implementation.hasFearure("KeyboardEvents", "3.0")){
    event = document.createEvent("KeyboardEvent");
    //创建的对象中有一个名为initKeyboardEvent()的方法，该方法接受下列参数（type, bubbles, cancelable, view, key, location, modifiers, repeat）
    
    //初始化event
    event.initKeyboardEvent("keydown", true, true, document.defaultView, "a", 0, "Shift", 0);
}
//触发事件
textbox.dispatchEvent(event);
//此例模拟了按住shift+a键。
```

而在Firefox中，用另一种方法：

```js
//只适用于Firefox
var textbox = document.getElementById("textbox");

//创建事件对象
var event = document.createElement("KeyEvents");
//event中包含一个initKeyEvent()方法用来初始化对象，它接受10个参数（type, bubbles, cancelable, view, ctrlKey, altKey, shiftKey, metaKey, keyCode, charCode）

//初始化event
event.initKeyEvent("keypress", true, true, document.defaultView, false, false, false, false, 65, 65);

//触发事件
textbox.dispatchEvent(event);
```

在其他浏览器中，则只需要创建一个通用的事件，然后再向事件对象中添加键盘事件特有的信息。例如：

```js
var textbox = document.getElementById("textbox");

//创建event
var event = document.createEvent("Events");

//初始化事件对象
event.initEvent(type, bubbles, cancelable);
event.view = document.defaultView;
event.altKey = false;
event.shiftKey = false;
event.charCode = 65;

//触发事件
textbox.dispatchEvent(event);

//在此必须使用通用事件，而不能使用UI事件，因为UI事件不允许向event对象中再添加新属性（Safari除外）。
//像这样模拟事件虽然会触发键盘事件，但却不会像文本框中向文本中写入任何文本，这是由于无法精确模拟键盘事件造成的。
```

#### 3.模拟其他事件

模拟变动事件：

```js
var event = docuemnt.createEvent("MutationEvents");
//创建一个包含initMutationEvent()方法的变动事件对象，接受5个参数（type, bubbles, cancelable, relatedNode, preValue, newValue, attrName, attrChange）;
event.initMutationEvent("DOMNodeInserted", true, false, someNode, "", "", "", 0);

target.dispatchEvent(event);
```

模拟HTML事件：

```js
var event = document.createEvent("HTMLEvents");

event.initEvent("focus", true, false);

target.dispatcheEvent(event);
```

浏览器很少使用变动事件和HTML事件，因为使用它们会受到一些限制。

#### 4.自定义DOM事件

自定义事件不是DOM原生触发的，它的目的是让开发人员创建自己的事件。

```js
var div = document.getElementById("div1"),
    event;
EventUtil.addHandler(div, "myevent", function (event) {
    alert("DIV:" + event.detail);
});
EventUtil.addHandler(document, "myevent", function (event){
    alert("DOCUMENT:" + event.detail);
});

if(dicument.implementation.hasFeature("CustomEvents", "3.0")){
    event = document.createEvent("CustonEvent");//创建自定义事件
    event.initCustonEvent("myevent", true, false, "Hello world!");//4个参数（触发的事件类型，是否冒泡，是否可以取消事件，任意值，保存在event.detail属性中）
}
```

支持自定义DOM事件的浏览器有IE9+和Firefox6+。

### IE中的事件模拟

```js
var btn = document.getElementById("btn");

//创建事件对象
var event = document.createEventObject();//不接受参数，结果返回一个通用的event对象，然后你必须给这个对象手工添加所有必要的信息。

//初始化事件对象
event.screenX = 100;
event.screenY = 0;
event.clientX = 0;
event.ctrlKey = false;
event.button = 0;
//这里可以添加任意属性，哪怕IE并不支持。

//触发事件
btn.fireEvent("onclick", event);//两个参数（事件处理程序名称， 事件对象）

//模拟任何IE支持的事件都采用相同的模式。
```

模拟keypress触发：

```js
var textbox = document.getElementById("textbox");

var event = document.createEventObject();

event.altKey = false;
event.keyCode = 65;

textbox.fireEvent("onkeypress", event);

//正如DOM中墨迹键盘事件一样，运行这个例子也不会因模拟了keypress而在文本框中看到任何字符，即使触发了事件处理程序。
```

## 小结

事件时JavaScript中最重要的主题之一，深入理解事件的工作机制以及它们对性能的影响至关重要。





















































































































































































































































































































































































 