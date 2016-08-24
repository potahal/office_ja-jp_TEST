---
ms.Toctitle: Outlook Calendar REST API reference
title: "Outlook 予定表 REST API リファレンス"
description: "ms.TocTitle:Outlook 予定表 REST API リファレンスTitle:Outlook 予定表 REST API リファレンスDescription:Exchange Online でイベント、予定表、および予定表グループへのアクセスを提供する予定表 REST API とクライアント ライブラリ API を操作する方法のリファレンス。ms.ContentId:443f1cdf-1adb-46a2-b299-228c6f429954ms.topic: リファレンス (API) ms.date:2016 年 5 月 19 日"
ms.ContentId: 443f1cdf-1adb-46a2-b299-228c6f429954
ms.date: July 19, 2016

---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Outlook 予定表 REST API リファレンス


[!INCLUDE [Add the Outlook REST API filters--v2 default](../includes/controls/addOutlookVersion_v2default.xml)]

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<p class="previewnote">< p class="previewnote">このドキュメントでは、会議時間の検索、イベントのキャンセル、およびプレビュー段階での添付ファイルの参照を行うための API について説明します。 Preview features are subject to change prior to finalization, and may break code that uses them. プレビュー機能は、最終処理までに変更される場合があります。それらを使用するコードを破棄する場合もあります。このため、一般に、実稼働コードでは実稼働バージョンの API のみを使用してください。可能な場合には、現在バージョン 2.0 が優先バージョンです。</p> If available, v2.0 is currently the preferred version.</p>

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

   
 _**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

予定表 API は、Office 365 の Azure Active Directory によって保護されているイベント、予定表、および予定表グループ データへのアクセス、および特にこれらのドメイン内の Microsoft アカウントの類似したデータへのアクセスを提供します。Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com。 
  

<!-- Can add the following sentence back once the client libraries have been updated for converged auth and outlook.com

You can access the Calendar API by calling the corresponding REST APIs directly in your apps, or by using the Office 365 client libraries and SDKs.
-->

**注意** 

- The exception is the API to [find meeting times](#FindMeetingTimesPreview), which applies to only Office 365 mailboxes (on Azure AD) and not to Microsoft accounts. 
- **メモ** リファレンスをわかりやすくするため、この記事の残りの部分では "Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として使用しています。


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
 
 
 
## すべての予定表 API 操作

Event エンティティは、ユーザーの予定表にある予定または会議を表します。 イベントはユーザーの予定表にある予定または会議を表します。イベントには、シリーズ マスター (定期的なイベントの場合)、発生、単一インスタンス、または例外を指定できます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

[Get events](#GetEvents) | [Sync events](#SyncCalendarView) | [Find meeting times (preview)](#FindMeetingTimesPreview) | [Create events](#CreateEvents) | 
[Update events](#UpdateEvents) | [Respond to events](#RespndToEvents) | [Delete events](#DeleteEvents) | [Cancel events (preview)](#CancelEvents) | 
[Get attachments](#GetAttachments) | [Create attachments](#CreateAttachments) | [Delete attachments](#DeleteAttachments) | 
[Get reminders](#GetReminders) | [Snooze reminders](#SnoozeReminders)] | [Dismiss reminders](#DismissReminders)


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

[Get events](#GetEvents) | [Sync events](#SyncCalendarView) | [Create events](#CreateEvents) | 
[Update events](#UpdateEvents) | [Respond to events](#RespndToEvents) | [Delete events](#DeleteEvents) | 
[Get attachments](#GetAttachments) | [Create attachments](#CreateAttachments) | [Delete attachments](#DeleteAttachments) | 
[Get reminders](#GetReminders) | [Snooze reminders](#SnoozeReminders)] | [Dismiss reminders](#DismissReminders)


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

[Get events](#GetEvents) | [Sync events](#SyncCalendarView) | [Create events](#CreateEvents) | 
[Update events](#UpdateEvents) | [Respond to events](#RespndToEvents) | [Delete events](#DeleteEvents) | 
[Get attachments](#GetAttachments) | [Create attachments](#CreateAttachments) | [Delete attachments](#DeleteAttachments) | 
[Get reminders](#GetReminders) | [Snooze reminders](#SnoozeReminders)] | [Dismiss reminders](#DismissReminders)

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




<a name="CalendarOperations"> </a>
**Calendar operations** A calendar serves as a container for events. A user can have multiple calendars. 予定表は、イベントのコンテナーの役割をします。ユーザーは複数の予定表を持つことができます。Office 365 では、各予定表は予定表グループに割り当てることができます。

[Get calendars](#GetCalendars) | [Create calendars](#CreateCalendars) | [Update calendars](#UpdateCalendars) | [Delete calendars](#DeleteCalendars) 

<a name="CalendarGroupOperations"> </a>
**Calendar group operations** Calendar groups are a way to organize multiple calendars. Users can add multiple calendars into a single calendar group in Outlook or Outlook Web App. This makes it easier for users to quickly view all calendars within the group.


**メモ** Outlook.com によってサポートされるのは、../me/calendars`../me/calendars` ショートカットでアクセス可能な既定の予定表グループのみです。 You cannot delete that calendar group, or create another calendar group.


[Get calendar groups](#GetCalendarGroups) | [Create calendar groups](#CreateCalendarGroups) | 
[Update calendar groups](#UpdateCalendarGroups) | [Delete calendar groups](#DeleteCalendarGroups)  

関連項目:

REST API イベント リソース  REST API 予定表リソース  REST API 予定表グループ リソース



<a name="Overview"> </a>
## 予定表 REST API の使用

### 認証

Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the Calendar API, you should include a valid access token. アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you. Keep this in mind as you proceed with the specific operations in the Calendar API.

<a name="SupportedVersions"> </a>

###API のバージョン

予定表 REST API は、すべてのバージョンの Outlook REST API でサポートされています。機能は、特定のバージョンによって異なる場合があります。

###ターゲット ユーザー

予定表 API 要求は、常に現在のユーザーのために実行されます。 

Outlook REST API のすべてのサブセットに共通な情報について詳しくは、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」をご覧ください。

****

<a name="GetEvents"> </a>
## イベントを取得する

イベント コレクションまたはイベントを取得します。 

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

予定表イベントを取得するすべての操作では、_Prefer: outlook.timezone_ HTTP ヘッダーを使用して、応答の開始時刻と終了時刻のタイムゾーンを指定できます。たとえば、次の _Prefer: outlook.timezone_ ヘッダーは、東部標準時での応答の開始時刻と終了時刻を設定します。 For example, the following _Prefer: outlook.timezone_ header sets the start and end times in the response to Eastern Standard Time.

```
Prefer: outlook.timezone="Eastern Standard Time"
``` 

_Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

_イベント_リソース上で _OriginalStartTimeZone_ プロパティと _OriginalEndTimeZone_ プロパティを使用して、イベント作成時に使用されたタイム ゾーンを検索することができます。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->
<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

予定表イベントを取得するすべての操作では、_Prefer: outlook.timezone_ HTTP ヘッダーを使用して、応答の開始時刻と終了時刻のタイムゾーンを指定できます。たとえば、次の _Prefer: outlook.timezone_ ヘッダーは、東部標準時での応答の開始時刻と終了時刻を設定します。 For example, the following _Prefer: outlook.timezone_ header sets the start and end times in the response to Eastern Standard Time.

```
Prefer: outlook.timezone="Eastern Standard Time"
``` 

_Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。サポートされているタイム ゾーン名については、[この一覧](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone)をご参照ください。 

_イベント_リソース上で _OriginalStartTimeZone_ プロパティと _OriginalEndTimeZone_ プロパティを使用して、イベント作成時に使用されたタイム ゾーンを検索することができます。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


予定表ビューを取得する (REST)  
[Get series master and single events (REST)](#GetEventCollection) |
 [Get event instances (REST)](#GetEventInstances) | [Get an event (REST)](#GetEvent) 

クライアント ライブラリ:[ユーザーの予定表からイベントを取得する (クライアント)](#GetEventsClient)

****

<a name="GetCalendarView"> </a>
###予定表ビューを取得する (REST) 

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

ユーザーの標準として設定されている予定表 (../me/calendarview`../me/calendarview`) または別の予定表から、時間範囲で定義した予定表ビューのイベントの発生、例外、および単一インスタンスを取得します。
   

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
GET https://outlook.office.com/api/beta/me/calendars/{calendar_id}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイム ゾーン。|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID (特定の予定表から予定表ビューを取得している場合)。|
|start_datetime|datetimeoffset|イベントが開始する日時。|
|end_datetime|datetimeoffset|イベントが終了する日時。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. _Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

**メモ**: 既定では、応答内の各イベントにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を使用します。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

たとえば、各イベントの Subject プロパティのみを返す 10 月の月間予定表ビューを取得します: Assuming that the _Prefer: outlook.timezone_ header is not included in the request, the time zone will be UTC. 

```
GET https://outlook.office.com/api/beta/me/calendarview?startDateTime=2014-10-01T01:00:00Z&endDateTime=2014-10-31T23:00:00Z&$select=Subject
```

 **応答の種類**

指定した時間範囲の展開された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
GET https://outlook.office.com/api/v2.0/me/calendars/{calendar_id}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイム ゾーン。|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID (特定の予定表から予定表ビューを取得している場合)。|
|start_datetime|datetimeoffset|イベントが開始する日時。|
|end_datetime|datetimeoffset|イベントが終了する日時。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. _Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

**メモ**: 既定では、応答内の各イベントにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を使用します。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

たとえば、各イベントの Subject プロパティのみを返す 10 月の月間予定表ビューを取得します: Assuming that the _Prefer: outlook.timezone_ header is not included in the request, the time zone will be UTC. 

```
GET https://outlook.office.com/api/v2.0/me/calendarview?startDateTime=2014-10-01T01:00:00&endDateTime=2014-10-31T23:00:00&$select=Subject
```

 **応答の種類**

指定した時間範囲の展開された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/calendarview?start={start_datetime}&end={end_datetime}
GET https://outlook.office.com/api/v1.0/me/calendars/{calendar_id}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID (特定の予定表から予定表ビューを取得している場合)。|
|start_datetime|datetimeoffset|イベントが開始する日時。|
|end_datetime|datetimeoffset|イベントが終了する日時。|

**メモ**: 既定では、応答内の各イベントにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を使用します。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

たとえば、各イベントの Subject プロパティのみを返す 10 月の月間予定表ビューを取得します: 

```
GET https://outlook.office.com/api/v1.0/me/calendarview?startDateTime=2014-10-01T01:00:00Z&endDateTime=2014-10-31T23:00:00Z&$select=Subject
```

 **応答の種類**

指定した時間範囲の展開された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


****

<a name="GetEventCollection"> </a>
###シリーズ マスターと単一イベントを取得する (REST) 

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

シリーズ マスターと単一インスタンス イベントのコレクションを、ユーザーの標準として設定されている予定表 (../me/events`../me/events`) または別の予定表から取得します。拡張イベントのインスタンスを取得するには、予定表ビューを取得するまたはイベントのインスタンスを取得することができます。 シリーズ マスターと単一インスタンス イベントのコレクションを、ユーザーの標準として設定されている予定表 (../me/events) または別の予定表から取得します。拡張イベントのインスタンスを取得するには、 [予定表ビューを取得する](#GetCalendarView)または[イベントのインスタンスを取得する](#GetEventInstances)ことができます。



<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/events
GET https://outlook.office.com/api/beta/me/calendars/{calendar_id}/events
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_Header parameters|
|Prefer:|outlook.timezone|応答に対するイベントの既定のタイム ゾーン。|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID (特定の予定表からイベントを取得している場合)。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. _Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

**メモ**: 応答内の各イベントにはそのプロパティがすべて含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. See the next example. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、イベントの **Subject**、**Organizer**、**Start**、および **End** プロパティのみを返すように指定する方法を示しています。 次の例は、**$select** を使用して、応答内の各イベントの Subject、Organizer、Start、および End プロパティのみを返すように指定する方法を示しています。$select を使用しない場合のイベントに返されるプロパティの完全な一覧に関しては、「[イベントを取得する (REST)](#GetEvent)」の最初の応答サンプルをご参照ください。


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/events?$select=Subject,Organizer,Start,End
```

**応答のサンプル**

状態コード :200

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events(Subject,Organizer,Start,End)",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI28tEyDAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAA/LpDWw==\"",
            "Id": "AAMkAGI28tEyDAAA=",
            "Subject": "Scrum",
            "Start": {
                "DateTime": "2015-11-02T17:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-11-02T17:30:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "user0TestUser",
                    "Address": "user0@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI28tEyCAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAA/LpDWg==\"",
            "Id": "AAMkAGI28tEyCAAA=",
            "Subject": "team lunch",
            "Start": {
                "DateTime": "2015-11-02T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-11-03T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "user0TestUser",
                    "Address": "user0@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2ADTG93AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x49w==\"",
            "Id": "AAMkAGI2G93AAA=",
            "Subject": "Weekly Meeting on Contoso Project",
            "Start": {
                "DateTime": "2014-10-13T21:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-10-13T22:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG92AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x49g==\"",
            "Id": "AAMkAGI2TG92AAA=",
            "Subject": "Daily Team Meeting",
            "Start": {
                "DateTime": "2014-10-13T18:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-10-13T18:30:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG91AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x47Q==\"",
            "Id": "AAMkAGI2TG91AAA=",
            "Subject": "Rob:Alex 1:1",
            "Start": {
                "DateTime": "2014-10-15T16:30:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-10-15T17:30:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG90AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x46g==\"",
            "Id": "AAMkAGI2TG90AAA=",
            "Subject": "Thanksgiving Holiday",
            "Start": {
                "DateTime": "2015-11-26T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-11-27T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG9zAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x46Q==\"",
            "Id": "AAMkAGI2TG9zAAA=",
            "Subject": "Thanksgiving Holiday",
            "Start": {
                "DateTime": "2014-11-27T00:00:00"
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-11-28T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG9yAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x49Q==\"",
            "Id": "AAMkAGI2TG9yAAA=",
            "Subject": "New Year's Day Holiday",
            "Start": {
                "DateTime": "2015-01-01T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-01-02T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG9xAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x45w==\"",
            "Id": "AAMkAGI2TG9xAAA=",
            "Subject": "Christmas Holiday",
            "Start": {
                "DateTime": "2014-12-25T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-12-26T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
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


```no-highlight
GET https://outlook.office.com/api/v2.0/me/events
GET https://outlook.office.com/api/v2.0/me/calendars/{calendar_id}/events
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイム ゾーン。|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID (特定の予定表からイベントを取得している場合)。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. _Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

**メモ**: 応答内の各イベントにはそのプロパティがすべて含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. See the next example. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、イベントの **Subject**、**Organizer**、**Start**、および **End** プロパティのみを返すように指定する方法を示しています。 次の例は、**$select** を使用して、応答内の各イベントの Subject、Organizer、Start、および End プロパティのみを返すように指定する方法を示しています。$select を使用しない場合のイベントに返されるプロパティの完全な一覧に関しては、「[イベントを取得する (REST)](#GetEvent)」の最初の応答サンプルをご参照ください。


**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/events?$select=Subject,Organizer,Start,End
```

**応答のサンプル:**

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events(Subject,Organizer,Start,End)",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI28tEyDAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAA/LpDWw==\"",
            "Id": "AAMkAGI28tEyDAAA=",
            "Subject": "Scrum",
            "Start": {
                "DateTime": "2015-11-02T17:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-11-02T17:30:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "user0TestUser",
                    "Address": "user0@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI28tEyCAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAA/LpDWg==\"",
            "Id": "AAMkAGI28tEyCAAA=",
            "Subject": "team lunch",
            "Start": {
                "DateTime": "2015-11-02T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-11-03T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "user0TestUser",
                    "Address": "user0@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2ADTG93AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x49w==\"",
            "Id": "AAMkAGI2G93AAA=",
            "Subject": "Weekly Meeting on Contoso Project",
            "Start": {
                "DateTime": "2014-10-13T21:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-10-13T22:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG92AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x49g==\"",
            "Id": "AAMkAGI2TG92AAA=",
            "Subject": "Daily Team Meeting",
            "Start": {
                "DateTime": "2014-10-13T18:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-10-13T18:30:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG91AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x47Q==\"",
            "Id": "AAMkAGI2TG91AAA=",
            "Subject": "Rob:Alex 1:1",
            "Start": {
                "DateTime": "2014-10-15T16:30:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": "2014-10-15T17:30:00Z",
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG90AAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x46g==\"",
            "Id": "AAMkAGI2TG90AAA=",
            "Subject": "Thanksgiving Holiday",
            "Start": {
                "DateTime": "2015-11-26T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-11-27T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG9zAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x46Q==\"",
            "Id": "AAMkAGI2TG9zAAA=",
            "Subject": "Thanksgiving Holiday",
            "Start": {
                "DateTime": "2014-11-27T00:00:00"
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-11-28T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG9yAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x49Q==\"",
            "Id": "AAMkAGI2TG9yAAA=",
            "Subject": "New Year's Day Holiday",
            "Start": {
                "DateTime": "2015-01-01T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2015-01-02T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
                "EmailAddress": {
                    "Name": "Alex D",
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG9xAAA=')",
            "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x45w==\"",
            "Id": "AAMkAGI2TG9xAAA=",
            "Subject": "Christmas Holiday",
            "Start": {
                "DateTime": "2014-12-25T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "End": {
                "DateTime": "2014-12-26T00:00:00",
                "TimeZone": "Pacific Standard Time"
            },
            "Organizer": {
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

```no-highlight
GET https://outlook.office.com/api/v1.0/me/events
GET https://outlook.office.com/api/v1.0/me/calendars/{calendar_id}/events
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID (特定の予定表からイベントを取得している場合)。|

**メモ**: 応答内の各イベントにはそのプロパティがすべて含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. See the next example. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、イベントの **Subject**、**Organizer**、**Start**、および **End** プロパティのみを返すように指定する方法を示しています。 次の例は、**$select** を使用して、応答内の各イベントの Subject、Organizer、Start、および End プロパティのみを返すように指定する方法を示しています。$select を使用しない場合のイベントに返されるプロパティの完全な一覧に関しては、「[イベントを取得する (REST)](#GetEvent)」の最初の応答サンプルをご参照ください。


[!code-REST-i[calendar_api_get_events](./_data/calendar_api_get_events.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****


<a name="GetEventInstances"></a>
###イベント インスタンスを取得する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

指定した時間範囲のイベントのインスタンス (発生) を取得できます。イベントが **SeriesMaster** タイプである場合、これは指定した時間範囲内のイベントの発生と例外を返します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/events/{event_id}/instances?startDateTime={start_datetime}&endDateTime={end_datetime}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイムゾーン。|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|start_datetime|datetimeoffset|イベントが開始する UTC 日時。|
|end_datetime|datetimeoffset|イベントが終了する UTC 日時。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. _Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

 **応答の種類**

要求された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)のコレクション。

**メモ**: 既定では、応答内の各イベントにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を使用します。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned.  
パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

たとえば、10 月の特定のイベントのインスタンスを取得します。これには、各インスタンスの **Subject**、**Start**、および **End** プロパティのみが含まれます。 

<!--don't use httprequest for this because it renders as lowercase, which breaks the call-->
```
GET https://outlook.office.com/api/beta/me/events/AAMkAGE0MGM1Y2M5LWEAAA=/instances?startDateTime=2014-10-01T01:00:00Z&endDateTime=2014-10-31T23:00:00Z&$select=Subject,Start,End
```



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/events/{event_id}/instances?startDateTime={start_datetime}&endDateTime={end_datetime}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイム ゾーン。|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|start_datetime|datetimeoffset|イベントが開始する UTC 日時。|
|end_datetime|datetimeoffset|イベントが終了する UTC 日時。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. _Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

 **応答の種類**

要求された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)のコレクション。

**メモ**: 既定では、応答内の各イベントにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を使用します。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned.  
パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

たとえば、10 月の特定のイベントのインスタンスを取得します。これには、各インスタンスの **Subject**、**Start**、および **End** プロパティのみが含まれます。 

<!--don't use httprequest for this because it renders as lowercase, which breaks the call-->
```
GET https://outlook.office.com/api/v2.0/me/events/AAMkAGE0MGM1Y2M5LWEAAA=/instances?startDateTime=2014-10-01T01:00:00Z&endDateTime=2014-10-31T23:00:00Z&$select=Subject,Start,End
```



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->

<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


```no-highlight
GET https://outlook.office.com/api/v1.0/me/events/{event_id}/instances?startDateTime={start_datetime}&endDateTime={end_datetime}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|start_datetime|datetimeoffset|イベントが開始する UTC 日時。|
|end_datetime|datetimeoffset|イベントが終了する UTC 日時。|


 **応答の種類**

要求された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)のコレクション。

**メモ**: 既定では、応答内の各イベントにそのプロパティがすべて含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、$select を使用します。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned.  
パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

たとえば、10 月の特定のイベントのインスタンスを取得します。これには、各インスタンスの **Subject**、**Start**、および **End** プロパティのみが含まれます。 

<!--don't use httprequest for this because it renders as lowercase, which breaks the call-->
```
GET https://outlook.office.com/api/v1.0/me/events/AAMkAGE0MGM1Y2M5LWEAAA=/instances?startDateTime=2014-10-01T01:00:00Z&endDateTime=2014-10-31T23:00:00Z&$select=Subject,Start,End
```


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****


<a name="GetEvent"> </a>
###イベントを取得する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

ID でイベントを取得します。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/events/{event_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイムゾーン。|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names.If the _Prefer: outlook.timezone_ header is not specified, the start and end times are returned in UTC.

**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/events/AAMkAGI2TG93AAA=
```

**応答のサンプル:**

```
    {
        "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
        "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG93AAA=')",
        "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x48w==\"",
        "Id": "AAMkAGI2TG93AAA=",
        "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x48w==",
        "Categories": [],
        "CreatedDateTime": "2014-10-19T23:13:47.3959685Z",
        "LastModifiedDateTime": "2014-10-19T23:13:47.6772234Z",
        "Subject": "Weekly Meeting on Contoso Project",
        "BodyPreview": "Setting up some time to review the budget and planning on the Contoso Project",
        "Body": {
            "ContentType": "HTML",
            "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nSetting up some time to review the budget and planning on the Contoso Project\r\n</body>\r\n</html>\r\n"
        },
        "Importance": "Normal",
        "HasAttachments": false,
        "Start": {
            "DateTime": "2014-10-13T21:00:00",
            "TimeZone": "Pacific Standard Time"
        },
        "End": {
            "DateTime": "2014-10-13T22:00:00",
            "TimeZone": ""Pacific Standard Time"
        },        
        "Location": {
            "DisplayName": "Alex's Office",
            "Address": null,
            "Coordinates": null,
            "LocationEmailAddress": "",
            "LocationUri": ""
        },
        "ShowAs": "Busy",
        "IsAllDay": false,
        "IsCancelled": false,
        "IsOrganizer": true,
        "ResponseRequested": true,
        "Type": "SeriesMaster",
        "SeriesMasterId": null,
        "Attendees": [
            {
                "EmailAddress": {
                    "Address": "janets@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Janet Schorr"
                },
                "Status": {
                    "Response": "None",
                    "Time": "0001-01-01T00:00:00Z"
                },
                "Type": "Required"
            },
            {
                "EmailAddress": {
                    "Address": "pavelb@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Pavel Bansky"
                },
                "Status": {
                    "Response": "None",
                    "Time": "0001-01-01T00:00:00Z"
                },
                "Type": "Required"
            }
        ],
        "Recurrence": {
            "Pattern": {
                "Type": "Weekly",
                "Interval": 1,
                "Month": 0,
                "Index": "First",
                "FirstDayOfWeek": "Sunday",
                "DayOfMonth": 0,
                "DaysOfWeek": [
                    "Monday"
                ]
            },
            "RecurrenceTimeZone": "Pacific Standard Time",
            "Range": {
                "Type": "NoEnd",
                "StartDate": "2014-10-13",
                "EndDate": "2014-11-13",
                "NumberOfOccurrences": 0
            }
        },
        "OriginalEndTimeZone": "Pacific Standard Time",
        "OriginalStartTimeZone": "Pacific Standard Time",
        "Organizer": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        },
        "OnlineMeetingUrl": null
    }
```


**応答の種類**

要求された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。

**メモ**： 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、イベントの **Subject**、**Organizer**、**Start**、および **End** プロパティのみを返すように指定する方法を示しています。 


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/events/AAMkAGI2TG93AAA=?$select=Subject,Organizer,Start,End
```

**応答のサンプル:**

```
    {
        "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
        "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG93AAA=')",
        "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x48w==\"",
        "Id": "AAMkAGI2TG93AAA=",
        "Subject": "Weekly Meeting on Contoso Project",
        "Start": {
            "DateTime": "2014-10-13T21:00:00",
            "TimeZone": "Pacific Standard Time"
        },
        "End": {
            "DateTime": "2014-10-13T22:00:00",
            "TimeZone": ""Pacific Standard Time"
        }, 
        "Organizer": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        }
    }
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/events/{event_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイム ゾーン。|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. _Prefer: outlook.timezone_ ヘッダーを指定しない場合は、応答の開始時刻と終了時刻は UTC で返されます。

**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/events/AAMkAGI2TG93AAA=
```

**応答のサンプル:**

```
    {
        "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
        "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG93AAA=')",
        "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x48w==\"",
        "Id": "AAMkAGI2TG93AAA=",
        "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x48w==",
        "Categories": [],
        "CreatedDateTime": "2014-10-19T23:13:47.3959685Z",
        "LastModifiedDateTime": "2014-10-19T23:13:47.6772234Z",
        "Subject": "Weekly Meeting on Contoso Project",
        "BodyPreview": "Setting up some time to review the budget and planning on the Contoso Project",
        "Body": {
            "ContentType": "HTML",
            "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nSetting up some time to review the budget and planning on the Contoso Project\r\n</body>\r\n</html>\r\n"
        },
        "Importance": "Normal",
        "HasAttachments": false,
        "Start": {
            "DateTime": "2014-10-13T21:00:00",
            "TimeZone": "Pacific Standard Time"
        },
        "End": {
            "DateTime": "2014-10-13T22:00:00",
            "TimeZone": ""Pacific Standard Time"
        },        
        "Location": {
            "DisplayName": "Alex's Office",
            "Address": null
        },
        "ShowAs": "Busy",
        "IsAllDay": false,
        "IsCancelled": false,
        "IsOrganizer": true,
        "ResponseRequested": true,
        "Type": "SeriesMaster",
        "SeriesMasterId": null,
        "Attendees": [
            {
                "EmailAddress": {
                    "Address": "janets@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Janet Schorr"
                },
                "Status": {
                    "Response": "None",
                    "Time": "0001-01-01T00:00:00Z"
                },
                "Type": "Required"
            },
            {
                "EmailAddress": {
                    "Address": "pavelb@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Pavel Bansky"
                },
                "Status": {
                    "Response": "None",
                    "Time": "0001-01-01T00:00:00Z"
                },
                "Type": "Required"
            }
        ],
        "Recurrence": {
            "Pattern": {
                "Type": "Weekly",
                "Interval": 1,
                "Month": 0,
                "Index": "First",
                "FirstDayOfWeek": "Sunday",
                "DayOfMonth": 0,
                "DaysOfWeek": [
                    "Monday"
                ]
            },
            "RecurrenceTimeZone": "Pacific Standard Time",
            "Range": {
                "Type": "NoEnd",
                "StartDate": "2014-10-13",
                "EndDate": "2014-11-13",
                "NumberOfOccurrences": 0
            }
        },
        "OriginalEndTimeZone": "Pacific Standard Time",
        "OriginalStartTimeZone": "Pacific Standard Time",
        "Organizer": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        }
    }
```


**応答の種類**

要求された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。

**メモ**： 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、イベントの **Subject**、**Organizer**、**Start**、および **End** プロパティのみを返すように指定する方法を示しています。 

**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/events/AAMkAGI2TG93AAA=?$select=Subject,Organizer,Start,End
```

**応答のサンプル:**

```
    {
        "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
        "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2TG93AAA=')",
        "@odata.etag": "W/\"nfZyf7VcrEKLNoU37KWlkQAAA0x48w==\"",
        "Id": "AAMkAGI2TG93AAA=",
        "Subject": "Weekly Meeting on Contoso Project",
        "Start": {
            "DateTime": "2014-10-13T21:00:00",
            "TimeZone": "Pacific Standard Time"
        },
        "End": {
            "DateTime": "2014-10-13T22:00:00",
            "TimeZone": ""Pacific Standard Time"
        }, 
        "Organizer": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        }
    }
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->

<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


```no-highlight
GET https://outlook.office.com/api/v1.0/me/events/{event_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

[!code-REST-i[calendar_api_get_event_by_id](./_data/calendar_api_get_event_by_id.json)]


**応答の種類**

要求された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。

**メモ**： 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、イベントの **Subject**、**Organizer**、**Start**、および **End** プロパティのみを返すように指定する方法を示しています。 

[!code-REST-i[calendar_api_get_event_by_id_select](./_data/calendar_api_get_event_by_id_select.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****

<a name="GetEventsClient"></a>
### ユーザーの予定表からイベントを取得する (クライアント)

ユーザーの予定表からイベントを取得する (クライアント) ユーザーの既定の予定表からイベントを取得します。別の予定表からイベントを取得するには、その予定表の **Events** プロパティを呼び出します。

例`outlookClient.Me.Calendars[calendarId].Events.ExecuteAsync()`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


特定のイベントを取得するには、**Events** コレクションのインデックスとしてイベント ID を指定するか、**GetById** メソッドを使用します。

**メモ**: イベント コレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。

この例では、[Outlook サービス クライアントを作成する](..\api\use-outlook-rest-api.md#GetClient)メソッドを呼び出します。

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar" -->

```cs-i
var outlookClient = await CreateOutlookClientAsync("Calendar");
var events = await outlookClient.Me.Events
  .Take(10)
  .ExecuteAsync();
 
foreach(var calendarEvent in events.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine("Event '{0}'.", calendarEvent.Subject);
}
 
```

```javascript-i
outlookClient.me.events.getEvents().fetch().then(function (result) {
    result.currentPage.forEach(function (event) {
console.log('Event "' + event.subject + '"')
    });
}, function(error) {
    console.log(error);
});
```

<!-- ENDSECTION -->


この呼び出しでは、定期的なイベント (毎週のチーム ミーティングなど) の個々の展開されたインスタンスではなく、一連のイベントが返されます。

クライアント ライブラリでは、イベント インスタンスの照会は現在サポートされていません。REST API を使用して、 [Calendar](..\api\calendar-rest-operations.md#CalendarResource) リソースの **CalendarView** プロパティまたは [Event](..\api\calendar-rest-operations.md#EventResource) リソースの **Instances** プロパティを照会できます。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/events/{event_id}/instances?startDateTime={start_datetime}&endDateTime={end_datetime}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/events/{event_id}/instances?startDateTime={start_datetime}&endDateTime={end_datetime}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->

<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/events/{event_id}/instances?startDateTime={start_datetime}&endDateTime={end_datetime}
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->
 
<!--Update c# example to get instance-->
<!--Update js example and remove note when this works in js-->

****

<a name="SyncCalendarView"> </a>
##同期イベント  

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

Synchronize and get new, updated, or deleted events in a specified time range from the user's primary calendar (`../me/calendarview`) or from a different calendar. Such a set of events in a time range is also known as a calendar view. The returned events may include occurrences and exceptions of a recurring series, and single instances. 

予定表ビューの同期には、通常、2 つ以上の同期要求のラウンドが必要があり、それぞれが GET 呼び出しです。予定表ビューを同期するには、次の要求ヘッダーを除いて、予定表ビューを取得する方法と同じように GET メソッドを使用します。 To synchronize a calendar view, use the GET method much like the way you [get a calendar view](#GetCalendarView), except that you include certain request headers, and _deltaToken_ or a _skipToken_ when appropriate.  

**要求ヘッダー**

- 以前の同期要求から返される skipToken を含む同期要求を除き、すべての同期要求で "Prefer: odata.track-changes" ヘッダーを指定する必要があります。(skipToken に関する詳細については、以下の 2 番目の応答データのサンプルを参照)。 In the first response, look for the _Preference-Applied: odata.track-changes_ header to confirm that the resource supports synchronizing before proceeding. (More information about a `skipToken` in [sample second response data](#SyncCalendarViewSampleSecondResponse) below.)
- "Prefer: odata.maxpagesize={x}" ヘッダーを指定して、同期要求が返す完全なイベントの最大数を示すことができます。

Here's a typical round of synchronizing events in a calendar view:

1. Make the initial GET request with the mandatory _Prefer: odata.track-changes_ header. The initial response to a sync request always returns a _deltaToken_. 2 番目以降の GET 要求は、前の応答で受信した _deltaToken_ または _skipToken_ のいずれかを含むため、最初の GET 要求とは異なります。

2. If the first response returns the _Preference-Applied: odata.track-changes_ header, you can proceed with synchronizing.

  - Make a second GET request. Specify the _Prefer: odata.track-changes_ header and the _deltaToken_ returned from the first GET to determine if there are any additional events. The second request will return additional events, and either a _skipToken_ if there are more events available, or a _deltaToken_ if the last event has been synchronized, in which case you can stop.

  - Continue synchronizing by sending a GET call and including a _skipToken_ that's returned from the previous call. Stop when you get a final response that contains an _@odata.deltaLink_ header with a _deltaToken_ again, which indicates the sync is complete.

同期のラウンドにおける最初とそれ以降の呼び出しの構文を見てみましょう。

<!-- ==================================== Begin beta content ======================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**既定の予定表で同期するには**

最初の要求： 

```no-highlight
GET https://outlook.office.com/api/beta/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```

2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/beta/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$deltatoken={delta_token}
```

同じラウンドの 3 番目またはそれ以降の要求  

```no-highlight
GET https://outlook.office.com/api/beta/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$skiptoken={skip_token}
```

**特定の予定表で同期するには**

最初の要求：

```no-highlight
GET https://outlook.office.com/api/beta/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```


2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/beta/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$deltatoken={delta_token}
```


同じラウンドの 3 番目またはそれ以降の要求:

```no-highlight
GET https://outlook.office.com/api/beta/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$skiptoken={skip_token}
```

**パラメーター**

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイムゾーン。|
|_URL のパラメーター_|
|user_context|string|ユーザー コンテキスト。現在のユーザーのコンテキストを示すには、値 "me" を使用できます。また、users/{upn} の形式も使用できます。**upn** はユーザー プリンシパル名であり、通常はユーザーの電子メール アドレスです。|
|calendar_id|string|予定表 ID (特定の予定表から予定表ビューを取得している場合)。|
|start_datetime|datetimeoffset|イベントが開始する日時。|
|end_datetime|datetimeoffset|イベントが終了する日時。|
|delta_token|string|以前の同期の応答で、@odata.deltaLink の値の一部として返される deltaToken`deltaToken` 文字列。|
|skip_token|string|以前の同期の応答で、@odata.nextLink の値の一部として返される skipToken`skipToken` 文字列。 |

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names.If the _Prefer: outlook.timezone_ header is not specified, the start and end times are returned in UTC.

**注意** 

- 最初の要求で "Prefer: odata.track-changes" を指定する際に、応答が同期をサポートしている場合は、応答のヘッダーに "Preference-applied: odata.track-changes" が含まれます。
- サポートされていないリソースを同期しようとした場合、またはこれが最初の同期要求でない場合は、応答に
- クエリ パラメーター startdatetime と enddatetime を変更することで、変更タイム ウィンドウを変更できます。  
- 応答内の各イベントにそのプロパティがすべて含まれます。 
- 定期的なアイテムの場合、同期応答には、定期的なマスターおよび例外イベントのイベント全体が返されます。 
- 定期的なアイテムのインスタンスは省略されて、Start および End のプロパティのみが含まれます。残りの発生イベント情報は定期的なマスター イベントから取得できます。参照情報については、「イベント リソース」をご覧ください。 定期的なアイテムのインスタンスは省略されて、Start および End のプロパティのみが含まれます。残りの発生イベント情報は定期的なマスター イベントから取得できます。参照情報については、「イベント リソース」をご覧ください。 See [Event resource](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) for reference information.
- $Filter、$count、$select、$skip、$top、および $search のクエリ パラメーターを使用することはできません。 

**応答の種類**

指定した時間範囲の展開された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)と省略されたイベント。

**例**

ユーザーの既定の予定表を同期する最初と 2 番目の同期要求の例を次に示します。各要求では、一度に 1 つの完全なイベントのみを返すように指定されています。
- 最初の応答は、1 つのイベント、deltaLink`deltaLink` および deltaToken`deltaToken` を返します。 
- The second request uses that `deltatoken`. 最初の応答は、1 つのイベント、deltaLink`nextLink` および deltaToken`skipToken` を返します。 

To complete the sync, use the `skipToken` returned from the previous sync request until the sync response returns a `deltaLink` and `deltaToken`, in which case this round of sync is complete. このような場合、このラウンドの同期は完了しています。次のラウンドの同期の deltaToken`deltaToken` を保存します。 

詳細については、「[Outlook 予定表ビューでイベントを同期する](..\howto\sync-calendar-view.md)」をご参照ください。
 
<a name="SyncCalendarViewSampleInitialRequest"></a>

**最初の要求のサンプル**

```
    GET https://outlook.office.com/api/beta/me/calendarview?startdatetime=2015-01-01T00:00:00Z&enddatetime=2015-04-10T00:00:00Z HTTP/1.1
    Authorization: Bearer <token>
    Prefer: odata.track-changes
    Prefer: odata.maxpagesize=1
    Prefer: outlook.timezone="Pacific Standard Time"
```

**最初の応答データのサンプル**

```
Preference-Applied: odata.track-changes

    {
        "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/CalendarView",
        "value": [
            {
                "@odata.id": "https://outlook.office.com/api/beta/Users('user0@contoso.com')/Events('asdas==')",
                "@odata.etag": "W/\"L8Z+4Y4u7k+97uRKg==\"",
                "Id": "AQMkANJAAAAA==",
                "ChangeKey": "L8Z+AAAAARKg==",
                "Categories": [
                ],
                "DateTimeCreated": "2015-04-10T17:54:49.2725912Z",
                "DateTimeLastModified": "2015-04-10T17:54:49.3038538Z",
                "Subject": "Discuss the Calendar REST API",
                "BodyPreview": "I think it will meet our requirements!",
                "Body": {
                    "ContentType": "HTML",
                    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
                },
                "Importance": "Normal",
                "HasAttachments": false,
                "Start": {
                    "DateTime": "2015-04-05T18:00:00",
                    "TimeZone": "Pacific Standard Time"
                }
                "End": {
                    "DateTime": "2015-04-05T19:00:00",
                    "TimeZone": "Pacific Standard Time"
                }
                "ReminderMinutesBeforeStart": "15",
                "IsReminderOn": "true",
                "Location": {
                    "DisplayName": "",
                    "Address": null,
                    "Coordinates": null,
                    "LocationEmailAddress": "",
                    "LocationUri": ""
                },
                "ResponseStatus": {
                    "Response": "Organizer",
                    "Time": "0001-01-01T00:00:00Z"
                },
                "ShowAs": "Busy",
                "IsAllDay": false,
                "IsCancelled": false,
                "IsOrganizer": true,
                "ResponseRequested": true,
                "Type": "SingleInstance",
                "SeriesMasterId": null,
                "Attendees": [
                ],
                "Recurrence": null,
                "OriginalEndTimeZone": "Pacific Standard Time",
                "OriginalStartTimeZone": "Pacific Standard Time",
                "Organizer": {
                    "EmailAddress": {
                        "Address": "user0@contoso.com",
                        "Name": "user0"
                    }
                },
                "iCalUId": "040000008200E9888E07599CCFA23",
                "WebLink": "https://outlook.office.com/owa/?ItemID=AAAINJAAAAA%3D%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory",
                "OnlineMeetingUrl": null
            }
        ],
        "@odata.deltaLink": "https://outlook.office.com/api/beta/me/calendarview/?startdatetime=2015-01-01T00%3a00%3a00Z&enddatetime=2015-04-10T00%3a00%3a00Z&%24deltatoken=v2%2cH4roCAAA%3d%2c1.0%2cFalse%2cA00%2c"
    }
```

**2 番目の要求のサンプル**

```
    GET https://outlook.office.com/api/beta/me/calendarview?startdatetime=2015-01-01T00:00:00Z&enddatetime=2015-04-10T00:00:00Z&$deltatoken=v2%2cH4roCAAA%3d%2c1.0%2cFalse%2cA00%2c
    Authorization: Bearer <token>
    Prefer: odata.track-changes
    Prefer: odata.maxpagesize=1
    Prefer: outlook.timezone="Pacific Standard Time"
```

<a name="SyncCalendarViewSampleSecondResponse"></a>

**2 番目の応答データのサンプル**

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/CalendarView/$delta",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('user0@contoso.com')/Events('AAMkAD0jAAA=')",
            "@odata.etag": "W/\"P2fd7QAAAAAVFA==\"",
            "Id": "AAMkADNkNmVlOTITVAAAAAA0jAAA=",
            "ChangeKey": "P2fdmIU1QAAAAAVFA==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-15T18:59:11.0226221Z",
            "DateTimeLastModified": "2015-04-15T18:59:11.0694979Z",
            "Subject": "1 hour",
            "BodyPreview": "\u200b",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html><body>content</body></html>"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": {
                "DateTime": "2015-04-16T18:00:00",
                "TimeZone": "Pacific Standard Time"
            }
            "End": {
                "DateTime": "2015-04-16T19:00:00",
                "TimeZone": Pacific Standard Time"
            }
            "ReminderMinutesBeforeStart": "15",
            "IsReminderOn": "true",
            "Location": {
                "DisplayName": "",
                "Address": {
                    "Street": "",
                    "City": "",
                    "State": "",
                    "CountryOrRegion": "",
                    "PostalCode": ""
                },
                "Coordinates": {
                    "Accuracy": "NaN",
                    "Altitude": "NaN",
                    "AltitudeAccuracy": "NaN",
                    "Latitude": "NaN",
                    "Longitude": "NaN"
                },
                "LocationEmailAddress": "",
                "LocationUri": ""
            },
            "ResponseStatus": {
                "Response": "Organizer",
                "Time": "0001-01-01T00:00:00Z"
            },
            "ShowAs": "Busy",
            "IsAllDay": false,
            "IsCancelled": false,
            "IsOrganizer": true,
            "ResponseRequested": true,
            "Type": "SingleInstance",
            "SeriesMasterId": null,
            "Attendees": [
            ],
            "Recurrence": null,
            "OriginalEndTimeZone": "Pacific Standard Time",
            "OriginalStartTimeZone": "Pacific Standard Time",
            "Organizer": {
                "EmailAddress": {
                    "Address": "user0@contoso.com",
                    "Name": "user0"
                }
            },
            "iCalUId": "040000008200E09BB89A316862",
            "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADNkNmVlOAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory",
            "OnlineMeetingUrl": null
        }
    ],
    "@odata.nextLink": "https://outlook.office.com/api/beta/me/calendarview/?startdatetime=2015-01-01T00%3a00%3a00Z&enddatetime=2015-08-10T00%3a00%3a00Z&%24skipToken=530c9d02ae1a4d96804538bd4d381546"
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


**既定の予定表で同期するには**

最初の要求： 

```no-highlight
GET https://outlook.office.com/api/v2.0/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```

2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/v2.0/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$deltatoken={delta_token}
```

同じラウンドの 3 番目またはそれ以降の要求  

```no-highlight
GET https://outlook.office.com/api/v2.0/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$skiptoken={skip_token}
```

**特定の予定表で同期するには**

最初の要求：

```no-highlight
GET https://outlook.office.com/api/v2.0/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```


2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/v2.0/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$deltatoken={delta_token}
```


同じラウンドの 3 番目またはそれ以降の要求:

```no-highlight
GET https://outlook.office.com/api/v2.0/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$skiptoken={skip_token}
```

**パラメーター**

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_context|string|ユーザー コンテキスト。現在のユーザーのコンテキストを示すには、値 "me" を使用できます。また、users/{upn} の形式も使用できます。**upn** はユーザー プリンシパル名であり、通常はユーザーの電子メール アドレスです。|
|calendar_id|string|予定表 ID (特定の予定表から予定表ビューを取得している場合)。|
|start_datetime|datetimeoffset|イベントが開始する日時。|
|end_datetime|datetimeoffset|イベントが終了する日時。|
|delta_token|string|以前の同期の応答で、@odata.deltaLink の値の一部として返される deltaToken`deltaToken` 文字列。|
|skip_token|string|以前の同期の応答で、@odata.nextLink の値の一部として返される skipToken`skipToken` 文字列。 |

**注意** 

- 最初の要求で "Prefer: odata.track-changes" を指定する際に、応答が同期をサポートしている場合は、応答のヘッダーに "Preference-applied: odata.track-changes" が含まれます。
- サポートされていないリソースを同期しようとした場合、またはこれが最初の同期要求でない場合は、応答に
- クエリ パラメーター startdatetime と enddatetime を変更することで、変更タイム ウィンドウを変更できます。  
- 応答内の各イベントにそのプロパティがすべて含まれます。 
- 定期的なアイテムの場合、同期応答には、定期的なマスターおよび例外イベントのイベント全体が返されます。 
- 定期的なアイテムのインスタンスは省略されて、Start および End のプロパティのみが含まれます。残りの発生イベント情報は定期的なマスター イベントから取得できます。参照情報については、「イベント リソース」をご覧ください。 定期的なアイテムのインスタンスは省略されて、Start および End のプロパティのみが含まれます。残りの発生イベント情報は定期的なマスター イベントから取得できます。参照情報については、「イベント リソース」をご覧ください。 See [Event resource](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) for reference information.
- $Filter、$count、$select、$skip、$top、および $search のクエリ パラメーターを使用することはできません。 

**応答の種類**

指定した時間範囲の展開された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)と省略されたイベント。

**例**

ユーザーの既定の予定表を同期する最初と 2 番目の同期要求の例を次に示します。各要求では、一度に 1 つの完全なイベントのみを返すように指定されています。
- 最初の応答は、1 つのイベント、deltaLink`deltaLink` および deltaToken`deltaToken` を返します。 
- The second request uses that `deltatoken`. 最初の応答は、1 つのイベント、deltaLink`nextLink` および deltaToken`skipToken` を返します。 

To complete the sync, use the `skipToken` returned from the previous sync request until the sync response returns a `deltaLink` and `deltaToken`, in which case this round of sync is complete. このような場合、このラウンドの同期は完了しています。次のラウンドの同期の deltaToken`deltaToken` を保存します。 

詳細については、「[Outlook 予定表ビューでイベントを同期する](..\howto\sync-calendar-view.md)」をご参照ください。
 
<a name="SyncCalendarViewSampleInitialRequest"></a>

**最初の要求のサンプル**

```
    GET https://outlook.office.com/api/v2.0/me/calendarview?startdatetime=2015-01-01T00:00:00Z&enddatetime=2015-04-10T00:00:00Z HTTP/1.1
    Authorization: Bearer <token>
    Prefer: odata.track-changes
    Prefer: odata.maxpagesize=1
```

**最初の応答データのサンプル**

```
Preference-Applied: odata.track-changes

    {
        "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/CalendarView",
        "value": [
            {
                "@odata.id": "https://outlook.office.com/api/v2.0/Users('user0@contoso.com')/Events('asdas==')",
                "@odata.etag": "W/\"L8Z+4Y4u7k+97uRKg==\"",
                "Id": "AQMkANJAAAAA==",
                "ChangeKey": "L8Z+AAAAARKg==",
                "Categories": [
                ],
                "DateTimeCreated": "2015-04-10T17:54:49.2725912Z",
                "DateTimeLastModified": "2015-04-10T17:54:49.3038538Z",
                "Subject": "Discuss the Calendar REST API",
                "BodyPreview": "I think it will meet our requirements!",
                "Body": {
                    "ContentType": "HTML",
                    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
                },
                "Importance": "Normal",
                "HasAttachments": false,
                "Start": {
                    "DateTime": "2015-04-05T18:00:00",
                    "TimeZone": "Pacific Standard Time"
                }
                "End": {
                    "DateTime": "2015-04-05T19:00:00",
                    "TimeZone": "Pacific Standard Time"
                }
                "ReminderMinutesBeforeStart": "15",
                "IsReminderOn": "true",
                "Location": {
                    "DisplayName": "",
                    "Address": null
                },
                "ResponseStatus": {
                    "Response": "Organizer",
                    "Time": "0001-01-01T00:00:00Z"
                },
                "ShowAs": "Busy",
                "IsAllDay": false,
                "IsCancelled": false,
                "IsOrganizer": true,
                "ResponseRequested": true,
                "Type": "SingleInstance",
                "SeriesMasterId": null,
                "Attendees": [
                ],
                "Recurrence": null,
                "OriginalEndTimeZone": "Pacific Standard Time",
                "OriginalStartTimeZone": "Pacific Standard Time",
                "Organizer": {
                    "EmailAddress": {
                        "Address": "user0@contoso.com",
                        "Name": "user0"
                    }
                },
                "iCalUId": "040000008200E9888E07599CCFA23",
                "WebLink": "https://outlook.office.com/owa/?ItemID=AAAINJAAAAA%3D%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
            }
        ],
        "@odata.deltaLink": "https://outlook.office.com/api/v2.0/me/calendarview/?startdatetime=2015-01-01T00%3a00%3a00Z&enddatetime=2015-04-10T00%3a00%3a00Z&%24deltatoken=v2%2cH4roCAAA%3d%2c1.0%2cFalse%2cA00%2c"
    }
```

**2 番目の要求のサンプル**

```
    GET https://outlook.office.com/api/v2.0/me/calendarview?startdatetime=2015-01-01T00:00:00Z&enddatetime=2015-04-10T00:00:00Z&$deltatoken=v2%2cH4roCAAA%3d%2c1.0%2cFalse%2cA00%2c
    Authorization: Bearer <token>
    Prefer: odata.track-changes
    Prefer: odata.maxpagesize=1
```

<a name="SyncCalendarViewSampleSecondResponse"></a>

**2 番目の応答データのサンプル**

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/CalendarView/$delta",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('user0@contoso.com')/Events('AAMkAD0jAAA=')",
            "@odata.etag": "W/\"P2fd7QAAAAAVFA==\"",
            "Id": "AAMkADNkNmVlOTITVAAAAAA0jAAA=",
            "ChangeKey": "P2fdmIU1QAAAAAVFA==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-15T18:59:11.0226221Z",
            "DateTimeLastModified": "2015-04-15T18:59:11.0694979Z",
            "Subject": "1 hour",
            "BodyPreview": "\u200b",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html><body>content</body></html>"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": {
                "DateTime": "2015-04-16T18:00:00",
                "TimeZone": "Pacific Standard Time"
            }
            "End": {
                "DateTime": "2015-04-16T19:00:00",
                "TimeZone": Pacific Standard Time"
            }
            "ReminderMinutesBeforeStart": "15",
            "IsReminderOn": "true",
            "Location": {
                "DisplayName": "",
                "Address": {
                    "Street": "",
                    "City": "",
                    "State": "",
                    "CountryOrRegion": "",
                    "PostalCode": ""
                }
             },
            "ResponseStatus": {
                "Response": "Organizer",
                "Time": "0001-01-01T00:00:00Z"
            },
            "ShowAs": "Busy",
            "IsAllDay": false,
            "IsCancelled": false,
            "IsOrganizer": true,
            "ResponseRequested": true,
            "Type": "SingleInstance",
            "SeriesMasterId": null,
            "Attendees": [
            ],
            "Recurrence": null,
            "OriginalEndTimeZone": "Pacific Standard Time",
            "OriginalStartTimeZone": "Pacific Standard Time",
            "Organizer": {
                "EmailAddress": {
                    "Address": "user0@contoso.com",
                    "Name": "user0"
                }
            },
            "iCalUId": "040000008200E09BB89A316862",
            "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADNkNmVlOAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        }
    ],
    "@odata.nextLink": "https://outlook.office.com/api/v2.0/me/calendarview/?startdatetime=2015-01-01T00%3a00%3a00Z&enddatetime=2015-08-10T00%3a00%3a00Z&%24skipToken=530c9d02ae1a4d96804538bd4d381546"
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->

<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


**既定の予定表で同期するには**

最初の要求： 

```no-highlight
GET https://outlook.office.com/api/v1.0/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```

2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/v1.0/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$deltatoken={delta_token}
```

同じラウンドの 3 番目またはそれ以降の要求  

```no-highlight
GET https://outlook.office.com/api/v1.0/{user_context}/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$skiptoken={skip_token}
```

**特定の予定表で同期するには**

最初の要求：

```no-highlight
GET https://outlook.office.com/api/v1.0/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}
```


2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/v1.0/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$deltatoken={delta_token}
```


同じラウンドの 3 番目またはそれ以降の要求:

```no-highlight
GET https://outlook.office.com/api/v1.0/{user_context}/calendars('{calendar_id}')/calendarview?startDateTime={start_datetime}&endDateTime={end_datetime}&$skiptoken={skip_token}
```

**パラメーター**

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_context|string|ユーザー コンテキスト。現在のユーザーのコンテキストを示すには、値 "me" を使用できます。また、users/{upn} の形式も使用できます。**upn** はユーザー プリンシパル名であり、通常はユーザーの電子メール アドレスです。|
|calendar_id|string|予定表 ID (特定の予定表から予定表ビューを取得している場合)。|
|start_datetime|datetimeoffset|イベントが開始する日時。|
|end_datetime|datetimeoffset|イベントが終了する日時。|
|delta_token|string|以前の同期の応答で、@odata.deltaLink の値の一部として返される deltaToken`deltaToken` 文字列。|
|skip_token|string|以前の同期の応答で、@odata.nextLink の値の一部として返される skipToken`skipToken` 文字列。 |

**注意** 

- 最初の要求で "Prefer: odata.track-changes" を指定する際に、応答が同期をサポートしている場合は、応答のヘッダーに "Preference-applied: odata.track-changes" が含まれます。
- サポートされていないリソースを同期しようとした場合、またはこれが最初の同期要求でない場合は、応答に
- クエリ パラメーター startdatetime と enddatetime を変更することで、変更タイム ウィンドウを変更できます。  
- 応答内の各イベントにそのプロパティがすべて含まれます。 
- 定期的なアイテムの場合、同期応答には、定期的なマスターおよび例外イベントのイベント全体が返されます。 
- 定期的なアイテムのインスタンスは省略されて、Start および End のプロパティのみが含まれます。残りの発生イベント情報は定期的なマスター イベントから取得できます。参照情報については、「イベント リソース」をご覧ください。 定期的なアイテムのインスタンスは省略されて、Start および End のプロパティのみが含まれます。残りの発生イベント情報は定期的なマスター イベントから取得できます。参照情報については、「イベント リソース」をご覧ください。 See [Event resource](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) for reference information.
- $Filter、$count、$select、$skip、$top、および $search のクエリ パラメーターを使用することはできません。 

**応答の種類**

指定した時間範囲の展開された[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)と省略されたイベント。

**例**

ユーザーの既定の予定表を同期する最初と 2 番目の同期要求の例を次に示します。各要求では、一度に 1 つの完全なイベントのみを返すように指定されています。
- 最初の応答は、1 つのイベント、deltaLink`deltaLink` および deltaToken`deltaToken` を返します。 
- The second request uses that `deltatoken`. 最初の応答は、1 つのイベント、deltaLink`nextLink` および deltaToken`skipToken` を返します。 

To complete the sync, use the `skipToken` returned from the previous sync request until the sync response returns a `deltaLink` and `deltaToken`, in which case this round of sync is complete. このような場合、このラウンドの同期は完了しています。次のラウンドの同期の deltaToken`deltaToken` を保存します。 

詳細については、「[Outlook 予定表ビューでイベントを同期する](..\howto\sync-calendar-view.md)」をご参照ください。
 
<a name="SyncCalendarViewSampleInitialRequest"></a>

**最初の要求のサンプル**

```
    GET https://outlook.office.com/api/v1.0/me/calendarview?startdatetime=2015-01-01T00:00:00Z&enddatetime=2015-04-10T00:00:00Z HTTP/1.1
    Authorization: Bearer <token>
    Prefer: odata.track-changes
    Prefer: odata.maxpagesize=1
```

**最初の応答データのサンプル**

```
Preference-Applied: odata.track-changes

    {
        "@odata.context": "https://outlook.office.com/api/v1.0/$metadata#Me/CalendarView",
        "value": [
            {
                "@odata.id": "https://outlook.office.com/api/v1.0/Users('user0@contoso.com')/Events('asdas==')",
                "@odata.etag": "W/\"L8Z+4Y4u7k+97uRKg==\"",
                "Id": "AQMkANJAAAAA==",
                "ChangeKey": "L8Z+AAAAARKg==",
                "Categories": [
                ],
                "DateTimeCreated": "2015-04-10T17:54:49.2725912Z",
                "DateTimeLastModified": "2015-04-10T17:54:49.3038538Z",
                "Subject": "Discuss the Calendar REST API",
                "BodyPreview": "I think it will meet our requirements!",
                "Body": {
                    "ContentType": "HTML",
                    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
                },
                "Importance": "Normal",
                "HasAttachments": false,
                "Start": "2015-04-05T18:00:00Z",
                "StartTimeZone": "Pacific Standard Time",
                "End": "2015-04-05T19:00:00Z",
                "EndTimeZone": "Pacific Standard Time",
                "Reminder": 15,
                "Location": {
                    "DisplayName": "",
                    "Address": null
                },
                "ResponseStatus": {
                    "Response": "Organizer",
                    "Time": "0001-01-01T00:00:00Z"
                },
                "ShowAs": "Busy",
                "IsAllDay": false,
                "IsCancelled": false,
                "IsOrganizer": true,
                "ResponseRequested": true,
                "Type": "SingleInstance",
                "SeriesMasterId": null,
                "Attendees": [
                ],
                "Recurrence": null,
                "Organizer": {
                    "EmailAddress": {
                        "Address": "user0@contoso.com",
                        "Name": "user0"
                    }
                },
                "iCalUId": "040000008200E9888E07599CCFA23",
                "WebLink": "https://outlook.office.com/owa/?ItemID=AAAINJAAAAA%3D%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
            }
        ],
        "@odata.deltaLink": "https://outlook.office.com/api/v1.0/me/calendarview/?startdatetime=2015-01-01T00%3a00%3a00Z&enddatetime=2015-04-10T00%3a00%3a00Z&%24deltatoken=v2%2cH4roCAAA%3d%2c1.0%2cFalse%2cA00%2c"
    }
```

**2 番目の要求のサンプル**

```
    GET https://outlook.office.com/api/v1.0/me/calendarview?startdatetime=2015-01-01T00:00:00Z&enddatetime=2015-04-10T00:00:00Z&$deltatoken=v2%2cH4roCAAA%3d%2c1.0%2cFalse%2cA00%2c
    Authorization: Bearer <token>
    Prefer: odata.track-changes
    Prefer: odata.maxpagesize=1
```

<a name="SyncCalendarViewSampleSecondResponse"></a>

**2 番目の応答データのサンプル**

```
{
    "@odata.context": "https://outlook.office.com/api/v1.0/$metadata#Me/CalendarView/$delta",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v1.0/Users('user0@contoso.com')/Events('AAMkAD0jAAA=')",
            "@odata.etag": "W/\"P2fd7QAAAAAVFA==\"",
            "Id": "AAMkADNkNmVlOTITVAAAAAA0jAAA=",
            "ChangeKey": "P2fdmIU1QAAAAAVFA==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-15T18:59:11.0226221Z",
            "DateTimeLastModified": "2015-04-15T18:59:11.0694979Z",
            "Subject": "1 hour",
            "BodyPreview": "\u200b",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html><body>content</body></html>"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-04-16T18:00:00Z",
            "StartTimeZone": "Pacific Standard Time",
            "End": "2015-04-16T19:00:00Z",
            "EndTimeZone": "Pacific Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "",
                "Address": {
                    "Street": "",
                    "City": "",
                    "State": "",
                    "CountryOrRegion": "",
                    "PostalCode": ""
                }
            },
            "ResponseStatus": {
                "Response": "Organizer",
                "Time": "0001-01-01T00:00:00Z"
            },
            "ShowAs": "Busy",
            "IsAllDay": false,
            "IsCancelled": false,
            "IsOrganizer": true,
            "ResponseRequested": true,
            "Type": "SingleInstance",
            "SeriesMasterId": null,
            "Attendees": [
            ],
            "Recurrence": null,
            "Organizer": {
                "EmailAddress": {
                    "Address": "user0@contoso.com",
                    "Name": "user0"
                }
            },
            "iCalUId": "040000008200E09BB89A316862",
            "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkADNkNmVlOAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        }
    ],
    "@odata.nextLink": "https://outlook.office.com/api/v1.0/me/calendarview/?startdatetime=2015-01-01T00%3a00%3a00Z&enddatetime=2015-08-10T00%3a00%3a00Z&%24skipToken=530c9d02ae1a4d96804538bd4d381546"
}
```


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****

<a name="FindMeetingTimesPreview"></a>
## 会議の日時を検索する (プレビュー)

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

パラメーターとして指定された開催者と出席者の空き時間、および時間または場所の制約に基づいて、会議時間の提案を検索します。 

この操作は、現在プレビュー段階であり、ベータ版のみで利用可能です。 It applies to only Office 365 mailboxes (on Azure AD) and not to Microsoft accounts.

```no-highlight
POST https://outlook.office.com/api/{version}/me/findmeetingtimes
```

All the supported parameters are listed below. サポートされているすべてのパラメーターは以下のとおりです。シナリオに応じて、**FindMeetingTimes** アクションの要求本文で必要なパラメーターを指定します。 

|**パラメーター**|**型**|**説明**|**必須**|
|:-----|:-----|:-----|:-----|
| 出席者 | Collection([AttendeeBase](complex-types-for-mail-contacts-calendar.md#AttendeeBase)) | Attendees or resources for the meeting. 会議の出席者またはリソース。コレクションを空にすると、**FindMeetingTimes** は開催者のみの空き時間帯を検索します。 | 省略可 |
| LocationConstraint | [LocationConstraint](complex-types-for-mail-contacts-calendar.md#LocationConstraint) | 会議の場所の提案が必要かどうか、または会議のみが開催できる特定の場所があるか、など、会議の場所に関する開催者の要件。 | 省略可 |
| TimeConstraint | [TimeConstraint](complex-types-for-mail-contacts-calendar.md#TimeConstraint) | 会議を開催する開始時刻と終了時刻の範囲。 | 省略可 |
| MeetingDuration | Edm.Duration |ISO 8601 形式 (期間) で表された会議の長さ。たとえば、PT1H`PT1H` は 1 時間を表します。会議の期間を指定しない場合、FindMeetingTimes は既定値の 30 分を使用します。 ISO 8601 形式 (期間) で表された会議の長さ。たとえば、PT1H は 1 時間を表します。会議の期間を指定しない場合、**FindMeetingTimes** は既定値の 30 分を使用します。 | 省略可 |
| MaxCandidates | Edm.Int32 |応答で返す会議提案の最大数です。 | 省略可 |
| IsOrganizerOptional | Edm.Boolean | 開催者が必ずしも出席する必要がない場合は、true`true` を指定します。既定値は false です。 既定値は、長さ 0 の文字列 ("") です。 | 省略可 |
| ReturnSuggestionHints | Edm.Boolean | SuggestionHint プロパティで各会議提案の理由を返すには、true を指定します。既定値は false であり、そのプロパティを返しません。 The default is `false` to not return that property. | 省略可 |

指定したパラメーターに基づいて、**FindMeetingTimes** は開催者と出席者の標準予定表で空き時間情報を確認します。アクションは、最も開催可能な会議の日時を計算し、会議の提案を返します。

**応答の種類**

会議提案のコレクションを含む [MeetingTimeCandidatesResult](complex-types-for-mail-contacts-calendar.md#MeetingTimeCandidatesResult)、[MeetingTimeCandidate](#MeetingTimeCandidate) の各種類、および **EmptySuggestionsHint** プロパティ。

<!-- Location suggestions are based on the organizer's booking history during the past 7 days and next 2 days. If the organizer doesn't have sufficient booking history 
in the corresponding time window, the request returns HTTP 200 OK, but does not include any meeting suggestions in the response. -->

Each suggestion is defined as a [MeetingTimeCandidate](complex-types-for-mail-contacts-calendar.md#MeetingTimeCandidate), with attendees having [on the average a confidence level of 50% chance or higher to attend](complex-types-for-mail-contacts-calendar.md#ConfidenceScoreDetails). By default, each meeting time suggestion is returned in UTC. Apply the `Prefer: outlook.timezone` request header to have meeting time suggestions retuned in a different time zone, for example:

```no-highlight
Prefer: outlook.timezone="Pacific Standard Time"
```

**FindMeetingTimes** が会議提案を返すことができない場合は、応答で、**EmptySuggestionsHint** プロパティに理由が示されます。この値に基づいて、パラメーターをさらに調整して、**FindMeetingTimes** を再度呼び出すことができます。


**注意**

現時点では、**FindMeetingTimes** は以下を前提としています。

- Any [Attendee](..\api\complex-types-for-mail-contacts-calendar.md#Attendee) who is a person (as opposed to a resource) is a required attendee. 人である (リソースとは異なる) 参加者は、必須の出席者です。そのため、**Attendees** コレクション パラメーターの一部として、対応する **Type** プロパティで人に Required`Required` を、リソースに Resource`Resource` を指定します。
- 会議は、開催者または出席者の勤務時間内のみで提案されます。[TimeConstraint](..\api\complex-types-for-mail-contacts-calendar.md#TimeConstraint) の **ActivityDomain** プロパティの指定は無視することができます 

以下のそれぞれの例は、**FindMeetingTimes** を呼び出しますが、下記のように出席者の空き時間、時間と場所の制約によって異なります。

- [出席者と会議を開催する時間と場所を検索する (REST)](#FindTimeToMeet) 
- [既知の場所で会議を開催する時間を検索し、各提案の理由を取得する (REST)](#FindTimeToMeetAtKnownLocation) 
- [会議を開催する時間を検索したが、出席可能な出席者がいない (REST)](#FindTimeToMeetButNobodyAvailable) 
- [会議を開催する時間を検索したが、一部の出席者のみが出席可能である (REST)](#FindTimeToMeetSomeAvailable)
- [サインイン中のユーザーの空き時間帯を検索する (REST)](#FindFreeSlots)


<a name="FindTimeToMeet"></a>

### 特定の出席者と会議を開催する時間と場所を検索する (REST)

要求本文で次のパラメーターを指定して、会議の時間と場所を検索します。
- **出席者**
- **TimeConstraint**
- **MeetingDuration**

**要求のサンプル**

次の例では、要求されている会議時間の範囲と要求されている時間の長さに基づき、開催者と出席者の空き時間を考慮して会議の時間と場所を提案しています。

```
POST https://outlook.office.com/api/beta/me/findmeetingtimes

Prefer: outlook.timezone="Pacific Standard Time"
Content-Type: application/json 

{ 
  "Attendees": [ 
    { 
      "Type": "Required",  
      "EmailAddress": { 
        "Address": "fannyd@prosewareltd.onmicrosoft.com" 
      } 
    } 
  ],  
  "TimeConstraint": { 
    "Timeslots": [ 
       { 
        "Start": { 
          "Date": "2016-05-20",  
          "Time": "7:00:00",  
          "TimeZone": "Pacific Standard Time" 
        },  
        "End": { 
          "Date": "2016-05-20",  
          "Time": "17:00:00",  
          "TimeZone": "Pacific Standard Time" 
        } 
      } 
    ] 
  },  
  "MeetingDuration": "PT1H" 
} 
```



**応答のサンプル**

状態コード :200

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Microsoft.OutlookServices.MeetingTimeCandidatesResult",
    "MeetingTimeSlots": [
        {
            "MeetingTimeSlot": {
                "Start": {
                    "Date": "2016-05-20",
                    "Time": "10:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                },
                "End": {
                    "Date": "2016-05-20",
                    "Time": "11:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                }
            },
            "Confidence": 100.0,
            "OrganizerAvailability": "Free",
            "AttendeeAvailability": [
                {
                    "Attendee": {
                        "Type": "Required",
                        "EmailAddress": {
                            "Address": "fannyd@prosewareltd.onmicrosoft.com"
                        }
                    },
                    "Availability": "Free"
                }
            ],
            "Locations": [
               {
                    "DisplayName": "Tokyo conference room",
                    "LocationEmailAddress": "",
                    "LocationUri": "",
                    "Address": null,
                    "Coordinates": null
                }
            ]
        },
        {
            "MeetingTimeSlot": {
                "Start": {
                    "Date": "2016-05-20",
                    "Time": "11:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                },
                "End": {
                    "Date": "2016-05-20",
                    "Time": "12:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                }
            },
            "Confidence": 100.0,
            "OrganizerAvailability": "Free",
            "AttendeeAvailability": [
                {
                    "Attendee": {
                        "Type": "Required",
                        "EmailAddress": {
                            "Address": "fannyd@prosewareltd.onmicrosoft.com"
                        }
                    },
                    "Availability": "Free"
                }
            ],
            "Locations": [
               {
                    "DisplayName": "Paris conference room",
                    "LocationEmailAddress": "",
                    "LocationUri": "",
                    "Address": null,
                    "Coordinates": null
                }
            ]
        }
    ],
    "EmptySuggestionsHint": ""
}
```


****

<a name="FindTimeToMeetAtKnownLocation"></a>

### 既知の場所で会議を開催する時間を検索し、各提案の理由を取得する (REST)

要求本体で次のパラメーターを指定して、あらかじめ決められた会議を開催する時間を検索し、各提案の理由を要求します。
- **出席者**
- **LocationConstraint**
- **TimeConstraint**
- **MeetingDuration**
- **ReturnSuggestionHints**

**FindMeetingTimes** が任意の提案を返す場合は、**ReturnSuggestionHints** パラメーターを設定することで、**SuggestionHint** プロパティの各提案の説明も取得できます。


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/findmeetingtimes

Prefer: outlook.timezone="Pacific Standard Time"
Content-Type: application/json 

{ 
  "Attendees": [ 
    { 
      "Type": "Required",  
      "EmailAddress": { 
        "Address": "fannyd@prosewareltd.onmicrosoft.com" 
      } 
    } 
  ],  
  "LocationConstraint": { 
    "IsRequired": "false",  
    "SuggestLocation": "false",  
    "Locations": [ 
      { 
        "DisplayName": "Conf room Hood" 
      } 
    ] 
  },  
  "TimeConstraint": { 
    "Timeslots": [ 
      { 
        "Start": { 
          "Date": "2016-05-20",  
          "Time": "7:00:00",  
          "TimeZone": "Pacific Standard Time" 
        },  
        "End": { 
          "Date": "2016-05-20",  
          "Time": "17:00:00",  
          "TimeZone": "Pacific Standard Time" 
        } 
      } 
    ] 
  },  
  "MeetingDuration": "PT2H",
  "ReturnSuggestionHints": "true"
} 
```



**応答のサンプル**

状態コード :200

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Microsoft.OutlookServices.MeetingTimeCandidatesResult",
    "MeetingTimeSlots": [
        {
            "MeetingTimeSlot": {
                "Start": {
                    "Date": "2016-05-20",
                    "Time": "10:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                },
                "End": {
                    "Date": "2016-05-20",
                    "Time": "12:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                }
            },
            "Confidence": 100.0,
            "OrganizerAvailability": "Free",
            "AttendeeAvailability": [
                {
                    "Attendee": {
                        "Type": "Required",
                        "EmailAddress": {
                            "Address": "fannyd@prosewareltd.onmicrosoft.com"
                        }
                    },
                    "Availability": "Free"
                }
            ],
            "Locations": [
                {
                    "DisplayName": "Conf room Hood"
                }
            ],
            "SuggestionHint": "Suggested because it is one of the nearest times when all attendees are available."
        }
    ],
    "EmptySuggestionsHint": ""
}
```


****

<a name="FindTimeToMeetButNobodyAvailable"></a>

### 会議を開催する時間を検索したが、出席可能な出席者がいない (REST)

要求本体で次のパラメーターを指定して、あらかじめ決められた会議を開催する時間を検索します。
- **出席者**
- **LocationConstraint**
- **TimeConstraint**
- **MeetingDuration**

この例では、指定したパラメーターと出席者の空き時間に基づいて、**FindMeetingTimes** は提案を 1 つも返すことができず、代わりに EmptySuggestionsHint プロパティの理由 AttendeesUnavailable を返します。 

「[会議提案が 1 つも返されないことの考えられる他の理由](..\api\complex-types-for-mail-contacts-calendar.md#ReasonsNoMeetingSuggestion)」をご参照ください。


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/findmeetingtimes

Prefer: outlook.timezone="Pacific Standard Time"
Content-Type: application/json 

{ 
  "Attendees": [ 
    { 
      "Type": "Required",  
      "EmailAddress": { 
        "Address": "fannyd@prosewareltd.onmicrosoft.com" 
      } 
    } 
  ],  
  "LocationConstraint": { 
    "IsRequired": "false",  
    "SuggestLocation": "false",  
    "Locations": [ 
      { 
        "DisplayName": "Conf room Hood" 
      } 
    ] 
  },  
  "TimeConstraint": { 
    "Timeslots": [ 
      { 
        "Start": { 
          "Date": "2016-05-20",  
          "Time": "7:00:00",  
          "TimeZone": "Pacific Standard Time" 
        },  
        "End": { 
          "Date": "2016-05-20",  
          "Time": "14:00:00",  
          "TimeZone": "Pacific Standard Time" 
        } 
      } 
    ] 
  },  
  "MeetingDuration": "PT2H" 
}
```



**応答のサンプル**

状態コード :200

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Microsoft.OutlookServices.MeetingTimeCandidatesResult",
    "MeetingTimeSlots": [
    ],
    "EmptySuggestionsHint": "AttendeesUnavailable"
}
```

****

<a name="FindTimeToMeetSomeAvailable"></a>
### 会議を開催する時間を検索したが、一部の出席者のみが出席可能である (REST)

要求本体で次のパラメーターを指定して、あらかじめ決められた会議を開催する時間を検索します。
- **出席者**
- **LocationConstraint**
- **TimeConstraint**
- **MeetingDuration**
- **ReturnSuggestionHints**

この例では、2 人の出席者のうち 1 人だけが出席できます。FindMeetingTimes が返すそれぞれの会議提案には、次が含まれています。 Each meeting suggestion that **FindMeetingTimes** returns includes:
- 各出席者の空き時間情報
- 計算された会議の確度 50%
- **ReturnSuggestionHints** パラメーターの設定以降の **SuggestionHInt**。 

詳しくは、「[会議の確度](..\api\complex-types-for-mail-contacts-calendar.md#ConfidenceScoreDetails)」をご覧ください。


**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/findmeetingtimes

Prefer: outlook.timezone="Pacific Standard Time"
Content-Type: application/json 

{ 
  "Attendees": [ 
    { 
      "Type": "Required",  
      "EmailAddress": { 
        "Address": "fannyd@prosewareltd.onmicrosoft.com" 
      } 
    },
   { 
      "Type": "Optional",  
      "EmailAddress": { 
        "Address": "danas@prosewareltd.onmicrosoft.com" 
      } 
    } 
  ],  
  "LocationConstraint": { 
    "IsRequired": "false",  
    "SuggestLocation": "false",  
    "Locations": [ 
      { 
        "DisplayName": "Conf room Hood" 
      } 
    ] 
  },  
  "TimeConstraint": { 
    "Timeslots": [ 
      { 
        "Start": { 
          "Date": "2016-05-20",  
          "Time": "9:00:00",  
          "TimeZone": "Pacific Standard Time" 
        },  
        "End": { 
          "Date": "2016-05-20",  
          "Time": "17:00:00",  
          "TimeZone": "Pacific Standard Time" 
        } 
      } 
    ] 
  },  
  "MeetingDuration": "PT1H",
  "ReturnSuggestionHints": "true"
}
```



**応答のサンプル**

状態コード :200
```
{
   "@odata.context":"https://outlook.office.com/api/beta/$metadata#Microsoft.OutlookServices.MeetingTimeCandidatesResult",
   "MeetingTimeSlots":[
      {
         "MeetingTimeSlot":{
            "Start":{
               "Date":"2016-05-20",
               "Time":"10:00:00.0000000",
               "TimeZone":"Pacific Standard Time"
            },
            "End":{
               "Date":"2016-05-20",
               "Time":"11:00:00.0000000",
               "TimeZone":"Pacific Standard Time"
            }
         },
         "Confidence":50.0,
         "OrganizerAvailability":"Free",
         "AttendeeAvailability":[
            {
               "Attendee":{
                  "Type":"Required",
                  "EmailAddress":{
                     "Address":"fannyd@prosewareltd.onmicrosoft.com"
                  }
               },
               "Availability":"Free"
            },
            {
               "Attendee":{
                  "Type":"Required",
                  "EmailAddress":{
                     "Address":"danas@prosewareltd.onmicrosoft.com"
                  }
               },
               "Availability":"Busy"
            }
         ],
         "Locations":[
            {
               "DisplayName":"Conf room Hood"
            }
         ],
         "SuggestionHint":"Suggested because it is one of the nearest times when most attendees are available."
      },
      {
         "MeetingTimeSlot":{
            "Start":{
               "Date":"2016-05-20",
               "Time":"11:00:00.0000000",
               "TimeZone":"Pacific Standard Time"
            },
            "End":{
               "Date":"2016-05-20",
               "Time":"12:00:00.0000000",
               "TimeZone":"Pacific Standard Time"
            }
         },
         "Confidence":50.0,
         "OrganizerAvailability":"Free",
         "AttendeeAvailability":[
            {
               "Attendee":{
                  "Type":"Required",
                  "EmailAddress":{
                     "Address":"fannyd@prosewareltd.onmicrosoft.com"
                  }
               },
               "Availability":"Free"
            },
            {
               "Attendee":{
                  "Type":"Required",
                  "EmailAddress":{
                     "Address":"danas@prosewareltd.onmicrosoft.com"
                  }
               },
               "Availability":"Busy"
            }
         ],
         "Locations":[
            {
               "DisplayName":"Conf room Hood"
            }
         ],
         "SuggestionHint":"Suggested because it is one of the nearest times when most attendees are available."
      }
   ],
   "EmptySuggestionsHint":""
}
```

****

<a name="FindFreeSlots"></a>
###サインイン中のユーザーのみの空き時間帯を検索する (REST)

要求本文で次のパラメーターを指定して、日付範囲でサインインしているユーザーの標準予定表の空き時間帯を検索します。

- **TimeConstraint**
- **MeetingDuration**

**要求のサンプル:**

この例では、**TimeConstraint** で指定された期間内のサインインしているユーザーの標準予定表で、**MeetingDuration** で指定されたように、1 時間の空き時間帯を探します。

```
POST https://outlook.office.com/api/beta/me/findmeetingtimes

Prefer: outlook.timezone="Pacific Standard Time"
Content-Type: application/json 

{ 
  "Attendees": [],
  "TimeConstraint": { 
    "Timeslots": [ 
      { 
        "Start": { 
          "Date": "2016-05-20",  
          "Time": "7:00:00",  
          "TimeZone": "Pacific Standard Time" 
        },  
        "End": { 
          "Date": "2016-05-20",  
          "Time": "17:00:00",  
          "TimeZone": "Pacific Standard Time" 
        } 
      } 
    ] 
  },  
  "MeetingDuration": "PT1H"
}
```



**応答のサンプル**

状態コード :200
```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Microsoft.OutlookServices.MeetingTimeCandidatesResult",
    "MeetingTimeSlots": [
        {
            "MeetingTimeSlot": {
                "Start": {
                    "Date": "2016-05-20",
                    "Time": "08:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                },
                "End": {
                    "Date": "2016-05-20",
                    "Time": "09:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                }
            },
            "Confidence": 100.0,
            "OrganizerAvailability": "Free",
            "AttendeeAvailability": [
            ],
            "Locations": [
            ]
        },
        {
            "MeetingTimeSlot": {
                "Start": {
                    "Date": "2016-05-20",
                    "Time": "09:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                },
                "End": {
                    "Date": "2016-05-20",
                    "Time": "10:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                }
            },
            "Confidence": 100.0,
            "OrganizerAvailability": "Free",
            "AttendeeAvailability": [
            ],
            "Locations": [
            ]
        },
        {
            "MeetingTimeSlot": {
                "Start": {
                    "Date": "2016-05-20",
                    "Time": "12:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                },
                "End": {
                    "Date": "2016-05-20",
                    "Time": "13:00:00.0000000",
                    "TimeZone": "Pacific Standard Time"
                }
            },
            "Confidence": 100.0,
            "OrganizerAvailability": "Free",
            "AttendeeAvailability": [
            ],
            "Locations": [
            ]
        }
    ],
    "EmptySuggestionsHint": ""
}
```

****


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

<a name="CreateEvents"> </a>
## イベントを作成する

REST API:[予定表イベントを作成する](#CreateAnEvent)

クライアント ライブラリ:[予定表イベントを作成する (クライアント)](#CreateEventsClient)

<a name="CreateAnEvent"></a>
###予定表イベントを作成する  (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

予定表の events`events` エンドポイントに投稿することによって、ユーザーの標準として設定されている予定表または特定の予定表にイベントを作成します。イベントが作成されると、サーバーはすべての出席者に招待状を送信します。 When the event is created, the server send invitations to all attendees.

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/events
POST https://outlook.office.com/api/beta/me/calendars/{calendar_id}/events
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|

**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/events
Content-Type: application/json
```

```
{
  "Subject": "Discuss the Calendar REST API",
  "Body": {
    "ContentType": "HTML",
    "Content": "I think it will meet our requirements!"
  },
  "Start": {
      "DateTime": "2014-02-02T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2014-02-02T19:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "janets@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Janet Schorr"
      },
      "Type": "Required"
    }
  ]
}
```

**応答のサンプル**

状態コード :201

```
{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGE4v1RAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeNheA==\"",
  "Id": "AAMkAGE4v1RAAA=",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeNheA==",
  "Categories": [],
  "CreatedDateTime": "2014-01-22T20:56:10.1058291Z",
  "LastModifiedDateTime": "2014-01-22T20:56:10.3402186Z",
  "Subject": "Discuss the Calendar REST API",
  "BodyPreview": "I think it will meet our requirements!",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "Start": {
      "DateTime": "2014-02-02T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2014-02-02T19:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Location": {
    "DisplayName": "",
    "Address": null,
    "Coordinates": null,
    "LocationEmailAddress": "",
    "LocationUri": ""
  },
  "ShowAs": "Busy",
  "IsAllDay": false,
  "IsCancelled": false,
  "IsOrganizer": true,
  "ResponseRequested": true,
  "Type": "SingleInstance",
  "SeriesMasterId": null,
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "janets@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Janet Schorr"
      },
      "Status": {
        "Response": "None",
        "Time": "0001-01-01T00:00:00Z"
      },
      "Type": "Required"
    }
  ],
  "Recurrence": null,
  "OriginalEndTimeZone": "Pacific Standard Time",
  "OriginalStartTimeZone": "Pacific Standard Time",
  "Organizer": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "alexd"
    }
  },
  "OnlineMeetingUrl": null
}
```


**応答の種類**

新しい[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。

メモ： 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. The following is an example to include only the **Start** and **End** properties of the new event in the response.

```
POST https://outlook.office.com/api/beta/me/events?$Select=Start,End
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/v2.0/me/events
POST https://outlook.office.com/api/v2.0/me/calendars/{calendar_id}/events
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|

**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/events
Content-Type: application/json
```

```
{
  "Subject": "Discuss the Calendar REST API",
  "Body": {
    "ContentType": "HTML",
    "Content": "I think it will meet our requirements!"
  },
  "Start": {
      "DateTime": "2014-02-02T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2014-02-02T19:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "janets@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Janet Schorr"
      },
      "Type": "Required"
    }
  ]
}
```

**応答のサンプル**

状態コード :201

```
{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGE4v1RAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeNheA==\"",
  "Id": "AAMkAGE4v1RAAA=",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeNheA==",
  "Categories": [],
  "CreatedDateTime": "2014-01-22T20:56:10.1058291Z",
  "LastModifiedDateTime": "2014-01-22T20:56:10.3402186Z",
  "Subject": "Discuss the Calendar REST API",
  "BodyPreview": "I think it will meet our requirements!",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "Start": {
      "DateTime": "2014-02-02T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2014-02-02T19:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Location": {
    "DisplayName": "",
    "Address": null
  },
  "ShowAs": "Busy",
  "IsAllDay": false,
  "IsCancelled": false,
  "IsOrganizer": true,
  "ResponseRequested": true,
  "Type": "SingleInstance",
  "SeriesMasterId": null,
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "janets@a830edad9050849NDA1.onmicrosoft.com",
        "Name": "Janet Schorr"
      },
      "Status": {
        "Response": "None",
        "Time": "0001-01-01T00:00:00Z"
      },
      "Type": "Required"
    }
  ],
  "Recurrence": null,
  "OriginalEndTimeZone": "Pacific Standard Time",
  "OriginalStartTimeZone": "Pacific Standard Time",
  "Organizer": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "alexd"
    }
  }
}
```


**応答の種類**

新しい[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。

メモ： 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. The following is an example to include only the **Start** and **End** properties of the new event in the response.

```
POST https://outlook.office.com/api/v2.0/me/events?$Select=Start,End
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->

<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


By default, the **Start** and **End** time values are in UTC. You can specify time zones for **Start** and **End**, express the time in the corresponding time zone, and include a time offset from UTC. The example below shows how to assign time values in Pacific Standard Time. Note that if you specify one time zone, you must specify a value for the other one as well.


```no-highlight
POST https://outlook.office.com/api/v1.0/me/events
POST https://outlook.office.com/api/v1.0/me/calendars/{calendar_id}/events
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|

[!code-REST-i[calendar_api_create_event_in_calendar](./_data/calendar_api_create_event_in_calendar.json)]


**応答の種類**

新しい[イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)。

メモ： 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。常に The **Id** property is always returned. The following is an example to include only the **Start** and **End** properties of the new event in the response.

```
POST https://outlook.office.com/api/v1.0/me/events?$Select=Start,End
```


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

****

<a name="CreateEventsClient"></a>
### 予定表イベントを作成する (クライアント)

イベント メッセージの作成 (POST)  イベントを作成します。別の予定表にイベントを追加するには、追加先の予定表の **Events** プロパティを使用します。

例`await client.Me.Calendars["AQMkADE3..."].Events.AddEventAsync(newEvent);`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- CHUCK, feel free to provide client library example that reflects the event breaking changes for beta.  --> 

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


この例では、既に [Outlook サービス クライアントが取得されている](..\api\use-outlook-rest-api.md#GetClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Create a location for the event
Location location = new Location 
{ 
    DisplayName = "Water cooler" 
}; 

// Create a description for the event   
ItemBody body = new ItemBody 
{ 
    Content = "Status updates, blocking issues, and next steps", 
    ContentType = BodyType.Text 
}; 
    
// Create an attendee for the event 
Attendee[] attendees =  
{  
    new Attendee  
    {  
        Type = AttendeeType.Required,  
        EmailAddress = new EmailAddress  
        {  
            Address = "katiej@a830edad9050849NDA1.onmicrosoft.com"  
        },  
    },  
}; 
    
// Create the event object
Event newEvent = new Event 
{ 
    Subject = "Sync up", 
    Location = location, 
    Attendees = attendees, 
    Start = new DateTimeTimeZone()
    {
        TimeZone = TimeZoneInfo.Local.Id,
        DateTime = new DateTime(2015, 12, 1, 9, 30, 0).ToString("s")
    },
    End = new DateTimeTimeZone()
    {
        TimeZone = TimeZoneInfo.Local.Id,
        DateTime = new DateTime(2015, 12, 1, 10, 30, 0).ToString("s")
    },    
    Body = body 
};
    
// Add the event to the default calendar
await client.Me.Events.AddEventAsync(newEvent);

// Get the event ID.
string eventId = newEvent.Id;
```
```javascript
        var event = new Microsoft.OutlookServices.Event();
        event.subject = 'Your Subject';
        event.start = new Date("October 30, 2014 11:13:00").toISOString();
        event.end = new Date("October 30, 2014 12:13:00").toISOString();

        // Body
        event.body = new Microsoft.OutlookServices.ItemBody();
        event.body.content = 'Body Content';
        event.body.contentType = Microsoft.OutlookServices.BodyType.Text;

        // Location
        event.location = new Microsoft.OutlookServices.Location();
        event.location.displayName = 'Location';

        // Attendee
        var attendee1 = new Microsoft.OutlookServices.Attendee();
        var emailAddress1 = new Microsoft.OutlookServices.EmailAddress();
        emailAddress1.name = "Katie Jordan";
        emailAddress1.address = "katiej@a830edad9050849NDA1.onmicrosoft.com";

        attendee1.emailAddress = emailAddress1;

        event.attendees.push(attendee1);
        
        outlookClient.me.calendar.events.addEvent(event)
        .then(function (response) {
            console.log(response._Id);
        });    

```
<!-- ENDSECTION -->


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


この例では、既に [Outlook サービス クライアントが取得されている](..\api\use-outlook-rest-api.md#GetClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Create a location for the event
Location location = new Location 
{ 
    DisplayName = "Water cooler" 
}; 

// Create a description for the event   
ItemBody body = new ItemBody 
{ 
    Content = "Status updates, blocking issues, and next steps", 
    ContentType = BodyType.Text 
}; 
    
// Create an attendee for the event 
Attendee[] attendees =  
{  
    new Attendee  
    {  
        Type = AttendeeType.Required,  
        EmailAddress = new EmailAddress  
        {  
            Address = "katiej@a830edad9050849NDA1.onmicrosoft.com"  
        },  
    },  
}; 
    
// Create the event object
Event newEvent = new Event 
{ 
    Subject = "Sync up", 
    Location = location, 
    Attendees = attendees, 
    Start = new DateTimeOffset(new DateTime(2014, 12, 1, 9, 30, 0)), 
    End = new DateTimeOffset(new DateTime(2014, 12, 1, 10, 0, 0)), 
    Body = body 
}; 
    
// Add the event to the default calendar
await client.Me.Events.AddEventAsync(newEvent);

// Get the event ID.
string eventId = newEvent.Id;
```
```javascript
        var event = new Microsoft.OutlookServices.Event();
        event.subject = 'Your Subject';
        event.start = new Date("October 30, 2014 11:13:00").toISOString();
        event.end = new Date("October 30, 2014 12:13:00").toISOString();

        // Body
        event.body = new Microsoft.OutlookServices.ItemBody();
        event.body.content = 'Body Content';
        event.body.contentType = Microsoft.OutlookServices.BodyType.Text;

        // Location
        event.location = new Microsoft.OutlookServices.Location();
        event.location.displayName = 'Location';

        // Attendee
        var attendee1 = new Microsoft.OutlookServices.Attendee();
        var emailAddress1 = new Microsoft.OutlookServices.EmailAddress();
        emailAddress1.name = "Katie Jordan";
        emailAddress1.address = "katiej@a830edad9050849NDA1.onmicrosoft.com";

        attendee1.emailAddress = emailAddress1;

        event.attendees.push(attendee1);
        
        outlookClient.me.calendar.events.addEvent(event)
        .then(function (response) {
            console.log(response._Id);
        });    

```
<!-- ENDSECTION -->

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->






****


<a name="UpdateEvents"> </a>
## イベントを更新する

REST API:[予定表イベントを更新する](#UpdateAnEvent)

クライアント ライブラリ:[予定表イベントを更新する (クライアント)](#UpdateEventsClient)

<a name="UpdateAnEvent"></a>
###予定表イベントを更新する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

イベントを変更します。指定したプロパティのみが変更されます。ユーザーが開催者である場合、サーバーはすべての出席者に会議の更新を送信します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
PATCH https://outlook.office.com/api/beta/me/events/{event_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

要求本文に任意の書き込み可能な [event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) プロパティを指定します。

```
PATCH https://outlook.office.com/api/beta/me/events/AAMkAGE1MFKPQWAAA=?$select=Location
```

**要求のサンプル:**

```
PATCH https://outlook.office.com/api/beta/me/events/AAMkAGE0M4v1OAAA=
Content-Type: application/json

{
  "Location": {
    "DisplayName": "Your office",
    "Address": null,
    "Coordinates": null,
    "LocationEmailAddress": "",
    "LocationUri": ""
  }
}
```

**応答のサンプル:**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGE0M4v1OAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeNheQ==\"",
  "Id": "AAMkAGE0M4v1OAAA=",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeNheQ==",
  "Categories": [],
  "CreatedDateTime": "2014-01-22T20:49:05.5657528Z",
  "LastModifiedDateTime": "2014-01-22T21:14:17.4886416Z",
  "Subject": "Discuss the Calendar REST API",
  "BodyPreview": "I think it will meet our requirements!",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "Start": {
      "DateTime": "2014-02-02T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2014-02-02T19:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Location": {
    "DisplayName": "Your office",
    "Address": null,
    "Coordinates": null,
    "LocationEmailAddress": "",
    "LocationUri": ""
  },
  "ShowAs": "Busy",
  "IsAllDay": false,
  "IsCancelled": false,
  "IsOrganizer": true,
  "ResponseRequested": true,
  "Type": "SingleInstance",
  "SeriesMasterId": null,
  "Attendees": [],
  "Recurrence": null,
  "OriginalEndTimeZone": "Pacific Standard Time",
  "OriginalStartTimeZone": "Pacific Standard Time",
  "Organizer": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "alexd"
    }
  },
  "OnlineMeetingUrl": null
}
```

**応答の種類**

The updated [event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource). 更新されたイベント。ユーザーが開催者である場合、サーバーはすべての出席者に会議の更新を送信します。

既定では、応答に更新されたイベントのすべてのプロパティが含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。**Id** プロパティは常に返されます。 




[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/events/{event_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

要求本文に任意の書き込み可能な [event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) プロパティを指定します。

**要求のサンプル:**

```
PATCH https://outlook.office.com/api/v2.0/me/events/AAMkAGE0M4v1OAAA=
Content-Type: application/json

{
  "Location": {
    "DisplayName": "Your office",
    "Address": null
  }
}
```

**応答のサンプル:**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Events/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGE0M4v1OAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeNheQ==\"",
  "Id": "AAMkAGE0M4v1OAAA=",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeNheQ==",
  "Categories": [],
  "CreatedDateTime": "2014-01-22T20:49:05.5657528Z",
  "LastModifiedDateTime": "2014-01-22T21:14:17.4886416Z",
  "Subject": "Discuss the Calendar REST API",
  "BodyPreview": "I think it will meet our requirements!",
  "Body": {
    "ContentType": "HTML",
    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
  },
  "Importance": "Normal",
  "HasAttachments": false,
  "Start": {
      "DateTime": "2014-02-02T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2014-02-02T19:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Location": {
    "DisplayName": "Your office",
    "Address": null
  },
  "ShowAs": "Busy",
  "IsAllDay": false,
  "IsCancelled": false,
  "IsOrganizer": true,
  "ResponseRequested": true,
  "Type": "SingleInstance",
  "SeriesMasterId": null,
  "Attendees": [],
  "Recurrence": null,
  "OriginalEndTimeZone": "Pacific Standard Time",
  "OriginalStartTimeZone": "Pacific Standard Time",
  "Organizer": {
    "EmailAddress": {
      "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "alexd"
    }
  }
}
```

**応答の種類**

The updated [event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource). 更新されたイベント。ユーザーが開催者である場合、サーバーはすべての出席者に会議の更新を送信します。

既定では、応答に更新されたイベントのすべてのプロパティが含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。**Id** プロパティは常に返されます。 

```
PATCH https://outlook.office.com/api/v2.0/me/events/AAMkAGE1MFKPQWAAA=?$select=Location
```




[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v1.0/me/events/{event_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

要求本文に任意の書き込み可能な [event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) プロパティを指定します。

[!code-REST-i[calendar_api_update_event_by_id](./_data/calendar_api_update_event_by_id.json)]

**応答の種類**

The updated [event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource). 更新されたイベント。ユーザーが開催者である場合、サーバーはすべての出席者に会議の更新を送信します。

既定では、応答に更新されたイベントのすべてのプロパティが含まれます。最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。**Id** プロパティは常に返されます。 

```
PATCH https://outlook.office.com/api/v1.0/me/events/AAMkAGE1MFKPQWAAA=?$select=Location
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->






****

<a name="UpdateEventsClient"></a>
### 予定表イベントを更新する (クライアント)

イベントを変更します。

次のパターンを使用して、クライアント側で複数の更新を定義し、要求をまとめて (バッチ処理で) 送信できます。
1. Call `UpdateAsync(true)` for each entity you want to update. 更新するエンティティごとに UpdateAsync(true)`true` を呼び出します。true を指定すると、更新がクライアント上でローカルに登録されますが、サーバーには投稿されません。
2. outlookClient.Context.SaveChangesAsync()`client.Context.SaveChangesAsync()` を呼び出して、ローカルに登録されているすべての更新を投稿します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

<!-- CHUCK, feel free to provide client library example that reflects the event breaking changes for beta.  --> 

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[イベント ID](#GetEvents) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Get an existing event by ID
IEvent eventToUpdate = await client.Me.Events[eventId].ExecuteAsync();

// Add attendees
eventToUpdate.Attendees.Add(new Attendee
{
    Type = AttendeeType.Required,
    EmailAddress = new EmailAddress
    {
        Address = "garthf@a830edad9050849NDA1.onmicrosoft.com",
    },
});

// Make other changes
eventToUpdate.Start = new DateTimeOffset(new DateTime(2014, 12, 1, 14, 30, 0));
eventToUpdate.End = new DateTimeOffset(new DateTime(2014, 12, 1, 15, 0, 0));
eventToUpdate.Subject = "New event name";
    
// Commit all changes to the event
await eventToUpdate.UpdateAsync();

// Get an updated property.
string newEventName = eventToUpdate.Subject;
```

<!-- ENDSECTION -->


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[イベント ID](#GetEvents) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Get an existing event by ID
IEvent eventToUpdate = await client.Me.Events[eventId].ExecuteAsync();

// Add attendees
eventToUpdate.Attendees.Add(new Attendee
{
    Type = AttendeeType.Required,
    EmailAddress = new EmailAddress
    {
        Address = "garthf@a830edad9050849NDA1.onmicrosoft.com",
    },
});

// Make other changes
eventToUpdate.Start = new DateTimeOffset(new DateTime(2014, 12, 1, 14, 30, 0));
eventToUpdate.End = new DateTimeOffset(new DateTime(2014, 12, 1, 15, 0, 0));
eventToUpdate.Subject = "New event name";
    
// Commit all changes to the event
await eventToUpdate.UpdateAsync();

// Get an updated property.
string newEventName = eventToUpdate.Subject;
```

<!-- ENDSECTION -->

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->








****

<a name="RespndToEvents"></a>
## イベントに応答する。

REST API:イベントを承諾する (REST)  イベントを仮承諾する (REST)  イベントを辞退する (REST)

<a name="AcceptEvent"></a>
###イベントを承諾する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

指定したイベントを承諾します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/events/{event_id}/accept
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/accept

Content-Type: application/json

{
  "Comment": "Great idea!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
POST https://outlook.office.com/api/v2.0/me/events/{event_id}/accept
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/accept

Content-Type: application/json

{
  "Comment": "Great idea!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/events/{event_id}/accept
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/v1.0/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/accept

Content-Type: application/json

{
  "Comment": "Great idea!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->





****

<a name="TentAcceptEvent"></a>
###イベントを仮承諾する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

指定したイベントを仮承諾します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/events/{event_id}/tentativelyaccept
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/tentativelyaccept

Content-Type: application/json

{
  "Comment": "I'll confirm later!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
POST https://outlook.office.com/api/v2.0/me/events/{event_id}/tentativelyaccept
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/tentativelyaccept

Content-Type: application/json

{
  "Comment": "I'll confirm later!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/events/{event_id}/tentativelyaccept
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/v1.0/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/tentativelyaccept

Content-Type: application/json

{
  "Comment": "I'll confirm later!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


****

<a name="DeclineEvent"></a>
###イベントを辞退する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

指定したイベントへの招待を辞退します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]



```no-highlight
POST https://outlook.office.com/api/beta/me/events/{event_id}/decline
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/beta/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/decline

Content-Type: application/json

{
  "Comment": "Sorry, maybe next time!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]



```no-highlight
POST https://outlook.office.com/api/v2.0/me/events/{event_id}/decline
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/v2.0/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/decline

Content-Type: application/json

{
  "Comment": "Sorry, maybe next time!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


```no-highlight
POST https://outlook.office.com/api/v1.0/me/events/{event_id}/decline
```


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。 必須。|
|_Body parameters_|
|Comment|string|応答に含まれるテキスト。省略可。|
|SendResponse|ブール値| 応答が開催者に送信される場合は、true`true`。それ以外の場合は、false`false`。省略可。既定値は true です。 省略可能。 Default is `true`.|
 
**要求のサンプル:**

```
POST https://outlook.office.com/api/v1.0/me/events('AAMkAGE1M2IyNGNmLTI5MT_bs88AAAXDJwEAAA=')/decline

Content-Type: application/json

{
  "Comment": "Sorry, maybe next time!",
  "SendResponse": "true"
}
```

**応答**

成功した応答は、HTTP 202 承認済みの応答コードで示されます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->



****

<a name="DeleteEvents"> </a>
## イベントを削除する

REST API:[予定表イベントを削除する (REST)](#DeleteAnEvent)

クライアント ライブラリ:[予定表イベントを削除する (クライアント)](#DeleteEventsClient)

<a name="DeleteAnEvent"></a>
###予定表イベントを削除する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

イベントをサインインしているユーザーの削除済みアイテム フォルダーに移動します。イベントが会議であり、サインインしているユーザーが開催者である場合は、サーバーはすべての出席者にキャンセルを送信します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

このアクションは、会議の開催者と出席者に対して**削除**が使用可能であるという点で、[キャンセル](#CancelEvents)とは異なります。サインインしているユーザーが会議の開催者である場合は、そのユーザーは出席者に対しキャンセルのカスタム メッセージを送信しないで単に会議をキャンセルします。 このアクションは、会議の開催者と出席者に対して削除が使用可能であるという点で、キャンセルとは異なります。サインインしているユーザーが会議の開催者である場合は、そのユーザーは出席者に対しキャンセルのカスタム メッセージを送信しないで単に会議をキャンセルします。

```no-highlight
DELETE https://outlook.office.com/api/beta/me/events/{event_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/beta/me/events/AAMkAGE0M4v1OAAA=
```

**応答のサンプル**

状態コード :204


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/events/{event_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

**要求のサンプル**

```
DELETE https://outlook.office.com/api/v2.0/me/events/AAMkAGE0M4v1OAAA=
```

**応答のサンプル**

状態コード :204



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/events/{event_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|

[!code-REST-i[calendar_api_delete_event_by_id](./_data/calendar_api_delete_event_by_id.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->



****

<a name="DeleteEventsClient"></a>
### 予定表イベントを削除する (クライアント)

イベントを削除済みアイテム フォルダーに移動します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[イベント ID](#GetEvents) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Get an existing event by ID
IEvent eventToDelete = await client.Me.Events[eventId].ExecuteAsync();

//Delete the event
await eventToDelete.DeleteAsync();
```

<!-- ENDSECTION -->

****

<a name="CancelEvents"> </a>
## イベントをキャンセルする (プレビュー)

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

このアクションでは、会議の開催者はキャンセル メッセージを送信し、イベントをキャンセルできます。 

The action moves the event to the Deleted Items folder. The organizer can also cancel an occurrence of a recurring meeting by providing the occurrence event ID. An attendee calling this action gets an error (HTTP 400 Bad Request), with the following error message:

"Your request can't be completed. "要求を完了できません。会議を取り消すには、開催者である必要があります。"

このアクションは、開催者に対してのみ**キャンセル**が使用可能であるという点で、[削除](#DeleteEvents)とは異なり、開催者はキャンセルに関するカスタム メッセージを出席者に送信できます。

```no-highlight
POST https://outlook.office.com/api/beta/me/events/{event_id}/Cancel
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|_Body parameters_|
|Comment|string|すべての出席者に送信されるキャンセルに関してのコメント。|

**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/events/AAMkAGE0M4v1OAAA=/Cancel

Content-Type: application/json
{
  "Comment": "Cancelling this meeting as there is a time conflict for most folks."
}
```

**応答のサンプル**

状態コード :202 承認済み


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

<a name="GetAttachments"> </a>
## 添付ファイルの取得

添付ファイルのコレクションまたは添付ファイルを取得できます。

REST API:添付ファイルのコレクションを取得する (REST)  添付ファイルを取得する (REST)

<a name="GetAttachmentCollection"> </a>
###添付ファイルのコレクションを取得する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

特定のイベントから添付ファイルを取得します。

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/events/{event_id}/attachments
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|


**応答の種類**

[FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)、[ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)、または [ReferenceAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource) の種類を使用できる添付ファイル コレクション。


**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/events/AAMkAGI2NGTG9yAAA=/attachments
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events('AAMkAGI2NG9yAAA%3D')/Attachments",
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2NGTG9yAAA=')/Attachments('AAMkAGI2NGLwydGuOzcHf1FBlo=')",
            "Id": "AAMkAGI2NGLwydGuOzcHf1FBlo=",
            "LastModifiedDateTime": "2014-10-22T00:30:26Z",
            "Name": "Company Party.docx",
            "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
            "Size": 11647,
            "IsInline": false,
            "ContentId": null,
            "ContentLocation": null,
            "ContentBytes": "UEsDBBQABgAIAAAAIQDfpNJs...AAF4pAAAAAA=="
        }
    ]
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/events/{event_id}/attachments
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|


**応答の種類**

[FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource) または [ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource) の種類を使用できる添付ファイル コレクション。


**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/events/AAMkAGI2NGTG9yAAA=/attachments
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Events('AAMkAGI2NG9yAAA%3D')/Attachments",
    "value": [
        {
            "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2NGTG9yAAA=')/Attachments('AAMkAGI2NGLwydGuOzcHf1FBlo=')",
            "Id": "AAMkAGI2NGLwydGuOzcHf1FBlo=",
            "LastModifiedDateTime": "2014-10-22T00:30:26Z",
            "Name": "Company Party.docx",
            "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
            "Size": 11647,
            "IsInline": false,
            "ContentId": null,
            "ContentLocation": null,
            "ContentBytes": "UEsDBBQABgAIAAAAIQDfpNJs...AAF4pAAAAAA=="
        }
    ]
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/events/{event_id}/attachments
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|


**応答の種類**

[FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource) または [ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource) の種類を使用できる添付ファイル コレクション。


[!code-REST-i[calendar_api_get_attachments](./_data/calendar_api_get_attachments.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->



****


<a name="GetAttachment"> </a>
###添付ファイルを取得する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

特定のイベントから添付ファイルを取得します。

<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/events/{event_id}/attachments/{attachment_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|attachment_id|string|添付ファイル ID。|

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


**応答の種類**

要求された[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)、[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)、または[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource)。


**要求のサンプル**

次の例では、特定のイベントに添付するファイルを取得します。

```
GET https://outlook.office.com/api/beta/me/events/AAMkAGI2WRAAADTG9yAAA=/attachments/AAMkAGI2TG9yAAABEgAQALxJtn1LwydGuOzcHf1FBlo=
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events('AAMkAGI2WRAAADTG9yAAA%3D')/Attachments/$entity",
    "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2WRAAADTG9yAAA=')/Attachments('AAMkAGI2TG9yAAABEgAQALxJtn1LwydGuOzcHf1FBlo=')",
    "Id": "AAMkAGI2TG9yAAABEgAQALxJtn1LwydGuOzcHf1FBlo=",
    "LastModifiedDateTime": "2014-10-22T00:30:26Z",
    "Name": "Company Party.docx",
    "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    "Size": 11647,
    "IsInline": false,
    "ContentId": null,
    "ContentLocation": null,
    "ContentBytes": "UEsDBBQABgAIAAAAIQD...AAF4pAAAAAA=="
}
```

**要求のサンプル (参照添付ファイル)**

次の例では、イベントの参照添付ファイルを取得します。

```
GET https://outlook.office.com/api/beta/me/events('AAMkAGE1Mbs88AADggYEcAAA=')/attachments('AAMkAGE1Mbs88AADggYEcAAABEgAQAABWAoLgP3REt_LWRG8ORv4=')
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/events('AAMkAGE1Mbs88AADggYEcAAA%3D')/attachments/$entity",
    "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
    "id": "AAMkAGE1Mbs88AADggYEcAAABEgAQAABWAoLgP3REt_LWRG8ORv4=",
    "lastModifiedDateTime": "2016-03-22T22:27:20Z",
    "name": "Hydrangea picture",
    "contentType": null,
    "size": 412,
    "isInline": false,
    "sourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/hydrangea.jpg",
    "providerType": "oneDriveBusiness",
    "thumbnailUrl": null,
    "previewUrl": null,
    "permission": "edit",
    "isFolder": false
}

```


**要求のサンプル (添付ファイルの $expand)**

次の例では、イベント プロパティを持つ 2 つの参照添付ファイルのインラインを取得して展開します。

```
GET https://outlook.office.com/api/beta/me/events('AAMkAGE1Mbs88AADggYEcAAA=')?$expand=attachments
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/events/$entity",
    "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAA4KZrtA==\"",
    "id": "AAMkAGE1Mbs88AADggYEcAAA=",
    "createdDateTime": "2016-03-22T22:19:58.1359352Z",
    "lastModifiedDateTime": "2016-03-22T22:39:09.9335289Z",
    "changeKey": "mODEKWhc/Um6lA3uPm7PPAAA4KZrtA==",
    "categories": [
    ],
    "originalStartTimeZone": "Pacific Standard Time",
    "originalEndTimeZone": "Pacific Standard Time",
    "responseStatus": {
        "response": "organizer",
        "time": "0001-01-01T00:00:00Z"
    },
    "iCalUId": "040000008200E00074C5B7101A82E008000000005895FCF78884D10100000000000000001000000099DD7CA6BCF37C4F9F7DAACCADDD6B89",
    "reminderMinutesBeforeStart": 15,
    "isReminderOn": true,
    "hasAttachments": true,
    "subject": "Plan Easter egg hunt!",
    "body": {
        "contentType": "html",
        "content": "<html>\r\n<body>\r\nLet's get organized for this weekend's gathering.\r\n</body>\r\n</html>\r\n"
    },
    "bodyPreview": "Let's get organized for this weekend's gathering.",
    "importance": "normal",
    "sensitivity": "normal",
    "start": {
        "dateTime": "2016-03-26T17:00:00.0000000",
        "timeZone": "UTC"
    },
    "end": {
        "dateTime": "2016-03-26T18:00:00.0000000",
        "timeZone": "UTC"
    },
    "location": {
        "displayName": "",
        "address": {
            "type": "unknown"
        },
        "coordinates": {
        }
    },
    "isAllDay": false,
    "isCancelled": false,
    "isOrganizer": true,
    "recurrence": null,
    "responseRequested": true,
    "seriesMasterId": null,
    "showAs": "busy",
    "type": "singleInstance",
    "attendees": [
        {
            "status": {
                "response": "none",
                "time": "0001-01-01T00:00:00Z"
            },
            "type": "required",
            "emailAddress": {
                "name": "Randi Welch",
                "address": "randiw@contoso.onmicrosoft.com"
            }
        }
    ],
    "organizer": {
        "emailAddress": {
            "name": "Dana Swope",
            "address": "danas@contoso.onmicrosoft.com"
        }
    },
    "webLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1Mbs88AADggYEcAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory",
    "onlineMeetingUrl": null,
    "attachments@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/events('AAMkAGE1Mbs88AADggYEcAAA%3D')/attachments",
    "attachments": [
        {
            "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment"",
            "id": "AAMkAGE1Mbs88AADggYEcAAABEgAQAABWAoLgP3REt_LWRG8ORv4=",
            "lastModifiedDateTime": "2016-03-22T22:27:20Z",
            "name": "Hydrangea picture",
            "contentType": null,
            "size": 412,
            "isInline": false,
            "sourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/hydrangea.jpg",
            "providerType": "oneDriveBusiness",
            "thumbnailUrl": null,
            "previewUrl": null,
            "permission": "edit",
            "isFolder": false
        },
        {
            "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment"",
            "id": "AAMkAGE1Mbs88AADggYEcAAABEgAQAE1rHHth7YNLlR9WsgnODy0=",
            "lastModifiedDateTime": "2016-03-22T22:39:09Z",
            "name": "Koala picture",
            "contentType": null,
            "size": 382,
            "isInline": false,
            "sourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/koala.jpg",
            "providerType": "oneDriveBusiness",
            "thumbnailUrl": null,
            "previewUrl": null,
            "permission": "edit",
            "isFolder": false
        }
    ]
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/events/{event_id}/attachments/{attachment_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|attachment_id|string|添付ファイル ID。|

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


**応答の種類**

要求された[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)。


**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/events/AAMkAGI2WRAAADTG9yAAA=/attachments/AAMkAGI2TG9yAAABEgAQALxJtn1LwydGuOzcHf1FBlo=
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Events('AAMkAGI2WRAAADTG9yAAA%3D')/Attachments/$entity",
    "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGI2WRAAADTG9yAAA=')/Attachments('AAMkAGI2TG9yAAABEgAQALxJtn1LwydGuOzcHf1FBlo=')",
    "Id": "AAMkAGI2TG9yAAABEgAQALxJtn1LwydGuOzcHf1FBlo=",
    "LastModifiedDateTime": "2014-10-22T00:30:26Z",
    "Name": "Company Party.docx",
    "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    "Size": 11647,
    "IsInline": false,
    "ContentId": null,
    "ContentLocation": null,
    "ContentBytes": "UEsDBBQABgAIAAAAIQD...AAF4pAAAAAA=="
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/events/{event_id}/attachments/{attachment_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|attachment_id|string|添付ファイル ID。|

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


**応答の種類**

要求された[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)。


[!code-REST-i[calendar_api_get_attachment_by_id](./_data/calendar_api_get_attachment_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->




****


<a name="CreateAttachments"> </a>
## 添付ファイルを作成する
イベントの添付ファイルまたは[アイテムの添付ファイルを作成](#CreateItemAttachment)できます。

REST API:添付ファイルを作成する (REST)  アイテムの添付ファイルを作成する (REST)  参照添付ファイルを作成する (REST)


<a name="CreateFileAttachment"></a>
###添付ファイルを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

イベントに添付ファイルを追加します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/events/{event_id}/attachments
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
POST https://outlook.office.com/api/v2.0/me/events/{event_id}/attachments
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/events/{event_id}/attachments
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|_Body parameters_|
|@odata.type| string | #Microsoft.OutlookServices.FileAttachment |
|名前|string|添付ファイルの名前。|
|ContentBytes|binary|添付するファイル。|
 
<!-- Add post GA
[!code-REST-i[calendar_api_create_file_attachment](./_data/calendar_api_create_file_attachment.json)]
-->

**応答の種類**

新しい[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)。

****


<a name="CreateItemAttachment"></a>
###アイテムの添付ファイルを作成する (REST)

__**Minimum required scope**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

イベントにアイテムの添付ファイルを追加します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/events/{event_id}/attachments
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
POST https://outlook.office.com/api/v2.0/me/events/{event_id}/attachments
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/events/{event_id}/attachments
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|_Body parameters_|
|@odata.type| string | #Microsoft.OutlookServices.ItemAttachment |
|名前|string|添付ファイルの名前。|
|アイテム|[Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)、[Event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)、または [Contact](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource) エンティティ。|添付するアイテム。|
 

<!--Post GA
```REST
[!code-REST-i[calendar_api_create_item_attachment](./_data/calendar_api_create_item_attachment.json)]
-->


**応答の種類**

新しい[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)。

****

<a name="CreateReferenceAttachment"></a>

###参照添付ファイルを作成する (REST)

<!-- ==================================== Start beta content ====================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

__**必要な範囲**: https://outlook.office.com/mail.readwrite__

イベントに参照添付ファイルを追加します。

```no-highlight
POST https://outlook.office.com/api/beta/me/events/{event_id}/attachments
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|String|イベント ID。|
|_Body parameters_|
|@odata.type|String|```#Microsoft.OutlookServices.ReferenceAttachment```|
|名前|String|添付ファイルの表示名。必須。|
|SourceUrl|String | 添付ファイルの内容を取得するための URL。フォルダーへの URL の場合、Outlook または Outlook on the web 上でフォルダーが正しく表示されるには、**IsFolder** を true に設定します。必須。|

要求本文に、**Name** と**SourceUrl** パラメーターおよび書き込み可能な[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource) プロパティを指定します。



**応答の種類**

[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource)。


**要求のサンプル:**

次の例では、既存のイベントに参照添付ファイルを追加します。添付ファイルは、OneDrive for Business 上のファイルへのリンクです。 次の例では、既存メッセージに添付ファイルを追加します。添付ファイルは、OneDrive for Business 上のファイルへのリンクです。

```
POST https://outlook.office.com/api/beta/me/events('AAMkAGE1Mbs88AADggYEcAAA=')/attachments
Content-Type: application/json

{
    "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment"", 
    "Name": "Hydrangea picture", 
    "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/hydrangea.jpg", 
    "ProviderType": "oneDriveBusiness", 
    "Permission": "Edit", 
    "IsFolder": "False" 
 }
```


**応答のサンプル:**

```
Status code: 201 Created

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/events('AAMkAGE1Mbs88AADggYEcAAA%3D')/attachments/$entity",
    "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment"",
    "Id": "AAMkAGE1Mbs88AADggYEcAAABEgAQAABWAoLgP3REt_LWRG8ORv4=",
    "LastModifiedDateTime": "2016-03-22T22:27:20Z",
    "Name": "Hydrangea picture",
    "ContentType": null,
    "Size": 412,
    "IsInline": false,
    "SourceUrl": "https://contoso-my.spoppe.com/personal/danas_contoso_onmicrosoft_com/Documents/Pics/hydrangea.jpg",
    "ProviderType": "oneDriveBusiness",
    "ThumbnailUrl": null,
    "PreviewUrl": null,
    "Permission": "edit",
    "IsFolder": false
}
```


**要求のサンプル**

次の例では、イベントを作成する場合と同じ呼び出しで、参照添付ファイルを追加します。添付ファイルは、OneDrive for Business 上のファイルへのリンクです。

```
POST https://outlook.office.com/api/beta/me/events
Content-Type: application/json

{
  "Subject": "Plan Easter egg hunt!",
  "Body": {
    "ContentType": "HTML",
    "Content": "Let's get organized for this weekend's gathering."
  },
  "Start": {
      "DateTime": "2016-03-25T10:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2016-03-25T11:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "randiw@contoso.onmicrosoft.com",
        "Name": "Randi Welch"
      },
      "Type": "Required"
    }
  ],
  "Attachments": [
      {
        "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment"", 
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
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/events/$entity",
    "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAA4KZrrg==\"",
    "Id": "AAMkAGE1Mbs88AADggYEbAAA=",
    "CreatedDateTime": "2016-03-22T22:09:26.2918604Z",
    "LastModifiedDateTime": "2016-03-22T22:09:27.0754806Z",
    "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAA4KZrrg==",
    "Categories": [
    ],
    "OriginalStartTimeZone": "Pacific Standard Time",
    "OriginalEndTimeZone": "Pacific Standard Time",
    "ResponseStatus": {
        "Response": "Organizer",
        "Time": "0001-01-01T00:00:00Z"
    },
    "iCalUId": "040000008200E00074C5B7101A82E0080000000043FB607F8784D101000000000000000010000000B3A31CD7479252448ECE77242DE92587",
    "ReminderMinutesBeforeStart": 15,
    "IsReminderOn": true,
    "HasAttachments": true,
    "Subject": "Plan Easter egg hunt!",
    "Body": {
        "contentType": "html",
        "content": "<html>\r\n<body>\r\nLet's get organized for this weekend's gathering.\r\n</body>\r\n</html>\r\n"
    },
    "BodyPreview": "Let's get organized for this weekend's gathering.",
    "Importance": "normal",
    "Sensitivity": "normal",
    "Start": {
        "DateTime": "2016-03-25T10:00:00.0000000",
        "TimeZone": "Pacific Standard Time"
    },
    "End": {
        "DateTime": "2016-03-25T11:00:00.0000000",
        "TimeZone": "Pacific Standard Time"
    },
    "Location": {
        "DisplayName": "",
        "Address": {
            "Type": "unknown"
        },
        "Coordinates": {
        }
    },
    "IsAllDay": false,
    "IsCancelled": false,
    "IsOrganizer": true,
    "Recurrence": null,
    "ResponseRequested": true,
    "SeriesMasterId": null,
    "ShowAs": "busy",
    "Type": "singleInstance",
    "Attendees": [
        {
            "Status": {
                "Response": "none",
                "Time": "0001-01-01T00:00:00Z"
            },
            "Type": "required",
            "EmailAddress": {
                "Name": "Randi Welch",
                "Address": "randiw@contoso.onmicrosoft.com"
            }
        }
    ],
    "Organizer": {
        "EmailAddress": {
            "Name": "Dana Swope",
            "Address": "danas@contoso.onmicrosoft.com"
        }
    },
    "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1Mbs88AADggYEbAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory",
    "OnlineMeetingUrl": null,
    "Attachments@odata.context": "https://outlook.office.com/api/beta/$metadata#users('ddfcd489-628b-40d7-b48b-57002df800e5')/events('AAMkAGE1Mbs88AADZ0CU9AAA%3D')/attachments",
    "Attachments": [
    {
      "@odata.type": "#Microsoft.OutlookServices.ReferenceAttachment",
      "Id": "AAMkAGE1Mbs88AADZ0CU9AAABEgAQAGe4H1iqXwtLsrCCLLkDxqo=",
      "LastModifiedDateTime": null,
      "Name": Hydrangea picture,
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
## 添付ファイルを削除する

イベントの添付ファイルを削除します。

REST API:[イベントの添付ファイルを削除する (REST)](#DeleteAnEventAttachment)

<a name="DeleteAnEventAttachment"></a>
###イベントの添付ファイルを削除する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Delete the specified attachment of an event. メッセージの指定した添付ファイルを削除します。添付ファイルは、[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)、[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)、または[参照添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ReferenceAttachmentResource)にすることができます。

```no-highlight
DELETE https://outlook.office.com/api/beta/me/events/{event_id}/attachments/{attachment_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|attachment_id|string|添付ファイル ID。|

**要求のサンプル**

```
DELETE https:/outlook.office.com/api/beta/me/events/AAMkAGE0MG4v1OAAA=/attachments/AAMkAGITG9yAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Delete the specified attachment of an event. メッセージの指定した添付ファイルを削除します。添付ファイルは、[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)にすることができます。

```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/events/{event_id}/attachments/{attachment_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|attachment_id|string|添付ファイル ID。|

**要求のサンプル**

```
DELETE https:/outlook.office.com/api/v2.0/me/events/AAMkAGE0MG4v1OAAA=/attachments/AAMkAGITG9yAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

Delete the specified attachment of an event. メッセージの指定した添付ファイルを削除します。添付ファイルは、[添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource)または[アイテムの添付ファイル](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource)にすることができます。

```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/events/{event_id}/attachments/{attachment_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|event_id|string|イベント ID。|
|attachment_id|string|添付ファイル ID。|

[!code-REST-i[calendar_api_delete_attachment](./_data/calendar_api_delete_attachment.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

<a name="GetReminders" > </a>
##アラームを取得する

予定表から 2 つの日付と時刻の間でイベントのアラーム一覧を取得します。

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/ReminderView(StartDateTime='{DateTime}',EndDateTime='{DateTime}')
```
|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイムゾーン。|
|_URL のパラメーター_|
|StartDateTime|string|返されるアラームの開始日と開始時刻。|
|EndDateTime|string|返されるアラームの終了日と終了時刻。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. If the _Prefer: outlook.timezone_ header is not specified, the time zone is set to UTC.

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/ReminderView(StartDateTime='{DateTime}',EndDateTime='{DateTime}')
```
|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|__Header parameters_|
|Prefer: |outlook.timezone|応答に対するイベントの既定のタイムゾーン。|
|_URL のパラメーター_|
|StartDateTime|string|返されるアラームの開始日と開始時刻。|
|EndDateTime|string|返されるアラームの終了日と終了時刻。|

Use the _Prefer: outlook.timezone_ header to specify the time zone to use for the event start and end times in the response.
If the event was created in a different time zone, the start and end times will be adjusted to the specified time zone.
See [this list](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone) for the supported time zone names. If the _Prefer: outlook.timezone_ header is not specified, the time zone is set to UTC.

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版と v2.0 バージョンでのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[v2.0]** または **[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="SnoozeReminders"> </a>
##アラームを再通知する

アラームを再通知して、新しい時間までアラームを延期します。

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/Events('{id}')/SnoozeReminder
```

|**必要なパラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|id|string|イベントの ID。|
|_Body parameters_|
|NewReminderTime|[DateTimeTimeZone](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone)|アラームをトリガーする新しい日付と時刻。|

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/Events('{id}')/SnoozeReminder
```

|**必要なパラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|id|string|イベントの ID。|
|_Body parameters_|
|NewReminderTime|[DateTimeTimeZone](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZone)|アラームをトリガーする新しい日付と時刻。|

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版と v2.0 バージョンでのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[v2.0]** または **[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

<a name="DismissReminders"> </a>
##アラームを消す

トリガーされたアラームを消します。

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/Events({id})/DismissReminder
```

|**必要なパラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|id|string|イベントの ID。|


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/Events({id})/DismissReminder
```

|**必要なパラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|id|string|イベントの ID。|

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版と v2.0 バージョンでのみ利用できます。詳細については、記事の右上隅にコントロールを使用して、**[v2.0]** または **[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


<a name="GetCalendars"> </a>
##予定表を取得する

予定表のコレクションまたは予定表を取得できます。

REST API:予定表のコレクションを取得する (REST)  予定表を取得する (REST)

クライアント ライブラリ:[予定表のコレクションまたは予定表を取得する (クライアント)](#GetCalendarsClient)

<a name="GetCalendarCollection"> </a>
###予定表のコレクションを取得する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

ユーザーのすべての予定表 (calendars`calendars`) を取得するか、または特定の予定表グループの予定表を取得します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]



```no-highlight
GET https://outlook.office.com/api/beta/me/calendars
GET https://outlook.office.com/api/beta/me/calendargroups/{calendar_group_id}/calendars
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#UseOdataQueryParameters)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calender_group_id|string|予定表グループ ID。|


**要求のサンプル**

```
https://outlook.office.com/api/beta/me/calendars
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Calendars",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGI2TGuLAAA=')",
            "Id": "AAMkAGI2TGuLAAA=",
            "Name": "Calendar",
            "Color": "Auto",
            "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+w=="
        }
    ]
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]



```no-highlight
GET https://outlook.office.com/api/v2.0/me/calendars
GET https://outlook.office.com/api/v2.0/me/calendargroups/{calendar_group_id}/calendars
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#UseOdataQueryParameters)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calender_group_id|string|予定表グループ ID。|

**要求のサンプル**

```
https://outlook.office.com/api/v2.0/me/calendars
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Calendars",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGI2TGuLAAA=')",
            "Id": "AAMkAGI2TGuLAAA=",
            "Name": "Calendar",
            "Color": "Auto",
            "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+w=="
        }
    ]
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


```no-highlight
GET https://outlook.office.com/api/v1.0/me/calendars
GET https://outlook.office.com/api/v1.0/me/calendargroups/{calendar_group_id}/calendars
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#UseOdataQueryParameters)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calender_group_id|string|予定表グループ ID。|


[!code-REST-i[calendar_api_get_calendars](./_data/calendar_api_get_calendars.json)]

**応答の種類**

要求された[予定表](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource)のコレクション。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->



****

<a name="GetCalendar"> </a>
###予定表を取得する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

ID で予定表グループを取得します。 ID で予定表を取得します。../me/calendar`../me/calendar` エンドポイントを使用して、ユーザーの標準として設定されている予定表を取得できます。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/calendars/{calendar_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/calendars/AAMkAGI2TGuLAAA=
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Calendars/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGI2TGuLAAA=')",
    "Id": "AAMkAGI2TGuLAAA=",
    "Name": "Calendar",
    "Color": "Auto",
    "IsDefaultCalendar": false,
    "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+w=="
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/calendars/{calendar_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/calendars/AAMkAGI2TGuLAAA=
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Calendars/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGI2TGuLAAA=')",
    "Id": "AAMkAGI2TGuLAAA=",
    "Name": "Calendar",
    "Color": "Auto",
    "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+w=="
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/calendars/{calendar_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|


[!code-REST-i[calendar_api_get_calendar_by_id](./_data/calendar_api_get_calendar_by_id.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

**応答の種類**

要求された[予定表](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource)。

****

<a name="GetCalendarsClient"> </a>
###予定表のコレクションまたは予定表を取得する (クライアント)

Get the user's calendars. To get the user's default calendar, use the `client.Me.Calendar` shortcut property. ユーザーの予定表グループを取得します。別の予定表グループを取得するには、**CalendarGroups** コレクションのインデックスとして予定表グループ ID を指定するか、**GetById** メソッドを使用します。

例`client.Me.Calendars[calendarId].ExecuteAsync()`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


**メモ**: 予定表コレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。

この例では、[Outlook サービス クライアントを取得する](..\api\use-outlook-rest-api.md#GetClient)メソッドを呼び出します。

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar" -->

```cs-i
var outlookClient = await CreateOutlookClientAsync("Calendar");
var calendars = await outlookClient.Me.Calendars
  .Take(10)
  .ExecuteAsync();

foreach(var calendar in calendars.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine("Calendar '{0}'.", calendar.Name);
}

```
```javascript-i
outlookClient.me.calendars.getCalendars().fetchAll(100).then(function(result) {
    result.forEach(function (calendar) {
        console.log('Calendar "' + calendar.name + '", URL ' + calendar.path)
    });
}, function(error) {
    console.log(error);
});
```

<!-- ENDSECTION -->

****

<a name="CreateCalendars"> </a>
## 予定表を作成する

REST API:[予定表を作成する (REST)](#CreateACalendar)

クライアント ライブラリ:[予定表を作成する (クライアント)](#CreateCalendarsClient)

<a name="CreateACalendar"></a>
###予定表を作成する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

../me/calendars`../me/calendars` ショートカットを使用して既定の予定表グループに予定表を作成するか、グループの calendars`calendars` エンドポイントに投稿することによって特定の予定表グループに予定表を作成します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
POST https://outlook.office.com/api/beta/me/calendars
POST https://outlook.office.com/api/beta/me/calendargroups/{calendar_group_id}/calendars
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calender_group_id|string|予定表グループ ID (特定のグループから予定表を取得している場合)。|
|_Body parameters_|
|名前|string|新しい予定表の名前。|
 

**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/calendars
Content-Type: application/json

{
  "Name": "Social"
}

```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Calendars/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGE4xLHAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SQ==\"",
  "Id": "AAMkAGE4xLHAAA=",
  "Name": "Social",
  "Color": "LightBlue",
  "IsDefaultCalendar": false,
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SQ=="
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
POST https://outlook.office.com/api/v2.0/me/calendars
POST https://outlook.office.com/api/v2.0/me/calendargroups/{calendar_group_id}/calendars
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calender_group_id|string|予定表グループ ID (特定のグループから予定表を取得している場合)。|
|_Body parameters_|
|名前|string|新しい予定表の名前。|
 

**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/calendars
Content-Type: application/json

{
  "Name": "Social"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Calendars/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGE4xLHAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SQ==\"",
  "Id": "AAMkAGE4xLHAAA=",
  "Name": "Social",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SQ=="
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/calendars
POST https://outlook.office.com/api/v1.0/me/calendargroups/{calendar_group_id}/calendars
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calender_group_id|string|予定表グループ ID (特定のグループから予定表を取得している場合)。|
|_Body parameters_|
|名前|string|新しい予定表の名前。|
 


[!code-REST-i[calendar_api_create_calendar](./_data/calendar_api_create_calendar.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->



**応答の種類**

新しい[予定表](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource)。

****

<a name="CreateCalendarsClient"> </a>
### 予定表を作成する (クライアント)

予定表を作成する (クライアント) 予定表を作成します。イベントの作成方法の例については、「[イベントを作成する](#CreateEventsClient)」を参照してください。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、既に [Outlook サービス クライアントが取得されている](..\api\use-outlook-rest-api.md#GetClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
Calendar newCal = new Calendar
{
    Name = "Personal"
};

// Add the calendar to the Calendars collection
await client.Me.Calendars.AddCalendarAsync(newCal);

// Get the ID of the calendar
string calendarId = newCal.Id;
```

<!-- ENDSECTION -->

****


<a name="UpdateCalendars"> </a>
## 予定表を更新する

REST API:[予定表を更新する (REST)](#UpdateACalendar)

クライアント ライブラリ:[予定表を更新する (クライアント)](#UpdateCalendarsClient)

<a name="UpdateACalendar"></a>
###予定表を更新する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

Change the name of a calendar. 予定表の名前を変更します。**Name** は、唯一の書き込み可能な[予定表](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource)のプロパティです。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
PATCH https://outlook.office.com/api/beta/me/calendars/{calendar_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|
|_Body parameters_|
|名前|string|予定表の新しい名前。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/beta/me/calendars/AAMkAGE4xLIAAA=
Content-Type: application/json

{
  "Name": "Social events"
}
```

**応答のサンプル:**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Calendars/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGE4xLIAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Sw==\"",
  "Id": "AAMkAGE4xLIAAA=",
  "Name": "Social events",
  "Color": "LightBlue",
  "IsDefaultCalendar": false,
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Sw=="
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/calendars/{calendar_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|
|_Body parameters_|
|名前|string|予定表の新しい名前。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/v2.0/me/calendars/AAMkAGE4xLIAAA=
Content-Type: application/json

{
  "Name": "Social events"
}
```

**応答のサンプル:**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Calendars/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Calendars('AAMkAGE4xLIAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Sw==\"",
  "Id": "AAMkAGE4xLIAAA=",
  "Name": "Social events",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Sw=="
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v1.0/me/calendars/{calendar_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|
|_Body parameters_|
|名前|string|予定表の新しい名前。|

[!code-REST-i[calendar_api_update_calendar_by_id](./_data/calendar_api_update_calendar_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->



**応答の種類**

更新された[予定表](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource)。

****

<a name="UpdateCalendarsClient"> </a>
### 予定表を更新する (クライアント)

Change the name of a calendar. 予定表の名前を変更します。**Name** は、予定表の唯一の書き込み可能なプロパティです。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[予定表 ID](#GetCalendars) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Get an existing calendar by ID
ICalendar calendarToUpdate = await client.Me.Calendars[calendarId].ExecuteAsync();
calendarToUpdate.Name = "Family";

// Commit the change
await calendarToUpdate.UpdateAsync();

// Get the updated property
string newCalendarName = calendarToUpdate.Name;
```

<!-- ENDSECTION -->


次のパターンを使用して、クライアント側で複数の更新を定義し、要求をまとめて (バッチ処理で) 送信できます。
1. Call `UpdateAsync(true)` for each entity you want to update. 更新するエンティティごとに UpdateAsync(true)`true` を呼び出します。true を指定すると、更新がクライアント上でローカルに登録されますが、サーバーには投稿されません。
2. outlookClient.Context.SaveChangesAsync()`client.Context.SaveChangesAsync()` を呼び出して、ローカルに登録されているすべての更新を投稿します。

****

<a name="DeleteCalendars"> </a>
## 予定表を削除する

予定表を削除します。

REST API:[予定表を削除する (REST)](#DeleteACalendar)

クライアント ライブラリ:[予定表を削除する (クライアント)](#DeleteCalendarsClient)

<a name="DeleteACalendar"></a>
###予定表を削除する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]



```no-highlight
DELETE https://outlook.office.com/api/beta/me/calendars/{calendar_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/beta/me/calendars/AAMkAGE4xLIAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]



```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/calendars/{calendar_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/v2.0/me/calendars/AAMkAGE4xLIAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/calendars/{calendar_id}
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。|

[!code-REST-i[calendar_api_delete_calendar_by_id](./_data/calendar_api_delete_calendar_by_id.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->




****

<a name="DeleteCalendarsClient"> </a>
### 予定表を削除する (クライアント)


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[予定表 ID](#GetCalendars) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Get an existing calendar by ID
ICalendar calendarToDelete = await client.Me.Calendars[calendarId].ExecuteAsync();
    
// Delete the calendar
await calendarToDelete.DeleteAsync(false);
```

<!-- ENDSECTION -->

****

<a name="GetCalendarGroups"> </a>
## 予定表グループを取得する

予定表グループのコレクションまたは[予定表グループを取得](#GetCalendarGroup)できます。


**メモ** Outlook.com によってサポートされるのは、../me/calendars`../me/calendars` ショートカットでアクセス可能な既定の予定表グループのみです。


REST API:予定表グループのコレクションを取得する (REST)  予定表グループを取得する (REST)

クライアント ライブラリ:[予定表グループを取得する (クライアント)](#GetCalendarGroupsClient)


****


<a name="GetCalendarGroupCollection"> </a>
###予定表グループのコレクションを取得する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

メールボックス内の予定表グループを取得します。 


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/calendargroups
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

**要求のサンプル:**

```
GET https://outlook.office.com/api/beta/me/calendargroups
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/CalendarGroups",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGI2TGuKAAA=')",
            "Id": "AAMkAGI2TGuKAAA=",
            "Name": "My Calendars",
            "ClassId": "0006f0b7-0000-0000-c000-000000000046",
            "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+g=="
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGI2TGuMAAA=')",
            "Id": "AAMkAGI2TGuMAAA=",
            "Name": "Other Calendars",
            "ClassId": "0006f0b8-0000-0000-c000-000000000046",
            "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0/A=="
        }
    ]
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/calendargroups
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

**要求のサンプル:**

```
GET https://outlook.office.com/api/v2.0/me/calendargroups
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/CalendarGroups",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGI2TGuKAAA=')",
            "Id": "AAMkAGI2TGuKAAA=",
            "Name": "My Calendars",
            "ClassId": "0006f0b7-0000-0000-c000-000000000046",
            "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+g=="
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGI2TGuMAAA=')",
            "Id": "AAMkAGI2TGuMAAA=",
            "Name": "Other Calendars",
            "ClassId": "0006f0b8-0000-0000-c000-000000000046",
            "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0/A=="
        }
    ]
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/calendargroups
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


[!code-REST-i[calendar_api_get_calendar_groups](./_data/calendar_api_get_calendar_groups.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


**応答の種類**

要求された[予定表グループ](..\api\complex-types-for-mail-contacts-calendar.md#CalendarGroupResource)のコレクション。

****


<a name="GetCalendarGroup"> </a>
###予定表グループを取得する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.read_
- _wl.calendars_
- _wl.contacts\_calendars_

ID で予定表グループを取得します。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
GET https://outlook.office.com/api/beta/me/calendargroups/{calendar_group_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/calendargroups/AAMkAGI2TGuKAAA=
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/CalendarGroups/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGI2TGuKAAA=')",
    "Id": "AAMkAGI2TGuKAAA=",
    "Name": "My Calendars",
    "ClassId": "0006f0b7-0000-0000-c000-000000000046",
    "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+g=="
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

```no-highlight
GET https://outlook.office.com/api/v2.0/me/calendargroups/{calendar_group_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/calendargroups/AAMkAGI2TGuKAAA=
```

**応答のサンプル:**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/CalendarGroups/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGI2TGuKAAA=')",
    "Id": "AAMkAGI2TGuKAAA=",
    "Name": "My Calendars",
    "ClassId": "0006f0b7-0000-0000-c000-000000000046",
    "ChangeKey": "nfZyf7VcrEKLNoU37KWlkQAAA0x0+g=="
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/calendargroups/{calendar_group_id}
```

**メモ**： パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|


[!code-REST-i[calendar_api_get_calendar_groups_by_id](./_data/calendar_api_get_calendar_groups_by_id.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


**応答の種類**

要求された[予定表グループ](..\api\complex-types-for-mail-contacts-calendar.md#CalendarGroupResource)。

****

<a name="GetCalendarGroupsClient"> </a>
### 予定表グループを取得する (クライアント)

ユーザーの予定表グループを取得します。別の予定表グループを取得するには、**CalendarGroups** コレクションのインデックスとして予定表グループ ID を指定するか、**GetById** メソッドを使用します。

例`client.Me.CalendarGroups[calendarGroupId].ExecuteAsync()`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


**メモ**: 予定表グループコレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。

この例では、既に [Outlook サービス クライアントが取得されている](..\api\use-outlook-rest-api.md#GetClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IPagedCollection<ICalendarGroup> calendarGroupsResults = await client.Me.CalendarGroups.ExecuteAsync();

// Get the ID of the first calendar group
string groupId = calendarGroupsResults.CurrentPage[0].Id;
```

<!-- ENDSECTION -->

****


<a name="CreateCalendarGroups"> </a>
## 予定表グループを作成する

予定表グループを作成する (クライアント) 予定表グループを作成します。**Name** は、予定表グループの唯一の書き込み可能なプロパティです。


**メモ** Outlook.com によってサポートされるのは、../me/calendars`../me/calendars` ショートカットでアクセス可能な既定の予定表グループのみです。 You cannot create another calendar group in Outlook.com.


REST API:[予定表グループを作成する (REST)](#CreateACalendarGroup)

クライアント ライブラリ:[予定表グループを作成する (クライアント)](#CreateCalendarGroupsClient)

<a name="CreateACalendarGroup"></a>
###予定表グループを作成する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_



<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
POST https://outlook.office.com/api/beta/me/calendargroups
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメータ_|
|_Body parameters_|
|名前|string|予定表グループの名前。|


**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/calendargroups
Content-Type: application/json

{
  "Name": "Birthdays"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/CalendarGroups/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGE0M4xLGAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Rw==\"",
  "Id": "AAMkAGE0M4xLGAAA=",
  "Name": "Birthdays",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Rw==",
  "ClassId": "4d969bba-8942-42a0-ae33-c7d4410d1e11"
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
POST https://outlook.office.com/api/v2.0/me/calendargroups
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメータ_|
|_Body parameters_|
|名前|string|予定表グループの名前。|


**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/calendargroups
Content-Type: application/json

{
  "Name": "Birthdays"
}
```

**応答のサンプル:**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/CalendarGroups/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGE0M4xLGAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Rw==\"",
  "Id": "AAMkAGE0M4xLGAAA=",
  "Name": "Birthdays",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/Rw==",
  "ClassId": "4d969bba-8942-42a0-ae33-c7d4410d1e11"
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
POST https://outlook.office.com/api/v1.0/me/calendargroups
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメータ_|
|_Body parameters_|
|名前|string|予定表グループの名前。|


[!code-REST-i[calendar_api_create_calendar_group](./_data/calendar_api_create_calendar_group.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


**応答の種類**

新しい[予定表グループ](..\api\complex-types-for-mail-contacts-calendar.md#CalendarGroupResource)。


****

<a name="CreateCalendarGroupsClient"> </a>
### 予定表グループを作成する (クライアント)


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、既に [Outlook サービス クライアントが取得されている](..\api\use-outlook-rest-api.md#GetClient)と仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
CalendarGroup newCalendarGroup = new CalendarGroup
{
    Name = "Business"
};

// Add it to the CalendarGroups collection
await client.Me.CalendarGroups.AddCalendarGroupAsync(newCalendarGroup);

// Get the ID of the calendar group
string calendarGroupId = newCalendarGroup.Id;
```

<!-- ENDSECTION -->


****


<a name="UpdateCalendarGroups"> </a>
## 予定表グループを更新する

REST API:[予定表グループを更新する (REST)](#UpdateACalendarGroup)

クライアント ライブラリ:[予定表グループを更新する (クライアント)](#UpdateCalendarGroupsClient)

<a name="UpdateACalendarGroup"></a>
### 予定表グループを更新する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_

予定表グループの名前を変更します。Name は、唯一の書き込み可能な予定表グループ のプロパティです。 予定表グループの名前を変更します。**Name** は、唯一の書き込み可能な[予定表グループ](..\api\complex-types-for-mail-contacts-calendar.md#CalendarGroupResource) のプロパティです。


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
PATCH https://outlook.office.com/api/beta/me/calendargroups/{calendar_group_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|
|_Body parameters_|
|名前|string|更新された予定表グループの名前。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/beta/me/calendargroups/AAMkAGE0M4xLGAAA=
Content-Type: application/json

{
  "Name": "Holidays"
}
```

**応答のサンプル:**

```
{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/CalendarGroups/$entity",
  "@odata.id": "https://https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGE0MGM4xLGAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SA==\"",
  "Id": "AAMkAGE0MGM4xLGAAA=",
  "Name": "Holidays",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SA==",
  "ClassId": "4d969bba-8942-42a0-ae33-c7d4410d1e11"
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/calendargroups/{calendar_group_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|
|_Body parameters_|
|名前|string|更新された予定表グループの名前。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/v2.0/me/calendargroups/AAMkAGE0M4xLGAAA=
Content-Type: application/json

{
  "Name": "Holidays"
}
```

**応答のサンプル:**

```
{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/CalendarGroups/$entity",
  "@odata.id": "https://https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/CalendarGroups('AAMkAGE0MGM4xLGAAA=')",
  "@odata.etag": "W/\"Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SA==\"",
  "Id": "AAMkAGE0MGM4xLGAAA=",
  "Name": "Holidays",
  "ChangeKey": "Jj9S59cHB0Wq4jXUzBgDvQAAFeN/SA==",
  "ClassId": "4d969bba-8942-42a0-ae33-c7d4410d1e11"
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
PATCH https://outlook.office.com/api/v1.0/me/calendargroups/{calendar_group_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|
|_Body parameters_|
|名前|string|更新された予定表グループの名前。|


[!code-REST-i[calendar_api_update_calendar_group_by_id](./_data/calendar_api_update_calendar_group_by_id.json)]


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

**応答の種類**

更新された[予定表グループ](..\api\complex-types-for-mail-contacts-calendar.md#CalendarGroupResource)。

****

<a name="UpdateCalendarGroupsClient"> </a>
### 予定表グループを更新する (クライアント)

予定表グループの名前を変更します。Name は、唯一の書き込み可能な予定表グループ のプロパティです。 予定表グループの名前を変更します。**Name** は、予定表グループの唯一の書き込み可能なプロパティです。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[予定表グループ ID](#GetCalendarGroups) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Get an existing calendar group by ID
ICalendarGroup groupToUpdate = await client.Me.CalendarGroups[groupId].ExecuteAsync();
groupToUpdate.Name = "Contoso";

// Commit the change
await groupToUpdate.UpdateAsync();

// Get the updated property
string newCalendarGroupName = groupToUpdate.Name;
```

<!-- ENDSECTION -->


次のパターンを使用して、クライアント側で複数の更新を定義し、要求をまとめて (バッチ処理で) 送信できます。
1. Call `UpdateAsync(true)` for each entity you want to update. 更新するエンティティごとに UpdateAsync(true)`true` を呼び出します。true を指定すると、更新がクライアント上でローカルに登録されますが、サーバーには投稿されません。
2. outlookClient.Context.SaveChangesAsync()`client.Context.SaveChangesAsync()` を呼び出して、ローカルに登録されているすべての更新を投稿します。


****


<a name="DeleteCalendarGroups"> </a>
## 予定表グループを削除する

予定表グループを削除します。


**メモ** Outlook.com によってサポートされるのは、../me/calendars`../me/calendars` ショートカットでアクセス可能な既定の予定表グループのみです。 Do not delete this calendar group.


REST API:[予定表グループを削除する (REST)](#DeleteACalendarGroup)

クライアント ライブラリ:[予定表グループを削除する (クライアント)](#DeleteCalendarGroupsClient)

<a name="DeleteACalendarGroup"></a>
###予定表グループを削除する (REST)

__**最低限必要な範囲**: 次のいずれか:__
- _https://outlook.office.com/calendars.readwrite_
- _wl.calendars\_update_
- _wl.events\_create_


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


```no-highlight
DELETE https://outlook.office.com/api/beta/me/calendargroups/{calendar_group_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/beta/me/calendargroups/AAMkAGE0MGM4xLGAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/calendargroups/{calendar_group_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|

**要求のサンプル**

```
DELETE https://outlook.office.com/api/v2.0/me/calendargroups/AAMkAGE0MGM4xLGAAA=
```

**応答のサンプル:**

```
Status code: 204
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/calendargroups/{calendar_group_id}
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_group_id|string|予定表グループ ID。|


[!code-REST-i[calendar_api_delete_calendar_group_by_id](./_data/calendar_api_delete_calendar_group_by_id.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


****

<a name="DeleteCalendarGroupsClient"> </a>
### 予定表グループを削除する (クライアント)


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアント](..\api\use-outlook-rest-api.md#GetClient)と[予定表グループ ID](#GetCalendarGroups) が既に取得されていると仮定します。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Get an existing calendar group by ID
ICalendarGroup groupToDelete = await client.Me.CalendarGroups[groupId].ExecuteAsync();

// Delete the group
await groupToDelete.DeleteAsync(); 
```

<!-- ENDSECTION -->

****

<a name="NextSteps"> </a>
## 次のステップ

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。


- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。
  
- Office REST API を実際に試してみるには、対話形式の [API サンドボックス](http://apisandbox.msdn.microsoft.com/)をお使いください。
    
- Want samples? サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。
    

Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)
    
- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)
    
- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)
  
- [メール API リファレンス](..\api\mail-rest-operations.md)
  
- [連絡先 API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API (プレビュー)](..\api\task-rest-operations.md)

- [OneDrive API](https://dev.onedrive.com/)

- [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)



[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]



