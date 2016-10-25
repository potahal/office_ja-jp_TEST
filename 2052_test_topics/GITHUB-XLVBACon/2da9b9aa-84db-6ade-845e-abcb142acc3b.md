
# BarShape 属性

返回或设置用于指定三维条形图或柱形图的形状。读/写 XlBarShape。


||
|:-----|
|XlBarShape 可为以下 XlBarShape 常量之一。|
|**xlConeToMax**|
|**xlCylinder**|
|**xlPyramidToPoint**|
|**xlBox**|
|**xlConeToPoint**|
|**xlPyramidToMax**|

 _expression_. **BarShape**

 _表达式_ 必需。该表达式返回"应用于"列表中的一个对象。

## 示例

以下示例设置用于图表上第一个数据系列的形状。


```
myChart.SeriesCollection(1).BarShape = xlConeToPoint
```

