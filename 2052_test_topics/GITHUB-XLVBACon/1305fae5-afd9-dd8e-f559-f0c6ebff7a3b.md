
# AxisTitle.Text Property (Excel)

Returns or sets the text for the specified object. Read/write  **String**.


## Syntax

 _表达式_. **Text**

 _表达式_ A variable that represents an **AxisTitle** object.


## Example

This example sets the axis title text for the category axis in Chart1.


```
With Charts("Chart1").Axes(xlCategory) 
 .HasTitle = True 
 .AxisTitle.Text = "Month" 
End With
```


## 另请参阅


#### 概念


[AxisTitle Object](563d3ba5-aa77-b6fc-236a-7838d75eaa53.md)
#### 其他资源


[AxisTitle Object Members](http://msdn.microsoft.com/library/84970b5a-91a1-b785-5632-97a0de4410f2%28Office.15%29.aspx)