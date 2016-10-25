
# AutoCaption.AutoInsert Property (Word)

 **True** if a caption is automatically added when the item is inserted into a document. Read/write **Boolean**.


## Syntax

 _表达式_. **AutoInsert**

 _表达式_ A variable that represents an **[AutoCaption](895b5181-d36f-7f63-572a-c2d37c878e17.md)** object.


## Example

This example enables Word to add captions to tables automatically. Then the example collapses the selection to an insertion point, and inserts a table. A caption is automatically added to the new table.


```
AutoCaptions("Microsoft Word Table").AutoInsert = True 
Selection.Collapse Direction:=wdCollapseStart 
ActiveDocument.Tables.Add Range:=Selection.Range, _ 
 NumRows:=2, NumColumns:=2
```


## 另请参阅


#### 概念


[AutoCaption Object](895b5181-d36f-7f63-572a-c2d37c878e17.md)
#### 其他资源


[AutoCaption Object Members](http://msdn.microsoft.com/library/48332cba-c2a5-a641-dc08-4cc2774ee5e6%28Office.15%29.aspx)