# SharePoint のリスト定義に基づいて作成されたリストの置換
SharePoint のリスト定義を使用して作成されたリストとライブラリを置換します。 

_**適用対象:** SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

リスト定義を使用してファーム ソリューションでリストを作成した場合、それらを、クライアント オブジェクト モデル (CSOM) を使用して同様の機能を提供する新しいソリューションに変換する方法について理解してください。この記事では、クライアント オブジェクト モデル (CSOM) を使用して以下の操作を行う方法について説明します。

- リスト定義を使用して作成されたリストとライブラリを検出します。
    
- 新しいリストを作成して構成します。
    
- リストにビューを追加したり、リストから削除したりします。
    
- 元のリストのコンテンツを新しいリストに移行します。

**重要** <p>ファーム ソリューションは、SharePoint Online に移行できません。 この記事で取り上げられている技法とコードを適用すると、ファーム ソリューションと同様の機能を提供する新しいソリューションを構築して、SharePoint Online に展開できます。 インプレース変換方式を使用している場合は、新しいソリューションを SharePoint Online に展開することが必要になることもあります。 スウィング方式またはコンテンツ移行方式を使用している場合は、サード パーティ製のツールでリストを作成してください。 詳細については、「[新しい SharePoint アドインを展開するための変換方法](https://msdn.microsoft.com/library/dn986827.aspx#sectionSection1)」を参照してください。 完全に機能するソリューションを実現するには、この記事のコードに、追加のコードが必要になります。 たとえば、この記事では、Office 365 に対する認証を実行する方法や、必要な例外処理を実装する方法などについては取り上げていません。 追加のコード サンプルについては、[Office 365 Developer Patterns and Practices プロジェクト](https://github.com/OfficeDev/PnP)を参照してください。</p><p>この記事で説明されている技法は、一度に 2、3 のリストのみを更新する場合に使用してください。 また、更新対象のリストを検索するときには、リストにリストの種類によるフィルターを適用しないようにしてください。</p>

## はじめに

既存のファーム ソリューションについて確認し、この記事で取り上げられている技法について理解し、その後、既存のファーム ソリューションにそうした技法を適用する方法を計画するのが理想的です。ファーム ソリューションに精通していない場合、または作業対象の既存のファーム ソリューションがない場合には、以下の事柄が役立つ可能性があります。

1. [Contoso.Intranet ソリューション](https://github.com/OfficeDev/PnP/tree/master/Reference%20Material/Contoso.Intranet)をダウンロードします。「 [置換するサイトとライブラリの初期状態を確認する](https://github.com/OfficeDev/TrainingContent/blob/master/O3658/10%20Transformation%20guidance%20from%20farm%20solutions%20to%20app%20model/10-2%20Replacing%20Lists%20Created%20from%20Custom%20Templates/Lab.md#examine-the-initial-state-of-the-site-and-library-for-replacement)」を調べて、リスト定義を使用してリストが宣言的に作成された方法の概要を把握します。
    
    
                **メモ:**Contoso.Intranet の場合、SP\ListTemplate\LTContosoLibrary の elements.xml ファイル内ではカスタム リスト テンプレートの ID は 10003 です。 これは、リストの作成に使ったテンプレートを識別するのに使います。 また、リストに定義されている構成設定とビューを確認します。

2. ファーム ソリューションについて理解します。 詳細については、「[SharePoint 2010 アーキテクチャの概要](https://msdn.microsoft.com/en-us/library/office/gg552610%28v=office.14%29.aspx)」および「[SharePoint 2013 でのファーム ソリューションの作成](https://msdn.microsoft.com/library/jj163902.aspx)」を参照してください。
    
## リスト定義に基づいて作成されたリストの置換

リスト定義から作成されたリストを置換するには、次のようにします。

1. 特定の基本テンプレートを使用して作成されたリストを検出します。
    
2. すぐに使用可能なリスト定義を使用して新しいリストを作成します。 
    
3. リスト設定を構成します。
    
4. 元のリストで設定されていたコンテンツ タイプに基づいて新しいリストでコンテンツ タイプを設定します。
    
5. (省略可能) ビューを追加します。 
    
6. (省略可能) ビューを削除します。
    
7. 元のリストのコンテンツを新しいリストに移行します。
    
次のコードで、特定の基本テンプレートを使用して作成されたリストを検出する方法を示しています。この方式では、以下の操作が実行されます。

1. [ClientContext](https://msdn.microsoft.com/library/microsoft.sharepoint.client.clientcontext.aspx) を [Web.Lists](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.web.lists.aspx) と一緒に使用して、現在の Web にあるすべてのリストを取得します。Include ステートメントをラムダ式で使用して、リストのコレクションを戻します。戻されるリストには、 [BaseTemplate](https://msdn.microsoft.com/library/microsoft.sharepoint.client.list.basetemplate.aspx)、 [BaseType](https://msdn.microsoft.com/library/microsoft.sharepoint.client.list.basetype.aspx)、 [Title](https://msdn.microsoft.com/library/microsoft.sharepoint.client.list.title.aspx) で指定されたすべてのプロパティ値が含まれるはずです。
    
2. 戻されたリスト コレクション内の各リストで、 **List.BaseTemplate** が 10003 の場合、対象リストを置換対象のリスト コレクション ( **listsToReplace** ) に追加します。10003 は、Contoso.Intranet サンプルを確認したときのカスタムのリスト テンプレート ID です。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
static void Main(string[] args)
{
    using (var clientContext = new ClientContext("http://w15-sp/sites/ftclab"))
    {
        Web web = clientContext.Web;
        ListCollection listCollection = web.Lists;
        clientContext.Load(listCollection,
                            l => l.Include(list => list.BaseTemplate,
                                            list => list.BaseType,
                                            list => list.Title));
        clientContext.ExecuteQuery();
        var listsToReplace = new List<List>();
        foreach (List list in listCollection)
        {
            // 10003 is the custom list template ID of the list template you're replacing.
            if (list.BaseTemplate == 10003)
            {
                listsToReplace.Add(list);
            }
        }
        foreach (List list in listsToReplace)
        {
            ReplaceList(clientContext, listCollection, list);
        }
    }
}
```


                **重要:**前述のコードでは、最初に ListCollection を反復して変更が必要なリストを選択しています。その後で、リストの変更を開始する ReplaceList を呼び出しています。 このパターンが必要になる理由は、コレクションの反復中にコレクションのコンテンツに変更があると、例外がスローされるためです。

置換が必要なリストを特定した後、**ReplaceList** でリストの置換を実行する操作順を示します。

```C#
private static void ReplaceList(ClientContext clientContext, ListCollection listCollection, List listToBeReplaced)
{
    var newList = CreateReplacementList(clientContext, listCollection, listToBeReplaced);

    SetListSettings(clientContext, listToBeReplaced, newList);

    SetContentTypes(clientContext, listToBeReplaced, newList);

    AddViews(clientContext, listToBeReplaced, newList);

    RemoveViews(clientContext, listToBeReplaced, newList);

    MigrateContent(clientContext, listToBeReplaced, newList);
}
```

新しいリストを作成するために、 **CreateReplacementList** では [ListCreationInformation](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.listcreationinformation.aspx) を使用します。新しいリストのタイトルは、既存のリストのタイトルに Add-in が追加された名前に設定されます。 [ListTemplateType](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.listtemplatetype.aspx) 列挙体が、リストのテンプレートの種類をドキュメント ライブラリに設定するときに使用されます。別のテンプレートの種類に基づいてリストを作成する場合には、正しいテンプレートの種類を使用していることを確認してください。たとえば、予定表リストを作成する場合、 **ListTemplateType.DocumentLibrary** ではなく **ListTemplateType.Events** を使用してください。

```C#
private static List CreateReplacementList(ClientContext clientContext, ListCollection lists,List listToBeReplaced)
{
    var creationInformation = new ListCreationInformation
    {
        Title = listToBeReplaced.Title + "add-in",
        TemplateType = (int) ListTemplateType.DocumentLibrary,
    };
    List newList = lists.Add(creationInformation);
    clientContext.ExecuteQuery();
    return newList;
}
```

**SetListSettings** は、元のリスト設定を以下のようにして新しいリストに適用します。

1. 元のリストの各種リスト設定を取得します。
    
2. 元のリストのリスト設定を新しいリストに適用します。

```C#
private static void SetListSettings(ClientContext clientContext, List listToBeReplaced, List newList)
{
    clientContext.Load(listToBeReplaced, 
                        l => l.EnableVersioning, 
                        l => l.EnableModeration, 
                        l => l.EnableMinorVersions,
                        l => l.DraftVersionVisibility );
    clientContext.ExecuteQuery();
    newList.EnableVersioning = listToBeReplaced.EnableVersioning;
    newList.EnableModeration = listToBeReplaced.EnableModeration;
    newList.EnableMinorVersions= listToBeReplaced.EnableMinorVersions;
    newList.DraftVersionVisibility = listToBeReplaced.DraftVersionVisibility;
    newList.Update();
    clientContext.ExecuteQuery();
}
```


                **メモ:**それぞれの要件に応じて、元のリストのリスト設定は異なる可能性があります。 自分のリスト設定を確認して、元のリスト設定が確実に新しいリストに適用されるように **SetListSettings** を変更してください。

**SetContentTypes** は、次のようにして新しいリストにコンテンツ タイプを設定します。

1. 元のリストからコンテンツ タイプ情報を取得します。
    
2. 新しいリストからコンテンツ タイプ情報を取得します。
    
3. 元のリストでコンテンツ タイプが有効かどうかを判別します。元のリストでコンテンツ タイプが有効ではない場合、 **SetContentTypes** は終了します。元のリストでコンテンツ タイプが有効な場合には、 **SetContentTypes** は新しいリストのコンテンツ タイプで有効になります。その際、 **newList.ContentTypesEnabled = true** を使用します。
    
4. 元のリストのそれぞれのコンテンツ タイプに関して、 **newList.ContentTypes.Any(ct => ct.Name == contentType.Name)** を使用してコンテンツ タイプの名前に基づいて新しいリストのコンテンツ タイプで一致するコンテンツ タイプがないかどうか検索します。対象のコンテンツ タイプが新しいリストには含まれていない場合、リストに追加されます。
    
5. **AddExistingContentType** によってコンテンツ タイプが変更されている可能性があるため、 [newList](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttypecollection.addexistingcontenttype.aspx) が再び読み込まれます。です。
    
6. **newList** のそれぞれのコンテンツ タイプに関して、 [ContentType.Name](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttype.name.aspx) に基づいて元のリストのコンテンツ タイプと対象コンテンツ タイプが一致するかどうかを **listToBeReplaced.ContentTypes.Any(ct => ct.Name == contentType.Name)** を使用して判別します。一致項目が見つからないと、対象コンテンツ タイプが新しいリストから削除されることになる **contentTypesToDelete** に追加されます。
    
7. [ContentType.DeleteObject](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttype.deleteobject.aspx) を呼び出して、コンテンツ タイプを削除します。


            **メモ:**インプレース変換方式を使用しているときに、機能フレームワークを使用してコンテンツ タイプが宣言的に展開されていた場合は、次に示す操作が必要になります。 

 1. 新しいコンテンツ タイプを作成します。

 2. 元のリストのコンテンツを新しいリストに MigrateContent を使用して移行するときに、リスト項目にコンテンツ タイプを設定します。


```C#
private static void SetContentTypes(ClientContext clientContext, List listToBeReplaced, List newList)
{
    clientContext.Load(listToBeReplaced,
                        l => l.ContentTypesEnabled,
                        l => l.ContentTypes);
    clientContext.Load(newList,
                        l => l.ContentTypesEnabled,
                        l => l.ContentTypes);
    clientContext.ExecuteQuery();

    // If the original list doesn't use content types, do not proceed any further.
    if (!listToBeReplaced.ContentTypesEnabled) return;

    newList.ContentTypesEnabled = true;
    newList.Update();
    clientContext.ExecuteQuery();
    foreach (var contentType in listToBeReplaced.ContentTypes)
    {
        if (!newList.ContentTypes.Any(ct => ct.Name == contentType.Name))
        {
            // Current content type needs to be added to the new list. Note that the content type is added to the list, not the site.           
            newList.ContentTypes.AddExistingContentType(contentType.Parent);
            newList.Update();
            clientContext.ExecuteQuery();
        }
    }
    // Reload the content types on newList because they might have changed when AddExistingContentType was called. 
    clientContext.Load(newList, l => l.ContentTypes);
    clientContext.ExecuteQuery();
    // Remove any content types that are not needed.
    var contentTypesToDelete = new List<ContentType>();
    foreach (var contentType in newList.ContentTypes)
    {
        if (!listToBeReplaced.ContentTypes.Any(ct => ct.Name == contentType.Name))
        {
            // Current content type needs to be removed from new list.
            contentTypesToDelete.Add(contentType);
        }
    }
    foreach (var contentType in contentTypesToDelete)
    {
        contentType.DeleteObject();
    }
    newList.Update();
    clientContext.ExecuteQuery();
}
```


                **メモ:**この時点で、新しいリストは元のリストからのコンテンツを受け入れることができます。 また、オプションとしてビューの追加と削除ができます。 

ユーザーは、ビジネス上のニーズに応じて、リストにビューを追加したり、定義されているビューを削除したりできます。このため、新しいリストでビューの追加または削除を行える必要があります。 **AddViews** は、次のようにして新しいリストに元のリストのビューを追加します。

1. [List.Views](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.list.views.aspx) を使用して、元のリストのすべてのビューを取得します。
    
2. ラムダ式を使用して、 **views** コレクションの各種ビュー プロパティを読み込みます。
    
3. 元のリストのそれぞれのビューに関して、 [ViewCreationInformation](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.viewcreationinformation.aspx) を使用して、ビューを作成します。各種プロパティが、元のビューのビュー プロパティに基づいて新しいビューに設定されます。 **GetViewType** ヘルパー メソッドを呼び出して、ビューの [ViewType](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.viewtype.aspx) を判別します。次に、新しいビューを **viewsToCreate** というビュー リストに追加します。
    
4. リストの Views コレクションに対して  **Add** メソッドを使用することによって、 **List.Views** コレクションにビューを追加します。

```C#
private static void AddViews(ClientContext clientContext, List listToBeReplaced, List newList)
{
    ViewCollection views = listToBeReplaced.Views;
    clientContext.Load(views,
                        v => v.Include(view => view.Paged,
                            view => view.PersonalView,
                            view => view.ViewQuery,
                            view => view.Title,
                            view => view.RowLimit,
                            view => view.DefaultView,
                            view => view.ViewFields,
                            view => view.ViewType));
    clientContext.ExecuteQuery();

    // Build a list of views which exist on the original list only.
    var viewsToCreate = new List<ViewCreationInformation>();
    foreach (View view in listToBeReplaced.Views)
    {
      var createInfo = new ViewCreationInformation
      {
          Paged = view.Paged,
          PersonalView = view.PersonalView,
          Query = view.ViewQuery,
          Title = view.Title,
          RowLimit = view.RowLimit,
          SetAsDefaultView = view.DefaultView,
          ViewFields = view.ViewFields.ToArray(),
          ViewTypeKind = GetViewType(view.ViewType),
      };
      viewsToCreate.Add(createInfo);
    }
    
    foreach (ViewCreationInformation newView in viewsToCreate)
    {
        newList.Views.Add(newView);
    }
    newList.Update();
}

private static ViewType GetViewType(string viewType)
{
    switch (viewType)
    {
        case "HTML":
            return ViewType.Html;
        case "GRID":
            return ViewType.Grid;
        case "CALENDAR":
            return ViewType.Calendar;
        case "RECURRENCE":
            return ViewType.Recurrence;
        case "CHART":
            return ViewType.Chart;
        case "GANTT":
            return ViewType.Gantt;
        default:
            return ViewType.None;
    }
}
```

次のコードで、 **RemoveViews** は、次のようにして新しいリストからビューを削除します。

1. **List.Views** プロパティを使用して、新しいリストのリスト ビューを取得します。
    
2. 新しいリストのそれぞれのビューに関して、 **!listToBeReplaced.Views.Any(v => v.Title == view.Title** を使用してビューのタイトルを突き合わせて元のリストに対象ビューが存在しないことを判別します。新しいリスト内のビューが元のリスト内のビューと一致しない場合、そのビューを **viewsToRemove** に追加します。
    
3. **viewsToRemove** にあるすべてのビューを削除します。その際、 [View.DeleteObject](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.view.deleteobject.aspx) を使用します。
    
```C#
private static void RemoveViews(ClientContext clientContext, List listToBeReplaced, List newList)
{
    // Get the list of views on the new list.
    clientContext.Load(newList, l => l.Views);
    clientContext.ExecuteQuery();

    var viewsToRemove = new List<View>();
    foreach (View view in newList.Views)
    {
        if (!listToBeReplaced.Views.Any(v => v.Title == view.Title))
        {
            // If there is no matching view in the source list, add the view to the list of views to be deleted.
            viewsToRemove.Add(view);
        }
    }
    foreach (View view in viewsToRemove)
    {
        view.DeleteObject();
    }
    newList.Update();
    clientContext.ExecuteQuery();
}
```

**MigrateContent** は、次のようにして元のリストにコンテンツを新しいリストに移行します。

1. ファイルのコピー先を取得します。または、 [List.RootFolder](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.list.rootfolder.aspx) を使用して新しいリストのルート フォルダーを取得します。宛先リスト フォルダーのサーバー相対 URL を、 [Folder.ServerRelativeUrl](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.folder.serverrelativeurl.aspx) を使用して取得します。
    
2. ファイルのソースを取得します。または、 **List.RootFolder** を使用して元のリストのルート フォルダーを取得します。ソースのルート フォルダー内のリスト フォルダーとすべてのファイルのサーバー相対 URL を、 **clientContext** オブジェクトを使用して読み込みます。
    
3. ソースの各ファイルに関して、 **newUrl** を作成します。ここに、ファイルの新しい URL を格納します。 **newUrl** は、ソース ルート フォルダーを宛先のルート フォルダーに置換して作成されます。
    
4. [File.CopyTo](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.file.copyto.aspx) を使用して、ファイルを宛先ルート フォルダーの URL にコピーします。または、 [File.MoveTo](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.file.moveto.aspx) メソッドを使用して宛先 URL にファイルを移動することもできます。
    

                **メモ:**次に示すコードは、すべてのリスト項目を返します。 運用環境では、ループを実装して、少数のリスト項目を移行する複数の反復処理を使用することで、このコードを最適化することを検討してください。

```C#
private static void MigrateContent(ClientContext clientContext, List listToBeReplaced, List newList)
{
    ListItemCollection items = listToBeReplaced.GetItems(CamlQuery.CreateAllItemsQuery());
    Folder destination = newList.RootFolder;
    Folder source = listToBeReplaced.RootFolder;
    clientContext.Load(destination,
                        d => d.ServerRelativeUrl);
    clientContext.Load(source,
                        s => s.Files,
                        s => s.ServerRelativeUrl);
    clientContext.Load(items,
                        i => i.IncludeWithDefaultProperties(item => item.File));
    clientContext.ExecuteQuery();


    foreach (File file in source.Files)
    {
        string newUrl = file.ServerRelativeUrl.Replace(source.ServerRelativeUrl, destination.ServerRelativeUrl);
          file.CopyTo(newUrl, true);
          //file.MoveTo(newUrl, MoveOperations.Overwrite);
    }
    clientContext.ExecuteQuery();
}
```


                **メモ:**前述のコードは、リストのルート フォルダーに格納されているファイルを移行する方法を示しています。 リストにサブフォルダーが含まれている場合は、サブフォルダーとその内容を移行するための追加のコードが必要になります。 リストでワークフローを使用する場合は、ワークフローをリストに関連付けるための追加のコードが必要必要になります。

## その他のリソース
<a name="bk_addresources"> </a>

- [ファーム ソリューションの SharePoint アドイン モデルへの変換](Transform-farm-solutions-to-the-SharePoint-app-model.md)
    
- [SharePoint 2013](https://msdn.microsoft.com/library/office/jj162979.aspx)
