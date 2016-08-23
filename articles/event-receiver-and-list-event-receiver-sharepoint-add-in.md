SharePoint アドイン モデル内のイベント レシーバーとリスト イベント レシーバー
=======================================================================

概要
-------

新しい SharePoint アドイン モデルでは、SharePoint のイベントを処理するための方法が完全信頼コードを使用していたものと異なっています。  
一般的な完全信頼コード (FTC)/ファーム ソリューション シナリオでは、イベント レシーバーとリスト イベント レシーバーが SharePoint サーバー側オブジェクト モデル コードで作成され、ソリューション経由で展開されていました。  このシナリオでは、イベント レシーバー コードは SharePoint サーバー上で実行されます。

SharePoint アドイン モデル シナリオでは、イベント レシーバーは SharePoint の外部にある Web サービスの内部で作成され、SharePoint に登録されます。  
これらは*リモート イベント レシーバー (RER)* と呼ばれます。 このシナリオでは、Web サービスがホストされる Web サーバーでイベント レシーバー コードが実行されます。

大まかなガイドライン
---------------------

イベント レシーバーの作成については、経験に基づく次のような大まかなガイドラインが提供されています。

- イベント レシーバーを実装するサービス エンドポイントは、匿名ユーザーによるアクセスが可能になっている必要があります。
- アドイン Web に追加されたイベント レシーバーにより、アクセス トークンを返すことができます。
- ホスト Web に追加されたイベント レシーバーは、アプリ アクセス トークンを使用するアプリ コンテキストから適用され、ホスト Web 内の操作がエンド ユーザー駆動型 (ItemAdded など) である場合に、アクセス トークンを返します。
- AppInstalled イベントは 30 秒以内で完了する必要があります。そうでない場合、SharePoint では失敗したとみなされます。SharePoint ではそのイベントが 3 回実行され、4 回目の実行の後、アドインのインストールが失敗したとみなされる
- ItemAdded のような通常のイベントにも 30 秒のタイムアウトがあるが、再試行のメカニズムはない
- SharePointOnlineCredentials やその他の手段などにより、アプリ コンテキストなしでホスト Web に追加されたイベント レシーバーではアクセス トークンが返されず、アプリ専用アクセス トークンでホスト Web にアクセスする必要があります。
- ホスト Web へのイベント レシーバーの追加はサポートされています。
    + これは CSOM/REST API によってのみ可能であり、Visual Studio のウィザードで行うことはできません。
- SharePoint では、特定のイベント用に構成されたイベント レシーバー エンドポイントが必ず呼び出されます。ただし、イベント レシーバー エンドポイント内のコードは SharePoint サーバー上で実行されないため、必ず実行されるという保証はありません。
    
    次に例を示します。
    
    イベント レシーバーのエンド ポイント URL を使用できない場合、イベント レシーバー コードは実行されません。  さまざまな理由により、URL を使用できないことがあります。  最も一般的な理由には、エンド ポイント URL の登録時における構成の誤り、URL を解決するときの DNS の問題、エンド ポイントの Web サイト ホスティングがシャットダウンされたか操作不能な状態である、などがあります。

    また、適切に記述されていないイベント レシーバー コードにバグがある場合、バグが発生し、イベントを再実行する必要があることを SharePoint に通知する方法はありません。  SharePoint リストに接続されているイベント レシーバーで、この問題をある程度回避することができます。  この方法について詳しくは、以下を参照してください。  
- イベント レシーバーで大量のコードが実行される場合は、非同期パターンを使用する必要があります。
    + このパターンの詳細については、次の MSDN ブログ投稿を参照してください。[Office 365 の非同期アクションにおける Azure ストレージ キューと Web ジョブの使用 (MSDN ブログ投稿)](http://blogs.msdn.com/b/vesku/archive/2015/03/02/using-azure-storage-queues-and-webjobs-for-async-actions-in-office-365.aspx)
    + イベント レシーバーでのタイムアウトの詳細については、次の MSDN 記事を参照してください。(記事で「タイムアウト」を検索してください)。[SharePoint アドインのイベントの処理 (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/jj220048.aspx)
- イベント レシーバーで SharePoint リストを操作する場合は、高品質の処理を保証するため、イベント レシーバーと共に特定の変更追跡メカニズムを使用することをお勧めします。
    + このパターンとその実装方法の詳細については、次の O365 PnP コード サンプルを参照してください。[Core.ListItemChangeMonitor (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ListItemChangeMonitor)


イベント レシーバーのデバッグ
-------------------------
イベント レシーバーをデバッグするには、Azure と Visual Studio でいくつかの異なる点を構成する必要があります。次の記事には、デバッグに関する詳しい手順と詳細が記載されています。 
- [SharePoint アドインでのリモート イベント レシーバーのデバッグとトラブルシューティング (MSDN ブログ投稿)](https://msdn.microsoft.com/en-us/library/office/dn275975.aspx) 
- [Visual Studio によるリモート イベント レシーバーのデバッグ (MSDN ブログ投稿)](http://blogs.msdn.com/b/officeapps/archive/2013/01/03/debugging-remote-event-receivers-with-visual-studio.aspx)
- [Visual Studio 2012 による SharePoint 2013 リモート イベントのデバッグに関する更新情報 (MSDN ブログ投稿)](http://blogs.msdn.com/b/officeapps/archive/2013/03/21/update-to-debugging-sharepoint-2013-remote-events-using-visual-studio-2012.aspx)
- [SharePoint Online でのリモート イベント レシーバーの "インストーラー アプリ" の作成とデバッグ (Scott Hillier)](http://www.itunity.com/article/create-debug-remote-event-receiver-installer-apps-sharepoint-online-775)

その他の例
-------------
- [Core.EventReceivers (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EventReceivers)
    + このサンプルは、SharePoint アドインで AppInstalled イベントを使用してホスト Web で追加処理 (たとえば、ホスト Web 内のリストにイベント レシーバーを接続する) を実行する方法を示しています。
    + このパターンの詳細については、次の MSDN ブログ投稿を参照してください。[ホスト Web 内のリストへのリモート イベント レシーバーの接続 (MSDN ブログ投稿)](http://blogs.msdn.com/b/kaevans/archive/2014/02/26/attaching-remote-event-receivers-to-lists-in-the-host-web.aspx)
- [Core.AppEvents.HandlerDelegation (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppEvents.HandlerDelegation)
    + このサンプルは、AppInstalled および AppUninstalling イベント用のハンドラーを次のように実装する方法を示しています。
        1. ハンドラーでエラーが発生した場合に使用するロールバック ロジックを組み込みます。
        2. ハンドラーの処理が失敗するか完了に 30 秒以上かかる場合に SharePoint で 3 回まで再試行が行われるという動作に対応するための "完了済み" ロジックを組み込みます。
        3. ハンドラー委任戦略を使用して、ハンドラー Web サービスから SharePoint への呼び出しを最小限にします。
        4. CSOM クラスの ExceptionHandlingScope と ConditionalScope を使用します。
- [Core.AppEvents (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppEvents)
    + このサンプルは、AppInstalled および AppUninstalling イベント用のハンドラーを次のように実装する方法を示しています。
        1. ハンドラーでエラーが発生した場合に使用するロールバック ロジックを組み込みます。
        2. ハンドラーの処理が失敗するか完了に 30 秒以上かかる場合に SharePoint で 3 回まで再試行が行われるという動作に対応するための "完了済み" ロジックを組み込みます。

関連リンク
=============
- [SharePoint 2013 のリモート イベント レシーバーに関する FAQ (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn456315.aspx)
- [SharePoint アドインでリモート イベント レシーバーを作成する (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/jj220043.aspx)
- [SharePoint 2013 でアプリ イベント レシーバーを作成する (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/jj220052.aspx)
- [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事") にあるガイダンス記事
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Core.ListItemChangeMonitor (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ListItemChangeMonitor)
- [Core.EventReceivers (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EventReceivers)
- [Core.AppEvents.HandlerDelegation (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppEvents.HandlerDelegation)
- [Core.AppEvents (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppEvents)
- https://github.com/OfficeDev/PnP にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) 
- SharePoint 2013 オンプレミス
