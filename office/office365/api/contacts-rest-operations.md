---
ms.TocTitle: Outlook Contacts REST API reference
title: "Outlook の連絡先 REST API リファレンス"
Description: "Exchange Online でユーザーの連絡先や連絡先フォルダーへのアクセスを提供する、Office 365 Contacts REST API とクライアント ライブラリ API を操作する方法のリファレンスです。"
ms.ContentId: dcfb466f-0faf-4015-971f-aab4f8fc9c07
ms.date: July 20, 2016

---


[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Outlook の連絡先 REST API リファレンス

[!INCLUDE [Add the Outlook REST API filters--v2 default](../includes/controls/addOutlookVersion_v2default.xml)]


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<p class="previewnote">このドキュメントでは、プレビューに表示される連絡先と連絡先フォルダーの同期のベータ バージョンについて説明します。 プレビュー機能は、最終版までに変更される場合があり、それらの機能を使用するコードが動作しなくなる場合もあります。 このため、一般に、運用コードでは運用バージョンの API のみを使用してください。 入手可能な場合、現時点ではバージョン 2.0 が優先バージョンです。</p>

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

  
 _**適用対象:**Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_


<a name="Overview"></a>Outlook 連絡先 API は、Office 365 の Azure Active Directory によって保護されているユーザーの連絡先と連絡先フォルダーへのアクセス、および Microsoft アカウントの類似したデータへのアクセスを提供します。具体的には、次のドメインです。Hotmail.com、Live.com、MSN.com、Outlook.com、および Passport.com。


<!-- Can add the following sentence back once the client libraries have been updated for converged auth and outlook.com
You can access the Contacts API by calling the corresponding REST APIs directly in your apps, or by using the Office 365 client libraries and SDKs.
-->

**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**ベータ版の API が不要な場合** 右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**API v2.0 が不要な場合** 右上のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

**API v1.0 が不要な場合** 右上のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




##連絡先 API のすべての操作

<a name="ContactOperations"></a>
**連絡先の操作** 連絡先は連絡先フォルダーに保存されます。 連絡先を取得、作成、変更、および削除することができます。

[連絡先を取得する](#GetContacts) | [連絡先と連絡先フォルダーを同期する](#SyncContacts) (プレビュー) | [連絡先を作成する](#CreateContacts) | [連絡先を更新する](#UpdateContacts) | [連絡先を削除する](#DeleteContacts)

<a name="ContactFoldersOperations"></a>
**連絡先フォルダーの操作** 連絡先フォルダーには連絡先や別の連絡先フォルダーを含めることができます。
連絡先フォルダーを取得し、連絡先フォルダーに連絡先を作成できます。

[連絡先フォルダーの取得](#GetContactFolders)

**連絡先の写真の操作** それぞれの連絡先に、オプションで連絡先の写真を指定できます。 連絡先の写真を取得また設定することができます。

[連絡先の写真を取得する](#GetContactPhotoandMetadata) | [連絡先の写真を設定する](#SetContactPhoto)

関連項目:

[REST API 連絡先リソース](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource) |
[REST API 連絡先フォルダーのリソース](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)


## 連絡先 REST API の使用

###認証

他の [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI) と同様に、連絡先 API へのすべての要求に対して、有効なアクセス トークンを含める必要があります。 アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。 効率化された登録と承認のオプションに関する[詳細情報](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow)を参照してください。
連絡先 API で特定の操作を続行する際には、この点に留意してください。

<a name="SupportedVersions"> </a>

###API のバージョン

連絡先 REST API は、すべてのバージョンの Outlook REST API でサポートされています。機能は、特定のバージョンによって異なる場合があります。

###対象ユーザー

連絡先 API 要求は、常に現在のユーザーのために実行されます。 

Outlook REST API のすべてのサブセットに共通な情報の詳細については、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。

****

<a name="GetContacts"> </a>
##連絡先を取得する

連絡先フォルダーから、連絡先のコレクションまたは個々の連絡先を取得できます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

<a name="GetContactCollection"> </a>
###連絡先のコレクションを取得する

サインインしているユーザーのメールボックス内のすべての連絡先を取得する (`.../me/contacts`) か、指定された連絡先フォルダーから取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/contacts
GET https://outlook.office.com/api/beta/me/contactfolders/{contact_folder_id}/contacts
```


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定のフォルダーから連絡先を取得している場合は、連絡先フォルダー ID です。|

**注** 既定では、応答内の各連絡先にそのプロパティがすべて含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。 **Id** プロパティは常に返されます。 パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/contacts?$select=EmailAddresses,GivenName,Surname
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Contacts(EmailAddresses,GivenName,Surname)",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGI2THk3AAA=')",
            "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa7\"",
            "Id": "AAMkAGI2THk3AAA=",
            "GivenName": "Rob",
            "Surname": "Young",
            "EmailAddresses": [
                {
                    "Name": "roby@a830edad9050849NDA1.onmicrosoft.com",
                    "Address": "roby@a830edad9050849NDA1.onmicrosoft.com"
                }
            ]
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGI2THkzAAA=')",
            "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa3\"",
            "Id": "AAMkAGI2THkzAAA=",
            "GivenName": "Janet",
            "Surname": "Schorr",
            "EmailAddresses": [
                {
                    "Name": "janets@a830edad9050849NDA1.onmicrosoft.com",
                    "Address": "janets@a830edad9050849NDA1.onmicrosoft.com"
                }
            ]
        }
    ]
}
```


**応答の種類**

要求された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)のコレクションです。

*****

<a name="GetContact"> </a>
###連絡先を取得する

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_


連絡先 ID を使用して連絡先を取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/contacts/{contact_id}
```


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先 ID。|

 **応答の種類**

要求された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。


**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/contacts/AAMkADlkAAAMRFUEAAA=
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context":"https://outlook.office365.com/api/beta/$metadata#Me/Contacts/$entity",
    "@odata.id":"https://outlook.office365.com/api/beta/Users('af183ae6-7efa-41e4-aa87-fe8790598625@9ac5b33f-49cf-45f7-9ef1-b581dce364d8')/Contacts('AAMkADlkAAAMRFUEAAA=')",
    "@odata.etag":"W/\"EQAAABYAAADDii8zlkFETIcBiRn8yAbgAAAMRdhl\"",
    "Id":"AAMkADlkAAAMRFUEAAA=",
    "CreatedDateTime":"2016-07-16T06:43:15Z",
    "LastModifiedDateTime":"2016-07-16T06:43:15Z",
    "ChangeKey":"EQAAABYAAADDii8zlkFETIcBiRn8yAbgAAAMRdhl",
    "Categories":[
    ],
    "ParentFolderId":"AAMkADlk8yAbgAAAAAAEkAAA=",
    "Birthday":null,
    "FileAs":"",
    "DisplayName":"Garret Vargas",
    "GivenName":"Garret",
    "Initials":null,
    "MiddleName":null,
    "NickName":null,
    "Surname":"Vargas",
    "Title":null,
    "YomiGivenName":null,
    "YomiSurname":null,
    "YomiCompanyName":null,
    "Generation":null,
    "EmailAddresses":[
        {
            "Name":"Garret Vargas",
            "Address":"GarretV@contoso.onmicrosoft.com"
        }
    ],
    "Websites":[
    ],
    "ImAddresses":[
        "sip:garretv@contoso.onmicrosoft.com"
    ],
    "JobTitle":"CVP Operations",
    "CompanyName":"",
    "Department":"Operations",
    "OfficeLocation":"36/2121",
    "Profession":null,
    "AssistantName":null,
    "Manager":null,
    "Phones":[
        {
            "Type":"Home",
            "Number":""
        },
        {
            "Type":"Business",
            "Number":"+1 206 555 0105"
        },
        {
            "Type":"Mobile",
            "Number":""
        }
    ],
    "PostalAddresses":[
        {
            "Type":"Business",
            "City":"Seattle"
        }
    ],
    "SpouseName":null,
    "PersonalNotes":null,
    "Children":[
    ],
    "Gender":null,
    "IsFavorite":null,
    "Flag":{
        "FlagStatus":"NotFlagged"
    }
}
```



**注** 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。 **Id** プロパティは常に返されます。 パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内の連絡先の**EmailAddresses**、**GivenName**、および **Surname** プロパティのみを返すように指定する方法を示しています。


**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/contacts/AAMkAGI2THk0AAA=?$select=EmailAddresses,GivenName,Surname
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Contacts(EmailAddresses,GivenName,Surname)/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('af183ae6-7efa-41e4-aa87-fe8790598625@9ac5b33f-49cf-45f7-9ef1-b581dce364d8')/Contacts('AAMkADlkAAAMRFUEAAA=')",
    "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa4\"",
    "Id": "AAMkADlkAAAMRFUEAAA=",
    "GivenName": "Garth",
    "Surname": "Vargas",
    "EmailAddresses": [
       {
            "Name":"Garret Vargas",
            "Address":"GarretV@contoso.onmicrosoft.com"
        }
    ]
}
```


****

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

<a name="GetContactCollection"> </a>
###連絡先のコレクションを取得する

サインイン中のユーザーの既定の連絡先フォルダーから連絡先のコレクションを取得する (`.../me/contacts`) か、指定した連絡先フォルダーから取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/contacts
GET https://outlook.office.com/api/v2.0/me/contactfolders/{contact_folder_id}/contacts
```


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定のフォルダーから連絡先を取得している場合は、連絡先フォルダー ID です。|

**注** 既定では、応答内の各連絡先にそのプロパティがすべて含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。 **Id** プロパティは常に返されます。 パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/contacts?$select=EmailAddresses,GivenName,Surname
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Contacts(EmailAddresses,GivenName,Surname)",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGI2THk3AAA=')",
            "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa7\"",
            "Id": "AAMkAGI2THk3AAA=",
            "GivenName": "Rob",
            "Surname": "Young",
            "EmailAddresses": [
                {
                    "Name": "roby@a830edad9050849NDA1.onmicrosoft.com",
                    "Address": "roby@a830edad9050849NDA1.onmicrosoft.com"
                }
            ]
        },
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGI2THkzAAA=')",
            "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa3\"",
            "Id": "AAMkAGI2THkzAAA=",
            "GivenName": "Janet",
            "Surname": "Schorr",
            "EmailAddresses": [
                {
                    "Name": "janets@a830edad9050849NDA1.onmicrosoft.com",
                    "Address": "janets@a830edad9050849NDA1.onmicrosoft.com"
                }
            ]
        }
    ]
}
```


**応答の種類**

要求された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)のコレクションです。

****

<a name="GetContact"> </a>
###連絡先を取得する

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_


連絡先 ID を使用して連絡先を取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/contacts/{contact_id}
```


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先 ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/contacts/AAMkAGI2THk0AAA=
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Contacts/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGI2THk0AAA=')",
    "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa4\"",
    "Id": "AAMkAGI2THk0AAA=",
    "CreatedDateTime": "2014-10-19T23:08:24Z",
    "LastModifiedDateTime": "2014-10-19T23:08:24Z",
    "ChangeKey": "EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa4",
    "Categories": [],
    "ParentFolderId": "AAMkAGI2AAEOAAA=",
    "Birthday": null,
    "FileAs": "Fort, Garth",
    "DisplayName": "Garth Fort",
    "GivenName": "Garth",
    "Initials": "G.F.",
    "MiddleName": null,
    "NickName": "Garth",
    "Surname": "Fort",
    "Title": null,
    "YomiGivenName": null,
    "YomiSurname": null,
    "YomiCompanyName": null,
    "Generation": null,
    "EmailAddresses": [
        {
            "Name": "Garth",
            "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com"
        }
    ],
    "ImAddresses": [
        "sip:garthf@a830edad9050849nda1.onmicrosoft.com"
    ],
    "JobTitle": "Web Marketing Manager",
    "CompanyName": "Contoso, Inc.",
    "Department": "Sales & Marketing",
    "OfficeLocation": "20/1101",
    "Profession": null,
    "BusinessHomePage": "http://www.contoso.com",
    "AssistantName": null,
    "Manager": null,
    "HomePhones": [],
    "MobilePhone1": null,
    "BusinessPhones": [
        "+1 918 555 0101"
    ],
    "HomeAddress": {},
    "BusinessAddress": {
      "Street": "10 Contoso Way",
      "City": "Redmond",
      "State": "WA",
      "CountryOrRegion": "USA",
      "PostalCode": "98075"  
    },
    "OtherAddress": {},
    "SpouseName": null,
    "PersonalNotes": null,
    "Children": []
}
```


 **応答の種類**

要求された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。

**注** 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。 **Id** プロパティは常に返されます。 パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内の連絡先の**EmailAddresses**、**GivenName**、および **Surname** プロパティのみを返すように指定する方法を示しています。

**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/contacts/AAMkAGI2THk0AAA=?$select=EmailAddresses,GivenName,Surname
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Contacts(EmailAddresses,GivenName,Surname)/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGI2THk0AAA=')",
    "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa4\"",
    "Id": "AAMkAGI2THk0AAA=",
    "GivenName": "Garth",
    "Surname": "Fort",
    "EmailAddresses": [
        {
            "Name": "garthf@a830edad9050849NDA1.onmicrosoft.com",
            "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com"
        }
    ]
}
```

****

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->

<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]


REST API:[連絡先のコレクションを取得する (REST)](#GetContactCollection) | [連絡先を取得する (REST)](#GetContact)

クライアント ライブラリ:[既定の連絡先フォルダーから連絡先を取得する (クライアント)](#GetContactsClient)

<a name="GetContactCollection"> </a>
###連絡先のコレクションを取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

サインイン中のユーザーの既定の連絡先フォルダーから連絡先のコレクションを取得する (`.../me/contacts`) か、指定した連絡先フォルダーから取得します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/contacts
GET https://outlook.office.com/api/v1.0/me/contactfolders/{contact_folder_id}/contacts
```


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定のフォルダーから連絡先を取得している場合は、連絡先フォルダー ID です。|

**注** 既定では、応答内の各連絡先にそのプロパティがすべて含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。 **Id** プロパティは常に返されます。 パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内の各連絡先の **EmailAddresses**、**GivenName**、および **Surname** プロパティのみを返すように指定する方法を示しています。**$select** を使用しない場合の連絡先に返されるプロパティの完全な一覧については、「[連絡先を取得する (REST)](#GetContact)」の最初の応答サンプルを参照してください。

[!code-REST-i[contacts_api_get_contacts.json](./_data/contacts_api_get_contacts.json)]


 **応答の種類**

要求された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)のコレクションです。


****


<a name="GetContact"> </a>
###連絡先を取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_


連絡先 ID を使用して連絡先を取得します。

```no-highlight
GET https://outlook.office.com/api/{version}/me/contacts/{contact_id}
```


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|バージョン|文字列|API の[バージョン](#SupportedVersions)。|
|contact_id|string|連絡先の ID。|


[!code-REST-i[contacts_api_contact_get_contact_by_id](./_data/contacts_api_contact_get_contact_by_id.json)]

 **応答の種類**

要求された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。

**注** 既定では、応答に連絡先のすべてのプロパティが含まれます。 最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、**$select** を使用します。 **Id** プロパティは常に返されます。 パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

次の例は、**$select** を使用して、応答内の連絡先の**EmailAddresses**、**GivenName**、および **Surname** プロパティのみを返すように指定する方法を示しています。

[!code-REST-i[contacts_api_contact_get_contact_by_id_select](./_data/contacts_api_contact_get_contact_by_id_select.json)]

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

****

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->



<a name="GetContactsClient"> </a>
###既定の連絡先フォルダーから連絡先を取得する (クライアント)

既定の連絡先フォルダーから連絡先を取得します。特定の連絡先を取得するには、**Contacts** コレクションのインデックスとして連絡先 ID を指定するか、**GetById** メソッドを使用します。

例: `outlookClient.Me.Contacts[contactId].ExecuteAsync()`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


**注** 連絡先コレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。

この例では、[Outlook サービス クライアントを作成する](#GetClient)メソッドを呼び出します。

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Contacts" -->

```cs-i
var outlookClient = await CreateOutlookClientAsync("Contacts");
var contacts= await outlookClient.Me.Contacts
  .OrderBy(c => c.DisplayName)
  .Take(10)
  .ExecuteAsync();

foreach(var contact in contacts.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine(contact.DisplayName);
  foreach(var emailAddress in contact.EmailAddresses)
  {
    if (emailAddress != null)
    {
      System.Diagnostics.Debug.WriteLine("*  {0}", emailAddress.Address);
    }
  }
}

```
```javascript-i
outlookClient.me.contacts.getContacts().orderBy('DisplayName asc').fetch().then(
function (result) {
    result.currentPage.forEach(function (contact) {
        console.log(contact.displayName);
contact.emailAddresses.forEach(function(emailAddress) {
console.log('*  ' + emailAddress.address);
});
    });
}, function(error) {
    console.log(error);
}
);
```

<!-- ENDSECTION -->


****

<a name="SyncContacts"> </a>
##連絡先と連絡先フォルダーを同期する (プレビュー)

ローカルの連絡先の一覧とサーバーの連絡先を同期できます。 Contactssynchronization はフォルダーごとの操作であり、たとえばルート連絡先フォルダーのすべての連絡先を同期できます。 その他の連絡先フォルダーがある場合は、各フォルダーを個別に同期する必要があります。

同期は、完全同期のみサポートしています。各要求には、指定したフォルダー内のすべての連絡先が返されます。

通常、連絡先フォルダーを同期するには、2 つ以上の GET 要求が必要です。 GET 要求は[連絡先を取得する](#GetContacts)と同じ方法で実行できますが、次の要求ヘッダーを追加する必要があります。

- すべての同期要求で、_Prefer: odata.track-changes_ ヘッダーを指定する必要があります。
- _Prefer: odata.maxpages={n}_ ヘッダーを指定して、要求ごとに返される連絡先の最大数を指定することができます。
  
  2 番目以降の GET 要求は、前の応答で受信した _deltaToken_ または _skipToken_ のいずれかを含むため、最初の GET 要求とは異なります。
  
  同期要求に対する最初の応答では、常に _deltaToken_ が返されます。 追加の連絡先がある場合は、2 番目の GET 要求には常に _deltaToken_ を使用します。 2 番目の要求は、追加の連絡先、および他にも連絡先がある場合は _skipToken_、最後の連絡先が送信された場合は _deltaToken_ が返されます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

```no-highlight
GET https://outlook.office.com/api/beta/me/Contacts
GET https://outlook.office.com/api/beta/me/ContactFolders/{folderName}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_ヘッダー パラメーター_|
|優先|odata.track-changes|要求が同期要求であることを示します。|
|Prefer|odata.maxpagesize|各応答で返される連絡先の数を設定します。|
|_URL パラメーター_|
|folderName|文字列|同期するフォルダーの名前。|
|odata.deltaLink|String|前回フォルダーが同期されたことを示すトークン。|
|odata.skiptoken|String|ダウンロードするメッセージがまだあることを示すトークン。|

**応答の種類**

要求された連絡先と、サーバーからの連絡先データの追加ページを要求し、増分同期を要求するために使用する _deltaToken_ を含むコレクションです。 返された連絡先の数が _odata.maxpagesize_ ヘッダーで指定した値より多い場合、応答は複数のページで返されます。

応答には _Preference-Applied: odata.track-changes_ ヘッダーが含まれます。 サポートされていないリソースを同期しようとすると、応答でこのヘッダーが返されません。
エラーを回避するには、応答を処理する前にこのヘッダーを確認します。

**注** 既定では、応答に指定された連絡先のすべてのプロパティが含まれます。
最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、_$select_ を使用します。 **Id** プロパティは常に返されます。 $filter、$orderby、$search、または $top は使用しないでください。これらは連絡先または連絡先フォルダーの同期ではサポートされません。
詳細については、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

** 例 **

完全な同期の最初の要求です。

 ```no-highlight
GET https://outlook.office.com/api/beta/Me/Contacts
```

次のヘッダーが含まれています。
* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

完全な同期要求に続くサーバーへの 2 番目の要求です。

```no-highlight
https://outlook.office.com/api/beta/Me/Contacts/?%24deltatoken=169ca50467d34d9fb8adb664961b9836
```
次のヘッダーが含まれています。

* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

追加ページのある、サーバーからの 2 番目の応答です。

ヘッダー

Preference-Applied: odata.track-changes

本文

@odata.deltaLink=https://outlook.office.com/api/beta/me/Contacts/messages/?%24skiptoken=169ca50467d34d9fb8adb664961b9836

_ペイロード メッセージ_

すべての連絡先が送信された場合の、2 番目以降のサーバーからの応答です。

ヘッダー

Preference-Applied: odata.track-changes

本文

@odata.deltaLink=https://outlook.office.com/api/beta/me/Contacts/?%24deltatoken=169ca50467d34d9fb8adb664961b9836

_ペイロード メッセージ_

追加ページがある場合のサーバーへの要求です。

```no-highlight
https://outlook.office.com/api/beta/Me/Contacts/?%24skiptoken=169ca50467d34d9fb8adb664961b9836
```
次のヘッダーが含まれています。

* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

```no-highlight
GET https://outlook.office.com/api/v2.0/me/Contacts
GET https://outlook.office.com/api/v2.0/me/ContactFolders/{folderName}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_ヘッダー パラメーター_|
|優先|odata.track-changes|要求が同期要求であることを示します。|
|Prefer|odata.maxpagesize|各応答で返される連絡先の数を設定します。|
|_URL パラメーター_|
|folderName|文字列|同期するフォルダーの名前。|
|odata.deltaLink|String|前回フォルダーが同期されたことを示すトークン。|
|odata.skiptoken|String|ダウンロードするメッセージがまだあることを示すトークン。|

**応答の種類**

要求された連絡先と、サーバーからの連絡先データの追加ページを要求し、増分同期を要求するために使用する _deltaToken_ を含むコレクションです。 返された連絡先の数が _odata.maxpagesize_ ヘッダーで指定した値より多い場合、応答は複数のページで返されます。

応答には _Preference-Applied: odata.track-changes_ ヘッダーが含まれます。 サポートされていないリソースを同期しようとすると、応答でこのヘッダーが返されません。
エラーを回避するには、応答を処理する前にこのヘッダーを確認します。

**注** 既定では、応答に指定された連絡先のすべてのプロパティが含まれます。
最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、_$select_ を使用します。 **Id** プロパティは常に返されます。 $filter、$orderby、$search、または $top は使用しないでください。これらは連絡先または連絡先フォルダーの同期ではサポートされません。
詳細については、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

** 例 **

完全な同期の最初の要求です。

 ```no-highlight
GET https://outlook.office.com/api/v2.0/Me/Contacts
```

次のヘッダーが含まれています。
* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

完全な同期要求に続くサーバーへの 2 番目の要求です。

```no-highlight
https://outlook.office.com/api/v2.0/Me/Contacts/?%24deltatoken=169ca50467d34d9fb8adb664961b9836
```
次のヘッダーが含まれています。

* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

追加ページのある、サーバーからの 2 番目の応答です。

ヘッダー

Preference-Applied: odata.track-changes

本文

@odata.deltaLink=https://outlook.office.com/api/v2.0/me/Contacts/messages/?%24skiptoken=169ca50467d34d9fb8adb664961b9836

_ペイロード メッセージ_

すべての連絡先が送信された場合の、2 番目以降のサーバーからの応答です。

ヘッダー

Preference-Applied: odata.track-changes

本文

@odata.deltaLink=https://outlook.office.com/api/v2.0/me/Contacts/?%24deltatoken=169ca50467d34d9fb8adb664961b9836

_ペイロード メッセージ_

追加ページがある場合のサーバーへの要求です。

```no-highlight
https://outlook.office.com/api/v2.0/Me/Contacts/?%24skiptoken=169ca50467d34d9fb8adb664961b9836
```
次のヘッダーが含まれています。

* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

```no-highlight
GET https://outlook.office.com/api/v1.0/me/Contacts
GET https://outlook.office.com/api/v1.0/me/ContactFolders/{folderName}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_ヘッダー パラメーター_|
|優先|odata.track-changes|要求が同期要求であることを示します。|
|Prefer|odata.maxpagesize|各応答で返される連絡先の数を設定します。|
|_URL パラメーター_|
|folderName|文字列|同期するフォルダーの名前。|
|odata.deltaLink|String|前回フォルダーが同期されたことを示すトークン。|
|odata.skiptoken|String|ダウンロードするメッセージがまだあることを示すトークン。|

**応答の種類**

要求された連絡先と、サーバーからの連絡先データの追加ページを要求し、増分同期を要求するために使用する _deltaToken_ を含むコレクションです。 返された連絡先の数が _odata.maxpagesize_ ヘッダーで指定した値より多い場合、応答は複数のページで返されます。

応答には _Preference-Applied: odata.track-changes_ ヘッダーが含まれます。 サポートされていないリソースを同期しようとすると、応答でこのヘッダーが返されません。
エラーを回避するには、応答を処理する前にこのヘッダーを確認します。

**注** 既定では、応答に指定された連絡先のすべてのプロパティが含まれます。
最適なパフォーマンスを得るために必要なプロパティのみを指定する場合は、_$select_ を使用します。 **Id** プロパティは常に返されます。 $filter、$orderby、$search、または $top は使用しないでください。これらは連絡先または連絡先フォルダーの同期ではサポートされません。
詳細については、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。

** 例 **

完全な同期の最初の要求です。

 ```no-highlight
GET https://outlook.office.com/api/v1.0/Me/Contacts
```

次のヘッダーが含まれています。
* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

完全な同期要求に続くサーバーへの 2 番目の要求です。

```no-highlight
https://outlook.office.com/api/v1.0/Me/Contacts/?%24deltatoken=169ca50467d34d9fb8adb664961b9836
```
次のヘッダーが含まれています。

* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

追加ページのある、サーバーからの 2 番目の応答です。

ヘッダー

Preference-Applied: odata.track-changes

本文

@odata.deltaLink=https://outlook.office.com/api/v1.0/me/Contacts/messages/?%24skiptoken=169ca50467d34d9fb8adb664961b9836

_ペイロード メッセージ_

すべての連絡先が送信された場合の、2 番目以降のサーバーからの応答です。

ヘッダー

Preference-Applied: odata.track-changes

本文

@odata.deltaLink=https://outlook.office.com/api/v1.0/me/Contacts/?%24deltatoken=169ca50467d34d9fb8adb664961b9836

_ペイロード メッセージ_

追加ページがある場合のサーバーへの要求です。

```no-highlight
https://outlook.office.com/api/v1.0/Me/Contacts/?%24skiptoken=169ca50467d34d9fb8adb664961b9836
```
次のヘッダーが含まれています。

* Prefer: odata.track-changes
* Prefer: odata.maxpagesize=100

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->
 
 ****

<a name="CreateContacts"> </a>
##連絡先を作成する

指定した連絡先フォルダーに連絡先を作成します。

REST API:[連絡先を作成する (REST)](#CreateAContact)

クライアント ライブラリ:[既定の連絡先フォルダーに連絡先を作成する (クライアント)](#CreateContactsClient)

<a name="CreateAContact"></a>
###連絡先を作成する (REST)

<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

連絡先をルート連絡先フォルダーまたは別の連絡先フォルダーの `contacts` エンドポイントに追加します。

```no-highlight
POST https://outlook.office.com/api/beta/me/contacts
POST https://outlook.office.com/api/beta/me/contactfolders/{contact_folder_id}/contacts
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定の連絡先フォルダーに連絡先を作成する場合は連絡先フォルダー ID です。|
|_本文パラメーター_|
|GivenName|string|連絡先の名前。|

要求本文に **GivenName** パラメーターと任意の書き込み可能な[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)プロパティを指定します。


**要求のサンプル**

```
POST https://outlook.office.com/api/beta/me/contacts
Content-Type: application/json

{
  "GivenName": "Pavel",
  "Surname": "Bansky",
  "EmailAddresses": [
    {
      "Address": "pavelb@contoso.onmicrosoft.com",
      "Name": "Pavel Bansky"
    }
  ],
  "Phones": [
    {
      "Type": "Business",
      "Number": "+1 732 555 0102"
    }
  ]
}
```

**応答のサンプル**

```
Status code: 201

{
  "@odata.context":"https://outlook.office365.com/api/beta/$metadata#Me/Contacts/$entity",
  "@odata.id":"https://outlook.office365.com/api/beta/Users('af183ae6-7efa-41e4-aa87-fe8790598625@9ac5b33f-49cf-45f7-9ef1-b581dce364d8')/Contacts('AAMkADlkAAARKMK7AAA=')",
  "@odata.etag":"W/\"EQAAABYAAADDii8zlkFETIcBiRn8yAbgAAARKpia\"",
  "Id":"AAMkADlkAAARKMK7AAA=",
  "CreatedDateTime":"2016-07-20T23:59:37Z",
  "LastModifiedDateTime":"2016-07-20T23:59:38Z",
  "ChangeKey":"EQAAABYAAADDii8zlkFETIcBiRn8yAbgAAARKpia",
  "Categories":[
  ],
  "ParentFolderId":"AAMkADlk8yAbgAAAAAAEOAAA=",
  "Birthday":null,
  "FileAs":"",
  "DisplayName":"Pavel Bansky",
  "GivenName":"Pavel",
  "Initials":null,
  "MiddleName":null,
  "NickName":null,
  "Surname":"Bansky",
  "Title":null,
  "YomiGivenName":null,
  "YomiSurname":null,
  "YomiCompanyName":null,
  "Generation":null,
  "EmailAddresses":[
    {
      "Name":"Pavel Bansky",
      "Address":"pavelb@contoso.onmicrosoft.com"
    }
  ],
  "Websites":[
  ],
  "ImAddresses":[
  ],
  "JobTitle":null,
  "CompanyName":null,
  "Department":null,
  "OfficeLocation":null,
  "Profession":null,
  "AssistantName":null,
  "Manager":null,
  "Phones":[
    {
      "Type":"Business",
      "Number":"+1 732 555 0102"
    }
  ],
  "PostalAddresses":[
  ],
  "SpouseName":null,
  "PersonalNotes":null,
  "Children":[
  ],
  "Gender":null,
  "IsFavorite":null,
  "Flag":{
    "FlagStatus":"NotFlagged"
  }
}
```


 **応答の種類**

新しい[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

連絡先をルート連絡先フォルダーまたは別の連絡先フォルダーの `contacts` エンドポイントに追加します。

```no-highlight
POST https://outlook.office.com/api/v2.0/me/contacts
POST https://outlook.office.com/api/v2.0/me/contactfolders/{contact_folder_id}/contacts
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定の連絡先フォルダーに連絡先を作成する場合は連絡先フォルダー ID です。|
|_本文パラメーター_|
|GivenName|string|連絡先の名前。|

要求本文に **GivenName** パラメーターと任意の書き込み可能な[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)プロパティを指定します。


**要求のサンプル**

```
POST https://outlook.office.com/api/v2.0/me/contacts
Content-Type: application/json

{
  "GivenName": "Pavel",
  "Surname": "Bansky",
  "EmailAddresses": [
    {
      "Address": "pavelb@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Pavel Bansky"
    }
  ],
  "BusinessPhones": [
    "+1 732 555 0102"
  ]
}
```

**応答のサンプル**

```
Status code: 201

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Contacts/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGE0M4xqVAAA=')",
  "@odata.etag": "W/\"EQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAV41eC\"",
  "Id": "AAMkAGE0M4xqVAAA=",
  "ChangeKey": "EQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAV41eC",
  "Categories": [],
  "CreatedDateTime": "2014-10-22T20:38:18Z",
  "LastModifiedDateTime": "2014-10-22T20:38:19Z",
  "ParentFolderId": "AAMkAGE0MAAEOAAA=",
  "Birthday": null,
  "FileAs": "",
  "DisplayName": "Pavel Bansky",
  "GivenName": "Pavel",
  "Initials": null,
  "MiddleName": null,
  "NickName": null,
  "Surname": "Bansky",
  "Title": null,
  "Generation": null,
  "EmailAddresses": [
    {
      "Address": "pavelb@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "Pavel Bansky"
    },
    null,
    null
  ],
  "ImAddresses": [
    null,
    null,
    null
  ],
  "JobTitle": null,
  "CompanyName": null,
  "Department": null,
  "OfficeLocation": null,
  "Profession": null,
  "BusinessHomePage": null,
  "AssistantName": null,
  "Manager": null,
  "HomePhones": [
    null,
    null
  ],
  "BusinessPhones": [
    "+1 732 555 0102",
    null
  ],
  "MobilePhone1": null,
  "HomeAddress": {
    "Street": null,
    "City": null,
    "State": null,
    "CountryOrRegion": null,
    "PostalCode": null
  },
  "BusinessAddress": {
    "Street": null,
    "City": null,
    "State": null,
    "CountryOrRegion": null,
    "PostalCode": null
  },
  "OtherAddress": {
    "Street": null,
    "City": null,
    "State": null,
    "CountryOrRegion": null,
    "PostalCode": null
  },
  "SpouseName": null,
  "PersonalNotes": null,
  "Children": [],
  "YomiSurname": null,
  "YomiGivenName": null,
  "YomiCompanyName": null
}
```


 **応答の種類**

新しい[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

連絡先をルート連絡先フォルダーまたは別の連絡先フォルダーの `contacts` エンドポイントに追加します。

```no-highlight
POST https://outlook.office.com/api/v1.0/me/contacts
POST https://outlook.office.com/api/v1.0/me/contactfolders/{contact_folder_id}/contacts
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定の連絡先フォルダーに連絡先を作成する場合は連絡先フォルダー ID です。|
|_本文パラメーター_|
|GivenName|string|連絡先の名前。|

要求本文に **GivenName** パラメーターと任意の書き込み可能な[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)プロパティを指定します。

[!code-REST-i[contacts_api_create_contact](./_data/contacts_api_create_contact.json)]

 **応答の種類**

新しい[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。




[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

****

<a name="CreateContactsClient"> </a>
###既定の連絡先フォルダーに連絡先を作成する (クライアント)

既定の連絡先フォルダーに連絡先を作成します。


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、既に [Outlook サービス クライアントが取得されている](#GetClient)と仮定します。


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
Contact newContact = new Contact
{
    GivenName = "Katie",
    Surname = "Jordan",
    JobTitle = "Auditor",
    Department = "Finance",
    BusinessPhones = { "+1 412 555 0109" },
    MobilePhone1 = "+1 412 555 9010",
    EmailAddresses = new List<EmailAddress>
    {
        new EmailAddress
        {
            Address = "katiej@a830edad9050849NDA1.onmicrosoft.com"
        }
    }
};
await client.Me.Contacts.AddContactAsync(newContact);

// Get the contact ID.
string contactId = newContact.Id;
```

<!-- ENDSECTION -->


****

<a name="UpdateContacts"> </a>
##連絡先を更新する

連絡先のプロパティを変更します。

REST API:[連絡先を更新する (REST)](#UpdateAContact)

クライアント ライブラリ:[連絡先を更新する (クライアント)](#UpdateContactsClient)

<a name="UpdateAContact"></a>
###連絡先を更新する (REST)

<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

要求本文に任意の書き込み可能な[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)プロパティを指定します。
指定したプロパティのみが変更されます。

```no-highlight
PATCH https://outlook.office.com/api/beta/me/contacts/{contact_id}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先 ID。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/beta/me/contacts/AAMkADlkAAARKMK7AAA=
Content-Type: application/json

{
  "PostalAddresses": [
    {
      "Type": "Business",
      "Street": "Some street",
      "City": "Seattle",
      "State": "WA",
      "PostalCode": "98121"
    }
  ]
  "Birthday": "1974-07-22"
}
```

**応答のサンプル**

```
Status code: 200

{
  "@odata.context":"https://outlook.office365.com/api/beta/$metadata#Me/Contacts/$entity",
  "@odata.id":"https://outlook.office365.com/api/beta/Users('af183ae6-7efa-41e4-aa87-fe8790598625@9ac5b33f-49cf-45f7-9ef1-b581dce364d8')/Contacts('AAMkADlkAAARKMK7AAA=')",
  "@odata.etag":"W/\"EQAAABYAAADDii8zlkFETIcBiRn8yAbgAAARKpia\"",
  "Id":"AAMkADlkAAARKMK7AAA=",
  "CreatedDateTime":"2016-07-20T23:59:37Z",
  "LastModifiedDateTime":"2016-07-20T23:59:38Z",
  "ChangeKey":"EQAAABYAAADDii8zlkFETIcBiRn8yAbgAAARKpia",
  "Categories":[
  ],
  "ParentFolderId":"AAMkADlk8yAbgAAAAAAEOAAA=",
  "Birthday":"1974-07-22T00:00:00Z",
  "FileAs":"",
  "DisplayName":"Pavel Bansky",
  "GivenName":"Pavel",
  "Initials":null,
  "MiddleName":null,
  "NickName":null,
  "Surname":"Bansky",
  "Title":null,
  "YomiGivenName":null,
  "YomiSurname":null,
  "YomiCompanyName":null,
  "Generation":null,
  "EmailAddresses":[
    {
      "Name":"Pavel Bansky",
      "Address":"pavelb@contoso.onmicrosoft.com"
    }
  ],
  "Websites":[
  ],
  "ImAddresses":[
  ],
  "JobTitle":null,
  "CompanyName":null,
  "Department":null,
  "OfficeLocation":null,
  "Profession":null,
  "AssistantName":null,
  "Manager":null,
  "Phones":[
    {
      "Type":"Business",
      "Number":"+1 732 555 0102"
    }
  ],
  "PostalAddresses":[
    {
      "Type": "Business",
      "Street": "Some street",
      "City": "Seattle",
      "State": "WA",
      "PostalCode": "98121"
    }
  ],
  "SpouseName":null,
  "PersonalNotes":null,
  "Children":[
  ],
  "Gender":null,
  "IsFavorite":null,
  "Flag":{
    "FlagStatus":"NotFlagged"
  }
}
```

 **応答の種類**

更新された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

要求本文に任意の書き込み可能な[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)プロパティを指定します。
指定したプロパティのみが変更されます。

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/contacts/{contact_id}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先 ID。|


**要求のサンプル**

```
PATCH https://outlook.office.com/api/v2.0/me/contacts/AAMkAGI2THkzAAA=
Content-Type: application/json

{
  "HomeAddress": {
    "Street": "Some street",
    "City": "Seattle",
    "State": "WA",
    "PostalCode": "98121"
  },
  "Birthday": "1974-07-22"
}
```

**応答のサンプル**

```
Status code: 200

{
  "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Contacts/$entity",
  "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Contacts('AAMkAGI2THkzAAA=')",
  "@odata.etag": "W/\"EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa3\"",
  "Id": "AAMkAGI2THkzAAA=",
  "ChangeKey": "EQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIa3",
  "Categories": [],
  "CreatedDateTime": "2014-10-19T23:08:18Z",
  "LastModifiedDateTime": "2014-10-19T23:08:18Z",
  "ParentFolderId": "AAMkAGI2AAEOAAA=",
  "Birthday": "1974-07-22T00:00:00Z",
  "FileAs": "Schorr, Janet",
  "DisplayName": "Janet Schorr",
  "GivenName": "Janet",
  "Initials": null,
  "MiddleName": null,
  "NickName": null,
  "Surname": "Schorr",
  "Title": null,
  "Generation": null,
  "EmailAddresses": [
    {
      "Address": "janets@a830edad9050849NDA1.onmicrosoft.com",
      "Name": "janets@a830edad9050849NDA1.onmicrosoft.com"
    },
    null,
    null
  ],
  "ImAddresses": [
    "sip:janets@a830edad9050849nda1.onmicrosoft.com",
    null,
    null
  ],
  "JobTitle": "Product Marketing Manager",
  "CompanyName": null,
  "Department": "Sales &amp; Marketing",
  "OfficeLocation": "18/2111",
  "Profession": null,
  "BusinessHomePage": null,
  "AssistantName": null,
  "Manager": null,
  "HomePhones": [
    null,
    null
  ],
  "BusinessPhones": [
    "+1 425 555 0109",
    null
  ],
  "MobilePhone1": null,
  "HomeAddress": {
    "Street": "Some street",
    "City": "Seattle",
    "State": "WA",
    "CountryOrRegion": null,
    "PostalCode": "98121"
  },
  "BusinessAddress": {
    "Street": null,
    "City": null,
    "State": null,
    "CountryOrRegion": null,
    "PostalCode": null
  },
  "OtherAddress": {
    "Street": null,
    "City": null,
    "State": null,
    "CountryOrRegion": null,
    "PostalCode": null
  },
  "SpouseName": null,
  "PersonalNotes": null,
  "Children": [],
  "YomiSurname": null,
  "YomiGivenName": null,
  "YomiCompanyName": null
}
```

 **応答の種類**

更新された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

要求本文に任意の書き込み可能な[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)プロパティを指定します。
指定したプロパティのみが変更されます。

```no-highlight
PATCH https://outlook.office.com/api/v1.0/me/contacts/{contact_id}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先の ID。|


[!code-REST-i[contacts_api_update_contact_by_id](./_data/contacts_api_update_contact_by_id.json)]

 **応答の種類**

更新された[連絡先](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)です。



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

****

<a name="UpdateContactsClient"> </a>
###連絡先を更新する (クライアント)


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアントの取得](#GetClient)と[連絡先 ID の取得](#GetContactsClient)が済んでいると仮定します。


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IContact contact = await client.Me.Contacts[contactId].ExecuteAsync();
contact.HomeAddress = new PhysicalAddress
{
    Street = "Some Street St",
    City = "Seattle",
    State = "WA",
    PostalCode = "98121"
};
await contact.UpdateAsync();

// Get an updated property.
string contactPostalCode = contact.HomeAddress.PostalCode;
```

<!-- ENDSECTION -->

次のパターンを使用して、クライアント側で複数の更新を定義し、要求をまとめて (バッチ処理で) 送信できます。
1. 更新するエンティティごとに `UpdateAsync(true)` を呼び出します。 `true` を指定すると、更新がクライアント上でローカルに登録されますが、サーバーには投稿されません。
2. `client.Context.SaveChangesAsync()` を呼び出して、ローカルに登録されているすべての更新を投稿します。

****


<a name="DeleteContacts"> </a>
##連絡先を削除する

連絡先を削除します。 削除した内容を回復できない可能性があります。
詳細については、「[アイテムの削除](http://msdn.microsoft.com/library/c81e3160-e12b-47e0-b3d6-4be28537f301%28Office.15%29.aspx)」を参照してください。
 
REST API:[連絡先を削除する (REST)](#DeleteAContact)

クライアント ライブラリ:[連絡先を削除する (クライアント)](#DeleteContactsClient)

<a name="DeleteAContact"></a>
###連絡先を削除する (REST)

<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

```no-highlight
DELETE https://outlook.office.com/api/beta/me/contacts/{contact_id}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先 ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/beta/me/contacts/AAMkAGE0Myy2hAAA=
```

**応答のサンプル**

```
Status code: 204
```



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

```no-highlight
DELETE https://outlook.office.com/api/v2.0/me/contacts/{contact_id}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先 ID。|


**要求のサンプル**

```
DELETE https://outlook.office.com/api/v2.0/me/contacts/AAMkAGE0Myy2hAAA=
```

**応答のサンプル**

```
Status code: 204
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]



_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

```no-highlight
DELETE https://outlook.office.com/api/v1.0/me/contacts/{contact_id}
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|string|連絡先の ID。|


[!code-REST-i[contacts_api_delete_contact_by_id](./_data/contacts_api_delete_contact_by_id.json)]






[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


****

<a name="DeleteContactsClient"> </a>
###連絡先を削除する (クライアント)


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


この例では、[Outlook サービス クライアントの取得](#GetClient)と[連絡先 ID の取得](#GetContactsClient)が済んでいると仮定します。


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IContact contact = await client.Me.Contacts[contactId].ExecuteAsync();
await contact.DeleteAsync();
```

<!-- ENDSECTION -->

****

<a name="GetContactFolders"> </a>
##連絡先フォルダーの取得

連絡先フォルダーのコレクションを取得したり、連絡先フォルダーを取得したりすることができます。

REST API:[連絡先フォルダーのコレクションを取得する (REST)](#GetContactFolderCollection) | [連絡先フォルダーを取得する (REST)](#GetContactFolder)

クライアント ライブラリ:[連絡先フォルダーの取得 (クライアント)](#GetContactFoldersClient)

<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]



<a name="GetContactFolderCollection"> </a>
###連絡先フォルダーのコレクションを取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

サインインしているユーザーのメールボックス内のすべての連絡先フォルダーを取得する (`.../me/contactfolders`) か、指定された連絡先フォルダーから取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/contactfolders
GET https://outlook.office.com/api/beta/me/contactfolders/{contact_folder_id}/childfolders
```

**注** パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定の連絡先フォルダーから連絡先フォルダーを取得している場合は、連絡先フォルダー ID です。|


**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/contactfolders
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/ContactFolders",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/ContactFolders('AAMkAGI2TKI5AAA=')",
            "Id": "AAMkAGI2TKI5AAA=",
            "ParentFolderId": "AAMkAGI2AAEOAAA=",
            "DisplayName": "Finance",
            "WellKnownName": null
        },
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/ContactFolders('AQMkADA1MTgAAAA==')",
            "Id": "AQMkADA1MTgAAAA==",
            "ParentFolderId": "AAMkAGI2AAEOAAA=",
            "DisplayName": "Contacts",
            "WellKnownName": "contacts"
        }
    ]
}
```

**応答の種類**

要求された[連絡先フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)のコレクションです。

****


<a name="GetContactFolder"> </a>
###連絡先フォルダーの取得 (REST) 

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

連絡先フォルダー ID を使用して連絡先フォルダーを取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/contactfolders/{contact_folder_id}
```

**注** パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|連絡先フォルダー ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/contactfolders/AAMkAGI2TKI5AAA=
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/ContactFolders/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/ContactFolders('AAMkAGI2TKI5AAA=')",
    "Id": "AAMkAGI2TKI5AAA=",
    "ParentFolderId": "AAMkAGI2AAEOAAA=",
    "DisplayName": "Finance",
    "WellKnownName": null
}
```

 **応答の種類**

要求された[連絡先フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)です。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


<a name="GetContactFolderCollection"> </a>
###連絡先フォルダーのコレクションを取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

サインイン中のユーザーの既定の連絡先フォルダーから連絡先フォルダーのコレクションを取得する (`.../me/contactfolders`) か、指定した連絡先フォルダーから取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/contactfolders
GET https://outlook.office.com/api/v2.0/me/contactfolders/{contact_folder_id}/childfolders
```

**注** パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定の連絡先フォルダーから連絡先フォルダーを取得している場合は、連絡先フォルダー ID です。|


**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/contactfolders
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/ContactFolders",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/ContactFolders('AAMkAGI2TKI5AAA=')",
            "Id": "AAMkAGI2TKI5AAA=",
            "ParentFolderId": "AAMkAGI2AAEOAAA=",
            "DisplayName": "Finance"
        }
    ]
}
```

**応答の種類**

要求された[連絡先フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)のコレクションです。

****


<a name="GetContactFolder"> </a>
###連絡先フォルダーの取得 (REST) 

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

連絡先フォルダー ID を使用して連絡先フォルダーを取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/contactfolders/{contact_folder_id}
```

**注** パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|連絡先フォルダー ID。|


**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/contactfolders/AAMkAGI2TKI5AAA=
```

**応答のサンプル**

```
Status code: 200

{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/ContactFolders/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/ContactFolders('AAMkAGI2TKI5AAA=')",
    "Id": "AAMkAGI2TKI5AAA=",
    "ParentFolderId": "AAMkAGI2AAEOAAA=",
    "DisplayName": "Finance"
}
```

 **応答の種類**

要求された[連絡先フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)です。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]



<a name="GetContactFolderCollection"> </a>
###連絡先フォルダーのコレクションを取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

サインイン中のユーザーの既定の連絡先フォルダーから連絡先フォルダーのコレクションを取得する (`.../me/contactfolders`) か、指定した連絡先フォルダーから取得します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/contactfolders
GET https://outlook.office.com/api/v1.0/me/contactfolders/{contact_folder_id}/childfolders
```

**注** パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|特定の連絡先フォルダーから連絡先フォルダーを取得している場合は、連絡先フォルダー ID です。|


[!code-REST-i[contacts_api_contact_folder_get_collection_folder_path](./_data/contacts_api_contact_folder_get_collection_folder_path.json)]

 **応答の種類**

要求された[連絡先フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)のコレクションです。

****


<a name="GetContactFolder"> </a>
###連絡先フォルダーの取得 (REST) 

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

連絡先フォルダー ID を使用して連絡先フォルダーを取得します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/contactfolders/{contact_folder_id}
```

**注** パラメーターのフィルタリング、並べ替え、およびページングについては、「[OData クエリ パラメーター](..\api\complex-types-for-mail-contacts-calendar.md#OdataQueryParams)」を参照してください。


|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_folder_id|string|連絡先フォルダー ID。|

[!code-REST-i[contacts_api_contact_get_contact_folder_by_id](./_data/contacts_api_contact_get_contact_folder_by_id.json)]


 **応答の種類**

要求された[連絡先フォルダー](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)です。


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->


****

<a name="GetContactFoldersClient"> </a>
###連絡先フォルダーの取得 (クライアント)

既定の連絡先フォルダーにある連絡先フォルダーを取得します。特定の連絡先フォルダーを取得するには、**ContactFolders** コレクションのインデックスとしてフォルダー ID を指定するか、**GetById** メソッドを使用します。

例: `client.Me.ContactFolders[folderId].ExecuteAsync()`


**注意** Outlook.com のメールボックス データにアクセスしている場合は、クライアント ライブラリを使用して REST API を直接呼び出さないでください。


**注** 連絡先フォルダー コレクションでは、**Select**、**OrderBy**、**Take** などのクエリ式がサポートされます。

この例では、[Outlook サービス クライアントを取得する](#GetClient)メソッドを呼び出します。

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Contacts" -->

```cs
var outlookClient = await CreateOutlookClientAsync("Contacts");
var contactFolders = await outlookClient.Me.ContactFolders
  .Take(10)
  .ExecuteAsync();

foreach(var contactFolder in contactFolders.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine("Contact folder '{0}'.", contactFolder.Id);
}

```
```javascript-i
outlookClient.me.contactFolders.getContactFolders().orderBy('DisplayName asc').fetch().then(
function (result) {
    result.currentPage.forEach(function (folder) {
        console.log('Folder "' + folder.displayName + '"');
    });
}, function(error) {
    console.log(error);
}
);
```

<!-- ENDSECTION -->

<!--Write not supported for GA
<a name="CreateContactFolders"> </a>
## Create contact folders

Create a contact folder in the default Contacts folder.

This example assumes you already [got the Outlook Services client](#GetClient).


```cs
ContactFolder newContactFolder = new ContactFolder
{
    DisplayName = "Company"
};
await client.Me.ContactFolders.AddContactFolderAsync(newContactFolder);

// Get the ID of the contact folder.
string contactFolderId = newContactFolder.Id;
```



<a name="UpdateContactFolders"> </a>
## Update contact folders

Change a contact folder's display name.

This example assumes you already [got the Outlook Services client](#GetClient) and [got the folder ID](#GetContactFolders).



```cs
IContactFolder contactFolder = await client.Me.ContactFolders[contactFolderId].ExecuteAsync();
contactFolder.DisplayName = "Business";
await contactFolder.UpdateAsync();

// Get the updated property.
string newContactFolderName = contactFolder.DisplayName;
```



You can define multiple updates client-side and send the requests all at once (batch them) by using the following pattern:
1. Call `UpdateAsync(true)` for each entity you want to update. Specifying `true` registers the updates locally on the client but doesn't post them to the server.
2. Call `client.Context.SaveChangesAsync()` to post all updates that are registered locally.



<a name="DeleteContactFolders"> </a>
## Delete contact folders

Delete a contact folder.
This example assumes you already [got the Outlook Services client](#GetClient) and [got the folder ID](#GetContactFolders).



```cs
IContactFolder contactFolder = await client.Me.ContactFolders[contactFolderId].ExecuteAsync();
await contactFolder.DeleteAsync();
```

-->



****

<a name="GetContactPhotoandMetadata"></a>
##連絡先の写真とメタデータを取得する

<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

[連絡先の写真を取得する](#GetContactPhoto) | [連絡先の写真のメタデータを取得する](#GetContactPhotoMetadata)

<a name="GetContactPhoto"></a>

###連絡先の写真を取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

指定したサインインしているユーザーの連絡先の写真を取得します。 


```no-highlight
GET https://outlook.office.com/api/beta/me/contacts('{contact_id}')/photo/$value
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|文字列|サインインしているユーザーの特定の連絡先を指定する ID です。|

**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/contacts('AAMkAGE1M2IyNGNm===')/photo/$value
Content-Type: image/jpg
```

**応答データ**

要求した写真のバイナリ データが含まれています。 HTTP 応答コードは 200 です。

この操作では、Exchange Online で連絡先に連絡先の写真がまだない場合に、HTTP 404 を返します。

****

<a name="GetphotoMetadata"></a>

###連絡先の写真とメタデータを取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

コンテンツの種類、幅および高さがピクセル単位で含まれている連絡先の写真のメタデータを取得します。 


```no-highlight
GET https://outlook.office.com/api/beta/me/contacts('{contact_id}')/photo
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|文字列|サインインしているユーザーの特定の連絡先を指定する ID です。|

**要求のサンプル**

```
GET https://outlook.office.com/api/beta/me/contacts('AAMkAGE1M2IyNGNm')/photo
```

**応答データのサンプル**

成功した要求は、HTTP 200 を返します。

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Contacts('AAMkAGE1M2IyNGNm')/photo/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-b826-40d7-b48b-57002df800e5@1717622f-49d1-4d0c-9d74-709fad664b77')/contacts('AAMkAGE1M2IyNGNm')/photo",
    "@odata.readLink": "https://outlook.office.com/api/beta/Users('ddfcd489-b826-40d7-b48b-57002df800e5@1717622f-49d1-4d0c-9d74-709fad664b77')/contacts('AAMkAGE1M2IyNGNm')/photo",
    "@odata.mediaContentType": "image/jpeg",
    "Id": "103X77",
    "Width": 103,
    "Height": 77
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

[連絡先の写真を取得する](#GetContactPhoto) | [連絡先の写真のメタデータを取得する](#GetContactPhotoMetadata)

<a name="GetContactPhoto"></a>

###連絡先の写真を取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

指定したサインインしているユーザーの連絡先の写真を取得します。 


```no-highlight
GET https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/photo/$value
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|文字列|サインインしているユーザーの特定の連絡先を指定する ID です。|

**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/contacts('AAMkAGE1M2IyNGNm===')/photo/$value
Content-Type: image/jpg
```

**応答データ**

要求した写真のバイナリ データが含まれています。 HTTP 応答コードは 200 です。

この操作では、Exchange Online で連絡先に連絡先の写真がまだない場合に、HTTP 404 を返します。

****

<a name="GetContactPhotoMetadata"></a>

###連絡先の写真とメタデータを取得する (REST)

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.read_
- _wl.basic_

コンテンツの種類、幅および高さがピクセル単位で含まれている連絡先の写真のメタデータを取得します。 


```no-highlight
GET https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/photo
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|文字列|サインインしているユーザーの特定の連絡先を指定する ID です。|

**要求のサンプル**

```
GET https://outlook.office.com/api/v2.0/me/contacts('AAMkAGE1M2IyNGNm')/photo
```

**応答データのサンプル**

成功した要求は、HTTP 200 を返します。

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Contacts('AAMkAGE1M2IyNGNm')/photo/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-b826-40d7-b48b-57002df800e5@1717622f-49d1-4d0c-9d74-709fad664b77')/contacts('AAMkAGE1M2IyNGNm')/photo",
    "@odata.readLink": "https://outlook.office.com/api/v2.0/Users('ddfcd489-b826-40d7-b48b-57002df800e5@1717622f-49d1-4d0c-9d74-709fad664b77')/contacts('AAMkAGE1M2IyNGNm')/photo",
    "@odata.mediaContentType": "image/jpeg",
    "Id": "103X77",
    "Width": 103,
    "Height": 77
}
```



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は V2.0 とベータ版で利用できます。詳細については、記事の右上隅にコントロールを使用して、どちらかのバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

****


<a name="SetContactPhoto"></a>

##連絡先の写真を設定する



<!-- ============================================================================================================ -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

指定したサインインしているユーザーの連絡先に写真を割り当てます。 写真はバイナリ形式にする必要があります。 該当する連絡先の既存の写真と置き換えられます  
。 

ベータ版では、この操作にのみ PUT を使用します。


```no-highlight
PUT https://outlook.office.com/api/beta/me/contacts('{contact_id}')/photo/$value
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|文字列|サインインしているユーザーの特定の連絡先を指定する ID です。|


**要求のサンプル**

```
PUT https://outlook.office.com/api/beta/me/contacts('AAMkAGE1M2IyNGNm===')/photo/$value
Content-Type: image/jpeg
```

要求の本文に、写真のバイナリ データを含めます。
 

**応答データ**

成功した要求は、HTTP 200 を返します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End BETA content ====================================================== -->

<!-- ============================================================================================================ -->


<!-- ============================================================================================================ -->

<!-- ==================================== Start v2 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

_**最小限必要なスコープ**: 次のいずれかです。_
- _https://outlook.office.com/contacts.readwrite_
- _wl.contacts_create_

指定したサインインしているユーザーの連絡先に写真を割り当てます。 写真はバイナリ形式にする必要があります。 該当する連絡先の既存の写真と置き換えられます  
。 

バージョン 2.0 では、この操作に PATCH または PUT を使用できます。


```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/photo/$value

PUT https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')/photo/$value
```

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|contact_id|文字列|サインインしているユーザーの特定の連絡先を指定する ID です。|


**要求のサンプル**

```
PUT https://outlook.office.com/api/v2.0/me/contacts('AAMkAGE1M2IyNGNm===')/photo/$value
Content-Type: image/jpeg
```

要求の本文に、写真のバイナリ データを含めます。
 

**応答データ**

成功した要求は、HTTP 200 を返します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v2 content ======================================================== -->

<!-- ============================================================================================================ -->



<!-- ============================================================================================================ -->

<!-- ==================================== Start v1 content ====================================================== -->

<!-- ============================================================================================================ -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は V2.0 とベータ版で利用できます。詳細については、記事の右上隅にコントロールを使用して、どちらかのバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ============================================================================================================ -->

<!-- ==================================== End v1 content ======================================================== -->

<!-- ============================================================================================================ -->

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

- [Mail API リファレンス](..\api\mail-rest-operations.md)

- [Calendar API リファレンス](..\api\calendar-rest-operations.md)

- [タスク REST API (プレビュー)](..\api\task-rest-operations.md)

- [OneDrive API](https://dev.onedrive.com/)

- [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)   

[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]




