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

# iOS视频成像的原理

<details>
<summary>查看答案</summary>


![image-20200331094005407](https://tva1.sinaimg.cn/large/00831rSTgy1gdoj1hyfhlj30pu0fz3zo.jpg) 

由CPU进行位图合成，交给GPU进行图层混合和纹理合成。GPU结果存放在缓冲区（Frame Buffer）中，再由视频控制器通过VSync信号在指定时间从缓冲区提取屏幕显示内容显示在显示器上。

</details>

# UI卡顿掉帧原因

<details>
<summary>查看答案</summary>

![image-20200331094935050](https://raw.githubusercontent.com/joserccblog/uPic/upic/uPic/image-20200331094935050.png)

当`CPU`和`GPU`处理时间大于`16.67ms`时候就会造成掉帧和卡顿。 

</details>

# CPU优化

<details>
<summary>查看答案</summary>
  
- 对象的创建
  - 尽量用轻量的对象代替重量的对象 比如不需要交互的可以用`CALayer`代替`UIView`
  - 尽量在子线程创建对象
  - 尽量代码创建对象替代`StoryBoard`创建对象
  - 懒加载创建对象
  - 尽可能的对象重用
- 对象调整
  - 尽量避免不必要`UIView`对象的调整
  - 尽量避免移动`UIView`图层层次和添加或者移出
- 对象销毁
  - 尽可能在后台线程销毁对象
- 布局计算
  - 尽量提前计算好视图布局
  - 尽量布局复杂少用自动布局
  - 布局可以考虑添加缓存机制
- 文本计算
  - 大量文本可以在子线程用`[NSAttributedString boundingRectWithSize:options:context:]`计算文本的宽度
  - 大量文本可以在子线程用`[NSAttributedString drawWithRect:options:context:] `绘制文本
- 文本渲染
  - 显示大量文本时候，可以自定义文本控制，通过缓存绘制信息提升绘制效率
- 图片解码
  - 在子线程将图片绘制在`CGBitmapContext`，在`Bitmap`直接的创建图片
- 图像的绘制
  - 图像的绘制可以放在后台线程中

</details>

# GPU优化

<details>
<summary>查看答案</summary> 

- 纹理的渲染
  - 尽可能减少短时间内加载多张图片 比如多张图片合成一张
  - 图片的大小和视图大小不要超出纹理尺寸上线`4096*4096`。
- 图层混合
  - 尽量减少图层层次和数量
  - 在不需要透明的视图标记`opaque`属性
  - 可以将多张图片合成一张
- 图形生成
  - 后台线程生成圆角图片代理`layer.corner`设置圆角
  - 开启`layer.shouldRasterize`属性将绘制可以转交给`CPU`分解压力

</details>



