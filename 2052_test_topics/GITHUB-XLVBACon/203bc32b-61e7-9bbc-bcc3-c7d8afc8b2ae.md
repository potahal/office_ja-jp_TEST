
# ChartGroups 集合 (Excel)

指定图表中的所有 **[ChartGroup](8a485a8c-e181-a039-60b9-a02c2c89b26e.md)** 对象的集合。每个 **ChartGroup** 对象表示图表中的同一格式绘制的一个或多个系列。一张图表包含一个或多个图表组，每个图表组包含一个或多个系列，每个数据系列包含一个或多个点。例如，单张图表可能既包含折线图图表组，其中包含所有用折线图格式，绘制的数据系列也条形图图表组，其中包含所有用条形图格式绘制的数据系列。


## ChartGroups 集合用法

使用  **ChartGroups** 方法可返回 **ChartGroups** 集合。下例显示图表中图表组的数目。


```
MsgBox myChart.ChartGroups.Count
```

使用  **ChartGroups** ( _index_ ) 可返回单个 **ChartGroup** 对象，其中 _index_ 为图表组的索引号。下例向图表中的第一个图表组添加垂直线。




```
myChart.ChartGroups(1).HasDropLines = True
```

因为当特定图表组所用的图表格式改变时，该图表组的索引号可能改变，所以使用图表组的命名快捷方法来返回特定的图表组可能更方便。 **PieGroups** 方法返回指定图表中饼图图表组的集合， **LineGroups** 方法返回折线图图表组的集合，依此类推。使用这些方法时，都可以用索引号返回单个 **ChartGroup** 对象或不指定索引号而返回 **ChartGroups** 集合。以下方法适用于图表组：


-  **[AreaGroups](ec2a4a28-2f10-4f4f-bd91-642bf1b8ebe2.md)** 方法
    
-  **[BarGroups](a00e484e-05ec-2eaa-cc33-05b77a4af0b5.md)** 方法
    
-  **[ColumnGroups](dcb4d7e0-ce56-46d9-35d9-d9653bbb6f97.md)** 方法
    
-  **[DoughnutGroups](41ca4213-c17b-7bba-c357-7ba65fd55d39.md)** 方法
    
-  **[LineGroups](3a8083b5-8b71-e28b-c775-6be50544d6b2.md)** 方法
    
-  **[PieGroups](f7fd5497-f7a0-6c28-1a59-9e6f37a0885e.md)** 方法
    
