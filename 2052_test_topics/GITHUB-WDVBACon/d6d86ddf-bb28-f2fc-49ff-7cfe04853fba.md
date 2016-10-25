
# Sections.PageSetup Property (Word)

Returns a  **PageSetup** object that's associated with the specified document, range, section, sections, or selection.


## Syntax

 _表达式_. **PageSetup**

 _表达式_ A variable that represents a **[Sections](cf6f77ba-9eee-5614-e697-bc031c4c6dcd.md)** collection.


## Example

This example sets the gutter for the first section in Summary.doc to 36 points (0.5 inch).


```
Documents("Summary.doc").Sections(1).PageSetup.Gutter = 36
```


## 另请参阅


#### 概念


[Sections Collection Object](cf6f77ba-9eee-5614-e697-bc031c4c6dcd.md)
#### 其他资源


[Sections Object Members](http://msdn.microsoft.com/library/adbf6532-f5f6-dece-837d-9ae3b38a0da2%28Office.15%29.aspx)