# SharePoint 2013 と SharePoint Online での検索ソリューション

SharePoint の検索アーキテクチャ、検索 API、および検索アドインについて説明します。

_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

SharePoint 2013 での検索は、単一のエンタープライズ検索プラットフォームで、構成および展開の容易さを FAST Search Server のスケーラビリティおよび拡張性と同時に実現しました。

SharePoint 2013 には、さまざまなシナリオに合わせて検索をカスタマイズするのに役立つ、検索プラットフォームの共通のパターンが含まれます。 次に例を示します。

- 動画検索と会話検索が、設定なしですぐに使用できる[バーティカル検索](https://technet.microsoft.com/en-us/library/dn794227.aspx)として含まれています。
    
- トピック ページと検索によるコンテンツによって、Web コンテンツ管理機能と検索型サイトやナレッジ マネージメント サイトなどのシナリオが強化されます。
    
- マイ タスクによってプロジェクト タスクがまとめられ、複数のサイトで割り当てられたタスクをユーザーが 1 つの場所 (OneDrive for Business サイト) から追跡できるようになります。

## SharePoint 2013 検索アーキテクチャ

SharePoint 2013 の検索アーキテクチャには、連携して動作するコンポーネントとデータベースが含まれます。 

**SharePoint 2013 の検索コンポーネント**

|**コンポーネント**|**説明**|
|:-----|:-----|
|クロール |プロパティとメタデータを収集するためにコンテンツ ソースをクロールし、その情報をコンテンツ処理コンポーネントに送信します。|
|コンテンツ処理 |クロールされたアイテムを変換し、インデックス コンポーネントに送信します。また、このコンポーネントはクロールされたプロパティから管理プロパティへのマップも行います。|
|分析処理 |検索分析と利用状況分析を実行します。|
|インデックス |コンテンツ処理コンポーネントから処理済みのアイテムを受信し、その情報を検索インデックスに書き込みます。このコンポーネントは、受信クエリの処理、検索インデックスからの情報の取得、クエリ処理コンポーネントへの結果セットの返信なども実行します。|
|クエリ処理 |受信クエリを分析します。これにより、精度、再現率、関連性が最適化されます。クエリはインデックス コンポーネントに送信され、ここからクエリの検索結果のセットが返されてきます。|
|検索管理 |検索用のシステム処理を実行し、検索コンポーネントの新しいインスタンスの追加と初期化を行います。|

**SharePoint 2013 の検索データベース**

|**データベース**|**説明**|
|:-----|:-----|
|クロール |ドキュメントや URL などのクロールされたアイテムに関する追跡情報と履歴情報を格納します。また、最終クロール時刻、最終クロール ID、最終クロール時の更新の種類 (追加、更新、削除) などの情報も格納します。|
|リンク |コンテンツ処理コンポーネントによって抽出された未処理の情報と検索クリックに関する情報を格納します。分析処理コンポーネントは、この情報を分析します。|
|分析レポート |使用状況分析の結果を格納します。|
|検索管理 |検索構成データを格納します。|

### クロールとコンテンツの処理

クロール処理は、さまざまなソースのコンテンツ (たとえば、HTTP、ファイル共有、SharePoint など) で開始されます。コンテンツをインデックスに追加するため、クローラーは、コンテンツ ソースに接続する方法や、ソース内のコンテンツ アイテムにアクセスする方法をクローラーに示すコネクタを使用します。クローラーはコンテンツ アイテムを検出すると、適切な形式のハンドラーを使用してコンテンツを解析します。

クロール コンポーネントはコンテンツを取得した後、クロールされたアイテムをコンテンツ処理コンポーネントに渡します。すると、アイテムはそこで処理され、インデックス コンポーネントに送信されます。これには、ドキュメントの解析、クロールされたプロパティを関連する管理プロパティにマッピングすること、言語の検出やエンティティの抽出などの言語処理が含まれます。また、コンテンツ処理コンポーネントはリンクと URL に関する情報をリンク データベースに書き込みます。

### クエリ処理

クエリ処理コンポーネントは、検索クエリを分析して処理し、精度、再現率、および関連性を最適化します。これには、単語分割やステミングなどの言語処理の実行も含まれます。処理されたクエリはインデックス コンポーネントに送信されます。インデックス コンポーネントは処理されたクエリに基づいて結果セットをクエリ処理コンポーネントに返し、クエリ処理コンポーネントはその結果セットを処理します。

### 検索分析

SharePoint は、コンテンツ自体 (検索分析) とユーザーがコンテンツとやりとりする方法 (利用状況分析) の両方を分析し、この情報を活用して検索を向上させます。

検索分析とは、リンク、アイテムがクリックされた回数、アンカー テキスト、人に関連付けられたデータ、メタデータなどの情報をリンク データベースから抽出することに関するものです。検索分析は関連性を判断するための基盤となります。一方、利用状況分析は、イベント ストアを経由してフロント エンドから受信した利用状況ログ情報を分析することに関するものです。利用状況分析は使用状況と統計のレポートの基盤になります。

分析の結果は、検索インデックスのアイテムに追加されます。さらに、使用状況分析の結果は、分析レポート データベースに格納されます。

## 検索機能をカスタマイズするための構成要素

SharePoint 2013 および SharePoint Online での検索には、検索機能をカスタマイズできる新機能と機能強化が含まれています。多くの機能強化では、コードを記述する必要はありません。SharePoint 検索には、カスタマイズするためにコードを記述する必要がある場合、またはアドインを作成して SharePoint 外部の SharePoint の検索結果にアクセスする場合に役立つ CSOM と REST API が含まれています。

### 検索センター サイト

検索センターは、検索のために設定する SharePoint サイトです。このサイトは、組織のイントラネット上のコンテンツを検索できるポータルとなり、容易にカスタマイズできる一元的なユーザー インターフェイスを提供します。この記事では、検索センターのページと Web パーツについて説明するとともに、コードをあまり書かずにカスタム検索機能を作成したり、またカスタム検索アプリケーションを作成したりするために変更できる検索構成設定について説明します。

検索センターのサイトを作成すると、SharePoint は既定の検索ホーム ページと既定の検索結果ページを作成します。さらに、バーティカル検索と呼ばれるいくつかのページも作成されます。バーティカル検索は、人や動画など特定のコンテンツ タイプを検索するためにカスタマイズされた検索結果ページで、これらのページには、特定のコンテンツ タイプまたはクラスに合わせてフィルター処理および書式設定された検索結果が表示されます。

以下のページがページ ライブラリの検索センター サイト コレクションに作成されます。

- **default.aspx** - 検索センターのホームページであり、エンド ユーザーがクエリを入力するページ。
    
- **results.aspx** - 検索センターの既定の検索結果ページ。 [ **すべて** ] バーティカル検索の検索結果ページでもあります。
    
- **peopleresults.aspx** - [ **人** ] バーティカル検索の検索結果ページ。
    
- **conversationresults.aspx** - [ **会話** ] バーティカル検索の検索結果ページ。
    
- **videoresults.aspx** - [ **動画** ] バーティカル検索の検索結果ページ。
    
- 
            **advanced.aspx** - エンド ユーザーが検索語句に制限を適用できる検索ページ。たとえば、完全に一致する語句に検索を制限します。
    
すべてのバーティカル検索ページには、検索結果 Web パーツが含まれますが、Web パーツはバーティカル検索ごとに異なる方法で構成されます。それぞれのバーティカル検索では、検索結果 Web パーツのクエリが、そのバーティカル検索に適用可能な特定の検索先に送られます。たとえば、peopleresults.aspx ページの検索結果 Web パーツのクエリは、[ローカルのひとの結果] の検索先に制限されています。SharePoint 2013 での既定のバーティカル検索の構成方法について理解していると、独自のバーティカル検索を作成したり、検索センターをカスタマイズしたりするのに役立ちます。

検索センターを使用するのに役立つその他のリソースを以下に示します。

- [SharePoint 2013 での検索センターのセットアップ](http://technet.microsoft.com/en-us/library/dn794206%28v=office.15%29.aspx)
    
- [SharePoint 2013 で検索センター サイト コレクションを作成してコンテンツのクロールを有効にする方法](http://technet.microsoft.com/en-us/library/dn794245%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 で検索センター サイトを作成する](http://technet.microsoft.com/en-us/library/hh582314%28v=office.15%29.aspx)
    
- [検索センターを管理する](http://technet.microsoft.com/en-us/library/jj851144%28v=office.15%29.aspx)
    
- [SharePoint Online で検索センターを管理する](http://office.microsoft.com/en-us/office365-sharepoint-online-enterprise-help/manage-the-search-center-in-sharepoint-online-HA103994122.aspx)

### 検索センターの Web パーツ

検索センターのページには、検索ボックス、検索結果、検索ナビゲーション、絞り込みの 4 種類の Web パーツがあります。 

#### 検索ボックス Web パーツ

検索ボックス Web パーツには、ユーザーが検索するテキストを入力するテキスト ボックスが表示されます。既定では、検索ボックス Web パーツは、検索センターのホーム ページ (default.aspx) およびすべての既定の検索結果ページ (results.aspx、peopleresults.aspx、conversationresults.aspx、videoresults.aspx) で使用されます。

Web パーツのツール ウィンドウでプロパティを編集することによって、検索ボックス Web パーツをカスタマイズできます。これにより、次のようなことが可能になります。

- 検索結果が表示される場所を変更します。たとえば、カスタム検索結果 Web パーツまたはカスタム検索結果ページに結果を表示できます。
    
- クエリ候補や人の候補を無効にします。
    
- 検索設定ページと高度な検索ページへのリンクを表示します。
    
- Web パーツの表示テンプレートを変更します。
    
詳細については、「 [SharePoint Server 2013 で検索ボックス Web パーツのプロパティを構成する](http://technet.microsoft.com/en-us/library/gg576963%28v=office.15%29.aspx)」および「 [SharePoint Server 2013 の検索ボックス Web パーツに表示されるテキストを変更する方法](http://technet.microsoft.com/en-us/library/dn794238%28v=office.15%29.aspx)」を参照してください。

#### 検索結果 Web パーツ

検索結果 Web パーツには、検索クエリの結果が表示されます。既定では、すべての既定のバーティカル検索ページ (results.aspx、peopleresults.aspx、conversationresults.aspx、videoresults.aspx) で、検索結果 Web パーツが使用されます。検索結果 Web パーツは、絞り込み Web パーツや検索ナビゲーション Web パーツにも検索結果を送信するため、他の検索 Web パーツが機能できるよう、検索結果ページに検索結果 Web パーツが存在している必要があります。

Web パーツ ツール ウィンドウで検索結果 Web パーツのプロパティを編集することによって、検索クエリを変更したり、検索結果ページの動作や結果の外観を変更したりできます。プロパティの値を変更することによって、次の操作を実行できます。

- 検索先を変更して検索対象となるコンテンツを指定します。
    
- クエリ変数やプロパティ フィルターを追加し、さまざまなユーザーまたはユーザー グループの検索結果をカスタマイズします。
    
- 検索結果内のアイテムまたはページを昇格または降格します。
    
- 検索結果の並べ替えを変更します。
    
- 表示テンプレートを変更します。
    
検索結果 Web パーツの詳細については、「 [SharePoint Server 2013 の検索結果 Web パーツのプロパティを構成する](http://technet.microsoft.com/en-us/library/gg549987.aspx)」および「 [SharePoint 2013 で新しい検索先を使用するように検索結果 Web パーツを構成する方法](http://technet.microsoft.com/en-us/library/dn794200%28v=office.15%29.aspx)」を参照してください。

#### 検索ナビゲーション Web パーツ

検索ナビゲーション Web パーツには、別のバーティカル検索 (すべて、人、会話、および動画) との間をユーザーがすばやく移動できるようにするためのリンクが表示されます。検索ナビゲーション Web パーツは検索結果 Web パーツからの検索結果を使用し、ユーザーがバーティカル検索リンクを選択したら、検索結果がフィルター処理されて、バーティカル検索の設定に従って表示されるようにします。Web パーツのツール ウィンドウで検索ナビゲーション Web パーツのプロパティを編集することによって、次のように Web パーツをカスタマイズできます。

- 結果を取得する別の Web パーツを指定します。
    
- 表示するバーティカル検索リンクの数を変更します。
    
- Web パーツの外観とレイアウトを変更します。
    
また、リボンで **[サイト設定]** > **[検索の設定]** を選択して以下を変更することもできます。

- リンクの表示名を変更します。
    
- リンクの順序を変更します。

#### 絞り込み Web パーツ

絞り込み Web パーツは、絞り込み条件と呼ばれるカテゴリで検索結果をフィルターに掛けます。ユーザーは絞り込み条件を選択して、検索結果を絞り込むことができます。絞り込み条件は、 _Refinable_ および _Queryable_ としてマークが付けられる管理プロパティです。これらの設定については、「 [SharePoint Server 2013 の検索スキーマの概要](http://technet.microsoft.com/en-us/library/jj219669%28v=office.15%29.aspx)」を参照してください。Web パーツ ツール ウィンドウで絞り込み Web パーツのプロパティを編集して、以下を指定できます。

- 検索結果をフィルターに掛ける検索結果 Web パーツ。
    
- 絞り込み Web パーツで使用する絞り込み条件。
    
- 各絞り込み条件に適用する表示テンプレート。
    
- 絞り込み Web パーツの外観、レイアウト、および動作。
    
既定では、絞り込み Web パーツに各絞り込み条件値の結果の数は表示されません。絞り込み条件の表示テンプレートを変更することによって、絞り込み条件カウントを追加できます。この機能の詳細については、「 [SharePoint Server 2013 の絞り込み Web パーツのプロパティを構成する](http://technet.microsoft.com/en-us/library/gg549985%28v=office.15%29.aspx)」を参照してください。

絞り込み Web パーツおよび絞り込み条件の詳細については、「 [SharePoint 2013 の検索結果ページで絞り込み条件を使用することを計画する](http://technet.microsoft.com/en-us/library/dn794223%28v=office.15%29.aspx)」および「 [SharePoint 2013 で検索結果ページに絞り込み条件を追加する方法](http://technet.microsoft.com/en-us/library/dn794243%28v=office.15%29.aspx)」を参照してください。

### 検索先

検索先は、特定のコンテンツ、または検索結果のサブセットに検索を制限します。以下を指定することによって、検索先を定義できす。

- 検索結果を取得する検索プロバイダーまたはソース URL。たとえば、ローカル SharePoint 検索サービスの検索インデックス。
    
- 検索結果を取得するために使用するプロトコル。たとえば、 [OpenSearch](http://www.opensearch.org/Home) プロトコル。
    
- クエリの変換。特定の検索プロバイダー または URL からの結果を、特定の結果のサブセット (たとえば、特定のコンテンツ タイプを持つ結果のセット) に絞り込むことができます。
    
SharePoint 2013 では、ローカル SharePoint の結果、会話、および現在のユーザーに関連するアイテムなど 16 個の事前構成された検索先が提供されています。 **[検索先の管理]** ページで、検索先についての詳細を表示できます。 (**[サイト設定]** > **[検索]** > **[検索先]**)。 その後、**[検索先の管理]** ページでは、次のいずれかの方法で新しい検索先を作成できます。

- **[新しい検索先]** を選択して、目的の検索先を選択します。 (詳細については、「[SharePoint Server 2013 で検索先を構成する](http://technet.microsoft.com/en-us/library/jj683115%28v=office.15%29.aspx)」を参照してください)。
    
- 既存の検索先の横にある矢印をポイントして **[コピー]** を選択し、必要に応じてコピーを変更してから新しい名前で保存します。
    
検索先には検索結果を取得する 4 つのプロトコルのいずれかを指定します。 検索先が **[ローカルの SharePoint]** 以外のプロトコルを使用する場合、検索先には検索結果の取得先の URL も指定する必要があります。

**検索先プロトコルとそのプロバイダー**

|**検索先プロトコル**|**プロバイダー**|**URL**|
|:-----|:-----|:-----|
|ローカル SharePoint|ローカル検索サービスの検索インデックス|なし。|
|リモート SharePoint|別のファームにホストされている検索サービスの検索インデックス。|リモート SharePoint ファームのルート サイト コレクションのアドレス。 |
|OpenSearch 1.0/1.1|OpenSearch プロトコルを使用して検索結果を提供する外部検索プロバイダー (リモート検索エンジンやフィードなど)。|OpenSearch プロトコルを使用する検索プロバイダーの RSS フィードの URL。|
|Exchange|Exchange Web サービス (EWS)。|EWS URL。|

詳細については、以下を参照してください。

- [SharePoint Server 2013 の検索に関する結果ソースの概要](http://technet.microsoft.com/en-us/library/dn186229%28v=office.15%29.aspx)
    
- [検索先とフェデレーションについて](http://technet.microsoft.com/en-us/library/jj219577%28v=office.15%29.aspx#Section12)
    
- [検索先について](http://office.microsoft.com/en-us/sharepoint-server-help/understanding-result-sources-HA102848849.aspx?CTT=1)
    
- [検索先を管理する](http://office.microsoft.com/en-us/office365-sharepoint-online-enterprise-help/manage-result-sources-HA103639370.aspx)

### クエリ ルール

ユーザーにとって特に重要なクエリの検索機能をカスタマイズするにはクエリ ルールを使用します。クエリ ルールでは、コンテキスト、条件、関連するアクションを指定します。すると、指定したコンテキストで、かつクエリが指定した条件を満たす場合に、検索は相関性のあるアクションを実行し、検索結果の関連性が向上します。

コンテキストに関しては、クエリ ルールのクエリを以下のものに制限できます。 

- 指定した検索先に対して実行されるクエリ。
    
- 指定されたトピック カテゴリからのクエリ。
    
- 指定したユーザー セグメントに一致するユーザーによって実行されたクエリ。
    
次の表に、クエリ ルールの実行条件として指定可能なものを示します。

**クエリ ルール条件**

|**条件**|**説明**|
|:-----|:-----|
|クエリがキーワードに正確に一致する|クエリが指定した単語や語句に正確に一致する場合に、クエリ ルールを適用します。|
|クエリにアクション用語が含まれている|ユーザーが何かを実行しようとしていることを示す 1 つの単語または語句の形で用語がクエリに含まれている場合に、クエリ ルールを適用します。用語はクエリの先頭または末尾にある必要があり、動詞、コマンド、またはフィルターを指定できます。|
|クエリが辞書に正確に一致する|クエリが辞書エントリと正確に一致する場合に、クエリ ルールを適用します。このエントリは、用語ストアにある用語か、または人名辞書のエントリです。 |
|ソースでよく使用されるクエリ|ユーザーのクエリが現在の検索先とは別の検索先に対してよく実行される場合に、クエリ ルールを適用します。この条件では、ユーザーがさまざまな検索先で入力したクエリの分析が使用されます。|
|よくクリックされる検索結果の種類|特定の検索結果の種類の結果をユーザーが選択することが多いクエリの場合に、クエリ ルールを適用します。新しい検索結果の種類を作成するときに、クエリ ルールで使用するためにこれらの選択肢を記録するように指定できます。|
|高度なクエリ テキスト照合|クエリが正規表現と一致する場合に、クエリ ルールを適用します。前述のキーワード、アクション用語、辞書の条件のバリエーションを使用することもできますが、より高度な制御が可能です。|

クエリ ルールは、次の 3 つの種類の操作を指定できます。

- ランク付けされた結果の上に表示される「**昇格した結果**」 (以前は「**おすすめコンテンツ**」と呼ばれていました) を追加します。たとえば、「病欠」というクエリの場合、クエリ ルールには、休暇に関する会社のポリシーの声明が記載されたサイトへのリンクなど、特定の昇格した結果を指定できます。
    
- 「結果ブロック」と呼ばれる結果の 1 つ以上のグループを追加します。結果ブロックには、特定の方法でクエリに関連付けられている結果の小さいサブセットが含まれています。個々 の検索結果のように、結果ブロックを昇格させたり、他の検索結果と共にランク付けしたりできます。 
    
- クエリを変更することにより、結果のランク付けを変更します。たとえば、「ダウンロード ツールボックス」を含むクエリの場合、クエリ ルールはアクション用語として「ダウンロード」という単語を認識して、イントラネット上の特定のダウンロード サイトをポイントする検索結果を昇格させることができます。
    
クエリ ルールの詳細については、「 [SharePoint Server 2013 でクエリ ルールを管理する](http://technet.microsoft.com/en-us/library/jj871676.aspx)」を参照してください。

### クエリの変換

ユーザーのクエリに対して適切な検索結果を提供するために、クエリを変更する必要が生じる場合があります。クエリの変換でこれに対応します。動画、人、会話など、SharePoint 2013 に含まれている既定のバーティカル検索すべてには、そのバーティカル検索の検索機能を最適化するために事前定義されたクエリの変換が用意されています。

次の 3 つの場所で、クエリ変換を構成できます。

- 検索結果 Web パーツなどの Web パーツ。 
    
- クエリ ルール。これは、特定の条件が満たされた場合にのみ、特定のアクションを実行することを指定します。 
    
- クエリが検索結果を取得するために使用する検索先。
    
ユーザー クエリは、最初に Web パーツによって、次に適用されるクエリ ルールによって、最後に検索先によって変換されます。検索先で変換を構成する場合、検索先は最後にクエリを変換するので、変換の変更が破棄されたり、上書きされたりすることはありません。検索先のクエリ変換は Web パーツや結果のブロックで再利用でき、特定の検索先からの結果にのみ適用されるクエリ ルールまたは検索結果の種類を作成できます。

クエリ変換を作成してテストするために、クエリ ビルダーを使用することができます。クエリ変数に一時的なテストの値を設定し、クエリを実行して、検索結果をプレビューすることによって、クエリ ビルダー内からクエリをテストできます。クエリ変換の詳細については、「 [SharePoint 2013 でクエリの変換と注文の結果を計画する](http://technet.microsoft.com/en-us/library/60a1110d-27dc-45d0-86e2-cddc72d072b2%28v=office.15%29)」を参照してください。

### 検索結果の種類と表示テンプレート

SharePoint 2013 の検索には、検索結果の表示方法をカスタマイズしやすくする新しい結果フレームワークが含まれています。検索結果の表示方法を変更するためにカスタム XSLT を作成する代わりに、表示テンプレートと検索結果の種類を使用して、検索結果の重要な種類の表示形式をカスタマイズできます。

#### 検索結果の種類

検索結果を異なる方法で表示するには、検索結果を異なる検索結果の種類に分類する必要があります。検索結果の種類は、1 つの検索結果を別の検索結果から区別する検索結果の分類です。これは、以下のコレクションによって構成されます。

- **ルール** - 検索結果の検索先やコンテンツ タイプなどの各検索結果を比較する対象となる、1 つ以上の特性または条件。等号、比較、論理演算子を使用してルール条件を結合できます。
    
- **プロパティ** - 検索結果用の管理プロパティのリスト。管理プロパティを表示 テンプレートにマップする前に、管理プロパティをプロパティ リストに追加する必要があります。
    
- **表示テンプレート** - 条件を満たすすべての結果が検索結果ページで表示される方法および動作する方法を制御します。
    
SharePoint 検索には、いくつかの既定の結果の種類が含まれています。 それらを表示するには、**[サイト設定]** > **[サイト コレクションの管理]** > **[検索結果の種類]** に移動します。 既定の結果の種類は、いずれも編集できません。 既定の結果の種類をコピーし、それを編集することで、新しい検索結果の種類を作成してください。 SharePoint 2013 に含まれる既定の結果の詳細については、「[SharePoint Server 2013 で検索結果の表示に使用される検索結果の種類と表示テンプレート](http://technet.microsoft.com/en-us/library/dn386874%28v=office.15%29.aspx)」を参照してください。

#### 表示テンプレート

表示テンプレートは、表示レイアウトと検索結果の動作を定義します。これは、どの管理プロパティが検索結果に表示されるか、またそれらがどのように表示されるかを制御します。SharePoint は、マスター ページ ギャラリー内の Display Templates フォルダーの Search サブフォルダーに表示テンプレートを格納します。各表示テンプレートは、次の 2 つのファイルで構成されます。 

- HTML エディターで編集できる表示テンプレートの HTML バージョン。
    
- SharePoint が使用する .js ファイル。 
    
表示テンプレートを使用する場合は、HTML ファイルを変更します。.js ファイルは、SharePoint によって作成され、変更されます。このファイルの編集は行いません。

表示テンプレートには、以下の 2 つの主要な種類があります。

- **コントロール表示テンプレート** - 検索結果が表示される方法の全体的な構造を決定します。
    
- **アイテム表示テンプレート** - セット内の各検索結果が表示される方法を決定します。
    
コントロール表示テンプレートは、検索結果を表示する方法の全体的なレイアウトを構成する HTML を提供します。たとえば、コントロール表示テンプレートには、見出しおよびリストの始まりと終わりの HTML が用意されている場合があります。コントロール表示テンプレートは、Web パーツで一度だけレンダリングされます。

アイテム表示テンプレートには、結果セット内の各アイテムを表示する方法を決定する HTML があります。たとえば、アイテム表示テンプレートは、画像、およびそのアイテムに関連付けられたさまざまな管理プロパティにマップされる 3 行のテキストを含んだリスト アイテムの HTML を提供するかもしれません。アイテム表示テンプレートは、結果セット内の各アイテムに対して 1 回レンダリングされます。したがって、結果セットに 10 個のアイテムが含まれる場合、アイテム表示テンプレートは HTML のそのセクションを 10 回作成します。

表示テンプレートとその構造の詳細については、「 [SharePoint 2013 デザイン マネージャー表示テンプレート](http://msdn.microsoft.com/en-us/library/jj945138.aspx)」および「 [SharePoint 2013 ページ モデルの概要](http://msdn.microsoft.com/en-us/library/jj191506.aspx)」の「 [検索型 Web パーツおよび表示テンプレート](http://msdn.microsoft.com/en-us/library/jj191506.aspx)」を参照してください。

SharePoint 2013 で使用可能な表示テンプレートの詳細については、「 [SharePoint Server 2013 の表示テンプレート リファレンス](http://technet.microsoft.com/en-us/library/jj944947.aspx)」を参照してください。

#### 表示テンプレートのカスタマイズ

SharePoint に含まれている表示テンプレートをカスタマイズする場合は、変更する表示テンプレートの内容をコピーして新しい表示テンプレートを作成し、それから新しいバージョンをカスタマイズします。既存の表示テンプレートのコピーから開始する方法は、必要なすべての要素がそろった状態で開始できるため、新しい表示テンプレートを作成する最も簡単な方法です。

表示テンプレートを操作する際のもう 1 つのヒントは、ネットワーク ドライブをマスター ページ ギャラリーにマップすることです。詳細については、「[[方法]: SharePoint 2013 マスター ページ ギャラリーへのネットワーク ドライブの割り当て](https://msdn.microsoft.com/en-us/library/office/jj733519.aspx)」を参照してください。

表示テンプレートに使用される HTML ファイルは、 `head` と `body` 要素を備えた完全に形成された HTML ドキュメントです。 `head` 要素内には、表示テンプレートの表示名を指定する `title` 要素があります。このタグ内のテキストは、SharePoint UI で構成を実行する際 (たとえば、検索結果の種類の構成時) に表示される内容になります。

`title` 要素の後に、カスタム ドキュメント プロパティの要素 (`mso:CustomDocumentProperties`) があります。 アイテム表示テンプレートでは、この要素は `mso:ManagedPropertyMapping` 要素を含んでいます。ここで SharePoint の検索によって使用される管理プロパティが、表示テンプレートで使用される値にマッピングされます 次の例に示すように、このために使用される構文は `<display template reference name>:<managed property name>` です。

```HTML
<mso:ManagedPropertyMapping msdt:dt="string">'Title':'Title','Path':'Path','Description':'Description'
```

`body` 要素内には `script` 要素があります。この要素には、表示テンプレートの外部にある、CSS ファイルや JavaScript ファイルなどの外部リソースを含めることができます。 script 要素に外部リソースを含める方法の例は、「[SharePoint 2013 デザイン マネージャー表示テンプレート](http://msdn.microsoft.com/en-us/library/jj945138.aspx)」の「スクリプト ブロック」セクションを参照してください。

次の要素は  `div` 要素です。これは、表示テンプレートの一部として使用する HTML またはスクリプトを配置する場所です。表示テンプレートの構造を理解する良い方法は、検索結果の既定の表示テンプレートである Control_SearchResults.html (コントロール表示テンプレート) および Item_Default.html (アイテム表示テンプレート) のコピーをダウンロードすることです。

#### 検索結果の種類と表示テンプレートに関するその他のリソース

表示テンプレートと検索結果の種類に関するその他のリソースを次に示します。

- [SharePoint 2013 で検索結果の種類をカスタマイズする](http://technet.microsoft.com/en-us/library/dn135239%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 で検索結果の表示方法を変更する方法](http://technet.microsoft.com/en-us/library/dn794204%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 での検索結果の表示の概要](http://technet.microsoft.com/en-us/library/dn794212%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 でのアイテム表示テンプレートと検索語句の強調表示の機能を理解する](http://technet.microsoft.com/en-us/library/dn794249%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 で新しい結果の種類を作成する方法](http://technet.microsoft.com/en-us/library/dn794234%28v=office.15%29.aspx)
    
- [検索結果でカスタム管理プロパティの値を表示する方法 - オプション 1 (SharePoint Server 2013)](http://technet.microsoft.com/en-us/library/dn794209%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 でカスタム管理プロパティの値を検索結果で表示する方法 - オプション 2](http://technet.microsoft.com/en-us/library/dn794240%28v=office.15%29.aspx)
    
- [SharePoint Server 2013 でホバー パネルにカスタム管理プロパティの値を表示する方法](http://technet.microsoft.com/en-us/library/dn794247%28v=office.15%29.aspx)

## クエリ API と検索アドイン

SharePoint 2013 の検索には, .NET と JavaScript のクライアント オブジェクト モデルに加えて、オンライン、オンプレミス、およびモバイル開発の検索結果にアクセスできるようにする REST サービスが含まれます。 

**検索クエリ API**

|**API**|**クラス ライブラリまたはスキーマ パス**|**例**|
|:-----|:-----|:-----|
|.NET CSOM|Microsoft.SharePoint.Client.Search.dll[SharePoint Server 2013 クライアント コンポーネント SDK のダウンロード](http://www.microsoft.com/en-us/download/details.aspx?id=35585)%ProgramFiles%\Common Files\Microsoft Shared\web server extensions\15\ISAPI[SharePoint SharePoint Online クライアント コンポーネント SDK のダウンロード](http://www.microsoft.com/en-us/download/details.aspx?id=42038)%ProgramFiles%\Common Files\Microsoft Shared\web server extensions\16\ISAPI|[SharePoint 2013: 管理クライアントでのクエリ検索](http://code.msdn.microsoft.com/Query-Search-with-the-649f1bc1)[オブジェクト モデル (コード ギャラリ)](http://code.msdn.microsoft.com/Query-Search-with-the-649f1bc1)|
|JavaScript CSOM|SP.search.js%ProgramFiles%\SharePoint Client Components\Scripts|[JavaScript クライアント オブジェクト モデルでの検索クエリ](http://code.msdn.microsoft.com/SharePoint-2013-Querying-a629b53b) (コード ギャラリ)|
|REST サービス|http://server/_api/search/queryhttp://server/_api/search/postqueryhttp://server/_api/search/suggest|[SharePoint 2013: アドインから検索 REST サービスを使用する](http://code.msdn.microsoft.com/sharepoint/SharePoint-2013-Perform-a-1bf3e87d) (コード ギャラリ)|

### 検索クエリ .NET CSOM

クエリ .NET CSOM を使用するには、 [T:Microsoft.SharePoint.Client.ClientContext](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.clientcontext.aspx) クラスの新しいインスタンスを作成します。これは、Microsoft.SharePoint.Client.dll の [Microsoft.SharePoint.Client](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.aspx) 名前空間にあります。次いで、 [Microsoft.SharePoint.Search.Client.Query](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.search.query.aspx) 名前空間のクエリ オブジェクト モデルを使用します。次に簡単な例を示します。

```C#
using Microsoft.SharePoint.Client; 
using Microsoft.SharePoint.Client.Search.Query;
â€¦
using (ClientContext clientContext = new ClientContext("http://intranet.contoso.com"))
{
    KeywordQuery keywordQuery = new KeywordQuery(clientContext);
    keywordQuery.QueryText = "Argument";
    SearchExecutor searchExecutor = new SearchExecutor(clientContext);
    ClientResult<ResultTableCollection> results = searchExecutor.ExecuteQuery(keywordQuery);
    clientContext.ExecuteQuery();
}
 
```

これで、検索結果を反復処理できるようになりました。次の例では、それぞれの検索結果のタイトルを書き込んでいます。

```C#
foreach (var row in results.Value[0].ResultRows) 
{ 
    Console.WriteLine(row["Title"]); 
}
```

### 検索クエリ REST サービス

検索クエリ REST サービスは、**POST** と **GET** の両方の HTTP 要求をサポートしています。 検索 REST サービスを呼び出すときには、要求でクエリ パラメーターを指定します。検索では、それらのクエリ パラメーターを使用して検索クエリを作成します。 **GET** 要求では、URL でクエリ パラメーターを指定します。 **POST** 要求の場合は、JavaScript Object Notation (JSON) 形式のクエリ パラメーターを本文で渡します。

**JSON GET および POST 要求**

|動詞|URI|
|:---|:---|
| **GET** 要求| `http://server/_api/search/query`|
| **POST** 要求| `http://server/_api/search/postquery`|

**検索 REST サービスのサンプル GET 要求**

|**要求の種類**|**要求 URL**|
|:-----|:-----|
|キーワード|http://server/site/_api/search/query?querytext='{KQL Query}‘|
|プロパティの選択|http://server/site/_api/search/query?querytext='test'&amp;selectproperties='Title,Rank'|
|並べ替え|http://server/site/_api/search/query?querytext='test'&amp;sortlist='LastModifiedTime:descending'http://server/site/_api/search/query?querytext='test'&amp;sortlist='LastModifiedTime:descending,Rank:ascending'|

使用可能なクエリ パラメーターの完全なリストとその使用方法については、「 [SharePoint 検索 REST API の概要](http://msdn.microsoft.com/library/8a4f7863-e4c1-4099-9189-a1894db36930%28Office.15%29.aspx)」を参照してください。サンプル コードについては、「 [SharePoint 2013: SharePoint アドインからの検索 REST サービスの使用](https://code.msdn.microsoft.com/sharepoint/SharePoint-2013-Perform-a-1bf3e87d)」を参照してください。

### 検索アドイン

SharePoint アドイン (以前は「SharePoint 用アプリ」と呼ばれていました) は、SharePoint Web サイトの機能を拡張する自己完結型の機能です。検索アドイン (以前は「検索アプリ」と呼ばれていました) は検索機能を使用する SharePoint アドインです。検索アドインで、検索クエリ API を使用して検索結果を取得できます。また、その検索結果を使用して、検索構成を 1 つの SharePoint インストールから別のインストールに配布することもできます。

検索アドインを作成するための開発環境のセットアップ方法については、「 [SharePoint アドインのオンプレミスの開発環境をセットアップする](http://msdn.microsoft.com/library/b0878c12-27c9-4eea-ae3b-7e79e5a8838d%28Office.15%29.aspx)」または「 [Office 365 で SharePoint アドインの開発環境をセットアップする](http://msdn.microsoft.com/library/b22ce52a-ae9e-4831-9b68-c9210af6dc54%28Office.15%29.aspx)」を参照してください。

#### アクセス許可

検索アドインは、ユーザー レベルのアクセス許可 (属性値は  _QueryAsUserIgnoreAppPrincipal_) のみを必要とします。このアクセス許可によって、ユーザーのアクセス許可に基づいて検索アドインを照会できます。つまり、検索結果はユーザーの ACL に基づいて返されます。検索を使用するためにアクセス許可をアドインに付与するには、次の手順を実行します。

1. **ソリューション エクスプローラー**で、**AppManifest.xml** を開きます。
    
2. **[アクセス許可]** タブで **[検索の範囲]** を選択して、**[QueryAsUserIgnoreAppPrincipal]** を選択します。
    
詳細については、「[SharePoint 2013 でのアドインのアクセス許可](http://msdn.microsoft.com/library/5f7a8440-3c09-4cf8-83ec-c236bfa2d6c4%28Office.15%29.aspx)」を参照してください。

#### クエリ API

.NET CSOM、JavaScript CSOM、または検索 REST サービスを使用して、検索アドインで検索結果を取得できます。次の例には、クエリ .NET CSOM を使用して検索アドインで検索結果を取得する方法が示されています。

```C#
var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);
using (var clientContext = spContext.CreateUserClientContextForSPHost())
{
    KeywordQuery keywordQuery = new KeywordQuery(clientContext);
    keywordQuery.QueryText = "Argument";
    SearchExecutor searchExecutor = new SearchExecutor(clientContext);
    ClientResult<ResultTableCollection> results = searchExecutor.ExecuteQuery(keywordQuery);
    clientContext.ExecuteQuery();
}

```

## このセクションの内容

- [SharePoint の検索のカスタマイズ](search-customizations-for-sharepoint.md)

## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint アドイン](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx)
    
- [SharePoint 2013 での検索アドイン](http://msdn.microsoft.com/library/21682e45-dd78-4f3c-8f1e-cdd48de3bde2%28Office.15%29.aspx)
    
- [検索機能を SharePoint アドインに追加する](http://blogs.msdn.com/b/officeapps/archive/2013/05/30/add-search-capabilities-to-your-apps-for-sharepoint.aspx)
