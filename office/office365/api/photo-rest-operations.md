---
ms.Toctitle: Outlook User Photo REST API reference
title: "Outlook ユーザー写真 REST API リファレンス" 
description: "ms.TocTitle: Outlook ユーザー写真 REST API リファレンス (プレビュー)Title: Outlook ユーザー写真 REST API リファレンス (プレビュー)Description: Office 365 のユーザー写真 REST API を使用して組織内のユーザーの写真を取得する方法や写真のメタデータ (サイズと種類など) を取得する方法のリファレンスです。ms.ContentId: 1bc24940-25ef-43fa-8602-be6df30439f7 ms.topic: リファレンス (API)"  
ms.ContentId: 1bc24940-25ef-43fa-8602-be6df30439f7
ms.date: July 12, 2016

---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Outlook ユーザー写真 REST API リファレンス  


[!INCLUDE [Add the Outlook REST API filters--v2 default v1 disabled](../includes/controls/addOutlookversion_v2default_v1disabled.xml)]


 _**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_


<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->


[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<p class="previewnote"><p class="previewnote">このドキュメントは、プレビュー版に含まれるユーザー写真 API のベータ バージョンについて説明します Preview features are subject to change prior to finalization, and may break code that uses them. プレビュー機能は、最終処理までに変更される場合があります。それらを使用するコードを破棄する場合もあります。このため、一般に、実稼働コードでは実稼働バージョンの API のみを使用してください。可能な場合には、現在バージョン 2.0 が優先バージョンです。</p> If available, v2.0 is currently the preferred version.</p>

ユーザー写真 API を使用すると、Azure ActiveDirectory によってセキュリティ保護されているメールボックスを所有する Office 365 のユーザー、または Microsoft アカウント (具体的には、Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com のいずれかのドメイン) のユーザーの写真をダウンロードまたは設定できます。

**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

**Not interested in the beta version of the API?** API v2.0 が不要な場合右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

ユーザー写真 API を使用すると、Azure ActiveDirectory によってセキュリティ保護されているメールボックスを所有する Office 365 のユーザー、または Microsoft アカウント (具体的には、Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com のいずれかのドメイン) のユーザーの写真をダウンロードまたは設定できます。

**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。


**Not interested in v2.0 of the API?** API v2.0 が不要な場合右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


## ユーザー写真 REST API の使用

### 認証

Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the Outlook User Photo API, you should include a valid access token. アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you.
Keep this in mind as you proceed with the specific operations in the User Photo API.

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

ユーザー写真 API には、**photo** エンティティに加えて、ベータ バージョンでのみ使用可能なプレビュー版の **photos** コレクションもあります。photos コレクションを使用すると、関心のあるユーザー写真について具体的なサイズを指示できます。 The **photos** collection lets you indicate specific sizes of the user photo that you're interested in.

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<a name="GetPhotoMetadataOperations"> </a>
###写真のメタデータを取得する

要求したユーザー写真に関する情報 (コンテンツ タイプ、eTag、ピクセル単位の幅と高さなど) を取得します。

指定したユーザー (サインインしているユーザーであってもかまいません) の写真のメタデータを取得するには、次のいずれかのスコープを使用します。
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

|**省略可能なパラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|
|size|string| A photo size. The value of '1x1' is autogenerated in the case a photo is not present in both Active Directory and the mailbox. <br/> If the photo is stored in the mailbox, then the predefined sizes are: '48x48', '64x64', '96x96', '120x120', '240x240', '360x360','432x432', '504x504', and '648x648'. If the user doesn't upload a large enough photo, then only the sizes that can be represented by the smaller predefined sizes are available. For example, if the user uploads a photo that is 504x504 pixels, then all but the 648x648 size of photo will be available for download. <br/> Photos can by any dimension if they are stored in Active Directory.  

この要求は、サインインしているユーザーの 240x240 ピクセル画像のメタデータを取得します。

```
GET https://outlook.office.com/api/beta/me/photos('240x240')
```

**応答データのサンプル**

次の応答データは、写真のメタデータを示しています。HTTP 応答コードは 200 です。 次の応答データは、写真のメタデータを示しています。HTTP 応答コードは 200 です。
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


|**省略可能なパラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|
 

この要求は、サインインしているユーザーのユーザー写真のメタデータを取得します。

```
GET https://outlook.office.com/api/v2.0/me/photo
```

**応答データのサンプル**

次の応答データは、写真のメタデータを示しています。HTTP 応答コードは 200 です。 次の応答データは、写真のメタデータを示しています。HTTP 応答コードは 200 です。
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

This operation allows a tenant administrator to let an app get the user photo of any user in the tenant.

指定したユーザー (サインインしているユーザーであってもかまいません) の写真を取得するには、次のいずれかのスコープを使用します。
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

|**省略可能なパラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|user_id|string|ユーザーの電子メール アドレスです。|
|size|string| A photo size. The value of '1x1' is autogenerated in the case a photo is not present in both Active Directory and the mailbox. <br/> If the photo is stored in the mailbox, then the predefined sizes are: '48x48', '64x64', '96x96', '120x120', '240x240', '360x360','432x432', '504x504', and '648x648'. If the user doesn't upload a large enough photo, then only the sizes that can be represented by the smaller predefined sizes are available. For example, if the user uploads a photo that is 504x504 pixels, then all but the 648x648 size of photo will be available for download. <br/> Photos can by any dimension if they are stored in Active Directory.  


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


|**省略可能なパラメーター**|**型**|**説明**|
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

要求した写真のバイナリ データが含まれています。HTTP 応答コードは 200 です。 要求した写真のバイナリ データが含まれています。HTTP 応答コードは 200 です。

****


###ユーザー写真の設定


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Assign a photo to the specified user. The photo should be in binary. It replaces any existing photo for that user.

This operation allows a tenant administrator to let an app set the user photo of any user in the tenant. Use only PUT for this operation in the beta version.


**必須スコープ** 

Use the following scope to set the photo of the specified user, who can be any user in the tenant or the signed-in user:
- _user.readwrite.all_ 


サインインしていることが明確なユーザーの写真を取得する場合は、次のスコープを使用することもできます。
- _user.readwrite_

```no-highlight
PUT https://outlook.office.com/api/beta/me/photo/$value
PUT https://outlook.office.com/api/beta/users('{user_id}')/photo/$value
```

|**省略可能なパラメーター**|**型**|**説明**|
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

Assign a photo to the signed-in user. The photo should be in binary. It replaces any existing photo for that user. 

You can use either PATCH or PUT for this operation in version 2.0.


サインインしているユーザーの写真を設定する場合は、次のスコープを使用します。
- _user.readwrite_ 

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/photo/$value
PATCH https://outlook.office.com/api/v2.0/users('{user_id}')/photo/$value

PUT https://outlook.office.com/api/v2.0/me/photo/$value
PUT https://outlook.office.com/api/v2.0/users('{user_id}')/photo/$value
```

|**省略可能なパラメーター**|**型**|**説明**|
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
## 次のステップ

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。


- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](http://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)





[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]



        
      