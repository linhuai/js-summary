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

### 3、 检测属性

  可以通过 in 运算符、hasOwnProperty()、propertyIsEnumerable() 来检测属性是否存在于对象中

(1）in 运算符，用来检测对象**自有属性**或**继承属性**中是否包含这个属性(可枚举属性)，如果有则返回 true

(2) hasOwnProperty() 方法，用来检测给定的属性是否是对象**自有属性**（可枚取属性和不可枚举属性），如果是自有属性则返回 true

(3) propertyIsEnumerable() 方法 是 hasOwnProperty() 的增强版, 只有检测到自有属性且这个属性是**可枚举**的，则返回 true
	 

```
var o = { x: 1 }
console.log('x' in o) // true
console.log('y' in o) // false
console.log('toString' in o) // true 继承属性

console.log(o.hasOwnProperty('toString')) // false
console.log(o.hasOwnProperty('x')) // true

console.log(o.propertyIsEnumerable('toString')) // false
console.log(o.propertyIsEnumerable('x')) // true
```
**常用工具类**

```
/* 
 * 把 p 中的可枚取属性复制到 o 中，并返回 o
 * 如果 o 和 p 中包含同名属性，则覆盖 o 中的属性
 */
function extend(o, p) {
	for(let prop in p) {
		o[prop] = p[prop]
	}
	return o
}

/*
 * 将 p 中的可枚举属性复制到 o 中，并返回 o
 * 如果 o 和 p 中有同名属性，o 中的属性将不受影响
 * /
function merge(o, p) {
	for (let prop in p) {
		if (o.hasOwnProperty(prop)) continue
		o[prop] = p[prop]
	}
	return o
}
```










