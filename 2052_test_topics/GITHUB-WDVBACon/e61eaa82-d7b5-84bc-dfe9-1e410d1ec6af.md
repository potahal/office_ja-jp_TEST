
# Index.NumberOfColumns Property (Word)

Sets or returns the number of columns for each page of an index. Read/write  **Long**.


## Syntax

 _表达式_. **NumberOfColumns**

 _表达式_ An expression that an **[Index](6a2aab98-485b-01c3-8d9b-9e108b455e22.md)** object.


## Remarks

Specifying 0 (zero) sets the number of columns in the index to the same number as in the document.


## Example

This example sets the number of columns in the first index to the same number as in the active document.


```
ActiveDocument.Indexes(1).NumberOfColumns = 0
```

This example sets a two-column format for each index in the active document.




```
For Each myIndex In ActiveDocument.Indexes 
 myIndex.NumberOfColumns = 2 
Next myIndex
```


## 另请参阅


#### 概念


[Index Object](6a2aab98-485b-01c3-8d9b-9e108b455e22.md)
#### 其他资源


[Index Object Members](http://msdn.microsoft.com/library/de9f0a3c-dd30-84bd-e122-2d20fa6b3d37%28Office.15%29.aspx)