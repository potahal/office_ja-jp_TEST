---
ms.Toctitle: Release notes
title: "2015 年 4 月の Office 365 REST API のリリース ノート"
description: "2015 年 4 月の Office 365 API のリリースで利用可能になった開発者向けの新機能およびプレビュー機能を紹介します。また、既知の問題についてもご確認ください。"
ms.ContentId: 14c3c05f-313f-4bd0-802b-749adc074b8c
ms.date: June 2, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# 2015 年 4 月の Office 365 REST API のリリース ノート

<p class="previewnote">[適用対象:](.\platform-development-preview-features-overview.md)Office 365 Education</p>

_**適用対象:**Office 365_

この記事では、2015 年 4 月の Office 365 API のリリースで使用可能になった開発者向けの新機能、および知っておくべき既知の問題について説明します。 Office 365 の開発の詳細については、「[Office 365 API プラットフォームの概要](.\platform-development-overview.md)」を参照してください。

## Office 365 API の新しい GA 機能

次の Office 365 API の新機能が一般公開されています。

* 次のクロスオリジン リソース共有 (CORS) サポート
    * Exchange での CORS のサポート
    * SharePoint での CORS のサポート
    * Microsoft Azure Active Directory (Azure AD) での CORS のサポート
* Exchange での AppOnly のサポート
* SharePoint での AppOnly のサポート
* SharePoint での承認スコープのサポート (分類、検索、UPA)
* Outlook API の更新内容は、次のとおりです。
    * [予定表のビューの同期](..\howto\sync-calendar-view.md)
    * 指定の基準を満たすメッセージの[検索](..\api\complex-types-for-mail-contacts-calendar.md#Search)
    * 複合型の[フィルター](..\api\complex-types-for-mail-contacts-calendar.md#Filter)
    * 予定表 API での開始時刻と終了時刻に対する Windows **TimeZone** のサポート [Event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) エンティティの **StartTimeZone** および **EndTimeZone** プロパティに関する説明を参照してください。
    * Outlook Web App でメッセージまたはイベントを開く [Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) および **Event** エンティティの **WebLink** プロパテに関する説明を参照してください。
    
## Office 365 API の新しいプレビュー機能

次の Office 365 API の新しいプレビュー機能が利用可能になりました。

* Notes API (プレビュー)
* OneDrive API (プレビュー)
* Office 365 統合 API (プレビュー)
* Office Graph (プレビュー)
* Video REST API (プレビュー)
* Outlook API:
    * メール、連絡先、イベントの通知を取得するための Outlook Notifications REST API (プレビュー) 
    * 認証済みユーザーの HD の写真を取得するための Outlook User Photo REST API (プレビュー)
    * メールの拡張のプロパティ


新しいプレビュー機能の詳細については、「[Office 365 プラットフォームの開発者向けプレビュー機能](.\platform-development-preview-features-overview.md)」を参照してください。

## Office 365 統合 API (プレビュー) に関する既知の問題

Office 365 統合 API (プレビュー) に関する既知の問題は次のとおりです。

### 統合グループおよびソーシャル アクティビティ情報の可用性

プレビュー リリースでは、統合 API を介して統合グループのイベントおよび会話に全世界でアクセスできるようになるまで、多少時間がかかる可能性があります。 

### ユーザー エンティティの制限事項

*作成後にすぐにアクセスできない*

ユーザーは、ユーザー エンティティのポストを通じてすぐに作成できます。Office 365 サービスにアクセスするには、まず Office 365 ライセンスをユーザーに割り当てる必要があります。それでも、分散されているというサービスの性質上、統合 API を介してこのユーザーがファイル、メッセージ、およびイベント エンティティを使用できるようになるまで約 15 分かかる場合があります。この間、アプリには HTTP 404 エラー応答が表示されます。 

*拡張ユーザー プロファイル プロパティの設定*

現在、AboutMe や Skills などの拡張ユーザー プロファイル プロパティの設定はサポートされていません。 

### グループ エンティティの制限事項

*作成後にコンテンツにすぐにアクセスできない*

統合グループは、グループ エンティティのポストを通じてすぐに作成できます。ただし、統合 API を介して作成された統合グループでは、関連付けられたコンテンツにすぐにアクセスすることはできません。アプリでは、コンテンツ (ファイル、会話、イベント) をグループに追加できるようになるまで、次の一定時間を要します。 

* 会話およびイベントについては、グループの作成後に最大 40 分 
* ファイルについては、グループの作成後に最大 24 時間 

この時間が経過する前に、統合グループをコンテンツで更新しようとすると、500 HTTP エラー応答が表示されます。Office 365 統合 API を使用している概念実証アプリケーションでは、コンテンツにすぐにアクセスする必要がある場合、Outlook または Outlook Web App を使用して統合グループを作成することをお勧めします。 

*ポリシー*

Office 365 統合 APIを使用して統合グループを作成および名前付けすると、Outlook Web App を介して構成されたすべての統合グループ ポリシーがバイパスされます。Office 365 統合 API を使用している概念実証アプリケーションでは、Outlook または Outlook Web App を使用して統合グループを作成することをお勧めします。 

*アクセス許可のスコープ*

Office 365 統合 API では、統合グループに対して次の 2 つのアクセス許可スコープが公開されています。 
* Group.Read.All  
* Group.ReadWrite.All 

これらのスコープは、グループ管理機能 (グループの列挙、グループ メンバーの列挙) へのアクセスのほか、グループのコンテンツ (会話とイベント) へのアクセスを提供します。 ただし、統合グループのファイルにアクセスするには、Sites.Read.All または Site.ReadWrite.All アクセス許可スコープも要求する必要があります。  これらのアクセス許可スコープの詳細については、「[Office 365 統合 API (プレビュー) の概要](https://msdn.microsoft.com/office/office365/HowTo/get-started-with-office-365-unified-api#msg_register_app)」を参照してください。  

### 連絡先

現在、個人用連絡先はサポートされていません。組織の連絡先のみがサポートされています。

### OData の操作

ワークロード間でのフィルター/検索は利用できません。全文検索 ($search を使用) は、ユーザー、グループ、組織の連絡先、メッセージ、およびイベントに対してのみ利用できます。

### パフォーマンス SLA

要求待ち時間の 95 パーセンタイル値が高い可能性があります。

### スキーマの不一致

出席者および受信者の正規の ID は利用できません。

### 特定のフォルダーへのファイルのアップロード

ファイルを特定のフォルダーにアップロードするには、アプリケーションで次の 3 つの手順を実行する必要があります。 

1. ルート フォルダーに新しいファイルを作成します。 

2. ファイル コンテンツをアップロードします。
 
3. ファイルを目的のフォルダーに移動します。 

手順 2 と 3 は入れ替え可能です。また、フォルダーを別のフォルダー下に作成するには、新しいフォルダーをルート フォルダー下に作成してから、該当する目的のフォルダーに移動する必要があります。これらの操作を一度の呼び出しだけで実行できるように、これらの問題を解決する予定です。 

### ファイルおよびコンテンツのストリーミング

ファイル (統合グループやドライブ内のファイル、またはメール添付ファイル) のアップロードとダウンロードは、1 MB 以下に制限されています。 

### アプリ登録の互換性

プレビュー期間中、Office 365 統合 API を呼び出すための新しいアプリを登録する必要があります。Web アプリケーションについては、シングルテナントおよびマルチテナント アプリがサポートされています。ネイティブ クライアント アプリは、アプリケーションが登録されているテナント内でのみ使用できます。 

## Outlook API に関する既知の問題

### イベントのリマインダーを設定できない

現時点では、イベントの作成または更新中に、クライアント ライブラリから、または REST 経由で直接、リマインダーを設定することはできません。イベント リマインダー フィールドは更新されませんが、エラーは返されません。

## その他のリソース

- [Office 365 API プラットフォームの概要](https://msdn.microsoft.com/en-us/office/office365/howto/platform-development-overview)
- [Office 365 プラットフォームの開発者向けプレビュー機能](.\platform-development-preview-features-overview.md)
- [OneDrive API (プレビュー) リリースノート](http://aka.ms/odb-api-release-notes)
