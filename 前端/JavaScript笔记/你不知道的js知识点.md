1. typeof null '返回object'。

   ​	js会吧数据转换成二进制。二进制的前三位都是0的话会被认为是object，null的是全0，所以会被判定为object

   ```javascript
   (!a && typeof a === 'object')
   ```

2. 函数对象的length是其生命的参数个数

   ```javascript
   function a(x, y, z) {}
   a.length === 3
   ```

3. JavaScript异步编程

   - callback带来的问题

     表面上的问题：

     1. 回调地狱带来的的嵌套和缩进

     实质的问题：

     1. 在分析代码的时候，必须在函数之间跳来跳去。和大脑线性的思维方式不一样

     2. 当我们的回调传到第三方库中，会出现控制反转（就是把自己程序的执行权交给第三方）。控制反转带来的问题：

        - 调用回调的次数无法确定
        - 调用回调过早或者过晚
        - 吞掉可能出现的错误或异常

        有了这些问题之后对于这些无法信任的回调，我们不得不创建大量的混乱逻辑去处理这些错误，离回调地狱更近了一步

   - 尝试挽救回调

     1. 分离回调设计方式， ajax（url，success， fail）
     2. error-first风格
     
   - Promise采用分离回调设计，**promise的缺点**
   
     - 错误捕获。 p.then(f1, f2)，如果f1里面出错了，这个错误会被吞掉，最佳实践是对promise链的最后加上一个catch（handleError）。 但是问题又来了，如果handleError里面有又错误该怎么办呢？
     - 单决议。promise只能被决议一次，（完成或拒绝）
     - promise无法取消。一旦创建了promise，就没办法停止