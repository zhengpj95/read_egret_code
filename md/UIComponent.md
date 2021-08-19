# UIComponent

## UIComponent接口

> UIComponent 类是所有可视组件（可定制皮肤和不可定制皮肤）的基类。
egret.Event.RESIZE 当UI组件的尺寸发生改变时调度。
eui.UIEvent.MOVE 当UI组件在父级容器中的位置发生改变时调度。
eui.UIEvent.CREATION_COMPLETE 当UI组件第一次被添加到舞台并完成初始化后调度

UIComponent是一个接口， 继承 egret.DisplayObject （DisplayObject 类是可放在显示列表中的所有对象的基类）。

UIComponent的属性都是相对于其父级容器而言的。比如 right 表示距父级容器右边距离；percentWidth 表示相对父级容器宽度的百分比；percentHeight 表示相对父级容器高度的百分比。

## UIComponentImpl基类模板

> UIComponentImpl 是 EUI 显示对象基类模板。
仅作为 UIComponent 的默认实现，为egret.sys.implementUIComponent()方法提供代码模板。
注意：在此类里不允许直接使用super关键字访问父类方法。一律使用this.$super属性访问。

UIComponentImpl 继承了 egret.DisplayObject ，实现了 eui.UIComponent。

### $UIComponent 属性

$UIComponent 是一个 Object 对象，其中记录了组件各种属性的值和标记。如下所示。

```tsx
this.$UIComponent = {
    0: NaN,       //left
    1: NaN,       //right
    2: NaN,       //top
    3: NaN,       //bottom
    4: NaN,       //horizontalCenter
    5: NaN,       //verticalCenter
    6: NaN,       //percentWidth
    7: NaN,       //percentHeight
    8: NaN,       //explicitWidth
    9: NaN,       //explicitHeight
    10: 0,              //width
    11: 0,              //height
    12: 0,              //minWidth
    13: 100000,         //maxWidth
    14: 0,              //minHeight
    15: 100000,         //maxHeight
    16: 0,              //measuredWidth
    17: 0,              //measuredHeight
    18: NaN,      //oldPreferWidth
    19: NaN,      //oldPreferHeight
    20: 0,              //oldX
    21: 0,              //oldY
    22: 0,              //oldWidth
    23: 0,              //oldHeight
    24: true,           //invalidatePropertiesFlag
    25: true,           //invalidateSizeFlag
    26: true,           //invalidateDisplayListFlag
    27: false,          //layoutWidthExplicitlySet
    28: false,          //layoutHeightExplicitlySet
    29: false          //initialized
};
```

### initializeUIValues方法

UIComponentImpl 定义的所有变量请不要添加任何初始值，必须统一在此处初始化。

所有继承UIComponent基类的方法中都有一个initializeUIValues方法，但是只能在此方法内初始化。

### createChildren 和 childrenCreated方法

子类覆盖 createChildren 可以执行一些初始化子项操作，但是仅在组件第一次添加到舞台时回调一次。childrenCreated 是在子项创建完成后调用的，在createChildren之后执行。

eui的组件才有此两个方法，egret的组件是没有的。是因为 Component 类继承了 UIComponentImpl 基类模板，然后其他的 eui 组件又继承了 Component 类。

UIComponentImpl 基类下大多都是一些获取设置组件属性的 getter 和 setter 方法。

## isEmptyFunction全局方法

此方法在 eui.sys 命名空间下

```tsx
/**
 *检查一个函数的方法体是否为空。
*/
function isEmptyFunction(prototype: any, key: string): boolean {
    if (typeof prototype[key] != "function") {
        return false;
    }
    let body = prototype[key].toString();
    let index = body.indexOf("{");
    let lastIndex = body.lastIndexOf("}");
    body = body.substring(index + 1, lastIndex);
    return body.trim() == "";
}
```

## mixin全局方法

此方法在 eui.sys 命名空间下

```tsx
/**
 * @private
 * 拷贝模板类的方法体和属性到目标类上。
 * @param target 目标类
 * @param template 模板类
 */
export function mixin(target: any, template: any): void {
  for (let property in template) {
    if (property != "prototype" && template.hasOwnProperty(property)) {
      target[property] = template[property];
    }
  }
  let prototype = target.prototype;
  let protoBase = template.prototype;
  let keys = Object.keys(protoBase);
  let length = keys.length;
  for (let i = 0; i < length; i++) {
    let key = keys[i];
    if (key == "__meta__") {
      continue;
    }
    if (!prototype.hasOwnProperty(key) || isEmptyFunction(prototype, key)) {
      let value = Object.getOwnPropertyDescriptor(protoBase, key);
      Object.defineProperty(prototype, key, value);
    }
  }
}
```

### implementUIComponent全局方法

此方法在 eui.sys 命名空间下。

Group, BitmapLabel, Component, EditableText, Image, Label 组件都调用过此方法。

例如：在 Group.ts 类下，有 sys.implementUIComponent(Group, egret.DisplayObjectContainer, true) 来设置实现UIComponent接口。

```tsx
/**
 * @private
 * 自定义类实现UIComponent的步骤：
 * 1.在自定义类的构造函数里调用：this.initializeUIValues();
 * 2.拷贝UIComponent接口定义的所有内容(包括注释掉的protected函数)到自定义类，将所有子类需要覆盖的方法都声明为空方法体。
 * 3.在定义类结尾的外部调用sys.implementUIComponent()，并传入自定义类。
 * 4.若覆盖了某个UIComponent的方法，需要手动调用UIComponentImpl.prototype["方法名"].call(this);
 * @param descendant 自定义的UIComponent子类
 * @param base 自定义子类继承的父类
 */
export function implementUIComponent(descendant: any, base: any, isContainer?: boolean): void {
    //拷贝UIComponentImpl模板类的方法体和属性到目标类上。
    mixin(descendant, UIComponentImpl);

    //实现继承
    let prototype = descendant.prototype;
    prototype.$super = base.prototype;

    registerProperty(descendant, "left", "Percentage");
    registerProperty(descendant, "right", "Percentage");
    registerProperty(descendant, "top", "Percentage");
    registerProperty(descendant, "bottom", "Percentage");
    registerProperty(descendant, "horizontalCenter", "Percentage");
    registerProperty(descendant, "verticalCenter", "Percentage");
    if (isContainer) {
        prototype.$childAdded = function (child: egret.DisplayObject, index: number): void {
            this.invalidateSize();
            this.invalidateDisplayList();
        };
        prototype.$childRemoved = function (child: egret.DisplayObject, index: number): void {
            this.invalidateSize();
            this.invalidateDisplayList();
        };
    }
}
```
