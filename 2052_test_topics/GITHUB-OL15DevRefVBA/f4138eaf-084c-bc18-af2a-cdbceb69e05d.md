
# FromRssFeedRuleCondition.FromRssFeed 属性 (Outlook)

返回或设置一个 **String** 元素代表按规则条件计算的 RSS 订阅的数组。读/写。


## 语法

 _表达式_. **FromRssFeed**

 _表达式_ 一个代表 **FromRssFeedRuleCondition** 对象的变量。


## 说明

该数组的每个元素都是一个 RSS 订阅。多个 RSS 订阅按逻辑 OR 条件进行计算。

无法获得有效通过 Outlook 对象模型的 RSS 订阅名称的列表。您可以从 XML 文件 Outlook.Sharing.xml.obi，位于 \Documents 文件夹 [驱动器] 和 [用户名] Settings\ \Local Settings\Application Data\Microsoft\Outlook\ 获得有效 RSS 订阅名称的列表。< 本地 > 标记的 `name`属性包含的 RSS 订阅，必须为 **FromRssFeed** 提供的字符串数组中的名称。若要枚举所有的 RSS 订阅，检查 < 绑定 > 标记， `<binding prov="{0006F0AF-0000-0000-C000-000000000046}">`。

如果数组中有一个或多个元素包含空字符串或无效的 RSS 订阅，则返回错误。


## 另请参阅


#### 概念


[FromRssFeedRuleCondition 对象](8de6e629-7e3d-b4df-d758-a5bff3abd6a1.md)
#### 其他资源


[FromRssFeedRuleCondition 对象成员](0c0a949a-d654-6701-f70d-9a5bb908fed8.md)