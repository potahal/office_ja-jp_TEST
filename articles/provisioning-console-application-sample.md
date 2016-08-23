# コンソール アプリケーションのプロビジョニング サンプル

PnP プロビジョニング エンジンを用いてプロビジョニング テンプレートを作成して永続化し、新しい SharePoint サイト コレクションに適用する基本的な方法について説明します。

新しい アドイン モデルをサポートするために Office 365 Developer Patterns and Practicesプログラム (PnP) で導入されたプロビジョニング フレームワークを使用すると、ユーザーは、単にサイト モデルをポイントするだけでカスタムのサイト テンプレートを作成でき、そのモデルをプロビジョニング テンプレートとして永続化し、そのカスタム テンプレートを必要に応じて新規または既存のサイト コレクションに適用することができます。

このサンプルでは、PnP のコア プロビジョニング ライブラリのクラスを実装する基本的なコンソール アプリケーションを作成します。これらのクラスの実装により、PnP プロビジョニング エンジンが以下の必要なプロビジョニング タスクを実行することが可能になります。 

- サイトのカスタマイズを設計してモデル化します。新しいサイト デザインにすることも、既存のサイトをポイントして、それをプロビジョニング テンプレートとして保存することもできます。
    
- サイト モデルをプロビジョニング テンプレートとして保存して永続化し、再利用できるようにします。
    
- プロビジョニング テンプレートを新規または既存のサイト コレクションに適用します。
    
**メモ:** このサンプル チュートリアルは、現在 GitHub の「[PnP プロビジョニング エンジンの入門](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Framework.Console)」で入手できるサンプルのガイドです。このサンプルのコード (Program.cs) とソリューション ファイルは、ダウンロードして使用できます。また、Microsoft Channel 9 のサイトの「[PnP プロビジョニング エンジンの入門](https://channel9.msdn.com/blogs/OfficeDevPnP/Getting-Started-with-PnP-Provisioning-Engine)」にはこのプロセス (コードは若干異なります) について説明する 20 分間のビデオがあります。

## リモート プロビジョニングのチュートリアル

はじめに、Visual Studio プロジェクトを作成します。このサンプルでは、簡略化するために、プロビジョニング フレームワークの PnP コア ライブラリを使用して PnP プロビジョニング エンジンを実装する基本的なコンソール アプリケーションを作成します。ただし、このサンプル ソリューションをサポートするには、プロビジョニング フレームワーク PnP コア ライブラリをダウンロードしてインストールする必要があります。そのための手順を次に記します。

### Visual Studio プロジェクトの作成と準備

1. Visual Studio を起動し、[ **ファイル**]  >  [ **新規**]  >  [ **プロジェクト**] と選択します。
    
2. [ **新しいプロジェクト**] ウィザードで、[ **Visual C#**]、[ **コンソール アプリケーション**] と選択します。
    
3. プロジェクトに「Program」という名前を付け、[ **OK**] をクリックします (任意の名前を付けることができますが、このサンプル チュートリアルではプロジェクト名は「Program」とします)。
    
4. PnP コア ライブラリをダウンロードしてインストールします。これは NuGet パッケージとして 「 [OfficeDevPnP.Core パッケージ ](https://www.nuget.org/profiles/officedevpnp)」から入手できます。
    
    
                **メモ:**コア ライブラリには、2 つのバージョンがあります。 1 つ目のバージョンは、SharePoint Online と Office 365 をターゲットにした **OfficeDevPnP.Core** ライブラリです。 2 つ目のバージョンは、SharePoint 2013 オンプレミスをターゲットにした **OfficeDevPnP.Core (オンプレミス)** です。 使用可能なオプションのスクリーンショットを次に示します。

    ![2 つのコア ライブラリのダウンロードの選択肢](media/provisioning-console-application-sample/5b1adb8d-52e5-4c67-8792-6ef0ae41d655.png)

このサンプル チュートリアルでは、SharePoint Online を対象とする最初のオプションを使用しています。

1. 最初に、[NuGet クライアント インストーラー](http://docs.nuget.org/consume/installing-nuget)にアクセスして NuGet クライアントをインストールします。

2. NuGet クライアントのインストール後、 **NuGet パッケージ マネージャー**を実行します。Visual Studio の [ **ソリューション エクスプローラー**] で [ **参照**] ノードを右クリックし、[ **NuGet パッケージの管理**] を選択します。

3. パッケージ マネージャーで、[ **Online**]、[ **EntityFramework**] と選択し、検索用語として「OfficeDev」と入力して OfficeDevPnP.Core ライブラリを表示します。

4. 指示に従って  **OfficeDevPnP.Core** ライブラリをダウンロードしてインストールします。

   After the PnP Core library is referenced in your Visual Studio project, all library members are available to you as [extension methods](https://msdn.microsoft.com/en-us/library/bb383977.aspx) on existing object instances, for example, **web** and **list** instances.

5. Program.cs ファイルに以下のすべての  `using` ステートメントが含まれていることを確認します。

    ```c#
    using Microsoft.SharePoint.Client;
    using OfficeDevPnP.Core.Framework.Provisioning.Connectors;
    using OfficeDevPnP.Core.Framework.Provisioning.Model;
    using OfficeDevPnP.Core.Framework.Provisioning.ObjectHandlers;
    using OfficeDevPnP.Core.Framework.Provisioning.Providers.Xml;
    using System;
    using System.Net;
    using System.Security;
    using System.Threading;
    ```

**プロジェクトのセットアップ後**

プロジェクトをセットアップしたら、サイトのカスタマイズを作成できます。 カスタマイズの作成は、手動でもできますし、使用するデザインが使われているサイトをポイントすることによっても可能です。選択したサイト デザインをプロビジョニング テンプレートとして保存するだけです。または、両方の方法を組み合わせて使用することも可能です。このサンプルの目的上、ここでは単純に既存のサイトをポイントし、その構成済みの外観とサイト アーティファクト (コンテンツ以外) をプロビジョニング テンプレートとして保存します。

まず、プロビジョニング　テンプレートのモデルにするサイトに接続する必要があります。ユーザー名、パスワード、ソース URL などの接続情報を収集することから開始します。

### プロビジョニング テンプレートの作成、抽出、永続化

1. ユーザーから接続情報を収集します。プログラムの  `Main` ルーチンでは、接続情報の収集、プロビジョニング テンプレートの取得、プロビジョニング テンプレートの適用という 3 つの操作を行います。この大仕事は、以下で定義する **GetProvisioningTemplate** メソッドと **ApplyProvisioningTemplate** メソッドで行います。
    
    ```C#
    static void Main(string[] args)
        {
            ConsoleColor defaultForeground = Console.ForegroundColor;
    
            // Collect information 
            string templateWebUrl = GetInput("Enter the URL of the template site: ", false, defaultForeground);
            string targetWebUrl = GetInput("Enter the URL of the target site: ", false, defaultForeground);
            string userName = GetInput("Enter your user name:", false, defaultForeground);
            string pwdS = GetInput("Enter your password:", true, defaultForeground);
            SecureString pwd = new SecureString();
            foreach (char c in pwdS.ToCharArray()) pwd.AppendChar(c);
    
            // GET the template from existing site and serialize
            // Serializing the template for later reuse is optional
            ProvisioningTemplate template = GetProvisioningTemplate(defaultForeground, templateWebUrl, userName, pwd);
    
            // APPLY the template to new site from 
            ApplyProvisioningTemplate(defaultForeground, targetWebUrl, userName, pwd, template);
    
            // Pause and modify the UI to indicate that the operation is complete
            Console.ForegroundColor = ConsoleColor.White;
            Console.WriteLine("We're done. Press Enter to continue.");
            Console.ReadLine();
        }
    ```

2. プライベート  **GetInput** メソッドを作成し、これを使用して必要なユーザー資格情報を取得します。
    
    ```c#
    private static string GetInput(string label, bool isPassword, ConsoleColor defaultForeground)
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("{0} : ", label);
            Console.ForegroundColor = defaultForeground;
    
            string value = "";
    
            for (ConsoleKeyInfo keyInfo = Console.ReadKey(true); keyInfo.Key != ConsoleKey.Enter; keyInfo = Console.ReadKey(true))
            {
                if (keyInfo.Key == ConsoleKey.Backspace)
                {
                    if (value.Length > 0)
                    {
                        value = value.Remove(value.Length - 1);
                        Console.SetCursorPosition(Console.CursorLeft - 1, Console.CursorTop);
                        Console.Write(" ");
                        Console.SetCursorPosition(Console.CursorLeft - 1, Console.CursorTop);
                    }
                }
                else if (keyInfo.Key != ConsoleKey.Enter)
                {
                    if (isPassword)
                    {
                        Console.Write("*");
                    }
                    else
                    {
                        Console.Write(keyInfo.KeyChar);
                    }
                    value += keyInfo.KeyChar;
    
                }
    
            }
            Console.WriteLine("");
    
            return value;
        }
    ```

3. プロビジョニング テンプレートのモデルとなるサイトをポイントします。1 行のコードでソース サイトに対して取得操作を実行する際は、ここで定義した GET メソッド  `GetProvisioningTemplate()` を参照してください。 
    
    ```c#
    private static ProvisioningTemplate GetProvisioningTemplate(ConsoleColor defaultForeground, string webUrl, string userName, SecureString pwd)
        {
            using (var ctx = new ClientContext(webUrl))
            {
                // ctx.Credentials = new NetworkCredentials(userName, pwd);
                ctx.Credentials = new SharePointOnlineCredentials(userName, pwd);
                ctx.RequestTimeout = Timeout.Infinite;
    
                // Just to output the site details
                Web web = ctx.Web;
                ctx.Load(web, w => w.Title);
                ctx.ExecuteQueryRetry();
    
                Console.ForegroundColor = ConsoleColor.White;
                Console.WriteLine("Your site title is:" + ctx.Web.Title);
                Console.ForegroundColor = defaultForeground;
    
                ProvisioningTemplateCreationInformation ptci
                        = new ProvisioningTemplateCreationInformation(ctx.Web);
    
                // Create FileSystemConnector to store a temporary copy of the template 
                ptci.FileConnector = new FileSystemConnector(@"c:\temp\pnpprovisioningdemo", "");
                ptci.PersistComposedLookFiles = true;
                ptci.ProgressDelegate = delegate(String message, Int32 progress, Int32 total)
                {
                    // Only to output progress for console UI
                    Console.WriteLine("{0:00}/{1:00} - {2}", progress, total, message);
                };
    
                // Execute actual extraction of the template
                ProvisioningTemplate template = ctx.Web.GetProvisioningTemplate(ptci);
    
                // We can serialize this template to save and reuse it
                // Optional step 
                XMLTemplateProvider provider =
                        new XMLFileSystemTemplateProvider(@"c:\temp\pnpprovisioningdemo", "");
                provider.SaveAs(template, "PnPProvisioningDemo.xml");
    
                return template;
            }
        }
    ```

    上記のコード スニペットでは、**ProvisioningTemplateCreationInformation** 変数 (**pcti**) も定義します。 この変数により、テンプレートと共に保存できる成果物に関する情報を格納できます。
    
4. ファイル システム コネクター オブジェクトを作成し、別のサイトに適用するプロビジョニング テンプレートの一時的なコピーを格納できるようにします。
    
    
                **メモ:**この手順は省略できます。 XML にプロビジョニング テンプレートをシリアル化する必要はありません。 この段階で、テンプレートは単なる C# コードです。 シリアル化が省略可能であるだけでなく、必要なシリアル化形式を何でも使用できます。

5. 次の 1 行のコードを使用して、プロビジョニング テンプレートの抽出を実行します。      `ProvisioningTemplate template = ctx.Web.GetProvisioningTemplate(ptci);`
    
6. (省略可能) シリアル化したバージョンのプロビジョニング テンプレートを保存して格納し、再利用できるようにします。プロビジョニング テンプレートのシリアル化はどのような形式でも行えます。このサンプルでは、 **PnPProvisioningDemo.xml** という XML ファイルにシリアル化しました。このファイル自体は、ファイル システム ロケーションを指定した **XMLFileSystemTemplateProvider** オブジェクトです。
    
**プロビジョニング テンプレートの抽出、保存、永続化後**

プロビジョニング テンプレートを抽出、保存、永続化したら、次の最後の手順は、そのプロビジョニング テンプレートを新しい SharePoint サイト コレクションに適用することです。これを行うには、 **ApplyProvisioningTemplate** メソッドを使用します。

### 新規または既存のサイトへのプロビジョニング テンプレートの適用

1. 対象サイトの資格情報を取得します。
    
    ```c#
    // ctx.Credentials = new NetworkCredentials(userName, pwd);
                ctx.Credentials = new SharePointOnlineCredentials(userName, pwd);
                ctx.RequestTimeout = Timeout.Infinite;
    ```

2. コンパニオン メソッド  **ProvisioningTemplateApplyingInformation** を使用して、 **ProvisioningTemplateCreationInformation** メソッドによって格納されたサイト アーティファクトをキャプチャします。
    
    ```c#
    ProvisioningTemplateApplyingInformation ptai
                        = new ProvisioningTemplateApplyingInformation();
                ptai.ProgressDelegate = delegate(String message, Int32 progress, Int32 total)
                {
                    Console.WriteLine("{0:00}/{1:00} - {2}", progress, total, message);
                };
    ```

3. アセットのファイル コネクターへの関連付けを取得します。
    
    ```c#
    // Associate file connector for assets
                FileSystemConnector connector = new FileSystemConnector(@"c:\temp\pnpprovisioningdemo", "");
                template.Connector = connector;
    
    ```

4. (省略可能) プロビジョニング テンプレートはオブジェクト インスタンスであるため、実行時にサイト アーティファクトをカスタマイズするコードを作成できます。このインスタンスでは、新しい「contact」リストを追加します。
    
    ```c#
    // Since template is actual object, we can modify this using code as needed
                template.Lists.Add(new ListInstance()
                {
                    Title = "PnP Sample Contacts",
                    Url = "lists/PnPContacts",
                    TemplateType = (Int32)ListTemplateType.Contacts,
                    EnableAttachments = true
                });
    ```

5.  1 行のコードを使用して、新しいサイトにプロビジョニング テンプレートを適用します。
    
    ```c#
        web.ApplyProvisioningTemplate(template, ptai);
    ```

## その他のリソース
<a name="bk_addresources"> </a>

- [PnP プロビジョニング フレームワーク](pnp-provisioning-framework.md)
    
- [PnP プロビジョニング エンジンとコア ライブラリ](pnp-provisioning-engine-and-the-core-library.md)
