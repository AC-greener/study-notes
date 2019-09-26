#### Canvas

1. 当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。默认以canvas的左上角为原点

2. 替换内容：可以在<canvas>标签中提供了替换内容。不支持<canvas>的浏览器将会忽略容器并在其中渲染后备内容。而支持<canvas>的浏览器将会忽略在容器中包含的内容，并且只是正常渲染canvas。

3. canvas提供了三种方法绘制矩形：

   [`fillRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillRect)绘制一个填充的矩形

   [`strokeRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeRect)绘制一个矩形的边框

   [`clearRect(x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/clearRect)清除指定矩形区域，让清除部分完全透明。

4. 绘制路径

   ```
   beginPath()
   ```

   新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。

   ```
   closePath()
   ```

   闭合路径之后图形绘制命令又重新指向到上下文中。

   ```
   stroke()
   ```

   通过线条来绘制图形轮廓。

   ```
   fill()
   ```

   通过填充路径的内容区域生成实心的图形。

5. 移动触笔

   ```javascript
   moveTo(x, y)
   ```

   将笔触移动到指定的坐标x以及y上。

6. 绘制直线，需要用到的方法`lineTo()`。

   [`lineTo(x, y)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineTo)

   绘制一条从当前位置到指定x以及y位置的直线

7. 路径使用填充（fill）时，路径自动闭合，使用描边（stroke）则不会闭合路径。如果没有添加闭合路径`closePath()`到描述三角形函数中，则只绘制了两条线段，并不是一个完整的三角形。

8. 绘制圆弧

   [`arc(x, y, radius, startAngle, endAngle, anticlockwise)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/arc)

   画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。

9. 给图形上色

   [`fillStyle = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillStyle)

   设置图形的填充颜色。

   [`strokeStyle = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeStyle)

   设置图形轮廓的颜色。

10. 设置线型

    [`lineWidth = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineWidth)

    设置线条宽度。

    [`lineCap = type`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineCap)

    设置线条末端样式。
    
11. 绘制文本

    `fillText(text, x, y [, maxWidth\])`

    在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.

    `strokeText(text, x, y [, maxWidth\])`

    在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.