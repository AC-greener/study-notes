#### TS

TypeScript的主要特点如下：

1）免费开源，使用Apache授权协议。

2）基于ECMAScript标准进行拓展，是JavaScript的超集。

3）添加了可选静态类型、类和模块。

4）可以编译为可读的、符合ECMAScript规范的JavaScript。

5）成为一款跨平台的工具，支持所有的浏览器、主机和操作系统。

6）保证可以与JavaScript代码一起运行，无须修改。（这一点保证了JavaScript项目可以向TypeScript平滑迁移。）

7）文件拓展名是ts。

8）编译时检查，不污染运行时。



#### 类型系统

定义数组：

1. ```typescript
   let a: number[] = [1,2,3] 
   ```

2. ```typescript
   let a: Array<number> = [1,2,3] //使用数组泛型
   ```

断言：

1. ```typescript
   let a:any = 'hello'
   let l:number = (a as string).length
   ```

2. ```typescript
   let a:any = 'hello'
   let length:number = (<string>a).length
   ```

泛型：

1. ```typescript
   function hello<T>(arg: T):T {
   	return arg
   }
   hello('ts')
   hello(123)
   
   function hello2<T>(args: T[]):T[ss] {
   	return args.length
   }
   ```

类型别名：

```
type Age = number
type AgeCreator = () => Age
type Person<T> = { age: T}
```

交叉类型：交叉类型是将多个字典类型合并为一个新的字典类型。在interface中交叉类型取并集

```typescript
interface A {
	d:number
}
interface B {
	f:string
}
type C = A & B
let c: C
```

联合类型：表示一个变量可以是几种类型之一，在在interface中联合类型取交集

```typescript
function hello(arg: string | number) {

}
interface A {
	a: number
	b: string
}
interface B {
	a: number
	c: string
}
interface C {
	a: number
	f: string
}
let obj = A | B | C
```

类型保护机制

索引类型和映射类型

类型推导：在没有明确指出类型的地方，TypeScript编译器会自己去推测出当前变量的类型。

只读属性：可以使用readonly关键字将属性设置为只读的，只读属性必须在声明时或构造函数里进行初始化

```typescript
class Person {
	readonly name: string
	constructor(theName: string) {
		this.name = theName
	}
}
```



##### 接口和类

用接口描述函数类型：

```typescript
interface searchFn {
	(source: string, subString: string): boolean
}
let mySearch: searchFn
mySearch = function(source: string, subString: string){
 let result = source.search(subString)
}
//或者 ts会进行类型推断
mySearch = function(source, subString){
 let result = source.search(subString)
}
```

用接口描述索引类型：

```typescript
interface stringArray {
	[index: number]: string
}
let a: stringArray
a = ['hello', 'world']
let s: string = a[0]
```

表示当用number去索引stringArray时会得到string类型的返回值

**一个接口可以继承多个接口**

#### TS的模块系统