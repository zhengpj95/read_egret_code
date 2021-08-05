# startTick & stopTick

startTick : 注册并启动一个计时器，通常会以60FPS的速率触发回调方法，并传入当前时间戳。注册后将会持续触发回调方法，若要停止回调，需要手动调用stopTick()方法。

startTick 方法最终调用的还是 SystemTicker 下的 $startTick 方法；stopTick 方法对应 $stopTick 方法。

```tsx
/**
 * @param callBack 要执行的回调方法。参数 timeStamp 表示从启动Egret框架开始经过的时间(毫秒)。
 */
export function startTick(callBack:(timeStamp:number)=>boolean,thisObject:any):void {
    if (DEBUG && !callBack) {
        $error(1003, "callBack");//参数 {0} 不能为 null。
    }
    ticker.$startTick(callBack,thisObject);
}
```
