
# SharingItem.Permission 属性 (Outlook)

设置或返回一个  **[OlPermission](11126d37-33da-53f7-f5b6-ea8603998651.md)** 常量，该常量决定授予收件人对 **[SharingItem](63dd3451-44f3-7cc4-c6e2-7dad5835a7d2.md)** 的哪些权限。可读/写。


## 语法

 _表达式_. **Permission**

 _表达式_ 一个表示 **SharingItem** 对象的变量。


## 说明

 **权限** 属性应该与 **[PermissionTemplateGuid](166c2975-b6be-d1ca-4aa8-ad7deb42c68d.md)** 属性，以准确地反映 **SharingItem** 权限状态同步。将 **PermissionTemplateGuid** 属性设置为一个有效的 GUID 还应该招将 **权限** 属性设置为 **OlPermission.olPermissionTemplate** 。

当没有信息权限管理 (IRM) 已经被设置 (这种情况下， **权限** 属性为 **OlPermission.olUnrestricted** )，或限制是不转发 (在这种情况下的 **权限** 属性是 **OlPermission.olDoNotForward** ) **SharingItem** 时， **PermissionTemplateGuid** 属性的值应为一个空字符串。

尽管您可以查看任何运行 2007 Microsoft Office system 或更高版本的计算机上受 IRM 保护的内容，但是只有在安装了 Microsoft Office Professional Edition 2003、Microsoft Office Outlook 2007 或更高版本的 Outlook 的情况下才能创建或发送受 IRM 保护的电子邮件。


## 另请参阅


#### 概念


[SharingItem 对象](63dd3451-44f3-7cc4-c6e2-7dad5835a7d2.md)
#### 其他资源


[SharingItem 对象成员](719ad60e-2242-2c54-778f-006b61690389.md)