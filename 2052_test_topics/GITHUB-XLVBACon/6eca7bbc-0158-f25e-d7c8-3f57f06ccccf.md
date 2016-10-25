
# ChartTitle 对象

代表指定图表的标题。


## ChartTitle 对象用法

使用  **ChartTitle** 属性可返回 **ChartTitle** 对象。下例向图表中添加标题。


```
With myChart 
 .HasTitle = True 
 .ChartTitle.Text = "February Sales" 
End With
```


## 说明

 **ChartTitle** 对象不存在，并不能使用，除非该图表的 **[HasTitle](9ecc48d3-fd86-e185-a69f-0676230b3194.md)** 属性为 **True** 。

