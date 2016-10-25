
# Options.SnapToShapes Property (Word)

 **True** if Word automatically aligns AutoShapes or East Asian characters with invisible gridlines that go through the vertical and horizontal edges of other AutoShapes or East Asian characters. Read/write **Boolean**.


## Syntax

 _表达式_. **SnapToShapes**

 _表达式_ A variable that represents an **[Options](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)** collection.


## Remarks

This property creates additional invisible gridlines for each AutoShape.  **SnapToShapes** works independently of the **SnapToGrid** property.


## Example

This example sets Word to automatically align AutoShapes with invisible gridlines that go through the vertical and horizontal edges of other AutoShapes in a new document.


```
Options.SnapToShapes = True 
Documents.Add
```


## 另请参阅


#### 概念


[Options Object](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)
#### 其他资源


[Options Object Members](http://msdn.microsoft.com/library/76cd9dfe-6bbb-4c3d-0bfc-79a62bedd15e%28Office.15%29.aspx)