
# #include与#import的区别、#import与@class 的区别
<details>
<summary>查看答案</summary>
  #include和#import都可以引入头文件，但是#import只会引入一次。#import虽然只会引入一次，但是还会导致相互引入的问题。@class可以在头文件引入类，类可以是不存在的，可以在头文件@class引入类，在.m用#import引入类实现从而解决循环引用的问题。
</details>
