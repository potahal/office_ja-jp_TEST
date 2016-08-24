---
ms.Toctitle: Open the OneNote clients
title: "OneNote クライアントを開く" 
description: "**links** プロパティを使用して、OneNote Online またはネイティブ クライアントの OneNote ページとノートブックを開きます。"
ms.ContentId: 445543de-14db-4a53-bf2c-356d233e71fd
ms.date: November 18, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

# OneNote クライアントを開く

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

ページまたはノートブックの **links** プロパティを使用して、OneNote の特定のページまたはノートブックを開きます。 

**links** プロパティは 2 つの URL を含む JSON オブジェクトです。

``` 
{ 
    "links": {
        "oneNoteClientUrl": {
            "href": "onenote:https://..."
        },
        "oneNoteWebUrl": {
            "href": "https://..."
        }
    }
}
```

これらの URL は、ネイティブの OneNote クライアント アプリケーションまたは OneNote Online のページまたはノートブックを開きます。

<p id="outdent">**oneNoteClientUrl**</p>
<p id="indent">ネイティブのクライアントがデバイスにインストールされている場合は、そのクライアントを開きます。 この URL には、*onenote* プレフィックスが含まれています。<br />言語固有のバージョンがデバイスにインストールされている場合は、そのバージョンを開きます。 それ以外の場合は、プラットフォームの言語の設定を使用します。</p> 

<p id="outdent">**oneNoteWebUrl**</p>
<p id="indent">デバイスの既定のブラウザーが OneNote Online をサポートしている場合は、OneNote Online を開きます。<br />ブラウザーの言語設定を使用します。</p>

<br />
OneNote API は、次の操作の HTTP 応答で **links** プロパティを返します。

- [`POST pages`](../howto/onenote-create-page.md) 要求を送信してページを作成する
- `POST notebooks` 要求を送信してノートブックを作成する
- [`GET pages`](../howto/onenote-get-content.md#get-pages) または [`GET pages/{id}`](../howto/onenote-get-content.md#get-page) 要求を送信してページ メタデータを取得する
- [`GET notebooks`](../howto/onenote-get-content.md#get-notebooks) または [`GET notebooks/{id}`](../howto/onenote-get-content.md#get-notebook) 要求を送信してノートブック メタデータを取得する

次の例は、応答の状態コードを確認し、JSON を解析して URL を抽出し、クライアントを開く方法を示しています。

## iOS の例

次の例は、JSON 応答から OneNote クライアント URL を取得します。AFNetworking ライブラリ (http://afnetworking.com/) を使用して 2 つの URL を抽出します。この例では、**created** は応答値の格納に使用される ONSCPSStandardResponse オブジェクトへのポインターであり、**responseObject** は解析済みの JSON を保持します。

```
    /* Import the JSON library */
    #import "AFURLRequestSerialization.h"

    - (void)connectionDidFinishLoading:(NSURLConnection *)connection {
            if(delegate) {
                  int status = [returnResponse statusCode];
                  ONSCPSStandardResponse *standardResponse = nil;
                  if (status == 201) {
                        ONSCPSCreateSuccessResponse *created = 
                              [[ONSCPSCreateSuccessResponse alloc] init];
                        created.httpStatusCode = status;
                        NSError *jsonError;
                        NSDictionary *responseObject = 
                              [NSJSONSerialization JSONObjectWithData:returnData options:0 error:&jsonError];
                        if(responseObject && !jsonError) {
                              created.oneNoteClientUrl = ((NSDictionary *)
                                    ((NSDictionary *)responseObject[@"links"])[@"oneNoteClientUrl"])[@"href"];
                              created.oneNoteWebUrl = ((NSDictionary *)
                                    ((NSDictionary *)responseObject[@"links"])[@"oneNoteWebUrl"])[@"href"];
                        }
                  standardResponse = created;
                  }
                  else {
                        ONSCPSStandardErrorResponse *error = [[ONSCPSStandardErrorResponse alloc] init];
                        error.httpStatusCode = status;
                        error.message = [[NSString alloc] initWithData:returnData 
                              encoding:NSUTF8StringEncoding];
                        standardResponse = error;
                  }
                  // Send the response back to the client.
                  if (standardResponse) {
                        [delegate exampleServiceActionDidCompleteWithResponse: standardResponse];
                  }
            }
      }
``` 

応答からの URL を解析したので、以下のコードを使用して OneNote を開くことができます。**oneNoteClientUrl** を使用してインストール済みの OneNote クライアントを開くか、**oneNoteWebURL** を使用して OneNote Online を開きます。

```objective-c
NSURL *url = [NSURL URLWithString:standardResponse.oneNoteWebUrl];
[[UIApplication sharedApplication] openURL:url];
```

## Android の例

まず成功状態コードを確認し、次に JSON を解析します。この例では POST 要求が送信されていることを前提としているので、状態コード 201 が確認されます。GET 要求を行った場合は、代わりに状態コード 200 が確認されます。

```java
public ApiResponse getResponse() throws Exception {
    /* Get the HTTP response code and message from the connection object */
    int responseCode = mUrlConnection.getResponseCode();
    String responseMessage = mUrlConnection.getResponseMessage();
    String responseBody = null;

    /* Get the response if the new page was created successfully. */
    if ( responseCode == 201) {
        InputStream is = mUrlConnection.getInputStream();

        /* Verify that this byte array is big enough. */
        byte[] b1 = new byte[1024];
        StringBuffer buffer = new StringBuffer();

        /* Copy the body of the response into the new string. */
        /* Make sure the buffer is big enough. */
        while ( is.read(b1) != -1)
            buffer.append(new String(b1));

      /* When the returned data is complete, close the connection 
         and convert the byte array into a string. */
        mUrlConnection.disconnect();
        responseBody =  buffer.toString();
    }

    /* Create a new JSON object, and an object to hold the response URLs. */
    JSONObject responseObject = null;
    ApiResponse response = new ApiResponse();
    try {

        /* Store and verify the HTTP response code. */
        response.setResponseCode(responseCode);
        response.setResponseMessage(responseMessage);
        if ( responseCode == 201) {

            /* Retrieve the two URLs from the links property. */
            responseObject = new JSONObject(responseBody);
            String clientUrl = responseObject.getJSONObject(
                "links").getJSONObject("oneNoteClientUrl").getString("href");
            String webUrl = responseObject.getJSONObject(
                "links").getJSONObject("oneNoteWebUrl").getString("href");
            response.setOneNoteClientUrl(clientUrl);
            response.setOneNoteWebUrl(webUrl);
        }
    } catch (JSONException ex) {

        /* If the JSON was malformed or incomplete... */
        String msg = ex.getMessage();
        msg = msg;
    }
    return response;
}
```

次の例に示すように、応答プロパティを使用すると、アプリで OneNote Online を開くことができます。

```java 
if (response.getResponseCode() == 201) {
    Uri uriUrl = Uri.parse(response.getOneNoteWebUrl);  
    Intent launchBrowser = new Intent(Intent.ACTION_VIEW, uriUrl); 
    startActivity(launchBrowser);
}
```
 
または、Android デバイスのネイティブ OneNote クライアントを開くことができます。 **oneNoteClientUrl** プロパティを使用するときには、Intent を開始する前に、中かっこ `{ }` で GUID 文字列を囲む必要があります。 次の例に方法を示します。

```java 
if (response.getResponseCode() == 201) {

    // Get the URL from the OneNote API JSON response.
    String onenoteClientUrl = obtainClientLinkFromJSONResponse();
    String androidClientUrl = 
        onenoteClientUrl.replaceAll(
            "=([0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12})&",
            "={$1}&");

    // Open the URL: Open the newly created OneNote page.
    Uri uriUrl = Uri.parse(androidClientUrl);  
    Intent launchBrowser = new Intent(Intent.ACTION_VIEW, uriUrl); 
    startActivity(launchBrowser);
}
```

<a name="see-also"></a>
## その他のリソース

- [OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)
- [OneNote ページの作成](../howto/onenote-create-page.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)
