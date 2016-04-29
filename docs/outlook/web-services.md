
# Outlook アドインから Web サービスを呼び出す
Outlook 用 Outlook アドインから EWS またはその他の Web サービスを呼び出す方法を説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

アドインは、Exchange Server 2013 が実行されているコンピューター、アドインの UI のソースの場所を提供するサーバー上で利用できる Web サービス、またはインターネット上で利用できる Web サービスから Exchange Web サービス (EWS) を使用できます。この記事では、Outlook アドインからどのように EWS の情報を要求できるかを示す例を説明します。

Web サービスを呼び出す方法は、Web サービスは配置された場所によって異なります。表 1 は、場所により異なる Web サービスを呼び出すさまざまな方法を示しています。


**表 1. Outlook アドインから Web サービスを呼び出す方法**


|**Web サービスの場所**|**Web サービスを呼び出す方法**|
|:-----|:-----|
|クライアント メールボックスをホストする Exchange サーバー|[mailbox.makeEwsRequestAsync](../reference/outlook/Office.context.mailbox.md%28Office.15%29.md) メソッドを使用して、アドインがサポートしている EWS 操作を呼び出します。また、メールボックスをホストしている Exchange サーバーも EWS を公開します。|
|アドインの UI のソースの場所を提供する Web サーバー|標準の JavaScript の手法を使用して Web サービスを呼び出します。UI フレーム内の JavaScript コードは、UI を提供する Web サーバーのコンテキストで実行されます。そのため、クロスサイト スクリプト エラーを発生させることなく、そのサーバーで Web サービスを呼び出すことができます。|
|上記以外の場所|UI のソースの場所を提供する Web サーバー上で、Web サービスのプロキシを作成します。プロキシを作成しないと、クロスサイト スクリプト エラーによってアドインを実行できなくなります。プロキシを作成する方法の 1 つは JSON/P を使用することです。詳細については、「 [Office アドインのプライバシーとセキュリティ](87c59a88-10e2-4c88-b6a8-736bd356e5f8.md)」を参照してください。|

## makeEwsRequestAsync メソッドを使用して EWS 操作にアクセスする


[mailbox.makeEwsRequestAsync](../reference/outlook/Office.context.mailbox.md%28Office.15%29.md) メソッドを使用して、ユーザーのメールボックスをホストしている Exchange サーバーに EWS 要求を送ることができます。

EWS は、Exchange サーバーでの種々の操作をサポートしています。たとえば、アイテム レベルの操作としては、アイテムのコピー、検索、更新、または送信などがあり、フォルダー レベルの操作としては、フォルダーの作成、取得、または更新などがあります。EWS 操作を実行するには、その操作の XML SOAP 要求を作成します。操作が終了すると、その操作に関係するデータが含まれた XML SOAP 応答を受信します。EWS SOAP の要求と応答は、Messages.xsd ファイルで定義されているスキーマに従います。Messages.xsd ファイルは、他の EWS スキーマ ファイルと同様、EWS をホストしている IIS 仮想ディレクトリに配置されています。 

 **makeEwsRequestAsync** メソッドを使用して EWS 操作を開始するには、以下の項目を指定します。


- その EWS 操作の SOAP 要求に対する XML ( _data_ パラメーターへの引数)
    
- コールバック メソッド ( _callback_ 引数)
    
- そのコールバック メソッドに対するオプションの入力データ ( _userContext_ 引数)
    
EWS SOAP 要求が完了すると、Outlook が 1 つの引数 ([AsyncResult](../reference/outlook/simple-types.md%28Office.15%29.md) オブジェクト) を使用してコールバック メソッドを呼び出します。コールバック メソッドは、 **AsyncResult** オブジェクトの 2 つのプロパティにアクセスできます。1 つは **value** プロパティ (EWS 操作の XML SOAP 応答が含まれる)、もう 1 つはオプションの **asyncContext** プロパティ ( **userContext** パラメーターとして渡したデータが含まれる) です。通常、コールバック メソッドは、SOAP 応答内の XML を解析して、関係する情報を取得し、その情報を処理します。


## EWS 応答を解析するためのヒント


SOAP 応答を EWS 操作から解析する場合、ブラウザーに依存する以下の問題に注意してください。


- DOM メソッド  **getElementsByTagName** を使用する場合、Internet Explorer のサポートを組み込むため、タグ名に接頭辞を指定します。
    
     **getElementsByTagName** はブラウザーの種類に応じて異なる動作をします。たとえば、EWS 応答には以下の XML (表示用に書式設定して省略されています) が含まれる場合があります。
    


  ```XML
  <t:ExtendedProperty><t:ExtendedFieldURI PropertySetId="00000000-0000-0000-0000-000000000000" 
PropertyName="MyProperty" 
PropertyType="String"/>
<t:Value>{
...
}</t:Value></t:ExtendedProperty>
  ```


    以下に挙げるコードは、Chrome などのブラウザーで  **ExtendedProperty** タグで囲まれた XML を取得するために使用できます。
    


  ```
  var mailbox = Office.context.mailbox;
mailbox.makeEwsRequestAsync(mailbox.item.itemId), function(result) {
    var response = $.parseXML(result.value);
    var extendedProps = response.getElementsByTagName("ExtendedProperty");
  ```


    Internet Explorer では、以下に示すように、タグ名に接頭辞  `t:` を含める必要があります。
    


  ```
  var mailbox = Office.context.mailbox;
mailbox.makeEwsRequestAsync(mailbox.item.itemId), function(result) {
    var response = $.parseXML(result.value);
    var extendedProps = response.getElementsByTagName("t:ExtendedProperty");
  ```

- 以下のように DOM プロパティ  **textContent** を使用し、EWS 応答のタグの内容を取得します。
    
  ```
  content = $.parseJSON(value.textContent);
  ```


    Internet Explorer では、 **innerHTML** などのその他のプロパティは EWS 応答のいくつかのタグに対して機能しない場合があります。
    

## 例


次の例では、 **makeEwsRequestAsync** メソッドを呼び出して、 [GetItem](http://msdn.microsoft.com/ja-jp/library/e3590b8b-c2a7-4dad-a014-6360197b68e4%28Office.15%29.aspx) 操作を使用してアイテムの件名を取得します。この例には、次の 3 つの関数が含まれています。


-  `getSubjectRequest` ? 入力としてアイテム ID を受け取り、指定のアイテムに対する **GetItem** を呼び出すための SOAP 要求の XML を返します。
    
-  `sendRequest` ? `getSubjectRequest` を呼び出して指定のアイテムに対する SOAP 要求を取得し、SOAP 要求とコールバック メソッド ( `callback`) を  **makeEwsRequestAsync** に渡して、指定のアイテムの件名を取得します。
    
-  `callback` ? 指定のアイテムの件名とその他の情報が含まれている SOAP 応答を処理します。
    

```
function getSubjectRequest(id) {
   // Return a GetItem operation request for the subject of the specified item. 
   var result = 
'<?xml version="1.0" encoding="utf-8"?>' +
'<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"' +
'               xmlns:xsd="http://www.w3.org/2001/XMLSchema"' +
'               xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"' +
'               xmlns:t="http://schemas.microsoft.com/exchange/services/2006/types">' +
'  <soap:Header>' +
'    <RequestServerVersion Version="Exchange2013" xmlns="http://schemas.microsoft.com/exchange/services/2006/types" soap:mustUnderstand="0" />' +
'  </soap:Header>' +
'  <soap:Body>' +
'    <GetItem xmlns="http://schemas.microsoft.com/exchange/services/2006/messages">' +
'      <ItemShape>' +
'        <t:BaseShape>IdOnly</t:BaseShape>' +
'        <t:AdditionalProperties>' +
'            <t:FieldURI FieldURI="item:Subject"/>' +
'        </t:AdditionalProperties>' +
'      </ItemShape>' +
'      <ItemIds><t:ItemId Id="' + id + '"/></ItemIds>' +
'    </GetItem>' +
'  </soap:Body>' +
'</soap:Envelope>';

   return result;
}





function sendRequest() {
   // Create a local variable that contains the mailbox.
   var mailbox = Office.context.mailbox;

   mailbox.makeEwsRequestAsync(getSubjectRequest(mailbox.item.itemId), callback);
}

function callback(asyncResult)  {
   var result = asyncResult.value;
   var context = asyncResult.context;

   // Process the returned response here.
}


```


## アドインでサポートしている EWS 操作


Outlook アドインでは、 **makeEwsRequestAsync** メソッドを使用して、EWS で利用できる操作のサブセットにアクセスできます。EWS 操作、および **makeEwsRequestAsync** メソッドを使用して操作にアクセスする方法が不明な場合は、はじめに _data_ 引数をカスタマイズする SOAP 要求の例を確認してください。以下に、 **makeEwsRequestAsync** メソッドを使用する方法を説明します。


1. XML 内のアイテム ID および関係する EWS 操作属性を適切な値に置き換えます。
    
2.  **makeEwsRequestAsync** の _data_ パラメーターの引数として SOAP 要求を指定します。
    
3. コールバック メソッドを指定し、 **makeEwsRequestAsync** を呼び出します。
    
4. コールバック メソッド内で、SOAP 応答内の操作の結果を検証します。
    
5. 必要に応じて EWS 操作の結果を使用します。
    
次の表は、アドインがサポートしている EWS 操作を示しています。SOAP の要求と応答の例を表示するには、各操作のリンクを選択します。EWS 操作の詳細については、「 [Exchange での EWS の操作](http://msdn.microsoft.com/library/cf6fd871-9a65-4f34-8557-c8c71dd7ce09%28Office.15%29.aspx)」を参照してください。


**表 2. サポートされている EWS 操作**


|**EWS 操作**|**説明**|
|:-----|:-----|
|[CopyItem の操作](http://msdn.microsoft.com/library/bcc68f9e-d511-4c29-bba6-ed535524624a%28Office.15%29.aspx)|指定のアイテムをコピーし、Exchange ストア内の指定のフォルダーに新しいアイテムを入れます。|
|[CreateFolder 操作](http://msdn.microsoft.com/library/6f6c334c-b190-4e55-8f0a-38f2a018d1b3%28Office.15%29.aspx)|Exchange ストア内の指定の場所にフォルダーを作成します。|
|[CreateItem 操作](http://msdn.microsoft.com/library/78a52120-f1d0-4ed7-8748-436e554f75b6%28Office.15%29.aspx)|Exchange ストア内に指定のアイテムを作成します。|
|[FindConversation の操作](http://msdn.microsoft.com/library/2384908a-c203-45b6-98aa-efd6a4c23aac%28Office.15%29.aspx)|Exchange ストア内の指定のフォルダー内のスレッドのリストを列挙します。|
|[FindFolder 操作](http://msdn.microsoft.com/library/7a9855aa-06cc-45ba-ad2a-645c15b7d031%28Office.15%29.aspx)|指定のフォルダーのサブフォルダーを見つけ、一群のサブフォルダーを記述した一群のプロパティを返します。|
|[FindItem 操作](http://msdn.microsoft.com/library/ebad6aae-16e7-44de-ae63-a95b24539729%28Office.15%29.aspx)|Exchange ストア内の指定のフォルダー内にあるアイテムを識別します。|
|[GetConversationItems の操作](http://msdn.microsoft.com/library/8ae00a99-b37b-4194-829c-fe300db6ab99%28Office.15%29.aspx)|スレッドでノードに整理された 1 つ以上のアイテム セットを取得します。|
|[GetFolder の操作](http://msdn.microsoft.com/library/355bcf93-dc71-4493-b177-622afac5fdb9%28Office.15%29.aspx)|Exchange ストアから指定のプロパティとフォルダーの内容を取得します。|
|[GetItem 操作](http://msdn.microsoft.com/library/e3590b8b-c2a7-4dad-a014-6360197b68e4%28Office.15%29.aspx)|Exchange ストアから指定のプロパティとアイテムの内容を取得します。|
|[MarkAsJunk の操作](http://msdn.microsoft.com/library/1f71f04d-56a9-4fee-a4e7-d1034438329e%28Office.15%29.aspx)|電子メール メッセージを [迷惑メール] フォルダーに移動し、それらのメッセージの差出人を受信拒否リストに追加したり、受信拒否リストから削除したりします。|
|[MoveItem 操作](http://msdn.microsoft.com/library/dcf40fa7-7796-4a5c-bf5b-7a509a18d208%28Office.15%29.aspx)|アイテムを Exchange ストア内の単一のフォルダーに移動します。|
|[SendItem 操作](http://msdn.microsoft.com/library/337b89ef-e1b7-45ed-92f3-8abe4200e4c7%28Office.15%29.aspx)|Exchange ストア内にある電子メール メッセージを送信します。|
|[UpdateFolder 操作](http://msdn.microsoft.com/library/3494c996-b834-4813-b1ca-d99642d8b4e7%28Office.15%29.aspx)|Exchange ストア内の既存のフォルダーのプロパティを変更します。|
|[UpdateItem 操作](http://msdn.microsoft.com/library/5d027523-e0bc-4da2-b60b-0cb9fc1fdfe4%28Office.15%29.aspx)|Exchange ストア内の既存のアイテムのプロパティを変更します。|

## makeEwsRequestAsync メソッドの認証とアクセス許可について


 **makeEwsRequestAsync** メソッドを使用すると、現在のユーザーの電子メール アカウントの資格情報を使用して要求が認証されます。資格情報は **makeEwsRequestAsync** メソッドが管理するため、要求と共に認証資格情報を提供する必要はありません。


 >**メモ**  サーバー管理者は、 **makeEwsRequestAsync** メソッドで EWS 要求を送信できるようにするため、 [New-WebServicesVirtualDirctory](http://technet.microsoft.com/ja-jp/library/bb125176.aspx) コマンドレットまたは [Set-WebServicesVirtualDirecory](http://technet.microsoft.com/ja-jp/library/aa997233.aspx) コマンドレットを使用して、クライアント アクセス サーバーの EWS ディレクトリで _OAuthAuthentication_ パラメーターを **true** にする必要があります。

 **makeEwsRequestAsync** メソッドを使用するには、アドインのマニフェストにおいて **ReadWriteMailbox** アクセス許可を指定する必要があります。 **ReadWriteMailbox** アクセス許可の使用については、「 [ユーザーのメールボックスにアクセスする Outlook アドインのためのアクセス許可を指定する](../../docs/outlook/privacy/understanding-outlook-add-in-permissions.md)」の「 [ReadWriteMailbox アクセス許可](../outlook/understanding-outlook-add-in-permissions.md#olowa15conagave_permmodelreadwrite)」セクションを参照してください。


## その他の技術情報



- [Outlook アドイン](../outlook/outlook-add-ins.md)
    
- [Office アドインのプライバシーとセキュリティ](87c59a88-10e2-4c88-b6a8-736bd356e5f8.md)
    
- [Office アドインにおける同一生成元ポリシーの制限への対処](../../docs/develop/addressing-same-origin-policy-limitations.md)
    
- [EWS の参照の交換](http://msdn.microsoft.com/library/2a873474-1bb2-4cb1-a556-40e8c4159f4a%28Office.15%29.aspx)
    
- [Outlook と Exchange の EWS のメール アプリ](http://msdn.microsoft.com/library/821c8eb9-bb58-42e8-9a3a-61ca635cba59%28Office.15%29.aspx)
    
ASP.NET Web API を使用してアドイン用のバックエンド サービスを作成する場合は、以下の資料を参照してください。


- [ASP.NET Web API を使用して Office アドインの Web サービスを作成する](http://blogs.msdn.com/b/officeapps/archive/2013/06/10/create-a-web-service-for-an-app-for-office-using-the-asp-net-web-api.aspx)
    
- [ASP.NET Web API を使用した HTTP サービスの構築に関する基本](http://www.asp.net/web-api)
    
