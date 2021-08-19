# callLater

```ts
/**
  * 延迟函数到屏幕重绘前执行。
  * @param method {Function} 要延迟执行的函数
  * @param thisObject {any} 回调函数的this引用
  * @param ...args {any} 函数参数列表
  * @version Egret 2.4
  * @platform Web,Native
  * @includeExample egret/utils/callLater.ts
  * @language zh_CN
  */
export function callLater(method: Function, thisObject: any, ...args): void {
  $callLaterFunctionList.push(method);
  $callLaterThisList.push(thisObject);
  $callLaterArgsList.push(args);
}
```

需要 callLater 的函数，把其对应的函数以及this引用和参数列表分别push到对应的列表中保存着，在 SystemTicker 中执行一次屏幕渲染（render方法）时候再执行这些 callLater 函数。

异步调用的函数也是如此操作。
