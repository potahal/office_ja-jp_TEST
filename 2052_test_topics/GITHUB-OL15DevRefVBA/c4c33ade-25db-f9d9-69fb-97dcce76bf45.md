
# ContactItem.BeforeAttachmentSave 事件 (Outlook)

在保存附件前发生。


## 语法

 _表达式_. **BeforeAttachmentSave**( ** _Attachment_**, ** _Cancel_** )

 _表达式_ 一个代表 **ContactItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Attachment_|必需|**[Attachment](3e11582b-ac90-0948-bc37-506570bb287b.md)**|保存 **附件** 。|
| _Cancel_|必需|**Boolean**|(不使用在 VBScript 中)。 **假** 的事件发生时。如果事件过程将此参数设置为 **True** ，则保存操作没有完成，附件没有被更改。|

## 注解

此事件对应于将附件保存到邮件存储区时。之前在保存附件保存项时，  **BeforeAttachmentSave** 事件发生。如果用户编辑附件，然后保存这些更改， **BeforeAttachmentSave** 事件将不会发生在这段时间;相反它会发生以后保存项目本身。它也不会触发附件保存在使用 **SaveAsFile** 方法的硬盘上。

在 VBScript 中，如果您将此函数的返回值设置为 **False** ，保存操作将被取消，附件没有被更改。


## 另请参阅


#### 概念


[创建 ContactItem 对象](8e32093c-a678-f1fd-3f35-c2d8994d166f.md)
#### 其他资源


[创建 ContactItem 对象成员](a8b13369-4c87-02aa-e62a-1f3067e559fa.md)