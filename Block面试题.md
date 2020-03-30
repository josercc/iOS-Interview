
# Block的种类

<details>
<summary>查看答案</summary>

- NSGlobaleBlock

  > 全局的Block.也就是创建的静态 常量的Block

- NSMallocBlock

  > 在进程堆创建的Block 参数变量，临时变量。

- NSStackBlock

  > 在进程栈创建的Block，通过Copy之后的Block

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
