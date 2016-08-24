---
ms.Toctitle: Add Office 365 services in Visual Studio
title: "Visual Studio を使用したアプリの登録と Office 365 API の追加"
description: "ms.TocTitle:Visual Studio で Office 365 サービスを追加する Title:Visual Studio を使用したアプリの登録と Office 365 API の追加 Description:アプリを Microsoft Azure Active Directory に登録し、Office 365 API サービスを Visual Studio プロジェクトに追加して、アプリのアクセス許可を設定する方法について説明します。ms.ContentId:5a2fbbba-dd0f-4cfe-b27a-dcd2ede82884ms.topic: ms.ContentId: 記事 (方法) ms.date:2015 年 7 月 20 日"
ms.ContentId: 5a2fbbba-dd0f-4cfe-b27a-dcd2ede82884
ms.date: July 20, 2015
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Visual Studio を使用したアプリの登録と Office 365 API の追加
   
_**適用対象:** Office 365_

    
[開発環境のセットアップ](..\howto\setup-development-environment.md)を完了していると、Office 365 API サービスを Visual Studio プロジェクトに追加できます。


**この記事の内容**

-  [Visual Studio を使用して Azure AD にアプリを登録するための前提条件](#bk_RegistrationPrereqs)
-  [Visual Studio サービス マネージャを使用してアプリを登録し、Office 365 API を追加する](#addO365APIs)
-  [オプションの Office 365 API クライアント ライブラリ NuGet パッケージをプロジェクトに手動で追加する](#O365NuGets)
-  [次の手順](#NextSteps)

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
## Visual Studio サービス マネージャを使用してアプリを登録し、Office 365 API をプロジェクトに追加する

Visual Studio の**サービス マネージャ**を使用して、Office 365 API を追加して構成します。

[!INCLUDE [BEGIN VS2013 section](../includes/controls/vs2013section.xml)]

1. **ソリューション エクスプローラー**で、Office 365 サービスを追加するプロジェクト ノードを選択します。
    
2. プロジェクト ノードを右クリックするか、マウスのボタンを押したままにして、[追加] > [接続済みサービス] を選択します。
    
3. アプリを登録します。
    
    At the top of the  **Services Manager** dialog box, choose the **Office 365** link, and then choose **Register your app**. Office 365 開発者の組織のテナント管理者アカウントでサインインします。 

    This starts the app registration in Microsoft Azure Active Directory, which allows your app to authenticate via OAuth.
    
    After you've logged on to Office 365, a list of available Office 365 APIs services appears. You will see a list of Office 365 APIs.

    You will see that the **Permissions** column to the right of each service is empty.
    
4. 接続する Office 365 API を選択し、それぞれのアクセス許可レベルを指定します。
    
    In the list:
    1. 追加する Office 365 API を選択します
    1. **[アクセス許可]** を選択します。

        ![A screenshot that shows the Services Manager dialog box with the Calendar service Office 365 API selected and the Permissions link highlighted.](images\Office365_AddServiceToYourProject_ServiceManager1.png)

    In the  **[Office 365 service] Permissions** dialog box:

    1. プロジェクトに必要なアクセス許可を選択します
    2. **[適用]** を選択します。

        ![閲覧ユーザーの予定表許可が選択された [Calendar Permissions] ダイアログを示すスクリーン ショット。](images\Office365_AddServiceToYourProject_Permissions.png)

    When you do this, Visual Studio adds the Office 365 service(s) that contain the APIs you selected to your app in Azure AD, and sets the permission levels for the APIs to those you specified.
    
6. アプリのプロパティを設定します。

    Choose  **App Properties** in the **Services Manager** dialog box. 

    The app properties you can set differ depending on whether your app project is a web service or web application, or a native application, such as a mobile phone project.

    For example, for web applications, to make this sample app available to Office 365 organizations other than your developer organization:
    1. **[次に対してこのアプリケーションを使用可能にする]** の設定を **[複数の組織]** に変更します。 
    2. **[適用]** を選択します。
    
    ![A screenshot of the Office 365 App Properties dialog box showing the setting to select to make your app available to multiple organizations.](images\Office365_AddServiceToYourProject_AppProperties.png)
    
    The  **Services Manager** dialog box now lists:
    
    - プロジェクトに追加するために選択したサービス
    - 各サービスのアクセス許可。 

        ![A screenshot of the Services Manager dialog box after the Calendar service is configured, showing that the Calendar service has Read permissions.](images\Office365_AddServiceToYourProject_ServiceManager2.png)
    
6. **[OK]** をクリックします。
    
    
この時点で、Visual Studio は必要な **NuGet パッケージ**をプロジェクトに追加します。追加される NuGet パッケージは、追加した Office 365 API に応じて異なります。
    
[!INCLUDE [END VS2013 section](../includes/controls/vs2013section.xml)]

[!INCLUDE [BEGIN VS2015 section](../includes/controls/vs2015section.xml)]

1. **ソリューション エクスプローラー**で、Office 365 サービスを追加するプロジェクト ノードを選択します。
    
2. プロジェクト ノードを右クリックするか、マウスのボタンを押したままにして、[追加] > [接続済みサービス] を選択します。

3. **[Microsoft]** タブで、**[Office 365 API]** を選択し、**[構成]** を選択します。

    The **Configure Office 365 API Services** dialog box appears.
    
3. アプリを登録します。
    
    On the **Select Domain** page, enter or select the Office 365 domain in which you want your app to be registered; for example, <i>contoso.onmicrosoft.com</i>. **[次へ]** を選択します。 You may be required to sign into Office 365, if you are not already currently signed in. 
    
    On the **Configure Application** page, choose to create a new Azure AD application, or use an existing one. If you choose to use an existing Azure AD application, enter that application's client ID.
    
    If you want to use Single Sign-On with your application, check the box. Since Single Sign-On requires SSL, selecting this option automatically enables SSL in your project. 
    
    **[次へ]** を選択します。 

    Visual Studio registers your app.
    
4. 接続する Office 365 API を選択し、それぞれのアクセス許可レベルを指定します。
    
    1. Office 365 API を選択します (連絡先やマイ ファイルなど)。
    
    2. アプリに必要なアクセス許可を選択します。
        
        Remeber that permissions are additive in scope. For example, for the Mail API, the **Read and write to your mail** permission includes the **Read your mail** permission, so you don't need to select both. In general, select the least expansive permission that still enables your app to accomplish all it needs to do.
        
    2. **[次へ]** を選択します。
    
    When you do this, Visual Studio adds the Office 365 service(s) that contain the APIs you selected to your app in Azure AD, and sets the permission levels for the APIs to those you specified.
    
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

    This package includes the necessary files for adding Outlook Services or SharePoint Services to your Cordova project.

- Xamarin プロジェクトには、常に [Microsoft.Office365.OAuth.Xamarin](http://www.nuget.org/packages/Microsoft.Office365.OAuth.Xamarin) NuGet パッケージが必要です。 

<!--
- For MVC, WPF and WinForms, Windows Store and Phone projects, the [Microsoft.Office365.Discovery](http://www.nuget.org/packages/Microsoft.Office365.Discovery/) is always required.
- -->



Xamarin Studio は、NuGet パッケージを管理するためのサポートを提供するようになりました。

<a name="NextSteps"> </a>
## 次のステップ

O365 サービスをアプリに追加した後には、ユーザーのデータにアクセスできるようにするために、Office 365 でアプリを認証する必要があります。詳細については、「[.NET Visual Studio プロジェクトへの Office 365 API の統合](..\howto\Authenticate-and-use-services.md)」を参照してください。 


[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]


