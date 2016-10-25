
# FormRegionStartup.GetFormRegionIcon 方法 （Outlook）

获取一个将为窗体区域的特定类型图标显示的图标图像。


## 语法

 _表达式_. **GetFormRegionIcon**( ** _FormRegionName_**, ** _LCID_**, ** _Icon_** )

 _表达式_ 一个代表 **FormRegionStartup** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _FormRegionName_|必需|**String**|在 Windows 注册表中注册窗体区域时使用的窗体区域的名称。|
| _LCID_|必需|**长整型**|标识 Outlook 当前使用的语言的区域设置 ID。此值用于获取与窗体区域的此语言相对应的本地化字符串。|
| _Icon_|必需|**[OlFormRegionIcon](22a9e2aa-e264-8392-b1ad-a2ab995b6440.md)**|一个标识图标类型的常量。|

### 返回值

一个表示图像文件的原始字节的字节数组，或是一个 **IPictureDisp** 对象的变量。


## 说明

此方法通过加载项实现，并由 Outlook 调用。作为  **[FormRegionStartup](948ea6b7-2962-57e7-618d-fa0977b65651.md)** 接口的一部分，此方法和 **[GetFormRegionManifest](de752c6f-423a-ee2f-aa7e-d1107cf406a2.md)** 方法提供了一种机制，加载项可以通过这种机制注册窗体区域，并为 Outlook 提供 XML 清单和窗体区域图标。

如果您将外接程序来为窗体区域提供的图标，请指定的外接程序的 ProgID，当您在 Windows 注册表中注册窗体区域。注册窗体区域的详细信息，请参见[在 Windows 注册表中指定窗体区域](http://msdn.microsoft.com/library/0de3fcb1-b357-8300-c943-9a5a788d4976%28Office.15%29.aspx)。外接程序必须实现 **GetFormRegionManifest** 和 **FormRegionStartup** 接口的 **GetFormRegionIcon** 方法。

窗体区域下 **图标** 元素, 的 XML 清单中指定为每个您想要使用的自定义图标的子元素的值 `addin` 。 实现 **GetFormRegionIcon** ，以便当 Outlook 将作为 _图标_ 的参数传递该类型的图标， **GetFormRegionIcon** 会返回自定义图标的图像。如果您希望 Outlook 显示的默认图标，实现 **GetFormRegionIcon** ，以便它将返回 **null** ( **不** 在 Visual Basic 中) 为该类型的图标。 **GetFormRegionIcon** 还应返回 **null** ( **不** 在 Visual Basic 中) 当 _图标_ 是 **olFormRegionIconDefault** 。

当 Outlook 启动时，它会从 Windows 注册表中读取列表中的窗体区域并缓存的窗体区域相关联的数据。如果窗体区域中注册一个 ProgID，Outlook 将借助相应外接程序通过调用 **GetFormRegionIcon** 其实现了 `addin`作为 **图标** 元素的子元素的值的 XML 清单中的任何图标。请注意，是否您在 Windows 注册表中未指定任何 ProgID，Outlook 将不会调用的 **GetFormRegionManifest** 和 **GetFormRegionIcon** 方法。


## 另请参阅


#### 概念


[FormRegionStartup 接口](948ea6b7-2962-57e7-618d-fa0977b65651.md)
#### 其他资源


[FormRegionStartup 对象成员](c45b60b8-5d7e-d84b-a60e-ffcb54c25569.md)