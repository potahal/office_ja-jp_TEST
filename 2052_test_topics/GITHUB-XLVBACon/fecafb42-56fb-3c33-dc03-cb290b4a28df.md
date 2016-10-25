
# Chart.AutoScaling Property (Excel)

 **True** if Microsoft Excel scales a 3-D chart so that it's closer in size to the equivalent 2-D chart. The **[RightAngleAxes](632aa454-4113-97d3-a80c-eb745a950c6f.md)** property must be **True**. Read/write **Boolean**.


## Syntax

 _表达式_. **AutoScaling**

 _表达式_ A variable that represents a **Chart** object.


## Example

This example automatically scales Chart1. The example should be run on a 3-D chart.


```
With Charts("Chart1") 
 .RightAngleAxes = True 
 .AutoScaling = True 
End With
```


## 另请参阅


#### 概念


[Chart Object](179c32ce-49bd-6f36-ea12-89fb5443f3ea.md)
#### 其他资源


[Chart Object Members](http://msdn.microsoft.com/library/a3f8ac44-02d6-6f3f-b5e0-23f4bd5d6baf%28Office.15%29.aspx)