---
ms.Toctitle: Understand authentication
title: "Office 365 アプリ認証の概念"
description: "ms.TocTitle: 認証についてTitle: Office 365 アプリ認証の概念Description: Azure AD、ユーザー、および Office 365 API エンドポイントの間のアプリ承認フロー、ユーザーの同意、アクセス コード、トークン、およびスコープの役割について理解します。ms.ContentId: aff11bd0-62ae-4579-a459-faaa236eec26 ms.topic: 記事 (方法)"
ms.ContentId: aff11bd0-62ae-4579-a459-faaa236eec26
ms.date: October 6, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 API を使用した認証について

_**適用対象:** Office 365_

使用するアプリでユーザーの Office 365 データにアクセスできるようにするために、まずアプリは、それが用途にあったアプリであることと、ユーザーのデータへのアクセス許可を持っていることを証明する必要があります。このトピックでは、Office 365 サービスを使用してこの認証プロセスを実装する方法について概説し、アプリの認証処理について詳しく説明したリソースへのリンクを紹介しています。


## 認証の概念

承認について説明する前に、いくつかの用語を理解しておく必要があります。

- 認証: アプリケーションまたはユーザーの ID を検証することを指します。
    
     All user accounts, application registrations, and permissions are stored in Azure AD. This allows you to provide logon credentials and be confirmed as a valid user. 

    認証の詳細については、「Azure AD の認証シナリオ」を参照してください。

- 承認: とは、リソースへのアクセスを付与し、アクセス許可のレベルを指定するプロセスです。 
    
    For example, your application might need access to list a user's calendar events. When you specify  **Read** permission to the calendar, the application will have permission to generate a list of the user's calendar events.

    For more information about authorization, see  [Use Active Directory for authentication in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-authentication-authorization/)

- 同意: 承認と同様に、ユーザーまたはアプリケーションに対して特定のリソースを使用するためのアクセス許可を付与することを指します。これは、ユーザーが特定のリソースへのアクセスを許可または拒否する機会になります。 承認と同様に、同意がなされると、ユーザーまたはアプリケーションに対して、それが特定のリソースを使用するためのアクセス許可が付与されます。これは、ユーザーが特定のリソースへのアクセスを許可または拒否する機会となります。
    
    An example is when a user installs an app. The consent framework will ask the user if they want to allow the app to access their mailbox. The user will see the request for the app to access their mailbox in a dialog box. The user agrees or declines the request.

##Office 365 では認証に Azure AD を使用する

Office 365 API サービスは Azure Active Directory (Azure AD) を使用して、ユーザーの Office 365 データにセキュリティで保護された認証と承認を提供します。Azure AD は [OAuth 2.0 プロトコル](http://tools.ietf.org/html/rfc6749)に従って、承認フローを実装します。

このため、アプリを認証して Office 365 データにアクセスできるようにする方法は、次に示す 2 つの基本的な手順で構成されます。

- Azure AD にアプリを登録する
- 適切な認証フローを処理するコードをアプリに実装する 

### アプリを Azure AD に登録して Office 365 API サービスにアクセスする
<a name="bk_register"> </a>

Office 365 API サービスへのアクセスが必要なアプリケーションを作成するときには、アプリケーションがサービスへのアクセス権を保持しているかを確認する方法をサービスに提供する必要があります。Office 365 API では Azure AD を使用してサービスへのアクセス権をアプリケーションに付与するための認証サービスを提供します。 

アプリケーションが Office 365 API にアクセスできるようにするには、[アプリケーションを Azure AD に登録する](..\howto\add-common-consent-manually.md)必要があります。アプリを登録することにより、次の操作が可能になります。

- Establish an identity for your application. アプリケーションの ID を確立します。これには Azure 内でのアプリケーションの一意の識別子であるクライアント ID が含まれます。
- Azure が認証の際にリダイレクトに使用する、アプリのエンドポイントを指定します。
- アクセスする必要があるリソースと、それらのリソースにアクセスするためのアクセス許可レベル (またはスコープ) の設定を指定します。 
    
    For a complete list of available Office 365 API scopes, see [Office 365 API permissions reference](..\howto\application-manifest.md).


## 認証フローの概要
<a name="bk_authenticate"> </a>

Azure AD では、アプリケーションから Office 365 リソースへのアクセスの承認に [OAuth 2.0 プロトコル](http://tools.ietf.org/html/rfc6749)を使用します。OAuth 2.0 プロトコルは、いくつかの認証フローを定義します。MVC または Webフォーム ソリューションなどの Web アプリケーションでも、スマート フォンまたはモバイル デバイスなどのネイティブ アプリケーションでも、実装するフローは作成するアプリの種類によって異なります。

はじめに、一般的な認証フローについて以下に概説します。このケース (認証コードの付与フロー) は Web アプリケーションに対してよく使用されます。

アプリの認証を正常に処理するには、アプリの種類に適した認証フローを理解しておく必要があります。Azure AD で OAuth 2.0 を実装する方法の詳細については、「Azure AD での OAuth 2.0」を参照してください。 Azure AD で OAuth 2.0 を使用する方法の詳細については、「Azure AD での OAuth 2.0」を参照してください。

### 例: 認証コード付与のフロー
<a name="sectionSection4"> </a>


|**説明**|**フロー**|
|:-----|:-----|
|ユーザーは、Office 365 からの情報を必要とする Web アプリを閲覧します。アプリにより、Azure AD 認証エンドポイントにリダイレクトされます。 ユーザーは、Office 365 からの情報を必要とする Web アプリを閲覧します。アプリにより、Azure AD 認証エンドポイントにリダイレクトされます。|![ユーザーがアプリケーションを呼び出し、アプリケーションが Azure AD の認証とトークン発行エンドポイントを呼び出します。](images\O365APIs_AuthFlow_Step1.png)|
|<p>ユーザーは認証し、アプリケーションが制限付きのアクセス権で構成されている場合は、同意を与えます。Azure AD は認証コードを発行します。</p><p>ユーザーが認証して同意します。Azure AD が認証コードを発行します。 Authorization codes are used to request access codes for specific resources.</p> |![ユーザーが同意したら、Azure AD が認証コードをアプリに発行します。](images\O365APIs_AuthFlow_Step2.png)|
|<p>アプリケーションに認証コードが渡されると、アプリケーションはアクセス トークンと更新トークンを要求できるようになります。 アプリは、Azure AD エンドポイントにその認証コードを渡します。Azure AD はアクセス トークンと更新トークンを返します。</p><p>Azure AD returns access and refresh tokens.</p>|![アプリが認証コードを Azure AD に渡し、Azure AD がアクセス トークンと更新トークンをアプリに返します。](images\O365APIs_AuthFlow_Step3.png)|
|<p>アプリは、アクセス トークンと更新トークンを使用して、Office 365 API エンドポイントにアクセスしたり、データを返したりできるようになります。</p><p>Your app can then present these tokens, on behalf of the user, to the Office 365 API service(s). The specified resource then returns the information requested by the application.</p> |![次に、アプリはアクセス トークンと更新トークンを使用して、Office 365 API エンドポイントにアクセスします。](images\O365APIs_AuthFlow_Step4.png)|


アプリケーションが実行する Azure AD への呼び出しの数を減らすため、またユーザーが行う同意の数を減らすために、承認サービスによって複数リソース更新トークンが発行されます。この更新トークンを受信した後、それを使用して、複数のリソースへのアクセス トークン (および追加の更新トークン) を要求できます。 

更新トークンの詳細については、「複数のリソース用の更新トークン」を参照してください。


## Office 365 OAuth サンドボックス内での OAuth 2.0 の動作を調べる
<a name="sectionSection5"> </a>

OAuth 2.0 が Office 365 REST API を使用する方法を調べたい場合は、アプリケーションが Azure AD で認証する際に渡されるメッセージよびトークンを確認できる Web アプリケーション ([Office 365 OAuth サンドボックス](https://oauthplay.azurewebsites.net/)) が用意されています。Office 365 API をサンプルで呼び出し、返されるデータを確認することもできます。サンドボックスの使用に関する詳細については、「[Office 365 の予定表、連絡先、およびメールとの OAuth2 の連動](http://blogs.msdn.com/b/exchangedev/archive/2014/10/28/oauth2-in-action-with-the-release-of-office-365-calendar-contacts-and-mail.aspx)」を参照してください。

## 承認を実装する Office 365 API コードのサンプル

GitHub の [Office 開発リポジトリ](https://github.com/OfficeDev) には、Azure AD を使用して Office 365 API にアクセスするコードのサンプルが数多く含まれています。一部を次に示します。

- [Office 365 iOS Connect のサンプル](https://github.com/OfficeDev/O365-iOS-Connect) 
- [Office 365 Android Connect のサンプル](https://github.com/OfficeDev/O365-Android-Connect)
- [Office 365 Windows Connect のサンプル](https://github.com/OfficeDev/O365-Win-Connect)

さらに、「[Azure Active Directory 開発者ガイド](http://aka.ms/aaddev)」には、クイック スタートや Azure AD 認証を [iOS](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-ios/)、[Android](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-android/)、[.NET](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-dotnet/)、[Xamarin](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-xamarin/)、[Cordova](https://azure.microsoft.com/en-us/documentation/articles/active-directory-devquickstarts-cordova/) など各種の開発プラットフォームに実装する方法の説明が含まれています。

## 予定表、連絡先
<a name="ConNavExample_resources"> </a>

-  [Azure Active Directory 開発者用ガイド](http://aka.ms/aaddev)

-  [Office 365 OAuth サンドボックス](https://oauthplay.azurewebsites.net/)

- [Office 365 の予定表、連絡先、およびメールとの OAuth2 の連動](http://blogs.msdn.com/b/exchangedev/archive/2014/10/28/oauth2-in-action-with-the-release-of-office-365-calendar-contacts-and-mail.aspx)
    
-  [Office 開発リポジトリ](https://github.com/OfficeDev) 
    
-  [Azure AD Authentication Library for .NET ](http://msdn.microsoft.com/en-us/library/jj573266.aspx)
    

    
