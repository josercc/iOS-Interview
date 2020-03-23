
# class 和 struct 的区别
<details>
  <summary>查看答案</summary>
  
  `Class`是引用类型，给其他变量复制只是复制指针。`Struct`是值类型，给其他变量赋值是值的拷贝。`Class`可以继承，`Struct`不支持继承。`Class`每个成员变量都需要初始化，`Struct`不需要对每个成员变量初始化。
</details>

# 不通过继承，代码复用（共享）的方式有哪些
<details>
  <summary>查看答案</summary>
  
  - 公共函数
  - 协议
  - 扩展
</details>

# 实现一个 min 函数，返回两个元素较小的元素
<details>
  <summary>查看答案</summary>
  
  ```objc
  func min<T:Comparable>(_ left:T, _ right:T) -> T {
    return left > right ? right : left
  }
  ```
</details>

# map、filter、reduce 的作用
<details>
<summary>查看答案</summary>
  
- map可以通过闭包将元素转换称其他元素
> 比如将一组数组转换成字符串
```objc
let numbers:[Int] = [1,2,3,4,5]
let strings:[String]? = try? numbers.map{"\($0)"}
```
- filter可以将元素过滤组成另外的集合
> 比如将一组数字过滤掉小于3的
```objc
let numbers:[Int] = [1,2,3,4,5]
let filters = try? numbers.filter{$0<3}
```
- reduce是将数组合并称一个元素
> 算出一组数字的和
```objc
let numbers:[Int] = [1,2,3,4,5]
let reduce = try? numbers.reduce(0){$0+$1}
```
</details>

# map 与 flatmap 的区别
<details>
<summary>查看答案</summary>
  
`flatmap`可以将集合的空值过滤。
> ⚠️最新`flatmap`已经废弃，替换称`compactMap`
```swift
let numbers:[Int] = [1,2,3,4,5]
let maps = numbers.map{$0<3 ? nil:"\($0)"}
let flatmaps = numbers.compactMap{$0<3 ? nil:"\($0)"}
print(maps) // [nil, nil, Optional("3"), Optional("4"), Optional("5")]
print(flatmaps) // ["3", "4", "5"]
```
</details>

# 什么是 copy on write
<details>
<summary>查看答案</summary>
  
开始只复制引用类型，到真正赋值才真正的值拷贝。
</details>

# guard 使用场景
<details>
<summary>查看答案</summary>
  
- 代码的黄金大道
- 对可选类型进行解包
</details>

# defer 使用场景
<details>
<summary>查看答案</summary>
  
在当前作用域退出之前执行，一般用于清理资源，或者返回值逻辑容错。
</details>
