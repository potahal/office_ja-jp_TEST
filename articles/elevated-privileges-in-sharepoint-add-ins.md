# SharePoint アドインの昇格した権限

アプリ専用のポリシーまたはサービス アカウントを使用して、SharePoint アドインでの権限を引き上げます。

_**適用対象:** SharePoint 用アプリ | SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

SharePoint アドインおよびファーム ソリューションでは、権限を昇格させるために、さまざまな方法が使用されます。ファーム ソリューションは、SharePoint サーバー側オブジェクト モデルに属する RunWithElevatedPrivileges(SPSecurity.CodeToRunElevated) を使用して権限を昇格させます。SharePoint アドインは、アプリ専用のポリシーまたはサービス アカウントのどちらかを使用します。

以下の場合に、昇格された権限をアドインで使用することができます。

* 完了させるために必要な個別の許可をユーザーが持っていない処理をアドインがユーザーのために実行する場合。許可レベルが高すぎるために、管理者はユーザーに特定の許可を割り当てないことがあります。

   たとえば、組織がカスタムのサイト コレクション プロビジョニング ソリューションを実装しており、ユーザーがサイト コレクションを作成する際にはそれを使用しなければならない場合が考えられます。組織は、すべての新しいサイト コレクションに、特定のリスト、コンテンツ タイプ、またはフィールドを関連付ける必要があると指定するかもしれません。ユーザーが独自のサイト コレクションを作成する場合に、それらのオブジェクトを忘れずに新しいコレクションに作成するかどうかは不確かです。このシナリオで、ユーザーはアドインを使用してサイト コレクションを作成しますが、サイト コレクションを作成する許可はユーザーに個別に割り当てられていません。

* アドインが何らかのユーザーのために機能するわけではない場合。たとえばガバナンスや管理プロセスなどです。

## アプリ専用ポリシーの承認

アプリ専用ポリシーは OAuth を使用してアドインを認証します。アドインがアプリ専用ポリシーを使用するとき、SharePoint はアドイン プリンシパルの許可だけを確認します。これらは、アドインに付与された許可です。現在のユーザーに関連付けられた許可とは関係なく、アドインにタスクを実行するための十分な許可があれば、承認は成功します。承認が成功すると、アクセス トークンがアドインに返されます。アドインはこのアクセス トークンを使用して、コードによって要求されるすべての操作を実行します。

詳細については、「[SharePoint 2013 のアドイン承認ポリシーの種類](https://msdn.microsoft.com/library/office/fp179892.aspx)」を参照してください。

**メモ:** アプリ専用ポリシーは、プロバイダー ホスト型アドインでのみ使用可能です。ホスト Web にアクセスする SharePoint ホスト型アドインは、ユーザー + アプリのポリシーを使用する必要があります。

アドインでアプリ専用ポリシーを使用することには、以下のような利点があります。

* 別個にユーザー ライセンスを付与する必要がありません。サービス アカウントには、別個のユーザー ライセンスが必要です。

* サービス アカウントに比べて、より詳細に制御できます。たとえば FullControl 許可を Web に適用できますが、これはサービス アカウントを使用する場合は不可能です。

以下の API にはアプリ専用ポリシーを使用できません。

* ユーザー プロファイル

* 検索

* 分類 (これは管理されたメタデータ サービスに書き込むシナリオにのみ適用されます)

アプリ専用ポリシーを使用するには、まず appinv.aspx を使用してアドインに許可を付与する必要があります。AppManifest.xml ファイルにある以下のコードは、アプリ専用ポリシーと許可をアドインに設定する方法を示しています。

```xml
 <AppPermissionRequests AllowAppOnlyPolicy="true">
   <AppPermissionRequest Scope="http://sharepoint/content/sitecollection/web" Right="FullControl" />
 </AppPermissionRequests>
```

アプリ専用ポリシーを使用するには、アドインが低信頼または高信頼の承認を使用している必要があります。SharePoint リソースへの承認されたアクセス許可を取得する 3 番目の方法である SharePoint クロスドメイン JavaScript ライブラリでは、このポリシーを使用できません。

### 低信頼の承認

Azure Access Control Service (ACS) を使用してプロバイダー ホスト型アドインと Office 365 サイトまたはオンプレミスの SharePoint ファームとの間に信頼を確立する際に、アドインは低信頼の承認を使用できます。詳細については、「[SharePoint 2013 アドインの 3 つの承認システム](https://msdn.microsoft.com/en-us/library/office/dn790706.aspx)」を参照してください。[ClientContext](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.clientcontext.aspx) オブジェクトへの参照を取得するため、アドインは以下を行う必要があります。

1. TokenHelper.GetAppOnlyAccessToken を使用してアクセス トークンを取得します。

2. TokenHelper.GetClientContextWithAccessToken を使用して ClientContext オブジェクトを取得します。

**メモ:** TokenHelper ファイルは、Microsoft Office Developer Tools for Visual Studio によって生成されるソース コードです。そのための参照ドキュメントはありませんが、TokenHelper クラスには広範なコメントが含まれています。コード コメントを参照するには、プロバイダー ホスト型アドインを Visual Studio 内に作成します。

**メモ:** この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```cs
Uri siteUrl = new Uri(ConfigurationManager.AppSettings["MySiteUrl"]);
try
{
    // Connect to a site using an app-only token.
    string realm = TokenHelper.GetRealmFromTargetUrl(siteUrl);
    var token = TokenHelper.GetAppOnlyAccessToken(TokenHelper.SharePointPrincipal, siteUrl.Authority, realm).AccessToken;

    using (var ctx = TokenHelper.GetClientContextWithAccessToken(siteUrl.ToString(), token))
    {
        // Perform operations on the ClientContext object, which uses the app-only token. 
    }
}
catch (Exception ex)
{
    Console.WriteLine("Error in execution: " + ex.Message);
}
```

### 高信頼の承認

アドインは、高信頼の承認システム (S2S プロトコルとも呼ばれる) を使用する場合、別の **TokenHelper** メソッドである **TokenHelper.GetS2SAccessTokenWithWindowsIdentity** を呼び出します。


              **重要:****TokenHelper.GetS2SAccessTokenWithWindowsIdentity** は、アプリ専用の呼び出しとユーザー + アプリの呼び出しとの両方で使用されます。メソッドの第 2 パラメーター (ユーザー ID を保持する) によって、使用されるポリシーが決まります。**null** を渡すと、アプリ専用ポリシーが使用されます。

## サービス アカウント

タスクを完了するために必要な許可がアプリ専用ポリシーによって提供されない場合にのみ、サービス アカウントを使用してアドインの権限を昇格させます。また、特定のシナリオではユーザー アカウントが必要です。たとえば、コードが以下のいずれかの SharePoint サービス アプリケーションと共に実行されるときには、サービス アカウントを使用する必要があります。

* クライアント側オブジェクト モデル (CSOM) を使用するユーザー プロファイル サービス

* 管理されたメタデータ サービス

* 検索

アドインでのサービス アカウントの使用を計画する際には、以下を検討します。

* サービス アカウントには、別個のユーザー ライセンスが必要です。

* アドインごとに 1 つのサービス アカウントを作成するか、または SharePoint 環境内のすべてのアドイン用に 1 つのサービス アカウントを作成します。

* 承認の際に、ユーザー名とパスワードを指定する必要があります。サービス アカウントの資格情報の保管と取得の安全を確認してください。

* アドインがサイトでのアクションを実行するには、サイトにアクセスする許可を事前にサービス アカウントに付与する必要があります。

**メモ:**Office ストアから購入したアドインでは、サービス アカウントを使用できません。

以下のコードは、[SharePointOnlineCredentials](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.sharepointonlinecredentials.aspx) をサービス アカウントと共に使用して認証を行う方法を示しています。

```cs
using (ClientContext context = new ClientContext("https://contoso.sharepoint.com"))
{

    // Use default authentication mode.
    context.AuthenticationMode = ClientAuthenticationMode.Default;  

    // Specify the credentials for the service account.
    context.Credentials = new SharePointOnlineCredentials("User Name", "Password");
}
```

## その他の技術情報

Office 365 開発パターンとプラクティス (ソリューション ガイダンス)


            [SharePoint 2013 のアドイン承認ポリシーの種類](https://msdn.microsoft.com/en-us/library/office/fp179892.aspx)


            [Office 365 SharePoint サイトを使用してオンプレミスの SharePoint サイトのプロバイダー向けのホスト型アドインに権限を付与する](https://msdn.microsoft.com/en-us/library/office/dn155905.aspx)
