---
ms.Toctitle: Build service and daemon apps in Office 365
title: "Office 365 でサービス アプリおよびデーモン アプリを構築する"
description: "Office 365 では、OAuth2 クライアント資格情報の付与フローの使用をサポートしています。そのため、デーモン アプリまたはサービス Web アプリはユーザーのサインインを必要とせずにアプリ専用トークンを要求できます。"
ms.ContentId: 14967061-1dfb-4d4b-9668-a7ac60343a55
ms.date: May 19, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 でサービス アプリおよびデーモン アプリを構築する

_**適用対象:**Office 365_

デバイス アプリケーションや Web サーバー アプリケーションでは、通常、ユーザーはサインインして、それらのアプリケーションが Office 365 の情報にアクセスしたり、使用したりすることに同意する必要があります。 アクセス権を得るための下層プロトコルは、[OAuth2 認証コードの付与フロー](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)に基づきます。 この OAuth2 フローの一部として、アプリケーションはアクセス トークンと更新トークンのペアを取得します。これは、一連のアクセス許可について個々のユーザーがアプリケーションに委任を付与したことを表します。 原則的に、アプリケーションはユーザーのデータにアクセスする前に、各ユーザーのアクセス トークンと更新トークンを取得する必要があります。それらを取得するには、ユーザーはアプリケーションに少なくとも 1 回はサインオンする必要があります。

デーモン アプリやサービス アプリなど、バック グラウンドで実行されるアプリケーションで、これを実行するこはできません。 これらのアプリは、ユーザーのサインインなしにアクセスできる必要があります。 OAuth2 では、こうした種類のアプリケーションに対して[クライアント資格情報の付与フロー](https://msdn.microsoft.com/en-us/library/azure/dn645543.aspx)を提供します。Office 365 は、このフローの使用をサポートしています。

このフローを使用する場合、アプリはクライアントの資格情報を OAuth2 トークン発行エンドポイントに提示し、代わりに、ユーザー情報なしにアプリケーション自体を表すアクセス トークンを取得します。このシナリオのトークンは、アプリ専用のアクセス トークンとして知られています。

アクセス トークンの有効期限が切れたときに、アプリは更新トークンを取得する必要はありません。OAuth2 トークン発行エンドポイントに戻って、新しいトークンを取得するだけで済みます。また、アプリ専用トークンにはユーザー情報がないため、このトークンを使用するときに、アプリは API 呼び出し内でユーザーを指定する必要があります。


## アクセス許可の定義
アプリ専用トークンに対してアプリを構成するには、まずアプリに必要なアクセス許可を指定する必要があります。Microsoft Azure Active Directory (Azure AD) の Azure 管理ポータルでのアプリのアプリケーション登録で、これらのアクセス許可を指定します。アプリケーションのアクセス許可の種類を選択します。これらはアプリケーションに直接割り当てられます。Azure AD のアプリケーション登録のアクセス許可セクションのスクリーン ショットについては、図 1 を参照してください。

**図 1. Azure AD でのアプリケーション アクセス許可の構成**
![Azure 管理コンソールでのアプリケーションのアクセス許可を示すスクリーン ショット。](images\O365_BuildingSvcApps_AppPerms.png)

**注:** Azure AD でアプリを登録するときに、**[Web アプリケーション] または [Web API]** タイプを選択する必要があります。アプリ専用アクセス許可は、ネイティブ クライアント アプリケーションでは使用できません。

## 同意とアプリ認証強度 
アクセス許可をアプリ登録内で指定すると、アプリを別の Office 365 組織で使用できるように同意を求めることができます。アプリのアクセス許可には、Office 365 テナント管理者による同意が必要です。これは、Office 365 組織内でアクセスできるデータの観点から、これらのアプリケーションにとって非常に大きなメリットになるためです。たとえば、あるサービス アプリケーションがクライアント資格情報フローを使用してアクセス トークンを取得する Mail.Read アクセス許可を持つ場合、Office 365 組織内の任意のメールボックスでメールを読み取ることができます。

これらのアプリケーションで使用できるアクセスが広範囲に及ぶため、アクセス トークンを正常に取得するために、アプリは X.509 証明書を公開/秘密キーのペアと共に使用する必要もあります。単純な対称キーの使用は許可されていません。アプリは対称キーを使用してアクセス トークンを取得できますが、API はこれらのアクセス トークンに対してアクセス拒否エラーを返します。自己発行の X.509 証明書は受け入れられますが、アプリは公開 X.509 証明書を Azure AD のアプリケーション定義に登録する必要があります。アプリは秘密キーを保持し、トークン発行エンドポイントに自身の ID をアサートするためにそのキーを使用します。

OAuth2 承認エンドポイントに要求を送信することで、認証コードの付与フローと類似した方法で同意フローを実装します。ただし、承認エンドポイントが認証コードを使用してアプリへリダイレクトを戻すと、残りのコードは無視されます。トークンの取得には、クライアントの資格情報のみが必要になります。初回 1 回限りの同意を実現するために、アプリで "所属組織へのサインアップ" のようなエクスペリエンスを構築できます。ここで重要なのは、デーモン アプリまたはサービス アプリの構成で 1 回限りの同意部分を作成する必要があるということです。同意が与えられると、アプリはアクセス トークンを取得して Office 365 API の呼び出しを開始できます。
### 同意の取り消し
Office 365 テナント管理者によってインストールされたその他のアプリケーションと同じように、サービス アプリケーションに対する同意を取り消すことができます。 管理者は、[Azure 管理ポータル](https://manage.windowsazure.com/)に移動して [アプリケーション] ビューでアプリケーションを見つけ、そのアプリケーションを選択して削除するか、Windows PowerShell の Azure AD モジュールから [Remove-MSOLServicePrincipal コマンドレット](https://msdn.microsoft.com/en-us/library/azure/dn194113.aspx)を使用できます。
## クライアント資格情報フローを使用してアクセス トークンを取得する
アプリ専用トークンの要求時に、テナント固有のトークン発行エンドポイントを使用する必要があります。 共通の (テナントに依存しない) エンドポイントを使用すると、エラーが返されます。

同意プロセス中に、承認エンドポイントがヒットし、コードがアプリへのリダイレクトで配信される場合、アプリは ID トークンをコードと一緒に要求できます。このトークンは **tid** クレーム タイプ (要求の種類) のテナント ID を含む JSON Web トークンであるため、このトークンを解析するだけでテナント固有のエンドポイントの ID を取得できます。
### 同意による最初の ID トークンの取得
次に、[OpenID Connect ハイブリッド フロー](http://openid.net/specs/openid-connect-core-1_0.html#HybridFlowAuth)要求を使用して同意をトリガーする方法の例を示します。

    GET https://login.microsoftonline.com/common/oauth2/authorize?state=e82ea723-7112-472c-94d4-6e66c0ca52b6&response_type=code+id_token&scope=openid&nonce=c328d2df-43d1-4e4d-a884-7cfb492beadc&client_id=0308CDD9-874D-4F87-85E0-A0DA7E05F999&redirect_uri=https:%2f%2flocalhost:44304%2fHome%2f&resource=https:%2f%2fgraph.windows.net%2f&prompt=admin_consent&response_mode=form_post HTTP/1.1

この要求は、アプリに同意フローを提供し、POST 要求でコードと ID トークンのリダイレクトを戻します。アプリはコードを無視して、受信したフォーム データから ID トークンを取得できます。フォームの POST 応答モードの詳細については、「[OpenID の仕様](http://openid.net/specs/oauth-v2-form-post-response-mode-1_0-02.html#FormPostResponseExample)」を参照してください。

ASP.Net では、リダイレクト ページの Request.Form["id_token"] を使用して、ID トークンを取得できます。テナント ID は **id_token** 内の **tid** クレームで見つけます。

##アプリ専用トークンの取得

次のコード スニペットは、クライアント資格情報のフローを使用してアプリ専用トークンを取得するために Azure AD クライアント ライブラリを使用する方法を示しています。

```cs
//need to address the tenant specific authority/token issuing endpoint
//https://login.microsoftonline.com/<tenant-id>/oauth2/authorize
//retrieved tenantID from ID token for the app during consent flow (authorize flow)
string authority = appConfig.AuthorizationUri.Replace("common", tenantId);
AuthenticationContext authenticationContext = new AuthenticationContext(authority,false);
string certfile = Server.MapPath(appConfig.ClientCertificatePfx);
X509Certificate2 cert = new X509Certificate(certfile,appConfig.ClientCertificatePfxPassword, X509KeyStorageFlags.MachineKeySet);
ClientAssertionCertificate cac = new ClientAssertionCertificate(appConfig.ClientId, cert);
var authenticationResult = await authenticationContext.AcquireTokenAsync(resource, cac);
return authenticationResult.AccessToken;
```
 
この例のアプリケーションでは、Web サーバー上で適切に保護されているディレクトリで秘密キーを PFX ファイルとして保持します。ただし、コンピューター アカウントの Windows 証明書ストレージなど、より安全なストレージで秘密キーを保持することをお勧めします。クライアント資格情報のフローを使用する Web アプリの完全なサンプルについては、[o365api-as-apponly-webapp のサンプル](https://github.com/mattleib/o365api-as-apponly-webapp)を参照してください。

##アプリケーション用の X.509 公開証明書の構成

最後の部分は、アプリ用の X.509 公開証明書の構成方法です。これは Azure 管理ポータル UI では公開されていないため、アプリ マニフェストを手動で編集することによってこれを構成する必要があります。これを実行するには、以下を行う必要があります。

- X.509 証明書がない場合は、自己発行の証明書を作成します。

- Windows PowerShell を使用して .cer x509 公開証明書ファイルから、base64 でエンコードされた証明書の値と拇印を取得します。

- アプリ マニフェスト ファイルで証明書をアップロードします。
    
  [.NET Framework ツール](https://msdn.microsoft.com/en-us/library/d9kh6s92.aspx)に含まれている [makecert.exe ツール](https://msdn.microsoft.com/en-us/library/bfsktky3.aspx)を使用すると、自己発行の証明書を生成できます。

###makecert.exe ツールを使用して自己発行の証明書を生成するには

1. コマンド ラインから次を入力して実行します。

    `makecert -r -pe -n "CN=MyCompanyName MyAppName Cert" -b 12/15/2014 -e 12/15/2016 -ss my -len 2048`

2. **[証明書]** 管理コンソール スナップインを開き、自分のユーザー アカウントに接続します。

3. **[個人用]** フォルダーで新しい証明書を見つけ、その証明書を base64 でエンコードされた CER ファイルにエクスポートします。

    **注**: X.509 証明書を生成する際、キーの長さが 2048 以上になっていることを確認します。 キーの長さがそれより短い場合は、有効なキーとして受け入れられません。

###.cer x509 公開証明書ファイルから、base64 でエンコードされた証明書の値と拇印を取得するには

1. Windows PowerShell プロンプトから次のコマンドレットを入力して実行します。

    ```
$cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$cer.Import("mycer.cer")
$bin = $cer.GetRawCertData()
$base64Value = [System.Convert]::ToBase64String($bin)
$bin = $cer.GetCertHash()
$base64Thumbprint = [System.Convert]::ToBase64String($bin)
$keyid = [System.Guid]::NewGuid().ToString()
```

    **注**: この手順では、Windows PowerShell を使用して x.509 証明書のプロパティを取得する方法を示しています。 その他のプラットフォームには、証明書のプロパティを取得する同様のツールがあります。

2. **$base64Thumbprint**、**$base64Value**、および **$keyid** の値を保存します。これらの値は、次の一連の手順で証明書をアップロードするときに必要になります。

###マニフェスト ファイルで証明書をアップロードするには

1. [Azure 管理ポータル](https://manage.windowsazure.com/)にサインインします。

2. 左側のメニューで **[Active Directory]** をクリックしてから、目的のディレクトリをクリックします。

3. トップ メニューで **[アプリケーション]** をクリックしてから、構成するアプリケーションをクリックします。**[クイック スタート]** ページがシングル サインオンとその他の構成情報と共に表示されます。

4. コマンド バーの **[マニフェストの管理]** をクリックして、**[マニフェストのダウンロード]** を選択します。

5. 編集用にダウンロードしたファイルを開き、空の **KeyCredentials** プロパティを次の JSON で置き換えます。

    ```json
"keyCredentials": [
    {
      "customKeyIdentifier": "$base64Thumbprint_from_above",
      "keyId": "$keyid_from_above",
      "type": "AsymmetricX509Cert",
      "usage": "Verify",
      "value":  "$base64Value_from_above"
     }
   ], 
```

  例:

  ```json
"keyCredentials": [
    {
      "customKeyIdentifier": "ieF43L8nkyw/PEHjWvj+PkWebXk=",
      "keyId": "2d6d849e-3e9e-46cd-b5ed-0f9e30d078cc",
      "type": "AsymmetricX509Cert",
      "usage": "Verify",
      "value": "MIICWjCCAgSgAwIBA***omitted for brevity***qoD4dmgJqZmXDfFyQ"
    }
  ],
```

  
  [KeyCredentials](http://msdn.microsoft.com/en-us/library/azure/dn151681.aspx) プロパティは、ロール オーバー シナリオでは複数の X.509 証明書のアップロードを可能にする、または漏えいシナリオでは証明書を削除することを可能にするコレクションです。

6.  変更内容を保存します。コマンド バーの **[マニフェストの管理]** をクリックして **[マニフェストのアップロード]** を選択し、更新したマニフェスト ファイルを参照してそのファイルを選択することで、更新済みアプリのマニフェスト ファイルをアップロードします。


  
  
  

