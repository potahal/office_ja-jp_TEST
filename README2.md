# <a name="office-add-in-that-uses-the-auth0-service-to-simplify-social-login"></a>Auth0 サービスを使用してソーシャル ログインを簡略化する Office アドイン

The Auth0 service simplifies the process of using social login provided by online services such as Facebook, Google, and Microsoft. This sample shows how to use Auth0 in an Office add-in. 

## <a name="table-of-contents"></a>目次
* [変更履歴](#change-history)
* [前提条件](#prerequisites)
* [プロジェクトを構成する](#configure-the-project)
* [Auth0 アカウントを作成し、Google、Facebook および Microsoft のアカウントを使用するように構成する](#create-an-auth0-account-and-configure-it-to-use-google,-facebook,-and-microsoft-account)
* [サンプル コードに Auth0 アカウントの値を追加する](#add-your-auth0-account-values-to-the-sample-code)
* [アドインを展開する](#deploy-the-add-in)
* [プロジェクトを実行する](#run-the-project)
* [アドインを起動する](#start-the-add-in)
* [アドインをテストする](#test-the-add-in)
* [質問とコメント](#questions-and-comments)
* [その他の技術情報](#additional-resources)

## <a name="change-history"></a>変更履歴

2016 年 9 月 6 日:

* 初期バージョン。

## <a name="prerequisites"></a>前提条件

* [Auth0](https://auth0.com) を使用したアカウント
* Windows 用の Word 2016 (16.0.6727.1000 以降のビルド)。
* [Node and npm](https://nodejs.org/en/) The project is configured to use npm as both a package manager and a task runner. It is also configured to use Lite Server as the web server that will host the add-in during development, so you can have the add-in up and running quickly. You are welcome to use another task runner or web server.
* [Git バッシュ](https://git-scm.com/downloads) (またはその他の Git クライアント。)

## <a name="configure-the-project"></a>プロジェクトを構成する

プロジェクトを配置するフォルダーで、Git バッシュ シェルで次のコマンドを実行します。

1. ローカル コンピューターにこのリポジトリのクローンを作成する ```git clone {URL of this repo}```
2. package.json ファイル内のアイテム化されたすべての依存関係をインストールする ```npm install```。
3. このサンプルを実行するために必要な証明書を作成する ```bash gen-cert.sh```。 

Set the certificate to be a trusted root authority. On a Windows machine, these are the steps:

1. ローカル コンピューターにあるリポジトリ フォルダーで、ca.crt をダブルクリックし、**[証明書のインストール]** を選択します。 
2. **[ローカル コンピューター]** を選択して、**[次へ]** を選択して続行します。 
3. **[証明書をすべて次のストアに配置する]** を選択してから **[参照]** を選択します。
4. **[信頼されたルート証明機関]** を選択して、**[OK]** を選択します。 
5. **[次へ]**、**[完了]** の順に選択します。 

## <a name="create-an-auth0-account-and-configure-it-to-use-google,-facebook,-and-microsoft-account"></a>Auth0 アカウントを作成し、Google、Facebook および Microsoft のアカウントを使用するように構成する

UI に関する前提事項をできる限り少なくするようにしていますが、必要な場合、実行する必要のある事項の要点を理解するためにこれらの手順を確認し、それから手順に関する Auth0 のヘルプを確認していただけます。

1. In your Auth0 dashboard create an account (or you can use an existing account). You will be prompted to choose and account name which will serve as the subdomain in auth0.com with which your add-in will interact; for example, `officeaddin.auth0.com`. Make a note of this name.
2. When you are prompted to choose providers, select Facebook, Google, and Microsoft Account. This sample doesn't use any others, so disable any others that are enabled by default, including the **Database** (or **Username-Password-Authentication**) option. You can change this setting later, if you want to extend the sample to other providers.
3. Auth0 creates a **Default App** (also called a **Client**) in the account. Navigate to the **Settings** for this app.
4. 後の手順で使用するので、クライアント ID を書き留めておきます。
5. **[クライアントの種類]** に、**[単一ページ アプリケーション]** を選択します。 
6. **[許可されているコールバック]** で、`https://localhost:3000/popupRedirect.html` を入力します。
7. 他のすべての設定は既定値のままにしておき、**[変更の保存]** をクリックします。

## <a name="add-your-auth0-account-values-to-the-sample-code"></a>サンプル コードに Auth0 アカウントの値を追加する

1. index.js ファイルを開き、上部付近にある次の行を探します。
```
Auth0AccountData.subdomain = '{Auth0 account subdomain}';
Auth0AccountData.clientID = '{Auth0 client ID}';
```
2. プレースホルダーを前の手順で記録した適切な文字列に置き換えます。

## <a name="deploy-the-add-in"></a>アドインを展開する

次に、Microsoft Word がアドインを検索する場所を認識できるようにする必要があります。

1. ネットワーク共有を作成するか、[フォルダーをネットワークに共有します](https://technet.microsoft.com/en-us/library/cc770880.aspx)。
2. プロジェクトのルートから、Office-Add-in-Auth0.xml マニフェスト ファイルのコピーを共有フォルダーに配置します。
3. Word を起動し、ドキュメントを開きます。
4. [**ファイル**] タブを選択し、[**オプション**] を選択します。
5. [**セキュリティ センター**] を選択し、[**セキュリティ センターの設定**] ボタンを選択します。
6. **[信頼されているアドイン カタログ]** を選択します。
7. **[カタログの URL]** フィールドに、Office-Add-in-Auth0.xml があるフォルダー共有へのネットワーク パスを入力して、**[カタログの追加]** を選択します。
8. **[メニューに表示する]** チェック ボックスをオンにし、**[OK]** を選択します。
9. これらの設定が Microsoft Office を次回起動したときに適用されることを示すメッセージが表示されます。Word を終了して、再起動します。

## <a name="run-the-project"></a>プロジェクトを実行する

1. プロジェクトのフォルダー内でノード コマンド ウィンドウを開き、```npm start``` を実行して Web サービスを開始します。
2. Open Internet Explorer or Edge and enter ```https://localhost:3000``` in the address box. If you do not receive any warnings about the certificate, close the browser and continue with the section below titled **Start the add-in**. If you do receive a warning that the certificate is not trusted, continue with the following steps:
3. 警告があっても、ブラウザーにはページを開くためのリンクが表示されます。
4. ページが開いたら、アドレス バーに赤い証明書エラーが表示されます。
5. **[証明書の表示]** を選択します。
5. **[証明書のインストール]** を選択します。
4. **[ローカル コンピューター]** を選択して、**[次へ]** を選択して続行します。 
3. **[証明書をすべて次のストアに配置する]** を選択してから **[参照]** を選択します。
4. **[信頼されたルート証明機関]** を選択して、**[OK]** を選択します。 
5. **[次へ]**、**[完了]** の順に選択します。
6. ブラウザーを閉じます。

## <a name="start-the-add-in"></a>アドインを起動する

1. Word を再起動して、Word 文書を開きます。
2. Word 2016 の**[挿入]** タブで、**[マイ アドイン]** を選択します。
3. **[共有フォルダー]** タブを選択します。
4. **[Auth0 を使用して認証する]** を選択して、**[OK]** を選択します。
5. ご使用の Word バージョンでアドイン コマンドがサポートされている場合、UI によってアドインが読み込まれたことが通知されます。
6. [ホーム] では、リボンは **Auth0** と呼ばれる新しいグループであり、**[表示する]** というラベル付きのボタンとアイコンが用意されています。

 > 注:アドイン コマンドが Word バージョンによってサポートされていない場合は、アドインが作業ウィンドウに読み込まれます。

## <a name="test-the-add-in"></a>アドインをテストする

1. The add-in opens with a Welcome page. Click the **Sign In** button.
2. ポップアップが開き、ID プロバイダーを選択するように求めるメッセージが表示されます。 
3. If you are not already signed in with that provider, the provider's sign-in page opens. (After you sign in the first time, you will be prompted to grant Auth0 permission to your profile.) After you are signed in, the dialog closes and the task pane shows the main working page of the add-in. (If you are already signed-in with the provider, the dialog closes immediately after you click the provider's button.)
4. Click the **Insert User Name** button. The your user name is inserted into the Word document.

## <a name="questions-and-comments"></a>質問とコメント

Word SpecKit サンプルについて、Microsoft にフィードバックをお寄せください。このリポジトリの「*問題*」セクションにフィードバックを送信できます。

Microsoft Office 365 開発全般の質問につきましては、「[スタック オーバーフロー](http://stackoverflow.com/questions/tagged/office-js+API)」に投稿してください。質問には、必ず [office-js] と [API] のタグを付けてください。

## <a name="additional-resources"></a>その他の技術情報

* [Office アドインのドキュメント](https://msdn.microsoft.com/en-us/library/office/jj220060.aspx)
* [Office デベロッパー センター](http://dev.office.com/)
* [Github の OfficeDev](https://github.com/officedev) にあるその他の Office アドイン サンプル

## <a name="copyright"></a>著作権
Copyright (c) Microsoft. All rights reserved.

