
# Block的种类

<details>
<summary>查看答案</summary>

- NSGlobaleBlock

  > 全局的Block.未使用局部变量的Block

- NSMallocBlock

  > 在进程堆创建的Block 通过Copy之后的Block

- NSStackBlock

  > 在进程栈创建的Block，使用局部变量并未copy操作的

</details>

# 为什么在默认情况下无法修改被block捕获的变量？ __block都做了什么

<details>
<summary>查看答案</summary>

 因为默认情况下，`Block`会将访问的变量的值`copy`一份值而不是变量的内存地址到`Block`结构体中。从而默认在`Block`默认情况下无法修改外部变量的值。

`Block`访问`__block`修饰的变量，会通过`__forwarding`基数将外部的变量`copy`一份内存地址到`Block`结构体内部，从而可以修改外部的变量。

</details>

# 什么是block

<details>
<summary>查看答案</summary>

`Block`是对象，封装了一块代码，可以在任何时候运行。`Block`可以作为方法参数，也可以作为方法返回值。自己又带有参数和返回值，和代理的功能相同。
</details>

# 使用block和使用delegate有什么不同

<details>
<summary>查看答案</summary>

-  Block

  > 代码更加的紧凑，使用方便

- Delegate

  > 方法语义明显，适合作为作为第三方接口

</details>

# __block和__weak修饰符的区别

<details>
<summary>查看答案</summary>

`__block` 修饰的变量可以在`Block`内部进行修改，`__weak`修饰的对象可以在`Block`使用防止循环引用。

</details>
