
# VaryByCategories 属性

如果 Microsoft Graph 为每个数据标记置以不同的颜色或图案，则该属性值为  **True** 。本属性所应用的图表只能包含一个数据系列。 **Boolean** 类型，可读写。


## 示例

本示例对第一个图表组中的每个数据标记指定一种不同的颜色或图案。本示例应在数据系列上有数据标记的二维折线图上运行。


```
myChart.ChartGroups(1).VaryByCategories = True
```

