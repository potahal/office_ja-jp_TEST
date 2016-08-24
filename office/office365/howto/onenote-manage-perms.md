---
ms.Toctitle: Manage permissions
title: "OneNote エンティティのアクセス許可を管理する"
description: "ノートブック、セクション グループおよびセクションの読み取りアクセス許可と書き込みアクセス許可を作成して管理する方法について説明します。"
ms.ContentId: 9dd85ba3-d01d-4148-b643-47b0f55cd99a
ms.date: May 24, 2016

---


[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# OneNote エンティティのアクセス許可を管理する

*__適用対象:__Office 365 のエンタープライズ ノートブック*

*permissions* エンドポイントを使用すると、ノートブック、セクション グループ、セクションに対する読み取りアクセス許可と書き込みアクセス許可管理できます。

<p id="indent1">`POST ../permissions`</p>
<p id="indent1">`GET ../permissions`</p>
<p id="indent1">`GET ../permissions/{permission-id}`</p>
<p id="indent1">`DELETE ../permissions/{permission-id}`</p>

>アクセス許可の管理は Office 365 の個人、サイト、統合グループのノートブックでサポートされていますが、OneDrive のコンシューマー ノートブックではサポートされていません。

<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、お使いのプラットフォーム用のサービス ルート URL から開始します。

[!INCLUDE [service root url enterprise only](../includes/onenote/service-root-url-ent.xml)]

その後に、ターゲットのノートブック、セクション グループ、またはセクション エンティティへのパスを追加します。それに続けて、*permissions* エンドポイントまたは *permissions/{id}* エンドポイントを追加します。

完全な要求 URI は、次の例のようになります。
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/notebooks/{id}/permissions/{id}`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/users/{id}/notes/sectiongroups/{id}/permissions`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/siteCollections/{id}/sites/{id}/notes/notebooks/{id}/permissions`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/groups/{id}/notes/sections/{id}/permissions/{id}`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]


<a name="create"></a>
## アクセス許可の作成または更新

ノートブック、セクション グループ、またはセクションのアクセス許可を作成または更新する場合は、該当するエンドポイントに POST 要求を送信します。要求ごとに作成または更新できるアクセス許可は 1 つのみです。 

アクセス許可は、[継承チェーン](#permission-inheritance-and-precedence)を下って、すべての OneNote のエンティティに適用されます。

より許容度の高いアクセス権を付与する場合は、アクセス許可を更新できます。ただし、アクセス権を制限するには、既存のアクセス許可を削除して、新しいアクセス許可を作成する必要があります「[アクセス許可、継承、および優先順位](#permission-inheritance-and-precedence)」を参照してください。


<p id="outdent">**ノートブックのアクセス許可を作成または更新する**</p>
<p id="indent">`POST ../notebooks/{notebook-id}/permissions`</p>

<p id="outdent">**セクション グループのアクセス許可を作成または更新する**</p>
<p id="indent">`POST ../sectiongroups/{sectiongroup-id}/permissions`</p>

<p id="outdent">**セクションのアクセス許可を作成または更新する**</p>
<p id="indent">`POST ../sections/{section-id}/permissions`</p>

メッセージ本文で、必要なパラメーターが含まれる JSON オブジェクトを送信します。 
 
```
{
    "userRole": "user-role", 
    "userId": "user-login-id"
}
```

| パラメーター | 説明 |  
|:------|:------|  
| userRole | [アクセス許可](#permission-inheritance-and-precedence)の種類: `Owner`、`Contributor`、または `Reader`。 |  
| userId | アクセス許可を割り当てるユーザーまたはグループのログイン。 API は、メンバーシップ プロバイダー名 (*i:0#.f&#124;membership&#124;username@domainname.com*)、またはユーザー プリンシパル ログイン名のみ (*username@domainname.com*) を含むクレーム形式を受け入れます。 |  

### 例

次の要求では、指定したノートブックのアクセス許可を作成します。 

#### 要求

```
POST ../v1.0/me/notes/notebooks/{notebook-id}/permissions
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "userRole": "Owner", 
    "userId": "i:0#.f|membership|alexd@domainname.com"
}
```

#### 応答

```
HTTP/1.1 201 Created

{
  "@odata.context":"https://www.onenote.com/api/v1.0/$metadata#me/notes/notebooks('1-313dc828-dd55-4c71-82c3-f9c30a40e7c5')/permissions/$entity",
  "userRole":"Owner",
  "userId":"i:0#.f|membership|alexd@domainname.com",
  "name":"Alex Darrow",
  "id":"1-23",
  "self":"https://www.onenote.com/api/v1.0/me/notes/notebooks/1-313dc828-dd55-4c71-82c3-f9c30a40e7c5/permissions/1-23",
}
```

### 要求および応答に関する情報

次の情報は、`POST /permissions` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。*{token}* は、登録済みアプリの有効な OAuth 2.0 アクセス トークンになります。</p><p>これがないか、無効の場合、要求は失敗し、401 ステータス コードが表示されます。 「[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)」を参照してください。</p> |  
| [アクセス許可のスコープ](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All |    

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 201 HTTP ステータス コード。 |   
| 応答本文 | JSON 形式でのアクセス許可の OData 表現。 Permission オブジェクトの説明については、「[アクセス許可の取得](#get-permissions)」を参照してください。 |
| エラー | 要求が失敗すると、API は応答本文に[エラー](../howto/onenote-error-codes.md)を返します。 |   
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="get"></a>
## アクセス許可の取得

ノートブック、セクション グループ、またはセクションのアクセス許可を取得する場合は、該当するエンドポイントに GET 要求を送信します。 

<p id="outdent">**ノートブックのアクセス許可を取得する**</p>
<p id="indent">`GET ../notebooks/{notebook-id}/permissions`</p>

<p id="outdent">**ノートブックの特定のアクセス許可を取得する**</p>
<p id="indent">`GET ../notebooks/{notebook-id}/permissions/{permission-id}`</p>

<p id="outdent">**セクション グループのアクセス許可を取得する**</p>
<p id="indent">`GET ../sectiongroups/{sectiongroup-id}/permissions`</p>

<p id="outdent">**セクション グループの特定のアクセス許可を取得する**</p>
<p id="indent">`GET ../sectiongroups/{sectiongroup-id}/permissions/{permission-id}`</p>

<p id="outdent">**セクションのアクセス許可を取得する**</p>
<p id="indent">`GET ../sections/{section-id}/permissions`</p>

<p id="outdent">**セクションの特定のアクセス許可を取得する**</p>
<p id="indent">`GET ../sections/{section-id}/permissions/{permission-id}`</p>

<br />
GET 要求は、ターゲット エンティティのユーザー ロールで最高のアクセス許可を返します。 詳細については、「[アクセス許可、継承、および優先順位](#permission-inheritance-and-precedence)」を参照してください。

`GET /permissions` 要求は、次に示すように OData クエリのオプションをサポートします。

<p id="indent">`GET ../permissions[?filter,orderby,select,top,skip,count]`</p>
<p id="indent">`GET ../permissions/{permission-id}[?select]`</p>

>*permissions* エンドポイントは、`expand` クエリ オプションをサポートしていません。

サポートされているクエリ文字列オプションや例などを含む OneNote エンティティの取得の詳細については、「[OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)」を参照してください。

### Permission オブジェクト

アクセス許可には、次のプロパティが含まれます。 

| プロパティ | 説明 | 
|:------|:------| 
| name | ユーザーまたはグループ プリンシパルの表示名。 例: `"name":"Everyone"` | 
| id | アクセス許可の一意の識別子 (`1-{principal-member-id}` の形式)。 例: `"id":"1-4"` | 
| self | Permission オブジェクトの URL。 | 
| userId | アクセス許可の割り当て先ユーザーまたはグループのログイン。 この値は、常にクレーム形式で返されます。例： *i:0#.f&#124;membership&#124;username@domainname.com*。 | 
| userRole | [アクセス許可](#permission-inheritance-and-precedence)の種類: `Owner`、`Contributor`、または `Reader`。 | 

### 例

次の要求では、指定したノートブックのすべてのアクセス許可を取得します。

#### 要求

```
GET ../v1.0/me/notes/notebooks/{notebook-id}/permissions
Authorization: Bearer {token}
Accept: application/json
```

#### 応答

```json
HTTP/1.1 200

{
  "@odata.context":"https://www.onenote.com/api/v1.0/$metadata#me/notes/notebooks('1-313dc828-dd55-4c71-82c3-f9c30a40e7c5')/permissions",
  "value":[
  {
    "userRole":"Owner",
    "userId":"c:0(.s|true",
    "name":"Everyone",
    "id":"1-4",
    "self":"https://www.onenote.com/api/v1.0/me/notes/notebooks/1-313dc828-dd55-4c71-82c3-f9c30a40e7c5/1-4"
  },
  {
    "userRole":"Owner",
    "userId":"c:0-.f|rolemanager|spo-grid-all-users/8461cbdd-15a6-45c8-b177-ac24f48a8bee",
    "name":"Everyone except external users",
    "id":"1-5",
    "self":"https://www.onenote.com/api/v1.0/me/notes/notebooks/1-313dc828-dd55-4c71-82c3-f9c30a40e7c5/permissions/1-5"
  },
  {
    "userRole":"Owner",
    "userId":"i:0#.f|membership|alexd@domainname.com",
    "name":"Alex Darrow",
    "id":"1-23",
    "self":"https://www.onenote.com/api/v1.0/me/notes/notebooks/1-313dc828-dd55-4c71-82c3-f9c30a40e7c5/permissions/1-23",
  }]
}
```

### 要求および応答に関する情報

次の情報は、`GET /permissions` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。*{token}* は、登録済みアプリの有効な OAuth 2.0 アクセス トークンになります。</p><p>これがないか、無効の場合、要求は失敗し、401 ステータス コードが表示されます。 「[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)」を参照してください。</p> |  
| [アクセス許可のスコープ](../howto/onenote-auth.md#onenote-perms-aad) | Notes.Read、Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、または Notes.ReadWrite.All |    

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 200 HTTP ステータス コードおよび要求されたアクセス許可。 |   
| 応答本文 | JSON 形式でのアクセス許可の OData 表現。 | 
| エラー | 要求が失敗すると、API は応答本文に[エラー](../howto/onenote-error-codes.md)を返します。 |   
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="delete"></a>
## アクセス許可の削除

ノートブック、セクション グループ、またはセクションのアクセス許可を削除する場合は、該当するエンドポイントに DELETE 要求を送信します。要求ごとに 1 つのアクセス許可を削除できます。

アクセス許可を削除すると、そのアクセス許可は[継承チェーン](#permission-inheritance-and-precedence)を下って、すべての OneNote エンティティから削除されます。

<p id="outdent">**ノートブックのアクセス許可を削除する**</p>
<p id="indent">`DELETE ../notebooks/{notebook-id}/permissions/{permission-id}`</p>

<p id="outdent">**セクション グループのアクセス許可を削除する**</p>
<p id="indent">`DELETE ../sectiongroups/{sectiongroup-id}/permissions/{permission-id}`</p>

<p id="outdent">**セクションのアクセス許可を削除する**</p>
<p id="indent">`DELETE ../sections/{section-id}/permissions/{permission-id}`</p>


### 例

次の要求では、指定したノートブックのアクセス許可を削除します。 

```
DELETE ../v1.0/me/notes/notebooks/1-313dc828-dd55-4c71-82c3-f9c30a40e7c5/permissions/1-23
Authorization: Bearer {token}
Accept: application/json
```

### 要求および応答に関する情報

次の情報は、`DELETE /permissions` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。*{token}* は、登録済みアプリの有効な OAuth 2.0 アクセス トークンになります。</p><p>これがないか、無効の場合、要求は失敗し、401 ステータス コードが表示されます。 「[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)」を参照してください。</p> |  
| [アクセス許可のスコープ](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All |    

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 204 HTTP ステータス コード。 |   
| エラー | 要求が失敗すると、API は応答本文に[エラー](../howto/onenote-error-codes.md)を返します。 |   
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="permission-inheritance-and-precedence"></a>
## アクセス許可、継承、および優先順位

次に示すアクセス許可は、ノートブック、セクション グループ、およびセクションに設定できます。

| アクセス許可 | 説明 | 
|:------|:------| 
| Reader | ノートブック、セクション グループ、およびセクションに対する読み取り専用アクセス。 | 
| Contributor | ノートブック、セクション グループ、およびセクションの追加、編集、および削除が可能。 | 
| Owner | 上記のすべてのアクセス許可に加え、アクセス許可の管理 (取得、作成、および削除) も可能。 | 

OneNote エンティティのアクセス許可を管理する場合は、アクセス許可の継承と優先順位について理解している必要があります。

- **継承**。エンティティは親のアクセス許可を継承します。つまり、ノートブックは、そのノートブックを含むドキュメント ライブラリのアクセス許可を継承することになります。このアクセス許可は、次に、そのノートブックに含まれる子セクション グループとセクションによって継承されます。ノートブック、セクション グループ、またはセクションにアクセス許可を明示的に設定すると、その子オブジェクトにもアクセス許可が反映されます。

- **優先順位**。OneNote エンティティに競合するアクセス許可が設定されると、最高の (最も許容度の高い) アクセス権が優先されます。1 つのエンティティに明示または継承によって複数のアクセス許可が適用される場合や、ユーザーまたはグループが複数のロールに属している場合には、競合するアクセス権のレベルがユーザーやグループに付与される可能性があります。 

こうした原則が、OneNote によるアクセス許可の管理方法を決定します。 次に例を示します。

- ノートブックまたはセクション グループのアクセス許可を作成すると、そのアクセス許可はすべての子に適用されます。

- ノートブックまたはセクション グループのアクセス許可を削除すると、そのアクセス許可はすべての子から削除されます。

- アクセス許可を取得すると、OneNote API は競合しているアクセス許可を持つロールの最高のアクセス許可のみを返します。

- より許容度の高いアクセス権をユーザーまたはグループに付与する場合は、アクセス許可を更新できます。 ただしアクセス権を制限する場合は、まず、より許容度の高いアクセス許可を削除してから、限定的なアクセス権で新しいアクセス許可を作成する必要があります。 
 これは、実際には `POST /permissions` 要求がエンティティのアクセス許可コレクションにユーザー ロールを追加していて、最も許容度の高いアクセス権が優先されるためです。 つまり、Reader アクセス許可は Contributor または Owner のアクセス権を含むように更新することはできますが、Contributor アクセス許可を Reader アクセス権のみが許可されるアクセス許可に更新することはできないということです。


<a name="root-url"></a>
## OneNote サービスのルート URL の構築

[!INCLUDE [service root url section enterprise only](../includes/onenote/service-root-section-ent.xml)]


<a name="see-also"></a>
## その他のリソース

- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://stackoverflow.com/questions/tagged/onenote-api+onenote) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)
