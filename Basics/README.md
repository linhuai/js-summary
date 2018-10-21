# Javascript 基础

### 1. 数据类型

  (1) 基础类型  (2) 对象类型  (3) symbol

  基础类型：Number、String、Boolean、null、undefined

  对象类型：Object、Array、Function、Date、RegExp、Error
  
  使用 **typeof** 可以判断数据类型
  
  返回值 String (number, string, boolean, object, symbol)
  
  ```
  console.log(typeof 1) // number
  
  console.log(typeof 'abc') // string
  
  console.log(typeof true) // boolean
  
  console.log(typeof null) // object
  
  console.log(typeof Symblo()) // symbol
  ```
  
### 2. 全局对象

全局属性： undefined、Infinity、NaN...

全局函数：isNaN()、Number()、parseInt()、parseFloat()...

构造函数：Date()、RegExp()、String()、Object()、Array()...

全局对象：Math、JSON...