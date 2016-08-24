---
ms.Toctitle: Outlook Notifications REST API reference
title: "Outlook 通知 REST API リファレンス"
description: "ms.TocTitle:Outlook 通知 REST API リファレンスTitle:Outlook 通知 REST API リファレンスDescription:Office 365 と Outlook.com の通知 REST API を使用し、イベントの場合にサービスによって送信されるプッシュ通知をサブスクライブしてリッスンする方法に関するリファレンスです。ms.ContentId: c324d0f3-b093-44d3-8ef7-6ff810f9c99ams.topic: リファレンス (API) ms.date:2016 年 5 月 16 日"
ms.ContentId: c324d0f3-b093-44d3-8ef7-6ff810f9c99a
ms.date: August 9, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Outlook 通知 REST API リファレンス

[!INCLUDE [Add the Outlook REST API filters--v2 default](../includes/controls/addOutlookversion_v2default_v1disabled.xml)]


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<p class="previewnote"><p class="previewnote">このバージョンのドキュメントは、プレビュー版の通知 API について説明します。 Preview features are subject to change prior to finalization, and may break code that uses them. プレビュー機能は、最終処理までに変更される場合があります。それらを使用するコードを破棄する場合もあります。このため、一般に、実稼働コードでは実稼働バージョンの API のみを使用してください。可能な場合には、現在バージョン 2.0 が優先バージョンです。</p> If available, v2.0 is currently the preferred version.</p>


_**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_


Outlook 通知 REST API では、Office 365 の Azure Active Directory により保護されるユーザーのメール、予定表、連絡先、またはタスクのデータ、および Microsoft アカウントの類似したデータに関する変更を、アプリが学習できるようにします。具体的には、次のドメインです。Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com。 

**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

**Not interested in the beta version of the API?** API v2.0 が不要な場合右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


_**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_


Outlook 通知 REST API では、Office 365 の Azure Active Directory により保護されるユーザーのメール、予定表、または連絡先データ、および Microsoft アカウントの類似したデータに関する変更を、アプリが学習できるようにします。具体的なドメインは、次のとおりです。Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com。 

**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

**Not interested in v2.0 of the API?** API v1.0 に関心がない場合: 右上のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->






## 概要


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

The Outlook notifications service and API use webhooks to deliver notifications to client apps. Webhooks are HTTP callbacks configured by usually a trusted, third-party backend web service. The service can configure webhooks to cause triggering events on one site to invoke behavior on another. When an app subscribes to notifications of a specific resource (such as messages in the user's Inbox), it specifies a webhook callback URL in the subscription request.  When a triggering event occurs (such as a new message in the Inbox), Office 365 pushes a notification via a webhook to the callback URL. The app, in turn, takes actions according to its business logic, such as getting the new message and updating the unread count.

Webhook の利用は、要求されたリソースについて、クライアント アプリケーションに変更を通知し続けるための効果的な方法です。これにより、変更をポーリングする必要がなくなり、クライアントが更新をほぼ即座に利用できるようになります。メール、予定表、CRM アプリは通常、通知を使用して、ローカル キャッシュ、対応するクライアントのビュー、またはバックエンドシステムで変更を更新します。 Mail, calendaring and CRM apps typically use notifications to update their local cache, corresponding client views, or backend system upon changes. 

アプリは、期限が切れる前にサブスクリプションを更新する必要があります。また、サブスクリプションを中止して、通知の受信をいつでも停止することができます。 アプリは、期限が切れる前にサブスクリプションを更新する必要があります。また、サブスクリプションを中止して、通知の受信をいつでも停止することができます。

<!-- Remove this image as confirmed by Michael that it's not correct. 
The following figure describes this notification method.

![Client application is redirected to login from backend server, provides consent, backend server subscribes for notifications, notifications are sent to the client application](images\O365APIs_REST_Notifications.png)
1.  Client application is opened. A request goes to the trusted backend server which has already been given app-only access to Microsoft Azure to get an access token to act on behalf of the client application user.
2.  The backend server subscribes for notifications from Office 365 using the access token.
3.  As changes are made to mail, contacts, and events, Office 365 pushes notifications to the backend server.
4.  The backend server relays the changes to the client application through a notification service.
-->


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


The Outlook notifications service and API use webhooks to deliver notifications to client apps. Webhooks are HTTP callbacks configured by usually a trusted, third-party backend web service. The service can configure webhooks to cause triggering events on one site to invoke behavior on another. When an app subscribes to notifications of a specific resource (such as messages in the user's Inbox), it specifies a webhook callback URL in the subscription request.  When a triggering event occurs (such as a new message in the Inbox), Office 365 pushes a notification via a webhook to the callback URL. The app, in turn, takes actions according to its business logic, such as getting the new message and updating the unread count.

Webhook の利用は、要求されたリソースについて、クライアント アプリケーションに変更を通知し続けるための効果的な方法です。これにより、変更をポーリングする必要がなくなり、クライアントが更新をほぼ即座に利用できるようになります。メール、予定表、CRM アプリは通常、通知を使用して、ローカル キャッシュ、対応するクライアントのビュー、またはバックエンドシステムで変更を更新します。 Mail, calendaring and CRM apps typically use notifications to update their local cache, corresponding client views, or backend system upon changes. 

アプリは、期限が切れる前にサブスクリプションを更新する必要があります。また、サブスクリプションを中止して、通知の受信をいつでも停止することができます。 アプリは、期限が切れる前にサブスクリプションを更新する必要があります。また、サブスクリプションを中止して、通知の受信をいつでも停止することができます。

<!-- Remove this image as confirmed by Michael that it's not correct. 
The following figure describes this notification method.

![Client application is redirected to login from backend server, provides consent, backend server subscribes for notifications, notifications are sent to the client application](images\O365APIs_REST_Notifications.png)
1.  Client application is opened. A request goes to the trusted backend server which has already been given app-only access to Microsoft Azure to get an access token to act on behalf of the client application user.
2.  The backend server subscribes for notifications from Office 365 using the access token.
3.  As changes are made to mail, contacts, and events, Office 365 pushes notifications to the backend server.
4.  The backend server relays the changes to the client application through a notification service.
-->


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


## 通知 REST API の使用

### 認証

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the Outlook Notifications API, you should include a valid access token. アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you.
Keep this in mind as you proceed with the specific operations in the Notifications API.


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the Outlook Notifications API, you should include a valid access token. アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you.
Keep this in mind as you proceed with the specific operations in the Notifications API.


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


###API のバージョン


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

この API は、プレビューから一般提供 (GA) の状態にレベル上げされています。Outlook REST API の v2.0 とベータのバージョンでサポートされます。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この API は、プレビューから一般提供 (GA) の状態にレベル上げされています。Outlook REST API の v2.0 とベータのバージョンでサポートされます。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


###対象ユーザー


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

通知 API 要求は、常に現在のユーザーのために実行されます。 

Outlook REST API のすべてのサブセットに共通な情報について詳しくは、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。

****

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

通知 API 要求は、常に現在のユーザーのために実行されます。 

Outlook REST API のすべてのサブセットに共通な情報について詳しくは、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。

****

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<a name="NotificationOperations"> </a>
##通知の操作<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

[Subscribe to notifications](#SubscribeOperation) | [Renew subscription](#RenewSub) | [Delete subscription](#DeleteSub)

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

[Subscribe to notifications](#SubscribeOperation) | [Renew subscription](#RenewSub) | [Delete subscription](#DeleteSub)

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->









<a name="SubscribeOperation"> </a>
###自分のメール、予定表、連絡先、およびタスクの変更のサブスクライブ<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


リスナー サービスをサブスクライブして、Office 365 または Outlook.com でメール、予定表イベント、連絡先、またはタスクが変更されたときに通知を受信します。


```no-highlight
POST https://outlook.office.com/api/beta/me/subscriptions
```

__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _https://outlook.office.com/tasks.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


サブスクライブは、クライアントがリソース (エンティティまたはエンティティのコレクション) の通知の取得を開始する最初のステップです。 

リソースには、以下が含まれています。
- メッセージ、イベント、連絡先、またはタスクの通常のフォルダーまたは検索フォルダー。例: As examples: 
  
  `https://outlook.office.com/api/beta/me/mailfolders('inbox')/messages`
  
  `https://outlook.office.com/api/beta/me/taskfolders('{folder_id}')/tasks`
   
- または、メッセージ、イベント、連絡先、またはタスクのトップ レベルのエンティティのコレクション。
  
  `https://outlook.office.com/api/beta/me/messages`
  
  `https://outlook.office.com/api/beta/me/events`
  
  `https://outlook.office.com/api/beta/me/contacts`
  
  `https://outlook.office.com/api/beta/me/tasks`


**Subscription process** 

サブスクリプション プロセスは、次のようになります。 

1. クライアント アプリが、特定のリソースのサブスクリプション要求 (POST) を行います。 It includes a notification URL, among other properties.

2. The Outlook notifications service tries to validate the notification URL with the listener service. It includes a validation token in the validation request.

3. If the listener service successfully validates the URL, it returns a success response within 5 seconds as follows:

   - 応答ヘッダーのコンテンツ タイプを text\plain に設定します。 
   - 応答の本文の検証トークンのみが含まれます。 
   - Returns an HTTP 200 response code. The listener can discard the validation token subsequently. 

4. Depending on the URL validation result, the Outlook notifications service sends a response to the client app:

   - URL の検証が成功した場合は、一意なサブスクリプション ID を使用してサブスクリプションを作成し、クライアントへの応答を送信します。 
   - URL の検証が正常に終了しない場合は、エラー コードとその他の詳細を含むエラー応答を送信します。

5. 正常な応答を受信するには、クライアント アプリケーションが、このサブスクリプションでの今後の通知に関連付けるためのサブスクリプション ID を格納する必要があります。 


**サブスクリプション要求** 

サブスクリプション要求に必要なプロパティを次に示します。詳細については、「通知エンティティ」を参照してください。 For more information, see [Notification Entities](#NotificationEntities). 

- odata.type - Include `"@odata.type":"#Microsoft.OutlookServices.PushSubscription"`. The **PushSubscription** entity defines **NotificationURL**.
- **Resource** - Specifies the resource to monitor and receive notifications on. 
- **NotificationURL** - Specifies where notifications should be sent to. 
- **ChangeType** - Specifies the types of events to monitor for that resource. 以下のいずれかの値を使用できます。 

In addition, the optional **ClientState** property indicates that each notification should be sent with a header by the same name **ClientState** value. This lets the listener check the legitimacy of each notification. 


**要求のサンプル:**

The following example shows how to subscribe to new events. 

```
POST https://outlook.office.com/api/beta/me/subscriptions HTTP/1.1
Content-Type: application/json

{
   "@odata.type":"#Microsoft.OutlookServices.PushSubscription",
   "Resource": "https://outlook.office.com/api/beta/me/events",
   "NotificationURL": "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",  
   "ChangeType": "Created",
   "ClientState": "c75831bd-fad3-4191-9a66-280a48528679"
}
```


**Refining notification conditions**

You can further refine the conditions for a notification by using the `$filter` query parameter. 

The following example requests a notification for a message being created in the Drafts folder, containing one or more attachments, and importance being `High`: 

```
POST https://outlook.office.com/api/beta/me/subscriptions HTTP/1.1 
Content-Type: application/json 
 
{ 
  @odata.type:"#Microsoft.OutlookServices.PushSubscription", 
  Resource: "https://outlook.office.com/api/beta/me/mailfolders('Drafts')/messages?$filter=HasAttachments%20eq%20true%20AND%20Importance%20eq%20%27High%27", 
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient", 
  ChangeType: "Created", 
  ClientState: "Has attachments and high importance" 
} 
```


The following example requests a notification for an all-day event being created: 

```
POST https://outlook.office.com/api/beta/me/subscriptions HTTP/1.1 
Content-Type: application/json 
 
{ 
  @odata.type:"#Microsoft.OutlookServices.PushSubscription", 
  Resource: "https://outlook.office.com/api/beta/me/events?$filter=IsAllDay%20eq%20true", 
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient", 
  ChangeType: "Created", 
  ClientState: "Notifications for events that IsAllDay." 
} 
```


The following example requests a notification for a contact being created with the company being Contoso: 

```
POST https://outlook.office.com/api/beta/me/subscriptions HTTP/1.1 
Content-Type: application/json 
 
{ 
  @odata.type:"#Microsoft.OutlookServices.PushSubscription", 
  Resource: "https://outlook.office.com/api/beta/me/contacts?$filter=CompanyName%20eq%20%27Contoso%27", 
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient", 
  ChangeType: "Created", 
  ClientState: "Contacts in Contoso." 
} 
```

One common application of `$filter` is to get notification on a change in a specific property of the specified resource.
For example, you can use `$filter` to subscribe to unread messages in a folder (the **IsRead** property is false), and include all the change types:

- A message added to or marked unread in the folder would trigger a `Created` notification. 
- Reading a message or marking it as read in the folder would trigger a `Deleted` notification.
- A change to any property, other than **IsRead**, of a message in the folder would trigger an `Updated` notification.

You can create such a subscription as follows:

```
POST https://outlook.office.com/api/beta/me/subscriptions HTTP/1.1
Content-Type: application/json

{
  @odata.type:"#Microsoft.OutlookServices.PushSubscription",
  Resource: "https://outlook.office.com/api/beta/me/mailfolders('folder_id')/messages$filter=IsRead%20eq%20false",
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",
  ChangeType: "Created,Deleted,Updated",
  ClientState: "Message unread"
}
```


If you don’t use a `$filter` when creating the subscription (as below):

- Adding a message to the folder would result in a `Created` notification.
- Reading a message in the folder, marking the message as read,or changing any other message property of a message in that folder would result in an `Updated` notification. 
- Deleting the message would result in a `Delete` notification.

```
POST https://outlook.office.com/api/beta/me/subscriptions HTTP/1.1
Content-Type: application/json

{
  @odata.type:"#Microsoft.OutlookServices.PushSubscription",
  Resource: "https://outlook.office.com/api/beta/me/mailfolders('folder_id')/messages,
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",
  ChangeType: "Created,Deleted,Updated",
  ClientState: "Message unread"
}
```

**評価の要求**

検証要求は、サブスクリプション要求でクライアント アプリケーションが指定する NotificationURL を検証しようとします。
```no-highlight
POST https://{notificationUrl}?validationToken={token}
```
The Outlook service specifies the NotificationURL in the subscription request in {notificationUrl}, and defines a string {token} as the validation token. Outlook のサービスでは、サブスクリプション要求の NotificationURL を{notificationURL} で指定し、文字列を {token} として定義します。クライアント アプリがサブスクリプション要求に **ClientState** ヘッダーを含める場合、Outlook サービスもそのヘッダーを含めます。


**サブスクリプション応答**

サブスクリプション応答には、要求のプロパティ、および次の追加のプロパティが含まれています。
- **Id**  – 今後の通知と一致するようにクライアント アプリが保存する必要がある一意なサブスクリプション ID。
- **ChangeType** -要求で指定された値とは別に、追加の通知タイプである  
- **SubscriptionExpirationDateTime** - サブスクリプションの有効期限が切れる日付と時刻。 SubscriptionExpirationDateTime - サブスクリプションの有効期限が切れる日付と時刻。要求に有効期限の時刻を指定しない場合、または要求が 3 日間よりも長い有効期限を指定している場合、応答は要求が送信された時刻から 3 日の最大値を設定します。
- 他の OData 関連のプロパティ。


**応答データのサンプル**

この応答では、リスナー サービスが新しいイベントと失敗した変更の通知の受信を想定していることを示します。変更に失敗した場合、クライアントはその変更をキャプチャするためにサービスと同期する必要があります。

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Subscriptions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.PushSubscription",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Subscriptions('Mjk3QNERDQQ==')",
    "Id": "Mjk3QNERDQQ==",
    "Resource": "https://outlook.office.com/api/beta/me/events",
    "ChangeType": "Created, Missed",
    "ClientState": "c75831bd-fad3-4191-9a66-280a48528679",
    "NotificationURL": "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",
    "SubscriptionExpirationDateTime": "2015-04-23T22:46:13.8805047Z"
}
```


サインインしているユーザーの既存のサブスクリプションのいずれかに関する詳細情報を検索するには、そのサブスクリプション ID を指定します。たとえば、最後の例で作成したサブスクリプションのクエリを実行するには、次のようにします。 As an example, to query the subscription created in the last example:
```
GET https://outlook.office.com/api/beta/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Subscriptions('Mjk3QNERDQQ==')
```
成功すると、クライアント アプリケーションが潜在的なセキュリティ リスクを回避するために指定した ClientState を除外するという点を除き、応答はサンプルの最後の応答データと同じです。  
the client app has specified to avoid potential security risks. 


**通知**

各通知には、その種類に関係なく、次のプロパティが含まれています。
- **ClientState** ヘッダー - クライアントがサブスクリプション要求で **ClientState** プロパティを指定した場合にのみ存在します。通知の正当性を確認するために、リスナーによって使用されます。
- **SubscriptionId** - この通知が所属するサブスクリプションをクライアント アプリに指定します。
- **SubscriptionExpirationDateTime** - サブスクリプションの有効期限が切れる日付と時刻。 
- **SequenceNumber** - 通知が欠落していないかどうかをクライアント アプリが識別できるようにする、通知の連続した番号。
- **ChangeType** - クライアント アプリケーションに通知しようとしているイベントの種類 (たとえば、メールを受信したときの **Created** イベント、メッセージを読むときの **Update** イベント)。 
- **リソース** – 監視されている特定のリソース アイテムの URL (変更されたメッセージまたはイベントへの URL など)。 
- **ResourceData** - リソースの変更に関連する通知 (メッセージを受信する、読む、削除するなど) が、変更されたアイテムのリソース ID を含むこの追加のプロパティを持っています。クライアントはこのリソース ID を使用して、ビジネス ロジック (このアイテムをフェッチする、フォルダーを同期するなど) に従ってこのアイテムを処理できます   ResourceData - リソースの変更に関連する通知 (メッセージを受信する、読む、削除するなど) が、変更されたアイテムのリソース ID を含むこの追加のプロパティを持っています。クライアントはこのリソース ID を使用して、ビジネス ロジック (このアイテムをフェッチする、フォルダーを同期するなど) に従ってこのアイテムを処理できます 

注:[Notification](#NotificationResource) エンティティでは、Id プロパティは使用されません。

<a name="ChangeNotificationPayload"> </a>

** 変更通知のペイロード **

In the following example, as the user receives a new event, the notifications service sends a notification with ChangeType set to "Created". The header of the notification would include the ClientState as specified in the initial subscription request. The receive event notification has a payload similar to this:

```no-highlight
{
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.Notification",
            "Id": null,
            "SubscriptionId": "Mjk3QNERDQQ==",
            "SubscriptionExpirationDateTime": "2015-04-23T22:46:13.8805047Z",
            "SequenceNumber": 1,
            "ChangeType": "Created",
            "Resource" : "https://outlook.office.com/api/beta/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Events('AAMkADNkNmAA=')",
            "ResourceData": {
                "@odata.type": "#Microsoft.OutlookServices.Event",
                "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Events('AAMkADNkNmAA=')",
                "Id": "AAMkADNkNmAA="
            }
        }
    ]
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


リスナー サービスをサブスクライブして、Office 365 または Outlook.com でメール、予定表イベント、または連絡先が変更されたときに通知を受信します。

**注** タスクとその通知は、現在プレビュー版でのみ利用できます。


```no-highlight
POST https://outlook.office.com/api/v2.0/me/subscriptions
```

__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


サブスクライブは、クライアントがリソース (エンティティまたはエンティティのコレクション) の通知の取得を開始する最初のステップです。 

リソースには、以下が含まれています。
- メッセージ、イベント、または連絡先の通常のフォルダーまたは検索フォルダー。例: As examples: 
  
  `https://outlook.office.com/api/v2.0/me/mailfolders('inbox')/messages`
  
  `https://outlook.office.com/api/v2.0/me/events`
  
  `https://outlook.office.com/api/v2.0/me/contacts`
  
- あるいは、メッセージ、イベント、または連絡先のトップ レベルのエンティティのコレクション。
  
  `https://outlook.office.com/api/v2.0/me/messages`
  
  `https://outlook.office.com/api/v2.0/me/events`


**Subscription process** 

サブスクリプション プロセスは、次のようになります。 

1. クライアント アプリが、特定のリソースのサブスクリプション要求 (POST) を行います。 It includes a notification URL, among other properties.

2. The Outlook notifications service tries to validate the notification URL with the listener service. It includes a validation token in the validation request.

3. If the listener service successfully validates the URL, it returns a success response within 5 seconds as follows:

   - 応答ヘッダーのコンテンツ タイプを text\plain に設定します。 
   - 応答の本文の検証トークンのみが含まれます。 
   - Returns an HTTP 200 response code. The listener can discard the validation token subsequently. 

4. Depending on the URL validation result, the Outlook notifications service sends a response to the client app:

   - URL の検証が成功した場合は、一意なサブスクリプション ID を使用してサブスクリプションを作成し、クライアントへの応答を送信します。 
   - URL の検証が正常に終了しない場合は、エラー コードとその他の詳細を含むエラー応答を送信します。

5. 正常な応答を受信するには、クライアント アプリケーションが、このサブスクリプションでの今後の通知に関連付けるためのサブスクリプション ID を格納する必要があります。 


**サブスクリプション要求** 

サブスクリプション要求に必要なプロパティを次に示します。詳細については、「通知エンティティ」を参照してください。 For more information, see [Notification Entities](#NotificationEntities). 

- odata.type - Include `"@odata.type":"#Microsoft.OutlookServices.PushSubscription"`. The **PushSubscription** entity defines **NotificationURL**. 
- **Resource** - Specifies the resource to monitor and receive notifications on. 
- **NotificationURL** - Specifies where notifications should be sent to. 
- **ChangeType** - Specifies the types of events to monitor for that resource. 以下のいずれかの値を使用できます。 

In addition, the optional **ClientState** property indicates that each notification should be sent with a header by the same name **ClientState** value. This lets the listener check the legitimacy of each notification. 


**要求のサンプル:**

The following example shows how to subscribe to new events. 

```
POST https://outlook.office.com/api/v2.0/me/subscriptions HTTP/1.1
Content-Type: application/json

{
   "@odata.type":"#Microsoft.OutlookServices.PushSubscription",
   "Resource": "https://outlook.office.com/api/v2.0/me/events",
   "NotificationURL": "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",  
   "ChangeType": "Created",
   "ClientState": "c75831bd-fad3-4191-9a66-280a48528679"
}

```


**Refining notification conditions** 

You can further refine the conditions for a notification by using the `$filter` query parameter. 

The following example requests a notification for a message being created in the Drafts folder, containing one or more attachments, and importance being `High`: 

```
POST https://outlook.office.com/api/v2.0/me/subscriptions HTTP/1.1 
Content-Type: application/json 
 
{ 
  @odata.type:"#Microsoft.OutlookServices.PushSubscription", 
  Resource: "https://outlook.office.com/api/v2.0/me/mailfolders('Drafts')/messages?$filter=HasAttachments%20eq%20true%20AND%20Importance%20eq%20%27High%27", 
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient", 
  ChangeType: "Created", 
  ClientState: "Has attachments and high importance" 
} 
```


The following example requests a notification for an all-day event being created: 

```
POST https://outlook.office.com/api/v2.0/me/subscriptions HTTP/1.1 
Content-Type: application/json 
 
{ 
  @odata.type:"#Microsoft.OutlookServices.PushSubscription", 
  Resource: "https://outlook.office.com/api/v2.0/me/events?$filter=IsAllDay%20eq%20true", 
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient", 
  ChangeType: "Created", 
  ClientState: "Notifications for events that IsAllDay." 
} 
```


The following example requests a notification for a contact being created with the company being Contoso: 

```
POST https://outlook.office.com/api/v2.0/me/subscriptions HTTP/1.1 
Content-Type: application/json 
 
{ 
  @odata.type:"#Microsoft.OutlookServices.PushSubscription", 
  Resource: "https://outlook.office.com/api/v2.0/me/contacts?$filter=CompanyName%20eq%20%27Contoso%27", 
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient", 
  ChangeType: "Created", 
  ClientState: "Contacts in Contoso." 
} 
```

One common application of `$filter` is to get notification on a change in a specific property of the specified resource.
For example, you can use `$filter` to subscribe to unread messages in a folder (the **IsRead** property is false), and include all the change types:

- A message added to or marked unread in the folder would trigger a `Created` notification. 
- Reading a message or marking it as read in the folder would trigger a `Deleted` notification.
- A change to any property, other than **IsRead**, of a message in the folder would trigger an `Updated` notification.

You can create such a subscription as follows:

```
POST https://outlook.office.com/api/v2.0/me/subscriptions HTTP/1.1
Content-Type: application/json

{
  @odata.type:"#Microsoft.OutlookServices.PushSubscription",
  Resource: "https://outlook.office.com/api/v2.0/me/mailfolders('folder_id')/messages$filter=IsRead%20eq%20false",
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",
  ChangeType: "Created,Deleted,Updated",
  ClientState: "Message unread"
}
```


If you don’t use a `$filter` when creating the subscription (as below):

- Adding a message to the folder would result in a `Created` notification.
- Reading a message in the folder, marking the message as read, or changing any other property of a message in that folder would result in an `Updated` notification. 
- Deleting the message would result in a `Delete` notification.

```
POST https://outlook.office.com/api/v2.0/me/subscriptions HTTP/1.1
Content-Type: application/json

{
  @odata.type:"#Microsoft.OutlookServices.PushSubscription",
  Resource: "https://outlook.office.com/api/v2.0/me/mailfolders('folder_id')/messages,
  NotificationURL: "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",
  ChangeType: "Created,Deleted,Updated",
  ClientState: "Message unread"
}
```


**評価の要求**

検証要求は、サブスクリプション要求でクライアント アプリケーションが指定する NotificationURL を検証しようとします。
```no-highlight
POST https://{notificationUrl}?validationToken={token}
```
The Outlook service specifies the NotificationURL in the subscription request in {notificationUrl}, and defines a string {token} as the validation token. Outlook のサービスでは、サブスクリプション要求の NotificationURL を{notificationURL} で指定し、文字列を {token} として定義します。クライアント アプリがサブスクリプション要求に **ClientState** ヘッダーを含める場合、Outlook サービスもそのヘッダーを含めます。


**サブスクリプション応答**

サブスクリプション応答には、要求のプロパティ、および次の追加のプロパティが含まれています。
- **Id**  – 今後の通知と一致するようにクライアント アプリが保存する必要がある一意なサブスクリプション ID。
- **ChangeType** -要求で指定された値とは別に、追加の通知タイプである  
- **SubscriptionExpirationDateTime** - サブスクリプションの有効期限が切れる日付と時刻。 SubscriptionExpirationDateTime - サブスクリプションの有効期限が切れる日付と時刻。要求に有効期限の時刻を指定しない場合、または要求が 3 日間よりも長い有効期限を指定している場合、応答は要求が送信された時刻から 3 日の最大値を設定します。
- 他の OData 関連のプロパティ。


**応答データのサンプル**

この応答では、リスナー サービスが新しいイベントと失敗した変更の通知の受信を想定していることを示します。変更に失敗した場合、クライアントはその変更をキャプチャするためにサービスと同期する必要があります。

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Subscriptions/$entity",
    "@odata.type": "#Microsoft.OutlookServices.PushSubscription",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Subscriptions('Mjk3QNERDQQ==')",
    "Id": "Mjk3QNERDQQ==",
    "Resource": "https://outlook.office.com/api/v2.0/me/events",
    "ChangeType": "Created, Missed",
    "ClientState": "c75831bd-fad3-4191-9a66-280a48528679",
    "NotificationURL": "https://mywebhook.azurewebsites.net/api/send/myNotifyClient",
    "SubscriptionExpirationDateTime": "2015-04-23T22:46:13.8805047Z"
}
```


サインインしているユーザーの既存のサブスクリプションのいずれかに関する詳細情報を検索するには、そのサブスクリプション ID を指定します。たとえば、最後の例で作成したサブスクリプションのクエリを実行するには、次のようにします。 As an example, to query the subscription created in the last example:
```
GET https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Subscriptions('Mjk3QNERDQQ==')
```
成功すると、クライアント アプリケーションが潜在的なセキュリティ リスクを回避するために指定した ClientState を除外するという点を除き、応答はサンプルの最後の応答データと同じです。  
the client app has specified to avoid potential security risks. 


**通知**

各通知には、その種類に関係なく、次のプロパティが含まれています。
- **ClientState** ヘッダー - クライアントがサブスクリプション要求で **ClientState** プロパティを指定した場合にのみ存在します。通知の正当性を確認するために、リスナーによって使用されます。
- **SubscriptionId** - この通知が所属するサブスクリプションをクライアント アプリに指定します。
- **SubscriptionExpirationDateTime** - サブスクリプションの有効期限が切れる日付と時刻。 
- **SequenceNumber** - 通知が欠落していないかどうかをクライアント アプリが識別できるようにする、通知の連続した番号。
- **ChangeType** - クライアント アプリケーションに通知しようとしているイベントの種類 (たとえば、メールを受信したときの **Created** イベント、メッセージを読むときの **Update** イベント)。 
- **リソース** – 監視されている特定のリソース アイテムの URL (変更されたメッセージまたはイベントへの URL など)。 
- **ResourceData** - リソースの変更に関連する通知 (メッセージを受信する、読む、削除するなど) が、変更されたアイテムのリソース ID を含むこの追加のプロパティを持っています。クライアントはこのリソース ID を使用して、ビジネス ロジック (このアイテムをフェッチする、フォルダーを同期するなど) に従ってこのアイテムを処理できます   ResourceData - リソースの変更に関連する通知 (メッセージを受信する、読む、削除するなど) が、変更されたアイテムのリソース ID を含むこの追加のプロパティを持っています。クライアントはこのリソース ID を使用して、ビジネス ロジック (このアイテムをフェッチする、フォルダーを同期するなど) に従ってこのアイテムを処理できます 

注:[Notification](#NotificationResource) エンティティでは、Id プロパティは使用されません。

<a name="ChangeNotificationPayload"> </a>

** 変更通知のペイロード **

In the following example, as the user receives a new event, the notifications service sends a notification with ChangeType set to "Created". The header of the notification would include the ClientState as specified in the initial subscription request. The receive event notification has a payload similar to this:

```no-highlight
{
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.Notification",
            "Id": null,
            "SubscriptionId": "Mjk3QNERDQQ==",
            "SubscriptionExpirationDateTime": "2015-04-23T22:46:13.8805047Z",
            "SequenceNumber": 1,
            "ChangeType": "Created",
            "Resource" : "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Events('AAMkADNkNmAA=')",
            "ResourceData": {
                "@odata.type": "#Microsoft.OutlookServices.Event",
                "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/Events('AAMkADNkNmAA=')",
                "Id": "AAMkADNkNmAA="
            }
        }
    ]
}
```



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->









<a name="RenewSub"> </a>

### サブスクリプションを更新する


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

更新要求の時間から最大 3 日間のサブスクリプションを更新します。  

```no-highlight
PATCH https://outlook.office.com/api/beta/me/subscriptions/{subscriptionId}
```

__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _https://outlook.office.com/tasks.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


既定のサブスクリプションの長さは 3 日です。ペイロードなしで更新要求を送信することができ、サブスクリプションは 3 日延長されます。


**要求のサンプル**

```
PATCH https://outlook.office.com/api/beta/me/subscriptions/Mjk3QNERDQQ==

{
   @odata.type:"#Microsoft.OutlookServices.PushSubscription",
   "SubscriptionExpirationDateTime": "2015-05-28T17:17:45.9028722Z"
}

```

成功した応答は、HTTP 200 応答コードで示されます。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


更新要求の時間から最大 3 日間のサブスクリプションを更新します。  

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/subscriptions/{subscriptionId}
```

__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


既定のサブスクリプションの長さは 3 日です。ペイロードなしで更新要求を送信することができ、サブスクリプションは 3 日延長されます。


**要求のサンプル**

```
PATCH https://outlook.office.com/api/v2.0/me/subscriptions/Mjk3QNERDQQ==

{
   @odata.type:"#Microsoft.OutlookServices.PushSubscription",
   "SubscriptionExpirationDateTime": "2015-05-28T17:17:45.9028722Z"
}

```

成功した応答は、HTTP 200 応答コードで示されます。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->







 

<a name="DeleteSub"></a>

### サブスクリプションを削除する


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

サブスクリプション ID を指定して、サブスクリプションを削除します。

```no-highlight
DELETE https://outlook.office.com/api/beta/me/subscriptions('{subscriptionId}')
```

__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _https://outlook.office.com/tasks.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


**要求のサンプル:**

```
Delete https://outlook.office.com/api/beta/me/subscriptions('Mjk3QNERDQQ==')
```

成功した応答は、HTTP 200 応答コードで示されます。  


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


サブスクリプション ID を指定して、サブスクリプションを削除します。

```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/subscriptions('{subscriptionId}')
```

__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_


**要求のサンプル:**

```
Delete https://outlook.office.com/api/v2.0/me/subscriptions('Mjk3QNERDQQ==')
```

成功した応答は、HTTP 200 応答コードで示されます。  


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->









<a name="NotificationEntities"> </a>
## Notification entities  <a name="SubscriptionResource"> </a>
### Subscription  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


サブスクリプションを表します。 基本のサブスクリプションを表します。これは抽象基底クラスです。

基本型: **Entity**

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| リソース | 文字列 | 変更の監視対象となるリソースを指定します。 | はい | N/A |
| ChangeType | ChangeType | 通知を発生させるイベントの種類を示します。列挙値は次のとおりです。作成 =1、更新=2、削除=4、失敗=16。サービスで通知が失われた場合は、通知の失敗が発生します。 | はい  | N/A |
| ClientState | 文字列 | 通知ごとにサービスによって送信される ClientState ヘッダーの値を指定します。最大の長さは 255 文字です。クライアントはサービスから通知が送られたことを確認できます。これは、ClientState プロパティに設定されている値と、各通知と共に受信された ClientState ヘッダーの値を比較することによって行います。 | はい | いいえ |

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


サブスクリプションを表します。 基本のサブスクリプションを表します。これは抽象基底クラスです。

基本型: **Entity**

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| リソース | 文字列 | 変更の監視対象となるリソースを指定します。 | はい | N/A |
| ChangeType | ChangeType | 通知を発生させるイベントの種類を示します。列挙値は次のとおりです。作成 =1、更新=2、削除=4、失敗=16。サービスで通知が失われた場合は、通知の失敗が発生します。 | はい  | N/A |
| ClientState | 文字列 | 通知ごとにサービスによって送信される ClientState ヘッダーの値を指定します。最大の長さは 255 文字です。クライアントはサービスから通知が送られたことを確認できます。これは、ClientState プロパティに設定されている値と、各通知と共に受信された ClientState ヘッダーの値を比較することによって行います。 | はい | いいえ |


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->







<a name="NotificationResource"> </a>
###通知<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

リスナー サービスに送られる通知を表します。

基本型: **Entity**

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| SubscriptionId | string | サブスクリプションの一意の識別子です。 | いいえ | いいえ |
| SubscriptionExpirationDateTime | Edm.DateTimeOffset | 通知サブスクリプションの有効期限が切れる日時を指定します。時刻は UTC です。 | はい | N/A |
| SequenceNumber | int32 | 変更通知のシーケンス値を示します。 | いいえ  | N/A |
| ChangeType | ChangeType | 通知を発生させたイベントの種類を示します。列挙値は次のとおりです。作成 =1、更新=2、削除=4、失敗=16。通知の失敗は、通知サービスが要求された通知を送信できない場合に発生します。 | いいえ | 該当なし |
| リソース | 文字列 | 変更の監視対象となっているリソース アイテムを指定します。 | はい | N/A |
| ResourceData | エンティティ | 変更されたエンティティを識別します。これはナビゲーションのプロパティです。 | いいえ | N/A |



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


リスナー サービスに送られる通知を表します。

基本型: **Entity**

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| SubscriptionId | string | サブスクリプションの一意の識別子です。 | いいえ | いいえ |
| SubscriptionExpirationDateTime | Edm.DateTimeOffset | 通知サブスクリプションの有効期限が切れる日時を指定します。時刻は UTC です。 | はい | N/A |
| SequenceNumber | int32 | 変更通知のシーケンス値を示します。 | いいえ  | N/A |
| ChangeType | ChangeType | 通知を発生させたイベントの種類を示します。列挙値は次のとおりです。作成 =1、更新=2、削除=4、失敗=16。通知の失敗は、通知サービスが要求された通知を送信できない場合に発生します。 | いいえ | 該当なし |
| リソース | 文字列 | 変更の監視対象となっているリソース アイテムを指定します。 | はい | N/A |
| ResourceData | エンティティ | 変更されたエンティティを識別します。これはナビゲーションのプロパティです。 | いいえ | N/A |



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->









<a name="PushSubscription"> </a>
### PushSubscription   <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

サブスクリプションを表します。

基本型:[サブスクリプション](#SubscriptionResource)

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| NotificationURL | 文字列 | プッシュ通知を受け取るアプリケーションの URL です。  | はい | N/A |
| SubscriptionExpirationDateTime | Edm.DateTimeOffset | 通知サブスクリプションの有効期限が切れる日時を指定します。時刻は UTC です。 | はい | N/A |



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


サブスクリプションを表します。

基本型:[サブスクリプション](#SubscriptionResource)

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| NotificationURL | 文字列 | プッシュ通知を受け取るアプリケーションの URL です。  | はい | N/A |
| SubscriptionExpirationDateTime | Edm.DateTimeOffset | 通知サブスクリプションの有効期限が切れる日時を指定します。時刻は UTC です。 | はい | N/A |



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->








<a name="NextSteps"> </a>
##次のステップ<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](https://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)

- [メール REST API リファレンス](..\api\mail-rest-operations.md)

- [予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API](..\api\task-rest-operations.md) (プレビュー)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](https://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)

- [メール REST API リファレンス](..\api\mail-rest-operations.md)

- [予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]



