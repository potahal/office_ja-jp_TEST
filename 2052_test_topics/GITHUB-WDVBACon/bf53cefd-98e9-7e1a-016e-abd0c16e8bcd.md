
# Document.EndReview Method (Word)

Terminates a review of a file that has been sent for review using the  **[SendForReview](2f2cdd5c-eeca-d03f-bd58-b5586f8f461f.md)** method or that has been automatically placed in a review cycle by sending a document to another user in an e-mail message.


## Syntax

 _表达式_. **EndReview**

 _表达式_ Required. A variable that represents a **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object.


## Remarks

When executed, the  **EndReview** method displays a message asking the user whether to end the review.


## Example

This example terminates the review of the active document. This example assumes the active document part of a review cycle.


```
Sub EndDocRev() 
 ActiveDocument.EndReview 
End Sub
```


## 另请参阅


#### 概念


[Document Object](8d83487a-2345-a036-a916-971c9db5b7fb.md)
#### 其他资源


[Document Object Members](http://msdn.microsoft.com/library/fc9ab457-0888-f917-3d52-387168ac23b9%28Office.15%29.aspx)