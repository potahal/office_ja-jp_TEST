# SharePoint 開発ツールとデザイン ツールおよび操作

SharePoint のデザイン ツールと開発ツールを使用して、SharePoint サイトにブランド化を適用できます。

_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Online_

この記事では、SharePoint で利用可能な開発とデザインのオプションに関する情報を取り上げます。また、リモート プロビジョニング パターンを使用して SharePoint サイトにブランド化アセットを適用する方法も記します。

## SharePoint 開発とデザインに関する主要な用語と概念
<a name="sectionSection0"> </a>

|**用語または概念**|**定義**|**詳細情報**|
|:-----|:-----|:-----|
|デザイン マネージャー|発行機能が有効にされている SharePoint 発行サイトまたはチーム サイトでアクティブにされている機能で、サイトのブランド化アセットのインポートや管理、およびデザイン パッケージへのアセットのエクスポートに使用できます。|デザイン マネージャーを使用して、Adobe PhotoShop や Adobe DreamWeaver などの他のツールで作成したブランド化アセットを SharePoint にインポートします。<br/>SharePoint Designer は、OneDrive for Business または発行機能が有効化されていない SharePoint チーム サイトには使用できません。|
|デザイン パッケージ|SharePoint 2013 発行サイトで使用するように設計され、デザイン マネージャーに格納されているブランド化アセットが含まれています。| [SharePoint 2013 Design Manager デザイン パッケージ](http://msdn.microsoft.com/library/85ad1993-4d75-4806-9097-b934865a899a.aspx)|
|リモート プロビジョニング|プロバイダー向けのホスト型アドインで SharePoint 外で実行するテンプレートとコードを使用してサイトのプロビジョニングを実行する際に使用するモデルです。| [SharePoint 2013 でのサイト プロビジョニングの手法とリモート プロビジョニング](http://blogs.msdn.com/b/vesku/archive/2013/08/23/site-provisioning-techniques-and-remote-provisioning-in-sharepoint-2013.aspx)<br/>[SharePoint 2013 のアドインを使用したセルフサービス サイトのプロビジョニング](http://blogs.msdn.com/b/richard_dizeregas_blog/archive/2013/04/04/self-service-site-provisioning-using-apps-for-sharepoint-2013.aspx)|
|ルート Web|サイト コレクション内の最初の Web。場合によっては、ルート Web は Web アプリケーション ルートを指すこともあります。||
|セキュリティで保護されたソリューション|アセンブリ、コンパイルされていない他のコンポーネント、XML マニフェスト ファイルが含まれる .wsp ファイル。セキュリティで保護されたソリューションでは、部分的に信頼されているコードが使用されます。| [セキュリティで保護されたソリューション](https://msdn.microsoft.com/en-us/library/ff798382.aspx)|
|SharePoint Designer 2013|HTML デザイナー、および SharePoint でブレンド化要素を管理するためのデザイン アセット管理ツール。 SharePoint 2013 では、主に SharePoint Designer によってカスタム ワークフローがサポートされています。| [SharePoint Designer 2013 での変更点](https://msdn.microsoft.com/en-us/library/office/jj728659.aspx)<br/>[SharePoint 2013 サイト開発の最新情報](https://msdn.microsoft.com/en-us/library/office/jj163942.aspx)|
|.wsp ファイル|SharePoint ソリューション ファイル。.wsp はサイト アセットをカテゴリに分類し、manifest.xml ファイルに整理する .cab ファイルです。| [ソリューションの概要](https://msdn.microsoft.com/en-us/library/office/aa543214%28v=office.14%29.aspx)|

## 開発オプション
<a name="sectionSection1"> </a>

SharePoint 2013 を開発プラットフォームとして使用する場合、コンテンツを開発、テスト、ビルド、展開するための環境の作成が必要になります。 開発オプションの詳細については、「[SharePoint Server 2013 のアプリケーション ライフサイクル管理](http://msdn.microsoft.com/library/caaf9a09-2e6a-49e3-a8d6-aaf7f93a842a.aspx)」の「[開発環境の注意点](http://msdn.microsoft.com/library/caaf9a09-2e6a-49e3-a8d6-aaf7f93a842a.aspx#DevEnvironment)」を参照してください。 表 2 に、各種開発オプションの考慮事項を示します。 

**SharePoint の開発、テスト、承諾のオプション**

|**オプション**|**考慮事項**|
|:-----|:-----|
|Team Foundation Server|- 簡単にアクセスできるように Visual Studio Online に配置されています。<br/>- ソース コードとライフ サイクルの一元化管理システムが含まれています。|
|クラウドのテストと承諾の環境|- 承諾テストのために別個のテナントを使用します。<br/>- オンプレミスのテスト用の別個のテスト環境を備えます。|
|オンプレミスのテストと承諾の環境|- オンプレミスの SharePoint の展開に使用します。<br/>- お客様のオンプレミスまたは Microsoft Azure によってホストされます。|
 
ほとんどの場合は、少なくとも以下のテナントが必要になります。ただし、要件によって異なる可能性があります。

-  
                **開発テナント**。 独自の開発者サイトをプロビジョニングして使用するのがベスト プラクティスです。 このようにすると、開発者のデータが運用環境に混入することを避けられます。 開発者サイトにサインアップしてプロビジョニングするには、「[Office 365 Developer サブスクリプションにサインアップしてツールと環境をセットアップする](http://msdn.microsoft.com/library/b22ce52a-ae9e-4831-9b68-c9210af6dc54.aspx)」の「[Office 365 開発者向けサイトにサインアップする](http://msdn.microsoft.com/library/b22ce52a-ae9e-4831-9b68-c9210af6dc54.aspx#o365_signup)」を参照してください。
    
-  
              **統合/テスト テナント**。このサイトは、新しいアプリおよび機能が複数のサイト コレクションで作動すること、および運用環境でサービスとデータが作動することを確認する場合に使用します。プレビュー段階の機能が含まれるように環境を構成します (そのためには、テナント管理コンソールで **[サービスの設定]** を選択し、**[更新]** 設定で **[最初のリリース]** を選択します)。Visual Studio Online を使用すると、自動テストを実行したり、その他の継続的統合テストを実行したりできます。
    
-  
              **運用テナント**。テスト、受け入れ、承認が済んだアプリをこのテナントにリリースします。このテナントに開発者サイトを作成し、対象範囲の小規模な、または他に影響を与えないアプリを開発してテストできます。通常、開発環境と運用環境が混在しないようにします。
    
## デザイン ツール
<a name="sectionSection2"> </a>

標準の Web デザイン ツールと開発ツールを使用して、HTML ファイル、イメージ ファイル、CSS ファイル、JavaScript ファイルなどの SharePoint サイト ブランド化アセットを作成します。たとえば、Adobe DreamWeaver および Adobe PhotoShop を使用して、SharePoint サイトのブランド化に使用する HTML ファイル、CSS ファイル、JavaScript ファイル、イメージ ファイルをデザインできます。または、SharePoint Designer 2013 を使用して、ブランド化アセットの作成、管理、カスタマイズ、または Visual Studio 2013 におけるカスタム ソリューションの作成も可能です。

### SharePoint デザイン パッケージと .wsp ファイル

デザイン パッケージは、デザイン アセットをパッケージ化するための予測可能な規則に従う、デザイン マネージャーによって作成される .wsp ファイルです。本質的には、セキュリティで保護されたソリューションです。別のツールを使用して .wsp ファイルにブランド化アセットをパッケージする場合には、ブランド化アセットがこのような一定の予測可能な状態にはなりません。

デザイン パッケージには、カスタマイズされたファイルすべてが含まれます。たとえば、カスタム コンテンツ タイプを使用したページ レイアウトを作成した場合、デザイン パッケージにはそのページ レイアウト、ページ レイアウトが使用するカスタム コンテンツ タイプ、すべてのカスタム サイト列が含まれます。また、次の場所にアップロードされたファイルを含む、ご使用の SharePoint サイトに適用された構成済み外観に関連するいくつかのファイルもデザイン パッケージに入ります。

- サイト アセット ライブラリ
    
- スタイル ライブラリ
    
- マスター ページ ギャラリー
    
サイトに対してカスタムのブランド化を適用する前に構成済み外観を適用した場合には、デザイン パッケージに .themedcss と .themedpng ファイル拡張子のファイルが含まれます。SharePoint サイトに対してデザイン パッケージ内のブランド化アセットを適用するには、デザイン パッケージをエクスポートして、リモート プロビジョニング パターンを使用してデザイン パッケージのコンテンツを適用します。

SharePoint 2013 には、デザイン パッケージの操作に使用できる API が含まれています。 SSOM、CSOM、または JSOM のいずれかを使用している場合は、[DesignPackage](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.designpackage.aspx) クラスまたは [DesignPackageInfo](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.designpackageinfo.aspx) クラスを使用できます。

#### デザイン パッケージ CSOM を使用してデザイン パッケージのコンテンツを SharePoint サイトに適用する

次の例は、リモート プロビジョニング パターンでデザイン パッケージ API を使用して、デザイン パッケージのコンテンツを SharePoint サイトに適用する方法を示しています。

このコードは、発行サイトで使用することを目的に設計されました。デザイン パッケージ API を使用して発行機能が有効になっているチーム サイトにブランド化を適用することもできますが、長期にわたるサポートに関する問題が生じる可能性があります。

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```
using Microsoft.SharePoint.Client;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using Microsoft.SharePoint.Client.Publishing;
namespace ProviderSharePointAppWeb
{
    public partial class Default : System.Web.UI.Page
    {
        protected void Page_PreInit(object sender, EventArgs e)
        {
            Uri redirectUrl;
            switch (SharePointContextProvider.CheckRedirectionStatus(Context, out redirectUrl))
            {
                case RedirectionStatus.Ok:
                    return;
                case RedirectionStatus.ShouldRedirect:
                    Response.Redirect(redirectUrl.AbsoluteUri, endResponse: true);
                    break;
                case RedirectionStatus.CanNotRedirect:
                    Response.Write("An error occurred while processing your request.");
                    Response.End();
                    break;
            }
        }

        protected void Page_Load(object sender, EventArgs e)
        {
            // Use TokenHelper to get the client context and Title property.
            // To access other properties, the add-in might need to request permissions
            // on the host web.
            var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);
            
            // Publishing feature GUID to use the infrastructure for publishing. 
            Guid PublishingFeature = Guid.Parse("f6924d36-2fa8-4f0b-b16d-06b7250180fa");

            // The site-relative URL of the design package to install.
            // This sandbox design package should be uploaded to a document library.
            // For practical purposes, this can be a configuration setting in web.config.
            string fileRelativePath = @"/sites/devsite/brand/Dev.wsp";

            //string fileUrl = @"https://SPXXXXX.com/sites/devsite/brand/Dev.wsp";
            
        
            using (var clientContext = spContext.CreateUserClientContextForSPHost())
            {
                // Load the site context explicitly or while installing the API, the path for
// the package will not be resolved.
                // If the package cannot be found, an exception is thrown. 
                var site = clientContext.Site;
                clientContext.Load(site);
                clientContext.ExecuteQuery();
              
                // Validate whether the Publishing feature is active. 
                if (IsSiteFeatureActivated(clientContext,PublishingFeature))
                {
                    DesignPackageInfo info = new DesignPackageInfo()
                    {
                        PackageGuid = Guid.Empty,
                        MajorVersion = 1,
                        MinorVersion = 1,
                        PackageName = "Dev"
                    };
                    Console.WriteLine("Installing design package ");
                    
                    DesignPackage.Install(clientContext, clientContext.Site, info, fileRelativePath);
                    clientContext.ExecuteQuery();

                    Console.WriteLine("Applying design package");
                    DesignPackage.Apply(clientContext, clientContext.Site, info);
                    clientContext.ExecuteQuery();
                }
            }
        }
        public  bool IsSiteFeatureActivated( ClientContext context, Guid guid)
        {
            var features = context.Site.Features;
            context.Load(features);
            context.ExecuteQuery();

            foreach (var f in features)
            {
                if (f.DefinitionId.Equals(guid))
                    return true;
            }
            return false;
        }
 
    }
}
```

#### FileCreationInformation を使用してブランド化アセットとマスター ページをチーム サイトにアップロードする

SharePoint 2013 CSOM 機能を使用すると、デザイン パッケージのインストールとアンインストール、および SharePoint Online サイトへのデザイン パッケージのエクスポートを実行できます。 たとえば、[SP.Publishing.DesignPackage.install Method (sp.publishing)](http://msdn.microsoft.com/library/26500127-210f-6c52-c0de-cf2894939a91.aspx) を使用して、次の例に示されているように、デザイン パッケージをサイトにインストールできます。

```
public static void Install(
        ClientRuntimeContext context,
        Site site,
        DesignPackageInfo info,
        string path
)
```

[DesignPackageInfo](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.publishing.designpackageinfo.aspx) クラスでは、インストールするデザイン パッケージのコンテンツを記述するメタデータを指定します。 [Uninstall](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.designpackage.uninstall.aspx) メソッドは、次の例に示されているように、サイトからデザイン パッケージをアンインストールする場合に使用します。

```
public static void UnInstall(
        ClientRuntimeContext context,
        Site site,
        DesignPackageInfo info
)
```

発行機能が有効にされているチーム サイトをブランド化する必要がある場合や、SharePoint Online 上の発行サイトをブランドする必要がある場合は、[ExportEnterprise](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.designpackage.exportenterprise.aspx) メソッドまたは [ExportSmallBusiness](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.designpackage.exportsmallbusiness.aspx) メソッドを使用することで、サイト テンプレートのデザイン パッケージをソリューション ギャラリーにエクスポートできます。 次の例に示されているように、小規模ビジネスのサイト テンプレートに **ExportSmallBusiness** メソッドを使用し、その他のすべてのサイト テンプレートには **ExportEnterprise** メソッドを使用します。 この例では、packageName はデザイン パッケージの名前を示す文字列になります。

```
public static ClientResult<DesignPackageInfo> ExportEnterprise(
        ClientRuntimeContext context,
        Site site,
        bool includeSearchConfiguration
)
```

このメソッドを使用する場合、次の例に示されているように、デザイン パッケージに検索構成を含めることが可能です。すべてのデザイン パッケージ メソッドが、対象サイト コレクション レベルで作用することに注意してください。

```
public static ClientResult<DesignPackageInfo> ExportSmallBusiness(
        ClientRuntimeContext context,
        Site site,
        string packageName,
        bool includeSearchConfiguration
)
```

## SharePoint Online のデザイン ツール オプション
<a name="sectionSection3"> </a>

SharePoint Online サイトのブランド化に使用できるツールは、ブランド化に使用する SharePoint Online エディションとサイトの種類によって異なります。たとえば、Small Business エディションには 1 つのチーム サイトと 1 つのパブリック サイトが含まれます。発行サイトは含まれません。SharePoint Online のサイト ビルダー アドインを使用すると、パブリック サイトのブランド化をカスタマイズできます。

Enterprise エディションには、発行は含まない、ドメインのルート Web アプリケーションのチーム サイト コレクションが含まれます。パブリック サイトは含まれません。デザイン マネージャーを使用して、SharePoint Online Enterprise エディションの発行サイトの SharePoint サイト ブランド化要素を管理します。

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint 2013 および SharePoint Online のブランディングおよびサイト プロビジョニング ソリューション](Branding-and-site-provisioning-solutions-for-SharePoint.md)
    
-  [SharePoint Server 2013 のアプリケーション ライフサイクル管理](http://msdn.microsoft.com/library/caaf9a09-2e6a-49e3-a8d6-aaf7f93a842a.aspx)
    
-  [セキュリティで保護されたソリューション](https://msdn.microsoft.com/en-us/library/ff798382.aspx)
    
-  [SharePoint 2013 でのサイト プロビジョニングの手法とリモート プロビジョニング](http://blogs.msdn.com/b/vesku/archive/2013/08/23/site-provisioning-techniques-and-remote-provisioning-in-sharepoint-2013.aspx)
    
-  [SharePoint 2013 Design Manager デザイン パッケージ](http://msdn.microsoft.com/library/85ad1993-4d75-4806-9097-b934865a899a.aspx)
