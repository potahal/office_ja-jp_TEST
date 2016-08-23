# サンドボックス ソリューションの変換ガイダンス - イベント レシーバー 
この記事では、サンドボックス ソリューションに存在するイベント レシーバーを置き換える際のオプションと戦略を特定する方法について説明します。

_
            **適用対象:**SharePoint 用アドイン | SharePoint 2013 | SharePoint 2016 | SharePoint Online_

コードベースのサンドボックス ソリューションは、2014 年以降[非推奨になっています](https://blogs.msdn.microsoft.com/sharepointdev/2014/01/14/deprecation-of-custom-code-in-sandboxed-solutions/)。また、SharePoint Online では、この機能を完全に削除するためのプロセスが始まっています。 コードベースのサンドボックス ソリューションは、SharePoint 2013 および SharePoint 2016 でも非推奨になっています。

## 概要

SharePoint アドイン モデルでは、SharePoint でイベントを処理する方法が、完全信頼コードを使用していた方法やコード化されたサンドボックス ソリューションでの方法と若干異なります。 一般に、これまでのソリューションでは、イベント レシーバーは SharePoint サーバー側オブジェクト モデルを使用して作成され、ソリューション パッケージによって展開されていました。このイベント レシーバーは、SharePoint サーバー上でコードを実行します。 その一方で、SharePoint アドイン モデルでは、イベント レシーバーの実装がイベント レシーバーをホストしている Web サーバー上で実行されます。このようなイベント レシーバーは、リモート イベント レシーバー (RER) と呼ばれます。 多くの場合、イベント レシーバーはリモート イベント レシーバーの実装に置き換えることができます。 この記事では、さまざまなオプションと設計に関する考慮事項について説明します。


## イベント レシーバーを置き換える場合のオプション
<a name="sectionSection2"> </a>

|**方法**|**追加情報**|
|:-----|:-----|
|リモート イベント レシーバー|</p><lu><li>[SharePoint でリモート イベント レシーバーを使用する](https://msdn.microsoft.com/en-us/pnp_articles/use-remote-event-receivers-in-sharepoint)</li><li>[SharePoint アドインにリモート イベント レシーバーを使用する方法](https://channel9.msdn.com/blogs/OfficeDevPnP/How-to-use-remote-event-receivers-for-your-SharePoint-add-ins)</li><li>[SharePoint アドイン モデル内のイベント レシーバーとリスト イベント レシーバー](https://msdn.microsoft.com/en-us/pnp_articles/event-receiver-and-list-event-receiver-sharepoint-add-in)</li></lu><li>[自動タグ付けサンプル アドイン (SharePoint)](https://msdn.microsoft.com/en-us/pnp_articles/autotagging-sample-app-for-sharepoint)</li><li>[SharePoint アドインのイベントの処理](https://msdn.microsoft.com/en-us/library/office/jj220048.aspx)</li></lu></p>|
|WebHooks|<p>SharePoint 対応の WebHooks は現在開発段階にあり、プレビュー版が近日中に公開されます。<lu><li>[SharePoint WebHooks の紹介](http://dev.office.com/blogs/introducing-sharepoint-webhooks)</li></p>
|変更を監視するためのリモート タイマー ジョブ|<p>**ChangeQuery** オブジェクトを使用して、変更についてサイトまたはリストを監視します。 このパターンは、リモート イベント レシーバーの代替手段になります<lu><li>[SharePoint リスト アイテム変更モニター](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ListItemChangeMonitor)</li><li>[リモート タイマー ジョブのパターン](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SimpleTimerJob)</p>|

## 設計に関する考慮事項
### リモート イベント レシーバー
- ホスト用のインフラストラクチャが必要になります
- ホスト用のインフラストラクチャは高可用性であることが必要です
- リモート イベント レシーバーをホストするサービス エンドポイントは、匿名認証に対応するように構成されている必要があります
- SharePoint Online を使用している場合は、信頼されたサード パーティ証明書が必要になります
- 長時間実行の操作には適していません 
- アドインのコンテキストの外側でアタッチされたリモート イベント レシーバー (コンソール アプリケーションまたは PowerShell を使用してアタッチされたもの) は、起動されたときに SharePoint コンテキスト トークンを受け取らなくなり、アプリ専用のアクセス許可ではなく SharePointOnlineCredentials を使用する必要があります
- 再試行のメカニズムがありません 

### WebHooks
- ホスト用のインフラストラクチャが必要になります
- ホスト用のインフラストラクチャは高可用性であることが必要です
- 同期イベントをサポートしていません
- イベントが発生した後で変更を処理します
- SharePoint Online については、2016 年の夏期にパブリック プレビューが公開されます
- 現時点では、オンプレミスの SharePoint のビルドでは使用できません。

### リモート タイマー ジョブ
- ホスト用のインフラストラクチャが必要になります
- イベントが発生した後で変更を処理します
- 変更の処理にポーリング メカニズムを使用します

## サイトからサンドボックス コードを削除する

            <a name="sectionSection3"></a> 既存のサンドボックス ソリューションをサイトから非アクティブ化すると、宣言的オプションを使用して展開されたアセットやファイルは削除されませんが、サンドボックス ソリューションの機能は自動的に非アクティブ化され、イベント レシーバーは削除されます。 

## その他の技術情報
<a name="bk_addresources"> </a>
-  [SharePoint Online でコード ベースのサンドボックス ソリューションを削除する](http://dev.office.com/blogs/removing-code-based-sandbox-solutions-in-sharepoint-online)
-  [サンドボックス ソリューションの変換ガイダンス](https://msdn.microsoft.com/en-us/pnp_articles/sandbox-solution-transformation-guidance)
-  [Visual Studio で作成したサンドボックス ソリューションからアセンブリ参照を削除する](https://support.microsoft.com/en-us/kb/3183084)
-  [SharePoint 用アドインのビルド](https://msdn.microsoft.com/library/office/fp179930.aspx)
-  [Office 365 開発および SharePoint のパターンとプラクティス (ソリューション ガイダンス)](https://msdn.microsoft.com/en-us/pnp_articles/office-365-development-patterns-and-practices-solution-guidance)
