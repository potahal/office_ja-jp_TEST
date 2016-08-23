# SharePoint のファーム ソリューションのモジュールを使用して展開されたファイルの置換

ファーム ソリューションに含まれるモジュールを使用して展開された、SharePoint のマスター ページやページ レイアウトなどのファイルを置換します。そのためには、新しいファイルをアップロードし、それらのファイルを使用して参照を更新します。

_**適用対象:** SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

ファーム ソリューションでモジュールを使用してファイルを宣言的に展開した場合、それらを新しいソリューションに変換する方法について理解します。その際、ファイルに対する参照を更新し、クライアント オブジェクト モデル (CSOM) を使用して同様の機能を提供します。これまでは、モジュールを使用して、マスター ページやページ レイアウトなどのファイルを展開しました。この記事では、以下を目的として変換プロセスを使用する方法について取り上げます。

1. マスター ページとページ レイアウトをアップロードします。
    
2. 新しいマスター ページとページ レイアウト ファイルを使用するように参照を更新します。
    
次の場合に、この変換プロセスを使用します。

- 既存のファーム ソリューションで、モジュールを使用してファイルを展開した場合。
    
-  ファーム ソリューション内のマスター ページとページ レイアウトを新しいマスター ページとページ レイアウトに置換し、それを SharePoint Online に移行する場合。
    
- ファーム ソリューションのマスター ページやページ レイアウトに宣言的にドキュメント コンテンツ タイプを設定してある場合。


              **重要** ファーム ソリューションを SharePoint Online に移行することはできません。この記事で取り上げられている技法とコードを適用すると、ファーム ソリューションと同様の機能を提供する新しいソリューションを構築し、ファイルへの参照を更新した後に、新しいソリューションを SharePoint Online に展開できます。その後、機能を非アクティブにして以前のファーム ソリューションを取り消すことができます。完全に作動するソリューションとするためには、この記事のコードに対してさらにコードを追加することが必要です。たとえば、この記事では、Office 365 に対する認証方法、必要な例外処理を実装する方法などについては取り上げていません。他のコード サンプルについては、[Office 365 Developer Patterns and Practices プロジェクト](https://github.com/OfficeDev/PnP)を参照してください。

## はじめに

既存のファーム ソリューションについて確認し、この記事で取り上げられている技法について理解し、その後、既存のそうした技法をご自分のシナリオに適用する方法を計画するのが理想的です。ファーム ソリューションに精通していない場合、または作業対象の既存のファーム ソリューションがない場合には、以下の事柄が役立つ可能性があります。

- [Contoso.Intranet ソリューション](https://github.com/OfficeDev/PnP/tree/master/Reference%20Material/Contoso.Intranet)をダウンロードします。「 [完全に信頼できるソリューションのモジュールによってプロビジョニングされたファイルの置換](https://github.com/OfficeDev/TrainingContent/blob/master/O3658/10%20Transformation%20guidance%20from%20farm%20solutions%20to%20app%20model/10-1%20Replacement%20of%20files%20deployed%20via%20Modules/Lab.md)」の課題 1 を確認し、既にあるマスター ページとページ レイアウトの構築方法の概要を理解してください。
    
- ファーム ソリューションについて理解します。 詳細については、「[SharePoint 2010 アーキテクチャの概要](https://msdn.microsoft.com/en-us/library/office/gg552610%28v=office.14%29.aspx)」および「[SharePoint 2013 でのファーム ソリューションの作成](https://msdn.microsoft.com/library/jj163902.aspx)」を参照してください。
    
コード サンプルを実行する前に、次に示す手順を実行して、サイト コレクションとサイトの発行機能を有効にしてください。

1. サイト コレクションで  **SharePoint Server 発行インフラストラクチャ**機能をアクティブにするには、次のようにします。
    
    1. SharePoint サイトを開き、**[設定]** > **[サイトの設定]** の順に移動します。
    
    2. [ **サイト コレクションの管理**] で、[ **サイト コレクションの機能**] を選択します。
    
    3. [ **SharePoint Server 発行インフラストラクチャ**] で、[ **有効にする**] を選択します。
    
2. サイトで [ **SharePoint Server 発行**] 機能をアクティブにするには、次のようにします。
    
    1. SharePoint サイトを開き、**[設定]** > **[サイトの設定]** の順に移動します。
    
    2. [ **サイト操作**] で、[ **サイト機能の管理**] を選択します。
    
    3. [ **SharePoint Server 発行**] で、[ **有効にする**] を選択します。
    
Visual Studio プロジェクトをセットアップするには、次のようにします。

1. [サンプル プロジェクト](https://github.com/OfficeDev/TrainingContent/blob/master/O3658/10%20Transformation%20guidance%20from%20farm%20solutions%20to%20app%20model/Student.zip)をダウンロードします。[ **生データの表示**] を選択してサンプル プロジェクトのダウンロードを開始し、zip ファイルからファイルを抽出し、Module9/ModuleReplacement フォルダーに移動します。
    
2. ModuleReplacement.sln を開きます。
    
3. settings.xml を開きます。以下の属性を確認し、お客様の要件に合わせて更新します。
    
    1.  **masterpage** 要素 - **file** 属性を使用してマスター ページ ギャラリーに展開するよう新しいマスター ページを指定します。また **replaces** 属性を使用して、マスター ページ ギャラリー内の既存のマスター ページを指定します。
    
    2.  **pagelayout** 要素 - **file** 属性を使用して新しいページ レイアウト ファイルを指定し、 **replaces** 属性を使用して既存のページ レイアウト ファイルを指定します。また、他のいくつかの属性を使用して、他のページ レイアウト情報を指定します。

    **メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

    ```XML
        <?xml version="1.0" encoding="utf-8" ?>
        <branding>
          <masterPages>
               <masterPage file="contoso_app.master" replaces="contoso.master"/>
          </masterPages>
          <pageLayouts>
               <pageLayout file="ContosoWelcomeLinksApp.aspx" replaces="ContosoWelcomeLinks.aspx" title="Contoso Welcome Links add-in" associatedContentTypeName="Contoso Web Page" defaultLayout="true" />
          </pageLayouts>
        </branding>
    
    ```

4. MasterPageGalleryFiles.cs の  **MasterPageGalleryFile** クラスと **LayoutFile** クラスは、SharePoint にアップロードする新しいマスター ページとページ レイアウトについての情報を格納するビジネス オブジェクトを定義します。

## マスター ページのアップロードとマスター ページへの参照の更新

SharePoint サイトでマスター ページをアップロードしてマスター ページへの参照を更新するには、次のようにします。

1. Program.cs で、次の  **using** ステートメントを追加します。
    
    ```C#
        using System.Security;
    ```

2. Program.cs で、Office 365 に対する認証を実行するために次のメソッドを追加します。
    
  ```C#
  static SecureString GetPassword()
        {
            SecureString sStrPwd = new SecureString();
            try
            {
                Console.Write("Password: ");

                for (ConsoleKeyInfo keyInfo = Console.ReadKey(true); keyInfo.Key != ConsoleKey.Enter; keyInfo = Console.ReadKey(true))
                {
                    if (keyInfo.Key == ConsoleKey.Backspace)
                    {
                        if (sStrPwd.Length > 0)
                        {
                            sStrPwd.RemoveAt(sStrPwd.Length - 1);
                            Console.SetCursorPosition(Console.CursorLeft - 1, Console.CursorTop);
                            Console.Write(" ");
                            Console.SetCursorPosition(Console.CursorLeft - 1, Console.CursorTop);
                        }
                    }
                    else if (keyInfo.Key != ConsoleKey.Enter)
                    {
                        Console.Write("*");
                        sStrPwd.AppendChar(keyInfo.KeyChar);
                    }

                }
                Console.WriteLine("");
            }
            catch (Exception e)
            {
                sStrPwd = null;
                Console.WriteLine(e.Message);
            }

            return sStrPwd;
        }

        static string GetUserName()
        {
            string strUserName = string.Empty;
            try
            {
                Console.Write("Username: ");
                strUserName = Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
                strUserName = string.Empty;
            }
            return strUserName;
        }
  ```

3. Program.cs で、以下のタスクを実行する次のコードを  **Main** メソッドに追加します。
    
    1. settings.xml ファイルを読み込みます。
    
    2. [GetCatalog](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.web.getcatalog.aspx)(116) を使用してマスター ページ ギャラリーに対する参照を取得します。マスター ページは、マスター ページ ギャラリーにアップロードされます。マスター ページ ギャラリーのリスト テンプレート ID は 116 です。詳しくは、「[ListTemplate 要素 (サイト)](https://msdn.microsoft.com/library/ms439434.aspx)」をご覧ください。 [List.ContentTypes](https://msdn.microsoft.com/library/microsoft.sharepoint.client.list.contenttypes.aspx) を使用してマスター ページ ギャラリーに関連付けられているコンテンツ種類を取得する方法など、マスター ページ ギャラリーに関する他の詳細情報が読み込まれます。
    
    3. Web.ContentTypes、Web.MasterUrl、Web.CustomMasterUrl などの Web オブジェクトのプロパティを読み込みます。
    
    4. **parentMasterPageContentTypeId** を、マスター ページのコンテンツ タイプ ID に設定します。 詳細については、「[コンテンツ タイプの基本的な階層](https://msdn.microsoft.com/library/ms452896%28v=office.14%29.aspx)」を参照してください。
    
    5. マスター ページ ギャラリーからマスター ページのコンテンツ タイプを取得します。この記事では後ほど、このコンテンツ タイプを使用して新しいマスター ページのコンテンツ タイプを設定します。 
    
    ```C#
    static void Main(string[] args)
        { 
            string userName = GetUserName();
            SecureString pwd = GetPassword();
            
         // End program if no credentials were entered.
            if (string.IsNullOrEmpty(userName) || (pwd == null))
              return;
                                   
            using (var clientContext = new ClientContext("http://sharepoint.contoso.com"))
            {
                            
                clientContext.AuthenticationMode = ClientAuthenticationMode.Default;
                clientContext.Credentials = new SharePointOnlineCredentials(userName, pwd);
                    
                XDocument settings = XDocument.Load("settings.xml");
    
                // Get a reference to the master page gallery which will be used to upload new master pages. 
             
                Web web = clientContext.Web;
                List gallery = web.GetCatalog(116);
                Folder folder = gallery.RootFolder;
            
                 // Load additional master page gallery properties.
                clientContext.Load(folder);
                clientContext.Load(gallery,
                                    g => g.ContentTypes,
                                    g => g.RootFolder.ServerRelativeUrl);
        
                // Load the content types and master page information from the web.
                clientContext.Load(web,
                                    w => w.ContentTypes,
                                    w => w.MasterUrl,
                                    w => w.CustomMasterUrl);
                clientContext.ExecuteQuery();
    
                // Get the content type for the master page from the master page gallery using the content type ID for masterpages (0x010105).
                const string parentMasterPageContentTypeId = "0x010105";  
                ContentType masterPageContentType = gallery.ContentTypes.FirstOrDefault(ct => ct.StringId.StartsWith(parentMasterPageContentTypeId));
                UploadAndSetMasterPages(web, folder, clientContext, settings, masterPageContentType.StringId);
                             
            }
    
        }
    ```

4. Program.cs で、 **UploadAndSetMasterPages** メソッドを追加します。このメソッドは、以下のようにして、 **MasterPageGalleryFile** ビジネス オブジェクトのリストを作成します。
    
    1. settings.xml で定義されているすべての  **masterPage** 要素を読み取ります。
    
    2. それぞれの  **masterPage** 要素は、SharePoint にアップロードするマスター ページを指定します。 また、 **MasterPageGalleryFile** ビジネス オブジェクトに属性値を読み込みます。
    
    3. リスト内の各  **MasterPageGalleryFile** ビジネス オブジェクトは、 **UploadAndSetMasterPage** を呼び出します。
    
    ```C#
        
    private static void UploadAndSetMasterPages(Web web, Folder folder, ClientContext clientContext, XDocument settings, string contentTypeId)
    {
        IList<MasterPageGalleryFile> masterPages = (from m in settings.Descendants("masterPage")
                                                    select new MasterPageGalleryFile
                                                    {
                                                        File = (string)m.Attribute("file"),
                                                        Replaces = (string)m.Attribute("replaces"),
                                                        ContentTypeId = contentTypeId
                                                    }).ToList();
        foreach (MasterPageGalleryFile masterPage in masterPages)
        {
            UploadAndSetMasterPage(web, folder, clientContext, masterPage);
        }
    }
    ```

5. Program.cs で、 **UploadAndSetMasterPage** メソッドを追加します。このメソッドは、以下のタスクを実行します。
    
    1. マスター ページをチェックアウトします。
    
    2. [FileCreationInformation](https://msdn.microsoft.com/library/microsoft.sharepoint.client.filecreationinformation.aspx) を使用して新しいファイルをアップロードします。
    
    3. 新たにアップロードされたファイルに関連付けられているリスト項目を取得します。
    
    4. リスト項目に各種プロパティを設定します。それには、リスト項目のコンテンツ タイプ ID をマスター ページのコンテンツ タイプ ID に設定するという操作も含まれます。
    
    5. 新しいマスター ページをチェックイン、発行、承認します。 
    
    6. 現在のサイトのマスター ページまたはカスタムのマスター ページの URL が古いマスター ページに設定されている場合は、[Web.MasterUrl](https://msdn.microsoft.com/library/microsoft.sharepoint.client.web.masterurl.aspx) または [Web.CustomMasterUrl](https://msdn.microsoft.com/library/microsoft.sharepoint.client.web.custommasterurl.aspx) を更新して、新しくアップロードされたマスター ページの URL を使用するようにします。
    
    ```C#
      private static void UploadAndSetMasterPage(Web web, Folder folder, ClientContext clientContext, MasterPageGalleryFile masterPage)
    {
        using (var fileReadingStream = System.IO.File.OpenRead(masterPage.File))
        {
            // If necessary, ensure that the master page is checked out.
            PublishingHelper.CheckOutFile(web, masterPage.File, folder.ServerRelativeUrl);
    
    // Use the FileCreationInformation class to upload the new file.
            var fileInfo = new FileCreationInformation();
            fileInfo.ContentStream = fileReadingStream;
            fileInfo.Overwrite = true;
            fileInfo.Url = masterPage.File;
            File file = folder.Files.Add(fileInfo);
    
    // Get the list item associated with the newly uploaded master page file.
            ListItem item = file.ListItemAllFields;
            clientContext.Load(file.ListItemAllFields);
            clientContext.Load(file,
                                f => f.CheckOutType,
                                f => f.Level,
                                f => f.ServerRelativeUrl);
            clientContext.ExecuteQuery();
    
    item["ContentTypeId"] = masterPage.ContentTypeId;
            item["UIVersion"] = Convert.ToString(15);
            item["MasterPageDescription"] = "Master Page Uploaded using CSOM";
            item.Update();
            clientContext.ExecuteQuery();
    
    // If necessary, check in, publish, and approve the new master page file. 
            PublishingHelper.CheckInPublishAndApproveFile(file);
    
    // On the current site, update the master page references to use the new master page. 
            if (web.MasterUrl.EndsWith("/" + masterPage.Replaces))
            {
                web.MasterUrl = file.ServerRelativeUrl;
            }
            if (web.CustomMasterUrl.EndsWith("/" + masterPage.Replaces))
            {
                web.CustomMasterUrl = file.ServerRelativeUrl;
            }
            web.Update();
            clientContext.ExecuteQuery();
        }
    }
    ```

## ページ レイアウトのアップロードとページ レイアウトへの参照の更新


            **メモ:**このセクションのコード サンプルは、この記事内ですでに取り上げているコード サンプルに基づいて構築されています。 

ファーム ソリューションでモジュールを使用して展開されたページ レイアウトを置換するには、次のようにして、新しいページ レイアウトをアップロードし、そのレイアウトを使用するように参照を更新します。


1. 以下のコードを使用して  **Main** メソッドを更新します。このコードには、ページ レイアウト ファイルのアップロードとそのファイルへの参照を更新するための追加手順が含まれています。
    
    1. **parentPageLayoutContentTypeId** をページ レイアウトのコンテンツ タイプ ID に設定します。
    
    2.  マスター ページ ギャラリーから **parentPageLayoutContentTypeId** に一致するコンテンツ タイプを取得します。
    
    3. **UploadPageLayoutsAndUpdateReferences** を呼び出します。
    
    ```C#
                static void Main(string[] args)
        { 
            string userName = GetUserName();
            SecureString pwd = GetPassword();
            
         // End program if no credentials were entered.
            if (string.IsNullOrEmpty(userName) || (pwd == null))
              return;
                                   
            using (var clientContext = new ClientContext("http://sharepoint.contoso.com"))
            {
                            
                clientContext.AuthenticationMode = ClientAuthenticationMode.Default;
                clientContext.Credentials = new SharePointOnlineCredentials(userName, pwd);
                    
                XDocument settings = XDocument.Load("settings.xml");
    
                // Get a reference to the master page gallery, which will be used to upload new master pages. 
             
                Web web = clientContext.Web;
                List gallery = web.GetCatalog(116);
                Folder folder = gallery.RootFolder;
            
                 // Load additional master page gallery properties. 
                clientContext.Load(folder);
                clientContext.Load(gallery,
                                    g => g.ContentTypes,
                                    g => g.RootFolder.ServerRelativeUrl);
        
                // Load the content types and master page information from the web.
                clientContext.Load(web,
                                    w => w.ContentTypes,
                                    w => w.MasterUrl,
                                    w => w.CustomMasterUrl);
                clientContext.ExecuteQuery();
    
                // Get the content type for the master page from the master page gallery using the content type ID for masterpages (0x010105).
                const string parentMasterPageContentTypeId = "0x010105";  
                ContentType masterPageContentType = gallery.ContentTypes.FirstOrDefault(ct => ct.StringId.StartsWith(parentMasterPageContentTypeId));
                UploadAndSetMasterPages(web, folder, clientContext, settings, masterPageContentType.StringId);
                             
    
               // Get the content type ID for the page layout, and then update references to the page layouts.
               const string parentPageLayoutContentTypeId = "0x01010007FF3E057FA8AB4AA42FCB67B453FFC100E214EEE741181F4E9F7ACC43278EE811"; 
               ContentType pageLayoutContentType = gallery.ContentTypes.FirstOrDefault(ct => ct.StringId.StartsWith(parentPageLayoutContentTypeId));
               UploadPageLayoutsAndUpdateReferences(web, folder, clientContext, settings, pageLayoutContentType.StringId);
            }
    
        }
    ```

2. Program.cs で、 **UploadPageLayoutsAndUpdateReferences** メソッドを追加します。このメソッドは、以下の手順を実行します。
    
    1. ページ レイアウト置換情報を読み込みます。その際、settings.xml から  **pagelayout** 要素を読み取り、ページ レイアウト置換情報を **LayoutFile** ビジネス オブジェクトに格納し、すべてのビジネス オブジェクトをリストに追加します。
    
    2. 置換する各ページ レイアウトについて、以下を行います。
    
        1. サイトのコンテンツ タイプに関するクエリを実行します。その際、 [Web.ContentTypes](https://msdn.microsoft.com/library/microsoft.sharepoint.client.web.contenttypes.aspx) を使用して、サイトのコンテンツ タイプが、新しいページ レイアウト用に settins.xml で指定された **associatedContentTypeName** と等しいコンテンツ タイプを検出します。
    
        2. **UploadPageLayout** を呼び出します。
    
        3. **UpdatePages** を呼び出し、新しいページ レイアウトを使用するように既存のページを更新します。

  ```C#
        private static void UploadPageLayoutsAndUpdateReferences(Web web, Folder folder, ClientContext clientContext, XDocument settings, string contentTypeId)
{
    // Read the replacement settings stored in pageLayout elements in settings.xml.
    IList<LayoutFile> pageLayouts = (from m in settings.Descendants("pageLayout")
                                 select new LayoutFile
                                 {
                                     File = (string)m.Attribute("file"),
                                     Replaces = (string)m.Attribute("replaces"),
                                     Title = (string)m.Attribute("title"),
                                     ContentTypeId = contentTypeId,
                                     AssociatedContentTypeName = (string)m.Attribute("associatedContentTypeName"),
                                     DefaultLayout = m.Attribute("defaultLayout") != null &amp;&amp; (bool)m.Attribute("defaultLayout")
                                 }).ToList();

// Update the content type association.
            foreach (LayoutFile pageLayout in pageLayouts)
    {
        ContentType associatedContentType =
            web.ContentTypes.FirstOrDefault(ct => ct.Name == pageLayout.AssociatedContentTypeName);
        pageLayout.AssociatedContentTypeId = associatedContentType.StringId;                
        UploadPageLayout(web, folder, clientContext, pageLayout);
    }
            
            UpdatePages(web, clientContext, pageLayouts);
        }

  ```

3.  Program.cs で、 **UploadPageLayout** メソッドを追加します。このメソッドは、以下のタスクを実行します。
    
    1. ページ レイアウト ファイルをチェックアウトします。
    
    2. [FileCreationInformation](https://msdn.microsoft.com/library/microsoft.sharepoint.client.filecreationinformation.aspx) を使用して新しいファイルをアップロードします。
    
    3. 新たにアップロードされたファイルに関連付けられているリスト項目を取得します。
    
    4. リスト項目に各種プロパティを設定します。そうしたプロパティには、 **ContentTypeId** や **PublishingAssociatedContentType** などがあります。
    
    5. 新しいページ レイアウト ファイルをチェックイン、発行、および承認します。
    
    6.  古いページ レイアウト ファイルに対する参照を削除するには、 **PublishingHelper.UpdateAvailablePageLayouts** を呼び出して、現在のサイトの **PageLayouts** プロパティに格納されている XML を更新します。
    
    7. settings.xml に基づく、新たにアップロードされたページ レイアウト ファイルの **defaultLayout** 属性値が **true** に設定されている場合、**PublishingHelper.SetDefaultPageLayout** を使用して、現在のサイトの **DefaultPageLayout** プロパティに格納されている XML を更新します。
    

    ```C#
    private static void UploadPageLayout(Web web, Folder folder, ClientContext clientContext, LayoutFile pageLayout)
    {
        using (var fileReadingStream = System.IO.File.OpenRead(pageLayout.File))
        {
            PublishingHelper.CheckOutFile(web, pageLayout.File, folder.ServerRelativeUrl);
            // Use FileCreationInformation to upload the new page layout file.
            var fileInfo = new FileCreationInformation();
            fileInfo.ContentStream = fileReadingStream;
            fileInfo.Overwrite = true;
            fileInfo.Url = pageLayout.File;
            File file = folder.Files.Add(fileInfo);
            // Get the list item associated with the newly uploaded file.
            ListItem item = file.ListItemAllFields;
            clientContext.Load(file.ListItemAllFields);
            clientContext.Load(file,
                                f => f.CheckOutType,
                                f => f.Level,
                                f => f.ServerRelativeUrl);
            clientContext.ExecuteQuery();
    
    item["ContentTypeId"] = pageLayout.ContentTypeId;
            item["Title"] = pageLayout.Title;
            item["PublishingAssociatedContentType"] = string.Format(";#{0};#{1};#", pageLayout.AssociatedContentTypeName, pageLayout.AssociatedContentTypeId);
            item.Update();
            clientContext.ExecuteQuery();
    
    PublishingHelper.CheckInPublishAndApproveFile(file);
    
    PublishingHelper.UpdateAvailablePageLayouts(web, clientContext, pageLayout, file);
    
    if (pageLayout.DefaultLayout)
            {
                PublishingHelper.SetDefaultPageLayout(web, clientContext, file);
            }
        }
    }
    ```

4. Program.cs に、**UpdatePages** メソッドを追加します。このメソッドは、次に示すタスクを実行します。
    
    1. **Pages** ライブラリを取得して、**Pages** ライブラリのすべてのリスト項目を取得します。
    
    2. 各リスト項目に関して、リスト項目の **PublishingPageLayout** フィールドを使用して、以前は settings.xml に格納され、現在では **pagelayouts** に格納されている、アップロード対象となる一致するページ レイアウトを検出します。リスト項目の **PublishingPageLayout** フィールドを更新して、新しいページ レイアウトを参照するようにする必要がある場合には、次のようにします。
    
        1. **PublishingHelper.CheckOutFile** を使用して、リスト項目のファイルをチェックアウトします。
    
        2. 新しいページ レイアウト ファイルを参照するようにページ レイアウトの URL を更新してから、リスト項目の  **PublishingPageLayout** フィールドを更新します。
    
        3. リスト項目が参照するファイルをチェックイン、発行、承認します。

    ```C#
    private static void UpdatePages(Web web, ClientContext clientContext, IList<LayoutFile> pageLayouts)
    {
        // Get the Pages Library, and then get all the list items in it.
        List pagesList = web.Lists.GetByTitle("Pages");
        var allItemsQuery = CamlQuery.CreateAllItemsQuery();
        ListItemCollection items = pagesList.GetItems(allItemsQuery);
        clientContext.Load(items);
        clientContext.ExecuteQuery();
        foreach (ListItem item in items)
        {
            // Only update those pages that are using a page layout which is being replaced.
            var pageLayout = item["PublishingPageLayout"] as FieldUrlValue;
            if (pageLayout != null)
            {
                LayoutFile matchingLayout = pageLayouts.FirstOrDefault(p => pageLayout.Url.EndsWith("/" + p.Replaces));
                if (matchingLayout != null)
                {
                    // Check out the page so that we can update the page layout. 
                    PublishingHelper.CheckOutFile(web, item);
    
    // Update the pageLayout reference.
                    pageLayout.Url = pageLayout.Url.Replace(matchingLayout.Replaces, matchingLayout.File);
                    item["PublishingPageLayout"] = pageLayout;
                    item.Update();
                    File file = item.File;
    
    // Get the file and other attributes so that you can check in the file.
                    clientContext.Load(file,
                        f => f.Level,
                        f => f.CheckOutType);
                    clientContext.ExecuteQuery();
    
    
                    PublishingHelper.CheckInPublishAndApproveFile(file);
                }
            }
        }
    }
    ```

## その他のリソース
<a name="bk_addresources"> </a>

- [ファーム ソリューションの SharePoint アドイン モデルへの変換](Transform-farm-solutions-to-the-SharePoint-app-model.md)
    
- [SharePoint 2013](https://msdn.microsoft.com/library/office/jj162979.aspx)
