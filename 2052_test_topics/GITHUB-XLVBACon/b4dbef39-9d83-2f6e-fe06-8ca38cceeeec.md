
# HasLegend 属性

如果图表有图例，则该属性值为  **True** 。 **Boolean** 类型，可读写。


## 示例

以下示例打开图表的图例，并将图例字体颜色设置为蓝色。


```
With myChart 
 .HasLegend = True 
 .Legend.Font.ColorIndex = 5 
End With
```

