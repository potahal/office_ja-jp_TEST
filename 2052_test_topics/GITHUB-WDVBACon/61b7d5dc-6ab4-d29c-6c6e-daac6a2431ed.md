
# Document.Frames Property (Word)

Returns a  **[Frames](d0f526b5-ae1d-ad7a-0da3-5a7b30526b55.md)** collection that represents all the frames in a document. Read-only.


## Syntax

 _表达式_. **Frames**

 _表达式_ A variable that represents a **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object.


## Remarks

For information about returning a single member of a collection, see [Returning an Object from a Collection](28f76384-f495-9640-a7c8-10ada3fac727.md).


## Example

This example adds a frame around the selection and returns a frame object to the myFrame variable.


```
Set myFrame = ActiveDocument.Frames.Add(Range:=Selection.Range)
```


## 另请参阅


#### 概念


[Document Object](8d83487a-2345-a036-a916-971c9db5b7fb.md)
#### 其他资源


[Document Object Members](http://msdn.microsoft.com/library/fc9ab457-0888-f917-3d52-387168ac23b9%28Office.15%29.aspx)