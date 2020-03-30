
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
