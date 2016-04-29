
# Excel 用アドインで表の書式設定を行う
Excel 用アドインでテーブルの書式を指定する方法について説明します。

 _ **適用対象:** apps for Office?| Excel?| Office Add-ins_

この記事では、書式設定 API のさまざまな機能を説明し、それらの使用方法について概説します。このリリースでは、(  **Office.CoercionType.Text** または **Office.CoercionType.Matrix** データ構造ではなく) テーブルのみにセルの書式設定とその他のオプションをプログラムで指定でき、また Excel アドインでのみこれを行うことができます。アドインで書式設定を行うには、

- ユーザーがテーブル (またはプログラムでテーブルを挿入する場所) を選択します。その後、書式を設定するテーブルに対してアドインで  **Document.setSelectedDataAsync** メソッドを呼び出すことができます。
    
    または…
    
- ブックにバインドされたテーブルが既に含まれている場合 (またはアドインが [Bindings](http://msdn.microsoft.com/ja-jp/library/09979e31-3bfb-45be-adda-0f7cc2db1fe1%28Office.15%29.aspx) オブジェクトの "addFrom" メソッドのいずれかを使用して、アドインの初期化時にバインドされたテーブルを作成する場合)、アドインは、バインドされたテーブルで書式設定を行うために **Binding.setDataAsync** メソッドを呼び出すことができます。
    
 **重要:** Excel アドインで表の書式設定をする新しいメソッドおよび更新されたメソッドを使用するためには、アドインのプロジェクトで [ Office.js v1.1 以上を使用するか、その使用のためにプロジェクトを更新する](../docs/develop/update-your-javascript-api-for-office-and-manifest-schema-version.md)必要があります。

## 書式を指定する

設定する書式を指定するには、キーと値のペアを 1 つ以上含む JavaScript オブジェクト リテラルを作成します。一連の書式設定を JavaScript オブジェクト内のリストにまとめることができます。以下に例を示します。 


```
var myFormat = {fontStyle:"bold", width:"autoFit", borderColor:"purple"};
```

書式設定を適用するために、JavaScript オブジェクトをデータの書式設定やテーブルのその他の機能をサポートするメソッドのいずれかに渡します。

書式設定は次の 2 つの方法で操作できます。


- アドインは、初めてデータを選択範囲またはバインドに書き込む際、 [Document.setSelectedDataAysnc](http://msdn.microsoft.com/ja-jp/library/998f38dc-83bd-4659-a759-4758c632a6ef%28Office.15%29.aspx) メソッドまたは [Binding.setDataAsync](http://msdn.microsoft.com/ja-jp/library/6a59bb6d-40b6-4a95-9b98-d70d4616de09%28Office.15%29.aspx) メソッドに渡される _options_ オブジェクトにあるオプションの _cellFormat_ パラメーターまたは _tableOptions_ パラメーターを指定します。
    
- 書式設定した後は、 [書式設定をクリアまたは更新する](#書式設定の更新とクリア)ための専用の新しいメソッドのいずれかを使用して、書式設定をクリアまたは更新できます。
    

## データ設定メソッドでオプションのパラメーターを使用する

テーブル バインディングでは、 _tableOptions_ と _cellFormat_ というオプション パラメーターを使用して **Document.setSelectedData** メソッドと **Binding.setDataAsync** メソッドのどちらかを使用してデータを設定するときに書式設定を指定できます。


### tableOptions オプション パラメーター

 _tableOptions_ オプション パラメーターは、既定のテーブル スタイルを指定し、 **見出し行**、 **集計行**、 **縞模様行**などの特定のテーブル機能をオンまたはオフにするために使用します。 _tableOptions_ パラメーターとして渡す値はキーと値のペアのリストを含む JavaScript オブジェクトです。以下に例を示します。


```
tableOptions: {bandedRows: true, filterButton: false, style:"TableStyleMedium3"};
```


### cellFormat オプション パラメーター

 _cellFormat_ オプション パラメーターは、幅、高さ、フォント、背景、配置などのセルの書式設定値を変更するために使用します。 _cellFormat_ パラメーターとして渡す値は対象となるセルとそれらに適用する書式を指定する JavaScript オブジェクトのリストを含む配列です。以下に例を示します。


```
cellFormat: 
    [{cells: {row: 1}, format: {fontColor: "yellow"}}, 
        {cells: Office.Table.Headers, format: {fontColor: "yellow"}}, 
        {cells: {row: 3, column: 4}, format: {borderColor: "white", fontStyle: "bold"}]
```

複数の  `cells:` と `format:` のペアを _cellFormat_ 配列にまとめることによって、書式設定の適用に必要な関数呼び出しの回数を最低限に抑えることができます。


#### cells

 `cells:` は、書式設定を適用する列、行、およびセルの範囲を指定するために使用します。


**サポートされている cells 値の範囲**


|**cells の範囲の設定**|**説明**|
|:-----|:-----|
| `{row: i}`|テーブル内の i データ行までの範囲を指定します。|
| `{column: i}`|テーブル内の i データ列までの範囲を指定します。|
| `{row: i, column: j}`|テーブル内の i データ行から j データ列までのセルの範囲を指定します。|
| `Office.Table.All`|列見出し、データ、集計 (もしあれば) を含むテーブル全体を指定します。|
| `Office.Table.Data`|テーブル内のデータのみ (見出しと集計を含まない) を指定します。|
| `Office.Table.Headers`|見出し行のみを指定します。|

#### format

 `format:` を使用して、 `cells:` で定義されている範囲に適用する書式を JavaScript のキーと値のペアのリストとして指定します。サポートされる値のリストについては、「 [サポートされている書式設定のキーと値](#サポートされている書式設定のキーと値)」を参照してください。

 **Excel Online の書式設定を指定する際の制限**

Excel Online で書式設定をする場合、 _cellFormat_ パラメーターに渡される _書式設定グループ_ の数は 100 を超えることはできません。1 つの書式設定グループは、指定したセル範囲に適用される一連の書式設定で構成されます。(言い換えれば、 _cellFormat_ に渡される配列内のいずれかのオブジェクト リテラル `cells:` にすべてが指定されます。) たとえば、次の呼び出しでは、2 つの書式設定グループを _cellFormat_ に渡しています。




```
Office.context.document.setSelectedDataAsync(
    {cellFormat:[{cells: {row: 1}, format: {fontColor: "yellow"}}, 
        {cells: {row: 3, column: 4}, format: {borderColor: "white", fontStyle: "bold"}}]}, 
    function (asyncResult){});
```


#### オプション パラメーターを適用する

このリリースでは、 **Document.setSelectedDataAsync** メソッドと **TableBinding.setDataAsync** メソッドだけが、 _tableOptions_ オプション パラメーターと _cellFormat_ オプション パラメーターを使用した同じ呼び出し内のテーブルへのデータの書き込みと書式設定をサポートします。次の例では、各メソッドの最初のパラメーター ( _data_ パラメーター) に渡される `tableData` の値は、書き込まれるテーブルとデータの定義を含む [TableData](http://msdn.microsoft.com/ja-jp/library/2183ea52-5a40-4048-b9a4-7cd66bb0ad5d%28Office.15%29.aspx) オブジェクトでなければなりません。

 **Document.setSelectedDataAsync の例**




```
Office.context.document.setSelectedDataAsync(tableData, 
    {tableOptions: {headerRow:false}, 
        cellFormat: [{cells: Office.Table.Headers, format: {fontColor: "yellow"}}]}, 
    function (asyncResult) {});
```

 **TableBinding.setDataAsync の例**




```
Office.select("bindings#myBinding").setDataAsync(tableData, 
    {tableOptions: {headerRow:false}, 
        cellFormat: [{cells: Office.Table.Headers, format: {fontColor: "yellow"}}]}, 
    function (asyncResult) {});
```

 **注:** `Office.select("bindings#myBinding")` への呼び出しは、 `myBinding` という名前のバインディングがすでにワークシートに存在することを前提としています。


## 書式設定の更新とクリア


 **Document.setSelectedDataAsync** メソッドまたは **TableBinding.setDataAsync** メソッドの _cellFormat_ オプション パラメーターと _tableOptions_ オプション パラメーターを使用して書式を設定する場合は、それらを初めて呼び出した場合にのみ書式設定が実行されます。書式設定を更新またはクリアするには、 **TableBinding** オブジェクトの 3 つの新しいメソッド ( **setFormatsAsync**、 **setTableOptionsAsync**、および  **clearFormatsAsync**) を使用する必要があります。


### 書式の更新

[TableBinding.setFormatsAsync](http://msdn.microsoft.com/ja-jp/library/49712906-f582-4055-9ef8-6edde6e97679%28Office.15%29.aspx) メソッドは、幅、高さ、フォント、背景、配置などのセルの書式設定の更新専用です。このメソッドは、必須パラメーターとして _cellFormat_ を使用します。


```
Office.select("bindings#myBinding").setFormatsAsync(
    [{cells: {row: 1}, format: {fontColor: "yellow"}}, 
        {cells: {row: 3, column: 4}, format: {borderColor: "white", fontStyle: "bold"}}], 
    function (asyncResult){});
```

[TableBinding.setTableOptionsAsync](http://msdn.microsoft.com/ja-jp/library/2885fc57-4527-4ca4-a43d-9ee447ec27d3%28Office.15%29.aspx) メソッドは、縞模様行やフィルター ボタンなどのテーブル オプションの更新専用です。このメソッドは、必須パラメーターとして _tableOptions_ を使用します。




```
var tableOptions = {bandedRows: true, filterButton: false, style: "TableStyleMedium3"}; 

Office.select("bindings#myBinding").setTableOptionsAsync(tableOptions, function(asyncResult){});
```


### 書式のクリア

[TableBinding.clearFormatsAsync](http://msdn.microsoft.com/ja-jp/library/cc56e9c0-b33c-4d9b-b676-a7e50f757c10%28Office.15%29.aspx) メソッドは、テーブル内のすべての書式設定をクリアするためのものです。このメソッドは、 _asyncContext_ オプション パラメーターとオプション コールバック関数を使用します。


```
Office.select("bindings#myBinding").clearFormatsAsync();
```


## サポートされている書式設定のキーと値


下の表は、 _cellFormat_ パラメーターまたは _tableOptions_ パラメーターに渡すことができる、サポートされているキーと値のペアを示しています。

 `format:` の値で指定可能な設定は、[ **セルの書式設定**] ダイアログ ボックスのサブセットに該当します (右クリック > [ **セルの書式設定**] またはリボンの [ **ホーム**] タブの [ **書式**] > [ **セルの書式設定**])。 `tableOptions:` の値については、リボンの [ **テーブル ツール**] | [ **デザイン**] タブにある [ **テーブル スタイルのオプション**] グループと [ **テーブル スタイル**] グループに表示される設定に対応します。


 >**重要**  書式設定 API のメソッドは、以下に要約されたオプションと値のみをサポートします。これ以外の書式設定のオプションまたは値を指定した場合、処理の動作は未定義です。これらの未定義の処理動作は、サポートされるプラットフォーム間で必ずしも一致しているわけではありません。したがって、どのプラットフォームであっても、これらの未定義の動作の副次的影響に基づいてアドインを開発しないでください。ただし、この未定義の処理動作は、アドインの状態や UI また相互作用するドキュメントに悪影響を与えることはありません。


**配置**


|**キー**|**値**|**備考**|
|:-----|:-----|:-----|
| `alignHorizontal:`|"標準" | "左詰め" | "中央揃え" | "右詰め" | "繰り返し" | "両端揃え" | "選択範囲内で中央" | "均等割り付け"|インデント値と組み合わせる場合は、以下の組み合わせのみがサポートされます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="code">alignHorizontal: "left"</span> および <span class="code">indentLeft: <value></span></p></li><li><p><span class="code">alignHorizontal: "right"</span> および <span class="code">indentRight: <value></span></p></li><li><p><span class="code">alignHorizontal: "distributed"</span> および <span class="code">indentDistributed: <value></span></p></li></ul>|
| `alignVertical:`|"上詰め" | "中央揃え" | "下詰め" | "両端揃え" | "均等割り付け"||



**背景**


|**キー**|**値**|**備考**|
|:-----|:-----|:-----|
| `backgroundColor:`|"なし" | <定義済みの色の名前> | #RRGGBB|定義済みの色の名前:"黒"、"青"、"灰"、"緑"、"オレンジ"、"ピンク"、"紫"、"赤"、"ティール"、"青緑"、"スミレ"、"白"、"黄"|



**罫線**


|**キー**|**値**|**備考**|
|:-----|:-----|:-----|
| `borderStyle:`|"なし" | <定義済みの線のスタイルの名前>|定義済みの線のスタイルの名前前:"一点鎖線"、"二点鎖線"、"破線"、"点線"、"二重線"、"細線"、"実線 (一点鎖点)"、"実線 (二点鎖線)"、"実線 (破線)"、"実線"、"傾斜 (一点鎖線)"、"極太線"、"実線"指定された範囲内のすべての罫線に適用されます ([ **セルの書式設定**] ダイアログ ボックスの [ **罫線**] タブで [ **外枠**] と [ **内側**] の両方のプリセットを使用して線のスタイルを指定した場合と等価です)。 **注意:** Excel 2013 は、定義済みの 13 の境界線スタイルすべての描画をサポートしていますが、Excel Online ではサポートされない境界線スタイルがあります。次の表は、Excel Online でスプレッドシートを開いたときに各境界線スタイルに使用される描画について説明しています。

|**Excel 2013**|**Excel Online**|
|:-----|:-----|
|「一点鎖線」|破線 (1px)|
|「二点鎖線」|点線 (1px)|
|「破線」|点線 (1px)|
|「点線」|破線 (1px)|
|「二重線」|二重線 (3px)|
|「細線」|実線 (1px)|
|「実線 (一点鎖線)」|破線 (2px)|
|「実線 (二点鎖線)」|点線 (2px)|
|「実線 (破線)」|破線 (2px)|
|「並太線」|実線 (2px)|
|「傾斜 (一点鎖線)」|破線 (2px)|
|「太線」|実線 (3px)|
|「細線」|実線 (1px)|
|
| `borderColor:`|"自動" | <定義済みの色の名前> | #RRGGBB|指定された範囲内のすべての罫線に適用されます。|
| `borderTopStyle:`|"なし" | <定義済みの線のスタイルの名前>|指定された範囲内のすべての罫線に適用されます。|
| `borderTopColor:`|"自動" | <定義済みの色の名前> | #RRGGBB|指定された範囲内のすべての罫線に適用されます。|
| `borderBottomStyle:`|"なし" | <定義済みの線のスタイルの名前>|指定された範囲内のすべての罫線に適用されます。|
| `borderBottomColor:`|"自動" | <定義済みの色の名前> | #RRGGBB|指定された範囲内のすべての罫線に適用されます。|
| `borderLeftStyle:`|"なし" | <定義済みの線のスタイルの名前>|指定された範囲内のすべての罫線に適用されます。|
| `bordeerLeftColor:`|"自動" | <定義済みの色の名前> | #RRGGBB|指定された範囲内のすべての罫線に適用されます。|
| `borderRightStyle:`|"なし" | <定義済みの線のスタイルの名前>|指定された範囲内のすべての罫線に適用されます。|
| `borderRightColor:`|"自動" | <定義済みの色の名前> | #RRGGBB|指定された範囲内のすべての罫線に適用されます。|
| `borderOutlineStyle:`|"なし" | <定義済みの線のスタイルの名前>|指定された範囲内のすべての罫線に適用されます。|
| `borderOutlineColor:`|"自動" | <定義済みの色の名前> | #RRGGBB|指定された範囲内のすべての罫線に適用されます。|
| `borderInlineStyle:`|"なし" | <定義済みの線のスタイルの名前>|指定された範囲内の内側の罫線にのみ適用されます ([ **セルの書式設定**] ダイアログ ボックスの [ **罫線**] タブで [ **内側**] プリセットのみを使用して線のスタイルを指定した場合と等価です)。|
| `borderInlineColor:`|"自動" | <定義済みの色の名前> | #RRGGBB|指定された範囲内の内側の罫線にのみ適用されます。 |



**セルの幅、高さ、および折り返し**


|**キー**|**値**|
|:-----|:-----|
| `width:`|"自動配置" |  **Number**|
| `height:`|"自動配置" |  **Number**|
| `wrapping:`|**Boolean**|



**フォント**


|**キー**|**値**|**備考**|
|:-----|:-----|:-----|
| `fontFamily:`|<使用可能なすべてのフォント名>|Excel Online にフォントを設定する際、ブラウザーでフォントが使用できない場合、API は、Segoe UI、Thonburi、Arial、Verdana、Microsoft Sans Serif の順序でフォントへのフォール バックを試みます。これらのいずれのフォントも使用できない場合は、ブラウザーの既定のフォントが使用されます。|
| `fontStyle:`|"標準" | "斜体" | "太字" | "太字 斜体"|
 >**メモ**  この記事の発行時点では、 `fontStyle:` を "斜体" に設定し、その後で "太字"(またはその逆) に設定すると、この 2 つの設定の和集合として動作します。たとえば、最初に "斜体" に設定し、後から "太字" に設定すると、結果は "太字斜体" になります。以前に太字または斜体に設定した範囲を、斜体か太字のいずれか一方 _のみ_ に設定するには、最初に `fontStyle:"regular"` を設定して以前の書式設定をクリアする必要があります。

|
| `fontSize:`|**Number**||
| `fontUnderlineStyle:`|"なし" | "下線" | "二重下線" | "下線 (会計)" | "二重下線 (会計)"||
| `fontColor:`|"自動" | <定義済みの色の名前> | #RRGGBB||
| `fontDirection:`|"対象" | "左から右" | "右から左"|現在、Excel Online は右から左へのテキスト表示をサポートしていません。ただし、アドインが Excel Online で実行しているときに  `fontDirection:` が [右から左] に設定されている場合、その書式設定はブック ファイルに保存され、ブックがデスクトップの Excel で開かれたときに正しく表示されます。|
| `fontStrikethrough:`|**Boolean**||
| `fontSuperscript:`|**Boolean**||
| `fontSubScript:`|**Boolean**||
| `fontNormal:`|**Boolean**|フォント、フォント スタイル、サイズ、および文字飾りを標準スタイルに設定します。これにより、セル フォントの書式設定が既定値にリセットされます。 [ **セルの書式設定**] ダイアログ ボックスの [ **フォント**] タブで [ **標準フォント**] チェック ボックスをオンにした場合と等価です。|



**インデント**


|**キー**|**値**|**備考**|
|:-----|:-----|:-----|
| `indentLeft:`|**Number**|配置の値と組み合わせる場合は、以下の組み合わせのみがサポートされます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="code">alignHorizontal: "left"</span> および <span class="code">indentLeft: <value></span></p></li></ul>|
| `indentRight:`|**Number**|配置の値と組み合わせる場合は、以下の組み合わせのみがサポートされます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="code">alignHorizontal: "right"</span> および <span class="code">indentRight: <value></span></p></li></ul>|
| `indentDistributed:`|**Number**|配置の値と組み合わせる場合は、以下の組み合わせのみがサポートされます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="code">alignHorizontal: "distributed"</span> および <span class="code">indentDistributed: <value></span></p></li></ul>|



**数値の書式**


|**キー**|**値**|**備考**|
|:-----|:-----|:-----|
| `numberFormat:`|**String**|数値の書式を指定するには、カスタム表示形式文字列を使用します。たとえば、桁区切り記号としてカンマを使用して小数点以下第 2 位までを指定するには、次のように指定します。  `numberFormat:"#,###.00"`これらは、[[セルの書式設定] ダイアログ ボックスの [表示形式] タブにあるユーザー定義の表示形式分類を使用して作成する](http://office.microsoft.com/ja-jp/excel-help/create-or-delete-a-custom-number-format-HA102749035.aspx?CTT=1)ことが可能なものと同じユーザー定義の表示形式文字列です。 **ヒント:** 次の手順を実行すると、Excel の [ **セルの書式設定**] ダイアログ ボックスの標準分類として書式設定文字列がどのように表示されるかを確認できます。 
<ol xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>[<span class="ui">分類</span>] リストから [<span class="ui">通貨</span>] などの標準表示形式分類を選択します。</p></li><li><p>ダイアログ ボックスの右側で表示形式のオプションを設定します。</p></li><li><p>[<span class="ui">ユーザー定義</span>] 分類を選択すると、[<span class="ui">種類</span>] リストの一番上に表示形式文字列が表示されます。</p></li></ol>|



**テーブル オプション**


|**キー**|**値**|**備考**|
|:-----|:-----|:-----|
| `style:`|"なし" | <定義済みのテーブル スタイル名>|定義済みのテーブル スタイル名:"TableStyleLight1"、"TableStyleLight2"、"TableStyleLight3"、"TableStyleLight4"、"TableStyleLight5"、"TableStyleLight6"、"TableStyleLight7"、"TableStyleLight8"、"TableStyleLight9"、"TableStyleLight10"、"TableStyleLight11"、"TableStyleLight12"、"TableStyleLight13"、"TableStyleLight14"、"TableStyleLight15"、"TableStyleLight16"、"TableStyleLight17"、"TableStyleLight18"、"TableStyleLight19"、"TableStyleLight20"、"TableStyleLight21"、"TableStyleMedium1"、"TableStyleMedium2"、"TableStyleMedium3"、"TableStyleMedium4"、"TableStyleMedium5"、"TableStyleMedium6"、"TableStyleMedium7"、"TableStyleMedium8"、"TableStyleMedium9"、"TableStyleMedium10"、"TableStyleMedium11"、"TableStyleMedium12"、"TableStyleMedium13"、"TableStyleMedium14"、"TableStyleMedium15"、"TableStyleMedium16"、"TableStyleMedium17"、"TableStyleMedium18"、"TableStyleMedium19"、"TableStyleMedium20"、"TableStyleMedium21"、"TableStyleMedium22"、"TableStyleMedium23"、"TableStyleMedium24"、"TableStyleMedium25"、"TableStyleMedium26"、"TableStyleMedium27"、"TableStyleMedium28"、"TableStyleDark1"、"TableStyleDark2"、"TableStyleDark3"、"TableStyleDark4"、"TableStyleDark5"、"TableStyleDark6"、"TableStyleDark7"、"TableStyleDark8"、"TableStyleDark9"、"TableStyleDark10"、"TableStyleDark11"テーブル スタイルの外観を表示するには、Excel でテーブルを挿入して、[ **テーブル ツール**] | [ **デザイン**] タブで [ **クイック スタイル**] ドロップダウンを選択してから、定義済みのスタイルを選択します。スタイルに関するツールヒントは上記リスト内の値のいずれかに対応します。|
| `headerRow:`|**Boolean**||
| `firstColumn:`|**Boolean**||
| `filterButton:`|**Boolean**||
| `totalRow:`|**Boolean**||
| `lastColumn:`|**Boolean**||
| `bandedRows:`|**Boolean**||
| `bandedColumns:`|**Boolean**||
