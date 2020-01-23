# 你怎么理解vue中的diff算法？

`vue`中的`diff`算法简单来说是将`Vnode`和`oldVnode`作对比，发现有不一样的地方就直接修改在真实的DOM上，然后使`oldVnode`的值为`Vnode`。 ` diff`的过程就是调用名为`patch`的函数，比较新旧节点，一边比较一边给**真实的DOM**打补丁。 

### diff流程图

 当数据发生改变时，set方法会让调用`Dep.notify`通知所有订阅者Watcher，订阅者就会调用`patch`给真实的DOM打补丁，更新相应的视图。 

![](E:\每日一题\task\imges\diff-l.png)