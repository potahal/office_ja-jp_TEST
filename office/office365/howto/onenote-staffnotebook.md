---
ms.Toctitle: Work with staff notebooks
title: "スタッフ ノートブックの操作"
description: "スタッフ ノートブックの作成方法と管理方法について説明します。"
ms.ContentId: 5ba2d15c-3d3f-46f9-8e5b-7eeae8a4b2d7
ms.date: July 26, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# スタッフ ノートブックの操作

*__適用対象:__Office 365 のエンタープライズ ノートブック*

世界中の学校や大学で[スタッフ ノートブック](https://www.onenote.com/staffnotebookedu)を使用して、生産性の向上、関心度の向上、共同作業の促進に役立てています。

*staffNotebooks* エンドポイントを使用して、スタッフ ノートブックの作成や、リーダーやメンバーの追加および削除などのスタッフ ノートブックの一般的なタスクを実行できます。

>OneNote API には、スタッフ ノートブックに固有の操作のための *staffNotebooks* エンドポイントが含まれています。


<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、お使いのプラットフォーム用のサービス ルート URL から開始します。

[!INCLUDE [service root url enterprise only](../includes/onenote/service-root-url-ent.xml)]

次に *staffNotebooks* エンドポイントを追加し、必要に応じてリソース パスを続けて追加します。

<p id="outdent1"><b>[スタッフ ノートブックの作成](#create)</b></p>
<p id="indent1">`../staffNotebooks[?omkt,sendemail]`</p>

<p id="outdent1"><b>[スタッフ ノートブックの更新](#update)</b></p>
<p id="indent1">`../staffNotebooks/{notebook-id}`</p>

<p id="outdent1"><b>[1 つまたは複数のスタッフ ノートブックの取得](#get)</b></p>
<p id="indent1">`../staffNotebooks`</p>
<p id="indent1">`../staffNotebooks/{notebook-id}`</p>

<p id="outdent1"><b>[スタッフ ノートブックの削除](#delete)</b></p>
<p id="indent1">`../staffNotebooks/{notebook-id}`</p>

<p id="outdent1"><b>[メンバーまたはリーダーの追加](#add-people)</b></p>
<p id="indent1">`../staffNotebooks/{notebook-id}/members`</p>
<p id="indent1">`../staffNotebooks/{notebook-id}/leaders`</p>

<p id="outdent1"><b>[メンバーまたはリーダーの削除](#remove-people)</b></p>
<p id="indent1">`../staffNotebooks/{notebook-id}/members/{member-id}`</p>
<p id="indent1">`../staffNotebooks/{notebook-id}/leaders/{leader-id}`</p>

<p id="outdent1"><b>[セクションの挿入](#insert-sections)</b></p>
<p id="indent1">`../staffNotebooks/{notebook-id}/copySectionsToContentLibrary`</p>

<br />
完全な要求 URI は、次の例のようになります。
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/staffNotebooks/{id}/leaders/{id}`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/users/{id}/notes/staffNotebooks/{id}/members`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/siteCollections/{id}/sites/{id}/notes/staffNotebooks`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/groups/{id}/notes/staffNotebooks/{id}`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/staffNotebooks/{id}/copySectionsToContentLibrary`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]


<a name="create"></a>
## スタッフ ノートブックの作成

スタッフ ノートブックを作成するには、POST 要求を *staffNotebooks* エンドポイントに送信します。 

<p id="indent">`POST ../staffNotebooks[?omkt,sendemail]`</p>

メッセージ本文で、スタッフ ノートブック作成パラメーターが含まれる JSON オブジェクトを送信します。 

```json
{
    "name": "notebook-name",
    "memberSections": [ 
        "section1-name", 
        "section2-name"
    ],
    "leaders": [
        {
            "id": "alias@tenant",
            "principalType": "Person-or-Group"
        }
    ],
    "members": [
        {
            "id": "alias@tenant",
            "principalType": "Person-or-Group" 
        },
        {
            "id": "alias@tenant",
            "principalType": "Person-or-Group"
        },
        {
            "id": "alias@tenant",
            "principalType": "Person-or-Group"
        }
   ], 
   "hasLeaderOnlySectionGroup": true
}
```

| パラメーター | 説明 |  
|:------|:------|  
| name | ノートブックの名前。 |  
| memberSections | 1 つまたは複数のセクション名を含む配列。 これらのセクションは、各メンバーのセクション グループに作成されます。 |  
| leaders | 1 つまたは複数のプリンシパル オブジェクトを含む配列。 |
| members | 1 つまたは複数のプリンシパル オブジェクトを含む配列。 セクション グループは、メンバーごとに作成されます。 |    
| hasLeaderOnlySectionGroup | リーダーのみに表示される *リーダー限定*セクション グループを作成する場合は `true`。 | 
| omkt | ノートブックの[言語](#supported-langs)を指定する URL クエリ パラメーター。 既定値は `en-us` です。 例: `?omkt=es-es` | 
| sendemail | ノートブックに割り当てられているリーダーおよびメンバーにノートブックが作成されるときに、電子メール通知を送信するかどうかを指定する URL クエリ パラメーター。 既定値は `false` です。 |

<br />
リーダーとメンバーは、以下のプロパティが含まれるプリンシパル オブジェクトで表されます。

| パラメーター | 説明 |  
|:------|:------|  
| id | Office 365 のユーザー プリンシパル名。<br /><br />ユーザーとグループの詳細については、[Azure AD Graph API の資料](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog)を参照してください。 |  
| principalType | `Person` または `Group` | 


<a name="supported-langs"></a>
### サポートされている言語

`omkt={language-code}` URL クエリ パラメーターを使用して、スタッフ ノートブックを特定の言語で作成できます。 次に例を示します。

<p id="indent">`POST ../staffNotebooks?omkt=de-de`</p>

以下の言語コードがサポートされています。 既定値は `en-us` です。

| コード | 言語 | 
|:------|:------| 
| bg-bg | Български (България) | 
| cs-cz | Čeština (Česká republika) | 
| da-dk | Dansk (Danmark) | 
| de-de | Deutsch (Deutschland) | 
| el-gr | Ελληνικά (Ελλάδα) | 
| en-us | 英語 (米国) | 
| es-es | Español (España) | 
| et-ee | Eesti (Eesti) | 
| fi-fi | Suomi (Suomi) | 
| fr-fr | Français (France) | 
| hi-in | हिंदी (भारत) | 
| hr-hr | Hrvatski (Hrvatska) | 
| hu-hu | Magyar (Magyarország) | 
| id-id | Bahasa Indonesia (Indonesia) | 
| it-it | Italiano (Italia) | 
| ja-jp | 日本語 (日本) | 
| kk-kz | ҚАЗАҚ (ҚАЗАҚСТАН) | 
| ko-kr | 한국어 (대한민국) | 
| lt-lt | Lietuvių (Lietuva) | 
| lv-lv | Latviešu (Latvija) | 
| ms-my | Bahasa Melayu (Asia Tenggara) | 
| nb-no | Norsk (Norge) | 
| nl-nl | Nederlands (Nederland) | 
| pl-pl | Polski (Polska) | 
| pt-br | Português (Brasil) | 
| pt-pt | Português (Portugal) | 
| ro-ro | Română (România) | 
| ru-ru | Русский (Россия) | 
| sk-sk | Slovenčina (Slovenská republika) | 
| sl-si | Slovenski (Slovenija) | 
| sr-latn-rs | Srpski (Rep. Srbija i Rep. Crna Gora) | 
| sv-se | Svenska (Sverige) | 
| th-th | ไทย (ไทย) | 
| tr-tr | Türkçe (Türkiye) | 
| uk-ua | Українська (Україна) | 
| vi-vn | Tiếng Việt (Việt Nam) | 
| zh-cn | 简体中文 (中国) | 
| zh-tw | 繁體中文 (台灣) | 


### 例

次の要求は、*Staff Meetings* という名前のスタッフ ノートブックを作成します。

```
POST ../v1.0/users/{leader-id}/notes/staffNotebooks?sendemail=true
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "name": "Staff Meetings",
    "memberSections": [
        "Staff Notes",
        "Meeting Summaries",
    ],
    "leaders": [
        {
            "id": "leader1@contoso.com",
            "principalType": "Person"
        }
    ],
    "members": [
        {
            "id": "member1@contoso.com",
            "principalType": "Person"
        },
        {
            "id": "member2@contoso.com",
            "principalType": "Person" 
        },
        {
            "id": "member3@contoso.com",
            "principalType": "Person"
        },
        {
            "id": "member4@contoso.com",
            "principalType": "Person"
        }
    ],
    "hasLeaderOnlySectionGroup": true
}
```

これによって、それぞれに配布資料、スタッフ ノート、会議の要約を含む 4 つのメンバー セクション グループのあるスタッフ ノートブックが作成されます。 メンバーごとに作成されたセクション グループには、そのメンバーとリーダーのみがアクセス可能です。 またこれは、リーダーのみに表示される *リーダー限定*セクション グループも作成します。 `sendemail=true` クエリ パラメーターは、ノートブックが作成されるとリーダーとメンバーに電子メール通知を送信するように指定します。


### 要求および応答に関する情報

次の情報は、`POST /staffNotebooks` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |  
| Content-Type ヘッダー | `application/json` |  
| Accept ヘッダー | `application/json` |  
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 201 HTTP ステータス コード。 |  
| 応答本文 | JSON 形式の新しいノートブックの OData 表記。<br /><br />[通常のノートブック プロパティ](http://dev.onenote.com/docs#/reference/get-notebooks)に加え、スタッフ ノートブックには次のプロパティが含まれます:<ul><li>**memberSections**. ノートブック内のメンバー セクション。</li><li>**leaders**。 ノートブックにアクセスできるリーダー。</li><li>**member**。 ノートブックにアクセスできるメンバー。</li><li>**hasLeaderOnlySectionGroup**. ノートブックに *リーダー限定*セクション グループが含まれている場合は `true`、含まれていない場合は `false`。</li></ul> |  
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |    
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="update"></a>
## スタッフ ノートブックの更新

スタッフ ノートブックを更新するには、PATCH 要求を *staffNotebooks/{notebook-id}* エンドポイントに送信します。 

>現時点では、**hasLeaderOnlySectionGroup** プロパティのみ PATCH 要求で更新することができます。 

<p id="indent">`PATCH ../staffNotebooks/{notebook-id}`</p>

メッセージ本文で、更新パラメーターが含まれる JSON オブジェクトを送信します。

```json
{
    "hasLeaderOnlySectionGroup": true
}
```

| パラメーター | 説明 |  
|:------|:------|  
| hasLeaderOnlySectionGroup | リーダーにのみ表示される *リーダー限定*セクション グループを追加する場合は `true`。 `false` はサポートされていません。 | 

スタッフ ノートブックを変更する他の方法について、次の方法を参照してください。「[メンバーまたはリーダーの追加](#add-people)」、「[メンバーまたはリーダーの削除](#remove-people)」、「[セクションの挿入](#insert-sections)」

### 例

次の要求によって、*リーダー限定*セクション グループを指定のスタッフ ノートブックに追加します。 

```
PATCH ../v1.0/users/{leader-id}/notes/staffNotebooks/{notebook-id}
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "hasLeaderOnlySectionGroup": true
}
```

新しい *リーダー限定*セクション グループがリーダーにのみ表示されます。


### 要求および応答に関する情報

次の情報は、`PATCH ../staffNotebooks/{notebook-id}` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |  
| Content-Type ヘッダー | `application/json` |  
| Accept ヘッダー | `application/json` |  
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 204 HTTP ステータス コード。 |  
| エラー | 要求が失敗すると、API は応答本文に[エラー](../howto/onenote-error-codes.md)を返します。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="get"></a>
## スタッフ ノートブックの取得

1 つまたは複数のスタッフ ノートブックを取得するには、GET 要求を *staffNotebooks* エンドポイントに送信します。

<p id="outdent"><b>1 つまたは複数のスタッフ ノートブックの取得</b></p>
<p id="indent">`GET ../staffNotebooks[?filter,orderby,select,top,skip,expand,count]`</p>

<p id="outdent"><b>特定のスタッフ ノートブックの取得</b></p>
<p id="indent">`GET ../staffNotebooks/{notebook-id}[?select,expand]`</p>

<br />
ノートブックは、`leaders` プロパティと `members` プロパティを展開できます。 既定の並べ替え順序は ￼`name asc` です。

スタッフ ノートブックは `GET /notebooks` 要求に対しても戻されますが、結果にはスタッフ ノートブックに固有のプロパティは含まれません。


### 例

次の要求では、2016 年 1 月 1 日以降に作成されたスタッフ ノートブックを取得します。

```
GET ../v1.0/users/{leader-id}/notes/staffNotebooks?filter=createdTime%20ge%202016-01-01 
Authorization: Bearer {token}
Accept: application/json
``` 

サポートされているクエリ文字列オプションや例などを含むノートブックの取得について詳しくは、「[OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)」を参照してください。

### 要求および応答に関する情報

次の情報は、`GET /staffNotebooks` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |  
| Accept ヘッダー | `application/json` | 
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.Read、Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、または Notes.ReadWrite.All | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 200 HTTP ステータス コード。 |  
| 応答本文 | JSON 形式のスタッフ ノートブックの OData 表記。<br /><br />[通常のノートブック プロパティ](http://dev.onenote.com/docs#/reference/get-notebooks)に加え、スタッフ ノートブックには次のプロパティが含まれます:<ul><li>**memberSections**. ノートブック内のメンバー セクション。</li><li>**leaders**。 ノートブックにアクセスできるリーダー。</li><li>**member**。 ノートブックにアクセスできるメンバー。</li><li>**hasLeaderOnlySectionGroup**. ノートブックに *リーダー限定*セクション グループが含まれている場合は `true`、含まれていない場合は `false`。</li></ul> |  
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |   
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |   


<a name="delete"></a>
## スタッフ ノートブックの削除

スタッフ ノートブックを削除するには、DELETE 要求を *staffNotebooks/{notebook-id}* エンドポイントに送信します。

<p id="indent">`DELETE ../staffNotebooks/{notebook-id}`</p>

### 例

次のような要求は、指定したスタッフのノートブックを削除します。

```
DELETE ../v1.0/users/{leader-id}/notes/staffNotebooks/{notebook-id} 
Authorization: Bearer {token}
Accept: application/json
``` 

### 要求および応答に関する情報

次の情報は、`DELETE ../staffNotebooks/{notebook-id}` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |   
| Accept ヘッダー | `application/json` |  
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 204 HTTP ステータス コード。 |  
| エラー | 要求が失敗すると、API は応答本文に[エラー](../howto/onenote-error-codes.md)を返します。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |   


<a name="add-people"></a>
## メンバーおよびリーダーの追加

リーダーおよびメンバーを追加すると、そのスタッフ ノートブックへのアクセスが可能になります。 メンバーを追加すると、メンバー セクション グループも作成されます。 このセクション グループは、そのメンバーとリーダーのみがアクセス可能で、ノートブックに対して定義されているメンバー セクションが含まれています。

スタッフ ノートブックにメンバーまたはリーダーを追加するには、適切なエンドポイントに POST 要求を送信します。

<p id="outdent"><b>メンバーを追加する</b></p>
<p id="indent">`POST ../staffNotebooks/{notebook-id}/members`</p>

<p id="outdent"><b>リーダーを追加する</b></p>
<p id="indent">`POST ../staffNotebooks/{notebook-id}/leaders`</p>

<br />
JSON のプリンシパル オブジェクトを、メッセージの本文に送信します。 要求ごとに 1 名のメンバー、または 1 名のリーダーを追加できます。 

```json
{
    "id": "alias@tenant",
    "principalType": "Person-or-Group"
}
```

リーダーとメンバーは、以下のプロパティが含まれるプリンシパル オブジェクトで表されます。

| パラメーター | 説明 |  
|:------|:------|  
| id | Office 365 のユーザー プリンシパル名。ユーザーとグループの詳細については、[Azure AD Graph API の資料](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog) を参照してください。 |  
| principalType | `Person` または `Group` |  


### 例

次の要求によって、リーダーを指定のスタッフ ノートブックに追加します。

```
POST ../v1.0/users/{leader-id}/notes/staffNotebooks/{notebook-id}/leaders 
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "id": "leader2@contoso.com",
    "principalType": "Person"
}
``` 

### 要求および応答に関する情報

次の情報は、`POST /members` および `POST /leaders` の要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |  
| Content-Type ヘッダー | `application/json` |    
| Accept ヘッダー | `application/json` |  
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All |  

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 201 HTTP ステータス コード。 |  
| 応答本文 | メンバーまたはリーダーが追加されました。 |
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |


<a name="remove-people"></a>
## メンバーおよびリーダーの削除

メンバーとリーダーをスタッフ ノートブックから削除すると、ノートブックへのアクセスが取り消されますが、コンテンツは削除されません。

スタッフ ノートブックからメンバーまたはリーダーを削除するには、適切なエンドポイントに DELETE 要求を送信します。

<p id="outdent"><b>メンバーを削除する</b></p>
<p id="indent">`DELETE ../staffNotebooks/{notebook-id}/members/{member-id}`</p>

<p id="outdent"><b>リーダーを削除する</b></p>
<p id="indent">`DELETE ../staffNotebooks/{notebook-id}/leaders/{leader-id}`</p>

<br />
要求ごとに 1 名のメンバー、または 1 名のリーダーを削除できます。


### 例

次の要求は、指定したスタッフ ノートブックから指定したメンバーを削除します。

```
DELETE ../v1.0/users/{leader-id}/notes/staffNotebooks/{notebook-id}/members/{member-id} 
Authorization: Bearer {token}
Accept: application/json
``` 

### 要求および応答に関する情報

次の情報は、`DELETE /members` および `DELETE /leaders` の要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |   
| Accept ヘッダー | `application/json` |  
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All |    

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 204 HTTP ステータス コード。 |   
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |   
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="insert-sections"></a>
## セクションの挿入

Office 365 のノートブックからの特定のセクションをコピーし、それらをスタッフ ノートブックのコンテンツ ライブラリに挿入するには、*copySectionsToContentLibrary* を使用します。 コンテンツ ライブラリとは、リーダーが読み取り/書き込みアクセス許可を持ち、メンバーが読み取りアクセス許可を持つ、スタッフ ノートブック内のセクション グループです。

スタッフ ノートブックにセクションを挿入するには、POST 要求を対象のスタッフ ノートブックの *copySectionsToContentLibrary* エンドポイントに送信します。 次に例を示します。

<p id="indent">`POST ../staffNotebooks/{notebook-id}/copySectionsToContentLibrary`</p>

メッセージ本文で、*sectionIds* パラメーターが含まれる JSON オブジェクトを送信します。 

```json
{
    "sectionIds": [
        "section1-id", 
        "section2-id",
        ...
    ]
}
```

| パラメーター | 説明 |  
|:------|:------|  
| sectionIds | スタッフ ノートブックに挿入するセクションの ID を含む配列です。 |    

ターゲット セクションとノートブック (所有または共有) へのアクセス権限が必要です。 すべてのターゲットは、同じテナント内にある必要があります。

### 例

次の要求は、指定したスタッフ ノートブックのコンテンツ ライブラリに 2 つのセクションを挿入します。

```
POST ../v1.0/me/notes/staffNotebooks/{notebook-id}/copySectionsToContentLibrary
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "sectionIds": [
        "1-85ba33b1-4959-4102-8dcd-d98e4e56e56f", 
        "1-8ba42j81-4959-4102-8dcd-d98e4e94s62ef"
    ]
}
```

### 要求および応答に関する情報

次の情報は、`POST /copySectionsToContentLibrary` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |  
| Content-Type ヘッダー | `application/json` |  
| Accept ヘッダー | `application/json` |  
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、Notes.ReadWrite.All | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 201 HTTP ステータス コード。 |  
| エラー | 作成要求が失敗すると、API は応答本文に[エラー](../howto/onenote-error-codes.md)を返します。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="root-url"></a>
## OneNote サービスのルート URL の構築

[!INCLUDE [service root url section enterprise only](../includes/onenote/service-root-section-ent.xml)]


<a name="see-also"></a>
## その他のリソース

- [OneNote スタッフ ノートブック](https://www.onenote.com/staffnotebookedu) (概要および機能)
- [クラス ノートブックの操作](../howto/onenote-classnotebook.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://stackoverflow.com/questions/tagged/onenote-api+onenote) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)
