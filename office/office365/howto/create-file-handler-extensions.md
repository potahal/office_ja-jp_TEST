---
ms.Toctitle: Create file handlers in Office 365
title: "Office 365 でのファイル ハンドラーの作成"
description: "Create an Office 365 file handler with basic functionality, based on modifying a project template."
ms.ContentId: d8f4bc98-118c-444a-9e64-e16fb36bf605
ms.date: December 10, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 でのファイル ハンドラーの作成

    
_**適用対象:** Office 365_


このチュートリアルでは、File Handler アプリケーション プロジェクト テンプレートを使用して、カスタムのファイルの種類に対応する基本的なファイル ハンドラーの作成方法について説明します。Visual Studio 2013 と Visual Studio 2015 のテンプレートは、「[File Handler Application](https://visualstudiogallery.msdn.microsoft.com/16013099-a1c2-4718-b111-c581a095996e)」からダウンロードできます。また、**[新しいプロジェクト]** ダイアログの **[オンライン テンプレート]** セクションから検索することもできます。


<a name="sectionSection102"> </a>
## Office 365 アプリケーションの作成とカスタマイズ

前述したように、このサンプルは File Handler アプリケーション プロジェクト テンプレートに基づいているため、最初に、このテンプレートを使用して新しいプロジェクトを作成する必要があります。これは、「[File Handler Application](https://visualstudiogallery.msdn.microsoft.com/16013099-a1c2-4718-b111-c581a095996e)」からダウンロードできます。また、**[新しいプロジェクト]** ダイアログの **[オンライン テンプレート]** セクションから検索することもできます。

###テンプレートをダウンロードしてプロジェクトを作成するには

1. **[ファイル]** メニューの **[新規作成]** をクリックし、**[プロジェクト]** をクリックします。
2. 左側のウィンドウで、**[オンライン]** を選択して、**[Visual C#]** のカテゴリから **[Office]** を選択します。
3. **File Handler Application** は、中央のウィンドウにオプションとして表示されます。これを選択し、名前に「basicfilehandler」と入力してから **[OK]** をクリックして、プロジェクトを作成します。

まだテンプレートをインストールしていない場合は、テンプレートのインストールを求めるダイアログが表示されます。

次に、Azure AD でアプリを登録してから、ファイル ハンドラー拡張機能を追加できるようにアプリを変更します。

###Azure AD でアプリを登録および構成するには

1. ソリューション エクスプローラーで、プロジェクト名を右クリックして、[追加] > [接続済みサービス] の順に選択します。

2. **[Office 365 API]** を選択して、**[構成]** をクリックします。

3. **[アプリを登録する]** をクリックします。 

    **Note** The text in the **Add Connected Service** dialog may differ depending on Visual Studio version you are working in. Also, if **Register your app** is not available, you may need to remove placeholder values for the ClientId and ClientSecret in the web.config. To do this, search for the following keys and remove them from the web.config:
    
    ```xml
    <add key="ida:ClientId" value="[ClientId placeholder]" />
    <add key="ida:ClientSecret" value="[ClientSecret placeholder]" />
    ```

4. Office 365 開発者の組織のテナント管理者アカウントでサインインします。

6. **[アプリのプロパティ]** をクリックします。

7. **リダイレクト URI** リストに以下を追加します。

    - `http://basicfilehandler.azurewebsites.net` 

    - `https://basicfilehandler.azurewebsites.net` 

    **Note** The basicfilehandler.azurewebsites.net site is where the file handler application will be hosted. You will create this when you publish this project to Azure in a later step.

8. **[適用]** をクリックして **[アプリのプロパティ]** ダイアログを閉じてから、**[OK]** をクリックして **[サービス マネージャー]** ダイアログを閉じます。

この時点で、Visual Studio は必要な NuGet パッケージをプロジェクトに追加します。これで、アプリのアクセス許可を構成できるようになります。アクセス許可の構成は、Azure AD 管理ポータルで実行する必要があります。

9. [Azure 管理ポータル](https://manage.windowsazure.com/)にサインインします。

10. In the left navigation panel, select **Active Directory**. 左側のナビゲーション パネルで、**[Active Directory]** を選択します。[ディレクトリ] タブが選択されていることを確認してから、ディレクトリ名をクリックします。

11. ディレクトリのページで、[アプリケーション] を選択します。ファイル ハンドラー アプリケーションがリストに表示されます。リストに表示されていない場合は、**[表示]** ドロップダウンから **[自分の会社が所有するアプリケーション]** を選択します。

12. アプリケーションを選択して、トップ メニューの **[構成]** をクリックします。

13. ページの下側までスクロールして、**[他のアプリケーションに対するアクセス許可]** で **[アプリケーションの追加]** を選択します。

14. **[Microsoft Graph]** を選択してから、チェックマーク アイコンをクリックします。

15. **[他のアプリケーションに対するアクセス許可]** で、**[デリゲートされたアクセス許可]** 列をクリックし、**[ユーザーが選んだファイルの読み取りと書き込み]** を選択します。

16. 下側のナビゲーション バーで、**[保存]** をクリックします。




<a name="sectionSection0"> </a>
## ファイル ハンドラー アプリケーションのコーディング

ファイル ハンドラー固有のコードをアプリケーションに追加する準備が整いました。プロジェクトの作成に File Handler アプリケーション プロジェクト テンプレートを使用している場合は、この作業の大部分が既に済んでいます。残されている主なタスクは、NewFile、Open および Preview の各メソッドにコードを追加して、カスタムのファイルの種類に対する処理を指定することです。

これらのメソッドは、プロジェクト ソリューションの [コントローラー] フォルダーに含まれている FileHandlerController.cs ファイル内にあります。

Preview メソッドについては、次に示すメソッド宣言を探します。

```cs
public async Task<ActionResult> Preview()
```

Open メソッドについては、次に示すメソッド宣言を探します。

```cs
public async Task<ActionResult> Open()
```

NewFile メソッドについては、次に示すメソッド宣言を探します。

```cs
public async Task<ActionResult> NewFile()
```

**注** これらのメソッド内の最初のコードの部分では、アクティブ化パラメーターを読み込みます。アクティブ化パラメーターには、Office 365 がファイル ハンドラーに対する POST 要求の一部として含める情報が格納されています。 **注** これらのメソッド内の最初のコードの部分では、アクティブ化パラメーターを読み込みます。アクティブ化パラメーターには、Office 365 がファイル ハンドラーに対する POST 要求の一部として含める情報が格納されています。 プロジェクト テンプレートに組み込んだコードでは、ファイル ハンドラーが呼び出された直後に、これらの値にアクセスしてキャッシュします。使用可能なパラメーターの詳細については、「ファイル ハンドラーのアクティブ化パラメーター」を参照してください。 For more information about the parameters available, see File handler activation parameters.



<a name="sectionSection4"> </a>
## アプリケーションの発行

Azure にアプリケーションを発行する準備が整いました。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックして、**[発行]** を選択します。

2. **[Microsoft Azure Websites]** を選択します。

3. 資格情報のプロンプトが表示されたら、Azure のサブスクリプションを管理するために使用する資格情報を入力します。

4. **[既存の Web サイトの選択]** で、**[新規]** をクリックします。

5. **[サイト名]** に「**basicfilehandler**」と入力します。

6. セットアップされたデータベース サーバーがない場合は、**[新しいサーバーの作成]** を指定します。それ以外の場合は、使用するデータベース サーバーを選択します。

7. [作成] をクリックします。

8. サイトが作成されたら、**[発行]** をクリックします。


<a name="sectionSection5"> </a>
## ファイル ハンドラーの構成

ファイル ハンドラー アプリケーションを発行すると、Office 365 で構成できるようになります。 

1. [AddIns Manager サンプル ツール](https://addinsmanager.azurewebsites.net/)に移動します。このサンプル ツールは、ファイル ハンドラーを構成する Azure AD Graph API に必要なクエリを作成するために使用できます。AddIns Manager ツールを使用すると、Azure AD でアプリの構成が更新されます。

   **注** AddIns Manager サンプル ツールは、デモおよびテストのみを目的としたものです。運用環境では使用しないでください。 

2. ブラウザーにページが読み込まれたら、ページの右上にある **[Sign in]** をクリックします。

3. テナント管理者の資格情報を入力して、**[sign in]** をクリックします。

4. 左側のナビゲーション バーの **[My Applications]** で、ファイル ハンドラー アプリの名前を選択します。

5. **[Register Add-In]** をクリックします。

6. **[Register Add-In]** ダイアログで、**[File Handler]** を選択します。

7. **[File Handler Add-In]** のドロップダウンをクリックします。

8. ファイル ハンドラーの詳細を入力します。注 プロトコルは HTTPS にする必要があります。 ファイル ハンドラーの詳細を入力します。**注** プロトコルは HTTPS にする必要があります。

9. **[Update Add-In]** をクリックします。   

<a name="sectionSection103"> </a>
## ファイル ハンドラーのテスト

アプリケーションをテストするには、カスタム ファイル タイプを使用するいくつかのサンプル ファイルを SharePoint サイトにアップロードします。ドキュメント ライブラリを表示したときに、カスタム ファイル アイコンに指定したイメージとともにこれらのファイルが表示されるはずです。 アプリケーションをテストするには、カスタム ファイル タイプを使用するいくつかのサンプル ファイルを SharePoint サイトにアップロードします。ドキュメント ライブラリを表示したときに、カスタム ファイル アイコンに指定したイメージとともにこれらのファイルが表示されるはずです。
If you make add-in metadata changes in Azure Active Directory, you can see the changes in Office 365 by refreshing your browser.

<a name="sectionSection100"> </a>


<a name="sectionSection101"> </a>
