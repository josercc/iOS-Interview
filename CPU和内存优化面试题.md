# 什么时Page Memory

<details>
<summary>查看答案</summary>

一段内存是有一个或者多个`Page Memory`组成的，一个`Page Memory`大小时`16K`.`Page Memory`刚申请的内存状态为`Clean`，当存储数据之后状态变成`Dirty`。

</details>

# iOS的内存有几种内存类型，分别对应什么

<details>
  <summary>查看答案</summary>


iOS的内存类型分为三种

- Clean Memory

  > 可以被Page Out的内存空间,通常`.framework`中的`_DATA_CONST_`段

- Dirty Memory

  > 被App写入输入的内存，同时是堆区的对象，图像解码空间，`.framwork`中的`_DATA_`段和`_DATA_DIRTY_`段。在`.framework`使用单利初始化可以有效减少`Dirty Memory`的占用

- Compressed Memory

  > 当内存吃紧的时候，系统会将不适用的内存压紧。比如用`Dictionary`缓存数据占用三页内存，当内存吃紧被压缩为一页，当再次使用，再次被释放成三页。
  </details>

# 为什么缓存数据要推荐用NSCache替换Dictionary？

<details>
<summary>查看答案</summary>

因为在内存吃紧的时候，`NSCache`会自动释放内存，但是`Dirtionary`不会。
</details>

# 一张500x500大小的图片在iOS占用多少内存？

<details>
  <summary>查看答案</summary>


  500x500x8 / (1024 * 1024) ~= 0.95MB 
</details>

# 为什么界面会卡顿？

<details>
  <summary>查看答案</summary>


  因为设备的刷新频率时一秒60次，那么每次屏幕渲染的时间就是1/60秒大约0.0167秒时间交给CPU进行计算，当CPU压力大计算时间超多0.0167秒时候此时界面刷新没有内容出现就没有绘制，造成了卡顿的现象。
</details>
