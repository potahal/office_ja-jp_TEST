
# Series.HasErrorBars Property (Excel)

 **True** if the series has error bars. This property isn't available for 3-D charts. Read/write **Boolean**.


## Syntax

 _表达式_. **HasErrorBars**

 _表达式_ A variable that represents a **Series** object.


## Example

This example removes error bars from series one in Chart1. The example should be run on a 2-D line chart that has error bars for series one.


```
Charts("Chart1").SeriesCollection(1).HasErrorBars = False
```


## 另请参阅


#### 概念


[Series Object](c7d34b32-8172-f7a0-0a17-f01d44246b64.md)
#### 其他资源


[Series Object Members](http://msdn.microsoft.com/library/eeab4f69-b436-9de7-5d4a-0a5c63f2dfce%28Office.15%29.aspx)