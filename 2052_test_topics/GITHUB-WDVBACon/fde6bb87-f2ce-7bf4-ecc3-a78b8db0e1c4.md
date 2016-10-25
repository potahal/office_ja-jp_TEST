
# Endnote.Range Property (Word)

Returns a  **Range** object that represents the portion of a document that is contained in the specified object.


## Syntax

 _表达式_. **Range**

 _表达式_ Required. A variable that represents an **[Endnote](01f29be4-58e7-28f5-5fcb-dae50c33890e.md)** object.


## Remarks

For information about returning a range from a document or returning a shape range from a collection of shapes, see the  **Range** method.


## Example

This example changes the text of the first endnote in the active document.


```
With ActiveDocument.Endnotes(1).Range 
 .Delete 
 .Text = "new endnote text" 
End With
```


## 另请参阅


#### 概念


[Endnote Object](01f29be4-58e7-28f5-5fcb-dae50c33890e.md)
#### 其他资源


[Endnote Object Members](http://msdn.microsoft.com/library/5744789b-dbe0-594a-54d9-82acc41d2c7a%28Office.15%29.aspx)