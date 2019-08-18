# 第十四章 表单脚本

## 表单基础知识

在JavaScript中，表单对应的是HTMLForm-Element类型。它继承自HTMLElement，因而与其他HTML元素具有相同的默认属性，而且它也有自己独特的属性和方法：

- acceptCharset：服务器能够处理的字符集。
- action：接受请求的URL。
- elements：表单中所有控件的集合。
- enctype：请求编码类型。
- length：表单中控件的数量。
- method：要发送的HTTP请求类型。
- name：表单的名称。
- reset()：将所有表单域重复为默认值。
- submit()：提交表单。
- target：用于发送请求和接受响应的窗口名称。

通过document.forms可以取得页面中所有表单。在这个集合中，可以通过数值索引或name值来取得特定的表单。

```js
var firstForm = document.form[0];
var myForm = document.forms["form2"];
```

### 提交表单

用户单击提交按钮或图像按钮时，就会提交表单。

图像按钮时通过将\<input\>的ytpe特性值设置为“image”来定义的。

```js
//图像按钮
<input type="image" src="img/a.gif" onclick="alert('click')">
```

下面代码会阻止表单提交：

```js
var form = document.getElementById("form");
EventUtil.addHandler(form, "submit", function (event){
    
    event = EventUtil.getEvent(event);
    
    EventUtil.preventDefault(event);
});
```

在JavaScript中，以编程方式也可以调用submit()提交表单，这种方法无需表单包含提交按钮：

```js
var form = document.getElementById("form");

//提交表单
form.submit();
```

在以调用submit()方法的形式提交表单时，不会触发submit事件，所以要记得在调用此方法之前先验证表单数据。

提交表单时可能出现的最大问题，就是用户重复提交表单（在用户第一次提交然后没反应的情况下，不耐烦的用户就会反复单击提交按钮，如果是下订单，那么可能会多订好多份）。

解决这一问题的办法有两个：

- 在第一次提交表单后就禁用提交按钮
- 利用onsubmit事件处理程序取消后续的表单提交操作

### 重置表单

重置按钮：

```html
<input type="reset" value="reset">

<button type="reset">
    reset
</button>
```

在重置表单时，所有字段都恢复初始值。

用户单击重置按钮时，会触发reset事件。利用这个机会，我们可以在必要时取消重置操作。

```js
var form = document.getElementById("form");
EventUtil.addHandler(form, "reset", function (event) {
    //取得事件对象
    event = EventUtil.getEvent(event);
    
    //阻止表单重置
    EventUtil.preventDefault(event);
});

//也可以通过JavaScript重置表单
form.reset();

//与调用submit()方法不同，reset()方法会触发reset事件。

//事实上，充值表单的需求是很少见的。更常见的做法是提供一个取消按钮，让用户能够回到前一个页面，而不是不分青红皂白的重置表单中的所有值。
```

### 表单字段

每个表单都有elements属性（是一个有序列表），该属性时表单中所有元素的集合（如\<input>  \<button>等）。  可以按照位置和name特性来访问每个表单字段：

```js
var form = document.getElementById("form");

//取得表单中的第一个字段
var file1 = form.elements[0];

//取得名为  textbox1  的字段
var filed2 = form.elements["textbox1"];

//取得表单中包含的字段数量
var filed3 = form.elements.length;

```

如果有多个表单控件都在使用一个name（如单选按钮），那么就会返回以该name命名的一个NodeList。如下所示：

```html
<form method="post" id="myForm">
    <ul>
        <li><input type="radio" name="color" value="red">Red</li>
        <li><input type="radio" name="color" value="green">Green</li>
        <li><input type="radio" name="color" value="yellow">Yellow</li>
    </ul>
</form>

<script>
	var form = document.getElementById("myForm");
    
    var colorFileds = form.elements["color"];
    alert(colorFileds.length);
    var firstColorFiled = colorFileds[0];
    var firstFormFiled = form.elements[0];
    alert(firstColorFiled ==== firstFormFiled);//true;
</script>
```

#### 1.共有的表单字段

除了\<filedset>（用来将表单元素进行分组）元素之外，所有表单字段都拥有相同的一组属性。

表单字段共有的属性如下：

- disabled：布尔值，表示是否禁用。
- form：指向当前字段所属表单的指针。
- name：当前字段的名称。
- readOnly：布尔值，表示当前字段是否只读。
- tabIndex：规定元素的 tab 键控制次序（当 tab 键用于导航时）。
- type：当前字段的类型，如“checkbox”， “radio” 等等。
- value：当前字段将被提交给服务器的值。

处理form属性外，可以通过JavaScript动态修改其他任何属性。

```js
var form = document.getElementById("myForm");
var field = form.elements[0];

field.value = "Another value";

alert(field.form === form); //true

field.focus();

field.disabled = true;

filed.type = "checkbox";
```

解决用户多次单击提交按钮的问题，就是在第一次单击后就禁用提交按钮。只要侦听submit事件并禁用提交按钮即可：

```js
EventUtil.addHaneler(form, "submit", function (event) {
    event = EventUtil.addHandler(event);
    var target = EventUtil.getTarget(event);
    
    var btn = target.elements["submit-btn"];
    
    btn.disabled = true;
});

//注意，不能通过onclick事件处理程序来实现这个功能，原因是不同浏览器之间存在“时差”：有的浏览器submit在click之前触发，有的则相反。  因此最好用submit，不过这种方式不适合表单中没有提交按钮的情况。
```

单选列表的type属性值为  "select-one"；

多选列表的type属性值为  "select-multiple"；

其他的与其type值或标签名相同。

#### 2.共有的表单字段方法

每个表单字段都有两个方法：

- focus()：聚焦
- blur()：移焦

```js
EventUtil.addHandler(window, "load", function (event) {
    document.forms[0].elements[0].focus();
});
```



HTML5新增autofocus属性。用于自动把焦点移动到相应字段。

```html
<input type="text" autofocus>
```

为了保证前面代码正常运行，先检测是否设置了这个属性，如果设置了就不调用focus了：

```js
EventUtil.addHandler(window, "load", function (event) {
    var element = document.forms[0].elements[0];
    if(element.autofocus !== true){
        element.focus();
        console.log("JS focus");
    }
});
```

IE不支持autofocus属性。

在默认情况下，只有表单字段可以获得焦点。对于其他元素而言，如果该先将其tabIndex属性设置为-1，然后再调用focus()方法，也可以让这些元素获得焦点。只有Opera不支持这种技术。

blur()方法：它的作用是从元素中移走焦点。

#### 3.共有的表单字段事件

所有表单字段都支持下列3个事件：

- blur：当前字段失去焦点时触发。
- change：对于\<input>  \<textarea>元素，在失去焦点而且value值改变时触发；对于\<select>元素，在其选项改变时触发。
- focus：当前字段获得焦点时触发。

如下代码编写了一个使用上述事件的表单：

```js
var textbox = document.getElementById("textbox");

EventUtil.addHandler(textbox, "focus", function (event) {
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    
    if(target.style.backgroundColor != "red"){//输入状态
        target.style.backgroundColor = "yellow";
    }
});

//等代码。。。
```

关于blur和change事件的发生顺序关系，并没有严格的规定，因浏览器而异，这一点需要特别注意喔。

## 文本框脚本

在HTML中有两种文本框：

- \<input>
- \<textarea>

```html
<input type="text" size="25" maxlength="50" value="initial value">
此文本框能够显示25个字符，但输入不能超过50个字符，初始值 为initial value

<textarea rows="25" cols="5">initial value</textarea>
文本框字符行数为25，字符列数为5.
```

这两种方法都会将用户输入的内容保存在value属性中。可以这样读取和设置文本框的值：

```js
var textbox = document.forms[0].elements["text1"];
alert(textbox.value);

textbox.value = "Some new value";
//建议像这样使用value属性读取或设置文本框的值，不建议使用标准的DOM方法（如setAttribute()），原因是对value属性所做的修改，不一定会反映在DOM中。
//因此在处理文本框的值时，最好不要使用DOM方法。
```

### 选择文本

上述两种文本框都支持：

- select()方法：用于选择文本框中所有文本。

```js
var textbox = doucument.forms[0].elements["textbox1"];

textbox.select();
```

在文本框获得焦点时选择所有文本的程序：

```js
EventUtil.addHandler(textbox, "focus", function (event) {
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    
    target.select();
});
```

#### 1.选择（select）事件

- select事件：在选择了文本框中的文本时触发。在调用select()方法时也触发。

```js
var textbox = doucument.forms[0].elements["textbox1"];

EventUtil.addHandler(textbox, "select", function (event) {
    alert("Text selected" + textbox.value);
});
```

#### 2.取得选择的文本

HTML5规范添加了两个属性：

- selectionStart
- selectionEnd

默认值为0，表示所选择文本选区的开头和结尾的偏移量。

要取得用户在文本框中选择的文本，可以这样：

```js
function getSelectedText(textbox){
    return textbox.value.substring(textbox.selectionStart,textbox.selectionEnd);//截取字符串
};
```

但无奈的IE8-版本不支持这两个属性，它有一个document.selection对象，保存着用户在整个文档范围内选择的文本信息；也就是说，无法确定用户选择的是页面中哪个部位的文本。

要取得选择的文本，必须创建一个范围，然后在将文本提取出来：

```js
function getSelectedText(textbox){
    if(typeof textbox.selectionStart == "number"){
        return textbox.value.subString(textbox.selectionStart, textbox.selectionEnd);
    }else if(document.selection){
        return document.selection.createRange().text;//调用document.selection时，不需要考虑textbox参数。
    }
};
```

#### 3.选择部分文本

HTML5提供方案：

- setSelectionRange():所有文本框都有此方法，用来选择部分文本。接受2个参数（要选择第一个字符索引，要选择的最后一个字符之后的索引）。

IE8-版本是使用范围选择部分文本的。

跨浏览器的方法即：

```js
textbox.value = "Hello world!";

function selectText(textbox, startIndex, stopIndex){
    if(textbox.setSelectionRange){
        textbox.setSelectionRange(startIndex, stopIndex);
    } else if(textbox.crateTextRange){
        var range = textbox.createTextRange();
        range.collapse(true);//将范围折叠到范围的起点
        range.moveStart("character", startIndex);//范围起始位置，character表示逐个字符的移动。
        range.moveEnd("character", stopIndex - startIndex);//范围字符数
        range.select();//选择range范围中的所有文本
    }
    
    textbox.focus();//最后一定要聚焦一下，要不然用户看不到你所选择的文本在哪
};
```

### 过滤输入

#### 1.屏蔽字符

如写一个只允许用户输入数值的函数：

````js
EventUtil.addHandler(textbox, "keypress", function (event){//键盘按下字符按键事件
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    var charCode = EventUtil.getCharCode(event);//跨浏览器取得用户输入字符的Unicode编码
    
    if(!/\d/.test(String.fromCharCode(charCode)) && charCode > 9 && !event.ctrlKey){//将编码转换成字符与reg比较，避免屏蔽那些极为常用和必要的键（如删除键，退格键等）， 避免屏蔽Ctrl+v，Ctrl+c等快捷键
        EventUtil.preventDefault(event);//取消默认事件，也就是取消此次字符输入
    }
});
````

#### 2.操作剪贴板

6个剪贴板事件：

- beforecopy：在发生复制操作前触发。
- copy：在发生复制操作时触发。
- beforecut：在发生剪切操作时触发。
- cut：在发生剪贴操作时触发。
- beforepaste：在发生粘贴操作前触发。
- paste：在发生粘贴操作时触发。

通过clipboardDate对象访问剪贴板中的数据。在IE中它是window的对象，在其他浏览器中它是event对象的属性。

它有3个方法：

- getData():  从剪贴板中取得数据。
- setData()：向剪贴板中放入数据。
- clearData()：清除剪贴板中的数据。

由于各浏览器在此有差异，所以向EventUtil中再添加方法：

```js
var EventUtil = {
    //...
    getClipboardText: function (event) {//  从剪贴板中取得数据。
        var clipboardData = (event.clipboardData || window.clipboardData);//在IE中它是window的对象，在其他浏览器中它是event对象的属性。
        return clipboardData.getData("text");
    },
    setClipboardText: function (event, value) {//向剪贴板中放入数据。
        if(event.clipboardData){
            return event.clipboardData.setData("text/plain", value);
        } else if(window.clipboardData){
            return window.clipboardData.setData("text", value);
        }
    }
};
```

检测一个只接受文本框的粘贴值是否符合规则：

```js
EventUtil.addHandler(textbox, "paste", function (event) {
    event = EventUtil.getEvent(event);
    var text = EventUtil.getClipboardText(event);
    
    if(!/^\d*$/.text(text)){//!=非，^=行首（若在[]中即为 除了 的意思），*=0或多个（+=1或多个），$=行尾
        EventUtil.preventDefault(event);//取消默认事件（粘贴）
    }
});
```

由于并未所有浏览器都支持访问剪贴板，所以更简单的做法事屏蔽一或多个剪贴板操作。

### 自动切换焦点

为了方便用户输入，在用户填写完当前字段后，自动将焦点切换到下一个字段。

```html
<input type="text" name="tel1" id="text1" maxlength="3">
<input type="text" name="tel2" id="text2" maxlength="3">
<input type="text" name="tel3" id="text3" maxlength="4">

<script>
(function () {
    function tabForward(event) {
        event = EventUtil.getEvent(event);
        var target = EventUtil.getTarget(event);
        
        if(target.value.length == target.maxLength){//判断用户输入是否等于最大数量
            var form = target.form;//寻找用户输入时字段所在的表单
            
            for(var i=0, len=form.elements.length; i<len; i++){
                if(form.elements[i] == target){
                    if(form.elements[i+1]){//判断是否存在下一个字段
                        form.elements[i+1].focus();//聚焦
                    }
                    return;
                }
            }
        }
    }
    
    var text1 = document.getElementById("text1");
    var text2 = document.getElementById("text2");
    var text3 = document.getElementById("text3");
    
    EventUtil.addHandler(text1, "keyup", tabForward);
    EventUtil.addHandler(text1, "keyup", tabForward);
    EventUtil.addHandler(text1, "keyup", tabForward);
})();
</script>
```

请记住，这些代码只适用于前面给出的标记，而且没考虑隐藏字段。

### HTML5约束验API

HTML5新增表单验证功能。如果由于某种原因页面未能出现js，此时可以用HTML5的功能进行表单验证。

#### 1.必填字段

```html
<input type="text" name="username" required>
<p>
    required属性标注的字段，在提交的时候都不能空着
</p>

<script>
	//检查某个表单字段是否为必填字段
    var isUsernameReauired = document.forms[0].elements["username"].required;
    
    //检查浏览器是否支持required属性
    var isRequiredSupported = "require" in document.createElement("input");
</script>
```

#### 2.其他输入类型

```html
<input type="email" name="email" >
<input type="url" name="homepage" >
<p>
    第一个只能输入email格式的字段
    第二个只能书输入url格式的字段
</p>

<script>
	//检测浏览器是否支持这些属性
    var input = document.createElement("input");
    input.type = "email";
    
    var isEmailSupported = (input.type == "email");
</script>
```

#### 3.数值范围

```html
<input type="number" min="0" max="100" step="5" name="count" >
<p>
    想让用户输入0~100中5的倍数的值。
</p>
<p>
    html5还有另外几个输入元素：number,  range, datetime, datetime-local, date, month, week, time等。	但浏览器对这几个类型支持情况并不好，因此慎用。
</p>
```

#### 4.输入模式

```html
<input type="text" pattern="\d+" name="count" >
<p>
    pattern的属性值是一个RegExp，用于匹配用户输入字段。
    注意，模式的开头和结尾不用加^ 和 $号，假设已经有了。
    在使用前还是要检测浏览器的支持情况。
</p>
```

#### 5.检测有效性

checkValidity()方法：检测表单中的某个字段是否有效。如果有效返回true，反之false。

必填字段无值无效，与pattern属性不匹配也无效。

```js
if(document.forms[0].elements[0].checkValidity()){
    //字段有效，继续
}else {
    //字段无效
}

if(document.forms[0].checkValidity()){//此时即使有一个字段是无效的，就会返回false
    //表单有效
} else {
    //表单无效
}

```

validity属性会告诉你为什么字段有效或无效，这个对象包含一系列属性，每个属性返回一个布尔值：

- customError
- patternMismatch
- rangeOverflow
- stepMisMatch
- tooLong
- typeMismatch
- valid
- vallueMissing

可以通过以上属性来得到更具体的信息。

#### 6.禁止验证

通过设置novalidate属性，可以告诉表单不进行验证。

```html
<form method="post" action="sibnup.php" novalidate>
    <input type="text">
</form>

<script>
	//检测或设置novalidate属性值
   document.forms[0].noValidate = true;//
</script>

<p>
    如果表单中有多个提交按钮，为了指定点击某个提交按钮不必验证表单，可以在相应的按钮上添加formnovalidate属性
</p>
<form method="post" action="foo.php">
    <input type="submit" value="Tegular Submit">
    
    <input type="submit" formnovalidate name="btnNoValidate" value="Non-validating Submit" >
</form>

<script>
	//设置formNovalidate值，禁止验证
    document.forms[0].elements[1].formNoValidate = true;
</script>
```

## 选择框脚本

选择框通过以下两种元素创建：

- \<select>：创建单选或多选菜单
- \<option>：select中的选项。

除了表单字段共有的方法，HTMLSelectElement类型还提供了下列属性方法：

- add(newOption, relOption)：向控件中新插入\<option>元素。
- multiple：布尔值，表示是否允许多项选择。
- options：控件中所有 option 元素的HTMLCollection。
- remove(index)：移除给定位置的选项。
- selectedIndex：基于0的选项索引，若无选项，则为-1。
- size：选择框中可见的行数

type属性不是 “select-one” ，就是“select-multiple” 取决于HTML代码中是否有multiple特性。

value属性由当前选项决定：

- 没有选中的项：value=""。
- 选中一项，并且有value值：value=value（html中的）。
- 选中一项，并未定义value值：value=HTML文本。
- 选中多项：依据前两条规则取得第一个选项中的值。

在DOM中，每个\<option>元素都由一个HTMLOptionElement对象表示。为了便于访问数据，HTMLOptionElement对象添加了下列属性：

- index：当前选项在option集合中的索引。
- label：当前选项的标签。
- selected：布尔值，表示是否被选中。
- text：选项的文本。
- value：选项的值。

```js
var selectbox = document.forms[0].elements["location"];

//不推荐，效率较低
var text = selectbox.options[0].firstChild.nodeValue;
var value = selectbox.options[0].getAttribute("value");

//推荐，效率高
var text = selectbox.options[0].text;
var value = selectbox.options[0].value;
//所有浏览器都支持这些属性。
```

最后提醒一点：选择框的change事件与其他的change事件有些许不同：

- 其他表单字段的change事件触发的条件是值被修改且焦点离开。
- 而选择框字段的change事件触发的条件是只要选中的选项就会触发。

### 选择选项

对于单选：

- selectedIndex属性：返回选择框中选中项索引。

  - ```js
     var selectedOption = selectbox.options[selectbox.selectedIndex];
    //option方法返回索引所指的元素。
     alert("Selected index:" + selectedIndex + "\nSelected text:" + selectedOption.text + "\nSelected value:" +selectedOption.value);
    ```



























# 第二十章 JSON

JSON——JavaScriptObjectNotation（JavaScript对象标记/JavaScript对象表示法）。

关于JSON，最重要的是要理解它是一种数据格式，而不是一种编程语言。虽然具有相同的语法形式，但JSON并不从属于JavaScript。而且，其他语言也会用到JSON。

即使XML也可以实现同样的效果，但是JSON要简单的多。

## 语法

JSON的语法可以表示以下三种类型的值：

- 简单值：str，number，Boolean，null等，但不支持undefined。

  - JSON中的字符串必须用双引号“”，不可以用‘’。

- 对象：对象表示的是一组无序的键值对儿。

  - JavaScript中的对象：

    - ```js
      var object = {
          //name : "Nicholas",
          "name" : "Nicholas",//两种写法都可以，但是便于JSON，建议都加双引号
          //age : 29
          "age" : 29
      };
      ```

    - 

  - JSON 中的对象：

    - ```json
      {
          "name": "Nicholas",
          "age" : 29
      }
      ```

  - 与JavaScript相比，JSON没有声明变量（JSON中没有变量的概念）。其次，JSON没有加末尾分号。

  - 很重要的是：对象的属性在任何时候都必须加双引号，这在JSON中是必须的。

  - JSON中嵌入对象：

    - ```json
      {
          "name": "Nicholas",
          "age": 29,
          "school": {
              "name": "Merrimack College",
              "locatin": "North Andover, MA"
          }
      }
      ```

    - 同一个对象中绝对不能有两个同名的属性出现。

- 数组：表示一组一组有序的值的列表，可以通过索引访问其值。

  -  JavaScript中的数组：

    - ```js
      var values = [25, "hi", true];
      ```

  - JSON中的数组：

    - ```json
      [25, "hi", true]
      ```

    - 同样要注意，没有声明变量以及没有分号。

  - 把数组和对象结合起来，可以构成更复杂的数据集合：

    - ```json
      [
          {
              "title": "Professional JavaScript",
              "author": [
                  "Nicholas C. Zakas"
              ],
              "edition": 3,
              "year": 2011
          },
          {
              "title": "Professional JavaScript",
              "author": [
                  "Nicholas C. Zakas"
              ],
              "edition": 2,
              "year": 2009
          },
          {
              "title": "Professional Ajax",
              "author": [
                  "Nicholas C. Zakas",
                  "Jeremy McPeak",
                  "Joe Fawcett"
              ],
              "edition": 2,
              "year": 2--8
          },
          ...
      ]
      ```

    - 对象和数组通常是JSON数据结构的最外层形式。

JSON不支持变量、函数或的对象实例，它就是一种表示结构化数据的格式，并不局限于JavaScript的范畴。

## 解析与序列化

可以把JSON数据结构解析为有用的JavaScript对象。

如将上例解析为JavaScript对象后（假设把JSON的数据保存到了变量books中），只需一行代码就可以取得第三本书的名字：

```json
books[2].title
```

而用DOM方法实现这一功能很麻烦：

```js
doc.getElementsByTagName("books")[2].getAttribute("title")
```

所以JSON非常受欢迎，成了web服务开发中交换数据的事实标准。

### JSON对象

早期JSON解析器基本上就是使用JavaScript的eval()函数（它就像是一个完整的ECMAScript解析器，它只接收一个参数，即要执行的js代码的字符串形式。）来实现。

后来ECMAScript 5对解析JSON的行为进行规范，定义了全局对象JSON。

JSON对象有两个方法：

- stringify()：把JavaScript对象序列化为JSON字符串。

  - ```js
    //JavaScript --> JSON 
    var book = {
        title: "Professional JavaScript",
        authors: [
            "Nicholas C. Zakas"
        ],
        edition: 3,
        year: 2011
    };
    
    var jsonText = JSON.stringify(book);
    
    //序列化的JSON字符串中不包含任何空格字符或缩进
    
    alert(jsonText);
    "{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":3,"year":2011}"
    
    //而且所有函数及原型成员都会有意的被忽略，不体现在结果中。此外，值为undefined的任何属性也会被跳过。结果中都是只为有效JSON数据类型的实例属性。
    
    var book = {
        title: "Professional JavaScript",
        authors: [
            "Nicholas C. Zakas"
        ],
        edition: 3,
        year: 2011,
        function: function () {},
        und: undefined
    };
    undefined
    var jsonText = JSON.stringify(book);
    undefined
    jsonText
    "{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":3,"year":2011}"
    ```

- parse()：把JSON字符串解析为原生的JavaScript值。
  - ```js
    //JSON --> JavaScript
    var bookCopy = JSON.parse(jsonText);
    
    //注意，虽然book与bookCopy具有相同属性，但它们两个是独立的、没有任何关系的对象。
    ```

### 序列化选项

实际上JSON.stringify()方法还可以接受另外两个参数：

- 一个过滤器：可以使数组，也可以是函数；

  - 如果过滤器是数组，那么JSON.stringify()的结果将只包含数组中列出的属性：

    - ```js
      var book = {
          "title": "Professional JavaScript",
          "author": [
              "Nicholas C. Zakas"
          ],
          edition: 3,
          year: 2011
      };
      
      var jsonText = JSON.stringify(book,["title", "author"]);
      
      console.log(book);
      VM303:12 {title: "Professional JavaScript", author: Array(1), edition: 3, year: 2011}
      undefined
      
      console.log(jsonText);
      VM342:1 {"title":"Professional JavaScript","author":["Nicholas C. Zakas"]}
      undefined
      ```

  - 如通过过滤器是函数，传入的函数接受两个参数，属性名和属性值。

    - ```js
      //为了改变序列化对象的结果，函数返回的值就是相应键的值。不过要注意，如果函数返回undefined，那么相应的属性会被忽略
      
      var book = {
          "title": "Professional JavaScript",
          "author": [
              "Nicholas C. Zakas"
          ],
          edition: 3,
          year: 2011
      };
      
      var jsonText = JSON.stringify(book, function (key, value) {
          switch (key){
              case "author":
                  return value.join(",");
              case "year":
                  return 5000;
              case "edition":
                  return undefined;
              default:
                  return value;
              //最后一定要加default，此时返回传入的值，以便其他值都能正常出现在结果中。
          }
      });
      undefined
      
      jsonText
      "{"title":"Professional JavaScript","author":"Nicholas C. Zakas","year":5000}"
      
      
      ```

    - 

- 一个选项：用于控制结果中的缩进和空白符。

  - 如果这个参数是数值，则表示的是每个级别缩进的空格数。例如要在每个级别缩进4个空格，可以这样写：

    - ```js
      var book = {
          "title": "Professional JavaScript",
          "author": [
              "Nicholas C. Zakas"
          ],
          edition: 3,
          year: 2011
      };
      undefined
      
      var jsonText = JSON.stringify(book, null, 4);
      undefined
      
      jsonText
      "{
          "title": "Professional JavaScript",
          "author": [
              "Nicholas C. Zakas"
          ],
          "edition": 3,
          "year": 2011
      }"
      ```

    - 其实只要是传入有效的控制缩进的参数值，结果字符串就会包含换行符。最大空格缩进为10，更大的数值也会自动转换为10。

  - 如果这个参数是一个字符串而非数值，则这个字符串将在JSON字符串中被用作缩进字符（不再使用空格）：

    - ```js
      var book = {
          "title": "Professional JavaScript",
          "author": [
              "Nicholas C. Zakas"
          ],
          edition: 3,
          year: 2011
      };
      undefined
      
      var jsonText = JSON.stringify(book, null, "--");
      undefined
      
      jsonText
      "{
      --"title": "Professional JavaScript",
      --"author": [
      ----"Nicholas C. Zakas"
      --],
      --"edition": 3,
      --"year": 2011
      }"
      
      //字符串最大长度也是不能超过10，超过自动转换为10。
      ```

toJSON()方法：返回其自身的JSON数据格式。

可以为任意对象添加toJSON()方法：

```js
var book = {
    "title": "Professional JavaScript",
    "author": [
        "Nicholas C. Zakas"
    ],
    edition: 3,
    year: 2011,
    toJSON: function () {
        return this.title;
        //仅返回title属性值
    }
};
undefined

var jsonText = JSON.stringify(book);
undefined

jsonText
""Professional JavaScript""
```

与Date对象类似，这个对象也将被序列化为一个简单的字符串而非对象。可以让toJSON()返回任何值，它都能正常工作。

toJSON()可以作为函数过滤器的补充，因此**理解序列化的内部顺序非常重要**。

假设把一个对象插入JSON.stringigy()，序列化该对象的顺序如下：

1. 如果存在toJSON()方法而且能通过它取得有效的值，则调用该方法。否则，返回对象本身。
2. 如果提供了第二参数，应用这个函数过滤器。传入函数过滤器的值是第1步返回的值。
3. 对第2步返回的每个值进行相应的序列化。
4. 如果提供了第三个参数，执行相应的格式化。

### 解析选项

JSON.parse()方法也可以接受另一个参数，该参数是一个名叫 还原函数 的函数，将在每个键值对上调用。接收两个参数，一个键和一个值。返回一个值。

如果还原函数返回undefined，则表示要从结果中删除相应的键；

如果返回其他的值，则将该值插入到结果中。

在将日期字符串转换为Date对象是，经常要用到还原函数：

```js
var book = {
    "title": "Professional JavaScript",
    "author": [
        "Nicholas C. Zakas"
    ],
    edition: 3,
    year: 2011,
    releaseDate: new Date(2019, 5, 1)
};
undefined

var jsonText = JSON.stringify(book);
undefined

jsonText
"{"title":"Professional JavaScript","author":["Nicholas C. Zakas"],"edition":3,"year":2011,"releaseDate":"2019-05-31T16:00:00.000Z"}"

var copyBook = JSON.parse(jsonText, function (key,value) {
    if( key == "releaseDate"){
        return new Date(value);
    } else if (key == "title"){
        return undefined;
    } else {
        return value;
    }
});
undefined

copyBook
{author: Array(1), edition: 3, year: 2011, releaseDate: Sat Jun 01 2019 00:00:00 GMT+0800 (中国标准时间)}

```

# 第二十一章 Ajax与Comet

Ajax技术能够向服务器请求额外的数据而无须卸载页面，会带来更好的用户体验。

Ajax技术的核心是XMLHttpResquest对象（简称XHR）。

XHR为向服务器发送请求和解析服务器响应提供了流畅的接口。

虽然Ajax名字中包含XML的成分，但Ajax通信与数据格式无关。

## XMLHTTPRequest对象

在IE中可能会遇到3种不同版本的XHR对象，即MSXML2.XMLHttp 、MSXML2.XMLHttp.3.0、MSXML2.XMLHttpp.6.0。

要使用MSXML库中的对象，需要编写一个函数。

而IE7+、Firefox、Opera、Chrome和Safari都支持原生的XHR对象，在这些浏览器中创建XHR对象像下面这样直接使用XMLHTTPRequest构造函数即可：

```js
var xhr = new XMLHttpRequest();
```

所以，如果想做所有浏览器的兼容，可以像下面这样写一个函数：

```js
function createXHR() {
    if(typeof XMLHttpRequest != "undefined"){//绝大多数浏览器
        return new XMLHttpRequest();
    } else if (typeof ActiveXObject != "undefined"){//IE7-版本
        if(typeof arguments.callee.activeXString != "string"){
            var versions = ["MSXML2.XMLLHttp", "MSXML2.XMLLHttp.3.0", "MSXML2.XMLLHttp.6.0"],
                i,
                len;
            for(i=0,len=versions.length; i<len; i++){
                try{//这里只有一个版本能过去，其实个人觉得这里也可以直接if判断，emmm
                    new ActiveXObject(versions[i]);
                    arguments.callee.activeXString = versions[i];
                    break;
                }catch(ex){
                    //...
                };
            }
        }
        
        return new ActiveXObject(arguments.callee.activeXString);
    } else {
        throw new Error("No XHR object available.");
    }
};

//可以使用以下代码在所有浏览器中创建XHR对象了：
var xhr = createXHR();
```

### XHR的用法

在使用XHR对象是，要调用的第一个方法是：

- open()：它接收3个参数（要发送的请求类型-get、post等字符串形式，请求的URL和表示是否异步发送请求的布尔值）。

  - ```js
    var xhr = createXHR();
    
    xhr.open("get", "example.php", false);
    //这行代码会启动一个针对example.php的GET请求。
    //有关这行代码，需要说明两点：
    	//一是URL相对于执行代码的当前页面（也可以使用绝对路径）
    	//二是调用open()方法并不会真正发送请求，而只是启动一个请求以备发送。
    //只能向同一个域中使用相同的端口和协议的URL发送请求。
    ```

要发送特定的请求，必须调用send()方法：

- send():

  - ```js
    xhr.open("get", "example.php", false);
    xhr.send(null);
    //这里的send()方法接收一个参数，即要作为请求主体发送的数据。如果不需要通过请求主体发送数据，则必须传入null，因为这个参数对有些浏览器来说是必须的。调用send()方法之后，请求就会被分派到服务器。
    ```

这次请求是同步的，JavaScript代码会等到服务器响应之后再继续执行。在收到响应后，响应的数据会自动填充XHR对象的属性，相关的属性简介如下：

- responseText：作为响应主体被返回的文本。
- responseXML：如果响应的类型是xml，这个属性中将保存着包含响应数据XML DOM文档。
- status：相应的HTTP状态。
- statusText：HTTP状态的说明。

在接受到响应后，第一步是检查status属性，已确定响应已经成功返回。

一般来说，可以将HTTP状态代码为200作为成功的标志。

此时，responseText属性的内容已经就绪，而且在内容类型正确的情况下，responseXML也应该能够访问了。

此外，状态码为304表示请求的资源并没有被修改，可以直接使用浏览器中缓存的版本；当然，也意味着响应是有效的。

为确保接收到适当的响应，应该像下面这样检查上述这两种状态代码：

```js
xhr.open("post", "example.php", false);
xhr.send(null);

if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304)){
    alert(xhr.responseText);
} else {
    alert("Request was unsuccessful:" + xhr.status);
}
```



多数情况下，我们还是要发送异步请求，才能让javascript继续执行而不必等待响应。

此时，可以检测XHR对象的一个属性：

- readyState属性，该属性表示请求/响应过程的当前活动阶段。

这个属性可取的值如下：

- 0：未初始化。尚未调用open()方法。
- 1：启动。已调用open()方法，但尚未调用send()方法。
- 2：发送。已调用send()方法，但尚未接收到响应。
- 3：接收。已经接收到部分响应数据。
- 4：完成。已经接收到全部响应数据，而且可以使用了。

只要readyState属性的值由一个值变为另一个值，都会触发一次：

- readystatechange事件。

可以利用这个事件来检测每次状态变化后的readyState的值。

通常，我们只对readyState值为4的阶段感兴趣，因为这个时候所有的数据都已经就绪。

不过，必须在调用open()方法之前指定onreadystatechange事件处理程序才能确保跨浏览器兼容性。

如下所示：

```js
var xhr = createXHR();

xhr.onreadystatechange = function () {
    if(xhr.readystate == 4){
        if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            alert(xhr.responseText);
        } else {
            alert("Reauest was unsuccessful:" + xhr.status);
        }
    }
};
//这个例子使用了xhr对象而非this对象，原因是onreadyStatechange事件处理程序的作用域问题。如果使用this对象，在有的浏览器中会导致函数执行失败，或者导致错误发生。因此，使用实际的XHR对象实例变量是较为可靠的一种方式。

xhr.open("post", "example.php", true);
xhr.send(null);
```

另外，在接收到响应之前还可以调用：

- abort()：方法来取消异步请求

  - ```js
    xhr.abort();
    ```

### HTTP头部信息

每个HTTP请求和响应都会带有相应的头部信息。XHR对象也提供了操作这两种头部信息（即请求头和响应头）的方法：

默认情况下，在发送XHR请求的同时，还会发送下列头部信息：

- Accept：浏览器能够处理的内容类型。
- Accept-Charset
- Accept-Encoding
- Accept-Language
- Connection
- Cookie
- Host
- Referer
- User-Agent

虽然不同浏览器发送的信息头会有所不同，但以上的信息基本上所有浏览器都会发送。

- setRequestHeader()方法：可以设置自定义的请求头部信息。这个方法接收2个参数（头部字段的名称，头部字段的值）。    要成功发送请求头部信息，必须在open()方法之后，send()方法之前调用setRequestHeader()方法：

  - ```js
    var xhr = createXHR();
    xhr.onreadystatechange = function () {
        if(xhr.readyState == 4){
            if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
                alert(xhr.responseText);//响应主体
            } else {
                alert("Reauest was unsuccessful:" + xhr.status);
            }
        }
    };
    
    xhr.open("get", "example.php", true);
    xhr.setRequestHeader("MyHeader", "MyVaule");
    xhr.send(null);
    ```

建议使用自定义头部字段名称，不要使用浏览器正常发送的字段名称，否则有可能会影响服务器的响应。

有的浏览器支持开发人员重写默认头部信息，但有的浏览器不支持。

- getRequestHeader()方法：传入头部字段名称，返回字段信息。

- getAllRequestHeader()方法：可以取得一个包含所有头部信息的长字符串。

  - ```js
    var myHeader = xhr.getRequestHeader("MyHeader");
    var allHeaders = xhr.getAllRequestHeader();
    ```

在服务器端，也可以利用头部信息向浏览器发送额外的、结构化的数据。

### GET请求

GET是最常见的请求类型，最常用于向服务器查询某些信息。

必要时，可以将查询字符串参数追加到URL的末尾，以便将信息发送个服务器。

对XHR而言，这个字符串必须经过正确的编码。

使用GET请求经常会发生的一个错误就是查询字符串的格式有问题。

查询字符串中每个参数的名称和值都必须使用encodeURIComponent()进行编码，然后才能放到URLDL末尾；

而且所有名-值对儿都必须由和号（&）分隔：

```js
xhr.open("get", "example.php?name1=value1&name2=value2", true);

//写一个函数辅助向现有的URL末尾添加查询字符串：
function addURLParam(url, name, value) {
    url += (url.indexOf("?")!= -1 ? & : ?);//indexOf()方法用来查询数组元素在数组中的位置，返回位置，未找到则返回-1
    url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
    return url;
};

//举例
var url = "example.php";

url = addURLParam(url, "name", "Nicholas");
url = addURLParam(url, "book", "Professional Javascript");

xhr.open("get", url, true);
```

### POST请求

使用率仅次于GET，通常用于向服务器发送应该被保存的数据。

```js
//第一步
xhr.open("post", "example.php", true);

//第二步：向send()中传入某些数据，可以是任何想发送到服务器的字符串

//默认情况下，服务器对POST请求和web表单的请求不会一视同仁，但我们可以使用XHR来模仿表单提交：首先将Content头部信息设置为表单相同的内容类型，其次是以适当的格式创建一个字符串。

xhr.open("post", "example.php", true);
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");//模仿表单
var form = document.getElementById("form1");
xhr.send(serialize(form));//实现表单序列化，将form1表单中的数据序列化后发送给浏览器（a=1&b==2&c=3）
```

下面是PHP文件example.php就可以通过$_POST取得提交的数据了：

````php
<?php
    header("Content-Type: text/plain");
	echo <<<EOF
Name: {$_POST[ 'user-name' ]}
Email: {$_POST[ 'user-email' ]}
EOF;
?>
````

如果没有设置Content-Type头部信息，那么发送个服务器的数据就不会出现在$_POST超级全局变量中。



与GET相比，POST请求消耗的资源会更多一些。从性能角度看，以发送相同的数据计，GET请求的速度最多可达到POST请求的两倍。

## XMLHttpRequest2级

并非所有浏览器都完整的实现XMLHttpRequest2级规范，但所有浏览器都实现了它规定的部分内容。

### FormData

FormData为序列化表单以及创建与表单格式相同的数据（用于通过XHR传输）提供了便利。

下面创建了一个FormData对象，并向其中添加了一些数据：

```js
va data = new FormData();
data.append("name", "Nicholas");//append()接受连个参数（名，值），可以向这样添加任意多个键值对儿

//而通过向FormData构造函数中传入表单元素，也可以用表单元素的数据预先向其中填入键值对儿：
var xhr = createXHR();

xhr.open("post", "example.php", true);
var form = document.getElementById("form");
xhr.send(new FormData(form));
//使用FormData的方便之处体现在不必明确地在XHR对象上设置请求头部。XHR对象能够识别传入的数据类型是FormData的实例，并配置适当的头部信息。
```

### 超时设定

IE8为XHR对象添加了一个timeout属性，表示请求在等待响应多少毫秒后就终止。

在给timeout设置一个数值后，如果在规定的时间内浏览器还没有接收到响应，那么就会触发timeout事件，进而会调用ontimeout事件处理程序。

这项功能后来也被收入了XMLHTTPRequest2级规范中。

来看下面例子：

```js
var xhr = createXHR();
xhr.onreadyStateChange = function () {
    if(xhr.readyState == 4){
        try{//这里为什么用try-catch呢？看下面解析
            if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
                alert(xhr.responseText);
            } else {
                alert("Request was unsuccessful :" + xhr.status);
            }
        }catch(ex){
            //假设由ontimeout事件处理程序处理
        };
    }
};

xhr.open("post", "timeout.php", ture);
xhr.timeout = 1000;//将超时设置为1秒钟（仅适用于IE8+），意味着如果请求在1秒钟内还没有返回，就会自动终止。请求终止时，会调用ontimeout事件处理程序。但此时readyState可能已经改变为4了，这意味着调用onreadystatechange事件处理程序，可是如果在超时调用终止请求之后再访问status属性，就会导致错误。为了避免浏览器报错，就要用try-catch语句处理一下。
xhr.ontimeout = function () {
    alert("Request did not return inn a second.");
};
xhr.send(null);
```

### overrideMimeType()方法

Firefox最早引入了overrideMimeType()方法，用于重写XHR响应的MIME类型。

返回响应的MIME类型决定了XHR对象如何处理它。

```js
var xhr = createXHR();
xhr.open("post", "text.php", true);
xhr.overrideMimeType("text/xml");//这里强迫XHR对象将响应当作XML而非纯文本来处理。调用overrideMimeType()必须在send()方法之前，才能保证重写响应的MIME类型。
xhr.send(null);
```

IE不支持此方法。

## 进度事件

有以下6个进度事件：

- loadstart：在接收到响应数据的第一个字节时触发。
- progress：在接收响应期间持续触发。
- error：在请求发生错误时触发。
- abort： 在因为调用abort()方法而终止连接时触发。
- load：在接收到完整的响应数据时触发。
- loadend：在通信完成或者触发error、abort或load事件后触发。

每个请求都从触发loadstart事件开始，接下来是一个或多个progress事件，然后触发error、abort或load事件中的一个，最后以触发loadend事件结束。

Opera、IE8+只支持load事件，目前没有浏览器支持loadend事件。

有两个事件的细节需要注意：

### load事件

Firefox实现中引入了load事件，用于代替readystatechange事件。因为响应接受完毕后触发load事件，因此可以不必检测readyState属性。

```js
var xhr = createXHR();
xhr.onload = function () {//用onload事件代替readystatechange事件
    if((xhr.status >= 200 && xhr.status < 300) || xhr.starus == 304){
        alert(xhr.responseText);
    } else {
        alert("Response was unsuccessful : " + xhr.status);
    }
};
xhr.open("get", "altevents.php", true);
xhr.send(null);
```

只要浏览器接收到服务器的响应，不管其状态如何，都会触发load事件。

### progress事件

这个事件会在浏览器接收新数据期间周期性触发。

而onprocess事件处理程序会接收到一个event对象，其target属性是xhr对象，但包含着额外的3个属性：

- lengthComputable：表示进度信息是否可用的布尔值。
- position：表示已经接收的字节数。
- totalSize：表示根据Content-Length响应头部确定的预期字节数。

有了这些信息，我们就可以为用户创建一个进度指示器了：

```js
//进度指示器
var xhr = createXHR();
xhr.onload = function () {
    if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
        alert(xhr.responseText);
    } else {
        alert("...");
    }
};

xhr.onprocess = function (event) {//必须在open()之前添加onprogress事件处理程序
    var divStatus = document.getElementById("status");
    if(event.lengthComputable){
        divStatus.innerHTML = "Received" + event.position + " of " + event.totalSize + " bytes";
    }
};

xhr.open("get", "altevents.php", true);
xhr.send(null);
```

## 跨域资源共享-CORS

通过XHR实现Ajax通信的一个主要限制，就是XHR只能访问同域资源。

CORS（跨域资源共享）是W3C的一个工作草案，定义了在必须访问跨域资源时，浏览器与服务器应该如何沟通。

CORS背后的基本思想，就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。

### IE对CORS的实现

微软在IE8中引入了XDR类型。这个对象与XHR类似，也有4点不同，但能实现安全可靠的跨域通信。

```js
var xdr = new XDomainRequest();//创建实例

xdr.onload = function () {//所有的XDR请求都是异步执行的。请求返回之后，会触发load事件
    alert(xdr.responseText);//响应的数据也会保存在responseText属性中
};

xdr.onerror = function () {//响应失败就会触发error事件。但没有任何其他信息可用，只能确定error。鉴于导致XDR请求失败的因素很多，因此建议不要忘记onerror事件处理程序来捕获该事件；否则，即使请求失败也不会有任何提示。
    	alert("An error occurred.");
};

xdr.timeout = 1000;//XDR对象也支持timeout属性以及ontimeout事件处理程序
xdr.ontimeout = function () {
    alert("Request took too long");
};
xdr.open("get", "http://www.somewhere-else.com/page/");//xdr的open()只接受两个参数，是所有XDR请求都是异步的
xdr.send(null);

xdr.abort();//用于终止请求
```

为支持POST请求，XDR对象提供了contentType属性，用来表示发送数据的格式。

```js
var xdr = new XDomainRequest();

xdr.onload = function () {
    alert(xdr.responseText);
};

xdr.onerror = function () {
    alert("An error occurred.");
};

xdr.open("post", "http://www.somewhere-else.com/page/");
xdr.contentType = "application/x-www-form-urlencoded";//XDR模仿表单提交，这个属性时通过XDR对象影响头部信息的唯一方式。
xdr.send("name1=value1&name2=value2");
```

### 其他浏览器对CORS的实现

Firefox3.5+、Safari4+、Chrome、iOS版Safari和Android平台中的WebKit都通过XMLHttpRequest对象实现了对CORS的原生支持。

在尝试打开不同来源的资源时，无需额外编写代码。

要请求位于另一个域中的资源，使用标准的XHR对象并在open()方法中传入绝对的URL即可：

```js
var xhr = createXHR();
xhr.onreadystatechange = function () {
    if(xhr.readystate == 4){
        if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            alert(xhr.responseText);
        } else {
            alert("Reauest was unsuccessful:" + xhr.status);
        }
    }
};
xhr.open("get","http://www.somewhere-else.com/page/", true);
xhr.send(null);
//与IE不同，XHR对象可以访问status和statusText属性，而且还支持同步请求。
```

跨域XDR对象基于安全考虑也有一些限制：

- 不能使用setRequestHeader()设置自定义头部。
- 不能发送和接收cookie。
- 调用getAllRequestHeaders()方法总会返回空字符串。

由于同源请求和跨域请求都是用相同的接口，因此对于本地资源，最好使用相对URL，在访问远程资源时在使用绝对URL。这样做能消除一些问题。

### Preflighted Requests

一个透明服务器验证机制。

在使用下列高级选项来发送请求时，就会向服务器发送一个Preflight请求，这种请求使用OPTONS方法，发送下列头部：

```js
//举例说明
Origin:http://www.nczonline.net     //与简单的请求相同
Access-Control-Request-Method:POST  //请求自身使用的方法
Access-Control-Request-Headers:NCZ  //（可选）自定义头部，多个以逗号分隔。
```

发送这个请求之后，服务器可以决定是否允许这种类型的请求。

服务器通过在响应中发送如下头部与浏览器进行沟通。

```js
//举例说明
Access-Control-Allow-Oritin:http://www.nczonline.net 	//与简单的请求相同
Access-Control-Allow-Methods:POST,GET			//允许的方法，多个方法以逗号隔开
Access-Control-Allow-Headers:NCZ				//允许的头部，多个头部以逗号隔开
Access-Control-Max-Age:1728000					//应该将这个Preflight请求缓存多长时间
```

Preflight请求结束后，结果将按照响应中指定的时间缓存起来。而为此付出的代价只是第一次发送这种请求时会多一次HTTP请求。

Firefox、Safari、Chrome支持Preflight请求，IE10-版本不支持。

按照我的理解就是：由于跨域请求资源存在安全问题，浏览器不会将所有的跨域资源都无限制的添加到某网页上。所以，如果你需要跨域获得资源时，你需要向某服务器发送请求，而在请求发出之前，浏览器会先发一个OPTIONS请求到目标“域”的服务器上，这个提前发出的请求，就是Preflighted Requests。

这个跨域请求的流程为：

- 你有需求，打算向目标服务器发送请求
- 在你的请求发送之前，浏览器先给目标服务器发送一个用OPTIONS方法的Preflighted Requests
- 浏览器收到服务器的响应Preflighted Response
- 浏览器会根据Preflighted Requests中的上述几个信息来判断你要发送的Requests是否符合要求
- 如果Requests符合要求，request被发出，网页中可以正常的收到response；如果Requests不符合要求，那么这个request干脆就不会被发出。

### 带凭据的请求

默认情况下，跨源请求不提供凭据（cookie、HTTP认证过及客户端SSL证明等）。

通过将withCredentials属性设置为true，可以指定某个请求应该发送凭据.

如果服务器接受带凭据的请求,会用下面的HTTP头部来响应:

​	Access-Control-Allow-Credentials: true

如果发送的是带凭据的请求,但服务器的响应中没有包含这个头部,那么浏览器就不会把响应交给JavaScript.

支持withCredentials属性的浏览器有Firefox   Safari Chrome  ,IE10-版本不支持。

### 跨浏览器的CORS

即使浏览器对CORS的支持程度并不都一样，但所有浏览器都支持简单的请求，因此有必要实现一个跨浏览器的方案。

检测XHR是否支持CORS的最简单方式，就是检查是否存在withCredentials属性。

再结合检测XDomainRequest对象（IE中的XDR）是否存在，就可以兼顾所有浏览器了。

```js
function createCORSRequest (method, url,){
    var xhr = new XMLHttpRequest();
    if("withCredentials" in xhr){//此处in的用法是判断对象中（后）是否存在某属性（前）
        xhr.open(method,url,true);
    } else if (typeof XDomainRequest != "undefined"){
        xhr = new XDomainRequest();
        xhr.open(method, url);
    } else {
        xhr = null;
    }
    
    return xhr;
};

var request = createCORSRequest("get", "http://www.somewhere.com/page/");
if(request){
    request.onload = function () {
        //对request.responseText进行处理
    };
    request.send(null);
}
```

XDR与XMR这两个对象有共同的属性/方法如下：

- abort()
- onerror
- onload
- responseText
- send()

以上成员都包含在createCORSRequest()函数的返回对象中，在所有浏览器都能正常使用。

## 其他跨域技术

### 图像Ping

动态创建图像经常用于图像Ping。

图像Ping是与服务器进行简单的、单向的跨域通信的一种方式。

这种方式浏览器得不到任何数据，只能通过load和error事件知道响应是什么时候接收到的。

```js
var img = new Image();

img.onload = img.onerror = function () {
    alert("Done!");
};

img.src = "http://www.example.com/test?name=Nicholas";
//请求从设置src属性那一刻开始。
```

图像Ping最常用于跟踪用户点击页面或动态广告曝光次数。

图像Ping的两个缺点：只能发送get请求；无法访问服务器的响应文本。

因此，图像Ping只能用于浏览器与服务器之间的单项通信。

### JSONP

JSONP看起来与JSON差不多，只不过是被包含在函数调用中的JSON：

​	callback({"name": "Nicholas"});

JSONP由两部分组成：回调函数和数据。

下面是一个典型的JSONP的请求：

​	http://freegeoip.net/json/?callback=handleResponse

​	这里指定的回调函数的名字叫handleResponse()。

举例：通过查询地理定位服务来显示你的ip地址和位置信息：

```js
function handleRessponse(response){
    alert("You're at IP address " + response.ip + ", which is in " + response.city + ", " + response.region_name);
};

var script = document.createElement("script");
script.src = "http://freegenip.net/json/?callback=handleResponse";
document.body.insertBefore(script,document.body.firdtChild);
```

JSONP简单易用，并且支持在浏览器与服务器之间的双向通信。

但也有它的不足：如果你所需的其他域不安全，只能完全放弃JSONP调用；其次，要确定JSONP请求是否失败并不容易。

### Comet

Comet是一种更高级的Ajax技术。

Ajax是一种从页面向服务器请求数据的技术，而Comet则是一种服务器向页面推送数据的技术。

Comet能够让信息近乎实时地被推送到页面上，非常适合处理体育比赛的分数和股票报价。

有两种实现Comet的方式：长轮询和流。

#### 长轮询

是传统轮询（短轮询）的一个翻版。

长轮询和短轮询的区别：

- 短轮询的工作机制是：浏览器向服务器发送请求，然后服务器立即发送响应，无论数据是否有效。
- 长轮询的工作机制是：浏览器向服务器发送请求，然后等待服务器中数据有变化然后发送响应，将数据返回到浏览器后，再次发送下一条请求。

轮询的优势是所有浏览器都支持，因为使用XHR对象和setTimeout()就能实现。而你要做的就是决定什么时候发送请求。

#### HTTP流

实现Comet机制是：浏览器向服务器发送请求，而服务器保持连接打开，然后周期性的向浏览器发送数据。

所有服务器端语言都支持打印到输出缓存然后刷新的功能。而这正是实现HTTP流的关键所在。

使用XHR对象实现HTTP流的典型代码如下所示：

```js
function creatStreamingClient(url, progress, finished) {//创建客户端流（要连接的URL，在接收到数据时调用的函数，关闭连接时调用的函数）
    var xhr = new XMLHttpRequest(),
        received = 0;
    
    xhr.open("get", url, true);
    xhr.onreadystatechange = function () {
        var result;
        
        if(xhr.readystate == 3){//已经接受到部分响应数据  
            //只取得最新数据并调整计数器
            result = xhr.responseText.substring(received);//substring()提取介于两下标之间的字符串
            received += result.length;
            
            //调用progress回调函数
            progress(result);
        } else if (xhr.readystate == 4){
            finished(xhr.responseText);//关闭此次连接
        }
    };
    xhr.send(null);
    return xhr;
};

//应用此函数
var client = createStreamingClient("streaming.php", function (data){
    alert("Received: " + data);
}, function (data) {
    alert("Done!");
});
```

### 服务器发送事件

SSE--服务器发送事件，是围绕只读Comet交互退出的API或者模式。

SSE API 用于创建到服务器的单向连接，服务器通过这个连接可以发送任意数量的数据。

SSE支持短轮询、长轮询和HTTP流，而且能在断开连接时自动确定何时重新连接。

也有了这么简单实用的API，再实现Comet就容易多了。

#### SSE API

要预定新的事件流，首先创建一个新的EventSource对象，并传入一个入口点：

var source = new EventSource("myevents.php");

注意，传入的URL必须与创建对象的页面同源。

EventSource的实例有个readyState属性：

- 0：表示正连接到服务器。
- 1：表示打开了连接。
- 2：表示关闭了连接。

另外还有3个事件：

- open：在建立连接时触发。
- message：在从服务器接收到新事件时触发。
- error：在无法建立连接时触发。

服务器发回的数据以字符串形式保存在event.data中。

#### 事件流

data: foo

id: 1

...

### Web Sockets

web sockets的目标是在一个单独的持久连接上提供全双工、双向通信。

web sockets 需要使用自定义的协议，所以URL也有所不同：

- http:// --> ws://
- https:// --> wss://

使用自定义协议的好处：能够在客户端与服务端发送非常少的数据，而不必担心http那样字节级的开销。由于传递的数据包非常小，因此web sockets非常适合移动的应用。

使用自定义协议的缺点：制定协议的时间比制定JavaScript API的时间还要长。

#### Web Sockets API

要创建 Web Socket， 先实力一个WebSocket对象并传入要连接的URL：

```js
var socket = new WebSocket("ws://www.wcample.com/server/php");//一定是绝对的URL
```

websocket也有一个表示当前状态的readyState属性，其属性的值为：

- WebSocket.OPENING(0):正在建立连接
- WebSocket.OPEN(1):已经建立连接
- WebSocket.CLOSING(2):正在关闭连接
- WebSocket.CLOSE(3):已经关闭连接

关闭Web Sockket连接：

```js
socket.close();
```

#### 发送和接受数据

使用send()方法并传入字符串来向服务器发送数据：

```js
var socket = new WebSocket("ws://www.example.com/server/php");
socket.send("Hello world");
```

对于复杂的数据结构必须经过序列化再发送：

```js
var message = {
    time: new Date(),
    text: "Hello world",
    clientId: "adfad"
};

socket.send(JSON.stringify(message));
```

### SSE与Web Sockets

在考虑是用哪个时，先考虑以下因素：

- Web Socket需要制定协议，现有的服务器不能用于Web Socket。
- SSE可以通过常规的HTTP通信。
- Web Socket支持双向通信。
- SSE需要组合XHR才能实现双向通信。

## 安全

可以通过XHR访问的任何URL也可以通过浏览器或服务器来访问。

CSRF：是未被授权系统有权访问某个资源的情况。



## 小结

图像Ping和JSONP是另外两种跨域通信的技术，但不如CORS稳妥。

Comet是对Ajax的进一步扩展，让服务器几乎能够实时地向客户端推送数据。

SSE是一种实现Comet交互的浏览器API。

Web Sockets是一种与服务器进行全双工、双向通信的信道。

















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































