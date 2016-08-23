SharePoint アドイン モデル内のドキュメント ID プロバイダー
===================================================

概要
-------

新しい SharePoint アドイン モデルで SharePoint 内のドキュメントの一意識別子を設定するために使用するアプローチは、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、ドキュメントの一意識別子を、SharePoint サーバー側オブジェクト モデル コードを実行しているリスト アイテム イベント ハンドラーによって設定し、SharePoint ソリューションを使用して展開していました。

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側オブジェクト モデル (CSOM) や SharePoint REST API を使用して、ドキュメントの一意識別子を設定します。

大まかなガイドライン
---------------------

新しい SharePoint アドイン モデルにおけるドキュメントの一意識別子の設定については、経験に基づく次のような大まかなガイドラインが提供されています。

- ドキュメントの一意識別子を設定するには、SharePoint クライアント側オブジェクト モデル (CSOM) や SharePoint REST API を使用します。
- 現在、標準のドキュメント ID プロバイダーを置換するためのリモート処理コードを関連付けるために使用できる標準のサポート済みメカニズムはないため、この機能をリモート API によって変更することはネイティブでサポートされてはいません。
- ただし、アドイン モデルにおける他の多くのケースと同様、代替ルートがないか調査中です。

ドキュメントで一意識別子を設定する方法
--------------------------------------------

***基本的には、ドキュメントの一意識別子を設定するとは、SharePoint リスト / ドキュメント ライブラリで列の値を設定することを意味します。***  

SharePoint (CSOM) や REST API は列の値を設定するために使用できるため、ドキュメントの一意識別子を設定するのに使用することができます。これらの API とそれを使用して列の値を設定する方法の詳細については、以下の記事を参照してください。  

- [SharePoint 2013 のクライアント ライブラリ コードを使用して基本的な操作を完了する (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/fp179912.aspx#BasicOps_SPListItemTasks) 
- [REST を使用したリスト アイテムの操作 (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/dn292552.aspx#ListItems)

ドキュメントの一意識別子を設定するためのオプション
-----------------------------------------------
SharePoint に保存されているドキュメントの一意識別子を設定するためのオプションは複数あります。

- リモート イベント レシーバーを使用する
- バックグラウンド プロセスを使用する

リモート イベント レシーバーを使用する
--------------------------
このパターンでは、SharePoint ライブラリに新しいドキュメントがアップロードされたときにリモート イベント レシーバーが起動します。リモート イベント レシーバーにより CSOM または REST API 呼び出しが行われ、ドキュメントの一意識別子が設定されます。

- このパターンは、SharePoint にドキュメントがアップロードされた直後に実行されます。
    + リモート イベント レシーバーに関連付けられているサービス内のコードが適切なタイミングで実行される限り、ドキュメント ID は SharePoint に新しいドキュメントがアップロードされた直後に設定されます。
- このパターンは、SharePoint にアップロードされた新しいドキュメントに対してのみ動作し、SharePoint にすでに保存されているドキュメントの一意識別子が設定されることはありません。
- 一括アップロード操作が行われると、リモート イベント レシーバーに関連付けられているサービスの呼び出しが複数回トリガーされます。そのため、一括アップロード操作によってサービスがオーバーロードされないように計画してください。
- 一意のドキュメント ID の設定が失敗した場合、リモート イベント レシーバーがそのことを SharePoint に通知する方法はありません。

**適切な場合**

ドキュメントが SharePoint にアップロードされた直後にその一意識別子を設定しなければならず、一括アップロード操作が予期されていない場合。

**作業の開始**

「[イベント レシーバーとリスト イベント レシーバー(SharePoint アドイン レシピ)](event-receiver-and-list-event-receiver-sharepoint-add-in.md)」には、アドイン モデルでイベント レシーバーを実装する方法の説明と、複数のサンプルや記事へのリンクが含まれています。

バックグラウンド プロセスを使用する
------------------------
このパターンでは、バックグラウンド プロセスにより SharePoint 内のドキュメントがチェックされ、一意識別子が設定されているかどうかが判別されます。あるドキュメントの一意識別子が見つからない場合、バックグラウンド プロセスによってそのドキュメントの一意識別子が設定されます。バックグラウンド プロセスによってドキュメントの一意識別子が設定される際は、CSOM または REST API 呼び出しが行われます。

- このパターンは、定義されたスケジュールに従って実行されます。
- このパターンは、コードのクロール対象であるすべてのドキュメントに対して動作します。
- SharePoint 検索サービスを使用して、一意識別子が設定されていないドキュメントのリストを返すフィルターを含むクエリを実行することをお勧めします。
    + このパターンは最も速く実行され、他のパターンより拡張性が優れています。
    + このパターンを使用すると、バックグラウンド サービス内のカスタム クエリ ロジックが不要になります。
    + このパターンでは、いくらかの検索設定が必要です。

        検索構成の詳細については、「[検索構成 (SharePoint アドイン レシピ)](search-configuration-sharepoint-add-in.md)」を参照してください。
- Web およびリスト オブジェクトをループ処理して再帰的にクエリし、SharePoint 環境内のすべてのドキュメントに関するメタデータを戻すことはお勧めしません。
    + このパターンは最も遅く実行され、他のクエリ パターンより拡張性が劣ります。  
    + このパターンを使用すると、API スロットル制限に達する可能性があります。
    + このパターンでは、バックグラウンド サービスにカスタム クエリ ロジックが含まれます。
    + このパターンは Azure Web ジョブを使用して実装することができます。

**適切な場合**

- ドキュメントの一意識別子を設定する必要があり、リモート イベント レシーバーが使用可能なオプションではない場合。
- ドキュメントの一括アップロードが予期される場合。
- ドキュメントで一意のドキュメント ID が確実に設定されるようにする必要がある場合。
    + 一意のドキュメント ID の設定が失敗した場合、イベント レシーバーがそのことを SharePoint に通知する方法はありません。
- SharePoint にすでに保存されているドキュメントを処理する必要がある場合。

**はじめに**

「[リモート タイマー ジョブ (SharePoint アドイン レシピ)](remote-timer-jobs-sharepoint-add-in.md)」には、アドイン モデルでリモート タイマー ジョブを実装する方法の説明と、複数のサンプルや記事へのリンクが含まれています。

関連リンク
=============
- [イベント レシーバーとリスト イベント レシーバー (SharePoint アドイン レシピ)](event-receiver-and-list-event-receiver-sharepoint-add-in.md)
- [検索構成 (SharePoint アドイン レシピ)](search-configuration-sharepoint-add-in.md)
- [リモート タイマー ジョブ (SharePoint アドイン レシピ)](remote-timer-jobs-sharepoint-add-in.md)
- [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事") にあるガイダンス記事
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [OD4B.NavLinksInjection (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/OD4B.NavLinksInjection)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス