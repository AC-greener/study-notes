#### node基础

node的版本号是按照semVer编制的（6.9.1，主要、次要、补丁版本号）

node可以做什么：

1. web程序
2. 命令行工具和后台程序（后台服务如PM2）
3. 桌面程序

##### node模块系统

模块的导出：

1. 只返回一个函数或变量，使用module.exports

   ```javascript
   function add(){}
   module.exports = add
   ```

2. 返回多个函数或变量，使用exports

   ```javascript
   function odd(){}
   const a = 100
   exports.a = a
   exports.add = add
   ```

既有exports也有module.exports，exports会被忽

模块的本质：

​	最终程序导出的是module.exports，exports只是对module.exports的一个全局引用，被定义为一个可以添加属性的空对象。

exports.myfunc知识module.exports.myfunc的简写

模块的缓存：

​	node可以把某块作为对象缓存起来，如果程序中的两个文件引入了相同的模块，第一个require会把模块中返回的数据存到内存中，第二个require就不再去访问模块的源文件了

模块的查找规则：

##### node异步编程

流程控制：

- 串行流程控制
- 并行流程控制

libuv负责处理I/O

##### node核心模块

###### 文件系统

fs

path

###### 网络

net

http

https

dns

###### 其他：

events：事件模块

1. EventEmitter，一个可以继承、能够添加事件发射的类，node中很多核心功能都继承自它

os：查询平台信息

assert：测试断言库

##### node web开发

resutfulAPI

1. POST /articles 创建新文章
2. GET /articles/:id 获取指定文章
3. DELETE /articles/:id 删除指定文章
4. GET /articles 获取所有文章