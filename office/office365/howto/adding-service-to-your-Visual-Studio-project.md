---
ms.Toctitle: Add Office 365 services in Visual Studio
title: "Visual Studio を使用したアプリの登録と Office 365 API の追加"
description: "アプリを Microsoft Azure Active Directory に登録し、Office 365 API サービスを Visual Studio プロジェクトに追加して、アプリのアクセス許可を設定する方法について説明します。"
ms.ContentId: 5a2fbbba-dd0f-4cfe-b27a-dcd2ede82884
ms.date: July 20, 2015
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Visual Studio を使用したアプリの登録と Office 365 API の追加
   
_**適用対象:**Office 365_

    
[開発環境のセットアップ](..\howto\setup-development-environment.md)を完了していると、Office 365 API サービスを Visual Studio プロジェクトに追加できます。


**この記事の内容**

-  [Visual Studio を使用して Azure AD にアプリを登録するための前提条件](#bk_RegistrationPrereqs)
-  [Visual Studio サービス マネージャーを使用してアプリを登録し、Office 365 API を追加する](#addO365APIs)
-  [オプションの Office 365 API クライアント ライブラリ NuGet パッケージをプロジェクトに手動で追加する](#O365NuGets)
-  [次のステップ](#NextSteps)

<a name="bk_intro"> </a>
## Office 365 サービスをプロジェクトに追加する際に、Visual Studio を利用してアプリを登録する

Office 365 API サービスは、ユーザーの Office 365 データに対し、Azure AD を使用してセキュリティ保護された認証を行います。Office 365 API にアクセスするには、Azure AD にアプリを登録する必要があります。 

Visual Studio プロジェクトを作成する場合は、Office 365 サービスをプロジェクトに追加すると、アプリが自動的に登録されます。 

使用している Visual Studio のバージョンを選択してください。

[!INCLUDE [Add the VS versions filter bar](../includes/controls/vsversionsfilter.xml)]

Office 365 サービスをプロジェクトに追加するプロセスでは、Visual Studio を使用して次の操作を実行できます。

[!INCLUDE [BEGIN VS2013 section](../includes/controls/vs2013section.xml)]

-  アプリが Web アプリケーションまたはネイティブ アプリケーションのどちらであるかを指定して、アプリを Azure AD に登録する
-  アプリのプロパティ (アプリの名前、リダイレクト/応答エンドポイント、テナント範囲など) を構成する

[!INCLUDE [END VS2013 section](../includes/controls/vs2013section.xml)]

[!INCLUDE [BEGIN VS2015 section](../includes/controls/vs2015section.xml)]

-  Azure AD にアプリを登録する

[!INCLUDE [END VS2015 section](../includes/controls/vs2015section.xml)]

-  Office 365 サービスに接続する
-  Office 365 サービスでアプリに必要な API に対するアクセス許可レベルを指定する
-  アプリが接続する Office 365 サービスに応じて必要になる **NuGet パッケージ**をプロジェクトに追加する

次のプロジェクト テンプレートでは、接続済みサービスとしての Office 365 API の追加をサポートしています。

- .NET Windows ストア 8.1 アプリ
- .NET Windows ストア 8.1 ユニバーサル アプリ
- .NET Windows Phone 8.1 アプリ
- .NET Windows Phone 8.1 Silverlight アプリ
- Windows フォーム アプリケーション
- WPF アプリケーション
- ASP.NET MVC Web アプリケーション
- ASP.NET Web フォーム アプリケーション
- Xamarin Android および iOS アプリケーション
- マルチデバイス ハイブリッド (Cordova) アプリ

Office 365 API を使用する必要のある既存の Visual Studio プロジェクトがない場合、または Office 365 API を使用する完成版プロジェクトを試してみたい場合は、「[Office 365 API スタート プロジェクトおよびサンプル コード](..\howto\Starter-projects-and-code-samples.md)」ページに一覧表示されているプロジェクトのいずれかをダウンロードしてください。

<a name="bk_RegistrationPrereqs"> </a>
##Visual Studio を使用して Azure AD にアプリを登録するための前提条件

アプリを登録するための前提条件は、次のとおりです。

- [Office 365 開発者向けサイト](..\howto\setup-development-environment.md#bk_Office365Account)
- [Office 365 開発者向けサイトに関連付けられている](..\howto\setup-development-environment.md#bk_CreateAzureSubscription) Azure AD テナント
- [Visual Studio および Office Developer Tools for Visual Studio](..\howto\setup-development-environment.md#bk_VisualStudio) 


<a name="addO365APIs"> </a>
## Visual Studio サービス マネージャーを使用してアプリを登録し、Office 365 API をプロジェクトに追加する

Visual Studio の**サービス マネージャー**を使用して、Office 365 API を追加して構成します。

[!INCLUDE [BEGIN VS2013 section](../includes/controls/vs2013section.xml)]

1. **ソリューション エクスプローラー**で、Office 365 サービスを追加するプロジェクト ノードを選択します。
    
2. プロジェクト ノードを右クリックするか、マウスのボタンを押したままにして、**[追加]** > **[接続済みサービス]** を選択します。
    
3. アプリを登録します。
    
    **[サービス マネージャー]** ダイアログ ボックスの上部にある **[Office 365]** リンクをクリックして、**[アプリを登録する]** を選択します。 Office 365 開発者の組織のテナント管理者アカウントでサインインします。 

    これにより、Microsoft Azure Active Directory でアプリの登録が開始され、OAuth によるアプリの認証が可能になります。
    
    Office 365 にログオンすると、利用可能な Office 365 API サービスの一覧が表示されます。 Office 365 API の一覧も表示されます。

    各サービスの右側の **[アクセス許可]** 列が空であることがわかります。
    
4. 接続する Office 365 API を選択し、それぞれのアクセス許可レベルを指定します。
    
    一覧では、次の操作を実行します。
    1. 追加する Office 365 API を選択します
    1. **[アクセス許可]** を選択します。

        ![ユーザーとグループ サービス Office 365 API が選択され、アクセス許可リンクが強調表示されている [サービス マネージャー] ダイアログ ボックスを示すスクリーン ショット。](images\Office365_AddServiceToYourProject_ServiceManager1.png)

    **[Office 365 サービス] アクセス許可**ダイアログ ボックスで、次の操作を行います。

    1. プロジェクトに必要なアクセス許可を選択します
    2. **[適用]** を選択します。

        ![ユーザーの予定表の読み取りアクセス許可が選択された状態の [予定表のアクセス許可] ダイアログ ボックスを示すスクリーン ショット。](images\Office365_AddServiceToYourProject_Permissions.png)

    これにより、Visual Studio は、選択した API が含まれている Office 365 サービスを Azure AD のアプリに追加し、指定した API のアクセス許可レベルを設定します。
    
6. アプリのプロパティを設定します。

    **[サービス マネージャー]** ダイアログ ボックスで **[アプリのプロパティ]** を選択します。 

    設定できるアプリのプロパティは、アプリ プロジェクトが Web サービスまたは Web アプリケーションであるか、あるいはネイティブ アプリケーション (携帯電話プロジェクトなど) であるかによって異なります。

    たとえば、Web アプリケーションの場合、これをサンプル アプリとして開発者組織以外の Office 365 組織に使用可能にするには、以下のようにします。
    1. **[次に対してこのアプリケーションを使用可能にする]** を **[複数の組織]** に変更します。 
    2. **[適用]** を選択します。
    
    ![アプリを複数の組織に利用可能にするために選択する設定が表示されている、[Office 365 アプリのプロパティ] ダイアログ ボックスのスクリーン ショット。](images\Office365_AddServiceToYourProject_AppProperties.png)
    
    **[サービス マネージャー]** ダイアログ ボックスに、以下が表示されます。
    
    - プロジェクトに追加するために選択したサービス。
    - 各サービスのアクセス許可。 

        ![予定表サービスを構成した後の、予定表サービスの読み取りアクセス許可が設定された [サービス マネージャー] ダイアログ ボックスのスクリーン ショット。](images\Office365_AddServiceToYourProject_ServiceManager2.png)
    
6. **[OK]** をクリックします。
    
    
この時点で、Visual Studio は必要な **NuGet パッケージ**をプロジェクトに追加します。追加される NuGet パッケージは、追加した Office 365 API に応じて異なります。
    
[!INCLUDE [END VS2013 section](../includes/controls/vs2013section.xml)]

[!INCLUDE [BEGIN VS2015 section](../includes/controls/vs2015section.xml)]

1. **ソリューション エクスプローラー**で、Office 365 サービスを追加するプロジェクト ノードを選択します。
    
2. プロジェクト ノードを右クリックするか、マウスのボタンを押したままにして、**[追加]** > **[接続済みサービス]** を選択します。

3. **[Microsoft]** タブで、**[Office 365 API]** を選択して、**[構成]** を選択します。

    **[Office 365 API サービスの構成]** ダイアログ ボックスが表示されます。
    
3. アプリを登録します。
    
    **[ドメインの選択]** ページで、アプリを登録する Office 365 ドメインを入力するか選択します。例: <i>contoso.onmicrosoft.com</i>。 **[次へ]** を選択します。 現在まだサインインしていない場合は、Office 365 へのサインインが必要になる場合があります。 
    
    新しい Azure AD アプリケーションを作成する、または既存のものを使用するには、**[アプリケーションの構成]** ページで選択します。 既存の Azure AD アプリケーションを使用する場合は、そのアプリケーションのクライアント ID を入力します。
    
    アプリケーションでシングル サインオンを使用する場合は、チェック ボックスをオンにします。 シングル サインオンには SSL が必要なため、このオプションを選択すると、自動的にプロジェクトの SSL が有効になります。 
    
    **[次へ]** を選択します。 

    Visual Studio がアプリを登録します。
    
4. 接続する Office 365 API を選択し、それぞれのアクセス許可レベルを指定します。
    
    1. Office 365 API を選択します (連絡先やマイ ファイルなど)。
    
    2. アプリに必要なアクセス許可を選択します。
        
        アクセス許可は範囲内で追加可能であることにご注意ください。 たとえば、メール API の場合は、**メールの読み取りと書き込み**アクセス許可には、**メールの読み取り**アクセス許可が含まれるので、両方を選択する必要はありません。 通常、アプリがすべての必要な処理を達成できる範囲で最小の拡張アクセス許可を選択します。
        
    2. **[次へ]** を選択します。
    
    これにより、Visual Studio は、選択した API が含まれている Office 365 サービスを Azure AD のアプリに追加し、指定した API のアクセス許可レベルを設定します。
    
6. アプリに必要な API をすべて追加したら、**[完了]** を選択します。
    
この時点で、Visual Studio は必要な **NuGet パッケージ**をプロジェクトに追加します。追加される NuGet パッケージは、追加した Office 365 API に応じて異なります。
    

[!INCLUDE [END VS2015 section](../includes/controls/vs2015section.xml)]



<a name="O365NuGets"> </a>
## オプションの Office 365 API クライアント ライブラリ NuGet パッケージをプロジェクトに手動で追加する

Office 365 サービスを追加すると、Office 365 ツールによって必要な NuGet パッケージが自動的に追加されますが、[[NuGet パッケージの管理] ダイアログ ボックス](http://docs.nuget.org/consume/package-manager-dialog)または [NuGet パッケージ マネージャ コンソールと PowerShell](http://docs.nuget.org/consume/package-manager-console) を使用して、NuGet パッケージを手動で追加および管理することもできます。 

次の表に、追加する Office 365 API サービスと、作成したプロジェクトの種類に応じて必要になる NuGet パッケージを記載します。


|**Office 365 サービス**|.NET Windows ストア アプリ (Windows ストアおよび Windows Phone)|Windows デスクトップ アプリ (WPF および Windows フォーム)|ASP.NET Web アプリケーション (MVC および Web フォーム)|Xamarin アプリケーション (iOS および Android)|Cordova アプリケーション|
|:-----|:-----|:-----|:-----|:-----|:-----|
|ユーザーとグラフ|[Microsoft.Azure.ActiveDirectory.GraphClient](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)|[Microsoft.Azure.ActiveDirectory.GraphClient](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)|[Microsoft.Azure.ActiveDirectory.GraphClient](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)|[Microsoft.Azure.ActiveDirectory.GraphClient](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)|[Microsoft.Azure.ActiveDirectory.GraphClient.JS](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient.JS/)|
|Outlook サービス|[Microsoft.Office365.OutlookServices](http://www.nuget.org/packages/Microsoft.Office365.OutlookServices/)|[Microsoft.Office365.OutlookServices](http://www.nuget.org/packages/Microsoft.Office365.OutlookServices/)|[Microsoft.Office365.OutlookServices](http://www.nuget.org/packages/Microsoft.Office365.OutlookServices/)|[Microsoft.Office365.OutlookServices](http://www.nuget.org/packages/Microsoft.Office365.OutlookServices/)| |
|SharePoint サービス|[Microsoft.Office365.SharePoint](http://www.nuget.org/packages/Microsoft.Office365.SharePoint/)|[Microsoft.Office365.SharePoint](http://www.nuget.org/packages/Microsoft.Office365.SharePoint/)|[Microsoft.Office365.SharePoint](http://www.nuget.org/packages/Microsoft.Office365.SharePoint/)|[Microsoft.Office365.SharePoint](http://www.nuget.org/packages/Microsoft.Office365.SharePoint/)| |
|探索クライアント|[Microsoft.Office365.Discovery](http://www.nuget.org/packages/Microsoft.Office365.Discovery/)|[Microsoft.Office365.Discovery](http://www.nuget.org/packages/Microsoft.Office365.Discovery/)|[Microsoft.Office365.Discovery](http://www.nuget.org/packages/Microsoft.Office365.Discovery/)|[Microsoft.Office365.Discovery](http://www.nuget.org/packages/Microsoft.Office365.Discovery/)||
|すべてのサービス| | | |[Microsoft.Office365.OAuth.Xamarin](http://www.nuget.org/packages/Microsoft.Office365.OAuth.Xamarin)|[Microsoft.Office365.ClientLib.JS](http://www.nuget.org/packages/Microsoft.Office365.ClientLib.JS/)|



プロジェクトの種類によっては、どの Office 365 サービスを追加する場合にも特定の NuGet パッケージが必要になります。

- Cordova プロジェクトには、常に [Microsoft.Office365.ClientLib.JS](http://www.nuget.org/packages/Microsoft.Office365.ClientLib.JS/) NuGet パッケージが必要です。 

    このパッケージには、Cordova プロジェクトに Outlook サービスまたは SharePoint サービスを追加するために必要なファイルが含まれています。

- Xamarin プロジェクトには、常に [Microsoft.Office365.OAuth.Xamarin](http://www.nuget.org/packages/Microsoft.Office365.OAuth.Xamarin) NuGet パッケージが必要です。 

<!--
- For MVC, WPF and WinForms, Windows Store and Phone projects, the [Microsoft.Office365.Discovery](http://www.nuget.org/packages/Microsoft.Office365.Discovery/) is always required.
- -->



Xamarin Studio は、NuGet パッケージを管理するためのサポートを提供するようになりました。

<a name="NextSteps"> </a>
## 次のステップ

O365 サービスをアプリに追加した後には、ユーザーのデータにアクセスできるようにするために、Office 365 でアプリを認証する必要があります。詳細については、「[.NET Visual Studio プロジェクトへの Office 365 API の統合](..\howto\Authenticate-and-use-services.md)」を参照してください。 


[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]


