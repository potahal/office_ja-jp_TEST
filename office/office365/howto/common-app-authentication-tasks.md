---
ms.Toctitle: Understand authentication
title: "Office 365 アプリ認証の概念"
description: "Azure AD、ユーザー、Office 365 API エンドポイントの間のアプリ承認フロー、ユーザーの同意、アクセス コード、トークン、スコープの役割について理解します。"
ms.ContentId: aff11bd0-62ae-4579-a459-faaa236eec26
ms.date: October 6, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 API を使用した認証について

_**適用対象:**Office 365_

使用するアプリでユーザーの Office 365 データにアクセスできるようにするために、まずアプリは、それが用途にあったアプリであることと、ユーザーのデータへのアクセス許可を持っていることを証明する必要があります。このトピックでは、Office 365 サービスを使用してこの認証プロセスを実装する方法について概説し、アプリの認証処理について詳しく説明したリソースへのリンクを紹介しています。


## 認証の概念

承認について説明する前に、いくつかの用語を理解しておく必要があります。

- _認証_: アプリケーションまたはユーザーの ID を検証することを指します。
    
     すべてのユーザー アカウント、アプリケーションの登録、およびアクセス許可は、Azure AD に格納されます。 これにより、ユーザーがログオン資格情報を提示すれば、有効なユーザーとして確認することができます。 

    認証の詳細については、「[Azure AD の認証シナリオ](http://msdn.microsoft.com/en-US/library/azure/dn499820.aspx)」を参照してください。

- _承認_: リソースへのアクセス権を付与し、アクセス許可のレベルを指定するプロセスです。 
    
    たとえば、アプリケーションには、ユーザーの予定表イベントを一覧表示するためのアクセス権が必要であるとします。 予定表への**読み取り**アクセス許可を指定すると、アプリケーションにはユーザーの予定表イベントの一覧を生成するためのアクセス許可が付与されます。

    承認の詳細については、「[Azure App Service での認証には、Active Directory を使用します](https://azure.microsoft.com/en-us/documentation/articles/web-sites-authentication-authorization/)」を参照してください。

- _同意_: 承認と同様に、特定のリソースを使用するためのアクセス許可をアプリケーションまたはユーザーに付与することを指します。 これにより、_ユーザー_は特定のリソースへのアクセスを許可または拒否する機会が得られます。
    
    その一例として、ユーザーがアプリをインストールする場合が挙げられます。 同意のフレームワークにより、ユーザーは、アプリがメールボックスにアクセスすることを許可するかどうかを尋ねられます。 アプリによるユーザーのメールボックスへのアクセスの要求が、ダイアログ ボックスでユーザーに表示されます。 ユーザーは要求に同意するか、拒否します。

##Office 365 では認証に Azure AD を使用する

Office 365 API サービスは Azure Active Directory (Azure AD) を使用して、ユーザーの Office 365 データにセキュリティで保護された認証と承認を提供します。Azure AD は [OAuth 2.0 プロトコル](http://tools.ietf.org/html/rfc6749)に従って、承認フローを実装します。

このため、アプリを認証して Office 365 データにアクセスできるようにする方法は、次に示す 2 つの基本的な手順で構成されます。

- Azure AD にアプリを登録する
- 適切な認証フローを処理するコードをアプリに実装する 

### アプリを Azure AD に登録して Office 365 API サービスにアクセスする
<a name="bk_register"> </a>

Office 365 API サービスへのアクセスが必要なアプリケーションを作成するときには、アプリケーションがサービスへのアクセス権を保持しているかを確認する方法をサービスに提供する必要があります。Office 365 API では Azure AD を使用してサービスへのアクセス権をアプリケーションに付与するための認証サービスを提供します。 

アプリケーションが Office 365 API にアクセスできるようにするには、[アプリケーションを Azure AD に登録する](..\howto\add-common-consent-manually.md)必要があります。アプリを登録することにより、次の操作が可能になります。

- アプリケーションの ID を確立します。 これには Azure 内でのアプリケーションの一意の識別子であるクライアント ID が含まれます。
- Azure が認証の際にリダイレクトに使用する、アプリのエンドポイントを指定します。
- アクセスする必要があるリソースと、それらのリソースにアクセスするためのアクセス許可レベル (つまり_スコープ_) の設定を指定します。 
    
    使用可能な Office 365 API のスコープの完全な一覧については、「[Office 365 API のアクセス許可リファレンス](..\howto\application-manifest.md)」を参照してください。


## 認証フローの概要
<a name="bk_authenticate"> </a>

Azure AD では、アプリケーションから Office 365 リソースへのアクセスの承認に [OAuth 2.0 プロトコル](http://tools.ietf.org/html/rfc6749)を使用します。OAuth 2.0 プロトコルは、いくつかの認証フローを定義します。MVC または Webフォーム ソリューションなどの Web アプリケーションでも、スマート フォンまたはモバイル デバイスなどのネイティブ アプリケーションでも、実装するフローは作成するアプリの種類によって異なります。

はじめに、一般的な認証フローについて以下に概説します。このケース ([認証コードの付与フロー](http://msdn.microsoft.com/en-US/library/azure/dn645542.aspx)) は、Web アプリケーションに頻繁に使用されます。

アプリの認証を正常に処理するには、アプリの種類に適した認証フローを理解しておく必要があります。 Azure AD で OAuth 2.0 を実装する方法の詳細については、「[Azure AD での OAuth 2.0](http://msdn.microsoft.com/en-us/library/azure/dn645545.aspx)」を参照してください。

### 例: 認証コード付与のフロー
<a name="sectionSection4"> </a>


|**説明**|**フロー**|
|:-----|:-----|
|ユーザーは、Office 365 からの情報を必要とする Web アプリを閲覧します。 アプリにより、Azure AD 認証エンドポイントにリダイレクトされます。|![ユーザーがアプリケーションを呼び出し、アプリケーションが Azure AD の認証エンドポイントを呼び出します。](images\O365APIs_AuthFlow_Step1.png)|
|<p>ユーザーは認証し、アプリケーションが制限付きのアクセス権で構成されている場合は、同意を与えます。</p><p>Azure AD は認証コードを発行します。 認証コードは、特定のリソースに対するアクセス コードを要求するために使用します。</p> |![ユーザーが同意したら、Azure AD が認証コードをアプリに発行します。](images\O365APIs_AuthFlow_Step2.png)|
|<p>アプリケーションに認証コードが渡されると、アプリケーションはアクセス トークンと更新トークンを要求できるようになります。 アプリは、Azure AD トークン発行エンドポイントに認証コードを渡します。</p><p>Azure AD はアクセス トークンと更新トークンを返します。</p>|![アプリは、認証コードを Azure AD に渡します。Azure AD はアクセス トークンと更新トークンをアプリに返します。](images\O365APIs_AuthFlow_Step3.png)|
|<p>アプリは、アクセス トークンと更新トークンを使用して、Office 365 API エンドポイントにアクセスしたり、データを返したりできるようになります。</p><p>アプリは、ユーザーの代わりに該当するトークンを Office 365 API サービスに提示します。 指定されたリソースは、アプリケーションが要求した情報を返します。</p> |![アプリはアクセス トークンと更新トークンを使用して、Office 365 API エンドポイントにアクセスします。](images\O365APIs_AuthFlow_Step4.png)|


アプリケーションが実行する Azure AD への呼び出しの数を減らすため、またユーザーが行う同意の数を減らすために、承認サービスによって複数リソース更新トークンが発行されます。この更新トークンを受信した後、それを使用して、複数のリソースへのアクセス トークン (および追加の更新トークン) を要求できます。 

更新トークンの詳細については、「[複数のリソース用の更新トークン](http://msdn.microsoft.com/en-us/library/azure/dn645538.aspx)」を参照してください。


## Office 365 OAuth サンドボックス内での OAuth 2.0 の動作を調べる
<a name="sectionSection5"> </a>

OAuth 2.0 が Office 365 REST API を使用する方法を調べたい場合は、アプリケーションが Azure AD で認証する際に渡されるメッセージよびトークンを確認できる Web アプリケーション ([Office 365 OAuth サンドボックス](https://oauthplay.azurewebsites.net/)) が用意されています。Office 365 API をサンプルで呼び出し、返されるデータを確認することもできます。サンドボックスの使用に関する詳細については、「[Office 365 の予定表、連絡先、およびメールとの OAuth2 の連動](http://blogs.msdn.com/b/exchangedev/archive/2014/10/28/oauth2-in-action-with-the-release-of-office-365-calendar-contacts-and-mail.aspx)」を参照してください。

## 承認を実装する Office 365 API コードのサンプル

GitHub の [Office 開発リポジトリ](https://github.com/OfficeDev)には、Azure AD を使用して Office 365 API にアクセスするコードのサンプルが数多く含まれています。一部を次に示します。

- [Office 365 iOS Connect のサンプル](https://github.com/OfficeDev/O365-iOS-Connect) 
- [Office 365 Android Connect のサンプル](https://github.com/OfficeDev/O365-Android-Connect)
- [Office 365 Windows Connect のサンプル](https://github.com/OfficeDev/O365-Win-Connect)

さらに、「[Azure Active Directory 開発者ガイド](http://aka.ms/aaddev)」には、クイック スタートや Azure AD 認証を [iOS](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-ios/)、[Android](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-android/)、[.NET](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-dotnet/)、[Xamarin](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-xamarin/)、[Cordova](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-cordova/) など各種の開発プラットフォームに実装する方法の説明が含まれています。

## その他のリソース
<a name="ConNavExample_resources"> </a>

-  [Azure Active Directory 開発者用ガイド](http://aka.ms/aaddev)

-  [Office 365 OAuth サンドボックス](https://oauthplay.azurewebsites.net/)

- [Office 365 の予定表、連絡先、およびメールとの OAuth2 の連動](http://blogs.msdn.com/b/exchangedev/archive/2014/10/28/oauth2-in-action-with-the-release-of-office-365-calendar-contacts-and-mail.aspx)
    
-  [Office 開発リポジトリ](https://github.com/OfficeDev) 
    
-  [.NET 用 Azure AD 認証ライブラリ ](http://msdn.microsoft.com/en-us/library/jj573266.aspx)
    

    
