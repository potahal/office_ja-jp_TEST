
# Options.DisplayGridLines Property (Word)

 **True** if Microsoft Word displays the document grid. This property is the equivalent of the **Gridlines** command on the **View** menu. Read/write **Boolean**.


## Syntax

 _表达式_. **DisplayGridLines**

 _表达式_ A variable that represents a **[Options](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)** object.


## Remarks

This property affects only the document grid. For table gridlines, use the  **[TableGridlines](02ef1d7b-185b-ed17-e811-a752faa11b3f.md)** property.


## Example

This example switches between displaying and hiding the document grid in the active window.


```
Options.DisplayGridLines = Not Options.DisplayGridLines
```


## 另请参阅


#### 概念


[Options Object](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)
#### 其他资源


[Options Object Members](http://msdn.microsoft.com/library/76cd9dfe-6bbb-4c3d-0bfc-79a62bedd15e%28Office.15%29.aspx)