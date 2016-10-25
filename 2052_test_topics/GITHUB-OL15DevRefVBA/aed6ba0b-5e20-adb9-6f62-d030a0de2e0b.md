
# Store.GetSearchFolders 方法 （Outlook）

返回一个  **[Folders](0c814c3c-74fc-414c-982d-a0097fcb35c2.md)** 集合对象，该对象代表为 **[Store](1eb22fe9-8849-7476-5388-2515b48591b9.md)** 对象定义的搜索文件夹。


## 语法

 _表达式_. **GetSearchFolders**

 _表达式_ 一个代表 **Store** 对象的变量。


### 返回值

表示 **存储** 对象的所有搜索文件夹的 **文件夹** 集合对象。


## 说明

 **GetSearchFolders** 返回的 **存储区** 所有可见的活动的搜索文件夹。它将不再返回未初始化或已清除的搜索文件夹。

 **GetSearchFolders** 返回 **Folders** 集合对象与 **[Folders.Count](b1884cc1-5b50-0ea8-315a-3616d11db0e6.md)** 等于零 (0) 如果没有搜索文件夹已定义 **存储** 。

对于 **文件夹** 集合对象表示一个集合的搜索文件夹， **[Folders.Parent](4fe483ec-7e6e-ca82-8a1d-d039a7b9e89c.md)** 返回与 **[Store.GetRootFolder](09da4d57-c33d-6946-cc21-7233e89efb10.md)** 相同的对象。 **[Folder.Folders](41464c32-023e-9079-4f24-51586305325c.md)** 返回 **Null** ( **不** 在 Visual Basic 中)。


## 示例

以下 Microsoft Visual Basic for Applications (VBA) 代码示例枚举当前会话的所有存储区上的搜索文件夹。


```
Sub EnumerateSearchFoldersInStores() 
 
 Dim colStores As Outlook.Stores 
 
 Dim oStore As Outlook.Store 
 
 Dim oSearchFolders As Outlook.folders 
 
 Dim oFolder As Outlook.Folder 
 
 
 
 On Error Resume Next 
 
 Set colStores = Application.Session.Stores 
 
 For Each oStore In colStores 
 
 Set oSearchFolders = oStore.GetSearchFolders 
 
 For Each oFolder In oSearchFolders 
 
 Debug.Print (oFolder.FolderPath) 
 
 Next 
 
 Next 
 
End Sub
```


## 另请参阅


#### 概念


[存储对象](1eb22fe9-8849-7476-5388-2515b48591b9.md)
#### 其他资源


[存储对象的成员](84c1d423-e507-0b3b-6570-33829b94be04.md)