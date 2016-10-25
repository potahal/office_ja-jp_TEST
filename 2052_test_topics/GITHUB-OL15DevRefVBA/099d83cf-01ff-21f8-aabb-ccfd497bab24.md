
# AddressEntry.Update 方法 （Outlook）

发邮件系统中对 **[AddressEntry](d4a0a85e-8bab-bc56-57bc-d70c3c570c8e.md)** 对象的更改。


## 语法

 _表达式_. **Update**( ** _MakePermanent_**, ** _Refresh_** )

 _表达式_ 返回 **AddressEntry** 对象的表达式。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _MakePermanent_|可选|**Variant**|**True** 值表明刷新属性缓存，并将所有更改都提交到基本通讯簿中。 **False** 值表明刷新属性缓存，但不是提交到永久存储区。默认值为 **True** 。|
| _Refresh_|可选|**Variant**|**True** 值表明从基本通讯簿中的值，重新加载属性缓存。如果值为 **False** 表示不会重新加载属性缓存。默认值为 **False** 。|

## 注解

新条目或对现有条目的更改将不保留集合中之前已使用 _MakePermanent_参数设置为 **True** 时调用 **Update** 方法。

要刷新缓存，然后再重新加载通讯簿中的值，请将 _Refresh_参数设置为 **True** 将 _MakePermanent_参数设置为 **False** 并且调用 **Update** 。


## 另请参阅


#### 概念


[AddressEntry 对象](d4a0a85e-8bab-bc56-57bc-d70c3c570c8e.md)
#### 其他资源


[AddressEntry 对象成员](74c88069-aec4-952b-556f-03873fbb488b.md)