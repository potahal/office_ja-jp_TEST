# 在清单中定义外接程序命令

外接程序命令提供了一种简单方法可以使用执行操作的 UI 元素来自定义默认的 Office UI。例如，你可以在功能区上添加自定义按钮。 若要创建命令，你可以将“**[VersionOverrides](../../../reference/manifest/versionoverrides.md)(#versionoverrides)**”节点添加到现有的任务窗格清单。 

当清单中包含 **VersionOverrides** 元素时，支持外接程序命令的 Word、Excel、Outlook 和 PowerPoint 版本将使用该元素内的信息加载外接程序。 不支持外接程序命令的早期版本的 Office 产品将忽略此元素。

当客户端应用程序识别“**VersionOverrides**”节点时，外接程序名称会出现在功能区中，而不是在任务窗格或读取/撰写窗格中。 外接程序不会显示在这两个位置中。
 

## VersionOverrides 节点

[VersionOverrides](../../../reference/manifest/versionoverrides.md)(#versionoverrides) 元素是包含由外接程序实施的外接程序命令信息的根元素。 其在清单架构 v1.1 和更新版本中受支持，但是在 VersionOverrides v1.0 架构中定义的。 

VersionOverrides 元素包括以下子元素：

- [Description](../../../reference/manifest/description.md)
- [Requirements](../../../reference/manifest/requirements.md)
- [Hosts](../../../reference/manifest/hosts.md)
- [Resources](../../../reference/manifest/resources.md)

下图显示了用于定义外接程序命令的元素的层次结构。 

![清单中的外接程序命令元素层次结构](../../../images/080da303-51c4-4882-b74a-7ba11517c0ad.png)

## Outlook 外接程序命令的规则更改

以下更改会影响清单中的规则：

- 激活规则现在位于每个入口点内部。
    
- [Rule](../../../reference/manifest/rule.md)(#rule) 的 **ItemIs** 属性已经修改。 **ItemType** 可以是 Message 或 AppointmentAttendee。 **FormType** 属性已经删除。
    
- [Rule](../../../reference/manifest/rule.md)(#rule) 元素的 **ItemHasKnownEntity** 属性已更新以接受 EntityType 的字符串。
    

## 示例清单

有关实施 Word、Excel 和 PowerPoint 的外接程序命令的示例清单，请参阅 [简单的外接程序命令示例](https://github.com/OfficeDev/Office-Add-in-Commands-Samples/tree/master/Simple)(#简单的外接程序命令示例)。

有关实施 Outlook 的外接程序命令的示例清单，请参阅 [Outlook 外接程序的示例清单文件](https://gist.github.com/mlafleur/95b7ac030bb7a7ae742527e85a36b095)(#outlook-外接程序的示例清单文件)。


## 其他资源


- [适用于 Outlook 的外接程序命令](../../outlook/add-in-commands-for-outlook.md)
    
- [Outlook 外接程序清单](../../outlook/manifests/manifests.md)
    
- [Outlook 外接程序命令演示示例](https://github.com/jasonjoh/command-demo)
