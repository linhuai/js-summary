# js-summary
js 积累总结

### 相等运算符 

**算法细节

相等运算符 

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
