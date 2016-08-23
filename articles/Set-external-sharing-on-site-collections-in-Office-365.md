# Office 365 でサイト コレクションに外部共有を設定する

Office 365 の SharePoint サイト コレクションに対する外部共有の設定を制御すると、外部ユーザー (Office 365 サブスクリプションの組織アカウントを持っていないユーザー) が組織のサイト コレクションにアクセスできるようになります。

_
            **適用対象:** SharePoint 用アドイン | SharePoint Online | Office 365_

[Core.ExternalSharing](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ExternalSharing) コード サンプルは、SharePoint サイト コレクションに対する外部共有の設定を制御する方法を示しています。 このソリューションを使用して、以下を行うことができます。

- サイトのプロビジョニング プロセス中に外部共有の設定を制御します。
    
- 外部ユーザーと共有するようにサイト コレクションを準備します。


            **メモ:**外部共有の設定は、Office 365 でのみ利用可能です。

## はじめに
<a name="sectionSection0"> </a>

まず、[Core.ExternalSharing](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ExternalSharing) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## Core.ExternalSharing アプリを使用する
<a name="sectionSection1"> </a>

組織の Office 365 サブスクリプションで外部共有が許可されていることを確認します。 これを実行するには、次の操作を実行します。

1. **[Office 365 管理センター]** を開きます。
    
2. 左側のナビゲーション メニューで、 **[外部共有]** を選択します。
    
3. **[共有の概要]** を選択します。
    
4. **[サイト]** で、 **[外部ユーザーにサイトへのアクセスを許可します]** が **オン** であることを確認します。
    
SharePoint サイト コレクションに対する外部サイトの設定を確認します。それには、次の手順を実行します。

1. **Office 365 管理センター**を開きます。
    
2. 左側のナビゲーション メニューで  **[SharePoint]** を選択して、 **[SharePoint 管理センター]** を開きます。
    
3. 外部共有設定を確認する対象のサイト コレクションの URL の横にあるチェック ボックスをオンにします。
    
4. リボンで、 **[共有]** を選択します。
    
5. **[共有]** ダイアログで外部共有の設定を確認します。コード サンプルを実行した後、 **[共有]** ダイアログに戻り、外部共有の設定が変更されたことを確認します。
    
このコード サンプルを実行すると、Program.cs の " **Main** " によって次のタスクが実行されます。

- Office 365 管理センターの URL を取得します。
    
- 外部共有の設定をオンに構成するサイト コレクションの URL を取得します。
    
- Office 365 管理者の資格情報を取得します。
    
- **GetInputSharing** を呼び出します。このとき、サイト コレクションに適用する外部共有の設定 ([SharingCapabilities](https://msdn.microsoft.com/library/office/microsoft.online.sharepoint.tenantmanagement.sharingcapabilities.aspx)) を選択するようユーザーに求めるメッセージが表示されます。 外部共有の設定の選択肢は次のとおりです。
    
    -  
            **Disabled**。サイトに対する外部共有をオフにします。
    
    -  **ExternalUserAndGuestSharing** 。外部ユーザーとゲストがサイトを共有できるようにします。
    
    -  **ExternalUserSharingOnly** 。外部ユーザーとの共有のみを有効にします。
    
- **SetSiteSharing** を呼び出します。

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
 static void Main(string[] args)
        {
           
            /* Prompt for your Office 365 admin center URL*/
            Console.WriteLine("Enter your Tenant Admin URL for your Office 365 subscription:");
            string tenantAdminURL = GetSite();

            /* End Program if no Office 365 admin center URL is supplied*/
            if (string.IsNullOrEmpty(tenantAdminURL))
            {
                Console.WriteLine("Hmm, i tried to work on it but you didn't supply your admin tenant url:");
                return;
            }
               
            // Prompt the user for an Office365 site collection 
            Console.WriteLine("Enter your Office 365 Site Collection URL:");
            string siteUrl = GetSite();

            /* Prompt for Credentials */
            Console.WriteLine("Enter Credentials for your Office 365 Site Collection {0}:", siteUrl);

            string userName = GetUserName();
            SecureString pwd = GetPassword();

            /* End program if no credentials are entered */
            if (string.IsNullOrEmpty(userName) || (pwd == null))
            {
                Console.WriteLine("Hmm, i tried to work on it but you didn't supply your credentials:");
                return;
            }

            try 
            {
                SharingCapabilities _sharingSettingToApply = GetInputSharing(siteUrl);
                using (ClientContext cc = new ClientContext(tenantAdminURL))
                { 
                    cc.AuthenticationMode = ClientAuthenticationMode.Default;
                    cc.Credentials = new SharePointOnlineCredentials(userName, pwd);
                    SetSiteSharing(cc, siteUrl, _sharingSettingToApply);
                }
            }
            catch(Exception ex)
            {
                Console.WriteLine("Oops, Mistakes can happen to anyone. An Error occured : {0}", ex.Message);
               
            }

            Console.WriteLine("Hit Enter to exit.");
            Console.Read();

        
        }
```

**SetSiteSharing** は、以下の処理を実行します。

-  **Tenant.GetSitePropertiesByUrl** を使用して、サイト コレクションの **SiteProperties** を取得します。
    
- **Tenant.SharingCapability** を使用して、Office 365 サブスクリプションで外部共有が有効になっているかどうかを判別します。
    
-  Office 365 サブスクリプションで共有が有効になっている場合は、 **SiteProperties.SharingCapability** を、ユーザーが入力した外部共有の設定に設定します。

```C#
public static void SetSiteSharing(ClientContext adminCC, string siteCollectionURl, SharingCapabilities shareSettings)
        {
            var _tenantAdmin = new Tenant(adminCC);
            SiteProperties _siteprops = _tenantAdmin.GetSitePropertiesByUrl(siteCollectionURl, true);
            adminCC.Load(_tenantAdmin);
            adminCC.Load(_siteprops);
            adminCC.ExecuteQuery();

            SharingCapabilities _tenantSharing = _tenantAdmin.SharingCapability;
            var _currentShareSettings = _siteprops.SharingCapability;
            bool _isUpdatable = false;

            if(_tenantSharing == SharingCapabilities.Disabled)
            {
                Console.WriteLine("Sharing is currently disabled in your tenant! I am unable to work on it.");
            }
            else
            {  
                if(shareSettings == SharingCapabilities.Disabled)
                { _isUpdatable = true; }
                else if(shareSettings == SharingCapabilities.ExternalUserSharingOnly)
                {
                    _isUpdatable = true;   
                }
                else if (shareSettings == SharingCapabilities.ExternalUserAndGuestSharing)
                {
                    if (_tenantSharing == SharingCapabilities.ExternalUserAndGuestSharing)
                    {
                        _isUpdatable = true;
                    }
                    else
                    {
                        Console.WriteLine("ExternalUserAndGuestSharing is currently disabled in your tenant! I am unable to work on it.");
                    }
                }
            }
            if (_currentShareSettings != shareSettings &amp;&amp; _isUpdatable)
            {
                _siteprops.SharingCapability = shareSettings;
                _siteprops.Update();
                adminCC.ExecuteQuery();
                Console.WriteLine("Set Sharing on site {0} to {1}.", siteCollectionURl, shareSettings);
            }
        }
```

## その他のリソース
<a name="bk_addresources"> </a>

-  [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
-  [SharePoint Online 環境の外部共有を管理する](https://support.office.com/article/Manage-external-sharing-for-your-SharePoint-Online-environment-C8A462EB-0723-4B0B-8D0A-70FEAFE4BE85)
