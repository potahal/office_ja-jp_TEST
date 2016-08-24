---
ms.Toctitle: Create JavaScript web apps using CORS to access files
title: "CORS を使用して Office 365 API にアクセスする JavaScript Web アプリを作成する" 
description: "ms.TocTitle:CORS を使用してファイルにアクセスする JavaScript Web アプリを作成するTitle:CORS を使用して Office 365 API にアクセスする JavaScript Web アプリを作成するDescription:Office 365 API に対してクロスオリジン リソース共有 (CORS) の要求を行うように JavaScript Web アプリを構成します。この例の完全なソース コードを取得します。ms.ContentId: e3c6a490-be58-4967-8be6-29e8b4f4bf8cms.topic: 記事 (方法) ms.date:2015 年 8 月 13 日"
ms.ContentId: e3c6a490-be58-4967-8be6-29e8b4f4bf8c
ms.date: August 13, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# CORS を使用して Office 365 API にアクセスする JavaScript Web アプリを作成する

_**適用対象:** Office 365_

Office 365 API は、Exchange Online からメール、予定表、連絡先、SharePoint Online および OneDrive for Business からファイルやフォルダー、Azure AD からユーザーやグループにアクセスできるようにします。

クロスオリジン リソース共有 (CORS) を使用することにで、サーバー コードをほとんどまたは全く使用せずに、JavaScript クライアント アプリケーションからすべてのデータにアクセスすることが可能です。開発者は、CORS を使用して Office 365 API に要求を送信し、データへのアクセス、変更、および作成を確実に行うことができます。 

CORS 要求を使用して Office 365 API へのアクセスを開始する前に、[Office 365 開発環境のセットアップ](https://msdn.microsoft.com/en-us/office/office365/howto/setup-development-environment)を行う必要があります。その後、ここの手順に従って、Azure Active Directory でアプリを登録し、リソース エンドポイントを決定し、アクセス トークンを取得し、Office 365 API への認証済み要求を行うことにより、JavaScript アプリケーションが Office 365 API に対して CORS 要求を行えるようにします。 

**メモ:** このサンプルを実行する Internet Explorer には、[MS15-032:Internet Explorer 用の累積的なセキュリティ更新プログラム (KB3038314)](https://www.microsoft.com/en-us/download/details.aspx?id=46655) がインストールされている Internet Explorer 11 を使用する必要があります。 

## Azure AD にアプリを登録する

まず、アプリケーションを Azure AD に登録する必要があります。開始するには [Azure 管理ポータル](https://manage.windowsazure.com)にサインインします。サインインしたら、以下の手順に従います。 

1. 左側の列にある **[Active Directory]** ノードをクリックして、Office 365 サブスクリプションにリンクされているディレクトリを選択します。 

2. **[アプリケーション]** タブを選択してから、画面の下部にある **[追加]** を選択します。 

3. ポップアップ ウィンドウで、**[所属組織が開発しているアプリケーションの追加]** を選択します。 

4. *o365-cors-sample* などのアプリケーションの名前を選択し、種類として [Web アプリケーションや Web API] を選択します。矢印をクリックして続行します。 Then click the arrow to continue. 

5. **[サインオン URL]** の値は、アプリケーションをホストする場所の URL です。 

6. App ID URI は、アプリケーションを識別する Azure AD の一意の識別子です。これは、組織の Azure AD の一意の値である必要があります。 You can use *http://{your_subdomain}/o365-cors-sample*, where *{your_subdomain}* is the subdomain of **.onmicrosoft** you specified while signing up for your Office 365 Developer Site. Then click the check mark to provision your application. 

7. アプリケーションがプロビジョニングされたら、**[構成]** タブを選択します。 

8. **[他のアプリケーションへのアクセス許可]** セクションにスクロールダウンして、**[アプリケーションの追加]** ボタンをクリックします。 

9. このサンプルでは、ユーザーのファイルを取得して、**Office 365 SharePoint Online** アプリケーションを追加する方法について説明します。アプリケーションの行にあるプラス記号をクリックし、右上にあるチェック マークをクリックしてそれを追加します。次に、右下にあるチェック マークをクリックして続行します。

10. **Office 365 SharePoint Online** の行で、**[デリゲートされたアクセス許可]** を選択し、選択の一覧の **[ユーザーのファイルを読み取る]** を選択します。 

11. **[保存]** をクリックして、アプリケーションの構成を保存します。 

### OAuth 2.0 の暗黙的な付与フローを許可するようアプリケーションを構成する

Office 365 API 要求のアクセス トークンを取得するために、アプリケーションは OAuth の暗黙的な付与フローを使用します。既定では OAuth の暗黙的な付与フローは許可されていないため、これを許可するようにアプリケーションのマニフェストを更新する必要があります。 

1. Azure 管理ポータルで、アプリケーションのエントリの **[構成]** タブを選択します。 

2. ドロワーの **[マニフェストの管理]** ボタンを使用して、アプリケーションのマニフェスト ファイルをダウンロードし、コンピューターに保存します。

3. Open the manifest file with a text editor. Search for the *oauth2AllowImplicitFlow* property. By default it is set to *false*; change it to *true* and save the file.

4. **[マニフェストの管理]** ボタンを使用して、更新されたマニフェスト ファイルをアップロードします。

アプリケーションが Azure AD に正常に登録されました。 

## リソース エンドポイントの決定

任意の API 要求を行うには、使用するリソースの正しいエンドポイントを決定する必要があります。使用するエンドポイントは、Office 365 から取得する情報によって決まります。目的のエンドポイントを取得するには、API リファレンス ドキュメントを参照してください。 

* [メール API リファレンス](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations)

* [連絡先 API リファレンス](https://msdn.microsoft.com/office/office365/APi/contacts-rest-operations)

* [予定表 API リファレンス](https://msdn.microsoft.com/office/office365/APi/calendar-rest-operations)

* [Files API リファレンス](https://msdn.microsoft.com/office/office365/APi/files-rest-operations)

>We recommend that you take advantage of the Microsoft Graph when possible to access all of the APIs from a single endpoint, ```https://graph.microsoft.com```. Refer to the [Microsoft Graph documentation](https://graph.microsoft.io/) to browse all of the supported endpoints. 

For this sample, we will use the Files API to get a user's files. このサンプルでは、ファイル API を使用してユーザーのファイルを取得します。この操作のエンドポイントは ```https://{your_subdomain}-my.sharepoint.com/_api/v1.0/me/files```https://{your_subdomain}-my.sharepoint.com/_api/v1.0/me/files```{your_subdomain}``` です。{your_subdomain} は、Office 365 開発者向けサイトでサインアップするときに指定した **.onmicrosoft** のサブドメインです。 

**メモ:** マルチテナント アプリケーションを開発している場合は、[探索サービス](https://msdn.microsoft.com/en-us/office/office365/howto/discover-service-endpoints)を使用して、必要なエンドポイントを決定することができます。ただし、これはクライアント側の JavaScript アプリケーションでは実行できず、サーバー側のコードが必要になります。 

## Azure からのアクセス トークンの取得

Office 365 は、Azure AD によって発行される OAuth 2.0 のトークンを使用して、JavaScript クライアントを認証します。トークンは、OAuth 2.0 の暗黙的な付与フローを使用して取得されます。アプリケーションは、暗黙的な付与を使用して、現在サインイン中のユーザーのアクセス トークンを Azure AD に要求します。これはアプリケーションが、ユーザーを認証 URL に送信することによって行われます。認証 URL でユーザーが Office 365 の資格情報を使用してサインインすると、この URL 内のトークンと共にアプリにリダイレクトされます。  

次の関数は、認証 URL を作成し、その URL に移動して認証プロセスを開始します。

```javascript
function requestToken() { 
  // Change clientId and replyUrl to reflect your app's values 
  // found on the Configure tab in the Azure Management Portal. 
  // Also change {your_subdomain} to your subdomain for both endpointUrl and resource. 
  var clientId    = 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX';
  var replyUrl    = 'http://replyUrl/'; 
  var endpointUrl = 'https://{your_subdomain}-my.sharepoint.com/_api/v1.0/me/files';
  var resource = "https://{your_subdomain}-my.sharepoint.com"; 

  var authServer  = 'https://login.windows.net/common/oauth2/authorize?';  
  var responseType = 'token'; 

  var url = authServer + 
            "response_type=" + encodeURI(responseType) + "&" + 
            "client_id=" + encodeURI(clientId) + "&" + 
            "resource=" + encodeURI(resource) + "&" + 
            "redirect_uri=" + encodeURI(replyUrl); 

  window.location = url; 
}
```

ユーザーが自分の Office 365 のデータへのアクセス権をアプリに付与することを選択すると、URL 内のアクセス トークンと共にアプリケーションにリダイレクトされます。次の関数は、ページが読み込まれるたびに呼び出され、URL 内にアクセス トークンが存在するかチェックして、存在する場合はそれを抽出します。 

```javascript
var urlParameterExtraction = new (function () { 
  function splitQueryString(queryStringFormattedString) { 
    var split = queryStringFormattedString.split('&'); 

    // If there are no parameters in URL, do nothing.
    if (split == "") {
      return {};
    } 

    var results = {}; 

    // If there are parameters in URL, extract key/value pairs. 
    for (var i = 0; i < split.length; ++i) { 
      var p = split[i].split('=', 2); 
      if (p.length == 1) 
        results[p[0]] = ""; 
      else 
        results[p[0]] = decodeURIComponent(p[1].replace(/\+/g, " ")); 
    } 

    return results; 
  } 
  
  // Split the query string (after removing preceding '#'). 
  this.queryStringParameters = splitQueryString(window.location.hash.substr(1)); 
})(); 

// Extract token from urlParameterExtraction object.
var token = urlParameterExtraction.queryStringParameters['access_token']; 
```

これで、Office 365 API に対する CORS 要求を認証するためのアクセス トークンが用意できました。 

## Office 365 API への認証済み要求を実行する

At this point, you've registered your app with Azure AD, determined the correct endpoint for the resource you want access to, and have been granted an access token from Azure AD. All that's left to do is to use these pieces to make an HTTP request to the API. You'll do this by sending off an ```XMLHttpRequest``` to the Files API endpoint with the OAuth access token attached to the Authorization header of the request. 

次の関数は、要求を行い、*結果* と同じ ID を持つ DOM 要素に応答の内容を配置します。 

```javascript
function getFilesFromO365() { 
  try 
  { 
    var endpointUrl = 'https://{your_subdomain}-my.sharepoint.com/_api/v1.0/me/files'; 
    var xhr = new XMLHttpRequest(); 
    xhr.open("GET", endpointUrl); 

    // The APIs require an OAuth access token in the Authorization header, formatted like this: 'Authorization: Bearer <token>'. 
    xhr.setRequestHeader("Authorization", "Bearer " + token); 

    // Process the response from the API.  
    xhr.onload = function () { 
      if (xhr.status == 200) { 
        var formattedResponse = JSON.stringify(JSON.parse(xhr.response), undefined, 2);
        document.getElementById("results").textContent = formattedResponse; 
      } else { 
        document.getElementById("results").textContent = "HTTP " + xhr.status + "<br>" + xhr.response; 
      } 
    } 

    // Make request.
    xhr.send(); 
  } 
  catch (err) 
  {  
    document.getElementById("results").textContent = "Exception: " + err.message; 
  } 
}  
```

The response object of the ```XMLHttpRequest``` contains the Office 365 data. XMLHttpRequest の応答オブジェクトには、Office 365 のデータが含まれています。この例では、これはサインインしているユーザーのファイルか、要求を完了できなかった理由を示すエラーです。 

## 次の手順

ここまでで、JavaScript クライアント アプリケーションから、CORS を使用して Office 365 API に対して要求を行えるようにする方法について説明しました。これは、クライアント側の JavaScript アプリケーションで CORS を使用する方法の簡単な例です。大規模な実稼働アプリケーションを作成する場合は、[JavaScript 版 Azure Active Directory Library (ADAL JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) および「[Office 365 APIでAngular アプリを作成する](..\howto\getting-started-Office-365-APIs.md)」を詳しく調べる必要があります。このライブラリは、単一ページのアプリケーションの Azure AD 認証を処理します。  

以下は、この例のソース コードの全体であり、これを見るとそれぞれがどのように連携するかをより明確に理解することができます。 

**メモ:** この例のコードを任意の場所でホストする場合は、**getToken** 関数の ```clientId```clientId```replyUrl``````{your-subdomain}``` と ```resource```replyUrl の値に Azure 管理ポータルで確認できるアプリケーションの値を設定し、resource 変数の {your-subdomain} の部分を使用するサブドメインに変更してください。 

```javascript
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>O365 CORS Sample</title>
    <style> 
      body { 
        font-family:'Segoe UI'; 
      } 

      .paddedElement {
        margin-top: 5px; 
      }

      .hidden {
        visibility: hidden; 
      }
    </style>
  </head>
  <body>
    <h2>O365 CORS Sample</h2>
    <label for="TxtOauthToken">OAuth Token: </label><input type="text" id="TxtOauthToken" size="80"/>
    <br />
    <label for="endpointUrl">Endpoint URL: </label><input type="text" id="endpoint" size="80" /> 
    <br />
    <input type="button" class="paddedElement" id="getToken" value="Get Token"> 
    <input type="button" class="paddedElement" id="doCors" value="Make CORS Request" visibility="hidden" />
    <br /> 
    <br />
    <label for="results" class="hidden paddedElement" id="resultHeading">Results: </label>
    <br />
    <label id="debugMessage"></label>
    <pre id="results"></pre>
    <script type="text/javascript"> 
      (function (window) { 
        var isCorsCompatible = function() {
          try
          {
              var xhr = new XMLHttpRequest();
              xhr.open("GET", getEndpointUrl());
              xhr.setRequestHeader("authorization", "Bearer " + token);
              xhr.setRequestHeader("accept", "application/json");
              xhr.onload = function () {
                 // CORS is working.
                 console.log("Browser is CORS compatible."); 
              }
              xhr.send();
          }
          catch (e)
          {
              if (e.number == -2147024891)
              {
                  console.log("Internet Explorer users must use Internet Explorer 11 with MS15-032: Cumulative security update for Internet Explorer (KB3038314) installed for this sample to work.");
              }
              else
              {
                  console.log("An unexpected error occurred. Please refresh the page."); 
              }

          }
        };

        var urlParameterExtraction = new (function () { 
          function splitQueryString(queryStringFormattedString) { 
            var split = queryStringFormattedString.split('&'); 

            // If there are no parameters in URL, do nothing.
            if (split == "") {
              return {};
            } 

            var results = {}; 

            // If there are parameters in URL, extract key/value pairs. 
            for (var i = 0; i < split.length; ++i) { 
              var p = split[i].split('=', 2); 
              if (p.length == 1) 
                results[p[0]] = ""; 
              else 
                results[p[0]] = decodeURIComponent(p[1].replace(/\+/g, " ")); 
            } 

            return results; 
          } 

          // Split the query string (after removing preceding '#'). 
          this.queryStringParameters = splitQueryString(window.location.hash.substr(1)); 
        })(); 

        var token = urlParameterExtraction.queryStringParameters['access_token']; 

        if (token == undefined) {
          document.getElementById("TxtOauthToken").value = "Click \"Get Token\" to trigger sign-in after entering the endpoint you want to use."; 

          document.getElementById("doCors").style.visibility = "hidden"; 
        }
        else {
          document.getElementById("TxtOauthToken").value = token;
          document.getElementById("doCors").style.visibility = "visible"; 
          document.getElementById("getToken").style.display = "none"; 
        }

        // Constructs the authentication URL and directs the user to it. 
        function requestToken() { 
          // Change clientId and replyUrl to reflect your app's values 
          // found on the Configure tab in the Azure Management Portal.
          var clientId    = 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX';
          var replyUrl    = 'http://replyUrl/';
          var resource    = "https://{your_domain}-my.sharepoint.com";

          var authServer  = 'https://login.windows.net/common/oauth2/authorize?'; 
          var endpointUrl = getEndpointUrl();

          var responseType = 'token'; 

          var url = authServer + 
                    "response_type=" + encodeURI(responseType) + "&" + 
                    "client_id=" + encodeURI(clientId) + "&" + 
                    "resource=" + encodeURI(resource) + "&" + 
                    "redirect_uri=" + encodeURI(replyUrl); 
 
          window.location = url; 
        } 

        document.getElementById("getToken").onclick = requestToken; 

        function getEndpointUrl() { 
          return document.getElementById("endpoint").value; 
        } 

        function getFilesFromO365() { 
          document.getElementById("results").textContent = ""; 

          // Check browser compatbility. Check console output for details.
          isCorsCompatible(); 

          try 
          { 
            var xhr = new XMLHttpRequest(); 
            xhr.open("GET", getEndpointUrl()); 

            // The APIs require an OAuth access token in the Authorization header, formatted like this: 'Authorization: Bearer <token>'. 
            xhr.setRequestHeader("Authorization", "Bearer " + token); 

            // Process the response from the API.  
            xhr.onload = function () { 
              document.getElementById("resultHeading").style.visibility = "visible"; 

              if (xhr.status == 200) { 
                var formattedResponse = JSON.stringify(JSON.parse(xhr.response), undefined, 2);
                document.getElementById("results").textContent = formattedResponse; 
              } else { 
                document.getElementById("results").textContent = "HTTP " + xhr.status + "<br>" + xhr.response; 
              } 
            } 

            // Make request.
            xhr.send(); 
          } 
          catch (err) 
          { 
            document.getElementById("resultHeading").style.visibility = "visible"; 
            document.getElementById("results").textContent = "Exception: " + err.message; 
          } 
        } 

        document.getElementById("doCors").onclick = getFilesFromO365; 

      })(window); 
    </script> 
  </body>
</html>
```

## その他の技術情報

**コード サンプル**

-  [Office 365 API スタート プロジェクトおよびサンプル コード](..\howto\Starter-projects-and-code-samples.md) 
-  [GitHub 上の Office 開発者](https://github.com/OfficeDev)

**リファレンス**

-  [Office 365 API で Angular アプリを作成する](..\howto\getting-started-Office-365-APIs.md)
-  [Office 365 REST API リファレンス](..\api\API-Catalog.md)
-  [JavaScript 版 Active Directory Authentication Library (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-js)