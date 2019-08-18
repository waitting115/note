# 第十章 DOM

DOM（文档对象模型）是针对HTML和XML文档的一个API（应用编程接口）。

注意，IE中的所有DOM对象都是以COM对象的形式实现的。这意味着IE中的DOM对象与原生的JavaScript对象的行为活动特点并不一致。

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

每个节点都有一个childNodes，保存着NodeList对象。NodeList是以追踪类数组对象，用于保存着一组有序节点，可通过位置访问这些节点。但要注意它并不是Array实例。

DOM结构的变化能够自动反映在NodeList对象中。 我们常说，NodeList是有生命、有呼吸的对象，而不是某一瞬间拍下的照片。

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

以这种方式加载的代码会在全局作用域中执行，而且当脚本执行后将立即可用。实际上，这与在全局作用域中把相同的字符串传递给eval()是一样的。

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
tbody.row[0].insertCell(0);
tbody.row[0].cell[0].appendChild(createTextNode("Cell 1,1"));
tbody.row[0].insertCell(1);
tbody.row[0].cell[1].appendChile(createTextNode("Cell 1,2"));

//创建第二行，以及里面的节点
tbody.insertRow(1);
tbody.row[1].insertCell(0);
tbody.row[1].cell[0].appendChild(createTextNode("Cell 2,1"));
tbody.row[1].insertCell(1);
tbody.row[1].cell[1].appendChild(createTextNode("Cell 2,2"));

//将创建好的表格插入到body中
document.body.appendChild(table);
```

### 使用NoodeList

要理解NodeList及其  近亲  NamedNodeMap和HTMLCollection，是从整体上理解DOM的关键之所在。

这三个集合都是动态的。

换句话说，每当文档结构发生变化时，它们都会得到更新。因此它们始终保存着最新的、最准确的信息。

从本质上说，所有NodeList对象都是在访问DOM文档时运行的查询。

一般来说，应该尽量减少访问NodeList的次数。因为每次访问NodeList，都会运行一次基于文档结构的查询。所以，可以考虑将从NodeList中取得 的值缓存起来。

## 小结

理解DOM的关键，就是理解DOM对性能 的影响。DOM操作往往是JavaScript中开销最大的部分，而因访问NodeList导致的问题最多。NodeList对象都是“动态的”，这就意味着每次访问NodeList对象，都会运行一次查询。

有鉴于此，最好的办法就是尽量减少DOM操作。

# 第十一章 DOM扩展

对DOM的两个主要的扩展是Selectors API和HTML5。

## 选择符API

众多JavaScript库中最常用的一项功能。

### querySelector()方法

它接受一个CSS选择符，返回与该模式匹配的第一个元素，如果没找到匹配元素，返回null。

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

它也接受一个CSS选择符，但返回的是所有匹配的元素，而不仅仅是第一个元素。这个方法返回的是一个NodeList的实例。

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

接受一个包含一或多个雷鸣的字符串作为参数，返回带有指定类的所有元素的NodeList。

```js
//取得所有类中包含 username 和current 的元素，类名的先后顺序无所谓
var allCurrentUsernames = document.getElementsByClassName("username current");

//取得ID为 myDiv 的元素中带有类名 selected 的所有元素
var selecteds = document.getELementById("myDiv").getElementsByClassName("selected");
```

因为返回的对象是NodeList（实时更新的对象），所以也会有性能问题。

#### classList属性

HTML5新增了一种操作类名的方式，那就是为所有元素添加classList属性，表示元素的所有class值的集合，它是新集合类型DOMTokenList的实例。

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

这个属性始终会引用DOM中当前获得了焦点的元素。元素获得焦点的方式有页面加载、用户输入和在代码中调用focus()方法。

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

HTML5规定可以为元素添加非标准的属性，但要添加前缀data-，目的是为元素提供与渲染无关的信息，或者提供语义信息。

这些属性可以任意添加、随便命名，只要以data-开头即可

```html
<div id="myDiv" data-appId="21345" data-myName="Nicholas">
    
</div>
```

添加了自定义属性后，可以通过元素的dataset属性来访问自定义属性的值。

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

- 在读模式下，innerHTML属性返回与调用元素的所有子节点对应的HTML标记。
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

因此在使用innerHTML、outerHTML属性和insertAdjacentHTML()方法时，最好要手动删除要被替换的元素的所有事件处理程序和JavaScript对象的属性。

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

scrollIntoView()可以在所有HTML元素上调用，通过滚动浏览器窗口或某个容器元素，调用元素就可以出现在视口中。

它接受 两个参数：

- true：或者不传参数，那么窗口滚动之后会让元素顶部与视口顶部尽可能平齐；
- false：调用元素会尽可能全部出现在视口中，不过顶部不一定平齐。

当页面发生变化时，一般用这个方法来吸引用户注意力。

支持scrollIntoView()方法的浏览器有IE、Firefox、Safari和Opera。

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

由于IE29之前的版本与其他浏览器在处理文本节点中的空白符时有差异，因此就出现了children属性。

这个属性时HTMLCollection的实例，只包含元素中同样还是元素的子节点。

```js
var childrenCount = element.children.length;
var firstChild = element.children[0];
```

IE8及更早版本的children属性中也会包含注释节点 ，但IE9之后的版本则只返回元素节点。

### contains()方法

用于检查一个节点是否是调用节点的子节点。是返回true，否则返回false。（不通过DOM）

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

通过innerText属性可以操作元素中包含的所有文本。

textContent属性也有类似的作用。但是两个属性浏览器兼容性不同，所以有必要写两个函数来检测属性和修改属性：

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

除了作用范围扩大到了包含调用它的节点之外，outerText与innerText基本上没有多大区别。

## 滚动

下面几个方法是对HTMLElement类型的扩展：

- scrollIntoViewIfNeeded(alignCenter):  只有在当前元素在视口中不可见的情况下，才滚动浏览器窗口或容器元素，最终让它可见。
- scrollByLines(lineCount)：将元素的内容滚动指定的行高，lineCount可正可负。
- scrollByPages(pageCount)：将元素的内容滚动指定的页面高度。

这三个方法都是只有Safari和Chrome实现了。

由于scrollIntoView()是唯一一个所有浏览器都支持的方法，因此还是这个方法最常用。

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
    obj[name] = function () {
        if(fnc.length === arguments.length){
            return fnc.apply(this, arguments);
        } else if(typeof oldMethod === "function") {
            return oldMethod.apply(this, arguments);
        }
        //函数的长度就是定义形参的个数,我们可以利用这一点来写重载函数。
        
        //call、apply和bind的区别是：call第二个及以后的参数接受的是和一个参数列表，而apply接受的是参数数组。而bind()方法调用并改变函数运行时上下文后，返回一个新的函数，供我们需要时再调用。
    }
};

var people = {
    values: ["zhang san", "li si"];//values在这里
};

method(people, "find", function () {
    console.log("无参数");
    return this.values;//返回全部
});

method(people,"find",function (firstName) {
    console.log("一个参数");
    var ret = [];
    for(var i=0; i<this.values.length; i++){
        if{this.values[i].indexOf(firstName) == 0}{//indexOf()方法会返回某个字符串在大字符串中首次出现的位置
            ret.push(this.values[i]);
        }
    }
    return ret;
});

method(people, "find", function (firstName, lastName){
    console.log("两个参数");
    var ret = [];
    for(var i=0; i<this.values.length; i++){
        if(this.values[i] == firstName + " " + lastName){
            ret.push(this.values[i]);
        }
    }
    return ret;;
});

console.log(people.find());				//["Zhang san","Li si"]
console.log(people.find("Zhang"));		//["Zhang san"]
console.log(people.find("Li"));			//["Li si"]
console.log(people.find("Zhang","Li"));	//["Zhang san","Li si"]
console.log(people.find("san"));		//[]
```



**思路**：这段代码第一眼看到我是懵逼的，再看有点思路，再看又懵了。这种方法巧妙的运用了闭包原理，既然js后面的函数会覆盖前面的同名函数，我就强行让所有的函数都留在内存里，等我需要的时候再去找它。有了这个想法，是不是就想到了闭包，函数外访问函数内的变量，从而使函数留在内存中不被删除。就是闭包。

**实现过程**：我们看一下上面这段代码，最重要的是method方法的定义：这个方法中最重要的一点就是这个oldMethod，这个oldMethod真的很巧妙。它的作用相当于一个指针，指向上一次被调用的method函数，这样说可能有点不太懂，我们根据代码来说，js的解析顺序从上到下为。

　　1.解析method（先不管里面的东西）

　　2.method(people,"find",function()  执行这句的时候，它就回去执行上面定义的方法，然后此时oldMethod的值为空，因为你还没有定义过这个函数，所以它此时是undefined，然后继续执行，这时我们才定义 obj[name] = function()，然后js解析的时候发现返回了fnc函数，更重要的是fnc函数里面还调用了method里面的变量，这不就是闭包了，因为fnc函数的实现是在调用时候才会去实现，所以js就想，这我执行完也不能删除啊，要不外面那个用啥，就留着吧先（此处用call函数改变了fnc函数内部的this指向）

　　3.好了第一次method的使用结束了，开始了第二句，method(people,"find",function(firstname) 然后这次使用的时候，又要执行oldMethod = obj[name]此时的oldMethod是什么，是函数了，因为上一条语句定义过了，而且没有删除，那我这次的oldMethod实际上指向的是上次定义的方法，它起的作用好像一个指针，指向了上一次定义的 obj[name]。然后继续往下解析，又是闭包，还得留着。

　　4.第三次的method调用开始了，同理oldMethod指向的是上次定义的 obj[name] 同样也还是闭包，还得留着。

　　5.到这里，内存中实际上有三个 obj[name]，因为三次method的内存都没有删除，这是不是实现了三个函数共存，同时还可以用oldMethod将它们联系起来是不是很巧妙

　　6.我们 people.find() 的时候，就会最先调用最后一次调用method时定义的function，如果参数个数相同 也就是  arguments.length === fnc.length 那么就执行就好了，也不用找别的函数了，如果不相同的话，那就得用到oldMethod了  return oldMethod.apply(this,arguments); oldMethod指向的是上次method调用时定义的函数，所以我们就去上一次的找，如果找到了，继续执行 arguments.length === fnc.length  如果找不到，再次调用ooldMethod 继续向上找，只要你定义过，肯定能找到的，对吧！

　　总结：运用闭包的原理使三个函数共存于内存中，oldMethod相当于一个指针，指向上一次定义的function，每次调用的时候，决定是否需要寻找。

![重载](E:\TyporaFiles\img\重载.png)

执行过程很容易说明这一点：首先第一次调用的时候 oldMethod肯定不是函数，所以instance判断是false，继续调用的话就会为true。然后，我们调用method的顺序，是从没有参数到两个参数，所以我们最先调用find方法，是最后一次method调用时定义的，所以fnc的length长度是2.然后向上找，length为1，最后终于找到了length为0的然后执行，输出。

# 第十二章 DOM2和DOM3

## DOM变化

DOM有很多的API，可以通过以下代码来确定浏览器是否支持这些DOM模块：

```js
var supportsDOM2Core = document.implementation.hasFeature("Core", "2.0");
var supportsDOM3Core = document.implementation.hasFeature("Core", "3.0");
var supportsDOM2Views = document.implementation.hasFeature("Views", "2.0");
```

### HTML  XHTML  XML的区别

XML是可扩展标记语言，为文档的创建，结构化的存储和编码提供了规则。它是一种语法，或者说是一个编写规范，它定义如何写数据，而不是写什么数据。

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

此时，<svg>中的所有元素都被认为属于http://www.w3.org/2000/svg命名空间。即使这个文档从技术上说是一个XHTML文档，但因为有了命名空间，其中的SVG代码仍然也是有效的。

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

在js中，由于一个项目都是由多个前端工程师共同完成，所以也会有命名冲突的问题，解决办法就是将每个人的变量以及方法都放在自己的对象中，引用的时候引用自己对象中的变量名以及方法名。

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

由于特性时由NameNodeMap表示的，因此这些方法多数情况下只针对特性使用。

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



用例子表示三集合对象：

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

这里的title1就不会编程property。

即，只要是DOM标签中出现的属性，都是Attribute。然后有些常用的特性（id，class，title等），会被转化为property。可以形象的说，这些特性/属性，是脚踏两只船的，因为它们既是特性又是属性。

最后注意，class边长property后就变成了className了，因为class是js中的关键字。

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

对于使用-分隔的css属性，必须将其转化为驼峰大小写形式，才能通过JavaScript来访问。

如：

| CSS属性          | JavaScript属性        |
| ---------------- | --------------------- |
| background-image | style.backgroundInage |
| color            | style.color           |
| display          | style.display         |
| font-family      | style.fontFamily      |

其中有一个不能直接转化的css属性就是float，因为float是JavaScript的保留字，因此不能用作属性名。所以将其写为 CSSFloat，IE中写为styleFloat。

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

### 操作样式表

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

  ````js
  function getStyleSheet(element){
      return elemet.sheet || element.styleSheet;
  }
  
  //取得第一个link元素引入的样式表
  var link = document.getElementsByTagName("link")[0];
  var sheet = getStyleSheet(link);
  ````

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

````html
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
````

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





















































