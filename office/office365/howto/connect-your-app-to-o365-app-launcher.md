---
ms.Toctitle: Connect to the app launcher
title: "アプリを Office 365 アプリ ランチャーに表示させる"
description: "ms.TocTitle: アプリ ランチャーに接続するTitle: アプリを Office 365 アプリ ランチャーに表示させるDescription: [個人用アプリ] ページからアプリを Office 365アプリ ランチャーに接続する方法、および Office 365 資格情報を使用したシングル サインオン エクスペリエンスを提供する方法について説明しています。ms.ContentId: 089d8e30-a9d3-423f-acf8-c727b32ac36f ms.topic: 記事 (方法)"
ms.ContentId: 089d8e30-a9d3-423f-acf8-c727b32ac36f
ms.date: June 30, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]


# アプリを Office 365 アプリ ランチャーに表示させる

_**適用対象:** Office 365_
 
アプリ ランチャーは、ユーザーがアプリを簡単に検索してそれにアクセスできるようにする、Office 365 の新機能です。この記事では、開発者がユーザーのランチャーにアプリが表示されるようにする方法、そして Office 365 資格情報を使用したシングル サインオン エクスペリエンスをユーザーに提供する方法について説明します。
 
## アプリ ランチャーは、アプリ、クイック タスクのタイル、そしてユーザーの [個人用アプリ] ページに接続します。
 
ランチャーを起動するには、上部のナビゲーション バーでランチャー アイコンを選択します。

![The default tiles displayed from a user's app launcher include basic apps in a tenancy and quick links to functions, such as Outlook tasks or the Office 365 Admin Portal.](images\Office365_AppLauncher_Launcher.png)
 
ユーザーのアプリ ランチャーには、特定のテナント内の基本アプリのいくつかが既定で表示されるだけでなく、Outlook タスクや Office 365 管理ポータルなど、すばやいアクセスが必要となるような他の機能へのリンクも表示されます。ランチャー アイコンは、どのアプリケーションが実行されているかに関係なく表示されるので、アプリを簡単に起動できます。ランチャーの右下隅にある [個人用アプリ] を選択すると、[個人用アプリ] ページが表示されます。これは、ユーザーのすべてのアプリに共通のホーム ページです。 The launcher icon appears no matter what application is running, so that other apps can be quickly started. Selecting **My apps** from the lower right corner of the launcher brings up the **My apps** page – home to all of a user’s apps.

The launcher is both customizable and extensible. ランチャーは、カスタマイズすることも、拡張することもできます。ユーザーは [個人用アプリ] ページから任意のアプリをランチャーに組み込むことができます。上部のナビゲーション バーに最大 3 つのアプリのアイコンが既定で表示されるようにすれば、それらのアプリにすぐにアクセスできます。ランチャーの詳細については、「Office 365 の最近の変更内容についてのヘルプを見つける」を参照してください。 For more information on the launcher, see [Finding help for the latest changes in Office 365](https://support.office.com/en-us/article/Find-help-for-the-latest-changes-in-Office-365-22E9A8BF-EF08-4B95-B10F-6E839440339C).
 
## アプリがユーザーの [個人用アプリ] ページに含まれるように Azure AD と統合する
 
You can use the integration between **Office 365** and **Azure Active Directory** to get your app assigned to specific users, and show up on their **My apps** page. Every Office 365 tenant has an associated Azure AD, which is the source for all app registration, configuration, and permissions for the tenant. The associated Azure AD is available free of charge with an Office 365 subscription, but to administer you must complete Azure registration; see [additional information on Azure registration](http://technet.microsoft.com/en-us/library/dn832618).

次のように、開発者はさまざまなシナリオで、 Azure AD を使用して認証を行うアプリを開発します。開発したアプリは、次の 2 つの方法でユーザーに割り当てることができます (ユーザーの [個人用アプリ] ページには、そのユーザーに割り当てられているアプリが表示されます)。 次に示すように、開発者はさまざまなシナリオで、Azure AD を使用して認証を行うアプリを開発します。開発したアプリは、いくつかの方法でユーザーに割り当てることができます (ユーザーの [個人用アプリ] ページには、そのユーザーに割り当てられているアプリが表示されます)。

- Develop an app for a single organization, and register it in the organization’s Azure AD. Or, as an independent software vendor (ISV), write a multi-tenant app that is registered in the Azure application gallery. An organization’s Azure AD administrator then configures the app and assigns it to individual users, and the app will show up on their **My apps** pages. To learn more about the admin option, skip to [An admin can configure custom apps or shared apps in Azure AD](#section_3).

- ユーザーが自分の Office 365 資格情報を使用してサインオンできるようにして、管理者を割り当てずにユーザーの Azure AD に登録するアプリを開発します。「[アプリのシングル サインオン フロー](#section_2)」のセクションを参照してください。

- ユーザーが **Office ストア**から直接インストールできる Web アプリを開発します。次のセクション、「[Office ストアのシングル サインオン Web アプリ](#section_1_5)」を参照してください。

## Office ストアのシングル サインオン Web アプリ

シングル サインオン フローを使用する Web アプリを作成して、そのアプリを **Office ストア**に提出できます。ユーザーは、アプリ ランチャーにある **Office ストア**の既定のアイコンを選択して、Office 365 の資格情報を使用してアプリをインストールします。アプリの作成手順は、「[アプリのシングル サインオン フロー](#section_2)」に示した手順とほとんど同じですが、インストール アクションはベンダーの Web サイトからではなくストアから実行します。ストアにアプリを提出する場合は、「[販売者ダッシュボードに Office 365 用 Web アプリを提出する](https://msdn.microsoft.com/office/office365/HowTo/submit-web-apps-seller-dashboard)」を参照してください。  

## アプリのシングル サインオン フロー

Developers can create applications using **Office 365 API Tools for Visual Studio** that let users install the app using their Office 365 credentials. After installation, the app will appear on the user’s **My apps** page, and be launched from then on without further authentication. Such applications are developed by independent software vendors (ISVs) to be run by many different organizations (multi-tenant). Users can visit the ISV’s website, and install the app using their Office 365 credentials.

###サインオンからランチャーまでのユーザー エクスペリエンス

1. ユーザーがプロバイダーの Web サイト上のリンクを選択し、 Office 365 資格情報を使用してログオンします。
2. Azure AD により、サインオン ページが表示されます。サインオン ページに、アプリの名前と特定のリソース (プロファイル、連絡先など) に対する要求が示されます。ユーザーは [OK] を選択して同意します。 User consents by choosing **OK**.
3. プロバイダーがユーザーとのセッションを開始し、ユーザーとの間で情報 (サービス契約の条項、電子メール アドレスの確認) の要求および受信を行います。
4. プロバイダーのアプリへのユーザー サインインが完了します。サインイン後は、ユーザーが **[個人用アプリ]** ページでアプリを見つけて、そのアプリをランチャーに組み込むことができます。

###承認と認証

Azure AD では、標準 **OAuth 2.0** プロトコルを使用して Office 365 のアプリを認証できます。アプリは Azure AD の **認証エンドポイント**から**認証コード**を取得して、そのコードを**トークン エンドポイント**に送信することで、Office 365 API を使用した要求を有効にするためのアクセス トークンおよび更新トークンを取得します。詳細およびフローについては、「[Office 365 のアプリ認証とリソース承認](https://msdn.microsoft.com/office/office365/howto/common-app-authentication-tasks)」を参照してください。

Apps with single sign-on flow use an additional protocol: [**OpenID Connect**](http://msdn.microsoft.com/en-us/library/azure/dn645541.aspx). While **OAuth** requests an **auth code**, **OpenID Connect** requests an **identity token**. シングル サインオン フローを使用するアプリは、さらに追加のプロトコルとして  OpenID Connect を使用します。OAuth は認証コードを要求する一方、OpenID Connect は ID トークンを要求します。このトークンはシングル サインオンを有効にするだけでなく、ID 要求とユーザー プロファイル情報との突き合わせなどといった、サインオン フローの他の要素も使用可能にします。 As of late 2015 we also support the submission of valid SAML based applications to the Office Store.

###シングル サインオン アプリケーションを構築する際の重要な概念

- **Office 365 API Tools for Visual Studio** provides all you need to enable single sign-on flow. Add references to **OpenID Connect**, and the [**Open Web Interface for .NET (OWIN)**](http://owin.org/) framework. Request permissions for user resources through the **Add -> Connected Service** dialog. A related code sample to explore the tooling required may be found on GitHub at: [https://github.com/OfficeDev/O365-WebApp-MultiTenant](https://github.com/OfficeDev/O365-WebApp-MultiTenant).

- アプリ ランチャーは、アプリケーションのサインオン URL へのサインオンを開始します。サインオン URL の値が空の場合、ランチャーは Application オブジェクト プロパティの応答 URL を既定値として使用します。シームレスなシングル サインオン エクスペリエンスを提供するには、この値を、アプリケーションのユーザー認証フローを開始する所定の宛先に設定することが重要です。 アプリ ランチャーは、アプリケーションのサインオン URL へのサインオンを開始します。サインオン URL の値が空の場合、ランチャーは Application オブジェクト プロパティの応答 URL を既定値として使用します。シームレスなシングル サインオン エクスペリエンスを提供するには、この値を、アプリケーションのユーザー認証フローを開始する所定の宛先に設定することが重要です。

- アプリを効率的に開発、実動前、実動へと移すために、リダイレクト URI の BaseUrl は実行時に計算されるようにしてください (例については、前述のサンプル コードの startup.auth.cs を参照してください)。

- Azure AD で OAuth を使用して認証エンドポイントを呼び出すための構文は、 です。ここで、  は、検証済みのドメイン、テナント ID、または Common (マルチテナント アプリの場合) に置き換えることができます。 

- アプリでは、匿名および認証を適用したユーザー エクスペリエンスの組み合わせ、または認証のみを適用したユーザー エクスペリエンスをサポートできます。認証を必要とする関数については、常に、該当するコード セクションの先頭で [Authorize] 属性を使用してください。 アプリでは、匿名および認証を適用したユーザー エクスペリエンスの組み合わせ、または認証のみを適用したユーザー エクスペリエンスをサポートできます。認証を必要とする関数については、常に、該当するコード セクションの先頭で [Authorize] 属性を使用してください。

##管理者による Azure AD でのカスタム アプリまたは共有アプリの構成

Azure AD 管理者は次の 2 種類のアプリケーションを構成できます。

- 単一の組織を対象とした基幹業務 (LoB) アプリケーション (シングル テナント アプリ)。これは、エンタープライズ開発者が作成し、組織の Azure AD に登録されるアプリケーションです。

- 複数の組織を対象とした Software-as-a-Service (SaaS) アプリケーション (マルチテナント アプリ)。これは、独立系ソフトウェア ベンダー (ISV) が作成し、Azure アプリケーション ギャラリーに保管されるアプリケーションです。

これら 2 種類のアプリケーションを開発および登録する際のガイダンスについては、「アプリケーションの追加、更新、および削除」を参照してください。Office 365 に対する Azure AD での認証のガイドについては、 「Office 365 のアプリ認証とリソース承認」を参照してください。 これら 2 種類のアプリケーションを開発および登録する際のガイダンスについては、「アプリケーションの追加、更新、および削除」を参照してください。Office 365 に対する Azure AD での認証のガイドについては、 「Office 365 のアプリ認証とリソース承認」を参照してください。

管理者が Azure AD にサインオンするには、  Azure 管理ポータル を使用するか、または Office 365 管理センターの左側のダッシュボードにある [Azure AD] メニュー項目を使用します。Azure AD にサインオンしたら、管理者は該当するディレクトリを選択して、メニューから [アプリケーション] を選択した後、コマンド バーの [追加] ボタンを選択します。これにより、次のオプションが表示されます。 Once on Azure AD, an admin selects the appropriate directory, then **Applications** from the menu, then the **ADD** button on the command bar. The following options are displayed:

- **組織が開発中のアプリケーションを追加**
- **ギャラリーからアプリケーションを追加**

管理者が最初のオプションを選択した場合、 [アプリケーションの追加 ウィザード] のアプリケーションの [名前]、[種類]、[アプリ ID の URI]、[サインオン URL] (Application オブジェクト プロパティの [App URL]) などのフィールドにデータが取り込まれます。必要に応じて、[ロゴ ウィザード] でカスタム ロゴをアップロードします。 There is also a **Logo wizard** to upload a custom logo if desired.

2 番目のオプション [ギャラリーからアプリケーションを追加する] を選択すると、 [アプリケーション ギャラリー] が表示されます (以下を参照)。

![After a signed-in Azure AD admin selects Add an Application from the Gallery, the admin can search for available apps in the Application Gallery or install an organization's custom app.](images\Office365_AppLauncher_AppGallery.png)

ギャラリーでは、管理者特定のアプリを検索したり、[カスタム] を選択して、組織が使用している他のアプリケーションをインストールしたりできます。アプリを選択してインストールすると、構成メニューが表示されます。このメニューでは、次のオプションから選択できます。 ギャラリーでは、管理者特定のアプリを検索したり、[カスタム] を選択して、組織が使用している他のアプリケーションをインストールしたりできます。アプリを選択してインストールすると、構成メニューが表示されます。このメニューでは、次のオプションから選択できます。
 
- **Microsoft Azure AD によるシングル サインオンを有効にする**
- **[アプリの名前] に対する自動ユーザー プロビジョニングを有効にする (アプリによっては、表示されない場合もあります)**
- **[アプリの名前] にユーザーを割り当てる**

[Microsoft Azure AD によるシングル サインオンを有効にする] を選択すると、次のオプションが表示されます。

- Microsoft Azure AD シングル サインオン (フェデレーション サインオンを使用するすべてのアプリに適用されます)
- **パスワードによるシングル サインオン**
- **既存のシングル サインオン**

これらのオプションのいずれかを選択すると、Azure AD がアプリケーションに必要なアクセス情報を格納して管理するようになるため、ユーザーは Office 365 資格情報だけを使用して、自分に割り当てられているすべてのアプリにアクセスできます。シングル サインオンの各オプションの詳細については、「のシングル サインオンのサポート」を参照してください。 For more information on each of the single sign-on options, see [Support for single sign-on](https://msdn.microsoft.com/library/azure/dn308588.aspx#bkmk_supportsso). 

The next step is to assign users to the application. On the previous configuration menu, select **Assign users to [appname]**. The following screen is shown:

![After an Azure AD admin selects Assign Users, the admin can select currently unassigned users and then choose ASSIGN on the command bar at the bottom of the screen.](images\Office365_AppLauncher_AssignUser.png)

The admin can select one or more users who are **Unassigned**, and then choose **ASSIGN** on the command bar. For federated apps, there may be pre-set user roles, which can be matched to specific user roles as part of the assignment. 管理者は、[未割当] から 1 名以上のユーザーを選択できます。アプリに割り当てるユーザーを選択したら、コマンド バーで [割り当て] を選択します。フェデレーション アプリケーションで、あらじめ設定されたユーザー ロールがある場合は、割り当ての一環として、それらのユーザー ロールを特定のユーザー ロールに対応させることができます。非フェデレーション アプリケーションの場合、管理者はユーザーに代わって資格情報を入力するオプションを使用できます。これを省略すると、ユーザーが最初にアプリを使用するときに、ユーザー名とパスワードの入力を求めるプロンプトが出されます。

アプリへの割り当てが完了すると、ユーザーの [個人用アプリ] ページにアプリのアイコンが表示されます。アプリのアイコンはランチャーに組み込むことができます。アイコンをランチャーに組み込むと、ユーザーはアプリを起動するたびに資格情報を入力しなくても済むようになります。

##その他の技術情報

- [Find help for the latest changes in Office 365](https://support.office.com/en-us/article/Find-help-for-the-latest-changes-in-Office-365-22E9A8BF-EF08-4B95-B10F-6E839440339C)
- [Office 365 のアプリ認証とリソース承認](http://msdn.microsoft.com/en-us/office/office365/howto/common-app-authentication-tasks)
- [Azure AD の認証シナリオ](https://msdn.microsoft.com/en-us/library/azure/dn499820)
- [アプリケーションの追加、更新、および削除](https://msdn.microsoft.com/en-us/library/azure/dn132599)
- [Azure Active Directory のアプリケーション アクセス機能](http://msdn.microsoft.com/en-us/library/azure/dn308588.aspx)
- [Azure AD Authentication Library for .NET](http://msdn.microsoft.com/en-us/library/azure/jj573266.aspx)
- [Application Submissions to the Azure Gallery](http://blogs.msdn.com/b/azureappgallery/archive/2014/07/23/application-submissions-to-the-azure-gallery.aspx)




