
# Outlook で新規作成フォームのアイテムに添付ファイルを追加および削除する
Outlook アドインから非同期メソッドと Exchange Web サービス (EWS) ID を使用してユーザーが Outlook で作成するアイテムに添付ファイルを追加する JavaScript の例をご覧ください。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 添付ファイルとしてファイルまたは Outlook アイテムを追加する


[addFileAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) メソッドおよび [addItemAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) メソッドを使用して、ユーザーが作成しているアイテムにファイルおよび Outlook アイテムをそれぞれ添付することができます。どちらも非同期メソッドであり、添付ファイルの追加アクションの完了を待たずに実行を進めることができます。追加される添付ファイルの元の場所とサイズに応じて、添付ファイルの追加の非同期呼び出しが完了するのに少し時間がかかる場合があります。アクションの完了に依存するタスクがある場合は、それらのタスクをコールバック メソッドで実行する必要があります。このコールバック メソッドはオプションで、添付ファイルのアップロードが完了したときに呼び出されます。コールバック メソッドは、添付ファイルの追加アクションのステータス、エラー、および戻り値を提供する出力パラメーターとして [AsyncResult](http://dev.outlook.com/reference/add-ins/simple-types.html%28Office.15%29.md) オブジェクトを受け取ります。コールバックが追加のパラメーターを必要とする場合は、オプションの _options.aysncContext_ パラメーターで指定できます。 _options.asyncContext_ は、コールバック メソッドが必要とする任意の型にすることができます。

たとえば、 _options.asyncContext_ を、1 つ以上のキーと値のペアを含む JSON オブジェクト (":" 文字でキーと値の間を区切り、"," でキーと値のペアの間を区切ったもの) として定義できます。Office アドイン プラットフォームの [オプションのパラメーターを非同期メソッドに渡す](../../docs/develop/asynchronous-programming-in-office-add-ins.md#AsyncProgramming_OptionalParameters)ことに関するほかの例については、「 [Office アドインにおける非同期プログラミング](../../docs/develop/asynchronous-programming-in-office-add-ins.md)」を参照してください。次の使用例では、 **asyncContext** パラメーターを使用して、2 つの引数をコールバック メソッドに渡す方法を示します。




```
{ asyncContext: { var1: 1, var2: 2} }
```

コールバック メソッドで  **AsyncResult** オブジェクトの **status** プロパティと **error** プロパティを使用して、非同期メソッド呼び出しの成功またはエラーを確認できます。添付が正常に完了した場合は、 **AsyncResult.value** プロパティを使用して添付ファイル ID を取得できます。添付ファイル ID は整数であり、後で添付ファイルを削除するときに使用できます。


 >**メモ**  ベスト プラクティスとして、添付ファイル ID を使用して添付ファイルを削除するのは、同じアドインが同じセッションでその添付ファイルを追加した場合のみにしてください。Outlook Web App と デバイス用 OWA では、添付ファイル ID は同じセッション内でのみ有効です。ユーザーがアドインを閉じたとき、またはユーザーがインライン フォームで作成を開始した後インライン フォームから出て別のウィンドウで作業を続行したとき、セッションは終了します。


### ファイルの添付

 **addFileAttachmentAsync** メソッドを使用してファイルの URI を指定することにより、新規作成フォームのメッセージまたは予定にファイルを添付できます。ファイルが保護されている場合、URI クエリ文字列パラメーターとして、該当する ID または認証トークンを 含めることができます。Exchange は添付ファイルを取得するために URI への呼び出しを行い、ファイルを保護する Web サービスは認証手段としてトークンを使用する必要があります。

次の JavaScript の例は、Web サーバーのファイル picture.png を作成中のメッセージまたは予定に添付する新規作成アドインです。コールバック メソッドは  **asyncResult** をパラメーターとして受け取り、添付のステータスを確認して、添付が成功した場合は添付ファイル ID を取得します。




```
var mailbox;
var attachmentURI = "https://webserver/picture.png";
var attachmentID;

Office.initialize = function () {
    mailbox = Office.context.mailbox;
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
        // Add the specified file attachment to the item
        // being composed.
        // When the attachment finishes uploading, the
        // callback method is invoked and gets the attachment ID. 
        // You can optionally pass any object that you would  
        // access in the callback method as an argument to  
        // the asyncContext parameter.
        mailbox.item.addFileAttachmentAsync(
            attachmentURI,
            'picture.png',
            { asyncContext: null },
            function (asyncResult) {
                if (asyncResult.status == Office.AsyncResultStatus.Failed){
                    write(asyncResult.error.message);
                }
                else {
                    // Get the ID of the attached file.
                    attachmentID = asyncResult.value;
                    write('ID of added attachment: ' + attachmentID);
                }
            });
    });
}

// Writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```


### Outlook アイテムの添付

Outlook アイテム (メール、予定表、連絡先アイテムなど) の Exchange Web Services (EWS) ID を指定して  **addItemAttachmentAsync** メソッドを使用して、新規作成フォームのメッセージまたは予定にそのアイテムを添付できます。ユーザーのメールボックスのメール、予定表、連絡先、タスク アイテムの EWS ID は、 [mailbox.makeEwsRequestAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md) メソッドを使用して、EWS 操作の [FindItem](http://msdn.microsoft.com/en-us/library/ebad6aae-16e7-44de-ae63-a95b24539729%28Office.15%29.aspx) にアクセスすることにより取得できます。閲覧フォームの既存のアイテムでは、 [item.itemId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティでも EWS ID が取得できます。

次の JavaScript 関数  `addItemAttachment` は、上記の最初の例を拡張して、作成中のメールまたは予定に添付ファイルとしてアイテムを追加します。この関数は、添付するアイテムの EWS ID を引数として受け取ります。添付に成功すると添付ファイル ID を取得するので、同じセッションでの添付ファイルの削除など、それ以降の処理に使用できます。




```
// Adds the specified item as an attachment to the composed item.
// ID is the EWS ID of the item to be attached.
function addItemAttachment(ID) {
    // When the attachment finishes uploading, the
    // callback method is invoked. Here, the callback
    // method uses only asyncResult as a parameter,
    // and if the attaching succeeds, gets the attachment ID.
    // You can optionally pass any other object you wish to 
    // access in the callback method as an argument to 
    // the asyncContext parameter.
    mailbox.item.addItemAttachmentAsync(
        ID,
        'Welcome email',
        { asyncContext: null },
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write(asyncResult.error.message);
            }
            else {
                attachmentID = asyncResult.value;
                write('ID of added attachment: ' + attachmentID);
            }
        });
}
```


 >**メモ**  新規作成アドインを使用して、Outlook Web App または デバイス用 OWA の定期的な予定のインスタンスを添付できます。ただし、サポート Outlook リッチ クライアントでは、インスタンスを 1 つ添付しようとしても、定期的な一連の予定 (マスター予定) が添付されます。


## 添付ファイルを削除する


新規作成フォームのメッセージまたは予定からのファイルまたはアイテムの添付の削除は、該当する添付ファイル ID を指定して [removeAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) メソッドを使用することにより実行できます。削除する添付ファイルは、同じアドインが同じセッションで追加した添付ファイルだけにする必要があります。添付ファイル ID が有効な添付ファイルに対応していることを確認する必要があります。対応していないと、メソッドがエラーを返します。 **addFileAttachmentAsync** メソッドや **addItemAttachmentAsync** メソッドと同様に、 **removeAttachmentAsync** は非同期メソッドです。ステータスとエラーを確認するには、コールバック メソッドを提供して **AsyncResult** 出力パラメーター オブジェクトを使用する必要があります。キーと値のペアの JSON オブジェクトであるオプションの **asyncContext** パラメーターを使用して、追加のパラメーターをコールバック メソッドに渡すこともできます。

次の JavaScript 関数  `removeAttachment` は、上記の例を引き続き拡張し、作成中のメールまたは予定から指定された添付ファイルを削除します。この関数は、削除する添付ファイルの ID を引数として受け取ります。 **addFileAttachmentAsync** メソッドまたは **addItemAttachmentAsync** メソッドの呼び出しが成功した後、添付ファイルの ID を取得することができ、その ID をその後の **removeAttachmentAsync** メソッドの呼び出しのために保存できます。




```
// Removes the specified attachment from the composed item.
// ID is the Exchange identifier of the attachment to be 
// removed. 
function removeAttachment(ID) {
    // When the attachment is removed, the
    // callback method is invoked. Here, the callback
    // method uses an asyncResult parameter and gets
    // the ID of the removed attachment if the removal
    // succeeds.
    // You can optionally pass any object you wish to 
    // access in the callback method as an argument to 
    // the asyncContext parameter.
    mailbox.item.removeAttachmentAsync(
        ID,
        { asyncContext: null },
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write(asyncResult.error.message);
            }
            else {
                write('Removed attachment with the ID: ' + asyncResult.value);
            }
        });
}
```


## 添付ファイルの追加と削除のヒント


新規作成アドインが添付ファイルを追加および削除する場合は、有効な添付ファイル ID を添付ファイルの削除呼び出しに渡し、 **AsyncResult.error** で **InvalidAttachmentId** が返される場合を処理するようにコードを構成してください。添付ファイルの場所とサイズに応じて、ファイルまたはアイテムの添付の完了に時間がかかる場合があります。次の使用例は、 **addFileAttachmentAsync**、 `write`、および **removeAttachmentAsync** の呼び出しを含みます。呼び出しは、1 つずつ連続して実行することを想定しています。


```
var attachmentURI = "https://webserver/picture.png";
var attachmentID;

// Gets the current time in minutes, seconds and milliseconds.
function minutesSecondsMilliSeconds()
{
    var d = new Date();
    return d.getMinutes() + ":" + d.getSeconds() + ":" + d.getMilliseconds();
}

Office.context.mailbox.item.addFileAttachmentAsync(
        attachmentURI,
        'Welcome document',
        { asyncContext: null },
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write('(1): ' + minutesSecondsMilliSeconds() + ' ' + 
                    asyncResult.error.message);
            }
            else {
                attachmentID = asyncResult.value;
                write('(2): ' + minutesSecondsMilliSeconds() + ' ' + 
                    'ID of added attachment: ' + attachmentID);
            }
            write ('(3): ' + minutesSecondsMilliSeconds() + ' ' + 
                'Finishing addFileAttachmentAsync callback method.');
        });

write ('(4): ' + minutesSecondsMilliSeconds() + ' ' + 
    'attachmentID is: ' + attachmentID);

Office.context.mailbox.item.removeAttachmentAsync(
        attachmentID,      
        { asyncContext: null },
       function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write('(5): ' + minutesSecondsMilliSeconds() + ' ' + 
                    asyncResult.error.message);
            }
            else {           
                write('(6): ' + minutesSecondsMilliSeconds() + ' ' + 
                    ID of removed attachment: ' + asyncResult.value);
            }
        });


```

 **addFileAttachmentAsync** が **removeAttachmentAsync** の前に開始されていても、 **addFileAttachmentAsync** は非同期であるため、 `write` 呼び出しと **removeAttachmentAsync** 呼び出しは、 **addFileAttachmentAsync** の完了前に開始する可能性があります。この場合、 `attachmentID` は **undefined** のままであり、次の出力のように **removeAttachmentAsync** 呼び出しに対してエラーが表示されます。




```
 (4): 46:18:245 attachmentID is: undefined
Error executing code: Sys.ArgumentException: Sys.ArgumentException: Value does not fall within the expected range. Parameter name: attachmentId
 (2): 46:18:255 ID of added attachment: 0
 (3): 46:18:262 Finishing addFileAttachmentAsync callback method.
```

これを回避する 1 つの方法は、 **removeAttachmentAsync** を呼び出す前に `attachmentID` が定義されていることを確認することです。別の方法としては、次の例に示すように、 **removeAttachmentAsync** 呼び出しを **addFileAttachmentAsync** のコールバック メソッド内から実行する方法があります。




```
var attachmentURI = "https://webserver/picture.png";
var attachmentID;

function minutesSecondsMilliSeconds()
{
    var d = new Date();
    return d.getMinutes() + ":" + d.getSeconds() + ":" + d.getMilliseconds();
}

Office.context.mailbox.item.addFileAttachmentAsync(
        attachmentURI,
        'Welcome document',
        { asyncContext: null },
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write('(1) ' + minutesSecondsMilliSeconds() + ' ' + 
                    asyncResult.error.message);
            }
            else {
                attachmentID = asyncResult.value;
                write('(2) ' + minutesSecondsMilliSeconds() + ' ' + 
                    'ID of added attachment: ' + attachmentID);

                // Move the write and removeAttachmentAsync calls here 
                // inside the addFileAttachmentAsync callback, after the 
                // attaching has succeeded.
                write ('(4): ' + minutesSecondsMilliSeconds() + ' ' + 
                    'attachmentID is: ' + attachmentID);

                Office.context.mailbox.item.removeAttachmentAsync(
                    attachmentID,
                    { asyncContext: null },
                    function (asyncResult) {
                        if (asyncResult.status == Office.AsyncResultStatus.Failed){
                            write('(5) ' + minutesSecondsMilliSeconds() + ' ' + 
                                asyncResult.error.message);
                        }
                        else {
                            write('(6) ' + minutesSecondsMilliSeconds() + ' ' + 
                                'ID of removed attachment: ' + attachmentID);
                        }
                    });
            }

            write('(3) ' + minutesSecondsMilliSeconds() + ' ' + 
                'Finishing addFileAttachmentAsync callback method.');
        });

// Writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

出力の例を次に示します。




```
(2) 49:25:775 ID of added attachment: 1
(4) 49:25:782 attachmentID is: 1
(3) 49:25:783 Finishing addFileAttachmentAsync callback method.
(6) 49:25:789 ID of removed attachment: 1
```

 **removeAttachmentAsync** のコールバックは、 **addFileAttachmentAsync** のコールバックの内部にネストされていることに注意してください。 **addFileAttachmentAsync** と **removeAttachmentAsync** は非同期であるため、 **addFileAttachmentAsync** のコールバックの最後の行は **removeAttachmentAsync** のコールバックが完了する前に実行できます。


## その他の技術情報



- [新規作成フォーム用の Outlook アドインを作成する](../outlook/compose-scenario.md)
    
- [Office アドインにおける非同期プログラミング](../../docs/develop/asynchronous-programming-in-office-add-ins.md)
    


