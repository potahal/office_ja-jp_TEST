---
ms.Toctitle: OneNote development
title: "OneNote" 
description: "Use the OneNote API to integrate with consumer and enterprise notebooks."
ms.ContentId: 085adac0-87ce-4c70-8d41-b7a53f3576a8
ms.date: July 26, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

<style>##simpletable {margin:-5px 0px 0px 0px; border:none;} #simplecell {border:none; padding:15px 20px; background-color:white;} .small {font-size:80%; margin:5px 0px 0px 0px;}</style>

# OneNote 開発

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*

OneNote は、オンラインで使用でき、かつ多くのモバイルおよびタブレット プラットフォームで使用できる人気の高いノート作成ツールです。普段使用しているアプリを OneNote と統合することによって、これまで以上に簡単に、お気に入りのプラットフォームで利用できる非常に便利なアプリを構築し、世界中の数百万人のユーザーに利用してもらうことができるようになりました。 

![A sample OneNote page.](images\onenote\onenote-page.png)

OneNote のノートブック、セクション、ページ階層、簡単に使用できる API を活用することにより、計画を立てたり、アイデアや情報を整理したりできます。

<a name="overview"></a> 
## プラットフォームの概要

OneNote サービスは Microsoft のクラウドで実行され、OneNote コンテンツへのプログラムでのアクセスを可能にする RESTful インターフェイスが提供されます。OneNote API は、JSON、HTML、および OData で構築される軽量でシンプルな API で、HTTP 要求をサポートするすべての言語またはプラットフォームで使用できます。 

次に、OneNote API 開発スタックを簡単に図示します。 

![Development stack for OneNote apps on various platforms. Apps use OAuth 2.0 to access OneNote content.](images\onenote\onenote-dev-diagram.png)

最初に、ユーザーは認証され、アプリへのアクセスを許可される必要があります。OneNote コンテンツとの対話に使用するアクセス トークンを取得します。この API では、OneNote リソースの CRUD サポートに加えて、光学式文字認識 (OCR)、フルテキスト検索、名刺の抽出などの機能も提供します。

>>[SDK](#sdks) を使用して認証プロセスを簡略化できます。 

**OneNote API の使用**

OneNote API を使用するには、OneNote のサービス ルート URL の特定のエンドポイントに HTTP 要求を送信します。

<p id="indent">`https://www.onenote.com/api/{version}/{location}/notes/...`</p>


OneNote API を使用して、個人のページ、サイト、グループ ノートブックの作成、表示、管理などを行えます。API のしくみを理解していただくために、現在のユーザーの既定のノートブックにページを作成する簡単な POST 要求を次に示します。 

```
POST https://www.onenote.com/api/v1.0/me/notes/pages

Authorization: Bearer {token}
Content-Type: text/html; charset=utf-8
Accept: application/json
 ...

<!DOCTYPE html>
<html>
    <head>
    <title>My new OneNote page</title>
    <meta name="created" content="2015-09-9T12:45:00.000-8:00"/>
    </head>
    <body>
        <p>This is a simple HTML page.</p>
    </body>
</html>
```     

成功した場合、要求は次の応答を返します。この場合、新しいページの OData 表記は JSON 形式です。

```json
HTTP/1.1 201 Created
Location:https://www.onenote.com/api/v1.0/me/notes/pages/0-37e6dad...
X-CorrelationId:8943c159-ee49-4c71-8cd0-ebf0861d07a6
Date:Sun, 09 Aug 2015 21:36:40 GMT
Content-Type:application/json; odata.metadata=minimal; odata.streaming=true
 ...

{
  "@odata.context": "https://www.onenote.com/api/v1.0/$metadata#me/notes/pages/$entity",
  "title": "My new OneNote page",
  "createdByAppId": "WLID-0000000048219837",
  "links": {
    "oneNoteClientUrl": {
      "href": "onenote:https://d.docs.live.net/73dbaf9b..."
    },
    "oneNoteWebUrl": {
      "href": "https://onedrive.live.com/redir.aspx?cid=73dbaf9b..."
    }
  },
  "contentUrl": "https://www.onenote.com/api/v1.0/me/notes/pages/0-37e6dad.../content",
  "lastModifiedTime": "2015-09-09T12:45:00Z",
  "id": "0-37e6dad8c6eb489294656ad878431666!209-73DBAF9B7E5C4B4C!153",
  "self": "https://www.onenote.com/api/v1.0/me/notes/pages/0-37e6dad...",
  "createdTime": "2015-09-09T12:45:00Z"
}
```
*POST ページ*要求の詳細については、「[ページの作成](..\howto\onenote-create-page.md)」を参照してください。 

<a name="why-onenote"></a>
## OneNote アプリを作成する理由
OneNote を統合して、人気アプリを作成します。OneNote API を使用して、ノート、リスト、画像、ファイルなどを OneNote ノートブックで作成したり管理できます。

**メモやアイデアを収集して整理する**  
コンテンツを追加したり、アレンジしたりできるキャンバスとして OneNote を使用できます。OneNote API によって簡単にアプリケーションを作成できるため、学生がノートを取ったり調査したり、家族が計画やアイデアを共有したり、買い物客が画像を共有したり、みんなの注目を集めることをなんでも行えます。みんなが必要とする情報を集め、OneNote に送り、それらを整理することができます。

**さまざまな形式で情報をキャプチャする**  
HTML、埋め込み画像 (ローカルやパブリック URL にある)、ビデオ、オーディオ、メール メッセージ、その他の一般的なファイル タイプをキャプチャできます。OneNote では、Web ページや PDF ファイルをスナップショットとして表示することもできます。OneNote API は、OneNote ページ レイアウト用に標準 HTML および CSS のセットをサポートします。そのため、表、インライン イメージ、基本書式を使用して思いどおりの外観を実現できます。 

**OneNote エコシステムを使用して、コア シナリオを強化する**  
その他の強力な OneNote API 機能をタップで使用できます。この API では、イメージに対する OCR の実行、フルテキスト検索のサポート、クライアントの自動同期、イメージの処理、名刺キャプチャ、オンライン製品一覧やレシピ一覧の抽出などを実行できます。メモや軽量メディアのクラウドでのデジタル メモリ ストアとして、またはドメイン固有データのデータ フィードとして OneNote を使用します。 
 
**すべての主要プラットフォームの数百万人に上る OneNote ユーザー**  
OneNote を使用すると、アプリの利用数が増加します。新しい Windows デバイスにプレインストールされていて、人気の高いプラットフォームでも、OneNote Online として Web でも、さらに [Office 365](../howto/platform-development-overview.md) の一部としても使用できる OneNote は、世界中の 1 億人以上のユーザーが積極的に使用しています。機能の豊富な OneNote 環境を利用するアプリを公開する際、クロスプラットフォーム市場の可能性を無視することはできません。

<a name="get-started"></a>
## OneNote API を使ってみる
サンプルとチュートリアルを使用して、すぐにコーディングを開始したり、対話型コンソールを試してみたり、ドキュメントを深く掘り下げて調べたりできます。

### OneNote のサンプルとチュートリアル

これらのサンプルとチュートリアルでは、異なるプラットフォーム上で OneNote API を使用する際の基本事項が示されます。(GitHub のサンプルをすべてご覧ください。) (See all our [samples on GitHub](https://github.com/onenotedev).)

<table role="presentation" id="simpletable">
<tr>
<td id="simplecell">iOS<br />&nbsp;&nbsp;&nbsp;- [iOS-REST-API-Explorer](https://github.com/OneNoteDev/iOS-REST-API-Explorer) (MSA only)<br />チュートリアル</td>
<td id="simplecell">Windows<br />&nbsp;&nbsp;&nbsp;- [OneNoteAPISampleWinUniversal](http://go.microsoft.com/fwlink/?LinkID=524572)<br />&nbsp;&nbsp;&nbsp;</td>
</tr>
<tr>
<td id="simplecell">Android<br />&nbsp;&nbsp;&nbsp;- [Android-REST-API-Explorer](https://github.com/OneNoteDev/Android-REST-API-Explorer)</td>
<td id="simplecell">PHP<br />&nbsp;&nbsp;&nbsp;- [OneNoteAPISamplePHP](https://github.com/OneNoteDev/OneNoteAPISamplePHP) (MSA only)</td>
</tr>
<tr>
<td id="simplecell">Node.js<br />&nbsp;&nbsp;&nbsp;- [OneNoteAPISampleNodejs](https://github.com/OneNoteDev/OneNoteAPISampleNodejs) (MSA only)</td>
<td id="simplecell">Ruby<br />&nbsp;&nbsp;&nbsp;- [OneNoteAPISampleRuby](https://github.com/OneNoteDev/OneNoteAPISampleRuby) (MSA only)</td>
</tr>
<tr>
<td id="simplecell">ASP.NET MVC セクション<br />&nbsp;&nbsp;&nbsp;- [Tutorial](../howto/onenote-tutorial.md#aspnet) (AAD only)</td>
<td id="simplecell">&nbsp;</td>
</tr>
</table>

<p id="indent" class="small">*MSA = Microsoft アカウント認証、AAD = Azure Active Directory 認証</p>

<a name="console"></a>
###対話型コンソール
 
Microsoft が提供する対話型コンソールのいずれかを使用して、自分の OneNote ノートブックで OneNote API を試します。

**[対話型 REST 参照](http://dev.onenote.com/docs)**

![対話型 REST 参照](images\onenote\apiary123.png)

<p id="indent">(1) エンドポイントを選択し、(2) メソッド タイルをクリックして、(3) **[コンソールに切り替え]** を選択してから Microsoft アカウントの資格情報で認証を行います。必要に応じてサンプルのクエリ文字列オプションを変更して、[リソースの呼び出し] を選択します。 Change the example query string options as needed, and choose **Call Resource**.</p>

<br />
**[Apigee での OneNote](https://apigee.com/onenote/embed/console/onenote/)**

![Apigee での OneNote](images\onenote\apigee123.png)

<p id="indent">(1) ドロップダウン メニューから **[OAuth 2 の暗黙的な付与]** を選択して Microsoft アカウント資格情報で認証を行い、(2) エンドポイントを選択します。必要に応じて [クエリ]、[テンプレート]、[ヘッダー] タブに値を入力し、(3) [送信] を選択します。 Enter values on the Query, Template, and Header tabs as needed, and (3) choose **Send**.</p>
 

### 方法と概念に関する記事

より深く掘り下げる準備ができたら、方法と概念に関する次の記事を参照して、OneNote で行えることの詳細について学習します。

<p id="indent">[認証とアクセス許可](../howto/onenote-auth.md)</p>
<p id="indent">[ブランドのガイドライン](../howto/onenote-branding.md)</p>
<p id="indent">[サポート対象の REST 操作](../howto/onenote-supported-ops.md)</p>
<p id="indent">[OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)</p>
<p id="indent">[OneNote クライアントを開く](../howto/onenote-open-links.md)</p>
<p id="indent">[ノートブック、セクション、ページをコピーする](../howto/onenote-copy.md)</p>
<p id="indent">[ページを作成する](../howto/onenote-create-page.md)</p>
<p id="indent">[ページ コンテンツを更新する](../howto/onenote-update-page.md)</p>
<p id="indent">[画像とファイルを追加する](../howto/onenote-images-files.md)</p>
<p id="indent">[絶対的な位置要素を作成する](../howto/onenote-abs-pos.md)</p>
<p id="indent">[データを抽出する](../howto/onenote-extract-data.md)</p>
<p id="indent">[ノート シールを使用する](../howto/onenote-note-tags.md)</p>
<p id="indent">[OneNote エンティティのアクセス許可を管理する](../howto/onenote-manage-perms.md)</p>
<p id="indent">[クラス ノートブックの操作:](../howto/onenote-classnotebook.md)</p>
<p id="indent">[クラス ノートブックを使用する](../howto/onenote-staffnotebook.md)</p>
<p id="indent">[入力 HTML と出力 HTML](../howto/onenote-input-output-html.md)</p>
<p id="indent">[エラー コードと警告コード](../howto/onenote-error-codes.md)</p>
<p id="indent">[保存ダイアログを使用する](../howto/onenote-save-dialog.md)</p>
<p id="indent">[Webhooks を購読する](../howto/onenote-sync.md)</p>

<a name="sdks"></a>
###OneNote の開発のための SDK

[!INCLUDE [sdks](../includes/onenote/sdks.xml)]

<a name="contact"></a>
## つながる
機能の拡張や改善が行われた場合は、対応することをお勧めします。ご質問やコメントを歓迎しております。また、解決のお手伝いをしたいと思っておりますし、最新の機能を活用していただきたいとも願っています。Microsoft とつながる方法を以下に示します。

- ニュースや役に立つヒントについて [OneNote 開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)をお読みください。
- [Stack Overflow](http://go.microsoft.com/fwlink/?LinkID=390182)で専門家の答えを得てください。
- Twitter でフォローしてください ([@onenotedev](http://twitter.com/onenotedev))。 
- [UserVoice](http://go.microsoft.com/fwlink/?LinkID=396377) を使用して、アイデアやコメントをお寄せください。

<a name="changes"></a>
## 変更内容

次に、過去 1 年間に OneNote API とドキュメントに加えられた変更について示します。

<p id="outdent">2016 年 7 月 25 日</p>
<p id="indent">Added the [Work with staff notebooks](../howto/onenote-staffnotebook.md) topic.</p>
<p id="indent">Documented support for [embedded videos](../howto/onenote-images-files.md#videos).</p>
<p id="outdent">2016 年 5 月</p>
<p id="indent">Added [page preview](../howto/onenote-get-content.md#get-page-preview) support. ページ プレビューのサポートが追加されました。../pages/{id}/preview`../pages/{id}/preview` エンドポイントを使用して、ページのテキストと画像のプレビュー コンテンツを取得します。詳細については、ブログの投稿「ページ プレビュー API」を参照してください。 Read the [Page preview API](https://blogs.msdn.microsoft.com/onenotedev/2016/05/23/page-preview-api/) blog post to learn more.</p>
<p id="indent">../users/{id}/notes/`../users/{id}/notes/` に関するサポートが文書化されました。これにより、指定したユーザー (URL) が現在のユーザーと共有している OneNote コンテンツにアクセスできます。この機能は、エンタープライズ ノートブックのみで利用可能です。 Enterprise notebooks only.</p>
<p id="indent">ノートブック、セクション グループ、セクションのアクセス許可を設定できる[アクセス許可管理 API](../howto/onenote-manage-perms.md) が追加されました。この機能は、エンタープライズ ノートブックのみで利用可能です。 Enterprise notebooks only.</p>
<p id="indent">次の新しいクラス ノートブック操作が追加されました。[他のノートブックからのセクションの挿入](../howto/onenote-classnotebook.md#insert-sections)、[*教師のみ*セクション グループの追加](../howto/onenote-classnotebook.md#update)、[クラス ノートブックの削除](../howto/onenote-classnotebook.md#delete)、[指定した言語でのクラス ノートブックの作成](../howto/onenote-classnotebook.md#create)、[新規クラス ノートブックのメール通知の送信](../howto/onenote-classnotebook.md#create)。この機能は、エンタープライズ ノートブックのみで利用可能です。</p>
<p id="indent">Added support for the `<pre>` element in page HTML content. <p id="indent"><c>ページ HTML コンテンツに <pre></c> 要素のサポートが追加されました。MSDN や StackOverflow などのサイトからキャプチャされたコンテンツが適切なコード書式設定を使用してレンダリングされるようになりました。</p></p>
<p id="outdent">2016 年 3 月</p>
<p id="indent">DELETE pages`GET /pages/{id}/content?preAuthenticated=true` が運用環境にリリースされました。 GET /pages/{id}/content?preAuthenticated=true`preAuthenticated=true` が運用環境にリリースされました。preAuthenticated=true クエリ文字列オプションを使用してページ コンテンツを取得する場合、[出力 HTML](../howto/onenote-input-output-html.md#image-output-examples) にはページ上のイメージ リソースへのパブリック URL が含まれます。これらの事前認証の URL は、1 時間有効です。「パブリック リソースのワンタイム認証」を参照してください。 These pre-authenticated URLs are valid for one hour. See [One time Authentication for Public Resource](https://blogs.msdn.microsoft.com/onenotedev/2016/03/22/693/).</p>
<p id="indent">DELETE pages`PATCH /sections/{id}` が運用環境にリリースされました。 PATCH /sections/{id} が運用環境にリリースされました。これにより、次のようにメッセージ本文で *application/json* コンテンツ タイプを送信することによって、セクションの名前を変更できるようになりました。{ "name":"New section name" }`{ "name": "New section name" }`</p>
<p id="outdent">2016 年 2 月</p>
<p id="indent">トピック「[Webhooks の購読](../howto/onenote-sync.md)」と「[クラス ノートブックの操作](../howto/onenote-classnotebook.md)」が追加されました。現在、Webhooks は OneDrive のコンシューマー ノートブックのみでサポートされています。 トピック「Webhooks の購読」と「クラス ノートブックの操作」が追加されました。現在、Webhooks は OneDrive のコンシューマー ノートブックのみでサポートされています。</p>
<p id="outdent">2016 年 1 月</p>
<p id="indent">Turned on throttling. 調整が有効になりました。詳細については、「[OneNote API 調整とそれを回避するためのベスト プラクティス](http://blogs.msdn.com/b/onenotedev/archive/2016/01/13/onenote-api-throttling-and-best-practices.aspx)」を参照してください。</p>
<p id="indent">トピック「[サポート対象の REST 操作](../howto/onenote-supported-ops.md)」と「[ノートブック、セクション、ページをコピーする](../howto/onenote-copy.md)」が追加されました。現在、コピー機能は Office 365 ノートブックのみで使用できます。 トピック「サポート対象の REST 操作」と「ノートブック、セクション、ページをコピーする」が追加されました。現在、コピー機能は Office 365 ノートブックのみで使用できます。</p>
<p id="outdent">2015 年 11 月</p>
<p id="indent">Office 365 ノートブックのサポートがプレビューから運用環境に移動されました。これには、SharePoint サイトと Office 365 グループでのノートブックのサポート、およびこれらの組織レベルのノートブックにアクセスするために必要な **Notes.Read.All** と **Notes.ReadWrite.All** のアクセス許可が含まれます。</p>
<p id="indent">Released `POST /sectiongroups/{id}/sections`, `POST /notebooks/{id}/sectiongroups`, and `POST /sectiongroups/{id}/sectiongroups` to production.</p>
<p id="indent">**CopyNotebook**、**CopyToNotebook**、**CopyToSectionGroup**、**CopyToSection** が Office 365 ノートブックの運用環境にリリースされました。</p>
<p id="indent">Added the **parentSection** and **parentNotebook** navigation properties to pages. 既定のクエリは、親セクションを展開し、セクションの id、name、および self プロパティを選択します。</p>
<p id="indent">Added the **level** and **order** properties to pages. **level** と order プロパティがページに追加されました。これらのプロパティを取得するには、pagelevel パラメーターを、セクション内のページ コレクションのクエリ、または特定のページのクエリに含めます。例:GET ../sections/{id}/pages?pagelevel=true または GET ../pages/{id}?pagelevel=true Example: `GET ../sections/{id}/pages?pagelevel=true` or `GET ../pages/{id}?pagelevel=true`</p>
<p id="indent">ノートブック名の最大文字数が 50 から 128 文字に変更されました。</p>
<p id="indent">Moved the how-to and conceptual documentation. 方法と概念に関するドキュメントが移動されました。新しいドキュメントでは、コンシューマーとエンタープライズの両方の OneNote API について説明します。</p>
<p id="outdent">2015 年 9 月</p>
<p id="indent">**top** クエリ文字列オプションを使用した GET pages`GET pages` 要求で返される最大ページ数が 500 から 100 に変更されました。</p>
<p id="outdent">2015 年 7 月</p>
<p id="indent">onnvshort REST API エクスプローラーの 2 つのサンプル アプリをリリースしました。<br />- [iOS REST API Explorer](https://github.com/OneNoteDev/iOS-REST-API-Explorer)<br />- [Android REST API Explorer](https://github.com/OneNoteDev/Android-REST-API-Explorer)</p>
<p id="indent">DELETE pages`DELETE pages` が運用環境にリリースされました。</p>
<p id="indent">新たな推奨される /me/notes/`/me/notes/` ルートを使用するように、操作方法のトピックのルート サービス URL が更新されました。https://www.onenote.com/api/v1.0/me/notes/`https://www.onenote.com/api/v1.0/me/notes/`</p>
<p id="outdent">2015 年 6 月</p>
<p id="indent">「[ノート シールの使用](../howto/onenote-note-tags.md)」トピックが公開されました。</p>

## その他の技術情報

- [OneNote デベロッパー センター](http://dev.onenote.com/) 
