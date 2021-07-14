# DisplayObject

> DisplayObject 类是可放在显示列表中的所有对象的基类。该显示列表管理运行时中显示的所有对象。使用 DisplayObjectContainer 类排列显示列表中的显示对象。DisplayObjectContainer 对象可以有子显示对象，而其他显示对象（如 Shape 和 TextField 对象）是“叶”节点，没有子项，只有父级和同级。
>
> DisplayObject 类有一些基本的属性（如确定坐标位置的 x 和 y 属性），也有一些高级的对象属性（如 Matrix 矩阵变换）。
>
> DisplayObject 类包含若干广播事件。通常，任何特定事件的目标均为一个特定的 DisplayObject 实例。例如，added 事件的目标是已添加到显示列表的目标 DisplayObject 实例。若只有一个目标，则会将事件侦听器限制为只能监听在该目标上（在某些情况下，可监听在显示列表中该目标的祖代上）。但是对于广播事件，目标不是特定的 DisplayObject 实例，而是所有 DisplayObject 实例（包括那些不在显示列表中的实例）。这意味着您可以向任何DisplayObject 实例添加侦听器来侦听广播事件。

- $stage 显示对象的舞台，若有多个显示对象并加载到显示列表中，每个显示对象的 stage 属性是指向相同的 Stage 对象；若显示对象未添加到显示列表，则其 stage 属性为 null
- globalToLocal 将从舞台（全局）坐标转换为显示对象的（本地）坐标
- localToGlobal 将显示对象的（本地）坐标转换为舞台（全局）坐标
- hitTestPoint 计算显示对象，以确定它是否与 x 和 y 参数指定的点重叠或相交。注意，不要在大量物体中使用精确碰撞像素检测，这回带来巨大的性能开销
- $children 能够含有子项的类将子项列表存储在这个属性里
- $nestLevel 这个对象在显示列表中的嵌套深度，舞台为1，它的子项为2，子项的子项为3，以此类推。当对象不在显示列表中时此属性值为0.
