---
ms.Toctitle: Outlook User Photo REST API reference
title: "Outlook ユーザー写真 REST API リファレンス" 
description: "Office 365 のユーザー写真 REST API を使用して組織内のユーザーの写真を取得する方法や写真のメタデータ (サイズと種類など) を取得する方法のリファレンスです。"  
ms.ContentId: 1bc24940-25ef-43fa-8602-be6df30439f7
ms.date: July 12, 2016

---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Outlook ユーザー写真 REST API リファレンス  


[!INCLUDE [Add the Outlook REST API filters--v2 default v1 disabled](../includes/controls/addOutlookversion_v2default_v1disabled.xml)]


 _**適用対象:**Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->


[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<p class="previewnote">このドキュメントは、プレビュー版に含まれるユーザー写真 API のベータ版について説明します プレビュー機能は、最終版までに変更される場合があり、それらの機能を使用するコードが動作しなくなる場合もあります。 このため、一般に、運用コードでは運用バージョンの API のみを使用してください。 入手可能な場合、現時点ではバージョン 2.0 が優先バージョンです。</p>

ユーザー写真 API を使用すると、Azure ActiveDirectory によってセキュリティ保護されているメールボックスを所有する Office 365 のユーザー、または Microsoft アカウント (具体的には、Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com のいずれかのドメイン) のユーザーの写真をダウンロードまたは設定できます。

**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

**ベータ版の API が不要な場合** 右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

ユーザー写真 API を使用すると、Azure ActiveDirectory によってセキュリティ保護されているメールボックスを所有する Office 365 のユーザー、または Microsoft アカウント (具体的には、Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com のいずれかのドメイン) のユーザーの写真をダウンロードまたは設定できます。

**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。


**API v2.0 が不要な場合** 右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


## ユーザー写真 REST API の使用

### 認証

その他の [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI) と同様に、Outlook ユーザー写真 API へのすべての要求には、有効なアクセス トークンを含める必要があります。 アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。 効率化された登録と承認のオプションに関する[詳細情報](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow)を参照してください。
ユーザー写真 API で特定の操作を続行する際には、この点に留意してください。

###API のバージョン

この API は、プレビューから一般提供 (GA) の状態にレベル上げされています。Outlook REST API の v2.0 とベータのバージョンでサポートされます。 

###対象ユーザー

対象ユーザーは、サインインしているユーザー、またはユーザー ID で指定されているユーザーになります。 

この API の使用方法の詳細について、および Outlook REST API のすべてのサブセットに共通する情報については、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。 

****

<a name="UserPhotoOperations"> </a>

## ユーザー写真の操作

ユーザー写真の操作を使用すると、ユーザーの写真のメタデータとフォト ストリームをバイナリ形式で取得したり、ユーザー写真を設定したりできます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

ユーザー写真 API には、**photo** エンティティに加えて、ベータ版でのみ使用可能なプレビューの **photos** コレクションもあります。 **photos** コレクションを使用すると、関心のあるユーザー写真について具体的なサイズを指示できます。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<a name="GetPhotoMetadataOperations"> </a>
### 写真のメタデータを取得する

要求したユーザー写真に関する情報 (コンテンツ タイプ、eTag、ピクセル単位の幅と高さなど) を取得します。

**必要な範囲** 指定したユーザー (サインインしているユーザーであってもかまいません) の写真のメタデータを取得するには、次のいずれかのスコープを使用します。
- _user.readbasic.all_
- _user.read.all_
- _user.readwrite.all_

サインインしていることが明確なユーザーの写真のメタデータを取得する場合は、次のスコープを使用することもできます。 
- _user.read_ 

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


**最大の使用可能な写真のメタデータを取得する**

```no-highlight
GET https://outlook.office.com/api/beta/me/photo
GET https://outlook.office.com/api/beta/Users('{user_id}')/photo
```

**すべてのダウンロード可能な写真サイズのメタデータを取得する**
```no-highlight
GET https://outlook.office.com/api/beta/me/photos
GET https://outlook.office.com/api/beta/Users('{user_id}')/photos
```

**特定の写真サイズのメタデータを取得する**
```no-highlight
GET https://outlook.office.com/api/beta/me/photos('{size}')
GET https://outlook.office.com/api/beta/Users('{user_id}')/photos('{size}')
```

|**省略可能なパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|
|size|string| 写真のサイズ。 値 '1 x 1' は、Active Directory とメールボックスのどちらにも写真が存在しない場合に自動的に生成されます。 <br/> 写真がメールボックスに格納されている場合の事前定義済みサイズは、'48x48'、'64x64'、'96x96'、'120x120'、'240x240'、'360x360'、'432x432'、'504x504'、'648x648' です。 ユーザーがアップロードした写真が大きくない場合は、それよりも小さい事前定義済みサイズで表現できるサイズのみがダウンロード可能になります。 たとえば、アップロードした写真が 504x504 ピクセルの場合は、648×648 を除くすべてのサイズの写真がダウンロード可能になります。 <br/> 写真が Active Directory に格納されている場合は、サイズに関する制限はありません。  

**要求のサンプル** この要求は、サインインしているユーザーの 240x240 ピクセル画像のメタデータを取得します。

```
GET https://outlook.office.com/api/beta/me/photos('240x240')
```

**応答データのサンプル**

次の応答データは、写真のメタデータを示しています。 HTTP 応答コードは 200 です。
```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/photo/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/photo",
    "@odata.mediaContentType": "image/jpeg",
    "@odata.mediaEtag": "\"BA09D118\"",
    "Id": "240X240",
    "Width": 240,
    "Height": 240
}
```
次の応答データは、そのユーザーの写真がまだアップロードされていないときの応答の内容を示しています。HTTP 応答コードは 200 です。
```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/photo/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/photo",
    "@odata.mediaContentType": "image/gif",
    "@odata.mediaEtag": "",
    "Id": "1X1",
    "Width": 1,
    "Height": 1
}
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


**最大の使用可能な写真のメタデータを取得する**

```no-highlight
GET https://outlook.office.com/api/v2.0/me/photo
GET https://outlook.office.com/api/v2.0/Users('{user_id}')/photo
```


|**省略可能なパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|
 

**要求のサンプル** この要求は、サインインしているユーザーのユーザー写真のメタデータを取得します。

```
GET https://outlook.office.com/api/v2.0/me/photo
```

**応答データのサンプル**

次の応答データは、写真のメタデータを示しています。 HTTP 応答コードは 200 です。
```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/photo/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/photo",
    "@odata.mediaContentType": "image/jpeg",
    "@odata.mediaEtag": "\"BA09D118\"",
    "Id": "240X240",
    "Width": 240,
    "Height": 240
}
```
次の応答データは、そのユーザーの写真がまだアップロードされていないときの応答の内容を示しています。HTTP 応答コードは 200 です。
```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/photo/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-7d04-b48b-20075df800e5@1717622f-1d94-c0d4-9d74-f907ad6677b4')/photo",
    "@odata.mediaContentType": "image/gif",
    "@odata.mediaEtag": "",
    "Id": "1X1",
    "Width": 1,
    "Height": 1
}
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




****

<a name="GetPhotoOperations"> </a>
### 写真を取得する

指定したユーザーのユーザー写真を取得します。

この操作でテナント管理者は、テナントのユーザーのユーザー写真をアプリに取得させることができます。

**必要な範囲** 指定したユーザー (サインインしているユーザーであってもかまいません) の写真を取得するには、次のいずれかのスコープを使用します。
- _user.readbasic.all_
- _user.read.all_
- _user.readwrite.all_

サインインしていることが明確なユーザーの写真を取得する場合は、次のスコープを使用することもできます。 
- _user.read_ 
- _user.readwrite_



<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


**最大の使用可能なサイズを取得する**

```no-highlight
GET https://outlook.office.com/api/beta/me/photo/$value
GET https://outlook.office.com/api/beta/Users('{user_id}')/photo/$value
```

**指定したサイズの写真を取得する**
```no-highlight
GET https://outlook.office.com/api/beta/me/photos('{size}')/$value
GET https://outlook.office.com/api/beta/Users('{user_id}')/photos('{size}')/$value
```

|**省略可能なパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|
|size|string| 写真のサイズ。 値 '1 x 1' は、Active Directory とメールボックスのどちらにも写真が存在しない場合に自動的に生成されます。 <br/> 写真がメールボックスに格納されている場合の事前定義済みサイズは、'48x48'、'64x64'、'96x96'、'120x120'、'240x240'、'360x360'、'432x432'、'504x504'、'648x648' です。 ユーザーがアップロードした写真が大きくない場合は、それよりも小さい事前定義済みサイズで表現できるサイズのみがダウンロード可能になります。 たとえば、アップロードした写真が 504x504 ピクセルの場合は、648×648 を除くすべてのサイズの写真がダウンロード可能になります。 <br/> 写真が Active Directory に格納されている場合は、サイズに関する制限はありません。  


**要求のサンプル**

この要求は、サインインしているユーザーの写真を取得します。

```
GET https://outlook.office.com/api/beta/me/photo/$value
Content-Type: image/jpg
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


**最大の使用可能なサイズを取得する**

```no-highlight
GET https://outlook.office.com/api/v2.0/me/photo/$value
GET https://outlook.office.com/api/v2.0/Users('{user_id}')/photo/$value
```


|**省略可能なパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|


**要求のサンプル**

この要求は、サインインしているユーザーの写真を取得します。

```
GET https://outlook.office.com/api/v2.0/me/photo/$value
Content-Type: image/jpg
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



**応答データ**

要求した写真のバイナリ データが含まれています。 HTTP 応答コードは 200 です。

****


###ユーザー写真の設定


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

指定されたユーザーに写真を割り当てます。 写真はバイナリ形式にする必要があります。 該当するユーザーの既存の写真と置き換えられます。

この操作でテナント管理者は、テナントのユーザーのユーザー写真をアプリに設定させることができます。 ベータ版では、この操作に PUT のみを使用します。


**必須スコープ** 

指定したユーザー (テナントのユーザーまたはサインインしているユーザーであってもかまいません) の写真を設定するには、次のスコープを使用します。
- _user.readwrite.all_ 


サインインしていることが明確なユーザーの写真を設定する場合は、次のスコープを使用することもできます。
- _user.readwrite_

```no-highlight
PUT https://outlook.office.com/api/beta/me/photo/$value
PUT https://outlook.office.com/api/beta/users('{user_id}')/photo/$value
```

|**省略可能なパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|


**要求のサンプル**

```
PUT https://outlook.office.com/api/beta/me/photo/$value
Content-Type: image/jpeg
```

要求の本文に、写真のバイナリ データを含めます。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

サインインしているユーザーに写真を割り当てます。 写真はバイナリ形式にする必要があります。 該当するユーザーの既存の写真と置き換えられます。 

バージョン 2.0 では、この操作に PATCH と PUT のいずれかを使用できます。


**必要な範囲** サインインしているユーザーの写真を設定する場合は、次のスコープを使用します。
- _user.readwrite_ 

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/photo/$value
PATCH https://outlook.office.com/api/v2.0/users('{user_id}')/photo/$value

PUT https://outlook.office.com/api/v2.0/me/photo/$value
PUT https://outlook.office.com/api/v2.0/users('{user_id}')/photo/$value
```

|**省略可能なパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/v2.0/me/photo/$value
Content-Type: image/jpeg
```

要求の本文に、写真のバイナリ データを含めます。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


**応答データ**

成功した要求は、HTTP 200 を返します。

****



<a name="NextSteps"> </a>
## 次の手順

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。


- [メール、予定表、および連絡先 REST API の使用を開始します](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](http://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)





[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]



        
      