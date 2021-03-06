
# RuleActions 成员 (Outlook)


 **RuleActions** 对象包含一组 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象或派生自 **RuleAction** 的对象，这些对象代表对 **[Rule](ea2ddbcc-fd65-a636-c6da-79950033f385.md)** 对象执行的操作。


## 方法



|**名称**|**说明**|
|:-----|:-----|
|[项目](d37a3f0c-0273-e4c2-21e5-661484244671.md)|获取由指定的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象 _Index_ 这是 **[RuleActions](82ba76cd-86a4-3372-cb51-2df1d58c8b71.md)** 集合的数字索引。|

## 属性



|**名称**|**说明**|
|:-----|:-----|
|[应用程序](001f7719-084b-2b80-6660-097b5a47c433.md)|返回 **[应用程序](797003e7-ecd1-eccb-eaaf-32d6ddde8348.md)** 对象表示父对象的 Outlook 应用程序。只读的。|
|[AssignToCategory](7780487b-3dd4-6143-2250-2109872b6192.md)|使用 **[AssignToCategoryRuleAction.ActionType](bef50a28-967e-7336-ef0b-2e8edb843c0d.md)** 的 **olRuleAssignToCategory** 返回的 **[AssignToCategoryRuleAction](402f4742-72ba-2559-4e4c-e2b8248cd7f6.md)** 对象。只读的。|
|[抄送](edbaaf74-cfd2-304b-61f3-8d12a621239c.md)|与 **[SendRuleAction.ActionType](07b46194-32b4-f04f-d18e-d4b7f3db8f07.md)** 的 **olRuleActionCcMessage** 返回的 **[SendRuleAction](4ea8f519-8bb3-b0bf-9742-8a492e7ffff7.md)** 对象。只读的。|
|[类](99e959aa-7081-aca3-7415-827c6bc3bf64.md)|返回一个 **[OlObjectClass](33d724b3-df3c-2a7f-a80f-93b66d96f588.md)** 常量，该常量指示对象的类。只读的。|
|[ClearCategories](db594b52-1700-67a7-8445-338f7df254e9.md)|返回与 **olRuleActionClearCategories** **[RuleAction.ActionType](5701cd66-2f45-ae24-12b8-fc5e27bf8742.md)** 的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象。只读的。|
|[CopyToFolder](6e5c0ea8-6287-2904-c8d8-b3c6b5f7cb24.md)|使用 **[MoveOrCopyRuleAction.ActionType](204bef7d-a19a-abd1-d494-23c33aa9f145.md)** 的 **olRuleActionCopyToFolder** 返回一个 **[MoveOrCopyRuleAction](db951ad8-0d05-1696-acf4-c1da4fbdee33.md)** 对象。只读的。|
|[计数](91b4425f-0e17-fff1-0d9c-1697b205ff2a.md)|返回 **Long** 类型，表示指定集合中的对象的计数。只读的。|
|[删除](eb219d46-64c2-650c-ad39-e049ef33160f.md)|与 **[RuleAction.ActionType](5701cd66-2f45-ae24-12b8-fc5e27bf8742.md)** 的 **olRuleActionDelete** 返回的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象。只读的。|
|[DeletePermanently](fbd19516-c599-b7e6-cdd3-0c182d32b216.md)|与 **[RuleAction.ActionType](5701cd66-2f45-ae24-12b8-fc5e27bf8742.md)** 的 **olRuleActionDeletePermanently** 返回的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象。只读的。|
|[DesktopAlert](700c3e5a-ebb1-3cfe-e27d-eea305c27143.md)|与 **[RuleAction.ActionType](5701cd66-2f45-ae24-12b8-fc5e27bf8742.md)** 的 **olRuleActionDesktopAlert** 返回的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象。只读的。|
|[转发](48315808-5ef7-3262-a305-5b659986e7a8.md)|与 **[SendRuleAction.ActionType](07b46194-32b4-f04f-d18e-d4b7f3db8f07.md)** 的 **olRuleActionForward** 返回的 **[SendRuleAction](4ea8f519-8bb3-b0bf-9742-8a492e7ffff7.md)** 对象。只读的。|
|[ForwardAsAttachment](9e2eb736-35d9-b17e-8d6d-c5105388f513.md)|与 **[SendRuleAction.ActionType](07b46194-32b4-f04f-d18e-d4b7f3db8f07.md)** 的 **olRuleActionForwardAsAttachment** 返回的 **[SendRuleAction](4ea8f519-8bb3-b0bf-9742-8a492e7ffff7.md)** 对象。只读的。|
|[MarkAsTask](9dd48e1a-d780-0923-11b0-e980c1fe19ab.md)|使用 **[MarkAsTaskRuleAction.ActionType](d05f10cb-5c5d-37e5-1d6e-a5e4147bd1b3.md)** 的 **olRuleActionMarkAsTask** 返回一个 **[MarkAsTaskRuleAction](639d9242-7387-2b25-9d0f-f7a14cf16790.md)** 对象。只读的。|
|[MoveToFolder](6d9c577d-e022-72fc-45f2-bdda7a8761de.md)|使用 **[MoveOrCopyRuleAction.ActionType](204bef7d-a19a-abd1-d494-23c33aa9f145.md)** 的 **olRuleActionMoveToFolder** 返回一个 **[MoveOrCopyRuleAction](db951ad8-0d05-1696-acf4-c1da4fbdee33.md)** 对象。只读的。|
|[NewItemAlert](01de8523-7617-c3df-39c6-395f85eda57f.md)|**[操作请输入](e6cb9c35-48c3-f7fe-cfed-4eb45cb83149.md)** 的 **olRuleActionNewItemAlert** 返回的 **[NewItemAlertRuleAction](01d30816-50aa-ff23-69a0-4aa627b3d7e4.md)** 对象。只读的。|
|[NotifyDelivery](fd5e3831-6181-8452-10e5-17ff48d54ba7.md)|与 **[RuleAction.ActionType](5701cd66-2f45-ae24-12b8-fc5e27bf8742.md)** 的 **olRuleActionNotifyDelivery** 返回的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象。只读的。|
|[NotifyRead](922a1ea7-8992-0387-e4e1-2e74d6a2cf2a.md)|与 **[RuleAction.ActionType](5701cd66-2f45-ae24-12b8-fc5e27bf8742.md)** 的 **olRuleActionNotifyRead** 返回的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象。只读的。|
|[父](697b3625-f184-b6d7-9788-bf74377d636b.md)|返回指定对象的 **对象** 的父级。只读的。|
|[PlaySound](43a79f2d-9e7b-7053-6901-40e815220ac0.md)|使用 **[PlaySoundRuleAction.ActionType](f3b2ec1d-9b8b-64b8-cc02-9d1aec8fd764.md)** 的 **olRuleActionNotifyRead** 返回一个 **[PlaySoundRuleAction](6a7a1f78-640e-8ffc-558c-c26b87638d64.md)** 对象。只读的。|
|[重定向](a8e13e82-43c5-168a-0334-386fd02489f8.md)|与 **[SendRuleAction.ActionType](07b46194-32b4-f04f-d18e-d4b7f3db8f07.md)** 的 **olRuleActionRedirect** 返回的 **[SendRuleAction](4ea8f519-8bb3-b0bf-9742-8a492e7ffff7.md)** 对象。只读的。|
|[会话](10b906a5-421c-e858-f8f1-561818425f0a.md)|返回当前会话的 **[命名空间](f0dcaa19-07f5-5d42-a3bf-2e42b7885644.md)** 的对象。只读的。|
|[停止](62157e66-dc87-b12e-444d-864d34f4211f.md)|与 **[RuleAction.ActionType](5701cd66-2f45-ae24-12b8-fc5e27bf8742.md)** 的 **olRuleActionStop** 返回的 **[RuleAction](6451788f-e5ed-239c-a34d-b564b52d8955.md)** 对象。只读的。|
