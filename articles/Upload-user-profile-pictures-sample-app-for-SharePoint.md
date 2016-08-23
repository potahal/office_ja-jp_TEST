# ユーザー プロファイル画像をアップロードするサンプル アドイン (SharePoint)

プロバイダー向けのホスト型アドインを使用して、ファイル共有または SharePoint Online URL から、ユーザー プロファイル データを一括アップロードできます。
    
_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Online_
    
[Core.ProfilePictureUploader](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ProfilePictureUploader) サンプル アドインは、ファイル共有または SharePoint Online URL からユーザー プロファイル データを一括アップロードする方法と、アップロードしたイメージにユーザー プロファイル プロパティをリンクする方法を示します。
    
このサンプルを使用すると、以下の操作を実行する方法を学べます。

- ユーザーのプロファイル画像をオンプレミスの SharePoint Server 2013 から SharePoint Online に移行します。
    
- Azure Active Directory 同期ツール (dirsync) がユーザーのプロファイル画像を SharePoint Online と同期できなかったときに発生する問題を修正します。
    
- SharePoint Online 内の品質が低いユーザー プロファイル画像を置き換えます。
    
このサンプルは、コンソール アプリケーションを使用して以下の操作を実行します。

- ユーザー マッピング ファイルから、ユーザー名とイメージのファイル パスか URL を読み取ります。
    
- 1 つから 3 つのイメージを取得し、個人用サイトのホスト上の画像ライブラリにアップロードします。 
    
- アップロードしたイメージをユーザーのプロファイルにリンクするようにユーザー プロファイル プロパティを設定します。
    
- 追加 (オプション) のユーザー プロファイル プロパティを更新します。
    
## はじめに
<a name="sectionSection0"> </a>

まず、[Core.ProfilePictureUploader](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ProfilePictureUploader) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

このコード サンプルを実行する前に、次の操作を実行してください。

- アップロードしようとしているユーザー イメージをファイル共有か Web サーバー上の SharePoint Online に保存します。 
    
- userlist.csv ファイルを編集して、以下のものを組み込みます。
    
    - 値 **UserPrincipalName**、**SourceURL** が含まれるヘッダー行。
    
    - ユーザーごとに、そのユーザーの組織アカウント (またはユーザー プリンシパル名) と、アップロードするイメージのファイル パスか URL が含まれる新しい行を追加します。 
    
- このサンプルを Visual Studio から実行するには、次のコマンドライン引数を使用して **Core.ProfilePictureUploader** プロジェクトを構成します。`-SPOAdmin Username -SPOAdminPassword Password -Configuration filepath`
    
  指定するパラメーターは次のとおりです。
    
    - 
            *Username* は Office 365 管理者のユーザー名です。
    
    - 
            *Password* は Office 365 管理者のパスワードです。
    
    - 
            *Filepath* は configuration.xml ファイルのファイル パスです。
    
- Core.ProfilePictureUploader プロジェクトでコマンドライン引数を設定するには、次のようにします。
    
    - ソリューション エクスプローラーで、**Core.ProfilePictureUploader** プロジェクト > **[プロパティ]** を右クリックしてショートカット メニューを開きます。
    
    - **[デバッグ]** を選択します。
    
    - **[コマンド ライン引数]** に前述のコマンドライン引数を入力します。
    
- 要件を満たすようにアップロード プロセスを構成するには、以下の値を入力して、configuration.xml ファイルを編集します。
    
    - **tenantName** 要素内の Office 365 テナントの名前。 例: `<tenantName>contoso.onmicrosoft.com</tenantName>`
    
    - **pictureSourceCsv** 要素内のユーザー マッピングのファイル パス。 例: `<pictureSourceCsv>C:\temp\userlist.csv</pictureSourceCsv>`
    
    - **thumbs** 要素を使用したイメージのアップロード手順。 thumbs 要素内で以下の属性を編集します。
    
        -  
            **aupload3Thumbs** - ユーザーごとに 3 つのイメージをアップロードする場合は **true** に設定し、1 つのイメージのみアップロードする場合は **false** に設定します。
    
        -  
            **createSMLThumbs** - ソース イメージの 3 種類のサイズ (小、中、大) のイメージを作成する場合は **true** に設定し、同じサイズの 3 つのイメージをアップロードする場合は **false** に設定します。
    
    - **additionalProfilePropties** 要素を使用してユーザーのプロファイル上で設定する追加のプロパティ。たとえば、以下の XML は **SPS-PictureExchangeSyncState** という追加のユーザー プロファイルのプロパティを指定します。コード サンプルの実行時には、ユーザーのプロファイル上でこのプロパティをゼロに設定する必要があります。

    ```
    <additionalProfileProperties>
         <property name="SPS-PictureExchangeSyncState" value="0"/>
    </additionalProfileProperties>
    ```

  - **logfile** 要素内のログ ファイル パス。以下に例を示します。 `<logFile path="C:\temp\log.txt" enableLogging="true" loggingLevel="verbose" />`
    
  - **uploadDelay** 要素を使用して設定する、さまざまなイメージ ファイルのアップロード間のアップロード遅延 (ミリ秒)。**uploadDelay** の推奨される設定は 500 ミリ秒です。
    
- App.config ファイルで、**ProfilePictureUploader_UPSvc_UserProfileService** 設定の **value** 要素を変更して、SharePoint Online 管理センター内のユーザー プロファイル サービスへの参照を組み込みます。以下に例を示します。

```XML  
<Contoso.Core.ProfilePictureUploader.Properties.Settings>
      <setting name="ProfilePictureUploader_UPSvc_UserProfileService"
            serializeAs="String">
            <value>https://contoso-admin.sharepoint.com/_vti_bin/userprofileservice.asmx</value>
      </setting>
 </Contoso.Core.ProfilePictureUploader.Properties.Settings>
```


              **重要** SharePoint Online 管理センター上の userprofileservice.asmx Web サービスに接続すると、他のユーザーのユーザー プロファイル プロパティを更新できます。このサンプル コードを実行するときには、ユーザー プロファイルを管理する権限を持つ Office 365 管理者アカウントを使用します。 

## Core.ProfilePictureUploader アプリの使用
<a name="sectionSection1"> </a>

このコード サンプルは、コンソール アプリケーションとして実行します。 このコード サンプルを実行すると、Program.cs の **Main** メソッドにより次の処理が実行されます。

- **SetupArguments** と **InitializeConfiguration** を使用して、コンソール アプリケーションを初期化します。 
    
- **InitializeWebService** を呼び出して、SharePoint Online 内のユーザー プロファイル サービスに接続します。
    
- userlist.csv ファイルを反復処理してユーザーのユーザー プリンシパル名 (UPN) とユーザーのイメージ ファイルの場所を読み取ります。 
    
- **GetImagefromHTTPUrl** 内の **WebRequest** オブジェクトと **WebResponse** オブジェクトを使用して、ユーザーのイメージを取得します。
    
- **UploadImageToSpo** を呼び出してユーザーのイメージを SharePoint Online にアップロードします。
    
- **SetMultipleProfileProperties** を呼び出して、ユーザーの **PictureURL** と **SPS-PicturePlaceholderState** のユーザー プロファイル プロパティを設定します。
    
- **SetAdditionalProfileProperties** を呼び出して、イメージ ファイルのアップロード後にユーザー プロファイルに追加のプロパティを設定します。

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
static void Main(string[] args)
        {
            int count = 0;

            if (SetupArguments(args)) // Checks if args passed are valid 
            {

                if (InitializeConfiguration()) // Check that the configuration file is valid
                {
                    if (InitializeWebService()) // Initialize the web service end point for the SharePoint Online user profile service.
                    {
                        
                        using (StreamReader readFile = new StreamReader(_appConfig.PictureSourceCsv))
                        {
                            string line;
                            string[] row;
                            string sPoUserProfileName;
                            string sourcePictureUrl;

                            while ((line = readFile.ReadLine()) != null)
                            {
                                if (count > 0)
                                {
                                    row = line.Split(',');
                                    sPoUserProfileName = row[0]; 
                                    sourcePictureUrl = row[1]; 

                                    LogMessage("Begin processing for user " + sPoUserProfileName, LogLevel.Warning);

                                    // Get source picture from source image path.
                                    using (MemoryStream picturefromExchange = GetImagefromHTTPUrl(sourcePictureUrl))
                                    {
                                        if (picturefromExchange != null) // if we got image, upload to SharePoint Online
                                        {
                                            // Create SharePoint naming convention for image file.
                                            string newImageNamePrefix = sPoUserProfileName.Replace("@", "_").Replace(".", "_");
                                            // Upload source image to SharePoint Online.
                                            string spoImageUrl = UploadImageToSpo(newImageNamePrefix, picturefromExchange);
                                            if (spoImageUrl.Length > 0)// If upload worked.
                                            {
                                                string[] profilePropertyNamesToSet = new string[] { "PictureURL", "SPS-PicturePlaceholderState" };
                                                string[] profilePropertyValuesToSet = new string[] { spoImageUrl, "0" };
                                                // Set these two required user profile properties - path to uploaded image, and pictureplaceholder state.
                                                SetMultipleProfileProperties(_sPOProfilePrefix + sPoUserProfileName, profilePropertyNamesToSet, profilePropertyValuesToSet);
                                                // Set additional user profile properties based on your requirements.
                                                SetAdditionalProfileProperties(_sPOProfilePrefix + sPoUserProfileName);
                                            }
                                        }
                                    }

                                    LogMessage("End processing for user " + sPoUserProfileName, LogLevel.Warning);

                                    int sleepTime = _appConfig.UploadDelay;
                                    System.Threading.Thread.Sleep(sleepTime); // A pause between uploads is recommended. 
                                }
                                count++;
                            }
                        }
                    }
                }
            }


            LogMessage("Processing finished for " + count + " user profiles", LogLevel.Information);
         } 

```


              **InitializeWebService** は SharePoint Online に接続し、ユーザー プロファイル サービスの参照をインスタンス変数に設定します。このコード サンプル内の他のメソッドはこのインスタンス変数を使用して、更新をユーザー プロファイル プロパティに適用します。このサンプル コードは、SharePoint Online 管理センター内の userprofileservice.asmx Web サービスを使用して、ユーザー プロファイルを管理します。

```
static bool InitializeWebService()
        {
            try
            {
                string webServiceExt = "_vti_bin/userprofileservice.asmx";
                string adminWebServiceUrl = string.Empty;

                // Append the web service (ASMX) URL onto the admin website URL.
                if (_profileSiteUrl.EndsWith("/"))
                    adminWebServiceUrl = _profileSiteUrl + webServiceExt;
                else
                    adminWebServiceUrl = _profileSiteUrl + "/" + webServiceExt;

                LogMessage("Initializing SPO web service " + adminWebServiceUrl, LogLevel.Information);

                // Get secure password from clear text password.
                SecureString securePassword = GetSecurePassword(_sPoAuthPasword);

                // Set credentials from SharePoint Client API, used later to extract authentication cookie, so can replay to web services.
                SharePointOnlineCredentials onlineCred = new SharePointOnlineCredentials(_sPoAuthUserName, securePassword);

                // Get the authentication cookie by passing the URL of the admin website. 
                string authCookie = onlineCred.GetAuthenticationCookie(new Uri(_profileSiteUrl));

                // Create a CookieContainer to authenticate against the web service. 
                CookieContainer authContainer = new CookieContainer();

                // Put the authenticationCookie string in the container. 
                authContainer.SetCookies(new Uri(_profileSiteUrl), authCookie);

                // Setting up the user profile web service. 
                _userProfileService = new UPSvc.UserProfileService();

                // Assign the correct URL to the admin profile web service. 
                _userProfileService.Url = adminWebServiceUrl;

                // Assign previously created authentication container to admin profile web service. 
                _userProfileService.CookieContainer = authContainer;
               // LogMessage("Finished creating service object for SharePoint Online Web Service " + adminWebServiceUrl, LogLevel.Information);
                return true;
            }
            catch (Exception ex)
            {
                LogMessage("Error initiating connection to profile web service in SPO " + ex.Message, LogLevel.Error);
                return false;

            }

            
        }

```

Program.cs 内の **Main** メソッドは **UploadImageToSpo** を呼び出してユーザーのプロファイル画像を SharePoint Online にアップロードします。すべてのユーザーのプロファイル画像は個人用サイトのホスト上の画像ライブラリに格納されます。**UploadImageToSpo** は以下のタスクを実行します。

- SharePoint Online に接続します。
    
- ユーザーごとにアップロードするイメージの数を決めます。 
    
    - ユーザーごとに 1 つのイメージをアップロードするようにアプリケーションを構成した場合、イメージは画像ライブラリにアップロードされます。 
    
    - ユーザーごとに 3 つのイメージをアップロードするようにアプリケーションを構成した場合、アプリケーションは構成ファイル内の **createSMLThumbs** の値を調べて、3 種類のサイズのイメージが必要かどうかを判別します。3 種類のサイズのイメージが必要な場合は、アプリケーションは **ResizeImageSmall** と **ResizeImageLarge** を使用してソース イメージから 3 種類のサイズのイメージを作成します。その後アプリケーションは、サイズ変更されたイメージを画像ライブラリにアップロードします。3 種類のサイズのイメージが必要でない場合は、アプリケーションは同じサイズの 3 つのイメージを画像ライブラリにアップロードします。

```C#
static string UploadImageToSpo(string PictureName, Stream ProfilePicture)
        {
            try
            {

                string spPhotoPathTempate = "/User Photos/Profile Pictures/{0}_{1}Thumb.jpg"; // Path template to picture library on the My Site Host.
                string spImageUrl = string.Empty;

                // Create SharePoint Online Client context to My Site Host.
                ClientContext mySiteclientContext = new ClientContext(_mySiteUrl);
                SecureString securePassword = GetSecurePassword(_sPoAuthPasword);
                // Provide authentication credentials using Office 365 authentication.
                mySiteclientContext.Credentials = new SharePointOnlineCredentials(_sPoAuthUserName, securePassword);
                                
                if (!_appConfig.Thumbs.Upload3Thumbs) // Upload a single input image only to picture library, no resizing necessary.
                {
                    spImageUrl = string.Format(spPhotoPathTempate, PictureName, "M");
                    LogMessage("Uploading single image, no resize, to " + spImageUrl, LogLevel.Information);
                    Microsoft.SharePoint.Client.File.SaveBinaryDirect(mySiteclientContext, spImageUrl, ProfilePicture, true);
                }
                else if (_appConfig.Thumbs.Upload3Thumbs &amp;&amp; !_appConfig.Thumbs.CreateSMLThumbs)// Upload three images of the same size. 
                {
                    // The following code is not optimal. Upload the same source image three times with different names.
                    // No resizing of images necessary.
                    LogMessage("Uploading threes image to SPO, no resize", LogLevel.Information);

                    spImageUrl = string.Format(spPhotoPathTempate, PictureName, "M");
                    LogMessage("Uploading medium image to " + spImageUrl, LogLevel.Information);
                    Microsoft.SharePoint.Client.File.SaveBinaryDirect(mySiteclientContext, spImageUrl, ProfilePicture, true);

                    ProfilePicture.Seek(0, SeekOrigin.Begin);
                    spImageUrl = string.Format(spPhotoPathTempate, PictureName, "L");
                    LogMessage("Uploading large image to " + spImageUrl, LogLevel.Information);
                    Microsoft.SharePoint.Client.File.SaveBinaryDirect(mySiteclientContext, spImageUrl, ProfilePicture, true);
                    
                    ProfilePicture.Seek(0, SeekOrigin.Begin);
                    spImageUrl = string.Format(spPhotoPathTempate, PictureName, "S");
                    LogMessage("Uploading small image to " + spImageUrl, LogLevel.Information);
                    Microsoft.SharePoint.Client.File.SaveBinaryDirect(mySiteclientContext, spImageUrl, ProfilePicture, true);

                    
                }
                else if (_appConfig.Thumbs.Upload3Thumbs &amp;&amp; _appConfig.Thumbs.CreateSMLThumbs) //generate 3 different sized images
                {
                    LogMessage("Uploading threes image to SPO, with resizing", LogLevel.Information);
                    // Create three images based on recommended sizes for SharePoint Online.
                    // Create small-sized image.
                    using (Stream smallThumb = ResizeImageSmall(ProfilePicture, _smallThumbWidth))
                    {
                        if (smallThumb != null)
                        {
                            spImageUrl = string.Format(spPhotoPathTempate, PictureName, "S");
                            LogMessage("Uploading small image to " + spImageUrl, LogLevel.Information);
                            Microsoft.SharePoint.Client.File.SaveBinaryDirect(mySiteclientContext, spImageUrl, smallThumb, true);                            
                        }
                    }

                    // Create medium-sized image.
                    using (Stream mediumThumb = ResizeImageSmall(ProfilePicture, _mediumThumbWidth))
                    {
                        if (mediumThumb != null)
                        {
                            spImageUrl = string.Format(spPhotoPathTempate, PictureName, "M");
                            LogMessage("Uploading medium image to " + spImageUrl, LogLevel.Information);
                            Microsoft.SharePoint.Client.File.SaveBinaryDirect(mySiteclientContext, spImageUrl, mediumThumb, true);
                           
                        }
                    }

                    // Create large-sized image. This image is shown when you open the user's OneDrive for Business. 
                    using (Stream largeThumb = ResizeImageLarge(ProfilePicture, _largeThumbWidth))
                    {
                        if (largeThumb != null)
                        {

                            spImageUrl = string.Format(spPhotoPathTempate, PictureName, "L");
                            LogMessage("Uploading large image to " + spImageUrl, LogLevel.Information);
                            Microsoft.SharePoint.Client.File.SaveBinaryDirect(mySiteclientContext, spImageUrl, largeThumb, true);
                            
                        }
                    }
                  
                   
                }
                // Return URL of the medium-sized image to set properties on the user profile.
                return _mySiteUrl + string.Format(spPhotoPathTempate, PictureName, "M");                
                
            }
            catch (Exception ex)
            {
                LogMessage("User Error: Failed to upload thumbnail picture to SPO for " + PictureName + " " + ex.Message, LogLevel.Error);
                return string.Empty;
            }

        }

```


              **SetMultipleProfileProperties** は 1 回のメソッドの呼び出しで複数のユーザー プロファイル プロパティを設定します。このコード サンプル内の **SetMultipleProfileProperties** は、ユーザーの以下のユーザー プロファイル プロパティを設定します。

-  
            **PictureURL** - 個人用サイトのホスト上の画像ライブラリ内にアップロードした中サイズのイメージの URL に設定します。
    
-  
            **SPS-PicturePlaceholderState** - ゼロに設定して、SharePoint Online がアップロード済みの画像をユーザーに対して表示するように指示します。

```C#
static void SetMultipleProfileProperties(string UserName, string[] PropertyName, string[] PropertyValue)
        {

            LogMessage("Setting multiple SPO user profile properties for " + UserName, LogLevel.Information);

            try
            {
                int arrayCount = PropertyName.Count();

                UPSvc.PropertyData[] data = new UPSvc.PropertyData[arrayCount];
                for (int x = 0; x < arrayCount; x++)
                {
                    data[x] = new UPSvc.PropertyData();
                    data[x].Name = PropertyName[x];
                    data[x].IsValueChanged = true;
                    data[x].Values = new UPSvc.ValueData[1];
                    data[x].Values[0] = new UPSvc.ValueData();
                    data[x].Values[0].Value = PropertyValue[x];
                }

                _userProfileService.ModifyUserPropertyByAccountName(UserName, data);
                // LogMessage("Finished setting multiple SharePoint Online user profile properties for " + UserName, LogLevel.Information);

            }
            catch (Exception ex)
            {
                LogMessage("User Error: Exception trying to update profile properties for user " + UserName + "\n" + ex.Message, LogLevel.Error);
            }
        }

```


              **SetAdditionalProfileProperties** は、イメージ ファイルのアップロード後に更新する追加のユーザー プロファイル プロパティを設定します。Configuration.xml ファイル内で、更新する追加のプロパティを指定できます。

```C#
static void SetAdditionalProfileProperties(string UserName)
        {
            if (_appConfig.AdditionalProfileProperties.Properties == null) // If there are no additional properties to update. 
                return;

            int propsCount = _appConfig.AdditionalProfileProperties.Properties.Count();
            if (propsCount > 0)
            {
                string[] profilePropertyNamesToSet = new string[propsCount];
                string[] profilePropertyValuesToSet = new string[propsCount];
                // Loop through each property in configuration file.
                for (int i = 0; i < propsCount; i++)
                {
                    profilePropertyNamesToSet[i] = _appConfig.AdditionalProfileProperties.Properties[i].Name;
                    profilePropertyValuesToSet[i] = _appConfig.AdditionalProfileProperties.Properties[i].Value;
                }

                // Set all properties in a single call.
                SetMultipleProfileProperties(UserName, profilePropertyNamesToSet, profilePropertyValuesToSet);

            }
        }

```

## 予定表、連絡先
<a name="bk_addresources"> </a>

-  [SharePoint 2013 と SharePoint Online のユーザー プロファイル ソリューション](user-profile-solutions-for-sharepoint.md)
    
-  [Core.ProfilePictureUploader アプリ](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ProfilePictureUploader)
    
-  [UserProfile.Manipulation.CSOM](https://github.com/OfficeDev/PnP/tree/master/Samples/UserProfile.Manipulation.CSOM)
    
-  [Core.ProfileProperty.Migration](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ProfileProperty.Migration)
    
-  [UserProfile.Manipulation.CSOM.Console](https://github.com/OfficeDev/PnP/tree/master/Samples/UserProfile.Manipulation.CSOM.Console)
