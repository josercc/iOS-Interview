# UIView和CALayer的关系和联系?

<details>
<summary>查看答案</summary>


UIView都有一个Layer的属性用来绘制内容，UIView则负责内容的管理。UIView通过实现CALayerDelegate来让CALayer重新绘制内容。UIView可以响应事件，但是CALayer不可以。修改CALayer的属性可以有隐式的动画，但是修改UIView没有。
</details>
