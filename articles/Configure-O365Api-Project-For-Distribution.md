# 配布用の Office 365 API プロジェクトを構成する

### 概要 ###
このページでは、開発者が Office 365 API を利用するプロジェクトを他の開発者、顧客、または Team Foundation Server、Git、Visual Studio Online などのソース管理システムに配布する前に実施を検討する必要のあるいくつかの手順を説明します。

具体的には、このページでは 2 つの手順に注目します。

- [Azure AD グラフ クライアント NuGet パッケージ参照を修正する](#fixup-azure-ad-graph-client-nuget-package-reference)
- [`web.config` からアプリ別の詳細を消去する](#cleaning-the-webconfig-for-app-specific-details)

# Azure AD グラフ クライアント NuGet パッケージ参照を修正する

接続済みサービスの追加によって Office 365 API SDK を利用するすべてのプロジェクトには、Visual Studio で作成されたプロジェクトに Office 365 と Azure AD の参照を追加する NuGet パッケージが含まれています。 

Visual Studio の **Office 365 API ツール**によってプロジェクトに追加された NuGet パッケージは NuGet パッケージ レジストリに存在しないため、NuGet パッケージの復元を実行しようとする試みは、一致するパッケージが見つからないために失敗します。

## 問題を理解する ##

**Office 365 API Tools for Visual Studio 2013** バージョン 1.3.41104.1 では、[接続済みサービスの追加] ウィザードの一環として複数の NuGet パッケージがプロジェクトに追加されます。特に課題となる 1 つのパッケージは、**Microsoft Azure Active Directory Graph Client Library** です。

Visual Studio はそれ自体またはアドインに NuGet パッケージが含まれる方法で動作するため、開発者は必ずしもインターネットに接続して NuGet パッケージをダウンロードする必要がありません。 ツールに含まれるパッケージの ID は **Microsoft.Azure.ActiveDirectory.GraphClient**、バージョンは **1.0.22** です。

プロジェクトがソース管理にコミットされるとき、通常ではパッケージがコミットの一部として含められることはありません。他の開発者との共有時に余分の記憶領域が大量に必要になり、パッケージのサイズが不必要に大きくなる可能性があるためです。そのため、開発者がソース管理からプロジェクトのコピーを入手した後で最初に行うタスクの 1 つは、[NuGet パッケージの復元](http://docs.nuget.org/docs/reference/package-restore)の実行です。

問題になるのは、同じ ID とバージョンのパッケージが NuGet パッケージ レジストリに存在しない点です (ID が **Microsoft.Azure.ActiveDirectory.GraphClient**、バージョンが **1.0.22** のパッケージはありません)。 パッケージは NuGet パッケージ レジストリ **[Microsoft.Azure.ActiveDirectory.GraphClient](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)** に存在しますが、バージョンが異なります。

## Azure AD グラフ クライアント NuGet パッケージ参照の修正 ##

Office 365 API Tools for Visual Studio 2013 が更新されてこの問題が修正されるまでは、Team Foundation Server、Visual Studio Online、Git、その他のソリューションのいずれを使用するかにかかわらず、ソース管理システムにコミットする前にプロジェクトを変更することをお勧めします。

プロジェクトの作成後に、プロジェクトの `packages.config` ファイルを参照して、ID が**Microsoft.Azure.ActiveDirectory.GraphClient**、バージョンが **1.0.22** のパッケージを検索します。 プロジェクトを更新する最も安全な方法は、パッケージをアンインストールして、再度インストールすることです。

パッケージをアンインストールするには、Visual Studio で**パッケージ マネージャー コンソール**を開き、次のように入力します。

  ````powershell
  PM> Uninstall-Package -Id Microsoft.Azure.ActiveDirectory.GraphClient
  ````

  > アンインストール時にパッケージが見つからないというエラー メッセージが返された場合でも、`packages.config` ファイルから手動でパッケージ参照を削除して、変更を保存するだけです。

それから、パブリック レジストリにある同じ NuGet パッケージの公開バージョンをインストールします。

  ````powershell
  PM> Install-Package -Id Microsoft.Azure.ActiveDirectory.GraphClient -Version 2.0.2
  ````

  > 上の例では、Office 365 API で機能することがわかっている Azure AD グラフ クライアントの特定のバージョンを参照しています。将来のバージョンは機能する可能性があるため、`-Version` 引数の省略は任意です。

[先頭に戻る](#configure-office-365-api-projects-for-distribution)

# `web.config` からアプリ別の詳細を消去する

**Office 365 API Tools** for Visual Studio を使用すると、Visual Studio の [**接続済みサービス**] ウィザードを使用して Office 365 API 用の必要なアクセス許可を持つ新しい Azure AD アプリケーションを作成する機能が追加されます。このウィザードを完了する時点で、プロジェクトの `web.config` ファイル内に複数のエントリとカスタマイズが作成されます。

これらの変更には、次のアドイン設定が含まれます。

- **ida:ClientID**: Azure AD テナントで作成されたアプリケーションの一意の ID です。
- **ida:Password**: [接続済みサービス] ウィザードで生成された Azure AD アプリケーションのキーです。
- **ida:AuthorizationUri**: Azure AD による認証に使用されるエンドポイントです。

**ida:ClientID** と **ida:Password** は、Azure AD アプリに特有のものです。一部の開発チームは、独自のアプリに対してコーディングを行う開発者を選ぶことがあります。これは、独自のローカル開発データベースに対して開発者が作業する方法を選ぶのと似ています。そのため、**ida:ClientID** と **ida:Password** は、データベース接続文字列のようなものとみなすことができます。 

開発者が次に [接続済みサービス] ウィザードを使用してプロジェクトの新しい Azure AD アプリケーションを作成するとき、ウィザードは **ida: CliendID** を検出し、現在のユーザーの Azure AD テナント内のアプリケーションに接続しようとします。一致するものが見つからないと、[接続済みサービス] ウィザードはエラーを返します。

そのため、プロジェクトをソース管理にコミットしたり他の開発者と共有したりする前に、`web.config` 内の **ida:ClientID** & **ida:Password** アドイン設定から値を削除することをお勧めします。

[先頭に戻る](#configure-office-365-api-projects-for-distribution)

----------

### 関連リンク ###
- [NuGet: App for SharePoint Web Toolkit](http://www.nuget.org/packages/AppForSharePointWebToolkit)
- [NuGet: Package Restore (パッケージの復元)](http://docs.nuget.org/docs/reference/package-restore)
- [MSDN: Office 365 API クライアント ライブラリ NuGet パッケージ](http://msdn.microsoft.com/office/office365/HowTo/adding-service-to-your-Visual-Studio-project#O365NuGets)

### 適用対象 ###
-  Office 365 マルチテナント (MT)
-  Office 365 専用 (D)

### 作成者
Andrew Connell - [@andrewconnell](https://twitter.com/andrewconnell)

### バージョン履歴 ###
バージョン  | 日付 | コメント
---------| -----| --------
0.1  | 2014年12月31日 | 原案


