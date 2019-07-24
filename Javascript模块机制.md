

## Javascript模块机制

### AMD和CMD

1. AMD指的便是 The Asynchronosus Module Definition 规范。 是 RequireJS 在推广过程中对模块定义的规范化产出。

2. eg:

   ```javascript
   require(['./add', './square'], function(addModule, squareModule) {
       console.log(addModule.add(1, 1))
       console.log(squareModule.square(3))
   });
   ```

3. CMD 其实就是 SeaJS 在推广过程中对模块定义的规范化产出.

4. eg:

   ```javascript
   define(function(require, exports, module) {
       var addModule = require('./add');
       console.log(addModule.add(1, 1))
   
       var squareModule = require('./square');
       console.log(squareModule.square(3))
   });
   ```

5. 都是通过define() 函数来引入模块

6. 两者的区别

   1.  CMD 推崇**依赖就近**，AMD 推崇**依赖前置**。看两个项目中的 main.js：
   2. 对于依赖的模块，AMD 是**提前执行**，CMD 是**延迟执行**。AMD 是将需要使用的模块先加载完再执行代码，而 CMD 是在 require 的时候才去加载模块文件，加载完再接着执行。



### CommonJs

1. AMD 和 CMD 都是用于浏览器端的模块规范，而在服务器端比如 node，采用的则是 **CommonJS** 规范。

2. eg：

   ```javascript
   导出模块的方式：
   
   var add = function(x, y) {　
       return x + y;
   };
   module.exports.add = add;
   
   引入模块的方式：
   var add = require('./add.js');
   console.log(add.add(1, 1));
   ```

3. 特点：会对引入的模块进行缓存，不同的地方在于浏览器仅缓存文件，而node缓存的是编译和执行之后的对象。不论是node中的核心模块还是文件模块，require方法对相同模块的二次加载都一律采用缓存优先的方式

4. 

### ES6 

1. ES6 规定了新的模块加载方案。

导出模块的方式

```
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```

引入模块的方式：

```
import {firstName, lastName, year} from './profile';
```

跟 require.js 的执行结果是一致的，也就是将需要使用的模块先加载完再执行代码。

1. 它们有两个重大差异。
2. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
3. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
4. 第二个差异可以从两个项目的打印结果看出，导致这种差别的原因是：
5. 因为 CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

重点解释第一个差异。

CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。



举个例子：

```
// 输出模块 counter.js
var counter = 3;
function incCounter() {
  counter++;
}
module.exports = {
    counter: counter,
    incCounter: incCounter,
};
// 引入模块 main.js
var mod = require('./counter');

console.log(mod.counter);  // 3
mod.incCounter();
console.log(mod.counter); // 3
```

counter.js 模块加载以后，它的内部变化就影响不到输出的 mod.counter 了。这是因为 mod.counter 是一个原始类型的值，会被缓存。

但是如果修改 counter 为一个引用类型的话：

```javascript
// 输出模块 counter.js
var counter = {
    value: 3
};

function incCounter() {
    counter.value++;
}
module.exports = {
    counter: counter,
    incCounter: incCounter,
};
// 引入模块 main.js
var mod = require('./counter.js');

console.log(mod.counter.value); // 3
mod.incCounter();
console.log(mod.counter.value); // 4
```

value 是会发生改变的。不过也可以说这是 "值的拷贝"，只是对于引用类型而言，值指的其实是引用。

而如果我们将这个例子改成 ES6:

```javascript
// counter.js
export let counter = 3;
export function incCounter() {
  counter++;
}

// main.js
import { counter, incCounter } from './counter';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```





这是因为,ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令 import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的 import 有点像 Unix 系统的“符号连接”，原始值变了，import 加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。



### Babel如何编译import语法呢

鉴于浏览器支持度的问题，如果要使用 ES6 的语法，一般都会借助 Babel，可对于 import 和 export 而言，只借助 Babel 就可以吗？

让我们看看 Babel 是怎么编译 import 和 export 语法的。

```javascript
// ES6
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
// Babel 编译后
'use strict';

Object.defineProperty(exports, "__esModule", {
  value: true
});
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

exports.firstName = firstName;
exports.lastName = lastName;
exports.year = year;
```

是不是感觉有那么一点奇怪？编译后的语法更像是 CommonJS 规范，再看 import 的编译结果：

```javascript
// ES6
import {firstName, lastName, year} from './profile';
// Babel 编译后
'use strict';

var _profile = require('./profile');
```

你会发现 Babel 只是把 ES6 模块语法转为 CommonJS 模块语法，然而浏览器是不支持这种模块语法的，所以直接跑在浏览器会报错的，如果想要在浏览器中运行，还是需要使用打包工具将代码打包。

### webpack的打包

Babel 将 ES6 模块转为 CommonJS 后， webpack 又是怎么做的打包的呢？它该如何将这些文件打包在一起，从而能保证正确的处理依赖，以及能在浏览器中运行呢？

首先为什么浏览器中不支持 CommonJS 语法呢？

这是因为浏览器环境中并没有 module、 exports、 require 等环境变量。

换句话说，webpack 打包后的文件之所以在浏览器中能运行，就是靠模拟了这些变量的行为。

那怎么模拟呢？

我们以 CommonJS 项目中的 square.js 为例，它依赖了 multiply 模块：

```javascript
console.log('加载了 square 模块')

var multiply = require('./multiply.js');


var square = function(num) {　
    return multiply.multiply(num, num);
};

module.exports.square = square;
```

webpack 会将其包裹一层，注入这些变量：

```javascript
function(module, exports, require) {
    console.log('加载了 square 模块');

    var multiply = require("./multiply");
    module.exports = {
        square: function(num) {
            return multiply.multiply(num, num);
        }
    };
}
```

这样每个模块文件之间都进行了作用域隔离

那 webpack 又会将 CommonJS 项目的代码打包成什么样呢？我写了一个精简的例子，你可以直接复制到浏览器中查看效果：

```javascript
// 自执行函数
(function(modules) {

    // 用于储存已经加载过的模块
    var installedModules = {};

    function require(moduleName) {

        if (installedModules[moduleName]) {
            return installedModules[moduleName].exports;
        }

        var module = installedModules[moduleName] = {
            exports: {}
        };

        modules[moduleName](module, module.exports, require);

        return module.exports;
    }

    // 加载主模块
    return require("main");

})({
    "main": function(module, exports, require) {

        var addModule = require("./add");
        console.log(addModule.add(1, 1))

        var squareModule = require("./square");
        console.log(squareModule.square(3));

    },
    "./add": function(module, exports, require) {
        console.log('加载了 add 模块');

        module.exports = {
            add: function(x, y) {
                return x + y;
            }
        };
    },
    "./square": function(module, exports, require) {
        console.log('加载了 square 模块');

        var multiply = require("./multiply");
        module.exports = {
            square: function(num) {
                return multiply.multiply(num, num);
            }
        };
    },

    "./multiply": function(module, exports, require) {
        console.log('加载了 multiply 模块');

        module.exports = {
            multiply: function(x, y) {
                return x * y;
            }
        };
    }
})
```

最终的执行结果为：

```javascript
加载了 add 模块
2
加载了 square 模块
加载了 multiply 模块
9
```