SharePoint アドイン モデルにおけるモジュール
======================================

概要
-------

新しい SharePoint アドイン モデルで SharePoint 環境に成果物を展開する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、宣言型コード (機能フレームワーク XML ファイル) で定義されたモジュールは SharePoint の機能に追加されていました。モジュールには、SharePoint サーバーに展開する成果物のリストが含まれていました。モジュールはSharePoint ソリューションを使用して SharePoint の機能に追加され、展開されていました。機能をアクティブ化すると、モジュールで定義されている成果物は、SharePoint 環境に展開されていました。

SharePoint アドイン モデル シナリオでは、成果物を SharePoint 環境に展開するためにリモート プロビジョニング パターンが使用されています。

用語
-----------

*成果物*という用語は、この記事全体で使用されます。成果物は通常、SharePoint 環境に展開されているアイテムを示します。成果物に通常含まれるものは、次のとおりです。

- JavaScript ファイル
- CSS ファイル
- 画像ファイル (.jpg、.gif、.png など)
- マスター ページ
- ページ レイアウト
- リスト アイテム

基本ガイドライン
---------------------

SharePoint 環境への成果物の展開については、大まかに次のような基本ガイドラインが提供されています。

- アイテムを SharePoint 環境に展開するには、リモート プロビジョニング パターン (SharePoint クライアント側オブジェクト モデルと SharePoint REST API) を使用します。
- 成果物を SharePoint 環境に展開するのに宣言型コード モジュールまたは機能フレームワーク XML ファイルを使用しないでください。

**デバッグ**

コードを使用して成果物を展開する大きなメリットは、コードを使用する場合は、展開プロセスをデバッグできるということです。宣言型コード モジュールまたは機能フレームワーク XML ファイルを使用してアイテムを SharePoint 環境に展開する場合、展開プロセスをデバッグすることはできません。

**作業の開始**

次の O365 PnP コード サンプルは、リモート プロビジョニング パターンを使用して成果物を SharePoint 環境に展開する SharePoint アドインの作成方法について説明します。

  このサンプルでは、スタイル ライブラリで新しいフォルダーを作成し、JavaScript ファイルとイメージを新しいファイルに追加する方法を示しています。

- [Branding.ClientSideRendering (O365 PnP のコード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ClientSideRendering)
    + 詳細については、「[Default.aspx.cs クラス](https://github.com/OfficeDev/PnP/blob/master/Samples/Branding.ClientSideRendering/Branding.ClientSideRenderingWeb/Pages/Default.aspx.cs)」の***UploadJSFiles*** メソッドおよび ***UploadFileToFolder*** メソッドを参照してください。
    + これらのメソッドは、以下のクイック リファレンスにも表示されます。
    
        **フォルダーを作成し、JavaScript ファイルをフォルダーにアップロードする:**

        ```
        void UploadJSFiles(Web web)
        {
            //Delete the folder if it exists
            Microsoft.SharePoint.Client.List list = web.Lists.GetByTitle("Style Library");
            IEnumerable<Folder> results = web.Context.LoadQuery<Folder>(list.RootFolder.Folders.Where(folder => folder.Name == "JSLink-Samples"));
            web.Context.ExecuteQuery();
            Folder samplesJSfolder = results.FirstOrDefault();
        
            if (samplesJSfolder != null)
            {
                samplesJSfolder.DeleteObject();
                web.Context.ExecuteQuery();
            }
        
            //Create new folder
            samplesJSfolder = list.RootFolder.Folders.Add("JSLink-Samples");
            web.Context.Load(samplesJSfolder);
            web.Context.ExecuteQuery();
        
            //Upload JavaScript files to folder
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/Accordion.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/ConfidentialDocuments.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/DisableInput.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/HiddenField.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/PercentComplete.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/PriorityColor.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/ReadOnlySPControls.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/RegexValidator.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/SubstringLongText.js"), samplesJSfolder);
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/DependentFields.js"), samplesJSfolder);
        
            //Create another folder inside the folder that was just created
            Folder imgsFolder = samplesJSfolder.Folders.Add("imgs");
            web.Context.Load(imgsFolder);
            web.Context.ExecuteQuery();
        
            //Upload image files to folder
            UploadFileToFolder(web, Server.MapPath("../Scripts/JSLink-Samples/imgs/Confidential.png"), imgsFolder);
        }
        ```
        **フォルダーを作成し、画像ファイルをフォルダーにアップロードする:**
        ```
        public static void UploadFileToFolder(Web web, string filePath, Folder folder)
            {
            //Create a FileStream to the file to upload
                using (FileStream fs = new FileStream(filePath, FileMode.Open))
                {
                //Create FileCreationInformation object to set file metadata
                    FileCreationInformation flciNewFile = new FileCreationInformation();
    
                    flciNewFile.ContentStream = fs;
                    flciNewFile.Url = System.IO.Path.GetFileName(filePath);
                    flciNewFile.Overwrite = true;
    
                //Upload file to SharePoint
                        Microsoft.SharePoint.Client.File uploadFile = folder.Files.Add(flciNewFile);
    
                //Check in the file
                    uploadFile.CheckIn("CSR sample js file", CheckinType.MajorCheckIn);
    
                    folder.Context.Load(uploadFile);
                    folder.Context.ExecuteQuery();
                }
        }
        ```

このサンプルでは、マスター ページのアップロード、マスター ページのメタ データの設定および Web オブジェクトで CustomMasterUrl プロパティを設定することによってサイトにマスター ページを適用する方法を示しています。

- [Branding.ApplyBranding (O365 PnP のコード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ApplyBranding)
    + 詳細については、「[BrandingHelper.cs クラス](https://github.com/OfficeDev/PnP/blob/master/Samples/Branding.ApplyBranding/Branding.ApplyBranding.Console/BrandingHelper.cs)」の ***UploadPageLayout***、***CreatePublishingPage***、および ***SetSupportCaseContent*** メソッドを参照してください。
    + SharePoint で新しいアイテムを作成するだけでなくは、このサンプルは、アイテムを削除する方法も示しています。参考のため、アイテムを削除するメソッドを次に示します。  
        **ファイルを削除する:**
    
        ```
        private static void DeleteFile(Web web, string fileName, string serverPath, string serverFolder)
        {
            var fileUrl = string.Concat(serverPath, serverFolder, (string.IsNullOrEmpty(serverFolder) ? string.Empty : "/"), fileName);
            var fileToDelete = web.GetFileByServerRelativeUrl(fileUrl);
            fileToDelete.DeleteObject();
            web.Context.ExecuteQuery();
        }
    
        ```
        **フォルダーを削除する:**
        ```
        public static void RemoveFolder(ClientContext clientContext, string folder, string path)
            {
                var web = clientContext.Web;
                var filePath = web.ServerRelativeUrl.TrimEnd(Program.trimChars) + "/" + path + "/";
                var folderToDelete = web.GetFolderByServerRelativeUrl(string.Concat(filePath, folder));
                Console.WriteLine("Removing folder {0} from {1}", folder, path);
                folderToDelete.DeleteObject();
                clientContext.ExecuteQuery();
            }
        ```
        **マスター ページの割り当てを解除し、マスター ページを削除する**
        ```
        public static void RemoveMasterPage(ClientContext clientContext, string name, string folder)
            {
                var web = clientContext.Web;
                clientContext.Load(web, w => w.AllProperties);
                clientContext.ExecuteQuery();
    
                Console.WriteLine("Deactivating and removing {0} from {1}", name, web.ServerRelativeUrl);            
            
                //set master pages back to the defaults that were being used
                if (web.AllProperties.FieldValues.ContainsKey("OriginalMasterUrl"))
                {
                    web.MasterUrl = (string)web.AllProperties["OriginalMasterUrl"];
                }
                if (web.AllProperties.FieldValues.ContainsKey("CustomMasterUrl"))
                {
                    web.CustomMasterUrl = (string)web.AllProperties["CustomMasterUrl"];
                }
                web.Update();
                clientContext.ExecuteQuery();
    
                //now that the master page is set back to its default, re-reference the web from context and delete the custom master pages
                web = clientContext.Web;
                var lists = web.Lists;
                var gallery = web.GetCatalog(116);
                clientContext.Load(lists, l => l.Include(ll => ll.DefaultViewUrl));
                clientContext.Load(gallery, g => g.RootFolder.ServerRelativeUrl);
                clientContext.ExecuteQuery();
                var masterPath = gallery.RootFolder.ServerRelativeUrl.TrimEnd(new char[] { '/' }) + "/";
                DeleteFile(web, name, masterPath, folder);
            }
        ```
        **ページ レイアウトを削除する**
        ```
        public static void RemovePageLayout(ClientContext clientContext, string name, string folder)
        {
            var web = clientContext.Web;
                var lists = web.Lists;
                var gallery = web.GetCatalog(116);
                clientContext.Load(lists, l => l.Include(ll => ll.DefaultViewUrl));
                clientContext.Load(gallery, g => g.RootFolder.ServerRelativeUrl);
                clientContext.ExecuteQuery();
    
                Console.WriteLine("Removing page layout {0} from {1}", name, clientContext.Web.ServerRelativeUrl);
    
                var masterPath = gallery.RootFolder.ServerRelativeUrl.TrimEnd(Program.trimChars) + "/";
            
                DeleteFile(web, name, masterPath, folder);
        }
        ```

    + このサンプルの説明については、Office 365 PnP ビデオの「[SharePoint のアドインを使用して SharePoint サイトにブランド化を適用する](https://channel9.msdn.com/Blogs/Office-365-Dev/Applying-Branding-to-SharePoint-Sites-with-an-App-for-SharePoint-Office-365-Developer-Patterns-and-P)」を参照してください。

このサンプルには、ビデオの一部が含まれています。発行機能をアクティブ化、ページ レイアウトをアップロード、発行ページを作成、リスト、コンテンツ タイプおよびリスト アイテムを作成、発行ページを作成、および Web パーツとアドイン パーツをページに追加する方法を示しています。また、ホスト Web とアドイン Web の両方にリスト アイテムを展開する方法も示します。

- [Branding.ClientSideRendering (O365 PnP のコード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ClientSideRendering)
    + これらの操作の例については、[Utils.cs クラス](https://github.com/OfficeDev/PnP/blob/master/Samples/Core.DataStorageModels/Core.DataStorageModelsWeb/Util/Util.cs)のメソッドを参照してください。
    + 参考のために、これらのメソッドを次に示します。  
        **サイト コレクションとサイト レベルの発行機能をアクティブにする:**
        ```
        public static void ActivePublishingFeature(ClientContext ctx)
        {
            Guid publishingSiteFeatureId = new Guid("f6924d36-2fa8-4f0b-b16d-06b7250180fa");
            Guid publishingWebFeatureId = new Guid("94c94ca6-b32f-4da9-a9e3-1f3d343d7ecb");
    
            Site clientSite = ctx.Site;
            ctx.Load(clientSite);
    
            FeatureCollection clientSiteFeatures = clientSite.Features;
            ctx.Load(clientSiteFeatures);
    
            //Activate the site feature
            clientSiteFeatures.Add(publishingSiteFeatureId, true, FeatureDefinitionScope.Farm);
            ctx.ExecuteQuery();
    
            FeatureCollection clientWebFeatures = ctx.Web.Features;
            ctx.Load(clientWebFeatures);
    
            //Activate the web feature
            clientWebFeatures.Add(publishingWebFeatureId, true, FeatureDefinitionScope.Farm);
            ctx.ExecuteQuery();
        }
        ```
        **リストを作成する:**
        ```
        public static List CreateList(ClientContext ctx, int templateType,
                                           string title, string url, QuickLaunchOptions quickLaunchOptions)
        {
            ListCreationInformation listCreationInfo = new ListCreationInformation
            {
                TemplateType = templateType,
                Title = title,
                Url = url,
                QuickLaunchOption = quickLaunchOptions
            };
            List spList = ctx.Web.Lists.Add(listCreationInfo);
            ctx.Load(spList);
            ctx.ExecuteQuery();
    
            return spList;
        }
    
        ```
        **コンテンツ タイプを作成する**
        ```
        public static ContentType CreateContentType(ClientContext ctx, string ctyName, string group, string ctyId)
        {
            ContentTypeCreationInformation contentTypeCreation = new ContentTypeCreationInformation();
            contentTypeCreation.Name = ctyName;
            contentTypeCreation.Description = "Custom Content Type";
            contentTypeCreation.Group = group;
            contentTypeCreation.Id = ctyId;
    
            //Add the new content type to the collection
            ContentType ct = ctx.Web.ContentTypes.Add(contentTypeCreation);
            ctx.Load(ct);
            ctx.ExecuteQuery();
    
            return ct;
        }
    
        ```
        **ページ レイアウトをアップロードする**
        ```
        public static void UploadPageLayout(ClientContext ctx, string sourcePath, string targetListTitle, string targetUrl)
            {
            using (FileStream fs = new FileStream(sourcePath, FileMode.Open, FileAccess.Read))
            {
                byte[] data = new byte[fs.Length];
                fs.Read(data, 0, data.Length);
                using (MemoryStream ms = new MemoryStream())
                {
                    ms.Write(data, 0, data.Length);
                    var newfile = new FileCreationInformation();
                    newfile.Content = ms.ToArray();
                    newfile.Url = targetUrl;
                    newfile.Overwrite = true;
    
                    List docs = ctx.Web.Lists.GetByTitle(targetListTitle);
                    Microsoft.SharePoint.Client.File uploadedFile = docs.RootFolder.Files.Add(newfile);
                    uploadedFile.CheckOut();
                    uploadedFile.CheckIn("Data storage model", CheckinType.MajorCheckIn);
                    uploadedFile.Publish("Data storage model layout.");
    
                    ctx.Load(uploadedFile);
                    ctx.ExecuteQuery();
                }
            }
        }
    
        ```
        **発行ページを作成し、そのページ レイアウトを設定する:**
        ```
        public static void CreatePublishingPage(ClientContext clientContext, string pageName, string pagelayoutname, string url, string queryurl)
            {
                var publishingPageName = pageName + ".aspx";
    
                Web web = clientContext.Web;
                clientContext.Load(web);
    
                List pages = web.Lists.GetByTitle("Pages");
                clientContext.Load(pages.RootFolder, f => f.ServerRelativeUrl);
                clientContext.ExecuteQuery();
    
                Microsoft.SharePoint.Client.File file =
                    web.GetFileByServerRelativeUrl(pages.RootFolder.ServerRelativeUrl + "/" + pageName + ".aspx");
                clientContext.Load(file, f => f.Exists);
                clientContext.ExecuteQuery();
                if(file.Exists)
                {
                    file.DeleteObject();
                    clientContext.ExecuteQuery();
                }
                PublishingWeb publishingWeb = PublishingWeb.GetPublishingWeb(clientContext, web);
                clientContext.Load(publishingWeb);
    
                if (publishingWeb != null)
                {
                    List publishingLayouts = clientContext.Site.RootWeb.Lists.GetByTitle("Master Page Gallery");
    
                    ListItemCollection allItems = publishingLayouts.GetItems(CamlQuery.CreateAllItemsQuery());
                    clientContext.Load(allItems, items => items.Include(item => item.DisplayName).Where(obj => obj.DisplayName == pagelayoutname));
                    clientContext.ExecuteQuery();
    
                    ListItem layout = allItems.Where(x => x.DisplayName == pagelayoutname).FirstOrDefault();
                    clientContext.Load(layout);
    
                    PublishingPageInformation publishingpageInfo = new PublishingPageInformation()
                    {
                        Name = publishingPageName,
                        PageLayoutListItem = layout,
                    };
    
                    PublishingPage publishingPage = publishingWeb.AddPublishingPage(publishingpageInfo);
                    publishingPage.ListItem.File.CheckIn(string.Empty, CheckinType.MajorCheckIn);
                    publishingPage.ListItem.File.Publish(string.Empty);
                    clientContext.ExecuteQuery();
                }
                SetSupportCaseContent(clientContext, "SupportCasesPage", url, queryurl);
            }
        ```
        **リスト アイテムを作成する**
        ```
        public static void AddDemoDataToSupportCasesList(ClientContext ctx, List list, string title,
                                                           string status, string csr, string customerID)
        {
            ListItemCreationInformation itemCreateInfo = new ListItemCreationInformation();
            ListItem newItem = list.AddItem(itemCreateInfo);
            newItem["Title"] = title;
            newItem["FTCAM_Status"] = status;
            newItem["FTCAM_CSR"] = csr;
            newItem["FTCAM_CustomerID"] = customerID;
            newItem.Update();
            ctx.ExecuteQuery();
        }
        ```
        **コンテンツを発行ページ (検索によるコンテンツ Web パーツ、Script Editor Web パーツ、アドイン パーツ) にプロビジョニングして、ページを発行する:**
        ```
        public static void SetSupportCaseContent(ClientContext ctx, string pageName, string url, string queryurl)
        {
            List pages = ctx.Web.Lists.GetByTitle("Pages");
            ctx.Load(pages.RootFolder, f => f.ServerRelativeUrl);
            ctx.ExecuteQuery();
    
            Microsoft.SharePoint.Client.File file =
                ctx.Web.GetFileByServerRelativeUrl(pages.RootFolder.ServerRelativeUrl + "/" + pageName + ".aspx");
            ctx.Load(file);
            ctx.ExecuteQuery();
    
            file.CheckOut();
    
            LimitedWebPartManager limitedWebPartManager = file.GetLimitedWebPartManager(PersonalizationScope.Shared);
    
            string quicklaunchmenuFormat =
                @"<div><a href='{0}/{1}'>Sample Home Page</a></div>
                <br />
                <div style='font-weight:bold'>CSR Dashboard</div>
                <div class='cdsm_mainmenu'>
                    <ul>
                        <li><a href='{0}/CSRInfo/{1}'>My CSR Info</a></li>
                        <li><a href='{0}/CallQueue/{1}'>Call Queue</a></li>
                        <li>
                            <span class='collapse_arrow'></span>
                            <span><a href='{0}/CustomerDashboard/{1}'>Customer Dashboard</a></span>
                            <ul>
                                <li><a href='{0}/CustomerDashboard/Orders{1}'>Recent Orders</a></li>
                                <li><a class='current' href='#'>Support Cases</a></li>
                                <li><a href='{0}/CustomerDashboard/Notes{1}'>Notes</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>
                <div class='cdsm_submenu'>
                </div>";
    
            string quicklaunchmenu = string.Format(quicklaunchmenuFormat, url, queryurl);
    
            string qlwebPartXml = "<?xml version=\"1.0\" encoding=\"utf-8\"?><webParts><webPart xmlns=\"http://schemas.microsoft.com/WebPart/v3\"><metaData><type name=\"Microsoft.SharePoint.WebPartPages.ScriptEditorWebPart, Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c\" /><importErrorMessage>Cannot import this Web Part.</importErrorMessage></metaData><data><properties><property name=\"Content\" type=\"string\"><![CDATA[" + quicklaunchmenu + "]]></property><property name=\"ChromeType\" type=\"chrometype\">None</property></properties></data></webPart></webParts>";
            WebPartDefinition qlWpd = limitedWebPartManager.ImportWebPart(qlwebPartXml);
            WebPartDefinition qlWpdNew = limitedWebPartManager.AddWebPart(qlWpd.WebPart, "SupportCasesZoneLeft", 0);
            ctx.Load(qlWpdNew);
    
            //Customer Dropdown List Script Web Part
            string dpwebPartXml = System.IO.File.ReadAllText(System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath + "Assets/CustomerDropDownlist.webpart");
            WebPartDefinition dpWpd = limitedWebPartManager.ImportWebPart(dpwebPartXml);
            WebPartDefinition dpWpdNew = limitedWebPartManager.AddWebPart(dpWpd.WebPart, "SupportCasesZoneTop", 0);
            ctx.Load(dpWpdNew);
    
            //Support Case CBS Info Web Part
            string cbsInfoWebPartXml = System.IO.File.ReadAllText(System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath + "Assets/SupportCaseCBSWebPartInfo.webpart");
            WebPartDefinition cbsInfoWpd = limitedWebPartManager.ImportWebPart(cbsInfoWebPartXml);
            WebPartDefinition cbsInfoWpdNew = limitedWebPartManager.AddWebPart(cbsInfoWpd.WebPart, "SupportCasesZoneMiddle", 0);
            ctx.Load(cbsInfoWpdNew);
    
            //Support Case Content By Search Web Part
            string cbswebPartXml = System.IO.File.ReadAllText(System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath + "Assets/SupportCase CBS Webpart/SupportCaseCBS.webpart");
            WebPartDefinition cbsWpd = limitedWebPartManager.ImportWebPart(cbswebPartXml);
            WebPartDefinition cbsWpdNew = limitedWebPartManager.AddWebPart(cbsWpd.WebPart, "SupportCasesZoneMiddle", 1);
            ctx.Load(cbsWpdNew);
    
            //Support Cases App Part
            string appPartXml = System.IO.File.ReadAllText(System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath + "Assets/SupportCaseAppPart.webpart");
            WebPartDefinition appPartWpd = limitedWebPartManager.ImportWebPart(appPartXml);
            WebPartDefinition appPartdNew = limitedWebPartManager.AddWebPart(appPartWpd.WebPart, "SupportCasesZoneBottom", 0);
            ctx.Load(appPartdNew);
    
            //Get Host Web Query String and show support case list web part
            string querywebPartXml = System.IO.File.ReadAllText(System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath + "Assets/GetHostWebQueryStringAndShowList.webpart");
            WebPartDefinition queryWpd = limitedWebPartManager.ImportWebPart(querywebPartXml);
            WebPartDefinition queryWpdNew = limitedWebPartManager.AddWebPart(queryWpd.WebPart, "SupportCasesZoneBottom", 1);
            ctx.Load(queryWpdNew);
    
    
            file.CheckIn("Data storage model", CheckinType.MajorCheckIn);
            file.Publish("Data storage model");
            ctx.Load(file);
            ctx.ExecuteQuery();
        }
    ```

    + Seeリストアイテムをホスト Web とアドイン Web の両方に展開する方法の詳細については、「[SharePointService.cs クラス](https://github.com/OfficeDev/PnP/blob/master/Samples/Core.DataStorageModels/Core.DataStorageModelsWeb/Services/SharePointService.cs)」の***FillHostWebSupportCasesToThreshold*** メソッドと ***FillAppWebNotesListToThreshold*** メソッドを参照してください。    + ***
            ***重要なメモ:***このサンプルで示されているものと同じホスト Web およびアドイン Web は、適切な場所に展開する任意の種類の成果物に適用できます。       **AddWeb ホストのリストにリスト アイテムを追加する:        ```
        public string FillHostWebSupportCasesToThreshold()
            {
                using (var clientContext = SharePointContext.CreateUserClientContextForSPHost())
                {
                    List supportCasesList = clientContext.Web.Lists.GetByTitle("Support Cases");
                    ListItemCreationInformation itemCreateInfo = new ListItemCreationInformation();
                    for (int i = 0; i < 500; i++)
                    {
                        ListItem newItem = supportCasesList.AddItem(itemCreateInfo);
                        newItem["Title"] = "Wrong product received." + i.ToString();
                        newItem["FTCAM_Status"] = "Open";
                        newItem["FTCAM_CSR"] = "bjones";
                        newItem["FTCAM_CustomerID"] = "thresholds test";
                        newItem.Update();
                        if (i % 100 == 0)
                            clientContext.ExecuteQuery();
                    }
                    clientContext.ExecuteQuery();
    
               
                    clientContext.Load(supportCasesList, l => l.ItemCount);
                    clientContext.ExecuteQuery();
    
                    if(supportCasesList.ItemCount>=5000)
                        return "The Host Web Support Cases List has " + supportCasesList.ItemCount + " items, and exceeds the threshold.";
                    else
                        return 500 + " items have been added to the Host Web Support Cases List. " +
                         "There are " + (5000 - supportCasesList.ItemCount) + " items left to add.";    
            }
        }
        ```
    
        **Addアドイン Web のリストにリスト アイテムを追加する:        ```
        public string FillAppWebNotesListToThreshold()
            {
                using (var clientContext = SharePointContext.CreateUserClientContextForSPAppWeb())
                {
                    List notesList = clientContext.Web.Lists.GetByTitle("Notes");
    
                    var itemCreateInfo = new ListItemCreationInformation();
                    for (int i = 0; i < 500; i++)
                    {
                        ListItem newItem = notesList.AddItem(itemCreateInfo);
                        newItem["Title"] = "Notes Title." + i.ToString();
                        newItem["FTCAM_Description"] = "Notes description";
                        newItem.Update();
                        if (i % 100 == 0)
                            clientContext.ExecuteQuery();
                    }
                    clientContext.ExecuteQuery();
    
                    clientContext.Load(notesList, l => l.ItemCount);
                    clientContext.ExecuteQuery();
    
                    if (notesList.ItemCount >= 5000)
                        return "The Add-in Web Notes List has " + notesList.ItemCount + " items, and exceeds the threshold.";
                    else
                        return 500 + " items have been added to the Add-in Web Notes List. " +
                                       "There are " + (5000-notesList.ItemCount) + " items left to add.";          
                }
            }
        ```
    
Rel関連リンク===========

- [Masマスター ページ (SharePoint アドインのレシピ)aster-pages-sharepoint-add-in.md)
- Guiガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")RefMSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")Vidビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")el関連する PnP サンプル=================

- [BraBranding.ClientSideRendering (O365 PnP のコード サンプル)ttps://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ClientSideRendering)
- [BraBranding.ApplyBranding (O365 PnP のコード サンプル)ttps://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ApplyBranding)
- [BraBranding.ClientSideRendering (O365 PnP のコード サンプル)ttps://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ClientSideRendering)
- Sam[https://github.com/OfficeDev/PnP](https://github.com/OfficeDev/PnP) にあるサンプルとコンテンツpp適用対象========
- OffOffice 365 マルチテナント (MT)OffOffice 365 専用 (D) *一部*ShaSharePoint 2013 オンプレミス - *一部*Pat専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。