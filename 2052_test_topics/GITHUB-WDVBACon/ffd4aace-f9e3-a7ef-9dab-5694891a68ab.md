
# Document.ManualHyphenation Method (Word)

Initiates manual hyphenation of a document, one line at a time.


## Syntax

 _表达式_. **ManualHyphenation**

 _表达式_ Required. A variable that represents a **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object.


## Remarks

When you use the  **ManualHyphenation** method, Word prompts he user to accept or decline suggested hyphenations.


## Example

This example starts manual hyphenation of the active document.


```
ActiveDocument.ManualHyphenation
```

This example sets hyphenation options and then starts manual hyphenation of MyDoc.doc.




```
With Documents("MyDoc.doc") 
 .HyphenationZone = InchesToPoints(0.25) 
 .HyphenateCaps = False 
 .ManualHyphenation 
End With
```


## 另请参阅


#### 概念


[Document Object](8d83487a-2345-a036-a916-971c9db5b7fb.md)
#### 其他资源


[Document Object Members](http://msdn.microsoft.com/library/fc9ab457-0888-f917-3d52-387168ac23b9%28Office.15%29.aspx)