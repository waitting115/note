# js的柯里化

柯里化其实就是高阶函数的一个特殊用法。

**柯里化：英语--Currying，是把接受多个参数的函数变成接受单一参数（最初函数的第一个参数）的函数，并且返回接受余下参数并返回结果的新函数的技术。**

举例：add函数：

```js
		// 普通的add函数
		function add(x,y) {
			return x + y;
		}

		console.log(add(2,3));//5

		// 用柯里化技术实现的add功能函数
		function curryingAdd(x) {
			return function (y) {
				return x + y;
			}
		}

		console.log(curryingAdd(3)(4));//7
```

思路就是：只传递给函数一部分参数来调用它，让它返回一个函数来处理剩下的参数。

那么问题来了，费这么大劲封装一层，到底有什么好处呢？

## 柯里化的好处

### 1.参数复用

~~~js
		//正常正则验证字符串  reg.test(text);

		// 函数封装后
		function check(reg,text) {
			return reg.test(text);
		}

		console.log(check(/\d+/g,'test'));//f

		// Currying之后
		function curryingCheck(reg) {
			return function (text) {
				return reg.test(text);
			}
		}

		var hasNumber = curryingCheck(/\d+/g);
		var hasLetter = curryingCheck(/[a-z]+/g);

		console.log(hasNumber('test1'));//t
		console.log(hasNumber('testtest'));//f
		console.log(hasLetter('21122'));//f
		console.log(hasLetter('q121'));//t
~~~

如果我有很多地方需要校验是否有数字，字母，就需要将reg参数复用。这样其他地方就可以直接调用hasNumber、hasLetter等函数。

### 2.提前确认

~~~js
var on = function(element, event, handler) {
    if (document.addEventListener) {
        if (element && event && handler) {
            element.addEventListener(event, handler, false);
        }
    } else {
        if (element && event && handler) {
            element.attachEvent('on' + event, handler);
        }
    }
}

var on = (function() {
    if (document.addEventListener) {
        return function(element, event, handler) {
            if (element && event && handler) {
                element.addEventListener(event, handler, false);
            }
        };
    } else {
        return function(element, event, handler) {
            if (element && event && handler) {
                element.attachEvent('on' + event, handler);
            }
        };
    }
})();

//换一种写法可能比较好理解一点，上面就是把isSupport这个参数给先确定下来了
var on = function(isSupport, element, event, handler) {
    isSupport = isSupport || document.addEventListener;
    if (isSupport) {
        return element.addEventListener(event, handler, false);
    } else {
        return element.attachEvent('on' + event, handler);
    }
}
~~~

我们在做项目的过程中，封装一些dom操作可以说再常见不过，上面第一种写法也是比较常见，但是我们看看第二种写法，它相对一第一种写法就是自执行然后返回一个新的函数，这样其实就是提前确定了会走哪一个方法，避免每次都进行判断。

### 3.延迟运行

~~~js
		// bind()
		Function.prototype.bind = function (arg1) {
			var _this = this;
			var args = Array.prototype.slice.call(arguments,1);//slice函数是将数组指定位置返回出来，后面的参数1就代表返回下标为1到最后的元素

			return function (args) {
				return _this.apply(arg1,args);
			}
		}
~~~

像我们js中经常使用的bind，实现的机制就是Currying.

## 封装Currying

~~~js
		// 封装Currying
		function processCurrying(fn,args) {
			var _this = this;
			var len = fn.length;
			var args = args || [];

			return function () {
				var _args = Array.prototype.slice.call(arguments);
				Array.prototype.push.apply(args, _args);//将_args  push到args中

				// 如果参数个数小于最初fn的个数，那么继续递归调用，继续收集参数
				if(_args.length > len) {//_args &&  args
					return processCurrying.call(_this,fn,_args);//_args && args
				}

				//参数收集完毕，执行fn
				return fn.apply(this,_args);//this   _args?
			}
		}
~~~

## 性能问题

curry的一些性能问题你只要知道下面四点就差不多了：

- 存取arguments对象通常要比存取命名参数要慢一点
- 一些老版本的浏览器在arguments.length的实现上是相当慢的
- 使用fn.apply( … ) 和 fn.call( … )通常比直接调用fn( … ) 稍微慢点
- 创建大量嵌套作用域和闭包函数会带来花销，无论是在内存还是速度上

其实在大部分应用中，主要的性能瓶颈是在操作DOM节点上，这js的性能损耗基本是可以忽略不计的，所以curry是可以直接放心的使用。

## 经典面试题

~~~js
		//实现一个add方法，使计算结果能满足如下预期
		// add(1)(3)(5) = 9;
		// add(1,2,3)(4) = 10;
		// add(1)(2)(3)(4)(5) = 15;

		function add() {
			//第一次执行时，定义一个数组来保存所有所有参数
			var _args = Array.prototype.slice.call(arguments);
			console.log(_args);

			//在内部声明一个函数，利用闭包的特性保存_args并收集所有参数值
			var _adder = function () {
				_args.push(...arguments);
				return _adder;
			}

			//利用toString的隐式转换特性，当最后执行时隐式转换，并计算最终的值并返回
			_adder.toString = function () {
				return _args.reduce(function (a,b) {
					return a + b;
				});
			}
			return _adder;
		}

		console.log(add(1)(3)(5));//f 9
		console.log(add(1,2,3)(4));//f 10
		console.log(add(1)(2)(3)(4)(5));//f 15
~~~











