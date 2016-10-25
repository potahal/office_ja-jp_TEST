
# FillFormat.OneColorGradient Method (Excel)

Sets the specified fill to a one-color gradient.


## Syntax

 _表达式_. **OneColorGradient**( ** _Style_**, ** _Variant_**, ** _Degree_** )

 _表达式_ A variable that represents a **FillFormat** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Style_|必需|**[MsoGradientStyle](http://msdn.microsoft.com/library/1f0e723f-293c-3646-fd77-da2c8842c71f%28Office.15%29.aspx)**|The gradient style.|
| _Variant_|必需|**Integer**|The gradient variant. Can be a value from 1 through 4, corresponding to one of the four variants on the  **Gradient** tab in the **Fill Effects** dialog box. If _GradientStyle_ is **msoGradientFromCenter**, the _Variant_ argument can only be 1 or 2.|
| _Degree_|必需|**Single**|The gradient degree. Can be a value from 0.0 (dark) through 1.0 (light).|

## 另请参阅


#### 概念


[FillFormat Object](b602e09e-97ab-bfbe-1796-bc44ebb7dc28.md)
#### 其他资源


[FillFormat Object Members](http://msdn.microsoft.com/library/da1a1680-4b9d-c6fb-6562-bf1ec9f57921%28Office.15%29.aspx)