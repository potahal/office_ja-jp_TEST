
# CardView.Filter 属性 (Outlook)

返回或设置一个 **String** 值，该值代表视图的筛选器。读/写。


## 语法

 _表达式_. **Filter**

 _表达式_ 一个代表 **CardView** 对象的变量。


## 说明

此属性的值是字符串，在 DAV 搜索和定位 (DASL) 语法中，该字符串代表当前的视图筛选器。有关使用 DASL 语法筛选视图中的项目的详细信息，请参阅[筛选项目](http://msdn.microsoft.com/library/4038e042-1b07-5d18-18b0-c2b58c9c42da%28Office.15%29.aspx)。


## 示例

(VBA) 示例下面的 Visual Basic for Applications 通过使用 **[资源管理器中](026591e5-049f-503a-4166-34e6dbc225fb.md)** 的对象，然后设置 **视图** 的 **[筛选器](9a4b4b27-d543-df82-3058-e0a6ad2f51a1.md)** 属性对象以显示收到了上周的 Outlook 项目的 **[CurrentView](177e6387-9ccb-cb71-bbe5-332c25485848.md)** 属性获取 **[视图](41c8d149-9912-1685-4c8b-3c849cc6f1ed.md)** 对象。


```
Private Sub FilterViewToLastWeek() 
 
 Dim objView As View 
 
 
 
 ' Obtain a View object reference to the current view. 
 
 Set objView = Application.ActiveExplorer.CurrentView 
 
 
 
 ' Set a DASL filter string, using a DASL macro, to show 
 
 ' only those items that were received last week. 
 
 objView.Filter = "%lastweek(""urn:schemas:httpmail:datereceived"")%" 
 
 
 
 ' Save and apply the view. 
 
 objView.Save 
 
 objView.Apply 
 
End Sub 
 

```


## 另请参阅


#### 概念


[卡片视图-对象](cdac229b-f2b6-9ecb-e1a7-b53509426570.md)
#### 其他资源


[卡片视图-对象成员](8b9eda10-1ece-c961-e432-3fca6dfb4f07.md)