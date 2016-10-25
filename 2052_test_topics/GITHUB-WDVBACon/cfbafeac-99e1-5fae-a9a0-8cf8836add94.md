
# Selection.ClearParagraphStyle Method (Word)

Removes paragraph formatting that has been applied through paragraph styles from the selected text.


## Syntax

 _表达式_. **ClearParagraphStyle**

 _表达式_ An expression that returns a **[Selection](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)** object.


## Remarks

This method does not remove paragraph formatting that a user has applied manually. To remove manually applied paragraph formatting, use the  **[ClearParagraphDirectFormatting](66df2319-f02e-7cd9-4cef-fda6468dcd67.md)** method. To remove all paragraph formatting, both style and manual formatting, use the **[ClearParagraphAllFormatting](b3a88322-933a-ff14-e788-e1934aba243d.md)** method.


 **注释**  To remove character formatting, see the  **[ClearCharacterAllFormatting](1d0dfb43-4855-1534-5ec2-475232a6a457.md)**, **[ClearCharacterDirectFormatting](d2138876-c832-2407-a53e-5bd4af2421b7.md)**, or **[ClearCharacterStyle](ff9795f9-ea74-fa03-5d87-9c56152d179d.md)** method.


## 另请参阅


#### 概念


[Selection Object](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)
#### 其他资源


[Selection Object Members](http://msdn.microsoft.com/library/71e67a43-d40a-ad9a-8ef2-c5c487733e0d%28Office.15%29.aspx)