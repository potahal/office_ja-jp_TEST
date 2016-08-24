---
ms.Toctitle: Outlook Task REST API reference (preview)
title: "Outlook タスク REST API リファレンス (プレビュー)"
description: "Outlook および Outlook.com でタスクを作成、取得、更新、削除する REST API リファレンス。"
ms.ContentId: 6c3b5d47-656c-4a72-8427-c252dc1406db
ms.date: June 28, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Outlook タスク REST API リファレンス (プレビュー)

[!INCLUDE [Add the Outlook REST API filters--beta default v1 v2 disabled](../includes/controls/addOutlookversion_betadefault_v1v2disabled.xml)] 

 _**適用対象:**Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

<p class="previewnote">このドキュメントで取り上げる Outlook タスク REST API は、現時点ではプレビュー段階にあります。 プレビュー機能は、最終版までに変更される場合があり、それらの機能を使用するコードが動作しなくなる場合もあります。 このため、一般に、運用コードでは運用バージョンの API のみを使用してください。 入手可能な場合、現時点ではバージョン 2.0 が優先バージョンです。</p>

Outlook タスク REST API では、Office 365 での Azure Active Directory でセキュリティ保護された ユーザーのタスクを作成、読み取り、同期、更新、および削除することができます。 このユーザーのアカウントは、Office 365 アカウントまたは Microsoft アカウント (Hotmail.com、Live.com、MSN.com、Outlook.com、または Passport.com) にすることができます。

**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

## 概要
Outlook の [タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource) を使用してタスク アイテムをトラックすることができます。トラックの開始日、期限、実際の終了日、トラックの進行状況や状態、トラックを定期的に行うか、通知を必要とするかをメモすることができます。 

タスクは、[タスク フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource)で構成されます。タスク フォルダーは、[タスク グループ](..\api\complex-types-for-mail-contacts-calendar.md#TaskGroupResource)で構成されます。 各メールボックスには、既定の作業フォルダー (**Name** プロパティ `Tasks`) と既定のタスク グループ (**Name** プロパティは `My Tasks` です) があります。 

## すべてのタスク API の操作

**タスクの操作** &nbsp;
[タスクの作成](#CreateTasks) | [タスクの取得](#GetTasks) | [タスクの更新](#UpdateTasks) | [タスクの削除](#DeleteTasks) | 
[タスクの完了](#CompleteTasks) | [タスクまたはタスク フォルダーの同期](#SyncTasks) 


**タスク フォルダーの操作** &nbsp;
[タスク フォルダーの作成](#CreateTaskFolders) | [タスク フォルダーの取得](#GetTaskFolders) | [タスク フォルダーの更新](#UpdateTaskFolders) | 
[タスク フォルダーの削除](#DeleteTaskFolders) | [タスクまたはタスク フォルダーの同期](#SyncTasks) 


**タスク グループの操作** &nbsp;
[タスク グループの作成](#CreateTaskGroups) | [タスク グループの取得](#GetTaskGroups) | [タスク グループの更新](#UpdateTaskGroups) | 
[タスク グループの削除](#DeleteTaskGroups)


## タスク REST API の使用

###認証
他の [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI) と同様に、データ拡張機能 API へのすべての要求に対して、有効なアクセス トークンを含める必要があります。 アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。 効率化された登録と承認のオプションに関する[詳細情報](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow)を参照してください。
データ拡張機能 API で特定の操作を続行する場合は、この点に留意してください。

###API のバージョン

現時点では、タスク API はプレビュー状態で表示され、ベータ版の REST API エンドポイントでのみ利用できます。

`https://outlook.office.com/api/beta/`


### 対象ユーザー
タスク API 要求は、常にサインイン ユーザーのために実行されます。 

### URL のパラメーター

この記事の例では、REST 要求 URL のパラメーターに次の ID のプレース ホルダーを使用します。拡張機能を作成する対象のリソースのインスタンスの ID を指定する必要があります。

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|folder_id|string|既定の `Tasks` の既知のフォルダー名、またはユーザーのメールボックス内で一意である、タスク フォルダーの数値 ID。 |
|group_id|string|ユーザーのメールボックス内で一意である、タスク グループの数値 ID。 |
|task_id|string|ユーザーのメールボックス内で一意である、数値タスク ID。 |

<a name="NoteAboutStartDueDateTimes"></a>
### StartDateTime および DueDateTime プロパティの指定

タスクを作成する場合:
- **StartDateTime** および **DueDateTime** はオプションですが、**StartDateTime** を設定する場合は、**DueDateTime** を同じ日またはそれ以降の日付に設定する必要があります。 
- **StartDateTime** のみを設定した場合、**DueDateTime** は自動的に **StartDateTime** と同じ値に設定されます。 
- **DueDateTime** を `null` に設定すると、**StartDateTime** も自動的に `null` に設定されます。

タスクの作成または更新時に **StartDateTime** または **DueDateTime** を設定する場合:
- 日付とタイム ゾーンの情報を指定します。
- POST (または PATCH) メソッドでは、特定の時間を指定しても常に無視され、指定されたタイム ゾーンの午前 0 時が前提となっているため、これらのプロパティでは特定の時刻を指定しないでください。 
- 既定では、POST (または PATCH) メソッドにより、この値は UTC に変換され、応答も UTC で返されます。 

たとえば、**StartDateTime** で 4 月 26 日の東部標準時 (EST) を指定した場合:
```
  "StartDateTime": {
      "DateTime": "2016-04-26T09:00:00",
      "TimeZone": "Eastern Standard Time"
  }
```

POST (または PATCH) は時間部分を無視し、4 月 26 の午前 0 時 (EST) を UTC に変換し、応答でその値を UTC で返します。
```
  "StartDateTime": {
    "DateTime": "2016-04-26T04:00:00.0000000",
    "TimeZone": "UTC"
  }
```

`Prefer: outlook.timezone` ヘッダーを使用して、応答内のすべての日付関連プロパティを UTC 以外のタイム ゾーンで表すことができます。 

<a name="NoteAboutPreferHeader"></a>
### カスタム タイム ゾーンの日付関連プロパティを返す

[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource) リソースの日付関連プロパティには次が含まれます。
- **CompletedDateTime**
- **CreatedDateTime**
- **DueDateTime**
- **LastModifiedDateTime**
- **ReminderDateTime**
- **StartDateTime**

既定では、POST、GET、PATCH、および Complete 操作は、REST 応答の日付関連プロパティを UTC で返します。 `Prefer: outlook.timezone` ヘッダーを使用して、応答内のすべての日付関連プロパティを UTC 以外のタイム ゾーンで表すことができます。 次の例では、対応する応答の日付関連プロパティが EST で返されています。

```
Prefer: outlook.timezone="Eastern Standard Time"
```

Outlook REST API のすべてのサブセットに共通な情報の詳細については、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。


****

<a name="CreateTasks"> </a>
## タスクの作成 

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

タスクを作成します。 2 つの主なシナリオがあります。

ユーザーのメールボックスの既定のタスク グループ (`My Tasks`) および既定のタスク フォルダー (`Tasks`) にタスクを作成することができます。 この場合、タスク グループまたはタスク フォルダーを指定する必要はありません。

```no-highlight
POST https://outlook.office.com/api/beta/me/tasks
```

次のように、特定のタスク フォルダーにタスクを作成することもできます。

```no-highlight
POST https://outlook.office.com/api/beta/me/taskfolders('{folder_id}')/tasks
```

要求本文で、作成する[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)の JSON 表記を指定します。

**StartDateTime** および **DueDateTime** の設定については、[詳細を参照してください](#NoteAboutStartDueDateTimes)。

応答内のすべての日付関連プロパティに特定のタイム ゾーンを指定する[方法についてご確認ください](#NoteAboutPreferHeader)。

**応答**

成功状態コード:201 Created

応答本文:作成された[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)。


**要求のサンプル**

最初の例では、指定したタスク フォルダーにタスクを作成し、要求本文の **StartDateTime** および **DueDateTime** を太平洋標準時 (PST) で表記します。

```
POST https://outlook.office.com/api/beta/me/taskfolders('AAMkADIyAAAhrbPXAAA=')/tasks
Content-Type: application/json

{
  "Subject": "Shop for dinner",
  "StartDateTime": {
      "DateTime": "2016-04-23T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "DueDateTime":  {
      "DateTime": "2016-04-25T13:00:00",
      "TimeZone": "Pacific Standard Time"
  }
}
```

**応答のサンプル**

POST メソッドは、要求本文の時間部分を無視し、時間が常に、指定されたタイム ゾーン (PST) の午前 0 時であると想定します。既定では、POST メソッドはすべての日付関連プロパティを変換し、応答ではそれらを UTC で表示します。

```
Status code: 201 Created

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/TaskFolders('AAMkADIyAAAhrbPXAAA%3D')/Tasks/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Tasks('AAMkADIyAAAhrb_PAAA=')",
  "@odata.etag": "W/\"hmM7Eb/jgEec8l3+gkJEawAAIbAOlw==\"",
  "Id": "AAMkADIyAAAhrb_PAAA=",
  "CreatedDateTime": "2016-04-22T05:44:01.2012012Z",
  "LastModifiedDateTime": "2016-04-22T05:44:02.9980882Z",
  "ChangeKey": "1/KC9Vmu40G3DwB6Lgs7MAAAIOJMxw==",
  "Categories": [ ],
  "AssignedTo": null,
  "Body": {
    "ContentType": "Text",
    "Content": ""
  },
  "CompletedDateTime": null,
  "DueDateTime": {
    "DateTime": "2016-04-25T07:00:00.0000000",
    "TimeZone": "UTC"
  },
  "Importance": "Normal",
  "IsReminderOn": false,
  "Owner": "Administrator",
  "ParentFolderId": "AQMkADA1MTkAAAAIBEgAAAA==",
  "Recurrence": null,
  "ReminderDateTime": null,
  "StartDateTime": {
    "DateTime": "2016-04-23T07:00:00.0000000",
    "TimeZone": "UTC"
  },
  "Status": "NotStarted",
  "Subject": "Shop for dinner"
}
```

**要求のサンプル**

`Prefer: outlook.timezone` ヘッダーの動作のしくみを示すために、次の例ではタスクを作成し、**StartDateTime** と **DueDateTime** を東部標準時 (EST) で表し、`Prefer` ヘッダーを太平洋標準時 (PST) で表します。

```
POST https://outlook.office.com/api/beta/me/tasks HTTP/1.1

Content-Type: application/json
Prefer: outlook.timezone="Pacific Standard Time"

{
  "Subject": "Shop for children's weekend",
  "StartDateTime": {
      "DateTime": "2016-05-03T09:00:00",
      "TimeZone": "Eastern Standard Time"
  },
  "DueDateTime":  {
      "DateTime": "2016-05-05T16:00:00",
      "TimeZone": "Eastern Standard Time"
  }
}
```

**応答のサンプル**

先ほどの例と同じように、POST メソッドは、要求本文の **StartDateTime** および **DueDateTime** の時間部分を無視し、時間が常に、指定されたタイム ゾーン (EST) の午前 0 時であると想定します。

`Prefer` ヘッダーでは PST が指定されているため、POST メソッドは応答内のすべての日付関連プロパティを PST で表記します。 特に、**StartDateTime** および **DueDateTime** プロパティでは、POST メソッドは EST の午前 0 時を PST に変換し、応答ではそれらを PST で返します。

```
Status code: 201 Created

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Tasks/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Tasks('AAMkADA1MHgwAAA=')",
    "@odata.etag": "W/\"1/KC9Vmu40G3DwB6Lgs7MAAAIW9XXA==\"",
    "Id": "AAMkADA1MHgwAAA=",
    "CreatedDateTime": "2016-04-22T15:19:18.9526004-07:00",
    "LastModifiedDateTime": "2016-04-22T15:19:19.015101-07:00",
    "ChangeKey": "1/KC9Vmu40G3DwB6Lgs7MAAAIW9XXA==",
    "Categories": [
    ],
    "AssignedTo": null,
    "Body": {
        "ContentType": "Text",
        "Content": ""
    },
    "CompletedDateTime": null,
    "DueDateTime": {
        "DateTime": "2016-05-04T21:00:00.0000000",
        "TimeZone": "Pacific Standard Time"
    },
    "Importance": "Normal",
    "IsReminderOn": false,
    "Owner": "Administrator",
    "ParentFolderId": "AQMkADA1MTEgAAAA==",
    "Recurrence": null,
    "ReminderDateTime": null,
    "StartDateTime": {
        "DateTime": "2016-05-02T21:00:00.0000000",
        "TimeZone": "Pacific Standard Time"
    },
    "Status": "NotStarted",
    "Subject": "Shop for children's weekend"
}
```

****

<a name="GetTasks"> </a>
## タスクの取得 

[すべてのタスクの取得](#GetAllTasks) | [タスクの取得](#GetATask) 


<a name="GetAllTasks"> </a>
### すべてのタスクの取得 

_**必要なスコープ**: https://outlook.office.com/tasks.read_

複数のタスクを取得します。

サインインしているユーザーのメールボックス内のすべてのタスクを取得できます。
```no-highlight
GET https://outlook.office.com/api/beta/me/tasks
```

<a name="GetTasksInFolder"></a> または、特定のフォルダー内のすべてのタスクを取得できます。
```no-highlight
GET https://outlook.office.com/api/beta/me/taskfolders('{folder_id}')/tasks
```

1 つ以上のタスク グループがあり、特定のタスク グループ内のすべてのタスクを取得する場合は、最初に[タスク グループ内のすべてのタスク フォルダー](#GetTaskFolders)を取得してから、これらのタスク フォルダー内のタスクを取得します。 


**応答**

成功状態コード:200 OK

応答本文:[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource) コレクション

既定では、応答の日付関連プロパティは UTC で表されます。応答内のすべての日付関連プロパティに特定のタイム ゾーンを指定する[方法についてご確認ください](#NoteAboutPreferHeader)。


**要求のサンプル**

次の例では、ユーザーのメールボックス内のすべてのタスクを取得します。

```
GET https://outlook.office.com/api/beta/me/tasks
```

**応答のサンプル**

```
Status code: 200 OK

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Tasks",
  "value": [
    {
      "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Tasks('AAMkADA1MTrfAAA=')",
      "@odata.etag": "W/\"1/KC9Vmu40G3DwB6Lgs7MAAAIOJMxw==\"",
      "Id": "AAMkADA1MTrfAAA=",
      "CreatedDateTime": "2016-04-22T05:44:01.2012012Z",
      "LastModifiedDateTime": "2016-04-22T05:44:02.9980882Z",
      "ChangeKey": "1/KC9Vmu40G3DwB6Lgs7MAAAIOJMxw==",
      "Categories": [ ],
      "AssignedTo": null,
      "Body": {
        "ContentType": "Text",
        "Content": ""
      },
      "CompletedDateTime": null,
      "DueDateTime": {
        "DateTime": "2016-04-25T07:00:00.0000000",
        "TimeZone": "UTC"
      },
      "Importance": "Normal",
      "IsReminderOn": false,
      "Owner": "Administrator",
      "ParentFolderId": "AQMkADA1MTBEgAAAA==",
      "Recurrence": null,
      "ReminderDateTime": null,
      "StartDateTime": {
        "DateTime": "2016-04-23T07:00:00.0000000",
        "TimeZone": "UTC"
      },
      "Status": "NotStarted",
      "Subject": "Shop for dinner"
    },
    {
      "@odata.id": "https://outlook.office.com/api/beta/Users('86b6ceaf-57f7-4278-97c4-4da0a97f6cdb@70559e59-b378-49ea-8e53-07a3a3d27f5b')/Tasks('AAMkADA1MTrgAAA=')",
      "@odata.etag": "W/\"1/KC9Vmu40G3DwB6Lgs7MAAAIOJMyQ==\"",
      "Id": "AAMkADA1MTrgAAA=",
      "CreatedDateTime": "2016-04-22T06:03:35.9279794Z",
      "LastModifiedDateTime": "2016-04-22T06:03:35.9436052Z",
      "ChangeKey": "1/KC9Vmu40G3DwB6Lgs7MAAAIOJMyQ==",
      "Categories": [ ],
      "AssignedTo": null,
      "Body": {
        "ContentType": "Text",
        "Content": ""
      },
      "CompletedDateTime": null,
      "DueDateTime": {
        "DateTime": "2016-04-27T04:00:00.0000000",
        "TimeZone": "UTC"
      },
      "Importance": "Normal",
      "IsReminderOn": false,
      "Owner": "Administrator",
      "ParentFolderId": "AQMkADA1MTBEgAAAA==",
      "Recurrence": null,
      "ReminderDateTime": null,
      "StartDateTime": {
        "DateTime": "2016-04-26T04:00:00.0000000",
        "TimeZone": "UTC"
      },
      "Status": "NotStarted",
      "Subject": "Shop for dinner"
    }
  ]
}
```

<a name="GetATask"> </a>
### タスクの取得 

_**必要なスコープ**: https://outlook.office.com/tasks.read_

特定のタスクを取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/tasks('{task_id}')
```

**応答**

成功状態コード:200 OK

応答本文:要求された[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)。

既定では、応答の日付関連プロパティは UTC で表されます。応答内のすべての日付関連プロパティに特定のタイム ゾーンを指定する[方法についてご確認ください](#NoteAboutPreferHeader)。

**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/tasks('AAMkADA1MTrgAAA=') 
```

**応答のサンプル**

```
Status code: 200 OK

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Tasks/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Tasks('AAMkADA1MTrgAAA=')",
  "@odata.etag": "W/\"hmM7Eb/jgEec8l3+gkJEawAAIa/+kw==\"",
  "Id": "AAMkADA1MTrgAAA=",
  "CreatedDateTime": "2016-04-22T06:03:35.9279794Z",
  "LastModifiedDateTime": "2016-04-22T06:03:35.9436052Z",
  "ChangeKey": "1/KC9Vmu40G3DwB6Lgs7MAAAIOJMyQ==",
  "Categories": [ ],
  "AssignedTo": null,
  "Body": {
    "ContentType": "Text",
    "Content": ""
  },
  "CompletedDateTime": null,
  "DueDateTime": {
    "DateTime": "2016-04-27T04:00:00.0000000",
    "TimeZone": "UTC"
  },
  "Importance": "Normal",
  "IsReminderOn": false,
  "Owner": "Administrator",
  "ParentFolderId": "AQMkADA1MTBEgAAAA==",
  "Recurrence": null,
  "ReminderDateTime": null,
  "StartDateTime": {
    "DateTime": "2016-04-26T04:00:00.0000000",
    "TimeZone": "UTC"
  },
  "Status": "NotStarted",
  "Subject": "Shop for dinner"
}
```

****


<a name="UpdateTasks"> </a>
## タスクの更新 

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

タスクの書き込み可能なプロパティを変更します。

```no-highlight
PATCH https://outlook.office.com/api/beta/me/tasks/{task_id}
```

要求本文で、[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)で更新する書き込み可能なプロパティの JSON 表記を指定します。

**StartDateTime** および **DueDateTime** の設定については、[詳細を参照してください](#NoteAboutStartDueDateTimes)。

**CompletedDateTime** プロパティは、[Complete](#CompleteTasks) アクションによって、または明示的に PATCH 操作によって設定することができます。 PATCH を使用して **CompletedDateTime** を設定する場合は、**Status** も `Completed` にしてください。

既定では、応答の日付関連プロパティは UTC で表されます。応答内のすべての日付関連プロパティにカスタムのタイム ゾーンを指定する[方法についてご確認ください](#NoteAboutPreferHeader)。

**応答**

成功状態コード:200 OK

応答本文:更新された[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)。


**要求のサンプル**

次の例では、**DueDateTime** を変更し、`Prefer: outlook.timezone` ヘッダーを使用して、東部標準時 (EST) で表記する日付関連プロパティを指定します。
 
```
PATCH https://outlook.office.com/api/beta/me/tasks('AAMkADA1MTHgwAAA=')

Prefer: outlook.timezone="Eastern Standard Time"
Content-Type: application/json

{
  "DueDateTime":  {
      "DateTime": "2016-05-06T16:00:00",
      "TimeZone": "Eastern Standard Time"
  }
}
```

**応答のサンプル**

```
Status code: 200 OK

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Tasks/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Tasks('AAMkADA1MTHgwAAA=')",
  "@odata.etag": "W/\"hmM7Eb/jgEec8l3+gkJEawAAIa/+lg==\"",
  "Id": "AAMkADA1MTHgwAAA=",
    "CreatedDateTime": "2016-04-22T18:19:18.9526004-04:00",
    "LastModifiedDateTime": "2016-04-22T18:38:20.5541528-04:00",
    "ChangeKey": "1/KC9Vmu40G3DwB6Lgs7MAAAIW9XXg==",
    "Categories": [
    ],
    "AssignedTo": null,
    "Body": {
        "ContentType": "Text",
        "Content": ""
    },
    "CompletedDateTime": null,
    "DueDateTime": {
        "DateTime": "2016-05-06T00:00:00.0000000",
        "TimeZone": "Eastern Standard Time"
    },
    "Importance": "Normal",
    "IsReminderOn": false,
    "Owner": "Administrator",
    "ParentFolderId": "AQMkADA1MTIBEgAAAA==",
    "Recurrence": null,
    "ReminderDateTime": null,
    "StartDateTime": {
        "DateTime": "2016-05-03T00:00:00.0000000",
        "TimeZone": "Eastern Standard Time"
    },
    "Status": "NotStarted",
    "Subject": "Shop for children's weekend"
}
```


****

<a name="DeleteTasks"> </a>
## タスクの削除 

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

ユーザーのメールボックス内の指定されたタスクを削除します。

```no-highlight
DELETE https://outlook.office.com/api/beta/me/tasks('{task_id}')
```

**応答**

成功状態コード:204 No Content

応答本文:なし


**要求のサンプル**

```
DELETE https://outlook.office365.com/api/beta/me/tasks('AAMkADIyAAAhrb_QAAA=')
```

**応答のサンプル**

```
Status code: 204 No Content
```


****

<a name="CompleteTasks"> </a>
## タスクの完了 

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

タスクを完了し、**CompletedDateTime** プロパティを現在の日付に設定し、**Status** プロパティを `Completed` に設定します。

```no-highlight
POST https://outlook.office.com/api/beta/me/tasks('{task_id}')/complete
```

**注** 

**CompletedDateTime** は、タスクが完了する日付を表します。 **CompletedDateTime** の時間部分は、既定で UTC の午前 0 時に設定されます。 

アプリを使用して、`Prefer` 要求ヘッダーでカスタム タイム ゾーンを指定できます。 次の例では、**CompletedDateTime** を太平洋標準時 (PST) タイム ゾーンに設定します。

```
Prefer: outlook.timezone="Pacific Standard Time"
```

この要求ヘッダーでは、応答内のすべての日付関連プロパティを、指定されたタイム ゾーンに設定します。

**応答**

成功状態コード:200 OK

応答本文:タスク コレクション内の完了した[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)。 定期的なアイテムでタスクを完了する場合、タスク コレクションには系列にある完了したタスクと、系列にある次のタスクが含まれます。


**要求のサンプル**

次の例では、指定したタスクを終了に設定します。 `Prefer: outlook.timezone` ヘッダーで太平洋標準時 (PST) を指定しているため、応答内の **CompletedDateTime** およびその他の日付関連プロパティは、UTC で表されます。

```
POST https://outlook.office.com/api/beta/me/tasks('AAMkADA1MT15rfAAA=')/complete

Prefer: outlook.timezone="Pacific Standard Time"
```

**応答のサンプル**

```
Status code: 200 OK

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Tasks",
  "value": [
    {
      "@odata.id": "https://outlook.office365.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/Tasks('AAMkADA1MT15rfAAA=')",
      "@odata.etag": "W/\"hmM7Eb/jgEec8l3+gkJEawAAIa/+lw==\"",
      "Id": "AAMkADA1MT15rfAAA=",
      "CreatedDateTime": "2016-04-21T22:44:01.2012012-07:00",
      "LastModifiedDateTime": "2016-04-22T19:28:38.5300447-07:00",
      "ChangeKey": "1/KC9Vmu40G3DwB6Lgs7MAAAIW9XYQ==",
      "Categories": [
      ],
      "AssignedTo": null,
      "Body": {
          "ContentType": "Text",
          "Content": ""
      },
      "CompletedDateTime": {
          "DateTime": "2016-04-22T00:00:00.0000000",
          "TimeZone": "Pacific Standard Time"
      },
      "DueDateTime": {
          "DateTime": "2016-04-25T00:00:00.0000000",
          "TimeZone": "Pacific Standard Time"
      },
      "Importance": "Normal",
      "IsReminderOn": false,
      "Owner": "Administrator",
      "ParentFolderId": "AQMkADA1MTIBEgAAAA==",
      "Recurrence": null,
      "ReminderDateTime": null,
      "StartDateTime": {
          "DateTime": "2016-04-21T00:00:00.0000000",
          "TimeZone": "Pacific Standard Time"
      },
      "Status": "Completed",
      "Subject": "Shop for dinner"
    }
  ]
}
```

****


<a name="SyncTasks"></a>
## タスクまたはタスク フォルダーの同期

_**必要なスコープ**: http://outlook.office.com/tasks.readwrite_

ユーザーのメールボックス内のタスク フォルダーのタスクを同期することができます。 タスクの同期はフォルダー単位の操作であり、たとえば、既定のタスク フォルダー `Tasks` 内のすべてのタスクを同期できます。 フォルダー階層内のタスクを同期するには、各タスク フォルダーを個別に同期する必要があります。 タスクまたはタスク フォルダーを同期するプロセスも同様であり、通常はそれぞれが GET 呼び出しである 2 つ以上の同期要求のラウンドが必要です。 

[フォルダーのタスクを取得する](#GetTasksInFolder)、または[メールボックスのタスク フォルダーを取得する](#GetTaskFolders)場合とほぼ同じ方法で GET メソッドを使用します。ただし、特定の要求ヘッダーおよび必要に応じて _deltaToken_ または _skipToken_ を含める必要があります。

**要求ヘッダー** 

- 以前の同期要求から返される `skipToken` を含む同期要求を除き、すべての同期要求で `Prefer: odata.track-changes` ヘッダーを指定する必要があります。 最初の応答で _Preference-Applied: odata.track-changes_ ヘッダーを探して、リソースが同期をサポートすることを確認してから、先に進みます  (タスクを同期する場合、`skipToken` に関する詳細については、[タスクの 2 番目の応答データのサンプル](#SyncTasksSampleSecondResponse)を参照し、タスク フォルダーを同期する場合は、[タスク フォルダーの 2 番目の応答データのサンプル](#SyncTaskFoldersSampleSecondResponse)を参照してください)。
- `Prefer: odata.maxpagesize={x}` ヘッダーを指定して、各同期要求を返すタスク (同期する対象によってはタスク フォルダー) の最大数を指定することができます。

同期の一般的なラウンドは次のとおりです。

1. 必須の _Prefer: odata.track-changes_ ヘッダーを指定して最初の GET 要求を行います。 同期要求に対する最初の応答では、常に _deltaToken_ が返されます  (2 番目以降の GET 要求は、前の応答で受信した _deltaToken_ または _skipToken_ のいずれかを含むため、最初の GET 要求とは異なります)。

2. 最初の応答で _Preference-Applied: odata.track-changes_ ヘッダーが返された場合は、リソースの同期に進むことができます。

  - 2 番目 GET 要求を行います。 最初の GET 要求から返された _Prefer: odata.track-changes_ ヘッダーと _deltaToken_ を指定して、同期するリソースのインスタンスに追加があるかどうかを調べます。 2 番目の要求では、追加のインスタンスと、さらにインスタンスがある場合は _skipToken_ が、最後のインスタンスが同期された場合は _deltaToken_ が (この場合は停止できます)、返されます。

  - 前の呼び出しから返された _skipToken_ を指定して GET 呼び出しを送信することで、同期を続けます。 _@odata.deltaLink_ ヘッダーと再び _deltaToken_ (同期が完了したことを示します) が含まれる最後の応答を受け取ると、停止します。


同期のラウンドにおける最初とそれ以降の呼び出しの構文を見てみましょう。


**タスク フォルダーでタスクを同期するには: **

最初の要求：

```no-highlight
GET https://outlook.office.com/api/beta/me/TaskFolders('{folder_id}')/Tasks
```


2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/beta/me/TaskFolders('{folder_id}')/Tasks/?$deltatoken={delta_token}
```


同じラウンドの 3 番目以降の要求: `@odata.deltaLink` ヘッダーと `deltaToken` を再び含む応答を受け取ったら停止します。

```no-highlight
GET https://outlook.office.com/api/beta/me/TaskFolders('{folder_id}')/Tasks/?$skiptoken={skip_token}
```


**メールボックスのタスク フォルダーを同期するには**

最初の要求：

```no-highlight
GET https://outlook.office.com/api/beta/me/TaskFolders
```


2 番目の要求、またはそれ以降のラウンドの最初の要求：

```no-highlight
GET https://outlook.office.com/api/beta/me/TaskFolders/?$deltatoken={delta_token}
```


同じラウンドの 3 番目以降の要求: `@odata.deltaLink` ヘッダーと `deltaToken` を再び含む応答を受け取ったら停止します。

```no-highlight
GET https://outlook.office.com/api/beta/me/TaskFolders/?$skiptoken={skip_token}
```




**パラメーター**

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_ヘッダー パラメーター_|
|優先|odata.track-changes|要求が同期要求であることを示します。 ラウンドの最初の 2 つの GET 要求に対して必須です。|
|優先|odata.maxpagesize|各応答で返されるメッセージの数を設定します。 省略可能。|
|_URL パラメーター_|
|deltaToken|string|以前の同期の応答で、@odata.deltaLink の値の一部として返される `deltaToken` 文字列。|
|skipToken|string|以前の同期の応答で、@odata.nextLink の値の一部として返される `skipToken` 文字列。 |


**注** 

- 最初の要求で `Prefer: odata.track-changes` を指定する場合、応答が同期をサポートしていれば、応答のヘッダーには `Preference-applied: odata.track-changes` が含まれます。
- サポートされていないリソースを同期しようするか、またはこれが最初の同期要求でない場合は、応答のヘッダーに `Preference-applied` は表示されません。
- 応答時間を向上させるため、$select クエリ パラメーターを使用して、自分のシナリオに役立つプロパティのみを取得します。  
- $filter、$orderby、$search、および $top クエリ パラメーターを使用することはできません。  


**応答本文**

- タスクを同期する場合: コレクション内の要求された [Task](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource) オブジェクト。

- タスク フォルダーを同期する場合: コレクション内の要求された [TaskFolder](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource) オブジェクト。

オブジェクトの数は、`Prefer: odata.maxpagesize` 要求ヘッダーで設定された値によって決まります。


**例**

2 つの例を次に示します。
- [タスク フォルダーでタスクを同期する](#SyncTasksSampleInitialRequest)
- [タスク フォルダーを同期します](#SyncTaskFoldersSampleInitialRequest)。 

それぞれの例は、最初と 2 番目の同期要求を示しています。 
- 各要求は `Prefer: odata.maxpagesize=1` を指定して、1 つのオブジェクトだけを返します (タスクまたはタスク フォルダーでそれぞれ 1つ)。
- 最初の応答は、1 つの同期イベント、`deltaLink` および `deltaToken` を返します。 
- 2 番目の要求では、その `deltatoken` を使用します。 2 番目の応答は、1 つの同期イベント、`nextLink` および `skipToken` を返します。 

同期プロセスを反復処理し、次の GET 呼び出しで前の同期要求から返された `skipToken` を使用し、以下のように `deltaLink` および `deltaToken` を含む、同期応答を受け取るまで続けます。
```
"@odata.deltaLink": “https://outlook.office.com/api/beta/me/TaskFolders('AQMkAGMw80AAAIBEgAAAA==')/Tasks/?%24deltaToken=294a8f04cc0345c5ae093d484629e186”
```

このような場合、このラウンドの同期は完了しています。 次のラウンドの同期のために `deltaToken` を保存します。 


<a name="SyncTasksSampleInitialRequest"></a>

**最初の要求のサンプル (タスクの同期)**

```
    GET https://outlook.office.com/api/beta/me/TaskFolders('AQMkAGMwAAAIBEgAAAA==')/Tasks HTTP/1.1
    Prefer: odata.maxpagesize=1
  Prefer: odata.track-changes
```

**最初の応答データのサンプル (タスクの同期)**

```
HTTP/1.1 200 OK
Preference-Applied: odata.track-changes

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#me/TaskFolders('AQMkAGMwAAAIBEgAAAA%3D%3D')/Tasks",
  "value": [
    {
      "@odata.id": "https://outlook.office.com/api/beta/Users('47ec4680-f443-4f9c-a3e5-f7660f0aceae@b4ffe6c0-e717-4104-acd1-e9dfe38ff5f9')/Tasks('AAMkAGMwQsKVevNAAAG1VNmAAA=')",
      "@odata.etag": "W/\"3JfzyLwJe0mPNcULClXrzQAABtYBDw==\"",
      "Id": "AAMkAGMwQsKVevNAAAG1VNmAAA=",
      "CreatedDateTime": "2016-02-29T20:51:25.2226052Z",
      "LastModifiedDateTime": "2016-02-29T20:51:25.2538576Z",
      "ChangeKey": "3JfzyLwJe0mPNcULClXrzQAABtYBDw==",
      "Categories": [ ],
      "AssignedTo": null,
      "Body": {
        "ContentType": "Text",
        "Content": ""
      },
      "CompletedDateTime": null,
      "DueDateTime": null,
      "Importance": "Normal",
      "IsReminderOn": false,
      "Owner": "Administrator",
      "ParentFolderId": "AQMkAGMwAAAIBEgAAAA==",
      "Recurrence": null,
      "ReminderDateTime": null,
      "StartDateTime": null,
      "Status": "NotStarted",
      "Subject": "another task"
    }
  ],
  "@odata.deltaLink": "https://outlook.office.com/api/beta/me/TaskFolders('AQMkAGMwAAAIBEgAAAA==')/Tasks/?%24deltatoken=175e2e04482e431ea96e89145c212f8c"
}
```

**2 番目の要求のサンプル (タスクの同期)**

```
    GET https://outlook.office.com/api/beta/me/TaskFolders('AQMkAGMwAAAIBEgAAAA==')/Tasks/?%24deltatoken=175e2e04482e431ea96e89145c212f8c HTTP/1.1
    Prefer: odata.maxpagesize=1
  Prefer: odata.track-changes
```

<a name="SyncTasksSampleSecondResponse"></a>

**2 番目の応答データのサンプル (タスクの同期)**

```
HTTP/1.1 200 OK

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#me/TaskFolders('AQMkAGMwAAAIBEgAAAA%3D%3D')/Tasks/$delta",
  "value": [
    {
      "@odata.id": "https://outlook.office.com/api/beta/Users('47ec4680-f443-4f9c-a3e5-f7660f0aceae@b4ffe6c0-e717-4104-acd1-e9dfe38ff5f9')/Tasks('AAMkAGMwQsKVevNAAAG1VNlAAA=')",
      "@odata.etag": "W/\"3JfzyLwJe0mPNcULClXrzQAABtYBDQ==\"",
      "Id": "AAMkAGMwQsKVevNAAAG1VNlAAA=",
      "CreatedDateTime": "2016-02-29T20:51:02.5955351Z",
      "LastModifiedDateTime": "2016-02-29T20:51:03.9703679Z",
      "ChangeKey": "3JfzyLwJe0mPNcULClXrzQAABtYBDQ==",
      "Categories": [ ],
      "AssignedTo": null,
      "Body": {
        "ContentType": "Text",
        "Content": ""
      },
      "CompletedDateTime": null,
      "DueDateTimeTime": null,
      "Importance": "Normal",
      "IsReminderOn": false,
      "Owner": "Administrator",
      "ParentFolderId": "AQMkAGMwAAAIBEgAAAA==",
      "Recurrence": null,
      "ReminderDateTime": null,
      "StartDateTime": null,
      "Status": "NotStarted",
      "Subject": "another task"
    }
  ],
  "@odata.nextLink": "https://outlook.office.com/api/beta/me/TaskFolders('AQMkAGMw80AAAIBEgAAAA==')/Tasks/?%24skipToken=0fbce2031e844a2f9d13d8bee5ebe2c6"
}
```

タスクの同期を続行し、次の GET 呼び出しで前の応答の `@odata.nextLink` で返された `skiptoken` を使用し、最終的な応答に `@odata.deltaLink` および `deltaToken` が含まれるまで続けます。 次のラウンドの同期のために `deltaToken` を保存します。


<a name="SyncTaskFoldersSampleInitialRequest"></a>

**最初の要求のサンプル (タスク フォルダーの同期)**

```
    GET https://outlook.office.com/api/beta/me/TaskFolders HTTP/1.1
    Prefer: odata.maxpagesize=1
    Prefer: odata.track-changes
```

**最初の応答データのサンプル (タスク フォルダーの同期)**

```
HTTP/1.1 200 OK
Preference-Applied: odata.track-changes

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#me/TaskFolders",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('5bcd7334-a6c5-4f95-a370-319e077dfe10@e288a0d0-ab74-431b-9699-a3721aabb08f')/TaskFolders('AAMkAGJiAAAAAAESAAA=')",
            "Id": "AAMkAGJiAAAAAAESAAA=",
            "ChangeKey": "PG2a661l00Cy9qH3YxmDfwAAAAAAPA==",
            "Name": "Tasks",
            "IsDefaultFolder":true,
            "ParentGroupKey": "0006f0b7-0000-0000-c000-000000000046"
        }
    ],
    "@odata.deltaLink": "https://outlook.office.com/api/beta/me/TaskFolders/?%24deltatoken=OyZKBDxtmuutZdNAsvah92MZg38AAAAAZwkBAAAA"
}
```

**2 番目の要求のサンプル (タスク フォルダーの同期)**

```
    GET https://outlook.office.com/api/beta/me/TaskFolders/?%24deltatoken=OyZKBDxtmuutZdNAsvah92MZg38AAAAAZwkBAAAA HTTP/1.1
    Prefer: odata.maxpagesize=1
    Prefer: odata.track-changes
```

<a name="SyncTaskFoldersSampleSecondResponse"></a>

**2 番目の応答データのサンプル (タスク フォルダーの同期)**

```
HTTP/1.1 200 OK

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#me/TaskFolders/$delta",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('5bcd7334-a6c5-4f95-a370-319e077dfe10@e288a0d0-ab74-431b-9699-a3721aabb08f')/TaskFolders('AAMkAGI5AAAunDbWAAA=')",
            "Id": "AAMkAGI5AAAunDbWAAA=",
            "ChangeKey": "PmebZ1wYAUaTmrKkvU9LIQAALqEkaw==",
            "Name": "Bingo",
            "IsDefaultFolder":false,
            "ParentGroupKey": "db0823f2-93bd-4db6-8038-98bbc5f39a45"
        }
    ],
    "@odata.nextLink": "https://outlook.office.com/api/beta/me/TaskFolders/?%24skipToken=x_zCAz5nm2dcGAFGk5qypL1PSyEAAC6cRncCAAAA"
}

```

同期を続行し、次の GET 呼び出しで前の応答の `@odata.nextLink` で返された `skiptoken` を使用し、最終的な応答に `@odata.deltaLink` および `deltaToken` が含まれるまで続けます。 この例では、3 番目の要求で `deltaToken` が返され、同期はこのラウンドで完了します。


**3 番目の要求のサンプル (タスク フォルダーの同期)**

```
    GET https://outlook.office.com/api/beta/me/TaskFolders/?%24skipToken=x_zCAz5nm2dcGAFGk5qypL1PSyEAAC6cRncCAAAA HTTP/1.1
    Prefer: odata.maxpagesize=1
```

<a name="SyncTaskFoldersSampleSecondResponse"></a>

**3 番目の応答のサンプル (タスク フォルダーの同期)**

```
HTTP/1.1 200 OK

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#me/TaskFolders/$delta",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('5bcd7334-a6c5-4f95-a370-319e077dfe10@e288a0d0-ab74-431b-9699-a3721aabb08f')/TaskFolders('AAMkAGI5AAAunDbVAAA=')",
            "Id": "AAMkAGI5AAAunDbVAAA=",
            "ChangeKey": "PmebZ1wYAUaTmrKkvU9LIQAALqEkZA==",
            "Name":"Volunteer",
            "IsDefaultFolder":false,
            "ParentGroupKey": "db0823f2-93bd-4db6-8038-98bbc5f39a45"
        }
    ],
    "@odata.deltaLink":"https://outlook.office.com/api/beta/me/taskfolders/?%24deltaToken=x_zCBD5nm2dcGAFGk5qypL1PSyEAAC6cRncEAAAA"
}

```


****

<a name="CreateTaskFolders"> </a>
## タスク フォルダーの作成

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

タスク フォルダーを作成します。 

ユーザーのメールボックスの既定のタスク グループ (`My Tasks`) にタスクを作成することができます。

```no-highlight
POST https://outlook.office.com/api/beta/me/taskfolders
```

または、指定したタスク グループの下にタスク フォルダーを作成することができます。
```no-highlight
POST https://outlook.office.com/api/beta/me/taskgroups('{group_id}')/taskfolders
```

要求本文で、作成する [TaskFolder](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource) の JSON 表記を指定します。

**応答**

成功状態コード:201 Created

応答本文:作成された [TaskFolder](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource)。


**要求のサンプル**

次の例では、ユーザーのメールボックスの既定のタスク グループ (`My Tasks`) に `Volunteer` というタスク フォルダーを作成します。
```
POST https://outlook.office.com/api/beta/me/taskfolders 
Content-Type: application/json

{
  "Name": "Volunteer"
}
```

**応答のサンプル**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/TaskFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskFolders('AAMkADIyAAAhrbPWAAA=')",
  "Id": "AAMkADIyAAAhrbPWAAA=",
  "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAIbAGig==",
  "IsDefaultFolder": false,
  "Name": "Volunteer",
  "ParentGroupKey": "0006f0b7-0000-0000-c000-000000000046"
}
```

**要求のサンプル**

次の例では、指定したタスク グループに `Cooking` というタスク フォルダーを作成します。
```
POST https://outlook.office.com/api/beta/me/taskgroups('AAMkADIyAAAhrbe-AAA')/taskfolders 
Content-Type: application/json

{
  "Name": "Cooking"
}
```

**応答のサンプル**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/TaskGroups('AAMkADIyAAAhrbe-AAA%3D')/TaskFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskFolders('AAMkADIyAAAhrbPXAAA=')",
  "Id": "AAMkADIyAAAhrbPXAAA=",
  "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAIbAOlA==",
  "IsDefaultFolder": false,
  "Name": "Cooking",
  "ParentGroupKey": "63d640cf-946f-4734-9c29-60dda7b76acb"
}
```


****

<a name="GetTaskFolders"> </a>
## タスク フォルダーの取得 

_**必要なスコープ**: https://outlook.office.com/tasks.read_

複数のタスク フォルダーを取得します。 

ユーザーのメールボックス内のすべてのタスク フォルダーを取得できます。

```no-highlight
GET https://outlook.office.com/api/beta/me/taskfolders
```

または、特定のタスク グループ内のタスク フォルダーを取得できます。
```no-highlight
GET https://outlook.office365.com/api/beta/me/taskgroups('{group_id}')/taskfolders 
```



**応答**

成功状態コード:200 OK

応答本文:[taskfolder](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource) コレクション。


**要求のサンプル**

次の例では、ユーザーのメールボックス内のすべてのタスク フォルダーを取得します。
```
GET https://outlook.office.com/api/beta/me/taskfolders
```

**応答のサンプル**

```
Status code: 200 OK

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/TaskFolders",
  "value": [
    {
      "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskFolders('AAMkADIyAAAAABrJAAA=')",
      "Id": "AAMkADIyAAAAABrJAAA=",
      "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAAAAeAA==",
      "IsDefaultFolder": false,
      "Name": "Monthly tasks",
      "ParentGroupKey": "0006f0b7-0000-0000-c000-000000000046"
    },
    {
      "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskFolders('AAMkADIyAAAAAAESAAA=')",
      "Id": "AAMkADIyAAAAAAESAAA=",
      "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAAAAAPA==",
      "IsDefaultFolder": true,
      "Name": "Tasks",
      "ParentGroupKey": "0006f0b7-0000-0000-c000-000000000046"
    }
  ]
}
```

次の例では、指定したタスク グループ内のすべてのタスク フォルダーを取得します。
```
GET https://outlook.office365.com/api/beta/me/taskgroups('AAMkADIyAAAhrbe-AAA=')/taskfolders
```

**応答のサンプル**

```
Status code: 200 OK

{
  "@odata.context": "https://outlook.office365.com/api/beta/$metadata#Me/TaskGroups('AAMkADIyAAAhrbe-AAA%3D')/TaskFolders",
  "value": [
    {
      "@odata.id": "https://outlook.office365.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskFolders('AAMkADIyAAAhrbPXAAA=')",
      "Id": "AAMkADIyAAAhrbPXAAA=",
      "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAIbAOlA==",
      "IsDefaultFolder": false,
      "Name": "Cooking",
      "ParentGroupKey": "63d640cf-946f-4734-9c29-60dda7b76acb"
    }
  ]
}
```

****

<a name="UpdateTaskFolders"> </a>
## タスク フォルダーの更新 

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

タスク フォルダーの書き込み可能なプロパティを更新します。

既定のタスク フォルダー (`Tasks`) の **Name** プロパティ値は変更できません。
 
タスク フォルダー ID は、ユーザーのメールボックス内で一意です。

```no-highlight
PATCH https://outlook.office.com/api/beta/me/taskfolders('{folder_id}')
```

要求本文で、[TaskFolder](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource) で更新する書き込み可能なプロパティの JSON 表記を指定します。

**応答**

成功状態コード:200 OK

応答本文:更新された [TaskFolder](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource)。


**要求のサンプル**

次の例では、タスク フォルダーの名前を `Charity work` に変更します。
```
PATCH https://outlook.office.com/api/beta/me/taskfolders('AAMkADIyAAAhrbPWAAA=')
Content-Type: application/json

{
  "Name": "Charity work"
}
```

**応答のサンプル**

```
Status code: 200 OK

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/TaskFolders/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskFolders('AAMkADIyAAAhrbPWAAA=')",
  "Id": "AAMkADIyAAAhrbPWAAA=",
  "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAIbAKfQ==",
  "IsDefaultFolder": false,
  "Name": "Charity work",
  "ParentGroupKey": "0006f0b7-0000-0000-c000-000000000046"
}
```


****

<a name="DeleteTaskFolders"> </a>
## タスク フォルダーの削除

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

指定したタスク フォルダーを削除します。

既定のタスク フォルダー `Tasks` を削除しようとすると、”HTTP 400 正しくない要求” が返されます。  

```no-highlight
DELETE https://outlook.office.com/api/beta/me/taskfolders('{folder_id}')
```


**応答**

成功状態コード:204 No Content

応答本文:なし。


**要求のサンプル**

```
DELETE https://outlook.office365.com/api/beta/me/taskfolders('AAMkADIyAAAhrbPXAAA=')
```

**応答のサンプル**

```
Status code: 204
```


****

<a name="CreateTaskGroups"> </a>
## タスク グループの作成

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

ユーザーのメールボックスにタスク グループを作成します。

```no-highlight
POST https://outlook.office.com/api/beta/me/taskgroups
```

要求本文で、作成する [TaskGroup](..\api\complex-types-for-mail-contacts-calendar.md#TaskGroupResource) の JSON 表記を指定します。

**応答**

成功状態コード:201 Created

応答本文:作成された [TaskGroup](..\api\complex-types-for-mail-contacts-calendar.md#TaskGroupResource)。


**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/taskgroups
Content-Type: application/json

{
  "Name": "Leisure tasks"
}
```

**応答のサンプル**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/TaskGroups/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskGroups('AAMkADIyAAAhrbe-AAA=')",
  "Id": "AAMkADIyAAAhrbe-AAA=",
  "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAIbAGjg==".
  "IsDefaultGroup": false,
  "Name": "Leisure tasks",
  "GroupKey": "63d640cf-946f-4734-9c29-60dda7b76acb"
}
```


****

<a name="GetTaskGroups"> </a>
## タスク グループの取得

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

ユーザーのメールボックス内のすべてのタスク グループを取得します。

応答には常に、既定のタスク グループ `My Tasks` およびメールボックス内に作成されたその他のタスク グループが含まれます。

```no-highlight
GET https://outlook.office.com/api/beta/me/taskgroups
```


**応答**

成功状態コード:200 OK

応答本文:[TaskGroup](..\api\complex-types-for-mail-contacts-calendar.md#TaskGroupResource) コレクション。


**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/taskgroups
```

**応答のサンプル**

```
Status code: 200

{
  "@odata.context": "https://outlook.office365.com/api/beta/$metadata#Me/TaskGroups",
  "value": [
    {
      "@odata.id": "https://outlook.office365.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskGroups('AAMkADIyAAADJ5pYAAA=')",
      "Id": "AAMkADIyAAADJ5pYAAA=",
      "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAInHxLA==",
      "IsDefaultGroup": true,
      "Name": "My Tasks",
      "GroupKey": "0006f0b7-0000-0000-c000-000000000046"
    },
    {
      "@odata.id": "https://outlook.office365.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskGroups('AAMkADIyAAAhrbe-AAA=')",
      "Id": "AAMkADIyAAAhrbe-AAA=",
      "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAInHxKw==",
      "IsDefaultGroup": false,
      "Name": "Leisure Tasks",
      "GroupKey": "63d640cf-946f-4734-9c29-60dda7b76acb"
    }
  ]
}

```


****

<a name="UpdateTaskGroups"> </a>
## タスク グループの更新

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

タスク グループの書き込み可能なプロパティを更新します。

```no-highlight
PATCH https://outlook.office.com/api/beta/me/taskgroups('{group_id}')
```

要求本文で、**Name** プロパティなど、[TaskGroup](..\api\complex-types-for-mail-contacts-calendar.md#TaskGroupResource) で更新する書き込み可能なプロパティの JSON 表記を指定します。

**応答**

成功状態コード:200 OK

応答本文:更新された[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)。


**要求のサンプル**

次の例では、タスク グループの名前を "Personal Tasks" に変更します。 既定のタスク グループの名前である "マイ タスク" を変更することはできません。

```
PATCH https://outlook.office.com/api/beta/me/taskgroups('AAMkADIyAAAhrbe-AAA=')
Content-Type: application/json

{
  "Name": "Personal Tasks"
}
```

**応答のサンプル**

```
Status code: 200 

{
  "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/TaskGroups/$entity",
  "@odata.id": "https://outlook.office.com/api/beta/Users('dc2d952a-78ff-4609-b3ae-eb66271747bf@8638a6dc-2d66-40dc-aecb-b2436ec47fc0')/TaskGroups('AAMkADIyAAAhrbe-AAA=')",
  "Id": "AAMkADIyAAAhrbe-AAA=",
  "ChangeKey": "hmM7Eb/jgEec8l3+gkJEawAAIbAGjw==",
  "IsDefaultGroup": false,
  "Name": "Personal Tasks",
  "GroupKey": "63d640cf-946f-4734-9c29-60dda7b76acb"
}
```


****

<a name="DeleteTaskGroups"> </a>
## タスク グループの削除

_**必要なスコープ**: https://outlook.office.com/tasks.readwrite_

指定したタスク グループを削除します。 

既定のタスク グループ `My Tasks` を削除しようとすると、”HTTP 400 正しくない要求” が返されます。  

```no-highlight
DELETE https://outlook.office.com/api/beta/me/taskgroups('{group_id}')
```


**応答**

成功状態コード:204 No Content

応答本文:更新された[タスク](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource)。


**要求のサンプル**

```
DELETE https://outlook.office365.com/api/beta/me/taskgroups('AAMkADIyAAAhrbe-AAA=')
```

**応答のサンプル**

```
Status code: 204
```

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

- [Outlook REST API の使用](..\api\use-outlook-rest-api.md)

- [メール REST API リファレンス](..\api\mail-rest-operations.md)

- [予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)



[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]




