
# JavaScript API for Office およびマニフェスト スキーマ ファイルのバージョンを更新する
Office アドイン プロジェクトで使用する JavaScript API for Office ファイル (Office.js ファイルとアプリに固有の .js ファイル) とアドイン マニフェスト検証ファイル (offappmanifest-1.1.xsd) をバージョン 1.1 に更新します。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_


## 最新のプロジェクト ファイルを使用する


Visual Studio を使用して アドイン を開発する場合、JavaScript API for Office の [最新の API メンバー](http://msdn.microsoft.com/ja-jp/library/802cf4ae-7c18-4e7d-b4d6-ecaa84c569bc%28Office.15%29.aspx)および [アドイン マニフェストの v1.1 の機能](../../docs/overview/add-in-manifests.md) (offappmanifest-1.1.xsd に対して検証される) を使用するには、 [Visual Studio 2015 および最新の Office 開発者ツール](https://www.visualstudio.com/features/office-tools-vs)をダウンロードしてインストールする必要があります。

テキスト エディター、または Visual Studio 以外の IDE を使用してアドインを開発する場合は、Office.js に対する CDN への参照と、アドインのマニフェストで参照するスキーマのバージョンを更新する必要があります。

Office.js の新しい API や更新された API とアドインのマニフェスト機能を使用して開発したアドインを実行するには、ユーザー側で Office 2013 SP1 以降のオンプレミスの製品を実行し、該当する場合は SharePoint Server 2013 SP1 と関連するサーバー製品、Exchange Server 2013 Service Pack 1 (SP1)、または同等のオンライン ホスト製品である Office 365、SharePoint Online、および Exchange Online を実行している必要があります。

Office、SharePoint、Exchange SP1 の各製品ををダウンロードするには、次を参照してください。


- [Microsoft Office 2013 および関連のデスクトップ製品のすべての Service Pack 1 (SP1) の更新プログラムの一覧](https://support.microsoft.com/ja-jp/kb/2850036)
    
- [製品 Microsoft SharePoint Server 2013 と関連するサーバー製品のすべての Service Pack 1 (SP1) の更新プログラムの一覧](http://support.microsoft.com/kb/2850035)
    
- [Exchange Server 2013 Service Pack 1 の説明 ](https://support.microsoft.com/ja-jp/kb/2926248)
    

## Visual Studio で作成した Office アドイン プロジェクトを最新バージョンの JavaScript API for Office ライブラリとバージョン 1.1 アドイン マニフェスト スキーマを使用するように更新する


JavaScript API for Office とアドイン マニフェスト スキーマの v1.1 のリリース前に作成されたプロジェクトの場合は、 **NuGet パッケージ マネージャー**を使用してプロジェクトのファイルを更新してから、それらを参照するようにアドインの HTML ページを更新できます。 

なお、この更新プロセスは _プロジェクトごと_ に適用する必要があることに注意してください。v1.1 の Office.js とアドイン マニフェスト スキーマを使用するアドイン プロジェクトごとに、この更新プロセスを繰り返します。




### プロジェクトの JavaScript API for Office ライブラリ ファイルを最新のリリースに更新するには


1. Visual Studio 2015 で、 **Office アドイン**プロジェクトを開くか新規作成します。
    
      - 左側のウィンドウで、 **[更新]** をクリックしてパッケージの更新プロセスを実行します。
    
  - 手順 6 に進みます。
    
2. [ **ツール**] > [ **NuGet パッケージ マネージャー**] > [ **ソリューションの Nuget パッケージの管理**] を選択します。
    
3. [ **NuGet パッケージ マネージャー**] で、[ **パッケージ ソース**] に [ **nuget.org**] を選択して、[ **フィルター**] に [ **アップグレードを利用可能**] を選択し、Microsoft.Office.js を選択します。
    
4. 左側のウィンドウで、 **[更新]** をクリックしてパッケージの更新プロセスを実行します。
    
5. アドインの HTML ページの  **<head>** タグ内で、既存の office.js スクリプトに対する参照 (例: `<script src="https://appsforoffice.microsoft.com/lib/1.0/hosted/office.js" type="text/javascript"></script>)` があればコメント アウトするか削除して、更新後の JavaScript API for Office ライブラリを次のように参照します (バージョンの値を `1` に変更します)。
    
  ```
  <script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js" type="text/javascript"></script>
  ```


    CDN URL で  `office.js` の前にある `/1/` は、Office.js のバージョン 1 内で最新の増分リリースを使用するよう指定します。
    

### プロジェクト内のマニフェスト ファイルがバージョン 1.1 のスキーマを使用するように更新するには


- プロジェクトのアドイン マニフェスト ( _projectname_ Manifest.xml) ファイルで、 **OfficeApp** 要素の **xmlns** 属性のバージョン値を `1.1` に変更して更新します ( **xmlns** 以外の属性は変更しません)。
    
  ```XML
  <OfficeApp xsi:type="ContentApp" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" >
  ```


     >**メモ**  アドイン マニフェスト スキーマのバージョンを 1.1 に更新したら、 **Capabilities** 要素と **Capability** 要素を削除し、それらを [Hosts 要素と Host 要素](cff9fbdf-a530-4f6e-91ca-81bcacd90dcd.md) または [Requirements 要素と Requirement 要素](../../docs/overview/specify-office-hosts-and-api-requirements.md)に置き換える必要があります。

## テキスト エディターまたは他の IDE で作成した Office アドイン プロジェクトを最新バージョンの JavaScript API for Office ライブラリとバージョン 1.1 アドイン マニフェスト スキーマを使用するように更新する


JavaScript API for Office とアドイン マニフェスト スキーマの v1.1 のリリース前に作成されたプロジェクトについては、v1.1 のライブラリの CDN を参照するようにアドインの HTML ページを更新し、スキーマ v1.1 を使用するようにアドインのマニフェスト ファイルを更新する必要があります。 

なお、この更新プロセスは _プロジェクトごと_ に適用する必要があることに注意してください。v1.1 の Office.js とアドイン マニフェスト スキーマを使用するアドイン プロジェクトごとに、この更新プロセスを繰り返します。


 >**メモ**  Office アドインを開発するために、JavaScript API for Office ファイル (Office.js とアプリ固有の .js ファイル) のローカル コピーをダウンロードする必要はありません (Office.js の CDN を参照すれば、必要なファイルは実行時にダウンロードされます)。ただし、ライブラリ ファイルのローカル コピーが必要な場合は、 [NuGet コマンド ライン ユーティリティ](http://docs.nuget.org/consume/installing-nuget)の  `Install-Package Microsoft.Office.js` コマンドを使用してダウンロードできます。v1.1 アドイン マニフェストの XSD (XML スキーマ定義) のコピーを取得するには、[スキーマ マップ (アプリ マニフェスト スキーマ v1.1)](http://msdn.microsoft.com/library/d5f72bff-3446-c64f-02ca-ab10b5648789%28Office.15%29.aspx) のリストを参照してください。


### 最新のリリースを使用するようにプロジェクトの JavaScript API for Office ライブラリ ファイルを更新するには


1. テキスト エディターまたは IDE でアドインの HTML ページを開きます。
    
2. アドインの HTML ページの  **<head>** タグ内で、既存の office.js スクリプトに対する参照 (例: `<script src="https://appsforoffice.microsoft.com/lib/1.0/hosted/office.js" type="text/javascript"></script>)` があればコメント アウトするか削除して、更新後の JavaScript API for Office ライブラリを次のように参照します (バージョンの値を `1` に変更します)。
    
  ```
  <script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js" type="text/javascript"></script>
  ```


    CDN URL で  `office.js` の前にある `/1/` は、Office.js のバージョン 1 内で最新の増分リリースを使用するよう指定します。
    

### プロジェクト内のマニフェスト ファイルがバージョン 1.1 のスキーマを使用するように更新するには


- プロジェクトのアドイン マニフェスト ( _projectname_ Manifest.xml) ファイルで、 **OfficeApp** 要素の **xmlns** 属性のバージョン値を `1.1` に変更して更新します ( **xmlns** 以外の属性は変更しません)。
    
  ```XML
  <OfficeApp xsi:type="ContentApp" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" >
  ```


    アドイン マニフェスト スキーマのバージョンを 1.1 に更新したら、 **Capabilities** 要素と **Capability** 要素を削除し、それらを [Hosts 要素と Host 要素](cff9fbdf-a530-4f6e-91ca-81bcacd90dcd.md) または [Requirements 要素と Requirement 要素](../../docs/overview/specify-office-hosts-and-api-requirements.md)に置き換える必要があります。
    

## その他の技術情報



- [Office のホストと API の要件を指定する](../../docs/overview/specify-office-hosts-and-api-requirements.md)
    
- [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)
    
- [JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx)
    
- [Office アドインのマニフェスト向けのスキーマ リファレンス (v1.1)](http://msdn.microsoft.com/library/7e0cadc3-f613-8eb9-57ef-9032cbb97f92%28Office.15%29.aspx)
    
