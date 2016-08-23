# 既存の SharePoint サイトとページ領域のブランド化の更新

リボン、サイト ナビゲーション、設定メニュー、ツリー ビュー、ページ コンテンツなど、SharePoint ページの既存の SharePoint サイトまたは領域のブランド化をカスタマイズして更新します。

_**適用対象:** SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

既存の SharePoint サイトとサイト コレクションのブランド化を更新したり、SharePoint ページの領域をカスタマイズしたりできます。たとえば、サイトの新しいバージョンを更新する際に、そうした操作を実行できます。 

## 既存のサイトとサブサイトのブランド化の更新

GitHub の「 [Office 365 Developer パターンと操作](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Branding.Refresh)」プロジェクトの [Branding.Refresh](https://github.com/OfficeDev/PnP/tree/dev) サンプルは、OfficeDevPnP.Core ライブラリを使用して既存のサイトとサブサイトを繰り返し使用しながら、適用済みのブランド化を検証し、更新する方法を示しています。このサンプルはサイト ブランド化の更新方法を示していますが、同じ概念は、サイトのリストに新しいライブラリを配置したり、プロビジョニングの際に配置されたカスタム アクションを更新したりするなど、他の作業でも使用できます。同じプロセスを使用して、既存のサイトを新しいバージョンに移動できます。

この操作には、次の 2 つの手順が関係しています。

- 更新するサイトを取得します。
    
- サイトを更新します。

### 手順 1: 更新するサイトを取得する

最初に、更新するサイトとサブサイトのリストを取得します。このサンプルには検索機能を使用してそれを実行する方法が示されていますが、サイト ディレクトリから直接読み込んだり、管理者がリストを指定可能な管理 UI を備えたりする他の選択肢もあります。次の例では、検索機能を使用してリストを生成する方法を示します。

```
// Get a list of sites. Search is one way to get this list, an alternative can be a site directory.
List<SiteEntity> sites = cc.Web.SiteSearchScopedByUrl("https://bertonline.sharepoint.com");

// Generic settings (apply changes on all webs or just root web).
bool applyChangesToAllWebs = true;

// Optionally further refine the list of returned site collections.
var filteredSites = from p in sites
                    where p.Url.Contains("13003")
                    select p;

List<SiteEntity> sitesAndSubSites = new List<SiteEntity>();
if (applyChangesToAllWebs)
{
  // You want to update all sites, so the list of sites is extended to all subsites.
  foreach (SiteEntity site in filteredSites)
  {
    sitesAndSubSites.Add(new SiteEntity() { Url = site.Url, 
                                            Title = site.Title, 
                                            Template = site.Template });
    GetSubSites(cc, site.Url, ref sitesAndSubSites);
  }
  sites = sitesAndSubSites;
}
```

**GetSubSites** に対する呼び出しは再帰的であるため、サブサイト ツリーをフェッチします。ツリーがフェッチされると、返された数が正しいことを確認してから続行します。

### 手順 2: ブランド化を更新する

処理するサイトを選択した後、OfficeDevPnP.Core メソッドを使用してサイトを操作できます。このサンプルでは、サイトのブランド化の方法が示されていますが、同じ方法であらゆる種類の変更を処理できます。

このサンプルでは、Web プロパティ バッグを使用して、現在の設定についての情報を格納するパターンが使用されています。このコードは最初に Web プロパティ バッグ値を読み取り、次にそれぞれの値に適切なアクションを実行します。

```
// Verify that you have a property bag entry.
string themeName = cc.Web.GetPropertyBagValueString(BRANDING_THEME, "");

if (!String.IsNullOrEmpty(themeName))
{
  // If no theme property bag entry, assume no theme has been applied.
  if (themeName.Equals(currentThemeName, StringComparison.InvariantCultureIgnoreCase))
  {
    // The applied theme matches to the theme you want to update.
    int? brandingVersion = cc.Web.GetPropertyBagValueInt(BRANDING_VERSION, 0);
    if (brandingVersion < currentBrandingVersion)
    {
      DeployTheme(cc, currentThemeName);
      // Set the web propertybag entries
      cc.Web.SetPropertyBagValue(BRANDING_THEME, currentThemeName);
      cc.Web.SetPropertyBagValue(BRANDING_VERSION, currentBrandingVersion);
    }
  }
  else
  {
    if (forceBranding)
    {
      DeployTheme(cc, currentThemeName);
      // Set the web propertybag entries.
      cc.Web.SetPropertyBagValue(BRANDING_THEME, currentThemeName);
      cc.Web.SetPropertyBagValue(BRANDING_VERSION, currentBrandingVersion);
    }
  }
}
```

次にテーマを更新するコードは簡単で、OfficeDevPnP.Core メソッドに基づいています。

```
string themeRoot = Path.Combine(AppRootPath, String.Format(@"Themes\{0}", themeName));
string spColorFile = Path.Combine(themeRoot, string.Format("{0}.spcolor", themeName));
string spFontFile = Path.Combine(themeRoot, string.Format("{0}.spfont", themeName));
string backgroundFile = Path.Combine(themeRoot, string.Format("{0}bg.jpg", themeName));
string logoFile = Path.Combine(themeRoot, string.Format("{0}logo.png", themeName));

if (IsThisASubSite(cc.Url))
{
  // Retrieve the context of the root site of the site collection.
  using (ClientContext ccParent = new ClientContext(GetRootSite(cc.Url)))
  {
    ccParent.Credentials = cc.Credentials;
    cc.Web.DeployThemeToSubWeb(ccParent.Web, themeName, spColorFile, spFontFile, backgroundFile, "");
    cc.Web.SetThemeToSubWeb(ccParent.Web, themeName);
  }
}
else
{
  cc.Web.DeployThemeToWeb(themeName, spColorFile, spFontFile, backgroundFile, "");
  cc.Web.SetThemeToWeb(themeName);
}
```

## SharePoint ページ領域のカスタマイズ

SharePoint ページの領域をカスタマイズすることが目的の場合、対象ページの領域と関連付けられているファイル上でリモート プロビジョニングとカスタムのカスケード スタイル シート (CSS) を組み合わせて使用できます。その場合、SharePoint ページのさまざまな領域とどの SharePoint ファイルが関連付けられているかを最初に把握しておく必要があります。 

**表 1. SharePoint ページ領域と関連ファイル**

|**ページ領域**|**関連ファイル **|**詳細情報**|
|:-----|:-----|:-----|
|リボン|任意の既定のマスター ページ。 対応する CSS: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>メイン本文 - 本文: #s4-workspace</p></li><li><p>スイート バー - 左: #suiteBarLeft</p></li><li><p>スイート バー - 右: #suiteBarRight</p></li><li><p>リボン コンテナー: #globalNavBox</p></li></ul>|[ **コンテンツにフォーカスを移動**] ボタンを使用して、非表示にすることができます。|
|スイート ナビゲーション|任意の既定のマスター ページ。 |[ **コンテンツにフォーカスを移動**] ボタンを使用して、非表示にすることができます。|
|スイート リンク||[ **コンテンツにフォーカスを移動**] ボタンを使用して、非表示にすることができます。|
|[ようこそ] メニュー|.master|[ **コンテンツにフォーカスを移動**] ボタンを使用して、非表示にすることができます。|
|[設定] メニューまたはギア|.master|例については、「 [FTC から CAM への変換 ? SP アドインのカスタム アクションとプロパティ バッグ エントリ](http://blogs.msdn.com/b/vesku/archive/2013/10/02/ftc-to-cam-custom-actions-and-property-bag-entries.aspx)」をご覧ください。|
|ヘルプ|.master||
|リボンのコンテキスト タブ|任意の既定のマスター ページ。|例については、以下をご覧ください。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><a href="https://social.msdn.microsoft.com/Forums/sharepoint/en-US/df1e4e32-ef58-4b51-8ac8-a8c3690e648b/sharepoint-2013-duplicate-contextual-tabs?forum=sharepointdevelopment" target="_blank">SharePoint 2013 で重複するコンテキスト タブ</a></p></li><li><p><a href="https://social.msdn.microsoft.com/Forums/sharepoint/en-US/a3640d58-afe1-41d0-ac83-bd7886c37355/hide-a-contextual-ribbon-tab?forum=crmdevelopment" target="_blank">コンテキスト リボン タブを非表示にする</a></p></li><li><p><a href="https://social.msdn.microsoft.com/Forums/sharepoint/en-US/201306cf-5874-4778-b773-f870c67cee94/hideshow-contextual-tab-on-click-event-of-subgrid?forum=crmdevelopment" target="_blank">サブグリッドのクリック イベントでコンテキスト タブの非表示/表示を切り替える</a></p></li></ul>|
|クイック アクセス ツールバー|.master|[ **コンテンツにフォーカスを移動**] ボタンを使用して、非表示にすることができます。|
|サイト ロゴ|.master<p>対応する CSS: .ms-sitelcon-img</p>||
|トップ ナビゲーション|ナビゲーション CSOM/JSOM<p>.master</p>対応する CSS (編集モードではありません): <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>新しい項目を選択したとき: .ms-core-listMenu-horizontalBox li.static > .ms-core.listMenu-selected</p></li><li><p>新しい項目をポイントしたとき: .mscore-listMenu-horizontalBox li.static > a.ms-core-listMenu-item:hover</p></li><li><p>ポップアウト矢印: .ms-core-listMenu-horizontalBox .dynamic-children.additional-background</p></li><li><p>ナビゲーション項目 (上位メニュー項目に対応): .ms-core-listMenu-horizontalBox li.static > .ms-core-listMenu-item</p></li><li><p>ポップアウト項目: ul.dynamic .ms-core-listMenu-item</p></li><li><p>ポップアウト コンテナー: ul.dynamic</p></li><li><p>リンク編集: .ms-navedit-editLinksText > span> .ms-metadata</p></li></ul>対応する CSS (編集モード): <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>ナビゲーション編集モード リンク: .ms-core-listMenu-horizontalBox .ms-core-listMenuEdit > tr> .msnavedit-linkCell > .ms-core-listMenu-item</p></li><li><p>リンク追加: .ms-core-listMenu-horizontalBox a.ms-navedit-addNewLink</p></li></ul>||
|ページ タイトル|対応する CSS (編集モード): <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>ページ タイトルおよびリンクされているページ タイトル: .ms-core-pageTitle、.ms-core-pageTitle a</p></li><li><p>説明ボタン: #ms-pageDescriptionDiv</p></li><li><p>説明ボックス: .js-callout-mainElement</p></li><li><p>説明ボックス矢印: .js-callout-beak</p></li><li><p>説明テキスト: .js-callout-body</p></li></ul>||
|検索ボックス|ナビゲーション CSOM/JSOM<p>.master</p>対応する CSS: <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>検索ボックスの枠線: .ms-srch-sb-border</p></li><li><p>検索ボックスの枠線にポイントしたとき: .ms-srch-sb-border: hover</p></li><li><p>クリックしたときの検索ボックスの枠線: .ms-srch-sb-borderFocused</p></li><li><p>検索ボックスの入力テキスト ボックス: .ms-srch-sb-borderFocused</p></li><li><p>検索ボックスの本文: .ms-srch-sb</p></li><li><p>検索ボックスの入力テキスト ボックス: .ms-srch-sb-searching</p></li><li><p>検索</p></li></ul>||
|左ナビゲーション|ナビゲーション CSOM/JSOM<p>.master</p>|詳細については、次のトピックを参照してください。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>
            <a href="https://msdn.microsoft.com/en-us/library/office/ms558975(v=office.14).aspx" target="_blank">方法:SharePoint Server でナビゲーションをカスタマイズする</a></p></li><li><p><a href="http://msdn.microsoft.com/library/c9da5011-3c73-4b83-8e00-e7a03a71ed02(Office.15).aspx" target="_blank">SharePoint 2013 でのマネージ ナビゲーション</a></p></li></ul>|
|ツリー ビュー|.master|詳細については、「[組み込みのツリービュー ナビゲーターをカスタマイズする方法](https://social.msdn.microsoft.com/Forums/sharepoint/en-US/dd4d49fd-e107-469d-b326-d37c86ff66b8/how-to-customize-the-builtin-treeview-navigator-?forum=sharepointcustomizationprevious)」を参照してください。|
|ページ コンテンツ|ページ レイアウト/コンテンツ ページ<br>Web パーツ領域/Web パーツ<p>CSS に対応 (Web パーツ領域と Web パーツ):</p><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Web パーツ領域: .ms-webpart-zone</p></li><li><p>Web パーツ ホルダー: .ms-webpartzone-cell</p></li><li><p>Web パーツ タイトル: .ms-webpart-titleText</p></li><li><p>リンクされている Web パーツ タイトル: .ms-webpart-titleText > a</p></li><li><p>Web パーツの本文: .ms-WPBody</p></li></ul>|詳細については、「[方法:SharePoint 2013 でページ レイアウトを作成する方法](http://msdn.microsoft.com/library/5447e6a1-2f14-4667-81d0-7514b468be80%28Office.15%29.aspx)」を参照してください。|

ページのカスタマイズ領域に関連するサンプルについては、GitHub の「[Office 365 Developer パターンと操作](https://github.com/OfficeDev/PnP/tree/dev)」プロジェクトで、次を参照してください。

- [Branding.AlternateCSSAndSiteLogo](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.AlternateCSSAndSiteLogo)
    
- [Branding.Themes](https://github.com/Lauragra/PnP/tree/master/Scenarios/Branding.Themes)

### 既定の SharePoint マスター ページで必要となる「最小」コンテンツ プレースホルダー

SharePoint .master ページではコンテンツ プレースホルダーの使用が必要になります。コンテンツ プレースホルダーは、SharePoint ページがページ ライフサイクルをサポートするために必要な基本的なコンテンツと構造化要素をレンダリングします。表 2 に、それらのコンテンツ プレースホルダーをリストし、説明を記します。

**表 2. SharePoint マスター ページで最小限必要とされるコンテンツ プレースホルダー**

|**コンテンツ プレースホルダー**|**保持するコンテンツの対象**|
|:-----|:-----|
|PlaceHolderAdditionalPageHead|ページの <head> セクションの追加項目。|
|PlaceHolderBodyAreaClass|ページ ヘッダーの追加スタイル。|
|PlaceHolderBodyLeftBorder|ページ本文の左罫線要素。|
|PlaceHolderBodyRightBorder|ページ本文の右罫線要素。|
|PlaceHolderCalendarNavigator|予定表がページに表示される際に予定表のナビゲーションのための日付の選択。|
|PlaceHolderFormDigest|「フォーム ダイジェスト」セキュリティ コントロール。|
|PlaceHolderGlobalNavigation|グローバル ナビゲーション階層リスト (トップ ナビゲーション)。|
|PlaceHolderGlobalNavigationSiteMap|グローバル ナビゲーションのサイト マップ (トップ ナビゲーション)。|
|PlaceHolderHorizontalNav|ページのトップ ナビゲーション メニュー (トップ ナビゲーション)。|
|PlaceHolderLeftActions|左下部ナビゲーション領域 (左ナビゲーション)。|
|PlaceHolderLeftNavBar|左ナビゲーション領域 (左ナビゲーション)。|
|PlaceHolderLeftNavBarDataSource|左ナビゲーション メニューのデータ ソース (左ナビゲーション)。|
|PlaceHolderLeftNavBarTop|左上部ナビゲーション領域 (左ナビゲーション)。|
|PlaceHolderMain|ページのメイン コンテンツ (ページ コンテンツ)。|
|PlaceHolderMiniConsole|ページ レベルのコマンド (エンタープライズ Wiki ページにおけるページ編集、履歴、被リンクなど)。|
|PlaceHolderNavSpacer|左ナビゲーション領域の幅 (左ナビゲーション)。|
|PlaceHolderPageDescription|ページ コンテンツの説明。|
|PlaceHolderPageImage|ページの左上隅のページ アイコン (リボン)。|
|PlaceHolderPageTitle|ブラウザー ページのタイトル バーに表示されるページ タイトル (&lt;title&gt;) (ページ タイトル)。|
|PlaceHolderPageTitleInTitle|階層リストの直下に表示されるページ タイトル (ページ タイトル)。 |
|PlaceHolderQuickLaunchBottom|サイド リンク バー ナビゲーションの下部 (トップ ナビゲーション)。|
|PlaceHolderQuickLaunchTop|サイド リンク バー ナビゲーションの上部 (トップ ナビゲーション)。|
|PlaceHolderSearchArea|検索ボックス コントロールが表示される領域 (検索ボックス)。|
|PlaceHolderSiteName|サイトの名前 (スイート ナビゲーション)。|
|PlaceHolderTitleAreaClass|ページのタイトル領域 (スイート ナビゲーション)。|
|PlaceHolderTitleAreaSeparator|タイトル領域のシャドー (スイート ナビゲーション)。|
|PlaceHolderTitleBreadCrumb|タイトル階層リンクのコンテンツ領域。 |
|PlaceHolderTitleLeftBorder|タイトル領域の左罫線 (スイート ナビゲーション)。|
|PlaceHolderTitleRightMargin|タイトル領域の左余白 (スイート ナビゲーション)。|
|PlaceHolderTopNavBar|トップ ナビゲーション領域 (トップ ナビゲーション)。|
|PlaceHolderUtilities|ページ下部に表示する必要のあるその他のコンテンツ (ページ コンテンツ)。|
|SPNavigation|ナビゲーション関連の操作が含まれます。|
|WSSDesignConsole|ページが編集モードの場合のページ編集コントロール。|

表 2 にリストされているいずれかのコンテンツ プレースホルダーを SharePoint .master ページから削除すると、SharePoint によってエラーがスローされます。コンテンツ プレースホルダーを非表示状態で追加できます。そのようにすると、エンド ユーザーからは対象コンテンツが見えなくなります。

詳しくは、「 [Windows SharePoint Services の既定のマスタ ページ](https://msdn.microsoft.com/en-us/library/office/ms467402%28v=office.12%29.aspx)」をご覧ください (この記事では、SharePoint Services 3 の機能について取り上げられていますが、内容が当てはまります)。また、「 [コンテンツ プレースホルダー コントロールを操作する](https://support.office.com/en-us/article/Working-with-content-placeholder-controls-d8b87b85-d1ef-409d-a5c7-044890f89288?CorrelationId=517ec37c-89ef-40d9-b70e-54aa63ac994f&amp;ui=en-US&amp;rs=en-US&amp;ad=US)」もご覧ください。

既定の SharePoint マスター ページ (seattle.master および oslo.master など) には、SharePoint が必要とする以上に多くのコンテンツ プレースホルダーが含まれています。たとえば、これらのマスター ページには  `<SharePoint:Themes runat="server">` コントロールと `<SharePoint.CssRegistration runat="server">` コントロールが入っています。

**Themes** コントロールと **CssRegistration** コントロールのどちらも、ページ ライフサイクルの間、実行されます。リモート プロビジョニング パターンを使用すると、カスタム アクションを使用することによって、カスタム CSS を登録するためのサーバー コントロールを追加できます。

表示されないコンテンツ プレースホルダーも想定どおりに作動しますが、それらのプレースホルダーによって生成されるコンテンツはブラウザーが認識する HTML ソースから省略されます。 `Visible="false"` に設定されているコンテンツ プレースホルダーは非表示になります。


                **重要:**Seattle.master や Oslo.master など、すぐに使用できる .master ページにあるカスタム マスター ページ内に、カスタム プレースホルダーを作成しないでください。 致命的な例外が発生します。

### SharePoint Online スイート ナビゲーションのコントロール

SharePoint Online では、トップ ナビゲーションをレンダリングする、 [ **スイート ナビゲーション**] コントロール用の新しいマスター ページ マークアップが導入されています。[ **スイート ナビゲーション**] コントロール用の既定のマークアップは、サイトがイントラネットと一般向けのどちらであるかによって異なります。[ **スイート ナビゲーション**] コントロールについて、およびイントラネットおよび一般向けのサイトのマスター ページに追加可能なコード例については、「[SharePoint Online Suite Navigation コントロール](http://msdn.microsoft.com/library/ba93e5c0-e591-48d0-a716-a08ec7ef6cea%28Office.15%29.aspx)」をご覧ください。

### CSOM による SharePoint ページ領域のカスタマイズ

一般に、リンクや他の要素の追加または削除を実行する場合は、 [UserCustomAction](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.usercustomaction.aspx) クラスを使用することをお勧めします。これは、 [CustomAction](https://msdn.microsoft.com/en-us/library/office/ms460194.aspx) 要素を使用する SharePoint の変化形で、完全に信頼されるコード モデルに精通している場合には機能フレームワークの一部と見なすことができます。 **CustomAction** 要素と機能フレームワーク プロビジョニング パターンはクライアント側オブジェクト モデル (CSOM) で特にサポートされていなわけではありませんが、 **CustomAction** 要素で利用できるのと同じ場所 ID は CSOM コードでも使用できます。

```
<CustomAction
  RequiredAdmin = "Delegated | Farm | Machine"
  ControlAssembly = "Text"
  ControlClass = "Text"
  ControlSrc = "Text"
  Description = "Text"
  FeatureId = "Text"
  GroupId = "Text"
  Id = "Text"
  ImageUrl = "Text"
  Location = "Text"
  RegistrationId = "Text"
  RegistrationType = "Text"
  RequireSiteAdministrator = "TRUE" | "FALSE"
  Rights = "Text"
  RootWebOnly = "TRUE" | "FALSE"
  ScriptSrc = "Text"
  ScriptBlock = "Text"
  Sequence = "Integer"
  ShowInLists = "TRUE" | "FALSE"
  ShowInReadOnlyContentTypes = "TRUE" | "FALSE"
  ShowInSealedContentTypes = "TRUE" | "FALSE"
  Title = "Text"
  UIVersion = "Integer">
</CustomAction>
```

### SharePoint リボンのカスタマイズ

リボンをカスタマイズする場合、サーバーがレンダリングする HTML は、カスタム スタイルに割り当てたクラス名に割り当てられます。この機能を使用するには、スタイル ライブラリにアクセスし、リボンに追加するスタイルごとに新しい CSS ファイルを作成します。カスタム スタイルは、リボンの [ **ページ要素**] と [ **テキスト スタイル**] という 2 つのセクションに追加できます。追加するスタイルには以下の構文を使用します。

- [ ** ページ要素**] セクションの場合:  `span.ms-rteElement-<yourowndefinedname>`。または、スタイル H1、H2、H3、H4 を使用することもできます。これらで、スタイルの適用先テキストを囲みます。
    
- [ **テキスト スタイル**] セクションの場合:  `.ms-rteEStyle-<yourowndefinedname>`。
    
次に、CSS クラス定義で、以下の行を追加します。 

`-ms-name:"The name visual in the ribbon for your style";`

これで、カスタム .css ファイルにおけるカスタム CSS クラスの詳細を定義する作業は終わりです。

### Web パーツ ページにおけるスイート ナビゲーションのカスタマイズ

SharePoint 2013 の UI は、ページ タイルに基づくモダンな外観になっています。たとえば、ライブ タイルは既定で SharePoint 2013 の既定ページに表示されます。トップ ナビゲーションによって、ユーザーは簡単にソーシャル コンテンツと OneDrive for Business を表示してアクセスできます。CSS と JavaScript を組み合わせて使用すると、これらの領域の外観をカスタマイズできます。

Web パーツ ページの作成後、利用可能な Web パーツ領域にスクリプト エディター Web パーツ (SEWP) を追加します。この Web パーツを使用して、JavaScript をページに追加できます。SEWP に対して、トップ ナビゲーションを  **ElementId** で識別し、可視性プロパティを非表示設定にする JavaScript コードを追加できます。

### [設定] メニューまたはギアのカスタマイズ

次のコード例に示されているように、 [UserCustomAction](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.usercustomaction.aspx) クラスとプロパティ バッグ エントリを使用して、SharePoint サイトの設定メニューをカスタマイズできます。

```
public void AddCustomActions(ClientContext clientContext)
{
    // Add a site settings link if it doesn't already exist.
    if (!CustomActionAlreadyExists(clientContext, "Sample_CustomSiteSetting"))
    {
        // Add a site settings link.
        UserCustomAction siteSettingLink = clientContext.Web.UserCustomActions.Add();
        siteSettingLink.Group = "SiteTasks";
        siteSettingLink.Location = "Microsoft.SharePoint.SiteSettings";
        siteSettingLink.Name = "Sample_CustomSiteSetting";
        siteSettingLink.Sequence = 1000;
        siteSettingLink.Url = string.Format(DeployManager.appUrl, clientContext.Url);
        siteSettingLink.Title = "Modify Site Metadata";
        siteSettingLink.Update();
        clientContext.ExecuteQuery();
    }

    // Add a site actions link, if it doesn't already exist.
    if (!CustomActionAlreadyExists(clientContext, "Sample_CustomAction"))
    {
        UserCustomAction siteAction = clientContext.Web.UserCustomActions.Add();
        siteAction.Group = "SiteActions";
        siteAction.Location = "Microsoft.SharePoint.StandardMenu";
        siteAction.Name = "Sample_CustomAction";
        siteAction.Sequence = 1000;
        siteAction.Url = string.Format(DeployManager.appUrl, clientContext.Url); ;
        siteAction.Title = "Modify Site Metadata";
        siteAction.Update();
        clientContext.ExecuteQuery();
    }
}
```

### ツリー ビューのカスタマイズ

ツリー ビューの幅を変更する場合, .master ページのツリー タグの周りに  `<div>` タグを追加し、スタイル幅属性が含まれる CSS クラスを `<div>` に割り当てます。以下のスタイル定義を .css ファイルに追加して、 [ **サイド リンク バー**] ナビゲーションの幅を増やします。

```
.ms-nav {
  width: 220 px;
}
```

### ページ コンテンツのカスタマイズ

ページ コンテンツのカスタマイズ要件は、ページに含まれているコンテンツによって異なります。[ **サイト アクション**] メニューのカスタマイズの場合、[UserCustomAction](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.usercustomaction.aspx) オブジェクトを使用して Web パーツに対してブランド化をプロビジョニングできます。

発行サイトを構築している場合、「 [[方法]: SharePoint 2013 でページ レイアウトを作成する方法](http://msdn.microsoft.com/library/5447e6a1-2f14-4667-81d0-7514b468be80%28Office.15%29.aspx)」をご覧になり、概要を確認してください。ページ レイアウトは、CSOM の [ContentTypeId](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.contenttypeid.aspx) クラスの可用性に依存します。CSOM で使用できる他の要素については、Windows Communication Foundation (WCF) サービスを **ContentTypeId** を使用して作業するための一時的な回避策として用いることができます。

## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint サイト ブランド化とページ カスタマイズのソリューション](SharePoint-site-branding-and-page-customization-solutions.md)
    
- [SharePoint 2013 および SharePoint Online のブランディングおよびサイト プロビジョニング ソリューション](Branding-and-site-provisioning-solutions-for-SharePoint.md)
