
# MarkerBackgroundColor 属性

以 RGB 值返回或设置数据标记的背景色。仅适用于折线图、散点图和雷达图。 **Long** 类型，可读写。


## 示例

以下示例设置第一个数据系列中第二个数据点的数据标记的背景色和前景色。


```
With myChart.SeriesCollection(1).Points(2) 
 .MarkerBackgroundColor = RGB(0,255,0) ' green 
 .MarkerForegroundColor = RGB(255,0,0) ' red 
End With
```

