---
ms.Toctitle: Authenticate using v2.0
title: "詳細については、「v2.0 アプリ モード プレビューを使用して Office 365 および Outlook.com API を認証する」を参照してください。"
description: "ms.TocTitle:v2.0 を使用した認証 Title:v2.0 アプリ モデルを使用した Office 365 API と Outlook.com API の認証 Description:v2.0 認証エンドポイント プレビューを使用して、Azure AD と Microsoft アカウントの両方のユーザーをアプリと Office 365 API のエンドポイントにサインインします。ms.ContentId:30286232-50d7-4155-8112-121ce412a705ms.topic: 記事 (方法) ms.date:2015 年 12 月 1 日"
ms.ContentId: 30286232-50d7-4155-8112-121ce412a705
ms.date: December 1, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# v2.0 認証エンドポイント プレビューを使用した Office 365 API と Outlook.com API の認証

_**Applies to:** Office 365 | Outlook.com_



|**プレビュー ドキュメント** | 
|:-----|   
| パブリック プレビュー期間中には、まだサポートされていない v2.0 認証エンドポイントの機能があります。パブリック プレビュー中にアプリケーションを作成する場合は、注意する必要があります。詳細については、「[V2.0 アプリ モデル プレビューの制限事項と制約事項](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-limitations/)」を参照してください。 |


##Microsoft アカウントと Azure AD ユーザーの単一の認証モデルを使用したサインイン

v2.0 認証エンドポイント プレビューを使用すると、職場および学校 (Azure AD) の ID と、個人 (Microsoft アカウント) の ID の両方を受け入れるアプリを作成できます。 

以前は、Microsoft アカウントと Azure Active Directory の両方をサポートするアプリの開発者は、完全に独立した 2 つのシステムとの統合が必要でした。v2.0 認証エンドポイントを使用したアプリを作成し、両方の種類のアカウントでユーザーがサインインできるようにすることが可能になりました。1 つの単純なプロセスで、個人のアカウントと、仕事/学校のアカウントを持つ数百万に及ぶ両方のユーザーをすぐに獲得できるようになります。   

現時点では、v2.0 認証エンドポイント プレビューをアプリで使用してアクセスできる API は次のとおりです。

- Outlook のメール 
- Outlook の連絡先 
- Outlook の予定表

近い将来、他の Microsoft サービスが追加されます。


<a name="bk_samples"> </a>

##コード サンプル

次に示すコード サンプルを調べて、Office API にアクセスするために v2.0 認証エンドポイント プレビューを使用するアプリの作成方法について理解してください。

- [.NET MVC Web アプリ](https://dev.outlook.com/RestGettingStarted/Tutorial/dotnet)

<!--
- [Android](https://github.com/OfficeDev)
- [iOS](https://github.com/OfficeDev)
- [JavaScript](https://github.com/OfficeDev)
-->


<a name="bk_scopes"> </a>

##Office API 認証のスコープ

次の表に、v2 認証エンドポイント プレビューで使用する認証のスコープを示します。v2.0 エンドポイントでのスコープの使用に関する詳細、および Azure AD でのリソースの使用との違いについては、「[リソースではなく、スコープ](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-compare/#scopes-not-resources)」を参照してください。

<a name="bk_Outlookscopes"> </a>

###Outlook のメール、予定表、および連絡先のスコープ

|**スコープ** | **アクセス許可** | **説明** | 
|:-----|:-----|:-----|
| `https://outlook.office.com/mail.read` |ユーザーのメールの読み取り|アプリは、ユーザーのメールボックス内のメッセージを読み取ることができます。| 
| `https://outlook.office.com/mail.readwrite` |ユーザーのメールの読み取りと書き込みアクセス|アプリは、ユーザーのメールボックス内のメッセージの読み取り、更新、作成、削除を行えます。|
| `https://outlook.office.com/mail.send`  |ユーザーとしてメールを送信|アプリは、組織内のユーザーとしてメッセージを送信できます。|
| `https://outlook.office.com/contacts.read` |ユーザーの連絡先の読み取り|アプリは、ユーザーの連絡先を読み取ることができます。|
| `https://outlook.office.com/contacts.readwrite` |ユーザーの連絡先へのフル アクセス|アプリは、ユーザーの連絡先の読み取り、更新、作成、削除を行えます。|
| `https://outlook.office.com/calendars.read` |ユーザーの予定表の読み取り|アプリは、ユーザーの予定表内のイベントを読み取ることができます。|
| `https://outlook.office.com/calendars.readwrite` |ユーザーの予定表へのフル アクセス|アプリは、ユーザーの予定表内のイベントを読み取り、更新、作成、および削除できます。|



## Outlook の API

- これらの API は、すべての Office 365 ユーザーに対して機能します。outlook.com ユーザーに対しては、これらの API を段階的に使用できるようにしています。REST API に未対応の outlook.com メールボックスを対象とした要求をアプリで行うと、HTTP 状態 404 で特別なエラー "UserNotSupported" が返されます。この状況は、アプリで適切に処理する必要があります。

- 最適なパフォーマンスを得るために、すべての要求に x-AnchorMailbox ヘッダーを追加して、そのヘッダーに対象メールボックスのメール アドレスを設定することをお勧めします。

- 現時点では、エンドポイントに対して直接 REST API 要求を行う必要があります。Office 365 SDK は、これまでどおりに Azure AD エンドポイントを使用します。

- Outlook の API に v2.0 アプリ モデル プレビューを使用する場合は、次のコード サンプルを参照してください。
    - [.NET MVC Web アプリ](https://dev.outlook.com/RestGettingStarted/Tutorial/dotnet)



##次の手順

[v2.0 認証エンドポイントを使用するようにアプリを登録する](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-app-registration/)

##詳細を見る

[v2.0 エンドポイント プレビューの制限事項と制約事項](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-limitations/)

[v2.0 エンドポイント プレビューの新機能](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-compare)

More [v2.0 authentication endpoint preview documentation](https://azure.microsoft.com/en-us/documentation/articles/?service=active-directory&term=app+model+v2.0) on [azure.microsoft.com](https://azure.microsoft.com/)

