# SharePoint サイトの分類ソリューションを実装する

SharePoint でサイト分類ソリューションを実装します。

_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

優れたガバナンスがあっても SharePoint サイトが急激に増えて制御不能になる場合があります。サイトは必要に応じて作成されますが、削除されることはまれです。不使用のサイト コレクションのせいで検索クロールに負荷がかかり、検索結果は古く関連性のないものになります。サイト分類を使用すると、重要なデータを特定して保持できます。この記事では、 [Core.SiteClassification](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.SiteClassification) サンプルを使用してサイト分類ソリューションを実装する方法、および SharePoint サイト ポリシーを使用して強制的に削除する方法を示します。このソリューションを既存のサイト準備ソリューションと統合することによって、サイトの管理機能を向上させることができます。

## 始める前に

まず、 [Core.SiteClassification](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.SiteClassification) サンプルを、GitHub の [Office 365 Developer パターンおよびプラクティス](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## サイト ポリシーの定義と設定

最初に、すべてのサイト コレクションで利用できるようにするサイト ポリシーを定義して設定する必要があります。 Core.SiteClassification サンプルは SharePoint Online MT に適用されますが、SharePoint Online 専用または SharePoint オンプレミスでも使用できます。 サイト ポリシーは [コンテンツ タイプ ハブ] で設定します。SharePoint Online MT では `https://[tenanatname]/sites/contentTypeHub` にあります。 サイト ポリシーを設定するには、**[設定]** > **[サイト コレクション管理]** > **[サイト ポリシー]** > **[作成]** の順に移動します。 **[新しいサイト ポリシー]** ページが表示されます。 サイト ポリシーのオプションの詳細については、「[SharePoint 2013 のサイト ポリシーの概要](http://technet.microsoft.com/en-US/library/jj219569%28v=office.15%29.aspx)」を参照してください。

**[新しいサイト ポリシー]** ページでは、次に示すフィールドに入力します。

-  **名前:** HBI
    
-  **説明:** これはトップ シークレットです。
    
- [ **サイトを自動的に削除する**] ラジオ ボタンを設定します。
    
- [ **削除イベント:**] は、サイトの作成日 + 1 年間にします。
    
- [ **削除までの残りが次の値になったら、サイトの所有者に電子メール通知を送信します:**] チェック ボックスをオンにし、1 か月に設定します。
    
- [ **次の間隔でフォロー アップ通知を送信します:**] チェック ボックスをオンにし、14 日間に設定します。
    
- [ **所有者は間近に迫った削除を次の期間だけ延長できます:**] チェック ボックスをオンにし、1 か月に設定します。
    
- [ **サイト コレクションを閉じると読み取り専用になります**] チェック ボックスをオンにします。
    
上記の手順を 2 回繰り返します (名前  **MBI** および **LBI** )。削除と保持のポリシーに関して異なる設定を使用します。完了後、新しいポリシーを発行できます。

## カスタム アクションの挿入

サイト分類に関するカスタム アクションを [ **設定** ] ページと **SharePoint 歯車** アイコンに挿入できます。このアクションは、 **ManageWeb** アクセス許可を持つユーザーのみが利用できます。詳しくは、「 [既定のカスタム アクション場所と ID](http://msdn.microsoft.com/en-us/library/office/bb802730%28v=office.15%29.aspx)」を参照してください。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
/// <summary>
/// Adds a custom Action to a Site Collection.
/// </summary>
/// <param name="ctx">The Authenticated client context.</param>
/// <param name="hostUrl">The Provider hosted URL for the Application</param>

static void AddCustomAction(ClientContext ctx, string hostUrl)
{
    var _web = ctx.Web;
    ctx.Load(_web);
    ctx.ExecuteQuery();

    // You only want the action to show up if you have manage web permissions.
    BasePermissions _manageWebPermission = new BasePermissions();
    _manageWebPermission.Set(PermissionKind.ManageWeb);

    CustomActionEntity _entity = new CustomActionEntity()
    {
        Group = "SiteTasks",
        Location = "Microsoft.SharePoint.SiteSettings",
        Title = "Site Classification",
        Sequence = 1000,
        Url = string.Format(hostUrl, ctx.Url),
        Rights = _manageWebPermission,
    };

    CustomActionEntity _siteActionSC = new CustomActionEntity()
    {
        Group = "SiteActions",
        Location = "Microsoft.SharePoint.StandardMenu",
        Title = "Site Classification",
        Sequence = 1000,
        Url = string.Format(hostUrl, ctx.Url),
        Rights = _manageWebPermission
    };
    _web.AddCustomAction(_entity);
    _web.AddCustomAction(_siteActionSC);
}
```

## カスタムのサイト分類

[ **サイト情報の編集** ] ページを使用すると、以下の分類オプションを選択できます。

-  **対象ユーザーのスコープ:** [ **エンタープライズ**]、 [  **組織**]、または[ **チーム**] に設定します。
    
-  **セキュリティ分類:** [ **LBI**] など、入力したいずれかの分類カテゴリに設定します。
    
-  **有効期限:** 以前に入力した分類に基づいて既定の有効期限を上書きします。
    
[ **対象ユーザー リーチ**] と [ **サイト分類**] は検索が可能であり、クロールを実行した後、管理プロパティがこれらに関連付けられます。次いでこれらのプロパティを使用して、カスタム非表示リストによりサイト コレクション内で特定の種類のサイトを検索できます。このリストは、[Core.SiteClassification.Common](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.SiteClassification/Core.SiteClassification.Common) プロジェクト ( **SiteManagerImpl** クラス内) に実装されています。

```C#
private void CreateSiteClassificationList(ClientContext ctx)
{
  var _newList = new ListCreationInformation()
    {
    Title = SiteClassificationList.SiteClassificationListTitle,
    Description = SiteClassificationList.SiteClassificationDesc,
    TemplateType = (int)ListTemplateType.GenericList,
    Url = SiteClassificationList.SiteClassificationUrl,
    QuickLaunchOption = QuickLaunchOptions.Off
    };

  if(!ctx.Web.ContentTypeExistsById(SiteClassificationContentType.SITEINFORMATION_CT_ID))
    {
    // Content type.
    ContentType _contentType = ctx.Web.CreateContentType(SiteClassificationContentType.SITEINFORMATION_CT_NAME,
    SiteClassificationContentType.SITEINFORMATION_CT_DESC,
    SiteClassificationContentType.SITEINFORMATION_CT_ID,
    SiteClassificationContentType.SITEINFORMATION_CT_GROUP);

    FieldLink _titleFieldLink = _contentType.FieldLinks.GetById(new Guid("fa564e0f-0c70-4ab9-b863-0177e6ddd247"));
    titleFieldLink.Required = false;
    contentType.Update(false);

    // Key Field.
    ctx.Web.CreateField(SiteClassificationFields.FLD_KEY_ID, 
      SiteClassificationFields.FLD_KEY_INTERNAL_NAME, 
      FieldType.Text, 
      SiteClassificationFields.FLD_KEY_DISPLAY_NAME, 
      SiteClassificationFields.FIELDS_GROUPNAME);

    // Value field.
    ctx.Web.CreateField(SiteClassificationFields.FLD_VALUE_ID, 
      SiteClassificationFields.FLD_VALUE_INTERNAL_NAME, 
      FieldType.Text, 
      SiteClassificationFields.FLD_VALUE_DISPLAY_NAME, 
      SiteClassificationFields.FIELDS_GROUPNAME);

    // Add Key Field to content type.
    ctx.Web.AddFieldToContentTypeById(SiteClassificationContentType.SITEINFORMATION_CT_ID, 
      SiteClassificationFields.FLD_KEY_ID.ToString(), 
      true);

    // Add Value Field to content type.
    ctx.Web.AddFieldToContentTypeById(SiteClassificationContentType.SITEINFORMATION_CT_ID,
      SiteClassificationFields.FLD_VALUE_ID.ToString(),
      true);
    }

  var _list = ctx.Web.Lists.Add(_newList);

  list.Hidden = true;
  list.ContentTypesEnabled = true;
  list.Update();
  ctx.Web.AddContentTypeToListById(SiteClassificationList.SiteClassificationListTitle,
    SiteClassificationContentType.SITEINFORMATION_CT_ID, true);
  this.CreateCustomPropertiesInList(_list);
  ctx.ExecuteQuery();
  this.RemoveFromQuickLaunch(ctx, SiteClassificationList.SiteClassificationListTitle);

}
```

既定では、追加設定なしで、あるいは CSOM を使用してリストを作成する場合に、リストは [ **最近** ] メニューに表示されます。ただし、このリストは非表示にする必要があります。以下のコードによって、[最近] メニューからこの項目を削除します。

```C#
private void RemoveFromQuickLaunch(ClientContext ctx, string listName)
  {
  Site _site = ctx.Site;
  Web _web = _site.RootWeb;

  ctx.Load(_web, x => x.Navigation, x => x.Navigation.QuickLaunch);
  ctx.ExecuteQuery();

  var _vNode = from NavigationNode _navNode in _web.Navigation.QuickLaunch
      where _navNode.Title == "Recent"
      select _navNode;

  NavigationNode _nNode = _vNode.First<NavigationNode>();
  ctx.Load(_nNode.Children);
  ctx.ExecuteQuery();
  var vcNode = from NavigationNode cn in _nNode.Children
     where cn.Title == listName
     select cn;
  NavigationNode _cNode = vcNode.First<NavigationNode>();
 _cNode.DeleteObject();
  ctx.ExecuteQuery();    
  }
```

Core.SiteClassification サンプルは、サイト管理者またはアクセス許可を持つ他のユーザーが新しいリストを削除する可能性に備えています。このページがアクセスを受けるとリストが再作成されますが、サンプルはプロパティを元に戻しません。サンプルを拡張してリストのアクセス許可を変更し、サイト コレクション管理者のみがアクセスできるようにすれば、この事態を回避できます。または、[Core.SiteEnumeration](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.SiteEnumeration) PnP サンプルを使用して、リストでチェックを実行し、サイト管理者に適宜通知することもできます。

リスト検証チェックは、**SiteManagerImpl** クラスの **Initialize** メンバーでも実装できます。

```C#
internal void Initialize(ClientContext ctx)
{
try {
         var _web = ctx.Web;
         var lists = _web.Lists;
         ctx.Load(_web);
         ctx.Load(lists, lc => lc.Where(l => l.Title == SiteClassificationList.SiteClassificationListTitle));
         ctx.ExecuteQuery();
                
          if (lists.Count == 0) {
                this.CreateSiteClassificationList(ctx); 
          }
      }
      catch(Exception _ex)
         {

         }
     }
}
```


            **メモ:**詳細については、「[SharePoint アドインのインストール時にホスト Web でリストを作成し、最新のコンテンツ リストから削除する](http://blogs.technet.com/b/speschka/archive/2014/05/07/create-a-list-in-the-host-web-when-your-sharepoint-app-is-installed-and-remove-it-from-the-recent-stuff-list.aspx)」を参照してください。

## サイト ページへの分類インジケーターの追加

サイト ページにインジケーターを追加し、分類を表示できます。Core.SiteClassification サンプルは、分類を表示するイメージを [ **サイト タイトル** ] の隣に埋め込む方法を示しています。旧バージョンの SharePoint の場合、これはサーバー側の委任コントロールによって行われます。JavaScript を使用したカスタムのマスター ページを使用することもできますが、このサンプルは組み込みの JavaScript パターンを使用します。[ **サイト情報の編集** ] ページの [ **サイト ポリシー** ] を変更すると、サイト インジケーターが変更され、サイト分類の各オプションに対して異なる背景色を使用して小さなボックスが表示されます。

次のメソッドが、Core.SiteClassification Web プロジェクト、スクリプト、classifier.js で定義されています。このイメージは Microsoft Azure Web サイトに格納されます。環境に合わせてハード コーディングされた URL に変更する必要があります。

```C#
function setClassifier() {
    if (!classified)
    {
        var clientContext = SP.ClientContext.get_current();
        var query = "<View><Query><Where><Eq><FieldRef Name='SC_METADATA_KEY'/><Value Type='Text'>sc_BusinessImpact</Value></Eq></Where></Query><ViewFields><FieldRef Name='ID'/><FieldRef Name='SC_METADATA_KEY'/><FieldRef Name='SC_METADATA_VALUE'/></ViewFields></View>";
        var list = clientContext.get_web().get_lists().getByTitle("Site Information");
        clientContext.load(list);
        var camlQuery = new SP.CamlQuery();
        camlQuery.set_viewXml(query);
        var listItems = list.getItems(camlQuery);
        clientContext.load(listItems);

        clientContext.executeQueryAsync(Function.createDelegate(this, function (sender, args) {
            var listItemInfo;
            var listItemEnumerator = listItems.getEnumerator();

            while (listItemEnumerator.moveNext()) {
                listItemInfo = listItemEnumerator.get_current().get_item('SC_METADATA_VALUE');
                
                var pageTitle = $('#pageTitle')[0].innerHTML;
                if (pageTitle.indexOf("img") > -1) {
                    classified = true;
                }
                else {
                    var siteClassification = listItemInfo;
                    if (siteClassification == "HBI") {
                        var img = $("<a href='http://insertyourpolicy' target=_blank><img id=classifer name=classifer src='https://spmanaged.azurewebsites.net/content/img/hbi.png' title='Site contains personally identifiable information (PII), or unauthorized release of information on this site would cause severe or catastrophic loss to Contoso.' alt='Site contains personally identifiable information (PII), or unauthorized release of information on this site would cause severe or catastrophic loss to Contoso.'></a>");
                        $('#pageTitle').prepend(img);
                        classified = true;
                    }
                    else if (siteClassification == "MBI") {
                        var img = $("<a href='http://insertyourpolicy' target=_blank><img id=classifer name=classifer src='https://spmanaged.azurewebsites.net/content/img/mbi.png' title='Unauthorized release of information on this site would cause severe impact to Contoso.' alt='Unauthorized release of information on this site would cause severe impact to Contoso.'></a>");
                        $('#pageTitle').prepend(img);
                        classified = true;
                    }
                    else if (siteClassification == "LBI") {
                        var img = $("<a href='http://insertyourpolicy' target=_blank><img id=classifer name=classifer src='https://spmanaged.azurewebsites.net/content/img/lbi.png' title='Limited or no impact to Contoso if publically released.' alt='Limited or no impact to Contoso if publically released.'></a>");
                        $('#pageTitle').prepend(img);
                        classified = true;
                    }
                }
            }
        }));
    }
```

## 別の方法

[OfficeDevPnP.Core](https://github.com/OfficeDev/PnP/tree/96eff6153389d6d21358480878de9cc8fa21abab/OfficeDevPnP.Core) の ObjectPropertyBagEntry.cs ファイルの拡張メソッド **Web.AddIndexedPropertyBagKey** を使用すると、リストではなく、サイト プロパティ バッグに分類値を格納できます。このメソッドを使用すると、プロパティ バッグをクロールしたり検索したりできます。

## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint サイト プロビジョニング ソリューション](sharepoint-site-provisioning-solutions.md)
    
- [OfficeDevPnP.Core サンプル](https://github.com/OfficeDev/PnP/tree/96eff6153389d6d21358480878de9cc8fa21abab/OfficeDevPnP.Core)
    
- [Core.SiteEnumeration サンプル](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.SiteEnumeration)
