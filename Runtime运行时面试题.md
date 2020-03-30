# 解释一下isa指针

<details>
<summary>查看答案</summary>

 `isa`其实指向一个类的结构体，类结构体包含类的方法列表，属性列表，协议列表等。实例对象`isa`指向类对象，类对象的`isa`指向父类对象，父类对象的`isa` 指向根类对象，根类对象`isa`指向自己。

</details>

# SEL 与 IMP

<details>
<summary>查看答案</summary>

 `SEL`指代的方法的名称，`IMP`指代方法的具体实现。 

</details>

# 什么是runtime

<details>
<summary>查看答案</summary>

 `Runtime`是`OC`的运行时机制，主要时消息转发，对于`OC`来说，只有在运行时才能知道调用的函数。 

</details>

## 使用runtime Associate方法关联的对象，需要在主对象dealloc的时候释放么？

<details>
<summary>查看答案</summary>

 不管是在`ARC`还是`MRC`中关联的对象都不需要在主对象`delloc`时候释放，因为关联的对象释放的比较晚，会在`NSObject`调用`dealloc`方法中进行释放。 

</details>
