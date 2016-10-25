
# EmailAuthor.Style Property (Word)

Returns a  **Style** object that represents the style associated with the current e-mail author for unsent replies, forwards, or new e-mail messages.


## Syntax

 _表达式_. **Style**

 _表达式_ Required. A variable that represents an **[EmailAuthor](2749e018-42e9-7a1a-f18b-8605b38ff0ae.md)** object.


## Example

This example returns the style associated with the current author for unsent replies, forwards, or new e-mail messages and displays the name of the font associated with this style.


```
Set MyEmailStyle = _ 
 ActiveDocument.Email.CurrentEmailAuthor.Style 
Msgbox MyEmailStyle.Font.Name
```


## 另请参阅


#### 概念


[EmailAuthor Object](2749e018-42e9-7a1a-f18b-8605b38ff0ae.md)
#### 其他资源


[EmailAuthor Object Members](http://msdn.microsoft.com/library/76ddf916-7e7f-4a5a-3330-cdb47e2b4d1c%28Office.15%29.aspx)