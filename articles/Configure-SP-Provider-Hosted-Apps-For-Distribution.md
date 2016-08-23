# SharePoint プロバイダー ホスト型アドインを配布用に構成する

### 概要 ###

このページでは、SharePoint プロバイダー ホスト型アプリケーションを他の開発者と共有したり、Team Foundation Server、Git、Visual Studio Online などのソース管理システムからコピーを入手したりするときに発生する可能性のある問題について説明しています。

# SharePoint プロバイダー ホスト型アドインを配布用に構成する

Visual Studio 2013 を使用して作成されたすべての SharePoint プロバイダー ホスト型アドインには、SharePoint 特有のコードと SharePoint アドイン用の RemoteWeb として動作する Web アプリケーションへの参照を追加する NuGet パッケージが含まれています。 

Visual Studio の Office Developer Tools によって Web アプリケーション プロジェクトに追加された NuGet パッケージは NuGet パッケージ レジストリに存在しないため、NuGet パッケージの復元を実行しようとする試みは、一致するパッケージが見つからないために失敗します。

## 問題を理解する ##

**Office Developer Tools for Visual Studio 2013** バージョン 12.0.31105 では、SharePoint プロバイダー ホスト型アドイン用の RemoteWeb として作成された Web アプリケーションに、NuGet パッケージが追加されます。このパッケージ (**App for SharePoint Web Toolkit**) により、次のものが Web プロジェクトに追加されます。

- アセンブリと SharePoint クライアント側オブジェクト モデル (CSOM) アセンブリへの参照
- アドインの認証プロセスを支援するコード ファイル `TokenHelper.cs`。
- Web アプリケーション内で SharePoint コンテキストを作成し、メンテナンスを行うために役立つコード ファイル `SharePointContext.cs`。

Visual Studio はそれ自体またはアドインに NuGet パッケージが含まれる方法で動作するため、開発者は必ずしもインターネットに接続して NuGet パッケージをダウンロードする必要がありません。ツールによって含められるパッケージには、**AppForSharePoint16WebToolkit** という ID があります。

プロジェクトがソース管理にコミットされるとき、通常ではパッケージがコミットの一部として含められることはありません。他の開発者との共有時に余分の記憶領域が大量に必要になり、パッケージのサイズが不必要に大きくなる可能性があるためです。そのため、開発者がソース管理からプロジェクトのコピーを入手した後で最初に行うタスクの 1 つは、[NuGet パッケージの復元](http://docs.nuget.org/docs/reference/package-restore)の実行です。

問題になるのは、同じ ID のパッケージが NuGet パッケージ レジストリに存在しない点です (**AppForSharePoint16WebToolkit** という ID のパッケージはありません)。 代わりに、まったく同じパッケージが **AppForSharePointWebToolkit** ([http://www.nuget.org/packages/AppForSharePointWebToolkit](http://www.nuget.org/packages/AppForSharePointWebToolkit))** として NuGet パッケージ レジストリに追加されます (*ID に "16" が含まれないことに注意してください*)。

## SharePoint プロバイダー ホスト型アドイン プロジェクトのソース管理 / 配布用の準備 ##

Office Developer Tools for Visual Studio 2013 が更新されてこの問題が修正されるまでは、Team Foundation Server、Visual Studio Online、Git、その他のソリューションのいずれを使用するかにかかわらず、ソース管理システムにコミットする前にプロジェクトを変更することをお勧めします。

プロジェクトの作成後に、プロジェクトの `packages.config` ファイルを参照して、ID が **AppForSharePoint16WebToolkit** のパッケージを検索します。 プロジェクトを更新する最も安全な方法は、パッケージをアンインストールして、再度インストールすることです。

パッケージをアンインストールするには、Visual Studio で**パッケージ マネージャー コンソール**を開き、次のように入力します。

  ````powershell
  PM> Uninstall-Package -Id AppForSharePoint16WebToolkit
  ````

  > アンインストール時にパッケージが見つからないというエラー メッセージが返された場合でも、`packages.config` ファイルから手動でパッケージ参照を削除して、変更を保存するだけです。

それから、パブリック レジストリにある同じ NuGet パッケージの公開バージョンをインストールします。

  ````powershell
  PM> Install-Package -Id AppForSharePointWebToolkit
  ````

----------

### 関連リンク ###
- [NuGet: App for SharePoint Web Toolkit](http://www.nuget.org/packages/AppForSharePointWebToolkit)
- [NuGet: Package Restore (パッケージの復元)](http://docs.nuget.org/docs/reference/package-restore)

### 適用対象 ###
-  Office 365 マルチテナント (MT)
-  Office 365 専用 (D)
-  SharePoint 2013 オンプレミス

### 作成者
Andrew Connell - [@andrewconnell](https://twitter.com/andrewconnell)

### バージョン履歴 ###
バージョン  | 日付 | コメント
---------| -----| --------
0.1  | 2014年12月31日 | 原案
