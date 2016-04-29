
# サーバーから Outlook アイテムの添付ファイルを取得する
Outlook アドインでサービスを使用して Exchange サーバーから添付ファイルを取得できるようにします。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

Outlook アドインは、サーバーで実行されているリモート サービスに選択したアイテムの添付ファイルを直接渡すことができません。その代わりに、添付ファイル API を使用して、添付ファイルに関する情報をリモート サービスに送信できます。そうすれば、サービスは Exchange サーバーに直接アクセスして添付ファイルを取得できるようになります。

添付ファイルの情報をリモート サービスに送信するには、次のプロパティと関数を使用します。


- [Office.context.mailbox.ewsUrl](../reference/outlook/Office.context.mailbox.md%28Office.15%29.md) プロパティ ? メールボックスをホストしている Exchange サーバー上の Exchange Web サービス (EWS) の URL を提供します。サービスはこの URL を使用して、 [ExchangeService.GetAttachments](http://msdn.microsoft.com/ja-jp/library/office/dn600509%28v=exchg.80%29.aspx)? [EWS Managed API](http://msdn.microsoft.com/library/c2267733-6f4f-49e5-9614-1e4a24c3af1a%28Office.15%29.aspx) メソッドまたは [GetAttachment](http://msdn.microsoft.com/ja-jp/library/24d10a15-b942-415e-9024-a6375708f326%28Office.15%29.aspx) EWS 操作を呼び出します。
    
- [Office.context.mailbox.item.attachments](../reference/outlook/Office.context.mailbox.item.html%28Office.15%29.md) プロパティ ? [AttachmentDetails](https://dev.outlook.com/reference/add-ins/simple-types.md%28Office.15%29.md) オブジェクトの配列をアイテムの添付ファイルごとに 1 つ取得します。
    
- [Office.context.mailbox.getCallbackTokenAsync](../reference/outlook/Office.context.mailbox.md%28Office.15%29.md) 関数 ? メールボックスをホストする Exchange サーバーを非同期で呼び出し、添付ファイルの要求の認証のために Exchange サーバーに送り返すコールバック トークンを取得します。
    

## 添付ファイル API を使用する


添付ファイル API を使用して Exchange メールボックスから添付ファイルを取得するには、次の手順を実行します。


1. 添付ファイルを含むメッセージまたは予定が表示されているときは、アドインを表示します。
    
2. Exchange サーバーからコールバック トークンを取得します。
    
3. コールバック トークンと添付ファイルの情報をリモート サービスに送信します。
    
4.  **ExchangeService.GetAttachments** メソッドまたは **GetAttachment** 操作を使用して Exchange サーバーから添付ファイルを取得します。
    
各手順については、以下のセクションで [Outlook-Add-in-JavaScript-GetAttachments](https://github.com/OfficeDev/Outlook-Add-in-JavaScript-GetAttachments) サンプルのコードを使用して詳しく説明します。


 >**メモ**  以下の例に示すコードは、添付ファイルの情報を強調するために短縮されています。サンプルには、アドインをリモート サーバーで認証し、要求の状態を管理するためのコードも含まれています。


### アドインのアクティブ化


次の例に示すように、アドインのマニフェスト ファイルにある [ItemHasAttachment](http://msdn.microsoft.com/ja-jp/library/031db7be-8a25-5185-a9c3-93987e10c6c2%28Office.15%29.aspx) 規則を使用すると、選択されたアイテムに添付ファイルがある場合にアドインを表示することができます。


```XML
<Rule xsi:type="ItemHasAttachment" />
```


### コールバック トークンを取得する


[Office.context.mailbox](../reference/outlook/Office.context.mailbox.md%28Office.15%29.md) オブジェクトには、リモート サーバーが Exchange サーバーを認証するために使用できるトークンを取得する **getCallbackTokenAsync** 関数が用意されています。次のコードは、コールバック トークンを取得する非同期要求を開始するアドインの関数と、応答を取得するコールバック関数を示しています。コールバック トークンは、次のセクションで定義するサービス要求オブジェクトに格納されます。


```
function getAttachmentToken() {
    if (serviceRequest.attachmentToken == "" {
        Office.context.mailbox.getCallbackTokenAsync(attachmentTokenCallback);
    }
};
function attachmentTokenCallback(asyncResult, userContext) {
    if (asyncResult.status === "succeeded") {
        // Cache the result from the server.
        serviceRequest.attachmentToken = asyncResult.value;
        serviceRequest.state = 3;
        testAttachments();
    } else {
        showToast("Error", "Could not get callback token: " + asyncResult.error.message);
    }
};
```


### 添付ファイルの情報をリモート サービスに送信する


アドインが呼び出すリモート サービスによって、サービスへの添付ファイル情報の送信方法に関する詳細が定義されます。この例では、リモート サービスは Visual Studio 2013 を使用して作成された Web API アプリケーションです。リモート サービスは、添付ファイルの情報が JSON オブジェクトに格納されていることを前提とします。次のコードは、添付ファイルの情報を格納するオブジェクトを初期化します。


```
// Initialize a context object for the add-in.
//   Set the fields that are used on the request
//   object to default values.
serviceRequest = new Object();
serviceRequest.attachmentToken = "";
serviceRequest.ewsUrl = Office.context.mailbox.ewsUrl;
serviceRequest.attachments = new Array();
```

 `Office.context.mailbox.item.attachments` プロパティには、 **AttachmentDetails** オブジェクトのコレクションが格納されます。このオブジェクトは、アイテムの添付ファイルごとに 1 つずつ存在します。ほとんどの場合、アドインは **AttachmentDetails** オブジェクトの添付ファイル ID プロパティのみをリモート サービスに渡すことができます。リモート サービスが添付ファイルの詳細情報を必要とする場合は、 **AttachmentDetails** オブジェクトの一部または全体を渡すことができます。次のコードは、 **AttachmentDetails** 配列全体を `serviceRequest` オブジェクトに格納し、要求をリモート サービスに送信するメソッドを定義します。




```
    function makeServiceRequest() {
      // Format the attachment details for sending.
      for (var i = 0; i < mailbox.item.attachments.length; i++) {
        serviceRequest.attachments[i] = JSON.parse(JSON.stringify(mailbox.item.attachments[i].$0_0));
      }

      $.ajax({
        url: '../../api/Default',
        type: 'POST',
        data: JSON.stringify(serviceRequest),
        contentType: 'application/json;CHARSET=shift-jis'
      }).done(function (response) {
        if (!response.isError) {
          var names = "<h2>Attachments processed using " +
                        serviceRequest.service +
                        ": " +
                        response.attachmentsProcessed +
                        "</h2>";
          for (i = 0; i < response.attachmentNames.length; i++) {
            names += response.attachmentNames[i] + "<br />";
          }
          document.getElementById("names").innerHTML = names;
        } else {
          app.showNotification("Runtime error", response.message);
        }
      }).fail(function (status) {

      }).always(function () {
        $('.disable-while-sending').prop('disabled', false);
      })
    };

```


### Exchange サーバーから添付ファイルを取得する


リモート サービスは、 [GetAttachments](http://msdn.microsoft.com/ja-jp/library/office/dn600509%28v=exchg.80%29.aspx) EWS Managed API メソッドまたは [GetAttachment](http://msdn.microsoft.com/library/24d10a15-b942-415e-9024-a6375708f326%28Office.15%29.aspx) EWS 操作のいずれかを使用してサーバーから添付ファイルを取得できます。サービス アプリケーションは、JSON 文字列をサーバーで使用できる .NET Framework オブジェクトに逆シリアル化するために 2 つのオブジェクトを必要とします。次のコードに、逆シリアル化オブジェクトの定義を示します。


```C#



namespace AttachmentsSample
{
  public class AttachmentSampleServiceRequest
  {
    public string attachmentToken { get; set; }
    public string ewsUrl { get; set; }
    public string service { get; set; }
    public AttachmentDetails [] attachments { get; set; }
  }

  public class AttachmentDetails
  {
    public string attachmentType { get; set; }
    public string contentType { get; set; }
    public string id { get; set; }
    public bool isInline { get; set; }
    public string name { get; set; }
    public int size { get; set; }
  }
}
```


#### EWS Managed API を使用して添付ファイルを取得する

リモート サービスで [EWS Managed API](http://go.microsoft.com/fwlink/?LinkID=255472) を使用する場合は、 [GetAttachments](http://msdn.microsoft.com/ja-jp/library/office/dn600509%28v=exchg.80%29.aspx) メソッドを使用できます。このメソッドは、添付ファイルを取得するための EWS SOAP 要求を作成、送信、および受信します。EWS Managed API を使用すると、必要なコード行が少なく、EWS の呼び出しを行うための直感的なインターフェイスが提供されるため、この API の使用をお勧めします。次のコードは、1 回の要求ですべての添付ファイルを取得し、処理された添付ファイルの数と名前を返します。


```C#
    private AttachmentSampleServiceResponse GetAtttachmentsFromExchangeServerUsingEWSManagedApi(AttachmentSampleServiceRequest request)
    {
      var attachmentsProcessedCount = 0;
      var attachmentNames = new List<string>();

      // Create an ExchangeService object, set the credentials and the EWS URL.
      ExchangeService service = new ExchangeService();
      service.Credentials = new OAuthCredentials(request.attachmentToken);
      service.Url = new Uri(request.ewsUrl);

      var attachmentIds = new List<string>();

      foreach (AttachmentDetails attachment in request.attachments)
      {
        attachmentIds.Add(attachment.id);
      }

      // Call the GetAttachments method to retrieve the attachments on the message.
      // This method results in a GetAttachments EWS SOAP request and response
      // from the Exchange server.
      var getAttachmentsResponse =
        service.GetAttachments(attachmentIds.ToArray(),
                               null,
                               new PropertySet(BasePropertySet.FirstClassProperties,
                                               ItemSchema.MimeContent));

      if (getAttachmentsResponse.OverallResult == ServiceResult.Success)
      {
        foreach (var attachmentResponse in getAttachmentsResponse)
        {
          attachmentNames.Add(attachmentResponse.Attachment.Name);

          // Write the content of each attachment to a stream.
          if (attachmentResponse.Attachment is FileAttachment)
          {
            FileAttachment fileAttachment = attachmentResponse.Attachment as FileAttachment;
            Stream s = new MemoryStream(fileAttachment.Content);
            // Process the contents of the attachment here.
          }

          if (attachmentResponse.Attachment is ItemAttachment)
          {
            ItemAttachment itemAttachment = attachmentResponse.Attachment as ItemAttachment;
            Stream s = new MemoryStream(itemAttachment.Item.MimeContent.Content);
            // Process the contents of the attachment here.
          }

          attachmentsProcessedCount++;
        }
      }

      // Return the names and number of attachments processed for display
      // in the add-in UI.
      var response = new AttachmentSampleServiceResponse();
      response.attachmentNames = attachmentNames.ToArray();
      response.attachmentsProcessed = attachmentsProcessedCount;

      return response;
    }


```


#### EWS を使用して添付ファイルを取得する

リモート サービスで EWS を使用する場合は、Exchange サーバーから添付ファイルを取得するための [GetAttachment](http://msdn.microsoft.com/library/24d10a15-b942-415e-9024-a6375708f326%28Office.15%29.aspx) SOAP 要求を作成する必要があります。次のコードは、SOAP 要求を示す文字列を返します。リモート サービスは、 **String.Format** メソッドを使用して添付ファイルの添付ファイル ID を文字列に挿入します。


```C#
    private const string GetAttachmentSoapRequest =
@"<?xml version=""1.0"" encoding=""utf-8""?>
<soap:Envelope xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance""
xmlns:xsd=""http://www.w3.org/2001/XMLSchema""
xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/""
xmlns:t=""http://schemas.microsoft.com/exchange/services/2006/types"">
<soap:Header>
<t:RequestServerVersion Version=""Exchange2013"" />
</soap:Header>
  <soap:Body>
    <GetAttachment xmlns=""http://schemas.microsoft.com/exchange/services/2006/messages""
    xmlns:t=""http://schemas.microsoft.com/exchange/services/2006/types"">
      <AttachmentShape/>
      <AttachmentIds>
        <t:AttachmentId Id=""{0}""/>
      </AttachmentIds>
    </GetAttachment>
  </soap:Body>
</soap:Envelope>";

```

最後に、次のメソッドが、EWS の  **GetAttachment** 要求を使用して Exchange サーバーから添付ファイルを取得する処理を行います。この実装では、添付ファイルごとに個別の要求を行い、処理した添付ファイルの数を返します。各応答は、別々の **ProcessXmlResponse** メソッドで処理されます。このメソッドの定義は、この後で示します。




```C#
    private AttachmentSampleServiceResponse GetAttachmentsFromExchangeServerUsingEWS(AttachmentSampleServiceRequest request)
    {
      var attachmentsProcessedCount = 0;
      var attachmentNames = new List<string>();

      foreach (var attachment in request.attachments)
      {
        // Prepare a web request object.
        HttpWebRequest webRequest = WebRequest.CreateHttp(request.ewsUrl);
        webRequest.Headers.Add("Authorization",
          string.Format("Bearer {0}", request.attachmentToken));
        webRequest.PreAuthenticate = true;
        webRequest.AllowAutoRedirect = false;
        webRequest.Method = "POST";
        webRequest.ContentType = "text/xml; CHARSET=shift-jis";

        // Construct the SOAP message for the GetAttachment operation.
        byte[] bodyBytes = Encoding.UTF8.GetBytes(
          string.Format(GetAttachmentSoapRequest, attachment.id));
        webRequest.ContentLength = bodyBytes.Length;

        Stream requestStream = webRequest.GetRequestStream();
        requestStream.Write(bodyBytes, 0, bodyBytes.Length);
        requestStream.Close();

        // Make the request to the Exchange server and get the response.
        HttpWebResponse webResponse = (HttpWebResponse)webRequest.GetResponse();

        // If the response is okay, create an XML document from the reponse
        // and process the request.
        if (webResponse.StatusCode == HttpStatusCode.OK)
        {
          var responseStream = webResponse.GetResponseStream();

          var responseEnvelope = XElement.Load(responseStream);

          // After creating a memory stream containing the contents of the 
          // attachment, this method writes the XML document to the trace output.
          // Your service would perform it's processing here.
          if (responseEnvelope != null)
          {
            var processResult = ProcessXmlResponse(responseEnvelope);
            attachmentNames.Add(string.Format("{0} {1}", attachment.name, processResult));

          }

          // Close the response stream.
          responseStream.Close();
          webResponse.Close();

        }
        // If the response is not OK, return an error message for the 
        // attachment.
        else
        {
          var errorString = string.Format("Attachment \"{0}\" could not be processed. " +
            "Error message: {1}.", attachment.name, webResponse.StatusDescription);
          attachmentNames.Add(errorString);
        }
        attachmentsProcessedCount++;
      }

      // Return the names and number of attachments processed for display
      // in the add-in UI.
      var response = new AttachmentSampleServiceResponse();
      response.attachmentNames = attachmentNames.ToArray();
      response.attachmentsProcessed = attachmentsProcessedCount;

      return response;
    }

```

 **GetAttachment** 操作からの各応答は、 **ProcessXmlResponse** メソッドに送信されます。このメソッドは、応答にエラーがないかをチェックします。エラーが見つからなければ、添付ファイルとアイテムの添付ファイルを処理します。 **ProcessXmlResponse** メソッドは、添付ファイルを処理する作業の大部分を実行します。




```C#
    // This method processes the response from the Exchange server.
    // In your application the bulk of the processing occurs here.
    private string ProcessXmlResponse(XElement responseEnvelope)
    {
      // First, check the response for web service errors.
      var errorCodes = from errorCode in responseEnvelope.Descendants
                       ("{http://schemas.microsoft.com/exchange/services/2006/messages}ResponseCode")
                       select errorCode;
      // Return the first error code found.
      foreach (var errorCode in errorCodes)
      {
        if (errorCode.Value != "NoError")
        {
          return string.Format("Could not process result. Error: {0}", errorCode.Value);
        }
      }

      // No errors found, proceed with processing the content.
      // First, get and process file attachments.
      var fileAttachments = from fileAttachment in responseEnvelope.Descendants
                        ("{http://schemas.microsoft.com/exchange/services/2006/types}FileAttachment")
                            select fileAttachment;
      foreach(var fileAttachment in fileAttachments)
      {
        var fileContent = fileAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Content");
        var fileData = System.Convert.FromBase64String(fileContent.Value);
        var s = new MemoryStream(fileData);
        // Process the file attachment here. 
      }

      // Second, get and process item attachments.
      var itemAttachments = from itemAttachment in responseEnvelope.Descendants
                            ("{http://schemas.microsoft.com/exchange/services/2006/types}ItemAttachment")
                            select itemAttachment;
      foreach(var itemAttachment in itemAttachments)
      {
        var message = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Message");
        if (message != null)
        {
         // Process a message here.
          break;
        }
        var calendarItem = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}CalendarItem");
        if (calendarItem != null)
        {
          // Process calendar item here.
          break;
        }
        var contact = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Contact");
        if (contact != null)
        {
          // Process contact here.
          break;
        }
        var task = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Tontact");
        if (task != null)
        {
          // Process task here.
          break;
        }
        var meetingMessage = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingMessage");
        if (meetingMessage != null)
        {
          // Process meeting message here.
          break;
        }
        var meetingRequest = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingRequest");
        if (meetingRequest != null)
        {
          // Process meeting request here.
          break;
        }
        var meetingResponse = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingResponse");
        if (meetingResponse != null)
        {
          // Process meeting response here.
          break;
        }
        var meetingCancellation = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingCancellation");
        if (meetingCancellation != null)
        {
          // Process meeting cancellation here.
          break;
        }
      }
     
      return string.Empty;
    }

```


## その他の技術情報



- [閲覧フォーム用の Outlook アドインを作成する](../outlook/read-scenario.md)
    
- [Exchange、EWS の管理 API、EWS、および web サービスを探索します。](http://msdn.microsoft.com/library/0bc6f81d-cc10-42b0-ba5d-6f22ff55d51c%28Office.15%29.aspx)
    
- [Get started with EWS Managed API client applications](http://msdn.microsoft.com/library/c2267733-6f4f-49e5-9614-1e4a24c3af1a%28Office.15%29.aspx)
    
- [Outlook-Power-Hour_Code-Samples](https://github.com/OfficeDev/Outlook-Power-Hour-Code-Samples):  `MyAttachments` および `AttachmentsDemo`
    
