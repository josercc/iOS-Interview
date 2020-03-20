
# class 和 struct 的区别
<details>
  <summary>查看答案</summary>
  
  `Class`是引用类型，给其他变量复制只是复制指针。`Struct`是值类型，给其他变量赋值是值的拷贝。`Class`可以继承，`Struct`不支持继承。`Class`每个成员变量都需要初始化，`Struct`不需要对每个成员变量初始化。
<details>

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
