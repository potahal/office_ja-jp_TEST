
# ドキュメントまたはスプレッドシート内の領域へのバインド
この記事では、ドキュメントおよびスプレッドシートの領域へのバインドを作成し、そのバインドに対してデータの読み取りおよび書き込みを行う方法について説明します。また、バインド内のデータまたはユーザーの選択範囲の変更用のイベント ハンドラーを作成および削除する方法についても説明します。 

 _ **適用対象:** apps for Office?| Excel?| Office Add-ins?| Word_


## ドキュメントまたはスプレッドシート内の領域へのバインド

バインドベースのデータ アクセスにより、コンテンツ アドインおよび作業ウィンドウ アドインは、ドキュメントまたはスプレッドシートの特定の領域に ID を通じて一貫性をもってアクセスできます。アドインは、最初に、ドキュメントの部分と一意の ID を関連付けるいずれかのメソッド ([addFromPromptAsync](http://msdn.microsoft.com/ja-jp/library/9dc03608-b08b-4700-8be1-3c86ae236799%28Office.15%29.aspx)、 [addFromSelectionAsync](http://msdn.microsoft.com/ja-jp/library/edc99214-e63e-43f2-9392-97ead42fc155%28Office.15%29.aspx)、または [addFromNamedItemAsync](http://msdn.microsoft.com/ja-jp/library/afbadac7-60c7-47cb-9477-6e9466ded44c%28Office.15%29.aspx)) を呼び出すことによって、バインドを確立する必要があります。バインドが確立されると、アドインは提供された ID を使用して、ドキュメントまたはスプレッドシート内の関連付けられた領域に含まれるデータにアクセスできます。バインドの作成により、アドインに次の効果がもたらされます。


- テーブル、範囲、テキスト (連続する文字列) など、サポートされている Office アプリケーション間で共通のデータ構造にアクセスできます。
    
- ユーザーによる選択を必要とせずに、読み取り/書き込み操作が可能です。
    
- アドインとドキュメント内のデータの間にリレーションシップが確立されます。バインドはドキュメント内に保持され、後でアクセスできます。
    
また、バインドを確立すると、ドキュメントまたはスプレッドシートの特定の領域を範囲とする、データおよび選択範囲の変更イベントをサブスクライブできます。つまり、ドキュメントまたはスプレッドシート全体の全般的な変更ではなく、バインドされた領域内で発生する変更のみがアドインに通知されます。

[Bindings](http://msdn.microsoft.com/ja-jp/library/09979e31-3bfb-45be-adda-0f7cc2db1fe1%28Office.15%29.aspx) オブジェクトが公開している [getAllAsync](http://msdn.microsoft.com/ja-jp/library/ef902b73-cc4c-4551-95de-d8a51eeba82f%28Office.15%29.aspx) メソッドを使用すると、ドキュメントまたはスプレッドシートで確立されている一連のすべてのバインドにアクセスできます。個々のバインドに ID でアクセスするには、 [Bindings.getBindingByIdAsync](http://msdn.microsoft.com/ja-jp/library/2727c891-bc05-465c-9324-113fbfeb3fbb%28Office.15%29.aspx) メソッドまたは [Office.select](http://msdn.microsoft.com/ja-jp/library/23aeb136-da1f-4127-a798-99dc27bc4dae%28Office.15%29.aspx) メソッドを使用します。 **Bindings** オブジェクトのいずれかのメソッド ([addFromSelectionAsync](http://msdn.microsoft.com/ja-jp/library/edc99214-e63e-43f2-9392-97ead42fc155%28Office.15%29.aspx)、 [addFromPromptAsync](http://msdn.microsoft.com/ja-jp/library/9dc03608-b08b-4700-8be1-3c86ae236799%28Office.15%29.aspx)、 [addFromNamedItemAsync](http://msdn.microsoft.com/ja-jp/library/afbadac7-60c7-47cb-9477-6e9466ded44c%28Office.15%29.aspx)、または [releaseByIdAsync](http://msdn.microsoft.com/ja-jp/library/ad285984-8b44-435d-9b84-f0ade570c896%28Office.15%29.aspx)) を使用すると、新しいバインドを確立したり既存のバインドを削除したりできます。

 **addFromSelectionAsync** メソッド、 **addFromPromptAsync** メソッド、または **addFromNamedItemAsync** メソッドでバインドを作成する場合、 _bindingType_ パラメーターで指定するバインドには 3 種類あります。



|**バインドの種類**|**説明**|**ホスト アプリケーションのサポート**|
|:-----|:-----|:-----|
|テキスト バインド|テキストとして表現できるドキュメントの領域にバインドします。|Word では、連続する選択範囲の大部分が有効ですが、Excel では、単一セルの範囲のみがテキスト バインドの対象です。Excel では、プレーン テキストのみがサポートされます。Word では、3 つの形式 (プレーン テキスト、HTML、および Open XML for Office) がサポートされます。|
|マトリックス バインド|ヘッダーがない表形式のデータが含まれるドキュメントの固定領域にバインドします。マトリックス バインド内のデータは、2 次元の  **Array** として書き込みまたは読み取りが行われます。JavaScript では、これは、配列の配列として実装されています。たとえば、2 列の **string** 値が 2 行ある場合は ` [['a', 'b'], ['c', 'd']]` のように書き込みまたは読み取りが行われ、1 列が 3 行ある場合は `[['a'], ['b'], ['c']]` のように書き込みまたは読み取りが行われます。|Excel では、セルの連続する選択範囲を使用してマトリックス バインドを確立できます。Word では、表のみがマトリックス バインドをサポートします。|
|テーブル バインド|ヘッダーがある表が含まれるドキュメントの領域にバインドします。テーブル バインド内のデータは、[TableData](http://msdn.microsoft.com/ja-jp/library/2183ea52-5a40-4048-b9a4-7cd66bb0ad5d%28Office.15%29.aspx) オブジェクトとして書き込みまたは読み取りが行われます。 **TableData** オブジェクトは **headers** および **rows** プロパティを通じてデータを公開します。|Excel または Word の表はすべて、テーブル バインドの基礎にできます。テーブル バインドを確立すると、ユーザーが表に追加する新しい各行または各列が、自動的にバインドに含まれます。 |
 **Bindings** オブジェクトの 3 つの "addFrom" メソッドのいずれかを使用してバインドを作成した後、対応するオブジェクト ([MatrixBinding](http://msdn.microsoft.com/ja-jp/library/35e8568e-9129-4c00-b30f-d8c3b2555f1e%28Office.15%29.aspx)、 [TableBinding](http://msdn.microsoft.com/ja-jp/library/1508795b-1c70-456c-b3bf-666d40cf8f50%28Office.15%29.aspx)、または [TextBinding](http://msdn.microsoft.com/ja-jp/library/6b71b21d-f64d-425c-99d9-c62b2a9969be%28Office.15%29.aspx)) のメソッドのいずれかを使用して、バインドのデータおよびプロパティを操作できます。この 3 つのオブジェクトはすべて、バインド データを操作できる  **Binding** オブジェクトの [getDataAsync](http://msdn.microsoft.com/ja-jp/library/5372ffd8-579d-4fcb-9e5b-e9a2128f3201%28Office.15%29.aspx) メソッドと [setDataAsync](http://msdn.microsoft.com/ja-jp/library/6a59bb6d-40b6-4a95-9b98-d70d4616de09%28Office.15%29.aspx) メソッドを継承しています。


 >**ヒント**   **マトリックスとテーブルのバインドを使用する必要がある場合** **注:** 作業中の表形式のデータに集計行を含めるときに、アドインのスクリプトが集計行の値にアクセスする、または、ユーザーの選択が集計行にあることを検知する必要がある場合は、マトリックス バインドを使用する必要があります。集計行を含む表形式のデータにテーブル バインドを確立する場合、 [TableBinding.rowCount](http://msdn.microsoft.com/ja-jp/library/128d0e04-8ec7-4f52-a31d-421d225c39fa%28Office.15%29.aspx) プロパティおよびイベント ハンドラーの **BindingSelectionChangedEventArgs** オブジェクトの [rowCount](http://msdn.microsoft.com/ja-jp/library/110d45f7-40b7-4005-b080-ef748cbf337c%28Office.15%29.aspx) および [startRow](http://msdn.microsoft.com/ja-jp/library/3cc1c014-b18d-4e7b-9ec0-5500b43c4016%28Office.15%29.aspx) プロパティは、集計行のそれらの値に反映されません。この制限を回避するには、集計行で動作するマトリックス バインドを確立する必要があります。


### ユーザーの現在の選択範囲にバインドを追加する


次の例は、 [Bindings.addFromSelectionAsync](http://msdn.microsoft.com/ja-jp/library/edc99214-e63e-43f2-9392-97ead42fc155%28Office.15%29.aspx) メソッドを使用して、ドキュメントの現在の選択範囲に `myBinding` というテキスト バインドを追加する方法を示しています。


```
Office.context.document.bindings.addFromSelectionAsync(Office.BindingType.Text, { id: 'myBinding' }, function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write('Action failed. Error: ' + asyncResult.error.message);
    } else {
        write('Added new binding with type: ' + asyncResult.value.type + ' and id: ' + asyncResult.value.id);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

この例では、指定したバインドの種類はテキストです。つまり、選択範囲に対して [TextBinding](http://msdn.microsoft.com/ja-jp/library/6b71b21d-f64d-425c-99d9-c62b2a9969be%28Office.15%29.aspx) が作成されます。バインドが備えているデータと操作はバインドの種類ごとに異なります。 [Office.BindingType](http://msdn.microsoft.com/ja-jp/library/e5591c38-806a-423d-b9d1-3041c726d524%28Office.15%29.aspx) は、使用できるバインドの種類の値を示す列挙型です。

2 番目のオプションのパラメーターは、作成している新しいバインドの ID を指定するオブジェクトです。指定しない場合、ID は自動的に生成されます。

最後の  _callback_ パラメーターで関数に渡している匿名関数は、バインドの作成が完了したときに実行されます。この関数には単一のパラメーター _asyncResult_ を渡しており、これを通じて、呼び出しの状態を示す [AsyncResult](http://msdn.microsoft.com/ja-jp/library/540c114f-0398-425c-baf3-7363f2f6bc47%28Office.15%29.aspx) オブジェクトにアクセスできます。 [AsyncResult.value](http://msdn.microsoft.com/ja-jp/library/453a4b43-0fdc-4ea9-967a-c033fab31507%28Office.15%29.aspx) プロパティには、新規作成するバインドとして指定した種類の [Binding](http://msdn.microsoft.com/ja-jp/library/42882642-d22b-47d2-a8d3-3aa8c6a4435e%28Office.15%29.aspx) オブジェクトへの参照が格納されます。この **Binding** オブジェクトを使用して、データを取得および設定できます。


### プロンプトからバインドを追加する


次の例は、Excel 2013 および Excel Onlineでのみサポートされる [Bindings.addFromPromptAsync](http://msdn.microsoft.com/ja-jp/library/9dc03608-b08b-4700-8be1-3c86ae236799%28Office.15%29.aspx) メソッドを使用して、 `myBinding` という名前のテキスト バインドを追加する方法を示しています。このメソッドでは、ユーザーはアプリケーションの組み込み範囲選択プロンプトを使用してバインドの範囲を指定できます。


```
function bindFromPrompt() {
    Office.context.document.bindings.addFromPromptAsync(Office.BindingType.Text, { id: 'myBinding' }, function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Failed) {
            write('Action failed. Error: ' + asyncResult.error.message);
        } else {
            write('Added new binding with type: ' + asyncResult.value.type + ' and id: ' + asyncResult.value.id);
        }
    });
}

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

この例では、指定されているバインドの種類はテキストです。つまり、ユーザーがプロンプトで指定した選択範囲に対して [TextBinding](http://msdn.microsoft.com/ja-jp/library/6b71b21d-f64d-425c-99d9-c62b2a9969be%28Office.15%29.aspx) が作成されます。

2 番目のパラメーターは、作成している新しいバインドの ID を含むオブジェクトです。指定しない場合、ID は自動的に生成されます。

3 番目の  _callback_ パラメーターとして関数に渡される匿名関数は、バインドの作成が完了すると実行されます。コールバック関数が実行されると、 [AsyncResult](http://msdn.microsoft.com/ja-jp/library/540c114f-0398-425c-baf3-7363f2f6bc47%28Office.15%29.aspx) オブジェクトには呼び出しのステータスおよび新しく作成されたバインドが格納されます。

図 1 は、Excel の組み込み範囲選択プロンプトを示しています。


**図 1. Excel のデータ選択 UI**

![Excel のデータ選択 UI](../../images/AgaveAPIOverview_ExcelSelectionUI.png)


### 名前付きアイテムにバインドを追加する


次の例は、 [Bindings.addFromNamedItemAsync](http://msdn.microsoft.com/ja-jp/library/afbadac7-60c7-47cb-9477-6e9466ded44c%28Office.15%29.aspx) メソッドを使用して、既存の `myRange` という名前のアイテムにマトリックス ("matrix") バインドを追加し、そのバインドの **id** に "myMatrix" を割り当てる方法を示しています。


```
function bindNamedItem() {
    Office.context.document.bindings.addFromNamedItemAsync("myRange", "matrix", {id:'myMatrix'}, function (result) {
        if (result.status == 'succeeded'){
            write('Added new binding with type: ' + result.value.type + ' and id: ' + result.value.id);
            }
        else
            write('Error: ' + result.error.message);
    });
}

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}

```

 **Excel の場合**、 **addFromNamedItemAsync** メソッドの _itemName_ パラメーターは、既存の名前付き範囲 (A1 スタイルの参照 ("A1:A3") で指定された範囲) またはテーブルを参照できます。既定では、Excel のテーブルを追加すると、最初に追加したテーブルには "Table1"、次に追加したテーブルには "Table2" という名前が割り当てられます。Excel UI で意味のあるテーブル名を割り当てるには、リボンの [ **テーブル ツール | デザイン**] タブの [ **テーブル名**] プロパティを使用します。


 >**メモ**  Excel 2013 では、テーブルを名前付きアイテムとして指定する場合、 `"Sheet1!Table1"` の形式で完全修飾名を指定して、テーブルの名前にワークシートの名前を含める必要があります。

以下の例では、Excel のバインドを列 A の最初の 3 つのセル ( `"A1:A3"`) に対して作成し、 **id** `"MyCities"` を割り当て、バインドに 3 つの都市名を書き込みます。




```
 function bindingFromA1Range() {
    Office.context.document.bindings.addFromNamedItemAsync("A1:A3", "matrix", {id: "MyCities" },
        function (asyncResult) {
            if (asyncResult.status == "failed") {
                write('Error: ' + asyncResult.error.message);
            }
            else {
                // Write data to the new binding.
                Office.select("bindings#MyCities").setDataAsync([['Berlin'], ['Munich'], ['Duisburg']], { coercionType: "matrix" },
                    function (asyncResult) {
                        if (asyncResult.status == "failed") {
                            write('Error: ' + asyncResult.error.message);
                        }
                    });
            }
        });
}
// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

 **Word の場合**、 **addFromNamedItemAsync** メソッドの _itemName_ パラメーターは、[ **リッチ テキスト**] コンテンツ コントロールの [ **タイトル**] プロパティを参照します。([ **リッチ テキスト**] コンテンツ コントロール以外のコンテンツ コントロールにはバインドできません。)

既定では、コンテンツ コントロールには [ **タイトル**] 値が割り当てられません。Word UI で意味のあるテーブル名を割り当てるには、リボンの [ **開発者**] タブの [ **コントロール**] グループから [ **リッチ テキスト**] コンテンツ コントロールを挿入した後、[ **コントロール**] グループの [ **プロパティ**] コマンドを使用して [ **コンテンツ コントロールのプロパティ**] ダイアログ ボックスを表示します。次に、コンテンツ コントロールの [ **タイトル**] プロパティに、コードから参照する名前を設定します。

次の例では、 `"FirstName"` という名前のリッチ テキスト コンテンツ コントロールに Word のテキスト バインドを作成し、 **id** `"firstName"` を割り当て、その情報を表示します。




```
function bindContentControl() {
    Office.context.document.bindings.addFromNamedItemAsync('FirstName', 
        Office.BindingType.Text, {id:'firstName'},
        function (result) {
            if (result.status === Office.AsyncResultStatus.Succeeded) {
                write('Control bound. Binding.id: '
                    + result.value.id + ' Binding.type: ' + result.value.type);
            } else {
                write('Error:', result.error.message);
            }
    });
}
// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```


### すべてのバインドを取得する


次の例は、 [Bindings.getAllAsync](http://msdn.microsoft.com/ja-jp/library/ef902b73-cc4c-4551-95de-d8a51eeba82f%28Office.15%29.aspx) メソッドを使用して、ドキュメント内のすべてのバインドを取得する方法を示しています。


```
Office.context.document.bindings.getAllAsync(function (asyncResult) {
    var bindingString = '';
    for (var i in asyncResult.value) {
        bindingString += asyncResult.value[i].id + '\n';
    }
    write('Existing bindings: ' + bindingString);
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

 _callback_ パラメーターとして関数に渡される匿名関数は、操作の完了時に実行されます。この関数は、ドキュメント内のバインドの **array** が格納される _asyncResult_ という 1 つのパラメーターを使用して呼び出されます。配列は反復処理されて、バインドの ID が格納される文字列が作成されます。この文字列がメッセージ ボックスに表示されます。


### Bindings オブジェクトの getByIdAsync メソッドを使用して ID でバインドを取得する


次の例は、 [Bindings.getByIdAsync](http://msdn.microsoft.com/ja-jp/library/2727c891-bc05-465c-9324-113fbfeb3fbb%28Office.15%29.aspx) メソッドを使用し、ID を指定してドキュメント内のバインドを取得する方法を示しています。この例では、前述のメソッドのいずれかを使用して `'myBinding'` という名前のバインドがドキュメントに追加されたと想定しています。


```
Office.context.document.bindings.getByIdAsync('myBinding', function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write('Action failed. Error: ' + asyncResult.error.message);
    } 
    else {
        write('Retrieved binding with type: ' + asyncResult.value.type + ' and id: ' + asyncResult.value.id);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

この例で、最初の  _id_ パラメーターは取得するバインドの ID です。

2 番目の  _callback_ パラメーターとして関数に渡される匿名関数は、操作の完了時に実行されます。この関数は、呼び出しのステータスおよび ID が "myBinding" であるバインドが格納される _asyncResult_ という 1 つのパラメーターを使用して呼び出されます。


### Office オブジェクトの select メソッドを使用して ID でバインドを取得する


次の例は、 [Office.select](http://msdn.microsoft.com/ja-jp/library/23aeb136-da1f-4127-a798-99dc27bc4dae%28Office.15%29.aspx) メソッドを使用して、セレクター文字列に ID を指定することによってドキュメント内の **Binding** オブジェクトの promise を取得する方法を示しています。その後、 [Binding.getDataAsync](http://msdn.microsoft.com/ja-jp/library/5372ffd8-579d-4fcb-9e5b-e9a2128f3201%28Office.15%29.aspx) メソッドを呼び出して、指定したバインドからデータを取得します。この例では、前述のメソッドのいずれかを使用して `'myBinding'` という名前のバインドがドキュメントに追加されたと想定しています。


```
Office.select("bindings#myBinding", function onError(){}).getDataAsync(function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write('Action failed. Error: ' + asyncResult.error.message);
    } else {
        write(asyncResult.value);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```


 >**メモ**   **select** メソッドの promise が正常に **Binding** オブジェクトを返す場合、このオブジェクトは [Binding](http://msdn.microsoft.com/ja-jp/library/42882642-d22b-47d2-a8d3-3aa8c6a4435e%28Office.15%29.aspx) オブジェクトの [getDataAsync](http://msdn.microsoft.com/ja-jp/library/5372ffd8-579d-4fcb-9e5b-e9a2128f3201%28Office.15%29.aspx)、 [setDataAsync](http://msdn.microsoft.com/ja-jp/library/6a59bb6d-40b6-4a95-9b98-d70d4616de09%28Office.15%29.aspx)、 [addHandlerAsync](http://msdn.microsoft.com/ja-jp/library/b9c2f4ea-726c-4b48-a3fb-89beda337a17%28Office.15%29.aspx)、および [removeHandlerAsync](http://msdn.microsoft.com/ja-jp/library/5ae3a860-1fc4-46ce-858e-98545c3e2d77%28Office.15%29.aspx) の 4 つのメソッドのみを公開します。promise が **Binding** オブジェクトを返すことができない場合は、 _onError_ コールバックを使用して [asyncResult.error](http://msdn.microsoft.com/ja-jp/library/51c46d36-972d-4d82-91aa-da99cbeb8d4f%28Office.15%29.aspx) オブジェクトにアクセスし、詳細情報を取得できます。 **select** メソッドによって返される **Binding** オブジェクトの promise によって公開される 4 つのメソッド以外の **Binding** オブジェクトのメンバーを呼び出す必要がある場合は、代わりに [getByIdAsync](http://msdn.microsoft.com/ja-jp/library/2727c891-bc05-465c-9324-113fbfeb3fbb%28Office.15%29.aspx) メソッドを使用します。 [Document.bindings](http://msdn.microsoft.com/ja-jp/library/6512eabc-a177-42da-bc52-99665817515f%28Office.15%29.aspx) プロパティと [Bindings.getByIdAsync](http://msdn.microsoft.com/ja-jp/library/2727c891-bc05-465c-9324-113fbfeb3fbb%28Office.15%29.aspx) を使用して **Binding** オブジェクトを取得します。


### ID でバインドを解除する


次の例は、 [Bindings.releaseByIdAsync](http://msdn.microsoft.com/ja-jp/library/ad285984-8b44-435d-9b84-f0ade570c896%28Office.15%29.aspx) メソッドを使用し、ID を指定してドキュメント内のバインドを解除する方法を示しています。


```
Office.context.document.bindings.releaseByIdAsync('myBinding', function (asyncResult) {
    write('Released myBinding!');
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

例の中で、最初の  _id_ パラメーターは、解除するバインドの ID です、

2 番目のパラメーターとして関数に渡される匿名関数は、操作の完了時に実行されます。この関数は、呼び出しのステータスが格納される  _asyncResult_ という 1 つのパラメーターを使用して呼び出されます。


### バインドからデータを読み取る


次の例は、 [Binding.getDataAsync](http://msdn.microsoft.com/ja-jp/library/5372ffd8-579d-4fcb-9e5b-e9a2128f3201%28Office.15%29.aspx) メソッドを使用して既存のバインドからデータを取得する方法を示しています。


```
myBinding.getDataAsync(function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write('Action failed. Error: ' + asyncResult.error.message);
    } else {
        write(asyncResult.value);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

 `myBinding` は、ドキュメント内の既存のテキスト バインドを格納している変数です。代わりに、 [Office.select メソッド](#office-オブジェクトの-select-メソッドを使用して-id-でバインドを取得する)を使用して ID によってバインドにアクセスし、 **getDataAsync** メソッドの呼び出しを開始 ( `Office.select("bindings#myBindingID").getDataAsync` など) できます。

関数に渡される匿名関数は、操作の完了時に実行されるコールバックです。 [AsyncResult.value](http://msdn.microsoft.com/ja-jp/library/453a4b43-0fdc-4ea9-967a-c033fab31507%28Office.15%29.aspx) プロパティには、 `myBinding` 内のデータが格納されます。その値の型は、バインドの種類により異なります。この例のバインドはテキスト バインドです。そのため、値には文字列が格納されます。マトリックス バインドおよびテーブル バインドを使用して作業する追加の例については、 [Binding.getDataAsync](http://msdn.microsoft.com/ja-jp/library/5372ffd8-579d-4fcb-9e5b-e9a2128f3201%28Office.15%29.aspx) メソッドのトピックを参照してください。


### バインドにデータを書き込む


次の例は、 [Binding.setDataAsync](http://msdn.microsoft.com/ja-jp/library/6a59bb6d-40b6-4a95-9b98-d70d4616de09%28Office.15%29.aspx) メソッドを使用して、既存のバインドにデータを設定する方法を示しています。


```
myBinding.setDataAsync('Hello World!', function (asyncResult) { });
```

 `myBinding` は、ドキュメント内の既存のテキスト バインドを格納している変数です。

この例で、最初のパラメーターは  `myBinding` に設定する値です。これはテキスト バインドのため、値は **string** です。バインドの種類が異なる場合、異なる型のデータが使用されます。

関数に渡される匿名関数は、操作の完了時に実行されるコールバックです。この関数は、結果のステータスが格納される  _asyncResult_ という 1 つのパラメーターを使用して呼び出されます。

 **注意:**Excel 2013 SP1 および Excel Online の関連するビルドのリリースから、 [バインド テーブルでデータの書き込みと更新を行う際に書式設定](../../docs/excel/format-tables-in-add-ins-for-excel.md)ができるようになりました。


### バインド内のデータまたは選択範囲の変更を検出する


次の例は、ID が "MyBinding" であるバインドの [DataChanged](http://msdn.microsoft.com/ja-jp/library/7b9ed4bf-3ce5-44eb-8548-2b081afd868d%28Office.15%29.aspx) イベントにイベント ハンドラーを関連付ける方法を示しています。


```
function addHandler() {
Office.select("bindings#MyBinding").addHandlerAsync(
    Office.EventType.BindingDataChanged, dataChanged);
}
function dataChanged(eventArgs) {
    write('Bound data changed in binding: ' + eventArgs.binding.id);
}
// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

 `myBinding` は、ドキュメント内の既存のテキスト バインドを格納している変数です。

[binding.addHandlerAsync](http://msdn.microsoft.com/ja-jp/library/b9c2f4ea-726c-4b48-a3fb-89beda337a17%28Office.15%29.aspx) メソッドの最初の _eventType_ パラメーターは、サブスクライブするイベントの名前を指定します。 [Office.EventType](http://msdn.microsoft.com/ja-jp/library/82c79659-52da-48b0-92a9-831226eb9a7f%28Office.15%29.aspx) は、使用できるイベントの種類の値の列挙型です。 **Office.EventType.BindingDataChanged** は文字列 `"bindingDataChanged"` と評価されます。

2 番目の  _handler_ パラメーターとして関数に渡される `dataChanged` 関数は、バインド内のデータが変更されたときに実行されるイベント ハンドラーです。この関数は、バインドへの参照が格納される _eventArgs_ という 1 つのパラメーターを使用して呼び出されます。このバインドを使用して、更新されたデータを取得できます。

同様に、バインドの [SelectionChanged](http://msdn.microsoft.com/ja-jp/library/5bcbb5e2-f8e6-48ee-bde0-60d12d43ff5f%28Office.15%29.aspx) イベントにイベント ハンドラーを関連付けることによって、バインド内の選択範囲の変更を検出できます。これを行うには、 **binding.addHandlerAsync** メソッドの _eventType_ パラメーターに **Office.EventType.BindingSelectionChanged** または `"bindingSelectionChanged"` を指定します。

 **addHandlerAsync** メソッドを再び呼び出して、 _handler_ パラメーターに追加のイベント ハンドラー関数を指定すると、特定のイベントに複数のイベント ハンドラーを追加できます。この場合、各イベント ハンドラー関数の名前は一意である必要があります。


### イベント ハンドラーを削除する


イベントのイベント ハンドラーを削除するには、最初の  _eventType_ パラメーターにイベントの種類を指定し、2 番目の _handler_ パラメーターに削除するイベント ハンドラー関数の名前を指定して、 [Binding.removeHandlerAsync](http://msdn.microsoft.com/ja-jp/library/5ae3a860-1fc4-46ce-858e-98545c3e2d77%28Office.15%29.aspx) メソッドを呼び出します。たとえば、次の例では、前のセクションの例で追加した `dataChanged` イベント ハンドラー関数が削除されます。


```
function removeEventHandlerFromBinding() {
    Office.select("bindings#MyBinding").removeHandlerAsync(
        Office.EventType.BindingDataChanged, {handler:dataChanged});
}
```


 >**重要**   **removeHandlerAsync** メソッドを呼び出すときにオプションの _handler_ パラメーターの指定を省略すると、指定された _eventType_ のすべてのイベント ハンドラーが削除されます。


## その他の技術情報



- [作業ウィンドウ アドインとコンテンツ アドインの概要](task-pane-and-content-add-ins.md)
    
- [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)
    
- [Office アドインにおける非同期プログラミング](../../docs/develop/asynchronous-programming-in-office-add-ins.md)
    
- [ドキュメントまたはスプレッドシート内のアクティブな選択範囲へのデータの読み取りおよび書き込み](../../docs/develop/read-and-write-data-to-the-active-selection-in-a-document-or-spreadsheet.md)
    
