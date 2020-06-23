#### 一、函数式编程简介

##### 函数与方法

函数式一段可以通过其名称被调用的代码

方法是一段必须通过其名称及其关联对象的名称被调用的代码

```javascript
var bar = (a) => {return a}
bar(1)
```

```javascript
var obj = {
	bar: (a) => {return a}
}
obj.bar(1)
```

##### 函数的引用透明性

所有的函数对于相同的输入都会返回相同的值

可以用值替换函数

```javascript
4 = double(2)
Math.max(1,2,3,4) = 5
```



##### 函数式编程主张声明式编程和编写抽象的代码

命令式遍历数组

声明式遍历数组

**纯函数**

纯函数产生可测试的代码

给定输入就会返回相同输出的函数，遵循引用透明性

不应该依赖外部变量，也不应该改变外部变量

##### 管道与组合

纯函数应该只做一件事情

举一个linux命令的例子：我们想要在一个文件中找到一个字符串并统计它的出现次数：

```shell
cat jsBook | grep –i “composing” | wc
```

上面的命令通过组合多个函数解决了这个问题，组合不是linux命令独有的，但他是函数式编程的核心，也叫作函数式组合（Funtional Composition）

#### 二、JavaScript函数基础

#### 三、高阶函数

高阶函数是接收函数作为参数并且/或者返回函数作为输出的函数（Higher-Order functions HOC）



#### 四、闭包和高阶函数

```javascript
var fn = (arg) => {
  let outer = "Visible"
  let innerFn = () => {
    console.log(outer)
    console.log(arg)
  }
	return innerFn
}
var closureFn = fn(5)
```

当innerFn被return的时候为他设置作用域， 返回函数的引用被存储在closureFn里面

#### 五、数组API

#### 六、柯里化和偏应用

柯里化：吧一个多参数函数转换成一个嵌套的一元函数的过程

处理两个参数的柯里化函数

```javascript
const add = (x,y) => x + y;
const curry = (binaryFn) => {
  return function (firstArg) {
    return function (secondArg) {
    	return binaryFn(firstArg, secondArg);
    };
  };
};
let autoCurriedAdd = curry(add)
autoCurriedAdd(2)(2)
=> 4
```

为什么需要柯里化呢？

柯里化实战：在数组中寻找数字

```javascript
let match = curry(function(expr, str) {
	return str.match(expr);
});

let hasNumber = match(/[0-9]+/)

let filter = curry(function(f, ary) {
	return ary.filter(f);
});

let findNumbersInArray = filter(hasNumber)	

findNumbersInArray(["js","number1"])

=> ["number1"]
```

Partial Application  偏应用，颠倒参数的传递顺序

将回调函数和时间参数调换

```javascript
setTimeout(() => console.log("Do X task"),10);
setTimeout(() => console.log("Do Y task"),10);
//柯里化之后
const setTimeoutWrapper = (time,fn) => {
setTimeout(fn,time);
}
const delayTenMs = curry(setTimeoutWrapper)(10)
delayTenMs(() => console.log("Do X task"))
delayTenMs(() => console.log("Do Y task"))
```

偏函数定义：

```javascript
const partial = function (fn,...partialArgs){
  let args = partialArgs;
    return function(...fullArguments) {
      let arg = 0;
      for (let i = 0; i < args.length && arg < fullArguments.length; i++) {
        if (args[i] === undefined) {
          args[i] = fullArguments[arg++];
        }
    	}
    	return fn.apply(null, args);
  };
};
```

#### 七、组合（函数组合）和管道（Composition and Pipelines）

UNIX哲学：

1. 每个程序只做好一件事情，如果要添加新功能，请重新构建而不是在复杂的旧程序中添加新功能
2. 每个程序的输出应该是另一个尚未可知程序的输入

接收两个参数的compose函数的实现：

```javascript
const compose = (a, b) => (c) => a(b(c))
```

接收n个参数：

```javascript
const compose = (...fns) => (value) =>  reduce(fns.reverse(), (accumulator, fn) => fn(accumulator), value) 

```

#### 八、函子Functor

函子是一个普通对象，它实现了map函数，用map操作对象值的时候生成一个新对象

换句话说：函子是一个实现了map方法的对象

用Maybe和Either函子处理错误

Maybe函子

```javascript
const MayBe = function(val) {
	this.value = val
}
MayBe.of = function(val) {
	return new MayBe(val)
}
MayBe.prototype.isNothing = function() {
	return (this.value === null || this.value === undefined);
}
MayBe.prototype.map = function(fn) {
	return this.isNothing() ? MayBe.of(null) : MayBe.of(fn(this.value));
}
```

```javascript
MayBe.of("George")
.map((x) => x.toUpperCase())
.map((x) => "Mr. " + x)
//MayBe { value: 'Mr. GEORGE' }

MayBe.of("George")
.map(() => undefined)
.map((x) => "Mr. " + x)
//MayBe { value: null }
```



```javascript
let getTopTenSubRedditData = (type) => {
  let response = getTopTenSubRedditPosts(type);
  return MayBe.of(response).map((arr) => arr['data'])
    .map((arr) => arr['children'])
    .map((arr) => arrayUtils.map(arr,
      (x) => {
        return {
          title : x['data'].title,
          url : x['data'].url
        }
    	}
  ))
}
```

不足：不知道map函数在哪一步出错了

Either函子///

```javascript
const Nothing = function(val) {
	this.value = val;
};
Nothing.of = function(val) {
	return new Nothing(val);
};
Nothing.prototype.map = function(f) {
	return this;
};
const Some = function(val) {
	this.value = val;
};
Some.of = function(val) {
	return new Some(val);
};
Some.prototype.map = function(fn) {
	return Some.of(fn(this.value));
}
const Either = {
  Some : Some,
  Nothing: Nothing
}
let getTopTenSubRedditPostsEither = (type) => {
  let response
  //对成功和失败用不同的函数处理，就可以捕获错误的信息
  try{
    response = Some.of(JSON.parse(request('GET',"https://www.reddit.
    com/r/subreddits/" + type + ".json?limit=10").getBody('utf8')))
  }catch(err) {
    response = Nothing.of({ message: "Something went wrong" , errorCode:
    err['statusCode'] })
  }
  return response
}
let getTopTenSubRedditDataEither = (type) => {
  let response = getTopTenSubRedditPostsEither(type);
  return response.map((arr) => arr['data'])
    .map((arr) => arr['children'])
    .map((arr) => arrayUtils.map(arr,
    (x) => {
    return {
      title : x['data'].title,
      url : x['data'].url
    }
  }
  ))
}
getTopTenSubRedditDataEither('wrong')
Nothing { value: { message: 'Something went wrong', errorCode: 404 } }
```

#### 九、单子Monad（也是一种函子）

```javascript
const MayBe = function(val) {
	this.value = val
}
MayBe.of = function(val) {
	return new MayBe(val)
}
MayBe.prototype.isNothing = function() {
	return (this.value === null || this.value === undefined);
}
MayBe.prototype.map = function(fn) {
	return this.isNothing() ? MayBe.of(null) : MayBe.of(fn(this.value));
}
MayBe.prototype.join = function() {
	return this.isNothing() ? MayBe.of(null) : this.value;
}
MayBe.prototype.chain = function(f){
	return this.map(f).join()
}
```

通过join返回maybe对象里面的value值，脱掉了Maybe对象的外衣

​	Monad is a functor that has a chain method!

MayBe with only of and map is a Functor. a functor with chain is a Monad!

#### 十、异步编程Generator

##### Generator基础

创建一个G

```javascript
function* gen() {
	return 'first generator';
}
```

使用它：

```javascript
let generatorResult = gen()
console.log(generatorResult)
gen {[[GeneratorStatus]]: "suspended", [[GeneratorReceiver]]: Window}
```

那么如何得到里面的value呢？可以使用next()方法

```javascript
generator.next().value
=> 'first generator'
```

如果我们调用两次next方法呢？

```javascript
let generatorResult = gen()
//第一次
generatorResult.next().value
=> 'first generator'
//第二次
generatorResult.next().value
=> undefined
```

###### yield关键字

```javascript
function* generatorSequence() {
  yield 'first';
  yield 'second';
  yield 'third';
}
```

```javascript
let generatorSequence = generatorSequence();
generatorSequence.next().value
=> first
generatorSequence.next().value
=> second
```



带有yield关键字的G都会以惰性求值的方式进行，也就是说，代码直到调用的时候才会执行

![1592808120993](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592808120993.png)

###### done Property of Generator

G里面有很多只，我们如何知道何时停止调用next方法呢？

每次对next函数的调用都会返回一个对象

```javascript
{value: 'value', done: false}
```

done是一个可以判断G序列是否被完全消费的属性

```javascript
//get generator instance variable
let generatorSequenceResult = generatorSequence();
console.log('done value for the first time',generatorSequenceResult.next())
console.log('done value for the second time',generatorSequenceResult.next())
console.log('done value for the third time',generatorSequenceResult.next())

done value for the first time { value: 'first', done: false }
done value for the second time { value: 'second', done: false }
done value for the third time { value: 'third', done: false }

console.log(generatorSequenceResult.next())
=> { value: undefined, done: true }
```

![1592808548006](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592808548006.png)

用循环遍历G

```javascript
for(let value of generatorSequence())
console.log("for of value of generatorSequence is",value)
which is going to print:
for of value of generatorSequence is first
for of value of generatorSequence is second
for of value of generatorSequence is third
```

###### Passing Data to Generators ***

```javascript
function* sayFullName() {
  var firstName = yield;
  var secondName = yield;
  console.log(firstName + secondName);
}
let fullName = sayFullName()
fullName.next() //代码会暂停在这一行var firstName = yield;
fullName.next('anto') //yield会被auto取代，并赋值给firstname
// yield 会暂停在 var secondName = yield 这一步
fullName.next('aravinth')
=> anto aravinth
```

![1592811054474](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592811054474.png)



##### 使用G处理异步的代码

###### 使用setTimeout的例子

```javascript
let generator;
let getDataOne = () => {
  setTimeout(function(){
  //call the generator and
  //pass data via next
    console.log(333)
  generator.next('dummy data one')
  }, 1000);
}
let getDataTwo = () => {
  setTimeout(function(){
  //call the generator and
  //pass data via next
  generator.next('dummy data two')
  }, 1000);
}
function* main() {
  console.log('1111')
  let dataOne = yield getDataOne();
  console.log('2222')
  let dataTwo = yield getDataTwo();
  console.log("data one",dataOne)
  console.log("data two",dataTwo)
  console.log('4444')
}
generator = main()
generator.next()
```

```javascript
generator = main()
generator.next()
```

调用main函数，并遇到第一个yield语句，进入了暂停模式，但在暂停之前，他调用了getDataOne函数，

一秒钟之后setTimeout的回调会执行generator.next('dummy data one')，yield语句将会返回'dummy data one'，dataOneb变成'dummy data one'

然后会执行

let dataTwo = yield getDataTwo();

执行方式和之前一样

![1592818946657](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1592818946657.png)

###### 真实的例子：

```javascript
let https = require('https');
function httpGetAsync(url,callback) {
  return https.get(url,
    function(response) {
      var body = '';
      response.on('data', function(d) {
      	body += d;
      });
      response.on('end', function() {
        let parsed = JSON.parse(body)
        callback(parsed)
      })
    }
  );
}
```

```javascript
httpGetAsync('https://www.reddit.com/r/pics/.json',(data)=> {
	console.log(data)
})
{ 
   modhash: '',
  children:
    [ { kind: 't3', data: [Object] },
    { kind: 't3', data: [Object] },
    { kind: 't3', data: [Object] },
    . . .
    { kind: 't3', data: [Object] } ],
  after: 't3_5bzyli',
  before: null 
}
```

```javascript
httpGetAsync('https://www.reddit.com/r/pics/.json',(picJson)=> {
  httpGetAsync(picJson.data.children[0].data.url+".
    json",(firstPicRedditData) => {
    console.log(firstPicRedditData)
  })
})
```

造成了回调地狱，用G来改写

```javascript
let generator 
function request(url) {
  httpGetAsync( url, function(response){
    generator.next( response );
  });
}
```

```javascript
function *main() {
  let picturesJson = yield request( "https://www.reddit.com/r/pics/.json" );
  let firstPictureData = yield request(picturesJson.data.children[0].data.
  url+".json")
  console.log(firstPictureData)
}
generator = main()
generator.next()
```

