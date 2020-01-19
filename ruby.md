- 获得任何新技能的第一步，是先别想着独立解决什么，而是重复一遍前人已竟之事，这是掌握一门技能最快的方法
- 对几门语言均略知一二，这其实是一项相当实用的技能，因为我常常发现，网上的某个程序有助于解决手头问题，却没法直接拿来使用，还得针对问题稍作调整才行。这程序用什么语言写的都有可能，所以懂点儿Python、Ruby什么的就非常管用。
- 不过本书的高明之处在于，它会带你踏上快速成长的互动之旅。你每周都会遇到一些小型的编程挑战和一个实战项目。解决它们虽非易事，但这既能增长你的见识，还可让你体验编程之乐
- 如果你阅读本书时不做任何习题，那不过是对语法有了个粗浅认识。如果你在尝试独立解答习题之前，先去网上搜索答案，那也一样意味着不及格。你首先要有试着解答习题的主观愿望，同时也要充分认识到，有一小部分习题可能超出了你的能力范围。要知道，学会语法永远比学思考简单
- 语言的类型模型是什么
  - 强类型
  - 弱类型
  - 静态
    静态类型系统的情况下，编译器和工具能捕获更多错误
  - 动态
    直到真正尝试执行代码时，Ruby才进行类型检查。这一概念称做动态类型。
- 语言的编程范型是什么？
  - 面向对象
  - 函数式
  - 过程式
  - 几种范型的组合式
- Ruby
  - 是一种解释型、面向对象、动态类型的语言解释型，意味着Ruby代码由解释器而非编译器执行。动态类型，意味着类型在运行时而非编译时绑定。
  
  - 进入命令行：irb
  
  - ruby中一切皆为对象，就连每个单独的数字也不例外
  
  - 除了false和nil，其他值都代表true
  
  - 鸭子类型（duck typing）
    - 数组的第一个元素是String ，第二个元素是 Float ，而把它们转换成整数的代码，却都用的是 to_i 。
    - 鸭子类型并不在乎其内在类型可能是什么。只要它像鸭子一样走路，像鸭子一样嘎嘎叫，那它就是只鸭子
    
  - 面向对象语言利用继承，将行为传播到相似的对象上。而Ruby采用的是模块。模块是函数和常量的集合。
  
  - ruby中的四种变量
  
    - 局部变量
      以英文字母或者 _ 开头。
    - 全局变量
      以 $ 开头。
    - 实例变量
      以 @ 开头。
    - 类变量
      以 @@ 开头。
  
  - 一些规则：
  
    - 用于逻辑测试的函数和方法一般要加上问号如 if test?
    - 类应以大写字母开头，并且一般采用骆驼命名法，如 CamelCase 。
    - 实例变量和方法名以小写字母开头，并采用下划线命名法，如underscore_style 。
    - attr 关键字可用来定义实例变量
      - attr 定义实例变量和访问变量的同名方法
      - 而 attr_accessor 定义实例变量、访问方法和设置方法
  
  - ruby的方法
  
    - each 方法、loop 方法，方法本身可以与伴随的块一起被调用。这种与块一起被调用的方法，我们称之为带块的方法。
    - 带块的方法可是使用do/end或者{}
    - 实例方法
    - 类方法
    - 函数方法
  
  - ruby的基础语法
  
    ```ruby
    puts('hello world')
    puts 'hello world'
    => hello world
    #调用方法时，ruby可以省略()
    
    # 输出变量
    language = 'ruby'
    puts "hello #{language}"
    => hello ruby
    
    # 多重赋值
    a, b, c = 1, 2, 3
    puts a, b, c
    => 1 2 3
    a, b = 0, 1
    a, b = b, a 
    puts a, b
    => 1, 0
    
    # 单引号和双引号的区别
    puts 'Hello, \nRuby\n!\n'
    => Hello, \nRuby\n!\n
    puts "Hello, \nRuby\n!\n"
    => hello,
    => Ruby
    => !
    
    
    # 试试其他两种输出方法
    print 'hello world'
    p 'hello world'
    
    ```
  
  - ruby中的条件判断和循环
  
    ```ruby
    x = 4
    if x == 4
        puts 'hello'
    end
    
    # 上面if语句可简化为：
    puts 'hello' if x == 4
    
    # 这三条语句都不会输出任何值, unless语句和if语句用法相反
    puts 'i have a dog ' if not true
    puts 'i have a dog ' if !true
    puts 'i have a dog ' unless true
    
    # while 和 until语句
    x = 0
    while x < 10
        x += 1
    end
    
    x = 0
    until x > 9
        x += 1
    end
    #可以简化为下面两条语句之一：
    x = x + 1 while x < 10
    x = x + 1 until x > 9
    
    # for 语句
    sum = 0
    for i in 1..5
      sum = sum + i
    end
    puts sum
    => 55
    
    languages = ['c', 'c++', 'ruby', 'i love Javascript']
    for language in languages
      puts language
    end
    
    #如果循环的的次数确定，可以使用times方法
    100.times do
      puts 'hello world'
    end
    => 输出100行hello world
      
    ```
  
  - ruby中的数组
  
    ```ruby
    animals = ['dog', 'cat', 'tiger']
    
    puts animals
    => dog cat tiger
    
    animals[0]
    => dog
    
    animals[-1]
    => tiger
    
    animals[0..1]
    => dog cat
    
    animals.push('bear')
    => ["dog", "cat", "tiger", "bear"]
    
    animals.size
    => 4
    
    #遍历数组
    animals.each {|animal|  puts animal}
    => dog cat tiger bear
    ```
  
  - ruby中散列表（hash）
  
    ```ruby
    numbers = {1 => 'one', 2 => 'two'}
    numbers[1]
    => one
    
    # 符号（ symbol ）与字符串很相似，以:开始，通常用作hash的键（key）
    person = {:age=> 18, :name=> 'zhazhahui'}
    person[:name]
    => zhazhahui
    
    #将符号当作键时，还可以这样写：
    person = {age: 18, name: 'zhazhahui'}
    person[:name]
    => zhazhahui
    
    # 散列的遍历
    person.each {|key, value| puts "key is: #{key}, value is: #{value}"}
    =>key is: age, value is: 18
    =>key is: name, value is: zhazhahui
    ```
  
  - ruby中的代码块 
  
    - 单行使用{}
    - 多行使用do/end
  
    ```ruby
    animals = ['dog', 'cat', 'tiger']
    animals.each {|animal| puts "i have a #{animal}"}
    => i have a dog 
    => i have a cat
    => i have a tiger
    
    animals.each do |animal|
      puts "i have a #{animal}"
    end
    => i have a dog 
    => i have a cat
    => i have a tiger
    ```
  
    
  
  - ruby中的函数
  
    - 如果没有显示的指定返回值，ruby会返回退出函数前最后处理的表达式的值
  
    ```ruby
    def hello(name)
    	puts "hello, #{name}"
    end
    hello('zhazhahui')
    => zhazhahui
    
    # 无参数的函数
    def hello 
      name = 'zhangsan'
    end
    hello
    => zhangsan
    # 注意：如果没有显示的指定返回值，ruby会返回退出函数前最后处理的表达式的值，这里例子中会返回zhangsan
    
    # 函数接收代码块
    def hello(&block)
    	block.call
    end
    hello {puts 'hello, my name is zhazhahui'}
    =>  my name is zhazhahui
    
    ```
  
  - ruby中的模块
  
    ```ruby
    
    ```
  
    
  
  - ruby中的类
  
    - 类的定义：
      - 以 @@ 开始的是类变量，类变量是在类的所有实例中共享的变量，可以被所有的对象实例访问
      - 以 @ 开始的是实例变量
      
    - 写一个类
    
  ```ruby
    class Box
        
       # 定义实例类变量
       attr_accessor :width, :height
       # 构造器方法
       def initialize(w,h)
          @width, @height = w, h
       end
    
       def getArea
          getWidth * getHeight
       end
    
       def getWidth
          @width
       end
        
       def getHeight
          @height
       end
        
       # 让这两个方法变成私有的
       private :getWidth, :getHeight
    
    end
    
    # 创建对象
    box = Box.new(10, 20)
    
    box.getArea()
    
    ```
    
  pipelineId=5c7514a78af4e06d19179b1c&experimentId=5c7514a78af4e06d19179b13&dataType=IMAGE_CLASSIFICATION&runId=5c7514a78af4e06d19179b1c&modelId=5c7514a8113f5b065e0957e9&tab=info
    
  - pipelineId=5df8d58f8af4e000a983f65a&experimentId=5df8d58f8af4e000a983f64d&dataType=IMAGE_CLASSIFICATION&runId=5df8d58f8af4e000a983f65b&modelId=5df8d5987cf3240100b2d77c&tab=info

没有problem_type