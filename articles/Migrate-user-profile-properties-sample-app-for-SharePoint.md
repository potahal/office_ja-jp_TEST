# ユーザー プロファイル プロパティの移行サンプル アドイン (SharePoint)

プロバイダー向けのホスト型アドインを使用して、SharePoint ユーザー プロファイル データの移行やインポートを実行できます。

_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Online_
    
[Core.ProfileProperty.Migration](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ProfileProperty.Migration) サンプル アドインは、SharePoint Server 2010 または SharePoint Server 2013 から SharePoint Online にユーザー プロファイル データを移行する方法を示します。
    
このサンプルには 2 つのコンソール アプリケーションが含まれています。その両方とも userprofileservice.asmx Web サービスを使用して、単数値または複数値を持つユーザー プロファイル データを XML ファイルに抽出し、抽出したデータを SharePoint Online 内のユーザー プロファイル サービスへインポートします。このコード サンプルは、以下の操作を行う場合に使用します。

- SharePoint Server 2010 または SharePoint Server 2013 内のユーザー プロファイル データを抽出します。
    
- ユーザー プロファイル データを SharePoint Online へインポートします。

## はじめに
<a name="sectionSection0"> </a>

まず、[Core.ProfileProperty.Migration](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ProfileProperty.Migration) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

**Contoso.ProfileProperty.Migration.Extract** プロジェクトの場合

- このコード サンプルではサーバー側オブジェクト モデルが使用されているので、SharePoint Server 2010 か SharePoint Server 2013 がインストールされているサーバー上でプロジェクトを実行しようとしていることを確認してください。
    
- SharePoint ファーム管理者権限のあるアカウントを使用してください。
    
- 表 1 で一覧表示されている構成情報を使用して App.config ファイルを編集してください。
    
- すべてのユーザーについて、**[勤務先の電子メール]** ユーザー プロファイル プロパティが空でないことを確認してください。**[勤務先の電子メール]** ユーザー プロファイル プロパティの値が空の場合は、抽出プロセスが途中で終了します。
    
- このコード サンプルは SharePoint Server 2010 からユーザー プロファイルを抽出します。 SharePoint Server 2013 からユーザー プロファイルを抽出する場合は、次に示す操作を実行します。
    
  a.  **[Contoso.ProfileProperty.Migration.Extract]** > **[プロパティ]** を右クリックして、ショートカット メニューを開きます。
    
  b.  **[アプリケーション]** の下の **[対象のフレームワーク]** で、**[.NET Framework 4]** を選択します。
    
  c. **[はい]** を選択してから **[保存]** を選択します。
    
**表 1.  App.Config ファイルの構成設定**

|**構成設定名**|**説明**|例|
|:-----|:-----|:-----|
|**MYSITEHOSTURL** |ソースの SharePoint Server 2010 または SharePoint Server 2013 ファーム上の個人用サイトの URL。|http://my.contoso.com |
|**PROPERTYSEPERATOR** |複数値を持つユーザー プロファイル プロパティ内で複数の値を区切るために使用する文字。| |
|**USERPROFILESSTORE** |抽出されたユーザー プロファイル データの書き込みに使用する XML ファイル パス。|C:\temp\ProfileData.xml|
|**LOGFILE** |抽出されたユーザー プロファイル データの書き込みに使用する XML ファイル パス。|C:\temp\Extract.log|
|**ENABLELOGGING** |ディスクのログ出力を有効にします。|True|
|**TESTRUN** |テスト抽出を実行して、App.Config 内の構成設定が正しいことを確認します。|テスト抽出を実行しようとしている場合は、`TESTRUN=true` を設定します。 テストを実行すると、ユーザー プロファイル サービスから 1 人のユーザーのみ抽出します。<br /> ユーザー プロファイル サービスからすべてのユーザーを抽出しようとしている場合は、`TESTRUN=false` を設定します。 |

**Contoso.ProfileProperty.Migration.Import** プロジェクトの場合

- Office 365 内にユーザー プロファイルがあることを確認してください。 
    
- ユーザーの **[勤務先の電子メール]** のアドレスが、オンプレミスの SharePoint Server 2013 と Office 365 ユーザー プロファイル サービスで同じであることを確認してください。
    
- 以下の例のように、App.config ファイル内の **Contoso_ProfileProperty_Migration_Import_UPSvc_UserProfileService** 設定の **value** 要素を変更して、SharePoint Online 管理センター内のユーザー プロファイル サービスへの参照を組み込んでください。

    ```XML
    <applicationSettings>
    <Contoso.ProfileProperty.Migration.Import.Properties.Settings>
    <setting name="Contoso_ProfileProperty_Migration_Import_UPSvc_UserProfileService" serializeAs="String">
    <value>https://contoso-admin.sharepoint.com/_vti_bin/userprofileservice.asmx</value>
    </setting>
    </Contoso.ProfileProperty.Migration.Import.Properties.Settings>
    </applicationSettings>
    ```

- 表 2 で一覧表示されている構成設定を使用して App.config ファイルを編集してください。
    
**表 2.  App.config ファイルの構成設定**

|**構成設定名**|**説明**|例|
|:-----|:-----|:-----|
|**tenantName** |テナントの名前です。|テナントの URL が http://contoso.onmicrosoft.com の場合、テナント名として「contoso」と入力します。|
|**PROPERTYSEPERATOR** |複数値を持つユーザー プロファイル プロパティ内で値を区切るために使用する文字。| |
|**USERPROFILESSTORE** |抽出されたユーザー プロファイルのデータの読み取りに使用する XML ファイル。|C:\temp\ProfileData.xml|
|**LOGFILE** |イベント ログに使用するログ ファイル。|C:\temp\Extract.log|
|**ENABLELOGGING** |ディスクのログ出力を有効にします。|True|
|**SPOAdminUserName** |Office 365 管理者のユーザー名。|該当なし。|
|**SPOAdminPassword** |Office 365 管理者のパスワード。|該当なし。|

## Core.ProfileProperty.Migration アプリの使用
<a name="sectionSection1"> </a>

このコード サンプルは、コンソール アプリケーションとして実行します。 このコード サンプルを実行すると、Program.cs の **Main** 関数により次の処理が実行されます。

- 個人用サイトのホストに接続し、**UserProfileManager** を使用してユーザー プロファイル サービスに接続します。**UserProfileManager** は **Microsoft.Office.Server.UserProfiles.dll** アセンブリに属しています。
    
- **pData** という一覧を作成して、抽出されたユーザー プロファイル データを格納します。
    
- ユーザー プロファイル サービス内のすべてのユーザーについて、以下の処理を行います。
    
    - **GetSingleValuedProperty** を使用して、**WorkEmail** と **AboutMe** のユーザー プロファイル プロパティを **userData** という **UserProfileData** オブジェクトにコピーします。
    
    - **GetMultiValuedProperty** を使用して、**SPS-Responsibility** ユーザー プロファイル プロパティを **userData** にコピーします。
    
- **UserProfileCollection.Save** を使用して、**userData** を XML ファイルにシリアル化します。 この XML ファイルは、App.config で指定したファイル パスに保存されます。

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
static void Main(string[] args)
        {
            int userCount = 1;

            try
            {

                if (Convert.ToBoolean(ConfigurationManager.AppSettings["TESTRUN"]))
                {
                    LogMessage(string.Format("******** RUNNING IN TEST RUN MODE **********"), LogLevel.Debug);
                }
                
                LogMessage(string.Format("Connecting to My Site Host: '{0}'...", ConfigurationManager.AppSettings["MYSITEHOSTURL"]), LogLevel.Info);
                using (SPSite mySite = new SPSite(ConfigurationManager.AppSettings["MYSITEHOSTURL"]))
                {
                    LogMessage(string.Format("Connecting to My Site Host: '{0}'...Done!", ConfigurationManager.AppSettings["MYSITEHOSTURL"]), LogLevel.Info);

                    LogMessage(string.Format("getting Service Context..."), LogLevel.Info);
                    SPServiceContext svcContext = SPServiceContext.GetContext(mySite);
                    LogMessage(string.Format("getting Service Context...Done!"), LogLevel.Info);

                    LogMessage(string.Format("Connecting to Profile Manager..."), LogLevel.Info);
                    UserProfileManager profileManager = new UserProfileManager(svcContext);
                    LogMessage(string.Format("Connecting to Profile Manager...Done!"), LogLevel.Info);

                    // Size of the List is set to the number of profiles.
                    List<UserProfileData> pData = new List<UserProfileData>(Convert.ToInt32(profileManager.Count));
                    
                    // Initialize Serialization Class.
                    UserProfileCollection ups = new UserProfileCollection();

                    foreach (UserProfile spUser in profileManager)
                    {
                        // Get Profile Information.
                        LogMessage(string.Format("processing user '{0}' of {1}...", userCount,profileManager.Count),LogLevel.Info);                       
                        UserProfileData userData = new UserProfileData();
                        
                        userData.UserName = GetSingleValuedProperty(spUser, "WorkEmail");
                        
                        if (userData.UserName != string.Empty)
                        {
                            userData.AboutMe = GetSingleValuedProperty(spUser, "AboutMe");
                            userData.AskMeAbout = GetMultiValuedProperty(spUser, "SPS-Responsibility");
                            pData.Add(userData);
                            // Add to Serialization Class List of Profiles.
                            ups.ProfileData = pData;
                        }
                        
                        LogMessage(string.Format("processing user '{0}' of {1}...Done!", userCount++, profileManager.Count), LogLevel.Info);

                        // Only process the first item if we are in test mode.
                        if (Convert.ToBoolean(ConfigurationManager.AppSettings["TESTRUN"]))
                        {
                            break;
                        }

                    }
                    
                    // Serialize profiles to disk.
                    ups.Save();

                }
            }
            catch(Exception ex)
            {
                LogMessage("Exception trying to get profile properties:\n" + ex.Message, LogLevel.Error);
            }
```

**GetSingleValuedProperty** メソッドは userprofileservice.asmx を使用して、単一値を持つユーザー プロファイル プロパティを取得することに注意してください。次のコード例のように、**GetSingleValuedProperty** は以下の処理を実行します。

- **spuser[userProperty]** を使用して、データの抽出元のプロパティ オブジェクトを取得します。
    
- 値が **null** でない場合、**UserProfileValueCollection** 内の最初の値を返します。 

```C#
private static string GetSingleValuedProperty(UserProfile spUser,string userProperty)
        {
            string returnString = string.Empty;
            try
            {
                UserProfileValueCollection propCollection = spUser[userProperty];

                if (propCollection[0] != null)
                {
                    returnString = propCollection[0].ToString();
                }
                else
                {
                    LogMessage(string.Format("User '{0}' does not have a value in property '{1}'", spUser.DisplayName, userProperty), LogLevel.Warning);                       
                }
            }
            catch 
            {
                LogMessage(string.Format("User '{0}' does not have a value in property '{1}'", spUser.DisplayName, userProperty), LogLevel.Warning);                       
            }


            return returnString;
            
        }
```

**GetMultiValuedProperty** メソッドは userprofileservice.asmx を使用して、複数値を持つユーザー プロファイル プロパティを取得することに注意してください。次のコード例のように、**GetMultiValuedProperty** は以下の処理を実行します。

- **spuser[userProperty]** を使用して、更新対象のユーザー プロファイル プロパティ オブジェクトを取得します。
    
- App.config ファイルで指定された **PROPERTYSEPARATOR** でユーザー プロファイル プロパティ値を区切った文字列を構築します。

```C#
private static string GetMultiValuedProperty(UserProfile spUser, string userProperty)
        {
            StringBuilder sb = new StringBuilder("");
            string seperator = ConfigurationManager.AppSettings["PROPERTYSEPERATOR"];

            string returnString = string.Empty;
            try
            {

                UserProfileValueCollection propCollection = spUser[userProperty];

                if (propCollection.Count > 1)
                {
                    for (int i = 0; i < propCollection.Count; i++)
                    {
                        if (i == propCollection.Count - 1) { seperator = ""; }
                        sb.AppendFormat("{0}{1}", propCollection[i], seperator);
                    }
                }
                else if (propCollection.Count == 1)
                {
                    sb.AppendFormat("{0}", propCollection[0]);
                }

            }
            catch
            {
                LogMessage(string.Format("User '{0}' does not have a value in property '{1}'", spUser.DisplayName, userProperty), LogLevel.Warning);
            }

            return sb.ToString();

        }
```

## Contoso.ProfileProperty.Migration.Import の使用
<a name="sectionSection2"> </a>

このコード サンプルは、コンソール アプリケーションとして実行します。 このコード サンプルを実行すると、Program.cs の **Main** メソッドにより次の処理が実行されます。

- **InitializeConfiguration** と **InitializeWebService** を使用してコンソール アプリケーションを初期化します。 
    
- 抽出されたユーザー プロファイル データが含まれている XML ファイルを逆シリアル化します。
    
- XML ファイル内のすべてのユーザーについて、以下の処理を行います。
    
    - XML ファイルから **UserName** プロパティを抽出します。
    
    - **SetSingleMVProfileProperty** を使用して、ユーザーのプロファイル上で **SPS-Responsibility** を設定します。
    
    - **SetSingleMVProfileProperty** を使用して、ユーザーのプロファイル上で **AboutMe** を設定します。
    

              **InitializeWebService** は SharePoint Online に接続し、ユーザー プロファイル サービスの参照をインスタンス変数に設定します。このコード サンプル内の他のメソッドはこのインスタンス変数を使用して、値をユーザー プロファイル プロパティに書き込みます。このサンプル コードは、SharePoint Online 管理センター内の userprofileservice.asmx Web サービスを使用して、ユーザー プロファイルを管理します。

```C#
static bool InitializeWebService()
        {
            try
            {
                string webServiceExt = "_vti_bin/userprofileservice.asmx";
                string adminWebServiceUrl = string.Empty;
                
                if (_profileSiteUrl.EndsWith("/"))
                    adminWebServiceUrl = _profileSiteUrl + webServiceExt;
                else
                    adminWebServiceUrl = _profileSiteUrl + "/" + webServiceExt;

                LogMessage("Initializing SPO web service " + adminWebServiceUrl, LogLevel.Information);

                SecureString securePassword = GetSecurePassword(_sPoAuthPasword);
                SharePointOnlineCredentials onlineCred = new SharePointOnlineCredentials(_sPoAuthUserName, securePassword);

                string authCookie = onlineCred.GetAuthenticationCookie(new Uri(_profileSiteUrl));

                CookieContainer authContainer = new CookieContainer();
                authContainer.SetCookies(new Uri(_profileSiteUrl), authCookie);

                // Setting up the user profile web service.
                _userProfileService = new UPSvc.UserProfileService();
                _userProfileService.Url = adminWebServiceUrl;

                // Assign previously created auth container to admin profile web service. 
                _userProfileService.CookieContainer = authContainer;
                return true;
            }
            catch (Exception ex)
            {
                LogMessage("Error initiating connection to profile web service in SPO " + ex.Message, LogLevel.Error);
                return false;

            }
            
        }
```

**SetSingleMVProfileProperty** メソッドは、以下のようにして、**SPS-Responsibility** などの複数値を持つユーザー プロファイル プロパティを設定します。

- **PropertyValue** を **arrs** という文字列配列に分割して、ユーザー プロファイル プロパティ値を格納します。App.Config 内で指定された **PROPERTYSEPERATOR** 構成設定を使用して、文字列が分割されます。
    
- **arrs** の値を、ユーザー プロファイル サービス上の **ValueData** 配列に割り当てます。
    
- ユーザー プロファイル サービス上に **PropertyData** 配列を作成します。ユーザー プロファイル プロパティの名前と **ValueData** 配列が **PropertyData** オブジェクト上のプロパティに渡されます。複数値を持つユーザー プロファイル プロパティが 1 つだけインポートされるので、この配列にある要素は 1 つだけです。
    
SharePoint Online 管理センターの **userprofileservice.asmx** Web サービス上で **ModifyUserPropertyByAccountName** を使用して、データがユーザー プロファイル サービスに書き込まれます。このコード サンプルを実行するユーザーは Office 365 管理者でなければなりません。

```C#
static void SetSingleMVProfileProperty(string UserName, string PropertyName, string PropertyValue)
        {

            try
            {
                string[] arrs = PropertyValue.Split(ConfigurationManager.AppSettings["PROPERTYSEPERATOR"][0]);
                
               UPSvc.ValueData[] vd = new UPSvc.ValueData[arrs.Count()];
               
               for (int i=0;i<=arrs.Count()-1;i++)
               {
                    vd[i] = new UPSvc.ValueData();
                    vd[i].Value = arrs[i];
                }
               
                UPSvc.PropertyData[] data = new UPSvc.PropertyData[1];
                data[0] = new UPSvc.PropertyData();
                data[0].Name = PropertyName;
                data[0].IsValueChanged = true;
                data[0].Values = vd;
                               
                _userProfileService.ModifyUserPropertyByAccountName(string.Format(@"i:0#.f|membership|{0}", UserName), data);

            }
            catch (Exception ex)
            {
                LogMessage("Exception trying to update profile property " + PropertyName + " for user " + UserName + "\n" + ex.Message, LogLevel.Error);
            }

        }

```

**SetSingleValuedProperty** メソッドは、**AboutMe** などの単数値を持つユーザー プロファイル プロパティを設定します。**SetSingleValuedProperty** は **SetSingleMVProfileProperty** と同じ手法を実装していますが、要素が 1 つだけある **ValueData** 配列を使用します。

```C#
static void SetSingleProfileProperty(string UserName, string PropertyName, string PropertyValue)
        {

            try
            {
                UPSvc.PropertyData[] data = new UPSvc.PropertyData[1];
                data[0] = new UPSvc.PropertyData();
                data[0].Name = PropertyName;
                data[0].IsValueChanged = true;
                data[0].Values = new UPSvc.ValueData[1];
                data[0].Values[0] = new UPSvc.ValueData();
                data[0].Values[0].Value = PropertyValue;
                _userProfileService.ModifyUserPropertyByAccountName(UserName, data);
            }
            catch (Exception ex)
            {
                LogMessage("Exception trying to update profile property " + PropertyName + " for user " + UserName + "\n" + ex.Message, LogLevel.Error);
            }

        }

```

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint 2013 と SharePoint Online のユーザー プロファイル ソリューション](user-profile-solutions-for-sharepoint.md)
    
-  [Core.ProfilePictureUploader](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ProfilePictureUploader)
    
-  [UserProfile.Manipulation.CSOM](https://github.com/OfficeDev/PnP/tree/master/Samples/UserProfile.Manipulation.CSOM)
    
-  [UserProfile.Manipulation.CSOM.Console](https://github.com/OfficeDev/PnP/tree/master/Samples/UserProfile.Manipulation.CSOM.Console)
