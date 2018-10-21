
# 继承

### 1､ 实现 f1 继承 f2 的所有方法

```
function f2 () {
	this.a2 = 3
	this.b2 = 4
}

function f1 () {
	this.a1 = 1
	this.a2 = 2
}

```

实现方法

```
f1.prototype = new f2()
f1.prototype.constructor= f1
```

### 2､ 如何实现继承，举例说明

Javascript 要实现继承，其实就是三层含义
（1）子类可以共享父类的方法

（2) 子类可以覆盖父类的方法 或 扩展新方法

（3) 子类和父类都是子类实例的“类型”

例子：
	
使用原型继承，中间使用临时对象作为 Child 的原型属性，临时对象的原型属性再指向父类型的原型属性，防止所有子类和父类原型属性都指向同一个对象，这样当修改子类的原型属性，就不会影响其他子类和父类
	

```
function extend(Child, Parent) {
	var F = function () { }
	F.prototype = Parent.prototype
	Child.prototype = new F()
	Child.prototype.constructor = Child
	Child.base = Parent.prototype
}
```

```
function Parent(name) {
	this.aa = 123
	this.getName = function () { // 使用闭包模拟私有属性
		return name
	}
	this.setName = function (value) {
		name = value
	}
}

Parent.prototype.print = function() {
	alert('print')
}
Parent.prototype.hello = function () {
	alert(this.getName() + 'Parent')
}

function Child(name, age) {
	Parent.apply(this, arguments) // 调用父类构造函数来继承父类
	this.age = age
}

extend(Child, Parent) // 继承 Parent

Child.prototype.hello = function () { // 重写父类 hello 方法
	alert(this.getName() + 'Child')
	Parent.prototype.apply(this, arguments) // 调用父类同名方法
}

// 子类方法
Child.prototype.doSomeghing = function () {
	alert(this.age + "child doSomething")
}

var p1 = new CHild('Jack', 22)
var p2 = new Child('Davi', 33)

p1.hello()
p2.hello()

p1.doSomthing() 	// 子类方法
p1.print() //父类方法

alert(p1 instanceof CHild) // true
alert(p1 instanceof Parent) // true


```
