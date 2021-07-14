# DisplayObjectContainer

> DisplayObjectContainer 类是基本显示列表构造块：一个可包含子项的显示列表节点。

DisplayObjectContainer 就是一个容器，能展示在舞台上的组件都能放入其中。其实舞台对象也是一个DisplayObjectContainer。

DisplayObjectContainer 是继承 DisplayObject 的。在 DisplayObject 中主要都是一些属性获取或设置的方法。在 DisplayObjectContainer 中主要都是操作显示对象，比如添加到容器中，从容器中移除，改变孩子的Z轴层级。

- getChildAt 根据索引返回子对象， getChildAt方法从缓存数组$children中访问子项
- getChildByName 返回具有指定名称的子显示对象，如果有多个相同的，则返回第一个。此方法则必须遍历链接的列表来访问子项，效率比getChildAt低。

其中有两个方法主要是用于在eui组件的子类中使用的：

- $childAdded 一个子项被添加到容器内，此方法不仅在操作addChild()时会被回调，在操作setChildIndex()或swapChildren时也会回调。当子项索引发生改变时，会先触发$childRemoved()方法，然后触发$childAdded()方法。
- $childRemoved 一个子项从容器内移除，此方法不仅在操作removeChild()时会被回调，在操作setChildIndex()或swapChildren时也会回调。当子项索引发生改变时，会先触发$childRemoved()方法，然后触发$childAdded()方法。

eui.Component 和 eui.Group 就是继承 egret.DisplayerObjectContainer，其中就实现了上面的两个方法。（这一部分在读到eui源码时会看到implementUIComponent，这个就是实现eui的组件继承于egret的组件的。）
