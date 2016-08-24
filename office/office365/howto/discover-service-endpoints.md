---
ms.Toctitle: Discovery tasks
title: "検出サービス API を使用したエンドポイントの検出に関する共通タスク"
description: "ms.TocTitle: 検出タスクTitle: 検出サービス API を使用したエンドポイントの検出に関する共通タスクDescription: Office 365 検出サービスを使用して、Microsoft サービスおよびユーザー リソースのエンドポイントを動的に検索し、アクセスを取得するのに必要な情報を入手します。ms.ContentId: ef8f781f-31c3-4871-929d-ab6155b02917 ms.topic: 記事 (手順ガイド)"
ms.ContentId: ef8f781f-31c3-4871-929d-ab6155b02917
ms.date: June 30, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]


# O365API リポジトリ スタイルの追加
    
_**適用対象:** Office 365_

[!INCLUDE [Use Microsoft Graph](../includes/use-msgraph-note.txt)]

Office 365 API は、Microsoft サービス経由で、イベント、連絡先、メール、ファイルへのアクセスを提供します。アプリケーションのユーザーが Microsoft サービスおよびユーザー リソースにアクセスできるようになる前に、アプリケーションにはエンドポイントが必要です。検出サービスはこれらのエンドポイントを動的に検出し、アクセスを取得するのに必要な情報を入手します。

The Discovery Service exposes a RESTful API. Client SDKs are available for the [.NET platform](..\howto\getting-started-Office-365-APIs.md), [Android](http://msdn.microsoft.com/en-us/office/office365/howto/develop-apps-for-android), and [iOS](http://dev.office.com/iOS). 探索サービス API と対話するには、HTTP および OData 要求を送信します。探索サービスでは、**予定表**、**連絡先**、**メール**、**MyFiles** (OneDrive および OneDrive for Business のサービス エンドポイントの場合)、**ノート** (OneNote の場合)、および **RootSite** (SharePoint の場合) の探索をサポートしています。

注 検出サービスは、Office 365 オンライン環境向けのみに機能を提供しており、オンプレミスのデプロイには利用できません。

探索サービスの詳細については、「[探索サービス REST API の操作リファレンス](..\api\discovery-service-rest-operations.md)」を参照してください。Office 365 API を使用してアクセスするサービスのエンドポイントを探すための探索サービス API の使用方法に関するコード サンプルは、「[Office 365 API: 探索サービスの使用方法](https://code.msdn.microsoft.com/Office-365-APIs-How-to-use-609102ea#content)」および「[Office 365 探索サービスのサンプル](https://github.com/OfficeDev/Office365-Discovery-Service-Sample)」を参照してください。

探索サービスの API エンドポイント URI を次に示します。

探索サービスの API エンドポイント URI を次に示します。
```
ApiEndpoint = "https://api.office.com/discovery/v1.0/me/;

```

バージョン v1.0:
```
ApiEndpoint = "https://api.office.com/discovery/v2.0/me/;

```

バージョン v2.0:
```
ResourceId = "https://api.office.com/discovery/";

```

## The Resource ID for Discovery Service:

検出サービスを使用する前に、Office 365 開発環境をセットアップする必要があります。


## 検出サービスのプロセス

検出サービスのプロセス


**以下は、検出サービスを使用したアプリのワークフローです。**


|**表 1.Office 365 API を呼び出す際のアプリ、検出サービス、Azure AD 間の相互作用**|**手順**|**説明**|
|:-----|:-----|:-----|
|1|Azure の管理ポータルでアプリを登録し、クライアント ID およびリダイレクト URI を使用してアプリのコードを構成します。その後、Azure の管理ポータルでアプリのアクセス許可を構成します。| |
|2|アプリは、ユーザーのメール アドレスを取得します。アプリは、メール アドレスおよびアプリがアクセスするスコープのセットを使用して検出サービスに問い合わせます。|![アプリは、ユーザーのメール アドレスを取得します。アプリは、メール アドレスおよびアプリがアクセスするスコープのセットを使用して検出サービスに問い合わせます。](images\O365APIs_DiscoveryFlow_Step1.png)|
|3|アプリは Azure AD 認証エンドポイントにアクセスし、ユーザーは認証を行い、同意を付与します (以前に同意を付与していない場合)。Azure AD は認証コードを発行します。|![アプリは Azure AD 認証エンドポイントにアクセスし、ユーザーは認証を行い、同意を付与します (以前に同意を付与していない場合)。Azure AD は認証コードを発行します。](images\O365APIs_DiscoveryFlow_Step2.png)|
|4|アプリが認証コードを使用します。Azure がアクセス トークンと更新トークンを返します。 アプリは認証コードを使用します。Azure はアクセス トークンと更新トークンを返します。|![アプリは認証コードを使用します。Azure はアクセス トークンと更新トークンを返します。](images\O365APIs_DiscoveryFlow_Step3.png)|
|5|アプリは、アクセス トークンを使用して検出サービスに問い合わせます。検出サービスは、Office 365 サービスのリソース ID とエンドポイント URI と共に HTTP 応答を返します。|![アプリは、アクセス トークンを使用して検出サービスに問い合わせます。検出サービスは、Office 365 サービスのリソース ID とエンドポイント URI と共に HTTP 応答を返します。](images\O365APIs_DiscoveryFlow_Step4.png)|
|6|アプリは、Azure AD トークン エンドポイントで更新トークンを使用し、必要な Office 365 リソースのアクセス トークンを入手します。Azure AD トークン エンドポイントは、指定したリソースのアクセス トークンと、更新トークンを返します。|![アプリは、Azure AD トークン エンドポイントで更新トークンを使用し、必要な Office 365 リソースのアクセス トークンを入手します。Azure AD トークン エンドポイントは、指定したリソースのアクセス トークンと、更新トークンを返します。](images\O365APIs_DiscoveryFlow_Step5.png)|
|7|これで、アプリが探索サービスの URI とアクセス トークンを使用して、Office 365 を呼び出すことができます。 Office 365 returns Http Response.|![アプリは、検出サービスの URI とアクセス トークンを使用して、Office 365 API を呼び出せるようになります。Office 365 は HTTP 応答を返します。](images\O365APIs_DiscoveryFlow_Step6.png)|

検出サービスの使用方法を示す例については、「Office 365 API: 検出サービスの使用方法」を参照してください。

検出サービスの使用方法を示す例については、「[Office 365 API: 検出サービスの使用方法](..\api\discovery-service-rest-operations.md)」を参照してください。


## 予定表、連絡先
<a name="bk_addresources"> </a>


-  [Office 365 開発環境のセットアップ](..\howto\setup-development-environment.md)
    
-  [Office 365 ASP.NET MVC アプリの構築](..\howto\getting-started-Office-365-APIs.md)
    
-  [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)
    
-  [Office 365 API: 検出サービスの使用方法](http://code.msdn.microsoft.com/Office-365-APIs-How-to-use-609102ea)

-  [Office 365 API 入門 (トレーニングビデオ)](http://www.microsoftvirtualacademy.com/training-courses/introduction-to-office-365-development?m=10072&ct=31602) 

