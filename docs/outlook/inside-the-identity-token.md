
# Exchange の ID トークンの内部
Exchange 2013の ID トークンの中身について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

Exchange サーバーから Outlook アドインに送信される認証 ID トークンがどのようなものであるかをアドインは知りません。また、ユーザーの側で ID トークンをサーバーに送信するためにトークンの中身を調べる必要はありません。しかし、Outlook アドインとのやり取りを行う Web サービス コードを記述する場合には、この ID トークンに何が含まれているかを理解しておく必要があります。

## ID トークンとは


ID トークンとは、そのトークンを送信する Exchange サーバーによって自己署名された Base64 URL エンコードの文字列です。このトークンは暗号化されていません。署名の検証に使用する公開キーは、そのトークンを発行した Exchange サーバーに保存されています。このトークンは、ヘッダー、ペイロード、および署名の 3 つのパーツから成ります。トークン文字列では、トークンを容易に分割できるように、各パーツが文字 "." (ピリオド) で区切られています。

Exchange 2013では、JSON Web トークン (JWT) を ID トークンに使用します。JWT トークンについては、「[JSON Web Token (JWT) Internet Draft](http://self-issued.info/docs/draft-goland-json-web-token-00.mdl)」を参照してください。


### ID トークンのヘッダー

ヘッダーはトークンを識別し、どのような種類のトークンが提示されているかを Web サービスに知らせます。以下に、トークンのヘッダーの例を示します。

{ "typ" : "JWT", "alg" : "RS256", "x5t" : "Un6V7lYN-rMgaCoFSTO5z707X-4" }以下の表に、ID トークンのヘッダーの各パーツを示します。


**ID トークンのヘッダーのパーツ**


|**クレーム**|**値**|**説明**|
|:-----|:-----|:-----|
|typ|"JWT"|トークンを JSON Web トークンとして識別します。Exchange サーバーから提供される ID トークンはすべて JWT トークンです。|
|alg|"RS256"|署名の作成に使用されるハッシュ アルゴリズムです。Exchange サーバーから提供されるトークンはすべて RS-256 アルゴリズムを使用します。|
|x5t|証明書の拇印|トークンの X.509 拇印です。|

### ID トークンのペイロード

ペイロードには、電子メール アカウントの識別と、トークンを送信した Exchange サーバーの識別を行う認証クレームが含まれます。以下に、ペイロード セクションの例を示します。

{ "aud" : "https://mailhost.contoso.com/IdentityTest.html", "iss" : "00000002-0000-0ff1-ce00-000000000000@mailhost.contoso.com", "nbf" : "1331579055", "exp" : "1331607855", "appctxsender":"00000002-0000-0ff1-ce00-000000000000@mailhost.context.com", "isbrowserhostedapp":"true", "appctx" : { "msexchuid" : "53e925fa-76ba-45e1-be0f-4ef08b59d389@mailhost.contoso.com" "version" : "ExIdTok.V1" "amurl" : "https://mailhost.contoso.com:443/autodiscover/metadata/json/1" } }以下の表に、ID トークンのペイロードの各パーツを示します。


**ID トークンのペイロードのパーツ**


|**クレーム**|**説明**|
|:-----|:-----|
|aud|トークンを要求したアドインの URL です。トークンは、クライアントのブラウザー内で実行されているアドインから送信された場合にのみ有効です。 アドインが Office アドイン マニフェスト スキーマ v1.1 を使用する場合、この URL はフォーム タイプ  **SourceLocation** または **ItemRead** (アドイン マニフェスト内で [FormSettings](http://msdn.microsoft.com/ja-jp/library/0d1a311d-939d-78c1-e968-89ddf7ebc4b4%28Office.15%29.aspx) 要素の一部としていずれか最初に出現する方) の下にある、最初の **ItemEdit** 要素に指定された URL です。|
|iss|トークンを発行した Exchange サーバーの一意の識別子です。この Exchange サーバーから発行されるトークンはすべて同じ識別子になります。|
|nbf|トークンの有効期間の開始日時です。この値は 1970 年 1 月 1 日を起点とする秒数です。 |
|exp|トークンの有効期間の終了日時です。この値も 1970 年 1 月 1 日を起点とする秒数です。|
|appctxsender|アプリケーション コンテキストを送信した Exchange サーバーの一意の識別子です。|
|isbrowserhostedapp|アドインがブラウザーでホストされるかどうかを指定します。|
|appctx|トークンのアプリケーション コンテキストです。 |
appctx クレーム内の情報は、電子メール アカウントのアドレスと、そのアカウントの一意の識別子を提供します。以下の表に、appctx クレームの各パーツを示します。



|**appctx クレームのパーツ**|**説明**|
|:-----|:-----|
|msexchuid|電子メール アカウントと Exchange サーバーに割り当てられた一意の識別子です。|
|version|トークンのバージョン番号です。Exchange 2013を実行中のサーバーによって提供されるトークンの場合、この値はすべて "ExIdTok.V1" です。|
|amurl|トークンの署名に使用された X.509 証明書の公開キーを含む認証メタデータ ドキュメントの URL です。認証メタデータ ドキュメントの使用方法の詳細については、「 [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)」を参照してください。|

### ID トークンの署名

この署名は、ヘッダーおよびペイロード セクションに対して、ヘッダーで指定されたアルゴリズムを使用したハッシュ処理を行うと共に、ペイロード内で指定された場所にあるサーバー上の自己署名された X509 証明書を使用することで作成されます。Web サービスは、この署名を検証して、ID トークンがその送信元として想定されるサーバーから発行されたものであることを確認できます。


## その他のリソース



- [Exchange の ID トークンを使用して Outlook アドインを認証する](../outlook/authentication.md)
    
- [Exchange で ID トークンを使用して Outlook アドインからサービスを呼び出す](../outlook/call-a-service-by-using-an-identity-token.md)
    
- [Exchange のトークン検証ライブラリを使用する](../outlook/use-the-token-validation-library.md)
    
- [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)
    
