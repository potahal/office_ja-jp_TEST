
# NavigationFolder 对象 （Outlook）

代表导航窗格内导航模块的导航组中显示的导航文件夹。


## 说明

使用 **[Item](1688b2ef-a4a1-fc8a-513e-0d5e234f10dd.md)** 方法可从父 **[NavigationGroup](a96eb2b1-af1f-71b2-6a0b-dcb5078beb1f.md)** 对象的 **[NavigationFolders](ecff93b8-0c3f-5f31-5b61-c46d2622d2af.md)** 集合中检索一个 **NavigationFolder** 对象。使用 **NavigationFolders** 集合的 **[Add](f88fd69a-8684-bfc4-bc20-1cff5c44974e.md)** 方法来创建一个新的 **NavigationFolder** 对象，基于现有的 **[文件夹](3cf6cda8-6d70-666e-2643-9d9c5b9cacfc.md)** 对象。

使用 **[文件夹](0d8edd40-3f8d-dc2b-5cba-80ed1662cc48.md)** 方法可以返回或设置一个 **NavigationFolder** 对象所基于的 **文件夹** 对象。

使用  **[IsSelected](a8fb9430-0477-2417-0dba-e30e9f8ebe8d.md)** 属性可以确定导航文件夹是否被选中，使用 **[Position](cfa86104-c191-51f8-4da3-dc3c26d6a7ed.md)** 属性可以返回或设置导航窗格中导航文件夹的显示位置。还可以使用 **[DisplayName](51bdcbaf-0fa7-8cba-953d-13da4a5abc27.md)** 属性返回导航窗格中导航文件夹的显示名称。

使用 **[IsRemovable](9fff5f32-2ac4-5ed3-c6d5-10962de8b34f.md)** 属性以确定是否能从 **NavigationFolders** 集合和 **[IsSideBySide](00a49ce6-ad74-1f24-2aaa-e79a3409c9c9.md)** 属性返回或设置与 **[CalendarModule](9203024d-9cef-75e0-600f-f3899e24761a.md)** 对象关联的导航文件夹的查看模式中删除导航文件夹。


## 另请参阅


#### 其他资源


[NavigationFolder 对象成员](1ec2e16d-c7ca-86b1-9283-839a2b9aca05.md)
[Outlook 对象模型引用](http://msdn.microsoft.com/library/73221b13-d8d8-99b8-3394-b95dbbfd5ddc%28Office.15%29.aspx)