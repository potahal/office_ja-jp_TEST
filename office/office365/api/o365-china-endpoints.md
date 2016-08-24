---
ms.Toctitle: API endpoints of Office 365 for China
title: "中国向け Office 365 の API エンドポイント"
description: "中国向け Office 365 のアプリケーションについては、Office 365 API のリソースとサービスの URL が異なります。 ここでは、21Vianet が運用する中国向け Office 365 API エンドポイントを示します。" 
ms.ContentId: F8110642-6BAB-4C68-9306-A0B01B820A81
ms.date: April 19, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# 21Vianet が運用している Office 365 の API エンドポイント

 _**適用対象:**21Vianet が運用する中国向け Office 365_


中国で 21Vianet が運営している Office 365 のアプリケーションについては、Office 365 API のリソースと関連サービスの URL が中国以外で Office 365 が提供しているものの URL と異なります。これらの相違点を以下に記載します。 

## Microsoft Graph API 

Office 365 統合 API を使用するアプリケーションは、[Microsoft Graph](https://graph.microsoft.io/en-us/) を使用するようにアップグレードできるようになりました。

###サービスのルート エンドポイント

|**21Vianet によって運用されている Microsoft Graph**|**Microsoft Graph**|
|:-----|:-----|
| `https://microsoftgraph.chinacloudapi.cn` | `https://graph.microsoft.com` | 

###Microsoft Graph Explorer

|**21Vianet によって運営されている Microsoft Graph**|**Microsoft Graph**|
|:-----|:-----|
| `https://graphexplorerchina.azurewebsites.net` | `https://graphexplorer2.azurewebsites.net` | 


[Microsoft Graph のエンドポイントとサービスの機能](https://graph.microsoft.io/en-us/docs/overview/deployments)について、詳細を確認してください。

## Azure Active Directory Graph API 

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|  
| `https://graph.chinacloudapi.cn` | `https://graph.windows.net` |

## 探索サービス API
|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| まだ利用不可 |  `https://api.office.com/discovery` |

Microsoft Graph (`https://graph.microsoft.com/`) を呼び出す場合、探索サービスは不要です。

## 予定表 API

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://partner.outlook.cn` | `https://graph.microsoft.com`<br /><br />`https://outlook.office.com` |

たとえば、21Vianet が運営している Office 365 の v2.0 を使用してユーザーの予定表を取得する場合は、`GET` 要求を `https://partner.outlook.cn/api/v2.0/me/calendars` に送信します。

## 連絡先 API

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://partner.outlook.cn` | `https://graph.microsoft.com`<br /><br />`https://outlook.office.com` |

たとえば、21Vianet が運用している Office 365 の v2.0 を使用してユーザーの連絡先を取得する場合は、`GET` 要求を `https://partner.outlook.cn/api/v2.0/me/contacts` に送信します。

## メール API

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://partner.outlook.cn` | `https://graph.microsoft.com`<br /><br />`https://outlook.office.com` |

たとえば、21Vianet が運用している Office 365 の v2.0 を使用してユーザーの電子メールを取得する場合は、`GET` 要求を `https://partner.outlook.cn/api/v2.0/me/messages` に送信します。

## ファイル API

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://{tenant}-my.sharepoint.cn/_api/v1.0/me`<br /><br />`https://{tenant}.sharepoint.cn/{site-path}/_api/v1.0` | `https://graph.microsoft.com`<br /><br />`https://{tenant}-my.sharepoint.com/_api/v1.0/me` (OneDrive for Business)<br /><br />`https://{tenant}.sharepoint.com/{site-path}/_api/v1.0` (SharePoint サイト) |

## 予定表、連絡先、およびメールの OData エンティティ

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://partner.outlook.cn/api/{version}/$metadata` | `https://graph.microsoft.com/{version}/$metadata`<br /><br />`https://outlook.office.com/api/{version}/$metadata` |  

正確なバージョンを `{version}` に指定します。
たとえば、21Vianet が運用している Office 365 の v2.0 の場合、予定表エンティティの `@odata.context` は、`https://partner.outlook.cn/api/v2.0/$metadata#Me/Calendars/$entity` になります。

## ファイルの OData エンティティ

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://{tenantId}-my.sharepoint.cn/_api/v1.0/$metadata` | `https://graph.microsoft.com/v1.0/$metadata`<br /><br />`https://{tenantId}-my.sharepoint.com/_api/v1.0/$metadata` |  

<br />
次に示す URL は、アカウントと認証に関連しています。

## Microsoft Azure

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
|`https://account.windowsazure.cn` | `https://account.windowsazure.com` |

21Vianet が運用している Office 365 の開発環境をセットアップするには、`https://account.windowszaure.cn` にアクセスします。 詳細については、「[Office 365 開発環境のセットアップ](..\howto\setup-development-environment.md)」を参照してください。

## Azure OpenID Connect と OAuth

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://login.chinacloudapi.cn` | `https://login.microsoftonline.com` |

21Vianet が運用している Office 365 を呼び出すアプリでは、ユーザーの認証に `https://login.chinacloudapi.cn/common/oauth2/authorize` を使用し、トークンの取得に `https://login.chinacloudapi.cn/common/oauth2/token` を使用します。

## Office 365 開発者向けサイト

次の URL の `{your-sub-domain}` は、開発者向けサイトの設定時に指定する名前です。

|**21Vianet が運用している Office 365**|**Office 365**|
|:-----|:-----|
| `https://{your-sub-domain}.partner.onmschina.cn` | `https://{your-sub-domain}.onmicrosoft.com` |