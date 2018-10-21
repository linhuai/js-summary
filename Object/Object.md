# Object

### 1、 访问对象原型的 4 种方法

1. Person.prototype

	可以直接使用 prototype 属性来访问原型
	
2. Object.getPrototypeOf(person)

	可以使用 Object.getPrototypeOf 函数来访问一个实例对象的原型

3. Reflect.getPrototypeOf(person)

	可以使用 Reflect.getPrototypeOf 函数来访问一个实例对象的原型

4. person.__proto__

	这个属性暴露对象内部可以访问的原型属性
	
	
### 2、 通过 new 创建实例

通过 new 创建实例会经历以下 4 个步骤 

（1）创始一个新对象

（2) 将构造函数的作用域赋给新的对象（因此 this 指向了这个新对象）

（3) 执行构造函数中的代码（为这个对象添加属性）

（4) 返回新对象