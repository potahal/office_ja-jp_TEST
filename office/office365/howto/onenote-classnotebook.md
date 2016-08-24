---
ms.Toctitle: Work with class notebooks
title: "クラス ノートブックの操作"
description: "クラス ノートブックの作成方法と管理方法について説明します。"
ms.ContentId: b3eca630-4f08-4157-b01b-c38df7782648
ms.date: May 24, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# クラス ノートブックの操作

*__適用対象:__Office 365 のEnterprise ノートブック*

世界中の学校や大学で[クラス ノートブック](https://www.onenote.com/classnotebook)を使用して、生産性の向上、関心度の向上、共同作業の促進に役立てています。すべてのクラス、プロジェクト、学期、課題でもクラス ノートブックを活用できます。

*classNotebooks* エンドポイントを使用して、クラス ノートブックの作成や、生徒の追加および削除などのクラス ノートブックの一般的なタスクを実行できます。

>OneNote API には、クラス ノートブックに固有の操作のための *classNotebooks* エンドポイントが含まれています。


<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、お使いのプラットフォーム用のサービス ルート URL から開始します。

[!INCLUDE [service root url enterprise only](../includes/onenote/service-root-url-ent.xml)]

次に *classNotebooks* エンドポイントを追加し、必要に応じてリソース パスを続けて追加します。

<p id="outdent1"><b>[クラス ノートブックの作成](#create)</b></p>
<p id="indent1">`../classNotebooks[?omkt,sendemail]`</p>

<p id="outdent1"><b>[クラス ノートブックの更新](#update)</b></p>
<p id="indent1">`../classNotebooks/{notebook-id}`</p>

<p id="outdent1"><b>[1 つまたは複数のクラス ノートブックの取得](#get)</b></p>
<p id="indent1">`../classNotebooks`</p>
<p id="indent1">`../classNotebooks/{notebook-id}`</p>

<p id="outdent1"><b>[クラス ノートブックの削除](#delete)</b></p>
<p id="indent1">`../classNotebooks/{notebook-id}`</p>

<p id="outdent1"><b>[生徒または教師の追加](#add-people)</b></p>
<p id="indent1">`../classNotebooks/{notebook-id}/students`</p>
<p id="indent1">`../classNotebooks/{notebook-id}/teachers`</p>

<p id="outdent1"><b>[生徒または教師の削除](#remove-people)</b></p>
<p id="indent1">`../classNotebooks/{notebook-id}/students/{student-id}`</p>
<p id="indent1">`../classNotebooks/{notebook-id}/teachers/{teacher-id}`</p>

<p id="outdent1"><b>[セクションの挿入](#insert-sections)</b></p>
<p id="indent1">`../classNotebooks/{notebook-id}/copySectionsToContentLibrary`</p>

<br />
完全な要求 URI は、次の例のようになります。
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/classNotebooks/{id}/teachers/{id}`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/users/{id}/notes/classNotebooks/{id}/students`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/siteCollections/{id}/sites/{id}/notes/classNotebooks`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/groups/{id}/notes/classNotebooks/{id}`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/classNotebooks/{id}/copySectionsToContentLibrary`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]


<a name="create"></a>
## クラス ノートブックの作成

クラス ノートブックを作成するには、POST 要求を *classNotebooks* エンドポイントに送信します。 

<p id="indent">`POST ../classNotebooks[?omkt,sendemail]`</p>

メッセージ本文で、クラス ノートブック作成パラメーターが含まれる JSON オブジェクトを送信します。 

```json
{
    "name": "notebook-name",
    "studentSections": [ 
        "section1-name", 
        "section2-name"
    ],
    "teachers": [
        {
            "id": "alias@tenant",
            "principalType": "Person-or-Group"
        }
    ],
    "students": [
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
   "hasTeacherOnlySectionGroup": true
}
```

| パラメーター | 説明 |  
|:------|:------|  
| name | ノートブックの名前。 |  
| studentSections | 1 つまたは複数のセクション名を含む配列。これらのセクションは、各生徒のセクション グループに作成されます。 |  
| 教師 | 1 つまたは複数のプリンシパル オブジェクトを含む配列。 |
| 生徒 | 1 つまたは複数のプリンシパル オブジェクトを含む配列。セクション グループは、生徒ごとに作成されます。 |    
| hasTeacherOnlySectionGroup | 教師のみに表示される*教師のみ* セクション グループを作成する場合は `true`。 | 
| omkt | ノートブックの[言語](#supported-langs)を指定する URL クエリ パラメーター。 既定値は `en-us` です。 例: `?omkt=es-es` | 
| sendemail | ノートブックに割り当てられている教師および生徒にノートブックが作成されるときに、電子メール通知を送信するかどうかを指定する URL クエリ パラメーター。既定値は `false` です。 |

<br />
教師と生徒は、以下のプロパティが含まれるプリンシパル オブジェクトで表されます。

| パラメーター | 説明 |  
|:------|:------|  
| id | Office 365 のユーザー プリンシパル名。<br /><br />ユーザーとグループの詳細については、[Azure AD Graph API の資料](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog)を参照してください。 |  
| principalType | `Person` または `Group` | 


<a name="supported-langs"></a>
### サポートされている言語

`omkt={language-code}` URL クエリ パラメーターを使用して、クラス ノートブックを特定の言語で作成できます。例:

<p id="indent">`POST ../classNotebooks?omkt=de-de`</p>

以下の言語コードがサポートされています。既定値は `en-us` です。

| Code | 言語 | 
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

次の要求は、*Math 101* という名前のクラス ノートブックを作成します。

```
POST ../v1.0/users/{teacher-id}/notes/classNotebooks?sendemail=true
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "name": "Math 101",
    "studentSections": [
        "Handouts",
        "Class Notes",
        "Homework",
        "Quizzes"
    ],
    "teachers": [
        {
            "id": "teacher1@contoso.com",
            "principalType": "Person"
        }
    ],
    "students": [
        {
            "id": "student1@contoso.com",
            "principalType": "Person"
        },
        {
            "id": "student2@contoso.com",
            "principalType": "Person" 
        },
        {
            "id": "student3@contoso.com",
            "principalType": "Person"
        },
        {
            "id": "student4@contoso.com",
            "principalType": "Person"
        }
    ],
    "hasTeacherOnlySectionGroup": true
}
```

4 つの生徒のセクション グループが含まれるクラス ノートブックを作成し、それぞれのグループに配布資料、授業のノート、宿題、クイズのセクションが含まれます。生徒ごとに作成されたセクション グループには、生徒と教師のみがアクセス可能です。またこれは、教師のみに表示される*教師のみ* セクション グループも作成します。`sendemail=true` クエリ パラメーターは、ノートブックが作成されると教師と生徒に電子メール通知を送信するように指定します。


### 要求および応答に関する情報

次の情報は、`POST /classNotebooks` 要求に当てはまります。

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
| 応答本文 | JSON 形式の新しいノートブックの OData 表記。<br /><br />[通常のノートブックのプロパティ](http://dev.onenote.com/docs#/reference/get-notebooks)に加え、クラス ノートブックには、次のプロパティが含まれます:<ul><li>**studentSections**。ノートブックの生徒セクション。</li><li>**teachers** ノートブックにアクセスできる教師。</li><li>**students** ノートブックにアクセスできる生徒。</li><li>**hasTeacherOnlySectionGroup**. ノートブックに *教師のみ*セクション グループが含まれている場合は `true`、含まれていない場合は `false`。</li></ul> |  
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |    
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="update"></a>
## クラス ノートブックの更新

クラス ノートブックを更新するには、PATCH 要求を *classNotebooks/{notebook-id}* エンドポイントに送信します。 

>現時点では、**hasTeacherOnlySectionGroup** プロパティのみ PATCH 要求で更新することができます。 

<p id="indent">`PATCH ../classNotebooks/{notebook-id}`</p>

メッセージ本文で、更新パラメーターが含まれる JSON オブジェクトを送信します。

```json
{
    "hasTeacherOnlySectionGroup": true
}
```

| パラメーター | 説明 |  
|:------|:------|  
| hasTeacherOnlySectionGroup | 教師のみに表示される*教師のみ* セクション グループを追加する場合は `true`。 `false` はサポートされていません。 | 

クラス ノートブックを変更する他の方法について、次の方法をご覧ください。「[生徒または教師の追加](#add-people)」、「[生徒または教師の削除](#remove-people)」、「[セクションの挿入](#insert-sections)」。

### 例

次の要求によって、*教師のみ*セクション グループを指定のクラス ノートブックに追加します。

```
PATCH ../v1.0/users/{teacher-id}/notes/classNotebooks/{notebook-id}
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "hasTeacherOnlySectionGroup": true
}
```

新しい*教師のみ*セクション グループは、教師のみに表示されます。


### 要求および応答に関する情報

次の情報は、`PATCH ../classNotebooks/{notebook-id}` 要求に当てはまります。

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
## クラス ノートブックの取得

1 つまたは複数のクラス ノートブックを取得するには、GET 要求を *classNotebooks* エンドポイントに送信します。

<p id="outdent"><b>1 つまたは複数のクラス ノートブックの取得</b></p>
<p id="indent">`GET ../classNotebooks[?filter,orderby,select,top,skip,expand,count]`</p>

<p id="outdent"><b>特定のクラス ノートブックの取得</b></p>
<p id="indent">`GET ../classNotebooks/{notebook-id}[?select,expand]`</p>

<br />ノートブックは、`teachers` プロパティと `students` プロパティを展開できます。既定の並べ替え順序は `name asc`です。

クラス ノートブックは `GET /notebooks` 要求に対しても戻されますが、結果にはクラス ノートブックに固有のプロパティは含まれません。


### 例

次の要求では、2016 年 1 月 1 日以降に作成されたクラス ノートブックを取得します。

```
GET ../v1.0/users/{teacher-id}/notes/classNotebooks?filter=createdTime%20ge%202016-01-01 
Authorization: Bearer {token}
Accept: application/json
``` 

サポートされているクエリ文字列オプションや例などを含むノートブックの取得について詳しくは、[OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)を参照してください。

### 要求および応答に関する情報

次の情報は、`GET /classNotebooks` 要求に当てはまります。

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。[Azure AD を使用した認証 (エンタープライズ アプリ)](..\howto\onenote-auth.md#aad-auth)を参照してください。</p> |  
| Accept ヘッダー | `application/json` | 
| [アクセス許可の適用範囲](../howto/onenote-auth.md#onenote-perms-aad) | Notes.Read、Notes.ReadWrite.CreatedByApp、Notes.ReadWrite、または Notes.ReadWrite.All | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 200 HTTP ステータス コード。 |  
| 応答本文 | JSON 形式のクラス ノートブックの OData 表記。<br /><br />[通常のノートブック プロパティ](http://dev.onenote.com/docs#/reference/get-notebooks)に加え、クラス ノートブックには次のプロパティが含まれます:<ul><li>**studentSections**。ノートブックの生徒セクション。</li><li>**teachers**。ノートブックにアクセスできる教師。</li><li>**student**。ノートブックにアクセスできる生徒。</li><li>**hasTeacherOnlySectionGroup**。ノートブックに*教師のみ* セクション グループが含まれる場合は `true`、それ以外の場合は `false`。</li></ul> |  
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |   
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |   


<a name="delete"></a>
## クラス ノートブックの削除

クラスのノートブックを削除するには、DELETE 要求を *classNotebooks/{notebook-id}* エンドポイントに送信します。

<p id="indent">`DELETE ../classNotebooks/{notebook-id}`</p>

### 例

次のような要求は、指定したクラスのノートブックを削除します。

```
DELETE ../v1.0/users/{teacher-id}/notes/classNotebooks/{notebook-id} 
Authorization: Bearer {token}
Accept: application/json
``` 

### 要求および応答に関する情報

次の情報は、`DELETE ../classNotebooks/{notebook-id}` 要求に当てはまります。

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
## 生徒と教師の追加

先生と生徒を追加すると、彼らにクラス ノートブックへのアクセス権限が付与されます。また生徒を追加すると、生徒用のセクション グループも作成されます。このセクション グループは、生徒と教師のみがアクセス可能で、ノートブックに対して定義されている生徒用のセクションが含まれています。

クラス ノートブックに生徒または教師を追加するには、適切なエンドポイントに POST 要求を送信します。

<p id="outdent"><b>生徒の追加</b></p>
<p id="indent">`POST ../classNotebooks/{notebook-id}/students`</p>

<p id="outdent"><b>教師の追加</b></p>
<p id="indent">`POST ../classNotebooks/{notebook-id}/teachers`</p>

<br />JSON のプリンシパル オブジェクトを、メッセージの本文に送信します。1 つの要求で追加できるのは生徒 1 人または教師 1 人です。 

```json
{
    "id": "alias@tenant",
    "principalType": "Person-or-Group"
}
```

教師と生徒は、以下のプロパティが含まれるプリンシパル オブジェクトで表されます。

| パラメーター | 説明 |  
|:------|:------|  
| id | Office 365 のユーザー プリンシパル名。ユーザーとグループの詳細については、[Azure AD Graph API の資料](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog) を参照してください。 |  
| principalType | `Person` または `Group` |  


### 例

次の要求は、指定したクラスのノートブックに教師を追加します。

```
POST ../v1.0/users/{teacher-id}/notes/classNotebooks/{notebook-id}/teachers 
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json

{
    "id": "teacher2@contoso.com",
    "principalType": "Person"
}
``` 

### 要求および応答に関する情報

次の情報は、`POST /students` および `POST /teachers` の要求に当てはまります。

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
| 応答本文 | 生徒または教師が追加されました。 |
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |


<a name="remove-people"></a>
## 生徒と教師の削除

生徒と教師をクラス ノートブックから削除すると、ノートブックへのアクセスが取り消されますが、コンテンツは削除されません。

生徒または教師をクラス ノートブックから削除するには、適切なエンドポイントに DELETE 要求を送信します。

<p id="outdent"><b>生徒の削除</b></p>
<p id="indent">`DELETE ../classNotebooks/{notebook-id}/students/{student-id}`</p>

<p id="outdent"><b>教師の削除</b></p>
<p id="indent">`DELETE ../classNotebooks/{notebook-id}/teachers/{teacher-id}`</p>

<br />
1 つの要求で削除できるのは生徒 1 人または教師 1 人です。


### 例

次の要求は、指定したクラス ノートブックから指定した生徒を削除します。

```
DELETE ../v1.0/users/{teacher-id}/notes/classNotebooks/{notebook-id}/students/{student-id} 
Authorization: Bearer {token}
Accept: application/json
``` 

### 要求および応答に関する情報

次の情報は、`DELETE /students` および `DELETE /teachers` の要求に当てはまります。

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

Office 365 のノートブックからの特定のセクションをコピーし、それらをクラス ノートブックのコンテンツ ライブラリに挿入するには、*copySectionsToContentLibrary* を使用します。コンテンツ ライブラリとは、教師が読み取り/書き込みアクセス許可を持ち、生徒が読み取りアクセス許可を持つ、クラス ノートブック内のセクション グループです。

クラス ノートブックにセクションを挿入するには、POST 要求を対象のクラス ノートブックの *copySectionsToContentLibrary* エンドポイントに送信します。例:

<p id="indent">`POST ../classNotebooks/{notebook-id}/copySectionsToContentLibrary`</p>

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
| sectionIds | クラス ノートブックに挿入するセクションの ID を含む配列です。 |    

ターゲット セクションとノートブック (所有または共有) へのアクセス権限が必要です。すべてのターゲットは、同じテナント内にある必要があります。

### 例

次の要求は、指定したクラス ノートブックのコンテンツ ライブラリに 2 つのセクションを挿入します。

```
POST ../v1.0/me/notes/classNotebooks/{notebook-id}/copySectionsToContentLibrary
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

- [OneNote ノートブック クラス](https://www.onenote.com/classnotebook) (概要および機能)
- [スタッフ ノートブックの操作](../howto/onenote-staffnotebook.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://stackoverflow.com/questions/tagged/onenote-api+onenote) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)
