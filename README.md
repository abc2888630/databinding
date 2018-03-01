# databinding

目前了解到双向数据绑定实现的方式有三种：

1、是backbone的发布者和订阅者模式
2、angularJs的脏检测模式
3、vue 数据挟持模式


双向绑定本质上是一种IoC的思想，通过让：

页面元素声明自己依赖的model，

JS逻辑不依赖具体dom，

达到了解耦合的目的。


# 这里以angularjs为例，附上angularjs的启动流程

1）浏览器启动后，浏览器会解释DOM树

2）当遇到angularjs就会停下解析，开始执行angular.min.js脚本

3）angularjs监听到DOMContentLoad事件-->启动angularjs应用

4）这个时候开始angularjs初始化阶段，寻找ng-App指令，这就是angularjs的上下文

5）初始化必要的组件：$injector、$compile、$rootScope

6）开始解析DOM树

7）进入到编译、链接的阶段

8）$compile遍历DOM树，搜集ng开头的指令

9）执行每个指令的compile函数

10）处理DOM转换成编译模板

11）调用链接函数生成实时的视图，给每个指令注册监听事件

12）angularjs等待事件触发，执行$digest循环

13）检测到变化，触发事件调用$watch函数

14）再次执行$digest循环，直到没有变化

15）结束



# Angularjs的双向数据绑定，其实是在编译阶段:


1）将ng-app下的所有元素节点缓存起来

2）然后检查每个元素节点的属性值，如果有ng-开头的指令，就为这个指令注册一个监听函数，angularjs在此会有一堆watch表达式，监听它绑定的事件

3）watch对象包括:监听函数、上次变化的值、获取监听表达式的方法以及监听表达式

4）当view改变的时候，浏览器事件会通知到watch对象，然后触发$digest循环，angularjs会将当前值和上次变化值比较，然后更新model

5）接着再执行一个digest循环，直到model不再变化

