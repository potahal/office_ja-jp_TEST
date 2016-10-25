
# DataLabel 对象

表示指定的点或图表中的趋势线的数据标签。系列的 **数据标签** 对象是 **[数据标签](597c7269-71ed-5dcc-af6b-34dc908e9d58.md)** 集合，其中包含的每个点的 **数据标签** 对象的成员。对于没有可定义点 (如区域系列)， **数据标签** 集合包含单个 **数据标签** 对象。


## DataLabel 对象用法

使用  **DataLabels** ( _index_ )（其中 _index_ 为数据标签的索引号）可返回单个 **DataLabel** 对象。下例设置图表上第一个数据系列中的第五个数据标签的数字格式。


```
myChart.SeriesCollection(1).DataLabels(5).NumberFormat = "0.000"
```

使用  **DataLabel** 属性可返回单个数据点的 **DataLabel** 对象。下例打开图表上第一个数据系列中第二个数据点的数据标签，并将该数据标签的文字设置为"Saturday"。




```
With myChart 
 With .SeriesCollection(1).Points(2) 
 .HasDataLabel = True 
 .DataLabel.Text = "Saturday" 
 End With 
End With
```

对于趋势线， **DataLabel** 属性返回与趋势线一起显示的文本。这些文本可能是公式、R 平方值或两者均有（如果两者都出现）。下例使趋势线的文本中仅出现公式，并将数据标签文本置于数据表上的单元格 A1 中。




```
With myChart.SeriesCollection(1).Trendlines(1) 
 .DisplayRSquared = False 
 .DisplayEquation = True 
 x = .DataLabel.Text 
End With 
With myChart.Application.DataSheet 
 .Range("A1").Value = x 
End With
```

