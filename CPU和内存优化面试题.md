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

  因为设备的刷新频率时一秒60次，那么每次屏幕渲染的时间就是1/60秒大约0.0167秒时间。这0.0167秒时间需要CPU计算和GPU渲染完成，当两个时间大于0.0167秒时候就会显示上一显示的内容。从而形成卡顿的现象，GPU渲染完毕，就会在下一侦显示。
</details>

# 从CPU和GPU的角度分析一下怎么优化卡顿

<details>
  <summary>查看答案</summary>


  - CPU优化

  > - 尽量使用轻量的对象
  > - 尽量减少UIView属性不必要的更改
  > - 尽量用计算好的frame替换到自动布局的autolayout
  > - 图片的尺寸保持和控件大小一致
  > - 控制线程的最大并发量
  > - 尽量把耗时的操作放在子线程
  > - 文本处理
  > - 图像处理

- GPU优化

> - 尽量减少视图的层次和数量
>
> - 纹理不要超过4096*4096的尺寸
> - 尽量避免大量图片的显示 尽可能合成一张图片
> - 减少透明度的图层
> - 减少图层混染

</details>

# 离屏渲染消耗性能原理

<details>
<summary>查看答案</summary>


- 需要创建新的缓存区

- 离屏渲染的过程需要多次切换上下文

</details>

# 离屏渲染出现的原因？

<details>
<summary>查看答案</summary>

- layer.shouldRasterize = YES
- 设置遮罩 layer.mask
- 设置圆角
- 设置阴影

</details>

# 一段动画的执行步骤

<details>
<summary>查看答案</summary>

- 布局
- 显示
- 准备
- 提交

</details>

# 介绍一下CPU和GPU负责的工作

<details>
<summary>查看答案</summary>

- CPU
  - 对象的创建和销毁
  - 对象属性的调整
  - 布局计算
  - 文本的计算和排版
  - 图像格式的转码和解码
  - 图像的绘制
- GPU
  - 纹理的渲染

</details>
