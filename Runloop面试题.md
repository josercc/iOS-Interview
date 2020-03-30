
# RunLoop的作用

<details>
<summary>查看答案</summary>

  `Runloop`是一种循环机制，在不需要处理事件时候休眠，在需要处理事件时候唤醒处理事件。`Runloop`就是让线程不退出，随时能处理事件的一种机制。

</details>

# RunLoop与线程之间的关系

<details>
<summary>查看答案</summary>

`Runloop`和线程是一一对应的，存在于全部的字典当中。线程创建的时候是没有对应`Runloop`的，只有获取当前`Runloop`才会创建，`Runloop`会在线程结束销毁。

</details>

# NSTimer在列表滑动时失效的原因和解决方法

<details>
<summary>查看答案</summary>

`Runloop`存在五种运行`mode`，`NSTimer`默认运行在`default mode`上面的，当列表滚动的时候，切换称`Tracking Mode`。`default mode`就会暂停，这就是为什么`NStimer`在列表滚动时候失效，解决的办法将`NStimer`添加到`common mode`里面。

</details>

# Runloop的作用

<details>
<summary>查看答案</summary>

- 保持应用的持续运行
- 处理App的各种事件
- 节省CPU资源，提升性能。
- 负责渲染界面的UI

</details>

# Runloop 获取

<details>
<summary>查看答案</summary>

- 获取主线程对应的`Runloop`

  > ```objc
  > [NSRunLoop mainRunLoop]
  > ```

- 获取当前线程对应的`RunLoop`

  > ```objc
  > [NSRunLoop currentRunLoop]
  > ```

</details>

# RunLoop和AutoreleasePool的关系

<details>
<summary>查看答案</summary>

`RunLoop`进行处理事件的时候会自动创建一个`AutoreleasePool`，在处理事件过程中会将发送`autorelease`消息的对象添加到`AutoreleasePool`中。等待`RunLoop`处理事件结束，就释放当前的`AutoreleasePool`。`AutoreleasePool`则会将所有的对象进行`release`-1操作。

</details>
