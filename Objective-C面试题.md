
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
<details>
  <summary>查看答案</summary>
  当使用代理 或者声明@IBOutlet和不持有对象的时候可以使用weak关键词。weak是弱引用，只是用指针指向对应对象的内存地址，不会对对象进行引用技术加1。assign会将声明的基本变量放在栈区，交给系统自动管理内存。
</details>

# @protocol 和 category 中如何使用 @property
<details>
  <summary>查看答案</summary>
	@protocol中的@property和@category中的@property其实都是包含了set和get方法。对于@protocol所有实现协议的类都要实现对应@property属性的set和get方法。@category中的要自己实现set和get方法，虽然分类不支持属性，但是我们可以通过关联对象来实现。

  - @protocol

  ```objc
@protocol ProtocolA <NSObject>
@property (nonatomic, assign) int number;
@end

@interface ClassA : NSObject<ProtocolA>

@end

@implementation ClassA {
    int _number;
}

- (void)setNumber:(int)number {
    _number = number;
}

- (int)number {
    return _number;
}

```

```objc
@protocol ProtocolA <NSObject>
@property (nonatomic, assign) int number;
@end

@interface ClassA : NSObject<ProtocolA>

@end

@implementation ClassA
@synthesize number = _number;


@end

@interface AppDelegate ()

@end
```
我们看出来通过中间变量自己实现set和get方法和通过@synthesize关键字让系统自动生成都是可以的，不过还是使用@synthesize让系统自动生成set和get方法比较简单

- @category

```objc
@interface ClassA : NSObject
@end

@implementation ClassA
@end

@interface ClassA (Number)
@property (nonatomic, assign) int number;
@end

@implementation ClassA (Number)

- (int)number {
    return [objc_getAssociatedObject(self,@selector(number)) intValue];
}

- (void)setNumber:(int)number {
    objc_setAssociatedObject(self, @selector(number), @(number), OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

@end
```
我们可以通过`objc_getAssociatedObject`和`objc_setAssociatedObject`两个方法来为分类添加属性。
</details>

# 用@property声明的NSString（或NSArray，NSDictionary）经常使用copy关键字，为什么？如果改用strong关键字，可能造成什么问题？
<details>
  <summary>查看答案</summary>
  我们知道`copy`关键词是复制一份内存地址，用新的指针指向。是属于深拷贝，而用`strong`关键字是通过一个指针指向对象的内存地址，通过内存地址访问对象，是属于浅拷贝。对于`strong`声明的字符串 数组字典其他地方可以通过地址直接修改对象。
</details>

# 这个写法会出什么问题： @property (copy) NSMutableArray *array
<details>
<summary>查看答案</summary>
通过`copy`修饰的NSMutableArry对象变成了NSArray，这样在编译类型是NSMutableArray，在运行时是NSArray，这样我们在编译时候调用NSMutableArray方法是不报错的，但是在运行时候类型不一样从而导致崩溃发生。
</details>

# 如何为 Class 定义一个对外只读对内可读写的属性
<details>
<summary>查看答案</summary>
我们可以在.h文件声明属性用readonly关键词，在.m声明同样的属性用readwrite默认的关键词。

- .h

```objc
@interface ClassA : NSObject
@property (nonatomic, assign, readonly) int number;
@end
```
- .m

```objc
@interface ClassA ()
@property (nonatomic, assign) int number;
@end

@implementation ClassA
@end
```
</details>

# block和代理的区别，哪个更好？
<details>
<summary>查看答案</summary>
block使用起来更加简单，比如访问作用域的变量还有代码逻辑的连贯性。代理更能表现是面向接口，能更好的描述功能和作用。如果是对外面向的接口推荐使用代理，日常开发页面之间的传值可以使用block.并且block和代理之间，block的执行会比较快一些。
</details>

# 在block内如何修改block外部变量？
<details>
<summary>查看答案</summary>
通过block内部访问外部的变量，block会自动copy到自己的闭包空间。也就是复制了一份变量在自己的作用域内部，所以是无法修改外部的变量。通过__block关键词修饰之后，通过复制变量的引用地址来修改和访问外部变量的。
</details>

# 类别和类扩展的区别
<details>
<summary>查看答案</summary>
类别可以添加属性 添加的方法不实现会报警告。可以添加实例变量，但是只能自己类访问。扩展可以添加方法，不实现不会报警告。不能添加实例变量，属性可以通过关联对象实现。
</details>

# 分类的作用？分类和继承的区别？
<details>
<summary>查看答案</summary>
分类可以将一个臃肿的类分散到多个文件中，可以将功能按照模块划分，多人多人开发一个类。分类可以给系统类添加方法，还可以拦截系统方法。继承拥有对类完全控制权，分类只对于引入文件有效。分类可以在不获取原来代码基础上增加方法，不可以删除方法，也不可以新增属性，分类具有更高的优先级。继承可以添加和删除方法，也可以新增属性。
</details>

# 重写一个类的方法用继承好还是分类好? 为什么?
<details>
<summary>查看答案</summary>
分类只对于本次有效，推荐用分类。
</details>

# 若一个类有实例变量NSString *_foo，调用setValue:forKey:时，可以以foo还是_foo作为key
<details>
<summary>查看答案</summary>
这个其实是考察我们KVC的基本原理，都是可以的。
</details>

# isMemberOfClass 和 isKindOfClass 联系与区别
<details>
<summary>查看答案</summary>
isMemberOfClass只能判断是否是当前的类，isKindOfClass可以用来判断是否是当前类或者子类。
</details>

# 一个objc对象的isa的指针指向什么？有什么作用？
<details>
<summary>查看答案</summary>
isa其实是一个结构体 isa对象指向当前类 当前类指向父类 父类指向元类也就是NSObject，元类指向自己。作用是方便找到对应所在的方法。
</details>

# 请描述一个你所遇到retain cycle例子
<details>
<summary>查看答案</summary>
这个第一次解除Block时候应该经常用到，在Block内部使用self调用属性或者方法。
</details>

# 请谈谈内存的使用和优化的注意事项
<details>
<summary>查看答案</summary>
我们知道创建对象 加载图片 加载文件 创建线程都需要消耗内存，特别是加载图片和加载文件会大量消耗内存。我们还知道在大量循环中创建临时变量就直线性的内存暴涨。通过上面我们可以才去进一步的优化内存，对于创建对象，因为单利会驻留在内存里面，不要创建消耗内存大的对象作为单利。加载图片imageName:会把图片驻留在内存里面做缓存提高下一次访问速度，我们可以把大图片用imageWithContentsOfFile的方法进行加载。尽可能减少多线程创建和数量，使用NSCache作为缓存机制替换我们常用的数组和字段作为缓存手段。在大量循环中用外部变量防止多次创建变量或者在循环内部使用runloop.如果是列表形式，做到数据重用。尽可能少的创建和消耗内存。
</details>

