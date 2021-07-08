# HashObject

Egret顶级对象。框架内所有对象的基类，为对象实例提供唯一的hashCode值。

此对象有一个 getter 方法，返回此对象唯一的哈希值,用于唯一确定一个对象。hashCode为大于等于1的整数。

```bash
public get hashCode(): number {
  return this.$hashCode;
}
```

egret中有一个全局的哈希计数变量 **$hashCount**，其就是每创建一个egret对象，都会让其加1。

```bash
export let $hashCount: number = 1;
```
