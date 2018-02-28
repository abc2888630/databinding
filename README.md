# databinding

目前了解到双向数据绑定实现的方式有三种：

1、是backbone的发布者和订阅者模式
2、angularJs的脏检测模式
3、vue 数据挟持模式

双向绑定本质上是一种IoC的思想，通过让：

页面元素声明自己依赖的model
JS逻辑不依赖具体dom
达到了解耦合的目的。
