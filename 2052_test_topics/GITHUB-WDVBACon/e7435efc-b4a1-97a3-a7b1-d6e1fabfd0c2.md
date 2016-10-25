
# EmailOptions.AutoFormatAsYouTypeApplyTables Property (Word)

 **True** if Word automatically creates a table when you type a plus sign, a series of hyphens, another plus sign, and so on, and then press ENTER. Read/write **Boolean**.


## Syntax

 _表达式_. **AutoFormatAsYouTypeApplyTables**

 _表达式_ A variable that represents an **[EmailOptions](41fefa03-c993-e218-0f92-0cf30c0bfbd4.md)** collection.


## Remarks

The plus signs become the column borders, and the hyphens become the column widths. 


## Example

This example sets Word to automatically create tables as you type.


```
Options.AutoFormatAsYouTypeApplyTables = True
```

This example returns the status of the  **Tables** option on the **AutoFormat As You Type** tab in the **AutoCorrect** dialog box ( **Tools** menu).




```
Dim blnAutoFormat as Boolean 
 
blnAutoFormat = Options.AutoFormatAsYouTypeApplyTables
```


## 另请参阅


#### 概念


[EmailOptions Object](41fefa03-c993-e218-0f92-0cf30c0bfbd4.md)
#### 其他资源


[EmailOptions Object Members](http://msdn.microsoft.com/library/0f8a549b-283c-dc9d-dc1e-1179a9d6fb0b%28Office.15%29.aspx)