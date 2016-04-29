
# Exchange のトークン検証ライブラリを使用する
EWS Managed API 検証ライブラリを使用して Exchange の ID トークンを検証する方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

Exchange Server 2013を実行するサーバーにアドインが要求する ID トークンを使用して、Outlook アドインのクライアントを識別できます。JSON Web トークンとしてフォーマットされたトークンは、Exchange サーバーの電子メール アカウントに一意の識別子を提供します。Exchange Web Services (EWS) Managed API には ID トークンの使用を簡素化するヘルパー クラスがあります。

## 検証ライブラリを使用する前提条件


Exchange の ID トークンを検証するには、EWS Managed API 認証ライブラリと Windows Identity Foundation (WIF) に加えて、JSON トークンのハンドラーによって WIF を拡張する DLL が必要です。次のリソースをダウンロードしてください。


- [Exchange Web Services Managed API](http://go.microsoft.com/fwlink/?LinkID=255472)
    
- [Windows Identity Foundation ](http://www.microsoft.com/ja-jp/download/details.aspx?id=17331)
    
- [Windows.IdentityModel.Extensions.dll (32 ビット アプリケーション用)](http://download.microsoft.com/download/0/1/D/01D06854-CA0C-46F1-ADBA-EBF86010DCC6/MicrosoftIdentityExtensions-32.msi)
    
- [Windows.IdentityModel.Extensions.dll (64 ビット アプリケーション用)](http://download.microsoft.com/download/0/1/D/01D06854-CA0C-46F1-ADBA-EBF86010DCC6/MicrosoftIdentityExtensions-64.msi)
    

## Exchange の ID トークンを検証する


EWS Managed API 検証ライブラリには、Exchange の ID トークンを管理する  **AppIdentityToken** クラスがあります。次のメソッドは、 **AppIdentityToken** インスタンスを作成し、 **Validate** メソッドを呼び出してトークンが有効であることを検証する方法を示します。


```
// Required to use the validation library.
using Microsoft.Exchange.WebServices.Auth.Validate;

        private AppIdentityToken CreateAndValidateIdentityToken(string rawToken, string hostUri)
        {
            try
            {
                AppIdentityToken token = (AppIdentityToken)AuthToken.Parse(rawToken);
                token.Validate(new Uri(hostUri));

                return token;
            }
            catch (TokenValidationException ex)
            {
                throw new ApplicationException("A client identity token validation error occurred.", ex);
            }
        }

```


## その他のリソース



- [Exchange の ID トークンを使用して Outlook アドインを認証する](../outlook/authentication.md)
    
- [Exchange の ID トークンの内部](../outlook/inside-the-identity-token.md)
    
- [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)
    
