
# Outlook で予定またはメッセージを作成するときに本文にデータを挿入する
ユーザーが作成している予定またはメッセージの本文に Outlook アドインからデータを挿入する方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 新規作成フォームの本文へのデータの挿入


非同期メソッド ([Body.getAsync](../reference/outlook/Body.html%28Office.15%29.md)、 [Body.getTypeAsync](https://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)、 [Body.prependAsync](https://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)、 [Body.setAsync](https://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)、 [Body.setSelectedDataAsync](https://dev.outlook.com/reference/add-ins/Body.md%28Office.15%29.md)) を使用して、本文の種類を取得し、ユーザーが作成している予定またはメッセージのアイテムの本文にデータを挿入することができます。これらの非同期メソッドは新規作成アドインでのみ使用できます。これらのメソッドを使用するには、「 [新規作成フォーム用の Outlook アドインを作成する](../outlook/compose-scenario.md)」のセクション「 [新規作成フォーム用の Outlook アドインのセットアップ](../outlook/compose-scenario.md#mod_off15_CreatingForCompose_SettingUp)」の説明に従って、Outlook が新規作成フォーム内でアドインをアクティブ化できるようにアドイン マニフェストが適切にセットアップされているか確認してください。

Outlook では、メッセージはテキスト形式、HTML 形式、またはリッチ テキスト形式 (RTF) で作成でき、予定は HTML 形式で作成できます。挿入する前に、追加の手順を実行しなければならない可能性があるため、 **getTypeAsync** を呼び出して、サポートされるアイテムの形式を最初に確認する必要があります。 **getTypeAsync** が返す値は、元のアイテムの形式によって、またデバイスのオペレーティング システムやホストが HTML 形式での編集をサポートするかどうかによって異なります1。 **prependAsync** または **setSelectedDataAsync** の _coercionType_ パラメーターをそれぞれ設定し2 、以下の表のようにデータを挿入します 。引数を指定しない場合、 **prependAsync** および **setSelectedDataAsync** は挿入するデータがテキスト形式であると想定します。



|**挿入するデータ**|**getTypeAsync によって返されるアイテム形式**|**使用する coercionType**|
|:-----|:-----|:-----|
|テキスト|テキスト1|テキスト|
|HTML|テキスト1|テキスト2|
|テキスト|HTML|テキスト/HTML|
|HTML|HTML |HTML|
1タブレットやスマートフォンでは、オペレーティング システムまたはホストが元々 HTML 形式で作成されたアイテムの編集をサポートしない場合、 **getTypeAsync** は **Office.MailboxEnums.BodyType.Text** を返します。

2挿入するデータが HTML であるのに、 **getTypeAsync** がそのアイテムに対してテキスト タイプを返す場合、データをテキストとして再編成し、 **Office.MailboxEnums.BodyType.Text** を _coercionType_ としてそのデータを挿入します。単に HTML データをテキスト強制型変換タイプで挿入すると、ホストでは HTML タグがテキストとして表示されます。 **Office.MailboxEnums.BodyType.Html** を _coercionType_ として HTML データを挿入しようとすると、エラーが発生します。

API for Office のほとんどの非同期メソッドと同様に、 **getTypeAsync**、 **prependAsync** および **setSelectedDataAsync** には、 _coercionType_ の他に、オプションの入力パラメーターがあります。これらのオプションの入力パラメーターの指定について詳しくは、「 [Office アドインにおける非同期プログラミング](../../docs/develop/asynchronous-programming-in-office-add-ins.md)」の「 [オプションのパラメーターを非同期メソッドに渡す](http://msdn.microsoft.com/ja-jp/library/7fe6bb42-3178-4d96-85f5-af5caea7b950%28Office.15%29.aspx#AsyncProgramming_OptionalParameters)」を参照してください。


## 現在のカーソル位置にデータを挿入する方法


ここでは、作成中のアイテムの本文タイプを  **getTypeAsync** を使用して検査してから、 **setSelectedDataAsync** を使用して現在のカーソル位置にデータを挿入するサンプル コードを示します。

コールバック メソッドとオプションの入力パラメーターを  **getTypeAsync** に渡し、ステータスと結果を _asyncResult_ 出力パラメーターで受け取ることができます。メソッドが成功した場合、アイテム本文のタイプを [AsyncResult.value](http://msdn.microsoft.com/en-us/library/453a4b43-0fdc-4ea9-967a-c033fab31507%28Office.15%29.aspx) プロパティで受け取ることができます。その値は、"text" または "html" です。

 **setSelectedDataAsync** への入力パラメーターとして、データ文字列を渡す必要があります。アイテム本文のタイプに応じて、このデータ文字列はテキスト形式または HTML 形式で指定できます。 前述したように、挿入するデータのタイプをオプションの _coercionType_ パラメーターに指定できます。また、コールバック メソッド、およびそのパラメーターがあればそのパラメーターを、オプションの入力パラメーターとして指定できます。

ユーザーがアイテム本文にカーソルを置いていない場合、 **setSelectedDataAsync** はデータを本文の先頭に挿入します。ユーザーがアイテム本文のテキストを選択した場合、 **setSelectedDataAsync** は選択されたテキストを指定されたデータに置き換えます。ユーザーがカーソル位置の変更とアイテムの作成を同時に行う場合、 **setSelectedDataAsync** が失敗する可能性があることに注意してください。1 回で挿入できる文字の最大数は、1,000,000 文字です。

このサンプル コードは、以下に示すように、アドイン マニフェストのルールが、予定またはメッセージの新規作成フォームでアドインをアクティブにすることを想定しています。




```XML
<Rule xsi:type="RuleCollection" Mode="Or">
  <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Edit"/>
  <Rule xsi:type="ItemIs" ItemType="Message" FormType="Edit"/>
</Rule>

```




```
var item;

Office.initialize = function () {
    item = Office.context.mailbox.item;
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
        // Set data in the body of the composed item.
        setItemBody();
    });
}


// Get the body type of the composed item, and set data in 
// in the appropriate data type in the item body.
function setItemBody() {
    item.body.getTypeAsync(
        function (result) {
            if (result.status == Office.AsyncResultStatus.Failed){
                write(result.error.message);
            }
            else {
                // Successfully got the type of item body.
                // Set data of the appropriate type in body.
                if (result.value == Office.MailboxEnums.BodyType.Html) {
                    // Body is of HTML type.
                    // Specify HTML in the coercionType parameter
                    // of setSelectedDataAsync.
                    item.body.setSelectedDataAsync(
                        '<b> Kindly note we now open 7 days a week.</b>',
                        { coercionType: Office.CoercionType.Html, 
                        asyncContext: { var3: 1, var4: 2 } },
                        function (asyncResult) {
                            if (asyncResult.status == 
                                Office.AsyncResultStatus.Failed){
                                write(asyncResult.error.message);
                            }
                            else {
                                // Successfully set data in item body.
                                // Do whatever appropriate for your scenario,
                                // using the arguments var3 and var4 as applicable.
                            }
                        });
                }
                else {
                    // Body is of text type. 
                    item.body.setSelectedDataAsync(
                        ' Kindly note we now open 7 days a week.',
                        { coercionType: Office.CoercionType.Text, 
                            asyncContext: { var3: 1, var4: 2 } },
                        function (asyncResult) {
                            if (asyncResult.status == 
                                Office.AsyncResultStatus.Failed){
                                write(asyncResult.error.message);
                            }
                            else {
                                // Successfully set data in item body.
                                // Do whatever appropriate for your scenario,
                                // using the arguments var3 and var4 as applicable.
                            }
                         });
                }
            }
        });

}

// Writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```


## アイテムの本文の先頭にデータを挿入する方法


別の方法として、 **prependAsync** を使用して現在のカーソル位置にかかわらず、データをアイテム本文の先頭に挿入することもできます。挿入の位置が異なることを除けば、 **prependAsync** と **setSelectedDataAsync** の動作は同じです。


- メッセージ本文の先頭に HTML データを付加する場合、テキスト形式のメッセージの先頭に HTML データを付加することがないように、まずメッセージ本文のタイプを調べる必要があります。
    
-  **prependAsync** への入力パラメーターとして、テキスト形式または HTML 形式のデータ文字列、挿入されるデータの形式 (オプション)、コールバック メソッド、およびそのパラメーター (パラメーターがある場合) を指定します。
    
- 同時に先頭に付加できる文字の最大数は、1,000,000 文字です。
    
以下の JavaScript コードは、予定およびメッセージの新規作成フォーム内でアクティブ化されるサンプル アドインの一部です。このサンプルは、 **getTypeAsync** を呼び出して、アイテム本文の種類を検査し、アイテムが予定または HTML メッセージの場合にはアイテム本文の先頭に HTML データを付加し、それ以外の場合にはテキスト形式のデータを挿入します。




```
var item;

Office.initialize = function () {
    item = Office.context.mailbox.item;
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
        // Insert data in the top of the body of the composed 
        // item.
        prependItemBody();
    });
}

// Get the body type of the composed item, and prepend data  
// in the appropriate data type in the item body.
function prependItemBody() {
    item.body.getTypeAsync(
        function (result) {
            if (result.status == Office.AsyncResultStatus.Failed){
                write(asyncResult.error.message);
            }
            else {
                // Successfully got the type of item body.
                // Prepend data of the appropriate type in body.
                if (result.value == Office.MailboxEnums.BodyType.Html) {
                    // Body is of HTML type.
                    // Specify HTML in the coercionType parameter
                    // of prependAsync.
                    item.body.prependAsync(
                        '<b>Greetings!</b>',
                        { coercionType: Office.CoercionType.Html, 
                        asyncContext: { var3: 1, var4: 2 } },
                        function (asyncResult) {
                            if (asyncResult.status == 
                                Office.AsyncResultStatus.Failed){
                                write(asyncResult.error.message);
                            }
                            else {
                                // Successfully prepended data in item body.
                                // Do whatever appropriate for your scenario,
                                // using the arguments var3 and var4 as applicable.
                            }
                        });
                }
                else {
                    // Body is of text type. 
                    item.body.prependAsync(
                        'Greetings!',
                        { coercionType: Office.CoercionType.Text, 
                            asyncContext: { var3: 1, var4: 2 } },
                        function (asyncResult) {
                            if (asyncResult.status == 
                                Office.AsyncResultStatus.Failed){
                                write(asyncResult.error.message);
                            }
                            else {
                                // Successfully prepended data in item body.
                                // Do whatever appropriate for your scenario,
                                // using the arguments var3 and var4 as applicable.
                            }
                         });
                }
            }
        });

}

// Writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```


## その他の技術情報



- [Outlook で新規作成フォームのアイテム データを取得および設定する](../outlook/get-and-set-item-data-in-a-compose-form.md)
    
- [読み取りまたは新規作成フォームの Outlook アイテム データを取得および設定する](../outlook/item-data.md)
    
- [新規作成フォーム用の Outlook アドインを作成する](../outlook/compose-scenario.md)
    
- [Office アドインにおける非同期プログラミング](../../docs/develop/asynchronous-programming-in-office-add-ins.md)
    
- [Outlook の予定またはメッセージを作成するときに受信者を取得、設定、または追加する](../outlook/get-set-or-add-recipients.md)
    
- [Outlook で予定またはメッセージを作成するときに件名を取得または設定する](../outlook/get-or-set-the-subject.md)
    
- [Outlook で予定を作成するときに場所を取得または設定する](../outlook/get-or-set-the-location-of-an-appointment.md)
    
- [Outlook で予定を作成するときに時刻を取得または設定する](../outlook/get-or-set-the-time-of-an-appointment.md)
    
