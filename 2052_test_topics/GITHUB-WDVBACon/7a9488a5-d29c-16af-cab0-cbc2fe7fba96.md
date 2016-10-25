
# Conflicts.Count Property (Word)

Returns the number of items in the  **Conflicts** collection. Read-only.


## Syntax

 _表达式_. **Count**

 _表达式_ An expression that returns a **Conflicts** object.


## Example

The following code example gets the number of  **Conflict** objects in the active document.


```
Dim confCount as Long 
 
confCount = ActiveDocument.CoAuthoring.Conflicts.Count 

```


## 另请参阅


#### 概念


[Conflicts Object](476e8f6d-c93e-b372-2fa7-1c9a4a84a182.md)
#### 其他资源


[Conflicts Object Members](http://msdn.microsoft.com/library/395fd60d-6772-9e2a-83b8-562b3c6c6342%28Office.15%29.aspx)