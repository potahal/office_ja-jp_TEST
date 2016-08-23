# SharePoint Web パーツをアドイン パーツに置き換える

変換プロセスを使用し、SharePoint クライアント オブジェクト モデル (CSOM) を使用して、Web パーツをアドイン パーツに置換します。

_**適用対象:** SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

変換プロセスを使用して、ページ上にある SharePoint Web パーツをアドイン パーツに置換できます。その際、CSOM を使用して特定の Web パーツを検索および削除し、新しいアドイン パーツを追加します。

**重要:** ファーム ソリューションを SharePoint Online に移行することはできません。この記事で取り上げられている技法とコードを適用すると、ファーム ソリューションと同様の機能を提供する新しいソリューションを構築し、それを SharePoint Online に展開できます。こうした技法を適用すると アドイン パーツを使用するようにページが更新されるので、SharePoint Online に移行できます。この記事のコードを完全に作動するソリューションとするためには、さらにコードを追加することが必要です。たとえば、この記事では、Office 365 に対する認証を行う方法、必要な例外処理を実装する方法などについては取り上げていません。他のコード サンプルについては、[Office 365 Developer Patterns and Practices プロジェクト](https://github.com/OfficeDev/PnP)を参照してください。

## 始める前に

この記事に記されている手順を実行して Web パーツをアドイン パーツに置換する前に、以下を確認してください。

- 使用している SharePoint 環境が アドイン をサポートするように構成されていますか。SharePoint Online はアドインをサポートするように構成されています。オンプレミスの SharePoint Server 2013 を使用している場合には、「[SharePoint アドインの環境を構成する (SharePoint 2013)](https://technet.microsoft.com/library/fp161236%28v=office.15%29)」をご覧ください。
    
- 新しいアドイン パーツが SharePoint に展開されていますか。
    
- **Web** でアドインに **FullControl** アクセス許可を割り当てます。詳細については、「 [SharePoint 2013 アドインのアクセス許可](https://msdn.microsoft.com/library/office/fp142383.aspx)」を参照してください。

## Web パーツをアドイン パーツに置換する

Web パーツを新しいアドイン パーツに置換するには次のようにします。

1. 新しいアドイン パーツをエクスポートします。エクスポートされたアドイン パーツを使用して、アドイン パーツ定義を取得します。 
    
2. 置換対象の Web パーツが含まれるページすべてを検出し、それらの Web パーツを削除します。 
    
3. アドイン パーツ 定義を使用して、ページ上に新しいアドイン パーツを作成します。 
    
CSOM を使用して Web パーツをアドイン パーツに置換するには、アドイン パーツをエクスポートしてアドイン パーツ定義を取得する必要があります。アドイン パーツの定義を取得するためにアドイン パーツをエクスポートするには、次のようにします。

1. SharePoint サイトを開き、[ **設定**] を選択するか、ギア アイコンを選択し、[ **ページの編集**] を選択します。
    
2. リボンで、[ **挿入**]  >  [ **アドイン パーツ**] と選択します。
    
3. ご使用のアドイン パーツを選択し、[ **追加**] を選択します。
    
4. アドイン パーツがページに追加されたら、アドイン パーツの右上隅にある下矢印を選択し、[ **Web パーツの編集**] を選択します。
    
5. [ **詳細**] の [ **エクスポート モード**] で [  **すべてのデータをエクスポート**]、[ **OK**] の順に選択します。
    
6. ご使用のアドイン パーツが表示された編集ページに戻り、アドイン パーツの右上隅にある下矢印を選択して、[ **エクスポート**] を選択します。
    
7. [ **保存**] を選択します。
    
8. ダウンロードの完了後、[ **フォルダーを開く**] を選択します。
    
9. [ **メモ帳**] を開き、[ **ファイル**]  >  [ **開く**] と選択します。
    
10. ダウンロードしたアドイン パーツ ファイルを選択し、[ **開く**] を選択してアドイン パーツ定義を表示します。
    
**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```XML
<webParts>
  <webPart xmlns="http://schemas.microsoft.com/WebPart/v3">
    <metaData>
      <type name="Microsoft.SharePoint.WebPartPages.ClientWebPart, Microsoft.SharePoint, Version=16.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c" />
      <importErrorMessage>Cannot import this Web Part.</importErrorMessage>
    </metaData>
    <data>
      <properties>
        <property name="TitleIconImageUrl" type="string" />
        <property name="Direction" type="direction">NotSet</property>
        <property name="ExportMode" type="exportmode">All</property>
        <property name="HelpUrl" type="string" />
        <property name="Hidden" type="bool">False</property>
        <property name="Description" type="string">WelcomeAppPart Description</property>
        <property name="FeatureId" type="System.Guid, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">0b846986-3474-4f1a-93cf-b7817ef057f9</property>
        <property name="CatalogIconImageUrl" type="string" />
        <property name="Title" type="string">WelcomeAppPart</property>
        <property name="AllowHide" type="bool">True</property>
        <property name="ProductWebId" type="System.Guid, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">741c5404-f43e-4f01-acfb-fcd100fc7d24</property>
        <property name="AllowZoneChange" type="bool">True</property>
        <property name="TitleUrl" type="string" />
        <property name="ChromeType" type="chrometype">Default</property>
        <property name="AllowConnect" type="bool">True</property>
        <property name="Width" type="unit" />
        <property name="Height" type="unit" />
        <property name="WebPartName" type="string">WelcomeAppPart</property>
        <property name="HelpMode" type="helpmode">Navigate</property>
        <property name="AllowEdit" type="bool">True</property>
        <property name="AllowMinimize" type="bool">True</property>
        <property name="ProductId" type="System.Guid, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">0b846986-3474-4f1a-93cf-b7817ef057f8</property>
        <property name="AllowClose" type="bool">True</property>
        <property name="ChromeState" type="chromestate">Normal</property>
      </properties>
    </data>
  </webPart>
</webParts>
```

CSOM コードでアドイン パーツ定義を使用するには、次のようにします。

- アドイン パーツ定義にあるすべての二重引用符 (") を二重引用符のペア ("") に置き換えます。
    
- アドイン パーツ定義を、CSOM コードで使用する文字列に割り当てます。

```C#
private const string appPartXml = @"<webParts>
  <webPart xmlns=""http://schemas.microsoft.com/WebPart/v3"">
    <metaData>
      <type name=""Microsoft.SharePoint.WebPartPages.ClientWebPart, Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"" />
      <importErrorMessage>Cannot import this Web Part.</importErrorMessage>
    </metaData>
    <data>
      <properties>
        <property name=""TitleIconImageUrl"" type=""string"" />
        <property name=""Direction"" type=""direction"">NotSet</property>
        <property name=""ExportMode"" type=""exportmode"">All</property>
        <property name=""HelpUrl"" type=""string"" />
        <property name=""Hidden"" type=""bool"">False</property>
        <property name=""Description"" type=""string"">WelcomeAppPart Description</property>
        <property name=""FeatureId"" type=""System.Guid, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"">0b846986-3474-4f1a-93cf-b7817ef057f9</property>
        <property name=""CatalogIconImageUrl"" type=""string"" />
        <property name=""Title"" type=""string"">WelcomeAppPart Title</property>
        <property name=""AllowHide"" type=""bool"">True</property>
        <property name=""ProductWebId"" type=""System.Guid, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"">717c00a1-08ea-41a5-a2b7-4c8f9c1ce770</property>
        <property name=""AllowZoneChange"" type=""bool"">True</property>
        <property name=""TitleUrl"" type=""string"" />
        <property name=""ChromeType"" type=""chrometype"">Default</property>
        <property name=""AllowConnect"" type=""bool"">True</property>
        <property name=""Width"" type=""unit"" />
        <property name=""Height"" type=""unit"" />
        <property name=""WebPartName"" type=""string"">WelcomeAppPart</property>
        <property name=""HelpMode"" type=""helpmode"">Navigate</property>
        <property name=""AllowEdit"" type=""bool"">True</property>
        <property name=""AllowMinimize"" type=""bool"">True</property>
        <property name=""ProductId"" type=""System.Guid, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"">0b846986-3474-4f1a-93cf-b7817ef057f8</property>
        <property name=""AllowClose"" type=""bool"">True</property>
        <property name=""ChromeState"" type=""chromestate"">Normal</property>
      </properties>
    </data>
  </webPart>
</webParts>";
```

**ReplaceWebPartsWithAppParts** は、次のようにして、置換対象の Web パーツの検出プロセスを開始します。

1. [Web](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.web.aspx) からいくつかのプロパティを取得して、サイト上の **Pages** ライブラリを見つけます。
    
2. **Pages** ライブラリで、リスト項目と、そのリスト項目に関連付けられているファイルを取得します。
    
3. **Pages** ライブラリから戻されるそれぞれのリスト項目に関して、 **FindWebPartToReplace** を呼び出します。

```C#
protected void ReplaceWebPartsWithAppParts(object sender, EventArgs e)
{
    var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);
    using (var clientContext = spContext.CreateUserClientContextForSPHost())
    {
        Web web = clientContext.Web;
        // Get properties from the Web.
        clientContext.Load(web,
                            w => w.ServerRelativeUrl,
                            w => w.AllProperties);
        clientContext.ExecuteQuery();
        // Read the Pages library name from the Web properties.
        var pagesListName = web.AllProperties["__pageslistname"] as string;

        var list = web.Lists.GetByTitle(pagesListName);
        var items = list.GetItems(CamlQuery.CreateAllItemsQuery());
        // Get the file associated with each list item.
        clientContext.Load(items,
                            i => i.Include(
                                    item => item.File));
        clientContext.ExecuteQuery();

        // Iterate through all pages in the Pages list.
        foreach (var item in items)
        {
            FindWebPartForReplacement(item, clientContext, web);
        }
    }
}
```

**FindWebPartToReplace** は、次のようにして新しいアドイン パーツに置換する必要がある Web パーツを検出します。

1. **page** を、 **Pages** ライブラリから戻されるリスト項目と関連付けられているファイルに設定します。
    
2. [LimitedWebPartManager](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.webparts.limitedwebpartmanager.aspx) を使用して、特定のページ上のすべての Web パーツを検出します。各 Web パーツのタイトルも、ラムダ式で戻ります。
    
3. Web パーツ マネージャーの [WebParts](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.webparts.limitedwebpartmanager.webparts.aspx) プロパティにあるそれぞれの Web パーツに関して、Web パーツのタイトルと **oldWebPartTitle** 変数 (置換前の Web パーツのタイトルに設定されています) が等しいかどうか判別します。Web パーツ タイトルと **oldWebPartTitle** が等しい場合には、 **ReplaceWebPart** を呼び出します。等しくない場合には、 **foreach** ループの次の反復を続行します。

```C#
private static void FindWebPartToReplace(ListItem item, ClientContext clientContext, Web web)
{
    File page = item.File;
    // Requires Full Control permissions on the Web.
    LimitedWebPartManager webPartManager = page.GetLimitedWebPartManager(PersonalizationScope.Shared);
    clientContext.Load(webPartManager,
                        wpm => wpm.WebParts,
                        wpm => wpm.WebParts.Include(
                                            wp => wp.WebPart.Title));
    clientContext.ExecuteQuery();

    foreach (var oldWebPartDefinition in webPartManager.WebParts)
    {
        var oldWebPart = oldWebPartDefinition.WebPart;
        // Modify the Web Part if we find an old Web Part with the same title.
        if (oldWebPart.Title != oldWebPartTitle) continue;

        ReplaceWebPart(web, item, webPartManager, oldWebPartDefinition, clientContext, page);
    }
}
```

**ReplaceWebPart** は、次のようにして、新しい Web パーツを置換します。

1. [LimitedWebPartManager.ImportWebPart ](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.webparts.limitedwebpartmanager.importwebpart.aspx) を使用して、 **appPartXml** 内の XML 文字列を [WebPartDefinition](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.webparts.webpartdefinition.aspx) に変換します。
    
2. [LimitedWebPartManager.AddWebPart](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.webparts.limitedwebpartmanager.addwebpart.aspx) を使用して、Web パーツを、特定の Web パーツ領域のページに追加します。 **zoneId** を **AddWebPart** に渡してください。そうしないと、 **ExecuteQuery** の実行時に例外がスローされます。
    
3. [WebPartDefinition.DeleteWebPart](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.webparts.webpartdefinition.deletewebpart.aspx) を使用して、ページから古い Web パーツを削除します。

```C#
private static void ReplaceWebPart(Web web, ListItem item, LimitedWebPartManager webPartManager,
      WebPartDefinition oldWebPartDefinition, ClientContext clientContext, File page)
  {
     
      // Create a Web Part definition using the XML string.
      var definition = webPartManager.ImportWebPart(appPartXml);
      webPartManager.AddWebPart(definition.WebPart, "RightColumn", 0);

      // Delete the old Web Part from the page.
      oldWebPartDefinition.DeleteWebPart();
      clientContext.Load(page,
                          p => p.CheckOutType,
                          p => p.Level);

      clientContext.ExecuteQuery();
     
  }
```

## その他のリソース
<a name="bk_addresources"> </a>

- [ファーム ソリューションの SharePoint アドイン モデルへの変換](Transform-farm-solutions-to-the-SharePoint-app-model.md)
    
- [Provisioning.Pages](https://github.com/OfficeDev/PnP/tree/master/Scenarios/Provisioning.Pages)
