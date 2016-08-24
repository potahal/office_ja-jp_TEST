---
ms.Toctitle: Office 365 Data Extensions REST API reference
title: "Office 365 データ拡張機能 REST API リファレンス"
description: "Office 365 のデータ拡張機能 REST API と Outlook.com を使用して、oData オープン型の拡張情報の一部としてプロパティを動的に作成し、カスタム データの取得、設定および削除を行う方法のリファレンスです。"
ms.ContentId: 6034eaed-831f-4a6e-888a-0c670476023f
ms.date: April 20, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Office 365 データ拡張機能 REST API リファレンス

[!INCLUDE [Add the Outlook REST API filters--v2 default v1 disabled](../includes/controls/addOutlookversion_v2default_v1disabled.xml)]

 _**適用対象:**Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<p class="previewnote">このドキュメントでは、プレビュー段階の、ベータ版の Office 365 データ拡張機能 API について説明します プレビュー機能は、最終版までに変更される場合があり、それらの機能を使用するコードが動作しなくなる場合もあります。 このため、一般に、運用コードでは運用バージョンの API のみを使用してください。 入手可能な場合、現時点ではバージョン 2.0 が優先バージョンです。</p>

Office 365 データ拡張機能 REST API を使用すると、アプリによりユーザー アカウントのメッセージ、イベント、または連絡先でカスタム データを動的に保存できるようになります。 Office 365 アカウントまたは Microsoft アカウント (Hotmail.com、Live.com、MSN.com、Outlook.com、Passport.com) をこのアカウントの対象にすることができます。

**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

**ベータ版の API が不要な場合** 右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Office 365 データ拡張機能 REST API を使用すると、アプリによりユーザー アカウントのメッセージ、イベント、または連絡先でカスタム データを動的に保存できるようになります。 Office 365 アカウントまたは Microsoft アカウント (Hotmail.com、Live.com、MSN.com、Outlook.com、Passport.com) をこのアカウントの対象にすることができます。

**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

**API v2.0 が不要な場合** 右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


## 概要

Outlook REST API のデータ拡張情報は、OData v4 オープン型で、実行時に指定できるプロパティが含まれています。 データ拡張機能 API を使用すると、JSON ペイロード内でカスタムのプロパティや値を動的に指定することにより、Entity Data Model (EDM) で既に定義されているエンティティ型 (メッセージ、イベント、連絡先) のインスタンスを_拡張_できます。 したがって、このようなエンティティ型の定義の柔軟性が増し、この目的だけのために新しいエンティティ型を定義する時間を節約できます。

すべての拡張情報に対して定義されるプロパティは、**ExtensionName** プロパティだけです。 拡張情報名を一意にするのに役立つ方法の 1 つとして、_独自のドメイン_ (`Com.Contoso.Contact`) に依存しているドメイン ネーム システム (DNS) 逆引きの方法を使用できます。 拡張情報名に Microsoft ドメインを含めることはできません。

拡張情報はオープン型なので、エンティティのインスタンスに固有の追加データを指定できます。 たとえば、次に示すように、個々の取引先担当者に関する拡張情報を作成することにより会社名や初期参照元などのカスタム データを追跡できるようにして、JSON ペイロード内でデータを指定できます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```
POST https://outlook.office.com/api/beta/me/contacts('{contact_id}')/extensions
{
   @odata.type: "Microsoft.OutlookServices.OpenTypeExtension",  
   "ExtensionName": "Com.Contoso.Customer",
   "CompanyName": "Alpine Skis",
   "InitialReferrer":  "Robin McCall"
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```
POST https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/extensions
{
   @odata.type: "Microsoft.OutlookServices.OpenTypeExtension",  
   "ExtensionName": "Com.Contoso.Customer",
   "CompanyName": "Alpine Skis",
   "InitialReferrer":  "Robin McCall"
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


データ拡張機能 API を使用すると、新しいリソースや既存のリソースに対して CRUD 操作を実行できます。 詳細については、[サポートされている操作](#ExtensionOperations)を参照してください。 

OData オープン型の詳細については、[OData.org](http://www.odata.org/documentation/) 上の OData v4 のドキュメントを参照してください。

## データ拡張機能 REST API の使用法

###データ拡張情報と拡張プロパティのどちらを使用するか

データ拡張機能は、カスタム データの格納およびカスタムデータへのアクセスを必要とするほとんどのシナリオに対して推奨されるソリューションです。 ただし、[拡張プロパティとこの REST API](..\api\extended-properties-rest-operations.md) は、Outlook REST API のメタデータを通じてまだ公開されていない Outlook MAPI プロパティのカスタム データにアクセスする必要がある場合にのみ使用します。 {version} を v2.0、beta など、選択したバージョンに置き換えて、メタデータが https://outlook.office.com/api/{version}/$metadata でどのプロパティを公開するかを確認できます。


###認証
他の [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI) と同様に、データ拡張機能 API へのすべての要求に対して、有効なアクセス トークンを含める必要があります。 アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。 効率化された登録と承認のオプションに関する[詳細情報](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow)を参照してください。
データ拡張機能 API で特定の操作を続行する際には、この点に留意してください。

###サポートされている REST リソース 

Outlook REST エンドポイントで、次のリソースのインスタンスに対する拡張情報を作成できます。
- [メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)
- [イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)
- [連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)


###API のバージョン

この API は、プレビューから一般提供 (GA) の状態にレベル上げされています。 v2.0 とベータのバージョンでサポートされます。

`https://outlook.office.com/api/v2.0/`

`https://outlook.office.com/api/beta/`

### URL パラメーター

この記事の例では、REST 要求 URL のパラメーターに次の ID のプレース ホルダーを使用します。拡張機能を作成する対象のリソースのインスタンスの ID を指定する必要があります。

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先の ID。 |
|event_id|string|イベント ID。 |
|message_id|string|メッセージ ID。 |


Outlook REST API と Office 365 データ拡張機能 REST API のすべてのサブセットに共通の情報の詳細については、「[Outlook REST API の使用法](..\api\use-outlook-rest-api.md)」を参照してください。


****

<a name="ExtensionOperations"> </a>
## 拡張情報の操作

[既存のアイテムの拡張情報を作成する](#CreateExtensionInExistingItem) | [新しいアイテムの拡張情報を作成する](#CreateExtensionInNewItem) | 
[拡張情報を取得する](#GetExtension) | [拡張情報を使用して展開されたアイテムを取得する](#GetExpandedExtension) |  
[拡張情報を使用してアイテムを検索したり展開したりする](#FindAndExpandItemsWithExtension) | 
[拡張情報のデータを追加したり変更したりする](#UpdateExtension) | [拡張機能を削除する](#DeleteExtension)


<a name="CreateExtensionInExistingItem"> </a>

### 既存のアイテムの拡張情報を作成する

指定したリソースのインスタンスに関する拡張情報を作成してカスタム プロパティを追加します。Office 365 または Outlook.com のメッセージ、予定表イベント、または連絡先をリソースにすることができます。JSON ペイロード内のデータは、プリミティブ型か、プリミティブ型の配列にすることができます。

サポートされているリソースごとに拡張情報を作成するには、次のようにします。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/messages('{message_id}')/extensions
POST https://outlook.office.com/api/beta/me/events('{event_id}')/extensions
POST https://outlook.office.com/api/beta/me/contacts('{contact_id}')/extensions
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages('{message_id}')/extensions
POST https://outlook.office.com/api/v2.0/me/events('{event_id}')/extensions
POST https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/extensions
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


_**最小限必要なスコープ**: 次の対象リソースに対応する読み取り/書き込みスコープのうちのいずれかです。_
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_本文パラメーター_|
|ExtensionName|string|拡張情報の一意のテキスト識別子。 必須。|

 
**要求のサンプル**

この例では、指定されたメッセージに関する拡張情報を作成します。 要求本文には、拡張情報に関する次のものが含まれます。
- 型 `Microsoft.OutlookServices.OpenTypeExtension`。Outlook REST API メタデータ内で OData オープン型として定義されています。 
- 拡張情報名 "Com.Contoso.Referral"。
- JSON ペイロード内にカスタム プロパティとして保存される追加のデータ: プリミティブ型が含まれる `CompanyName`、`DealValue`、`ExpirationDate` とプリミティブ型の配列が含まれる `TopModels`、`TopSalespersons`です。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```
POST https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions

Content-Type: application/json

{ 
  "@odata.type" : "Microsoft.OutlookServices.OpenTypeExtension", 
  "ExtensionName" : "Com.Contoso.Referral", 
  "CompanyName" : "Wingtip Toys",
  "DealValue@odata.type": "#Int64", 
  "DealValue" : 500050, 
  "ExpirationDate" : "2015-12-03T10:00:00.000Z",
  "TopModels": [
     3001,
     4002,
     5003
  ],
  "TopSalespersons": [
     "Dana Swope",
     "Fanny Downs",
     "Randi Welch"
  ]
} 
```

**応答のサンプル**

成功した応答は、`HTTP 201 Created` 応答コードで示されます。

応答本文には、新しい拡張情報に関する次のものが含まれています。
- 既定のプロパティ **ExtensionName**。
- **Id** プロパティと `Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral` の完全修飾名。
- 保存されるカスタム データ。  

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions
('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
    "ExtensionName": "Com.Contoso.Referral",
    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys",
    "DealValue@odata.type": "#Int64",
    "DealValue": 500050,
    "ExpirationDate": "2015-12-03T10:00:00.000Z",
    "TopModels@odata.type": "#Collection(Int32)",
    "TopModels": [
      3001,
      4002,
      5003
    ],
    "TopSalespersons@odata.type": "#Collection(String)",
    "TopSalespersons": [
      "Dana Swope",
      "Fanny Downs",
      "Randi Welch"
    ]
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```
POST https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions

Content-Type: application/json

{ 
  "@odata.type" : "Microsoft.OutlookServices.OpenTypeExtension", 
  "ExtensionName" : "Com.Contoso.Referral", 
  "CompanyName" : "Wingtip Toys",
  "DealValue@odata.type": "#Int64", 
  "DealValue" : 500050, 
  "ExpirationDate" : "2015-12-03T10:00:00.000Z",
  "TopModels": [
     3001,
     4002,
     5003
  ],
  "TopSalespersons": [
     "Dana Swope",
     "Fanny Downs",
     "Randi Welch"
  ]
} 
```

**応答のサンプル**

成功した応答は、`HTTP 201 Created` 応答コードで示されます。

応答本文には、新しい拡張情報に関する次のものが含まれています。
- 既定のプロパティ **ExtensionName**。
- **Id** プロパティと `Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral` の完全修飾名。
- 保存されるカスタム データ。  

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions
('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
    "ExtensionName": "Com.Contoso.Referral",
    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys",
    "DealValue@odata.type": "#Int64",
    "DealValue": 500050,
    "ExpirationDate": "2015-12-03T10:00:00.000Z",
    "TopModels@odata.type": "#Collection(Int32)",
    "TopModels": [
      3001,
      4002,
      5003
    ],
    "TopSalespersons@odata.type": "#Collection(String)",
    "TopSalespersons": [
      "Dana Swope",
      "Fanny Downs",
      "Randi Welch"
    ]
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


****

<a name="#CreateExtensionInNewItem)"></a>

### 新しいアイテムの拡張情報を作成する

同一の POST 呼び出しで新しいリソースのインスタンスを作成しながら 1 つ以上の拡張情報を作成し、その拡張情報にカスタム プロパティを追加します。 Office 365 または Outlook.com のメッセージ、予定表イベント、または連絡先をリソースにすることができます。 JSON ペイロード内のデータは、プリミティブ型か、プリミティブ型の配列にすることができます。

サポートされているリソースごとに拡張情報を作成するには、そのリソースを作成する場合と同様に POST 呼び出しを行い、POST 要求の本文に拡張情報を含めます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/messages
POST https://outlook.office.com/api/beta/me/events
POST https://outlook.office.com/api/beta/me/contacts
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages
POST https://outlook.office.com/api/v2.0/me/events
POST https://outlook.office.com/api/v2.0/me/contacts
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


_**最小限必要なスコープ**: 次の対象リソースに対応する読み取り/書き込みスコープのうちのいずれかです。_
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_本文パラメーター_|
|ExtensionName|string|拡張情報の一意のテキスト識別子。 必須。|

 
**要求のサンプル**

この例では、同一の呼び出しでメッセージと拡張情報を作成します。 要求本文には、次のものが含まれます。
- 新しいメッセージ固有の **Subject**、**Body**、**ToRecipients** プロパティ。 
- 拡張情報に関する次のもの。
  - 型 `Microsoft.OutlookServices.OpenTypeExtension`。Outlook REST API メタデータ内で OData オープン型として定義されています。 
  - 拡張情報名 "Com.Contoso.Referral"。
  - JSON ペイロード内にカスタム プロパティとして保存される追加のデータ: プリミティブ型が含まれる `CompanyName`、`ExpirationDate`、`DealValue` とプリミティブ型の配列が含まれる `TopModels`、`TopSalespersons`です。  

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```
POST https://outlook.office.com/api/beta/me/messages

Content-Type: application/json

{
  "Subject": "Annual review",
  "Body": {
    "ContentType": "HTML",
    "Content": "You should be proud!"
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "rufus@contoso.com"
      }
    }
  ],
  "Extensions": [
    {
      "@odata.type": "Microsoft.OutlookServices.OpenTypeExtension",
      "ExtensionName": "Com.Contoso.Referral",
      "CompanyName": "Wingtip Toys",
      "ExpirationDate": "2015-12-30T11:00:00.000Z",
      "DealValue": 10000,
      "TopModels": [
        3001,
        4002,
        5003
      ],
      "TopSalespersons": [
        "Dana Swope",
        "Fanny Downs",
        "Randi Welch"
      ]
    }
  ]
}
```

**応答のサンプル**

成功した応答は、`HTTP 201 Created` 応答コードで示されます。

応答本文には、新しいメッセージのプロパティと、新しい拡張情報に関する次のものが含まれています。
- **Id** プロパティと `Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral` の完全修飾名。 
- 要求で指定されている既定のプロパティ **ExtensionName**。
- 要求で指定されている、カスタム プロパティとして保存されるカスタム データ。 

```
{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df800e5@1717f226-49d1-4d0c-9d74-709fad664b77')/Messages
('AAMkAGEbs88AAB84uLuAAA=')",
  "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AAB88LOj\"",
  "Id": "AAMkAGEbs88AAB84uLuAAA=",
  "CreatedDateTime": "2015-10-30T03:03:43Z",
  "LastModifiedDateTime": "2015-10-30T03:03:43Z",
  "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AAB88LOj",
  "Categories": [ ],
  "ReceivedDateTime": "2015-10-30T03:03:43Z",
  "SentDateTime": "2015-10-30T03:03:43Z",
  "HasAttachments": false,
  "Subject": "Annual review",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r
\n<meta content=\"text/html; charset=us-ascii\">\r\n</head>\r\n<body>\r\nYou should be proud!\r\n</body>\r
\n</html>\r\n"
  },
  "BodyPreview": "You should be proud!",
  "Importance": "Normal",
  "ParentFolderId": "AQMkAGEwAAAIBDwAAAA==",
  "Sender": null,
  "From": null,
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "rufus@contoso.com",
        "Name": "John Doe"
      }
    }
  ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkAGEFGugh3SVdMzzc=",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?
ItemID=AAMkAGEbs88AAB84uLuAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "MentionedMe": null,
  "Mentioned": [ ],
  "InferenceClassification": "Focused",
  "Extensions@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages
('AAMkAGEbs88AAB84uLuAAA%3D')/Extensions",
  "Extensions": [
    {
      "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
      "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df800e5@1717f226-49d1-4d0c-9d74-709fad664b77')/Messages
('AAMkAGEbs88AAB84uLuAAA=')/extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
      "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
      "ExtensionName": "Com.Contoso.Referral",
      "CompanyName": "Wingtip Toys",
      "ExpirationDate": "2015-12-30T11:00:00.000Z",
      "DealValue": 10000,
      "TopModels@odata.type": "#Collection(Int32)",
      "TopModels": [
        3001,
        4002,
        5003
      ],
      "TopSalespersons@odata.type": "#Collection(String)",
      "TopSalespersons": [
        "Dana Swope",
        "Fanny Downs",
        "Randi Welch"
      ]
    }
  ]
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```
POST https://outlook.office.com/api/v2.0/me/messages

Content-Type: application/json

{
  "Subject": "Annual review",
  "Body": {
    "ContentType": "HTML",
    "Content": "You should be proud!"
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "rufus@contoso.com"
      }
    }
  ],
  "Extensions": [
    {
      "@odata.type": "Microsoft.OutlookServices.OpenTypeExtension",
      "ExtensionName": "Com.Contoso.Referral",
      "CompanyName": "Wingtip Toys",
      "ExpirationDate": "2015-12-30T11:00:00.000Z",
      "DealValue": 10000,
      "TopModels": [
        3001,
        4002,
        5003
      ],
      "TopSalespersons": [
        "Dana Swope",
        "Fanny Downs",
        "Randi Welch"
      ]
    }
  ]
}
```

**応答のサンプル**

成功した応答は、`HTTP 201 Created` 応答コードで示されます。

応答本文には、新しいメッセージのプロパティと、新しい拡張情報に関する次のものが含まれています。
- **Id** プロパティと `Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral` の完全修飾名。 
- 要求で指定されている既定のプロパティ **ExtensionName**。
- 要求で指定されている、カスタム プロパティとして保存されるカスタム データ。 

```
{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfc984d-b826-40d7-b48b-57002df800e5@1717f226-49d1-4d0c-9d74-709fad664b77')/Messages
('AAMkAGEbs88AAB84uLuAAA=')",
  "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AAB88LOj\"",
  "Id": "AAMkAGEbs88AAB84uLuAAA=",
  "CreatedDateTime": "2015-10-30T03:03:43Z",
  "LastModifiedDateTime": "2015-10-30T03:03:43Z",
  "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AAB88LOj",
  "Categories": [ ],
  "ReceivedDateTime": "2015-10-30T03:03:43Z",
  "SentDateTime": "2015-10-30T03:03:43Z",
  "HasAttachments": false,
  "Subject": "Annual review",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r
\n<meta content=\"text/html; charset=us-ascii\">\r\n</head>\r\n<body>\r\nYou should be proud!\r\n</body>\r
\n</html>\r\n"
  },
  "BodyPreview": "You should be proud!",
  "Importance": "Normal",
  "ParentFolderId": "AQMkAGEwAAAIBDwAAAA==",
  "Sender": null,
  "From": null,
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "rufus@contoso.com",
        "Name": "John Doe"
      }
    }
  ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkAGEFGugh3SVdMzzc=",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?
ItemID=AAMkAGEbs88AAB84uLuAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "MentionedMe": null,
  "Mentioned": [ ],
  "InferenceClassification": "Focused",
  "Extensions@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages
('AAMkAGEbs88AAB84uLuAAA%3D')/Extensions",
  "Extensions": [
    {
      "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
      "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfc984d-b826-40d7-b48b-57002df800e5@1717f226-49d1-4d0c-9d74-709fad664b77')/Messages
('AAMkAGEbs88AAB84uLuAAA=')/extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
      "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
      "ExtensionName": "Com.Contoso.Referral",
      "CompanyName": "Wingtip Toys",
      "ExpirationDate": "2015-12-30T11:00:00.000Z",
      "DealValue": 10000,
      "TopModels@odata.type": "#Collection(Int32)",
      "TopModels": [
        3001,
        4002,
        5003
      ],
      "TopSalespersons@odata.type": "#Collection(String)",
      "TopSalespersons": [
        "Dana Swope",
        "Fanny Downs",
        "Randi Welch"
      ]
    }
  ]
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


****

<a name="GetExtension"> </a>

### 拡張情報を取得する

指定されたリソースのインスタンス内で、名前か ID で拡張情報を取得します。Office 365 または Outlook.com のメッセージ、予定表イベント、または連絡先をリソースにすることができます。 

名前と ID のどちらで拡張情報を取得しても、同じ応答本体が返されます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/messages('{message_id}')/extensions('{extensionId}')
GET https://outlook.office.com/api/beta/me/events('{event_id}')/extensions('{extensionId}')
GET https://outlook.office.com/api/beta/me/contacts('{contact_id}')/extensions('{extensionId}')
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages('{message_id}')/extensions('{extensionId}')
GET https://outlook.office.com/api/v2.0/me/events('{event_id}')/extensions('{extensionId}')
GET https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/extensions('{extensionId}')
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


_**最小限必要なスコープ**: 次の対象リソースに対応する読み取りスコープのうちのいずれかです。_
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|extensionId|string|リソース インスタンスのすべての拡張情報間で一意のテキスト識別子である拡張機能名にするか、拡張情報の種類と一意のテキスト識別子を連結した完全修飾名にすることができます。完全修飾名は、拡張情報の作成時に id プロパティ内に返されます。必須。 |


<a name="GetExtensionExample"></a> 

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**要求のサンプル**

最初の例では、名前で拡張情報を参照し、指定されたメッセージの拡張情報を取得します。

```
GET https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Com.Contoso.Referral')
```

2 番目の例では、ID (完全修飾名) で拡張情報を参照し、指定されたメッセージの拡張情報を取得します。

```
GET https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')
```

**応答のサンプル**

成功した応答は、`HTTP 200 OK` 応答コードで示されます。 

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions
('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
    "ExtensionName": "Com.Contoso.Referral",
    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys",
    "DealValue": 500050,
    "ExpirationDate": "2015-12-03T10:00:00Z"
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**要求のサンプル**

最初の例では、名前で拡張情報を参照し、指定されたメッセージの拡張情報を取得します。

```
GET https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Com.Contoso.Referral')
```

2 番目の例では、ID (完全修飾名) で拡張情報を参照し、指定されたメッセージの拡張情報を取得します。

```
GET https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')
```

**応答のサンプル**

成功した応答は、`HTTP 200 OK` 応答コードで示されます。 

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions
('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
    "ExtensionName": "Com.Contoso.Referral",
    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys",
    "DealValue": 500050,
    "ExpirationDate": "2015-12-03T10:00:00Z"
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


****

<a name="GetExpandedExtension"> </a>

### 拡張情報を使用して展開されたアイテムを取得する

`Id` のフィルターで指定されている拡張情報を使用して展開されたリソースのインスタンスを取得します。 Office 365 または Outlook.com のメッセージ、予定表イベント、または連絡先をリソースにすることができます。

以下のように、`Id` で拡張情報名または完全修飾名に対してフィルター処理してから、拡張情報を使用して展開されたインスタンスを取得できます。 フィルター文字列内のスペース文字に [URL エンコード](http://www.w3schools.com/tags/ref_urlencode.asp)を適用していることを確認してください。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/messages('{message_id}')?$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/beta/me/events('{event_id}')?$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/beta/me/contacts('{contact_id}')?$expand=Extensions($filter=Id eq '{extensionId}')
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages('{message_id}')?$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/v2.0/me/events('{event_id}')?$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')?$expand=Extensions($filter=Id eq '{extensionId}')
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


_**最小限必要なスコープ**: 次の対象リソースに対応する読み取りスコープのうちのいずれかです。_
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|extensionId|string|リソース インスタンスのすべての拡張情報間で一意のテキスト識別子である拡張機能名にするか、拡張情報の種類と一意のテキスト識別子を連結した完全修飾名にすることができます。完全修飾名は、拡張情報の作成時に id プロパティ内に返されます。必須。 |


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**要求のサンプル**

次の例では、フィルターから返される拡張情報を含めることにより、指定されたメッセージを取得して展開します。 このフィルターは、`Id` が完全修飾名と一致する拡張情報を返します。

利便性を考慮して、以下の要求では、予約文字のスペースの URL エンコーディングで示しています。

```
GET https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')?$expand=Extensions($filter=Id%20eq%20'Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')
```


**応答のサンプル**

成功した応答は、`HTTP 200 OK` 応答コードで示されます。

応答本文には、指定されたメッセージのすべてのプロパティと、フィルターから返される拡張情報が含まれます。

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')",
    "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AABNsWMM\"",
    "Id": "AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===",
    "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AABNsWMM",
    "Categories": [
    ],
    "CreateDateTime": "2015-06-19T02:03:31Z",
    "LastModifiedDateTime": "2015-08-13T02:28:00Z",
    "Subject": "Attached is the requested info",
    "BodyPreview": "See the images attached.",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<style type=\"text/css\" style=\"display:none;\"><!-- P {margin-top:0;margin-bottom:0;} --></style>\r\n</head>\r\n<body dir=\"ltr\">\r\n<div id=\"divtagdefaultwrapper\" style=\"font-size:12pt;color:#000000;background-color:#FFFFFF;font-family:Calibri,Arial,Helvetica,sans-serif;\">\r\n<p>See the images attached. <br>\r\n</p>\r\n</div>\r\n</body>\r\n</html>\r\n"
    },
    "Importance": "Normal",
    "HasAttachments": true,
    "ParentFolderId": "AQMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===QAAAA==",
    "From": {
        "EmailAddress": {
            "Address": "desmond@contoso.com",
            "Name": "Desmond Raley"
        }
    },
    "Sender": {
        "EmailAddress": {
            "Address": "desmond@contoso.com",
            "Name": "Desmond Raley"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Address": "wendy@contoso.com",
                "Name": "Wendy Molina"
            }
        }
    ],
    "CcRecipients": [
    ],
    "BccRecipients": [
    ],
    "ReplyTo": [
    ],
    "ConversationId": "AAQkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===mivdTmQ=",
    "ReceivedDateTime": "2015-06-19T02:05:04Z",
    "SentDateTime": "2015-06-19T02:04:59Z",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsDraft": false,
    "IsRead": true,
    "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===%2FNJTqt5NqHlVnKVBwCY4MQpaFz9SbqUDe4%2Bbs88AAAAAAEJAACY4MQpaFz9SbqUDe4%2Bbs88AAApA4JMAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
    "MentionedMe": null,
    "Mentioned": [
    ],
    "InferenceClassification": "Focused",
    "Extensions@odata.context": "https://outlook.office.com/api/beta/$metadata#Users('desmond40contoso.com')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions", 
    "Extensions": [ 
      { 
        "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
        "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
        "ExtensionName": "Com.Contoso.Referral",
        "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
        "CompanyName": "Wingtip Toys",
        "DealValue": 500050,
        "ExpirationDate": "2015-12-03T10:00:00Z"
      }
     ]
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**要求のサンプル**

次の例では、フィルターから返される拡張情報を含めることにより、指定されたメッセージを取得して展開します。 このフィルターは、`Id` が完全修飾名と一致する拡張情報を返します。

利便性を考慮して、以下の要求では、予約文字のスペースの URL エンコーディングで示しています。

```
GET https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')?$expand=Extensions($filter=Id%20eq%20'Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')
```


**応答のサンプル**

成功した応答は、`HTTP 200 OK` 応答コードで示されます。

応答本文には、指定されたメッセージのすべてのプロパティと、フィルターから返される拡張情報が含まれます。

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')",
    "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AABNsWMM\"",
    "Id": "AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===",
    "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AABNsWMM",
    "Categories": [
    ],
    "CreateDateTime": "2015-06-19T02:03:31Z",
    "LastModifiedDateTime": "2015-08-13T02:28:00Z",
    "Subject": "Attached is the requested info",
    "BodyPreview": "See the images attached.",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<style type=\"text/css\" style=\"display:none;\"><!-- P {margin-top:0;margin-bottom:0;} --></style>\r\n</head>\r\n<body dir=\"ltr\">\r\n<div id=\"divtagdefaultwrapper\" style=\"font-size:12pt;color:#000000;background-color:#FFFFFF;font-family:Calibri,Arial,Helvetica,sans-serif;\">\r\n<p>See the images attached. <br>\r\n</p>\r\n</div>\r\n</body>\r\n</html>\r\n"
    },
    "Importance": "Normal",
    "HasAttachments": true,
    "ParentFolderId": "AQMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===QAAAA==",
    "From": {
        "EmailAddress": {
            "Address": "desmond@contoso.com",
            "Name": "Desmond Raley"
        }
    },
    "Sender": {
        "EmailAddress": {
            "Address": "desmond@contoso.com",
            "Name": "Desmond Raley"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Address": "wendy@contoso.com",
                "Name": "Wendy Molina"
            }
        }
    ],
    "CcRecipients": [
    ],
    "BccRecipients": [
    ],
    "ReplyTo": [
    ],
    "ConversationId": "AAQkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===mivdTmQ=",
    "ReceivedDateTime": "2015-06-19T02:05:04Z",
    "SentDateTime": "2015-06-19T02:04:59Z",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsDraft": false,
    "IsRead": true,
    "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===%2FNJTqt5NqHlVnKVBwCY4MQpaFz9SbqUDe4%2Bbs88AAAAAAEJAACY4MQpaFz9SbqUDe4%2Bbs88AAApA4JMAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
    "MentionedMe": null,
    "Mentioned": [
    ],
    "InferenceClassification": "Focused",
    "Extensions@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Users('desmond40contoso.com')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions", 
    "Extensions": [ 
      { 
        "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
        "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
        "ExtensionName": "Com.Contoso.Referral",
        "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
        "CompanyName": "Wingtip Toys",
        "DealValue": 500050,
        "ExpirationDate": "2015-12-03T10:00:00Z"
      }
     ]
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


****

<a name="FindAndExpandItemsWithExtension"></a>
### 拡張情報を使用してアイテムを検索したり展開したりする

フィルターと一致する拡張情報が含まれているリソースのインスタンスを検索できます。 さらに、同じクエリで、この拡張情報を使用して展開されたこれらのインスタンスを取得できます。 このセクションのクエリは、このようなインスタンスを検索し、展開して、応答に拡張情報を含めます。 

Office 365 または Outlook.com のメッセージ、予定表イベント、または連絡先をリソースにすることができます。

次のように、`Id` で拡張情報名または完全修飾名に対してフィルター処理してから、拡張情報を使用して展開されたインスタンスを取得できます。 フィルター文字列内のスペース文字に [URL エンコード](http://www.w3schools.com/tags/ref_urlencode.asp)を適用していることを確認してください。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$filter=Extensions/any(f:f/Id eq '{extensionId}')&$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/beta/me/events?$filter=Extensions/any(f:f/Id eq '{extensionId}')&$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/beta/me/contacts?$filter=Extensions/any(f:f/Id eq '{extensionId}')&$expand=Extensions($filter=Id eq '{extensionId}')
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$filter=Extensions/any(f:f/Id eq '{extensionId}')&$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/v2.0/me/events?$filter=Extensions/any(f:f/Id eq '{extensionId}')&$expand=Extensions($filter=Id eq '{extensionId}')

GET https://outlook.office.com/api/v2.0/me/contacts?$filter=Extensions/any(f:f/Id eq '{extensionId}')&$expand=Extensions($filter=Id eq '{extensionId}')
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


_**最小限必要なスコープ**: 次の対象リソースに対応する読み取りスコープのうちのいずれかです。_
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|extensionId|string|リソース インスタンスのすべての拡張情報間で一意のテキスト識別子である拡張機能名にするか、拡張情報の種類と一意のテキスト識別子を連結した完全修飾名にすることができます。完全修飾名は、拡張情報の作成時に id プロパティ内に返されます。必須。 |

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**要求のサンプル**

次の例では、サインインしているユーザーのメールボックス内のすべてのメッセージを検索して、フィルターと一致する拡張情報が含まれているメッセージを検出し、拡張情報を組み込んでそれらのメッセージを展開します。 フィルターは、`Id` が拡張情報名 `Com.Contoso.Referral` と一致する拡張情報を返します。

利便性を考慮して、以下の要求では、予約文字のスペースの URL エンコーディングで示しています。

```
GET https://outlook.office.com/api/beta/me/messages?$filter=Extensions/any(f:f/Id%20eq%20'Com.Contoso.Referral')&$expand=Extensions($filter=Id%20eq%20'Com.Contoso.Referral')
```


**応答のサンプル**

成功した応答は、`HTTP 200 OK` 応答コードで示されます。

応答本文には、拡張情報が一致するすべてのメッセージとすべてのメッセージ プロパティが含まれています。この例では、応答には 2 つのメッセージが含まれています。

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages",
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.EventMessage",
            "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyDREAAA=')",
            "@odata.etag": "W/\"DAAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ9tn\"",
            "Id": "AAMkADIyDREAAA=",
            "CreatedDateTime": "2016-01-06T02:20:17Z",
            "LastModifiedDateTime": "2016-01-13T02:13:54Z",
            "ChangeKey": "DAAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ9tn",
            "Categories": [
            ],
            "ReceivedDateTime": "2016-01-06T02:20:18Z",
            "SentDateTime": "2016-01-06T02:20:18Z",
            "HasAttachments": false,
            "InternetMessageId": "<BY1PR19MB0023E92E0B43F5E268406F0DF5F40@BY1PR19MB0023.namprd19.prod.outlook.com>",
            "Subject": "Accepted: Latin American Product Manual Group",
            "Body": {
                "ContentType": "Text",
                "Content": ""
            },
            "BodyPreview": "",
            "Importance": "Normal",
            "ParentFolderId": "AAMkADIyAAAAEJAAA=",
            "Sender": {
                "EmailAddress": {
                    "Name": "MOD Administrator",
                    "Address": "admin@adatumltd.onmicrosoft.com"
                }
            },
            "From": {
                "EmailAddress": {
                    "Name": "MOD Administrator",
                    "Address": "admin@adatumltd.onmicrosoft.com"
                }
            },
            "ToRecipients": [
                {
                    "EmailAddress": {
                        "Name": "Engineering",
                        "Address": "engineering@adatumltd.onmicrosoft.com"
                    }
                }
            ],
            "CcRecipients": [
            ],
            "BccRecipients": [
            ],
            "ReplyTo": [
            ],
            "ConversationId": "AAQkADIyWejUoYAg=",
            "IsDeliveryReceiptRequested": null,
            "IsReadReceiptRequested": false,
            "IsRead": true,
            "IsDraft": false,
            "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADIyDREAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
            "InferenceClassification": "Focused",
            "UnsubscribeData": [
            ],
            "UnsubscribeEnabled": false,
            "MeetingMessageType": "MeetingAccepted",
            "StartDateTime": {
                "DateTime": "2015-01-05T19:00:00.0000000",
                "TimeZone": "UTC"
            },
            "EndDateTime": {
                "DateTime": "2015-01-05T20:30:00.0000000",
                "TimeZone": "UTC"
            },
            "Location": {
                "DisplayName": "Mt. Adams"
            },
            "Type": "SeriesMaster",
            "Recurrence": {
                "Pattern": {
                    "Type": "RelativeMonthly",
                    "Interval": 2,
                    "Month": 0,
                    "DayOfMonth": 0,
                    "DaysOfWeek": [
                        "Monday"
                    ],
                    "FirstDayOfWeek": "Sunday",
                    "Index": "First"
                },
                "Range": {
                    "Type": "NoEnd",
                    "StartDate": "2015-01-05",
                    "EndDate": "0001-01-01",
                    "RecurrenceTimeZone": "tzone://Microsoft/Utc",
                    "NumberOfOccurrences": 0
                }
            },
            "IsOutOfDate": false,
            "Extensions@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkADIyDREAAA%3D')/Extensions",
            "Extensions": [
                {
                    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
                    "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyDREAAA=')/Extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
                    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
                    "ExtensionName": "Com.Contoso.Referral",
                    "CompanyName": "Wingtip Toys",
                    "DealValue@odata.type": "#Int64",
                    "DealValue": 500300,
                    "ExpirationDate": "2015-12-03T10:00:00.000Z"
                }
            ],
            "Event@odata.associationLink": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Events('AAMkADIyAAAAABrdAAA=')/$ref",
            "Event@odata.navigationLink": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Events('AAMkADIyAAAAABrdAAA=')"
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyAHVAAA=')",
            "@odata.etag": "W/\"CQAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ6aq\"",
            "Id": "AAMkADIyAHVAAA=",
            "CreatedDateTime": "2016-01-06T02:20:02Z",
            "LastModifiedDateTime": "2016-01-13T02:24:50Z",
            "ChangeKey": "CQAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ6aq",
            "Categories": [
            ],
            "ReceivedDateTime": "2016-01-06T02:20:02Z",
            "SentDateTime": "2016-01-06T02:20:01Z",
            "HasAttachments": false,
            "InternetMessageId": "<CY1PR19MB0032531C620A46068FDDD45DE3F40@CY1PR19MB0032.namprd19.prod.outlook.com>",
            "Subject": "International Launch Planning for XT2000",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=us-ascii\">\r\n</head>\r\n<body>\r\nWe will be ready to ship XT 2000 on 10/1, how's the International launch plan going?<br>\r\n<div style=\"display:inline-block\">\r\n<table border=\"0\" cellspacing=\"0\" style=\"background-color:#F4F4F4\">\r\n<tbody>\r\n<tr>\r\n<td style=\"padding:20px; font-size:12px; color:#666666\">You're receiving this message because you're a subscribed member of the Engineering group.<br>\r\n<a href=\"https://outlook.office.com/owa/engineering@adatumltd.onmicrosoft.com/groupsubscription.ashx?realm=adatumltd.onmicrosoft.com&amp;action=conversations&amp;source=EscalatedMessage\">View group conversations</a> |\r\n<a href=\"https://adatumltd.sharepoint.com/_layouts/groupstatus.aspx?id=dbcbe107-6244-40ba-b0f1-1c46ad35435d&amp;target=documents\">\r\nView group files</a> | <a id=\"BD5134C6-8D33-4ABA-A0C4-08581FDF89DB\" href=\"https://outlook.office.com/owa/engineering@adatumltd.onmicrosoft.com/groupsubscription.ashx?realm=adatumltd.onmicrosoft.com&amp;action=unsubscribe&amp;source=EscalatedMessage\">\r\nUnsubscribe</a></td>\r\n</tr>\r\n</tbody>\r\n</table>\r\n</div>\r\n</body>\r\n</html>\r\n"
            },
            "BodyPreview": "We will be ready to ship XT 2000 on 10/1, how's the International launch plan going?\r\nYou're receiving this message because you're a subscribed member of the Engineering group.\r\nView group conversations | View group files | Unsubscribe",
            "Importance": "Normal",
            "ParentFolderId": "AAMkADIyAAAEMAAA=",
            "Sender": {
                "EmailAddress": {
                    "Name": "Engineering",
                    "Address": "engineering@adatumltd.onmicrosoft.com"
                }
            },
            "From": {
                "EmailAddress": {
                    "Name": "Garret Vargas",
                    "Address": "GarretV@adatumltd.onmicrosoft.com"
                }
            },
            "ToRecipients": [
                {
                    "EmailAddress": {
                        "Name": "Engineering",
                        "Address": "engineering@adatumltd.onmicrosoft.com"
                    }
                }
            ],
            "CcRecipients": [
            ],
            "BccRecipients": [
            ],
            "ReplyTo": [
            ],
            "ConversationId": "AAQkADIyLESZnSPc=",
            "IsDeliveryReceiptRequested": null,
            "IsReadReceiptRequested": false,
            "IsRead": false,
            "IsDraft": false,
            "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADIyAHVAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
            "InferenceClassification": "Focused",
            "UnsubscribeData": [
            ],
            "UnsubscribeEnabled": false,
            "Extensions@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkADIyAHVAAA%3D')/Extensions",
            "Extensions": [
                {
                    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
                    "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyAHVAAA=')/Extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
                    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
                    "ExtensionName": "Com.Contoso.Referral",
                    "CompanyName": "Wingtip Toys",
                    "DealValue@odata.type": "#Int64",
                    "DealValue": 500050,
                    "ExpirationDate": "2015-12-03T10:00:00.000Z"
                }
            ]
        }
    ]
} 
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**要求のサンプル**

次の例では、サインインしているユーザーのメールボックス内のすべてのメッセージを検索して、フィルターと一致する拡張情報が含まれているメッセージを検出し、拡張情報を組み込んでそれらのメッセージを展開します。 フィルターは、`Id` が拡張情報名 `Com.Contoso.Referral` と一致する拡張情報を返します。

利便性を考慮して、以下の要求では、予約文字のスペースの URL エンコーディングで示しています。

```
GET https://outlook.office.com/api/v2.0/me/messages?$filter=Extensions/any(f:f/Id%20eq%20'Com.Contoso.Referral')&$expand=Extensions($filter=Id%20eq%20'Com.Contoso.Referral')
```


**応答のサンプル**

成功した応答は、`HTTP 200 OK` 応答コードで示されます。

応答本文には、拡張情報が一致するすべてのメッセージとすべてのメッセージ プロパティが含まれています。この例では、応答には 2 つのメッセージが含まれています。

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages",
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.EventMessage",
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyDREAAA=')",
            "@odata.etag": "W/\"DAAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ9tn\"",
            "Id": "AAMkADIyDREAAA=",
            "CreatedDateTime": "2016-01-06T02:20:17Z",
            "LastModifiedDateTime": "2016-01-13T02:13:54Z",
            "ChangeKey": "DAAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ9tn",
            "Categories": [
            ],
            "ReceivedDateTime": "2016-01-06T02:20:18Z",
            "SentDateTime": "2016-01-06T02:20:18Z",
            "HasAttachments": false,
            "InternetMessageId": "<BY1PR19MB0023E92E0B43F5E268406F0DF5F40@BY1PR19MB0023.namprd19.prod.outlook.com>",
            "Subject": "Accepted: Latin American Product Manual Group",
            "Body": {
                "ContentType": "Text",
                "Content": ""
            },
            "BodyPreview": "",
            "Importance": "Normal",
            "ParentFolderId": "AAMkADIyAAAAEJAAA=",
            "Sender": {
                "EmailAddress": {
                    "Name": "MOD Administrator",
                    "Address": "admin@adatumltd.onmicrosoft.com"
                }
            },
            "From": {
                "EmailAddress": {
                    "Name": "MOD Administrator",
                    "Address": "admin@adatumltd.onmicrosoft.com"
                }
            },
            "ToRecipients": [
                {
                    "EmailAddress": {
                        "Name": "Engineering",
                        "Address": "engineering@adatumltd.onmicrosoft.com"
                    }
                }
            ],
            "CcRecipients": [
            ],
            "BccRecipients": [
            ],
            "ReplyTo": [
            ],
            "ConversationId": "AAQkADIyWejUoYAg=",
            "IsDeliveryReceiptRequested": null,
            "IsReadReceiptRequested": false,
            "IsRead": true,
            "IsDraft": false,
            "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADIyDREAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
            "InferenceClassification": "Focused",
            "UnsubscribeData": [
            ],
            "UnsubscribeEnabled": false,
            "MeetingMessageType": "MeetingAccepted",
            "StartDateTime": {
                "DateTime": "2015-01-05T19:00:00.0000000",
                "TimeZone": "UTC"
            },
            "EndDateTime": {
                "DateTime": "2015-01-05T20:30:00.0000000",
                "TimeZone": "UTC"
            },
            "Location": {
                "DisplayName": "Mt. Adams"
            },
            "Type": "SeriesMaster",
            "Recurrence": {
                "Pattern": {
                    "Type": "RelativeMonthly",
                    "Interval": 2,
                    "Month": 0,
                    "DayOfMonth": 0,
                    "DaysOfWeek": [
                        "Monday"
                    ],
                    "FirstDayOfWeek": "Sunday",
                    "Index": "First"
                },
                "Range": {
                    "Type": "NoEnd",
                    "StartDate": "2015-01-05",
                    "EndDate": "0001-01-01",
                    "RecurrenceTimeZone": "tzone://Microsoft/Utc",
                    "NumberOfOccurrences": 0
                }
            },
            "IsOutOfDate": false,
            "Extensions@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkADIyDREAAA%3D')/Extensions",
            "Extensions": [
                {
                    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
                    "@odata.id": "https://outlook.office.com/api/v2.0/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyDREAAA=')/Extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
                    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
                    "ExtensionName": "Com.Contoso.Referral",
                    "CompanyName": "Wingtip Toys",
                    "DealValue@odata.type": "#Int64",
                    "DealValue": 500300,
                    "ExpirationDate": "2015-12-03T10:00:00.000Z"
                }
            ],
            "Event@odata.associationLink": "https://outlook.office.com/api/v2.0/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Events('AAMkADIyAAAAABrdAAA=')/$ref",
            "Event@odata.navigationLink": "https://outlook.office.com/api/v2.0/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Events('AAMkADIyAAAAABrdAAA=')"
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyAHVAAA=')",
            "@odata.etag": "W/\"CQAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ6aq\"",
            "Id": "AAMkADIyAHVAAA=",
            "CreatedDateTime": "2016-01-06T02:20:02Z",
            "LastModifiedDateTime": "2016-01-13T02:24:50Z",
            "ChangeKey": "CQAAABYAAACGYzsRv+OAR5zyXf6CQkRrAAADJ6aq",
            "Categories": [
            ],
            "ReceivedDateTime": "2016-01-06T02:20:02Z",
            "SentDateTime": "2016-01-06T02:20:01Z",
            "HasAttachments": false,
            "InternetMessageId": "<CY1PR19MB0032531C620A46068FDDD45DE3F40@CY1PR19MB0032.namprd19.prod.outlook.com>",
            "Subject": "International Launch Planning for XT2000",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=us-ascii\">\r\n</head>\r\n<body>\r\nWe will be ready to ship XT 2000 on 10/1, how's the International launch plan going?<br>\r\n<div style=\"display:inline-block\">\r\n<table border=\"0\" cellspacing=\"0\" style=\"background-color:#F4F4F4\">\r\n<tbody>\r\n<tr>\r\n<td style=\"padding:20px; font-size:12px; color:#666666\">You're receiving this message because you're a subscribed member of the Engineering group.<br>\r\n<a href=\"https://outlook.office.com/owa/engineering@adatumltd.onmicrosoft.com/groupsubscription.ashx?realm=adatumltd.onmicrosoft.com&amp;action=conversations&amp;source=EscalatedMessage\">View group conversations</a> |\r\n<a href=\"https://adatumltd.sharepoint.com/_layouts/groupstatus.aspx?id=dbcbe107-6244-40ba-b0f1-1c46ad35435d&amp;target=documents\">\r\nView group files</a> | <a id=\"BD5134C6-8D33-4ABA-A0C4-08581FDF89DB\" href=\"https://outlook.office.com/owa/engineering@adatumltd.onmicrosoft.com/groupsubscription.ashx?realm=adatumltd.onmicrosoft.com&amp;action=unsubscribe&amp;source=EscalatedMessage\">\r\nUnsubscribe</a></td>\r\n</tr>\r\n</tbody>\r\n</table>\r\n</div>\r\n</body>\r\n</html>\r\n"
            },
            "BodyPreview": "We will be ready to ship XT 2000 on 10/1, how's the International launch plan going?\r\nYou're receiving this message because you're a subscribed member of the Engineering group.\r\nView group conversations | View group files | Unsubscribe",
            "Importance": "Normal",
            "ParentFolderId": "AAMkADIyAAAEMAAA=",
            "Sender": {
                "EmailAddress": {
                    "Name": "Engineering",
                    "Address": "engineering@adatumltd.onmicrosoft.com"
                }
            },
            "From": {
                "EmailAddress": {
                    "Name": "Garret Vargas",
                    "Address": "GarretV@adatumltd.onmicrosoft.com"
                }
            },
            "ToRecipients": [
                {
                    "EmailAddress": {
                        "Name": "Engineering",
                        "Address": "engineering@adatumltd.onmicrosoft.com"
                    }
                }
            ],
            "CcRecipients": [
            ],
            "BccRecipients": [
            ],
            "ReplyTo": [
            ],
            "ConversationId": "AAQkADIyLESZnSPc=",
            "IsDeliveryReceiptRequested": null,
            "IsReadReceiptRequested": false,
            "IsRead": false,
            "IsDraft": false,
            "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADIyAHVAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
            "InferenceClassification": "Focused",
            "UnsubscribeData": [
            ],
            "UnsubscribeEnabled": false,
            "Extensions@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkADIyAHVAAA%3D')/Extensions",
            "Extensions": [
                {
                    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
                    "@odata.id": "https://outlook.office.com/api/v2.0/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Messages('AAMkADIyAHVAAA=')/Extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
                    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
                    "ExtensionName": "Com.Contoso.Referral",
                    "CompanyName": "Wingtip Toys",
                    "DealValue@odata.type": "#Int64",
                    "DealValue": 500050,
                    "ExpirationDate": "2015-12-03T10:00:00.000Z"
                }
            ]
        }
    ]
} 
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



****

<a name="UpdateExtension"></a>

### 拡張情報のデータを追加したり変更したりする

要求本文内のプロパティを使用して拡張機能を更新します。
- 要求本文内のプロパティが拡張情報内の既存のプロパティの名前と一致すると、拡張情報内のデータが更新されます。
- 一致しないと、このプロパティとそのデータは拡張情報に追加されます。 

拡張情報は、Office 365 または Outlook.com のメッセージ、予定表イベント、または連絡先のリソース内にあります。名前か ID で参照でき、どちらの方法でも同じ応答を返します。JSON ペイロード内のデータは、プリミティブ型か、プリミティブ型の配列にすることができます。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
PATCH https://outlook.office.com/api/beta/me/messages('{message_id}')/extensions('{extensionId}')
PATCH https://outlook.office.com/api/beta/me/events('{event_id}')/extensions('{extensionId}')
PATCH https://outlook.office.com/api/beta/me/contacts('{contact_id}')/extensions('{extensionId}')
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/messages('{message_id}')/extensions('{extensionId}')
PATCH https://outlook.office.com/api/v2.0/me/events('{event_id}')/extensions('{extensionId}')
PATCH https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/extensions('{extensionId}')
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


_**最小限必要なスコープ**: 次の対象リソースに対応する読み取り/書き込みスコープのうちのいずれかです。_
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|extensionId|string|リソース インスタンスのすべての拡張情報間で一意のテキスト識別子である拡張機能名にするか、拡張情報の種類と一意のテキスト識別子を連結した完全修飾名にすることができます。完全修飾名は、拡張情報の作成時に id プロパティ内に返されます。必須。 |
|_本文パラメーター_|
|ExtensionName|string|拡張情報の一意のテキスト識別子。 必須。|


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**要求のサンプル**

このセクションの 2 つの例では、それぞれ上記の[「拡張情報を取得する」の例](#GetExtensionExample)の拡張情報を使用しています。最初の例は拡張情報を名前で参照し、2 番目の例は ID で参照しています。要求本文と応答は同じです。

各例では、次の方法で[上記の拡張情報](#GetExtensionExample)を更新します。
- `CompanyName` を `Wingtip Toys` から `Wingtip Toys (USA)` に変更する
- `DealValue` を `500050` から `500100` に変更する
- 新しいデータをカスタム プロパティ `Updated` として追加する

次の例は、拡張情報を名前で参照します。
```
PATCH https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions('Com.Contoso.Referral')

Content-Type: application/json

{
    "@odata.type": "Microsoft.OutlookServices.OpenTypeExtension",
    "ExtensionName": "Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys (USA)",
    "DealValue": "500100",
    "ExpirationDate": "2015-12-03T10:00:00.000Z",
    "Updated": "2015-10-29T11:00:00.000Z"
} 
```


次の例は、拡張情報を ID (完全修飾名) で参照します。
```
PATCH https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')

Content-Type: application/json

{
    "@odata.type": "Microsoft.OutlookServices.OpenTypeExtension",
    "ExtensionName": "Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys (USA)",
    "DealValue": "500100",
    "ExpirationDate": "2015-12-03T10:00:00.000Z",
    "Updated": "2015-10-29T11:00:00.000Z"
} 
```

**応答のサンプル**

成功した応答は `HTTP 200 OK` 応答コードで示され、応答本文内に更新された拡張情報が示されています。

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions
('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
    "ExtensionName": "Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys (USA)",
    "DealValue": 500100,
    "ExpirationDate": "2015-12-03T10:00:00Z",
    "Updated": "2015-10-29T11:00:00.000Z"
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**要求のサンプル**

このセクションの 2 つの例では、それぞれ上記の[「拡張情報を取得する」の例](#GetExtensionExample)の拡張情報を使用しています。最初の例は拡張情報を名前で参照し、2 番目の例は ID で参照しています。要求本文と応答は同じです。

各例では、次の方法で[上記の拡張情報](#GetExtensionExample)を更新します。
- `CompanyName` を `Wingtip Toys` から `Wingtip Toys (USA)` に変更する
- `DealValue` を `500050` から `500100` に変更する
- 新しいデータをカスタム プロパティ `Updated` として追加する

次の例は、拡張情報を名前で参照します。
```
PATCH https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions('Com.Contoso.Referral')

Content-Type: application/json

{
    "@odata.type": "Microsoft.OutlookServices.OpenTypeExtension",
    "ExtensionName": "Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys (USA)",
    "DealValue": "500100",
    "ExpirationDate": "2015-12-03T10:00:00.000Z",
    "Updated": "2015-10-29T11:00:00.000Z"
} 
```


次の例は、拡張情報を ID (完全修飾名) で参照します。
```
PATCH https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')

Content-Type: application/json

{
    "@odata.type": "Microsoft.OutlookServices.OpenTypeExtension",
    "ExtensionName": "Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys (USA)",
    "DealValue": "500100",
    "ExpirationDate": "2015-12-03T10:00:00.000Z",
    "Updated": "2015-10-29T11:00:00.000Z"
} 
```

**応答のサンプル**

成功した応答は `HTTP 200 OK` 応答コードで示され、応答本文内に更新された拡張情報が示されています。

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/Extensions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.OpenTypeExtension",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfc984d-b826-40d7-b48b-57002df85e00@1717f226-49d1-4d0c-9d74-709fad6677b4')/Messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions
('Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral')",
    "Id": "Microsoft.OutlookServices.OpenTypeExtension.Com.Contoso.Referral",
    "ExtensionName": "Com.Contoso.Referral",
    "CompanyName": "Wingtip Toys (USA)",
    "DealValue": 500100,
    "ExpirationDate": "2015-12-03T10:00:00Z",
    "Updated": "2015-10-29T11:00:00.000Z"
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


****

<a name="DeleteExtension"> </a>

### 拡張情報を削除する

指定されたリソースのインスタンスから拡張情報を削除します。Office 365 または Outlook.com のメッセージ、予定表イベント、または連絡先をリソースにすることができます。

サポートされている各リソースのインスタンスの拡張情報を削除するには、次のようにします。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
DELETE https://outlook.office.com/api/beta/me/messages('{message_id}')/extensions('{extension_name}')
DELETE https://outlook.office.com/api/beta/me/events('{event_id}')/extensions('{extension_name}')
DELETE https://outlook.office.com/api/beta/me/contacts('{contact_id}')/extensions('{extension_name}')
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/messages('{message_id}')/extensions('{extension_name}')
DELETE https://outlook.office.com/api/v2.0/me/events('{event_id}')/extensions('{extension_name}')
DELETE https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/extensions('{extension_name}')
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


_**最小限必要なスコープ**: 次の対象リソースに対応する読み取り/書き込みスコープのうちのいずれかです。_
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|extension_name|string|拡張情報の一意のテキスト識別子。 必須。|

 
**要求のサンプル**

次の例は、拡張情報を名前で参照し、指定されたメッセージ内の拡張情報を削除します。 

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```
DELETE https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Com.Contoso.Referral')
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```
DELETE https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVl===')/extensions('Com.Contoso.Referral')
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


**応答のサンプル**

成功した応答は、`HTTP 204 No Content` 応答コードで示されます。 

****


## 拡張情報のエンティティ

<a name="ExtensionEntity"> </a>
### Extension

型:**Microsoft.OutlookServices.Entity**

基本型が **Entity** エンティティの抽象型エンティティ。


<a name="OpenTypeExtensionEntity"> </a>
### OpenTypeExtension

型:**Microsoft.OutlookServices.Extension**

エンティティ型のインスタンスの更新目的で JSON ペイロード内でのプロパティとデータの動的指定をサポートする抽象型の OData v4 オープン型。   

|プロパティ|型|説明|書き込み可能|フィルター処理可能
|:-----|:-----|:-----|:-----|:-----|
|ExtensionName| string |一意の拡張情報名。Com.Contoso.Contact など、独自のドメインに基づくドメイン ネーム システム逆引きを使用します。 |はい|いいえ|



****


<a name="NextSteps"> </a>
## 次の手順

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API の使用を開始します](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](https://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)

- [メール REST API リファレンス](..\api\mail-rest-operations.md)

- [予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API (プレビュー)](..\api\task-rest-operations.md)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)



[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]






