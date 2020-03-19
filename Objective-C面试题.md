
# #include与#import的区别、#import与@class 的区别
<details>
<summary>查看答案</summary>
  #include和#import都可以引入头文件，但是#import只会引入一次。#import虽然只会引入一次，但是还会导致相互引入的问题。@class可以在头文件引入类，类可以是不存在的，可以在头文件@class引入类，在.m用#import引入类实现从而解决循环引用的问题。
</details>

# 请分别说明@public、@protected、@private的含义与作用
<details>
<summary>查看答案</summary>
  @public代表可以在任何地方都可以被访问，@protected表示只能可以在子类和本类可以访问，@private表示只允许在本类允许访问
</details>

# 请解释self = [super init]方法
<details>
<summary>查看答案</summary>
  因为子类是继承与父类的，如果父类都初始化失败返回nil,那么子类也没有必要的执行下去了。其实是一种容错的处理。
</details>

# 什么情况使用 weak 关键字，相比 assign 有什么不同？
当使用代理 或者声明@IBOutlet和不持有对象的时候可以使用weak关键词。weak是弱引用，只是用指针指向对应对象的内存地址，不会对对象进行引用技术加1。assign会将声明的基本变量放在栈区，交给系统自动管理内存。
