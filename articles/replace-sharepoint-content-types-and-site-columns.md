# SharePoint におけるコンテンツ タイプとサイト列の置換

CSOM を使用して、SharePoint コンテンツ タイプとサイト列を置換し、新しいコンテンツ タイプにサイト列を追加して、コンテンツ タイプを新しいコンテンツ タイプと置換します。

_**適用対象:** SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

SharePoint クライアント側オブジェクト モデル (CSOM) を使用して、コンテンツ タイプとサイト列を置換し、新しいコンテンツ タイプにサイト列を追加し、次に従来のコンテンツ タイプを新しいコンテンツ タイプと置換するときに使用する変換プロセスについて説明します。

**重要** ファーム ソリューションを SharePoint Online に移行することはできません。この記事で取り上げられている技法とコードを適用すると、更新されたコンテンツ タイプとサイト列を使用して、ファーム ソリューションと同様の機能や宣言型のサンドボックス ソリューションを提供する新しいソリューションを構築できます。その後、この新しいソリューションを SharePoint Online に展開できます。この記事のコードを、完全に作動するソリューションとするためにはさらにコードを追加することが必要です。たとえば、この記事では、Office 365 に対する認証を行う方法、必要な例外処理を実装する方法などについては取り上げていません。他のコード サンプルについては、[Office 365 Developer Patterns and Practices プロジェクト](https://github.com/OfficeDev/PnP)を参照してください。

## コンテンツ タイプとサイト列の置換

CSOM を使用して、コンテンツ タイプとサイト列を置換するには、次のようにします。

1. 新しいコンテンツ タイプを作成します。 
    
2. 新しいサイト列 (別名: フィールド) を作成します。
    
3. 新しいサイト列を新しいコンテンツ タイプに追加します。
    
4. 古いコンテンツ タイプの参照を新しいコンテンツ タイプに置換します。
    
次のコードで、 **Main** は CSOM を使用してコンテンツ タイプとサイト列を置換するために実行する操作順を示しています。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
static void Main(string[] args)
{
    using (var clientContext = new ClientContext("http://contoso.sharepoint.com"))
    {

        Web web = clientContext.Web;
        
        CreateContentType(clientContext, web);
        CreateSiteColumn(clientContext, web);
        AddSiteColumnToContentType(clientContext, web);

        // Replace the old content type with the new content type.
        ReplaceContentType(clientContext, web);
    }

}
```

次のコードで、 **GetContentTypeByName** は、現在のサイトから以下のようにしてコンテンツ タイプを取得します。

1. [Web.ContentTypes](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.web.contenttypes.aspx) プロパティを使用して [ContentTypeCollection](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttypecollection.aspx) (現在のサイトにおけるコンテンツ タイプのコレクション) を取得します。
    
2. サイトからコンテンツ タイプを検索して返します。その場合、サイトのコンテンツ タイプ名を既存のコンテンツ タイプ名と突き合わせます。コンテンツ タイプ名は **name** パラメーターで送信されます。

```C#
private static ContentType GetContentTypeByName(ClientContext cc, Web web, string name)
{
    ContentTypeCollection contentTypes = web.ContentTypes;
    cc.Load(contentTypes);
    cc.ExecuteQuery();
    return contentTypes.FirstOrDefault(o => o.Name == name);
}
```

次のコードで、 **CreateContentType** は、次のようにして新しいコンテンツ タイプを作成します。

1. コンテンツ タイプの名前を格納するために  **contentTypeName** という定数を作成します。新しいコンテンツ タイプの名前は、従来のコンテンツ タイプの名前に設定されます。
    
2. **GetContentTypeByName** を呼び出して、サイト上にある一致するコンテンツ タイプを検索します。
    
3. コンテンツ タイプが既に存在する場合には、追加アクションは不要で、 **return** が呼び出されるときに制御が **Main** に戻ります。コンテンツ タイプが存在しない場合、コンテンツ タイプのプロパティは、 [newCt](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttypecreationinformation.aspx) という **ContentTypeCreationInformation** オブジェクトを使用して設定されます。新しいコンテンツ タイプ ID は基本ドキュメントのコンテンツ タイプ ID **0x0101** を使用して **newCt.Id** に割り当てられます。詳細については、「 [コンテンツ タイプの基本的な階層](https://msdn.microsoft.com/library/office/ms452896%28v=office.14%29.aspx)」を参照してください。
    
4. [ContentTypeCollection.Add](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttypecollection.add.aspx) を使用して新しいコンテンツ タイプを追加します。

```C#
private static void CreateContentType(ClientContext cc, Web web)
{
    // The new content type will be created using this name.
    const string contentTypeName = "ContosoDocumentByCSOM";

    // Determine whether the content type already exists.
    var contentType = GetContentTypeByName(cc, web, contentTypeName);

    // The content type exists already. No further action required.
    if (contentType != null) return;

    // Create the content type using the ContentTypeInformation object.
    ContentTypeCreationInformation newCt = new ContentTypeCreationInformation();
    newCt.Name = "ContosoDocumentByCSOM";

    // Create the new content type based on the out-of-the-box document (0x0101) and assign the ID to the new content type.
    newCt.Id = "0x0101009189AB5D3D2647B580F011DA2F356FB2";

    // Assign the content type to a specific content type group.
    newCt.Group = "Contoso Content Types";

    ContentType myContentType = web.ContentTypes.Add(newCt);
    cc.ExecuteQuery();
}
```

次のコードで、 **CreateSiteColumn** は、以下のようにして、新しいサイト列を作成します。

1. フィールド名を格納するために  **fieldName** という定数を作成します。新しいフィールド名は、従来のフィールド名に設定されます。
    
2. [Web.Fields](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.web.fields.aspx) プロパティを使用して、サイト上に定義されているサイト列を取得します。
    
3. サイト上にある一致するフィールドを検出します。その際、サイト上のフィールド名を  **fieldName** に突き合わせます。対象フィールドが既に存在する場合には、追加アクションは不要で、 **return** が呼び出されると制御が **Main** に戻ります。対象フィールドが存在しない場合、フィールド スキーマで指定された CAML 文字列が **FieldAsXML** に割り当てられ、 [FieldCollection.AddFieldAsXml](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.fieldcollection.addfieldasxml.aspx) を使用してフィールドが作成されます。

```C#
private static void CreateSiteColumn(ClientContext cc, Web web)
{
    // The new field will be created using this name.
    const string fieldName = "ContosoStringCSOM";

    // Load the list of fields on the site.
    FieldCollection fields = web.Fields;
    cc.Load(fields);
    cc.ExecuteQuery();

    // Check fields on the site for a match.
    var fieldExists = fields.Any(f => f.InternalName == fieldName);

     // The field exists already. No further action required.    
    if (fieldExists) return;

    // Field does not exist, so create the new field.
    string FieldAsXML = @"<Field ID='{CB8E24F6-E1EE-4482-877B-19A51B4BE319}' 
                                Name='" + fieldName + @"' 
                                DisplayName='Contoso String by CSOM' 
                                Type='Text' 
                                Hidden='False' 
                                Group='Contoso Site Columns' 
                                Description='Contoso Text Field' />";
    Field fld = fields.AddFieldAsXml(FieldAsXML, true, AddFieldOptions.DefaultValue);
    cc.ExecuteQuery();
}
```

次のコードで、 **AddSiteColumnToContentType** は、コンテンツ タイプとフィールド間の接続を以下のようにして作成します。

1. コンテンツ タイプを読み込み、そのコンテンツ タイプを [ContentType.FieldLinks](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttype.fieldlinks.aspx) プロパティを使用してフィールドが参照します。
    
2. フィールドを読み込みます。
    
3. **contentType.FieldLinks.Any(f => f.Name == fieldName)** を使用して、フィールド名による突き合わせを実行することにより、コンテンツ タイプがフィールドを参照しているかどうかを判別します。
    
4.  コンテンツ タイプが対象フィールドを既に参照している場合、追加アクションは不要で、 **return** が呼び出されると制御が **Main** に戻ります。コンテンツ タイプが対象フィールドを参照していない場合には、フィールド参照プロパティは [FieldLinkCreationInformation](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.fieldlinkcreationinformation.aspx) オブジェクトで設定されます。
    
5. **FieldLinkCreationInformation** オブジェクトを **ContentType.FieldLinks** プロパティに追加します。

```C#
private static void AddSiteColumnToContentType(ClientContext cc, Web web)
{
    // The name of the content type. 
    const string contentTypeName = "ContosoDocumentByCSOM";
    // The field name
    const string fieldName = "ContosoStringCSOM";

    // Load the content type.
    var contentType = GetContentTypeByName(cc, web, contentTypeName);
    if (contentType == null) return; // content type was not found

    // Load field references in the content type.
    cc.Load(contentType.FieldLinks);
    cc.ExecuteQuery();

    // Load the new field.
    Field fld = web.Fields.GetByInternalNameOrTitle(fieldName);
    cc.Load(fld);
    cc.ExecuteQuery();

    // Determine whether the content type refers to the field.
    var hasFieldConnected = contentType.FieldLinks.Any(f => f.Name == fieldName);

    // A reference exists already, no further action is required.
    if (hasFieldConnected) return;

    // The reference does not exist, so we have to create the reference.
    FieldLinkCreationInformation link = new FieldLinkCreationInformation();
    link.Field = fld;
    contentType.FieldLinks.Add(link);
    contentType.Update(true);
    cc.ExecuteQuery();
}
```

次のコードで、**ReplaceContentType** は、すべてのライブラリにある項目すべてで古いコンテンツ タイプを参照するコンテンツを検査し、こうした参照を新しいコンテンツ タイプ (**ContosoDocumentByCSOM**) に以下のようにして置換します。

1. 古いコンテンツ タイプ ID を定数に割り当てます。
    
2. **GetContentTypeByName** を使用して新しいコンテンツ タイプを取得します。
    
3. [Web.Lists](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.web.lists.aspx) を使用して、サイト上のすべてのリストを取得します。
    
4. **cc.Load(lists, l => l.Include(list => list.ContentTypes)** を使用して、サイト上のすべてのリストを読み込み、各リストのコンテンツ タイプすべてを読み込みます。
    
5. 戻されるそれぞれのリストに関して、 **list.ContentTypes.Any(c => c.StringId.StartsWith(oldContentTypeId))** を使用してリストのコンテンツ タイプを検索し、コンテンツ タイプを古いコンテンツ タイプ ID と突き合わせます。一致項目が見つかると、古いコンテンツ タイプが含まれるリストは **listsWithContentType** に追加されます。
    
6. **listsWithContentType** の各リストについて、以下を行います。
    
  1. 新しいコンテンツ タイプがリストに追加されているかどうかを判別します。その新しいコンテンツ タイプがリストに追加されていない場合、 [ContentTypeCollection.AddExistingContentType ](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttypecollection.addexistingcontenttype.aspx) を使用して、対象の新しいコンテンツ タイプをリストに追加します。
    
  2. リストのすべての項目を取得します。
    
  3. それぞれのリスト項目に関して、リスト項目のコンテンツ タイプ ID を取得します。リスト項目のコンテンツ タイプ ID が古いコンテンツ タイプ ID と等しいかどうかを判別します。対象コンテンツ タイプ ID が等しくない場合、次のリスト項目にスキップます。対象コンテンツ タイプ ID が等しい場合、 [ContentType.StringId](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.contenttype.stringid.aspx) を使用して、その新しいコンテンツ タイプ ID をリスト項目に割り当てます。
    

**メモ:** 古いコンテンツ タイプはリストにあるものの、既に使用されていません。その場合、リストから古いコンテンツ タイプを削除し、取り消すことができます。この記事では、ドキュメントのコンテンツ タイプの置換方法のみを取り上げています。ページ レイアウトでコンテンツ タイプを置換する場合、サイト コレクションの各ページ レイアウトで [AssociatedContentType](https://msdn.microsoft.com/library/office/microsoft.sharepoint.publishing.pagelayout.associatedcontenttype.aspx) プロパティを必ず更新してください。

```C#
private static void ReplaceContentType(ClientContext cc, Web web)
{
    // The old content type. 
    const string oldContentTypeId = "0x010100C32DDAB6381C44868DCD5ADC4A5307D6";
    // The new content type name
    const string newContentTypeName = "ContosoDocumentByCSOM";

    // Get the new content type and lists on the site.
    ContentType newContentType = GetContentTypeByName(cc, web, newContentTypeName);
    ListCollection lists = web.Lists;
    
    // Load the new content type and the content types on all lists on the site. 
    cc.Load(newContentType);
    cc.Load(lists,
            l => l.Include(list => list.ContentTypes));
    cc.ExecuteQuery();
    var listsWithContentType = new List<List>();
    foreach (List list in lists)
    {
        bool hasOldContentType = list.ContentTypes.Any(c => c.StringId.StartsWith(oldContentTypeId));
        if (hasOldContentType)
        {
            listsWithContentType.Add(list);
        }
    }
    foreach (List list in listsWithContentType)
    {
        // Determine whether the new content type is already attached to the list.
        var listHasContentTypeAttached = list.ContentTypes.Any(c => c.Name == newContentTypeName);
        if (!listHasContentTypeAttached)
        {
            // Attach content type to list.
            list.ContentTypes.AddExistingContentType(newContentType);
            cc.ExecuteQuery();
        }
        // Get all list items.
        CamlQuery query = CamlQuery.CreateAllItemsQuery();
        ListItemCollection items = list.GetItems(query);
        cc.Load(items);
        cc.ExecuteQuery();

        // For each list item, determine whether the old content type is used, and then update to the new content type. 
        foreach (ListItem listItem in items)
        {
            // Get the current content type for this list item.
            var currentContentTypeId = listItem["ContentTypeId"] + "";
            var isOldContentTypeAssigned = currentContentTypeId.StartsWith(oldContentTypeId);

            // This item does not use the old content type - skip to next list item.
            if (!isOldContentTypeAssigned) continue;

            // Update the list item content type to the new content type.
            listItem["ContentTypeId"] = newContentType.StringId; // new content type Id;
            listItem.Update();
        }
        // Save all changes.
        cc.ExecuteQuery();
    }
}
```

## その他の技術情報
<a name="bk_addresources"> </a>

- [ファーム ソリューションの SharePoint アドイン モデルへの変換](Transform-farm-solutions-to-the-SharePoint-app-model.md)
    
- [SharePoint 2013](https://msdn.microsoft.com/library/office/jj162979.aspx)
