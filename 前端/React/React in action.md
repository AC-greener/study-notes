#### React

![1593427812918](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1593427812918.png)

react的思维模式广泛应用了函数式编程和面向对象的概念

##### 虚拟dom：

是模仿浏览器中的文档对象模型的数据结构

为什么使用虚拟dom？

出现这个问题的部分原因是浏览器处理与DOM交互的方式。当访问、修改或创建DOM元素时，浏览器常常要在一个结构化的树上执行查询来找到指定的元素。这只是访问一个元素，而且通常仅是更新的第一部分。通常情况下，作为更改的一部分，它不得不重新进行布局、缩放和其他操作——所有这些操作往往计算量都很大。虚拟DOM也无法绕过这个问题，但它可以在这些限制下帮助优化对DOM的更新。当创建和管理一个处理随时间变化的数据的大型应用时，可能需要对DOM进行许多更改，这些更改通常会发生冲突或以不太理想的方式完成。

虚拟dom的更新方式：先diff 在patch

思考：

1. React中如何使用图片
2. 如何改变url， React props属性中没有router或者history对象

#### Redux

#### React Router

#### Redux Thunk

#### React单元测试

重点：

1. 不要测试已经连接了store的组件，只需要测试组件本身即可

##### 测试组件

尽量去测试组件生命周期函数的调用，点击事件

```react
import { shallow, mount } from 'enzyme'
import React from  'react'
impport { App } from './App'

describe('APP test Suits', () => {
	it('render app', () => {
		const fetchList = jest.fn()
		const wrapper = shallow(<App fetchlist={fetchlist} />)
		wrapper.render()
	})
    
    it('fetchList should be call', () => {
		const fetchList = jest.fn()
		mount(<App fetchlist={fetchlist} />)
		expect(fetchList.mock.call.length).toBe(1)
	})
})
```

##### 测试Action

![image-20200801123226155](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801123226155.png)



![image-20200801123321791](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801123321791.png)

##### 测试reducer

![image-20200801123440894](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801123440894.png)