---
ms.Toctitle: Set up your dev environment
title: "Office 365 の開発環境のセットアップ"
description: "ms.TocTitle: 開発環境のセットアップTitle: Office 365 開発環境のセットアップDescription: アプリを登録して、Office 365 API を使って開発するためのツールを取得できるように、Azure テナントに接続する Office 365 Developer サイトを確認または作成します。ms.ContentId: c9fe7f08-bd9d-479c-9fa6-84f17d6a9826 ms.topic: 記事 (方法)"
ms.ContentId: c9fe7f08-bd9d-479c-9fa6-84f17d6a9826
ms.date: July 20, 2015
---

<script src="https://cdn.optimizely.com/js/3097170014.js"></script>

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



[!INCLUDE [Add the platforms filter bar](../includes/controls/addplatformsfilter.xml)]

# Office 365 開発環境のセットアップ

_**適用対象:** Office 365_

[!INCLUDE [Use Microsoft Graph](../includes/use-msgraph-note.txt)]

Office 365 API にアクセスするアプリケーションを作成する前に、開発者環境を設定する必要があります。これは、ツールと環境が正常であることを確認する 3 つの 1 回限りのタスクから構成されます。


1. アプリの作成に使用する[開発者用ツールをダウンロードします](#bk_VisualStudio)。
3. Office 365 API にアクセスするために、[ビジネス向け Office 365 のアカウントを取得します](#bk_Office365Account)。
2. アプリを作成して管理できるように、[Office 365 アカウントを Azure AD と関連付けます](#bk_CreateAzureSubscription)。


<a name="bk_VisualStudio"> </a>


## Angular 開発者用ツールをインストールしてアプリを作成する
[!INCLUDE [BEGIN iOS section](../includes/controls/iossection.xml)]

**Not developing iOS apps?** JavaScript Web アプリを開発するのではない場合 右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。


You use [Xcode](https://developer.apple.com/xcode/downloads/) to develop iOS apps. Xcode を使用して、iOS アプリを開発します。詳細については、「[Start Developing iOS Apps Today](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/RoadMapiOS/)」の **Get the tools** を参照してください。

[iOS 用の Office 365 SDK](https://github.com/OfficeDev/Office-365-SDK-for-iOS) を使用して、Office 365 API にアクセスする iOS アプリを開発します。より簡単に SDK をアプリに統合するには、[CocoaPods](http://cocoapods.org/) 依存関係マネージャーを使用できます。CocoaPods は Ruby に基づいており、OS X にインストールされている既定の Ruby で使用できます。CocoaPods を初めて使用する前に、Xcode プロジェクトで使用できるように、ターミナルから次の 2 つのコマンドを実行し、CocoaPods の環境を設定する必要があります。

```
sudo gem install cocoapods
pod setup
```

インストールとセットアップが成功すると、ターミナルに「**セットアップが完了しました**」というメッセージが表示されます。

**注**: これらのコマンドは 1 度だけ実行する必要があります。
  

[!INCLUDE [END iOS section](../includes/controls/iossection.xml)]

[!INCLUDE [BEGIN Android section](../includes/controls/androidsection.xml)]

**Not developing Android apps?** JavaScript Web アプリを開発するのではない場合 右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。

Android の開発には、[Android Studio](#bk_useAndroidStudio) または [Eclipse などの Android 開発者用ツール](#bk_useEclipse)の 2 つの選択肢があります。2 つは類似していますが、Android Studio ではライブラリの依存関係を管理するために Gradle サポートを利用できる一方、Eclipse ではそれを自分で管理する必要があります。

<a name="bk_useAndroidStudio" ></a>

* **Android Studio** Android 開発者向けサイトの手順に従って、[Android Studio をダウンロードしてインストールします](https://developer.android.com/sdk/index.html)。[Android Studio の依存関係の追加](#bk_dependenciesForStudio)の手順に従って、ライブラリをプロジェクトに追加します。 

<a name="bk_useEclipse"></a>

* **Android 開発者用ツールと Eclipse** Android 開発者向けサイトの手順に従って、[Eclipse などの Android 開発者用ツール](http://developer.android.com/tools/help/adt.html)をダウンロードしてインストールします。[Eclipse の依存関係の追加](#bk_dependenciesForEclipse)の手順に従って、ライブラリをプロジェクトに追加します。


[!INCLUDE [END Android section](../includes/controls/androidsection.xml)]

[!INCLUDE [BEGIN JavaScript section](../includes/controls/javascriptsection.xml)]

**Not developing Angular apps?** JavaScript Web アプリを開発するのではない場合 右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。


さまざまな IDE で Angular アプリケーションを記述できます。Office 365 API を使った開発に特に優位性のある IDE は提供されていないため、お好みの環境を選んで開始してください。[Brackets](http://brackets.io/)、[Sublime Text](http://www.sublimetext.com/)、[WebStorm](https://www.jetbrains.com/webstorm/)、および [Visual Studio](http://www.visualstudio.com/downloads) がよく使われています。

Office 365 API を使用して Angular アプリケーションを開発する場合、他のツールは必要ありません。ただし、次の [Office 365 API で Angular アプリを作成する](.\getting-started-Office-365-APIs.md)のチュートリアルに従う場合は、[Git](http://git-scm.com/) および [Node.js](http://nodejs.org/) などのサンプルをダウンロードして構成するために、追加のユーティリティをいくつかインストールする必要があります。

[!INCLUDE [END JavaScript section](../includes/controls/javascriptsection.xml)]

[!INCLUDE [BEGIN ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]


**Not developing apps with Visual Studio?** JavaScript Web アプリを開発するのではない場合 右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。
 
Visual Studio の Office 開発者用ツールには、クライアント ライブラリと Visual Studio の UI 拡張機能が含まれます。クライアント ライブラリを使用すると、.NET Framework と JavaScript で使用可能なライブラリを使用して、任意のデバイスまたはプラットフォームから Office 365 REST API と容易に対話できます。Visual Studio UI 拡張機能により、Office 365 サービスをアプリケーション プロジェクトに簡単に追加できます。

[Office Developer Tools for Visual Studio](http://aka.ms/officedevtoolsatvscom) をインストールします。 

####Visual Studio を持っていない場合

Visual Studio がインストールされていない場合は、次の操作を実行できます。


- [Visual Studio 2015 Community または Enterprise Edition](http://go.microsoft.com/fwlink/?LinkId=517106) をインストールします。 
    
    Visual Studio Community is available if you're an individual developer, or part of a organization using Visual Studio in a classroom learning environment, for academic research, or for contributing to open source projects. Developers at non-enterprise organizations may qualify as well. For more information, please refer to the [Visual Studio Community License Terms](https://www.visualstudio.com/support/legal/dn877550) and the [Visual Studio Licensing Whitepaper](https://www.microsoft.com/en-us/download/details.aspx?id=13350).

- [MSDN サブスクリプション](http://msdn.microsoft.com/en-us/subscriptions/downloads/)を介して Visual Studio をインストールします。
    

<!--
####To install the Office Developer Tools for Visual Studio 2013

- Download and install the [Office Developer Tools for Visual Studio 2013](http://aka.ms/OfficeDevToolsForVS2013).

####If you have Visual Studio 2012

If you have Visual Studio 2012 installed, you can download and install the [Office Developer Tools for Visual Studio 2012](http://aka.ms/OfficeDevToolsForVS2012).

-->    
 

[!INCLUDE [END ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]


<a name="bk_Office365Account"> </a>
## Office 365 API にアクセスするために Office 365 アカウントを取得する

メール、連絡先、カレンダー、およびファイルなどの Office 365 API にアクセスするには、グローバル管理者権限を持つ Office 365 ビジネス アカウントを有している必要があります。 

<!--
Tech reviewers: Is this correct, that you currently need global admin privileges to a) associate your O365 account with Azure AD, and b) manage apps through the Azure managment portal? I've gotten conflicting answers from various product team members. I know this'll change once the new MSA-enabled registration portal goes live, but is this accurate currently? Thanks!
-->

次のプランのいずれかになります。

- Office 365 Midsize Business
- Office 365 Enterprise E1、E3、E4、または K1
- Office 365 Education
- Office 365 Developer 

<!--
- Office 365 Government G1, G3, G4, or K1
-->

### ビジネス向けの Office 365 アカウントがすでにある場合

すでに上記のいずれかの Office 365 アカウントがある場合は、準備が整っています。次の手順では、Office 365 API を使用するアプリを作成および管理できるように、[Office 365 アカウント と Azure AD サブスクリプションを関連付けます](#bk_CreateAzureSubscription)。


### 現在、ビジネス向けの Office 365 アカウントがない場合

既存のビジネス向けの Office 365 アカウントがない場合は、次の操作を実行できます。

- 上記のいずれかの[ビジネス向けの Office 365 プラン](http://products.office.com/en-us/business/compare-office-365-for-business-plans)にサインアップする
- 以下に示すように Office 365 Developer アカウントにサインアップする


#### Office 365 Developer アカウントへのサインアップ

MSDN サブスクリプションがある場合は、 Office 365 Developer アカウント特典を使用します。この特典は、MSDN サブスクライバーで Visual Studio Ultimate と Visual Studio Premium を使用できます。 MSDN サブスクリプションがある場合は、Office 365 Developer アカウント特典を使用します。この特典は、MSDN サブスクライバーで Visual Studio Ultimate と Visual Studio Premium を使用できます。

**それ以外の場合は、**[30 日間の試用版](https://portal.microsoftonline.com/Signup/MainSignUp.aspx?OfferId=6881A1CB-F4EB-4db3-9F18-388898DAF510&amp;DL=DEVELOPERPACK)で開始するか、[Office 365 Developer アカウント](https://portal.microsoftonline.com/Signup/MainSignUp.aspx?OfferId=C69E7747-2566-4897-8CBA-B998ED3BAB88&amp;DL=DEVELOPERPACK)を購入します (いずれかのオプションで 1 つのユーザー ライセンス)。このアカウントの 1 年あたりの費用は 99.00 ドルです。

<!--
Follow up with PMG about their plan to give away 10,000 O365 Developers subscriptions through dev.office.com. If/when that program happens, make sure to update the topic to link to that offer.
-->


新しい Office 365 Developer アカウントにサインアップする場合は、次の手順に従います。

    
図 1 に示すように、サインアップ中に **.onmicrosoft.com** のサブドメインと、作成しているドメインに割り当てるユーザーの ID を指定するように要求されます。サインアップ後にアカウントを管理するポータル サイトにサインインするには、その結果の userid (_userid@yourdomain_.onmicrosoft.com の形式) を使用する必要があります。 サインアップの後で、生成された資格情報 (UserID@yourdomain.onmicrosoft.com の形式) を使用して、アカウントを管理する off365short ポータル サイトにサインインする必要があります。sposhort 開発者向けサイトは、新しいドメイン http://yourdomain.sharepoint.com  にプロビジョニングされます。 


**Note**  If you're logged on to another Microsoft account when you try to sign up for a Developer subscription, you might get this message: "Sorry, that user ID you entered didn't work. It looks like it's not valid. Be sure you enter the user ID that your organization assigned to you. Your user ID usually looks like  _someone@example.com_ or _someone@example.onmicrosoft.com_." このメッセージが表示された場合、使用していた Microsoft アカウントからログアウトし、もう一度やり直してください。引き続きこのメッセージが表示される場合は、ブラウザー キャッシュを消去するか、[InPrivate ブラウズ] に切り替え、フォームにデータを入力します。 If you still get the message, clear your browser cache or switch to a private browser session and then fill out the form.


**図 1. 図 1Office 365 Developer アカウントのドメイン名**

![During signup to create your Office 365 Developer subscription, you're asked to supply a unique subdomain of .onmicrosoft.com and a user ID.](images\SP15_app_O365Signup_bordered.png)
    

サインアップ プロセスが完了すると、お使いのブラウザーに Office 365 のインストール ページが表示されます。**管理**アイコンを選択して、管理センター ページを開きます。


<a name="bk_CreateAzureSubscription"> </a>
## Office 365 アカウントを Azure AD と関連付けてアプリを作成および管理する

アプリケーションを認証するには、Microsoft Azure Active Directory (Azure AD) にアプリケーションを登録する必要があります。ここに Office 365 のユーザー アカウントとアプリケーション情報が保存されています。Azure 管理ポータルから Azure AD を管理するには、Microsoft Azure サブスクリプションが必要です。Microsoft Azure の管理ポータルを使用すると、ユーザー、ロール、アプリを管理できます。 

- **既存の Microsoft Azure サブスクリプションをお持ちの場合は**、[ビジネス向けの Office 365 サブスクリプションとそのサブスクリプションを関連付ける](#bk_AssociateExistingAzureSubscription)ことができます。 

- それ以外の場合は、アプリを登録および管理するために、新しい Azure サブスクリプションを作成して、Office 365 アカウントに関連付ける必要があります。


<a name="bk_AssociateExistingAzureSubscription"> </a>
### 既存の Azure サブスクリプションと Office 365 アカウントを関連付けるには


1. 既存の Azure 資格情報 (例: user@live.com などの Microsoft ID) を使用して、[Microsoft Azure の管理ポータル](https://manage.windowsazure.com)にログオンします。
        
2. **[Active Directory]** ノードを選択し、**[ディレクトリ]** タブを選択し、画面下の **[新規]** を選択します。 
     
4. [**新規**] メニュー上で、[**Active Directory**]  >  [**ディレクトリ**]  >  [**カスタム作成**] を選択します。
    
5. [**ディレクトリの追加**] の [**ディレクトリ**] ドロップ ダウン ボックスで、[**既存のディレクトリの使用**] を選択します。[**サインアウトする準備ができました**] のチェックボックスをオンにして、右下隅にあるチェック マークを選択します。 
    
    This brings you back to the Azure Management Portal.
        
3. Office 365 のアカウント情報を使ってログインします。 
    
    off365short テナントでログインします。使用しているディレクトリを winazure で使用するかどうかの確認を求められます。 
    
    **Important** To associate your Office 365 account with Azure AD, you'll need  an Office 365 business account with global administrator privileges. 
    
        
4. **[続行]**、**[今すぐサインアウトする]** の順に選択します。
        
5. ブラウザーを閉じて、もう一度[ポータル](https://manage.windowsazure.com)を開きます。それ以外の場合は、アクセス拒否エラーを受け取ります。
    
        
6. 既存の Azure 資格情報 (Microsoft ID など: user@live.com) を使用して、もう一度ログオンします。[**Active Directory**] ノードに移動すると、[**ディレクトリ**] にお客様の Office 365 アカウントの一覧が表示されています。
    
    
<a name="bk_AssociateNewAzureSubscription"> </a>
### 新しい Azure サブスクリプションを作成して Office 365 アカウントを関連付けるには


1. Office 365 にログオンします。[**ホーム**] ページから [**管理**] アイコンを選択して、Office 365 管理センターを開きます。
2. ページの左側にあるメニュー ページで [**管理**] までスクロール ダウンして、[**Azure AD**] を選択します。

    **Important** To open the Office 365 admin center, and access Azure AD, you'll need  an Office 365 business account with global administrator privileges. 
    
3. 新しいサブスクリプションを作成します。
        
    If you're using a trial version of Office 365, you'll see a message telling you that Azure AD is limited to customers with paid services. You can still create a trial 30-day Azure subscription at no charge, but you'll need to perform a few extra steps:
    
    1. 国または地域を選択して、**[Azure サブスクリプション]** を選択します。
    2. 個人情報を入力します。確認のために連絡可能な電話番号を入力して、テキスト メッセージの送信または通話のいずれかを指定します。
    3. 認証コードを受信したら、そのコードを入力して **[コードの確認]** を選択します。
    4. 支払い情報を入力して、合意のチェック ボックスをオンにし、**[サインアップ]** を選択します。
        
        Your credit card will not be charged.
        
        Do not close or refresh your browser while your Azure subscription is being created.
            
4. Azure サブスクリプションが作成されたら、**[ポータル]** を選択します。
        
5. Azure Tour が表示されます。表示するか、**[X]** をクリックしてツアーを閉じます。
        
    You should now see all items in your Azure subscription. It lists a directory with the name of your Office 365 tenant.
    

##次の手順

[!INCLUDE [BEGIN iOS section](../includes/controls/iossection.xml)]


これで、[最初のアプリをビルドして実行する](..\howto\getting-started-Office-365-APIs.md)準備ができました。 
    


- [iOS 用の Office 365 SDK](https://github.com/OfficeDev/Office-365-SDK-for-iOS) 


[!INCLUDE [END iOS section](../includes/controls/iossection.xml)]

[!INCLUDE [BEGIN Android section](../includes/controls/androidsection.xml)]


これで、[最初のアプリをビルドして実行する](..\howto\getting-started-Office-365-APIs.md)準備ができました。 


[!INCLUDE [END Android section](../includes/controls/androidsection.xml)]

[!INCLUDE [BEGIN JavaScript section](../includes/controls/javascriptsection.xml)]


これで、[最初のアプリをビルドして実行する](..\howto\getting-started-Office-365-APIs.md)準備ができました。 
    


[!INCLUDE [END JavaScript section](../includes/controls/javascriptsection.xml)]

[!INCLUDE [BEGIN ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]


これで、[最初のアプリをビルドして実行する](..\howto\getting-started-Office-365-APIs.md)準備ができました。 
    


[!INCLUDE [END ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]

## その他の技術情報


-  [Office 365 API スタート プロジェクト、サンプル コード、およびビデオ](..\howto\starter-projects-and-code-samples.md) 

-  [API サンドボックス](http://apisandbox.msdn.microsoft.com)を使用した Office 365 API のコードの記述


-  [Office 365 API リファレンス](..\api\API-Catalog.md)


    
    
[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]


