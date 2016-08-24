
# Office 365 サービス通信 API リファレンス (プレビュー)

 _**適用対象:** Office 365_

 **注** このドキュメントで取り上げる機能は、現時点ではプレビュー段階にあります。プレビュー機能を使用する方法の詳細については、「[Office 365 プラットフォームの開発者向けプレビュー機能](https://msdn.microsoft.com/en-us/office/office365/howto/platform-development-preview-features-overview)」を参照してください。

Office 365 サービス通信 API V2 を使用して、次のデータにアクセスできます。

-  **Get Services**:サブスクライブしているサービスのリストを取得します。
    
-  **Get Current Status**:現在進行中のサービス インシデントやメンテナンス イベントのリアルタイム ビューを取得します。
    
-  **Get Historical Status**:サービス インシデントやメンテナンス イベントなどの、サービス正常性の履歴ビューを取得します。
    
-  **Get Messages**:インシデント、計画済みメンテナンス、およびメッセージ センター通信を検索します。
    
現時点では、Office 365 サービス通信 API には、以下のサービスについてのデータが含まれています。Dynamics CRM、Dynamics Marketing、Exchange Online、Exchange Online Protection、ID サービス、モバイル デバイス管理、Office 365 パートナー管理センター、OneDrive for Business、Parature、Power BI for Office 365、Rights Management Service、SharePoint Online、SHD Admin、Skype for Business、Social Engagement、および Yammer Enterprise。

### 基礎

API のルート URL には、操作の範囲を単一のテナントに限定するテナント ID が含まれています。


```
https://manage.office.com/api/v1.0/{tenant_identifier}/ServiceComms/{operation}
```

The  **Office 365 Service Communications API** is a REST service that allows you to develop solutions using any web language and hosting environment that supports HTTPS and X.509 certificates. The API relies on **Microsoft Azure Active Directory** and the **OAuth2** protocol for authentication and authorization. To access the API from your application, you'll need to first register it in Azure AD and configure it with permissions at the appropriate scope. This will enable your application to request OAuth2 access tokens necessary for calling the API. You can find more information about registering and configuring an application in Azure AD at [Office 365 Management APIs getting started](https://msdn.microsoft.com/EN-US/library/dn707385.aspx).

すべての API 要求は、**ServiceHealth.Read** 要求が含まれている Azure AD から取得した有効な OAuth2 JWT のアクセス トークンがある、承認 HTTP ヘッダーを必要とします。テナント ID は、ルート URL のテナント ID と一致していなければなりません。




```
Authorization: Bearer {OAuth2 token}
```

 **要求ヘッダー:**

以下に示すのは、Office 365 サービス通信 API のすべての操作に対してサポートされている要求ヘッダーです。



|**ヘッダー**|**説明**|
|:-----|:-----|
|**Accept (省略可能)**|受け入れられる応答の表現は以下のとおりです。application/json;odata.metadata=full application/json;odata.metadata=minimal [ヘッダーが指定されていない場合の既定値] application/json;odata.metadata=none|
|**Authorization (必須)**|要求の認証トークン (Bearer JWT AAD トークン)。|
 **応答ヘッダー:**

以下に示すのは、Office 365 サービス通信 API のすべての操作に対して返される応答ヘッダーです。



|**ヘッダー**|**説明**|
|:-----|:-----|
|**コンテンツ長**|応答本体の長さです。|
|**コンテンツ タイプ**|応答の表現: application/json application/json;odata.metadata=full application/json;odata.metadata=minimal application/json;odata.metadata=none odata.streaming=true|
|**Cache-Control**|要求/応答チェーンに沿うすべてのキャッシュ機構が従う必要があるディレクティブを指定するために使用されます。|
|**Pragma**|実装固有の動作。|
|**Expires**|クライアントがリソースを有効期限切れにする必要があるタイミング。|
|**X-Activity-Id**|サーバー生成のアクティビティ ID。|
|**OData-Version**|サポートされている OData のバージョン (4.0)。|
|**Date**|サーバーから応答が送信されたときの、UTC 形式の日付。|
|**X-Time-Taken**|応答の生成にかかった時間 (ミリ秒)。|
|**X-Instance-Name**|(デバッグ目的で) 応答の生成に使用される Azure インスタンスの ID。|
|**Server**|(デバッグ目的で) 応答を生成するために使用されるサーバー。|
|**X-ASPNET-Version**|(デバッグ目的で) 応答を生成したサーバーにより使用される ASP.Net のバージョン。|
|**X-Powered-By**|(デバッグ目的で) 応答を生成したサーバーで使用されるテクノロジ。|

### Office 365 サービス通信 API の操作




#### Get Services

サブスクライブしているサービスのリストを返します。


||**サービス**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/Services`||
|**クエリ オプション**|$select|プロパティのサブセットを選択します。|
|**応答**|"Service" エンティティのリスト|"Service" エンティティには、"Id" (文字列)、"DisplayName" (文字列)、および "FeatureNames" (文字列のリスト) が含まれています。|
 **要求のサンプル:**




```

GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Services 
Authorization: Bearer {AAD_Bearer_JWT_Token}

```

 **応答のサンプル:**




```

{
    "value": [
        {
            "Id": "Exchange",
            "DisplayName": "Exchange Online",
            "FeatureNames": [
                "Sign-in",
                "E-Mail and calendar access",
                "E-Mail timely delivery",
                "Management and Provisioning",
                "Voice mail"
            ]
        },
        {
            "Id": "Lync",
            "DisplayName": "Lync Online",
            "FeatureNames": [
                "Audio and Video",
                "Federation",
                "Management and Provisioning",
                "Sign-In",
                "All Features",
                "Dial-In Conferencing",
                "Online Meetings",
                "Instant Messaging",
                "Presence",
                "Mobility"
            ]
        }
    ]
}

```


### 現在の状態の取得

サービスの現在の状態を示します。

||**サービス**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/CurrentStatus`||
|**フィルター**|Workload|ワークロードでフィルター (文字列、既定値: all)。|
|**クエリ オプション**|$select|プロパティのサブセットを選択します。|
|**応答**|"WorkloadStatus" エンティティのリスト。|"WorkloadStatus" エンティティには、"Id" (文字列)、"Workload" (文字列)、"StatusTime"(DateTimeOffset)、"WorkloadDisplayName" (文字列)、"Status" (文字列)、"IncidentIds" (文字列のリスト)、および FeatureGroupStatusCollection ("FeatureStatus" のリスト) が含まれます。"FeatureStatus" エンティティには、"Feature" (文字列)、"FeatureGroupDisplayName" (文字列)、および "FeatureStatus" (文字列) が含まれます。|
 **要求のサンプル:**




```
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/CurrentStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

 **応答のサンプル:**




```

{
    "value": [
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        },
        {
            "Id": "Lync",
            "Workload": "Lync",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Lync Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "AudioVideo",
                    "FeatureGroupDisplayName": "Audio and Video",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Federation",
                    "FeatureGroupDisplayName": "Federation",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "ManagementandProvisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Sign-In",
                    "FeatureGroupDisplayName": "Sign-In",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "All",
                    "FeatureGroupDisplayName": "All Features",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "DialInConferencing",
                    "FeatureGroupDisplayName": "Dial-In Conferencing",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "OnlineMeetings",
                    "FeatureGroupDisplayName": "Online Meetings",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "InstantMessaging",
                    "FeatureGroupDisplayName": "Instant Messaging",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Presence",
                    "FeatureGroupDisplayName": "Presence",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Mobility",
                    "FeatureGroupDisplayName": "Mobility",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        }
    ]
}


```


### Get Historical Status

特定の時間範囲内のサービスの履歴状態を、日単位で返します。

||**サービス**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/HistoricalStatus`||
|**フィルター**|Workload|ワークロードでフィルター (文字列、既定値: all)。|
||StatusTime|StatusTime より後の日付でフィルター処理します (DateTimeOffset、既定値: ge CurrentTime - 7 日)。|
|**クエリ オプション**|$select|プロパティのサブセットを選択します。|
|**応答**|"WorkloadStatus" エンティティのリスト。|"WorkloadStatus" エンティティには、"Id" (文字列)、"Workload" (文字列)、"StatusTime"(DateTimeOffset)、"WorkloadDisplayName" (文字列)、"Status" (文字列)、"IncidentIds" (文字列のリスト)、および FeatureGroupStatusCollection ("FeatureStatus" のリスト) が含まれます。"FeatureStatus" エンティティには、"Feature" (文字列)、"FeatureGroupDisplayName" (文字列)、および "FeatureStatus" (文字列) が含まれます。|
 **要求のサンプル:**




```
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/HistoricalStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

 **応答のサンプル:**




```
{
    "value": [
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        },
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-10T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "InformationAvailable",
            "IncidentIds": [
                "EX20870"
            ],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "InformationAvailable"
                }
            ]
        }
    ]
}


```


### Get Messages

 **説明:**

特定の時間範囲内のサービスに関するメッセージを返します。"サービス インシデント"、"計画済みメンテナンス"、または "メッセージ センター" メッセージをフィルター処理するためのタイプ フィルターを使用します。

||**サービス**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/Messages`||
|**フィルター**|Workload|ワークロードでフィルター (文字列、既定値: all)。|
||StartTime|開始時刻でフィルター処理します (DateTimeOffset、既定値: ge CurrentTime - 7 日)。|
||EndTime|終了時刻でフィルター処理します (DateTimeOffset、既定値: le CurrentTime)。|
||MessageType|メッセージの種類でフィルター処理します (文字列、既定値: all)。|
||ID|ID でフィルター処理します (文字列、既定値: all)。|
|**クエリ オプション**|$select|プロパティのサブセットを選択します。|
||$top|結果の最上位数を選択します (既定値および最大値 $top = 100)。|
||$skip|結果の数をスキップします (既定値: $skip = 0)。|
|**応答**|"Message" エンティティのリスト。|"Message" エンティティには、"Id" (文字列)、"StartTime" (DateTimeOffset)、"EndTime" (DateTimeOffset)、"Status" (文字列)、"Messages" ("MessagHistory" エンティティのリスト)、"LastUpdatedTime" (DateTimeOffset)、"Workload" (文字列)、"WorkloadDisplayName" (文字列)、"Feature" (文字列)、"FeatureDisplayName" (文字列)、"MessageType" (Enum、既定: all) が含まれます。"MessageHistory" エンティティには、"PublishedTime" (DateTimeOffset)、"MessageText" (文字列) が含まれます。|
 **要求のサンプル:**




```
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Messages
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

 **応答のサンプル:**




```
{
 {
    "value": [
        {
            "Id": "EX20119",
            "Name": "EX20119",
            "Title": null,
            "StartTime": "2015-04-01T22:25:00Z",
            "EndTime": "2015-04-07T21:48:00Z",
            "Status": "Service restored",
            "Messages": [
                {
                    "PublishedTime": "2015-04-01T22:34:28.76Z",
                    "MessageText": "Current Status: Engineers are investigating an issue in which some customers may be experiencing problems accessing or using Exchange Online services or features. This event is actively being investigated. More information will be provided shortly."
                },
                {
                    "PublishedTime": "2015-04-03T18:45:32.56Z",
                    "MessageText": "Current Status: Engineers are implementing a fix within the affected infrastructure to remediate Exchange Online archive access issues. \n\nUser Experience: Affected users are unable to access online archives when using Outlook Web App (OWA). As a workaround, users may be able to access online archives via the Outlook client.\n\nCustomer Impact: Customer impact appears to be limited at this time. This issue only affects hybrid customers.\n\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues.  \n\nNext Update by: Monday, April 6, 2015, at 9:00 PM UTC\n"
                },
                {
                    "PublishedTime": "2015-04-06T21:17:34.007Z",
                    "MessageText": "Current Status: Deployment of the fix is almost complete. Engineers are monitoring service health to ensure the issue has been remediated. \n\nUser Experience: Affected users are unable to access online archives when using Outlook Web App (OWA). As a workaround, users may be able to access online archives via the Outlook client. \n\nCustomer Impact: Customer impact appears to be limited at this time. This issue only affects hybrid customers.\n\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues. \n\nNext Update by: Tuesday, April 7, 2015, at 10:00 PM UTC "
                },
                {
                    "PublishedTime": "2015-04-08T21:54:49.06Z",
                    "MessageText": "Final Status: Engineers have implemented a fix which remediated end-user impact. \r\n\r\nUser Experience: Affected users were unable to access online archives when using Outlook Web App (OWA).\r\n\r\nCustomer Impact: Customer impact appears to have been limited. This issue only affected hybrid customers.\r\n\r\nIncident Start Time: Monday, March 30, 2015, at 9:28 AM UTC\r\n\r\nIncident End Time: Tuesday, April 7, 2015, at 8:00 PM UTC\r\n\r\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues.  \r\n\r\nNext Steps: The following is a list of known action item(s) associated with this incident. As part of the Office 365 problem management process, additional engineering actions may be identified to improve the overall service.\r\n- Action: Review the monitoring infrastructure for improvements as this event was reported by customers before an alert prompted a high priority investigation. \r\n\r\nA post-incident report will be available on the Service Health Dashboard within five business days."
                }
            ],
            "LastUpdatedTime": "2015-04-08T21:54:49.55Z",
            "Workload": "Exchange Online",
            "WorkloadDisplayName": "Exchange",
            "Feature": "Access",
            "FeatureDisplayName": "E-Mail and calendar access"
        },
        {
            "Id": "EX20789",
            "Name": "EX20789",
            "Title": null,
            "StartTime": "2015-04-06T20:00:00Z",
            "EndTime": "2015-04-07T23:00:00Z",
            "Status": "Service restored",
            "Messages": [
                {
                    "PublishedTime": "2015-04-07T23:26:44.643Z",
                    "MessageText": "Final Status: Engineers have validated that the deployed fix has resolved the issue and that service is restored.\r\n\r\nUser Experience: Affected users were unable to send or receive email while using Exchange Web Services (EWS) on their mobile devices.\r\n\r\nCustomer Impact: Customer impact appeared to be limited during this event. This issue was only affecting customers that use third-party Mobile Device Management software from Good Technology. As a workaround to provide immediate relief from impact, engineers implemented a configuration change for customers who reported this issue to Microsoft Support.\r\n\r\nIncident Start Time: Wednesday, April 1, 2015, at 2:00 PM UTC\r\n\r\nIncident End Time: Tuesday, April 7, 2015, at 10:00 PM UTC\r\n\r\nPreliminary Root Cause: As part of our ongoing efforts to improve service resiliency, an update was deployed to the infrastructure that facilitates connections from multiple Exchange Online protocols to mailbox databases. The update, however, contained a code issue that caused connectivity issues in some scenarios. \r\n\r\nNext Steps: The following is a list of known action item(s) associated with this incident. As part of the Office 365 problem management process, additional engineering actions may be identified to improve the overall service.\r\n- Action: Review the infrastructure update process to prevent reoccurrences of this scenario.\r\n\r\nPlease consider this service notification the final update on the event."
               }
            ],
            "LastUpdatedTime": "2015-04-07T23:26:45.08Z",
            "Workload": "Exchange Online",
            "WorkloadDisplayName": "Exchange",
            "Feature": "Access",
            "FeatureDisplayName": "E-Mail and calendar access"
        }
    ]
}

```


### エラー

サービスでエラーが検出されると、標準の HTTP エラー コードの構文を使用して、エラー応答コードが呼び出し元に報告されます。[OData V4 仕様](http://docs.oasis-open.org/odata/odata/v4.0/odata-v4.0-part1-protocol.html)に従って、失敗した呼び出しの本体には、追加情報が単一の JSON オブジェクトとして含められます。完全な JSON エラー本体の例を以下に示します。


```

{ 
    "error":{ 
        "code":"AF5000.  An internal server error occurred.",
        "message": "Retry the request." 
    } 
}

```

