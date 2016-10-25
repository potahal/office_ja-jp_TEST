
# MinimumScale 属性

返回或设置坐标轴的最小刻度值。 **Double** 类型，可读写。


## 说明

设置此属性将 **[MinimumScaleIsAuto](95ed7a2b-efda-b05a-da2e-789a166a97c8.md)** 属性设置为 **False** 。


## 示例

以下示例设置数值轴的最小和最大刻度值。


```
With myChart.Axes(xlValue) 
 .MinimumScale = 10 
 .MaximumScale = 120 
End With
```

