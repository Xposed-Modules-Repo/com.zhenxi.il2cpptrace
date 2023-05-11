主要两个功能 。

il2cpp的源码dump和方法调用跟踪 。

在不勾选il2cppTrace的时候默认使用Dump功能 。



## Dump功能简介：

Dump主要参考自：（尊重原创）

https://github.com/Perfare/Zygisk-Il2CppDumper 

主要使用Xposed去实现Dump，而非Magisk模块注入的方式去dump 。

Hook了 Linker在libil2cpp.so加载到内存里以后立刻进行dump和解密 ，防止被加固 。

dump文件保存在/data/data/包名/Funil2cpp_dump_时间.cs



## Trace功能简介：

主要是遍历所有Il2cpp方法地址，获取对应的方法名和类名 ，通过方法插装的方式在符合关键字的地址进行插装 （支持短指令方法），

监听某个地址或者il2cpp的方法调用 ，而非通过inlinehook的方式  。

插装方法来自Dobby 。

https://github.com/jmpews/Dobby

可以根据关键字进行过滤。方法调用文件保存在/data/data/包名/下 。



### 使用小技巧：

> 因为调用方法过多, 所以我们经常只需要对游戏的某个动作的方法调用进行触发即可 。
>
> 比如我希望获取人物换装备的方法调用栈，在模块勾选il2cppTrace，输入需要过滤的关键字 ：
>
> 1，先删除本地/data/data/包名下以保存的trace方法文件 。将之前没用的方法调用删除，防止干扰。
>
> 2，转换到游戏更换装备。
>
> 3 ,  退出游戏，防止其他调用干扰。

这么一来，保存在/data/data/包名下的即为更换装备调用栈 。

当然过滤关键字也可以传入**ALL** 。

可以针对全部方法的监听，但是这种及其不稳定 。因为一个游戏基本都是几万个或者几十万个方法。可能会存在崩溃等情况 ，

所以监听全部调用的不建议使用 。
