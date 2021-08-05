# eui库的createChildren方法

看一下下面链接里的 implementUIComponent 方法。

[UIComponent](./UIComponent.md)

createChildren 是在 UIComponentImpl 基类中才有的，Component 类通过 implementUIComponent 方法实现了 UIComponentImpl 类的属性和方法；然后其他的 eui 组件有继承了 Component 类，所以 eui 的组件都有了 createChildren 方法了。

createChildren 和 childrenCreated 方法都是只调用一次，在组件添加到舞台后调用一次。
