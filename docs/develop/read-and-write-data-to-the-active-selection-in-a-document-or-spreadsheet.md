
# ドキュメントまたはスプレッドシート内のアクティブな選択範囲へのデータの読み取りおよび書き込み
この記事では、ドキュメントまたはスプレッドシート内のユーザーの選択範囲への読み取りおよび書き込みを行う方法について説明します。また、ユーザーの選択範囲の変更用のイベント ハンドラーを作成する方法についても説明します。

 _ **適用対象:** apps for Office?| Excel?| Office Add-ins?| PowerPoint?| Project?| Word_


## 概要


[Document](http://msdn.microsoft.com/ja-jp/library/f8859516-cc1f-4b20-a8f3-cee37a983e70%28Office.15%29.aspx) オブジェクトが公開しているメソッドを使用すると、ユーザーのドキュメントまたはスプレッドシート内の現在の選択範囲への読み取りと書き込みを行うことができます。これは、 **Document** オブジェクトの **getSelectedDataAsync** メソッドと **setSelectedDataAsync** メソッドで行います。このトピックでは、ユーザーの選択範囲の読み取り方法、書き込み方法、およびその変更を検出するイベント ハンドラーの作成方法についても説明します。

 **getSelectedDataAsync** メソッドは、現在のユーザーの選択範囲のみに実行されます。実行中のアドインのセッション間で読み取りおよび書き取りに同じ選択範囲を利用できるように、ドキュメントの選択範囲を保持する必要がある場合、 [Bindings.addFromSelectionAsync](http://msdn.microsoft.com/ja-jp/library/edc99214-e63e-43f2-9392-97ead42fc155.aspx) メソッドを使用 (または、 [Bindings](http://msdn.microsoft.com/ja-jp/library/09979e31-3bfb-45be-adda-0f7cc2db1fe1.aspx) オブジェクトの他の "addFrom" メソッドの 1 つでバインドを作成) して、バインドを追加する必要があります。ドキュメントの領域にバインドを作成して、バインドの読み取りおよび書き込みを行う詳細については、「 [ドキュメントまたはスプレッドシート内の領域へのバインド](../../docs/develop/bind-to-regions-in-a-document-or-spreadsheet.md)」を参照してください。


### 選択されたデータを読み取る


次の例は、ドキュメント内の選択範囲のデータを [getSelectedDataAsync](http://msdn.microsoft.com/ja-jp/library/f85ad02c-64f0-4b73-87f6-7f521b3afd69%28Office.15%29.aspx) メソッドで取得する方法を示しています。


```
Office.context.document.getSelectedDataAsync(Office.CoercionType.Text, function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write('Action failed. Error: ' + asyncResult.error.message);
    }
    else {
        write('Selected data: ' + asyncResult.value);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

この例では、最初の  _coercionType_ パラメーターには **Office.CoercionType.Text** が指定されています (このパラメーターはリテラル文字列 `"text"` で指定することもできます)。この場合、コールバック関数の _asyncResult_ パラメーターから取得できる [AsyncResult](http://msdn.microsoft.com/ja-jp/library/453a4b43-0fdc-4ea9-967a-c033fab31507%28Office.15%29.aspx) オブジェクトの [value](http://msdn.microsoft.com/ja-jp/library/540c114f-0398-425c-baf3-7363f2f6bc47%28Office.15%29.aspx) プロパティは、ドキュメント内で選択されているテキストを格納している **string** を返します。別の型変換を指定すると、別の値が取得されます。 [Office.CoercionType](http://msdn.microsoft.com/ja-jp/library/735eaab6-5e31-4bc2-add5-9d378900a31b%28Office.15%29.aspx) は、使用できる型変換の値を表す列挙型です。 **Office.CoercionType.Text** は文字列 "text" として評価されます。


 >**ヒント**   **データ アクセスにマトリックスを使用する場合と、テーブルの coercionType を使用する場合。** 行と列が追加される際に選択した表形式データを動的に大きくする必要がある場合、また、テーブルのヘッダーと一緒に使用する必要がある場合、( `"table"` または **Office.CoercionType.Table** として **getSelectedDataAsync** メソッドの _coercionType_ パラメーターを使用して) テーブル データ型を使用する必要があります。データ構造内の行と列の追加は、テーブルとマトリックス データの両方でサポートされますが、行と列の追加はテーブル データでのみサポートされます。行と列の追加を予定していない場合、また、データにヘッダー機能を必要としない場合、( `"matrix"` または **Office.CoercionType.Matrix** として **getSelecteDataAsync** メソッドの _coercionType_ パラメーターを指定して) マトリックス データ型を使用する必要があります。これにより、データを処理するためのより簡単なモデルが提供されます。

2 番目の  _callback_ パラメーターとして関数に渡される匿名関数は、 **getSelectedDataAsync** 操作の完了時に実行されます。この関数は、結果および呼び出しのステータスが格納される _asyncResult_ という 1 つのパラメーターを使用して呼び出されます。呼び出しが失敗した場合は、 **AsyncResult** オブジェクトの [error](http://msdn.microsoft.com/ja-jp/library/51c46d36-972d-4d82-91aa-da99cbeb8d4f%28Office.15%29.aspx) プロパティから [Error](http://msdn.microsoft.com/ja-jp/library/36d1d048-b888-4bb5-9321-d340bcbc86f4%28Office.15%29.aspx) オブジェクトにアクセスできます。 [Error.name](http://msdn.microsoft.com/ja-jp/library/b76aaafd-bb34-4853-b29d-67adb1111b37%28Office.15%29.aspx) プロパティと [Error.message](http://msdn.microsoft.com/ja-jp/library/594e168e-4fdf-4e80-ba7e-4856a4a8ea5f%28Office.15%29.aspx) プロパティの値をチェックして、設定の操作が失敗した理由を判断できます。呼び出しが成功した場合は、ドキュメント内で選択されているテキストが表示されます。

 **if** ステートメントでは、呼び出しが成功したかどうかの判定に [AsyncResult.status](http://msdn.microsoft.com/ja-jp/library/eec9c712-79eb-4365-88a1-6d77649727c1%28Office.15%29.aspx) プロパティを使用しています。 [Office.AsyncResultStatus](http://msdn.microsoft.com/ja-jp/library/e2652105-03e8-4771-a985-e66c661fe3ea%28Office.15%29.aspx) は **AsyncResult.status** プロパティが取り得る値を表す列挙型です。 **Office.AsyncResultStatus.Failed** は文字列 "failed" として評価されます (こちらもリテラル文字列で指定することもできます)。


### 選択範囲にデータを書き込む


次の例は、"Hello World!" を表示するために選択範囲を設定する方法を示しています。


```
Office.context.document.setSelectedDataAsync("Hello World!", function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write(asyncResult.error.message);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

 _data_ パラメーターに異なるオブジェクト型を渡すと、結果が異なります。結果は、ドキュメントで現在選択されているもの、アドインをホストしているアプリケーション、および渡されたデータを現在の選択範囲に型変換できるかどうかによって異なります。

 _callback_ パラメーターとして [setSelectedDataAsync](http://msdn.microsoft.com/ja-jp/library/998f38dc-83bd-4659-a759-4758c632a6ef%28Office.15%29.aspx) メソッドに渡される匿名関数は、非同期呼び出しの完了時に実行されます。 **setSelectedDataAsync** メソッドを使用して選択範囲にデータを書き込む場合、コールバックの _asyncResult_ パラメーターは呼び出しのステータスへのアクセスのみを提供し、呼び出しが失敗した場合は [Error](http://msdn.microsoft.com/ja-jp/library/36d1d048-b888-4bb5-9321-d340bcbc86f4%28Office.15%29.aspx) オブジェクトにアクセスします。

 **注意:**Excel 2013 SP1 および Excel Online の関連するビルドのリリースから、 [現在の選択範囲にテーブルを書き込む際に書式設定](../../docs/excel/format-tables-in-add-ins-for-excel.md)ができるようになりした。


### 選択範囲の変更を検出する


次の例は、 [Document.addHandlerAsync](http://msdn.microsoft.com/ja-jp/library/8b2ec6c4-0983-4f5e-abd9-16f15b4fc87b%28Office.15%29.aspx) メソッドを使用して、 [SelectionChanged](http://msdn.microsoft.com/ja-jp/library/4cbc527c-a1d5-4fb0-b6db-28cc40c5d5e2%28Office.15%29.aspx) イベントのイベント ハンドラーをドキュメント上に追加することで、選択範囲の変更を検出する方法を示しています。


```
Office.context.document.addHandlerAsync("documentSelectionChanged", myHandler, function(result){} 
);

// Event handler function.
function myHandler(eventArgs){
write('Document Selection Changed');
}

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

最初の  _eventType_ パラメーターでは、サブスクライブするイベントの名前を指定しています。文字列 `"documentSelectionChanged"` をこのパラメーターに指定するのは、 [Office.EventType](http://msdn.microsoft.com/ja-jp/library/82c79659-52da-48b0-92a9-831226eb9a7f%28Office.15%29.aspx) 列挙型のイベントの種類 **Office.EventType.DocumentSelectionChanged** を渡すことに相当します。

2 番目の  _handler_ パラメーターとして関数に渡される `myHander()` 関数は、ドキュメントで選択範囲が変更されたときに実行されるイベント ハンドラーです。この関数は、非同期処理の完了時に、 [DocumentSelectionChangedEventArgs](http://msdn.microsoft.com/ja-jp/library/283f2d97-2595-444b-86a6-286efd77f638%28Office.15%29.aspx) オブジェクトへの参照が格納される _eventArgs_ という 1 つのパラメーターを使用して呼び出されます。 [DocumentSelectionChangedEventArgs.document](http://msdn.microsoft.com/ja-jp/library/12974085-c146-45ff-aede-70e247d8426f%28Office.15%29.aspx) プロパティを使用すると、このイベントが発生したドキュメントにアクセスできます。


 >**メモ**   **addHandlerAsync** メソッドを再び呼び出して、 _handler_ パラメーターに追加のイベント ハンドラー関数を指定すると、特定のイベントに複数のイベント ハンドラーを追加できます。この場合、各イベント ハンドラー関数の名前は一意である必要があります。


### 選択範囲の変更の検出を中止する


次の例は、 [document.removeHandlerAsync](http://msdn.microsoft.com/ja-jp/library/4cbc527c-a1d5-4fb0-b6db-28cc40c5d5e2%28Office.15%29.aspx) メソッドを呼び出して、 [Document.SelectionChanged](http://msdn.microsoft.com/ja-jp/library/47e0b00f-e301-4f21-836d-aeac783c42e0%28Office.15%29.aspx) イベントのリッスンを中止する方法を示しています。


```
Office.context.document.removeHandlerAsync("documentSelectionChanged", {handler:myHandler}, function(result){});
```

2 番目の  _handler_ パラメーターとして渡される `myHandler` 関数名は、 **SelectionChanged** イベントから削除されるイベント ハンドラーを指定します。


 >**重要**   **removeHandlerAsync** メソッドを呼び出すときにオプションの _handler_ パラメーターを省略すると、指定された _eventType_ のすべてのイベント ハンドラーが削除されます。


## その他のリソース



- [作業ウィンドウ アドインとコンテンツ アドインの概要](task-pane-and-content-add-ins.md)
    
- [バインドからデータを読み取る](bind-to-regions-in-a-document-or-spreadsheet.md#BindRegions_Read)
    
- [バインドにデータを書き込む](bind-to-regions-in-a-document-or-spreadsheet.md#BindRegions_Write)
    
