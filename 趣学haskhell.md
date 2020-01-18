haskhell

1. 函数式程序语言的一般思路：先取一个初始的集合并将其变形，执行过滤条件，最终取得正确的结果。

#### 3，Types and Typeclasses

##### haskhell的类型

1. :**t命令**后面跟任何可用的表达式就能得到该表达式的类型

2. 类型都是以大写字母开头

   ```haskell
   :t 'a'
   'a' :: char
   
   :t True
   True :: Bool
   
   :t (1, "hello")
   (1, "hello") :: Num t => (t, [char])
   
   :t ('a', 'b', 'c')
   ('a', 'b', 'c') :: (char, char, char)
   ```

3. 给函数加上类型，参数和返回值之间由 -> 分隔，返回值是最后一项1、

   ```haskell
   removeNonUppercase :: [Char] -> [Char]
   addThree :: Int -> Int -> Int -> Int
   ```

4. 几个常见的类型

   1. Int 整数，有上下限
   2. Integer，是无界的
   3. Float 单精度
   4. Double 双精度
   5. Bool 布尔值，只有True或False
   6. Char 表示一个字符
   7. Tuble 元组，取决于及其中项的类型

##### Type variables 类型变量

```haskell
:t head
head :: [a] -> a
```

其中a可以是任意的类型，head 函数的类型声明里标明了它可以取任意类型的 List 并返回其中的第一个元素

```haskell
:t fst
fat :: (a, b) -> a
```

 fst 取一个包含两个类型的 Tuple 作参数，并以第一个项的类型作为返回值。注意，a 和 b 是不同的类型变量，但它们不一定非得是不同的类型，它只是标明了首项的类型与返回值的类型相同。

##### Typeclasses

类型集，可以理解为接口

```haskell
ghci> :t (==)
(==) :: (Eq a) => a -> a -> Bool
```

1. =>左边的部分叫类型约束，可以这样阅读这段类型声明：“相等函数取两个相同类型的值作为参数并返回一个布林值，而这两个参数的类型同在 Eq 类之中 (即类型约束)

2. Eq 这一 Typeclass 提供了判断相等性的界面，凡是可比较相等性的类型必属于 Eq class。

3. Ord包含可比较大小的类型。除了函数以外，我们目前所谈到的所有类型都属于 Ord 类。compare 函数取两个 Ord 类中的相同类型的值作参数，返回比较的结果。这个结果是如下三种类型之一：GT, LT, EQ。

   ```haskell
   ghci> "Abrakadabra" < "Zebra"
   True
   ghci> "Abrakadabra" `compare` "Zebra"
   LT
   ```

4. Show 的成员为可用字符串表示的类型。目前为止，除函数以外的所有类型都是 Show 的成员。操作 Show Typeclass，最常用的函数表示 show。它可以取任一 Show 的成员类型并将其转为字符串。

   ```haskell
   ghci> show 3
   "3"
   ghci> show 5.334
   "5.334"
   ghci> show True
   "True"
   ```

   

5. Read 是与 Show 相反的 Typeclass。read 函数可以将一个字符串转为Read 的某成员类型。

   ```haskell
   ghci> read "5" :: Int
   5
   ghci> read "5" :: Float
   5.0
   ghci> (read "5" :: Float) * 4
   20.0
   ghci> read "[1,2,3,4]" :: [Int]
   [1,2,3,4]
   ghci> read "(3, 'a')" :: (Int, Char)
   (3, 'a')
   ```

6. Enum 的成员都是连续的类型 – 也就是可枚举。

7. Bounded 的成员都有一个上限和下限。

8. Num 是表示数字的 Typeclass，它的成员类型都具有数字的特征。Num 包含所有的数字：实数和整数。

9. Integral 同样是表示数字的 Typeclass。 Intgral 仅包含整数，其中的成员类型有 Int 和 Integer

10. Floating 仅包含浮点类型：Float 和 Double。

