# SharePoint のメタデータ、サイト ナビゲーション、および発行サイトの機能

SharePoint 発行サイトをサポートするために、SharePoint Online メタデータ管理機能、サイト ナビゲーションおよび発行サイト機能を使用します。

_**適用対象:** SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

この記事では、アドイン モデルを使用して、SharePoint Online でメタデータを管理する方法、およびサイト ナビゲーションと発行サイト機能をカスタマイズする方法を説明します。また、SharePoint 発行サイトのブランド化をカスタマイズするのに役立つ一般的な Web プログラミング パターンとライブラリの使用方法も説明します。

## メタデータ、ナビゲーション、および発行の主要な用語と概念

|**用語または概念**|**説明とガイダンス**|
|:-----|:-----|
|コンテンツ検索 Web パーツ|フォーマットしやすい方法で動的検索コンテンツをページに表示する SharePoint Web パーツ。詳しくは、以下を参照してください。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><a href="https://technet.microsoft.com/en-us/library/jj679900(v=office.15).aspx" target="_blank">SharePoint Server 2013 で検索 Web パーツを構成する</a></p></li><li><p><a href="https://support.office.com/en-us/article/Configure-a-Content-Search-Web-Part-in-SharePoint-0dc16de1-dbe4-462b-babb-bf8338c36c9a?CorrelationId=5024de0f-f4f4-436a-9c4a-b578b74b4764&amp;ui=en-US&amp;rs=en-US&amp;ad=US" target="_blank">SharePoint でコンテンツ検索 Web パーツを設定する</a></p></li><li><p><a href="https://technet.microsoft.com/en-us/library/sharepoint-online-search-service-description.aspx" target="_blank">SharePoint Online の検索機能</a></p></li></ul>|
|ContentTypeId|再帰的になるように設計されているコンテンツ タイプの一意の識別子。詳細については、「 [ContentTypeId クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.contenttypeid.aspx)」(CSOM) を参照してください。|
|コンテンツ タイプ|メタデータ (列)、ワークフロー、動作、および情報のカテゴリに関するその他の設定の、再利用可能で一元的なコレクション。詳細については、次を参照してください。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><a href="https://msdn.microsoft.com/en-us/library/office/ms196085(v=office.14).aspx" target="_blank">列</a></p></li><li><p><a href="https://msdn.microsoft.com/en-us/library/office/ms460224(v=office.14).aspx" target="_blank">コンテンツ タイプの作成</a></p></li><li><p><a href="https://msdn.microsoft.com/en-us/library/office/ms468437(v=office.14).aspx" target="_blank">コンテンツ タイプのユーザー設定情報</a></p></li></ul>|
|コンテンツ タイプのハブ|Managed Metadata Service アプリケーションのパーツ。これは、コンテンツ タイプをその他のサイト コレクションに発行するハブ サイトになります。1 つの場所からコンテンツ タイプの発行、再発行、および発行の取り消しを行えます。詳細については、次を参照してください。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><a href="https://support.office.com/en-us/article/Publish-a-content-type-from-a-content-publishing-hub-58081155-118d-4e7a-9cc5-d43b5dbb7d02?CorrelationId=49ebfe9f-6cf8-4f04-86a3-4d3fe23324fe&amp;ui=en-US&amp;rs=en-US&amp;ad=US" target="_blank">コンテンツ発行ハブからコンテンツ タイプを発行する</a></p></li><li><p><a href="https://support.office.com/en-us/article/View-error-logs-for-content-type-publishing-fa8bc6d4-f03e-4ce7-94de-5fd1c38fe32c?CorrelationId=3aaa4607-0888-417a-a0ff-8c1e44ce8167&amp;ui=en-US&amp;rs=en-US&amp;ad=US" target="_blank">コンテンツ タイプの発行エラー ログを表示する</a></p></li></ul>|
|デバイス チャネル|複数のデバイスを対象とする異なる設計を使用して、複数の方法で発行サイト ページをレンダリングする方法。詳細については、「 [SharePoint Server 2013 でのデバイス チャネル パネル スニペットを追加する方法](http://msdn.microsoft.com/library/612780a8-6267-49f6-a32d-33600bb5f6b4%28Office.15%29.aspx)」を参照してください。|
|表示テンプレート|表示テンプレートは、検索テクノロジを使用する Web パーツで使用され、検索インデックスに対して作成されるクエリの結果のレンダリングを制御します。表示テンプレートは、すべての検索 Web パーツで使用できます。詳細については、「 [SharePoint Server 2013 の表示テンプレート リファレンス](https://technet.microsoft.com/en-us/library/jj944947%28v=office.15%29.aspx)」を参照してください。|
|管理ナビゲーション|SharePoint の Managed Metadata Service (分類) を利用したサイト ナビゲーション。これを使用して、管理されたメタデータの分類に由来するサイト ナビゲーションを作成します。多くの場合、管理ナビゲーションは製品カタログと一緒に使用すると最も効率よく機能します。|
|管理されたメタデータ|定義するために使用できる一元的に管理された用語および SharePoint の属性項目の階層的なコレクション。 管理されたメタデータを使用して、コンテンツ タイプの発行の管理と分類の作成を行うことができます。 SharePoint 2013 および SharePoint Online では、管理ナビゲーション (分類を利用したサイト ナビゲーション) を作成するために、Managed Metadata Service が使用されます。&mdash; 詳細については、以下を参照してください。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><a href="https://msdn.microsoft.com/en-us/library/office/bb897739(v=office.14).aspx" target="_blank">ナビゲーション コントロールおよびプロバイダーをカスタマイズする (ECM)</a></p></li><li><p><a href="https://support.office.com/en-us/article/Introduction-to-managed-metadata-a180fa28-6405-4679-9ec3-81d2028c4efc?CorrelationId=2a4dbdb0-0199-40e2-ac58-568ca0a3f099&amp;ui=en-US&amp;rs=en-US&amp;ad=US" target="_blank">管理されたメタデータの概要</a></p></li><li><p><a href="https://support.office.com/en-us/article/Introduction-to-managed-metadata-in-SharePoint-Server-2010-b324aebd-67ab-45a8-933d-ceedb2d909ea?CorrelationId=9152b80c-9fe4-43c5-bacc-178cc49d0e8f&amp;ui=en-US&amp;rs=en-US&amp;ad=US" target="_blank">SharePoint Server 2010 の管理されたメタデータの概要</a></p></li><li><p><a href="https://technet.microsoft.com/en-us/library/ee530393(v=office.14).aspx" target="_blank">管理されたメタデータの管理 (SharePoint Server 2010)</a></p></li><li><p><a href="http://msdn.microsoft.com/library/b66d4ec1-a2ef-49cc-8ca5-a6b516bff02e(Office.15).aspx" target="_blank">SharePoint 2013 のマネージ メタデータとナビゲーション</a></p></li><li><p><a href="http://msdn.microsoft.com/library/c9da5011-3c73-4b83-8e00-e7a03a71ed02(Office.15).aspx" target="_blank">SharePoint 2013 でのマネージ ナビゲーション</a></p></li><li><p><a href="https://msdn.microsoft.com/en-us/library/office/hh147179(v=office.14).aspx" target="_blank">SharePoint Server 2010 の管理されたメタデータの移行</a></p></li></ul>|
|[ナビゲーション名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.navigation.aspx)|発行サイトのサイト ナビゲーションをサポートするクライアント オブジェクト モデル (CSOM) を格納します。 |
|[分類の名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.aspx)|分類機能をサポートする CSOM クラスを格納します。詳細については、「 [用語グループを同期するサンプル SharePoint アドイン](http://msdn.microsoft.com/library/76caa8e2-2a37-432c-8f98-9aa4acf86f79%28Office.15%29.aspx)」を参照してください。|
|ピン止め|用語の再利用と同じように、ピン止めはソースの用語と再利用インスタンスの間の共有関係を維持します。たとえば、クロスサイト発行シナリオでは共有関係がサイト コレクション全体に及ぶ場合があります。用語が追加または削除されると、そのアクションは、用語がピン止めされている階層全体で更新されます。詳細については、「 [[方法] SharePoint 2013 で用語セット内を移動するために用語を固定するコードを使用する](http://msdn.microsoft.com/library/4a2811dc-25fd-4eb2-b0ab-1edded64c556%28Office.15%29.aspx)」を参照してください。|
|製品カタログ|詳細については、以下を参照してください。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><a href="http://msdn.microsoft.com/library/33f49e69-c1d3-4a6e-8887-5df683cba022(Office.15).aspx" target="_blank">SharePoint 2013 でのクロスサイト発行</a></p></li><li><p><a href="https://technet.microsoft.com/en-us/library/jj656774(v=office.15).aspx" target="_blank">SharePoint Server 2013 でクロスサイト発行を構成する</a></p></li></ul>|
|スニペット|[デザイン マネージャー](http://msdn.microsoft.com/library/29834b3f-3815-4347-91d3-296387663114%28Office.15%29.aspx)によって生成された HTML ファイルに追加できる HTML 機能の一部。 たとえば、[デバイス チャネル] パネルを発行ページに追加する場合、そのためのスニペットがあります。 次の記事に、いくつかの例が示されています。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><a href="http://msdn.microsoft.com/library/612780a8-6267-49f6-a32d-33600bb5f6b4(Office.15).aspx" target="_blank">SharePoint Server 2013 でのデバイス チャネル パネル スニペットを追加する方法</a></p></li><li><p><a href="http://msdn.microsoft.com/library/7583b217-200c-4569-8f88-fe975c8ebd72(Office.15).aspx" target="_blank">[方法]: SharePoint 2013 で Web パーツ領域スニペットを追加する</a></p></li><li><p><a href="http://msdn.microsoft.com/library/4beaab08-760b-408a-b768-906312779379(Office.15).aspx" target="_blank">SharePoint 2013 でセキュリティによるトリミングのスニペットを追加する方法</a></p></li></ul>|
|構造化ナビゲーション|構造化ナビゲーションの詳細については、「[[方法] ナビゲーションをカスタマイズする (ECM)](https://msdn.microsoft.com/en-us/library/office/ms558975%28v=office.14%29.aspx)」を参照してください。|

## CSOM を使用して発行サイトをブランド化するための前提条件

既定では、匿名ユーザーは一般向け SharePoint オンプレミス Web サイトに公開された Web コンテンツを利用できます。既定では、匿名ユーザーは CSOM と REST をどちらも利用できません。


                **重要:**このシナリオでは、オンプレミスの SharePoint サイトに対する潜在的な脅威について説明します。 「[リモート プロビジョニングを使用して SharePoint ページをブランド化する](Use-remote-provisioning-to-brand-SharePoint-pages.md)」で説明されているリモート プロビジョニング モデルを使用して、発行サイトのブランド化をプロビジョニングする前に、サイトのセキュリティとアクセス許可が正しく設定されていることを確認し、匿名アクセスがセキュリティに与える影響について考慮してください。

発行テンプレートを使用するサイト コレクションを含んだ新しい Web アプリケーションをサイト管理者が作成し、匿名アクセスも有効にした場合、そのアプリケーションがアドイン カタログにアップロードされると、サイトのすべてのユーザーは匿名アクセスができるようになります。匿名アクセスはオンプレミスの SharePoint 発行サイトに対して有効にされていることを考えると、認証されていないユーザーがこのサイトに移動した場合に何が生じるでしょうか。

SharePoint ホスト型アドインを作成して、クライアント アドイン パーツをプロジェクトに追加する場合は、**[サーバーの全体管理]** > **[アドイン]** > **[アドイン カタログの管理]** に移動して Web アプリケーションのアドイン カタログを作成します。次に、Visual Studio を使用してアドイン パッケージを作成し、SharePoint ホスト型アドインを発行します。その後、アドイン パッケージをアドイン カタログにアップロードします。 この時点で、サイト コレクションの管理者は発行サイトにアドインを追加できます。 これで、発行サイトのページにアドインを追加できます。

発行サイトのメイン ページを編集してアドインをそのページに発行する場合、アドインは期待どおりに機能します。アドインのリンク付けされたタイトルを選択すると、ページ全体のエクスペリエンスが読み込まれます。SharePoint がエラーをスローする場合、匿名ユーザーには CSOM を使用するアクセス権がないことを意味します。匿名ユーザーは既定で CSOM と REST を使用できないため、これは当然です。

### "リモート インターフェイスの使用" 権限を無効にするとセキュリティ上のリスクが生じることに注意する

**[サイトの権限]** では、**["リモート インターフェイスの使用" 権限を必須にする]** チェック ボックスが既定でオンになっています。 **[リモート インターフェイスの使用権限]** チェック ボックスをオフにして、匿名ユーザーに CSOM と REST を使用するアクセス権を付与することができます。 これにより、ユーザーは SOAP、WebDAV、および CSOM へのアクセス権をユーザーに付与する "リモート インターフェイスの使用" 権限から切り離されます。 このチェック ボックスをオフにすると、SharePoint Designer も使用できなくなります。

CSOM の使用は許可しながら、SharePoint Designer を使用する機能は削除したい状況があるかもしれません。[ **リモート インターフェイスの使用権限**] チェック ボックスによって、"リモート インターフェイスの使用" 権限なしでも匿名ユーザーに CSOM を使用することを許可できます。[ **リモート インターフェイスの使用権限**] チェック ボックスをオフにした状態で、アドインのリンク付けされたタイトルを選択してページ全体のエクスペリエンスを読み込んでも、SharePoint はエラーをスローしません。基本的なエラー処理コードは、このケースを匿名ユーザーとして解釈します。

**注意:**  
- 発行テンプレートを使用する一般向け SharePoint サイトにアプリを追加する場合、[サイトの権限] で **[リモート インターフェイスの使用権限]** チェック ボックスをオフにしないでください。匿名ユーザーに対して CSOM を有効にすると、情報を公開してしまうリスク、それも想定以上の情報が漏れてしまう可能性があります。とはいえ、フル CSOM のアクセス権があっても SharePoint アクセス許可は適用されます。匿名ユーザーには、匿名ユーザーが使用できるように明示的に指定されたリストや項目しか表示されません。CSOM および REST を介すると、Web ページに表示される内容以上を匿名ユーザーが使用できるようになります。
- SharePoint オンプレミス発行サイトで匿名アクセス許可が有効にされている場合は、絶対に必要でない限り、**["リモート インターフェイスの使用" 権限を必須にする]** チェック ボックスをオフにしないでください。オフにすると、発行サイトと非発行サイトの両方のコンテンツが匿名ユーザーに公開され、サービス妨害攻撃に対して無防備な状態になる可能性があります。

### ViewFormPagesLockdown 機能の使用

一般向け SharePoint サイトのフォーム ページ (たとえば、Pages/Forms/AllItems.aspx) にユーザーがアクセスできないようにするには、 **ViewFormPagesLockdown** 機能を使用します。この機能は、作成者および変更者の情報がユーザーに表示されないようにするために設計されています。この機能は [ **アプリケーション ページの表示**] または [ **リモート インターフェイスの使用**] へのアクセス許可を削除します。この機能がアクティブであると、ユーザーは Pages/Forms/AllItems.aspx に移動して、そのライブラリにある項目を表示することができません。

CSOM と REST をすべての匿名ユーザーが使用できる場合、**[リモート インターフェイスの使用権限]** チェック ボックスがオフの場合、匿名ユーザーがブラウザーで **[作成者]** と **[変更者]** の情報を表示できないことに変わりはないものの、CSOM または REST を使用すれば、その情報にアクセスできます。&mdash;&mdash;

### セキュリティ トリミング

Web アプリケーションの匿名アクセス権を構成して、匿名ポリシー:_[書き込み拒否 - 書き込みアクセス権を与えません]&mdash; を指定します。_ これにより、匿名アクセス権を持つユーザーは、CSOM または REST コードを使用した場合でも、サイトに書き込みを行うことができません。&mdash; サイトへの匿名アクセス権が構成されている場合、匿名ユーザーは許可された情報を表示することしかできません。

既定では、未発行ページは匿名ユーザーには表示されません。 これらのユーザーには、匿名アクセスを有効にするリストのみ表示されます。 


            **重要:****[リモート インターフェイスの使用権限]** チェック ボックスがオフの場合は、アクセス許可モデルとその他の予防措置を使用して、匿名ユーザーがアクセスを禁止されているものに対してアクセスできないようにします。

### サービス拒否攻撃を防ぐ

CSOM にはキャッシュはありません。これは、悪意のある攻撃者が既定のリスト ビューのしきい値を超えずに SharePoint データベースに負荷を掛け続けながら、何千もの項目をリストから同時に照会できることを意味します。これが拡大すると、本格的なサービス拒否攻撃に発展する可能性があります。 

### アプリ専用ポリシーの使用

プロバイダー ホスト型アドインでアプリ専用ポリシーを使用することができます。アプリ専用ポリシーは、現在のユーザーが実行することを許可されていないアクションを、アドインが実行することを許可します。たとえば、読み取り専用アクセス許可を持ち、リストに書き込みができないユーザーであっても、リストへの書き込みを行うためのアプリ専用アクセス許可を持つアドインを使用することができます。

詳細については、「 [SharePoint アドインの承認と認証](http://msdn.microsoft.com/library/bde5647a-fff1-4b51-b67b-2139de79ce4a%28Office.15%29.aspx)」および「 [SharePoint アドインと Office の認証とアクセス許可について](http://channel9.msdn.com/Events/Build/2013/3-603)」 (Channel 9 動画) を参照してください。

### SSL の実装

アドイン モデルを使用する場合、Secure Sockets Layer (SSL) プロトコルを実装して、インターネットでのメッセージ送信のセキュリティを管理します。SSL は、リモート Web サイトが HTTP ヘッダー Authorization にアクセス トークンを入れて、ベアラーの値 + Base64 エンコード (暗号化なし) 文字列を使用して送信することによって機能します。


            **重要:**SSL は、承認トークンを取得してそのアクセスを悪用しようとする攻撃者から SharePoint 発行サイトを保護します。

## リモート プロビジョニングと発行サイト

[リモート プロビジョニング プラクティス](Use-remote-provisioning-to-brand-SharePoint-pages.md)を使用して、ブランド化とその他のカスタマイズを SharePoint 発行サイトにプロビジョニングできます。

発行サイトは、コンテンツ タイプと  **ContentTypeId** (コンテンツ タイプをページ レイアウトと表示テンプレートにリンクする) に依存します。SharePoint 発行ページ コンテンツのカスタマイズとプロビジョニングは、この機能に依存します。Managed Metadata Service や管理ナビゲーションなどのカスタム発行サイトのプロビジョニング動作のその他の側面は、 **ContentTypeId** には依存せず、CSOM で完全にサポートされます。

デバイス チャネルや表示テンプレートなどのその他の発行サイト カスタマイズ オプションは、カスタム CSOM を必要としません。それらのオプションは、デザイン マネージャーの機能、CSS、および HTML に依存します。それらは、一から作成するポストプロビジョニングのカスタマイズであり、移行する必要がありません。

## 管理されたメタデータ

管理されたメタデータ機能 (SharePoint 2010 で最初に導入) によって、SharePoint で使用するメタデータ タグのカスタム階層または分類を定義できます。カスタム サイト ナビゲーションを作成する場合、管理されたメタデータ インフラストラクチャに作成される管理ナビゲーション機能を使用できます。

分類は、類似性に応じてグループに編成される単語、ラベル、または用語の階層的分類です。SharePoint 分類の最小単位は、用語です。用語は用語セットにグループ化できます。用語セットは、関連性によってより大きいグループにまとめることができます。

SharePoint の分類の簡単な紹介については、「[エンタープライズ メタデータ管理の概要 (Microsoft SharePoint Server 2010 の開発者向け)](https://msdn.microsoft.com/en-us/library/office/ee832800%28v=office.14%29.aspx)」および「 [SharePoint 2010 でのエンタープライズ管理メタデータの紹介](https://support.office.com/en-us/article/Introduction-to-managed-metadata-in-SharePoint-Server-2010-b324aebd-67ab-45a8-933d-ceedb2d909ea?CorrelationId=2251ec3f-51ef-4924-89cc-d828b5068851&amp;ui=en-US&amp;rs=en-US&amp;ad=US)」を参照してください。

SharePoint 2013 では、管理されたメタデータ機能セットに新しい API と機能が導入されました。

### 管理されたメタデータ プログラミング モデル

SharePoint 2013 では、管理されたメタデータ用の .NET サーバー プログラミング モデルは、以下の一連の名前空間で定義されています。 

- [Taxonomy 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.aspx)
    
- [ContentTypeSync 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.contenttypesync.aspx)
    
- [eneric 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.generic.aspx)
    
- [CodeBehind 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.om.codebehind.aspx)
    
- [Upgrade 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.upgrade.aspx)
    
- [WebServices 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.webservices.aspx)
    
同等の CSOM クラスは、 [Client.Taxonomy 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.aspx)にあります。

SharePoint オブジェクト モデルのその他の領域とは異なり、管理されたメタデータでは, .NET サーバー プロビジョニング モデル クラスおよびメンバーと CSOM クラスおよびメンバーとの間に密接な関係があります。次にいくつかの重要な相違点を示します。

- CSOM はコンテンツ タイプの同期をサポートしません。
    
- **Group** クラス ([TermStore](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termstore.aspx) クラスにおける組織の最上位レイヤーを表します) は .NET サーバー オブジェクト モデルでのみ使用できます。CSOM でこれに相当するのは、 [TermGroup](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termgroup.aspx) クラスです。
    
- [GroupCollection クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.groupcollection.aspx) (グループ オブジェクトのコレクションを表します) は, .NET サーバー オブジェクト モデルでのみ使用できます。CSOM でこれに相当するのは、 [TermGroupCollection クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termgroupcollection.aspx) です。
    
- CSOM には、サーバーとの間でカスタム プロパティとラベル データの同期を保つ [CustomPropertyMatchInformation](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.custompropertymatchinformation.aspx) および [LabelMatchInformation](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.labelmatchinformation.aspx) クラスが含まれます。
    
次の表では, .NET サーバー オブジェクト モデルと CSOM オブジェクト モデルのクラスを比較します。

|**.NET サーバー オブジェクト モデル**|**同等の CSOM API**|
|:-----|:-----|
|[Microsoft.SharePoint.Taxonomy.ChangedGroup](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changedgroup.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedGroup](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changedgroup.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedItem](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changeditem.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedItem](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changeditem.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedItemCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changeditemcollection.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedItemCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changeditemcollection.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedItemType](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changeditemtype.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedItemType](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changeditemtype.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedOperationType](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changedoperationtype.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedOperationType](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changedoperationtype.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedSite](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changedsite.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedSite](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changedsite.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedTerm](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changedterm.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedTerm](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changedterm.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedTermSet](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changedtermset.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedTermSet](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changedtermset.aspx)|
|[Microsoft.SharePoint.Taxonomy.ChangedTermStore](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.changedtermstore.aspx)|[Microsoft.SharePoint.Client.Taxonomy.ChangedTermStore](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changedtermstore.aspx)|
|該当なし。|[Microsoft.SharePoint.Client.Taxonomy.ChangeInformation](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.changeinformation.aspx)|
|該当なし。|[Microsoft.SharePoint.Client.Taxonomy.CustomPropertyMatchInformation](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.custompropertymatchinformation.aspx)|
|[Microsoft.SharePoint.Taxonomy.FeatureIds](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.featureids.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.Group](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.group.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.HiddenListFullSyncJobDefinition](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.hiddenlistfullsyncjobdefinition.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.ImportManager](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.importmanager.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.Label](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.label.aspx)|[Microsoft.SharePoint.Client.Taxonomy.Label](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.label.aspx)|
|[Microsoft.SharePoint.Taxonomy.LabelCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.labelcollection.aspx)|[Microsoft.SharePoint.Client.Taxonomy.LabelCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.labelcollection.aspx)|
|該当なし。|[Microsoft.SharePoint.Client.Taxonomy.LabelMatchInformation]()|
|[Microsoft.SharePoint.Taxonomy.MobileTaxonomyField](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.mobiletaxonomyfield.aspx)|[Microsoft.SharePoint.Client.Taxonomy.MobileTaxonomyField](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.mobiletaxonomyfield.aspx)|
|[Microsoft.SharePoint.Taxonomy.StringMatchOption](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.stringmatchoption.aspx)|[Microsoft.SharePoint.Client.Taxonomy.StringMatchOption]()|
|[Microsoft.SharePoint.Taxonomy.TaxonomyField](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyfield.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TaxonomyField](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.stringmatchoption.aspx)|
|[Microsoft.SharePoint.Taxonomy.TaxonomyFieldControl](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyfieldcontrol.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.TaxonomyFieldEditor](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyfieldeditor.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.TaxonomyFieldValue](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyfieldvalue.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TaxonomyFieldValue](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.taxonomyfieldvalue.aspx)|
|[Microsoft.SharePoint.Taxonomy.TaxonomyFieldValueCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyfieldvaluecollection.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TaxonomyFieldValueCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.taxonomyfieldvaluecollection.aspx)|
|[Microsoft.SharePoint.Taxonomy.TaxonomyItem](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyitem.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TaxonomyItem](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.taxonomyitem.aspx)|
|[Microsoft.SharePoint.Taxonomy.TaxonomyItemPicker](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyitempicker.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.TaxonomyRights](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomyrights.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.TaxonomySession](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomysession.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TaxonomySession](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.taxonomysession.aspx)|
|[Microsoft.SharePoint.Taxonomy.TaxonomyWebTaggingControl](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.taxonomywebtaggingcontrol.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.Term](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.term.aspx)|[Microsoft.SharePoint.Client.Taxonomy.Term](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.term.aspx)|
|[Microsoft.SharePoint.Taxonomy.TermCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termcollection.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TermCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termcollection.aspx)|
|該当なし。|[Microsoft.SharePoint.Client.Taxonomy.TermGroup](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termgroup.aspx)|
|該当なし。|[Microsoft.SharePoint.Client.Taxonomy.TermGroupCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termgroupcollection.aspx)|
|[Microsoft.SharePoint.Taxonomy.TermProperty](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termproperty.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.TermPropertyToolPart](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termpropertytoolpart.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.TermSet](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termset.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TermSet](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termset.aspx)|
|[Microsoft.SharePoint.Taxonomy.TermSetCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termsetcollection.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TermSetCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termsetcollection.aspx)|
|[Microsoft.SharePoint.Taxonomy.TermSetItem](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termsetitem.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TermSetItem](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termsetitem.aspx)|
|[Microsoft.SharePoint.Taxonomy.TermStore](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termstore.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TermStore](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termstore.aspx)|
|[Microsoft.SharePoint.Taxonomy.TermStoreCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termstorecollection.aspx)|[Microsoft.SharePoint.Client.Taxonomy.TermStoreCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.taxonomy.termstorecollection.aspx)|
|[Microsoft.SharePoint.Taxonomy.TermStoreOperationException](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.termstoreoperationexception.aspx)|該当なし。|
|[Microsoft.SharePoint.Taxonomy.TreeControl](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.taxonomy.treecontrol.aspx)|該当なし。|

## ページ レイアウト

発行サイトでは、ページ レイアウトによって、特定のクラスのページのレイアウトが定義されます。同様に、コンテンツ プレースホルダー (ページ レイアウトの一致する領域からのコンテンツが埋め込まれる) を持つ、ページのカスタマイズ可能領域も定義されます。通常、ページ レイアウトはカスタム コンテンツ タイプに基づきます。コンテンツ タイプは、ページに表示するカスタム コンテンツ フィールドを定義します。通常、ユーザーまたは設計チームが以前に計画したページ デザインにマップするフィールドを含むカスタム コンテンツ タイプを作成します。

カスタム フィールド コントロールには、テキスト、イメージ、動画、またはその他のタイプのコンテンツを含めることができます。 SharePoint では、アーティクル、カタログ、ウェルカム ページなどのコンテンツ タイプが提供されます。 ページ レイアウト コンテンツにアクセスするには、**[サイト設定]** > **[サイト コンテンツ タイプ]** に移動します。 これらの既定のページ レイアウト コンテンツ タイプは、作成するカスタム コンテンツ タイプの親コンテンツ タイプとして機能できます。

すべてのページ レイアウト コンテンツ タイプは、ページ コンテンツ タイプを継承します。カスタム コンテンツ タイプを作成すると、新しいコンテンツ タイプがページ コンテンツ タイプから継承した列が SharePoint に表示されます。ページ レイアウトに表示する新しいカスタム フィールドを表す新しい サイト列を追加できます。サイト列ごとにタイプを指定します。タイプは "1 行テキスト" や "完全な HTML" などの値です。そのタイプを指定するときには、ユーザーがフィールドに対して持つ記述性やコントロールの程度などのファクターを考慮します。

ページ レイアウトのコンテンツを格納するフィールドをすべて作成すると、デザイン マネージャーでページ レイアウトを作成できます。 詳細については、「[方法:SharePoint 2013 でページ レイアウトを作成する](http://msdn.microsoft.com/library/5447e6a1-2f14-4667-81d0-7514b468be80%28Office.15%29.aspx)」を参照してください。 ページ レイアウトを作成したら、**[サイト設定]** > **[マスター ページとページ レイアウト]** に移動して、そのレイアウトを発行します。 HTML ファイルと ASPX ファイルが存在します。 HTML ファイルはマスターです。このファイルは任意の HTML エディターを使用して編集できます。 ファイルを保存して発行すると、デザイン マネージャーは変更を取り込み、更新された HTML ファイルを ASPX 形式に変換します。このファイルを使用して、SharePoint はページをレンダリングします。 ページ レイアウトを発行するには、HTML ファイルを選択し、リボンで **[発行]** を選択します。

新しいレイアウトに基づいてページを作成するには、**[新しいページ]** > **[ページ]** > **[ページ レイアウト]** に移動して、使用可能なページ レイアウトのリストに新しいページ レイアウトを表示します。 新しいページ レイアウトを選択すると、新しいページ レイアウトの新しいコンテンツ タイプを作成した際に指定した新しいフィールドがすべて表示されます。 ページを表示しても HTML が想定どおりのにレンダリングされない場合は、その HTML を編集し、更新したファイルをデザイン マネージャーでアップロードすることができます。

## サイト ナビゲーション

サイト ナビゲーションには 4 種類があります。 

- グローバル ナビゲーション
    
- ローカル ナビゲーション
    
- 構造化ナビゲーション
    
- 管理ナビゲーション
    
グローバル ナビゲーションは、ユーザーが 1 つの SharePoint サイトから別のサイトに移動する際に役立つナビゲーションの要素を指します。ローカル ナビゲーションは、SharePoint サイト内のナビゲーションを指します。構造化ナビゲーションと管理ナビゲーションは、少し複雑です。

### 構造化ナビゲーション

構造化ナビゲーションは、SharePoint サイトにナビゲーションを実装するための静的なアプローチです。通常、これは SharePoint サイト ナビゲーションを再構築する必要がある企業の構造に適合します。多くの場合、再構築の作業は、サブサイトやページの移動、および新しいターゲットを指すリンクの更新です。

企業の構造 (したがってサイト構造) が長い間安定している場合、ほとんどフラットで浅いサイト構造であれば、構造化ナビゲーションで十分な場合があります。より深くより複雑なサイト構造の場合や、動的に拡大および変更する必要のある公開サイト ナビゲーション構造を使用している企業の場合は、管理ナビゲーションの方が優れたオプションです。構造化ナビゲーションの詳細については、「 [[方法] ナビゲーションをカスタマイズする](https://msdn.microsoft.com/en-us/library/office/ms558975%28v=office.14%29.aspx)」を参照してください。

### 管理ナビゲーション

管理ナビゲーションは、中心となる発行サイトと分類インフラストラクチャを基にしています。管理ナビゲーションは、単一のサイト コレクションにバインドされます。管理ナビゲーションは、用語セットと用語を使用してグローバル ナビゲーションとローカル ナビゲーションを定義します。たとえば、グローバル ナビゲーション全体を定義する用語セットを作成してから、グローバル ナビゲーションの特定のナビゲーション要素に関する用語をその用語セットに追加することができます。

管理ナビゲーションの詳細については、以下の記事を参照してください。

- [SharePoint 2013 でのマネージ ナビゲーション](http://msdn.microsoft.com/library/c9da5011-3c73-4b83-8e00-e7a03a71ed02%28Office.15%29.aspx)
    
- [SharePoint Server 2013 の管理ナビゲーションの概要](https://technet.microsoft.com/en-us/library/dn194311%28v=office.15%29.aspx)
    
以下は、発行サイトの SharePoint ナビゲーションの拡張性モデルにおけるクラス、メソッド、およびプロパティに関する重要な点です。

-  **NavigationTerm** および **NavigationTermSet** クラスは、管理ナビゲーション固有のプロパティやメソッドを追加します。その他の状態は **NavigationTerm** クラスの **CustomProperties** プロパティに格納されます。
    
-  **NavigationTerm** および **NavigationTermSet** クラスには、編集可能モードと読み取り専用モードの 2 つのモードがあります。.NET サーバー オブジェクト モデルでは、 **NavigationTerm** オブジェクトは分類ナビゲーション キャッシュに格納されます。これには、 **TaxonomyNavigation** 静的クラスの関数しかアクセスできません。
    
-  .NET サーバー オブジェクト モデルの  **PortalSiteMapNodeProvider** オブジェクトは、データ駆動型サイト ナビゲーション機能とサイト マップ データ ソースの間のインターフェイスを提供します。通常、カスタム サイト マップ ノード プロバイダーを記述して、サイト マップを XML ファイルで、または既定では SharePoint によってサポートされないデータ形式で格納します。
    
CSOM には、以下の固有のクラスと列挙が含まれます。

- **NavigationLinkType** 列挙は、ナビゲーション ツリーのナビゲーション ノードのタイプを定義します。ノードをルート ノード、フレンドリ URL、または標準リンクとして指定できます。
    
- **StandardNavigationScheme** 列挙は、ナビゲーションをグローバルかローカルのいずれかとして識別します。
    
- **StandardNavigationSource** 列挙には、グローバル ナビゲーションとローカル ナビゲーションの両方に関する 3 つの選択肢が含まれます。それぞれの選択肢は、基盤となるプロバイダーの構成に対応する状態を表します。
    
- **StandardNavigationSettings** クラスは、グローバルとローカルのナビゲーション スキーマを管理します。
    
次の表は、サーバー オブジェクト モードの発行ナビゲーション クラスと、CSOM でそれに相当するものをリストしています。

|**サーバー オブジェクト モデル**|**CSOM**|
|:-----|:-----|
|[Microsoft.SharePoint.Publishing.Navigation.CachedObjectSiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.cachedobjectsitemapnode.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.NavigationComparer](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.navigationcomparer.aspx)|該当なし。|
|該当なし。|[Microsoft.SharePoint.Client.Publishing.Navigation.NavigationLinkType](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.navigationlinktype.aspx)|
|[Microsoft.SharePoint.Publishing.Navigation.NavigationTerm](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.navigationterm.aspx)|[Microsoft.SharePoint.Client.Publishing.Navigation.NavigationTerm](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.navigation.navigationterm.aspx)|
|該当なし。|[Microsoft.SharePoint.Client.Publishing.Navigation.NavigationTermCollection](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.navigation.navigationtermcollection.aspx)|
|[Microsoft.SharePoint.Publishing.Navigation.NavigationTermSet](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.navigationtermset.aspx)|[Microsoft.SharePoint.Client.Publishing.Navigation.NavigationTermSet](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.navigationtermset.aspx)|
|[Microsoft.SharePoint.Publishing.Navigation.NavigationTermSetItem](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.navigationtermsetitem.aspx)|[Microsoft.SharePoint.Client.Publishing.Navigation.NavigationTermSetItem](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.navigationtermsetitem.aspx)|
|[Microsoft.SharePoint.Publishing.Navigation.NavigationTermSetView](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.navigationtermsetview.aspx)|[Microsoft.SharePoint.Client.Publishing.Navigation.NavigationTermSetView](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.navigationtermsetview.aspx)|
|[Microsoft.SharePoint.Publishing.Navigation.PortalHierarchicalDataSourceView](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalhierarchicaldatasourceview.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalHierarchicalEnumerable](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalhierarchicalenumerable.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalHierarchyData](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalhierarchydata.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalListItemSiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portallistitemsitemapnode.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalListSiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portallistsitemapnode.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalNavigation](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalnavigation.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalSiteMapDataSource](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalsitemapdatasource.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalSiteMapDataSourceSwitch](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalsitemapdatasourceswitch.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalSiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portallistsitemapnode.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalSiteMapProvider](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalsitemapprovider.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.PortalWebSiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.portalwebsitemapnode.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.ProxySiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.proxysitemapnode.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.SiteNavigationSettings](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.sitenavigationsettings.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.SiteNavigationSettingsWriter](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.sitenavigationsettingswriter.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.SPNavigationSiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.spnavigationsitemapnode.aspx)|該当なし。|
|該当なし。|[Microsoft.SharePoint.Client.Publishing.Navigation.StandardNavigationSource](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.standardnavigationsource.aspx)|
|該当なし。|[Microsoft.SharePoint.Client.Publishing.Navigation.StandardNavigationSettings](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.standardnavigationsettings.aspx)|
|[Microsoft.SharePoint.Publishing.Navigation.TaxonomyNavigation](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.taxonomynavigation.aspx)|[Microsoft.SharePoint.Client.Publishing.Navigation.TaxonomyNavigation](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.taxonomynavigation.aspx)|
|[Microsoft.SharePoint.Publishing.Navigation.TaxonomyNavigationCacheConfig](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.taxonomynavigationcacheconfig.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.TaxonomyNavigationCacheStatistics](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.taxonomynavigationcachestatistics.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.TaxonomyNavigationContext](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.taxonomynavigationcontext.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.TaxonomySiteMapNode](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.taxonomysitemapnode.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.TaxonomySiteMapProvider](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.taxonomysitemapprovider.aspx)|該当なし。|
|[Microsoft.SharePoint.Publishing.Navigation.WebNavigationSettings](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.navigation.webnavigationsettings.aspx)|[Microsoft.SharePoint.Client.Publishing.Navigation.WebNavigationSettings](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.publishing.navigation.webnavigationsettings.aspx)|

## 発行サイトの機能

SharePoint 2013 および SharePoint Online には、SharePoint 発行サイトに固有のいくつかの機能が含まれています。

- デバイス チャネルは、1 つの発行サイトのデザインを複数のデバイスやブラウザーに適用できるようにします。
    
- 表示テンプレートは、検索関連の Web パーツの外観をブランド化およびカスタマイズできるようにします。
    
- イメージ表示は、SharePoint 2013 発行サイトのページにイメージを表示するために使用されるディメンションを定義します。
    
CSOM プログラミング モデルの [Publishing](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.aspx) 名前空間のクラスを使用することによって、これらの機能を実装できます。

### デバイス チャネルおよびデバイス チャネル パネル

デバイス チャネルおよびデバイス チャネル パネルを使用して、サイトを電話やタブレットに最適化できます。対象にするデバイスごとに固有のチャネルを作成することによって、複数の方法で SharePoint 発行サイトをレンダリングできます。単一のサイトを 1 回だけ作成し、そのサイトとそのコンテンツを異なるマスター ページとスタイル シートにマップすることによって、異なるターゲットに合わせることができます。

[デザイン マネージャー](http://msdn.microsoft.com/library/29834b3f-3815-4347-91d3-296387663114%28Office.15%29.aspx)を使用してチャネルを作成します。チャネルを作成したら、それを受信デバイスのユーザー エージェント文字列を使用してモバイル デバイスまたはブラウザーにマップします。

デバイスは、複数のチャネルに所属させることができます。その場合、デバイスが属しているイベントでチャネルにランクを付けることができます。たとえば、スマート フォン用のチャネルと特定のデバイス構成用のチャネルを作成する場合、特定の構成を持つデバイスが専用のチャネルを取得し、その他すべてのスマート フォンはスマート フォンのチャネルを取得できるようにチャネルにランクを付けることができます。

デザイン マネージャーでは、最大 10 個までチャネル (既定のチャネルを含む) を作成できます。既定のチャネルは、その他のチャネルのいずれにもキャッチされなかったすべてのトラフィックをキャプチャします。新しいチャネルを作成する場合は、次の表に示されるフィールドに入力します。

|**フィールド**|**必須かどうか**|**説明**|
|:-----|:-----|:-----|
|名前|必須|他から区別するためにチャネルを識別します。|
|チャネル エイリアス|必須|コード、クエリ文字列、Cookie、またはデバイス チャネル パネルがチャネル間の相違をどのように区別するか。|
|デバイス判定ルール|必須|デバイスからの要求の行き先を指示するユーザー エージェント サブ文字列 (それぞれ別の行にする必要があります)。|
|説明|省略可能|このチャネルの機能について説明します。|
|アクティブ|省略可|オンにした場合、チャネルはチャネルの関連資産を使用してトラフィックの行き先を指示します。それ以外の場合、クエリ文字列  `?DeviceChannel=<alias>` はチャネルでサイトをプレビューします。|
 **デバイス チャネル パネル**

デバイス チャネルを作成した後、それぞれにマスター ページをマップします。マスター ページのカスタマイズはあまり行われなくなっているため、多くの場合、これが既定のマスター ページ (seattle.master) になります。1 つ以上のデバイス チャネルに対して固有のマスター ページを作成する場合は、既定チャネルのマスター ページとは異なる CSS ファイルを参照できます。作成するページ レイアウトは、作成するすべてのチャネルで機能します。デバイス チャネル パネル コントロールを使用して、チャネル間のページ レイアウト デザインを区別することができます。

詳細については、「 [SharePoint 2013 デザイン マネージャーのデバイス チャネル](http://msdn.microsoft.com/library/a924bd7b-a5e3-41bf-b0a7-3e43945fa951%28Office.15%29.aspx)」を参照してください。

デバイス チャネル パネルは、1 つ以上のチャネルにマップされたコンテナー コントロールです。デバイス チャネル パネル コントロールをページ レイアウトに追加できます。そうすると、パネルは各チャネルでどのコンテンツをレンダリングするかを制御します。ページがレンダリングされるときにそれらのデバイス チャネルの 1 つ以上がアクティブである場合、デバイス チャネル パネルのすべてのコンテンツがレンダリングされます。デバイス チャネル パネルを使用して、1 つ以上の特定のチャネルの特定のコンテンツを含める条件を決定します。

10 個のフィールドを含むページ レイアウトがあるとします。これらのフィールドの一部はすべてのチャネルで使用でき、一部は特定のチャネルでレンダリングされます。たとえば、スマート フォンでのみレンダリングするモバイル ヘッダー バナー フィールド、またはデスクトップやタブレットでのみレンダリングする大きいカスタム サイドバーを考えてみてください。

また、デバイス チャネル パネルを使用し、チャネル固有の CSS を追加することによって、スタイルやページ上でのコンテンツの配置を変更することができます。詳細については、「 [SharePoint Server 2013 でのデバイス チャネル パネル スニペットを追加する方法](http://msdn.microsoft.com/library/612780a8-6267-49f6-a32d-33600bb5f6b4%28Office.15%29.aspx)」および「 [SharePoint 2013 で CSS を使ってスニペットをブランド化する方法](http://msdn.microsoft.com/library/d18d07b6-1a6b-4589-a65c-932b67cef595%28Office.15%29.aspx)」を参照してください。

**ユーザー エージェント文字列とデバイス チャネル**

ユーザー エージェント文字列は、ブラウザーを識別するデータの短い文字列です。サーバーにこの情報を送信すると、そこでユーザー エージェントが識別されます。デバイス チャネルは、ユーザーが閲覧に使用するデバイス (またはブラウザー) のユーザー エージェント文字列 (またはサブ文字列) に基づき、対応するデバイス チャネルに要求を割り当てます。フロント エンド Web 開発者は、トラフィックをキャプチャするチャネルを作成してセットアップします。詳細については、「 [Windows Internet Explorer がユーザー エージェント文字列として報告する内容](https://msdn.microsoft.com/en-us/library/cc817582.aspx)」を参照してください 。

**デバイス チャネルを解決する順序**

複数のチャネルを作成する場合、解決の順序に従ってそれらを配置します。ユーザー エージェント文字列に一致するデバイス判定ルールを含む最初のチャネルが使用されます。次の表に、このルールの例を示します。

|**チャネル**|**順序 1**|**順序 2**|
|:-----|:-----|:-----|
|1|Windows Phone 8|Windows Phone|
|2|Windows Phone|Windows Phone 8|
|3|既定値|既定値|
順序 1 がアクティブな場合、Windows 8 フォンからページを要求するユーザーは、Windows Phone 8 とラベルが付けられているデバイス チャネル 1 を受信します。その他の Windows フォンを使用するユーザーはチャネル 2 を使用し、それ以外のものはすべてチャネル 3 を使用します。ただし、順序 2 を使用すると、Windows 8 フォンからページを要求するユーザーは、Windows Phone とラベルが付けられたデバイス チャネル 1 を必ず受け取り、専用のデバイス チャネルは使用しません。

デバイス チャネルを定義して順序を付けたら、チャネルごとに異なるマスター ページを適用できます。既定では、すべてのチャネルは、既定のチャネルのマスター ページを使用します。 


            **メモ:**CSOM には、デバイス チャネルおよびデバイス チャネル パネルを操作するためのパブリック API は含まれません。

### 表示テンプレート

検SharePoint 発行サイトでは、表示テンプレートを使用して、検索結果に表示される管理プロパティや Web パーツに表示される方法を制御します。検索 Web パーツのみが表示テンプレートを使用します。コンテンツ クエリ Web パーツは検索 Web パーツではなく、表示テンプレートを使用しません。

次の表に、SharePoint が適用する順序で、表示テンプレートの種類を一覧で示します。 

|**表示テンプレート**|**メモ**|
|:-----|:-----|
|コントロール表示テンプレート|Web パーツ全体に適用されます。そのため、SharePoint は最初に 1 回だけこのテンプレートを適用します。検索結果を表示するための全体的なレイアウトを構造化する HTML を提供します。たとえば、コントロール表示テンプレートは、見出しおよびリストの先頭と末尾に関する HTML を提供します。このテンプレートは、Web パーツで一度だけレンダリングされます。 |
|グループ表示テンプレート|2 番目に適用され、グループごとに一度、検索結果 Web パーツに適用されます。 |
|アイテム表示テンプレート|フィルター表示テンプレートが適用される場合を除き、最後に適用されます。アイテム表示テンプレートは、各アイテムに適用されます。このテンプレートは、結果セットの各アイテムが Web パーツに表示される方法を決定します。たとえば、プレーン テキストであるリスト アイテム、画像を含むリスト アイテム、または一連の追加リンクと検索結果の詳細なコンテキストを提供するのに役立つ概要説明情報を書式設定するリスト アイテム用の HTML を提供することがあります。 |
SharePoint は、マスター ページ ギャラリーの表示テンプレート フォルダーに表示テンプレートを格納します。各表示テンプレートは、マスター ページ ギャラリーのコンテンツ タイプに関連付けられます。マップされたドライブの使用中に各表示テンプレート ファイルのコンテンツ タイプを識別するために、ファイル名を使用します。

マスター ページおよびページ レイアウトを HTML から JavaScript に変更して更新するイベント レシーバーは、表示テンプレートも HTML から JavaScript に変換します。変換と同期は一方向に実行されます。JavaScript から HTML に戻す変換は行われません。


            **メモ:**CSOM には、表示テンプレートを操作するためのパブリック API は含まれていません。

**表示テンプレートの構造**

各表示テンプレートには、以下が含まれます。 

- タイトル。
    
- `<mso:CustomDocumentProperties>` タグで囲まれた、カスタム要素を含むヘッダー。
    
- スクリプト ブロックを含む `<body>` タグ。
    
- `<div>` タグ。
    
カスタム ドキュメント プロパティは表示テンプレートに関する重要な情報を SharePoint に提供します。各表示テンプレートは、 `<ContenTypeId>` によって識別されるコンテンツ タイプに関連付けられます。他のプロパティを設定することにより、Web パーツの使用可能な表示テンプレートのリストにテンプレートを表示するかどうか、HTML から JavaScript への管理プロパティ マッピング、表示テンプレートが使用されるコンテキスト, .js ファイルが表示テンプレート HTML に現在関連付けられているかどうか、HTML から JavaScript への変換が成功したかあるいは警告やエラーが生成されたかどうかを判別することができます。 

`<script>` タグから、外部 CSS、またはメインの表示テンプレート HTML ファイルの外部にある JavaScript ファイルを参照できます。

マスター ページ ギャラリーでコンテンツの承認が必要な場合、マスター ページとページ レイアウトで使用可能にする前に、すべての CSS、JavaScript、およびその他のリソース ファイルを発行する必要があります。詳細については、「 [サイトまたはライブラリのアイテムの承認を必須にする](https://support.office.com/en-us/article/Require-approval-of-items-in-a-site-list-or-library-cd0761c4-8c3f-4ea2-9435-13c28aa23d08?CorrelationId=4efce7db-3017-4f50-854d-1f07eaf6bc16&amp;ui=en-US&amp;rs=en-US&amp;ad=US)」を参照してください。 `<div>` タグには、表示テンプレート HTML ファイルの名前と一致する ID が含まれています。この Web パーツの表示方法をカスタマイズする、含めようとしている CSS または JavaScript を、 `<div>` ブロックに配置します。

**表示テンプレートの処理**

SharePoint は HTML ファイルと JavaScript の表示テンプレートを定義します。表示テンプレート定義を含む HTML ファイルをデザイン マネージャーで変更して保存する場合、SharePoint は変更をコンパイルして、同じ名前を持つ JavaScript ファイルに組み込みます。SharePoint はこの JavaScript ファイルを使用して Web パーツをページにレンダリングします。


                **重要:**表示テンプレート定義が含まれる JavaScript ファイルは編集しないでください。 更新は HTML ファイルにのみ行います。 変換プロセスでは、HTML ファイルが XML に準拠している必要があります。 たとえば、**&lt;br/&gt;** ではなく **&lt;br&gt;** を使用します。 詳細については、「[方法:SharePoint 2013 で HTML ファイルをマスター ページに変換する](http://msdn.microsoft.com/library/a76ab289-3256-45de-ac63-d5112a74e3c7%28Office.15%29.aspx)」を参照してください。

**新しい表示テンプレートの作成**

新しい表示テンプレートを作成する最も簡単な方法は、既存のテンプレートを変更する方法です。さまざまな表示テンプレートがさまざまなタイプの検索関連の Web パーツの概観を変化させます (コンテンツ検索 Web パーツ、絞り込み Web パーツ、分類絞り込み Web パーツ、検索結果 Web パーツなど)。詳細については、以下を参照してください。

- [SharePoint 2013 デザイン マネージャー表示テンプレート](http://msdn.microsoft.com/library/1a782bac-48ee-4baf-8751-0f943a306e0f%28Office.15%29.aspx)
    
- [SharePoint Server 2013 で検索 Web パーツを構成する](https://technet.microsoft.com/en-us/library/jj679900%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 の絞り込み Web パーツのプロパティを構成する](https://technet.microsoft.com/en-us/library/gg549985%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 の表示テンプレート リファレンス](https://technet.microsoft.com/en-us/library/jj944947%28v=office.15%29.aspx)
    
**表示テンプレートで使用できるプロパティ**

表示テンプレートで使用できるプロパティを識別し始める前に、作成元となる既存の表示テンプレートを見つけ、そのテンプレートを新しい名前で保存します。表示テンプレート コードは、 `<mso:ManagedPropertyMapping>` タグ内にあります。

```
<mso:ManagedPropertyMapping msdt:dt="string">'Picture URL'{Picture URL}:'PublishingImage;PictureURL;PictureThumbnailURL','Link URL'{Link URL}:'Path','Line 1'{Line 1}:'Title','Line 2'{Line 2}:'Description','Line 3'{Line3}:'','SecondaryFileExtension','ContentTypeId'</mso:ManagedPropertyMapping>
```

次に、**[サイト設定]** > **[検索スキーマ]** を開き、表示テンプレートに含める列名を管理プロパティのフィルター ボックスで検索します。 管理プロパティを選択して、プロパティ名を編集およびコピーします。 

```
<mso:ManagedPropertyMapping msdt:dt="string">'Picture URL'{Picture URL}:'PublishingImage;PictureURL;PictureThumbnailURL','Link URL'{Link URL}:'Path','Line 1'{Line 1}:'Title','Line 2'{Line 2}:'Description','Line 3'{Line3}:'','owsTXTPrice','owsTXTColor'</mso:ManagedPropertyMapping>
```


            **メモ:**この例では、**PictureURL** は、検索で **PublishingImage**、**PictureURL**、または **PictureThumbnailURL** の結果が得られる場合に表示される、最初の管理プロパティにマップされます。

### イメージ表示

イメージ表示は、SharePoint 2013 発行サイトのページにイメージを表示するために使用されるディメンションを定義します。CSOM を使用して、イメージ表示のインスタンスの作成および操作を実行できます。 [ImageRendition](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.imagerendition.aspx) クラスを使用してイメージ表示の高さ、幅、名前、バージョンなどのメタデータを指定できます。 [SiteImageRenditions](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.siteimagerenditions.aspx) クラスのメソッドとプロパティを使用して、サイト コレクションのイメージ表示の読み取りと書き込みができます。

詳細については、「 [SharePoint 2013 デザイン マネージャーのイメージ表示](http://msdn.microsoft.com/library/d08a74c0-5674-4f26-8646-11ea1f081c85%28Office.15%29.aspx)」を参照してください。

## SharePoint と一般的な Web プログラミング手法

多くの SharePoint デザイナーおよび開発者は、発行サイトを設計するときに、標準的な Web プログラミング手法を SharePoint で使用することを好みます。レスポンシブ デザインまたはアダプティブ デザイン、あるいはデバイス チャネルとレスポンシブ デザインを一緒に使用することができます。

### レスポンシブ デザイン

デバイス チャネルでは、1 つのサイトを一度作成してからそのサイトの対象を複数のデバイスおよびブラウザーに設定することができます。Web 開発コミュニティでは、通常、レスポンシブ デザインと "流動型グリッド" アプローチを使用して、レイアウトがレンダリングされる方法を管理したり、任意のブラウザーやデバイスで正しくレンダリングされるようにサイトを設計したりします。レスポンシブ デザインでは、ユーザーのデバイスと画面方向に合うようにページの要素自体が再調整されます。

レスポンシブ デザインは、CSS3 のメディア クエリ機能に基づいています。メディア クエリを使用してデバイスのディスプレイの幅と一致させ、クライアント側のスタイルを適用してコンテンツをレンダリングします。メディア クエリによって、デザイナーは画面の幅などの特定のサイト プロパティを対象とすることができます。メディアのクエリを使用して、柔軟なレイアウトと画像を作成することができます。条件によっては代わりに CSS ファイルを呼び出すこともできます。

詳細および例については、「 [レスポンシブ SharePoint](http://responsivesharepoint.codeplex.com/)」および「 [SharePoint 2013 でレスポンシブ デザインを実装する](http://blogs.msdn.com/b/sharepointdev/archive/2013/04/01/implementing-your-responsive-designs-on-sharepoint-2013.aspx)」を参照してください。

### アダプティブ デザイン

アダプティブ Web デザイン ("アダプティブ Web 配信" とも呼ばれます) は、レスポンシブ Web デザインに似ています。アダプティブ デザインでは、デバイスまたはブラウザーをリッスンし、ページのレンダリングに最適な方法を選択します。 

SharePoint 2013 のデバイス チャネル機能は、アダプティブ デザインの 1 つです。ページ レイアウト、それぞれのデバイス チャネルの仕様、およびデバイス チャネル パネルで定義されている順序に基づいて、各デバイスに適応性のあるレイアウトを提供します。 

### デバイス チャネルとレスポンシブ デザインの併用

デバイス チャネルとレスポンシブ Web デザインの手法を併用して、レスポンシブな一般向け SharePoint 発行サイトを作成することができます。電話やタブレットなどのデバイス用に 1 つのカスタム マスター ページ、Web ブラウザー用に別のカスタム マスター ページを作成し、それぞれをデバイス チャネルに関連付けます。次に、流動型グリッド、柔軟性のあるイメージ、および CSS3 メディア クエリを使用して、サイトがサポートする必要のある各デバイスやブラウザー向けの最良の表示エクスペリエンスを作り上げます。

## jQuery を SharePoint サイトに追加する

jQuery を SharePoint サイトに追加できます。サイト レベルやページ レベルで行うこともできますし、ページのセクション内 (SharePoint ページ領域の 1 つまたはページ レイアウトに追加した Web パーツなど) で行うこともできます。

カスタム アクションを使用して、ドキュメント ライブラリから jQuery を読み込むことができます。SharePoint サイト内のすべてのページで jQuery を使用できるようにする必要がある場合にこれを実行します。このアプローチには柔軟性がありますが、制御は簡単ではありません。また、サイト デザイナーと管理者にも影響します。JavaScript ファイルをドキュメント ライブラリに格納して管理できますが、誤って変更または削除される可能性もあります。そのため、このアプローチはお勧めしません。

[ScriptLinkControl](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.webcontrols.scriptlink.aspx) を使用して、SharePoint ルートから jQuery を読み込むこともできます。コントロールを使用してリモート サイトで実行されるスクリプトを挿入したり、SharePoint インストールに触れることなくスクリプトを変更したりできます。 **ScriptLinkControl** アプローチは、アプリケーション ページで、またはページに表示される Web パーツで jQuery を使用する場合に意味があります。jQuery は一度に 1 ページに追加されるため、このオプションのプロビジョニングには時間が掛かりパフォーマンスにも影響を与えますが、jQuery ファイルを SharePoint ルールに展開すると、その他の従来の要件が回避されます。これは、SharePoint 2013 の完全に信頼できるコード (FTC) のソリューションを CSOM に移行する必要がある場合、また移行にカスタム JavaScript と jQuery 動作の移動とリファクタリングが含まれる場合に役立ちます。

最後に、コンテンツ エディター Web パーツを使用して、コンテンツ配信ネットワーク (CDN) から jQuery を読み込むことができます。これは、1 から数ページに jQuery を追加する場合に便利であり、これには Wiki や Web パーツのページが含まれます。CDN から jQuery ファイルを読み込むため、SharePoint サーバーの余分なファイルを格納する必要がなく、ユーザーは jQuery ファイルの分散されたキャッシュ バージョンの利点を享受できます。SharePoint は CDN から jQuery ファイルを呼び出します。自分で作成したカスタム jQuery コードを、コンテンツ エディタ Web パーツに追加できます。 

## SharePoint プロバイダー ホスト型アドインを ASP.NET MVC 5 で作成する

SharePoint では、カスタム プロバイダー ホスト型アドインをモデル ビュー コントローラー (MVC) パターンで作成できます。このモデルでは、アプリケーションは相互に接続される 3 つの部分に分けられます。これによって、情報の内部表現と、ユーザーがそれを表示する方法と、ユーザーがそれを受け入れる方法とが分離されます。このモデルは、ソフトウェアの基盤となる構造、ビュー (通常 UI 要素)、およびコントローラーを表し、このそれぞれはモデルとビューを接続するクラスです。 

ASP.NET MVC コンテンツを SharePoint サイト マスター ページ コンテンツでラップすることができます。実際に Office 365 API を使用して、SharePoint 2013 アドインを ASP.NET MVC 5 で作成できます。 

MVC 向け API (SharePoint 開発用) は、 `Filters\SharePointContextFilterAttribute.cs` と `SharePointContext.cs` で定義されます。これらの API は、SharePoint とシームレスに通信するために Web プロジェクトが実行する手順を 1 回の呼び出しにラップします。これにより、実装する必要のあるロジックが簡略化されます。

SharePoint からリモート Web アプリケーション (ホスト Web URL など) にリダイレクトされた場合、SharePoint コンテキスト フィルター属性は標準的な情報を取得する追加の処理を実行します。また、ユーザーがサインインする場合 (ブックマークなど) にアドインを SharePoint にリダイレクトする必要があるかどうかを判別します。コントローラーまたはビューのいずれかにこのフィルターを適用できます。アドイン Web とホスト Web 用に特定のコンテキストを作成し、SharePoint と通信できるように、SharePoint コンテキスト クラスは SharePoint からのすべての情報をカプセル化します。

詳細については、「[ASP.NET MVC の概要](http://www.asp.net/mvc/overview)」および「[SharePoint アドイン用の MVC サポートの概要](http://blogs.msdn.com/b/officeapps/archive/2013/07/09/introducing-mvc-support-for-apps-for-sharepoint.aspx)」を参照してください。

## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint 2013 および SharePoint Online のブランディングおよびサイト プロビジョニング ソリューション](Branding-and-site-provisioning-solutions-for-SharePoint.md)
    
- [SharePoint 2013 でページ フィールドにスタイルを適用する方法](http://msdn.microsoft.com/library/e227613d-0e4d-4312-924d-bb55e1fe4293%28Office.15%29.aspx)
    
- [[方法]: SharePoint 2013 でページ レイアウトを作成する方法](http://msdn.microsoft.com/library/5447e6a1-2f14-4667-81d0-7514b468be80%28Office.15%29.aspx)
    
- [SharePoint 2013 でカタログベースのサイトのページ レイアウトをカスタマイズする方法](http://msdn.microsoft.com/library/21d8db99-73b3-4429-b6cb-04e375af9f55%28Office.15%29.aspx)
    
- [[方法]: SharePoint 2013 マスター ページ ギャラリーへのネットワーク ドライブの割り当て](http://msdn.microsoft.com/library/7d416f6e-2471-4d03-97ae-4e8d907784c6%28Office.15%29.aspx)
