# uni-tooltips 工具提示组件
兼容微信小程序，h5，uni-app

工具提示组件，为了解决工具提示的问题。
组件名：uni-tooltips，代码块： uniTooltips。

# 案例

[demo 项目地址：https://github.com/bianmaren/uni-tooltips-example](https://github.com/bianmaren/uni-tooltips-example),

![demo.png](https://file.jxyunge.com/Sw5yEFc3P8mbGBKDBeb7_1580876419165.png)


# 使用方式
在 `script`  中引用组件

```
import bianmarenTooltip from '@/components/bianmaren-tooltips/bianmaren-tooltips.vue'
export default {
    components: {bianmarenTooltip}
}
```

在 `template` 中使用组件

```
<bianmarenTooltip
    :gravity="gravity"
    :tooltipShow="tooltipShow"
    :btns="tooltipBtns"
    :eleId="eleId"
    @btnClick="click"></bianmarenTooltip>
```

# 属性说明

|属性名|类型|默认值|说明|
|:---|:---:|---:|---:|
|tooltipShow|Boolean|false|是否显示|
|textColor|String|#ffffff|字体颜色|
|backgroundColor|String|#000000|背景颜色|
|splitColor|String|#ffffff|分隔符颜色|
|btns|Array|['HelloWorld']|提示里面显示的按钮|
|eleId|String|空|挂载元素ID|
|gravity|String|bottom,top,left,right|方向|
|distance|Number|10|工具提示距离挂载元素距离|

# 事件说明

|事件称名|说明|返回值|
|:---|:---:|---:|   
|btnClick|按钮点击|按钮名称|

