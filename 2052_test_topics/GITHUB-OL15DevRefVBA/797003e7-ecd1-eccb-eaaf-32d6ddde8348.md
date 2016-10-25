
# 应用程序对象 (Outlook)

代表整个 Microsoft Outlook 应用程序。


## 注解

这是可以通过使用 **[无论](09b6ff5b-a750-c07d-7499-c1f8a00214fe.md)** 返回层次结构中的唯一对象 方法或内部的 Visual Basic **GetObject** 函数。

Outlook **应用程序** 对象具有以下几个目的︰


- 作为根对象，允许访问 Outlook 层次结构中的其他对象。
    
- 允许直接访问使用  **[CreateItem](e5fbf367-db16-5042-823e-68e6b805e612.md)** 创建的新项目，而不必遍历对象层次结构。
    
- 允许访问活动界面对象（浏览器和检查器）。
    
自动化控制 Outlook 使用从其他应用程序时，使用 **CreateObject** 方法来创建一个 Outlook **应用程序** 对象。


## 示例

下面的 Visual Basic for Applications (VBA) 示例启动 Outlook（如果它尚未运行）并打开默认的"收件箱"文件夹。


```
Set myNameSpace = Application.GetNameSpace("MAPI") 
 
Set myFolder= _ 
 
 myNameSpace.GetDefaultFolder(olFolderInbox) 
 
myFolder.Display
```

下面的 Visual Basic for Applications (VBA) 示例使用 **应用程序** 对象创建并打开一个新的联系人。




```
Set myItem = Application.CreateItem(olContactItem) 
 
myItem.Display
```


## 事件



|**名称**|
|:-----|
|[AdvancedSearchComplete](4f33ad44-20a3-62cd-aa1b-db74581ebb3c.md)|
|[AdvancedSearchStopped](a1a4ec9f-c0e3-6acd-b63c-89194ed70efd.md)|
|[BeforeFolderSharingDialog](e06257eb-f2d9-63cf-1220-dda55ee0ea14.md)|
|[ItemLoad](aed0656d-4e5a-550a-1116-76773215a897.md)|
|[ItemSend](54f506ea-87a2-29b9-2b33-67bc87167933.md)|
|[MAPILogonComplete](db6f7cf8-2a45-560f-f592-613de86e08e2.md)|
|[NewMail](cfc848e8-98b1-163a-c177-53993c20bb14.md)|
|[NewMailEx](3b6873a3-0ccf-0e46-1cac-0eeabb3a896b.md)|
|[OptionsPagesAdd](aa13cd97-de96-00f8-a532-ca8ee9b00343.md)|
|[退出](ecf0b50b-db6f-7eaf-90bd-bae942bf9287.md)|
|[提醒](f8c9fa87-3daa-58e1-7b8d-3c819cd4cab2.md)|
|[启动](d4724d96-2572-b1e3-e202-0bfffb5cf7d5.md)|

## 方法



|**名称**|
|:-----|
|[ActiveExplorer](f6dd27c0-4319-c7fc-191f-8b3b2ea319d3.md)|
|[ActiveInspector](3f2b6491-7b4b-8165-327e-b319711d5656.md)|
|[ActiveWindow](5f5b4e8b-61e4-417b-6b0c-14d1ccb41594.md)|
|[AdvancedSearch](7b433d8b-08b9-dff1-b854-287d76b47a90.md)|
|[CopyFile](dc848d48-23e0-d0a9-049d-b2ae414151d5.md)|
|[CreateItem](e5fbf367-db16-5042-823e-68e6b805e612.md)|
|[CreateItemFromTemplate](5e6c0ec4-779d-3743-afdb-606ad512ba95.md)|
|[CreateObject](09b6ff5b-a750-c07d-7499-c1f8a00214fe.md)|
|[GetNamespace](6175d0d9-5a61-ce45-35c0-b70895d757b3.md)|
|[GetObjectReference](426ade68-155b-9076-b3f8-4108f44688b0.md)|
|[IsSearchSynchronous](cd757b43-5e3f-1504-9944-7431bda6f004.md)|
|[退出](664bc8ba-ad97-8d4f-02f9-7f9bdd04beea.md)|
|[RefreshFormRegionDefinition](35183f18-7c59-80c5-e281-af15afe39198.md)|

## 属性



|**名称**|
|:-----|
|[应用程序](c49cfea1-d126-75eb-fb3d-6f040526cef0.md)|
|[寻求帮助](14d6eb82-82ab-ea67-6a0b-103a535b8d41.md)|
|[类](5bfb1d90-8c16-fdbe-374f-0b10d64915c3.md)|
|[COMAddIns](f911199d-dc2e-9b88-d807-a5737a39f29e.md)|
|[DefaultProfileName](53c6a189-9337-6413-72e5-bf6ea8794361.md)|
|[资源管理器](bbbdbd6e-a238-8108-fbbd-5f7d7821aaa7.md)|
|[检查器](c2dde847-d033-90e3-30d2-62ff375d6843.md)|
|[IsTrusted](4caeb41a-9cc3-1195-22a9-ad8eae12ce53.md)|
|[LanguageSettings](8367a51a-629f-3349-fe0b-a978b2bbc9a5.md)|
|[名称](a0ac022e-4d46-fffb-aa13-f95249e30bdb.md)|
|[父级](d83e85a0-f3d4-bf95-0568-0411a5d09350.md)|
|[PickerDialog](14acc98b-c234-d59b-d089-d6782ffb08a0.md)|
|[ProductCode](cdb4678a-fa6b-7d4f-b0b1-b34811749bf5.md)|
|[提醒](1f5428f0-6362-a691-2fad-c80e48dce3f5.md)|
|[会话](720b2849-fe01-afb3-363c-f3bf0cd7d872.md)|
|[TimeZones](920e55d1-9914-fa74-101a-921083328d23.md)|
|[版本](08a74ab8-7e02-3956-1827-4b6690acdec1.md)|

## 另请参阅


#### 其他资源


[应用程序对象的成员](3519c89c-2353-85ee-7ddc-62e5dd85a8e7.md)
[Outlook 对象模型引用](http://msdn.microsoft.com/library/73221b13-d8d8-99b8-3394-b95dbbfd5ddc%28Office.15%29.aspx)