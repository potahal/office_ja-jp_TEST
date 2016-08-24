---
ms.Toctitle: Copy notebooks, sections, and pages
title: "ノートブック、セクション、ページをコピーする" 
description: "Use the OneNote API to copy notebooks, sections, and pages."
ms.ContentId: b21fe8c7-dab8-4efd-a3ac-b07c4c39f60d
ms.date: January 26, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

<style>##simpletable {margin:-5px 0px 0px 0px; border:none;} #simplecell {border:none; padding:15px 20px; background-color:white;}</style>


# ノートブック、セクション、ページをコピーする

**__適用対象:__Office 365 のエンタープライズ ノートブックのみ**

OneNote ノートブック、セクション、またはページをコピーする場合は、それぞれに対応する *copy* アクション エンドポイントに POST 要求を送信します。次に例を示します。 次に例を示します。

<p id="indent">`POST ../notes/sections/{id}/copyToNotebook`</p>

メッセージの本文で JSON のコピー オブジェクトを送信します。要求が成功すると、OneNote API は 202 HTTP 状態コードと **Operation-Location** ヘッダーを返します。その後、結果について、操作エンドポイントをポーリングできます。

>>現時点では、コピー機能は Office 365 の個人、サイト、および統合グループのノートブックでサポートされていますが、OneDrive のコンシューマー ノートブックではサポートされていません。

<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、お使いのプラットフォーム用のサービス ルート URL から開始します。

[!INCLUDE [service root url enterprise only](../includes/onenote/service-root-url-ent.xml)]

その後に、それぞれに対応する *copy* アクション エンドポイントを追加します。

<p id="outdent1"><b>ページをセクションにコピーする</b></p>
<p id="indent1">`../pages/{id}/copyToSection`</p>

<p id="outdent1"><b>セクションをノートブックにコピーする</b></p>
<p id="indent1">`../sections/{id}/copyToNotebook`</p>

<p id="outdent1"><b>セクションをセクション グループにコピーする</b></p>
<p id="indent1">`../sections/{id}/copyToSectionGroup`</p>

<p id="outdent1"><b>ノートブックをコピーする</b></p>
<p id="indent1">`../notebooks/{id}/copyNotebook`</p> 
<p id="indent1">ノートブックは、コピー先ドキュメント ライブラリの [ノートブック] フォルダーにコピーされます。[ノートブック] フォルダーが存在しない場合は、そのフォルダーが作成されます。 The Notebooks folder is created if it doesn't exist.</p>

<br />
完全な要求 URI は、次のいずれかのようになります。

<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/sections/{id}/copyToNotebook`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/users/{id}/notes/sections/{id}/copytosectiongroup`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/siteCollections/{id}/sites/{id}/notes/pages/{id}/copyToSection`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/groups/{id}/notes/notebooks/{id}/copyNotebook`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]


<a name="message-body"></a>
## メッセージ本文の構築

メッセージ本文では、操作に必要なパラメーターを格納する JSON オブジェクトを送信します。パラメーターが必要ない場合は、からの本文を送信してもかまいません。

| パラメーター | 説明 |  
|:------|:------|  
| id | コピー先のノートブックまたはセクション グループの ID (セクションの場合)、またはコピー先のセクションの ID (ページの場合)。copyToNotebook、copyToSectionGroup、copyToSection でのみ使用します。<br /><br />Used with **copyToNotebook**, **copyToSectionGroup**, and **copyToSection** only. |  
| siteCollectionId | アイテムのコピー先のサイトが含まれている SharePoint サイト コレクションの ID。siteId で使用し、SharePoint サイトにコピーする場合にのみ使用します。<br /><br />アイテムのコピー先のサイトが含まれている SharePoint サイト コレクションの ID。**siteId** で使用し、SharePoint サイトにコピーする場合にのみ使用します。 |   
| siteId | アイテムのコピー先の SharePoint サイト。siteCollectionIdで使用し、SharePoint サイトにコピーする場合にのみ使用します。<br /><br />アイテムのコピー先の SharePoint サイト。**siteCollectionId**で使用し、SharePoint サイトにコピーする場合にのみ使用します。 |  
| groupId | アイテムのコピー先のグループの ID。統合グループにコピーする場合にのみ使用します。<br /><br />アイテムのコピー先のグループの ID。統合グループにコピーする場合にのみ使用します。 |  
| renameAs | コピーするフィルターの名前を指定します。<br /><br />Used with **copyNotebook**, **copyToNotebook**, and **copyToSectionGroup** only. Defaults to the name of the existing item. |  

[ノートブック、セクション グループ、およびセクションの ID を取得する](../howto/onenote-get-content.md)方法と、[サイト コレクションおよびサイトの ID を取得する](#get-site-id)方法について学習してください。グループ ID の取得方法の詳細については、[Azure AD Graph API のドキュメント](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)を参照してください。


<a name="example"></a>
## コピー操作のフロー例

まず、コピーするアイテムに対する *copy* アクションに POST 要求を送信します。コピー元とコピー先が同じテナント内にあれば、ユーザーがアクセスできる (所有しているか、共有している) ノートブックからコピーできます。

次に示す例では、SharePoint チーム サイトに個人用のノートブックをコピーします。この要求には **renameAs** パラメーターが含まれていないため、新しいノートブックは既存の名前を使用することになります。 

```
POST https://www.onenote.com/api/v1.0/me/notes/notebooks/1-db247796-f4d1-4972-a869-942919bf9923/copyNotebook
Authorization: Bearer {token}
Content-Type: application/json 

{
  "siteCollectionId":"0f6dbd5d-d179-49c6-aabd-15830ea90ca8",
  "siteId":"3ba679cf-4470-466e-bc20-053bdfec75bf"
}
```

>>コピー操作では、コピー元ノートブックのアクセス許可が優先されるため、ノートブックをコピーするには、認証されたユーザーがコピー元ノートブックにアクセスできる必要があります。ただし、コピー元のアクセス許可がコピーに維持されることはありません。コピーには、そのコピーをユーザーが作成したものとしてのアクセス許可が付与されます。 However, copies don't retain the permissions of the source. The copy has permissions as though the user just created it. 

呼び出しが成功すると、OneNote API は 202 状態コードと **Operation-Location** ヘッダーを返します。次に、応答からの抜粋を示します。 Here's an excerpt of the response: 

```
HTTP/1.1 202 Accepted
Location: https://www.onenote.com/api/v1.0/me/notes/notebooks/1-db247796-f4d1-4972-a869-942919bf9923
X-CorrelationId: 8a211d7c-220b-413d-8022-9a946499fcfb
Operation-Location: https://www.onenote.com/api/beta/myOrganization/siteCollections/0f6dbd5d-d179-49c6-aabd-15830ea90ca8/sites/0f6dbd5d-d179-49c6-aabd-15830ea90ca8/notes/operations/copy-8a211d7c-220b-413d-8022-9a946499fcfb
...
```

その後で、**Operation-Location** エンドポイントをポーリングして、コピー操作の状態を取得します。 

```
GET https://www.onenote.com/api/beta/myOrganization/siteCollections/0f6dbd5d-d179-49c6-aabd-15830ea90ca8/sites/0f6dbd5d-d179-49c6-aabd-15830ea90ca8/notes/operations/copy-8a211d7c-220b-413d-8022-9a946499fcfb
Authorization: Bearer {token}
Accept: application/json
```
  
OneNote API は、現在の状態を示す **OperationModel** オブジェクトを返します。次に示す応答例は、状態が完了 (completed) のときに返されます。 OneNote API は、現在の状態を示す OperationModel オブジェクトを返します。次に示す応答例は、状態が完了 (completed) のときに返されます。 

```json
{
  "@odata.context":"https://www.onenote.com/api/beta/$metadata#myOrganization/siteCollections('0f6dbd5d-d179-49c6-aabd-15830ea90ca8')/sites('0f6dbd5d-d179-49c6-aabd-15830ea90ca8')/notes/operations/$entity",
  "id":"copy-1c5be75c-e7db-4219-8145-a2d6c3f171a33ec9f3da-2b24-4fb1-a776-fe8c8cd1410f",
  "status":"completed",
  "createdDateTime":"2015-09-16T17:32:07.048Z",
  "lastActionDateTime":"2015-09-16T17:32:17.7777639Z",
  "resourceLocation":"https://www.onenote.com/api/v1.0/myOrganization/siteCollections/0f6dbd5d-d179-49c6-aabd-15830ea90ca8/sites/3ba679cf-4470-466e-bc20-053bdfec75bf/notes/notebooks/1-bde29eeb-66e2-4fed-8d48-51cd1bf32511",
  "resourceId":null,"
  "error":null
}
```

状態は、完了 (**completed**)、実行中 (**running**)、または失敗 (**failed**) のいずれかになります。 

- **completed** の場合は、**resourceLocation** プロパティに新しいコピーのリソース エンドポイントが格納されます。 
- **running** の場合は、**percentComplete** プロパティで完了までの見積パーセンテージが示されます。
- **failed** の場合は、**error** プロパティと **@api.diagnostics** プロパティからエラー情報が得られます。

操作が完了または失敗するまで、操作エンドポイントをポーリングできます。 


<a name="request-response-info"></a>
## 要求および応答に関する情報
  
| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | `Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。<br /><br />欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。Azure AD を使用した認証 (エンタープライズ アプリ)を参照してください。 認証とアクセス許可 |  
| Content-Type ヘッダー | `application/json` |  
| 承諾ヘッダー | `application/json` | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 202 状態の HTTP 状態コード。 |   
| Operation-Location ヘッダー | The URL to poll for the status of the operation.<br /><br />操作の状態についてポーリングする URL。操作エンドポイントをポーリングすると、操作の状態などの情報を格納している **OperationModel** オブジェクトが返されます。 | 
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行うときに、この値を Date ヘッダーの値とともに使用できます。 |


<a name="root-url"></a>
### OneNote サービスのルート URL の構築

[!INCLUDE [service root url section enterprise only](../includes/onenote/service-root-section-ent.xml)]


<a name="permissions"></a>
## アクセス許可

OneNote のノートブック、セクション、およびページをコピーする場合は、適切なアクセス許可を要求する必要があります。アプリの動作に必要な最低限のアクセス許可を選択してください。

[!INCLUDE [Create perms](../includes/onenote/create-perms.md)] 

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md)」を参照してください。


<a name="see-also"></a>
## その他の技術情報

- [OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)
- [OneNote ページの作成](../howto/onenote-create-page.md)
- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182)
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  


