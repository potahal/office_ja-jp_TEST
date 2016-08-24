---
ms.Toctitle: Update page content
title: "OneNote ページ コンテンツを更新する"
description: "OneNote のページの HTML コンテンツを更新します。"
ms.ContentId: f597bd73-866e-48a3-95c1-91b9bfabffa2
ms.date: November 18, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# OneNote ページ コンテンツを更新する

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*


OneNote ページの HTML コンテンツを更新する場合は、ページの *content* エンドポイントに PATCH 要求を送信します。

<p id="indent">`PATCH ../notes/pages/{id}/content`</p>

メッセージの本文で JSON 変更オブジェクトを送信します。 要求が成功すると、OneNote API は 204 HTTP 状態コードを返します。

<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、サービス ルート URL から開始します。

[!INCLUDE [service root url](../includes/onenote/service-root-url.xml)]

次に、ページの *content* エンドポイントを追加します。

<p id="outdent1">**ページ HTML と、すべての定義済み *data-id* の値を取得する**</p>
<p id="indent">`../pages/{id}/content`</p>

<p id="outdent1">**ページ HTML、すべての定義済み *data-id* の値、およびすべての生成された *id* の値を取得する**</p>
<p id="indent">`../pages/{page-id}/content?includeIDs=true`</p>

**data-id** と **id** の値は、更新する要素の **target** 識別子として使用されます。

<br />
完全な要求 URI は、次のいずれかのようになります。

<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/pages/{id}/content`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myorganization/sitecollections/{id}/sites/{id}/notes/pages/{id}/content`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myorganization/groups/{id}/notes/pages/{id}/content`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]


<a name="message-body"></a>
## メッセージ本文の構築

OneNote ページの HTML には、**div** 要素、**img** 要素、**ol** 要素などの構造に編成されたテキスト、画像などのコンテンツが含まれています。OneNote ページのコンテンツを更新する場合は、ページ上で HTML 要素を追加および置換します。

変更内容は JSON 変更オブジェクトの配列としてメッセージ本文に入れて送信します。ターゲット要素、新しい HTML コンテンツ、コンテンツに対する操作が各オブジェクトにより指定されます。

2 つの変更を定義する配列を次に示します。最初の変更では画像が兄弟として段落の上に挿入され、2 番目の変更ではアイテムが最後の子としてリストに追加されます。

```
[
   {
    'target':'#para-id',
    'action':'insert',
    'position':'before',
    'content':'<img src="image-url-or-part-name" alt="Image above the target paragraph" />'
  }, 
  {
    'target':'#list-id',
    'action':'append',
    'content':'<li>Item at the end of the list</li>'
  }
]
```

[その他の例](#examples)を参照してください。


### JSON 変更オブジェクトの属性

PATCH 要求の JSON オブジェクトの定義には、**target** 属性、**action** 属性、**position** 属性、および **content** 属性を使用します。

**ターゲット**  
更新する要素。値として次の ID のいずれかを指定する必要があります。

| 識別子 | 説明 |  
|------|------|  
| #{data-id} | <p>この ID は、[ページを作成](..\howto\onenote-create-page.md)するときや[更新](..\howto\onenote-update-page.md)するときに、入力 HTML の要素で任意で定義されます。 値の先頭に # を付けます。</p><p> 例: `'target':'#intro'` は要素 `<div data-id="intro" ...>` をターゲットとします</p> |  
| id | <p>これは、OneNote API から得られる[生成済み ID](#generated-ids) であり、ほとんどの置換操作で必要になります。 プレフィックス # を追加してはいけません。</p><p> 例: `'target':'div:{33f8a2...}{37}'` は要素 `<div id="div:{33f8a2...}{37}" ...>` をターゲットとします</p><p>[入力 HTML](..\howto\onenote-input-output-html.md) に定義されている **id** 値とこれらを混同しないでください。 入力 HTML で送信される **id** 値は、すべて破棄されます。</p> |  
| body | ページ上で最初の div をターゲットにするキーワード。プレフィックス # を追加してはいけません。 |  
| title | ページ タイトルをターゲットにするキーワード。 プレフィックス # を追加してはいけません。 |  
 
**action**  
ターゲット要素で実行するアクション。 「[サポートされる要素とアクション](#support-matrix)」を参照してください。

| アクション | 説明 |  
|------|------|  
| append | <p>指定されたコンテンツを最初または最後の子としてターゲットに追加します。追加される位置 (最初または最後) は、**position** 属性によって決まります。</p><p>**body**、**div**、**ol**、**ul** 要素にのみ適用されます。</p> |  
| insert | 指定されたコンテンツを兄弟としてターゲットの前または後に追加します。追加される位置 (前または後) は、**position** 属性によって決まります。 |  
| prepend | <p>指定されたコンテンツを最初の子としてターゲットに追加します。 **append** + **before** のショートカット。</p><p>**body**、**div**、**ol**、**ul** 要素にのみ適用されます。</p> |  
| replace | <p>指定したコンテンツでターゲットを置換します。</p><p>ほとんどの **replace** アクションは、ターゲットの[生成された ID](#generated-ids) を使用する必要があります (ただし、div 内の **img** 要素と **object** 要素を除きます。これらは、**data-id** の使用もサポートしています)。</p> |  
 
**position**  
指定されたコンテンツを追加する位置。この位置は、ターゲット要素を基準としています。 省略すると、既定値として **after** が使用されます。

| 位置 | 説明 |  
|------|------|  
| after (既定値) | <p>- **append** と共に:ターゲット要素の最後の子。</p><p>- **insert** と共に:ターゲット要素の後続の兄弟。</p> |
| before | <p>- **append** と共に:ターゲット要素の最初の子。</p><p>- **insert** と共に:ターゲット要素の先行の兄弟。</p> |

**コンテンツ**  
ページに追加する整形式 HTML の文字列と画像またはファイル バイナリ データ。 コンテンツにバイナリ データが含まれている場合、コンテンツ タイプとして `multipart/form-data` を利用し、"Commands" パートを含む要求を送信する必要があります ([例](#multipart)参照)。 
 

<a name="generated-ids"></a>
### 生成された ID
OneNote API は、更新可能なページで要素に対して **id** 値を生成します。 生成された ID を取得するには、GET 要求で `includeIDs=true` クエリ文字列を使用します。

<p id="indent">`GET ../notes/pages/{page-id}/content?includeIDs=true`</p>

>API は、ページ作成要求およびページ更新要求の[入力 HTML](..\howto\onenote-input-output-html.md) で定義された **id** の値をすべて破棄します。

次の例は、ページの[出力 HTML](..\howto\onenote-input-output-html.md) の段落と画像に対して生成された ID を示します。

```html
<p id="p:{33f8a242-7c33-4bb2-90c5-8425a68cc5bf}{40}">Some text on the page</p>
<img id="img:{33f8a242-7c33-4bb2-90c5-8425a68cc5bf}{45}" ... />
```

生成された **id** 値はページ更新後に変更されることがあるため、これらの値を使用する PATCH 要求を作成する前に、現行値を取得する必要があります。
 
ターゲット要素は、**data-id** または **id** 値を使用して次のように指定できます。

- **append** アクションと **insert** アクションでは、いずれの ID もターゲット値として使用できます。
- **replace** アクションでは、すべての要素について、生成された IDを使用する必要があります。ただし、ページ タイトル、および div 内の画像とオブジェクトを除きます。 
    - タイトルを置換する場合は、**title** キーワードを使用します。 
    - div 内の画像とオブジェクトを置換する場合は、**data-id** または **id** のどちらかを使用します。

次の例では、ターゲットに **id** 値を使用します。 生成された ID と共に # プレフィックスを使用しないでください。

```
[
   {
    'target':'p:{33f8a242-7c33-4bb2-90c5-8425a68cc5bf}{40}',
    'action':'insert',
    'position':'before',
    'content':'<p>This paragraph goes above the target paragraph.</p>'
  }
]
```

<a name="support-matrix"></a>
## サポートされる要素とアクション
ページ上の要素の多くは更新可能ですが、要素の種類ごとに特定のアクションがサポートされています。サポートされるターゲット要素と、各要素でサポートされる更新アクションを次の表に示します。

| 要素 | 置換 | 子の追加 | 兄弟の挿入 |  
|------|------|------|------|  
| body<br /> (ページの最初の div をターゲットとする) | いいえ | **はい** | いいえ |  
| div<br /> ([絶対配置](../howto/onenote-abs-pos.md)) | いいえ | **はい** | いいえ |  
| div<br /> (div 内) | **はい** (id のみ) | **はい** | **はい** |   
| img、object<br /> (div 内) | **はい** | いいえ | **はい** |   
| ol、ul | **はい** (id のみ) | **はい** | **はい** |   
| table | **はい** (id のみ) | いいえ | **はい** |   
| p、li、h1-h6 | **はい** (id のみ) | いいえ | **はい** |   
| title | **はい** | no | いいえ |  
 

次の要素では更新アクションはサポートされていません。

- img ([絶対配置](../howto/onenote-abs-pos.md))
- object ([絶対配置](../howto/onenote-abs-pos.md))
- tr、td
- meta
- head
- span
- a
- style タグ


<a name="examples"></a>
## 要求の例
更新要求には、JSON 変更オブジェクトとして表現される変更が 1 つ以上含まれています。これらのオブジェクトでは、ページ上のさまざまなターゲットや、ターゲットに対するさまざまなアクションとコンテンツを定義できます。

次の例に、PATCH 要求で使用される JSON オブジェクトと完全な PATCH 要求を示します。

[子要素の追加](#append-examples)&nbsp;&nbsp;|&nbsp;&nbsp;[兄弟要素の追加](#insert-examples)&nbsp;&nbsp;|&nbsp;&nbsp;[要素の置換](#replace-examples)&nbsp;&nbsp;|&nbsp;&nbsp;[完全な PATCH 要求](#complete-requests)


<a name="append-examples"></a>
### 子要素の追加
**append** アクションでは、**body**、**div** (div 内)、**ol**、**ul** 要素に子が追加されます。 **position** 属性により、ターゲットの前または後に子を追加することが決定されます。 既定の位置は**後**です。

**div への追加**

次の例では、2 つの子ノードが **div1** 要素に追加されます。画像が最初の子として追加され、段落が 2 番目の子として追加されます。 

```
[
 {
    'target':'#div1',
    'action':'append',
    'position':'before',
    'content':'<img data-id="first-child" src="image-url-or-part-name" />'
  },
  {
    'target':'#div1',
    'action':'append',
    'content':'<p data-id="last-child">New paragraph appended to the div</p>'
  }
]
```
 

**body* 要素への追加***

**body** は、任意のページ上の最初の div 内に子要素を追加するためのショートカットとして使用できます。

次の例では、ページの最初の div に 2 つの段落を最初の子と最後の子として追加しています。 **body** ターゲットと共に # を使用しないでください。 この例では、**prepend** アクションを **append** + **before** として使用しています。

```
[
  {
    'target':'body',
    'action':'prepend',
    'content':'<p data-id="first-child">New paragraph as first child in the first div</p>'
  },
  {
    'target':'body',
    'action':'append',
    'content':'<p data-id="last-child">New paragraph as last child in the first div</p>'
  }
]
```
 

**リストへの追加**

次の例では、リスト項目がターゲット リストに最後の子として追加されます。この項目では既定以外のリスト スタイルが使用されるため、**list-style-type** プロパティが定義されます。

```
[
  {
    'target':'#circle-ul',
    'action':'append',
    'content':'<li style="list-style-type:circle">Item at the end of the list</li>'
  }
]
```
 

<a name="insert-examples"></a>
### 兄弟要素の挿入
**insert** アクションでは、兄弟がターゲット要素に追加されます。 **position** 属性により、ターゲットの前または後に兄弟を挿入することが決定されます。 既定の位置は**後**です。 「[**insert** をサポートする要素」](#support-matrix)を参照してください。

**兄弟の挿入**

次の例では、ページに 2 つの兄弟ノードが挿入されます。**para1** 要素の上に画像が追加され、**para2** 要素の下に段落が追加されます。

```none
[
  {
     'target':'#para1',
     'action':'insert',
     'position':'before',
     'content':'<img src="image-url-or-part-name" alt="Image inserted above the target" />'
  },
  {
    'target':'#para2',
     'action':'insert',
     'content':'<p data-id="next-sibling">Paragraph inserted below the target</p>'
  }
]
```
 

<a name="replace-examples"></a>
### 要素の置換
**data-id** と生成された **id** のいずれかをターゲット値として使用し、div 内にある **img** 要素および **object** 要素を置換できます。 ページ タイトルを置換する場合は、**title** キーワードを使用します。 [**replace** をサポートするその他すべての要素](#support-matrix)については、生成された ID を使用する必要があります。

**画像の置換**

次の例では、画像の **data-id** をターゲットとして使用して、div 内の画像が置換されます。 

```
[
  {
    'target':'#img1',
    'action':'replace',
    'content':'<div data-id="new-div"><p>This div replaces the image</p></div>'
  }
]
```
 

**表の更新**

次の例は、生成された ID を使用して表を更新する方法を示します。**tr** 要素と **td** 要素の置換はサポートされていませんが、表全体を置換することができます。

```
[
  {
    'target':'table:{de3e0977-94e4-4bb0-8fee-0379eaf47486}{11}',
    'action':'replace',
    'content':'<table data-id="football">
      <tr><td><p><b>Brazil</b></p></td><td><p>Germany</p></td></tr>
      <tr><td><p>France</p></td><td><p><b>Italy</b></p></td></tr>
      <tr><td><p>Netherlands</p></td><td><p><b>Spain</b></p></td></tr>
      <tr><td><p>Argentina</p></td><td><p><b>Germany</b></p></td></tr></table>'
  }
]
```
 

**タイトルの変更**

次の例は、ページのタイトルを変更する方法を示します。タイトルを変更するには、ターゲット値として **title** キーワードを使用します。title ターゲットでは # は使用しないでください。

```
[
  {
    'target':'title',
    'action':'replace',
    'content':'New title'
  }
]
```
 

**To Do アイテムの更新**

次に示す例では、replace アクションを使用して、To Do チェック ボックス アイテムを完了状態に変更します。

```
[
  {
    'target':'#task1',
    'action':'replace',
    'content':'<p data-tag="to-do:completed" data-id="task1">First task</p>'
  }
]
```


  **data-tag** 属性の使用法の詳細については、「[ノート シールを使用する](https://msdn.microsoft.com/library/office/mt159148.aspx)」を参照してください。


<a name="complete-requests"></a>
### 完全な PATCH 要求の例
次の例に、完全な PATCH 要求を示します。

**テキスト コンテンツのみの要求**  
次の例では、PATCH 要求でコンテンツ タイプとして **application/json** が使用されています。 コンテンツにバイナリ データが含まれていないとき、この形式を使用できます。

```none
PATCH https://www.onenote.com/api/v1.0/me/notes/pages/{page-id}/content

Content-Type: application/json
Authorization: Bearer {token}

[
   {
    'target':'#para-id',
    'action':'insert',
    'position':'before',
    'content':'<img src="image-url" alt="New image from a URL" />'
  }, 
  {
    'target':'#list-id',
    'action':'append',
    'content':'<li>Item at the bottom of the list</li>'
  }
]
```
 
<a name="multipart"></a>
**バイナリ コンテンツを含むマルチパート要求**  
次の例は、バイナリ データを含んでいるマルチパート PATCH 要求を示しています。マルチパート要求には、**application/json** コンテンツ タイプを指定し、JSON 変更オブジェクトの配列を含める "Commands" 部分が必要になります。その他のデータ部分に、バイナリ データを含めることができます。部分の名前は、通常、ミリ秒単位での現在の時刻、またはランダムな GUID が付加された文字列になります。

```none
PATCH https://www.onenote.com/api/v1.0/me/notes/pages/{page-id}/content

Content-Type: multipart/form-data; boundary=PartBoundary123
Authorization: Bearer {token}

--PartBoundary123
Content-Disposition: form-data; name="Commands"
Content-Type: application/json

[
  {
    'target':'img:{2998967e-69b3-413f-a221-c1a3b5cbe0fc}{42}',
    'action':'replace',
    'content':'<img src="name:image-part-name" alt="New binary image" />'
  }, 
  {
    'target':'#list-id',
    'action':'append',
    'content':'<li>Item at the bottom of the list</li>'
  }
]

--PartBoundary123
Content-Disposition: form-data; name="image-part-name"
Content-Type: image/png

... binary image data ...

--PartBoundary123--
```

<a name="request-response-info"></a>
## PATCH 要求の要求情報と応答情報

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。*{token}* は、登録済みアプリの有効な OAuth 2.0 アクセス トークンになります。</p><p>これがないか、無効の場合、要求は失敗し、401 ステータス コードが表示されます。 「[認証とアクセス許可](..\howto\onenote-auth.md)」を参照してください。</p> |  
| Content-Type ヘッダー | <p>JSON 変更オブジェクトの配列の `application/json`。メッセージ本文または[マルチパート要求](#multipart)の必須の "Commands" パートで直接送信。</p><p>マルチパート要求はバイナリ データを送信するときに必須であり、コンテンツ タイプとして `multipart/form-data; boundary=part-boundary` を使用します。*{part-boundary}* は、各データ パートの開始と終了を伝える文字列です。</p> |  
 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 204 HTTP ステータス コード。PATCH 要求に対して JSON データは返されません。 |  
| エラー | 更新要求が失敗すると、API は応答本文の **@api.diagnostics** オブジェクトでエラーを返します。 次の場合、要求は失敗します。<br /> - JSON オブジェクトに含まれている属性が無効であるか、形式が不正です。<br /> - **target**、**action**、**content** 属性がありません。<br /> - ターゲット要素が存在しません。<br /> - ターゲット値の形式が無効です。 たとえば、**data-id** にプレフィックスとして # が付いていません。<br /> - ターゲット要素が指定のアクションに対応していません。<br /> - **action** または **position** 値が無効です。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  
 

<a name="root-url"></a>
### OneNote サービスのルート URL の構築

[!INCLUDE [service root url section](../includes/onenote/service-root-section.xml)]


<a name="permissions"></a>
## アクセス許可

OneNote ページを更新するには、適切なアクセス許可を要求する必要があります。アプリの動作に必要な最低限のアクセス許可を選択してください。

[!INCLUDE [Update perms](../includes/onenote/update-perms.md)]

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md)」を参照してください。
   

<a name="see-also"></a>
## その他のリソース

- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  
