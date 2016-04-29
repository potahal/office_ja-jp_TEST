
# テキスト エディターを使用して Word 用や Excel 用の作業ウィンドウ アドインやコンテンツ アドインを作成する
テキスト エディターを使用して簡単な Office アドインを作成します。

 _ **適用対象:** apps for Office?| Excel?| Office Add-ins?| Word_

Excel 2013 または Word 2013 用の最も簡単な Office アドインは、Web ページまたは Web サイトを参照するマニフェスト XML ファイルで構成されます。この記事では、XML マニフェストと HTML ファイルのみで構成される簡単なアドインをテキスト エディターで作成する方法について説明します。

このアドインを実装するには、以下を作成する必要があります。


- HTML ファイル。アドインの UI を実装します。
    
- XML マニフェスト ファイル。Word または Excel でアドインを表示および実行するために必要なメタデータを定義します。
    
- CSS ファイル。アドイン用のスタイル シートを定義します。
    
- project.js ファイル。JavaScript API for Office (Office.js) を使用して Word 文書や Excel ドキュメント内のコンテンツに対してデータ アクセス操作を実行できる JavaScript プログラミング ロジックを記述します。
    

## Office アドイン "Hello World" を作成する


このアドインの UI は、必要に応じて JavaScript プログラミング ロジックを記述できる HTML ファイルによって提供されます。この手順の最初の部分では、プログラミング ロジックを持たない Hello World アドインの作成方法を示します。この Hello World アドインの完成後、文書またはワークシートのコンテンツを対話的に操作するプログラミング ロジックの追加方法を示します。


### Hello World アドイン用のファイルを作成するには


1. HelloWorld という名前のフォルダーをローカル ドライブ上に作成します (例: C:\HelloWorld)。以下の手順で作成するファイルはすべて、このフォルダー内に保存します。
    
2. 以下の HTML コードを記述したファイルを作成し、そのファイルに HelloWorld.html という名前を付けます。
    
  ```HTML
  <!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=Edge"/>
        <link rel="stylesheet" type="text/css" href="program.css" />
    </head>
    <body>
        <p>Hello World!</p>
    </body>
</html>

  ```


    このファイルには、アドインの UI を表示する最小限の HTML タグを記述しています。
    
3. 以下の CSS コードを記述したファイルを作成し、そのファイルに program.css という名前を付けます。
    
  ```
  body
{
    position:relative;
}
li :hover
{  
    text-decoration: underline;
    cursor:pointer;
}
h1,h3,h4,p,a,li
{
    font-family: "Segoe UI Light","Segoe UI",Tahoma,sans-serif;
    text-decoration-color:#4ec724;
}

  ```


    このファイルは、アドイン用のスタイル シートになります。
    
4. 以下の XML コードを記述したファイルを作成し、そのファイルに HelloWorld.xml という名前を付けます。
    
     >**重要**   `<id>` タグの値は、独自の GUID に置き換えます。

  ```XML
  <?xml version="1.0" encoding="utf-8"?>
<OfficeApp xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:type="TaskPaneApp">
  <Id>08afd7fe-1631-42f4-84f1-5ba51e242f98</Id>
  <Version>1.0</Version>
  <ProviderName>Microsoft</ProviderName>
  <DefaultLocale>EN-US</DefaultLocale>
  <DisplayName DefaultValue="Hello World add-in"/>
  <Description DefaultValue="My first app."/>
  <IconUrl DefaultValue=
    "http://officeimg.vo.msecnd.net/_layouts/images/general/office_logo.jpg"/>

  <Hosts>
    <Host Name="Document"/>
    <Host Name="Workbook"/>
  </Hosts>

  <DefaultSettings>
    <SourceLocation DefaultValue="\\MyShare\MyManifests\HelloWorld\HelloWorld.html"/>
  </DefaultSettings>
  <Permissions>ReadWriteDocument</Permissions>
</OfficeApp>

  ```


    このファイルは、アドインのマニフェスト XML ファイルになります。
    
以下の 2 つの手順では、ファイルをネットワーク共有にコピーし、その場所を信頼できるアドイン カタログとして指定して、アドインをテストできるようにする方法について説明します。


### マニフェスト用の信頼できる場所を指定するには


1. ネットワーク共有 (例: \\MyShare\MyManifests) にフォルダーを作成します。HelloWorld.xml マニフェスト ファイルの  `<SourceLocation>` 要素に、アドインの .html ページのこの場所を設定します。作成したファイルはすべてこのフォルダーに保存します。
    
     >**メモ**  または、HelloWorld.xml マニフェスト ファイルのみをこの共有に保存し, .html ファイルは Web サーバー上に置くこともできます。その場合は、HelloWorld.xml マニフェスト ファイルの  `<SourceLocation>` 要素に、そのサーバー上の HelloWorld.html ファイルの URL を設定します。
2. Excel または Word で新しいドキュメントまたは文書を開きます。
    
3. [ **ファイル**] タブを選択し、[ **オプション**] を選択します。
    
4. [ **セキュリティ センター**] を選択し、[ **セキュリティ センターの設定**] ボタンを選択します。
    
5. [ **信頼されているアドイン カタログ**] を選択します。
    
6. [ **カタログの URL**] ボックスで、手順 1. で作成したネットワーク共有のパスを入力し、[ **カタログの追加**] を選択します。
    
7. [ **メニューに表示する**] チェック ボックスをオンにし、[ **OK**] を選択します。
    
    これらの設定は Office を次回起動したときに適用されることを示すメッセージが表示されます。
    
8. Excel または Word を終了して再起動します。
    

### Hello World アドインをテストおよび実行するには


1. [ **挿入**] タブで [ **個人用アドイン**] を選択します。 
    
2. [ **Office アドイン**] ダイアログ ボックスで [ **共有フォルダー**] を選択します。
    
3. [ **Hello World アドイン**]、[ **開始**] の順に選択します。
    
    現在の文書またはワークシートの右側にある作業ウィンドウにアドインが表示されます。
    

## Hello World アドインにプログラミング ロジックを追加する


次の手順では、Hello World アドインに基本的なプログラミング ロジックを追加して、このアドインが文書またはワークシートのコンテンツを対話的に操作できるようにする方法について説明します。


### Hello World アドインにプログラミング ロジックを追加するには


1. HelloWorld.html ファイルを開いて、 `<head>` タグの内側に `<script>` タグを追加します。
    
  ```HTML
  <!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=Edge"/>
        <link rel="stylesheet" type="text/css" href="program.css" />
        <script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js"></script>
        <script src="http://ajax.aspnetcdn.com/ajax/jquery/jquery-1.9.0.min.js"></script>
        <script src="Program.js"></script>
    </head>
    <body>
        <p>Hello World!</p>
    </body>
</html>
  ```


    これにより、JavaScript API for Office を実装している Office.js ライブラリ ファイルへの参照が追加されます。また、Program.js への参照も追加されます。Program.js は、アドイン用のプログラミング ロジックを記述したファイルです。このファイルは後で作成します。
    
     >**メモ**   `<script>` タグの `src` 属性は、外部で使用可能になる JavaScript API for Office (office.js) を参照しています。
2.  `<p>Hello World!</p>` の部分を、ファイルの `<body>` タグの内側にある行に置き換えます。
    
  ```HTML
  <!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=Edge"/>
        <link rel="stylesheet" type="text/css" href="program.css" />
        <script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js"></script>
        <script src="Program.js"></script>
    </head>
    <body>
        <button onclick="writeData()"> Write Data </button></br>
        <button onclick="ReadData()"> Read Selected Data </button></br>
        Results: <div id="results"></div>
    </body>
</html>

  ```


    これにより、アドインの UI に 2 つのボタンが追加され、結果を表示するための  `div` が定義されます。
    
3. 以下の JavaScript コードを記述したファイルを作成し、そのファイルに Program.js という名前を付けます。
    
     >**メモ**  [Office.initialize](http://msdn.microsoft.com/ja-jp/library/727adf79-a0b5-48d2-99c7-6642c2c334fc%28Office.15%29.aspx) は、コード ファイルの冒頭部分で関数として初期化する必要があります。これを初期化しておくと、次のような関数から呼び出されたときに [Office.context](http://msdn.microsoft.com/ja-jp/library/6c4b2c16-d4fb-4ecf-b72c-1e33b205daaf%28Office.15%29.aspx) プロパティが使用できます。

  ```
  // The initialize function is required for all add-ins.
Office.initialize = function (reason) {
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
    // After the DOM is loaded, app-specific code can run.
    // Add any initialization logic to this function.
    });
}
var MyArray = [['Berlin'],['Munich'],['Duisburg']];

function writeData() {
    Office.context.document.setSelectedDataAsync(MyArray, { coercionType: 'matrix' });
}

function ReadData() {
    Office.context.document.getSelectedDataAsync("matrix", function (result) {
        if (result.status === "succeeded"){
            printData(result.value);
        }

        else{
            printData(result.error.name + ":" + err.message);
        }
    });
}

      function printData(data) {
    {
        var printOut = "";

        for (var x = 0 ; x < data.length; x++) {
            for (var y = 0; y < data[x].length; y++) {
                printOut += data[x][y] + ",";
            }
        }
       document.getElementById("results").innerText = printOut;
    }
}

  ```

4. 「マニフェスト用の信頼できる場所を指定するには」の手順に従って、アドイン ファイルを再度展開します。
    
5. 「Hello World アドインをテストおよび実行するには」の手順に従って、アドインを挿入してテストします。
    

## その他のリソース



- [作業ウィンドウ アドインとコンテンツ アドインの概要](task-pane-and-content-add-ins.md)
    
- [Office アドインの設計ガイドライン](../add-in-design.md)
    
- [Office アドインの開発ライフ サイクル](../../docs/design/add-in-development-lifecycle.md)
    
- [Office アドインを発行する](../publish/publish.md)
    
- [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)
    
- [Office アドインの XML マニフェスト](../../docs/overview/add-in-manifests.md)
    
- [Office アドインの API とスキーマ参照](../../reference/reference.md)
    
