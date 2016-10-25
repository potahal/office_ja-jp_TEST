
# ContactItem.AddPicture 方法 （Outlook）

向联系人项目添加图片。


## 语法

 _表达式_. **AddPicture**( ** _Path_** )

 _表达式_ 一个代表 **ContactItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Path_|必需|**String**|包含要添加到联系人项目的图片的完整路径和文件名的字符串。|

## 注解

如果联系人项目已附有图片，此方法将覆盖现有图片。

图片可以是图标、GIF、JPEG、BMP、TIFF、WMF、EMF 或 PNG 文件。Microsoft Outlook 会对图片自动进行必要的大小调整。


## 示例

以下 Microsoft Visual Basic for Applications (VBA) 示例提示用户指定联系人姓名和含有联系人图片的文件名称，然后将图片添加到联系人项目。如果联系人项目图片已经存在，将提示用户是否用新文件覆盖现有图片。


```
Sub AddPictureToAContact() 
 
 Dim myNms As Outlook.NameSpace 
 
 Dim myFolder As Outlook.Folder 
 
 Dim myContactItem As Outlook.ContactItem 
 
 Dim strName As String 
 
 Dim strPath As String 
 
 Dim strPrompt As String 
 
 
 
 Set myNms = Application.GetNamespace("MAPI") 
 
 Set myFolder = myNms.GetDefaultFolder(olFolderContacts) 
 
 strName = InputBox("Type the name of the contact: ") 
 
 Set myContactItem = myFolder.Items(strName) 
 
 If myContactItem.HasPicture = True Then 
 
 strPrompt = MsgBox("The contact already has a picture associated with it. Do you want to overwrite the existing picture?", vbYesNo) 
 
 If strPrompt = vbNo Then 
 
 Exit Sub 
 
 End If 
 
 End If 
 
 strPath = InputBox("Type the file name for the contact: ") 
 
 myContactItem.AddPicture (strPath) 
 
 myContactItem.Save 
 
 myContactItem.Display 
 
 End Sub
```


## 另请参阅


#### 概念


[创建 ContactItem 对象](8e32093c-a678-f1fd-3f35-c2d8994d166f.md)
#### 其他资源


[创建 ContactItem 对象成员](a8b13369-4c87-02aa-e62a-1f3067e559fa.md)