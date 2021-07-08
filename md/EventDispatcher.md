# EventDispatcher

## IEventDispatcher

> IEventDispatcher 接口定义用于添加或删除事件侦听器的方法，检查是否已注册特定类型的事件侦听器，并调度事件。事件目标是 Egret 事件模型的重要组成部分。事件目标是事件如何通过显示列表层次结构这一问题的焦点。当发生触摸轻拍事件时，会将事件对象调度到从显示列表根开始的事件流中。事件对象进行到事件目标的往返行程，在概念上，此往返行程被划分为三个阶段：捕获阶段包括从根到事件目标节点之前的最后一个节点的行程，目标阶段仅包括事件目标节点，冒泡阶段包括到显示列表的根的回程上遇到的任何后续节点。通常，使用户定义的类能够调度事件的最简单方法是扩展 EventDispatcher。如果无法扩展（即，如果该类已经扩展了另一个类），则可以实现 IEventDispatcher 接口，创建 EventDispatcher 成员，并编写一些简单的挂钩，将调用连接到聚合的 EventDispatcher 中。

IEventDispatcher中主要有以下几个方法：

```tsx
//注册事件
//useCapture: 确定侦听器是运行于捕获阶段还是运行于冒泡阶段；true：只在捕获阶段处理事件，false：只在冒泡阶段处理事件
//要在两个阶段都侦听事件，请调用 on() 两次：一次将 useCapture 设置为 true，一次将 useCapture 设置为 false。
addEventListener(type:string, listener:Function, thisObject:any, useCapture?:boolean, priority?:number):void;

//添加仅回调一次的事件侦听器，此方法与on()方法不同，on()方法会持续产生回调，而此方法在第一次回调时就会自动移除监听。
once(type:string, listener:Function, thisObject:any, useCapture?:boolean, priority?:number):void;

//删除侦听器
removeEventListener(type:string, listener:Function, thisObject:any, useCapture?:boolean):void;

//检查 EventDispatcher 对象是否为特定事件类型注册了任何侦听器。
hasEventListener(type:string):boolean;

//将事件分派到事件流中。
dispatchEvent(event:Event):boolean;

//检查是否用此 EventDispatcher 对象或其任何始祖为指定事件类型注册了事件侦听器。
willTrigger(type:string):boolean;
```

hasEventListener() 与 willTrigger() 的区别是：hasEventListener() 只检查它所属的对象，而 willTrigger() 检查整个事件流以查找由 type 参数指定的事件。

## EventDispatcher类

EventDispatcher 是 Egret 的事件派发器类，负责进行事件的发送和侦听。

EventDispatcher 有一个私有Object变量 $EventDispatcher，其key有四个，分别为 0, 1, 2, 3 。

其中 “0” 表示此 EventDispatcher 所抛出事件对象的 target 指向，也就是哪个对象抛出此事件。

“1” 表示侦听器只在冒泡阶段处理事件

“2” 表示侦听器只在捕获阶段处理事件

“3” 只是一个标记，在遍历 1 和 2 的事件时，防止外部修改原始数组导致遍历错误。

EventDispatcher 中有一个 **派发一个指定参数的事件** 的方法：

```tsx
public dispatchEventWith(type: string, bubbles?: boolean, data?: any, cancelable?: boolean): boolean {
            if (bubbles || this.hasEventListener(type)) {
                let event: Event = Event.create(Event, type, bubbles, cancelable);
                event.data = data;
                let result = this.dispatchEvent(event);
                Event.release(event);
                return result;
            }
            return true;
}
```

## egret.sys.EventBin

EventBin是一个定义 事件信息 的接口。说明定义一个Event所需的信息。

```tsx
export interface EventBin {
    type: string; //事件类型
    listener: Function; //监听函数
    thisObject: any;//监听函数绑定的this对象
    priority: number; //事件侦听器的优先级,数字越大，优先级越高, 如果两个或更多个侦听器共享相同的优先级，则按照它们的添加顺序进行处理。默认优先级为 0。
    target: IEventDispatcher;//事件派发器类，比如EventDispatcher，实现IEventDispatcher接口的自定义类，可从target移除新定义的事件
    useCapture: boolean;//确定侦听器是运行于捕获阶段还是运行于冒泡阶段。true: 捕获，false：冒泡
    dispatchOnce: boolean;//只触发一次
}
```

比如，插入一个事件到上面 EventDispacther 的 $EventDispatcher 对象中的key为1，2的对象中：

```tsx
let eventBin: sys.EventBin = {
                type: type, listener: listener, thisObject: thisObject, priority: priority,
                target: this, useCapture: useCapture, dispatchOnce: !!dispatchOnce
};

let values = this.$EventDispatcher;
let eventMap: any = useCapture ? values[Keys.captureEventsMap] : values[Keys.eventsMap];
//插入到eventMap的某一个位置，或者直接末尾
```
