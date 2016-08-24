---
ms.Toctitle: Sync events in calendar view
title: "Outlook カレンダー ビューでイベントを同期する"
description: "ユーザーの予定表で特定の時間範囲のイベントを同期化する手順を、段階を追って説明します。"
ms.ContentId: afcf00f0-9222-470c-bb2b-a0f5e1a63a30
ms.date: May 19, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

#Outlook カレンダー ビューでイベントを同期する

_**適用対象:**Exchange Online | Office 365_


Outlook 予定表 REST API を使用すると、ユーザーの予定表にある特定の時間範囲の新しいイベント、変更されたイベント、削除されたイベントを同期して取得できます。 [カレンダー ビューを同期する](..\api\calendar-rest-operations.md#SyncCalendarView)ための API は、次のことを指定する、カレンダー ビューに対する GET 操作です。
- 同期対象の予定表
- イベントを同期する時間範囲の開始日と終了日
- この GET 操作を、[カレンダー ビューの GET](..\api\calendar-rest-operations.md#GetCalendarView) 操作とは区別する特殊な要求ヘッダー
  - "Prefer: odata.track-changes" - すべての要求にこのヘッダーを含める必要があります。ただし、`skipToken` が含まれる場合には例外です (詳細は、ステップ 2 で説明します)。  
  - "Prefer: odata.maxpagesize={x}" - 同期要求が一度に返すことができるイベントの最大数を指定するために、このヘッダーを含めることができます。

カレンダー ビューの同期操作には 1 つ以上の要求を含めることができます。各要求が 1 つの GET 呼び出しになります。
カレンダー ビューを同期するために必要な GET 呼び出しの数は、指定した時間範囲の「差分」(新規イベント、更新イベント、削除イベントの数)、および各 GET 呼び出しに指定した `maxpagesize` によって異なります。 一連の GET 呼び出しによってカレンダー ビューを同期したのちにさらに同期を試行しても、対象のカレンダー ビューで再びイベントが追加されたり、変更されたり、削除されたりしていない限りは、差分は返されません。

<a name="SyncCalendarViewWorkflow"> </a>
## カレンダー ビューを同期するためのワークフロー

複数の GET 呼び出しを標準的な一連の同期でチェーン化する方法について、次に説明します。

![初期同期要求は deltalink を返します。 deltaToken および後続の skipToken を使用して、次の要求を行います。 deltalink が再び返された場合、クライアントはサービスと同期されています。](images\O365API_SyncCalView_Process.png) 

1. [最初の同期要求](#SampleInitialSyncReq):最初の同期要求は、同期状態を設定します。  
2. [最初の同期応答](#SampleInitialSyncResponse): 
  - 応答ヘッダーの "Preference-Applied: odata.track-changes" を調べて、同期が正常に行われたことと、リソースが同期をサポートしていることを確認します。
  - 同期が正常に実行された場合、初期の応答には、必ず `@odata.deltaLink` が `deltaToken` 値とともに含まれます。 
  応答に何らかのデータが含まれる場合、2 番目の要求のために `deltaToken` 値を保存します。
  - 初期応答が正常に行われなかった場合や、データが何も返されない場合は、指定したカレンダー ビューにイベントがなく、このラウンドの同期は終了します。  
3. 以降の同期要求:前の要求の `deltaToken` 値または `skipToken` 値を使用して、次の要求を発行します。 例として、[2 番目](#SampleSecondSyncReq)と [3 番目](#SampleThirdSyncReq)の同期要求を参照してください。
4. [以降の同期応答](#SampleSecondSyncResponse)
  - 応答によって何らかのデータが返り、対象の時間範囲に同期するデータがさらにある場合、応答には `@odata.nextLink` 値と `skipToken` 値が含まれます。 次の同期要求のために `skipToken` を保存します。
  - ステップ 3 に戻り、`nextLink` に従って (存在する場合)、次の同期要求で対応する `skipToken` 値を適用します。その後、後続の `nextLink` に従い、それを対象予定表の時間範囲内にあるすべてのデータを同期するまで繰り返します。
5. [最後の同期応答](#SampleThirdSyncResponse):カレンダー ビューのすべてのイベントを同期すると、この一連の動作の最後の応答には、`@odata.deltaLink` と `deltaToken` が再び入ります。 次の一連の同期のために、`deltaToken` 値を保存します。

一連の同期要求では、指定の時間範囲と予定表内のすべてのイベントを取得します。 次の一連の同期では、以前の一連の同期要求の `deltaToken` 値を使用して、最後の一連の同期以降の差異 (新しい追加イベント、変更イベント、削除イベント) のみが返ります。 
 

<a name="SyncCalendarViewRecurrence"> </a>
## 定期的なイベントと同期

予定表ビューの同期で定期的なイベントを処理する方法に関して知っておく必要がある情報を次に示します。

- サービスは会議の拡張を実行し、タイム ウィンドウ内のシリーズ マスター イベントとすべてのイベント インスタンスを送信します。
- シリーズ マスター イベントには、シリーズの定期パターンとタイム ウィンドウが含まれます。 
- イベントのインスタンスには、開始と終了時の情報およびイベント発生例外に関する情報が含まれます。 


<a name="SyncCalendarViewDeletions"> </a>
## 削除されたイベントと同期
削除されたイベントには、削除されたエンティティを示す値 "deleted" に設定された reason プロパティが含まれます。イベントが定期的マスター イベントである場合、すべての発生と例外を削除する必要があります。 



<a name="NextSteps"> </a>
## 次の手順

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。


- 開始する準備ができている場合は、 初めに[環境をセットアップし](..\howto\setup-development-environment.md)、次に[最初のアプリのウォークスルー](..\howto\getting-started-Office-365-APIs.md)をご覧ください。
    
- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](http://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。
    

Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)
    
- [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)
    
- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)
  
- [予定表 REST API 操作のリファレンス](..\api\calendar-rest-operations.md)

- [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)

- [メール、予定表、連絡先 REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)

<a name="SyncCalendarViewSamples"> </a>

## 付録:同期の要求と応答の例

このセクションには、ユーザーの既定の予定表の時間範囲に対する、一連の 3 つの同期要求と応答が含まれます。

<a name="SampleInitialSyncReq"> </a>

### 最初の同期要求

```
GET https://contoso.com/api/v1.0/me/calendarview?startDateTime=2015-04-25T00:00:00Z&endDateTime=2015-05-30T00:00:00Z HTTP/1.1
```

要求ヘッダーには、同期応答で最大 3 つのイベントを要求する次の 2 行が含まれています。
```
Prefer: odata.track-changes
Prefer: odata.maxpagesize=3
```

<a name="SampleInitialSyncResponse"> </a>

### 最初の同期応答

応答ヘッダーには、同期が成功したことを示す次の行が含まれています。
```
HTTP/1.1 200 OK
Preference-Applied: odata.track-changes
```

応答本体には、反復しない 3 つの単一イベントが含まれています。また、`deltaToken` 値が入っている `@odata.deltaLink` も含まれています。 
```
{
    "@odata.context": "https://contoso.com/api/v1.0/$metadata#Me/CalendarView",
    "value": [
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE1M2==_bs88AAAFKPQUAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAABSlf9A==\"",
            "Id": "AAMkAGE1M2==_bs88AAAFKPQUAAA=",
            "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAABSlf9A==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-24T23:18:44.1912484Z",
            "DateTimeLastModified": "2015-04-24T23:18:44.8932487Z",
            "Subject": "Bug bash",
            "BodyPreview": "Let's get this right!",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\n</body>\r\n</html>\r\n"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-04-24T23:30:00Z",
            "StartTimeZone": "Pacific Standard Time",
            "End": "2015-04-25T00:00:00Z",
            "EndTimeZone": "Pacific Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "My house",
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
                {
                    "EmailAddress": {
                        "Address": "may@contoso.com",
                        "Name": "May Walton"
                    },
                    "Status": {
                        "Response": "None",
                        "Time": "0001-01-01T00:00:00Z"
                    },
                    "Type": "Required"
                }
            ],
            "Recurrence": null,
            "Organizer": {
                "EmailAddress": {
                    "Address": "chasity@contoso.com",
                    "Name": "Chasity Bonner"
                }
            },
            "iCalUId": "040000008200==F283EB",
            "WebLink": "https://contoso.com/owa/?ItemID=AAMkAGE1M2%3D%3D%2Bbs88AAAAAAENAACY4MQpaFz9SbqUDe4%2Bbs88AAAFKPQUAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE1M2==_bs88AAAFKPQXAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAABSlf+A==\"",
            "Id": "AAMkAGE1M2==_bs88AAAFKPQXAAA=",
            "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAABSlf+A==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-25T03:23:32.2628681Z",
            "DateTimeLastModified": "2015-04-25T03:23:32.2784683Z",
            "Subject": "Dinner!",
            "BodyPreview": "",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\n</body>\r\n</html>\r\n"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-04-25T01:00:00Z",
            "StartTimeZone": "Eastern Standard Time",
            "End": "2015-04-25T01:30:00Z",
            "EndTimeZone": "Eastern Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "Kitchen",
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
                    "Address": "chasity@contoso.com",
                    "Name": "Chasity Bonner"
                }
            },
            "iCalUId": "040000008200==4F6A35",
            "WebLink": "https://contoso.com/owa/?ItemID=AAMkAGE1M2%3D%3D%2Bbs88AAAFKPQXAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE1M2==_bs88AAAFKPQcAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAABSlgBw==\"",
            "Id": "AAMkAGE1M2==_bs88AAAFKPQcAAA=",
            "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAABSlgBw==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-26T02:54:03.4260923Z",
            "DateTimeLastModified": "2015-04-26T02:54:03.6132924Z",
            "Subject": "Discuss all the REST API",
            "BodyPreview": "I think it will meet our requirements!",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r\n"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-04-26T02:00:00Z",
            "StartTimeZone": "Pacific Standard Time",
            "End": "2015-04-26T03:00:00Z",
            "EndTimeZone": "Pacific Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "",
                "Address": null,
                "Coordinates": null
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
                {
                    "EmailAddress": {
                        "Address": "kristopher@contoso.com",
                        "Name": "Kristopher Nemeth"
                    },
                    "Status": {
                        "Response": "None",
                        "Time": "0001-01-01T00:00:00Z"
                    },
                    "Type": "Required"
                }
            ],
            "Recurrence": null,
            "Organizer": {
                "EmailAddress": {
                    "Address": "chasity@contoso.com",
                    "Name": "Chasity Bonner"
                }
            },
            "iCalUId": "040000008200==D1D0BF",
            "WebLink": "https://contoso.com/owa/?ItemID=AAMkAGE1M2%3D%3D%2Bbs88AAAAAAENAACY4MQpaFz9SbqUDe4%2Bbs88AAAFKPQcAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        }
    ],
    "@odata.deltaLink": "https://contoso.com/api/v1.0/me/calendarview/?startDateTime=2015-04-25T00%3a00%3a00Z&endDateTime=2015-05-30T00%3a00%3a00Z&%24deltatoken=d17ffb043b724d3c9521e8464ed318d6"
}
```

<a name="SampleSecondSyncReq"> </a>

### 2 番目の同期要求

2 番目の同期要求は、最初の応答の `deltaToken` 値を使用します。

```
GET https://contoso.com/api/v1.0/me/calendarview?startDateTime=2015-04-25T00:00:00Z&endDateTime=2015-05-30T00:00:00Z&$deltatoken=d17ffb043b724d3c9521e8464ed318d6 HTTP/1.1
```

要求ヘッダーには、次の 2 行が含まれています。
```
Prefer: odata.track-changes
Prefer: odata.maxpagesize=3
```


<a name="SampleSecondSyncResponse"> </a>

### 2 番目の同期応答

2 番目の応答ヘッダーには、同期が成功したことを示す次の行が含まれています。
```
HTTP/1.1 200 OK
```

2 番目の応答本体には、一連のマスター イベント、2 つの単一インスタンス、および一連のマスター イベントに関連付けられている 5 回のイベントが含まれています。 また応答本体には、`skipToken` 値が含まれている `@odata.nextLink` も入っています。
```
{
    "@odata.context": "https://contoso.com/api/v1.0/$metadata#Me/CalendarView/$delta",
    "value": [
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==_bs88AAAFKPQVAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAB7h3QQ==\"",
            "Id": "AAMkAGE==_bs88AAAFKPQVAAA=",
            "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAAB7h3QQ==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-25T00:18:24.7188484Z",
            "DateTimeLastModified": "2015-04-29T01:27:42.8480225Z",
            "Subject": "Little nap",
            "BodyPreview": "",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\n</body>\r\n</html>\r\n"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-04-25T00:30:00Z",
            "StartTimeZone": "Pacific Standard Time",
            "End": "2015-04-25T01:00:00Z",
            "EndTimeZone": "Pacific Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "In the sun",
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
            "Type": "SeriesMaster",
            "SeriesMasterId": null,
            "Attendees": [
            ],
            "Recurrence": {
                "Pattern": {
                    "Type": "Daily",
                    "Interval": 1,
                    "Month": 0,
                    "Index": "First",
                    "FirstDayOfWeek": "Sunday",
                    "DayOfMonth": 0
                },
                "Range": {
                    "Type": "EndDate",
                    "StartDate": "2015-04-24T00:00:00-07:00",
                    "EndDate": "2015-04-28T00:00:00-07:00",
                    "NumberOfOccurrences": 0
                }
            },
            "Organizer": {
                "EmailAddress": {
                    "Address": "chasity@contoso.com",
                    "Name": "Chasity Bonner"
                }
            },
            "iCalUId": "040000008200==C98B54",
            "WebLink": "https://contoso.com/owa/?ItemID=AAMkAGE%3D%3D%2Bbs88AAAFKPQVAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==_bs88AAAFKPQdAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAABSlgCg==\"",
            "Id": "AAMkAGE==_bs88AAAFKPQdAAA=",
            "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAABSlgCg==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-04-26T02:54:17.4192923Z",
            "DateTimeLastModified": "2015-04-26T02:54:17.6064929Z",
            "Subject": "Discuss all the REST API",
            "BodyPreview": "I think it will meet our requirements!",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nI think it will meet our requirements!\r\n</body>\r\n</html>\r

\n"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-04-26T02:00:00Z",
            "StartTimeZone": "Pacific Standard Time",
            "End": "2015-04-26T03:00:00Z",
            "EndTimeZone": "Pacific Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "",
                "Address": null,
                "Coordinates": null
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
                {
                    "EmailAddress": {
                        "Address": "diana@contoso.com",
                        "Name": "Diana Michael"
                    },
                    "Status": {
                        "Response": "None",
                        "Time": "0001-01-01T00:00:00Z"
                    },
                    "Type": "Required"
                }
            ],
            "Recurrence": null,
            "Organizer": {
                "EmailAddress": {
                    "Address": "chasity@contoso.com",
                    "Name": "Chasity Bonner"
                }
            },
            "iCalUId": "040000008200==CDA6AD",
            "WebLink": "https://contoso.com/owa/?ItemID=AAMkAGE%3D%3D%2Bbs88AAAFKPQdAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        },

        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==_bs88AAAKB6KtAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAACgkNZA==\"",
            "Id": "AAMkAGE==_bs88AAAKB6KtAAA=",
            "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAACgkNZA==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-05-05T11:15:43.0696852Z",
            "DateTimeLastModified": "2015-05-05T11:15:43.1944851Z",
            "Subject": "Outlook APIs talk",
            "BodyPreview": "",
            "Body": {
                "ContentType": "HTML",
                "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\n</body>\r\n</html>\r\n"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-05-06T17:30:00Z",
            "StartTimeZone": "Pacific Standard Time",
            "End": "2015-05-06T18:30:00Z",
            "EndTimeZone": "Pacific Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "Downtown park",
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
                }
            },
            "ResponseStatus": {
                "Response": "NotResponded",
                "Time": "0001-01-01T00:00:00Z"
            },
            "ShowAs": "Tentative",
            "IsAllDay": false,
            "IsCancelled": false,
            "IsOrganizer": false,
            "ResponseRequested": true,
            "Type": "SingleInstance",
            "SeriesMasterId": null,
            "Attendees": [
                {
                    "EmailAddress": {
                        "Address": "Donovan@contoso.com",
                        "Name": "Donovan Sandberg"
                    },
                    "Status": null,
                    "Type": "Required"
                },
                {
                    "EmailAddress": {
                        "Address": "may@contoso.com",
                        "Name": "May Walton"
                    },
                    "Status": null,
                    "Type": "Required"
                },
                {
                    "EmailAddress": {
                        "Address": "Kristopher@contoso.com",
                        "Name": "Kristopher Nemeth"
                    },
                    "Status": null,
                    "Type": "Required"
                }
            ],
            "Recurrence": null,
            "Organizer": {
                "EmailAddress": {
                    "Address": "Donovan@contoso.com",
                    "Name": "Donovan Sandberg"
                }
            },
            "iCalUId": "040000008200==C81231",
            "WebLink": "https://contoso.com/owa/?ItemID=AAMkAGE%3D%3d%2Bbs88AAAKB6KtAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAB7h3QQ==\"",
            "Id": "AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==",
            "SeriesMasterId": "AAMkAGE_bs88AAAFKPQVAAA=",
            "Start": "2015-04-25T00:30:00Z",
            "End": "2015-04-25T01:00:00Z",
            "Type": "Occurrence"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAB7h3QQ==\"",
            "Id": "AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==",
            "SeriesMasterId": "AAMkAGE_bs88AAAFKPQVAAA=",
            "Start": "2015-04-26T00:30:00Z",
            "End": "2015-04-26T01:00:00Z",
            "Type": "Occurrence"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAB7h3QQ==\"",
            "Id": "AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==",
            "SeriesMasterId": "AAMkAGE_bs88AAAFKPQVAAA=",
            "Start": "2015-04-27T00:30:00Z",
            "End": "2015-04-27T01:00:00Z",
            "Type": "Occurrence"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAB7h3QQ==\"",
            "Id": "AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==",
            "SeriesMasterId": "AAMkAGE_bs88AAAFKPQVAAA=",
            "Start": "2015-04-28T00:30:00Z",
            "End": "2015-04-28T01:00:00Z",
            "Type": "Occurrence"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAB7h3QQ==\"",
            "Id": "AAMkAGE-Um6lA3uPm7PPAAABSj0FQAAEA==",
            "SeriesMasterId": "AAMkAGE_bs88AAAFKPQVAAA=",
            "Start": "2015-04-29T00:30:00Z",
            "End": "2015-04-29T01:00:00Z",
            "Type": "Occurrence"
        }
    ],
    "@odata.nextLink": "https://contoso.com/api/v1.0/me/calendarview/?startDateTime=2015-04-25T00%3a00%3a00Z&endDateTime=2015-05-30T00%3a00%3a00Z&%24skipToken=a1e5b10261804221aceb856143b8af19"
}
```

<a name="SampleThirdSyncReq"> </a>

### 最後の同期要求

3 番目の同期要求は、2 番目の応答の `skipToken` 値を使用します。

```
GET https://contoso.com/api/v1.0/me/calendarview?startDateTime=2015-04-25T00:00:00Z&endDateTime=2015-05-30T00:00:00Z&$skiptoken=a1e5b10261804221aceb856143b8af19 HTTP/1.1
```

要求ヘッダーには、次の 2 行が含まれています。
```
Prefer: odata.track-changes
Prefer: odata.maxpagesize=3
```

<a name="SampleThirdSyncResponse"> </a>

### 最後の同期応答

最後の応答ヘッダーには、同期が成功したことを示す次の行が含まれています。
```
HTTP/1.1 200 OK
```

最後の応答本体には、1 つの一連のマスター イベント、およびその一連のマスター イベントに関連付けられている 4 つのイベントが含まれています。 またこの応答本体には、`deltaToken` 値が入っている `@odata.deltaLink` も含まれ、これは対象のカレンダー ビューの同期が完了したことを示します。

```

{
    "@odata.context": "https://contoso.com/api/v1.0/$metadata#Me/CalendarView/$delta",
    "value": [
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==_bs88AAAKB6KrAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAACgkNYA==\"",
            "Id": "AAMkAGE==_bs88AAAKB6KrAAA=",
            "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAACgkNYA==",
            "Categories": [
            ],
            "DateTimeCreated": "2015-03-05T11:14:13.7752849Z",
            "DateTimeLastModified": "2015-03-05T11:14:13.8220851Z",
            "Subject": "Breakfast at Cafe",
            "BodyPreview": "",
            "Body": {
                "ContentType": "HTML",
                "Content": ""<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\n</body>\r\n</html>\r\n"
            },
            "Importance": "Normal",
            "HasAttachments": false,
            "Start": "2015-04-27T15:00:00Z",
            "StartTimeZone": "Pacific Standard Time",
            "End": "2015-04-27T16:00:00Z",
            "EndTimeZone": "Pacific Standard Time",
            "Reminder": 15,
            "Location": {
                "DisplayName": "City Hall",
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
                }
            },
            "ResponseStatus": {
                "Response": "NotResponded",
                "Time": "0001-01-01T00:00:00Z"
            },
            "ShowAs": "Tentative",
            "IsAllDay": false,
            "IsCancelled": false,
            "IsOrganizer": false,
            "ResponseRequested": true,
            "Type": "SeriesMaster",
            "SeriesMasterId": null,
            "Attendees": [
                {
                    "EmailAddress": {
                        "Address": "Donovan@contoso.com",
                        "Name": "Donovan Sandberg"
                    },
                    "Status": null,
                    "Type": "Required"
                },
                {
                    "EmailAddress": {
                        "Address": "May@contoso.com",
                        "Name": "May Walton"
                    },
                    "Status": null,
                    "Type": "Required"
                },
                {
                    "EmailAddress": {
                        "Address": "Kristopher@contoso.com",
                        "Name": "Kristopher Nemeth"
                    },
                    "Status": null,
                    "Type": "Required"
                }
            ],
            "Recurrence": {
                "Pattern": {
                    "Type": "Daily",
                    "Interval": 1,
                    "Month": 0,
                    "Index": "First",
                    "FirstDayOfWeek": "Sunday",
                    "DayOfMonth": 0
                },
                "Range": {
                    "Type": "EndDate",
                    "StartDate": "2015-04-27T00:00:00-07:00",
                    "EndDate": "2015-04-30T00:00:00-07:00",
                    "NumberOfOccurrences": 0
                }
            },
            "Organizer": {
                "EmailAddress": {
                    "Address": "Donovan@contoso.com",
                    "Name": "Donovan Sandberg"
                }
            },
            "iCalUId": "040000008200==D272AE",
            "WebLink": "https://contoso.com/owa/?ItemID=AAMkAGE%3D%3D2Bbs88AAAKB6KrAAA3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==7PPAAACgeiqwAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAACgkNYA==\"",
            "Id": "AAMkAGE==7PPAAACgeiqwAAEA==",
            "SeriesMasterId": "AAMkAGE==_bs88AAAKB6KrAAA=",
            "Start": "2015-04-27T15:00:00Z",
            "End": "2015-04-27T16:00:00Z",
            "Type": "Occurrence"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==7PPAAACgeiqwAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAACgkNYA==\"",
            "Id": "AAMkAGE==7PPAAACgeiqwAAEA==",
            "SeriesMasterId": "AAMkAGE==_bs88AAAKB6KrAAA=",
            "Start": "2015-04-28T15:00:00Z",
            "End": "2015-04-28T16:00:00Z",
            "Type": "Occurrence"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==7PPAAACgeiqwAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAACgkNYA==\"",
            "Id": "AAMkAGE==7PPAAACgeiqwAAEA==",
            "SeriesMasterId": "AAMkAGE==_bs88AAAKB6KrAAA=",
            "Start": "2015-04-29T15:00:00Z",
            "End": "2015-04-29T16:00:00Z",
            "Type": "Occurrence"
        },
        {
            "@odata.id": "https://contoso.com/api/v1.0/Users('chasity@contoso.com')/Events('AAMkAGE==7PPAAACgeiqwAAEA==')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAACgkNYA==\"",
            "Id": "AAMkAGE==7PPAAACgeiqwAAEA==",
            "SeriesMasterId": "AAMkAGE==_bs88AAAKB6KrAAA=",
            "Start": "2015-04-30T15:00:00Z",
            "End": "2015-04-30T16:00:00Z",
            "Type": "Occurrence"
        },

    ],
    "@odata.deltaLink": "https://contoso.com/api/v1.0/me/calendarview/?startDateTime=2015-04-25T00%3a00%3a00Z&endDateTime=2015-05-30T00%3a00%3a00Z&%24deltaToken=294a8f04cc0345c5ae093d484629e186"
}
```
