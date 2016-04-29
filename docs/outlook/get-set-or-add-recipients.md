
# Outlook の予定またはメッセージを作成するときに受信者を取得、設定、または追加する
ユーザーが Outlook で予定かメッセージを新規作成する際に Outlook アドインが受信者を取得、設定、追加する方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 新規作成モードで受信者を取得、設定、追加するための前提条件


JavaScript API for Office の提供する非同期メソッド ([Recipients.getAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)、 [Recipients.setAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)、 [Recipients.addAysnc](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)) はそれぞれ予定またはメッセージの新規作成フォームで受信者を取得、設定、追加するためのメソッドです。これらの非同期メソッドは新規作成アドインでのみ使用できます。これらのメソッドを使用するには、「 [新規作成フォーム用の Outlook アドインを作成する](../outlook/compose-scenario.md)」の「 [新規作成フォーム用の Outlook アドインのセットアップ](../outlook/compose-scenario.md#mod_off15_CreatingForCompose_SettingUp)」セクションの説明に従って、Outlook が新規作成フォームでアドインをアクティブにできるようにアドイン マニフェストが適切にセット アップされているか確認してください。

予定やメッセージ内の受信者を表すプロパティの一部は、新規作成フォームと閲覧フォームで読み取りアクセスに使用できます。予定の [optionalAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) と [requiredAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)、メッセージの [cc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) と [to](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) がこの種のプロパティに含まれます。閲覧フォームでは、以下のように親オブジェクトから直接プロパティにアクセスできます。




```
item.cc
```

しかし、新規作成フォームでは、ユーザーとアドインの両方が同時に受信者の挿入や変更を行うことがあるので、以下の例のように非同期メソッド  **getAsync** を使用してこれらのプロパティを取得する必要があります。




```
item.cc.getAsync
```

これらのプロパティを書き込みアクセスに使用できるのは新規作成フォームに限られ、閲覧フォームでは使用できません。

JavaScript API for Office のほとんどの非同期メソッドと同様に、 **getAsync**、 **setAsync**、および  **addAsync** にはオプションの入力パラメーターがあります。これらのオプション入力パラメーターの指定の詳細については、「 [Office アドインにおける非同期プログラミング](../../docs/develop/asynchronous-programming-in-office-add-ins.md)」の「 [オプションのパラメーターを非同期メソッドに渡す](http://msdn.microsoft.com/ja-jp/library/7fe6bb42-3178-4d96-85f5-af5caea7b950%28Office.15%29.aspx#AsyncProgramming_OptionalParameters)」を参照してください。


## 受信者を取得するには


このセクションでは、新規作成する予定やメッセージの受信者を取得し、その受信者の電子メール アドレスを表示するコード例を示します。下記のように、このコード例は、予定やメッセージ用の新規作成フォームでアドインをアクティブ化するルールがアドイン マニフェスト内にあることを前提にしています。 


```XML
<Rule xsi:type="RuleCollection" Mode="Or">
  <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Edit"/>
  <Rule xsi:type="ItemIs" ItemType="Message" FormType="Edit"/>
</Rule>
```

JavaScript API for Office では、予定の受信者を表すプロパティ ( **optionalAttendees** および **requiredAttendees**) とメッセージの受信者を表すプロパティ ([bcc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)、 **cc**、 **to**) は違うので、最初に [item.itemType](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティを使用して、新規作成するアイテムが予定かメッセージかを識別します。新規作成モードでは、これらの予定とメッセージのプロパティはすべて [Recipients](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md) オブジェクトなので、その後に非同期メソッド **Recipients.getAsync** を適用して対応する受信者を取得できます。

 **getAsync** を使用するには、非同期 **getAsync** 呼び出しによって返される状態、結果、エラーをチェックするコールバック メソッドを指定します。オプションの _asyncContext_ パラメーターを使用して、コールバック メソッドに引数を提供できます。コールバック メソッドは _asyncResult_ 出力パラメーターを返します。 **status** パラメーター オブジェクトの **error** プロパティと [error](http://dev.outlook.com/reference/add-ins/simple-types.html%28Office.15%29.md) プロパティを使用して非同期呼び出しの状態とエラー メッセージをチェックし、 **value** プロパティを使用して実際の受信者を取得できます。受信者は、 [EmailAddressDetails](http://dev.outlook.com/reference/add-ins/simple-types.html%28Office.15%29.md) オブジェクトの配列として表されます。

 **getAsync** メソッドは非同期なので、受信者が正常に取得されることに依存するアクションが後に続く場合、非同期呼び出しが正常に完了した場合のみ対応するコールバック メソッドでこの種のアクションを開始するようコードを編成する必要があることに注意してください。




```
var item;

Office.initialize = function () {
    item = Office.context.mailbox.item;
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
        // Get all the recipients of the composed item.
        getAllRecipients();
    });
}

// Get the email addresses of all the recipients of the composed item.
function getAllRecipients() {
    // Local objects to point to recipients of either
    // the appointment or message that is being composed.
    // bccRecipients applies to only messages, not appointments.
    var toRecipients, ccRecipients, bccRecipients;
    // Verify if the composed item is an appointment or message.
    if (item.itemType == Office.MailboxEnums.ItemType.Appointment) {
        toRecipients = item.requiredAttendees;
        ccRecipients = item.optionalAttendees;
    }
    else {
        toRecipients = item.to;
        ccRecipients = item.cc;
        bccRecipients = item.bcc;
    }
    
    // Use asynchronous method getAsync to get each type of recipients
    // of the composed item. Each time, this example passes an anonymous 
    // callback function that doesn't take any parameters.
    toRecipients.getAsync(function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Failed){
            write(asyncResult.error.message);
        }
        else {
            // Async call to get to-recipients of the item completed.
            // Display the email addresses of the to-recipients. 
            write ('To-recipients of the item:');
            displayAddresses(asyncResult);
        }    
    }); // End getAsync for to-recipients.

    // Get any cc-recipients.
    ccRecipients.getAsync(function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Failed){
            write(asyncResult.error.message);
        }
        else {
            // Async call to get cc-recipients of the item completed.
            // Display the email addresses of the cc-recipients.
            write ('Cc-recipients of the item:');
            displayAddresses(asyncResult);
        }
    }); // End getAsync for cc-recipients.

    // If the item has the bcc field, i.e., item is message,
    // get any bcc-recipients.
    if (bccRecipients) {
        bccRecipients.getAsync(function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Failed){
            write(asyncResult.error.message);
        }
        else {
            // Async call to get bcc-recipients of the item completed.
            // Display the email addresses of the bcc-recipients.
            write ('Bcc-recipients of the item:');
            displayAddresses(asyncResult);
        }
                        
        }); // End getAsync for bcc-recipients.
     }
}

// Recipients are in an array of EmailAddressDetails
// objects passed in asyncResult.value.
function displayAddresses (asyncResult) {
    for (var i=0; i<asyncResult.value.length; i++)
        write (asyncResult.value[i].emailAddress);
}

// Writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```


## 受信者を設定するには


このセクションでは、ユーザーが新規作成する予定やメッセージの受信者を設定するコード例を示しています。受信者を設定すると、既存の受信者が上書きされます。この例では、前述の新規作成フォームで受信者を取得する例と同様に、アドインが予定とメッセージの新規作成フォームでアクティブ化されることを想定しています。この例では、まず予定かメッセージのうち該当する方の受信者を表す適切なプロパティに対して非同期メソッド  **Recipients.setAsync** を適用するために、新規作成するアイテムが予定かメッセージかを確認します。

 **setAsync** を呼び出す際に、以下のいずれかの形式で、 _recipients_ パラメーターの入力引数として配列を指定します。


- SMTP アドレスである文字列の配列。
    
- 辞書の配列。次のコード例に示されているように、それぞれ表示名と電子メール アドレスが含まれています。
    
-  **getAsync** メソッドによって返される配列のような、 **EmailAddressDetails** オブジェクトの配列。
    
オプションで  **setAsync** メソッドに入力引数としてコールバック メソッドを提供し、受信者が正常に設定されることに依存するコードが、そのとおりになった場合に限り実行されるようにすることができます。オプションの _asyncContext_ パラメーターを使用してコールバック メソッドの引数を提供することもできます。コールバック メソッドを使用する場合は、 _asyncResult_ 出力パラメーターにアクセスし、 **AsyncResult** パラメーター オブジェクトの **status** プロパティや **error** プロパティを使用して非同期呼び出しの状態やエラー メッセージをチェックできます。




```
var item;

Office.initialize = function () {
    item = Office.context.mailbox.item;
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
        // Set recipients of the composed item.
        setRecipients();
    });
}

// Set the display name and email addresses of the recipients of 
// the composed item.
function setRecipients() {
    // Local objects to point to recipients of either
    // the appointment or message that is being composed.
    // bccRecipients applies to only messages, not appointments.
    var toRecipients, ccRecipients, bccRecipients;

    // Verify if the composed item is an appointment or message.
    if (item.itemType == Office.MailboxEnums.ItemType.Appointment) {
        toRecipients = item.requiredAttendees;
        ccRecipients = item.optionalAttendees;
    }
    else {
        toRecipients = item.to;
        ccRecipients = item.cc;
        bccRecipients = item.bcc;
    }
    
    // Use asynchronous method setAsync to set each type of recipients
    // of the composed item. Each time, this example passes a set of
    // names and email addresses to set, and an anonymous 
    // callback function that doesn't take any parameters. 
    toRecipients.setAsync(
        [{
            "displayName":"Graham Durkin", 
            "emailAddress":"graham@contoso.com"
         },
         {
            "displayName" : "Donnie Weinberg",
            "emailAddress" : "donnie@contoso.com"
         }],
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write(asyncResult.error.message);
            }
            else {
                // Async call to set to-recipients of the item completed.

            }    
    }); // End to setAsync.


    // Set any cc-recipients.
    ccRecipients.setAsync(
        [{
             "displayName":"Perry Horning", 
             "emailAddress":"perry@contoso.com"
         },
         {
             "displayName" : "Guy Montenegro",
             "emailAddress" : "guy@contoso.com"
         }],
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write(asyncResult.error.message);
            }
            else {
                // Async call to set cc-recipients of the item completed.
            }
    }); // End cc setAsync.


    // If the item has the bcc field, i.e., item is message,
    // set bcc-recipients.
    if (bccRecipients) {
        bccRecipients.setAsync(
            [{
                 "displayName":"Lewis Cate", 
                 "emailAddress":"lewis@contoso.com"
             },
             {
                 "displayName" : "Francisco Stitt",
                 "emailAddress" : "francisco@contoso.com"
             }],
            function (asyncResult) {
                if (asyncResult.status == Office.AsyncResultStatus.Failed){
                    write(asyncResult.error.message);
                }
                else {
                    // Async call to set bcc-recipients of the item completed.
                    // Do whatever appropriate for your scenario.
                }
        }); // End bcc setAsync.
    }
}

// Writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}

```


## 受信者を追加するには


予定やメッセージの既存の受信者を上書きしない場合は、 **Recipients.setAsync** を使用する代わりに、 **Recipients.addAsync** 非同期メソッドを使用して受信者を付加できます。 **addAsync** の働きは **setAsync** と似ていて、 _recipients_ 入力引数が必要です。オプションで、コールバック メソッドを指定し、asyncContext パラメーターを使用してコールバックの引数を提供できます。その後コールバック メソッドの _asyncResult_ 出力パラメーターを使用して、非同期 **addAsync** 呼び出しの状態、結果、エラーをチェックできます。次の例は、新規作成されるアイテムが予定かどうかチェックし、その予定に 2 人の必須の出席者を付加します。


```
// Add specified recipients as required attendees of
// the composed appointment. 
function addAttendees() {
    if (item.itemType == Office.MailboxEnums.ItemType.Appointment) {
        item.requiredAttendees.addAsync(
        [{
            "displayName":"Kristie Jensen", 
            "emailAddress":"kristie@contoso.com"
         },
         {
            "displayName" : "Pansy Valenzuela",
            "emailAddress" : "pansy@contoso.com"
          }],
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Failed){
                write(asyncResult.error.message);
            }
            else {
                // Async call to add attendees completed.
                // Do whatever appropriate for your scenario.
            }
        }); // End addAsync.
    }
}
```


## その他のリソース



- [Outlook で新規作成フォームのアイテム データを取得および設定する](../outlook/get-and-set-item-data-in-a-compose-form.md)
    
- [読み取りまたは新規作成フォームの Outlook アイテム データを取得および設定する](../outlook/item-data.md)
    
- [新規作成フォーム用の Outlook アドインを作成する](../outlook/compose-scenario.md)
    
- [Office アドインにおける非同期プログラミング](../../docs/develop/asynchronous-programming-in-office-add-ins.md)
    
- [Outlook で予定またはメッセージを作成するときに件名を取得または設定する](../outlook/get-or-set-the-subject.md)
    
- [Outlook で予定またはメッセージを作成するときに本文にデータを挿入する](../outlook/insert-data-in-the-body.md)
    
- [Outlook で予定を作成するときに場所を取得または設定する](../outlook/get-or-set-the-location-of-an-appointment.md)
    
- [Outlook で予定を作成するときに時刻を取得または設定する](../outlook/get-or-set-the-time-of-an-appointment.md)
    
