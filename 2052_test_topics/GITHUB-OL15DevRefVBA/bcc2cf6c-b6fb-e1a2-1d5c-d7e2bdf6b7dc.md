
# 项目成员 (Outlook)


包含文件夹中的[Outlook 项对象](6ea4babf-facf-4018-ef5a-4a484e55153a.md)的集合。


## 事件



|**名称**|**说明**|
|:-----|:-----|
|[ItemAdd](e46f5958-aff8-3a6b-b3df-5c4352b6c3d9.md)|在将一个或多个项目添加到指定的集合中时发生。该事件在将大量项目同时添加到文件夹时不运行。该事件在 Microsoft Visual Basic Scripting Edition (VBScript) 中不可用。|
|[ItemChange](6478357e-2a5a-300a-24e6-c125f8c81edd.md)|当指定集合中的项目更改时发生。此事件在 Microsoft Visual Basic Scripting Edition (VBScript) 中不可用。|
|[ItemRemove](c1b2d9cd-ab32-2c4a-85fa-9412c190ac4f.md)|当从指定集合中删除项目时发生。|

## 方法



|**名称**|**说明**|
|:-----|:-----|
|[添加](0ee68068-1452-0f29-b85a-88b801ac0448.md)|在文件夹的  **[Items](3a99730b-e62a-5ca6-f6ec-911c95173242.md)** 集合中创建一个新 Outlook 项目。|
|[查找](e7a791d8-b80b-df07-84a3-a85acabfcf80.md)|查找并返回Microsoft Outlook项对象满足给定 _Filter_.|
|[FindNext](2530f640-e024-3567-f539-6bdbf645401d.md)|**[Find](e7a791d8-b80b-df07-84a3-a85acabfcf80.md)** 方法运行后，此方法将查找并返回指定集合中的下一步的 Outlook 项目。|
|[GetFirst](142a6174-118e-6256-0511-8ae9e142e555.md)|返回集合中的第一个对象。|
|[GetLast](d02a20be-19fc-fb6e-feff-b66ca0273beb.md)|返回集合中的最后一个对象。|
|[GetNext](01c49c21-d9f9-37c4-8c64-ff8e2b1f9462.md)|返回集合中的下一个对象。|
|[GetPrevious](5dde47f8-2bd8-fdbe-d6e7-b1381e8a97a6.md)|返回集合中的上一个对象。|
|[项目](89a031e0-c0a3-fc22-f485-189df8db45f4.md)|从集合中返回一个 Outlook 项目。|
|[删除](d2838c82-d0ac-82cc-eed0-c34d55c67d63.md)|从集合中删除对象。|
|[ResetColumns](0543dd17-1e65-5484-ab21-d4791b3b1194.md)|清除使用  **[SetColumns](90206a68-baf8-282c-5793-fee029fed452.md)** 方法缓存的属性。|
|[限制](e3b0cda1-e43d-cc5e-2942-0f54935d9dab.md)|对  **[Items](3a99730b-e62a-5ca6-f6ec-911c95173242.md)** 集合应用筛选条件，返回一个新集合，其中包含原始集合中与筛选器匹配的所有项目。|
|[SetColumns](90206a68-baf8-282c-5793-fee029fed452.md)|缓存某些属性，从而极大地提高  **[Items](3a99730b-e62a-5ca6-f6ec-911c95173242.md)** 集合中每个项目的那些特定属性的访问速度。|
|[排序](7cb248a2-6885-8be5-df7b-fd5683081e01.md)|按指定属性对项目的集合进行排序。在完成该方法后将集合的索引重新设置为 1。|

## 属性



|**名称**|**说明**|
|:-----|:-----|
|[应用程序](b55a6901-fbd4-36a1-47e7-2c1e37e0a31c.md)|返回 **[应用程序](797003e7-ecd1-eccb-eaaf-32d6ddde8348.md)** 对象表示父对象的 Outlook 应用程序。只读的。|
|[类](783ed46a-fd40-c848-b440-8ea3c5d0e6b9.md)|返回一个 **[OlObjectClass](33d724b3-df3c-2a7f-a80f-93b66d96f588.md)** 常量，该常量指示对象的类。只读的。|
|[计数](c18b06be-3a21-3350-6d14-57c822a85d42.md)|返回 **Long** 类型，表示指定集合中的对象的计数。只读的。|
|[IncludeRecurrences](7d192112-889c-56ce-aab2-107d751c80c4.md)|返回 **boolean 类型的值** ，指示如果 **[Items](3a99730b-e62a-5ca6-f6ec-911c95173242.md)** 集合应包括定期模式 **，则返回 True** 。读/写。|
|[父](8e99782a-5654-ae1d-c6d8-9dbfcbcf44ac.md)|返回指定对象的 **对象** 的父级。只读的。|
|[会话](5c385dfc-042e-7649-0f32-5d34e53fca57.md)|返回当前会话的 **[命名空间](f0dcaa19-07f5-5d42-a3bf-2e42b7885644.md)** 的对象。只读的。|
