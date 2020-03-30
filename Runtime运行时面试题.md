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

# 输出结果是啥，会不会崩溃?

```objc
@interface MNPerson : NSObject
@property (nonatomic, copy)NSString *name;
- (void)print;
@end

@implementation MNPerson
- (void)print{
    NSLog(@"self.name = %@",self.name);
}
@end
---------------------------------------------------
@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    id cls = [MNPerson class];
    void *obj = &cls;
    [(__bridge id)obj print];
}
@end
```

<details>
<summary>查看答案</summary>

 输出结果为`self.name = <ViewController: 0x7fe667608ae0>`，不会崩溃。 

</details>

# Runtime的功能

<details>
<summary>查看答案</summary>

- 发送消息
- 交换方法
- 动态添加方法
- 关联属性
- 字典转模型

</details>

# class_ro_t 和 class_rw_t 的区别？

<details>
<summary>查看答案</summary>

  `class_ro_t`

> 包含了编译时期确定的协议列表`protocols`，方法列表`method_list`，属性列表`propertys`。

`class_rw_t`

> 包含了`class_ro_t`还包含编译和运行时全部的协议列表`protocols`，属性列表`propertys`，方法列表`mentods`。

</details>

# Category 的实现原理？

<details>
<summary>查看答案</summary>

 `Category`其实是一个`Category_t`的结构体，在运行时通过倒序的方法添加到原方法列表。所以分类的方法优先于原类的方法，分类的方法优先级取决于编译顺序。

</details>

# runtime 如何实现 weak 属性？

<details>
<summary>查看答案</summary>

  其实系统的内部维护着一个`weak`的`hashMap`,通过内存地址作为`key`，`weak`指针作为`value`，当对象的引用计数为0就会释放。

- 初始化时候，调用`objc_initWeak`创建一个`weak`指针指向对象内存地址
- 添加引用调用`objc_storeWeak() `。
- 当释放时候，会通过对象内存地址便利`weak`的`hashMap`，释放所有该对象的`weak`指针。

</details>

# class_copyPropertyList与class_copyIvarList区别

<details>
<summary>查看答案</summary>

- `class_copyPropertyList`只返回了对象`@property`声明的属性
- `class_copyIvarList`不仅返回对象`@property`的属性还包括了对象的实例变量。

</details>
