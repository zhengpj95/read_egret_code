# Sound

## 1. Sound接口

> Sound 允许您在应用程序中使用声音。使用 Sound 类可以创建 Sound 对象、将外部音频文件加载到该对象并播放该文件。可通过 SoundChannel 对声音执行更精细的控制，如控制音量和监控播放进度。

egret.Event.COMPLETE 音频加载完成时抛出

egret.IOErrorEvent.IO_ERROR 音频加载失败时抛出

Sound接口拓展自 EventDispatcher（事件派发器类）

方法和属性：

- load 启动从指定 URL 加载外部音频文件的过程
- play(startTime?: number, loops?: number): SoundChannel 生成一个新的 SoundChannel 对象来播放该声音。此方法返回 SoundChannel 对象，访问该对象可停止声音调整音量。startTime 应开始播放的初始位置（以秒为单位），默认值是 0；loops 播放次数，默认值是 0，循环播放。 大于 0 为播放次数，如 1 为播放 1 次；小于等于 0，为循环播放。
- close 关闭该流，从而停止所有数据的下载。
- type  类型，默认为 egret.Sound.EFFECT。 在 native 和 runtime 环境下，背景音乐同时只能播放一个，音效长度尽量不要太长。
- length 当前声音的长度（以秒为单位）。

## 2. SoundChannel接口

> SoundChannel 类控制应用程序中的声音。每个声音均分配给一个声道，而且应用程序可以具有混合在一起的多个声道。SoundChannel 类包含 stop() 方法、用于设置音量和监视播放进度的属性。

egret.Event.SOUND_COMPLETE 音频最后一次播放完成时抛出。

SoundChannel 接口继承自 IEventDispatcher。（为什么Sound继承自EventDispatcher呢？）

方法和属性：

- volume 音量范围从 0（静音）至 1（最大音量）
- position 当播放声音时，position 属性表示声音文件中当前播放的位置（以秒为单位）
- stop 方法 停止在该声道中播放声音
