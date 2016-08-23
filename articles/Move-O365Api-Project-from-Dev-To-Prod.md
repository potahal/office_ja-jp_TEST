# 開発 Office 365 サイトを Microsoft Azure に展開する #

### 概要 ###

任意の種類の Web アプリケーションを開発するとき、ほとんどの開発は http://localhost を使用してローカルで行われます。一部のプロジェクトでは、ローカル リソースまたはローカル リソースとリモート リソースの組み合わせを使用します。ローカルの開発環境からこれらのプロジェクトを実行するには、データベース接続文字列、URL、構成などの変更のように、さまざまなタスクを実行する必要があります。

Office 365 の API  を活用する Web プロジェクトに違いはありません。これらのプロジェクトでは、マイクロソフトの Azure AD サービスを活用して、アプリケーションを認証し、OAuth 2.0 のアクセス トークンを取得します。これらのトークンは、Office 365 API を使用して認証するため、Web アプリケーションで使用されます。

このページでは、Office 365 API 開発プロジェクトを立ち上げて、[Office 365](http://products.office.com/en-us/business/explore-office-365-for-business)、[Azure Active Directory](http://azure.microsoft.com/en-us/services/active-directory/) および [Azure Websites](http://azure.microsoft.com/en-us/services/websites/) を使用して Microsoft Azure で完全にホストされる作業サンプルを開始する手順について説明しています。

Microsoft Azure をローカルの開発環境から Office 365 API Web アプリケーションに展開するには、このページで説明したように 3 つの大まかな手順を実行する必要があります。

- [Azure Web を作成して構成する](#create-and-configure-an-azure-website)
- [Azure AD アプリケーションを構成する](#configure-the-azure-ad-application)
- [ASP.NET プロジェクトを構成する](#configure-the-aspnet-project)
- [Office 365 API の ASP.NET Web アプリケーションを展開する](#deploy-the-office-365-api-aspnet-web-application)

> このページでは、Office 365 API を使用し、ローカルで動作する ASP.NET アプリケーションがあることを前提としています。リファレンスとして、**[O365-WebApp-SingleTenant](https://github.com/OfficeDev/O365-WebApp-SingleTenant)** プロジェクト (GitHub の **[OfficeDev](https://github.com/OfficeDev)** アカウント) を使用します。

# Azure Web を作成して構成する

この手順では、Web アプリケーションのホストに使用される Azure Web サイトを作成します。 

1. [Azure 管理ポータル](https://manage.windowsazure.com)に移動し、組織の ID アカウントを使用してログインします。
1. ログイン後、ナビゲーション サイド バーを使用して、**Web サイト** を選択します。
1. **Web サイト** ページで左下隅にある **[新規]** リンクをクリックします。
1. 表示されたウィザードで **[簡易作成]** を選択し、**URL** フィールドにサイト名を入力し、**[Web ホスティング計画]** と **[サブスクリプション]** を選択します。 

  ![簡易作成の設定:[URL] フィールドは "365api-01" に設定されています。[Web ホスティング プラン] は "Default1 (East US, Standard)" に設定されています。[サブスクリプション] は "Azure MSDN (primary)"に設定されています。](media/Move-O365Api-Project-from-Dev-To-Prod/Move-O365Api-Project-from-Dev-To-Prod-01.png)

  > 後で必要となるため、作成する Web サイトの名前を書き留めておいてください。

1. 最後に、**[Web サイトの作成]** リンクをクリックします。

Azure がサイトを作成するまで数分待機します。 サイトの作成後、Web インターフェイスから*アプリケーション設定*を指定できます。 これにより、シンプルな `web.config` の変更の場合、サイト コードベースを展開しなくても、Web サイトの Web 管理インターフェイスからプロジェクトの `web.config` ファイル内で `<appSettings>` を上書きできます。

1. **Azure 管理ポータル**で、今作成した Web サイトをクリックします。
1. 上部ナビゲーションの **[構成]** リンクをクリックします。
1. 下方向にスクロールし、**[アプリケーションの設定]** セクションで次の 3 つの新しいエントリを追加します。
  - **ida:ClientID**
  - **ida:Password** 
  - **ida:TenantID** 
1. 作業中のプロジェクトの `web.config` から対応する値をコピーし、次の図に示すように、Azure の Web サイトの該当する設定値に貼り付けます。

  ![[WEBSITE_NODE_DEFAULT_VERSION] は「0.10.32」、[ida:ClientID] は「92b1e137-c36f-4bfe-9e1c-01ef546ce4a9」、[ida:Password] は「Bns06N18ZiyYfMcyU9qUfGnZbnkBiPZfUptLDsU6cml」です。[ida:TenantId] は部分的に消されています。 GUID の中央部分の番号は "-45ee-8afc-" です。](media/Move-O365Api-Project-from-Dev-To-Prod/Move-O365Api-Project-from-Dev-To-Prod-02.png)

1. フッターで **[保存]** ボタンをクリックし、変更を保存します。

この時点で Azure Web サイトが設定され、後の手順で展開する Office 365 API の Web プロジェクトをホストするよう構成されました。

[先頭に戻る](#deploying-development-office-365-sites-to-microsoft-azure)

# Azure AD アプリケーションを構成する

この手順では、Office 365 アプリケーションの開発およびテスト環境で使用される Azure AD アプリケーションを変更します。

1. [Azure 管理ポータル](https://manage.windowsazure.com)に移動し、組織の ID アカウントを使用してログインします。
1. ログインした後、ナビゲーション サイド バーで  **[ACTIVE DIRECTORY]** を選択します。
1. **Active directory** ページで、Office 365 テナントにリンクされているディレクトリを選択します。
1. 次に、上部のナビゲーション内で **[アプリケーション]** アイテムをクリックします。
1. **[プロパティ]** セクション内で、作成した Azure Web サイトの既定 URL を示すよう **[サインオン URL]** を更新します。すべての Azure Web サイトで提供されている HTTPS エンドポイントを使用することに注意してをください。

  ![[名前] は "O365-WebApp-SingleTenant.Office365App" に設定されていて、[サインオン URL] は "https://o365api-01.azurewebsites.net" に設定されています。](media/Move-O365Api-Project-from-Dev-To-Prod/Move-O365Api-Project-from-Dev-To-Prod-03.png)

1. **[シングル サインオン]** セクションで、Azure Web サイトのドメインを使用するよう (次の図に示すように) **[アプリケーション ID の URI]** を更新します。
1. 次に、Azure Web サイトのホームページのみを示すように **[応答 URL]** を更新します。

  ![[アプリケーション ID の URI] は "https://o365api-01.azurewebsites.net/O365-WebApp-SingleTenant"、[応答 URL] は "https://o365api-01.azurewebsites.net/" です。](media/Move-O365Api-Project-from-Dev-To-Prod/Move-O365Api-Project-from-Dev-To-Prod-04.png)

1. フッターで **[保存]** ボタンをクリックし、変更を保存します。

この時点で、Office 365 API Web プロジェクトが使用する Azure AD アプリケーションは、新しい Azure Web サイトで動作するよう構成されます。

[先頭に戻る](#deploying-development-office-365-sites-to-microsoft-azure)

# ASP.NET プロジェクトを構成する

この手順では、新しい Azure Web サイトを使用するアプリケーションで ASP.NET プロジェクトを構成します。

本ガイダンスの例で使用される同じものと同じアプリケーションの場合、通常、追加作業は必要ありません。ただし、Web アプリケーションには、Azure AD アプリケーションと Azure AD テナントが開発時に使用した `web.config` ファイル内の設定が含まれています。一部の開発者は、開発インスタンスと本番インスタンスに異なる Azure AD アプリケーション、または異なる Azure サブスクリプションを使用するよう選択する場合があります。

このページに記載されている前の手順では、Azure Web サイトの作成時、`web.config`に一般的に含まれるアプリケーションのアドイン設定を行います。Web アプリケーションが、Azure Web サイトの構成からこれらの値を受け取ることを確認するには、`web.config` 内の値をプレースホルダーの値に置き換えることをお勧めします。

1. プロジェクトの `web.config` ファイルを開きます。
1. **ida:ClientID**、**ida:Password** および **ida:TenantId** のアドイン設定を探します。
1. これらの設定の値をプレースホルダーの値に置き換えます。

  ````xml
  <add key="ida:TenantId" value="set-in-azure-website-config" />
  <add key="ida:ClientID" value="set-in-azure-website-config" />
  <add key="ida:Password" value="set-in-azure-website-config" />
  ````

1. 変更を保存します。

この時点で Web アプリケーション、Azure Web サイトと Azure AD のアプリケーションはすべて正しく構成され、展開する準備ができました。

[先頭に戻る](#deploying-development-office-365-sites-to-microsoft-azure)

# Office 365 API の ASP.NET Web アプリケーションを展開する

この手順では、Office 365 API Web アプリケーションを Azure Web サイトに発行します。サイトが展開されたら、テストを行い、すべて正常に動作することを確認します。

> この手順では、Microsoft [Azure SDK](http://azure.microsoft.com/en-us/downloads/)、バージョン 2.0 以降がインストールされていることを前提にしています。 

## ASP.NET Web アプリケーションを展開する

1. Visual Studio 2013 で Office 365 API Web アプリケーションを開きます。
1. ソリューション エクスプ ローラーのツール ウィンドウ内でプロジェクトを右クリックし、**[発行]** を選択し、**Web の発行**ウィザードを開始します。
1. **[プロファイル]** タブで、**[Microsoft Azure Web サイト]** を選択します。

  この時点では、組織 ID を使用して Azure サブスクリプションにログインするよう求めるメッセージが表示されます。

1. ログイン後、このページの前の手順で作成した Web サイトを選択し、**[OK]** をクリックします。

  ![[既存の Web サイトの選択] ダイアログ ボックスは、[既存の Web サイト] を "o365api-01" に設定していることを示しています。](media/Move-O365Api-Project-from-Dev-To-Prod/Move-O365Api-Project-from-Dev-To-Prod-05.png)

1. **[接続]** タブで **[接続の検証]** ボタンをクリックして、接続プロファイルが正常にダウンロードされて適用されたことを確認します。

  ![ダイアログ ボックスの下のほうにある [接続の検証] ボタンを矢印が指しています。このボタンの横には緑色のチェック マークが付いています。](media/Move-O365Api-Project-from-Dev-To-Prod/Move-O365Api-Project-from-Dev-To-Prod-06.png)

1. **[発行]** ボタンをクリックし、Web アプリケーションを Azure Web サイトに発行します。

## ASP.NET Web アプリケーションのテスト

Azure Web サイトに Web アプリケーションを発行した後、Visual Studio でブラウザーが開き、サイトのホーム ページに移動します。 

既定では、これは HTTP エンドポイントです。以前の手順で Azure AD アプリケーションを構成したとき、HTTPS エンドポイントからのサインオンのみを承認するよう設定したことを思い出してください。アプリケーションを使用する前に、HTTPS エンドポイントを示すよう URL を更新します。

1. ブラウザーでは、Azure Web サイトの HTTPS ホームページに移動するよう URL を更新します。このページの例では、https://o365api-01.azurewebsites.net です。
1. ページ右上のヘッダーにある **[サインイン]** リンクをクリックします。これにより、Azure AD サインイン ページにリダイレクトされます。

  > この時点でエラーが発生した場合は、Azure Web サイト用に作成した 3 つのアドイン設定の問題では可能性があります。前に戻り、値がアプリケーションと Azure AD テナントとアプリケーションの正しい値であることを確認します。次のような URL が表示されます 

1. ログインに成功すると、作成した Azure Web サイトの Web アプリケーション ホーム ページにリダイレクトされます。

この時点で、Office 365 API Web アプリケーションを Azure Web サイトで実行するよう正常に展開しました。

[先頭に戻る](#deploying-development-office-365-sites-to-microsoft-azure)

----------

### 関連リンク ###
-  [O365-WebApp-SingleTenant](https://github.com/OfficeDev/O365-WebApp-SingleTenant)

### 適用対象 ###
-  Office 365 マルチテナント (MT)
-  Office 365 専用 (D)

### 作成者
Andrew Connell - [@andrewconnell](https://twitter.com/andrewconnell)

### バージョン履歴 ###
バージョン  | 日付 | コメント
---------| -----| --------
0.1  | 2015 年 1 月 2 日 | 原案


