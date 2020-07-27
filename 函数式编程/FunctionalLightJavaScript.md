#### 函数式编程

命令式代码是指 主要关注如何做某事

注释不应该重复代码正在执行的操作，注释应该关注为什么，而不是什么



学习曲线：

![1592838314138](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592838314138.png)

什么是好的代码？ less to read

1. Functions
2. Closure
3. Composition
4. Immutability
5. Recursion
6. List/ Data Structures
7. Async
8. Fp Libraries

##### 1、理解Function

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592882394420.png)

###### 概念

函数：是一种语义在输入和输出之间

函数名应该做到描述输入和输出之间的关系，

思考一下 输入和输出之间有关系吗》？

![1592882421662](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592882421662.png)

###### 副作用 side effect

间接的输入或输出

![1592891119944](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592891119944.png)

![1592891189793](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592891189793.png)

函数的调用要避免产生副作用

![1592891403997](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592891403997.png)

###### pure function，

给一个相同的输入，就会有相同的输出

**重要的是函数调用的行为是纯还是不纯**

这是纯函数

![1592892105269](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592892105269.png)

要理解一行代码，我们必须在脑子里执行以下之前的所有内容（不能独立分析某一行代码）

![1592892803665](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592892803665.png)

缩小了z被可重新分配的范围，提高了可读性



函数的纯度

当函数不可避免要产生负作用时。Extract Impurity **提取杂质**。例子如下：

![1592894151071](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592894151071.png)

![1592894133326](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592894133326.png)

Containing Impurity **控制杂质**，减少他的表面积。

可以通过包装函数

也可以通过这些值，然后恢复他们

![1592894860628](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592894860628.png)

###### 参数

Arguments get assigned toparameters

add(x, y) {}   add(3, 4)

一个参数就是一元函数 unary

高阶函数可以将函数调整为一元



编写自己的高阶函数：当两件事情不合适的时候，可以制造一个适配器函数，

**高阶函数式函数式编程的核心**

适配器函数：调整参数顺序，反转参数，用数组去代替多个参数

![1592968421014](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592968421014.png)

![1592968600840](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592968600840.png)

![1592968816657](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592968816657.png)

等式推理：

用一个函数去定义另外一个函数

```javascript
output = console.log.bind(console)

function printIf(shouldPrintIt) {
	return function(msg) {
		if (shouldPrintIt(msg)) {
			output(msg);
		}
	};
}

function isShortEnough(str) {
	return str.length <= 5;
}
function isLongEnough(str) {
 	return !isShortEnough(str);
}
function not(fn) {
	return (...args) => !fn(...args)
}
var isLongEnough = not(isShortEnough)
```

```javascript
	function when(fn) {
		return function(predicate){
			return function(...args){
				if (predicate(...args)) {
					return fn(...args);
				}
			};
		};
	}
```

```javascript
printIf(isShortEnough)(msg)
when(output)(isShortEnough)(msg)
//因此
printIf === when(output)
```

![1592984836594](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592984836594.png)

##### 2、closure



![1592985469341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592985469341.png)

当一个函数记住他周围的变量时，这个函数也可以在任何其他地方执行

闭包的例子：

![1592985628501](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592985628501.png)

![1592985813135](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592985813135.png)

![1592985948248](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592985948248.png)