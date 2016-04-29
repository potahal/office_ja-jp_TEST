
# Exchange で ID トークンを使用して Outlook アドインからサービスを呼び出す
ID トークンを使用して、Outlook アドインの顧客の電子メール アカウントを、Web サービスが提供する情報にリンクする方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

ID トークンは、各顧客の一意の識別子を提供します。この識別子を使用して、提供するサービスをカスタマイズできます。コードで Exchange サーバーに ID トークンを要求するには、Outlook アドインに文字列を返す非同期メソッド呼び出しを使用します。この文字列には、JWT (JSON Web Token) ID トークンが含まれます。アドインでトークンを展開する必要はありません。トークンを Web サービスに渡すと、サービスがアドインからの要求を認証します。

アドインをサポートする Web サービスは、アドインの HTML および JavaScript ソース ファイルをホストするサーバー上で実行する必要があります。これにより、クロスサイト スクリプト エラーを防ぐことができます。アプリケーションの要求に応じて、サーバーが要求を別の Web サービスにプロキシすることもできます。

アドインが送信するサービス要求に ID トークンを追加するのは簡単です。トークンを要求して、トークンを使用し、Web サービス応答を使用するだけです。ここでは、 **XmlHttpRequest** メソッドを使用してサーバーに送信する簡単な XML ドキュメントでその方法を示します。

## Exchange サーバーに対するトークンの要求


このアドインの単純な初期化メソッドでは、 **getUserIdentityTokenAsync** メソッドを使用して Exchange サーバーから ID トークンを要求します。 _getUserIdentityToken_ パラメーターは、サーバーへの非同期要求が戻るときに呼び出される関数です。コールバック メソッドについては、次の手順を参照してください。


```
var _mailbox;
var _xhr;
// The initialize function is required for all add-ins.
Office.initialize = function () {
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
    // After the DOM is loaded, app-specific code can run.
        _mailbox = Office.context.mailbox;
    _mailbox.getUserIdentityTokenAsync(getUserIdentityTokenCallback);
    });
}

```


## ID トークンの使用


 **getUserIdentityTokenAsync** メソッドのコールバック関数にはパラメーターが 1 つあり、その **value** プロパティにユーザーの ID トークンが格納されます。

次のコールバック関数は、 **XMLHttpRequest** オブジェクトを作成して Web サービスを呼び出します。 **XMLHttpRequest** オブジェクトの **onreadystatechange** プロパティを、アドインが Web サービスから応答を受け取ったときに実行する必要がある関数の名前に設定します。




```
function getUserIdentityTokenCallback(asyncResult) {
    var token = asyncResult.value;

    _xhr = new XMLHttpRequest();
    _xhr.open("POST", "https://localhost:44300/IdentityTestService/UnpackTokenJSON");
    _xhr.setRequestHeader("Content-Type", "application/json; CHARSET=shift-jis");
    _xhr.onreadystatechange = readyStateChange;

    var request = new Object();
    request.token = token;
    request.phoneNumbers = _mailbox.item.getEntities().phoneNumbers;

    _xhr.send(JSON.stringify(request));
}
```


## Web サービス応答の使用


Web サービスからの応答を処理する別の単純な関数を次に示します。この関数は、 **XHMHttpResponse** コールバック関数の標準パターンに従っています。Web サービスから応答全体が返されるのを待機し、応答の内容をアドインの UI に配置します。この関数が解析する応答は、Web サービスからの応答です。この応答については、「 [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)」を参照してください。 


```
function readyStateChange() {
    if (_xhr.readyState == 4 &amp;&amp; _xhr.status == 200) {

        var response = JSON.parse(_xhr.responseText);

        if (undefined == response.error) {
            document.getElementById("msexchuid").value = response.token.msexchuid;
            document.getElementById("amurl").value = response.token.amurl;
            document.getElementById("uniqueID").value = response.token.uniqueID;
            document.getElementById("iss").value = response.token.iss;
            document.getElementById("x5t").value = response.token.x5t;
            document.getElementById("nbf").value = response.token.nbf;
            document.getElementById("exp").value = response.token.exp;
        }
        else {
            document.getElementById("error").value = response.error;
        }
    }
}
```


## 例: ID トークンを使用した Web サービスの呼び出し


ID トークンにより、サービスを呼び出しているクライアントの ID 情報が、サーバー上で実行されている Web サービスに提供されます。ID トークンを使用するには、次のものが必要です。


- Exchange サーバーから ID トークンを要求し、Web サービスに送信する Outlook アドイン。このトピックの情報は、Outlook アドインの作成に役立ちます。
    
- アドインの UI を提供するサーバー上で実行され、ID トークンを検証する Web サービス。この Web サービスを作成する際に必要な情報については、次のトピックを参照してください。
    
      - 「 [Exchange のトークン検証ライブラリを使用する](../outlook/use-the-token-validation-library.md)」 - Microsoft が提供する検証ライブラリを使用する場合
    
  - 「 [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)」 - 独自の検証コードを作成する場合
    

### サンプル アドインのコード


この記事に記載されているアドインでは、次のファイルが必要です。


- IdentityTest.js ? アドインのビジネス ロジックを提供する JavaScript ファイル。
    
- IdentityTest.html ? アドインの UI を提供する HTML ファイル。
    
また、ID テスト Web サービスも必要となります。この Web サービスについては、「 [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)」を参照してください。


#### IdentityTest.js

次の例は、IdentityTest.js ファイルを示しています。


```
var _mailbox;
var _xhr;

// The initialize function is required for all add-ins.
Office.initialize = function () {
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
    // After the DOM is loaded, app-specific code can run.
    _mailbox = Office.context.mailbox;
    _mailbox.getUserIdentityTokenAsync(getUserIdentityTokenCallback);
    });
}
function getUserIdentityTokenCallback(asyncResult) {
    var token = asyncResult.value;

    _xhr = new XMLHttpRequest();
    _xhr.open("POST", "https://localhost:44300/IdentityTestService/UnpackTokenJSON");
    _xhr.setRequestHeader("Content-Type", "application/json; CHARSET=shift-jis");
    _xhr.onreadystatechange = readyStateChange;

    var request = new Object();
    request.token = token;
    request.phoneNumbers = _mailbox.item.getEntities().phoneNumbers;

    _xhr.send(JSON.stringify(request));
}

function readyStateChange() {
    if (_xhr.readyState == 4 &amp;&amp; _xhr.status == 200) {

        var response = JSON.parse(_xhr.responseText);

        if (undefined == response.error) {
            document.getElementById("msexchuid").value = response.token.msexchuid;
            document.getElementById("amurl").value = response.token.amurl;
            document.getElementById("uniqueID").value = response.token.uniqueID;
            document.getElementById("iss").value = response.token.iss;
            document.getElementById("x5t").value = response.token.x5t;
            document.getElementById("nbf").value = response.token.nbf;
            document.getElementById("exp").value = response.token.exp;
        }
        else {
            document.getElementById("error").value = response.error;
        }
    }
}
```


#### IdentityTest.html

次の例は、IdentityTest.html ファイルを示しています。


```HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
    <title>Identity Test</title>

    <link rel="stylesheet" type="text/css" href="../Content/Office.css" />
    <link rel="stylesheet" type="text/css" href="../Content/App.css" />

    <script src="../Scripts/jquery-1.6.2.js"></script>
    <script src="../Scripts/Office/MicrosoftAjax.js"></script>
    <script src="../Scripts/Office/Office.js"></script>

    <!-- Add your JavaScript to the following JavaScript file -->
    <script src="../Scripts/IdentityTest.js"></script>
</head>
<body>
    <div id="SectionContent">
        <table style="width: 80%;">
            <tr>
                <th>Claim
                </th>
                <th>Contents
                </th>
            </tr>
            <tr>
                <td style="width: 25%;">Error:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="error" value="None" />
                </td>
            </tr>
            <tr>
                <td style="width: 25%;">User Exchange ID:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="msexchuid" />
                </td>
            </tr>
            <tr>
                <td style="width: 25%;">Authentication Metadata URL:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="amurl" />
                </td>
            </tr>
            <tr>
                <td style="width: 25%;">Unique identifier:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="uniqueID" />
                </td>
            </tr>
          </tr>
            <tr>
                <td style="width: 25%;">Audience:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="aud" />
                </td>
            </tr>
            <tr>
                <td style="width: 25%;">Issuer:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="iss" />
                </td>
            </tr>
            <tr>
                <td style="width: 25%;">Certificate thumbprint:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="x5t" />
                </td>
            </tr>
            <tr>
                <td style="width: 25%;">Valid from:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="nbf" />
                </td>
            </tr>
            <tr>
                <td style="width: 25%;">Valid to:
                </td>
                <td style="width: 75%;">
                    <input style="width: 100%;" id="exp" />
                </td>
            </tr>
        </table>
    </div>
</body>
</html>
```


## 次の手順


ID トークンの要求方法を理解したら、次に要求のサーバー側でトークンを使用する必要があります。次の記事が役立ちます。


- [Exchange のトークン検証ライブラリを使用する](../outlook/use-the-token-validation-library.md)
    
- [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)
    
- [Exchange の ID トークンを使用してユーザーを認証する](../outlook/authenticate-a-user-with-an-identity-token.md)
    

## その他の技術情報



- [Exchange の ID トークンを使用して Outlook アドインを認証する](../outlook/authentication.md)
    
- [Exchange の ID トークンの内部](../outlook/inside-the-identity-token.md)
    
