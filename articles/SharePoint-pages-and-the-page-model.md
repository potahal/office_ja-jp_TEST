# SharePoint ページとページ モデル

この記事では、マスター ページ、コンテンツ ページ、SharePoint ページの各部分、既定のページ ファイルの種類を含む SharePoint ページ モデルについて説明します。 

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Online_

レンダリングされる SharePoint ページは、次の 3 つの種類のページの組み合わせになります。 

- マスター ページ。コンテンツのレイアウトと外観を制御します。
    
- コンテンツ ページ。ページ フィールド コントロールが含まれます。
    
- ユーザーにとって分かりやすい作成ページ。ユーザーがコンテンツを追加するページです。
    
この記事では、SharePoint ページ モデルの概要について説明します。それには、ページの種類、SharePoint 2013 と SharePoint Online で使用可能な既定のページ ファイル、およびページの処理方法が含まれます。 

## SharePoint ページ モデルに関連する主要な用語と概念
<a name="sectionSection0"> </a>

|**用語または概念**|**定義**|**アクセス経路**|**詳細**|
|:-----|:-----|:-----|:-----|
|グループ作業サイト|チーム サイト。|||
|コンテンツ プレースホルダー|後ほどプログラムで置き換えることが可能なコントロールまたはコンテンツの場所を予約する、マスター ページ内のエントリ。|すべての SharePoint マスター ページ|コンテンツ プレースホルダーは、SharePoint マスター ページの構成要素です。|
|マスター ページ|SharePoint ページの左ナビゲーション要素およびトップ ナビゲーション要素の動作と表示を標準化するページです。|SharePoint ファイル システムのマスター ページ ギャラリー||
|マスター ページ ギャラリー|ブランド化要素 (マスター ページ、ページ レイアウト、JavaScript ファイル、CSS、イメージ) の既定格納場所である SharePoint 2013 内の特別なドキュメント ライブラリです。すべてのサイトごとに独自のマスター ページ ギャラリーがあります。| **[設定]**  >  **[サイトの設定]**  >  **[マスタ ページとページ レイアウト]**|マスター ページ ギャラリーには、マスター ページや CSS ファイルなどのブランド化アセットを格納するカタログが入れられます。<br/>**ヒント** カスタムのブランド化要素を作成する場合、カスタム アセットは既定のマスター ページ ギャラリー ファイル構造で格納します。<br/>[SharePoint 2013 のマスター ページ、マスター ページ ギャラリー、およびページ レイアウト](http://msdn.microsoft.com/library/80b9a360-bc2e-46c6-b0ca-1bc487b73db6.aspx)|
|ダウンロード最小化戦略 (MDS)|ユーザーが SharePoint ページ間でナビゲートするときにブラウザーによるデータ ダウンロード量を削減する戦略です。|サイトの設定|MDS がアクティブな場合、SharePoint はすべてのページ要求を `/_layouts/15/start.aspx` を介して渡し、新しいページ要求と既に読み込まれたページとの間に視覚上の違いがあるかをチェックします。<br/>[SharePoint 2013 でのページ パフォーマンスの最適化](http://msdn.microsoft.com/library/262caeef-64fd-4e02-b947-d772faf01159.aspx)<br/>[ダウンロード最小化戦略の概要](http://msdn.microsoft.com/library/9caa7d99-1e74-4889-96c7-ba5a10772ad7.aspx)|
|ナビゲーション|ユーザーが SharePoint サイトの情報アーキテクチャの中を移動できるようにする機能です。SharePoint のナビゲーション要素には、検索、ツリー コントロール、ボタン、リボン、ハイパーリンク、タブ、メニュー、分類があります。||[Navigation クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.navigation.aspx)<br/>[NavigationNode クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.navigationnode.aspx)|
|Oslo マスター|SharePoint 2013 における既定のマスター ページ。|SharePoint ファイル システムのマスター ページ ギャラリー|seattle.master マスター ページとは異なり、トップ ナビゲーション領域と同じ位置に現在のナビゲーションがあります。|
|ページ コンテンツ コントロール|Web パーツを追加可能な、発行サイト上のコントロール。|||
|ページ レイアウト|コンテンツの一貫性ある表示を適用するために発行ページに適用するテンプレート。|SharePoint ファイル システムのマスター ページ ギャラリー| [[方法]: SharePoint 2013 でページ レイアウトを作成する方法](http://msdn.microsoft.com/library/5447e6a1-2f14-4667-81d0-7514b468be80.aspx)|
|ページ モデル|ブラウザーでユーザーに結果的に表示される SharePoint ページのファイル、コンテンツ、対話式操作。|| [SharePoint 2013 ページ モデルの概要](http://msdn.microsoft.com/library/808b1af3-89ab-4f02-89cc-ea86cb1f9a6e.aspx)|
|発行ページ|発行サイトの .aspx ページ。|| [PublishingPage クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.publishingpage.aspx)|
|発行サイト|発行サイトと発行ページにアクセス可能な SharePoint サイト。ページ レイアウト、分類、管理ナビゲーション、および他の Web コンテンツ管理とエンタープライズ コンテンツ管理の機能が含まれます。 || [PublishingWeb クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.publishingweb.aspx)[SharePoint 2013 サイト開発の新機能](https://msdn.microsoft.com/en-us/library/office/jj163942.aspx)|
|Seattle.master|SharePoint 2013 内の既定のマスター ページ。|SharePoint ファイル システムのマスター ページ ギャラリー||
|チーム サイト|ドキュメント、wiki、アイデア、プロセスなどに関してユーザーがグループ作業をすることを意図して設計されているサイト。|||
|テキスト レイアウト|Wiki ページに表示されるコンテンツ領域を定義します。|||
|テキスト レイアウト コントロール|テキスト、画像、Web パーツ、アプリ パーツを含めることができる wiki ページ コントロール。|||
|トップレベル サイト|サーバーによって提供される既定のトップレベル サイト。|| [SharePoint サイトを作成する](https://support.office.com/en-us/article/Create-a-SharePoint-site-4b1c153a-ec2b-45df-9dd9-e31d25563d1b?CorrelationId=ad2fc24e-9e3e-4da4-be0f-500d2e89fc64&amp;ui=en-US&amp;rs=en-US&amp;ad=US)|
|Web パーツ|サイト ページのコンテキスト内で実行されるサーバー側コントロール。|| [SharePoint アプリからのカスタム アクションとプロパティ バグ エントリ](http://blogs.msdn.com/b/vesku/archive/2013/10/02/ftc-to-cam-custom-actions-and-property-bag-entries.aspx)|
|Web パーツ ページ|Web パーツを含めることが可能な、Web パーツ領域を構成するコンテンツ ページ。Web パーツは、[WebPartDefinition](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.webparts.webpartdefinition.aspx) オブジェクトによって Web パーツ ページで表されます。|| [Microsoft.SharePoint.Client.WebParts 名前空間](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.webparts.aspx)|
|Web パーツ領域|Web パーツを追加可能なページ上の領域。|||
|Wiki ページ|エンタープライズ Wiki サイト テンプレートを使用するコンテンツ ページ。|| [Provisioning.Pages サンプル アプリ](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Pages)|

## SharePoint マスター ページ
<a name="sectionSection1"> </a>

マスター ページは, .master 拡張子を持つ ASP.NET ファイルです。このページには  `<%@ Master` ディレクティブが含まれ、 **HTML**、 **Head**、 **Form** などのトップレベル HTML 要素が定義されます。最初にコントロールとアセンブリをリストし、次に **DOCTYPE** のドキュメント型定義を宣言します。この定義により、HTML の表示方法をブラウザーに指示します。SharePoint 2013 は、 **XHTML 1.0** と **HTML5** DOCTYPES を使用するとパフォーマンスが最も良くなるように調整されています。

SharePoint には、既定でいくつかのマスター ページが含まれています。これらのマスター ページによって、SKU とサイトの種類に適切な指定の SharePoint ページの既定の構造とクロムが、適切な場所、具体的にはページ上部と右側に提供されます。表 2 に、既定の SharePoint 2013 と SharePoint Online のマスター ページをまとめます。

**表 2. 既定の SharePoint マスター ページ**

|**マスター ページ**|**説明**|
|:-----|:-----|
|Custom.master|フォームやビューなどのシステム ページ。すべての SharePoint 2013 SKU と SharePoint Online SKU が使用します。|
|Default.master|発行サイトのサイト ページ。すべての SharePoint 2013 SKU と SharePoint Online SKU に含まれます。発行機能がアクティブな場合に利用できます。|
|Application.master|scope.aspx および keyword.aspx などの一部のシステム ページ。すべての SharePoint 2013 SKU と SharePoint Online SKU に含まれます。|
|Minimal.master|すべての SharePoint 2013 SKU で使用可能な既定のマスター ページ オプション。|
|Seattle.master|すべての SharePoint 2013 SKU と SharePoint Online SKU で使用可能な既定のマスター ページ オプション。|
|Oslo.master|すべての SharePoint 2013 SKU と SharePoint Online SKU で使用可能な既定のマスター ページ オプション。|
|Kyoto.master|SharePoint Online で使用可能なマスター ページ。 |
|Berlin.master|SharePoint Online で使用可能なマスター ページ。 |
|Lyon.master|SharePoint Online で使用可能なマスター ページ。 |
|Mysite15.master|OneDrive for Business サイト (以前: 個人用サイト、個人用サイト、OneDrive Pro サイト)。|
それぞれの既定の SharePoint マスター ページには、HTML、CSS、JavaScript など SharePoint で機能する一般的な Web プログラミング テクノロジに必要なコントロールが含まれます。

コンテンツ プレースホルダーは、コンテンツ ページで定義される情報の位置を保持します。コンテンツ プレースホルダーはページの領域に対応しています。.master ページの各領域を定義するコンテンツ プレースホルダーの数はごくわずかな場合もあれば、数百におよぶ場合もあります。

SharePoint マスター ページでは、ASP.NET (`<asp:`) 宣言および SharePoint (`<SharePoint:`) 宣言を両方使用します。 宣言内のコロンに後続するテキストで、コントロールの機能を定義します。たとえば、`SharePoint:PlaceholderGlobalNavigation` の場合は、SharePoint ページのグローバル ナビゲーションが、対象ページにある関連 HTML タグの内側に埋め込まれます。 マスター ページ内のコンテンツ コントロールは、**ContentPlaceHolderID** を使用してコンテンツとコンテンツ プレースホルダーをバインドします。

SharePoint には、システム マスター ページとサイト マスター ページという 2 種類のマスター ページが備わっています。システム マスター ページは、SharePoint サイト上のすべてのフォーム ページとビュー ページに適用されます。一方、サイト マスター ページは発行サイトのすべてのページによって使用されます。.master ページ ファイルを開き、 **Page** ディレクティブを表示すると、サイトでどの種類のマスター ページが使用されるかが分かります。システム マスター ページには、 `~masterurl/default.master` というようなページ ディレクティブがあります。サイト マスター ページの場合のディレクティブは `~masterurl/custom.master` などです。

CSOM コードを使用すると、マスター ページのプロパティを設定できます。主に、[Web](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.web.aspx) オブジェクトに対してコードを作成します。システム マスター ページを変更するには [MasterUrl](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.web.masterurl.aspx) プロパティを使用し、サイト マスター ページを変更する場合にはこのオブジェクトの [CustomMasterUrl](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.web.custommasterurl.aspx) プロパティを使用します。

多くの場合、コンテンツ プレースホルダーには動的トークンが含まれます。このトークンは、SharePoint ページ URL を構成する重要なコードの一部です。SharePoint は、サーバーと SharePoint ページ間におけるハイパーリンク情報の転送方法を定義した、HTTP などのプロトコル規則に従って URL 文字列を解析します。通常、CSS またはテーマ コントロールを指すコンテンツプレースホルダーは相対 URL を使用します。相対 URL では、SharePoint サーバー側オブジェクト モデルは  `~SPUrl` と表されます。

SharePoint は動的トークンを使用して、マスター ページをコンテンツ ページにバインドします。コンテンツ ページは .master ページ コードの  `<asp:content>` 宣言で定義します。表 3 に、SharePoint マスター ページにある動的トークン、およびページが処理されるときに置き換えられる CSOM プロパティ、またはコンテンツ プレースホルダーに SharePoint がレンダリングする URL 文字列の形式をまとめます。

**表 3. プロパティ値に置き換えられる、マスター ページ内の動的トークン**

|**動的トークン**|**置換後**|
|:-----|:-----|
|~masterurl/default.master|SPWeb.MasterUrl|
|~masterurl/custom.master|SPWeb.CustomMasterUrl|
|~site/&lt;xyz&gt;.master|http://&lt;siteColl&gt;/&lt;subsite1&gt;/&lt;subsite2&gt;/&lt;xyz&gt;.master|
|~sitecollection/&lt;abc&gt;.master|http://&lt;siteColl&gt;/&lt;abc&gt;.master|

**メモ** コンテンツ プレースホルダー内の動的トークンは、サーバー側の API プロパティとメソッドに対応します。リモート プロビジョニングを使用する場合には、CSOM または REST でコードを記述します。動的トークンと SharePoint URL の詳細については、「[SharePoint 2013 の URL とトークン](http://msdn.microsoft.com/library/161418d7-8123-4c4e-91a1-97e43c17f0e6.aspx)」を参照してください。SharePoint 用アドインは、サイト URL に適用するトークンを使用します。

## Web パーツ ページと Wiki ページ
<a name="sectionSection2"> </a>

Web パーツ ページには、構造化情報と非構造化情報を含めることができます。それらの情報が Web パーツ領域を構成します。Web パーツ領域に配置される Web パーツでは、リスト、検索結果、クエリに基づくデータの表示、複数のソースのデータのカスタム ビューの表示が可能です。Web パーツ ページには、標準の SharePoint チーム サイトとほとんど同じ要素が含まれます。タイトル バーには、タイトル、キャプション、説明、会社のロゴ、または他のイメージが入ります。Web パーツ ページには、次の要素が追加されます。

- Web パーツ ページ メニュー。Web パーツの追加または変更、ページ レイアウトの設計、個人用ビューと共有ビューの切り替えに使用できます。
    
- ツール ウィンドウ。Web パーツの検出と追加、Web パーツと Web パーツ ページに関連するプロパティの編集に使用できます。
    
Web パーツ ページとは対照的に、wiki ページは構造化の程度が低くなっています。半構造化または非構造化形式であるため、ユーザーにとってコンテンツの作成とグループ作業が簡単に実行できます。既定では、SharePoint は、初めてチーム サイトを表示するときに wiki ページが示されます。

エンタープライズ wiki 機能はすべてのバージョンの SharePoint で使用できます。エンタープライズ Wiki テンプレートを使用すると、wiki ページによってページ レイアウトの作成と使用を実行できます。wiki ページを編集すると、Web パーツ、テキスト、および他のコンテンツがテキスト レイアウトに表示されます。テキスト レイアウトによって、wiki ページ上のコンテンツ領域を配置します。

wiki ページを作成するために、リモート プロビジョニング パターンを使用できます。 [WikiPageCreationInformation](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.utilities.wikipagecreationinformation.aspx) クラスには wiki ページの作成に使用できるメソッドが備わっていて、 **WikiHtmlContent** プロパティはページ上の HTML コンテンツを取得して設定できます。 [Utility](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.utilities.utility.aspx) クラスには、 **CreateWikiPageInContextWeb** メソッドが含まれています。このメソッドを SharePoint は使用して、クライアント ランタイム コンテキストで wiki ページを作成します。その際に、 **WikiPageCreationInformation** クラスのパラメーターを使用します。

## ページ レイアウト
<a name="sectionSection3"> </a>

ページ レイアウトは発行サイト用に選択したコンテンツ ページです。ページ レイアウトは、ページの本文の構造をカスタマイズして、SharePoint サイト内の各種ページ (記事など) を定義したテンプレートです。Web パーツ ページがページ上に Web パーツ領域と Web パーツを配置するためのテンプレートであるのと同様、ページ レイアウトはページ上にフィールドを配置するためのものです。ページ レイアウトで定義されたフィールド コントロールには作成者が作成するコンテンツが入り、ページ レイアウトに基づいてそのコンテンツの構造が決まります。

**メモ**  ページ レイアウトには Web パーツ領域を含めることができます。

設計者はスタイルをページ フィールド コントロールに適用できます。これにより、CSS が各フィールドに適用されてレンダリングされる方法を設計者は制御でき、同時にユーザーは各ページ フィールド内のコンテンツを作成および管理できます。

SharePoint では、コンテンツ タイプは、特定のアイテムとドキュメントを定義する (列とも呼ばれる) メタデータと動作の再利用可能なコレクションです。たとえば、オンライン マガジンの記事のような外観と動作のコンテンツを作成したいという場合があるかもしれません。コンテンツ タイプを使用すると、簡単に作成できます。また、他の独特な種類のコンテンツを作成するものの、別のコンテンツ タイプの特性を再利用して共有したいという場合もあるかもしれません。それぞれのページ レイアウトは他のいずれかのコンテンツ タイプに基づいています。各コンテンツ タイプには一意の[コンテンツ タイプ ID](https://msdn.microsoft.com/en-us/library/office/aa543822%28v=office.14%29.aspx) が割り当てられます。

コンテンツ タイプの詳細については、「[コンテンツ タイプについて](https://msdn.microsoft.com/en-us/library/office/ms472236%28v=office.14%29.aspx)」、「[列](https://msdn.microsoft.com/en-us/library/office/ms196085%28v=office.14%29.aspx)」、「[コンテンツ タイプのユーザー設定情報](https://msdn.microsoft.com/en-us/library/office/ms468437%28v=office.14%29.aspx)」を参照してください。

**重要** 現在、リモート プロビジョニング パターンを使用してすぐに使えるページ レイアウトを SharePoint サイトに適用できます。CSOM コードでカスタム SharePoint 用アドイン コードを使用することによってカスタムのコンテンツ タイプをサイトにプロビジョニングすることもでき、CSOM によるカスタム **ContentTypeId** の設定は SharePoint Online でサポートされていますが、オンプレミスの SharePoint サイトでリモート プロビジョニングを使用して ContentTypeId をカスタム コンテンツ タイプに設定することは、現在のところサポートされていません。詳細については、「[[方法]: SharePoint 2013 でページ レイアウトを作成する方法](http://msdn.microsoft.com/library/5447e6a1-2f14-4667-81d0-7514b468be80.aspx)」を参照してください。

## SharePoint ページ処理モデル
<a name="sectionSection4"> </a>

SharePoint はテンプレート ベースのページ レンダリング システムで、マスター ページ、コンテンツ ページ、および作成されたコンテンツを組み合わせてページをレンダリングします。このページ レンダリング システムは、[ページ処理モデル](https://msdn.microsoft.com/en-us/library/office/ms498550%28v=office.14%29.aspx)とも呼ばれます。マスター ページはサイト内の適用先のすべてのページ インスタンスで使用され、コンテンツ ページは、そのコンテンツ ページに基づいているページのすべてのインスタンスで使用されます。

ページ処理モデルは、Web ブラウザーなどのユーザー エージェントがサーバーに対して実行するすべての要求を解釈して実行します。たとえば、contoso.aspx というページを要求しているユーザーについて考えてみましょう。この要求を実行するため、ASP.NET エンジンは次の 2 つのページを取得します。1 つは contoso.aspx と関連付けられているコンテンツ ページ、もう 1 つはファイル プロバイダーが SharePoint サイトと関連付けれたマスター ページです。またエンジンはフィールド コントロールと、フィールドの Web パーツを取得して、それをページ上にレンダリングします。

**メモ**  チーム サイトと サイトのページ処理ロジックは、発行ページのロジックと似ています。 

### ページ処理

SharePoint ユーザーが Web パーツ ページを読み込むと、SharePoint はテンプレート、ページ コンテンツ、コンテキストへのパスを解析してそのページを取得します。また、その Web パーツ ページに関連付けられている Web パースを設定し、[WebPartCollection](http://msdn2.microsoft.com/EN-US/library/k41e9930) インスタンスを対象ページに割り当て、Web パーツ ページとその Web パーツにコンテンツを設定します。

SharePoint ユーザーが wiki ページを読み込むと (チーム サイトまたは発行サイトのいずれかでエンタープライズ Wiki テンプレートを使用します)、SharePoint はテンプレート、ページ コンテンツ、コンテキストへのパスを解析してページを取得します。また、対象の Wiki ページに関連付けられているテキスト レイアウト コントロールを設定し、エンタープライズ wiki ページとそのテキスト レイアウトにコンテンツを設定します。リモート プロビジョニング パターンを使用することによる wiki ページのプロビジョニング方法の詳細については、「[Provisioning.Pages](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Pages)」サンプルを参照してください。

### ダウンロード最小化戦略と <AjaxDelta> コントロール

SharePoint では、ダウンロード最小化戦略機能によって、ページをレンダリングする前にマスター ページ上のどの特定のコンテンツを更新するかを管理します。この戦略が有効な場合、マスター ページで  `<SharePoint:AjaxDelta>` タグによって囲まれたコンテンツ プレースホルダーと関連付けられているコンテンツは、ページがレンダリングされる前に更新されます。逆に `<SharePoint:AjaxDelta>` タグで囲まれていないコンテンツ プレースホルダーは、ダウンロード最小化戦略が有効な場合にはレンダリングされません。

ダウンロード最小化戦略の有効と無効の切り替えは、セントラル サイトの管理、または SharePoint クライアント側オブジェクト モデル (CSOM) を使用して実行できます。この機能は [EnableMinimalDownload](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.web.enableminimaldownload.aspx) プロパティを使用してアクティブにすることができます。詳細については、「[ダウンロード最小化戦略の概要](http://msdn.microsoft.com/library/9caa7d99-1e74-4889-96c7-ba5a10772ad7.aspx)」を参照してください。ダウンロード最小化戦略を使用してマスター ページのパフォーマンスが向上するように最適化する方法の詳細については、「[MDS のための SharePoint コンポーネントの変更](http://msdn.microsoft.com/library/c967be7c-f29f-481a-9ce2-915ead315dcd.aspx)」を参照してください。

ダウンロード最小化戦略機能は、SharePoint チーム サイトでは既定で有効に、SharePoint 発行サイトと、発行が有効な状態の SharePoint チーム サイトでは既定で無効になっています。

### seattle.master に基づいてカスタム マスター ページを作成する

リモート プロビジョニングを使用すると、テーマなどのサイト ブランド化要素をサイトにプロビジョニングできますし、CSS または JavaScript を使用すると、要素またはページ コントロールを表示したり非表示にしたりできます。マスター ページをカスタマイズすると、ページ構造をより詳細に制御できます。カスタムのマスター ページを作成する場合、既定のマスター ページを編集してそれを既定の名前 (seattle.master など) で保存しないでください。代わりに、既定のマスター ページのコピーを作成してそれに変更を加え、名前を変更します。

**重要**   長期の継続的なサポートのコストと保守に関する問題に影響を及ぼす可能性があるため、新しいマスター ページの構造を変えないことをお勧めします。ブランド化をサポートするマスター ページについては、ヘッダーの色を変更すること、ページの特定の要素に背景色を追加すること、サイト ロゴを表示したり非表示にしたりするなど構造に影響を与えない変更を加えることができます。ページ上に含めるものの構造要素 (フッターなど) を含まない既定の .master ページの場合、すぐに使用できる別のマスター ページを使用してください。

カスタムのマスター ページで一貫した保守を実行できるようにするため、既存のコード化パターンに従ってください。たとえば、表を使用するページ領域では、表を使用してコード化パターンを強化します。 `<DIV>` タグまたは HTML5 が使用される領域では、カスタム コードが `<DIV>` タグまたは HTML5 に準拠するようにします。このようにしておくと、長期にわたり実行している間に、作成する必要のあるカスタムのマスター ページの保守が簡単に実行することができ、コストも低くなります。

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint 2013 および SharePoint Online のブランディングおよびサイト プロビジョニング ソリューション](Branding-and-site-provisioning-solutions-for-SharePoint.md)
    
-  [SharePoint 2013 のマスター ページ、マスター ページ ギャラリー、およびページ レイアウト](http://msdn.microsoft.com/library/80b9a360-bc2e-46c6-b0ca-1bc487b73db6.aspx)
    
-  [[方法]: SharePoint 2013 でページ レイアウトを作成する方法](http://msdn.microsoft.com/library/5447e6a1-2f14-4667-81d0-7514b468be80.aspx)
    
-  [SharePoint 2013:SharePoint 用アプリを使用して Wiki ページをプロビジョニングする](https://code.msdn.microsoft.com/SharePoint-2013-Use-an-app-5db977e8)
    
-  [ダウンロード最小化戦略の概要](http://msdn.microsoft.com/library/9caa7d99-1e74-4889-96c7-ba5a10772ad7.aspx)
    
-  [コンテンツ タイプについて](https://msdn.microsoft.com/en-us/library/office/ms472236%28v=office.14%29.aspx)
