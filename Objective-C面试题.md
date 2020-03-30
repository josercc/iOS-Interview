
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

# runtime 如何实现 weak 属性
<details>
<summary>查看答案</summary>
weak属性其实是系统维护的一个Hash表，系统会把weak声明的属性的内存地址作为key存放在hash表里面。当这个属性引用技术为0走dealloc方法时候，通过对应内存地址作为key再从hash表移出出去。
</details>

# runtime如何通过selector找到对应的IMP地址？
<details>
<summary>查看答案</summary>
每个类里面都有一个method_list数组存储着这个类所有的方法和实现，通过selector对应的方法名称找到对应的方法和实现。
</details>

# +(void)load; +(void)initialize；有什么用处？
<details>
<summary>查看答案</summary>
load放在会在第一次加载类方法会调用，调用顺序是先父类，然后子类，然后分类。initialize会在第一次调用类方法或者实例方法时候被调用。
</details>

# 如何访问并修改一个类的私有属性？
<details>
<summary>查看答案</summary>
可以通过KVC或者通过ivar进行访问。
	
- 类实现

```objc
@interface ClassA : NSObject
@end

@implementation ClassA {
    int _number;
}

- (instancetype)init {
    if (self = [super init]) {
        _number = 10;
    }
    return self;
}

@end
```
- KVC

```objc
ClassA *a = [[ClassA alloc] init];
NSLog(@"number = %@",[a valueForKey:@"number"]);
[a setValue:@(5) forKey:@"number"];
NSLog(@"number = %@",[a valueForKey:@"number"]);
```
- Ivar

```objc
ClassA *a = [[ClassA alloc] init];
Ivar var = class_getInstanceVariable([ClassA class], "_name");
NSLog(@"number = %@",object_getIvar(a, var));
object_setIvar(a, var, @"joser");
NSLog(@"number = %@",object_getIvar(a, var));
```

> Ivar对比KVC来说使用复杂，并且只支持对象类型。如果是基本类型直接在objc_getIvar方法崩溃。
</details>

# Objective-C 如何对已有的方法，添加自己的功能代码以实现类似记录日志这样的功能？
<details>
<summary>查看答案</summary>
我们可以利用分类，通过交换方法实现。可以在方法之前也可以在方法后面。

```objc
@interface ClassA : NSObject
@end

@implementation ClassA

- (void)printMyName {
    NSLog(@"josercc");
}

@end

@interface ClassA (Print)
@end

@implementation ClassA (Print)

+ (void)load {
    Method method1 = class_getClassMethod(self, @selector(printMyName));
    Method method2 = class_getClassMethod(self, @selector(printMyName1));
    method_exchangeImplementations(method1, method2);
}

- (void)printMyName1 {
    NSLog(@"Hello");
    [self printMyName1];
}

@end
```
</details>

# 请说明并比较以下关键词：strong, weak, assign, copy
<details>
<summary>查看答案</summary>

- strong 会持有对象，会使对象引用计数加1
- weak 不会持有对象 只是存储了对象的内存地址
- assgin 用于声明基本类型 被assgin声明的会存放在栈区
- copy 会复制一份内存地址 一般用于NSString NSArray NSDictionary
</details>

# 请说明并比较以下关键词：__weak，__block
<details>
<summary>查看答案</summary>
	
- __weak用来修饰实例变量
- __block用来修饰可以在block内部修改外部变量
</details>

# 请说明并比较以下关键词：atomatic, nonatomic
<details>
<summary>查看答案</summary>

- atomatic 是相对线程安全的因为会自动在set和get方法进行加锁 但是会额外的消耗性能 默认
- nonatomic 是线程不安全的 性能好
</details>

# 下面代码中有什么bug？
```objc
- (void)viewDidLoad {
  UILabel *alertLabel = [[UILabel alloc] initWithFrame:CGRectMake(100,100,100,100)];
  alertLabel.text = @"Wait 4 seconds...";
  [self.view addSubview:alertLabel];

  NSOperationQueue *backgroundQueue = [[NSOperationQueue alloc] init];
  [backgroundQueue addOperationWithBlock:^{
    [NSThread sleepUntilDate:[NSDate dateWithTimeIntervalSinceNow:4]];
    alertLabel.text = @"Ready to go!”
  }];
}
```
<details>
<summary>查看答案</summary>

> 我们是在子线程操作的UI应该在主线程操作UI不会更新(在Xcode11测试依然会更新，只是会报不能在子线程更新UI的警告 -[UILabel setText:] must be used from main thread only)。
</details>

# Object-C有多继承吗？没有的话用什么代替？cocoa 中所有的类都是NSObject 的子类
<details>
<summary>查看答案</summary>
OC是没有多继承的，可以通过协议进行代替。
</details>

# Object-C有私有方法吗？私有变量呢？
<details>
<summary>查看答案</summary>
OC并不存在严格来说的私有方法和私有变量。对于用@public修饰的属性和方法在.h都可以被外部调用。在.m实现的方法和实例变量只能本类调用就称作私有的。OC是一个有运行时的特性，所以我们可以获取方法列表找到私有方法通过消息转发调用。对于私有变量我们可以通过KVC,或者Ivar进行访问。所以OC不存在严格意义上面的私有方法和私有变量
</details>

# 关键字const什么含义
<details>
<summary>查看答案</summary>
	
对于const来说 声明在谁前面就意味着谁不能修改，如果`const`在第一位则向右移一位。比如`int const a`等效果于`const int a`都是让整形的`a`不可变。声明`const`可以让编译时候代码更紧凑，让系统修改变量时候随时产生报错，防止BUG的产生。
</details>

# tableView的重用机制？
<details>
<summary>查看答案</summary>
	
`UITableView`有两个实例变量，`visiableCells`保存当前界面显示的Cell的数组，`reusableTableCells`保存在可以重用的Cell的字典。当第一次加载界面，`reusableTableCells`没有任何重用的Cell就会走`UITableViewCell`的初始化方法进行创建，之后添加到`visiableCells`数组里面。当界面滚动时候，原本展示的Cell消失在屏幕之后。对应的`Cell`对象就会添加到`reusableTableCells`里面，即而从`visiableCells`数组里面进行移出。当一个新的cell将展示时候从`reusableTableCells`查看是否存在重用的，如果存在就展示重用的，如果没有就重新走创建的流程。
</details>

# ViewController的didReceiveMemoryWarning是在什么时候调用的？默认的操作是什么？
<details>
<summary>查看答案</summary>

当手机内存运行不足时候会调用`ViewController`的`didReceiveMemoryWarning`方法，默认时尝试释放`ViewController`所拥有的`View`。我们也可以重写做其他释放内存的任务。
</details>

# delegate和notification区别，分别在什么情况下使用？
<details>
<summary>查看答案</summary>
	
`delegate`是一对一的关系，`notification`是一对多的关系。`delegate`通畅用于开放接口用于其他模块的调用，`notification`可以做到模块分离，通畅用于模块之间的通信。
</details>

# id、nil代表什么？
<details>
<summary>查看答案</summary>
	
`id`代表在`OC`里面的对象的指针，`nil`代表是指针指向一个空的对象。
</details>

# timer的间隔周期准吗？为什么？怎样实现一个精准的timer?
<details>
<summary>查看答案</summary>
	
`NSTimer`因为是添加到`runloop`中的走的是默认的`mode`，当滑动表格的时候就会切换对应`mode`停止`timer`。`runloop`因为优先在触发时候执行输入源，才会执行`runloop`中的任务，虽有会有`50-100`毫秒的误差。

我们想做一个精准的`timer`就需要创建一个新的`runloop`来不受其他输入源的影响。

- 添加`Timer`到当前的`runloop`设置为`NSRunLoopCommonModes`
```objc
_timer = [NSTimer scheduledTimerWithTimeInterval:1.0
				      target:self
				    selector:@selector(timerStart)
				    userInfo:nil
				     repeats:YES];
[[NSRunLoop currentRunLoop] addTimer:_timer forMode:NSRunLoopCommonModes];
```
- 在新线程添加`NSTimer`
```objc
dispatch_async(dispatch_queue_create("", 0), ^{
@autoreleasepool {
    self->_timer = [NSTimer scheduledTimerWithTimeInterval:1.0
					      target:self
					    selector:@selector(timerStart)
					    userInfo:nil
					     repeats:YES];
    [[NSRunLoop currentRunLoop] run];
}
});
```
- GCD
```objc
timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, dispatch_get_main_queue());
dispatch_source_set_timer(timer, DISPATCH_TIME_NOW, 1.0 * NSEC_PER_SEC, 0 * NSEC_PER_SEC);
dispatch_source_set_event_handler(timer, ^{
NSLog(@"timer");
});
dispatch_resume(timer);
```
对比还是第三种方案简单，第二种执行的任务会在子线程，容易写出BUG，第一种容易遗漏添加到`runloop`设置对应的`mode`。第三种有现成的代码块，而且还十分的精准。
</details>

# 一个 NSObject 对象占用多少内存？
<details>
<summary>查看答案</summary>
	
系统分配18个字节，但是真正占用只有8个字节。
</details>

# 方法和选择器有何不同？(Difference between method and selector?)
<details>
<summary>查看答案</summary>

`selector`只是代表方法名称，而`method`包含了名称和实现。
</details>

# 你是否接触过OC中的反射机制？简单聊一下概念和使用
<details>
<summary>查看答案</summary>
	
反射机制就是通过字符串反射为对应的类，协议，方法。或者将协议，方法或者类反射为字符串。通常用于做模块化跳转，或者用于做`deeplink`等运行时创建类等功能。
</details>

# Objective-C中的协议默认是@optional还是@require？在使用协议的时候应当注意哪些问题？
<details>
<summary>查看答案</summary>

协议默认为`@require`，使用时候要注意用`weak`声明代理，防止循环引用。
</details>

# 通知的实现机制？为什么有时候监听的名称和发送通知名称相同却收不到回调。

<details>
<summary>查看答案</summary>
  
  通知底层核心是存在一个`notication_map`的字典，通知的名称作为`Key`，`Value`为数组结构，数组的每个元素包含了监听的对象，方法名称，还有传递的参数。当发送通知的时候，会在`notication_map`的字典当中找到对应`key`的数组，通过遍历数组，对数组里面所有的监听者发送通知。当发送参数和监听的参数不一样的时候，会被忽略，这就是为什么名字相同却收不到通知。

- 当接受通知的参数为空可以接受发送通知参数为空或者不为空

  可以接受到通知

  ```swift
  NotificationCenter.default.addObserver(self, selector: #selector(revicerNotication(_:)), name: NSNotification.Name(rawValue: "test notification"), object: nil)
          NotificationCenter.default.post(name: NSNotification.Name(rawValue: "test notification"), object: nil)
  ```

  ```swift
  NotificationCenter.default.addObserver(self, selector: #selector(revicerNotication(_:)), name: NSNotification.Name(rawValue: "test notification"), object: nil)
          NotificationCenter.default.post(name: NSNotification.Name(rawValue: "test notification"), object: "234")
  ```

  ```swift
  NotificationCenter.default.addObserver(self, selector: #selector(revicerNotication(_:)), name: NSNotification.Name(rawValue: "test notification"), object: "234")
          NotificationCenter.default.post(name: NSNotification.Name(rawValue: "test notification"), object: "234")
  ```

  不可以接受到通知

  ```swift
  NotificationCenter.default.addObserver(self, selector: #selector(revicerNotication(_:)), name: NSNotification.Name(rawValue: "test notification"), object: "234")
          NotificationCenter.default.post(name: NSNotification.Name(rawValue: "test notification"), object: nil)
  ```

  ```swift
  NotificationCenter.default.addObserver(self, selector: #selector(revicerNotication(_:)), name: NSNotification.Name(rawValue: "test notification"), object: "234")
          NotificationCenter.default.post(name: NSNotification.Name(rawValue: "test notification"), object: "123")
  ```

</details>




