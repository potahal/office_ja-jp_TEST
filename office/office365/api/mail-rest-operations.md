---
ms.Toctitle: Outlook Mail REST API reference
title: "Outlook メール REST API リファレンス"
description: "ms.TocTitle:Outlook メール REST API リファレンスTitle:Outlook メール REST API リファレンスDescription:フォルダー、電子メール メッセージ、および電子メールの添付ファイルへのアクセスを提供するメール REST API とクライアント ライブラリ API を操作する方法のリファレンスです。ms.ContentId: aaf0a812-2ddb-4a63-969f-88916c96ab01ms.topic: リファレンス (API) ms.date:2016 年 5 月 19 日"
ms.ContentId: aaf0a812-2ddb-4a63-969f-88916c96ab01
ms.date: July 19, 2016

---


[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Outlook メール REST API リファレンス

[!INCLUDE [Add the Outlook REST API filters--v2 default](../includes/controls/addOutlookVersion_v2default.xml)]

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


<p class="previewnote">This documentation covers synchronizing messages and their folders, unsubscribing messages, automatic replies, quick replies, and reference attachments which are all currently in preview. Preview features are subject to change prior to finalization, and may break code that uses them. プレビュー機能は、最終処理までに変更される場合があります。それらを使用するコードを破棄する場合もあります。このため、一般に、実稼働コードでは実稼働バージョンの API のみを使用してください。可能な場合には、現在バージョン 2.0 が優先バージョンです。</p> If available, v2.0 is currently the preferred version.</p>

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


 _**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

Outlook メール API では、メッセージと添付ファイルの読み取り、作成、送信、イベント メッセージの表示と応答、および Office 365 での Azure Active Directory で保護されているフォルダーの管理が可能です。また、次のドメインでは、特に Microsoft アカウントと同じ機能も提供されます:Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com。 Outlook メール API では、メッセージと添付ファイルの読み取り、作成、送信、イベント メッセージの表示と応答、および Office 365 での Azure Active Directory で保護されているフォルダーの管理が可能です。また、次のドメインでは、特に Microsoft アカウントと同じ機能も提供されます:Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com。


<!-- Can add the following sentence back once the client libraries have been updated for converged auth and outlook.com

You can access the Mail API by calling the corresponding REST APIs directly in your apps, or by using the Office 365 client libraries and SDKs.
-->

**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**Not interested in the beta version of the API?** API v2.0 が不要な場合右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**Not interested in v2.0 of the API?** API v1.0 に関心がない場合: 右上のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

**Not interested in v1.0 of the API?** API v1.0 に関心がない場合: 右上のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



## すべてのメール API の操作

**Message operations** Messages are stored in mailbox folders, so message endpoints often include the folder that contains the message.
メッセージはメールボックス フォルダーに格納されるため、多くの場合、メッセージ エンドポイントには、メッセージを含むフォルダーが含まれます。フォルダーは ID または既知のフォルダー名のいずれか (Inbox、Drafts、SentItems、または DeletedItems) で指定されます。

[Get messages](#GetMessages) | [Synchronize messages](#SynchronizeMessagesAPI) (preview) | [Create and send messages](#SendMessages) | [Reply or reply all to messages](#ReplyToMessages) | 
[Forward new or drafted messages](#ForwardMessages) | [Update messages](#UpdateMessages) | [Delete messages](#DeleteMessages) | [Move or copy messages](#MoveCopyMessages) |  
[Manage Focused Inbox](#ManageFocusedInbox) | 
[Unsubscribe](#Unsubscribe) (preview) | [Get auto reply settings](#GetAutoReplySettings) (preview) | [Update auto reply settings](#UpdateAutoReplySettings) (preview) | [Get attachments](#GetAttachments) | 
[Create attachments](#CreateAttachments) | [Delete attachments](#DeleteAttachments)


**Folder operations** Mailbox folders can contain messages and other folders. You can get, create, change, delete, and manage folders.
メールボックス フォルダーにはメッセージと他のフォルダーを含めることができます。フォルダーを取得、作成、変更、削除、管理することができます。ID の代わりに既知のフォルダー名 (Inbox、SentItems、Drafts、または DeletedItems) を使用して、対応するフォルダーを指定することができます。

[Get folders](#GetFolders) | [Synchronize folders](#SynchronizeFolders) (preview) | [Create folders](#CreateFolders) | [Update folders](#UpdateFolders) | [Delete folders](#DeleteFolders) | [Move or copy folders](#MoveCopyFolders)

関連項目:

REST API メッセージ リソース  REST API フォルダー リファレンス

<a name="Overview"> </a>
## メール REST API を使用する

###認証

Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the Mail API, you should include a valid access token. アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you.
Keep this in mind as you proceed with the specific operations in the Mail API.

<a name="SupportedVersions"> </a>

###API のバージョン

メール REST API は、すべてのバージョンの Outlook REST API でサポートされています。機能は、特定のバージョンによって異なる場合があります。

###対象ユーザー

すべてのメール API 要求は、指定しない限り常にサインイン ユーザーのために実行されます。優先受信トレイ API など、一部の API サブセットは、サインイン ユーザーまたはユーザー ID で指定したユーザーで、適切な権限を付与して実行できます。

Outlook REST API のすべてのサブセットに共通な情報について詳しくは、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」をご覧ください。

****


<a name="GetMessages"> </a>
##メッセージを取得する

メールボックス フォルダーからメッセージ コレクションまたは個々のメッセージを取得できます。

Each message in the response contains multiple properties, including the [Body](..\api\complex-types-for-mail-contacts-calendar.md#MessageBody) property. メッセージ本文は、HTML またはテキストのいずれかにできます。本文が HTML の場合、既定では、Body 応答内の各メッセージには、Body プロパティをはじめとする複数のプロパティが含まれます。メッセージ本文は、HTML またはテキストのいずれかにできます。本文が HTML の場合、既定では、**Body** プロパティに組み込まれている安全ではない可能性がある HTML (たとえば、JavaScript など) が、本文の内容が REST 応答で返される前に削除されます。元の HTML コンテンツ全体を取得するには、次の HTTP 要求ヘッダーを含めます。 元の HTML コンテンツ全体を取得するには、次の HTTP 要求ヘッダーを含めます。
```
Prefer: outlook.allow-unsafe-html
```

REST API:メッセージ コレクションを取得する (REST)  メッセージを取得する (REST)

クライアント ライブラリ:メッセージ コレクションを取得する (クライアント)  メッセージを取得する (クライアント)

<a name="GetMessageCollection"> </a>
###メッセージ コレクションを取得する (REST)

__**Minimum required scope**: 次のいずれか:__ 
- _https://outlook.office.com/mail.read_
- _wl.imap_

**Note** The behavior of the operations in this section vary by version. このセクションの操作の動作は、バージョンによって異なります。ページの右上隅のバージョンを選択して詳細を確認します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

サインインしたユーザーの ([削除済みアイテム] と [低優先メール] フォルダーを除く) メールボックス全体から、メッセージ コレクションを取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages
```

また、ユーザーのメールボックスのフォルダーを指定して、そのフォルダーからメッセージ コレクションを取得することもできます。

```no-highlight
GET https://outlook.office.com/api/beta/me/MailFolders/{folder_id}/messages
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|フォルダーの ID、または既知のフォルダー名 Inbox、Drafts、SentItems、または DeletedItems (特定のフォルダーからメッセージを取得する場合) です。|

**メモ**: 既定では、応答内の各メッセージにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned.
パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内の各メッセージの **Sender** と **Subject** プロパティのみを返すように指定する方法を示しています。**$select**. を使用しない場合のメッセージに返されるプロパティの完全な一覧に関しては、「[メッセージを取得する (REST)](#GetMessage)」の最初の応答サンプルをご参照ください。


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/MailFolders/sentitems/messages/?$select=Sender,Subject
```

**応答のサンプル:**

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailFolders('sentitems')/Messages(Sender,Subject)",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2TIzAAAA=')",
            "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqS\"",
            "Id": "AAMkAGI2TIzAAAA=",
            "Subject": "Meeting Notes",
            "Sender": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2TIy-AAA=')",
            "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP\"",
            "Id": "AAMkAGI2TIy-AAA=",
            "Subject": "Contract Signing",
            "Sender": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.type": "#Microsoft.OutlookServices.EventMessage",
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2TIy9AAA=')",
            "@odata.etag": "W/\"CwAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqJ\"",
            "Id": "AAMkAGI2TIy9AAA=",
            "Subject": "Rob:Alex 1:1",
            "Sender": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        }
    ]
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

サインインしたユーザーの ([削除済みアイテム] と [低優先メール] フォルダーを除く) メールボックス全体から、メッセージ コレクションを取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages
```

また、ユーザーのメールボックスのフォルダーを指定して、そのフォルダーからメッセージ コレクションを取得することもできます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}/messages
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|フォルダーの ID、または既知のフォルダー名 Inbox、Drafts、SentItems、または DeletedItems (特定のフォルダーからメッセージを取得する場合) です。|

**メモ**: 既定では、応答内の各メッセージにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned.
パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内の各メッセージの **Sender** と **Subject** プロパティのみを返すように指定する方法を示しています。**$select**. を使用しない場合のメッセージに返されるプロパティの完全な一覧に関しては、「[メッセージを取得する (REST)](#GetMessage)」の最初の応答サンプルをご参照ください。


**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/MailFolders/sentitems/messages/?$select=Sender,Subject
```

**応答のサンプル:**

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/MailFolders('sentitems')/Messages(Sender,Subject)",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2TIzAAAA=')",
            "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqS\"",
            "Id": "AAMkAGI2TIzAAAA=",
            "Subject": "Meeting Notes",
            "Sender": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2TIy-AAA=')",
            "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP\"",
            "Id": "AAMkAGI2TIy-AAA=",
            "Subject": "Contract Signing",
            "Sender": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.type": "#Microsoft.OutlookServices.EventMessage",
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2TIy9AAA=')",
            "@odata.etag": "W/\"CwAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqJ\"",
            "Id": "AAMkAGI2TIy9AAA=",
            "Subject": "Rob:Alex 1:1",
            "Sender": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        }
    ]
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

受信トレイからメッセージ コレクションを取得します。 

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages
```

また、ユーザーのメールボックスのフォルダーを指定して、そのフォルダーからメッセージ コレクションを取得することもできます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/MailFolders/{folder_id}/messages
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|フォルダーの ID、または既知のフォルダー名 Inbox、Drafts、SentItems、または DeletedItems (特定のフォルダーからメッセージを取得する場合) です。|

**メモ**: 既定では、応答内の各メッセージにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned.
パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内の各メッセージの **Sender** と **Subject** プロパティのみを返すように指定する方法を示しています。**$select**. を使用しない場合のメッセージに返されるプロパティの完全な一覧に関しては、「[メッセージを取得する (REST)](#GetMessage)」の最初の応答サンプルをご参照ください。

[!code-REST-i[mail_api_get_message_collection](./_data/mail_api_get_message_collection.json)]



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

要求された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) コレクションです。 


****

<a name="GetMessage"> </a>
###メッセージを取得する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_

ID でメッセージを取得します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/messages/{message_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|

**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/messages/AAMkAGI2THVSAAA=
```

**応答のサンプル:**

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2THVSAAA=')",
    "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIKz\"",
    "Id": "AAMkAGI2THVSAAA=",
    "CreatedDateTime": "2014-10-20T00:41:57Z",
    "LastModifiedDateTime": "2014-10-20T00:41:57Z",
    "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIKz",
    "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
    "Categories": [],
    "ReceivedDateTime": "2014-10-20T00:41:57Z",
    "SentDateTime": "2014-10-20T00:41:53Z",
    "HasAttachments": true,
    "Subject": "Re: Meeting Notes",
    "Body": {
        "ContentType": "Text",
        "Content": "\n________________________________________\nFrom: Alex D\nSent: Sunday, October 19, 2014 5:28 PM\nTo: Katie Jordan\nSubject: Meeting Notes\n\nPlease send me the meeting notes ASAP\n"
    },
    "BodyPreview": "________________________________________\nFrom: Alex D\nSent: Sunday, October 19, 2014 5:28 PM\nTo: Katie Jordan\nSubject: Meeting Notes\n\nPlease send me the meeting notes ASAP",
    "Importance": "Normal",
    "ParentFolderId": "AAMkAGI2AAEMAAA=",
    "Sender": {
        "EmailAddress": {
            "Name": "Katie Jordan",
            "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
        }
    },
    "From": {
        "EmailAddress": {
            "Name": "Katie Jordan",
            "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Name": "Alex D",
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
            }
        }
    ],
    "CcRecipients": [],
    "BccRecipients": [],
    "ReplyTo": [],
    "ConversationId": "AAQkAGI2yEto=",
    "ConversationIndex": "AQHRh3zqrkAcds2kw==",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsRead": false,
    "IsDraft": false,
    "WebLink": "https://outlook.office365.com/owa/?ItemID=AAMkAGI2THVSAAA%3D&exvsurl=1&viewmodel=ReadMessageItem"
}
```

 **応答の種類**

要求された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource).です。

**メモ**: 既定では、応答に指定されたメッセージのすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内のメッセージの **Sender** と **Subject** プロパティのみを返すように指定する方法を示しています。

```
GET https://outlook.office.com/api/beta/me/messages/AAMkAGE1I5MTAAA=?$select=Sender,Subject
```



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages/{message_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/messages/AAMkAGI2THVSAAA=
```

**応答のサンプル:**

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2THVSAAA=')",
    "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIKz\"",
    "Id": "AAMkAGI2THVSAAA=",
    "CreatedDateTime": "2014-10-20T00:41:57Z",
    "LastModifiedDateTime": "2014-10-20T00:41:57Z",
    "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIKz",
    "Categories": [],
    "ReceivedDateTime": "2014-10-20T00:41:57Z",
    "SentDateTime": "2014-10-20T00:41:53Z",
    "HasAttachments": true,
    "Subject": "Re: Meeting Notes",
    "Body": {
        "ContentType": "Text",
        "Content": "\n________________________________________\nFrom: Alex D\nSent: Sunday, October 19, 2014 5:28 PM\nTo: Katie Jordan\nSubject: Meeting Notes\n\nPlease send me the meeting notes ASAP\n"
    },
    "BodyPreview": "________________________________________\nFrom: Alex D\nSent: Sunday, October 19, 2014 5:28 PM\nTo: Katie Jordan\nSubject: Meeting Notes\n\nPlease send me the meeting notes ASAP",
    "Importance": "Normal",
    "ParentFolderId": "AAMkAGI2AAEMAAA=",
    "Sender": {
        "EmailAddress": {
            "Name": "Katie Jordan",
            "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
        }
    },
    "From": {
        "EmailAddress": {
            "Name": "Katie Jordan",
            "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Name": "Alex D",
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
            }
        }
    ],
    "CcRecipients": [],
    "BccRecipients": [],
    "ReplyTo": [],
    "ConversationId": "AAQkAGI2yEto=",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsRead": false,
    "IsDraft": false,
    "WebLink": "https://outlook.office365.com/owa/?ItemID=AAMkAGI2THVSAAA%3D&exvsurl=1&viewmodel=ReadMessageItem"
}
```

 **応答の種類**

要求された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource).です。

**メモ**: 既定では、応答に指定されたメッセージのすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内のメッセージの **Sender** と **Subject** プロパティのみを返すように指定する方法を示しています。

```
GET https://outlook.office.com/api/v2.0/me/messages/AAMkAGE1I5MTAAA=?$select=Sender,Subject
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages/{message_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|

[!code-REST-i[mail_api_get_message_by_id](./_data/mail_api_get_message_by_id.json)]


 **応答の種類**

要求された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource).です。

**メモ**: 既定では、応答に指定されたメッセージのすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内のメッセージの **Sender** と **Subject** プロパティのみを返すように指定する方法を示しています。

```
GET https://outlook.office.com/api/v1.0/me/messages/AAMkAGEI5MTAAA=?$select=Sender,Subject
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



****

<a name="GetMessagesClient"> </a>
###メッセージ コレクションを取得する (クライアント)

Get the messages in the Inbox by using the `Me.Messages` shortcut property. To get the messages from a different folder, use the folder's **Messages** property.
You can use the following well-known folder names instead of the ID for the corresponding folder: `Inbox`, `SentItems`, `Drafts`, `DeletedItems`.

例`outlookClient.Me.Folders["SentItems"].Messages.ExecuteAsync()`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


**メモ**: メッセージ コレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。

この例では、[Outlook サービス クライアントを作成する](..\api\use-outlook-rest-api.md#GetClient)メソッドを呼び出します。

<!-- BEGINSECTION class="tabbedCodeSnippets"  data-resources="OutlookServices.Mail" -->

<!--tested-->
```cs-i
var outlookClient = await CreateOutlookClientAsync("Mail");
    var messages = await outlookClient.Me.Folders["Inbox"].Messages
      .OrderByDescending(m => m.DateTimeReceived)
      .Take(10)
      .ExecuteAsync();

foreach(var message in messages.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine("Message '{0}' received at '{1}'.",
    message.Subject,
    message.DateTimeReceived.ToString());
}

```
```javascript-i
outlookClient.me.folders.getFolder('Inbox').messages.getMessages().orderBy('DateTimeReceived desc').fetchAll(10).then(function (result) {
    result.forEach(function (message) {
        console.log('Message "' + message.subject + '" received at "' + message.dateTimeReceived.toString() + '"');
    });
}, function(error) {
    console.log(error);
});
```

<!-- ENDSECTION -->

****

<a name="GetMessageClient"> </a>
###メッセージを取得する (クライアント)

特定のメッセージを取得するには、**メッセージ** コレクションのインデックスとしてメッセージ ID を指定するか、**GetById** メソッドを使用します。

****

<a name="SynchronizeMessagesAPI"></a>
##メッセージを同期する (プレビュー)

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_

You can synchronize your local data store with the messages on the server. Message synchronization is a per-folder operation, for example, you can synchronize all of the messages in your Inbox. To synchronize the messages in a folder hierarchy you need to synchronize each folder individually.

API では、フォルダー内のすべてのメッセージを取得する完全な同期と、最後の完全な同期から変更されたすべてのメッセージを取得する差分同期の両方がサポートされています。

Synchronizing a mail folder typically requires two or more GET requests. You make the GET request much like the way you [get messages](#GetMessages), except that you include certain request headers, and a _deltaToken_ or _skipToken_ when appropriate.

**要求ヘッダー**

- 以前の同期要求から返される skipToken を含む同期要求を除き、すべての同期要求で "Prefer: odata.track-changes" ヘッダーを指定する必要があります。(skipToken に関する詳細については、以下の 2 番目の応答データのサンプルを参照)。 In the first response, look for the _Preference-Applied: odata.track-changes_ header to confirm that the resource supports synchronizing before proceeding.

- _Prefer: odata.maxpagesize={x}_ ヘッダーを指定して、要求ごとに返されるメッセージの最大数を指定することができます。

Here's a typical round of synchronizing messages:

1. Make the initial GET request with the mandatory _Prefer: odata.track-changes_ header. The initial response to a sync request always returns a _deltaToken_. 2 番目以降の GET 要求は、前の応答で受信した _deltaToken_ または _skipToken_ のいずれかを含むため、最初の GET 要求とは異なります。

2. If the first response returns the _Preference-Applied: odata.track-changes_ header, you can proceed with synchronizing the folder.

  - Make a second GET request. Specify the _Prefer: odata.track-changes_ header and the _deltaToken_ returned from the first GET to determine if there are any additional messages. The second request will return additional messages, and either a _skipToken_ if there are more messages available, or a _deltaToken_ if the last message has been synchronized, in which case you can stop.

  - Continue synchronizing by sending a GET call and including a _skipToken_ that's returned from the previous call. Stop when you get a final response that contains an _@odata.deltaLink_ header with a _deltaToken_ again, which indicates the sync is complete.


### 特定の予定表で同期するには

**最初の要求：**

```no-highlight
GET https://outlook.office365.com/api/beta/me/MailFolders('{folder_id}')/messages
```

**2 番目の要求、またはそれ以降のラウンドの最初の要求：**

```no-highlight
GET https://outlook.office365.com/api/beta/me/MailFolders('{folder_id}')/messages/?$deltaToken={delta_token}
```

**同じラウンドの 3 番目またはそれ以降の要求**

Continue to send the next sync request if the previous response includes a `skipToken`. Stop when you get a response that contains an `@odata.deltaLink` header with a `deltaToken` again.

```no-highlight
GET https://outlook.office365.com/api/beta/me/MailFolders('{folder_id}')/messages/?$skipToken={skip_token}
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer|odata.track-changes|要求が同期要求であることを示します。 Required for the first 2 GET requests in a round.|
|Prefer|odata.maxpagesize|各応答で返されるメッセージの数を設定します。 省略可能。|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。 必須。|
|deltaToken|String|The token that identifies the last sync request for that folder. 以前の同期の応答で、@odata.deltaLink の値の一部として返される deltaToken 文字列。 Required for the second GET request.|
|skipToken|String|ダウンロードするメッセージがまだあることを示すトークン。 以前の同期の応答で、@odata.nextLink の値の一部として返される skipToken 文字列。|


既定では、同期によってフォルダー内のすべてのプロパティとメッセージが返されます。同期では、クエリ式 $select、$top、$expand がサポートされています。$filter および $orderby のサポートは制限されており、$search はサポートされていません。 $select クエリ式を使用して、最適なパフォーマンスに必要なプロパティのみを指定します。常に  The _Id_ property is always returned. 

Synchronization supports the query expressions $select, $top, $expand. There is limited support for $filter and $orderby, and no support for $search.

* サポートされている $filter 式は “$filter=ReceivedDateTime+ge+{value}” または “$filter=ReceivedDateTime+gt+{value}" だけです。
* The only supported $orderby expression is “$orderby=ReceivedDateTime+desc”. サポートされている $orderby 式は “$orderby=ReceivedDateTime+desc” だけです。$orderby 式を含めない場合、戻り値の順序は保証されません。
  
パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


**応答の種類**

A collection containing the requested messages, and a _deltaToken_ or _skipToken_ that you use to request additional pages of message data from server for an incremental synchronization. 




**例**

The following example shows a series of requests to synchronize a specific folder which contains 7 messages. The first sync request specifies returning 2 messages at a time (_odata.maxpagesize_ is 2), and only the **Sender** and **Subject** properties for each message.

- The initial response returns 2 messages, a `deltaLink` and `deltaToken`. 
- The second request uses that `deltaToken`. The second response returns 2 messages, a `nextLink` and `skipToken`.
- To complete the sync, the third and fourth requests use the `skipToken` returned from the previous sync request, until the fourth sync response returns a `deltaLink` and `deltaToken`, in which case this round of sync is complete. このような場合、このラウンドの同期は完了しています。次のラウンドの同期の deltaToken`deltaToken` を保存します。 

**最初の要求のサンプル**

```
GET https://outlook.office.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages?$select=Subject,Sender HTTP/1.1
Prefer: odata.maxpagesize=2
Prefer: odata.track-changes
```


**最初の応答データのサンプル**

The initial response includes a `Preference-Applied: odata.track-changes` header, indicating that this folder supports synchronization. The response also includes two messages and a `deltaToken`.

```
Preference-Applied: odata.track-changes

{
  "@odata.context":"https://outlook.office.com/api/beta/$metadata#Me/MailFolders('AAMkAGI5MAAAwW-j-AAA%3D')/Messages(Subject,Sender)",
  "value":[
    {
      "@odata.id":"https://outlook.office.com/api/beta/Users('f97adce1-d718-4a0e-9af8-b10167e3a346@0d76cf04-f6a0-46cc-947b-d2e1bdd98d11')/Messages('AAMkAGI5MAAAwXADPAAA=')",
      "@odata.etag":"W/\"CQAAABYAAAA+Z5tnXBgBRpOasqS9T0shAAAwYS9+\"",
      "Id":"AAMkAGI5MAAAwXADPAAA=",
      "Subject":"Updates from All Company",
      "Sender":{
        "EmailAddress":{
          "Name":"Contoso Demo on Yammer",
          "Address":"noreply@yammer.com"
        }
      }
    },
    {
      "@odata.id":"https://outlook.office.com/api/beta/Users('f97adce1-d718-4a0e-9af8-b10167e3a346@0d76cf04-f6a0-46cc-947b-d2e1bdd98d11')/Messages('AAMkAGI5MAAAwXADVAAA=')",
      "@odata.etag":"W/\"CQAAABYAAAA+Z5tnXBgBRpOasqS9T0shAAAwYS+E\"",
      "Id":"AAMkAGI5MAAAwXADVAAA=",
      "Subject":"RE: Latin American Ad Campaign - XT Series",
      "Sender":{
        "EmailAddress":{
          "Name":"Alex Darrow",
          "Address":"AlexD@contoso.onmicrosoft.com"
        }
      }
    }
  ],
  "@odata.deltaLink":"https://outlook.office.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages/?%24select=Subject%2cSender&%24deltatoken=MfzCBD5nm2dcGAFGk5qypL1PSyEAADFmX28BAAAA"
}
```

**2 番目の要求のサンプル**

The second request specifies the `deltaToken` returned from the previous response.

```
GET https://outlook.office.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages/?%24select=Subject%2cSender&%24deltatoken=MfzCBD5nm2dcGAFGk5qypL1PSyEAADFmX28BAAAA HTTP/1.1
Prefer: odata.maxpagesize=2
Prefer: odata.track-changes
```

**2 番目の応答データのサンプル**

The second response includes two more messages and a `skipToken`, indicating there are more messages to sync in the folder.

```
{
  "@odata.context":"https://outlook.office.com/api/beta/$metadata#Me/MailFolders('AAMkAGI5MAAAwW-j-AAA%3D')/Messages(Subject,Sender)/$delta",
  "value":[
    {
      "@odata.id":"https://outlook.office.com/api/beta/Users('f97adce1-d718-4a0e-9af8-b10167e3a346@0d76cf04-f6a0-46cc-947b-d2e1bdd98d11')/Messages('AAMkAGI5MAAAwXADQAAA=')",
      "@odata.etag":"W/\"CQAAABYAAAA+Z5tnXBgBRpOasqS9T0shAAAwYS9/\"",
      "Id":"AAMkAGI5MAAAwXADQAAA=",
      "Subject":"International Launch Planning for XT2000",
      "Sender":{
        "EmailAddress":{
          "Name":"Engineering",
          "Address":"engineering@contoso.onmicrosoft.com"
        }
      }
    },
    {
      "@odata.id":"https://outlook.office.com/api/beta/Users('f97adce1-d718-4a0e-9af8-b10167e3a346@0d76cf04-f6a0-46cc-947b-d2e1bdd98d11')/Messages('AAMkAGI5MAAAwXADUAAA=')",
      "@odata.etag":"W/\"CQAAABYAAAA+Z5tnXBgBRpOasqS9T0shAAAwYS+D\"",
      "Id":"AAMkAGI5MAAAwXADUAAA=",
      "Subject":"RE: Latin American Ad Campaign - XT Series",
      "Sender":{
        "EmailAddress":{
          "Name":"Anne Wallace",
          "Address":"AnneW@contoso.onmicrosoft.com"
        }
      }
    }
  ],
  "@odata.nextLink":"https://outlook.office.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages/?%24select=Subject%2cSender&%24skipToken=MfzCAj5nm2dcGAFGk5qypL1PSyEAADFmX28CAAAA"
}
```

**Sample third request**

The thirs request includes the `skipToken` returned from the previous response.

```
GET https://outlook.office.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages/?%24select=Subject%2cSender&%24skipToken=MfzCAj5nm2dcGAFGk5qypL1PSyEAADFmX28CAAAA HTTP/1.1
Prefer: odata.maxpagesize=2
```

**応答データのサンプル**

The third response returns two more messages and another `skipToken`, indicating there are still messages in the folder to sync.

```
{
  "@odata.context":"https://outlook.office.com/api/beta/$metadata#Me/MailFolders('AAMkAGI5MAAAwW-j-AAA%3D')/Messages(Subject,Sender)/$delta",
  "value":[
    {
      "@odata.id":"https://outlook.office.com/api/beta/Users('f97adce1-d718-4a0e-9af8-b10167e3a346@0d76cf04-f6a0-46cc-947b-d2e1bdd98d11')/Messages('AAMkAGI5MAAAwXADTAAA=')",
      "@odata.etag":"W/\"CQAAABYAAAA+Z5tnXBgBRpOasqS9T0shAAAwYS+C\"",
      "Id":"AAMkAGI5MAAAwXADTAAA=",
      "Subject":"RE: Latin American Ad Campaign - XT Series",
      "Sender":{
        "EmailAddress":{
          "Name":"Pavel Bansky",
          "Address":"PavelB@contoso.onmicrosoft.com"
        }
      }
    },
    {
      "@odata.id":"https://outlook.office.com/api/beta/Users('f97adce1-d718-4a0e-9af8-b10167e3a346@0d76cf04-f6a0-46cc-947b-d2e1bdd98d11')/Messages('AAMkAGI5MAAAwXADSAAA=')",
      "@odata.etag":"W/\"CQAAABYAAAA+Z5tnXBgBRpOasqS9T0shAAAwYS+B\"",
      "Id":"AAMkAGI5MAAAwXADSAAA=",
      "Subject":"Latin American Ad Campaign - XT Series",
      "Sender":{
        "EmailAddress":{
          "Name":"Engineering",
          "Address":"engineering@contoso.onmicrosoft.com"
        }
      }
    }
  ],
  "@odata.nextLink":"https://outlook.office.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages/?%24select=Subject%2cSender&%24skipToken=MfzCAj5nm2dcGAFGk5qypL1PSyEAADFmX28DAAAA"
}
```

**Sample fourth request**

The fourth request includes the `skipToken` from the previous response.

```
GET https://outlook.office.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages/?%24select=Subject%2cSender&%24skipToken=MfzCAj5nm2dcGAFGk5qypL1PSyEAADFmX28DAAAA HTTP/1.1
Prefer: odata.maxpagesize=2
```

**Sample fourth and final response data**

The fourth response returns the only remaining message in the folder, and a `deltaToken` which indicates synchronization is complete for this folder. Save the `deltaToken` for the next round of sync for this folder.

```
{
  "@odata.context":"https://outlook.office.com/api/beta/$metadata#Me/MailFolders('AAMkAGI5MAAAwW-j-AAA%3D')/Messages(Subject,Sender)/$delta",
  "value":[
    {
      "@odata.id":"https://outlook.office.com/api/beta/Users('f97adce1-d718-4a0e-9af8-b10167e3a346@0d76cf04-f6a0-46cc-947b-d2e1bdd98d11')/Messages('AAMkAGI5MAAAwXADRAAA=')",
      "@odata.etag":"W/\"CQAAABYAAAA+Z5tnXBgBRpOasqS9T0shAAAwYS+A\"",
      "Id":"AAMkAGI5MAAAwXADRAAA=",
      "Subject":"Data sheets for the XT2000 ",
      "Sender":{
        "EmailAddress":{
          "Name":"Engineering",
          "Address":"engineering@contoso.onmicrosoft.com"
        }
      }
    }
  ],
  "@odata.deltaLink": "https://outlook.office365.com/api/beta/Me/MailFolders('AAMkAGI5MAAAwW-j-AAA=')/messages/?%24deltaToken=0_zCBD5nm2dcGAFGk5qypL1PSyEAADBb9RkEAAAA"
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版でのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版でのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****

<a name="SendMessages"> </a>
##メッセージを作成して送信する

オンザフライで新しいメッセージを送信するか、下書きメッセージを作成してからそれを送信することができます。任意のフォルダーに下書きを作成できます。

REST API:オンザフライで新しいメッセージを送信する (REST)  下書きメッセージを作成する (REST)  下書きメッセージを送信する (REST)

クライアント ライブラリ:オンザフライで新しいメッセージを送信する (クライアント)  下書きメッセージを作成する (クライアント)  下書きメッセージを送信する (クライアント)


<a name="SendMessageOnTheFly"> </a>
###オンザフライで新しいメッセージを送信する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.send_
- _wl.imap_

**SendMail** メソッドを使用して、要求本文で指定されたメッセージを送信します。**Attachments** コレクション プロパティでこれを指定して、同じアクションの呼び出しに添付ファイルを含めることができます。また、[送信済みアイテム] フォルダーにメッセージを保存できます。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/sendmail
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_Body parameters_|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|送信するメッセージです。|
|SavetoSentItems|boolean|[送信済みアイテム] 内のメッセージを保存するかどうかを示します。既定値は true です。 既定は **True** です。|

要求本文に必要な **ToRecipients** プロパティと任意の書き込み可能な [message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) プロパティを設定して、**Message** パラメーターを指定します。**SaveToSentItems** パラメーターは、**false** の場合にのみ必要です。


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/sendmail
Content-Type: application/json

{
  "Message": {
    "Subject": "Meet for lunch?",
    "Body": {
      "ContentType": "Text",
      "Content": "The new cafeteria is open."
    },
    "ToRecipients": [
      {
        "EmailAddress": {
          "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com"
        }
      }
    ],
    "Attachments": [
      {
        "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
        "Name": "menu.txt",
        "ContentBytes": "bWFjIGFuZCBjaGVlc2UgdG9kYXk="
      }
    ]
  },
  "SaveToSentItems": "false"
}
```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/sendmail
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_Body parameters_|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|送信するメッセージです。|
|SavetoSentItems|boolean|[送信済みアイテム] 内のメッセージを保存するかどうかを示します。既定値は true です。 既定は **True** です。|

要求本文に必要な **ToRecipients** プロパティと任意の書き込み可能な [message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) プロパティを設定して、**Message** パラメーターを指定します。**SaveToSentItems** パラメーターは、**false** の場合にのみ必要です。


**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/sendmail

{
  "Message": {
    "Subject": "Meet for lunch?",
    "Body": {
      "ContentType": "Text",
      "Content": "The new cafeteria is open."
    },
    "ToRecipients": [
      {
        "EmailAddress": {
          "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com"
        }
      }
    ],
    "Attachments": [
      {
        "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
        "Name": "menu.txt",
        "ContentBytes": "bWFjIGFuZCBjaGVlc2UgdG9kYXk="
      }
    ]
  },
  "SaveToSentItems": "false"
}

```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/sendmail
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_Body parameters_|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|送信するメッセージです。|
|SavetoSentItems|boolean|[送信済みアイテム] 内のメッセージを保存するかどうかを示します。既定値は true です。 既定は **True** です。|

要求本文に必要な **ToRecipients** プロパティと任意の書き込み可能な [message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) プロパティを設定して、**Message** パラメーターを指定します。**SaveToSentItems** パラメーターは、**false** の場合にのみ必要です。

[!code-REST-i[mail_api_sendmail](./_data/mail_api_sendmail.json)]



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



****

<a name="CreateNewDraft"> </a>
###下書きメッセージを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

Create a draft of a new message. Drafts can be created in any folder and optionally [updated](#UpdateMessages) before [sending](#SendDraftMessages).
To save to the Drafts folder, use the `/me/messages` shortcut.

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/messages
POST https://outlook.office.com/api/beta/me/MailFolders/{folder_id}/messages
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox`Inbox` または Drafts`Drafts` の既知のフォルダー名です。|

要求本文に任意の書き込み可能な [message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) プロパティを指定します。

**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/MailFolders/inbox/messages
Content-Type: application/json

{
  "Subject": "Did you see last night's game?",
  "Importance": "Low",
  "Body": {
    "ContentType": "HTML",
    "Content": "They were <b>awesome</b>!"
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
      }
    }
  ]
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz7k0AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag5\"",
  "Id": "AAMkAGE0Mz7k0AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag5",
  "Categories": [],
  "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
  "CreatedDateTime": "2014-10-18T20:06:51Z",
  "LastModifiedDateTime": "2014-10-18T20:06:51Z",
  "Subject": "Did you see last night's game?",
  "BodyPreview": "They were awesome!",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nThey were <b>awesome</b>!\r\n</body>\r\n</html>\r\n"
  },
  "Importance": "Low",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEPAAA=",
  "From": null,
  "Sender": null,
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0Mpv2hisc=",
  "ConversationIndex": "AQHRf4zqrkAcds2kw==",
  "ReceivedDateTime": "2014-10-18T20:06:51Z",
  "SentDateTime": "2014-10-18T20:06:51Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": true,
  "IsRead": true
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages
POST https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}/messages
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox`Inbox` または Drafts`Drafts` の既知のフォルダー名です。|

要求本文に任意の書き込み可能な [message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) プロパティを指定します。

**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/MailFolders/inbox/messages
Content-Type: application/json

{
  "Subject": "Did you see last night's game?",
  "Importance": "Low",
  "Body": {
    "ContentType": "HTML",
    "Content": "They were <b>awesome</b>!"
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
      }
    }
  ]
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz7k0AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag5\"",
  "Id": "AAMkAGE0Mz7k0AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag5",
  "Categories": [],
  "CreatedDateTime": "2014-10-18T20:06:51Z",
  "LastModifiedDateTime": "2014-10-18T20:06:51Z",
  "Subject": "Did you see last night's game?",
  "BodyPreview": "They were awesome!",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nThey were <b>awesome</b>!\r\n</body>\r\n</html>\r\n"
  },
  "Importance": "Low",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEPAAA=",
  "From": null,
  "Sender": null,
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0Mpv2hisc=",
  "ReceivedDateTime": "2014-10-18T20:06:51Z",
  "SentDateTime": "2014-10-18T20:06:51Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": true,
  "IsRead": true
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages
POST https://outlook.office.com/api/v1.0/me/folders/{folder_id}/messages
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox`Inbox` または Drafts`Drafts` の既知のフォルダー名です。|

要求本文に任意の書き込み可能な [message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) プロパティを指定します。

[!code-REST-i[mail_api_create_message_in_folder](./_data/mail_api_create_message_in_folder.json)]



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



 **応答の種類**

下書き[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

****

<a name="SendDraftMessages"> </a>
###下書きメッセージを送信する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.send_
- _wl.imap_

**Send** メソッドを使用して、[新しいメッセージの下書き](#CreateNewDraft)、[返信の下書き](#CreateReplyDraft)、[全員に返信の下書き](#CreateReplyAllDraft)、または[転送の下書き](#CreateForwardDraft)を送信します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/send
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|送信する下書きメッセージの ID です。|


**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/messages/AAMkAGE0Mz7k0AAA=/send
```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/send
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|送信する下書きメッセージの ID です。|


**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz7k0AAA=/send
```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/send
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|送信する下書きメッセージの ID です。|

[!code-REST-i[mail_api_send_message_by_id](./_data/mail_api_send_message_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



****

<a name="SendMessageOnTheFlyClient"> </a>
###オンザフライで新しいメッセージを送信する (クライアント)

メッセージを作成し、それを **SendMailAsync** メソッドに渡します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、既に [Outlook サービス クライアントが取得されている](..\api\use-outlook-rest-api.md#GetClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
ItemBody body = new ItemBody
{
    Content = "It was <b>awesome</b>!",
    ContentType = BodyType.HTML
};
List<Recipient> toRecipients = new List<Recipient>();
toRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "katiej@a830edad9050849NDA1.onmicrosoft.com"
    }
});
toRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "pavelb@a830edad9050849NDA1.onmicrosoft.com"
    }
});
Message newMessage = new Message
{
    Subject = "Did you see last night's game?",
    Body = body,
    ToRecipients = toRecipients
};

// To send a message without saving to Sent Items, specify false for  
// the SavetoSentItems parameter.
await outlookClient.Me.SendMailAsync(newMessage, true);
```

<!-- ENDSECTION -->

****

<a name="CreateDraftClient"> </a>
###下書きメッセージを作成する (クライアント)

下書きメッセージを作成し、それを **AddMessageAsync メソッドに渡します。その後、下書きを更新して送信**できます。 下書きの転送メッセージを作成するには、CreateForwardAsync を呼び出します。その後、[下書きを更新](#UpdateMessagesClient)して[送信](#SendDraftClient)できます。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、既に [Outlook サービス クライアントが取得されている](..\api\use-outlook-rest-api.md#GetClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
ItemBody body = new ItemBody
{
    Content = "I'm coming out a week later.",
    ContentType = BodyType.HTML
};
List<Recipient> toRecipients = new List<Recipient>();
toRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "katiej@a830edad9050849NDA1.onmicrosoft.com"
    }
});
Message draftMessage = new Message
{
    Subject = "Changed my travel plans",
    Body = body,
    ToRecipients = toRecipients,
    Importance = Importance.High
};

// Save the draft message. Saving to Me.Messages saves the message in the Drafts folder.
await outlookClient.Me.Messages.AddMessageAsync(draftMessage);

// Get the ID of the message.
string messageId = draftMessage.Id;
```

<!-- ENDSECTION -->

**Me.Messages** コレクションにメッセージを追加すると [下書き] フォルダーに下書きが保存されますが、下書きは任意のフォルダーの **Messages** コレクションに保存できます。

例`outlookClient.Me.Folders["AAMkADE3N..."].Messages.AddMessageAsync(newMessage)`

****

<a name="SendDraftClient"> </a>
###下書きメッセージを送信する (クライアント)

Send a draft message by calling **SendAsync**. 下書きメッセージを送信するには、SendAsync を呼び出します。新規、返信、全員に返信、または転送メッセージの下書きを送信できます。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[メッセージ ID](#GetMessagesClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
await outlookClient.Me.Messages[messageId].SendAsync();
```

<!-- ENDSECTION -->

****

<a name="ReplyToMessages"> </a>
##メッセージを返信または全員に返信する

**Note** The behavior of the operations in this section vary by version. このセクションの操作の動作は、バージョンによって異なります。ページの右上隅のバージョンを選択して詳細を確認します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

1 回の呼び出しでメッセージに返信、コメントの追加、またはメッセージ プロパティを更新するか、1 回の呼び出しで最初に返信の下書きを作成してメッセージプロパティを更新してから、下書きを送信することができます。

メッセージの送信者にのみ返信することも、一度にすべての受信者に返信することもできます。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

オンザフライでコメントを返信できます。または、最初に返信の下書きを作成してから、下書きを更新して送信できます。メッセージの送信者にのみ返信することも、一度にすべての受信者に返信することもできます。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

オンザフライでコメントを返信できます。または、最初に返信の下書きを作成してから、下書きを更新して送信できます。メッセージの送信者にのみ返信することも、一度にすべての受信者に返信することもできます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


REST API:オンザフライで送信者に返信する (REST)  オンザフライで全員に返信する (REST)  下書きの返信メッセージを作成する (REST)  下書きの全員に返信メッセージを作成する (REST)

クライアント ライブラリ:オンザフライで返信または全員に返信する (クライアント)  下書きの返信または全員に返信メッセージを作成する (クライアント)

<a name="ReplyToSender"> </a>
###オンザフライで送信者に返信する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.send_
- _wl.imap_

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

1 回の**返信**呼び出しでメッセージ送信者に返信、コメントを追加、または[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。

または、最初に[下書きの返信メッセージを作成して](#CreateReplyDraft)コメントを含めるか、メッセージのプロパティを更新し、返信を[送信](#SendDraftMessages)します。


```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/reply
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|返信メッセージで更新する書き込み可能なプロパティです。 |

**注意**

- コメントまたはMessage`Message` パラメーターの **Body** プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。 コメントまたはMessage パラメーターの Body プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。
- **ReplyTo** が元のメッセージに指定されている場合、インターネット メッセージ形式 ([RFC 2822](http://www.rfc-editor.org/info/rfc2822)) ごとに、**From** の受信者ではなく、**ReplyTo** の受信者に返信を送信する必要があります。 


**要求のサンプル**

次の例では、コメントを含め、受信者を返信メッセージに追加します。

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAAqldOAAA=/reply
Content-Type: application/json

{
  "Message":{  
    "ToRecipients":[
      {
        "EmailAddress": {
          "Address":"fannyd@contoso.onmicrosoft.com",
          "Name":"Fanny Downs"
        }
      },
      {
        "EmailAddress":{
          "Address":"randiw@contoso.onmicrosoft.com",
          "Name":"Randi Welch"
        }
      }
     ]
  },
  "Comment": "Fanny, Randi, would you name the group please?" 
}
```

**応答のサンプル**

```
Status code: 202
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

メッセージの送信者に返信する場合は、**Reply** メソッドを使用します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。

または、返信のために[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更する必要がある場合、最初に[下書き返信メッセージを作成](#CreateReplyDraft)し、メッセージのプロパティを[更新](#UpdateMessages)してから、返信を[送信](#SendDraftMessages)します。

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/reply
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|

**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8DmAAA=/reply
Content-Type: application/json

{
  "Comment": "Sounds great! See you tomorrow."
}
```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

メッセージの送信者に返信する場合は、**Reply** メソッドを使用します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。

または、返信のために[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更する必要がある場合、最初に[下書き返信メッセージを作成](#CreateReplyDraft)し、メッセージのプロパティを[更新](#UpdateMessages)してから、返信を[送信](#SendDraftMessages)します。

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/reply
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|

[!code-REST-i[mail_api_send_reply_by_id](./_data/mail_api_send_reply_by_id.json)]



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


****


<a name="ReplyAll"> </a>
###オンザフライで全員に返信する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.send_
- _wl.imap_


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

コメントを指定して、[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更することによってメッセージ受信者全員に返信し、全員に **ReplyAll** メソッドを使用します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。

または、最初に[下書きの全員に返信メッセージを作成して](#CreateReplyAllDraft)コメントを含めるか、メッセージのプロパティを更新し、返信を[送信](#SendDraftMessages)します。


```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/replyall
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|全員に返信メッセージで更新する書き込み可能なプロパティです。 |

**注意**

- コメントまたはMessage`Message` パラメーターの **Body** プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。 コメントまたはMessage パラメーターの Body プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。
- **ReplyTo** が元のメッセージに指定されている場合、インターネット メッセージ形式 ([RFC 2822](http://www.rfc-editor.org/info/rfc2822)) ごとに、**From** と**ToRecipients** の受信者ではなく、**ReplyTo** と**ToRecipients** の受信者に返信を送信する必要があります。 


**要求のサンプル**

次の例では、コメントを含め、添付ファイルを全員に返信メッセージに追加します。

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAH5JaKAAA=/ReplyAll
Content-Type: application/json

{
    "Message":{
      "Attachments": [ 
        { 
          "@odata.type": "#Microsoft.OutlookServices.FileAttachment", 
          "Name": "guidelines.txt", 
          "ContentBytes": "bWFjIGFuZCBjaGVlc2UgdG9kYXk=" 
        } 
      ]
    },
    "Comment": "Please take a look at the attached guidelines before you decide on the name." 
}
```

**応答のサンプル**

```
Status code: 202
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

メッセージの送信者全員に返信する場合は、コメントを指定して、**ReplyAll** メソッドを使用します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。

または、返信のために[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更する必要がある場合、最初に[下書きの全員に返信メッセージを作成](#CreateReplyAllDraft)し、メッセージのプロパティを[更新](#UpdateMessages)してから、返信を[送信](#SendDraftMessages)します。

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/replyall
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|

**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0MSz8DmAAA=/replyall
Content-Type: application/json

{
  "Comment": "Thanks for the heads up."
}
```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

メッセージの送信者全員に返信する場合は、コメントを指定して、**ReplyAll** メソッドを使用します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。

または、返信のために[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更する必要がある場合、最初に[下書きの全員に返信メッセージを作成](#CreateReplyAllDraft)し、メッセージのプロパティを[更新](#UpdateMessages)してから、返信を[送信](#SendDraftMessages)します。


```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/replyall
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|

[!code-REST-i[mail_api_send_reply_all_by_id](./_data/mail_api_send_reply_all_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




****

<a name="CreateReplyDraft"> </a>
###下書きの返信メッセージを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

1 回の **CreateReply** 呼び出しで下書きの返信メッセージを作成して、コメントを含めるか[メッセージのプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を更新します。その後、下書きメッセージを[送信](#SendDraftMessages)できます。


```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/createreply
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|返信メッセージで更新する書き込み可能なプロパティです。 |

**注意**

- コメントまたはMessage`Message` パラメーターの **Body** プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。 コメントまたはMessage パラメーターの Body プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。
- **ReplyTo** が元のメッセージに指定されている場合、インターネット メッセージ形式 ([RFC 2822](http://www.rfc-editor.org/info/rfc2822)) ごとに、**From** の受信者ではなく、**ReplyTo** の受信者に返信する必要があります。 


**要求のサンプル**

次の例では、下書きの返信を作成し、要求の本文にコメントと受信者を追加します。

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAAqldOAAA=/createreply
Content-Type: application/json

{
  "Message":{  
    "ToRecipients":[
      {
        "EmailAddress": {
          "Address":"fannyd@contoso.onmicrosoft.com",
          "Name":"Fanny Downs"
        }
      },
      {
        "EmailAddress":{
          "Address":"randiw@contoso.onmicrosoft.com",
          "Name":"Randi Welch"
        }
      }
     ]
  },
  "Comment": "Fanny, Randi, would you name the group if the project is approved, please?" 
}

```

**応答のサンプル**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Messages('AAMkADA1MTAAAH5JKoAAA=')",
  "@odata.etag": "W/\"CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DO\"",
  "Id": "AAMkADA1MTAAAH5JKoAAA=",
  "CreatedDateTime": "2016-03-15T08:33:43Z",
  "LastModifiedDateTime": "2016-03-15T08:33:43Z",
  "ChangeKey": "CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DO",
  "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
  "Categories": [ ],
  "ReceivedDateTime": "2016-03-15T08:33:43Z",
  "SentDateTime": "2016-03-15T08:33:43Z",
  "HasAttachments": false,
  "InternetMessageId": "<DM2PR00MB00571796B16132601E1F286CF7890@DM2PR00MB0057.namprd00.prod.outlook.com>",
  "Subject": "RE: Let's start a group",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<body>Fanny, Randi, would you name the group if the project is approved, please?\r\n<b>From:</b> Fanny Downs<br>\r\n<b>Sent:</b> Friday, March 4, 2016 12:23:35 AM<br>\r\n<b>To:</b> Admin<br>\r\n<b>Subject:</b> Re: Let's start a group</font>\r\n<p>That's a great idea!<br>\r\n</body>\r\n</html>"
  },
  "BodyPreview": "Fanny, Randi, would you name the group if the project is approved, please?\r\n________________________________\r\nFrom: Fanny Downs\r\nSent: Friday, March 4, 2016 12:23:35 AM\r\nTo: Admin\r\nSubject: Re: Let's start a group\r\n\r\n\r\nThat's a gre",
  "Importance": "Normal",
  "ParentFolderId": "AQMkADA1MTAAAAIBDwAAAA==",
  "Sender": {
    "EmailAddress": {
      "Name": "Admin",
      "Address": "admin@contoso.onmicrosoft.com"
    }
  },
  "From": null,
  "ToRecipients": [
    {
      "EmailAddress": {
        "Name": "Fanny Downs",
        "Address": "fannyd@contoso.onmicrosoft.com"
      }
    },
    {
      "EmailAddress": {
        "Name": "Randi Welch",
        "Address": "randiw@contoso.onmicrosoft.com"
      }
    }
  ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkADA1MTVGjIwpLvWmGtIo-aFE=",
  "ConversationIndex": "AQHRdar6akFxUaMjCku9aYa0ij9oUZ9IbLr7gBHStBQ=",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADA1MTAAAH5JKoAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "MentionedMe": null,
  "AppliedHashtagsPreview": null,
  "LikesPreview": null,
  "MentionsPreview": null,
  "Mentioned": [ ],
  "InferenceClassification": "Focused",
  "UnsubscribeData": [ ],
  "UnsubscribeEnabled": false,
  "Flag": { "FlagStatus": "NotFlagged" }
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

1 回の **CreateReply 呼び出しで返信メッセージの下書きを作成し、コメントを追加します。次に、**メッセージのプロパティ**を更新し、下書きを送信**します。 You can then [update](#UpdateMessages) [message properties](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties) and [send](#SendDraftMessages) the draft.

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/createreply
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|

**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAAqldOAAA=/createreply
Content-Type: application/json

{
  "Comment": "Fanny, Randi, would you name the group if the project is approved, please?" 
}

```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Messages('AAMkADA1MTAAAH5JKoAAA=')",
  "@odata.etag": "W/\"CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DO\"",
  "Id": "AAMkADA1MTAAAH5JKoAAA=",
  "CreatedDateTime": "2016-03-15T08:33:43Z",
  "LastModifiedDateTime": "2016-03-15T08:33:43Z",
  "ChangeKey": "CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DO",
  "Categories": [ ],
  "ReceivedDateTime": "2016-03-15T08:33:43Z",
  "SentDateTime": "2016-03-15T08:33:43Z",
  "HasAttachments": false,
  "InternetMessageId": "<DM2PR00MB00571796B16132601E1F286CF7890@DM2PR00MB0057.namprd00.prod.outlook.com>",
  "Subject": "RE: Let's start a group",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<body>Fanny, Randi, would you name the group if the project is approved, please?\r\n<b>From:</b> Fanny Downs<br>\r\n<b>Sent:</b> Friday, March 4, 2016 12:23:35 AM<br>\r\n<b>To:</b> Admin<br>\r\n<b>Subject:</b> Re: Let's start a group</font>\r\n<p>That's a great idea!<br>\r\n</body>\r\n</html>"
  },
  "BodyPreview": "Fanny, Randi, would you name the group if the project is approved, please?\r\n________________________________\r\nFrom: Fanny Downs\r\nSent: Friday, March 4, 2016 12:23:35 AM\r\nTo: Admin\r\nSubject: Re: Let's start a group\r\n\r\n\r\nThat's a gre",
  "Importance": "Normal",
  "ParentFolderId": "AQMkADA1MTAAAAIBDwAAAA==",
  "Sender": {
    "EmailAddress": {
      "Name": "Admin",
      "Address": "admin@contoso.onmicrosoft.com"
    }
  },
  "From": null,
  "ToRecipients": [ ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkADA1MTVGjIwpLvWmGtIo-aFE=",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADA1MTAAAH5JKoAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "MentionedMe": null,
  "AppliedHashtagsPreview": null,
  "LikesPreview": null,
  "MentionsPreview": null,
  "Mentioned": [ ],
  "InferenceClassification": "Focused",
  "UnsubscribeData": [ ],
  "UnsubscribeEnabled": false,
  "Flag": { "FlagStatus": "NotFlagged" }
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

1 回の **CreateReply 呼び出しで返信メッセージの下書きを作成し、コメントを追加します。次に、**メッセージのプロパティ**を更新し、下書きを送信**します。 You can then [update](#UpdateMessages) [message properties](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties) and [send](#SendDraftMessages) the draft.

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/createreply
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|

**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAAqldOAAA=/createreply
Content-Type: application/json

{
  "Comment": "Fanny, Randi, would you name the group if the project is approved, please?" 
}

```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Messages('AAMkADA1MTAAAH5JKoAAA=')",
  "@odata.etag": "W/\"CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DO\"",
  "Id": "AAMkADA1MTAAAH5JKoAAA=",
  "CreatedDateTime": "2016-03-15T08:33:43Z",
  "LastModifiedDateTime": "2016-03-15T08:33:43Z",
  "ChangeKey": "CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DO",
  "Categories": [ ],
  "ReceivedDateTime": "2016-03-15T08:33:43Z",
  "SentDateTime": "2016-03-15T08:33:43Z",
  "HasAttachments": false,
  "InternetMessageId": "<DM2PR00MB00571796B16132601E1F286CF7890@DM2PR00MB0057.namprd00.prod.outlook.com>",
  "Subject": "RE: Let's start a group",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<body>Fanny, Randi, would you name the group if the project is approved, please?\r\n<b>From:</b> Fanny Downs<br>\r\n<b>Sent:</b> Friday, March 4, 2016 12:23:35 AM<br>\r\n<b>To:</b> Admin<br>\r\n<b>Subject:</b> Re: Let's start a group</font>\r\n<p>That's a great idea!<br>\r\n</body>\r\n</html>"
  },
  "BodyPreview": "Fanny, Randi, would you name the group if the project is approved, please?\r\n________________________________\r\nFrom: Fanny Downs\r\nSent: Friday, March 4, 2016 12:23:35 AM\r\nTo: Admin\r\nSubject: Re: Let's start a group\r\n\r\n\r\nThat's a gre",
  "Importance": "Normal",
  "ParentFolderId": "AQMkADA1MTAAAAIBDwAAAA==",
  "Sender": {
    "EmailAddress": {
      "Name": "Admin",
      "Address": "admin@contoso.onmicrosoft.com"
    }
  },
  "From": null,
  "ToRecipients": [ ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkADA1MTVGjIwpLvWmGtIo-aFE=",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADA1MTAAAH5JKoAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "MentionedMe": null,
  "AppliedHashtagsPreview": null,
  "LikesPreview": null,
  "MentionsPreview": null,
  "Mentioned": [ ],
  "InferenceClassification": "Focused",
  "UnsubscribeData": [ ],
  "UnsubscribeEnabled": false,
  "Flag": { "FlagStatus": "NotFlagged" }
}
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




 **応答の種類**

**ToRecipient**、**IsDraft**、およびその他の適切なプロパティが事前に設定された下書きの返信[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

****


<a name="CreateReplyAllDraft"> </a>
###下書きの全員に返信メッセージを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

1 回の **CreateReplyAll** 呼び出しで下書きの全員に返信メッセージを作成して、コメントを含めるか[メッセージのプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を更新します。その後、下書きメッセージを[送信](#SendDraftMessages)できます。


```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/createreplyall
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|全員に返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|全員に返信メッセージで更新する書き込み可能なプロパティです。 |

**注意**

- コメントまたはMessage`Message` パラメーターの **Body** プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。 コメントまたはMessage パラメーターの Body プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。
- **ReplyTo** が元のメッセージに指定されている場合、インターネット メッセージ形式 ([RFC 2822](http://www.rfc-editor.org/info/rfc2822)) ごとに、**From** と**ToRecipients** プロパティの受信者ではなく、**ReplyTo** と**ToRecipients** プロパティの受信者に返信する必要があります。 


**要求のサンプル:**

次の例では、1 回の **CreateReplyAll** 呼び出しで全員に返信する下書きを作成し、添付ファイルをコメントに追加します。

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAH5JaKAAA=/createreplyall
Content-Type: application/json

{
    "Message":{
      "Attachments": [ 
        { 
          "@odata.type": "#Microsoft.OutlookServices.FileAttachment", 
          "Name": "guidelines.txt", 
          "ContentBytes": "bWFjIGFuZCBjaGVlc2UgdG9kYXk=" 
        } 
      ]
    },
    "Comment": "if the project gets approved, please take a look at the attached guidelines before you decide on the name." 
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Messages('AAMkADA1MTAAAH5JKpAAA=')",
  "@odata.etag": "W/\"CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DP\"",
  "Id": "AAMkADA1MTAAAH5JKpAAA=",
  "CreatedDateTime": "2016-03-15T08:37:34Z",
  "LastModifiedDateTime": "2016-03-15T08:37:34Z",
  "ChangeKey": "CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DP",
  "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
  "Categories": [ ],
  "ReceivedDateTime": "2016-03-15T08:37:34Z",
  "SentDateTime": "2016-03-15T08:37:34Z",
  "HasAttachments": true,
  "InternetMessageId": "<DM2PR00MB005732BE05BD669AC7CE056EF7890@DM2PR00MB0057.namprd00.prod.outlook.com>",
  "Subject": "RE: Let's start a group",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<body dir=\"ltr\">\r\nif the project gets approved, please take a look at the attached guidelines before you decide on the name.\r\n<b>From:</b> Admin<br>\r\n<b>Sent:</b> Tuesday, March 15, 2016 6:36:32 AM<br>\r\n<b>To:</b> Fanny Downs; Randi Welch<br>\r\n<b>Subject:</b> RE: Let's start a group\r\n<div>Fanny, Randi, would you name the group please?\r\n<b>From:</b> Fanny Downs<br>\r\n<b>Sent:</b> Friday, March 4, 2016 12:23:35 AM<br>\r\n<b>To:</b> Admin<br>\r\n<b>Subject:</b> Re: Let's start a group</font>\r\n</body>\r\n</html>"
  },
  "BodyPreview": "if the project gets approved, please take a look at the attached guidelines before you decide on the name.\r\n________________________________\r\nFrom: Admin\r\nSent: Tuesday, March 15, 2016 6:36:32 AM\r\nTo: Fanny Downs; Randi Welch\r\nSubj",
  "Importance": "Normal",
  "ParentFolderId": "AQMkADA1MTAAAAIBDwAAAA==",
  "Sender": {
    "EmailAddress": {
      "Name": "Admin",
      "Address": "admin@contoso.onmicrosoft.com"
    }
  },
  "From": null,
  "ToRecipients": [
    {
      "EmailAddress": {
        "Name": "Fanny Downs",
        "Address": "fannyd@contoso.onmicrosoft.com"
      }
    },
    {
      "EmailAddress": {
        "Name": "Randi Welch",
        "Address": "randiw@contoso.onmicrosoft.com"
      }
    }
  ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkADA1MTLvWmGtIo-aFE=",
  "ConversationIndex": "AQHRdar6akFxUaMjCku9aYa0ij9oUZ9IbLr7gBGx9qGAACHRQA==",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADA1MTAAAH5JKpAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "MentionedMe": null,
  "AppliedHashtagsPreview": null,
  "LikesPreview": null,
  "MentionsPreview": null,
  "Mentioned": [ ],
  "InferenceClassification": "Focused",
  "UnsubscribeData": [ ],
  "UnsubscribeEnabled": false,
  "Flag": { "FlagStatus": "NotFlagged" }
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

1 回の **CreateReplyAll 呼び出しで全員に返信メッセージの下書きを作成し、コメントを追加します。次に、**メッセージのプロパティ**を更新し、下書きを送信**します。 You can then [update](#UpdateMessages) [message properties](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties) and [send](#SendDraftMessages) the draft.

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/createreplyall
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|全員に返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8DmAAA=/createreplyall
Content-Type: application/json

{
    "Comment": "If the project gets approved, please decide on the name." 
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz7k5AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhF\"",
  "Id": "AAMkAGE0Mz7k5AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhF",
  "Categories": [],
  "CreatedDateTime": "2014-10-18T21:21:06Z",
  "LastModifiedDateTime": "2014-10-18T21:21:06Z",
  "Subject": "RE: Check out the new Office 365 APIs",
  "BodyPreview": "If the project gets approved, please decide on the name.\r\n_________________________________\r\nFrom: Alex D\r\nSent: Saturday, October 18, 2014 9:18:18 PM\r\nTo: Katie Jordan; Garth Fort\r\nSubj",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n...</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEPAAA=",
  "From": null,
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    },
    {
      "EmailAddress": {
        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Garth Fort"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0M3HbTkEU=",
  "ReceivedDateTime": "2014-10-18T21:21:06Z",
  "SentDateTime": "2014-10-18T21:21:06Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": true,
  "IsRead": true
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/createreplyall
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|全員に返信するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。 含めるコメントです。空の文字列にすることができます。|

**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8DmAAA=/createreplyall
Content-Type: application/json

{
    "Comment": "If the project gets approved, please decide on the name." 
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz7k5AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhF\"",
  "Id": "AAMkAGE0Mz7k5AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhF",
  "Categories": [],
  "CreatedDateTime": "2014-10-18T21:21:06Z",
  "LastModifiedDateTime": "2014-10-18T21:21:06Z",
  "Subject": "RE: Check out the new Office 365 APIs",
  "BodyPreview": "If the project gets approved, please decide on the name.\r\n_________________________________\r\nFrom: Alex D\r\nSent: Saturday, October 18, 2014 9:18:18 PM\r\nTo: Katie Jordan; Garth Fort\r\nSubj",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n...</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEPAAA=",
  "From": null,
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    },
    {
      "EmailAddress": {
        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Garth Fort"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0M3HbTkEU=",
  "ReceivedDateTime": "2014-10-18T21:21:06Z",
  "SentDateTime": "2014-10-18T21:21:06Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": true,
  "IsRead": true
}
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



 **応答の種類**

**ToRecipient**、**IsDraft**、およびその他の適切なプロパティが事前に設定された下書きの全員に返信[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

****

<a name="ReplyOnTheFlyClient"></a>
###オンザフライで返信または全員に返信する (クライアント)

直接メッセージの送信者またはすべての受信者に返信するには、**ReplyAsync** または **ReplyAllAsync** を呼び出してコメントを渡します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[メッセージ ID](#GetMessagesClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
await outlookClient.Me.Messages[messageId].ReplyAsync("Count me in.");
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
await outlookClient.Me.Messages[messageId].ReplyAllAsync("Count me in.");
```

<!-- ENDSECTION -->

<a name="CreateDraftReplyClient"> </a>
###下書きの返信または全員に返信メッセージを作成する (クライアント)

下書きの返信または全員に返信メッセージを作成するには、**CreateReplyAsync** または **CreateReplyAllAsync を呼び出します。その後、下書きを更新**して送信できます。 下書きの転送メッセージを作成するには、CreateForwardAsync を呼び出します。その後、[下書きを更新](#UpdateMessagesClient)して[送信](#SendDraftClient)できます。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[メッセージ ID](#GetMessagesClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage replyDraft = await outlookClient.Me.Messages[messageId].CreateReplyAsync();

// Get the ID of the draft Reply message.
string replyMessageId = replyDraft.Id;
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IMessage replyAllDraft = await outlookClient.Me.Messages[messageId].CreateReplyAllAsync();

// Get the ID of the draft Reply All message.
string replyAllMessageId = replyAllDraft.Id;
```

<!-- ENDSECTION -->

**CreateReplyAsync** および **CreateReplyAllAsync** は、**ToRecipients**、**IsDraft**、およびその他の適切なプロパティが事前に設定されている下書きメッセージを作成します。


****


<a name="ForwardMessages"> </a>
##新規または下書きメッセージを転送する

**Note** The behavior of the operations in this section vary by version. このセクションの操作の動作は、バージョンによって異なります。ページの右上隅のバージョンを選択して詳細を確認します。
 
<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

1 回の呼び出しでメッセージを転送、コメントの追加、またはメッセージのプロパティを更新するか、1 回の呼び出しで最初に下書きを作成してメッセージのプロパティを更新してから、下書きを送信することができます。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

メッセージを直接転送するか、下書きの転送メッセージを作成し、それを更新してから送信することができます。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

メッセージを直接転送するか、下書きの転送メッセージを作成し、それを更新してから送信することができます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



REST API:メッセージを直接転送する (REST)  転送メッセージを作成する (REST)

クライアント ライブラリ:メッセージを直接転送する (クライアント)  下書きの転送メッセージを作成する (クライアント)

<a name="ForwardDirectly"></a>
###メッセージを直接転送する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.send_
- _wl.imap_

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

1 回の**転送**呼び出しでメッセージを転送、コメントを追加、または[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。

または、最初に[下書きの転送メッセージを作成して](#CreateForwardDraft)コメントを含めるか、メッセージのプロパティを更新し、転送メッセージを[送信](#SendDraftMessages)します。


```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/forward
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|転送するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|ToRecipients|Collection([Recipient](..\api\complex-types-for-mail-contacts-calendar.md#ComplexTypes))|受信者の一覧です。|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|返信メッセージで更新する書き込み可能なプロパティです。 |

**注意**

- コメントまたはMessage`Message` パラメーターの **Body** プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。 コメントまたはMessage パラメーターの Body プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。
- ToRecipients`ToRecipients` パラメーター、または Message`Message` パラメーターの **ToRecipients** プロパティを指定する必要があります。両方を指定するか、どちらも指定しないと、「HTTP 400 要求が正しくありません」というエラーが返されます。 ToRecipients パラメーター、または Message パラメーターの ToRecipients プロパティを指定する必要があります。両方を指定するか、どちらも指定しないと、「HTTP 400 要求が正しくありません」というエラーが返されます。


**要求のサンプル:**

次の例では、**IsDeliveryReceiptRequested** プロパティを true に設定し、コメントを追加してメッセージを転送します。

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAH5JaLAAA=/forward
Content-Type: application/json

{
  "Message":{  
    "IsDeliveryReceiptRequested": true,
    "ToRecipients":[
      {
        "EmailAddress": {
          "Address":"danas@contoso.onmicrosoft.com",
          "Name":"Dana Swope"
        }
      }
     ]
  },
  "Comment": "Dana, just want to make sure you get this." 
}
```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**Forward** メソッドを使用してメッセージを転送し、オプションでコメントを指定します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。 メッセージは直ちに送信され、コピーは [送信済みアイテム] フォルダーに保存されます。

または、メッセージを転送するために[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更する必要がある場合、最初に[下書きの転送メッセージを作成](#CreateForwardDraft)し、メッセージのプロパティを[更新](#UpdateMessages)してから、返信を[送信](#SendDraftMessages)します。


```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/forward
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|転送するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|ToRecipients|Collection([Recipient](..\api\complex-types-for-mail-contacts-calendar.md#ComplexTypes))|受信者の一覧です。|

要求本文に **Comment** と **ToRecipients** パラメーターを指定します。


**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8DmAAA=/forward
Content-Type: application/json

{
  "Comment": "FYI",
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
      }
    },
    {
      "EmailAddress": {
        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com"
      }
    }
  ]
}
```

**応答のサンプル:**

```
Status code: 202
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

**Forward** メソッドを使用してメッセージを転送し、オプションでコメントを指定します。その後、メッセージは [送信済みアイテム] フォルダーに保存されます。 メッセージは直ちに送信され、コピーは [送信済みアイテム] フォルダーに保存されます。

または、メッセージを転送するために[更新可能なプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を変更する必要がある場合、最初に[下書きの転送メッセージを作成](#CreateForwardDraft)し、メッセージのプロパティを[更新](#UpdateMessages)してから、返信を[送信](#SendDraftMessages)します。


```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/forward
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|転送するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|ToRecipients|Collection([Recipient](..\api\complex-types-for-mail-contacts-calendar.md#ComplexTypes))|受信者の一覧です。|

要求本文に **Comment** と **ToRecipients** パラメーターを指定します。

[!code-REST-i[mail_api_send_forward_message_by_id](./_data/mail_api_send_forward_message_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->





****

<a name="CreateForwardDraft"> </a>
###下書きの転送メッセージを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

1 回の **CreateForward** 呼び出しで下書きの転送メッセージを作成して、コメントを含めるか[メッセージのプロパティ](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties)を更新します。その後、下書きメッセージを[送信](#SendDraftMessages)できます。


```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/createforward
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|転送するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|ToRecipients|Collection([Recipient](..\api\complex-types-for-mail-contacts-calendar.md#ComplexTypes))|受信者の一覧です。|
|Message|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)|返信メッセージで更新する書き込み可能なプロパティです。 |

**注意**

- コメントまたはMessage`Message` パラメーターの **Body** プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。 コメントまたはMessage パラメーターの Body プロパティを指定できます。両方を指定すると、「HTTP 400 要求が正しくありません」というエラーが返されます。
- ToRecipients`ToRecipients` パラメーター、または Message`Message` パラメーターの **ToRecipients** プロパティを指定する必要があります。両方を指定するか、どちらも指定しないと、「HTTP 400 要求が正しくありません」というエラーが返されます。 ToRecipients パラメーター、または Message パラメーターの ToRecipients プロパティを指定する必要があります。両方を指定するか、どちらも指定しないと、「HTTP 400 要求が正しくありません」というエラーが返されます。


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/messages/AAMkADA1MTAAAH5JaLAAA=/createforward
Content-Type: application/json

{
  "Message":{  
    "IsDeliveryReceiptRequested": true,
    "ToRecipients":[
      {
        "EmailAddress": {
          "Address":"danas@contoso.onmicrosoft.com",
          "Name":"Dana Swope"
        }
      }
     ]
  },
  "Comment": "Dana, just want to make sure you get this; you'll need this if the project gets approved." 
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Messages('AAMkADA1MTAAAH5JKqAAA=')",
  "@odata.etag": "W/\"CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DQ\"",
  "Id": "AAMkADA1MTAAAH5JKqAAA=",
  "CreatedDateTime": "2016-03-15T08:42:10Z",
  "LastModifiedDateTime": "2016-03-15T08:42:10Z",
  "ChangeKey": "CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAH5/DQ",
  "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
  "Categories": [ ],
  "ReceivedDateTime": "2016-03-15T08:42:10Z",
  "SentDateTime": "2016-03-15T08:42:10Z",
  "HasAttachments": true,
  "InternetMessageId": "<DM2PR00MB0057E0EBC90EF37FC9233941F7890@DM2PR00MB0057.namprd00.prod.outlook.com>",
  "Subject": "FW: Let's start a group",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<body>\r\nDana, just want to make sure you get this; you'll need this if the project gets approved.\r\n<b>From:</b> Admin<br>\r\n<b>Sent:</b> Tuesday, March 15, 2016 6:47:54 AM<br>\r\n<b>To:</b> Fanny Downs; Randi Welch<br>\r\n<b>Subject:</b> RE: Let's start a group</body>\r\n</html>\r\n"
  },
  "BodyPreview": "Dana, just want to make sure you get this; you'll need this if the project gets approved.\r\n________________________________\r\nFrom: Admin\r\nSent: Tuesday, March 15, 2016 6:47:54 AM\r\nTo: Fanny Downs; Randi Welch\r\nSubject: RE: Let's st",
  "Importance": "Normal",
  "ParentFolderId": "AQMkADA1MTAAAAIBDwAAAA==",
  "Sender": {
    "EmailAddress": {
      "Name": "Admin",
      "Address": "admin@contoso.onmicrosoft.com"
    }
  },
  "From": null,
  "ToRecipients": [
    {
      "EmailAddress": {
        "Name": "Dana Swope",
        "Address": "danas@contoso.onmicrosoft.com"
      }
    }
  ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkADA1MTLvWmGtIo-aFE=",
  "ConversationIndex": "AQHRdar6akFxUaMjCku9aYa0ij9oUZ9IbLr7gBGx9qGAAAMtlIAAH+3c",
  "IsDeliveryReceiptRequested": true,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADA1MTAAAH5JKqAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "MentionedMe": null,
  "AppliedHashtagsPreview": null,
  "LikesPreview": null,
  "MentionsPreview": null,
  "Mentioned": [ ],
  "InferenceClassification": "Focused",
  "UnsubscribeData": [ ],
  "UnsubscribeEnabled": false,
  "Flag": { "FlagStatus": "NotFlagged" }
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

1 回の **CreateForward 呼び出しで下書きの転送メッセージを作成して、コメントまたは受信者を追加します。次に、**メッセージのプロパティ**を更新し、下書きを送信**します。 You can then [update](#UpdateMessages) [message properties](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties) and [send](#SendDraftMessages) the draft.


```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/createforward
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|転送するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|ToRecipients|Collection([Recipient](..\api\complex-types-for-mail-contacts-calendar.md#ComplexTypes))|受信者の一覧です。|

**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8DmAAA=/createforward
Content-Type: application/json

{
  "ToRecipients":[
    {
      "EmailAddress": {
        "Address":"danas@contoso.onmicrosoft.com",
        "Name":"Dana Swope"
      }
    }
   ],
  "Comment": "Dana, just want to make sure you get this." 
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz7k6AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhG\"",
  "Id": "AAMkAGE0Mz7k6AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhG",
  "Categories": [],
  "CreatedDateTime": "2016-03-15T08:42:10Z",
  "LastModifiedDateTime": "2016-03-15T08:42:10Z",
  "Subject": "FW: Let's start a group",
  "BodyPreview": "Dana, just want to make sure you get this.\r\n________________________________\r\nFrom: Admin\r\nSent: Tuesday, March 15, 2016 6:47:54 AM\r\nTo: Fanny Downs; Randi Welch\r\nSubject: RE: Let's st",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n...</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEPAAA=",
  "From": null,
  "Sender": {
    "EmailAddress": {
      "Address": "'alexd@contoso.onmicrosoft.com'",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [   
    {
      "EmailAddress": {
        "Address":"danas@contoso.onmicrosoft.com",
        "Name":"Dana Swope"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0M3HbTkEU=",
  "ReceivedDateTime": "2016-03-15T08:42:10Z",
  "SentDateTime": "2016-03-15T08:42:10Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": true,
  "IsRead": true
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

1 回の **CreateForward 呼び出しで下書きの転送メッセージを作成して、コメントまたは受信者を追加します。次に、**メッセージのプロパティ**を更新し、下書きを送信**します。 You can then [update](#UpdateMessages) [message properties](..\api\complex-types-for-mail-contacts-calendar.md#MessageProperties) and [send](#SendDraftMessages) the draft.

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/createforward
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|転送するメッセージの ID です。|
|_Body parameters_|
|Comment|string|含めるコメントです。空の文字列にすることができます。|
|ToRecipients|Collection([Recipient](..\api\complex-types-for-mail-contacts-calendar.md#ComplexTypes))|受信者の一覧です。|


**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8DmAAA=/createforward
Content-Type: application/json

{
  "ToRecipients":[
    {
      "EmailAddress": {
        "Address":"danas@contoso.onmicrosoft.com",
        "Name":"Dana Swope"
      }
    }
   ],
  "Comment": "Dana, just want to make sure you get this." 
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz7k6AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhG\"",
  "Id": "AAMkAGE0Mz7k6AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhG",
  "Categories": [],
  "CreatedDateTime": "2016-03-15T08:42:10Z",
  "LastModifiedDateTime": "2016-03-15T08:42:10Z",
  "Subject": "FW: Let's start a group",
  "BodyPreview": "Dana, just want to make sure you get this.\r\n________________________________\r\nFrom: Admin\r\nSent: Tuesday, March 15, 2016 6:47:54 AM\r\nTo: Fanny Downs; Randi Welch\r\nSubject: RE: Let's st",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n...</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEPAAA=",
  "From": null,
  "Sender": {
    "EmailAddress": {
      "Address": "'alexd@contoso.onmicrosoft.com'",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [   
    {
      "EmailAddress": {
        "Address":"danas@contoso.onmicrosoft.com",
        "Name":"Dana Swope"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0M3HbTkEU=",
  "ReceivedDateTime": "2016-03-15T08:42:10Z",
  "SentDateTime": "2016-03-15T08:42:10Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": true,
  "IsRead": true
}
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



 **応答の種類**

**IsDraft** と適切なプロパティが事前に設定されている下書きの転送[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

****


<a name="ForwardDirectlyClient"></a>
###メッセージを直接転送する (クライアント)

メッセージを直接転送するには、**ForwardAsync** を呼び出してコメントと受信者を渡します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[メッセージ ID](#GetMessagesClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
List<Recipient> recipients = new List<Recipient>();
recipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "garthf@a830edad9050849NDA1.onmicrosoft.com"
    }
});
await outlookClient.Me.Messages[messageId].ForwardAsync("Interested?", recipients);
```

<!-- ENDSECTION -->

<a name="CreateDraftForwardClient"> </a>
###下書きの転送メッセージを作成する (クライアント)

下書きの転送メッセージを作成するには、**CreateForwardAsync を呼び出します。その後、下書きを更新して送信**できます。 下書きの転送メッセージを作成するには、CreateForwardAsync を呼び出します。その後、[下書きを更新](#UpdateMessagesClient)して[送信](#SendDraftClient)できます。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[メッセージ ID](#GetMessagesClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage forwardDraft = await outlookClient.Me.Messages[messageId].CreateForwardAsync();

// Get the ID of the draft Forward message.
string forwardMessageId = forwardDraft.Id;
```

<!-- ENDSECTION -->

**CreateForward** は、**IsDraft** およびその他の適切なプロパティが事前に設定された下書きメッセージを作成します。

****

<a name="UpdateMessages"> </a>
##メッセージを更新する

メッセージの書き込み可能なプロパティを変更して、変更を保存します。

REST API:[メッセージを更新する (REST)](#UpdateAMessage)

クライアント ライブラリ:[メッセージを更新する (クライアント)](#UpdateAMessageClient)

<a name="UpdateAMessage"></a>
###メッセージを更新する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_


下書きまたは既存の[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)の書き込み可能なプロパティを変更します。指定したプロパティのみが変更されます。 下書きまたは既存のメッセージの書き込み可能なプロパティを変更します。指定したプロパティのみが変更されます。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
PATCH https://outlook.office.com/api/beta/me/messages/{message_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|更新するメッセージの ID です。|

要求本文に 1 つ以上の書き込み可能な[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)のプロパティを指定します。


**要求のサンプル:**

```
PATCH https://outlook.office.com/api/beta/me/messages/AAMkAGE0Mz8S-AAA=
Content-Type: application/json

{
  "Categories": [
    "Orange category",
    "Green category"
  ],
  "IsRead": true
}
```

**応答のサンプル:**

```
Status code: 200


{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz8S-AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIP\"",
  "Id": "AAMkAGE0Mz8S-AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIP",
  "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
  "Categories": [
    "Orange category",
    "Green category"
  ],
  "CreatedDateTime": "2014-10-17T17:12:15Z",
  "LastModifiedDateTime": "2014-10-19T03:24:35Z",
  "Subject": "Meeting notes from today",
  "BodyPreview": "See attached",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n...</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": true,
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "From": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0Mip-qvhs=",
  "ConversationIndex": "AQHRh5tqrkAcds2kw==",
  "ReceivedDateTime": "2014-10-17T17:12:15Z",
  "SentDateTime": "2014-10-17T17:12:12Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": false,
  "IsRead": true
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/messages/{message_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|更新するメッセージの ID です。|

要求本文に 1 つ以上の書き込み可能な[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)のプロパティを指定します。

**要求のサンプル:**

```
PATCH https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8S-AAA=
Content-Type: application/json

{
  "Categories": [
    "Orange category",
    "Green category"
  ],
  "IsRead": true
}
```

**応答のサンプル:**

```
Status code: 200


{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz8S-AAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIP\"",
  "Id": "AAMkAGE0Mz8S-AAA=",
  "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIP",
  "Categories": [
    "Orange category",
    "Green category"
  ],
  "CreatedDateTime": "2014-10-17T17:12:15Z",
  "LastModifiedDateTime": "2014-10-19T03:24:35Z",
  "Subject": "Meeting notes from today",
  "BodyPreview": "See attached",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n...</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": true,
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "From": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGE0Mip-qvhs=",
  "ReceivedDateTime": "2014-10-17T17:12:15Z",
  "SentDateTime": "2014-10-17T17:12:12Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": false,
  "IsRead": true
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v1.0/me/messages/{message_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|更新するメッセージの ID です。|

要求本文に 1 つ以上の書き込み可能な[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)のプロパティを指定します。

[!code-REST-i[mail_api_update_message_by_id](./_data/mail_api_update_message_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

更新された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

****

<a name="UpdateAMessageClient"> </a>
###メッセージを更新する (クライアント)

メッセージの書き込み可能なプロパティを変更し、**UpdateAsync** を呼び出して変更を保存します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[メッセージ ID](#GetMessagesClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage message = await outlookClient.Me.Messages[messageId].ExecuteAsync();
message.Body = new ItemBody
{
        Content = "I'm coming out a week earlier."
};
message.ToRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "garthf@a830edad9050849NDA1.onmicrosoft.com"
    }
});
await message.UpdateAsync();
```

<!-- ENDSECTION -->

次のパターンを使用して、クライアント側で複数の更新を定義し、要求をまとめて (バッチ処理で) 送信できます。
1. Call `UpdateAsync(true)` for each entity you want to update. 更新するエンティティごとに UpdateAsync(true)`true` を呼び出します。true を指定すると、更新がクライアント上でローカルに登録されますが、サーバーには投稿されません。
2. outlookClient.Context.SaveChangesAsync()`outlookClient.Context.SaveChangesAsync()` を呼び出して、ローカルに登録されているすべての更新を投稿します。



****


<a name="DeleteMessages"> </a>
##メッセージを削除する

メッセージを削除します。

**注**: メッセージを削除するときには注意してください。削除した内容を回復できない可能性があります。 注 フォルダーを削除するときには注意してください。削除した内容を回復できない可能性があります。
詳細については、「アイテムの削除」を参照してください。

REST API:[メッセージを削除する (REST)](#DeleteAMessage)

クライアント ライブラリ:[メッセージを削除する (クライアント)](#DeleteMessagesClient)

<a name="DeleteAMessage"></a>
###メッセージを削除する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]



```no-highlight
DELETE https://outlook.office.com/api/beta/me/messages/{message_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|削除するメッセージの ID です。|

**要求のサンプル**

```
DELETE https://outlook.office.com/api/beta/me/messages/AAMkAGE0Mz8TBAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/messages/{message_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|削除するメッセージの ID です。|

**要求のサンプル**

```
DELETE https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8TBAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/messages/{message_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|削除するメッセージの ID です。|

[!code-REST-i[mail_api_delete_message_by_id](./_data/mail_api_delete_message_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




****

<a name="DeleteMessagesClient"> </a>
###メッセージを削除する (クライアント)

メッセージを削除するには、**DeleteAsync** を呼び出します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[メッセージ ID](#GetMessagesClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage message = await outlookClient.Me.Messages[messageId].ExecuteAsync();
await message.DeleteAsync();
```

<!-- ENDSECTION -->

****


<a name="MoveCopyMessages"> </a>
##メッセージを移動またはコピーする

メッセージをフォルダーに移動またはコピーできます。

REST API:メッセージを移動する (REST)  メッセージをコピーする (REST)

クライアント ライブラリ:[メッセージを移動またはコピーする (クライアント)](#MoveMessagesClient)

<a name="MoveMessage"> </a>
###メッセージを移動する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

Move a message to a folder. メッセージをフォルダーに移動します。これにより、宛先フォルダーにメッセージの新しいコピーが作成されます。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/move
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|移動するメッセージの ID です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/messages/AAMkAGI2TIy-AAA=/move
Content-Type: application/json

{
  "DestinationId": "AAMkAGI2AAEJAAA="
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0MGz_vSAAA=')",
  "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP\"",
  "Id": "AAMkAGI2shBhAAA=",
  "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP",
  "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
  "Categories": [],
  "CreatedDateTime": "2014-10-20T00:13:21Z",
  "LastModifiedDateTime": "2014-10-20T00:13:23Z",
  "Subject": "Contract Signing",
  "BodyPreview": "There will be a detailed legal review of Project Falcon once the contract is ready.",
  "Body": {
    "ContentType": "Text",
    "Content": "There will be a detailed legal review of Project Falcon once the contract is ready."
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGI2AAEJAAA=",
  "From": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    },
    {
      "EmailAddress": {
        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Garth Fort"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGI2NGWgitxag=",
  "ConversationIndex": "AQHRh6etrkAcds2kw==",
  "ReceivedDateTime": "2014-10-20T00:13:21Z",
  "SentDateTime": "2014-10-20T00:13:21Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": false,
  "IsRead": true
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/move
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|移動するメッセージの ID です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGI2TIy-AAA=/move
Content-Type: application/json

{
  "DestinationId": "AAMkAGI2AAEJAAA="
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0MGz_vSAAA=')",
  "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP\"",
  "Id": "AAMkAGI2shBhAAA=",
  "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP",
  "Categories": [],
  "CreatedDateTime": "2014-10-20T00:13:21Z",
  "LastModifiedDateTime": "2014-10-20T00:13:23Z",
  "Subject": "Contract Signing",
  "BodyPreview": "There will be a detailed legal review of Project Falcon once the contract is ready.",
  "Body": {
    "ContentType": "Text",
    "Content": "There will be a detailed legal review of Project Falcon once the contract is ready."
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGI2AAEJAAA=",
  "From": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    },
    {
      "EmailAddress": {
        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Garth Fort"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGI2NGWgitxag=",
  "ReceivedDateTime": "2014-10-20T00:13:21Z",
  "SentDateTime": "2014-10-20T00:13:21Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": false,
  "IsRead": true
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/move
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|移動するメッセージの ID です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|

[!code-REST-i[mail_api_move_message_by_id](./_data/mail_api_move_message_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

移動された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

****


<a name="CopyMessage"> </a>
###メッセージをコピーする (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

メッセージをフォルダーにコピーします。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/copy
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|コピーするメッセージの ID です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/messages/AAMkAGI2TIy-AAA=/copy
Content-Type: application/json

{
  "DestinationId": "inbox"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz8TDAAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIS\"",
  "Id": "AAMkAGI2T8DtAAA=",
  "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP",
  "InternetMessageId": "SN2PR00M@SN2.namprd00.prod.outlook.com",
  "Categories": [],
  "CreatedDateTime": "2014-10-20T00:13:21Z",
  "LastModifiedDateTime": "2014-10-20T00:13:23Z",
  "Subject": "Contract Signing",
  "BodyPreview": "There will be a detailed legal review of Project Falcon once the contract is ready.",
  "Body": {
    "ContentType": "Text",
    "Content": "There will be a detailed legal review of Project Falcon once the contract is ready."
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "From": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    },
    {
      "EmailAddress": {
        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Garth Fort"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAKjRc0YJSUBJpofjWgitxag=",
  "ConversationIndex": "AQHRh6sdrkAcds2kw==",
  "ReceivedDateTime": "2014-10-20T00:13:21Z",
  "SentDateTime": "2014-10-20T00:13:21Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": false,
  "IsRead": true
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/copy
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|コピーするメッセージの ID です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/messages/AAMkAGI2TIy-AAA=/copy
Content-Type: application/json

{
  "DestinationId": "inbox"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE0Mz8TDAAA=')",
  "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIS\"",
  "Id": "AAMkAGI2T8DtAAA=",
  "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP",
  "Categories": [],
  "CreatedDateTime": "2014-10-20T00:13:21Z",
  "LastModifiedDateTime": "2014-10-20T00:13:23Z",
  "Subject": "Contract Signing",
  "BodyPreview": "There will be a detailed legal review of Project Falcon once the contract is ready.",
  "Body": {
    "ContentType": "Text",
    "Content": "There will be a detailed legal review of Project Falcon once the contract is ready."
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "From": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "Sender": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Alex D"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Katie Jordan"
      }
    },
    {
      "EmailAddress": {
        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Garth Fort"
      }
    }
  ],
  "CcRecipients": [],
  "BccRecipients": [],
  "ReplyTo": [],
  "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAKjRc0YJSUBJpofjWgitxag=",
  "ReceivedDateTime": "2014-10-20T00:13:21Z",
  "SentDateTime": "2014-10-20T00:13:21Z",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsDraft": false,
  "IsRead": true
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/copy
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|コピーするメッセージの ID です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


[!code-REST-i[mail_api_copy_message_by_id](./_data/mail_api_copy_message_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


**応答の種類**

[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)の新しいコピーです。

****

<a name="MoveMessagesClient"> </a>
### メッセージを移動またはコピーする (クライアント)

メッセージを移動またはコピーするには、宛先フォルダーの ID を **MoveAsync** または **CopyAsync** メソッドに渡します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)、[メッセージ ID](#GetMessagesClient)、および[宛先フォルダーの ID](#GetFoldersClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage messageToMove = await outlookClient.Me.Messages[messageId].ExecuteAsync();
await messageToMove.MoveAsync("AAMkADE3N...");
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IMessage messageToCopy = await outlookClient.Me.Messages[messageId].ExecuteAsync();
await messageToCopy.CopyAsync("Inbox");
```

<!-- ENDSECTION -->

****

<a name="ManageFocusedInbox"></a>
##優先受信トレイを管理します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Focused Inbox allows you to view important messages in the `Focused` tab of the Inbox, and the rest of the Inbox messages in the `Other` tab. The classification system initially organizes Inbox messages in a default way. You can correct and train the system over time through the user interface or programmatically. The more you use it, the better the system can infer which incoming message as important.

At the programmatic level, the Focused Inbox REST API works on the specified user's messages, and supports an **InferenceClassification** property for each message. The possible values are `Focused` and `Other`, which indicate whether the user considers that message as, respectively, more important and less important. To correct and train the message classification system, use the [PATCH operation on the **InferenceClassification** property](#UpdateMessageClassificationBeta) at the message level. 

The Focused Inbox REST API also lets you create overrides. Each override, represented by an [InferenceClassificationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassificationOverrideResource) instance, is an instruction for the classification system to always designate messages from a specific sender in a consistent way (i.e., always as "Focused" or always as "Other"), regardless of any previously learned approach. You can [create](#CreateOverrideBeta), [get](#GetAllSenderOverridesBeta), [update](#UpdateOverrideBeta) and [delete](#DeleteOverrideBeta) overrides for the specified user. That user's overrides, if any, are accessible in an **InferenceClassification** navigation property, which is a collection of **InferenceClassificationOverride** instances. 更新、および 削除することができます。ユーザーのオーバーライド (ある場合) とは、InferenceClassificationOverride インスタンスのコレクションであり、InferenceClassification ナビゲーション プロパティでアクセスできます。オーバーライドでは、ユーザーは受信メッセージの分類をさらに細かく制御でき、分類システムの信頼性を高めることができます。

Note that the classification system learns and applies classification only on incoming messages in the Inbox. Messages in other folders are by default "Focused". 分類システムでは受信トレイの受信メッセージについてのみ分類を学習し、適用することに注意してください。その他のフォルダーにあるメッセージは、既定で「優先」になっています。オーバーライドの設定は、今後、受信トレイに入ってくるメッセージに反映され、オーバーライドでは、受信トレイを含むフォルダーにある既存のメッセージの **InferenceClassification** プロパティは変更されません。


**メッセージ分類システムの記憶**

[メッセージ分類を更新する](#UpdateMessageClassificationBeta)


**オーバーライドを使用した、送信者ごとに一貫した分類**

送信者に対するオーバーライドを作成する  すべてのユーザーのオーバーライドを取得する  送信者に対するオーバーライドを更新する  送信者のオーバーライドを削除する 


<a name="UpdateMessageClassificationBeta"></a>
###メッセージ分類を更新する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

Change the **InferenceClassification** property of the specified message. If the message is in the Inbox, the user will see that message under the corresponding `Focused` or `Other` tab. This kind of correction also trains the message classification system to customize future classification for the specified user. 

```no-highlight
PATCH https://outlook.office.com/api/beta/me/messages('{message_id}')

PATCH https://outlook.office.com/api/beta/Users('{user_id}')/messages('{message_id}')
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|送信する下書きメッセージの ID です。|
|user_id|string|ユーザーの電子メール アドレスです。 |


**応答の種類**

更新された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

**要求のサンプル:**

この例では、サインイン ユーザーの指定したメッセージについて **InferenceClassification** プロパティを Other`Other` に変更します。
```
PATCH https://outlook.office.com/api/beta/me/messages('AAMkADA1MTQBAAA=')

{
    "InferenceClassification": "Other"
}
```

**応答のサンプル:**

個々に示す応答オブジェクトは、更新された **InferenceClassification** プロパティを示しており、切り詰めて簡略化されています。実際の PATCH 要求では、メッセージのすべてのプロパティが返されます。
```
Status code: 200 OK

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Messages('AAMkADA1MTQBAAA=')",
    "@odata.etag": "W/\"CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAffAsD\"",
    "Id": "AAMkADA1MTQBAAA=",
    "Importance": "Normal",
    "Sender": {
        "EmailAddress": {
            "Name": "Fanny Downs",
            "Address": "fannyd@adatum.onmicrosoft.com"
        }
    },
    "From": {
        "EmailAddress": {
            "Name": "Fanny Downs",
            "Address": "fannyd@adatum.onmicrosoft.com"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Name": "Admin",
                "Address": "admin@adatum.onmicrosoft.com"
            }
        }
    ],
    "InferenceClassification": "Other"
}
```


<a name="CreateOverrideBeta"></a>
###送信者のオーバーライドを作成する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

SMTP アドレスで示される送信者のオーバーライドを作成します。この SMTP アドレスからの将来のメッセージは、オーバーライドで指定されたとおり一貫して分類されます。

<a name="CreateOverrideNoteBeta"></a>

**注意**

- 同じ STMP アドレスですでにオーバーライドがある場合は、そのオーバーライドの **ClassifyAs** および **Name** フィールドが指定の値で更新されます。
- メールボックスでサポートされているオーバーライドは、一意の送信者 SMTP アドレスに基づき、最大 1000 件までです。
- POST 操作では、同時に作成できるオーバーライドは 1 件です。複数のオーバーライドを作成するには、複数の POST 要求を送信するか、または[バッチ](..\api\batch-outlook-rest-requests.md)で送信します。


```no-highlight
POST https://outlook.office.com/api/beta/me/InferenceClassification/Overrides

POST https://outlook.office.com/api/beta/Users('{user_id}')/InferenceClassification/Overrides
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。 |

**応答の種類**

新しく作成された [InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource)、または同じ SMTP アドレスがすでにある場合は更新された **InferenceClassicationOverride** インスタンス。


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/InferenceClassification/Overrides

{
    "ClassifyAs": "Focused",
    "SenderEmailAddress": {
        "Name": "Fanny Downs",
        "Address": "fannyd@adatum.onmicrosoft.com"
    }
}
```

**応答のサンプル:**

```
Status code: 201 Created

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/InferenceClassification/Overrides/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf11a9b9')",
    "Id": "98f5bdef-576a-404d-a2ea-07a3cf11a9b9",
    "ClassifyAs": "Focused",
    "SenderEmailAddress": {
        "Name": "Fanny Downs",
        "Address": "fannyd@adatum.onmicrosoft.com"
    }
}
```


<a name="GetAllSenderOverridesBeta"></a>
###すべてのユーザーのオーバーライドを取得する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_

ユーザーが設定したオーバーライドを取得して、特定の送信者からのメッセージを常に一定の方法で分類します。

それぞれのオーバーライドは、送信者の SMTP アドレスに対応します。最初は、ユーザーにはオーバーライドはありません。 それぞれのオーバーライドは、送信者の SMTP アドレスに対応します。最初は、ユーザーにはオーバーライドはありません。

```no-highlight
GET https://outlook.office.com/api/beta/me/InferenceClassification/Overrides

GET https://outlook.office.com/api/beta/Users('{user_id}')/InferenceClassification/Overrides
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。 |


**応答の種類**

A collection of [InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource) instances.
InferenceClassicationOverride インスタンスのコレクション。ユーザーがオーバーライドを設定していない場合は、空のコレクションが返されます。


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/InferenceClassification/Overrides
```

**応答のサンプル:**

```
Status code: 200 OK

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/InferenceClassification/Overrides",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf11a9b9')",
            "Id": "98f5bdef-576a-404d-a2ea-07a3cf11a9b9",
            "ClassifyAs": "Focused",
            "SenderEmailAddress": {
                "Name": "Fanny Downs",
                "Address": "fannyd@adatum.onmicrosoft.com"
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')",
            "Id": "98f5bdef-576a-404d-a2ea-07a3cf34af4r",
            "ClassifyAs": "Other",
            "SenderEmailAddress": {
                "Name": "Randi Welch",
                "Address": "randiw@adatum.onmicrosoft.com"
            }
        }
    ]
}
```


<a name="UpdateOverrideBeta"></a>
###送信者のオーバーライドを更新する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

指定のとおり、オーバーライドの **ClassifyAs** フィールドを変更します。 

[InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource) インスタンスのその他のフィールドの変更には、PATCH を使用できません。 

送信者にオーバーライドがあり、その送信者が表示名を変更すると、[既存のオーバーライドで POST を使用して [名前] フィールドの更新を実施できます](#CreateOverrideNoteBeta)。

送信者にオーバーライドがあり、その送信者が SMTP アドレスを変更すると、既存のオーバーライドを[削除](#DeleteOverrideBeta)して、新しい SMTP アドレスで新しくオーバーライドを[作成](#CreateOverrideBeta)することが、この送信者のオーバーライドを「更新」する唯一の方法になります。


```no-highlight
PATCH https://outlook.office.com/api/beta/me/InferenceClassification/Overrides('{override_id}')

PATCH https://outlook.office.com/api/beta/Users('{user_id}')/InferenceClassification/Overrides('{override_id}')
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|override_id|string|更新するオーバーライドの ID です。|
|user_id|string|ユーザーの電子メール アドレスです。 |


**応答の種類**

更新された [InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource) インスタンス。


**要求のサンプル:**

The following example changes an override for the signed-in user. 次の例では、サインイン ユーザーのオーバーライドを変更します。オーバーライドは SMTP アドレス randiw@adatum.onmicrosoft.com の送信者のものであり、Other`Other` から Focused`Focused` に変更されます。
```
PATCH https://outlook.office.com/api/beta/me/InferenceClassification/Overrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')

{
    "ClassifyAs": "Focused"
}

```

**応答のサンプル:**

```
Status code: 200 OK

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/InferenceClassification/Overrides/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')",
    "Id": "98f5bdef-576a-404d-a2ea-07a3cf34af4r",
    "ClassifyAs": "Focused",
    "SenderEmailAddress": {
        "Name": "Randi Welch",
        "Address": "randiw@adatum.onmicrosoft.com"
    }
}
```


<a name="DeleteOverrideBeta"></a>
###送信者のオーバーライドを削除する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

その ID で指定されたオーバーライドを削除します。


```no-highlight
DELETE https://outlook.office.com/api/beta/me/InferenceClassification/Overrides('{override_id}')

DELETE https://outlook.office.com/api/beta/Users('{user_id}')/InferenceClassification/Overrides('{override_id}')
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|override_id|string|送信する下書きメッセージの ID です。|
|user_id|string|ユーザーの電子メール アドレスです。 |


**要求のサンプル**

```
DELETE https://outlook.office.com/api/beta/me/InferenceClassification/Overrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')
```

**応答のサンプル:**

```
Status code: 204 No Content
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Focused Inbox allows you to view important messages in the `Focused` tab of the Inbox, and the rest of the Inbox messages in the `Other` tab. The classification system initially organizes Inbox messages in a default way. You can correct and train the system over time through the user interface or programmatically. The more you use it, the better the system can infer which incoming message as important.

At the programmatic level, the Focused Inbox REST API works on the specified user's messages, and supports an **InferenceClassification** property for each message. The possible values are `Focused` and `Other`, which indicate whether the user considers that message as, respectively, more important and less important. To correct and train the message classification system, use the [PATCH operation on the **InferenceClassification** property](#UpdateMessageClassificationV2) at the message level. 

The Focused Inbox REST API also lets you create overrides. Each override, represented by an [InferenceClassificationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassificationOverrideResource) instance, is an instruction for the classification system to always designate messages from a specific sender in a consistent way (i.e., always as "Focused" or always as "Other"), regardless of any previously learned approach. You can [create](#CreateOverrideV2), [get](#GetAllSenderOverridesV2), [update](#UpdateOverrideV2) and [delete](#DeleteOverrideV2) overrides for the specified user. That user's overrides, if any, are accessible in an **InferenceClassification** navigation property, which is a collection of **InferenceClassificationOverride** instances. 更新、および 削除することができます。ユーザーのオーバーライド (ある場合) とは、InferenceClassificationOverride インスタンスのコレクションであり、InferenceClassification ナビゲーション プロパティでアクセスできます。オーバーライドでは、ユーザーは受信メッセージの分類をさらに細かく制御でき、分類システムの信頼性を高めることができます。

Note that the classification system learns and applies classification only on incoming messages in the Inbox. Messages in other folders are by default "Focused". 分類システムでは受信トレイの受信メッセージについてのみ分類を学習し、適用することに注意してください。その他のフォルダーにあるメッセージは、既定で「優先」になっています。オーバーライドの設定は、今後、受信トレイに入ってくるメッセージに反映され、オーバーライドでは、受信トレイを含むフォルダーにある既存のメッセージの **InferenceClassification** プロパティは変更されません。


**メッセージ分類システムの記憶**

[メッセージ分類を更新する](#UpdateMessageClassificationV2)


**オーバーライドを使用した、送信者ごとに一貫した分類**

送信者に対するオーバーライドを作成する  すべてのユーザーのオーバーライドを取得する  送信者に対するオーバーライドを更新する  送信者のオーバーライドを削除する 


<a name="UpdateMessageClassificationV2"></a>
###メッセージ分類を更新する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

Change the **InferenceClassification** property of the specified message. If the message is in the Inbox, the user will see that message under the corresponding `Focused` or `Other` tab. This kind of correction also trains the message classification system to customize future classification for the specified user. 

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/messages('{message_id}')

PATCH https://outlook.office.com/api/v2.0/Users('{user_id}')/messages('{message_id}')
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|送信する下書きメッセージの ID です。|
|user_id|string|ユーザーの電子メール アドレスです。 |


**応答の種類**

更新された[メッセージ](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)です。

**要求のサンプル:**

この例では、サインイン ユーザーの指定したメッセージについて **InferenceClassification** プロパティを Other`Other` に変更します。
```
PATCH https://outlook.office.com/api/v2.0/me/messages('AAMkADA1MTQBAAA=')

{
    "InferenceClassification": "Other"
}
```

**応答のサンプル:**

個々に示す応答オブジェクトは、更新された **InferenceClassification** プロパティを示しており、切り詰めて簡略化されています。実際の PATCH 要求では、メッセージのすべてのプロパティが返されます。
```
Status code: 200 OK

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Messages('AAMkADA1MTQBAAA=')",
    "@odata.etag": "W/\"CQAAABYAAADX8oL1Wa7jQbcPAHouCzswAAAffAsD\"",
    "Id": "AAMkADA1MTQBAAA=",
    "Importance": "Normal",
    "Sender": {
        "EmailAddress": {
            "Name": "Fanny Downs",
            "Address": "fannyd@adatum.onmicrosoft.com"
        }
    },
    "From": {
        "EmailAddress": {
            "Name": "Fanny Downs",
            "Address": "fannyd@adatum.onmicrosoft.com"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Name": "Admin",
                "Address": "admin@adatum.onmicrosoft.com"
            }
        }
    ],
    "InferenceClassification": "Other"
}
```


<a name="CreateOverrideV2"></a>
###送信者のオーバーライドを作成する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

SMTP アドレスで示される送信者のオーバーライドを作成します。この SMTP アドレスからの将来のメッセージは、オーバーライドで指定されたとおり一貫して分類されます。

<a name="CreateOverrideNoteV2"></a>

**注意**

- 同じ STMP アドレスですでにオーバーライドがある場合は、そのオーバーライドの **ClassifyAs** および **Name** フィールドが指定の値で更新されます。
- メールボックスでサポートされているオーバーライドは、一意の送信者 SMTP アドレスに基づき、最大 1000 件までです。
- POST 操作では、同時に作成できるオーバーライドは 1 件です。複数のオーバーライドを作成するには、複数の POST 要求を送信するか、または[バッチ](..\api\batch-outlook-rest-requests.md)で送信します。


```no-highlight
POST https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides

POST https://outlook.office.com/api/v2.0/Users('{user_id}')/InferenceClassification/Overrides
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。 |


**応答の種類**

新しく作成された [InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource)、または同じ SMTP アドレスがすでにある場合は更新された **InferenceClassicationOverride** インスタンス。


**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides

{
    "ClassifyAs": "Focused",
    "SenderEmailAddress": {
        "Name": "Fanny Downs",
        "Address": "fannyd@adatum.onmicrosoft.com"
    }
}
```

**応答のサンプル:**

```
Status code: 201 Created

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/InferenceClassification/Overrides/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf11a9b9')",
    "Id": "98f5bdef-576a-404d-a2ea-07a3cf11a9b9",
    "ClassifyAs": "Focused",
    "SenderEmailAddress": {
        "Name": "Fanny Downs",
        "Address": "fannyd@adatum.onmicrosoft.com"
    }
}
```


<a name="GetAllSenderOverridesV2"></a>
###すべてのユーザーのオーバーライドを取得する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_

ユーザーが設定したオーバーライドを取得して、特定の送信者からのメッセージを常に一定の方法で分類します。

それぞれのオーバーライドは、送信者の SMTP アドレスに対応します。最初は、ユーザーにはオーバーライドはありません。 それぞれのオーバーライドは、送信者の SMTP アドレスに対応します。最初は、ユーザーにはオーバーライドはありません。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides

GET https://outlook.office.com/api/v2.0/Users('{user_id}')/InferenceClassification/Overrides
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。 |


**応答の種類**

A collection of [InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource) instances.
InferenceClassicationOverride インスタンスのコレクション。ユーザーがオーバーライドを設定していない場合は、空のコレクションが返されます。


**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides
```

**応答のサンプル:**

```
Status code: 200 OK

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/InferenceClassification/Overrides",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf11a9b9')",
            "Id": "98f5bdef-576a-404d-a2ea-07a3cf11a9b9",
            "ClassifyAs": "Focused",
            "SenderEmailAddress": {
                "Name": "Fanny Downs",
                "Address": "fannyd@adatum.onmicrosoft.com"
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')",
            "Id": "98f5bdef-576a-404d-a2ea-07a3cf34af4r",
            "ClassifyAs": "Other",
            "SenderEmailAddress": {
                "Name": "Randi Welch",
                "Address": "randiw@adatum.onmicrosoft.com"
            }
        }
    ]
}
```


<a name="UpdateOverrideV2"></a>
###送信者のオーバーライドを更新する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

指定のとおり、オーバーライドの **ClassifyAs** フィールドを変更します。 

[InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource) インスタンスのその他のフィールドの変更には、PATCH を使用できません。 

送信者にオーバーライドがあり、その送信者が表示名を変更すると、[既存のオーバーライドで POST を使用して [名前] フィールドの更新を実施できます](#CreateOverrideNoteV2)。

送信者にオーバーライドがあり、その送信者が SMTP アドレスを変更すると、既存のオーバーライドを[削除](#DeleteOverrideV2)して、新しい SMTP アドレスで新しくオーバーライドを[作成](#CreateOverrideV2)することが、この送信者のオーバーライドを「更新」する唯一の方法になります。


```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides('{override_id}')

PATCH https://outlook.office.com/api/v2.0/Users('{user_id}')/InferenceClassification/Overrides('{override_id}')
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|override_id|string|更新するオーバーライドの ID です。|
|user_id|string|ユーザーの電子メール アドレスです。 |


**応答の種類**

更新された [InferenceClassicationOverride](..\api\complex-types-for-mail-contacts-calendar.md#InferenceClassicationOverrideResource) インスタンス。


**要求のサンプル:**

The following example changes an override for the signed-in user. 次の例では、サインイン ユーザーのオーバーライドを変更します。オーバーライドは SMTP アドレス randiw@adatum.onmicrosoft.com の送信者のものであり、Other`Other` から Focused`Focused` に変更されます。
```
PATCH https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')

{
    "ClassifyAs": "Focused"
}

```

**応答のサンプル:**

```
Status code: 200 OK

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/InferenceClassification/Overrides/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/InferenceClassificationOverrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')",
    "Id": "98f5bdef-576a-404d-a2ea-07a3cf34af4r",
    "ClassifyAs": "Focused",
    "SenderEmailAddress": {
        "Name": "Randi Welch",
        "Address": "randiw@adatum.onmicrosoft.com"
    }
}
```


<a name="DeleteOverrideV2"></a>
###送信者のオーバーライドを削除する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

その ID で指定されたオーバーライドを削除します。


```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides('{override_id}')

DELETE https://outlook.office.com/api/v2.0/Users('{user_id}')/InferenceClassification/Overrides('{override_id}')
```

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|override_id|string|送信する下書きメッセージの ID です。|
|user_id|string|ユーザーの電子メール アドレスです。 |


**要求のサンプル**

```
DELETE https://outlook.office.com/api/v2.0/me/InferenceClassification/Overrides('98f5bdef-576a-404d-a2ea-07a3cf34af4r')
```

**応答のサンプル:**

```
Status code: 204 No Content
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在 v2.0 とベータ版で利用できます。詳細については、記事の右上隅にコントロールを使用して、どちらかのバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



****
 
<a name="Unsubscribe"></a>
##登録を解除する (プレビュー)

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.send_
- _wl.imap_

サイン インユーザーに代わって電子メール要求を送信し、電子メール配布リストから登録を解除します。List-Unsubscribe ヘッダー内の情報を使用します。 Uses the information in the `List-Unsubscribe` header.

```no-highlight
POST https://outlook.office.com/api/beta/me/messages('{message_id}')/unsubscribe
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|送信する下書きメッセージの ID です。|


メッセージ送信者は、メーリング リストで受信者を除外するオプションを組み込む簡単な方法を使用できます。これは、それぞれのメッセージで RFC-2369 に続いて List-Unsubscribe ヘッダーを指定することで処理できます。 They can do so by specifying the `List-Unsubscribe` header in each message following [RFC-2369](http://www.faqs.org/rfcs/rfc2369.html).

具体的には、**Unsubscribe** アクションを有効にするには、送信者が URL ベースの登録解除情報ではなく、mailto:`mailto:` を指定する必要があります。

そのヘッダーを設定すると、[メッセージ](../api/complex-types-for-mail-contacts-calendar.md#MessageResource) インスタンスの **UnsubscribeEnabled** プロパティが true`true` に設定され、**UnsubscribeData** プロパティがヘッダー データに設定されます。

メッセージの **UnsubscribeEnabled** プロパティが true`true` の場合、**Unsubscribe** アクションを使用して、メッセージ送信者が管理するとおり、同じような今後のメッセージについてユーザーを登録解除することができます。 

**Unsubscribe** アクションが完了すると、メッセージは削除済みアイテム フォルダーに移動します。将来のメール配布からのユーザーの実際の除外は、送信者によって管理されます。



**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/messages('AAMkADA1MTk1ZAAAKXBQCAAA=')/unsubscribe
```

**応答のサンプル:**

```
Status code: 202 Accepted
```
 


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****

<a name="GetAutoReplySettings"></a>
##自動応答の設定を取得する (プレビュー)

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

自動応答では、メールを受信したときに、そのメッセージの送信者に自動的にメッセージで通知することができます。たとえば、不在時に返信できない場合などにそれを通知できます。

自動応答はユーザーのメールボックス設定の一部 ([MailboxSettings](../api/complex-types-for-mail-contacts-calendar.md#MailboxSettings)) であるため、自動応答設定を含むすべてのメールボックス設定を取得、または自動応答設定だけを取得すると表示できます。

Prefer: outlook.timezone`Prefer: outlook.timezone`HTTP ヘッダーを使用して優先するタイムゾーンに **ScheduledStartDateTime** と **ScheduledEndDateTime** の値が表示されるように指定します。

REST API:すべてのメールボックス設定を取得する (プレビュー)  自動応答設定のみを取得する (プレビュー)


<a name="GetAllMailboxSettings"></a>
###すべてのメールボックスの設定を取得する (プレビュー)

__**最小必須範囲**: https://outlook.office.com/mailboxsettings.readwrite__

サインイン ユーザーのプライマリ メールボックスの設定を取得します。


```no-highlight
GET https://outlook.office.com/api/beta/me/MailboxSettings
```

 **応答の種類**

[MailboxSettings](../api/complex-types-for-mail-contacts-calendar.md#MailboxSettings).


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/MailboxSettings
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailboxSettings",
    "AutomaticRepliesSetting": {
        "Status": "Scheduled",
        "ExternalAudience": "All",
        "ScheduledStartDateTime": {
            "DateTime": "2016-03-14T07:00:00.0000000",
            "TimeZone": "UTC"
        },
        "ScheduledEndDateTime": {
            "DateTime": "2016-03-28T07:00:00.0000000",
            "TimeZone": "UTC"
        },
        "InternalReplyMessage": "<html>\n<body>\n<p>I'm at our company's worldwide reunion and will respond to your message as soon as I return.<br>\n</p></body>\n</html>\n",
        "ExternalReplyMessage": "<html>\n<body>\n<p>I'm at the Contoso worldwide reunion and will respond to your message as soon as I return.<br>\n</p></body>\n</html>\n"
    },
    "TimeZone": "Pacific Standard Time",
    "Language":{
        "Locale":"en-US",
        "DisplayName":"English (United States)"
    }
}
```


<a name="GetOnlyAutoReplySettings"></a>
###自動応答の設定を取得する (プレビュー)

__**最小必須範囲**: https://outlook.office.com/mailboxsettings.readwrite__

サインインしているユーザーのメールボックスの自動応答の設定を取得します。


```no-highlight
GET https://outlook.office.com/api/beta/me/MailboxSettings/AutomaticRepliesSetting
```

 **応答の種類**

[AutomaticRepliesSetting](#AutomaticRepliesSetting).


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/MailboxSettings/AutomaticRepliesSetting
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailboxSettings/AutomaticRepliesSetting",
    "Status": "AlwaysEnabled",
    "ExternalAudience": "None",
    "ScheduledStartDateTime": {
        "DateTime": "2016-03-19T02:00:00.0000000",
        "TimeZone": "UTC"
    },
    "ScheduledEndDateTime": {
        "DateTime": "2016-03-20T02:00:00.0000000",
        "TimeZone": "UTC"
    },
    "InternalReplyMessage": "<html>\n<body>\n<p>I'm at our company's worldwide reunion and will respond to your message as soon as I return.<br>\n</p></body>\n</html>\n",
    "ExternalReplyMessage": "<html>\n<body>\n<p>I'm at the Contoso worldwide reunion and will respond to your message as soon as I return.<br>\n</p></body>\n</html>\n"
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****



<a name = "UpdateAutoReplySettings"></a>
##自動応答の設定を更新する (プレビュー)

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

__**最小必須範囲**: https://outlook.office.com/mailboxsettings.readwrite__


自動応答はユーザーの ([MailboxSettings](../api/complex-types-for-mail-contacts-calendar.md#MailboxSettings) で表される) メールボックス設定の一部です。該当するメールボックス設定を更新して、自動応答を有効化、構成、または無効化することができます。 

**注** メールボックス設定は作成または削除できません。

```no-highlight
PATCH https://outlook.office.com/api/beta/me/MailboxSettings
```

**応答の種類**

[MailboxSettings](../api/complex-types-for-mail-contacts-calendar.md#MailboxSettings).


**要求のサンプル:**

自動応答設定の取得を示す[前の例](#GetOnlyAutoReplySettings)に続き、次の例では**ステータス**を AlwaysEnabled`AlwaysEnabled` から Scheduled`Scheduled` に変更し、開始日と終了日を異なる日付範囲に変更します。



```
PATCH https://outlook.office.com/api/beta/me/MailboxSettings
Content-Type: application/json

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailboxSettings",
    "AutomaticRepliesSetting": {
        "Status": "Scheduled",
        "ScheduledStartDateTime": {
          "DateTime": "2016-03-20T18:00:00.0000000",
          "TimeZone": "UTC"
        },
        "ScheduledEndDateTime": {
          "DateTime": "2016-03-28T18:00:00.0000000",
          "TimeZone": "UTC"
        }
    }
}
```



**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailboxSettings",
    "AutomaticRepliesSetting": {
        "Status": "Scheduled",
        "ExternalAudience": "None",
        "ScheduledStartDateTime": {
            "DateTime": "2016-03-20T02:00:00.0000000",
            "TimeZone": "UTC"
        },
        "ScheduledEndDateTime": {
            "DateTime": "2016-03-28T02:00:00.0000000",
            "TimeZone": "UTC"
        },
    "InternalReplyMessage": "<html>\n<body>\n<p>I'm at our company's worldwide reunion and will respond to your message as soon as I return.<br>\n</p></body>\n</html>\n",
    "ExternalReplyMessage": "<html>\n<body>\n<p>I'm at the Contoso worldwide reunion and will respond to your message as soon as I return.<br>\n</p></body>\n</html>\n"
    },
    "TimeZone": "Pacific Standard Time",
    "Language":{
        "Locale":"en-US",
        "DisplayName":"English (United States)"
    }
}


```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


****

<a name="GetAttachments"> </a>
##添付ファイルの取得

添付ファイルのコレクションまたは添付ファイルを取得できます。 Attachments can be files (for example, 

REST API:添付ファイルのコレクションを取得する (REST)  添付ファイルを取得する (REST)

クライアント ライブラリ:[1 つ以上のメッセージの添付ファイルを取得する (クライアント)](#GetAttachmentsClient)


<a name="GetAttachmentCollection"> </a>
###添付ファイルのコレクションを取得する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_

特定のメッセージから添付ファイルを取得します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/messages/{message_id}/attachments
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|

**メモ**: 既定では、応答内の各添付ファイルに、その添付ファイルの種類に対応するすべてのプロパティが含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

**応答の種類**

[FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)、[ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)、または [ReferenceAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource) の種類を使用できる添付ファイル コレクション。


**要求と応答のサンプル**

次の例は、**$select** を使用して、応答内の各添付ファイルの **Name** プロパティのみを返すように指定する方法を示しています。**$select** を使用しない場合の添付ファイルに返されるプロパティの完全な一覧に関しては、「[イベントを取得する (REST)](#GetAttachment)」の応答サンプルをご参照ください。

**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/messages/AAMkAGI2THVSAAA=/attachments?$select=Name
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkAGI2THVSAAA%3D')/Attachments(Name)",
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2THVSAAA=')/Attachments('AAMkAGI2j4kShdM=')",
            "Id": "AAMkAGI2j4kShdM=",
            "Name": "minutes.docx"
        }
    ]
}
```

次の例では、Outlook のメール アイテムである添付ファイルのみの取得を示します。応答には、添付メッセージの ID でもある添付ファイル ID が含まれています。

```
GET https://outlook.office.com/api/beta/me/messages('AAMkADFiNTPAAA=')/attachments

Content-Type: application/json

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkADFiNTPAAA%3D')/Attachments",
  "value": [
    {
      "@odata.type": "#Microsoft.OutlookServices.ItemAttachment",
      "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-20075df800e5@1717622f-1d94-4d0c-9d74-f907ad6677b4')/Messages('AAMkADFiNTPAAA=')/Attachments('AAMkADFiNTAUhhYuYi0=')",
      "Id": "AAMkADFiNTAUhhYuYi0=",
      "Name": "How to retrieve item attachment using Outlook REST API",
      "ContentType": message/rfc822,
      "Size": 71094,
      "IsInline": false,
      "LastModifiedDateTime": "2015-09-24T05:57:59Z",
    }
  ]
}
```




[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages/{message_id}/attachments
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|

**メモ**: 既定では、応答内の各添付ファイルに、その添付ファイルの種類に対応するすべてのプロパティが含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


**応答の種類**

[FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource) または [ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource) の種類を使用できる添付ファイル コレクション。


**要求と応答のサンプル**

次の例は、**$select** を使用して、応答内の各添付ファイルの **Name** プロパティのみを返すように指定する方法を示しています。**$select** を使用しない場合の添付ファイルに返されるプロパティの完全な一覧に関しては、「[イベントを取得する (REST)](#GetAttachment)」の応答サンプルをご参照ください。

**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/messages/AAMkAGI2THVSAAA=/attachments?$select=Name
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkAGI2THVSAAA%3D')/Attachments(Name)",
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2THVSAAA=')/Attachments('AAMkAGI2j4kShdM=')",
            "Id": "AAMkAGI2j4kShdM=",
            "Name": "minutes.docx"
        }
    ]
}
```

次の例では、Outlook のメール アイテムである添付ファイルのみの取得を示します。応答には、添付メッセージの ID でもある添付ファイル ID が含まれています。

```
GET https://outlook.office.com/api/v2.0/me/messages('AAMkADFiNTPAAA=')/attachments

Content-Type: application/json

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkADFiNTPAAA%3D')/Attachments",
  "value": [
    {
      "@odata.type": "#Microsoft.OutlookServices.ItemAttachment",
      "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-20075df800e5@1717622f-1d94-4d0c-9d74-f907ad6677b4')/Messages('AAMkADFiNTPAAA=')/Attachments('AAMkADFiNTAUhhYuYi0=')",
      "Id": "AAMkADFiNTAUhhYuYi0=",
      "Name": "How to retrieve item attachment using Outlook REST API",
      "ContentType": message/rfc822,
      "Size": 71094,
      "IsInline": false,
      "LastModifiedDateTime": "2015-09-24T05:57:59Z",
    }
  ]
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages/{message_id}/attachments
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|

**メモ**: 既定では、応答内の各添付ファイルに、その添付ファイルの種類に対応するすべてのプロパティが含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


**応答の種類**

[FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource) または [ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource) の種類を使用できる添付ファイル コレクション。


**要求と応答のサンプル**

次の例は、**$select** を使用して、応答内の各添付ファイルの **Name** プロパティのみを返すように指定する方法を示しています。**$select** を使用しない場合の添付ファイルに返されるプロパティの完全な一覧に関しては、「[イベントを取得する (REST)](#GetAttachment)」の応答サンプルをご参照ください。

[!code-REST-i[mail_api_get_attachments](./_data/mail_api_get_attachments.json)]


次の例では、Outlook のメール アイテムである添付ファイルのみの取得を示します。応答には、添付メッセージの ID でもある添付ファイル ID が含まれています。

```
GET https://outlook.office.com/api/v1.0/me/messages('AAMkADFiNTPAAA=')/attachments

Content-Type: application/json

{
  "@odata.context": "https://outlook.office.com/api/v1.0/$metadata#Me/Messages('AAMkADFiNTPAAA%3D')/Attachments",
  "value": [
    {
      "@odata.type": "#Microsoft.OutlookServices.ItemAttachment",
      "@odata.id": "https://outlook.office.com/api/v1.0/Users('ddfcd489-628b-40d7-b48b-20075df800e5@1717622f-1d94-4d0c-9d74-f907ad6677b4')/Messages('AAMkADFiNTPAAA=')/Attachments('AAMkADFiNTAUhhYuYi0=')",
      "Id": "AAMkADFiNTAUhhYuYi0=",
      "Name": "How to retrieve item attachment using Outlook REST API",
      "ContentType": message/rfc822,
      "Size": 71094,
      "IsInline": false,
      "DateTimeLastModified": "2015-09-24T05:57:59Z",
    }
  ]
}
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****


<a name="GetAttachment"> </a>
###添付ファイルを取得する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_

特定のメッセージから添付ファイルを取得します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/messages/{message_id}/attachments/{attachment_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|attachment_id|string|添付ファイル ID。|

**メモ**: 既定では、応答に指定されたメッセージのすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に Refer to [Get an attachment collection (REST)](#GetAttachmentCollection) for an example.  
The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

**応答の種類**

要求された[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)、[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)、または[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource)。


**サンプル要求 (添付ファイル)**

次の例では、特定のメッセージに添付するファイルを取得します。

```
GET https://outlook.office.com/api/beta/me/messages/AAMkAGI2THVSAAA=/attachments/AAMkAGI2j4kShdM=
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkAGI2THVSAAA%3D')/Attachments/$entity",
    "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2THVSAAA=')/Attachments('AAMkAGI2j4kShdM=')",
    "Id": "AAMkAGI2j4kShdM=",
    "LastModifiedDateTime": "2014-10-20T00:41:52Z",
    "Name": "minutes.docx",
    "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    "Size": 11585,
    "IsInline": false,
    "ContentId": null,
    "ContentLocation": null,
    "ContentBytes": "UEsDBBQABgAIAAAAIQDCAAA4KQAAAAA="
}
```

**サンプル要求 (参照添付ファイル)**

次の例では、メッセージの参照添付ファイルを取得します。

```
GET https://outlook.office.com/api/beta/me/messages/AAMkAGE1Mbs88AADUv0uFAAA=/attachments/AAMkAGE1Mbs88AADUv0uFAAABEgAQAPSg72tgf7hJp0PICVGCc0g=
```

**応答のサンプル**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/messages('AAMkAGE1Mbs88AADUv0uFAAA%3D')/attachments/$entity",
  "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
  "Id": "AAMkAGE1Mbs88AADUv0uFAAABEgAQAPSg72tgf7hJp0PICVGCc0g=",
  "LastModifiedDateTime": "2016-03-12T06:04:38Z",
  "Name": "Koala picture",
  "ContentType": null,
  "Size": 382,
  "IsInline": false,
  "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/koala.jpg",
  "ProviderType": "OneDriveBusiness",
  "ThumbnailUrl": null,
  "PreviewUrl": null,
  "Permission": "Edit",
  "IsFolder": false
}
```


**サンプル要求 (添付ファイルの $expand)**

次の例では、メッセージのプロパティを持つ 3 つの参照添付ファイルすべてのインラインを取得して展開します。

```
GET https://outlook.office.com/api/beta/me/messages/AAMkAGE1Mbs88AADUv0uFAAA=/?$expand=attachments
```

**応答のサンプル**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/messages/$entity",
  "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AADZsPav\"",
  "Id": "AAMkAGE1Mbs88AADUv0uFAAA=",
  "CreatedDateTime": "2016-03-08T01:01:57Z",
  "LastModifiedDateTime": "2016-03-12T06:18:54Z",
  "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AADZsPav",
  "Categories": [ ],
  "ReceivedDateTime": "2016-03-08T01:01:57Z",
  "SentDateTime": "2016-03-08T01:01:51Z",
  "HasAttachments": true,
  "InternetMessageId": "<SN2SR0101MB00299F0D7D22EE5D380104ED84B20@SN2SR0101MB0029.namsdf01.sdf.exchangelabs.com>",
  "Subject": "RE: New year activity",
  "Body": {
    "ContentType": "html",
    "Content": "<html>\r\n<<body>Let's gather to celebrate the new year! </body>\r\n</html>\r\n"
  },
  "BodyPreview": "What about the tulips?\r\n________________________________\r\nFrom: Dana Swope <danas@contoso.onmicrosoft.com>\r\nSent: Monday, March 7, 2016 10:51:39 PM\r\nTo: Dana Swope; Culinary Expert Group\r\nSubject: RE: New year activity\r\n\r\nLet's gather to celebrate the new year! ",
  "Importance": "Normal",
  "ParentFolderId": "AQMkAGE1MQN7j5uzzwAAAIBDAAAAA==",
  "Sender": {
    "EmailAddress": {
      "Name": "Dana Swope",
      "Address": "danas@contoso.onmicrosoft.com"
    }
  },
  "From": {
    "EmailAddress": {
      "Name": "Dana Swope",
      "Address": "danas@contoso.onmicrosoft.com"
    }
  },
  "ToRecipients": [
    {
      "EmailAddress": {
        "Name": "Dana Swope",
        "Address": "danas@contoso.onmicrosoft.com"
      }
    },
    {
      "EmailAddress": {
        "Name": "Culinary Expert Group",
        "Address": "Chefs@contoso.onmicrosoft.com"
      }
    }
  ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkAGE1MMM2SaRFsKgx7BKVfig=",
  "ConversationIndex": "AQHRaThgdSG4wzZJpEWwqDHsEpV+KJ9OtWGUgAAkYLI=",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": false,
  "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1Mbs88AADUv0uFAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "InferenceClassification": "Focused",
  "UnsubscribeData": [ ],
  "UnsubscribeEnabled": false,
  "Flag": { "FlagStatus": "NotFlagged" },
  "Attachments@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/messages('AAMkAGE1Mbs88AADUv0uFAAA%3D')/attachments",
  "Attachments": [
    {
      "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
      "Id": "AAMkAGE1Mbs88AADUv0uFAAABEgAQAL53d0u3BKBJmCxKVxZKBZ8=",
      "LastModifiedDateTime": "2016-03-12T05:54:31Z",
      "Name": "Personal pictures",
      "ContentType": null,
      "Size": 362,
      "IsInline": false,
      "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics",
      "ProviderType": "OneDriveBusiness",
      "ThumbnailUrl": null,
      "PreviewUrl": null,
      "Permission": "edit",
      "IsFolder": true
    },
    {
      "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
      "Id": "AAMkAGE1Mbs88AADUv0uFAAABEgAQAPSg72tgf7hJp0PICVGCc0g=",
      "LastModifiedDateTime": "2016-03-12T06:04:38Z",
      "Name": "Koala picture",
      "ContentType": null,
      "Size": 382,
      "IsInline": false,
      "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/koala.jpg",
      "ProviderType": "OneDriveBusiness",
      "ThumbnailUrl": null,
      "PreviewUrl": null,
      "Permission": "edit",
      "IsFolder": false
    },
    {
      "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
      "Id": "AAMkAGE1Mbs88AADUv0uFAAABEgAQAO3wkFiM3KlCpn81m8qS1W0=",
      "LastModifiedDateTime": "2016-03-12T06:18:54Z",
      "Name": "Hydrangea picture",
      "ContentType": null,
      "Size": 412,
      "IsInline": false,
      "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/hydrangea.jpg",
      "ProviderType": "OneDriveBusiness",
      "ThumbnailUrl": null,
      "PreviewUrl": null,
      "Permission": "edit",
      "IsFolder": false
    }
  ]
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages/{message_id}/attachments/{attachment_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|attachment_id|string|添付ファイル ID。|

**メモ**: 既定では、応答に指定されたメッセージのすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に Refer to [Get an attachment collection (REST)](#GetAttachmentCollection) for an example.  
The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

 **応答の種類**

要求された[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)。

**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/messages/AAMkAGI2THVSAAA=/attachments/AAMkAGI2j4kShdM=
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkAGI2THVSAAA%3D')/Attachments/$entity",
    "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGI2THVSAAA=')/Attachments('AAMkAGI2j4kShdM=')",
    "Id": "AAMkAGI2j4kShdM=",
    "LastModifiedDateTime": "2014-10-20T00:41:52Z",
    "Name": "minutes.docx",
    "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    "Size": 11585,
    "IsInline": false,
    "ContentId": null,
    "ContentLocation": null,
    "ContentBytes": "UEsDBBQABgAIAAAAIQDCAAA4KQAAAAA="
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages/{message_id}/attachments/{attachment_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|attachment_id|string|添付ファイル ID。|

**メモ**: 既定では、応答に指定されたメッセージのすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に Refer to [Get an attachment collection (REST)](#GetAttachmentCollection) for an example.  
The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

 **応答の種類**

要求された[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)。


[!code-REST-i[mail_api_get_attachment_by_id](./_data/mail_api_get_attachment_by_id.json)]



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



****

<a name="GetAttachmentsClient"> </a>
###1 つ以上のメッセージの添付ファイルを取得する (クライアント)

Get attachments on message by calling the **Attachments** property. メッセージの添付ファイルを取得するには、**Attachments** プロパティを呼び出します。特定の添付ファイルを取得するには、**添付ファイル** コレクションのインデックスとして添付ファイル ID を指定するか、GetById メソッドを使用します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


**メモ**: 添付ファイル コレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。


この例では、[Outlook サービス クライアントを作成する](..\api\use-outlook-rest-api.md#GetClient)メソッドを呼び出します。 この例では、Outlook サービス クライアントを作成するメソッドを呼び出します。この .NET コードでは、既に[メッセージ ID が取得されている](#GetMessagesClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Mail" -->

<!--tested-->
```cs-i
var outlookClient = await CreateOutlookClientAsync("Mail");
var messages = await outlookClient.Me.Folders["Inbox"].Messages
  .Where(m => m.HasAttachments == true)
  .Expand(m => m.Attachments)
  .Take(10)
  .ExecuteAsync();

foreach (var message in messages.CurrentPage)
{
  var attachments = message.Attachments.CurrentPage;
  System.Diagnostics.Debug.WriteLine("Message '{0}' contains '{1}' attachment(s).",
    message.Subject,
    attachments.Count);

  foreach (var attachment in attachments)
  {
    System.Diagnostics.Debug.WriteLine("*    '{0}' ({1} KB)",
      attachment.Name,
      attachment.Size);
  }
}

```
```javascript-i
outlookClient.me.folders.getFolder('Inbox').messages.getMessages().filter('HasAttachments eq true').fetchAll(10).then(function (result) {
    result.forEach(function (message) {
        message.attachments.getAttachments().fetchAll(100).then(function (attachmentResult) {
            console.log('Message "' + message.subject + '" contains ' + attachmentResult.length + ' attachment(s)');
            attachmentResult.forEach(function (attachment) {
                console.log('*    "' + attachment.name + '" (' + attachment.size + ' KB)');
            });
        });
    }, function (error) {
        console.log(error);
    });
}, function (error) {
    console.log(error);
});
```

<!-- ENDSECTION -->

****

<a name="CreateAttachments"> </a>
##添付ファイルを作成する
メッセージの添付ファイルまたは[アイテムの添付ファイルを作成](#CreateItemAttachment)できます。

REST API:添付ファイルを作成する (REST)  アイテムの添付ファイルを作成する (REST)  参照添付ファイルを作成する (REST)



<a name="CreateFileAttachment"></a>
###添付ファイルを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

メッセージに添付ファイルを追加します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/attachments
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/attachments
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/attachments
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|_Body parameters_|
|@odata.type|string|`#Microsoft.OutlookServices.FileAttachment`|
|名前|string|添付ファイルの名前。|
|ContentBytes|binary|添付するファイル。|

要求本文に、**Name** と **ContentBytes** パラメーターおよび書き込み可能な[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource) プロパティを指定します。


<!--comment out until I get a working request body
[!code-REST-i[mail_api_create_file_attachment](./_data/mail_api_create_file_attachment.json)]

-->

 **応答の種類**

新しい[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)。

****


<a name="CreateItemAttachment"></a>
###アイテムの添付ファイルを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

メッセージにアイテムの添付ファイルを追加します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/attachments
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages/{message_id}/attachments
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/messages/{message_id}/attachments
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|_Body parameters_|
|@odata.type|string|```#Microsoft.OutlookServices.ItemAttachment```|
|名前|string|添付ファイルの名前。|
|アイテム|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) または [Event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) エンティティです。|添付するアイテム。|

要求本文に、**Name** と **Item** パラメーターおよび書き込み可能な[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource) プロパティを指定します。

<!--comment out until I get a working request body
[!code-REST-i[mail_api_create_file_attachment](./_data/mail_api_create_file_attachment.json)]

-->

 **応答の種類**

新しい[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)。

<!--
<a name="AddAttachments"> </a>
## Add attachments

Add a **FileAttachment** or an **ItemAttachment** to a message.

Updating attachments is not supported. Instead, delete the existing attachment and add a new one with updated values.

### Add file attachments

Add a file attachment to a message by getting the file and calling **AddAttachmentAsync**.

This example assumes you already [got the Outlook Services client](..\api\use-outlook-rest-api.md#GetClient) and [got the message ID](#GetMessages).



```cs
    FileAttachment newFileAttachment = new FileAttachment
    {
        ContentBytes = System.IO.File.ReadAllBytes(@"C:\Attachments\capture.png"),
        Name = "capture.png"
    };
    await outlookClient.Me.Messages[messageId].Attachments.AddAttachmentAsync(newFileAttachment);
```



### Add item attachments

Add message attachment to a message by getting the message and calling **AddAttachmentAsync**.

This example assumes you already [got the Outlook Services client](..\api\use-outlook-rest-api.md#GetClient) and [got the message ID](#GetMessages) of the message to add the attachment to and the message to attach.



```cs

    IMessage messageToAttach = await outlookClient.Me.Messages[messageId].ExecuteAsync();
    ItemAttachment newItemAttachment = new ItemAttachment
    {
        Item = (Message)messageToAttach
    };
    await outlookClient.Me.Messages[messageId].Attachments.AddAttachmentAsync(newItemAttachment);
```
-->

****

<a name="CreateReferenceAttachment"></a>


###参照添付ファイルを作成する (プレビュー) (REST)

<!-- ==================================== Start beta content ====================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

メッセージに参照添付ファイルを追加します。

```no-highlight
POST https://outlook.office.com/api/beta/me/messages/{message_id}/attachments
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|String|メッセージ ID。|
|_Body parameters_|
|@odata.type|String|```#Microsoft.OutlookServices.ReferenceAttachment```|
|名前|String|添付ファイルの表示名。必須。|
|SourceUrl|String | 添付ファイルの内容を取得するための URL。フォルダーへの URL の場合、Outlook または Outlook on the web 上でフォルダーが正しく表示されるには、**IsFolder** を true に設定します。必須。|

要求本文に、**Name** と**SourceUrl** パラメーターおよび書き込み可能な[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource) プロパティを指定します。



**応答の種類**

[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource)。


**要求のサンプル:**

次の例では、既存メッセージに添付ファイルを追加します。添付ファイルは、OneDrive for Business 上のファイルへのリンクです。 次の例では、既存メッセージに添付ファイルを追加します。添付ファイルは、OneDrive for Business 上のファイルへのリンクです。

```
POST https://outlook.office.com/api/beta/me/messages/AAMkAGE1Mbs88AADUv0uFAAA=/attachments
Content-Type: application/json

{ 
    "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment", 
    "Name": "Koala picture", 
    "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/koala.jpg", 
    "ProviderType": "oneDriveBusiness", 
    "Permission": "Edit", 
    "IsFolder": "False" 
} 
```


**応答のサンプル:**

```
Status code: 201 Created

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/messages('AAMkAGE1Mbs88AADUv0uFAAA%3D')/attachments/$entity",
  "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
  "Id": "AAMkAGE1Mbs88AADUv0uFAAABEgAQAPSg72tgf7hJp0PICVGCc0g=",
  "LastModifiedDateTime": "2016-03-12T06:04:38Z",
  "Name": "Koala picture",
  "ContentType": null,
  "Size": 382,
  "IsInline": false,
  "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/koala.jpg",
  "ProviderType": "oneDriveBusiness",
  "ThumbnailUrl": null,
  "PreviewUrl": null,
  "Permission": "edit",
  "IsFolder": false
}
```


**要求のサンプル**

次の例では、下書きメッセージを作成する場合と同じ呼び出しで、参照添付ファイルを追加します。添付ファイルは、OneDrive for Business 上のファイルへのリンクです。

```
POST https://outlook.office.com/api/beta/me/messages
Content-Type: application/json

{
    "Subject": "Plan for dinner",
    "Body": {
      "ContentType": "HTML",
      "Content": "Office anniversary is coming soon!"
    },
    "ToRecipients": [
      {
        "EmailAddress": {
          "Address": "randiw@contoso.onmicrosoft.com"
        }
      }
    ],
    "Attachments": [
      {
        "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment", 
        "Name": "Hydrangea picture", 
        "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/hydrangea.jpg", 
        "ProviderType": "oneDriveBusiness", 
        "Permission": "Edit", 
        "IsFolder": "False" 
      }
    ]
}
```


**応答のサンプル**

```
Status code: 201 Created

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/messages/$entity",
  "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AADZ8qi1\"",
  "Id": "AAMkAGE1Mbs88AADZ0CU9AAA=",
  "CreatedDateTime": "2016-03-12T09:04:54Z",
  "LastModifiedDateTime": "2016-03-12T09:04:54Z",
  "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AADZ8qi1",
  "Categories": [ ],
  "ReceivedDateTime": "2016-03-12T09:04:54Z",
  "SentDateTime": "2016-03-12T09:04:54Z",
  "HasAttachments": true,
  "InternetMessageId": "<BL2SR0101MB00188944566BDECE6EDE57F384B60@BL2SR0101MB0018.namsdf01.sdf.exchangelabs.com>",
  "Subject": "Plan for dinner",
  "Body": {
    "ContentType": "html",
    "Content": "<html>\r\n<body>\r\nOffice anniversary is coming soon!\r\n</body>\r\n</html>\r\n"
  },
  "BodyPreview": "Office anniversary is coming soon!",
  "Importance": "normal",
  "ParentFolderId": "AQMkAGE1MQN7j5uzzwAAAIBDwAAAA==",
  "Sender": null,
  "From": null,
  "ToRecipients": [
    {
      "EmailAddress": {
      "Name": "Randi Welch",
      "address": "randiw@contoso.onmicrosoft.com"
      }
    }
  ],
  "CcRecipients": [ ],
  "BccRecipients": [ ],
  "ReplyTo": [ ],
  "ConversationId": "AAQkAGE1MMAAQAJk0cqqggzpKtIHErqyDkcU=",
  "ConversationIndex": "AQHRfD4+mTRyqqCDOkq0gcSurIORxQ==",
  "IsDeliveryReceiptRequested": false,
  "IsReadReceiptRequested": false,
  "IsRead": true,
  "IsDraft": true,
  "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1Mbs88AADZ0CU9AAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
  "InferenceClassification": "focused",
  "UnsubscribeData": [ ],
  "UnsubscribeEnabled": false,
  "Flag": { "flagStatus": "notFlagged" },
  "Attachments@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/messages('AAMkAGE1Mbs88AADZ0CU9AAA%3D')/attachments",
  "Attachments": [
    {
      "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
      "Id": "AAMkAGE1Mbs88AADZ0CU9AAABEgAQAGe4H1iqXwtLsrCCLLkDxqo=",
      "LastModifiedDateTime": null,
      "Name": "Hydrangea picture",
      "ContentType": null,
      "Size": 0,
      "IsInline": false,
      "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/hydrangea.jpg",
      "ProviderType": "oneDriveBusiness",
      "ThumbnailUrl": null,
      "PreviewUrl": null,
      "Permission": "edit",
      "IsFolder": false
    }
  ]
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End beta content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版でのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版でのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



****


<a name="DeleteAttachments"> </a>
##添付ファイルを削除する

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Delete the specified attachment of a message. メッセージの指定した添付ファイルを削除します。添付ファイルは、[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)、[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)、または[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource)にすることができます。

```no-highlight
DELETE https://outlook.office.com/api/beta/me/messages/{message_id}/attachments/{attachment_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|attachment_id|string|添付ファイル ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/beta/me/messages/AAMkAGE0Mz8S-AAA=/attachments/AAMkAGE0Mg67gL7o=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Delete the specified attachment of a message. メッセージの指定した添付ファイルを削除します。添付ファイルは、[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)にすることができます。

```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/messages/{message_id}/attachments/{attachment_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|attachment_id|string|添付ファイル ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/v2.0/me/messages/AAMkAGE0Mz8S-AAA=/attachments/AAMkAGE0Mg67gL7o=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

Delete the specified attachment of a message. メッセージの指定した添付ファイルを削除します。添付ファイルは、[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)にすることができます。

```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/messages/{message_id}/attachments/{attachment_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|message_id|string|メッセージ ID。|
|attachment_id|string|添付ファイル ID。|

[!code-REST-i[mail_api_delete_attachment](./_data/mail_api_delete_attachment.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<!--
<a name="DeleteAttachments"> </a>
## Delete attachments

You can delete an attachment by calling **DeleteAsync** on a **FileAttachment** or **ItemAttachment**.


This example assumes you already [got the Outlook Services client](..\api\use-outlook-rest-api.md#GetClient) and [got the message ID](#GetMessages).



```cs

    //No ToFileAttachment or Attachment.ExecuteAsync
    Attachment attachmentToDelete = await outlookClient.Me.Messages[messageId].
        Attachments[attachmentId].ToFileAttachment().ExecuteAsync();
    await attachmentToDelete.DeleteAsync();

    IMessage message = await outlookClient.Me.Messages[messageId].ExecuteAsync();
    IPagedCollection<IAttachment> attachmentsResults = message.Attachments;
    List<IAttachment> attachmentsPage = attachmentsResults.CurrentPage.ToList();

    // Delete the first attachment in the collection.
    Attachment attachmentToDelete = (Attachment)attachmentsPage[0];
    await attachmentToDelete.DeleteAsync();
```
-->



****


<a name="GetFolders"> </a>
##フォルダーを取得する

ユーザーのメールボックス内のフォルダーのコレクションまたはフォルダーを取得できます。

REST API:フォルダーのコレクションを取得する (REST)  フォルダーを取得する (REST)

クライアント ライブラリ:[フォルダーまたはフォルダーのコレクションを取得する (クライアント)](#GetFoldersClient)


<a name="GetFolderCollection"> </a>
###フォルダーのコレクションを取得する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_



<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

サインインしているユーザーのメールボックス内のすべてのメール フォルダーを取得する (.../me/MailFolders`.../me/MailFolders`) か、指定されたフォルダーからフォルダー コレクションを取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/MailFolders
GET https://outlook.office.com/api/beta/me/MailFolders/{folder_id}/childfolders
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|フォルダーの ID、または既知のフォルダー名 Inbox、Drafts、SentItems、または DeletedItems (特定のフォルダーからフォルダーを取得する場合) です。|


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/MailFolders
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailFolders",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEKAAA=')",
            "Id": "AAMkAGI2AAEKAAA=",
            "DisplayName": "Deleted Items",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 1,
            "WellKnownName": "deleteditems"
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEPAAA=')",
            "Id": "AAMkAGI2AAEPAAA=",
            "DisplayName": "Drafts",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 0,
            "WellKnownName": "drafts"
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEMAAA=')",
            "Id": "AAMkAGI2AAEMAAA=",
            "DisplayName": "Inbox",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 6,
            "TotalItemCount": 6,
            "WellKnownName": "inbox"
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEeAAA=')",
            "Id": "AAMkAGI2AAEeAAA=",
            "DisplayName": "Junk Email",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 0,
            "WellKnownName": "junkemail"
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAELAAA=')",
            "Id": "AAMkAGI2AAELAAA=",
            "DisplayName": "Outbox",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 0,
            "WellKnownName": "outbox"
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEJAAA=')",
            "Id": "AAMkAGI2AAEJAAA=",
            "DisplayName": "Sent Items",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 3,
            "WellKnownName": "sentitems"
        }
    ]
}

```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

サインイン中のユーザーの既定の連絡先フォルダーから連絡先フォルダーのコレクションを取得する (.../me/contactfolders`.../me/MailFolders`) か、指定した連絡先フォルダーから取得します。 サインイン中のユーザーのルート フォルダーからフォルダー コレクションを取得する (.../me/folders`.../me/MailFolders`) か、指定したフォルダーから取得します。.../me/folders ショートカットを使用して、最上位フォルダーのコレクションを取得して、別のフォルダーに移動することができます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/MailFolders
GET https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}/childfolders
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|フォルダーの ID、または既知のフォルダー名 Inbox、Drafts、SentItems、または DeletedItems (特定のフォルダーからフォルダーを取得する場合) です。|

**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/MailFolders
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/MailFolders",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEKAAA=')",
            "Id": "AAMkAGI2AAEKAAA=",
            "DisplayName": "Deleted Items",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 1
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEPAAA=')",
            "Id": "AAMkAGI2AAEPAAA=",
            "DisplayName": "Drafts",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 0
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEMAAA=')",
            "Id": "AAMkAGI2AAEMAAA=",
            "DisplayName": "Inbox",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 6,
            "TotalItemCount": 6
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEeAAA=')",
            "Id": "AAMkAGI2AAEeAAA=",
            "DisplayName": "Junk Email",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 0
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAELAAA=')",
            "Id": "AAMkAGI2AAELAAA=",
            "DisplayName": "Outbox",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 0
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEJAAA=')",
            "Id": "AAMkAGI2AAEJAAA=",
            "DisplayName": "Sent Items",
            "ParentFolderId": "AAMkAGI2AAEIAAA=",
            "ChildFolderCount": 0,
            "UnreadItemCount": 0,
            "TotalItemCount": 3
        }
    ]
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

サインイン中のユーザーの既定の連絡先フォルダーから連絡先フォルダーのコレクションを取得する (.../me/contactfolders`.../me/folders`) か、指定した連絡先フォルダーから取得します。 サインイン中のユーザーのルート フォルダーからフォルダー コレクションを取得する (.../me/folders`.../me/folders`) か、指定したフォルダーから取得します。.../me/folders ショートカットを使用して、最上位フォルダーのコレクションを取得して、別のフォルダーに移動することができます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/folders
GET https://outlook.office.com/api/v1.0/me/folders/{folder_id}/childfolders
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|フォルダーの ID、または既知のフォルダー名 Inbox、Drafts、SentItems、または DeletedItems (特定のフォルダーからフォルダーを取得する場合) です。|

[!code-REST-i[mail_api_get_folders](./_data/mail_api_get_folders.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

要求された[フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)のコレクションです。

****


<a name="GetFolder"> </a>
###フォルダーを取得する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.read_
- _wl.imap_

ID でフォルダーを取得します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/MailFolders/{folder_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/MailFolders/inbox
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailFolders/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEMAAA=')",
    "Id": "AAMkAGI2AAEMAAA=",
    "DisplayName": "Inbox",
    "ParentFolderId": "AAMkAGI2AAEIAAA=",
    "ChildFolderCount": 0,
    "UnreadItemCount": 6,
    "TotalItemCount": 6,
    "WellKnownName": "inbox"
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/MailFolders/inbox
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/MailFolders/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGI2AAEMAAA=')",
    "Id": "AAMkAGI2AAEMAAA=",
    "DisplayName": "Inbox",
    "ParentFolderId": "AAMkAGI2AAEIAAA=",
    "ChildFolderCount": 0,
    "UnreadItemCount": 6,
    "TotalItemCount": 6
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/folders/{folder_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|

[!code-REST-i[mail_api_get_folder_by_id](./_data/mail_api_get_folder_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

要求された[フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)です。

****

<a name="GetFoldersClient"> </a>
###フォルダーまたはフォルダーのコレクションを取得する (クライアント)

Get the top-level folders in the mailbox by using the `Me.Folders` shortcut property. To get the folders from a specific folder, use its **ChildFolders** property.
You can use the following well-known folder names instead of the ID for the corresponding folder: `Inbox`, `SentItems`, `Drafts`, `DeletedItems`.

例`outlookClient.Me.Folders["Drafts"].ChildFolders.ExecuteAsync()`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


特定のフォルダーを取得するには、**フォルダー** コレクションのインデックスとしてフォルダー ID を指定するか、**GetById** メソッドを使用します。

**メモ**: フォルダー コレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。

この例では、[Outlook サービス クライアントを取得する](..\api\use-outlook-rest-api.md#GetClient)メソッドを呼び出します。

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Mail" -->

<!--tested-->
```cs-i
var outlookClient = await CreateOutlookClientAsync("Mail");
var mailFolders = await outlookClient.Me.Folders
  .Take(10)
  .ExecuteAsync();

foreach(var mailFolder in mailFolders.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine("Mail folder '{0}'.", mailFolder.DisplayName);
}

```
```javascript-i
outlookClient.me.folders.getFolders().fetchAll(100).then(function (result) {
    result.forEach(function (folder) {
        console.log('Folder "' + folder.displayName + '"');
    });
}, function (error) {
    console.log(error);
});
```

<!-- ENDSECTION -->


****

<a name="SynchronizeFolders"></a>
##フォルダー階層を同期する (プレビュー)

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

メールボックス内のすべてのフォルダーのフラットなテーブルを取得できます。メール フォルダー階層を同期するときは、このカテゴリを要求します。 メールボックス内のすべてのフォルダーのフラットなテーブルを取得できます。メール フォルダー階層を同期するときは、このカテゴリを要求します。

|**エンドポイント**|**フォルダー カテゴリ**|
|:-----|:-----|
| Me/MailFolders | メール フォルダー |

各フォルダー カテゴリの最上位レベルのみを同期できます。たとえば、以下のように要求できます。 For example, you can request _Me/MailFolders_ but not _Me/MailFolders('inbox')_.

同期では、階層内のすべてのフォルダーを取得する完全な同期と、最後の完全な同期から変更されたすべてのフォルダーを取得する差分同期の両方がサポートされています。 

__**Minimum required scope**:__

|**フォルダー階層**|**アクセス許可**|
|:-----|:-----|
|Me/Folders|_https://outlook.office.com/mail.read_ または wl.imap|

```no-highlight
GET https://outlook.office365.com/api/beta/me/MailFolders
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_Header parameter_|
|Prefer|odata.trackchanges|要求が同期要求であることを示します。|
|_Body parameters_|
|odata.deltaLink|string|前回フォルダー階層が同期されたことを示すトークン。|

$filter、$orderby、$search、$top のクエリ パラメーターは、どれが要求に含まれていても無視されます。

**応答の種類**

要求されたカテゴリ内のフォルダーのフラット リストです。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版でのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版でのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****

<a name="CreateFolders"> </a>
##フォルダーを作成する

新しいフォルダーをフォルダーのコレクションに追加します。

REST API:[フォルダーを作成する (REST)](#CreateAFolder)

クライアント ライブラリ:[フォルダーを作成する (クライアント)](#CreateFoldersClient)

<a name="CreateAFolder"> </a>
###フォルダーを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

**DisplayName に指定した名前で子フォルダーを作成します。DisplayName は、フォルダー**の唯一の書き込み可能なプロパティです。 フォルダー名を変更します。DisplayName は、フォルダーの唯一の書き込み可能なプロパティです。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/MailFolders/{folder_id}/childfolders
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DisplayName|string|フォルダーの表示名です。|


**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/MailFolders/inbox/childfolders
Content-Type: application/json

{
  "DisplayName": "Company"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailFolders('inbox')/ChildFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-l_AAA=')",
  "Id": "AAMkAGE0Mz-l_AAA=",
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "DisplayName": "Company",
  "ChildFolderCount": 0,
  "UnreadItemCount": 2,
  "TotalItemCount": 27,
  "WellKnownName": ""
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}/childfolders
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DisplayName|string|フォルダーの表示名です。|

**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/MailFolders/inbox/childfolders
Content-Type: application/json

{
  "DisplayName": "Company"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/MailFolders('inbox')/ChildFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-l_AAA=')",
  "Id": "AAMkAGE0Mz-l_AAA=",
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "DisplayName": "Company",
  "ChildFolderCount": 0,
  "UnreadItemCount": 2,
  "TotalItemCount": 27
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/folders/{folder_id}/childfolders
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DisplayName|string|フォルダーの表示名です。|

[!code-REST-i[mail_api_create_folder](./_data/mail_api_create_folder.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

新しい[フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)です。

**注釈**

You can't create a top-level folder. 最上位フォルダーを作成することはできません。フォルダーを追加できるのは、childfolders`childfolders` エンドポイントのみです。

****

<a name="CreateFoldersClient"> </a>
###フォルダーを作成する (クライアント)

**Folder** オブジェクトを作成し、それを宛先フォルダーの **ChildFolders** コレクションの **AddFolderAsync** メソッドに渡します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と親フォルダーの[フォルダー ID](#GetFolders) が既に取得されていると仮定します。


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
Folder newFolder = new Folder
{
    DisplayName = "Private"
};
await outlookClient.Me.Folders["Inbox"].ChildFolders.AddFolderAsync(newFolder);

// Get the ID of the new folder.
string folderId = newFolder.Id;
```

<!-- ENDSECTION -->


****


<a name="UpdateFolders"> </a>
##フォルダーを更新する

フォルダー名を変更します。

REST API:[フォルダーを更新する (REST)](#UpdateAFolder)

クライアント ライブラリ:[フォルダーを更新する (クライアント)](#UpdateFoldersClient)

<a name="UpdateAFolder"> </a>
###フォルダーを更新する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

フォルダー名を **DisplayName で指定したものに変更します。この名前は、フォルダー**の唯一の書き込み可能なプロパティです。 フォルダー名を DisplayName で指定したものに変更します。この名前は、[フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)の唯一の書き込み可能なプロパティです。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
PATCH https://outlook.office.com/api/beta/me/MailFolders/{folder_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DisplayName|string|フォルダーの新しい表示名です。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/beta/me/MailFolders/AAMkAGE0Mz-l_AAA=
Content-Type: application/json

{
  "DisplayName": "Business"
}
```

**応答のサンプル:**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-l_AAA=')",
  "Id": "AAMkAGE0Mz-l_AAA=",
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "DisplayName": "Business",
  "ChildFolderCount": 0,
  "UnreadItemCount": 4,
  "TotalItemCount": 38,
  "WellKnownName": ""
}

```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DisplayName|string|フォルダーの新しい表示名です。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/v2.0/me/MailFolders/AAMkAGE0Mz-l_AAA=
Content-Type: application/json

{
  "DisplayName": "Business"
}
```

**応答のサンプル:**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/MailFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-l_AAA=')",
  "Id": "AAMkAGE0Mz-l_AAA=",
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "DisplayName": "Business",
  "ChildFolderCount": 0,
  "UnreadItemCount": 4,
  "TotalItemCount": 38
}

```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v1.0/me/folders/{folder_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DisplayName|string|フォルダーの新しい表示名です。|

[!code-REST-i[mail_api_update_folder_by_id](./_data/mail_api_update_folder_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

更新された[フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)です。

****

<a name="UpdateFoldersClient"> </a>
###フォルダーを更新する (クライアント)

フォルダー名を変更します。 フォルダー名を変更します。**DisplayName** は、フォルダーの唯一の書き込み可能なプロパティです。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[フォルダー ID](#GetFoldersClient) が既に取得されていると仮定します。


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
folder.DisplayName = "Personal";
await folder.UpdateAsync();

// Get the updated property.
string updatedName = folder.DisplayName;
```

<!-- ENDSECTION -->

次のパターンを使用して、クライアント側で複数の更新を定義し、要求をまとめて (バッチ処理で) 送信できます。
1. Call `UpdateAsync(true)` for each entity you want to update. 更新するエンティティごとに UpdateAsync(true)`true` を呼び出します。true を指定すると、更新がクライアント上でローカルに登録されますが、サーバーには投稿されません。
2. outlookClient.Context.SaveChangesAsync()`outlookClient.Context.SaveChangesAsync()` を呼び出して、ローカルに登録されているすべての更新を投稿します。

****


<a name="DeleteFolders"> </a>
##フォルダーを削除する

フォルダーとそのすべての内容を削除します。

**注** フォルダーを削除するときには注意してください。削除した内容を回復できない可能性があります。 注 フォルダーを削除するときには注意してください。削除した内容を回復できない可能性があります。
詳細については、「アイテムの削除」を参照してください。

REST API:[フォルダーを削除する (REST)](#DeleteAFolder)

クライアント ライブラリ:[フォルダーを削除する (クライアント)](#DeleteFoldersClient)

<a name="DeleteAFolder"> </a>
###フォルダーを削除する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

**folder_id** で指定したフォルダーを削除します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
DELETE https://outlook.office.com/api/beta/me/MailFolders/{folder_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
DELETE https://outlook.office.com/api/BETA/me/MailFolders/AAMkAGE0Mz-l_AAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
DELETE https://outlook.office.com/api/v2.0/me/MailFolders/AAMkAGE0Mz-l_AAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/folders/{folder_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|

[!code-REST-i[mail_api_delete_folder_by_id](./_data/mail_api_delete_folder_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


****

<a name="DeleteFoldersClient"> </a>
###フォルダーを削除する (クライアント)

フォルダーを削除するには、その ID を使用して **DeleteAsync** を呼び出します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[フォルダー ID](#GetFolders) が既に取得されていると仮定します。


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
await folder.DeleteAsync();
```

<!-- ENDSECTION -->

****

<a name="MoveCopyFolders"> </a>
##フォルダーを移動またはコピーする
フォルダーを別のフォルダーに移動またはコピーできます。

REST API:フォルダーを移動する (REST)  フォルダーをコピーする (REST)

クライアント ライブラリ:[フォルダーを移動またはコピーする (クライアント)](#MoveFoldersClient)

<a name="MoveFolders"> </a>
###フォルダーを移動する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

フォルダーとその内容を別のフォルダーに移動するには、**Move** メソッドを使用します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/MailFolders/{folder_id}/move
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/MailFolders/AAMkAGE0Mz-l_AAA=/move
Content-Type: application/json

{
  "DestinationId": "AAMkAGE0MyxQ9AAA="
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-l_AAA=')",
  "Id": "AAMkAGE0Mz-l_AAA=",
  "ParentFolderId": "AAMkAGE0MyxQ9AAA=",
  "DisplayName": "Business",
  "ChildFolderCount": 0,
  "UnreadItemCount": 4,
  "TotalItemCount": 38,
  "WellKnownName": ""
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}/move
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/MailFolders/AAMkAGE0Mz-l_AAA=/move
Content-Type: application/json

{
  "DestinationId": "AAMkAGE0MyxQ9AAA="
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/MailFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-l_AAA=')",
  "Id": "AAMkAGE0Mz-l_AAA=",
  "ParentFolderId": "AAMkAGE0MyxQ9AAA=",
  "DisplayName": "Business",
  "ChildFolderCount": 0,
  "UnreadItemCount": 4,
  "TotalItemCount": 38
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/folders/{folder_id}/move
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|

[!code-REST-i[mail_api_move_folder_by_id](./_data/mail_api_move_folder_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


 **応答の種類**

移動された[フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)です。

****


<a name="CopyFolders"> </a>
###フォルダーをコピーする (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/mail.readwrite_
- _wl.imap_

フォルダーとその内容を別のフォルダーにコピーするには、**Copy** メソッドを使用します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/MailFolders/{folder_id}/copy
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/MailFolders/AAMkAGE0Mz-l_AAA=/copy
Content-Type: application/json

{
  "DestinationId": "inbox"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/MailFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-mAAAA=')",
  "Id": "AAMkAGE0Mz-mAAAA=",
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "DisplayName": "Business",
  "ChildFolderCount": 0,
  "UnreadItemCount": 4,
  "TotalItemCount": 38,
  "WellKnownName": ""
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


**要求のサンプル:**


```no-highlight
POST https://outlook.office.com/api/v2.0/me/MailFolders/{folder_id}/copy
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|


```
POST https://outlook.office.com/api/v2.0/me/MailFolders/AAMkAGE0Mz-l_AAA=/copy
Content-Type: application/json

{
  "DestinationId": "inbox"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/MailFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/MailFolders('AAMkAGE0Mz-mAAAA=')",
  "Id": "AAMkAGE0Mz-mAAAA=",
  "ParentFolderId": "AAMkAGE0MAAEMAAA=",
  "DisplayName": "Business",
  "ChildFolderCount": 0,
  "UnreadItemCount": 4,
  "TotalItemCount": 38
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/folders/{folder_id}/copy
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|folder_id|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|
|_Body parameters_|
|DestinationId|string|宛先フォルダーの ID、あるいは Inbox または Drafts の既知のフォルダー名です。|

[!code-REST-i[mail_api_copy_folder_by_id](./_data/mail_api_copy_folder_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

**応答の種類**

[フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)の新しいコピーです。

****

<a name="MoveFoldersClient"> </a>
###フォルダーを移動またはコピーする (クライアント)

フォルダーを移動またはコピーするには、宛先フォルダーの ID を **MoveAsync** または **CopyAsync** メソッドに渡します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と、移動するフォルダーおよび宛先フォルダーの[フォルダー ID](#GetFoldersClient) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
await folder.MoveAsync("AAMkADE3N...");
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
await folder.CopyAsync("Inbox");
```

<!-- ENDSECTION -->

****

<a name="NextSteps"> </a>
## 次のステップ

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](http://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)

- [予定表 API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API](..\api\task-rest-operations.md) (プレビュー)

- [OneDrive API](https://dev.onedrive.com/)

- [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)


[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]


