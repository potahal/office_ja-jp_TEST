---
ms.Toctitle: Outlook Extended Properties REST API reference
title: "Outlook 拡張プロパティ REST API リファレンス (プレビュー)"
description: "ms.TocTitle:Outlook 拡張プロパティ REST API リファレンス (プレビュー)Title:Outlook 拡張プロパティ REST API リファレンス (プレビュー)Description:Office 365 と Outlook.com で拡張プロパティ REST API を使用して、拡張プロパティを動的に作成して取得し、カスタム データを格納する方法に関するリファレンスです。data.ms.ContentId:69a1ae02-3060-40c6-ad5e-2c23b0f00827ms.topic: リファレンス (API) ms.date:2016 年 5 月 16 日"
ms.ContentId: 69a1ae02-3060-40c6-ad5e-2c23b0f00827
ms.date: July 26, 2016

---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Outlook 拡張プロパティ REST API リファレンス (プレビュー)

[!INCLUDE [Add the Outlook REST API filters--v2 default](../includes/controls/addOutlookversion_v2default_v1disabled.xml)]


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


 _**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

<p class="previewnote">This version of the documentation covers the Outlook Extended Properties REST API in preview. Preview features are subject to change prior to finalization, and may break code that uses them. プレビュー機能は、最終処理までに変更される場合があります。それらを使用するコードを破棄する場合もあります。このため、一般に、実稼働コードでは実稼働バージョンの API のみを使用してください。可能な場合には、現在バージョン 2.0 が優先バージョンです。</p> If available, v2.0 is currently the preferred version.</p>

The Outlook Extended Properties REST API allows apps to store custom data and provides them a [fallback mechanism](#ExtendedPropsOrDataExtensions) to access custom data for Outlook MAPI properties when these properties are _not already exposed in the Outlook REST API metadata_. You can use the Extended Properties API to store or get such custom data in a message, mail folder, event, calendar, contact, or contact folder in the signed-in user's account. Office 365 データ拡張機能 REST API を使用すると、アプリによりユーザー アカウントのメッセージ、イベント、または連絡先でカスタム データを動的に保存できるようになります。Office 365 アカウントまたは Microsoft アカウント (Hotmail.com、Live.com、MSN.com、Outlook.com、Passport.com) をこのアカウントの対象にすることができます。

**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。 

**Not interested in the beta version of the API?** API v2.0 が不要な場合右上隅のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

 _**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

The Outlook Extended Properties REST API allows apps to store custom data and provides them a [fallback mechanism](#ExtendedPropsOrDataExtensions) to access custom data for Outlook MAPI properties when these properties are _not already exposed in the Outlook REST API metadata_. You can use the Extended Properties API to store or get such custom data in a message, mail folder, event, calendar, contact, or contact folder in the signed-in user's account. Office 365 データ拡張機能 REST API を使用すると、アプリによりユーザー アカウントのメッセージ、イベント、または連絡先でカスタム データを動的に保存できるようになります。Office 365 アカウントまたは Microsoft アカウント (Hotmail.com、Live.com、MSN.com、Outlook.com、Passport.com) をこのアカウントの対象にすることができます。

**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。 

**Not interested in v2.0 of the API?** API v1.0 に関心がない場合: 右上のコントロールを使用して、必要なバージョンを選択します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


## REST API の拡張プロパティを使用する

<a name="ExtendedPropsOrDataExtensions"></a>

###拡張プロパティとデータ拡張機能のどちらを使用するか

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Use extended properties and this REST API only if you need to access custom data for Outlook MAPI properties that are not already exposed through the Outlook REST API metadata. For most scenarios, you should use [Office 365 Data Extensions and its REST API](..\api\extensions-rest-operations.md) to store and access custom data for items in the user’s mailbox. You can verify which properties the metadata exposes at `https://outlook.office.com/api/{version}/$metadata`, substituting {version} by v2.0, beta, etc., for the version of your choice.

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Use extended properties and this REST API only if you need to access custom data for Outlook MAPI properties that are not already exposed through the Outlook REST API metadata. For most scenarios, you should use [Office 365 Data Extensions and its REST API](..\api\extensions-rest-operations.md) to store and access custom data for items in the user’s mailbox. You can verify which properties the metadata exposes at `https://outlook.office.com/api/{version}/$metadata`, substituting {version} by v2.0, beta, etc., for the version of your choice.


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



###認証

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the Extended Properties API, you should include a valid access token. アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you.
Keep this in mind as you proceed with the specific operations in the Extended Properties API.


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the Extended Properties API, you should include a valid access token. アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you.
Keep this in mind as you proceed with the specific operations in the Extended Properties API.


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


###API のバージョン

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

この API は、プレビューから一般提供 (GA) の状態にレベル上げされています。Outlook REST API の v2.0 とベータのバージョンでサポートされます。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この API は、プレビューから一般提供 (GA) の状態にレベル上げされています。Outlook REST API の v2.0 とベータのバージョンでサポートされます。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<a name="SupportedResources"></a>

###サポートされている REST リソース

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

次のリソースのインスタンスについて、拡張プロパティを作成できます。 
- [Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)
- [イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)
- [Contact](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)
- [MailFolder](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)
- [予定表](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource)
- [ContactFolder](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)

リソースは、Office 365 または Outlook.com のサインインしているユーザーのメールボックスに存在している必要があります。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

次のリソースのインスタンスについて、拡張プロパティを作成できます。 
- [Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource)
- [イベント](..\api\complex-types-for-mail-contacts-calendar.md#EventResource)
- [Contact](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource)
- [MailFolder](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource)
- [予定表](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource)
- [ContactFolder](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource)

リソースは、Office 365 または Outlook.com のサインインしているユーザーのメールボックスに存在している必要があります。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




###URL のパラメーター

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

この記事の例では、REST 要求 URL のパラメーターに次の ID のプレース ホルダーを使用します。拡張機能を作成する対象のリソースのインスタンスの ID を指定する必要があります。

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。 |
|contact_id|string|連絡先 ID。 |
|contactFolder_id|文字列|連絡先フォルダー ID。 |
|event_id|string|イベント ID。 |
|mailFolder_id|文字列|メール フォルダー ID。 |
|message_id|string|メッセージ ID。 |
|propertyId_value|文字列|[サポートされている形式](#ExtendedPropertyIdFormats)のいずれかの **PropertyId** の値。|
|property_value|文字列|拡張プロパティの値。|


Outlook REST API のすべてのサブセットに共通な情報について詳しくは、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この記事の例では、REST 要求 URL のパラメーターに次の ID のプレース ホルダーを使用します。拡張機能を作成する対象のリソースのインスタンスの ID を指定する必要があります。

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|calendar_id|string|予定表 ID。 |
|contact_id|string|連絡先 ID。 |
|contactFolder_id|文字列|連絡先フォルダー ID。 |
|event_id|string|イベント ID。 |
|mailFolder_id|文字列|メール フォルダー ID。 |
|message_id|string|メッセージ ID。 |
|propertyId_value|文字列|[サポートされている形式](#ExtendedPropertyIdFormats)のいずれかの **PropertyId** の値。|
|property_value|文字列|拡張プロパティの値。|


Outlook REST API のすべてのサブセットに共通な情報について詳しくは、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




****

## 概要

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

You can dynamically create and store data in an _extended property_ of an entity instance.
エンティティ インスタンスの拡張プロパティでデータを動的に作成して保存できます。単一の値、または複数の値を保存しようとしているかどうかに応じて、拡張プロパティを [SingleValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#SingleValueLegacyExtendedPropertyResource)、または [MultiValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#MultiValueLegacyExtendedPropertyResource) として作成できます。

これらのタイプはそれぞれ、**PropertyId** によってプロパティを識別し、データを **Value** に格納します。 

**PropertyId** を使用して、特定のエンティティ インスタンスをその拡張プロパティと一緒に取得するか、単一の値の拡張プロパティでフィルターして、そのプロパティのすべてのインスタンスを取得できます。 

**注** REST API を使用して、1 つの呼び出しで特定のインスタンスのすべての拡張プロパティを取得することはできません。  

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

You can dynamically create and store data in an _extended property_ of an entity instance.
エンティティ インスタンスの拡張プロパティでデータを動的に作成して保存できます。単一の値、または複数の値を保存しようとしているかどうかに応じて、拡張プロパティを [SingleValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#SingleValueLegacyExtendedPropertyResource_v2)、または [MultiValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#MultiValueLegacyExtendedPropertyResource_v2) として作成できます。

これらのタイプはそれぞれ、**PropertyId** によってプロパティを識別し、データを **Value** に格納します。 

**PropertyId** を使用して、特定のエンティティ インスタンスをその拡張プロパティと一緒に取得するか、単一の値の拡張プロパティでフィルターして、そのプロパティのすべてのインスタンスを取得できます。 

**注** REST API を使用して、1 つの呼び出しで特定のインスタンスのすべての拡張プロパティを取得することはできません。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




<a name="ExtendedPropertyIdFormats"></a>

### PropertyId の形式

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

単一値または複数値の拡張プロパティを作成する場合、文字列の名前または数値識別子のいずれか、および値の実際の型またはプロパティの値に基づき、2 つの形式のいずれかで PropertyId を指定できます。Sinceextended プロパティはほとんどの場合、Outlook REST API メタデータで公開されていない定義済みの MAPI プロパティと相互運用します。簡略化のため、選択する形式は、対応する MAPI プロパティが MAPI プロパティの識別子で文字列を使用しているか、数値を使用しているかを反映する必要があります。 単一値または複数値の拡張プロパティを作成する場合、文字列の名前または数値識別子のいずれか、および値の実際の型またはプロパティの値に基づき、2 つの形式のいずれかで PropertyId を指定できます。Sinceextended プロパティはほとんどの場合、Outlook REST API メタデータで公開されていない定義済みの MAPI プロパティと相互運用します。簡略化のため、選択する形式は、対応する MAPI プロパティが MAPI プロパティの識別子で文字列を使用しているか、数値を使用しているかを反映する必要があります。
You can find information that you would need to map an extended property to an existing MAPI property, such as the property identifier and GUID, in \[MS-OXPROPS\] Microsoft Corporation, ["Exchange Server Protocols Master Property List"](https://msdn.microsoft.com/en-us/library/cc433490%28v=exchg.80%29.aspx).

**注****PropertyId** にある形式を選択したら、その形式でのみ拡張プロパティにアクセスする必要があります。


**単一値の拡張プロパティに有効な PropertyId の形式**

|**形式**|**例**|**説明**|
|:---------|:----------|:--------------|
| "*{type} {guid} Name {name}*" | ```"String {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Name TestProperty"``` | 属している名前空間 (GUID) と名前で、プロパティを識別します。         |
| "*{type} {guid} Id {id}*"     | ```"Integer {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Id 0x8012"```        | 属している名前空間 (GUID) と識別子で、プロパティを識別します。  |


**複数値の拡張プロパティの有効な PropertyId の形式**

|**形式**|**例**|**説明**|
|:---------|:----------|:--------------|
| "*{type} {guid} Name {name}*" | ```"StringArray {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Name TestProperty"``` | 名前空間 (GUID) と名前でプロパティを識別します。         |
| "*{type} {guid} Id {id}*"     | ```"IntegerArray {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Id 0x8013"```        | 名前空間 (GUID) と識別子でプロパティを識別します。   |


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

単一値または複数値の拡張プロパティを作成する場合、文字列の名前または数値識別子のいずれか、および値の実際の型またはプロパティの値に基づき、2 つの形式のいずれかで PropertyId を指定できます。Sinceextended プロパティはほとんどの場合、Outlook REST API メタデータで公開されていない定義済みの MAPI プロパティと相互運用します。簡略化のため、選択する形式は、対応する MAPI プロパティが MAPI プロパティの識別子で文字列を使用しているか、数値を使用しているかを反映する必要があります。 単一値または複数値の拡張プロパティを作成する場合、文字列の名前または数値識別子のいずれか、および値の実際の型またはプロパティの値に基づき、2 つの形式のいずれかで PropertyId を指定できます。Sinceextended プロパティはほとんどの場合、Outlook REST API メタデータで公開されていない定義済みの MAPI プロパティと相互運用します。簡略化のため、選択する形式は、対応する MAPI プロパティが MAPI プロパティの識別子で文字列を使用しているか、数値を使用しているかを反映する必要があります。
You can find information that you would need to map an extended property to an existing MAPI property, such as the property identifier and GUID, in \[MS-OXPROPS\] Microsoft Corporation, ["Exchange Server Protocols Master Property List"](https://msdn.microsoft.com/en-us/library/cc433490%28v=exchg.80%29.aspx).

**注****PropertyId** にある形式を選択したら、その形式でのみ拡張プロパティにアクセスする必要があります。


**単一値の拡張プロパティに有効な PropertyId の形式**

|**形式**|**例**|**説明**|
|:---------|:----------|:--------------|
| "*{type} {guid} Name {name}*" | ```"String {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Name TestProperty"``` | 属している名前空間 (GUID) と名前で、プロパティを識別します。         |
| "*{type} {guid} Id {id}*"     | ```"Integer {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Id 0x8012"```        | 属している名前空間 (GUID) と識別子で、プロパティを識別します。  |


**複数値の拡張プロパティの有効な PropertyId の形式**

|**形式**|**例**|**説明**|
|:---------|:----------|:--------------|
| "*{type} {guid} Name {name}*" | ```"StringArray {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Name TestProperty"``` | 名前空間 (GUID) と名前でプロパティを識別します。         |
| "*{type} {guid} Id {id}*"     | ```"IntegerArray {8ECCC264-6880-4EBE-992F-8888D2EEAA1D} Id 0x8013"```        | 名前空間 (GUID) と識別子でプロパティを識別します。   |


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




****

<a name="ExtendedPropertyOperations"> </a>
## 拡張プロパティの操作

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

既存のアイテムの拡張プロパティを作成する  新しいアイテムの拡張プロパティを作成する  

拡張プロパティを使用して展開されているアイテムを取得する  単一値の拡張プロパティのフィルター 


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

既存のアイテムの拡張プロパティを作成する  新しいアイテムの拡張プロパティを作成する  

拡張プロパティを使用して展開されているアイテムを取得する  単一値の拡張プロパティのフィルター 


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




<a name="CreateExtendedPropertyInExistingItem"> </a>

### 既存のアイテムの拡張プロパティを作成する

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

[サポートされたリソース](#SupportedResources)の指定されたインスタンスの 1 つ以上の拡張プロパティを作成します。各拡張プロパティには、単一値または複数値を指定できます。 Each extended property can be single-valued or multi-valued. 

```no-highlight
PATCH https://outlook.office.com/api/beta/me/messages('{message_id}')
PATCH https://outlook.office.com/api/beta/me/events('{event_id}')
PATCH https://outlook.office.com/api/beta/me/contacts('{contact_id}')
PATCH https://outlook.office.com/api/beta/me/mailfolders('{mailFolder_id}')
PATCH https://outlook.office.com/api/beta/me/calendars('{calendar_id}')
PATCH https://outlook.office.com/api/beta/me/contactfolders('{contactFolder_id}')
```

__**最低限必要な範囲**: 次のいずれかのうち、対象リソースに対応する読み取り/書き込み範囲:__
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_Body parameters_|
|SingleValueExtendedProperties|コレクション ([SingleValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#SingleValueLegacyExtendedPropertyResource))| 1 つ以上の単一値を持つ拡張プロパティの配列。 |
|PropertyId|文字列|**SingleValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。単一値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|string|**SingleValueExtendedProperties** コレクションの各プロパティに対し、プロパティの値を特定します。必須。 必須。|
|MultiValueExtendedProperties|コレクション ([MultiValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#MultiValueLegacyExtendedPropertyResource)) | 1 つ以上の複数値を持つ拡張プロパティの配列。|
|PropertyId|文字列|**MultiValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。複数値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|Collection(string)|**MultiValueExtendedProperties** コレクションの各プロパティに対し、そのプロパティの値を指定します。必須。 必須。|


 
**要求のサンプル:**

The first example creates one single-value extended property for the specified message. That extended property is the only element in the **SingleValueExtendedProperties** array. The request body includes the following for the extended property:
- PropertyId はプロパティの型に 、GUID、および  という名前のプロパティを指定します。
- **Value** specifies `Green` as the value of the `Color` property.

```
PATCH https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2_bs88AACHsLqWAAA=')

Content-Type: application/json

{
  "SingleValueExtendedProperties": [
      {
         "PropertyId":"String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color",
         "Value":"Green"
      }
    ]
}
```

**応答のサンプル**

[メッセージの更新](..\api\mail-rest-operations.md#UpdateAMessage)からの応答と同様に、HTTP 200 OK`HTTP 200 OK` 応答コードによって正常な応答が示され、応答の本文に指定したメッセージが含まれています。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include the newly created extended property.


**要求のサンプル:**

2 番目の例では、指定したメッセージに対して1 つの複数値の拡張プロパティを作成します。そのプロパティは、**MultiValueExtendedProperties** 配列の唯一の要素です。要求本文には、次のものが含まれます。
- **PropertyId** 指定された GUID と名前 Recreation`Palette` の文字列の配列としてプロパティを指定します。 
- **値** 3 つの文字列値 ["Food", "Hiking", "Swimming"]`Palette` の配列として Recreation`["Green", "Aqua", "Blue"]` を指定します。

```
PATCH https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2_as77AACHsLrBBBA=')

Content-Type: application/json

{
  "MultiValueExtendedProperties": [
      {
         "PropertyId":"StringArray {66f5a359-4659-4830-9070-00049ec6ac6e} Name Palette",
         "Value":["Green", "Aqua", "Blue"]
      }
    ]
}
```

**応答のサンプル**

[メッセージの更新](..\api\mail-rest-operations.md#UpdateAMessage)からの応答と同様に、HTTP 200 OK`HTTP 200 OK` 応答コードによって正常な応答が示され、応答の本文に指定したメッセージが含まれています。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include the newly created extended property.

新しく作成された拡張プロパティを表示するには、[拡張プロパティを使用して展開されているメッセージを取得](#GetExpandedExtendedProperty)します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

[サポートされたリソース](#SupportedResources)の指定されたインスタンスの 1 つ以上の拡張プロパティを作成します。各拡張プロパティには、単一値または複数値を指定できます。 Each extended property can be single-valued or multi-valued. 

```no-highlight
PATCH https://outlook.office.com/api/v2.0/me/messages('{message_id}')
PATCH https://outlook.office.com/api/v2.0/me/events('{event_id}')
PATCH https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')
PATCH https://outlook.office.com/api/v2.0/me/mailfolders('{mailFolder_id}')
PATCH https://outlook.office.com/api/v2.0/me/calendars('{calendar_id}')
PATCH https://outlook.office.com/api/v2.0/me/contactfolders('{contactFolder_id}')
```

__**最低限必要な範囲**: 次のいずれかのうち、対象リソースに対応する読み取り/書き込み範囲:__
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_Body parameters_|
|SingleValueExtendedProperties|コレクション ([SingleValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#SingleValueLegacyExtendedPropertyResource_v2))| 1 つ以上の単一値を持つ拡張プロパティの配列。 |
|PropertyId|文字列|**SingleValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。単一値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|string|**SingleValueExtendedProperties** コレクションの各プロパティに対し、プロパティの値を特定します。必須。 必須。|
|MultiValueExtendedProperties|コレクション ([MultiValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#MultiValueLegacyExtendedPropertyResource_v2)) | 1 つ以上の複数値を持つ拡張プロパティの配列。|
|PropertyId|文字列|**MultiValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。複数値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|Collection(string)|**MultiValueExtendedProperties** コレクションの各プロパティに対し、そのプロパティの値を指定します。必須。 必須。|


 
**要求のサンプル:**

The first example creates one single-value extended property for the specified message. That extended property is the only element in the **SingleValueExtendedProperties** array. The request body includes the following for the extended property:
- PropertyId はプロパティの型に 、GUID、および  という名前のプロパティを指定します。
- **Value** specifies `Green` as the value of the `Color` property.

```
PATCH https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2_bs88AACHsLqWAAA=')

Content-Type: application/json

{
  "SingleValueExtendedProperties": [
      {
         "PropertyId":"String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color",
         "Value":"Green"
      }
    ]
}
```

**応答のサンプル**

[メッセージの更新](..\api\mail-rest-operations.md#UpdateAMessage)からの応答と同様に、HTTP 200 OK`HTTP 200 OK` 応答コードによって正常な応答が示され、応答の本文に指定したメッセージが含まれています。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include the newly created extended property.


**要求のサンプル:**

2 番目の例では、指定したメッセージに対して1 つの複数値の拡張プロパティを作成します。そのプロパティは、**MultiValueExtendedProperties** 配列の唯一の要素です。要求本文には、次のものが含まれます。
- **PropertyId** 指定された GUID と名前 Recreation`Palette` の文字列の配列としてプロパティを指定します。 
- **値** 3 つの文字列値 ["Food", "Hiking", "Swimming"]`Palette` の配列として Recreation`["Green", "Aqua", "Blue"]` を指定します。

```
PATCH https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2_as77AACHsLrBBBA=')

Content-Type: application/json

{
  "MultiValueExtendedProperties": [
      {
         "PropertyId":"StringArray {66f5a359-4659-4830-9070-00049ec6ac6e} Name Palette",
         "Value":["Green", "Aqua", "Blue"]
      }
    ]
}
```

**応答のサンプル**

[メッセージの更新](..\api\mail-rest-operations.md#UpdateAMessage)からの応答と同様に、HTTP 200 OK`HTTP 200 OK` 応答コードによって正常な応答が示され、応答の本文に指定したメッセージが含まれています。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include the newly created extended property.

新しく作成された拡張プロパティを表示するには、[拡張プロパティを使用して展開されているメッセージを取得](#GetExpandedExtendedProperty)します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




****

<a name="CreateExtendedPropertyInNewItem)"></a>

### 新しいアイテムの拡張プロパティを作成する

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

同一の POST 呼び出しで[サポートされるリソース](#SupportedResources)の新しいインスタンスを作成しながら、1 つ以上の拡張プロパティを作成します。POST 要求の本文に、拡張プロパティが含まれます。


```no-highlight
POST https://outlook.office.com/api/beta/me/messages
POST https://outlook.office.com/api/beta/me/events
POST https://outlook.office.com/api/beta/me/contacts
POST https://outlook.office.com/api/beta/me/mailfolders
POST https://outlook.office.com/api/beta/me/calendars
POST https://outlook.office.com/api/beta/me/contactfolders
```

__**最低限必要な範囲**: 次のいずれかのうち、対象リソースに対応する読み取り/書き込み範囲:__
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_Body parameters_|
|SingleValueExtendedProperties|コレクション ([SingleValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#SingleValueLegacyExtendedPropertyResource))| 1 つ以上の単一値を持つ拡張プロパティの配列。 |
|PropertyId|文字列|**SingleValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。単一値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|string|**SingleValueExtendedProperties** コレクションの各プロパティに対し、プロパティの値を特定します。必須。 必須。|
|MultiValueExtendedProperties|コレクション ([MultiValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#MultiValueLegacyExtendedPropertyResource)) | 1 つ以上の複数値を持つ拡張プロパティの配列。|
|PropertyId|文字列|**MultiValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。複数値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|Collection(string)|**MultiValueExtendedProperties** コレクションの各プロパティに対し、そのプロパティの値を指定します。必須。 必須。|


 
**要求のサンプル**

The first example creates a new event and a single-value extended property. 最初の例では、新しいイベントと単一値の拡張プロパティを作成します。新しいイベントに対して通常含めるプロパティとは別に、1 つの単一値の拡張プロパティを含む **SingleValueExtendedProperties** を要求の本文に含め、プロパティに対しては次のようにします。
- PropertyId はプロパティの型に 、GUID、および  という名前のプロパティを指定します。
- **Value** specifies `Food` as the value of the `Fun` property.

```
POST https://outlook.office.com/api/beta/me/events

Content-Type: application/json

{
  "Subject": "Celebrate Thanksgiving",
  "Body": {
    "ContentType": "HTML",
    "Content": "Let's get together!"
  },
  "Start": {
      "DateTime": "2015-11-26T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2015-11-26T23:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "Terrie@contoso.com",
        "Name": "Terrie Barrera"
      },
      "Type": "Required"
    }
  ],
  "SingleValueExtendedProperties": [
     {
           "PropertyId":"String {66f5a359-4659-4830-9070-00040ec6ac6e} Name Fun",
           "Value":"Food"
     }
  ]
}
```

**応答のサンプル**

[イベントのみの作成](..\api\calendar-rest-operations.md#Createevents)からの応答と同様に、HTTP 201 Created`HTTP 201 Created` 応答コードによって正常な応答が示され、応答の本文に新しいイベントが含まれます。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include any newly created extended properties.

新しく作成された拡張プロパティを表示するには、[拡張プロパティを使用して展開されているアイテムを取得](#GetExpandedExtendedProperty)します。


**要求のサンプル:**

The second example creates a multi-value extended property in a new event all in the same POST operation. Apart from the properties you'd normally include for a new event, the request body includes the **MultiValueExtendedProperties** array that contains one extended property. The request body includes the following for that multi-value extended property:
- **PropertyId** 指定された GUID と名前 Recreation`Recreation` の文字列の配列としてプロパティを指定します。 
- **値** 3 つの文字列値 ["Food", "Hiking", "Swimming"]`Recreation` の配列として Recreation`["Food", "Hiking", "Swimming"]` を指定します。

```
POST https://outlook.office.com/api/beta/me/events

Content-Type: application/json

{
  "Subject": "Family reunion",
  "Body": {
    "ContentType": "HTML",
    "Content": "Let's get together this Thanksgiving!"
  },
  "Start": {
      "DateTime": "2015-11-26T09:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2015-11-29T21:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "Terrie@contoso.com",
        "Name": "Terrie Barrera"
      },
      "Type": "Required"
    },
    {
      "EmailAddress": {
        "Address": "Lauren@contoso.com",
        "Name": "Lauren Solis"
      },
      "Type": "Required"
    }
  ],
  "MultiValueExtendedProperties": [
     {
           "PropertyId":"StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation",
           "Value": ["Food", "Hiking", "Swimming"]
     }
  ]
}
```

**応答のサンプル**

[イベントのみの作成](..\api\calendar-rest-operations.md#Createevents)からの応答と同様に、HTTP 201 Created`HTTP 201 Created` 応答コードによって正常な応答が示され、応答の本文に新しいイベントが含まれます。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include any newly created extended properties.

新しく作成された拡張プロパティを表示するには、[拡張プロパティを使用して展開されているアイテムを取得](#GetExpandedExtendedProperty)します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

同一の POST 呼び出しで[サポートされるリソース](#SupportedResources)の新しいインスタンスを作成しながら、1 つ以上の拡張プロパティを作成します。POST 要求の本文に、拡張プロパティが含まれます。


```no-highlight
POST https://outlook.office.com/api/v2.0/me/messages
POST https://outlook.office.com/api/v2.0/me/events
POST https://outlook.office.com/api/v2.0/me/contacts
POST https://outlook.office.com/api/v2.0/me/mailfolders
POST https://outlook.office.com/api/v2.0/me/calendars
POST https://outlook.office.com/api/v2.0/me/contactfolders
```

__**最低限必要な範囲**: 次のいずれかのうち、対象リソースに対応する読み取り/書き込み範囲:__
- _https://outlook.office.com/mail.readwrite_
- _https://outlook.office.com/calendars.readwrite_
- _https://outlook.office.com/contacts.readwrite_
- _wl.imap_
- _wl.calendars\_update_
- _wl.events\_create_
- _wl.contacts\_create_


|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
|_Body parameters_|
|SingleValueExtendedProperties|コレクション ([SingleValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#SingleValueLegacyExtendedPropertyResource_v2))| 1 つ以上の単一値を持つ拡張プロパティの配列。 |
|PropertyId|文字列|**SingleValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。単一値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|string|**SingleValueExtendedProperties** コレクションの各プロパティに対し、プロパティの値を特定します。必須。 必須。|
|MultiValueExtendedProperties|コレクション ([MultiValueLegacyExtendedProperty](..\api\complex-types-for-mail-contacts-calendar.md#MultiValueLegacyExtendedPropertyResource_v2)) | 1 つ以上の複数値を持つ拡張プロパティの配列。|
|PropertyId|文字列|**MultiValueExtendedProperties** コレクションの各プロパティに対してこれを指定し、プロパティを特定します。複数値の拡張プロパティに対し、[サポートされる形式](#ExtendedPropertyIdFormats)の 1 つに従う必要があります。必須。|
|値|Collection(string)|**MultiValueExtendedProperties** コレクションの各プロパティに対し、そのプロパティの値を指定します。必須。 必須。|


 
**要求のサンプル**

The first example creates a new event and a single-value extended property. 最初の例では、新しいイベントと単一値の拡張プロパティを作成します。新しいイベントに対して通常含めるプロパティとは別に、1 つの単一値の拡張プロパティを含む **SingleValueExtendedProperties** を要求の本文に含め、プロパティに対しては次のようにします。
- PropertyId はプロパティの型に 、GUID、および  という名前のプロパティを指定します。
- **Value** specifies `Food` as the value of the `Fun` property.

```
POST https://outlook.office.com/api/v2.0/me/events

Content-Type: application/json

{
  "Subject": "Celebrate Thanksgiving",
  "Body": {
    "ContentType": "HTML",
    "Content": "Let's get together!"
  },
  "Start": {
      "DateTime": "2015-11-26T18:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2015-11-26T23:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "Terrie@contoso.com",
        "Name": "Terrie Barrera"
      },
      "Type": "Required"
    }
  ],
  "SingleValueExtendedProperties": [
     {
           "PropertyId":"String {66f5a359-4659-4830-9070-00040ec6ac6e} Name Fun",
           "Value":"Food"
     }
  ]
}
```

**応答のサンプル**

[イベントのみの作成](..\api\calendar-rest-operations.md#Createevents)からの応答と同様に、HTTP 201 Created`HTTP 201 Created` 応答コードによって正常な応答が示され、応答の本文に新しいイベントが含まれます。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include any newly created extended properties.

新しく作成された拡張プロパティを表示するには、[拡張プロパティを使用して展開されているアイテムを取得](#GetExpandedExtendedProperty)します。


**要求のサンプル:**

The second example creates a multi-value extended property in a new event all in the same POST operation. Apart from the properties you'd normally include for a new event, the request body includes the **MultiValueExtendedProperties** array that contains one extended property. The request body includes the following for that multi-value extended property:
- **PropertyId** 指定された GUID と名前 Recreation`Recreation` の文字列の配列としてプロパティを指定します。 
- **値** 3 つの文字列値 ["Food", "Hiking", "Swimming"]`Recreation` の配列として Recreation`["Food", "Hiking", "Swimming"]` を指定します。

```
POST https://outlook.office.com/api/v2.0/me/events

Content-Type: application/json

{
  "Subject": "Family reunion",
  "Body": {
    "ContentType": "HTML",
    "Content": "Let's get together this Thanksgiving!"
  },
  "Start": {
      "DateTime": "2015-11-26T09:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "End": {
      "DateTime": "2015-11-29T21:00:00",
      "TimeZone": "Pacific Standard Time"
  },
  "Attendees": [
    {
      "EmailAddress": {
        "Address": "Terrie@contoso.com",
        "Name": "Terrie Barrera"
      },
      "Type": "Required"
    },
    {
      "EmailAddress": {
        "Address": "Lauren@contoso.com",
        "Name": "Lauren Solis"
      },
      "Type": "Required"
    }
  ],
  "MultiValueExtendedProperties": [
     {
           "PropertyId":"StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation",
           "Value": ["Food", "Hiking", "Swimming"]
     }
  ]
}
```

**応答のサンプル**

[イベントのみの作成](..\api\calendar-rest-operations.md#Createevents)からの応答と同様に、HTTP 201 Created`HTTP 201 Created` 応答コードによって正常な応答が示され、応答の本文に新しいイベントが含まれます。応答には、新しく作成された拡張プロパティは含まれません。 The response does not include any newly created extended properties.

新しく作成された拡張プロパティを表示するには、[拡張プロパティを使用して展開されているアイテムを取得](#GetExpandedExtendedProperty)します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->




****

<a name="GetExpandedExtendedProperty"> </a>

### 拡張プロパティを使用して拡張されたアイテムを取得する

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**PropertyId** のフィルターで指定されている拡張プロパティを使用して拡張された[サポートされるリソース](#SupportedResources)のインスタンスを取得します。 

PropertyId のフィルターで指定する文字列 {propertyId_value} は、サポートされる PropertyId の形式のいずれかに従う必要があります。フィルター文字列内のスペース文字に URL エンコードを適用していることを確認してください。 PropertyId のフィルターで指定する文字列 {propertyId_value} は、サポートされる PropertyId の形式のいずれかに従う必要があります。フィルター文字列内のスペース文字に [URL エンコード](http://www.w3schools.com/tags/ref_urlencode.asp)を適用していることを確認してください。

**単一値の拡張プロパティに一致するアイテムを展開する**

```no-highlight
GET https://outlook.office.com/api/beta/me/messages('{message_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/events('{event_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/contacts('{contact_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/mailfolders('{mailFolder_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/calendars('{calendar_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/contactfolders('{contactFolder_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')
```

**複数値の拡張プロパティに一致するアイテムを展開する**

```no-highlight
GET https://outlook.office.com/api/beta/me/messages('{message_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/events('{event_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/contacts('{contact_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/mailfolders('{mailFolder_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/calendars('{calendar_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/beta/me/contactfolders('{contactFolder_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')
```


__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_

 
**要求のサンプル:**

最初の例では、単一値の拡張プロパティを含めることによって指定されたメッセージを取得して展開します。フィルターは、PropertyId が文字列 String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color と一致する拡張プロパティを返します The filter returns the extended property that has its **PropertyId** matching the string `String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color` (with URL encoding removed here for ease of reading).

```
GET https://outlook.office.com/api/beta/me/messages('AAMkAGE1M2_bs88AACHsLqWAAA=')?$expand=SingleValueExtendedProperties($filter=PropertyId%20eq%20'String%20{66f5a359-4659-4830-9070-00047ec6ac6e}%20Name%20Color')
```


**応答のサンプル**

成功した応答は、HTTP 200 応答コードで示されます。

応答本文には、指定されたメッセージのすべてのプロパティと、フィルターから返される拡張プロパティが含まれています。

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE1M2_bs88AACHsLqWAAA=')",
    "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AACbyS4H\"",
    "Id": "AAMkAGE1M2_bs88AACHsLqWAAA=",
    "CreatedDateTime": "2015-11-11T02:41:24Z",
    "LastModifiedDateTime": "2015-12-09T04:07:57Z",
    "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AACbyS4H",
    "Categories": [
    ],
    "ReceivedDateTime": "2015-11-11T02:41:24Z",
    "SentDateTime": "2015-11-11T02:41:24Z",
    "HasAttachments": false,
    "InternetMessageId": "<SN2SR0101MB002977E7C30F9CA9AA55961484130@SN2SR0101MB0029.contoso.com>",
    "Subject": "RE: Talk about emergency prep",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=iso-8859-1\">\r\n<style type=\"text/css\" style=\"\">\r\n<!--\r\np\r\n\t{margin-top:0;\r\n\tmargin-bottom:0}\r\n-->\r\n</style>\r\n</head>\r\n<body dir=\"ltr\">\r\n<hr tabindex=\"-1\" style=\"display:inline-block; width:98%\">\r\n<div id=\"divRplyFwdMsg\" dir=\"ltr\"><font face=\"Calibri, sans-serif\" color=\"#000000\" style=\"font-size:11pt\"><b>From:</b> Christine Irwin<br>\r\n<b>Sent:</b> Sunday, November 8, 2015 12:28:31 AM<br>\r\n<b>To:</b> Terrie Barrera<br>\r\n<b>Subject:</b> Talk about emergency prep<br>\r\n<b>When:</b> Sunday, November 8, 2015 7:00 PM-8:00 PM.<br>\r\n<b>Where:</b> The Commons</font>\r\n<div>&nbsp;</div>\r\n</div>\r\n<div>\r\n<div id=\"divtagdefaultwrapper\" style=\"font-size:12pt; color:#000000; background-color:#FFFFFF; font-family:Calibri,Arial,Helvetica,sans-serif\">\r\n<p>Please see the attached before you come to the meeting.<br>\r\n</p>\r\n</div>\r\n</div>\r\n</body>\r\n</html>\r\n"
    },
    "BodyPreview": "________________________________",
    "Importance": "Normal",
    "ParentFolderId": "AQMkAGE1M2jgxCloXP1JupQN7j5uzzwAAAIBDwAAAA==",
    "Sender": {
        "EmailAddress": {
            "Name": "Christine Irwin",
            "Address": "christine@contoso.com"
        }
    },
    "From": null,
    "ToRecipients": [
        {
            "EmailAddress": {
                "Name": "Christine Irwin",
                "Address": "christine@contoso.com"
            }
        }
    ],
    "CcRecipients": [
    ],
    "BccRecipients": [
    ],
    "ReplyTo": [
    ],
    "ConversationId": "AAQkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVlLTY0YjcxZWUzNTE1MAAQAHWT1sRiMChEmlQCIZUadoU=",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsRead": true,
    "IsDraft": true,
    "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1M2%2Bbs88AACHsLqWAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
    "MentionedMe": null,
    "HashtagDetailsPreview": null,
    "LikesPreview": null,
    "Mentioned": [
    ],
    "InferenceClassification": "Focused",
    "UnsubscribeData": [
    ],
    "UnsubscribeEnabled": false,
    "SingleValueExtendedProperties@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Messages('AAMkAGE1M2_bs88AACHsLqWAAA%3D')/SingleValueExtendedProperties",
    "SingleValueExtendedProperties": [
        {
            "PropertyId": "String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color",
            "Value": "Green"
        }
    ]
}
```


**要求のサンプル**

2 番目の例では、複数値の拡張プロパティを含めることによって指定されたイベントを取得して展開します。フィルターは、PropertyId が文字列 StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation と一致する拡張プロパティを返します The filter returns the extended property that has its **PropertyId** matching the string `StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation` (with URL encoding removed here for ease of reading).

```
GET https://outlook.office.com/api/beta/me/events('AAMkAGE1M2_bs88AACbuFiiAAA=')?$expand=MultiValueExtendedProperties($filter=PropertyId%20eq%20'StringArray%20{66f5a359-4659-4830-9070-00050ec6ac6e}%20Name%20Recreation')
```


**応答のサンプル**

成功した応答は、HTTP 200 応答コードで示されます。

応答本文には、指定されたイベントのすべてのプロパティと、フィルターから返される拡張プロパティが含まれています。

```
{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events/$entity",
    "@odata.id": "https://outlook.office.com/api/beta/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGE1M2_bs88AACbuFiiAAA=')",
    "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAm8k15A==\"",
    "Id": "AAMkAGE1M2_bs88AACbuFiiAAA=",
    "CreatedDateTime": "2015-12-09T05:18:05.9477979Z",
    "LastModifiedDateTime": "2015-12-09T05:18:06.197802Z",
    "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAAm8k15A==",
    "Categories": [
    ],
    "OriginalStartTimeZone": "Pacific Standard Time",
    "OriginalEndTimeZone": "Pacific Standard Time",
    "ResponseStatus": {
        "Response": "Organizer",
        "Time": "0001-01-01T00:00:00Z"
    },
    "iCalUId": "04000000828A332796D9",
    "ReminderMinutesBeforeStart": 15,
    "IsReminderOn": true,
    "HasAttachments": false,
    "Subject": "Family reunion",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=us-ascii\">\r\n</head>\r\n<body>\r\nLet's get together this Thanksgiving!\r\n</body>\r\n</html>\r\n"
    },
    "BodyPreview": "Let's get together this Thanksgiving!",
    "Importance": "Normal",
    "Sensitivity": "Normal",
    "Start": {
        "DateTime": "2015-11-26T17:00:00.0000000",
        "TimeZone": "UTC"
    },
    "End": {
        "DateTime": "2015-11-30T05:00:00.0000000",
        "TimeZone": "UTC"
    },
    "Location": {
        "DisplayName": ""
    },
    "IsAllDay": false,
    "IsCancelled": false,
    "IsOrganizer": true,
    "Recurrence": null,
    "ResponseRequested": true,
    "SeriesMasterId": null,
    "ShowAs": "Busy",
    "Type": "SingleInstance",
    "Attendees": [
        {
            "Status": {
                "Response": "None",
                "Time": "0001-01-01T00:00:00Z"
            },
            "Type": "Required",
            "EmailAddress": {
                "Name": "Terrie Barrera",
                "Address": "Terrie@contoso.com"
            }
        },
        {
            "Status": {
                "Response": "None",
                "Time": "0001-01-01T00:00:00Z"
            },
            "Type": "Required",
            "EmailAddress": {
                "Name": "Lauren Solis",
                "Address": "Lauren@contoso.com"
            }
        }
    ],
    "Organizer": {
        "EmailAddress": {
            "Name": "Christine Irwin",
            "Address": "christine@contoso.com"
        }
    },
    "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1M2%2Bbs88AACbuFiiAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory",
    "MultiValueExtendedProperties@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events('AAMkAGE1M2_bs88AACbuFiiAAA%3D')/MultiValueExtendedProperties",
    "MultiValueExtendedProperties": [
        {
            "PropertyId": "StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation",
            "Value": [
                "Food",
                "Hiking",
                "Swimming"
            ]
        }
    ]
}
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**PropertyId** のフィルターで指定されている拡張プロパティを使用して拡張された[サポートされるリソース](#SupportedResources)のインスタンスを取得します。 

PropertyId のフィルターで指定する文字列 {propertyId_value} は、サポートされる PropertyId の形式のいずれかに従う必要があります。フィルター文字列内のスペース文字に URL エンコードを適用していることを確認してください。 PropertyId のフィルターで指定する文字列 {propertyId_value} は、サポートされる PropertyId の形式のいずれかに従う必要があります。フィルター文字列内のスペース文字に [URL エンコード](http://www.w3schools.com/tags/ref_urlencode.asp)を適用していることを確認してください。

**単一値の拡張プロパティに一致するアイテムを展開する**

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages('{message_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/events('{event_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/mailfolders('{mailFolder_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/calendars('{calendar_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/contactfolders('{contactFolder_id}')?$expand=SingleValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')
```

**複数値の拡張プロパティに一致するアイテムを展開する**

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages('{message_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/events('{event_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/contacts('{contact_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/mailfolders('{mailFolder_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/calendars('{calendar_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')

GET https://outlook.office.com/api/v2.0/me/contactfolders('{contactFolder_id}')?$expand=MultiValueExtendedProperties($filter=PropertyId eq '{propertyId_value}')
```


__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_

 
**要求のサンプル:**

最初の例では、単一値の拡張プロパティを含めることによって指定されたメッセージを取得して展開します。フィルターは、PropertyId が文字列 String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color と一致する拡張プロパティを返します The filter returns the extended property that has its **PropertyId** matching the string `String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color` (with URL encoding removed here for ease of reading).

```
GET https://outlook.office.com/api/v2.0/me/messages('AAMkAGE1M2_bs88AACHsLqWAAA=')?$expand=SingleValueExtendedProperties($filter=PropertyId%20eq%20'String%20{66f5a359-4659-4830-9070-00047ec6ac6e}%20Name%20Color')
```


**応答のサンプル**

成功した応答は、HTTP 200 応答コードで示されます。

応答本文には、指定されたメッセージのすべてのプロパティと、フィルターから返される拡張プロパティが含まれています。

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Messages('AAMkAGE1M2_bs88AACHsLqWAAA=')",
    "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AACbyS4H\"",
    "Id": "AAMkAGE1M2_bs88AACHsLqWAAA=",
    "CreatedDateTime": "2015-11-11T02:41:24Z",
    "LastModifiedDateTime": "2015-12-09T04:07:57Z",
    "ChangeKey": "CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AACbyS4H",
    "Categories": [
    ],
    "ReceivedDateTime": "2015-11-11T02:41:24Z",
    "SentDateTime": "2015-11-11T02:41:24Z",
    "HasAttachments": false,
    "InternetMessageId": "<SN2SR0101MB002977E7C30F9CA9AA55961484130@SN2SR0101MB0029.contoso.com>",
    "Subject": "RE: Talk about emergency prep",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=iso-8859-1\">\r\n<style type=\"text/css\" style=\"\">\r\n<!--\r\np\r\n\t{margin-top:0;\r\n\tmargin-bottom:0}\r\n-->\r\n</style>\r\n</head>\r\n<body dir=\"ltr\">\r\n<hr tabindex=\"-1\" style=\"display:inline-block; width:98%\">\r\n<div id=\"divRplyFwdMsg\" dir=\"ltr\"><font face=\"Calibri, sans-serif\" color=\"#000000\" style=\"font-size:11pt\"><b>From:</b> Christine Irwin<br>\r\n<b>Sent:</b> Sunday, November 8, 2015 12:28:31 AM<br>\r\n<b>To:</b> Terrie Barrera<br>\r\n<b>Subject:</b> Talk about emergency prep<br>\r\n<b>When:</b> Sunday, November 8, 2015 7:00 PM-8:00 PM.<br>\r\n<b>Where:</b> The Commons</font>\r\n<div>&nbsp;</div>\r\n</div>\r\n<div>\r\n<div id=\"divtagdefaultwrapper\" style=\"font-size:12pt; color:#000000; background-color:#FFFFFF; font-family:Calibri,Arial,Helvetica,sans-serif\">\r\n<p>Please see the attached before you come to the meeting.<br>\r\n</p>\r\n</div>\r\n</div>\r\n</body>\r\n</html>\r\n"
    },
    "BodyPreview": "________________________________",
    "Importance": "Normal",
    "ParentFolderId": "AQMkAGE1M2jgxCloXP1JupQN7j5uzzwAAAIBDwAAAA==",
    "Sender": {
        "EmailAddress": {
            "Name": "Christine Irwin",
            "Address": "christine@contoso.com"
        }
    },
    "From": null,
    "ToRecipients": [
        {
            "EmailAddress": {
                "Name": "Christine Irwin",
                "Address": "christine@contoso.com"
            }
        }
    ],
    "CcRecipients": [
    ],
    "BccRecipients": [
    ],
    "ReplyTo": [
    ],
    "ConversationId": "AAQkAGE1M2IyNGNmLTI5MTktNDUyZi1iOTVlLTY0YjcxZWUzNTE1MAAQAHWT1sRiMChEmlQCIZUadoU=",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsRead": true,
    "IsDraft": true,
    "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1M2%2Bbs88AACHsLqWAAA%3D&exvsurl=1&viewmodel=ReadMessageItem",
    "MentionedMe": null,
    "HashtagDetailsPreview": null,
    "LikesPreview": null,
    "Mentioned": [
    ],
    "InferenceClassification": "Focused",
    "UnsubscribeData": [
    ],
    "UnsubscribeEnabled": false,
    "SingleValueExtendedProperties@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Messages('AAMkAGE1M2_bs88AACHsLqWAAA%3D')/SingleValueExtendedProperties",
    "SingleValueExtendedProperties": [
        {
            "PropertyId": "String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color",
            "Value": "Green"
        }
    ]
}
```


**要求のサンプル**

2 番目の例では、複数値の拡張プロパティを含めることによって指定されたイベントを取得して展開します。フィルターは、PropertyId が文字列 StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation と一致する拡張プロパティを返します The filter returns the extended property that has its **PropertyId** matching the string `StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation` (with URL encoding removed here for ease of reading).

```
GET https://outlook.office.com/api/v2.0/me/events('AAMkAGE1M2_bs88AACbuFiiAAA=')?$expand=MultiValueExtendedProperties($filter=PropertyId%20eq%20'StringArray%20{66f5a359-4659-4830-9070-00050ec6ac6e}%20Name%20Recreation')
```


**応答のサンプル**

成功した応答は、HTTP 200 応答コードで示されます。

応答本文には、指定されたイベントのすべてのプロパティと、フィルターから返される拡張プロパティが含まれています。

```
{
    "@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Events/$entity",
    "@odata.id": "https://outlook.office.com/api/v2.0/Users('ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77')/Events('AAMkAGE1M2_bs88AACbuFiiAAA=')",
    "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAm8k15A==\"",
    "Id": "AAMkAGE1M2_bs88AACbuFiiAAA=",
    "CreatedDateTime": "2015-12-09T05:18:05.9477979Z",
    "LastModifiedDateTime": "2015-12-09T05:18:06.197802Z",
    "ChangeKey": "mODEKWhc/Um6lA3uPm7PPAAAm8k15A==",
    "Categories": [
    ],
    "OriginalStartTimeZone": "Pacific Standard Time",
    "OriginalEndTimeZone": "Pacific Standard Time",
    "ResponseStatus": {
        "Response": "Organizer",
        "Time": "0001-01-01T00:00:00Z"
    },
    "iCalUId": "04000000828A332796D9",
    "ReminderMinutesBeforeStart": 15,
    "IsReminderOn": true,
    "HasAttachments": false,
    "Subject": "Family reunion",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=us-ascii\">\r\n</head>\r\n<body>\r\nLet's get together this Thanksgiving!\r\n</body>\r\n</html>\r\n"
    },
    "BodyPreview": "Let's get together this Thanksgiving!",
    "Importance": "Normal",
    "Sensitivity": "Normal",
    "Start": {
        "DateTime": "2015-11-26T17:00:00.0000000",
        "TimeZone": "UTC"
    },
    "End": {
        "DateTime": "2015-11-30T05:00:00.0000000",
        "TimeZone": "UTC"
    },
    "Location": {
        "DisplayName": ""
    },
    "IsAllDay": false,
    "IsCancelled": false,
    "IsOrganizer": true,
    "Recurrence": null,
    "ResponseRequested": true,
    "SeriesMasterId": null,
    "ShowAs": "Busy",
    "Type": "SingleInstance",
    "Attendees": [
        {
            "Status": {
                "Response": "None",
                "Time": "0001-01-01T00:00:00Z"
            },
            "Type": "Required",
            "EmailAddress": {
                "Name": "Terrie Barrera",
                "Address": "Terrie@contoso.com"
            }
        },
        {
            "Status": {
                "Response": "None",
                "Time": "0001-01-01T00:00:00Z"
            },
            "Type": "Required",
            "EmailAddress": {
                "Name": "Lauren Solis",
                "Address": "Lauren@contoso.com"
            }
        }
    ],
    "Organizer": {
        "EmailAddress": {
            "Name": "Christine Irwin",
            "Address": "christine@contoso.com"
        }
    },
    "WebLink": "https://outlook.office.com/owa/?ItemID=AAMkAGE1M2%2Bbs88AACbuFiiAAA%3D&exvsurl=1&viewmodel=ICalendarItemDetailsViewModelFactory",
    "MultiValueExtendedProperties@odata.context": "https://outlook.office.com/api/v2.0/$metadata#Me/Events('AAMkAGE1M2_bs88AACbuFiiAAA%3D')/MultiValueExtendedProperties",
    "MultiValueExtendedProperties": [
        {
            "PropertyId": "StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation",
            "Value": [
                "Food",
                "Hiking",
                "Swimming"
            ]
        }
    ]
}
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->





****

<a name="GetItemByFilteringExtendedProperty"> </a>

### 拡張プロパティにフィルターを使用してアイテムを取得する

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Get instances of a [supported resource](#SupportedResources) that have the extended property specified by a filter on **PropertyId** and **Value**. The filter is applied to all instances of the resource in the signed-in user's mailbox. This filtering supports only single-value but not multi-value extended properties.

PropertyId のフィルターで指定する文字列 {propertyId_value} は、サポートされる PropertyId の形式のいずれかに従う必要があります。フィルター文字列内のスペース文字に URL エンコードを適用していることを確認してください。 Make sure you apply [URL encoding](http://www.w3schools.com/tags/ref_urlencode.asp) to the following characters in the filter string: forward slash and space. 

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/beta/me/events?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/beta/me/contacts?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/beta/me/mailfolders?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/beta/me/calendars?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/beta/me/contactfolders?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')
```


__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_

 
**要求のサンプル:**

最初の例では、フィルターで指定された単一値の拡張プロパティを持つメッセージを取得します。フィルターは、次のものを持つ拡張のプロパティを返します。 The filter returns the extended property that has:
- Its **PropertyId** matching the string `String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color` (with URL encoding removed here for ease of reading).
- Its **Value** being `Green`.

```
GET https://outlook.office.com/api/beta/Me/Messages?$filter=SingleValueExtendedProperties%2FAny(ep%3A%20ep%2FPropertyId%20eq%20'String%20{66f5a359-4659-4830-9070-00047ec6ac6e}%20Name%20Color'%20and%20ep%2FValue%20eq%20'Green')
```


**応答のサンプル**

正常な応答が HTTP 200 OK`HTTP 200 OK` 応答コードによって示され、応答の本文には、フィルターに一致する拡張プロパティを持つメッセージのすべてのプロパティが含まれます。応答本文は、メッセージのコレクションの取得からの応答に似ています。応答は、一致する拡張プロパティを含みません。 The response body is similar to the response from [getting a message collection](..\api\mail-rest-operations.md#GetMessages). The response does not include the matching extended property.

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Get instances of a [supported resource](#SupportedResources) that have the extended property specified by a filter on **PropertyId** and **Value**. The filter is applied to all instances of the resource in the signed-in user's mailbox. This filtering supports only single-value but not multi-value extended properties.

PropertyId のフィルターで指定する文字列 {propertyId_value} は、サポートされる PropertyId の形式のいずれかに従う必要があります。フィルター文字列内のスペース文字に URL エンコードを適用していることを確認してください。 Make sure you apply [URL encoding](http://www.w3schools.com/tags/ref_urlencode.asp) to the following characters in the filter string: forward slash and space. 

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/v2.0/me/events?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/v2.0/me/contacts?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/v2.0/me/mailfolders?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/v2.0/me/calendars?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')

GET https://outlook.office.com/api/v2.0/me/contactfolders?$filter=SingleValueExtendedProperties/Any(ep: ep/PropertyId eq '{propertyId_value}' and ep/Value eq '{property_value}')
```


__**最低限必要な範囲**: 次のいずれかのうち、対象となるリソースに対応する読み取り範囲:__
- _https://outlook.office.com/mail.read_
- _https://outlook.office.com/calendars.read_
- _https://outlook.office.com/contacts.read_
- _wl.imap_
- _wl.calendars_
- _wl.contacts\_calendars_
- _wl.basic_

 
**要求のサンプル:**

最初の例では、フィルターで指定された単一値の拡張プロパティを持つメッセージを取得します。フィルターは、次のものを持つ拡張のプロパティを返します。 The filter returns the extended property that has:
- Its **PropertyId** matching the string `String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color` (with URL encoding removed here for ease of reading).
- Its **Value** being `Green`.

```
GET https://outlook.office.com/api/v2.0/Me/Messages?$filter=SingleValueExtendedProperties%2FAny(ep%3A%20ep%2FPropertyId%20eq%20'String%20{66f5a359-4659-4830-9070-00047ec6ac6e}%20Name%20Color'%20and%20ep%2FValue%20eq%20'Green')
```


**応答のサンプル**

正常な応答が HTTP 200 OK`HTTP 200 OK` 応答コードによって示され、応答の本文には、フィルターに一致する拡張プロパティを持つメッセージのすべてのプロパティが含まれます。応答本文は、メッセージのコレクションの取得からの応答に似ています。応答は、一致する拡張プロパティを含みません。 The response body is similar to the response from [getting a message collection](..\api\mail-rest-operations.md#GetMessages). The response does not include the matching extended property.

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->





****


<a name="NextSteps"> </a>
## 次のステップ

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](https://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [メール REST API リファレンス](..\api\mail-rest-operations.md)

- [予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API](..\api\task-rest-operations.md) (プレビュー)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)

- [People API リファレンス](..\api\people-rest-operations.md) (プレビュー)

- [データ拡張機能 API リファレンス](..\api\extensions-rest-operations.md)

- [通知 REST API リファレンス](..\api\notify-rest-operations.md)

- [ユーザー写真 REST API リファレンス](..\api\photo-rest-operations.md) 

- バッチ Outlook REST 要求 (プレビュー)



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](https://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [メール REST API リファレンス](..\api\mail-rest-operations.md)

- [予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API](..\api\task-rest-operations.md) (プレビュー)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)

- [People API リファレンス](..\api\people-rest-operations.md) (プレビュー)

- [データ拡張機能 API リファレンス](..\api\extensions-rest-operations.md)

- [通知 REST API リファレンス](..\api\notify-rest-operations.md)

- [ユーザー写真 REST API リファレンス](..\api\photo-rest-operations.md) 

- バッチ Outlook REST 要求 (プレビュー)



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]



