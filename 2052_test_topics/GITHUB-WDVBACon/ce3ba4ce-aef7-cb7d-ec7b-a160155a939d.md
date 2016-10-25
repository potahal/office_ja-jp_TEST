
# ContentControl.Copy Method (Word)

Copies the content control from the active document to the Clipboard.


## Syntax

 _表达式_. **Copy**

 _表达式_ An expression that returns a **ContentControl** object.


## Remarks

When you use the  **Copy** method, the original content control remains in the active document, but a copy of the control, including all text and property settings, is moved to the Clipboard. You can then paste the content control into other sections of the active document. Use the **Paste** method of the **[Selection](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)** object or the **Paste** method of the **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object to insert the copied content control, or use the **Paste** function from within Microsoft Word.


## 另请参阅


#### 概念


[ContentControl Object](783dec26-9b63-11f8-6187-985f9c815f27.md)
#### 其他资源


[ContentControl Object Members](http://msdn.microsoft.com/library/d5aa195c-8d7a-0bad-09fa-6f1bfc9828cc%28Office.15%29.aspx)