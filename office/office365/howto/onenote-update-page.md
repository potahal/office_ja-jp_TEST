---
ms.Toctitle: Update page content
title: "OneNote ページ コンテンツを更新する"
description: "Update the HTML content of OneNote pages."
ms.ContentId: f597bd73-866e-48a3-95c1-91b9bfabffa2
ms.date: November 18, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# OneNote ページ コンテンツを更新する

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*


OneNote ページの HTML コンテンツを更新する場合は、ページの *content* エンドポイントに PATCH 要求を送信します。

<p id="indent">`PATCH ../notes/pages/{id}/content`</p>

Send a JSON change object in the message body. メッセージの本文で JSON 変更オブジェクトを送信します。要求が成功すると、OneNote API は 204 HTTP 状態コードを返します。

<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、サービス ルート URL から開始します。

[!INCLUDE [service root url](../includes/onenote/service-root-url.xml)]

次に、ページの *content* エンドポイントを追加します。

<p id="outdent1"> ページ HTML と、すべての定義済み data-id の値を取得する </p>
<p id="indent">`../pages/{id}/content`</p>

<p id="outdent1"> ページ HTML、すべての定義済み data-id の値、およびすべての生成された id の値を取得する </p>
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

**target**  
更新する要素。値として次の ID のいずれかを指定する必要があります。

| 識別子 | 説明 |  
|------|------|  
| ##{data-id} | <p>This ID is optionally defined on elements in the input HTML when [creating a page](..\howto\onenote-create-page.md) or [updating a page](..\howto\onenote-update-page.md). Prefix the value with a #.</p><p> Example: `'target':'#intro'` targets the element `<div data-id="intro" ...>`</p> |  
| id | <p>This is the [generated ID](#generated-ids) from the OneNote API, and is required for most replace operations. ページ タイトルをターゲットにするキーワード。プレフィックス # を追加してはいけません。</p><p> Example: `'target':'div:{33f8a2...}{37}'` targets the element `<div id="div:{33f8a2...}{37}" ...>`</p><p>Don't confuse these with any **id** values defined in the [input HTML](..\howto\onenote-input-output-html.md). All **id** values sent in the input HTML are discarded.</p> |  
| body | ページ上で最初の div をターゲットにするキーワード。プレフィックス # を追加してはいけません。 |  
| title | ページ タイトルをターゲットにするキーワード。プレフィックス # を追加してはいけません。 ページ タイトルをターゲットにするキーワード。プレフィックス # を追加してはいけません。 |  
 
**action**  
ターゲット要素に対して実行するアクション。「サポートされる要素とアクション」を参照してください。 See [supported actions for elements](#support-matrix).

| アクション | 説明 |  
|------|------|  
| append | <p>指定されたコンテンツを最初または最後の子としてターゲットに追加します。追加される位置 (最初または最後) は、**position** 属性によって決まります。body 要素、div 要素、ol 要素、および ul 要素にのみ適用されます。</p><p>Applies only to **body**, **div**, **ol**, and **ul** elements.</p> |  
| insert | 指定されたコンテンツを兄弟としてターゲットの前または後に追加します。追加される位置 (前または後) は、**position** 属性によって決まります。 |  
| prepend | <p>指定されたコンテンツを最初の子としてターゲットに追加します。append + before のショートカットです。body 要素、div 要素、ol 要素、および ul 要素にのみ適用されます。 Shortcut for **append** + **before**.</p><p>Applies only to **body**, **div**, **ol**, and **ul** elements.</p> |  
| replace | <p>Replaces the target with the supplied content.</p><p>指定されたコンテンツでターゲットを置換します。ほとんどの **replace** アクションは、ターゲットの[生成された ID](#generated-ids) を使用する必要があります (ただし、div 内の **img** 要素と **object** 要素を除きます。これらは、**data-id** の使用もサポートしています)。</p> |  
 
**position**  
指定されたコンテンツを追加する位置。この位置は、ターゲット要素を基準としています。省略すると、既定値として after が使用されます。 省略すると、既定の **render** になります。

| 位置 | 説明 |  
|------|------|  
| after (既定値) | <p>- **append** で使用する場合: ターゲット要素の最後の子。- insert で使用する場合: ターゲット要素の後続の兄弟。</p><p>- **append** で使用する場合: ターゲット要素の最後の子。- insert で使用する場合: ターゲット要素の後続の兄弟。</p> |
| before | <p>- **append** で使用する場合: ターゲット要素の最初の子。- insert で使用する場合: ターゲット要素の前の兄弟。</p><p>**before**- append で使用する場合: ターゲット要素の最初の子。insert で使用する場合: ターゲット要素の前の兄弟。</p> |

**コンテンツ**  
ページおよび画像またはファイル バイナリ データに追加する整形式 HTML の文字列。コンテンツにバイナリ データが含まれている場合、要求を送信するときに "Commands" 部分で multipart/form-data コンテンツ タイプを使用する必要があります。 (「例」を参照)。 ページおよび画像またはファイル バイナリ データに追加する整形式 HTML の文字列。コンテンツにバイナリ データが含まれている場合、要求を送信するときに "Commands" 部分で multipart/form-data`multipart/form-data` コンテンツ タイプを使用する必要があります。 (「[例](#multipart)」を参照)。 
 

<a name="generated-ids"></a>
### 生成された ID
OneNote API は、更新可能なページ上の要素に **id** の値を生成します。生成された ID を取得するには、GET 要求で includeIDs=true クエリ文字列式を使用します。 onnvshort API は、更新可能なページ要素の id`includeIDs=true` 値を生成します。生成された ID を取得するには、GET 要求で includeIDs=true クエリ文字列式を使用します。

<p id="indent">`GET ../notes/pages/{page-id}/content?includeIDs=true`</p>

>>API は、ページ作成要求およびページ更新要求の[入力 HTML](..\howto\onenote-input-output-html.md) で定義された **id** の値をすべて破棄します。

次の例は、ページの[出力 HTML](..\howto\onenote-input-output-html.md) の段落と画像に対して生成された ID を示します。

```html
<p id="p:{33f8a242-7c33-4bb2-90c5-8425a68cc5bf}{40}">Some text on the page</p>
<img id="img:{33f8a242-7c33-4bb2-90c5-8425a68cc5bf}{45}" ... />
```

生成された **id** 値はページ更新後に変更されることがあるため、これらの値を使用する PATCH 要求を作成する前に、現行値を取得する必要があります。
 
ターゲット要素は、**data-id** または **id** 値を使用して次のように指定できます。

- **append** アクションと **insert** アクションでは、いずれの ID もターゲット値として使用できます。
- **replace** アクションでは、すべての要素について、生成された IDを使用する必要があります。ただし、ページ タイトル、および div 内の画像とオブジェクトを除きます。 
    - タイトルを置換する場合は、 **title** キーワードを使用します。 
    - div 内の画像とオブジェクトを置換する場合は、**data-id** または **id** のどちらかを使用します。

次の例では、ターゲットの **id** 値が使用されています。生成された ID にプレフィックス # を使用しないでください。 次の例では、ターゲットに id 値が使用されています。生成された ID に # プレフィックスを使用しないでください。

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
| body<br /> body(ページの最初の div をターゲットとする) | いいえ | **はい** | いいえ |  
| </div><br /> object ([絶対配置](../howto/onenote-abs-pos.md)) | いいえ | **はい** | いいえ |  
| </div><br /> div (div 内) | **はい** (id のみ) | **はい** | **はい** |   
| div、img、object<br /> div (div 内) | **はい** | いいえ | **はい** |   
| ol、ul | **はい** (id のみ) | **はい** | **はい** |   
| テーブル | **はい** (id のみ) | いいえ | **はい** |   
| p、li、h1-h6 | **はい** (id のみ) | いいえ | **はい** |   
| title | **はい** | いいえ | いいえ |  
 

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

[Append child elements](#append-examples)&nbsp;&nbsp;|&nbsp;&nbsp;[Insert sibling elements](#insert-examples)&nbsp;&nbsp;|&nbsp;&nbsp;[Replace elements](#replace-examples)&nbsp;&nbsp;|&nbsp;&nbsp;[Complete PATCH requests](#complete-requests)


<a name="append-examples"></a>
### 子要素の追加
**append** アクションは、子を **body**、**div** (div 内)、**ol**、または ul 要素に追加します。position 属性は、子をターゲットの前と後のどちらに追加するかを指定します。既定の位置は **after** です。 The **position** attribute determines whether to append the child before or after the target. The default position is **after**.

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
 

 body 要素への追加

**body** は、任意のページ上の最初の div 内に子要素を追加するためのショートカットとして使用できます。

次の例では、ページ上の最初の div に 2 つの段落が最初の子および最後の子として追加されます。 body ターゲット内では # を使用しないでください。次の例では prepend が append + before のショートカットとして使用されています。 Don't use a # with the **body** target. This example uses the **prepend** action as a shortcut for **append** + **before**.

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
The **insert** action adds a sibling to the target element. The **position** attribute determines whether to insert the sibling before or after the target. The default position is **after**. See [elements that support **insert**](#support-matrix).

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
**data-id または生成された **id** をターゲット値として使用して、div 内の **img** 要素と **object** 要素を置換できます。ページ タイトルを置換するには、title キーワードを使用します。replaceをサポートする**その他のすべての要素には、生成された ID を使用する必要があります。 タイトルを置換する場合は、 **title** キーワードを使用します。 For all other [elements that support **replace**](#support-matrix), you must use the generated ID.

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

data-tag 属性の使用法の詳細については、「ノート シールを使用する」を参照してください。


<a name="complete-requests"></a>
### 完全な PATCH 要求の例
次の例に、完全な PATCH 要求を示します。

**テキスト コンテンツのみの要求**  
次の例は、**application/json** コンテンツ タイプを使用する PATCH 要求を示します。コンテンツにバイナリ データが含まれていない場合にこの形式を使用できます。 次の例は、application/json コンテンツ タイプを使用する PATCH 要求を示しています。コンテンツにバイナリ データが含まれていない場合にこの形式を使用できます。

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
 
バイナリ コンテンツを含むマルチパート要求  
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
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。Azure AD を使用した認証 (エンタープライズ アプリ)を参照してください。 認証とアクセス許可</p> |  
| Content-Type ヘッダー | <p>
           JSON 変更オブジェクトの配列の application/json`application/json`。直接メッセージ本文で送信するか、[マルチパート要求](#multipart)の必須の "Commands" 部分で送信するかです。マルチパート要求は、バイナリ データを送信するときに必要になり、multipart/form-data; boundary=part-boundary コンテンツ タイプを使用します。この {part-boundary} は、各データ部分の開始と終了を知らせる文字列です。</p><p>
           JSON 変更オブジェクトの配列の application/json`multipart/form-data; boundary=part-boundary`。直接メッセージ本文で送信するか、マルチパート要求の必須の "Commands" 部分で送信するかです。マルチパート要求は、バイナリ データを送信するときに必要になり、multipart/form-data; boundary=part-boundary コンテンツ タイプを使用します。この *{part-boundary}* は、各データ部分の開始と終了を知らせる文字列です。</p> |  
 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 204 HTTP ステータス コード。PATCH 要求に対して JSON データは返されません。 |  
| エラー | 要求が失敗すると、API は応答本文の @api.diagnostics オブジェクトに**エラー**を返します。 The request will fail if:<br /> JSON コンテンツに無効な属性が含まれているか、JSON コンテンツの形式が誤っている。<br /> **target**、**action**、または **content** 属性が欠落している。<br /> ターゲット要素が存在していない。<br /> - The format of the target value is invalid. Example, a **data-id** isn't prefixed with a #.<br /> 指定されたアクションがターゲット要素でサポートされていない。<br /> アクションまたは位置の値が無効である。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行うときに、この値を Date ヘッダーの値とともに使用できます。 |  
 

<a name="root-url"></a>
### OneNote サービスのルート URL の構築

[!INCLUDE [service root url section](../includes/onenote/service-root-section.xml)]


<a name="permissions"></a>
## アクセス許可

OneNote ページを更新するには、適切なアクセス許可を要求する必要があります。アプリの動作に必要な最低限のアクセス許可を選択してください。

[!INCLUDE [Update perms](../includes/onenote/update-perms.md)]

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md)」を参照してください。
   

<a name="see-also"></a>
## その他の技術情報

- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  
