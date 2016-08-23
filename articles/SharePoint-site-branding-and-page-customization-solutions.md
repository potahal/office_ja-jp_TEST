# SharePoint サイト ブランド化とページ カスタマイズのソリューション

SharePoint ページ モデル、構成済み外観、SharePoint 2013 テーマ エンジン、CSS を使用して、ご利用の SharePoint サイトとページをブランド化します。

_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Online_

SharePoint サイトの外観は次の 2 つの方法でカスタマイズできます。

- テーマ エンジンを使用して、カスタム テーマ (SharePoint 2013 と SharePoint Online における構成済み外観) を作成できます。テーマでは、少なくとも色を定義します。完全なテーマの場合には、色、フォント、背景イメージ、関連マスター ページ, .master ページがプレビューされる方法を定義した .preview ファイルを定義します。リモート プロビジョニング パターンを使用してテーマをサイトに適用できます。

- カスタムのカスケード スタイル シート (CSS) を作成して、SharePoint Online サイトに適用します。SharePoint 用アプリおよびリモート プロビジョニング パターンを使用すると、カスタム CSS を使用する SharePoint サイトをプロビジョニングできます。

ブランド化は、単純でコストの低いものから、複雑でコストの高いものまでその範囲はさまざまです。ユーザーは UI を使用して構成済み外観を適用できます。構成済み外観には、背景イメージ、カラー パレット、フォント、マスター ページ、マスター ページに関連付けられている .preview ファイルが含まれています。SharePoint 2013 テーマ エンジンを使用して構成済み外観を設計し、サイトをプロビジョニングできます。また、カスタム CSS を作成してサイトとその要素の外観を変更できます。

**重要**  カスタムのブランド化プロジェクトの一部としてカスタム マスター ページと他の構造化要素を作成することは可能ですが、カスタムのマスター ページと他のカスタム構造化要素をサポートする長期にわたるコストが高くなります。カスタム ブランド化を使用すると、アップグレードを適用し、継続的なサポートを提供する点で組織が必要とするコストを少なくできます。

このセクションでは、[SharePoint 開発ツールとデザイン ツールおよび操作](SharePoint-development-and-design-tools-and-practices.md)について説明し、SharePoint サイトの外観をカスタマイズする方法を示します。

## SharePoint ブランド化に関連する主要な用語と概念
<a name="sectionSection0"> </a>

|**用語または概念**|**定義**|**詳細情報**|
|:-----|:-----|:-----|
|代替 CSS|サイトの外観に適用可能な、既定以外の CSS ファイル。|代替 CSS は、サイトとそのすべてのサブサイトにカスタム CSS を適用する場合に使用します。 |
|CSS|HTML または XML のドキュメント スタイルをレンダリングする方法をブラウザーに指示する言語。CSS は、ドキュメント コンテンツ (HTML または XML) をコンテンツの表示方法とは分離します。||
|構成済みの外観|サイトに適用されるフォント、カラー パレット、背景イメージ、関連するマスター ページの組み合わせ。フォント パターンとカラー イメージは省略可能です。構成済みの外観の既定のファイル場所は次のとおりです。Theme Gallery\15 フォルダー|<p>構成済みの外観は、サイトの構造を変更することなく、サイトの外観を変更するのに便利な方法です。 

</p><p>SharePoint 2013 には、いくつかの構成済み外観が既定で用意されています。 ユーザーが構成済み外観を適用すると、SharePoint によって構成済み外観に含まれるすべての関連デザイン要素がサイトに適用されます。</p>|
|コンテンツ検索 Web パーツ (CSWP)|指定のクエリに基づく検索結果のコンテンツをレンダリングします。| <p>
            [SharePoint 2013 におけるコンテンツ検索 Web パーツ](http://social.technet.microsoft.com/wiki/contents/articles/15843.content-search-web-part-in-sharepoint-2013.aspx) (TechNet) </p><p>
            [SharePoint 2013 におけるコンテンツ検索 Web パーツ](http://msdn.microsoft.com/library/6fb4bf41-0846-4dca-ad9e-906afdfd3d2b.aspx) (MSDN)</p>|
|corev15.css|SharePoint 用のほとんどの主要な機能が含まれている CSS ファイル。既定のファイル場所は次のとおりです。_layouts\15 フォルダー||
|CSSRegistration|ほとんどの既定 UI に適用される大部分の CSS を読み込む、seattle.master などのマスター ページ内の参照。|既定の CSS を上書きするには、マスター ページの  **CSSRegistration** コントロールを使用します。|
|カスタム アクション|ホスト Web 上のリストおよびリボンをカスタマイズしたり対話したりするために使用できるアクション。| [方法:カスタム アクションを作成して、SharePoint アドインで展開する](http://msdn.microsoft.com/library/bbd11f94-1798-453e-bbb0-e5eb0df8dc75.aspx)|
|デバイス チャネル|1 つの SharePoint 発行サイトを複数の方法でレンダリングします。そのために、特定のデバイスでレンダリングを実行するコンテンツを、一意のチャネルを使用してターゲットとします。| [SharePoint 2013 デザイン マネージャーのデバイス チャネル](http://msdn.microsoft.com/library/a924bd7b-a5e3-41bf-b0a7-3e43945fa951.aspx)|
|表示テンプレート|検索インデックスに対するクエリ結果を表示するために検索 Web パーツが使用するテンプレート。| [SharePoint 2013 デザイン マネージャー表示テンプレート](http://msdn.microsoft.com/library/1a782bac-48ee-4baf-8751-0f943a306e0f.aspx)|
|イメージ表示|同一のソース イメージに基づく、発行サイトにおける異なるサイズ バージョンのイメージの表示。| [SharePoint 2013 デザイン マネージャーのイメージ表示](http://msdn.microsoft.com/library/d08a74c0-5674-4f26-8646-11ea1f081c85.aspx)|
|管理メタデータ| SharePoint 内の機能で、用語、用語セット、グループ、用語のラベルを定義できます。分類と呼ばれることもあります。SharePoint 2013 では、管理されたメタデータ システムが管理ナビゲーションの基礎となります。| [SharePoint 2013 のマネージ メタデータとナビゲーション](http://msdn.microsoft.com/library/b66d4ec1-a2ef-49cc-8ca5-a6b516bff02e.aspx)|
|管理されたナビゲーション|管理されたメタデータに基づいて構築された、発行サイトのナビゲーション。ナビゲーションは、用語ストア内の指定された用語セットで構築されます。 | [SharePoint 2013 でのマネージ ナビゲーション](http://msdn.microsoft.com/library/c9da5011-3c73-4b83-8e00-e7a03a71ed02.aspx)|
|マスター ページ|SharePoint ページの左ナビゲーションとトップ ナビゲーション領域の動作と表示を標準化するページ。| [SharePoint 2013 のマスター ページ、マスター ページ ギャラリー、およびページ レイアウト](http://msdn.microsoft.com/library/80b9a360-bc2e-46c6-b0ca-1bc487b73db6.aspx)|
|マスター ページ ギャラリー|SharePoint 2013 内の特別なドキュメント ライブラリで、既定では、すべてのブランド化要素、つまりマスター ページ、ページ レイアウト、JavaScript ファイル、CSS、イメージが格納されます。サイトごとに独自のマスター ページ ギャラリーがあります。カスタムのブランド化要素を作成する場合、カスタム アセットは既定のマスター ページ ギャラリー ファイル構造に格納することをお勧めします。| [SharePoint 2013 のマスター ページ、マスター ページ ギャラリー、およびページ レイアウト](http://msdn.microsoft.com/library/80b9a360-bc2e-46c6-b0ca-1bc487b73db6.aspx)|
|Oslo.master|SharePoint 2013 内のマスター ページ。|現在のナビゲーションを、トップ ナビゲーション領域と同じ位置に移動します。 |
|ページ コンテンツ コントロール|Web パーツを追加可能な、発行サイト上のコントロール。||
|ページ レイアウト|ユーザーがページ上に情報を一貫性のある方法でレイアウトできるようにするための SharePoint 発行サイト ページ用のテンプレート。||
|サイド リンク バー|グループ作業サイトのページの左側にあるナビゲーション要素を管理します。|グループ ナビゲーション項目に見出しリンクを追加できます。|
|REST|アーキテクチャ要素を抽象化し、HTTP 動詞を使用して XML ファイルを含む Web ページでのデータの読み取りと書き込みを実行するステートレス アーキテクチャ スタイル。| [SharePoint 2013 REST サービスの概要](http://msdn.microsoft.com/library/2de035a0-ac75-43bd-9665-5c5a59c4c590.aspx)|
|ルート Web|サイト コレクション内の最初の Web ページ。|またルート Web は、Web アプリケーション ルートと呼ばれることもあります。 |
|Seattle.master|SharePoint 2013 チーム サイトと発行サイトの既定の .master ページ。||
|サイト レイアウト|マスター ページをご覧ください。|サイト レイアウトは、テーマの .master ページを対応する .preview ファイルと結合します。|
|構造化ナビゲーション|発行サイトのサイト階層に基づく、発行サイトのナビゲーション構造。見出しやリンクを追加して、SharePoint が自動的に生成する構造化ナビゲーションを手動で置換したりカスタマイズしたりできます。| [[方法] ナビゲーションをカスタマイズする](https://msdn.microsoft.com/en-us/library/office/ms558975%28v=office.14%29.aspx)|
|テーマ|簡単なブランド化を SharePoint サイトに適用する単純な方法。テーマの既定のファイル場所はサイトの _themes フォルダーです。|<p>テーマは、SharePoint サイトにカスタム ブランド化を適用する簡単な方法です。</p><p>[SharePoint 2013 のテーマの概要](http://msdn.microsoft.com/library/ae585dd3-82fe-46bb-ac93-065edc0a16f4.aspx)</p><p> [方法:SharePoint 2013 でカスタム テーマを展開する](http://msdn.microsoft.com/library/f703df24-8e56-4e6a-bc37-95acbb3c83e8.aspx)</p>|
|テーマ エンジン|構成済み外観の外観、動作、ファイルの関連付けを定義する一連のファイルと機能。||
|ユーザー エージェント文字列|要求を作成するソフトウェアを識別する、ブラウザーが Web サイトに渡すサーバーからの情報。| [SharePoint 2013 デザイン マネージャーのデバイス チャネル](http://msdn.microsoft.com/library/a924bd7b-a5e3-41bf-b0a7-3e43945fa951.aspx)|
|ユーザー カスタム アクション|Web サイト、リスト、サイト コレクションのカスタム アクションのコレクションを返す CSOM プロパティ。 既定のファイルの場所は次のとおりです。15\TEMPLATE\FEATURES<p>次に例を示します。</p><p>&lt;HideCustomAction GroupId="Galleries"<br />&nbsp;&nbsp;&nbsp;HideActionId="Themes"<br />&nbsp;&nbsp;&nbsp;Location="Microsoft.SharePoint.SiteSettings"&gt;</p>|<p>[UserCustomAction クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.usercustomaction.aspx)</p><p>[方法:カスタム アクションを作成して、SharePoint アドインで展開する](http://msdn.microsoft.com/library/bbd11f94-1798-453e-bbb0-e5eb0df8dc75.aspx)</p><p>
            [方法:ユーザー カスタム アクションを操作する](https://msdn.microsoft.com/en-us/library/office/ee538686%28v=office.14%29.aspx)、[カスタム アクションの既定の場所および ID](http://msdn.microsoft.com/library/6889686e-f6e6-4da0-bfd4-099878614980.aspx)</p>|

## このセクションの内容
<a name="sectionSection1"> </a>

|**記事**|**説明される方法...**|
|:-----|:-----|
| [構成済みの外観を使用して SharePoint サイトをブランド化する](Use-composed-looks-to-brand-SharePoint-sites.md)|色、フォント、背景イメージが含まれる構成済み外観を SharePoint 2013 サイトと SharePoint Online サイトに適用します。|
| [リモート プロビジョニングを使用して SharePoint ページをブランド化する](Use-remote-provisioning-to-brand-SharePoint-pages.md)|リモート プロビジョニングを使用して SharePoint のテーマを操作します。|
| [CSS を使用して SharePoint ページをブランド化する](Use-CSS-to-brand-pages.md)|リモート プロビジョニングを使用して SharePoint のテーマを操作します。|
| [リモート プロビジョニングと CSS を使用して SharePoint ページをカスタマイズする](Customize-a-SharePoint-page-by-using-remote-provisioning-and-CSS.md)|CSS を使用して SharePoint のリッチ テキスト フィールドおよび Web パーツ領域をカスタマイズします。|
| [既存の SharePoint サイトとページ領域のブランド化の更新](update-the-branding-of-existing-sharepoint-sites-and-page-regions.md) | リボン、サイト ナビゲーション、設定メニュー、ツリー ビュー、ページ コンテンツなど、SharePoint ページの既存の SharePoint サイトまたは領域のブランド化をカスタマイズして更新します。
|[OneDrive for Business サイト ブランド化のカスタマイズ](Customization-Options-For-OD4B-Sites.md)| OneDrive for Business サイトのカスタマイズは、組織の要件に応じて Office 365 またはアドイン モデルを使用して行います。|

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint 2013 および SharePoint Online のブランディングおよびサイト プロビジョニング ソリューション](Branding-and-site-provisioning-solutions-for-SharePoint.md)

-  [SharePoint ページとページ モデル](SharePoint-pages-and-the-page-model.md)

-  [SharePoint 開発ツールとデザイン ツールおよび操作](SharePoint-development-and-design-tools-and-practices.md)
