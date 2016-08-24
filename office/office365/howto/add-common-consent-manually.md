---
ms.Toctitle: Manually register your app
title: "Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する"
description: "ms.TocTitle: アプリを手動で登録するTitle: Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録するDescription: OneDrive for Business のファイルや他のセキュリティで保護されたリソースにアクセスできるように、Azure Active Directory の共通コンテンツを手動で ASP.NET アプリケーションに追加します。ms.ContentId: 95479f73-15d7-426e-abdf-ae2c72b5cd33 ms.topic: 記事 (方法)"
ms.ContentId: 95479f73-15d7-426e-abdf-ae2c72b5cd33
ms.date: September 9, 2015
---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する   

_**適用対象:** Office 365_
 

<a name="bk_intro"> </a>

Office 365 API サービスは、ユーザーの Office 365 データに対し、Azure Active Directory (Azure AD) を使用してセキュリティ保護された認証を行います。Office 365 API にアクセスするには、Azure AD にアプリを登録する必要があります。 


<a name="bk_RegistrationPrereqs"> </a>
##Azure AD にアプリを登録するための前提条件

アプリを登録するには、次の 2 つのアカウントが必要です。

- Office 365 ビジネス アカウント

    If you don't have an existing Office 365 business account, there are several ways to create one. For more information, see [Get an Office 365 account](..\howto\setup-development-environment.md#bk_Office365Account).

- Office 365 ビジネス アカウントに関連付けられた Azure AD サブスクリプション

    This is where you'll actually register your app. You can use an existing Azure AD tenancy, or create a new one.
    When you sign up for an Office 365 business account, a Microsoft Azure subscription is automatically created and associated with that Office 365 account.
    
    既存の Microsoft Azure サブスクリプションをお持ちの場合は、ビジネス向けの Office 365 サブスクリプションとそのサブスクリプションを関連付けることができます。 If not, you'll need to create an association to the Azure subscription that was created when you signed up for your Office 365 business account.
    
    For more information, see [Associate your Office 365 account with Azure AD to create and manage apps](..\howto\setup-development-environment.md#bk_CreateAzureSubscription).
    



<a name="bk_RegisterApp"> </a>
## Azure 管理ポータルにアプリを登録する 

登録の一環として、アプリが Web アプリケーション (MVC または Web フォーム ソリューションなど) であるか、ネイティブ アプリ (スマート フォンやその他のモバイル デバイスなど) であるかを指定します。Azure AD は、この情報を使用して、アプリが Azure で認証されるために必要なリソースを生成します。Web アプリケーションの場合、Azure はクライアント ID とアプリ シークレットの両方を生成します。ネイティブ アプリケーションの場合、Azure はクライアント ID を生成します。

アプリを登録した後は、アプリのプロパティを構成できます。これらのプロパティには以下が含まれます。

- Azure が認証の際にリダイレクトに使用する、アプリのエンドポイントの指定
- Web アプリケーションの場合、Azure にテナントを登録して、そのテナントだけにアプリを利用可能にするか、あるいは複数のテナントに利用可能にするか
- Web アプリケーションの場合、アプリ シークレットを生成するかどうか、およびアプリ シークレットの有効期間

アプリを登録したら、アプリがアクセスする必要のある Office 365 サービスを指定できます。Office 365 サービスを指定する際に、アプリに必要な Office 365 API に対するアクセス許可レベルも指定します。


<a name="bk_RegisterNativeApp"> </a>
### Azure 管理ポータルにネイティブ アプリを登録する 

次の手順は、iOS および Android のアプリを含むネイティブ アプリ向けのものです。 

1. Office 365 ビジネス アカウント資格情報を使用して、 [Azure 管理ポータル](https://manage.windowsazure.com/)にサインインします。

2. 左側の列にある **[Active Directory]** ノードをクリックして、Office 365 サブスクリプションにリンクされているディレクトリを選択します。

    ![A screenshot of the Azure Management Portal website. The item 'Active Directory' is selected in the left navigation pane. In the main pane, the Directory tab is selected. The name of the current directory is highlighted.](images\O365APIs_RegisterApp_1.png)

3. **[アプリケーション]** タブを選択してから、ページの下部にある **[追加]** を選択します。
    
    ![A screenshot of the directory information page. In the menu bar at the bottom of the page, the New icon is highlighted.](images\O365APIs_RegisterApp_2.png)

4. ポップアップ ウィンドウで、**[所属組織が開発しているアプリケーションの追加]** を選択します。 Then click the arrow to continue.

5. アプリの名前を選択し、[種類] として **[ネイティブ クライアント アプリケーション]** を 選択します。矢印をクリックして続行します。 Then click the arrow to continue.
    
6. 次に、リダイレクト URI を指定します。Azure AD は OAuth 2.0 要求に対する応答として、この URI にユーザー エージェントをリダイレクトします。値は、物理的なエンドポイントである必要はありませんが、有効な URI でなければなりません。リダイレクト URL は、アプリの一意の識別子として考えることができます。
    
7. チェック マークをクリックして、アプリの登録を完了します。
    
ネイティブ アプリが Azure AD に登録されました。これで、「[Office 365 API サービスから、アプリに必要なアクセス許可レベルを指定する](#bk_ConnectAppToO365)」に進むことができます。




<a name="bk_RegisterWebApp"> </a>
### Azure の管理ポータルにブラウザー ベースの Web アプリケーションを登録する 

次の手順は、ブラウザー ベースの Web アプリケーション向けのものです。これらの種類のアプリは、サーバーからソース コードをロードした後、ブラウザーで完全に実行できるようになります。

1. Office 365 ビジネス アカウント資格情報を使用して、 [Azure 管理ポータル](https://manage.windowsazure.com/)にサインインします。

2. 左側の列にある **[Active Directory]** ノードをクリックして、Office 365 サブスクリプションにリンクされているディレクトリを選択します。

    ![A screenshot of the Azure Management Portal website. The item 'Active Directory' is selected in the left navigation pane. In the main pane, the Directory tab is selected. The name of the current directory is highlighted.](images\O365APIs_RegisterApp_1.png)

3. **[アプリケーション]** タブを選択してから、ページの下部にある **[追加]** を選択します。
    
    ![A screenshot of the directory information page. In the menu bar at the bottom of the page, the New icon is highlighted.](images\O365APIs_RegisterApp_2.png)

4. ポップアップ ウィンドウで、**[所属組織が開発しているアプリケーションの追加]** を選択します。 Then click the arrow to continue.

5. アプリの名前を選択して、[種類] として [**Web アプリケーションまたは Web API**] を選択します。矢印をクリックして続行します。 Then click the arrow to continue.

6. **サインオン URL** は、ユーザーがサインインしてアプリを使用できる Web ページのアドレスです。このプロパティに設定される値は、**応答 URL** の値としても設定されます。
    
7. **App ID URI** は、アプリケーションを識別する Azure AD の一意の識別子です。これは、組織の Azure AD の一意の値である必要があります。
    
7. チェック マークをクリックして、アプリの登録を完了します。
    
ブラウザー ベースの Web アプリケーションが Azure AD に登録されました。ただし、OAuth 2.0 の暗黙的な付与フローを許可するようアプリケーションを構成し、アプリの Azure テナンシー スコープを構成する必要があります。構成が完了したら、「[Office 365 API サービスから、アプリに必要なアクセス許可レベルを指定する](#bk_ConnectAppToO365)」に進むことができます。

#### OAuth 2.0 の暗黙的な付与フローを許可するようアプリケーションを構成する

ブラウザー ベースの Web アプリケーションは、アプリケーション シークレットをセキュリティ保護できないため、アプリケーションは OAuth の暗黙的な付与フローを使用します。既定では OAuth の暗黙的な付与フローは許可されていないため、これを許可するようにアプリケーションのマニフェストを更新する必要があります。

1. Azure の管理ポータルで、アプリケーションのエントリの **[構成]** タブを選択します。

2. ドロワーの **[マニフェストの管理]** ボタンを使用して、アプリケーションのマニフェスト ファイルをダウンロードし、コンピューターに保存します。

3. Open the manifest file with a text editor. Search for the *oauth2AllowImplicitFlow* property. By default it is set to *false*; change it to *true* and save the file.

4. **[マニフェストの管理]** ボタンを使用して、更新されたマニフェスト ファイルをアップロードします。

#### Web アプリを単一テナント アプリまたはマルチテナント アプリのどちらにするかを指定する

ブラウザー ベースのアプリケーションの場合、アプリの Azure テナント範囲を構成することもできます。つまり、Azure にテナントを登録して、そのテナントだけにアプリを利用可能にするか、あるいは複数の Axure テナントに利用可能にするかを指定します。 

The default is **No**. 既定値は [いいえ] です。テナント範囲は、必要に応じて後から変更できます。

1. Azure 管理ポータルで、アプリケーションを選択し、上部メニューの **[構成]** を選択します。**[マルチ テナント型アプリケーションにする]** までスクロールダウンして、**[はい]** または **[いいえ]** を選択します。






<a name="bk_RegisterServerApp"> </a>
### Azure の管理ポータルに Web サーバー アプリを登録する 

次の手順は、Node.js アプリなど、Web サーバー アプリ向けのものです。

1. Office 365 ビジネス アカウント資格情報を使用して、 [Azure 管理ポータル](https://manage.windowsazure.com/)にサインインします。

2. 左側の列にある **[Active Directory]** ノードをクリックして、Office 365 サブスクリプションにリンクされているディレクトリを選択します。

    ![A screenshot of the Azure Management Portal website. The item 'Active Directory' is selected in the left navigation pane. In the main pane, the Directory tab is selected. The name of the current directory is highlighted.](images\O365APIs_RegisterApp_1.png)

3. **[アプリケーション]** タブを選択してから、ページの下部にある **[追加]** を選択します。
    
    ![A screenshot of the directory information page. In the menu bar at the bottom of the page, the New icon is highlighted.](images\O365APIs_RegisterApp_2.png)

4. ポップアップ ウィンドウで、**[所属組織が開発しているアプリケーションの追加]** を選択します。 Then click the arrow to continue.

5. アプリの名前を選択して、[種類] として [**Web アプリケーションまたは Web API**] を選択します。矢印をクリックして続行します。 Then click the arrow to continue.

6. **サインオン URL** は、ユーザーがサインインしてアプリを使用できる Web ページのアドレスです。このプロパティに設定される値は、**応答 URL** の値としても設定されます。
    
7. **App ID URI** は、アプリケーションを識別する Azure AD の一意の識別子です。これは、組織の Azure AD の一意の値である必要があります。
    
7. チェック マークをクリックして、アプリの登録を完了します。
    
Web サーバー アプリが Azure AD に登録されました。ただし、アプリ シークレットを生成して、クライアントがサーバー アプリケーションで認証できるようにする必要があります。その後、「[Office 365 API サービスから、アプリに必要なアクセス許可レベルを指定する](#bk_ConnectAppToO365)」に進むことができます。

<a name="bk_ConfigureApp_Secret"> </a>
#### Web アプリケーションの新しいアプリ シークレットを生成する

1. Azure 管理ポータルで、アプリケーションを選択し、上部メニューの [構成] を選択します。 Scroll down to **keys**.

2. クライアント シークレットの有効期間を選択し、[保存] アイコンをクリックします。

    ![A screenshot of the app configuration page for your app, on the Azure Management Portal website. Under the page section titled 'keys', a drop-down list is shown for selecting the duration of the new key. In the menu bar at the bottom of the page, the SAVE icon is highlighted.](images\O365APIs_RegisterApp_7.png)
    
    Azure displays the app secret.

3. クリップボード アイコンをクリックし、クライアント シークレットをクリップボードにコピーします。

    ![A screenshot of the app configuration page for your app, on the Azure Management Portal website. Under the page section titled 'keys', the new client secret is now displayed. Next to the client secret, the Clipboard icon is highlighted.](images\O365APIs_RegisterApp_8.png)

    **Important** Azure only displays the client secret at the time you initially generate it. You cannot navigate back to this page and retrieve the client secret later. 



<a name="bk_ConnectAppToO365"> </a>
## アプリが必要とする Office 365 API のアクセス許可を指定する 

最後に、アプリに必要な Office 365 API の具体的なアクセス許可を指定する必要があります。それには、アプリに必要な API が含まれる Office 365 サービスへのアクセスを追加してから、そのサービスの API から必要なアクセス許可を指定します。Office 365 API のアクセス許可の完全なリストについては、「[Office 365 アプリケーション マニフェストとアクセス許可の詳細](..\howto\application-manifest.md)」を参照してください。  

1. Azure 管理ポータルで、アプリケーションの構成ページの一番下までスクロールダウンし、**[他のアプリケーションへのアクセス許可]** で **[アプリケーションの追加]** を選択します。
 
    ![A screenshot of the app configuration page for your app, on the Azure Management Portal website. Under the page section titled 'permission to other applications', the 'Add application' button is highlighted.](images\O365APIs_RegisterApp_4.png)

2. アプリがアクセス許可を必要とする Office 365 のサービスを選択します。
    

    1. サービス名を選択し、プラス記号をクリックしてサービスを追加します。 
    2. 追加したサービスが **[選択済み]** 列にリストされます。 
    3. チェック マーク アイコンをクリックして、選択を保存してください。
    ![A screenshot of the 'permissions to other applications' page. The available services are listed in a table. Next to the name of the selected service is a plus icon. At the far right is a column that will list the applications you add to your app. The check mark icon that you click to save your choices is highlighted at the top of the page.](images\O365APIs_RegisterApp_5.png)

    You are returned to your app's configuration page.

4. **[他のアプリケーションへのアクセス許可]** で、追加した各サービスの **[デリゲートされたアクセス許可]** 列をクリックし、アプリに必要なアクセス許可を指定します。

    ![A screenshot of the app configuration page for your app, on the Azure Management Portal website. Under the page section titled 'permissions to other applications', the services that you just added are listed in a table. Next to the name of each application is a column titled 'Delegated Permissions'. This column displays a drop-down menu of the permissions you can request for your app from each application you added.](images\O365APIs_RegisterApp_6.png)

    These are the permissions that will be displayed to your app user when  Azure prompts them to consent to your app's permission request. In general, request only the services your app actually requires, and specify the least level of permissions in each service that still enable your app to perform its functions.

    Also, be aware that permission levels are additive. There is no need to request multiple permission levels for a given API, as the more expansive permission level already includes the more restricted permission. For example, for the Mail API, the **Send email as a user** permission already includes the **Read and write access to users' email** permission.
    
    For more information on specific permissions, see [Office 365 application manifest and permission details](..\howto\application-manifest.md).


<a name="bk_NextSteps"> </a>
## 次の手順

アプリを登録し、構成し、Office 365 サービスに接続した後は、Azure AD でアプリを認証してユーザーの Office 365 データにアクセスするためのコードをアプリに追加できます。

次のセクションにリストされているスタート プロジェクト、サンプル コード、手順のトピック、参照資料を利用して、アプリを使用可能な状態にするために必要な設定を行ってください。

<a name="bk_AdditionalResources"> </a>
## その他の技術情報

- [Office 365 API スタート プロジェクト、サンプル コード、およびビデオ](..\howto\starter-projects-and-code-samples.md)

- [Office 365 API リファレンス](..\api\api-catalog.md)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 のアプリケーション マニフェストとアクセス許可の詳細](..\howto\application-manifest.md)
