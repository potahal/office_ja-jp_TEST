# PnP プロビジョニング エンジンとコア ライブラリ

リモート プロビジョニング プロセスを概観し、OfficeDevPnP.Core ライブラリを詳しく説明します。

PnP プロビジョニング エンジンはプロビジョニング フレームワークの心臓部で、その基礎となるのは OfficeDevPnP.Core ライブラリです。プロビジョニング エンジンはコア ライブラリの一部で、コア ライブラリの拡張機能を活用してプロビジョニング タスクを実装します。SharePoint CSOM/REST オブジェクト モデルの拡張メソッドで構成されているコア ライブラリにより、プロビジョニング テンプレートの列挙と取得、テンプレートの格納、新規および既存のサイトへのテンプレートの適用といったプロビジョニング タスクが可能になります。また、プロビジョニング タスクの自動化や、コード化されたロジックをプロビジョニング ルーチンに導入することも可能となります。

**メモ:** プロビジョニング テンプレートの作成、永続化、適用に関するビデオ チュートリアルを視聴するには、「[PnP プロビジョニング エンジンの入門](https://channel9.msdn.com/blogs/OfficeDevPnP/Getting-Started-with-PnP-Provisioning-Engine)」にアクセスしてください。

## PnP プロビジョニング エンジン

PnP プロビジョニング エンジンは、リモート プロビジョニング タスクの実行と自動化における、コア ライブラリ内のクラスの構造化された実装です。SharePoint アドイン モデルを特色とする新しい Microsoft 環境では、SharePoint Online、SharePoint 2013、Office 365 のプロビジョニング ソリューションにおいてクライアント オブジェクト モデル (CSOM) とプロビジョニング フレームワークを使用してサイト アーティファクトをプロビジョニングするようになりました。

PnP プロビジョニング エンジンを使用すると、サイト列、コンテンツの種類、リスト定義、構成済み外観、ページのデザインをモデル化できます。こうした機能のデザインは、既存のサイト デザイン機能をポイントするか、デザインを手動で作り上げるか、あるいはその両方の方法を組み合わせて行うことができます。その後、オプションでそのデザインをプロビジョニング テンプレートとして永続化し、保存して再利用することも可能です。

使用しているサイト デザインをプロビジョニング テンプレートとして抽出する方法には、コア ライブラリ (OfficeDevPnP.Core) によって提供される拡張メソッドを実装する CSOM/REST コードを使用する方法と、Windows PowerShell スクリプト (OfficeDevPnP.PowerShell) を使用する方法の 2 つがあります。

### Windows PowerShell スクリプトを使用してプロビジョニング テンプレートを抽出する

プロビジョニング エンジンで Windows PowerShell スクリプトを使用するには、最初に、OfficeDevPnP.PowerShell コマンドをダウンロードしてインストールしなければなりません。Windows PowerShell を実行するために必要なものすべて (ダウンロードとインストールに関する手順、Windows PowerShell コマンドの資料を含む) は、GitHub の [OfficeDevPnP.PowerShell コマンド](https://github.com/OfficeDev/PnP-PowerShell) リポジトリにあります。

**メモ:** OfficeDevPnP.PowerShell コマンド リポジトリには、2 つのバージョンの Windows PowerShell コマンド MSI が含まれています。1 つはオンプレミス用、もう 1 つは SharePoint Online 用です。

OfficeDevPnP.PowerShell コマンド リポジトリに含まれているコンテンツを使用すると、SharePoint Online を対象とする Windows PowerShell コマンドのライブラリを構築できます。このコマンドは CSOM を使用し、インストールした MSI パッケージに応じて SharePoint Online とオンプレミス SharePoint の両方で機能します。

さらに、[Channel 9](https://channel9.msdn.com/) の短いビデオ &mdash;[PnP PowerShell コマンドレットの紹介](https://channel9.msdn.com/blogs/OfficeDevPnP/Introduction-to-PnP-PowerShell-Cmdlets)&mdash; もあります。このビデオでは、Windows PowerShell パッケージをインストールする方法と、Office 365 サイトに接続する方法を説明しています。 このビデオでは、インストールと接続の操作に加えて、リモート プロビジョニングに Windows PowerShell 使用することに関連した役立つ情報についても説明しています。

### CSOM コードを使用してプロビジョニング テンプレートを抽出する

プロビジョニング テンプレートを抽出するために CSOM/REST コードを採用する場合は、Visual Studio などの開発環境を使用して、開発プロジェクトを作成します。 任意の種類のプロジェクト&mdash;を作成してください。たとえば、コンソール アプリケーションや Windows アプリケーション、または SharePoint アドインのプロジェクトを作成します。 開発プロジェクトを作成したら、NuGet パッケージとして入手可能な Core ライブラリをインストールする必要があります。

**メモ:** コア ライブラリ NuGet パッケージを見つけてインストールする方法の手順、およびリモート プロビジョニング サンプル コンソール アプリケーションに関する段階的な説明については、「[コンソール アプリケーションのプロビジョニング サンプル](provisioning-console-application-sample.md)」に記されています。コア ライブラリには 2 つのバージョン、つまり SharePoint Online、SharePoint 2013、Office 365 を対象とするものと、オンプレミスの SharePoint 2013 を対象するバージョンがあることに注意してください。

CSOM の使用法についての詳細な手順は「 [コンソール アプリケーションのプロビジョニング サンプル](provisioning-console-application-sample.md)」に記載されていますが、概要は次のとおりです。

1. Office 365 に対する接続を作成します。
    
2. **ClientContext** インスタンスを作成し、 **Web** オブジェクトに対する参照を取得します。
    
3. コア ライブラリの拡張メソッド  **GetProvisioningTemplate** を使用して、 **ProvisioningTemplate** オブジェクトを抽出します。
    
4. テンプレート プロバイダーおよびシリアル化フォーマッタを使用して、任意のファイル場所にプロビジョニング テンプレート インスタンスを保存します。
    
    
                **メモ:**テンプレート プロバイダーとシリアル化フォーマッタ オブジェクトをカスタマイズできるため、必要な永続ストレージやシリアル化形式なら何でも実装できます。 標準の PnP のプロビジョニング エンジンは、ファイル システム、SharePoint、Azure Blob ストレージ テンプレート プロバイダーのサポートを提供します。 XML と JSON のシリアル化フォーマッタもサポートします。

「[PnP プロビジョニング スキーマ](pnp-provisioning-schema.md)」という記事では、XML シリアル化の出力例を確認できます。また、XML シリアル化スキーマについての説明もあります。 さらに、このスキーマとドキュメントを GitHub の「[OfficeDev/PnP-Provisioning-Schema](https://github.com/OfficeDev/Pnp-Provisioning-Schema/)」から入手することもできます。 また、このスキーマについて紹介して説明している [Channel 9](https://channel9.msdn.com/) のビデオ &mdash;[PnP プロビジョニング エンジン スキーマの詳細](https://channel9.msdn.com/blogs/OfficeDevPnP/Deep-dive-to-PnP-provisioning-engine-schema)&mdash; もあります。

ただし、プロビジョニング エンジンはシリアル化形式独立ドメイン モデルをサポートしているという点を忘れないでください。実際、プロビジョニング エンジンはシリアル化形式とはまったく切り離されています。  **ProvisioningTemplate** インスタンスを手動で定義するかどうかは独自で判断できます。モデル サイトをポイントするか、PnP プロビジョニング スキーマに対する検証を行う XML ドキュメントを構成するか, .NET コードを作成してオブジェクト階層を構築します。これらの方法を複数組み合わせて使用することも可能です。また、モデル サイトを使用してプロビジョニング テンプレートのデザインを行い、それを XML ファイルに保存し、コードで **ProvisioningTemplate** インスタンスを処理しながらメモリ内カスタマイズを実行することもできます。

#### GetProvisioningTemplate を使用してテンプレートを抽出するコード例

「 [コンソール アプリケーションのプロビジョニング サンプル](provisioning-console-application-sample.md)」という記事に記載されているサンプルは、 **GetProvisioningTemplate** メソッドの使用法を示しています。このサンプルでは、以下のコード ブロックで、プロビジョニング テンプレートのエクスポート プロセスが実行されます。

```
ProvisioningTemplateCreationInformation ptci = new ProvisioningTemplateCreationInformation(ctx.Web);

// Create FileSystemConnector, so that we can store composed files somewhere temporarily 
ptci.FileConnector = new FileSystemConnector(@"c:\temp\pnpprovisioningdemo", "");
ptci.PersistComposedLookFiles = true;
ptci.ProgressDelegate = delegate (String message, Int32 progress, Int32 total)
{

// Use this to simply output progress to the console application UI
Console.WriteLine("{0:00}/{1:00} - {2}", progress, total, message);
};

// Execute actual extraction of the tepmplate
ProvisioningTemplate template = ctx.Web.GetProvisioningTemplate(ptci);
```

### プロビジョニング テンプレートの適用

プロビジョニング テンプレートの抽出同様、カスタマイズ済み  **ProvisioningTemplate** オブジェクト インスタンスの適用は、CSOM/REST コードまたは Windows PowerShell コマンドのいずれかを使用して行えます。GitHub のリポジトリから [OfficeDevPnP.PowerShell コマンドレット](https://github.com/OfficeDev/PnP-PowerShell)をダウンロードできます。

コア ライブラリの拡張メソッドを使用してプロビジョニング テンプレートを適用する場合、この例と同様のコード ブロックを使用できます。この例では、 **Web** タイプの **ApplyProvisioningTemplate** 拡張メソッドを使用して対象サイトにテンプレートが適用されます。

```
using (var context = new ClientContext(destinationUrl))
{
  context.Credentials = new SharePointOnlineCredentials(userName, password);
  Web web = context.Web;
  context.Load(web, w => w.Title);
  context.ExecuteQueryRetry();

  // Configure the XML file system provider
  XMLTemplateProvider provider =
  new XMLFileSystemTemplateProvider(
    String.Format(@"{0}\..\..\",
    AppDomain.CurrentDomain.BaseDirectory),
    "");

  // Load the template from the XML stored copy
  ProvisioningTemplate template = provider.GetTemplate(
    "<template name>.xml");

  // Apply the template to another site
  Console.WriteLine("Start: {0:hh.mm.ss}", DateTime.Now);

  // We can also use Apply-SPOProvisioningTemplate
  web.ApplyProvisioningTemplate(template);

  Console.WriteLine("End: {0:hh.mm.ss}", DateTime.Now);
}
```

Windows PowerShell コマンドを使用して SharePoint Online に対する接続を作成した後、 **Apply-SPOProvisioningTemple** コマンドレットを次のように使用します。

```
Apply-SPOProvisioningTemplate -Path "PnP-Provisioning-File.xml"
```

ここで、 `-Path` 引数は、ファイル システムに永続化したプロビジョニング テンプレートのファイル パスを指します。前述のコード ブロックでは、永続化されたファイルは XML ファイルですが、これらのテンプレート ファイルは任意のシリアル化形式で永続化できます。

## PnP コア ライブラリ

コア ライブラリ (OfficeDevPnP.Core) は、PnP プロビジョニング フレームワークをサポートする CSOM/REST オブジェクト モデルで、PnP プロビジョニング エンジンを使用できるようにします。コア ライブラリはいくつかの名前空間で構成されていて、一連の [拡張メソッド](https://msdn.microsoft.com/en-us/library/bb383977.aspx)が含まれています。これらのメソッドは、SharePoint オブジェクト モデルを拡張し、リモート プロビジョニング、エンティティー、タイマー ジョブ、プロビジョニング テンプレート、プロビジョニング フレームワーク全体の処理用オブジェクトをサポートします。表 1 に、コア ライブラリの名前空間についてまとめます。

**表 1. OfficeDevPnP.Core ライブラリの名前空間**

|**名前空間**|**説明**|
|:-----|:-----|
|OfficeDevPnP.Core||
|OfficeDevPnP.Core.AppModelExtensions|追加メソッドを使用して既存タイプを拡張する .NET クラス。|
|OfficeDevPnP.Core.Entities|AppModelExtensions の拡張メソッドからより複雑なオブジェクトを提供および取得するクラス。|
|OfficeDevPnP.Core.Enums|プロビジョニング操作をサポートする列挙セット。|
|OfficeDevPnP.Core.Extensions|文字列操作などに役立つメソッドなど、SharePoint に関連しない拡張メソッド。|
|OfficeDevPnP.Core.Framework.ObjectHandlers.TokenDefinitions|テンプレート オブジェクト内の特定のコンテンツを置換するトークン。|
|OfficeDevPnP.Core.Framework.Provisioning.Connectors|テンプレートとアセットが格納されている各種データ ソースに接続するコネクター。|
|OfficeDevPnP.Core.Framework.Provisioning.Extensibility|拡張プロバイダー コード。拡張プロバイダーを使用すると、エンジンによって固有にサポートされていない機能を追加できます。|
|OfficeDevPnP.Core.Framework.Provisioning.Model|テンプレート データ モデル オブジェクト。テンプレートは、実際の C# コードに抽出されるか、逆シリアル化されます。この名前空間には、この構造のクラスが含まれます。|
|OfficeDevPnP.Core.Framework.Provisioning.ObjectHandlers|テンプレートの抽出と作成の両方をサポートする各種 SharePoint 要素のハンドラー。|
|OfficeDevPnP.Core.Framework.Provisioning.Providers|テンプレート プロバイダー ベースの名前空間。テンプレート プロバイダーを使用して、コード ベースのモデルを特定形式にシリアル化または逆シリアル化します。|
|OfficeDevPnP.Core.Framework.Provisioning.Providers.Json|コード ベースのテンプレートと JSON 形式との間でシリアル化または逆シリアル化を行うための JSON テンプレート プロバイダー。|
|OfficeDevPnP.Core.Framework.Provisioning.Providers.Xml|コード ベースのテンプレートと XML 形式との間でシリアル化または逆シリアル化を行うための XML テンプレート プロバイダー。|
|OfficeDevPnP.Core.Framework.Provisioning.Providers.Xml.V201503|v201503 バージョンのスキーマの自動生成スキーマ ファイル。|
|OfficeDevPnP.Core.Framework.Provisioning.Providers.Xml.V201505|v201505 バージョンのスキーマの自動生成スキーマ ファイル。|
|OfficeDevPnP.Core.Framework.Provisioning.Providers.Xml.V201508|v201508 バージョンのスキーマの自動生成スキーマ ファイル。|

## その他のリソース
<a name="bk_addresources"> </a>

- [コンソール アプリケーションのプロビジョニング サンプル](provisioning-console-application-sample.md)
    
- [OfficeDevPnP.PowerShell コマンド](https://github.com/OfficeDev/PnP-PowerShell)
    
- [コンソール アプリケーションのプロビジョニング サンプル](provisioning-console-application-sample.md)
    
- [OfficeDev/PnP-Provisioning-Schema](https://github.com/OfficeDev/Pnp-Provisioning-Schema/)
    
- [拡張メソッド](https://msdn.microsoft.com/en-us/library/bb383977.aspx)
