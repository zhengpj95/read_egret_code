# Stage

> Stage 类代表主绘图区。

Stage 继承与 DisplayObjectContainer，有些继承过来的许多属性和方法不适用于 Stage 对象。

Stage 对象事件：

- egret.Event.RESIZE 当stageWidth或stageHeight属性发生改变时调度
- egret.Event.DEACTIVATE 当stage失去焦点后调度
- egret.Event.ACTIVATE 当stage获得焦点后调度

一些特殊属性或方法：

- $screen: egret.sys.Screen 设备屏幕引用 （在创建WebPlayer时，会创建一个Stage对象，其$screen就指向WebPlayer）
- frameRate 获取并设置舞台的帧速率。帧速率是指每秒显示的帧数。帧速率的有效范围为每秒 0.01 到 60 个帧。注意: 修改任何一个Stage的frameRate属性都会同步修改其他Stage的帧率。
- get stageWidth 舞台的当前宽度（以像素为单位）
- get stageHeight 舞台的当前高度（以像素为单位）
- invalidate 改变要广播Event.RENDER事件的标志（$invalidateRenderFlag）。调用 invalidate() 方法后，在显示列表下次呈现时，Egret 会向每个已注册侦听 Event.RENDER 事件的显示对象发送一个 Event.RENDER 事件。每次您希望 Egret 发送 Event.RENDER 事件时，都必须调用 invalidate() 方法。
- scaleMode 指定要使用哪种缩放模式的值（EXACT_FIT, SHOW_ALL, NO_SCALE, NO_BORDER, FIXED_WIDTH, FIXED_HEIGHT）
- orientation 屏幕横竖屏显示方式，目前 Native 下只能在配置文件里设置。
- textureScaleFactor 改变或获取绘制纹理的缩放比率，默认值为1
- maxTouches 设置屏幕同时可以触摸的数量。高于这个数量将不会被触发响应。
- setContentSize 设置分辨率尺寸
