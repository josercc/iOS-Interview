
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
