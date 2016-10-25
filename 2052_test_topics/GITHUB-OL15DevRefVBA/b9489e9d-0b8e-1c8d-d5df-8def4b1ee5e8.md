
# ExchangeUser 成员 (Outlook)


提供有关代表 Microsoft Exchange 邮箱用户的  **[AddressEntry](d4a0a85e-8bab-bc56-57bc-d70c3c570c8e.md)** 的详细信息。


## 方法



|**名称**|**说明**|
|:-----|:-----|
|[删除](d11a82c4-28de-efef-5170-20f999f2bf08.md)|将  **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象从其所属的 **[AddressEntries](db91b717-07c6-d1f2-c545-b766ee1f0c6b.md)** 集合对象中删除。|
|[详细信息](6c93a583-cc61-e527-7832-88dba525854a.md)|显示一个模式对话框，该对话框提供有关  **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象的详细信息。|
|[GetContact](443fb23a-cd26-e385-bd9d-e978aec56458.md)|因为与联系人通讯簿中的联系人不对应的 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象，则返回 **Null** ( **不** 在 Visual Basic 中)。|
|[GetDirectReports](753201ad-8001-3185-7d68-fda15907099d.md)|获取一个  **[AddressEntries](db91b717-07c6-d1f2-c545-b766ee1f0c6b.md)** 集合对象，其中包含直接向 Exchange 用户报告的所有用户。|
|[GetExchangeDistributionList](4ebc0448-97a9-ca5c-35f0-ef852de27324.md)|因为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象不对应的 **[ExchangeDistributionList](2830dfba-6c0a-a81f-6b98-92ac2aafb59d.md)** 对象，则返回 **Null** ( **不** 在 Visual Basic 中)。|
|[GetExchangeUser](ff0babba-895f-8205-fefb-c587ee53ea91.md)|返回  **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象。|
|[GetExchangeUserManager](ead5e950-7f7a-b213-0daf-c2bff890a30c.md)|返回一个  **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象，该对象代表 Exchange 用户的管理器。|
|[GetFreeBusy](0dcd36af-e9d7-ca1e-334f-c540c46254f7.md)|获取一个 **字符串** 表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的可用性 30 天内从开始日期开始，在指定日期的午夜开始。|
|[GetMemberOfList](1f4e8910-8998-85ab-05dc-d06f6fd323c3.md)|返回一个  **[AddressEntries](db91b717-07c6-d1f2-c545-b766ee1f0c6b.md)** 集合对象，它包含代表用户所属于的所有 Exchange 通讯组列表的 **[AddressEntry](d4a0a85e-8bab-bc56-57bc-d70c3c570c8e.md)** 对象。|
|[GetPicture](4298db85-0576-4982-9592-6eae666d966a.md)|获得 **[IPictureDisp](http://msdn.microsoft.com/en-us/library/ms680762%28VS.85%29.aspx)** object that represents the picture of the Microsoft Exchange user that is displayed inMicrosoft Outlook.|
|[更新](a2672fbf-f32a-f120-227c-24ee5c361f35.md)|在邮件系统中投递对  **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象的更改。|
|[GetUnifiedGroup](ec0f58fa-969d-ed38-705b-2c99ccbf3c86.md)||
|[GetUnifiedGroupFromStore](38a901d3-670f-afd2-a385-3b2bb859cb81.md)||
|[IsUnifiedGroup](46f9564a-1c0a-fe6c-3f06-989fb5f36adf.md)||

## 属性



|**名称**|**说明**|
|:-----|:-----|
|[地址](b3a36b16-e652-9e3f-86fd-7cea0c72d78c.md)|返回或设置一个 **字符串** ，表示 x400 电子邮件地址 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 。读/写。|
|[AddressEntryUserType](fb5b16be-8846-7c9f-22bf-847d2cfc0a54.md)|返回表示的 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 用户类型为 **[OlAddressEntryUserType](9f128fe4-9981-e06a-d69c-ca7cf9107fe9.md)** 枚举常量的 **olExchangeUserAddressEntry** 。只读的。|
|[别名](ea87a061-4f09-e0ed-fd3d-bfb44cccaf15.md)|返回一个 **字符串** 表示为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的别名。只读的。|
|[应用程序](17331aa1-d926-ada9-a782-02291cd6f720.md)|返回一个  **[Application](797003e7-ecd1-eccb-eaaf-32d6ddde8348.md)** 对象，它代表 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象的父应用程序 (Outlook)。只读。|
|[AssistantName](cca35d99-7031-ba46-4171-8c89b9ea2e2b.md)|返回一个 **字符串** ，表示助手的名称为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 。读/写。|
|[BusinessTelephoneNumber](c01f85bb-24a2-c08f-df4c-9e6506ca2077.md)|返回一个 **字符串** 表示的商务电话号码为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 。读/写。|
|[市/县](fcec3330-39fb-61e9-e447-4adca0146171.md)|返回一个 **字符串** 表示为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的城市。读/写。|
|[类](eea4ce34-a957-3771-ae7b-d8fdd959a37d.md)|返回  **[OlObjectClass](33d724b3-df3c-2a7f-a80f-93b66d96f588.md)** 枚举中的一个常量，该常量指示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象的类。只读。|
|[批注](b55f865c-c564-f209-7648-9977512dd5a5.md)|返回一个 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的表示注释的 **字符串** 。读/写。|
|[CompanyName](d7a630ec-0fbf-78ea-5f2a-51be6d001c23.md)|返回一个 **字符串** ，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的公司的名称。读/写。|
|[部门](3b2512ff-d741-53b2-6f1d-a0f74ffbbce1.md)|返回一个 **字符串** ，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的部门。读/写。|
|[DisplayType](3060a00b-9a99-7833-1a8a-5c18123d7d98.md)|返回表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的性质为 **[收件人](356e5f75-8aa2-e28d-64ee-27b78348ba7a.md)** 枚举常量的 **olUser** 。只读的。|
|[FirstName](6a72812a-31fd-aa6a-be08-f765018208ab.md)|返回一个 **字符串** ，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的名字。读/写。|
|[ID](b26ae0d3-ba96-f3ad-cd74-92ce5305e702.md)|返回一个 **字符串** 表示为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的唯一标识符。只读的。|
|[JobTitle](2cfa5301-3164-c472-3f8e-831c1eebc810.md)|返回一个 **字符串** ，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的职务。读/写。|
|[LastName](1f9f9675-3e72-da56-d654-a1473f4f71a7.md)|返回一个 **字符串** ，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的姓氏。读/写。|
|[MobileTelephoneNumber](9c76ef68-1f85-d072-11d9-015fbbe1658e.md)|返回一个 **字符串** **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 为代表的移动电话号码。读/写。|
|[名称](8b93c5a3-7c6a-4193-7fc3-621e1d0dda18.md)|返回或设置一个 **字符串** 值，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象的显示名称。读/写。|
|[OfficeLocation](b37d5622-27ba-b2c4-cfd3-6aa1e9e9296b.md)|返回一个 **字符串** ，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的办公地点。读/写。|
|[父](18a2505c-14aa-7924-ec59-74c8e85ac92e.md)|返回父 **对象** 的 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象。只读的。|
|[PostalCode](b135d7a6-daa1-4154-d6e7-506c59860a81.md)|返回一个 **字符串** 表示邮政编码 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 。读/写。|
|[PrimarySmtpAddress](2dda21da-44a2-fbfe-babc-58646c76689d.md)|返回一个 **字符串** 表示的主简单邮件传输协议 (SMTP) 地址为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 。只读的。|
|[PropertyAccessor](d1427525-8f6a-04a2-9cfa-b91ee0a89ec2.md)|返回一个  **[PropertyAccessor](2fc91e13-703c-3ec9-9066-ffee7144306c.md)** 对象，该对象支持创建、获取、设置和删除父 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 对象的属性。只读。|
|[会话](7d2d23f0-c441-281a-1784-fe63dfa47b9f.md)|返回当前会话的  **[NameSpace](f0dcaa19-07f5-5d42-a3bf-2e42b7885644.md)** 对象。只读。|
|[StateOrProvince](abac4889-800a-5573-5851-095f5b5176c5.md)|返回一个 **字符串** 表示的州或省的 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 。读/写。|
|[StreetAddress](155399c8-7d99-6537-ba30-84145b26ef21.md)|返回一个 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 为代表的街道地址的 **字符串** 。读/写。|
|[类型](de3652a8-023c-5d2c-9ced-88f768c22a87.md)|返回表示项的类型为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的 **字符串** 。读/写。|
|[YomiCompanyName](481fec99-c2ab-c4c7-8e05-ede9e6846d1e.md)|返回一个 **字符串** ，表示日语拼音的呈现方式 (yomigana) **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的公司名称。读/写。|
|[YomiDepartment](6bc06cf2-7dee-fa50-7380-73df8022ff18.md)|返回一个 **字符串** ，表示日语拼音的呈现方式 (yomigana) **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的部门名称。读/写。|
|[YomiDisplayName](71e97add-9cf1-86c7-3e94-985d2333ebbd.md)|返回一个 **字符串** ，表示 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** Exchange 显示名称的日文拼音呈现 (yomigana)。读/写。|
|[YomiFirstName](b44094df-af5a-21fd-0c09-ada48e51cfd8.md)|返回一个 **字符串** ，表示日语拼音呈现 (yomigana) 的名字为 **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 。读/写。|
|[YomiLastName](079ba8e7-4a3a-2f8c-fa17-0db5ab8f47c2.md)|返回一个 **字符串** ，表示日语拼音的呈现方式 (yomigana) **[ExchangeUser](6ec117d1-7fdb-aa36-b567-1242f8238df0.md)** 的最后一名。读/写。|
