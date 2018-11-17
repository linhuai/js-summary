# Javascript
js 积累总结

### 1、 相等运算符 算法细节

1. ReturnIfAbrupt(x).

   如果x不是正常值（比如抛出一个错误），中断执行。

2. ReturnIfAbrupt(y).

	如果y不是正常值，中断执行。

3. If Type(x) is the same as Type(y), then
Return the result of performing Strict Equality Comparison x === y.

	如果Type(x)与Type(y)相同，执行严格相等运算x === y。

4. If x is null and y is undefined, return true.

	如果x是null，y是undefined，返回true。

5. If x is undefined and y is null, return true.

	如果x是undefined，y是null，返回true。

6. If Type(x) is Number and Type(y) is String, return the result of the comparison x == ToNumber(y).

	如果Type(x)是数值，Type(y)是字符串，返回x == ToNumber(y)的结果。

7. If Type(x) is String and Type(y) is Number, return the result of the comparison ToNumber(x) == y.

	如果Type(x)是字符串，Type(y)是数值，返回ToNumber(x) == y的结果。

8. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.

	如果Type(x)是布尔值，返回ToNumber(x) == y的结果。

9. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).

	如果Type(y)是布尔值，返回x == ToNumber(y)的结果。

10. If Type(x) is either String, Number, or Symbol and Type(y) is Object, then return the result of the comparison x == ToPrimitive(y).

	如果Type(x)是字符串或数值或Symbol值，Type(y)是对象，返回x == ToPrimitive(y)的结果。

11. If Type(x) is Object and Type(y) is either String, Number, or Symbol, then return the result of the comparison ToPrimitive(x) == y.

	如果Type(x)是对象，Type(y)是字符串或数值或Symbol值，返回ToPrimitive(x) == y的结果。


12. Return false.

	返回false。
	

## javascript-函数的5个高级技巧

### 1. 作用域安全的构造函数

构造函数其实就是一个使用new操作符调用的函数

```
function Person (name,age,job){    
	this.name = name;    
	this.age = age;    
	this.job = job;
}

var person = new Person('match', 28, 'Software Engineer');
console.log(person.name); //match
```
如果没有使用new操作符，原本针对Person对象的三个属性被添加到window对象

```
function Person (name,age,job) {    
	this.name=name;    
	this.age=age;    
	this.job=job;
} 
         
var person = Person('match', 28, 'Software Engineer');
console.log(person); //undefined
console.log(window.name); //match
```
window的name属性是用来标识链接目标和框架的，这里对该属性的偶然覆盖可能会导致页面上的其它错误，这个问题的解决方法就是创建一个作用域安全的构造函数

```
function Person (name,age,job) {    
	if (this instanceof Person) {        
		this.name=name;        
		this.age=age;        
		this.job=job;
    } else {        
    	return new Person(name, age, job);
    }
}

var person = Person('match', 28, 'Software Engineer');
console.log(window.name); // ""
console.log(person.name); //'match'

var person= new Person('match', 28, 'Software Engineer');
console.log(window.name); // ""
console.log(person.name); //'match'
```
但是，对构造函数窃取模式的继承，会带来副作用。这是因为，下列代码中，this对象并非Polygon对象实例，所以构造函数Polygon()会创建并返回一个新的实例

```
function Polygon (sides) {   
	if (this instanceof Polygon) {        
		this.sides = sides;        
		this.getArea = function() {            
			return 0;
       }
    } else {        
    	return new Polygon(sides);
    }
}

function  Rectangle (wifth,height) {
    Polygon.call(this,2);    
    this.width = this.width;    
    this.height = height;    
    this.getArea = function () {        
    	return this.width * this.height;
    };
}

var rect= new Rectangle(5, 10);
console.log(rect.sides); //undefined
```

如果要使用作用域安全的构造函数窃取模式的话，需要结合原型链继承，重写Rectangle的prototype属性，使它的实例也变成Polygon的实例

```
function Polygon (sides) {    
	if (this instanceof Polygon) {        
		this.sides=sides;        
		this.getArea= function () {            
			return 0;
		}
    } else {        
    	return new Polygon(sides);
    }
}

function  Rectangle (wifth,height) {
    Polygon.call(this,2);    
    this.width = this.width;    
    this.height = height;    
    this.getArea = function () {        
    	return this.width * this.height;
    };
}
Rectangle.prototype= new Polygon();var rect= new Rectangle(5,10);
console.log(rect.sides); //2
```

### 2. 惰性载入函数

因为各浏览器之间的行为的差异，我们经常会在函数中包含了大量的if语句，以检查浏览器特性，解决不同浏览器的兼容问题。比如，我们最常见的为dom节点添加事件的函数

```
function addEvent (type, element, fun) {    
	if (element.addEventListener) {
		element.addEventListener(type, fun, false);
	} else if (element.attachEvent) {
   		element.attachEvent('on' + type, fun);
   } else {
       element['on' + type] = fun;
   }
}
```

每次调用addEvent函数的时候，它都要对浏览器所支持的能力进行检查，首先检查是否支持addEventListener方法，如果不支持，再检查是否支持attachEvent方法，如果还不支持，就用dom0级的方法添加事件。这个过程，在addEvent函数每次调用的时候都要走一遍，其实，如果浏览器支持其中的一种方法，那么他就会一直支持了，就没有必要再进行其他分支的检测了。也就是说，if语句不必每次都执行，代码可以运行的更快一些。

　　解决方案就是惰性载入。所谓惰性载入，指函数执行的分支只会发生一次，有两种实现惰性载入的方式

　　1、第一种是在函数被调用时，再处理函数。函数在第一次调用时，该函数会被覆盖为另外一个按合适方式执行的函数，这样任何对原函数的调用都不用再经过执行的分支了

　　我们可以用下面的方式使用惰性载入重写addEvent()

```
function addEvent(type, element, fun) {    
	if (element.addEventListener) {
		addEvent = function (type, element, fun) {
			element.addEventListener(type, fun, false);
		}
	} else if (element.attachEvent) {
		addEvent = function (type, element, fun) {
			element.attachEvent('on' + type, fun);
		}
	} else {
		addEvent = function (type, element, fun) {
			element['on' + type] = fun;
		}
	} 
	return addEvent(type, element, fun);
}
```
在这个惰性载入的addEvent()中，if语句的每个分支都会为addEvent变量赋值，有效覆盖了原函数。最后一步便是调用了新赋函数。下一次调用addEvent()时，便会直接调用新赋值的函数，这样就不用再执行if语句了

　　但是，这种方法有个缺点，如果函数名称有所改变，修改起来比较麻烦

　　2、第二种是声明函数时就指定适当的函数。 这样在第一次调用函数时就不会损失性能了，只在代码加载时会损失一点性能

　　以下就是按照这一思路重写的addEvent()。以下代码创建了一个匿名的自执行函数，通过不同的分支以确定应该使用哪个函数实现

```
var addEvent = (function () {    
	if (document.addEventListener) {        
		return function (type, element, fun) {
			element.addEventListener(type, fun, false);
		}
	} else if (document.attachEvent) {        
		return function (type, element, fun) {
			element.attachEvent('on' + type, fun);
		}
	} else {        
		return function (type, element, fun) {
			element['on' + type] = fun;
		}
	}
})();
```
### 3. 函数绑定

在javascript与DOM交互中经常需要使用函数绑定，定义一个函数然后将其绑定到特定DOM元素或集合的某个事件触发程序上，绑定函数经常和回调函数及事件处理程序一起使用，以便把函数作为变量传递的同时保留代码执行环境

```
<button id="btn">按钮</button>
<script>            
    var handler={
        message:"Event handled.",
        handlerFun:function(){
            alert(this.message);
        }
    };
	btn.onclick = handler.handlerFun;
</script>
```

上面的代码创建了一个叫做handler的对象。handler.handlerFun()方法被分配为一个DOM按钮的事件处理程序。当按下该按钮时，就调用该函数，显示一个警告框。虽然貌似警告框应该显示Event handled，然而实际上显示的是undefiend。这个问题在于没有保存handler.handleClick()的环境，所以this对象最后是指向了DOM按钮而非handler

　　可以使用闭包来修正这个问题
　　
```
<button id="btn">按钮</button>
<script>            
	var handler = {
	    message:"Event handled.",
	    handlerFun:function(){
	        alert(this.message);
	    }
	};
	btn.onclick = function(){
	    handler.handlerFun();    
	}
</script>
```
当然这是特定于此场景的解决方案，创建多个闭包可能会令代码难以理解和调试。更好的办法是使用函数绑定

　　一个简单的绑定函数bind()接受一个函数和一个环境，并返回一个在给定环境中调用给定函数的函数，并且将所有参数原封不动传递过去

```
function bind(fn,context){    
	return function () {        
		return fn.apply(context,arguments);
   }
}
```

这个函数似乎简单，但其功能是非常强大的。在bind()中创建了一个闭包，闭包使用apply()调用传入的函数，并给apply()传递context对象和参数。当调用返回的函数时，它会在给定环境中执行被传入的函数并给出所有参数

```
<button id="btn">按钮</button>
<script>  
	function bind(fn,context){    
		return function () {        
			return fn.apply(context,arguments);
    	}
	}          
	var handler={
	    message:"Event handled.",
	    handlerFun:function(){
	        alert(this.message);
	    }
	};
	btn.onclick = bind(handler.handlerFun,handler);
</script>
```

ECMAScript5为所有函数定义了一个原生的bind()方法，进一步简化了操作

　　只要是将某个函数指针以值的形式进行传递，同时该函数必须在特定环境中执行，被绑定函数的效用就突显出来了。它们主要用于事件处理程序以及setTimeout()和setInterval()。然而，被绑定函数与普通函数相比有更多的开销，它们需要更多内存，同时也因为多重函数调用稍微慢一点，所以最好只在必要时使用

### 4. 函数柯里化

与函数绑定紧密相关的主题是函数柯里化(function currying)，它用于创建已经设置好了一个或多个参数的函数。函数柯里化的基本方法和函数绑定是一样的：使用一个闭包返回一个函数。两者的区别在于，当函数被调用时，返回的函数还需要设置一些传入的参数

```
function add(num1,num2){    
	return num1+num2;
}
function curriedAdd (num2){    
	return add(5,num2);
}
console.log(add(2,3));	// 5
console.log(curriedAdd(3)); // 8
```
这段代码定义了两个函数：add()和curriedAdd()。后者本质上是在任何情况下第一个参数为5的add()版本。尽管从技术来说curriedAdd()并非柯里化的函数，但它很好地展示了其概念

　　柯里化函数通常由以下步骤动态创建：调用另一个函数并为它传入要柯里化的函数和必要参数。下面是创建柯里化函数的通用方式

```
function curry (fn) {    
	var args = Array.prototype.slice.call(arguments, 1);    
	return function () {        
		var innerArgs = Array.prototype.slice.call(arguments),
   		finalArgs = args.concat(innerArgs);        
    	return fn.apply(null, finalArgs);
    };
}
```
curry()函数的主要工作就是将被返回函数的参数进行排序。curry()的第一个参数是要进行柯里化的函数，其他参数是要传入的值。为了获取第一个参数之后的所有参数，在arguments对象上调用了slice()方法，并传入参数1表示被返回的数组包含从第二个参数开始的所有参数。然后args数组包含了来自外部函数的参数。在内部函数中，创建了innerArgs数组用来存放所有传入的参数(又一次用到了slice())。有了存放来自外部函数和内部函数的参数数组后，就可以使用concat()方法将它们组合为finalArgs，然后使用apply()将结果传递给函数。注意这个函数并没有考虑到执行环境，所以调用apply()时第一个参数是null。curry()函数可以按以下方式应用

```
function add (num1, num2){    
	return num1 + num2;
}
var curriedAdd = curry(add, 5);
alert(curriedAdd(3)); //8
```
在这个例子中，创建了第一个参数绑定为5的add()的柯里化版本。当调用cuurriedAdd()并传入3时，3会成为add()的第二个参数，同时第一个参数依然是5，最后结果便是和8。也可以像下例这样给出所有的函数参数：

```
function add(num1, num2){    
	return num1 + num2;
}

var curriedAdd2 = curry(add, 5, 12);
alert(curriedAdd2()); //17
```
在这里，柯里化的add()函数两个参数都提供了，所以以后就无需再传递给它们了

函数柯里化还常常作为函数绑定的一部分包含在其中，构造出更为复杂的bind()函数

```
function bind (fn, context) {    
	var args = Array.prototype.slice.call(arguments, 2);    
	return function () {        
		var innerArgs = Array.prototype.slice.call(arguments),
       finalArgs = args.concat(innerArgs);        
       return fn.apply(context, finalArgs);
    };
}
```
对curry()函数的主要更改在于传入的参数个数，以及它如何影响代码的结果。curry()仅仅接受一个要包裹的函数作为参数，而bind()同时接受函数和一个object对象。这表示给被绑定的函数的参数是从第三个开始而不是第二个，这就要更改slice()的第一处调用。另一处更改是在倒数第3行将object对象传给apply()。当使用bind()时，它会返回绑定到给定环境的函数，并且可能它其中某些函数参数已经被设好。要想除了event对象再额外给事件处理程序传递参数时，这非常有用

```
var handler = {
    message: "Event handled",
    handleClick: function(name, event){
        alert(this.message + ":" + name + ":" + event.type);
    }
};var btn = document.getElementById("my-btn");
EventUtil.addHandler(btn, "click", bind(handler.handleClick, handler, "my-btn"));
```
handler.handleClick()方法接受了两个参数：要处理的元素的名字和event对象。作为第三个参数传递给bind()函数的名字，又被传递给了handler.handleClick()，而handler.handleClick()也会同时接收到event对象

ECMAScript5的bind()方法也实现函数柯里化，只要在this的值之后再传入另一个参数即可

```
var handler = {
    message: "Event handled",
    handleClick: function(name, event){
        alert(this.message + ":" + name + ":" + event.type);
    }
};
var btn = document.getElementById("my-btn");
EventUtil.addHandler(btn, "click", handler.handleClick.bind(handler, "my-btn"));
```
javaScript中的柯里化函数和绑定函数提供了强大的动态函数创建功能。使用bind()还是curry()要根据是否需要object对象响应来决定。它们都能用于创建复杂的算法和功能，当然两者都不应滥用，因为每个函数都会带来额外的开销

### 5. 函数重写

由于一个函数可以返回另一个函数，因此可以用新的函数来覆盖旧的函数

```

function a () {
    console.log('a');
    a = function(){
        console.log('b');
    }
}
```
这样一来，当我们第一次调用该函数时会console.log('a')会被执行；全局变量a被重定义，并被赋予新的函数

当该函数再次被调用时， console.log('b')会被执行

再复杂一点的情况如下所示

```
var a = (function(){
    function someSetup(){        
    	var setup = 'done';
    }
    function actualWork(){
        console.log('work');
    }
    someSetup();    
    return actualWork;
})()
```
我们使用了私有函数someSetup()和actualWork()，当函数a()第一次被调用时，它会调用 someSetup()，并返回函数 actualWork()的引用

