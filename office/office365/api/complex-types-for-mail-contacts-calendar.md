---
ms.Toctitle: Resource reference for the Mail, Calendar, Contacts and Task REST APIs
title: "メール、予定表、連絡先、タスク REST API のリソース リファレンス"
description: "メール、予定表、連絡先、タスク REST API、さらには OData クエリを使用した要求のフィルター処理や結果の並べ替えとページングのための、リファレンス リソースに関する説明の一覧です。"
ms.ContentId: 061d80ce-2c24-4256-bc0e-bcf65d99635a
ms.date: August 4, 2016

---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# メール、予定表、連絡先、タスク REST API のリソース リファレンス

[!INCLUDE [Add the Outlook REST API filters--v2 default](../includes/controls/addOutlookVersion_v2default.xml)]

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<p class="previewnote">このドキュメントでは、会議の提案、自動応答、クイック応答、参照添付ファイル、タスク、"(プレビュー)" とマークされているタイム ゾーンの新しいリソースと変更されたリソースについて説明します。 プレビュー機能は、最終版までに変更される場合があり、それらの機能を使用するコードが動作しなくなる場合もあります。 このため、一般に、運用コードでは運用バージョンの API のみを使用してください。 入手可能な場合、現時点ではバージョン 2.0 が優先バージョンです。</p>

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


 _**適用対象:**Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

この記事では、REST API エンティティ、プロパティ、複合型、列挙型、OData クエリ パラメーターについて説明します。これらを、Outlook [メール](..\api\mail-rest-operations.md)、[予定表](..\api\calendar-rest-operations.md)、[連絡先](..\api\contacts-rest-operations.md)、[タスク](..\api\task-rest-operations.md) の API で使用すると、Office 365、Hotmail.com、Live.com、MSN.com、Outlook.com、Passport.com のユーザー メールボックス データにアクセスできます。

**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。 

Outlook REST API のすべてのサブセットに共通な情報の詳細については、「[Outlook REST API の使用](..\api\use-outlook-rest-api.md)」を参照してください。

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



<a name="Resources"> </a>
## REST API リソース  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


**ヒント**  Web ブラウザー (`https://outlook.office.com/api/beta/$metadata` など) で `$metadata` エンドポイントに移動することによって、メール、予定表、連絡先、およびタスク エンティティ データ モデルに関するメタデータ ドキュメントのすべてを表示できます。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


**ヒント**  Web ブラウザー (`https://outlook.office.com/api/v2.0/$metadata` など) で `$metadata` エンドポイントに移動することによって、メール、予定表、連絡先、およびタスク エンティティ データ モデルに関するメタデータ ドキュメントのすべてを表示できます。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

**ヒント**  Web ブラウザー (`https://outlook.office.com/api/v1.0/$metadata` など) で `$metadata` エンドポイントに移動することによって、メール、予定表、連絡先、およびタスク エンティティ データ モデルに関するメタデータ ドキュメントのすべてを表示できます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="Attachment"></a>
### 添付ファイル  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

ファイル、アイテム (連絡先、イベント、メッセージ)、またはファイルあるいはフォルダーへのリンク。[イベント](#EventResource)や[メッセージ](#MessageResource)に添付されるものです。    
対応する [fileAttachment](#FileAttachmentResource)、[itemAttachment](#ItemAttachmentResource)、[referenceAttachment](#ReferenceAttachmentResource) リソースは、いずれも **Attachment** リソースから派生します。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

[イベント](#EventResource)または[メッセージ](#MessageResource)に添付されるファイルまたはアイテム (連絡先、イベント、メッセージ)。    
対応する [fileAttachment](#FileAttachmentResource)、[itemAttachment](#ItemAttachmentResource) リソースは、**Attachment** リソースから派生します。

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

[イベント](#EventResource)または[メッセージ](#MessageResource)に添付されるファイルまたはアイテム (連絡先、イベント、メッセージ)。    
対応する [fileAttachment](#FileAttachmentResource)、[itemAttachment](#ItemAttachmentResource) リソースは、**Attachment** リソースから派生します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




型:**Microsoft.OutlookServices.Entity**

|プロパティ|型|説明|書き込み可能|フィルター処理可能
|:-----|:-----|:-----|:-----|:-----|
| ContentType  | String | 添付ファイルの MIME タイプ。 | はい | いいえ |
| IsInline | ブール型 | 添付ファイルがインライン添付ファイルの場合は `true`、それ以外の場合は `false`。 | はい | はい |
| LastModifiedDateTime | DateTimeOffset | 添付ファイルが最後に変更された日時。Timestamp 型は、ISO 8601 形式を使用して日付と時刻の情報を表し、必ず UTC 時間です。たとえば、2014 年 1 月 1 日午前 0 時 (UTC) は、次のようになります。'2014-01-01T00:00:00Z' | いいえ | はい |
| 名前 | String | 添付ファイルの表示名。実際のファイル名である必要はありません。 | はい | はい |
| Size | Int32 | 添付ファイルの長さ (バイト単位)。 | いいえ | いいえ |


<a name="CalendarResource"> </a>
### 予定表

イベントのコンテナーである予定表です。

型:**Microsoft.OutlookServices.Calendar**

Calendar コレクションは、OData 応答の **value** プロパティで予定表の配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/calendars/$count`) を取得します。

サポートされているアクションについては、「[Calendar の操作](..\api\calendar-rest-operations.md#CalendarOperations)」を参照してください。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

|プロパティ|型|説明|書き込み可能|フィルター処理可能
|:-----|:-----|:-----|:-----|:-----|
|名前| String|予定表の名前。|はい|はい|
|ChangeKey| String|予定表オブジェクトのバージョンを識別します。予定表を変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|Color|CalendarColor|UI で予定表を他の予定表から区別するための配色テーマを指定します。プロパティ値は次のとおりです。薄い青=0、薄い緑=1、薄いオレンジ=2、薄い灰色=3、薄い黄=4、薄い青緑=5、薄いピンク=6、薄い茶色=7、薄い赤=8、最大色=9、自動=-1 |はい|はい|
|IsDefaultCalendar|ブール型 (Boolean)|この予定表がユーザーの既定の予定表であれば True、そうでなければ False。|はい|はい|
|Id| String|予定表の一意の識別子。|いいえ|いいえ|
|CalendarView| Collection([Event](#EventResource))|予定表のカレンダー ビュー。 ナビゲーション プロパティです。|いいえ|いいえ|
|Events| Collection([Event](#EventResource))|予定表内のイベント。 ナビゲーション プロパティです。|いいえ|いいえ|
|MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい | 
|SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

|プロパティ|型|説明|書き込み可能|フィルター処理可能
|:-----|:-----|:-----|:-----|:-----|
|名前| String|予定表の名前。|はい|はい|
|ChangeKey| String|予定表オブジェクトのバージョンを識別します。予定表を変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|Color|CalendarColor|UI で予定表を他の予定表から区別するための配色テーマを指定します。プロパティ値は次のとおりです。薄い青=0、薄い緑=1、薄いオレンジ=2、薄い灰色=3、薄い黄=4、薄い青緑=5、薄いピンク=6、薄い茶色=7、薄い赤=8、最大色=9、自動=-1 |はい|はい|
|Id| String|予定表の一意の識別子。|いいえ|いいえ|
|CalendarView| Collection([Event](#EventResource))|予定表のカレンダー ビュー。 ナビゲーション プロパティです。|いいえ|いいえ|
|Events| Collection([Event](#EventResource))|予定表内のイベント。 ナビゲーション プロパティです。|いいえ|いいえ|
|MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource_v2) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい | 
|SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource_v2) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|プロパティ|型|説明|書き込み可能|フィルター処理可能
|:-----|:-----|:-----|:-----|:-----|
|名前| String|予定表の名前。|はい|はい|
|ChangeKey| String|予定表オブジェクトのバージョンを識別します。予定表を変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|Color|CalendarColor|UI で予定表を他の予定表から区別するための配色テーマを指定します。プロパティ値は次のとおりです。薄い青=0、薄い緑=1、薄いオレンジ=2、薄い灰色=3、薄い黄=4、薄い青緑=5、薄いピンク=6、薄い茶色=7、薄い赤=8、最大色=9、自動=-1 |はい|はい|
|Id| String|予定表の一意の識別子。|いいえ|いいえ|
|CalendarView| Collection([Event](#EventResource))|予定表のカレンダー ビュー。 ナビゲーション プロパティです。|いいえ|いいえ|
|Events| Collection([Event](#EventResource))|予定表内のイベント。 ナビゲーション プロパティです。|いいえ|いいえ|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="CalendarGroupResource"> </a>
### CalendarGroup

予定表のグループです。

**注** Outlook.com によってサポートされるのは、`../me/calendars` ショートカットでアクセス可能な既定の予定表グループのみです。 その予定表グループを削除することはできません。 

型:**Microsoft.OutlookServices.CalendarGroup**

CalendarGroup コレクションは、OData 応答の **value** プロパティで予定表グループの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/calendargroups/$count`) を取得します。

サポートされているアクションについては、「[CalendarGroup の操作](..\api\calendar-rest-operations.md#CalendarGroupOperations)」を参照してください。

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
|名前| String|グループの名前。|はい|はい|
|ChangeKey|String|予定表グループのバージョンを識別します。予定表グループを変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|ClassId|String|クラス識別子。|いいえ|はい|
|Id|String|予定表グループの一意の識別子。|いいえ|いいえ|
|Calendars|Collection([Calendar](#CalendarResource))|予定表グループ内の予定表。 ナビゲーション プロパティです。|いいえ|いいえ|




<a name="ContactResource"> </a>
### Contact

連絡先。ユーザーが通信する人や組織に関する情報を編成および保存するために使用する、Outlook 内のアイテムです。連絡先は連絡先フォルダーに格納されます。

型:**Microsoft.OutlookServices.Contact**

Contact コレクションは、OData 応答の **value** プロパティで連絡先の配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/contacts/$count`) を取得します。

サポートされているアクションについては、「[Contact の操作](..\api\contacts-rest-operations.md#ContactOperations)」を参照してください。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**
|:-----|:-----|:-----|:-----|:-----|
|AssistantName| String|連絡先のアシスタントの名前。|はい|はい|
|Birthday| datetimeoffset|連絡先の誕生日です。|はい|はい|
|カテゴリ| Collection(String)|連絡先に関連付けられたカテゴリ。|はい|いいえ|
|ChangeKey| String|連絡先のバージョンを識別します。連絡先を変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|Children|Collection(String)|連絡先の子供の名前。 |はい|はい|
|CompanyName| String|連絡先の会社の名前。|はい|はい|
|CreatedDateTime| datetimeoffset|連絡先が作成された時刻です。|いいえ|はい|
|Department| String|連絡先の部署。|はい|はい|
|DisplayName| String|連絡先の表示名。|はい|はい|
|EmailAddresses| Collection([EmailAddress](#EmailAddress))|連絡先のメール アドレス。|はい|いいえ|
|Extensions| Collection(Extension) |連絡先に対して定義されているオープン型のデータ拡張機能のコレクション。ナビゲーション プロパティです。 |いいえ |はい |
|FileAs| String|連絡先がファイルされる名前。|はい|はい|
|フラグ| [FollowupFlag](#FollowupFlag) | 連絡先の任意のフォローアップに関する情報。 | はい | はい |
|性別 |String |連絡先の性別。 |はい|はい|
|Generation|String|連絡先の世代。|はい|はい|
|GivenName| String|連絡先の名。|はい|はい|
|Id| String|連絡先の一意の識別子。|いいえ|いいえ|
|ImAddresses| Collection(String)|連絡先のインスタント メッセージング (IM) アドレス。|はい|いいえ|
|Initials| String|連絡先のイニシャル。|はい|はい|
|JobTitle| String|連絡先の役職。|はい|はい|
|LastModifiedDateTime| datetimeoffset|連絡先が変更された時刻です。|いいえ|はい|
|Manager| String|連絡先の上司の名前。|はい|はい|
|MiddleName| String|連絡先のミドル ネーム。|はい|はい|
|NickName| String|連絡先のニックネーム。|はい|はい|
|OfficeLocation|String|連絡先のオフィスの所在地。|はい|はい|
|ParentFolderId|String|連絡先の親フォルダーの ID。|いいえ|いいえ|
|PersonalNotes|String|連絡先に関するユーザーのメモ。|はい|はい|
|電話 |コレクション ([電話](#Phone)) |自宅電話、携帯電話、勤務先電話など、連絡先に関連付けられた電話番号。|はい|はい|
|PostalAddresses |[PhysicalAddress](PhysicalAddress.md) コレクション |自宅住所や勤務先住所など、連絡先に関連付けられた住所。 |はい|いいえ|
|Profession|String|連絡先の専門的職業。|はい|はい|
|SpouseName|String|連絡先の配偶者の名前。|はい|はい|
|Surname| String|連絡先の姓。|はい|はい|
|Title|String|連絡先の肩書。|はい|いいえ|
|Web サイト |コレクション ([Web サイト](#Website))| 連絡先に関連付けられた Web サイト。|はい|いいえ|
|WeddingAnniversary |日付 |連絡先の結婚記念日。 |はい|はい|
|YomiCompanyName|String|連絡先の会社名の読み仮名。このプロパティは省略可能です。|はい|いいえ|
|YomiGivenName|String|連絡先の名 (ファースト ネーム) の読み仮名。このプロパティは省略可能です。|はい|いいえ|
|YomiSurname|String|連絡先の姓 (ラスト ネーム) の読み仮名。このプロパティは省略可能です。|はい|いいえ|

ナビゲーション プロパティ **MultiValueExtendedProperties** と **SingleValueExtendedProperties** も、このリソースで利用できます。これらは、リソース インスタンスに定義されている拡張プロパティのそれぞれの型のコレクションを表します。詳細については、「[拡張プロパティ REST API](../api/extended-properties-rest-operations.md)」を参照してください。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**
|:-----|:-----|:-----|:-----|:-----|
|AssistantName| String|連絡先のアシスタントの名前。|はい|はい|
|Birthday| datetimeoffset|連絡先の誕生日です。|はい|はい|
|BusinessAddress| [PhysicalAddress](#PhysicalAddress)|連絡先の勤務先の住所。|はい|はい|
|BusinessHomePage| String|連絡先の勤務先のホーム ページ。|はい|はい|
|BusinessPhones| Collection(String)|連絡先の勤務先の電話番号。|はい|いいえ|
|Categories| Collection(String)|連絡先に関連付けられたカテゴリ。|はい|いいえ|
|ChangeKey| String|連絡先のバージョンを識別します。連絡先を変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|Children|Collection(String)|連絡先の子供の名前。 |はい|はい|
|CompanyName| String|連絡先の会社の名前。|はい|はい|
|Department| String|連絡先の部署。|はい|はい|
|CreatedDateTime| datetimeoffset|連絡先が作成された時刻です。|いいえ|はい|
|LastModifiedDateTime| datetimeoffset|連絡先が変更された時刻です。|いいえ|はい|
|DisplayName| String|連絡先の表示名。|はい|はい|
|EmailAddresses| Collection([EmailAddress](#EmailAddress))|連絡先のメール アドレス。|はい|いいえ|
|Extensions| Collection(Extension) |連絡先に対して定義されているオープン型のデータ拡張機能のコレクション。ナビゲーション プロパティです。 |いいえ |はい |
|FileAs| String|連絡先がファイルされる名前。|はい|はい|
|Generation|String|連絡先の世代。|はい|はい|
|GivenName| String|連絡先の名。|はい|はい|
|HomeAddress| [PhysicalAddress](#PhysicalAddress)|連絡先の自宅住所。|はい|はい|
|HomePhones| Collection(String)|連絡先の自宅の電話番号。|はい|いいえ|
|Id| String|連絡先の一意の識別子。|いいえ|いいえ|
|ImAddresses| Collection(String)|連絡先のインスタント メッセージング (IM) アドレス。|はい|いいえ|
|Initials| String|連絡先のイニシャル。|はい|はい|
|JobTitle| String|連絡先の役職。|はい|はい|
|Manager| String|連絡先の上司の名前。|はい|はい|
|MiddleName| String|連絡先のミドル ネーム。|はい|はい|
|MobilePhone1| String|連絡先の携帯電話番号。|はい|はい|
|NickName| String|連絡先のニックネーム。|はい|はい|
|OfficeLocation|String|連絡先のオフィスの所在地。|はい|はい|
|OtherAddress| [PhysicalAddress](#PhysicalAddress)|連絡先の別の住所。|はい|はい|
|ParentFolderId|String|連絡先の親フォルダーの ID。|いいえ|いいえ|
|PersonalNotes|String|連絡先に関するユーザーのメモ。|はい|はい|
|Profession|String|連絡先の専門的職業。|はい|はい|
|SpouseName|String|連絡先の配偶者の名前。|はい|はい|
|Surname| String|連絡先の姓。|はい|はい|
|Title|String|連絡先の肩書。|はい|いいえ|
|YomiCompanyName|String|連絡先の会社名の読み仮名。 |はい|いいえ|
|YomiGivenName|String|連絡先の名 (ファースト ネーム) の読み仮名。 |はい|いいえ|
|YomiSurname|String|連絡先の姓 (ラスト ネーム) の読み仮名。 |はい|いいえ|

ナビゲーション プロパティ **MultiValueExtendedProperties** と **SingleValueExtendedProperties** も、このリソースで利用できます。これらは、リソース インスタンスに定義されている拡張プロパティのそれぞれの型のコレクションを表します。詳細については、「[拡張プロパティ REST API](../api/extended-properties-rest-operations.md)」を参照してください。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**
|:-----|:-----|:-----|:-----|:-----|
|AssistantName| String|連絡先のアシスタントの名前。|はい|はい|
|Birthday| datetimeoffset|連絡先の誕生日です。|はい|はい|
|BusinessAddress| [PhysicalAddress](#PhysicalAddress)|連絡先の勤務先の住所。|はい|はい|
|BusinessHomePage| String|連絡先の勤務先のホーム ページ。|はい|はい|
|BusinessPhones| Collection(String)|連絡先の勤務先の電話番号。|はい|いいえ|
|Categories| Collection(String)|連絡先に関連付けられたカテゴリ。|はい|いいえ|
|ChangeKey| String|連絡先のバージョンを識別します。連絡先を変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|Children|Collection(String)|連絡先の子供の名前。 |はい|はい|
|CompanyName| String|連絡先の会社の名前。|はい|はい|
|Department| String|連絡先の部署。|はい|はい|
|DateTimeCreated| datetimeoffset|連絡先が作成された時刻です。|いいえ|はい|
|DateTimeLastModified| datetimeoffset|連絡先が変更された時刻です。|いいえ|はい|
|DisplayName| String|連絡先の表示名。|はい|はい|
|EmailAddresses| Collection([EmailAddress](#EmailAddress))|連絡先のメール アドレス。|はい|いいえ|
|FileAs| String|連絡先がファイルされる名前。|はい|はい|
|Generation|String|連絡先の世代。|はい|はい|
|GivenName| String|連絡先の名。|はい|はい|
|HomeAddress| [PhysicalAddress](#PhysicalAddress)|連絡先の自宅住所。|はい|はい|
|HomePhones| Collection(String)|連絡先の自宅の電話番号。|はい|いいえ|
|Id| String|連絡先の一意の識別子。|いいえ|いいえ|
|ImAddresses| Collection(String)|連絡先のインスタント メッセージング (IM) アドレス。|はい|いいえ|
|Initials| String|連絡先のイニシャル。|はい|はい|
|JobTitle| String|連絡先の役職。|はい|はい|
|Manager| String|連絡先の上司の名前。|はい|はい|
|MiddleName| String|連絡先のミドル ネーム。|はい|はい|
|MobilePhone1| String|連絡先の携帯電話番号。|はい|はい|
|NickName| String|連絡先のニックネーム。|はい|はい|
|OfficeLocation|String|連絡先のオフィスの所在地。|はい|はい|
|OtherAddress| [PhysicalAddress](#PhysicalAddress)|連絡先の別の住所。|はい|はい|
|ParentFolderId|String|連絡先の親フォルダーの ID。|いいえ|いいえ|
|PersonalNotes|String|連絡先に関するユーザーのメモ。|はい|はい|
|Profession|String|連絡先の専門的職業。|はい|はい|
|SpouseName|String|連絡先の配偶者の名前。|はい|はい|
|Surname| String|連絡先の姓。|はい|はい|
|Title|String|連絡先の肩書。|はい|いいえ|
|YomiCompanyName|String|連絡先の会社名の読み仮名。このプロパティは省略可能です。|はい|いいえ|
|YomiGivenName|String|連絡先の名 (ファースト ネーム) の読み仮名。このプロパティは省略可能です。|はい|いいえ|
|YomiSurname|String|連絡先の姓 (ラスト ネーム) の読み仮名。このプロパティは省略可能です。|はい|いいえ|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="ContactFolderResource"> </a>
### ContactFolder

連絡先が格納されたフォルダーです。

型:**Microsoft.OutlookServices.ContactFolder**

ContactFolder コレクションは、OData 応答の **value** プロパティで連絡先フォルダーの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/contactfolders/$count`) を取得します。

サポートされているアクションについては、「[ContactFolder の操作](..\api\contacts-rest-operations.md#ContactFolderOperations)」を参照してください。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**
|:-----|:-----|:-----|:-----|:-----|
|ChildFolders| Collection([ContactFolder](#ContactFolderResource))|フォルダー内の子フォルダーのコレクション。 ナビゲーション プロパティです。|いいえ|いいえ|
|Contacts| Collection([Contact](#ContactResource))|フォルダー内の連絡先。 ナビゲーション プロパティです。|いいえ|いいえ|
|DisplayName| String|フォルダーの表示名。|はい|はい|
|Id|String|連絡先フォルダーの一意の識別子。|いいえ|いいえ|
|ParentFolderId| String|フォルダーの親フォルダーの ID。|いいえ|いいえ|
|WellKnownName | String| フォルダーが認識されているフォルダーである場合、フォルダーの名前。 現在、認識されている連絡先フォルダーは `contacts` のみです。 |いいえ|いいえ|
|MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |
|SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**
|:-----|:-----|:-----|:-----|:-----|
|ChildFolders| Collection([ContactFolder](#ContactFolderResource))|フォルダー内の子フォルダーのコレクション。 ナビゲーション プロパティです。|いいえ|いいえ|
|Contacts| Collection([Contact](#ContactResource))|フォルダー内の連絡先。 ナビゲーション プロパティです。|いいえ|いいえ|
|DisplayName| String|フォルダーの表示名。|はい|はい|
|Id|String|連絡先フォルダーの一意の識別子。|いいえ|いいえ|
|ParentFolderId| String|フォルダーの親フォルダーの ID。|いいえ|いいえ|
|MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource_v2) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |
|SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource_v2) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**
|:-----|:-----|:-----|:-----|:-----|
|ChildFolders| Collection([ContactFolder](#ContactFolderResource))|フォルダー内の子フォルダーのコレクション。 ナビゲーション プロパティです。|いいえ|いいえ|
|Contacts| Collection([Contact](#ContactResource))|フォルダー内の連絡先。 ナビゲーション プロパティです。|いいえ|いいえ|
|DisplayName| String|フォルダーの表示名。|はい|はい|
|Id|String|連絡先フォルダーの一意の識別子。|いいえ|いいえ|
|ParentFolderId| String|フォルダーの親フォルダーの ID。|いいえ|いいえ|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




<a name="EventResource"> </a>
### イベント

予定表内のイベントです。

型:**Microsoft.OutlookServices.Event**

Event コレクションは、OData 応答の **value** プロパティでイベントの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/events/$count`) を取得します。

サポートされているアクションについては、「[Event の操作](..\api\calendar-rest-operations.md#EventOperations)」を参照してください。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]



|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
|添付ファイル| Collection(Attachment)|イベントの [FileAttachment](#FileAttachmentResource)、[ItemAttachment](#ItemAttachmentResource)、[ReferenceAttachment](#ReferenceAttachmentResource) の各添付ファイルのコレクション。 ナビゲーション プロパティです。|いいえ|いいえ|
|Attendees| Collection([Attendee](#Attendee))|イベントの参加者のコレクション。|はい|いいえ|
|Body| [ItemBody](#ItemBody)|イベントに関連付けられたメッセージの本文。|はい|いいえ|
|BodyPreview| String|イベントに関連付けられたメッセージのプレビュー。|いいえ|いいえ|
|予定表| [予定表](#CalendarResource)|イベントを含む予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
|Categories| Collection(String)|イベントに関連付けられたカテゴリ。|はい|いいえ|
|ChangeKey| String|イベント オブジェクトのバージョンを識別します。イベントを変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|CreatedDateTime| datetimeoffset|イベントが作成された日時です。|いいえ|はい|
|End| [DateTimeTimeZone](#DateTimeTimeZoneBeta)|イベントが終了する日時。|はい|はい|
|Extensions| Collection(Extension) |イベントに対して定義されているオープン型のデータ拡張機能のコレクション。ナビゲーション プロパティです。 |いいえ |はい |
|HasAttachments| boolean|イベントに添付ファイルが含まれている場合に、ｔrue に設定します。|いいえ|はい|
|Id|String|イベントの一意識別子。|いいえ|いいえ|
|Importance| Importance|次のようなイベントの重要度があります。低 = 0、標準 = 1、高 = 2。|はい|はい|
|Instances| Collection([Event](#EventResource))|イベントのインスタンス。 ナビゲーション プロパティです。|いいえ|いいえ|
|iCalUID| String|複数の予定表を通して 1 つのイベントのすべてのインスタンスで共有される一意の識別子。|いいえ|はい|
|IsAllDay| boolean|イベントが一日中続く場合に、true に設定します。 このプロパティを調整するには、イベントの **Start** および **End** プロパティの調整も必要になります。|はい|はい|
|IsCancelled| boolean|イベントがキャンセルされた場合に、true に設定します。|はい|はい|
|IsOrganizer| boolean|メッセージの送信者が主催者でもある場合に、true に設定します。|はい|はい|
|IsReminderOn|Boolean|ユーザーにイベントを通知するアラートを設定する場合は、true に設定します。|はい|はい|
|LastModifiedDateTime| datetimeoffset|イベントが最後に変更された日時です。|いいえ|はい|
|Location| [Location](#Location)|イベントの場所。|はい|はい|
|OnlineMeetingUrl| String | オンライン会議の URL。|はい|いいえ|
|Organizer| [Recipient](#Recipient)|イベントの主催者。|はい|はい|
|OriginalEndTimeZone| String|イベントが作成されたときに設定された終了タイム ゾーン。 有効なタイム ゾーンの一覧については、「[DateTimeTimeZone](#DateTimeTimeZoneBeta)」を参照してください。|いいえ|はい|
|OriginalStartTimeZone| String|イベントが作成されたときに設定された開始タイム ゾーン。 有効なタイム ゾーンの一覧については、「[DateTimeTimeZone](#DateTimeTimeZoneBeta)」を参照してください。|いいえ|はい|
|Recurrence| [PatternedRecurrence](#PatternedRecurrence)|イベントの繰り返しパターン。|はい|いいえ|
|ReminderMinutesBeforeStart|Int32|アラーム通知を行う、イベント開始時間前の分数。|はい|いいえ|
|ResponseRequested| boolean|イベントが承諾または辞退されたときに、送信者が応答を要求する場合に、true に設定します。|はい|はい|
|ResponseStatus| ResponseStatus|イベント メッセージへの応答で送信される応答のタイプを識別します。|いいえ|はい|
|Sensitivity|Sensitivity|イベントのプライバシーのレベルを示します。標準 = 0、個人 = 1、非公開 = 2、社外秘 = 3 です。|はい|はい|
|SeriesMasterId| String|アイテムに割り当てられたカテゴリ。|はい|いいえ|
|ShowAs| FreeBusyStatus|次のようなステータスを示します。予定なし = 0、仮の予定 = 1、予定あり = 2、休暇 = 3、離席 = 4、不明 = -1。|はい|可能|
|Start| [DateTimeTimeZone](#DateTimeTimeZoneBeta)|イベントの開始時刻です。|はい|はい|
|Subject| String|イベントの件名行のテキスト。|はい|はい|
|Type| EventType|次のようなイベント タイプがあります。単一インスタンス = 0、発生 = 1、例外 = 2、連続マスター = 3。|はい|はい|
|WebLink| String|Outlook Web App でイベントを開く URL。<br/><br/>Outlook Web App のメールボックスにログインしている場合、ブラウザーでイベントが開きます。 まだブラウザーでログインしていない場合、ログインするように求められます。<br/><br/>この URL には、iFrame 内からアクセスできます。|いいえ|いいえ|

<!-- Comment out for Mentions API

Add these lines back to above table

|Mentions| Collection ([Mention](#MentionResource)) | A collection of mentions in the event, ordered by the **CreatedDateTime** from the newest to the oldest. By default, a `GET /me/events` does not return this property unless you apply `$expand` on the property. Navigation property.|Yes|No|
|MentionsPreview| [MentionsPreview](#MentionsPreview) | Information about mentions in the event. When processing a `GET /events` request, the server sets this property and includes it in the response by default. The server returns `null` if there are no mentions in the event. Optional. |No|No| 
 Comment out for Mentions API -->



ナビゲーション プロパティ **MultiValueExtendedProperties** と **SingleValueExtendedProperties** も、このリソースで利用できます。これらは、リソース インスタンスに定義されている拡張プロパティのそれぞれの型のコレクションを表します。詳細については、「[拡張プロパティ REST API](../api/extended-properties-rest-operations.md)」を参照してください。

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
|添付ファイル| Collection(Attachment)|イベントの [FileAttachment](#FileAttachmentResource) 添付ファイルと [ItemAttachment](#ItemAttachmentResource) 添付ファイルのコレクションです。 ナビゲーション プロパティです。|いいえ|いいえ|
|Attendees| Collection([Attendee](#Attendeev10))|イベントの参加者のコレクション。|はい|いいえ|
|Body| [ItemBody](#ItemBody)|イベントに関連付けられたメッセージの本文。|はい|いいえ|
|BodyPreview| String|イベントに関連付けられたメッセージのプレビュー。|いいえ|いいえ|
|予定表| [予定表](#CalendarResource)|イベントを含む予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
|Categories| Collection(String)|イベントに関連付けられたカテゴリ。|はい|いいえ|
|ChangeKey| String|イベント オブジェクトのバージョンを識別します。イベントを変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|CreatedDateTime| datetimeoffset|イベントが作成された日時です。|いいえ|はい|
|LastModifiedDateTime| datetimeoffset|イベントが最後に変更された日時です。|いいえ|はい|
|End|[DateTimeTimeZone](#DateTimeTimeZoneV2)|イベントの終了時刻。|はい|はい|
|Extensions| Collection(Extension) |イベントに対して定義されているオープン型のデータ拡張機能のコレクション。ナビゲーション プロパティです。 |いいえ |はい |
|HasAttachments| boolean|イベントに添付ファイルが含まれている場合に、ｔrue に設定します。|いいえ|はい|
|Id|String|イベントの一意識別子。|いいえ|いいえ|
|Importance| Importance|次のようなイベントの重要度があります。低 = 0、標準 = 1、高 = 2。|はい|はい|
|Instances| Collection([Event](#EventResource))|イベントのインスタンス。 ナビゲーション プロパティです。|いいえ|いいえ|
|iCalUID| String|複数の予定表を通して 1 つのイベントのすべてのインスタンスで共有される一意の識別子。|いいえ|はい|
|IsAllDay| boolean|イベントが一日中続く場合に、true に設定します。 このプロパティを調整するには、イベントの **Start** および **End** プロパティの調整も必要になります。|はい|はい|
|IsCancelled| boolean|イベントがキャンセルされた場合に、true に設定します。|はい|はい|
|IsOrganizer| boolean|メッセージの送信者が主催者でもある場合に、true に設定します。|はい|はい|
|IsReminderOn|Boolean|ユーザーにイベントを通知するアラートを設定する場合は、true に設定します。|はい|はい|
|Location| [Location](#Locationv10)|イベントの場所。|はい|はい|
|OriginalEndTimeZone| String|イベントが作成されたときに設定された終了タイム ゾーン。 有効なタイム ゾーンの一覧については、「[DateTimeTimeZone](#DateTimeTimeZoneV2)」を参照してください。|いいえ|はい|
|OriginalStartTimeZone| String|イベントが作成されたときに設定された開始タイム ゾーン。 有効なタイム ゾーンの一覧については、「[DateTimeTimeZone](#DateTimeTimeZoneV2)」を参照してください。|いいえ|はい|
|Organizer| [Recipient](#Recipient)|イベントの主催者。|はい|はい|
|Recurrence| [PatternedRecurrence](#PatternedRecurrence)|イベントの繰り返しパターン。|はい|いいえ|
|ReminderMinutesBeforeStart|Int32|アラーム通知を行う、イベント開始時間前の分数。|はい|いいえ|
|ResponseRequested| boolean|イベントが承諾または辞退されたときに、送信者が応答を要求する場合に、true に設定します。|はい|はい|
|ResponseStatus| ResponseStatus|イベント メッセージへの応答で送信される応答のタイプを識別します。|いいえ|はい|
|Sensitivity|Sensitivity|イベントのプライバシーのレベルを示します。標準 = 0、個人 = 1、非公開 = 2、社外秘 = 3 です。|はい|はい|
|SeriesMasterId| String|アイテムに割り当てられたカテゴリ。|はい|いいえ|
|ShowAs| FreeBusyStatus|次のようなステータスを示します。予定なし = 0、仮の予定 = 1、予定あり = 2、休暇 = 3、離席 = 4、不明 = -1。|はい|可能|
|Start| [DateTimeTimeZone](#DateTimeTimeZoneV2)|イベントの開始時刻です。 |はい|はい|
|Type| EventType|次のようなイベント タイプがあります。単一インスタンス = 0、発生 = 1、例外 = 2、連続マスター = 3。|はい|はい|
|WebLink| String|Outlook Web App でイベントを開く URL。<br/><br/>Outlook Web App のメールボックスにログインしている場合、ブラウザーでイベントが開きます。 まだブラウザーでログインしていない場合、ログインするように求められます。<br/><br/>この URL には、iFrame 内からアクセスできます。|いいえ|いいえ|

ナビゲーション プロパティ **MultiValueExtendedProperties** と **SingleValueExtendedProperties** も、このリソースで利用できます。これらは、リソース インスタンスに定義されている拡張プロパティのそれぞれの型のコレクションを表します。詳細については、「[拡張プロパティ REST API](../api/extended-properties-rest-operations.md)」を参照してください。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
|添付ファイル| Collection(Attachment)|イベントの [FileAttachment](#FileAttachmentResource) 添付ファイルと [ItemAttachment](#ItemAttachmentResource) 添付ファイルのコレクションです。 ナビゲーション プロパティです。|いいえ|いいえ|
|Attendees| Collection([Attendee](#Attendeev10))|イベントの参加者のコレクション。|はい|いいえ|
|Body| [ItemBody](#ItemBody)|イベントに関連付けられたメッセージの本文。|はい|いいえ|
|BodyPreview| String|イベントに関連付けられたメッセージのプレビュー。|いいえ|いいえ|
|予定表| [予定表](#CalendarResource)|イベントを含む予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
|Categories| Collection(String)|イベントに関連付けられたカテゴリ。|はい|いいえ|
|ChangeKey| String|イベント オブジェクトのバージョンを識別します。イベントを変更するたびに ChangeKey も変更されます。これにより、Exchange は正しいバージョンのオブジェクトに変更を適用できます。|いいえ|いいえ|
|DateTimeCreated| datetimeoffset|イベントが作成された日時です。|いいえ|はい|
|DateTimeLastModified| datetimeoffset|イベントが最後に変更された日時です。|いいえ|はい|
|End| datetimeoffset|イベントが終了する日時。<br/><br/>既定で、終了時刻は UTC 単位です。 EndTimeZone でオプションのタイム ゾーンを指定して、そのタイム ゾーンで終了時刻を表現し、UTC からの時間オフセットを含めることができます。 EndTimeZone を使用する場合、StartTimeZone の値も指定する必要があります。<br/><br/>この例では、太平洋標準時で 2015 年 2 月 25 日午後 9:34 を指定します ("2015-02-25T21:34:00-08:00")。 |はい|はい|
|EndTimeZone| String | 会議終了時刻のタイム ゾーンを識別します (End プロパティを参照)。 このプロパティは、Windows に格納されているタイム ゾーン名に設定されます。 System.TimeZoneInfo.GetSystemTimeZones() を呼び出すことによって、タイム ゾーン名を取得できます。<br/><br/>このプロパティは、v1.0 では省略可能です。 ただし、StartTimeZone プロパティを使用する場合は、このプロパティを使用する必要があります。<br/><br/>詳細については、「[TimeZone](https://technet.microsoft.com/en-us/library/cc749073.aspx)」を参照してください。 |はい|いいえ|
|HasAttachments| boolean|イベントに添付ファイルが含まれている場合に、ｔrue に設定します。|いいえ|はい|
|Id|String|イベントの一意識別子。|いいえ|いいえ|
|Importance| Importance|次のようなイベントの重要度があります。低 = 0、標準 = 1、高 = 2。|はい|はい|
|Instances| Collection([Event](#EventResource))|イベントのインスタンス。 ナビゲーション プロパティです。|いいえ|いいえ|
|iCalUID| String|複数の予定表を通して 1 つのイベントのすべてのインスタンスで共有される一意の識別子。|いいえ|はい|
|IsAllDay| boolean|イベントが一日中続く場合に、true に設定します。 このプロパティを調整するには、イベントの **Start** および **End** プロパティの調整も必要になります。|はい|はい|
|IsCancelled| boolean|イベントがキャンセルされた場合に、true に設定します。|はい|はい|
|IsOrganizer| boolean|メッセージの送信者が主催者でもある場合に、true に設定します。|はい|はい|
|Location| [Location](#Locationv10)|イベントの場所。|はい|はい|
|Organizer| [Recipient](#Recipient)|イベントの主催者。|はい|はい|
|Recurrence| [PatternedRecurrence](#PatternedRecurrence)|イベントの繰り返しパターン。|はい|いいえ|
|ResponseRequested| boolean|イベントが承諾または辞退されたときに、送信者が応答を要求する場合に、true に設定します。|はい|はい|
|ResponseStatus| ResponseStatus|イベント メッセージへの応答で送信される応答のタイプを識別します。|いいえ|はい|
|Sensitivity|Sensitivity|イベントのプライバシーのレベルを示します。標準 = 0、個人 = 1、非公開 = 2、社外秘 = 3 です。|はい|はい|
|SeriesMasterId| String|アイテムに割り当てられたカテゴリ。|はい|いいえ|
|ShowAs| FreeBusyStatus|次のようなステータスを示します。予定なし = 0、仮の予定 = 1、予定あり = 2、休暇 = 3、離席 = 4、不明 = -1。|はい|可能|
|Start| datetimeoffset|イベントの開始時刻です。 <br/><br/>既定で、開始時刻は UTC 単位です。 EndTimeZone でオプションのタイム ゾーンを指定して、そのタイム ゾーンで開始時刻を表現し、UTC からの時間オフセットを含めることができます。 StartTimeZone を使用する場合、EndTimeZone の値も指定する必要があります。<br/><br/>この例では、太平洋標準時で 2015 年 2 月 25 日午後 7:34 を指定します ("2015-02-25T19:34:00-08:00")。  |はい|はい|
|StartTimeZone| String | 会議開始時刻のタイム ゾーンを識別します (Start プロパティを参照)。 このプロパティは、クライアントの代わりにサービスがタイム ゾーンの変更を処理できるようにします。 このプロパティは、Windows に格納されているタイム ゾーン名に設定されます。 System.TimeZoneInfo.GetSystemTimeZones() を呼び出すことによって、タイム ゾーン名を取得できます。 <br/><br/>このプロパティは、v1.0 では省略可能です。 ただし、EndTimeZone プロパティを使用する場合は、このプロパティを使用する必要があります。<br/><br/>このプロパティの値の例は、"太平洋標準時" です。 詳細については、「<a href="https://technet.microsoft.com/en-us/library/cc749073(v=ws.10).aspx">TimeZone</a>」を参照してください。  |はい|いいえ|
|Subject| String|イベントの件名行のテキスト。|はい|はい|
|Type| EventType|次のようなイベント タイプがあります。単一インスタンス = 0、発生 = 1、例外 = 2、連続マスター = 3。|はい|はい|
|WebLink| String|Outlook Web App でイベントを開く URL。<br/><br/>Outlook Web App のメールボックスにログインしている場合、ブラウザーでイベントが開きます。 まだブラウザーでログインしていない場合、ログインするように求められます。<br/><br/>この URL には、iFrame 内からアクセスできます。|いいえ|いいえ|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->






<a name="EventMessageResource"> </a>

### EventMessage

会議出席依頼、会議中止メッセージ、会議承諾メッセージ、会議仮承諾メッセージ、または会議辞退メッセージを表すメッセージです。

基本型： **[Message](#MessageResource)**

通常、**EventMessage** インスタンスは、イベント主催者が会議を作成した結果、または、参加者が会議出席依頼に応答した結果として、受信トレイ フォルダーに届けられます。 イベント メッセージには [Message](#MessageResource) と同様の方法で対処しますが、次の表に示すようなわずかな違いがあります。  

|**アクション/動詞**|**アクセス許可**|**説明**|
|:-----|:-----|:-----|
|イベント メッセージの作成 (POST)| **該当なし** | 許可されていません。 400 応答コードが生成されます。|
|イベント メッセージの更新 (PATCH) | **Mail.Write**| From、Sender、ToRecipients、CcRecipients、BccRecipients、ReplyTo、IsDeliveryReceiptRequested、IsReadReceiptRequested、IsDraft、IsRead、Subject、Body、Importance、および Categories の各プロパティを更新できます。|
|イベント メッセージの削除 (DELETE) | **Mail.Write**| [Message](#MessageResource) の場合と同じアクションです。 |
| イベント メッセージの移動 (POST) | **Mail.Write**  | [Message](#MessageResource) の場合と同じアクションです。  |
| イベント メッセージのコピー (POST) | **Mail.Write**  | [Message](#MessageResource) の場合と同じアクションです。  |
| 返信メッセージの下書きの作成 (POST) | **Mail.Write**  | [Message](#MessageResource) の場合と同じアクションです。 |
| 全員に返信メッセージの下書きの作成 (POST) | **Mail.Write**  | [Message](#MessageResource) の場合と同じアクションです。 |
| 返信の作成 (POST) | **Mail.Write**  | [Message](#MessageResource) の場合と同じアクションです。  |
| 全員に返信の作成 (POST) | **Mail.Write**  |  [Message](#MessageResource) の場合と同じアクションです。  |
| 既存のイベント メッセージの送信 (POST) | **Mail.Write**  | IsDraft プロパティの値が **true** になっているイベント メッセージのみを送信できます。メッセージのコピーは送信済みアイテム フォルダーに保存されます。   |
| 転送イベント メッセージの下書きの作成 | **Mail.Write**  | [Message](#MessageResource) の場合と同じアクションです。  |
| イベント メッセージの転送 | **Mail.Write**  | [Message](#MessageResource) の場合と同じアクションです。  |


**EventMessage** インスタンスには、基本型の **Message** のプロパティと、次の表のプロパティが含まれます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| EndDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta)|関連するイベントが終了する日時。|はい|はい|
| Event | [Event](#EventResource) | イベント メッセージに関連付けられたイベント。参加者または部屋リソースの前提は、会議出席依頼イベント メッセージが届いたときにイベントを含む予定表を自動的に更新するようにカレンダー アテンダントが設定されていることです。ナビゲーション プロパティです。 | いいえ | いいえ |
| IsAllDay | ブール型 (Boolean) | イベントが 1 日中続くかどうかを示します。 このプロパティを調整するには、イベントの **StartDateTime** および **EndDateTime** プロパティの調整も必要になります。 | はい | はい |
| IsOutOfDate | Boolean | この会議出席要求がより新しい要求によって古くなっているかどうかを示します。 | いいえ | いいえ |
| Location | [Location](#Location)|関連するイベントの場所。|はい|はい|
| MeetingMessageType | [MeetingMessageType](#MeetingMessageType) | イベント メッセージのタイプ。なし = 0、会議開催依頼 = 1、会議の中止 = 2、会議の承諾 = 3、会議の一時承諾 = 4、会議の辞退 = 5。 | いいえ |はい|
| Recurrence | [PatternedRecurrence](#PatternedRecurrence)|関連するイベントの繰り返しパターン。|はい|いいえ|
| StartDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta)|関連するイベントの開始時刻。|はい|はい|
| Type | EventType|関連するイベントの種類。単一インスタンス = 0、発生 = 1、例外 = 2、連続マスター = 3。|はい|はい|


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| Event | [Event](#EventResource) | イベント メッセージに関連付けられたイベント。参加者または部屋リソースの前提は、会議出席依頼イベント メッセージが届いたときにイベントを含む予定表を自動的に更新するようにカレンダー アテンダントが設定されていることです。ナビゲーション プロパティです。 | いいえ | いいえ |
| MeetingMessageType | [MeetingMessageType](#MeetingMessageType) | イベント メッセージのタイプ。なし = 0、会議開催依頼 = 1、会議の中止 = 2、会議の承諾 = 3、会議の一時承諾 = 4、会議の辞退 = 5。 | いいえ |はい|

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| Event | [Event](#EventResource) | イベント メッセージに関連付けられたイベント。参加者または部屋リソースの前提は、会議出席依頼イベント メッセージが届いたときにイベントを含む予定表を自動的に更新するようにカレンダー アテンダントが設定されていることです。ナビゲーション プロパティです。 | いいえ | いいえ |
| MeetingMessageType | [MeetingMessageType](#MeetingMessageType) | イベント メッセージのタイプ。なし = 0、会議開催依頼 = 1、会議の中止 = 2、会議の承諾 = 3、会議の一時承諾 = 4、会議の辞退 = 5。 | いいえ |はい|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="EventMessageRequest"></a>
### EventMessageRequest (プレビュー)  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

会議出席依頼を表すメッセージ。

基本型： [EventMessage](#EventMessageResource)

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| PreviousEndDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta)|会議出席依頼の以前の終了日時。|いいえ|はい|
| PreviousLocation | [Location](#Location)|会議出席依頼の以前の場所。|いいえ|はい|
| PreviousStartDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta)|会議出席依頼の以前の開始日時。|いいえ|はい|

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




<a name="ExtendedProperties"></a>
### Extended properties  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

エンティティのカスタム プロパティを、プロパティで対象とする値に応じて [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource) または [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource) として作成できます。  

<a name="MultiValueLegacyExtendedPropertyResource"> </a>
**MultiValueLegacyExtendedProperty**

複数の値のコレクションを含めることができる拡張プロパティ。 

型:**Microsoft.OutlookServices.MultiValueLegacyExtendedProperty**


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| 値 | Collection(String) | プロパティ値のコレクション。 | はい | いいえ |
| PropertyId | String | プロパティ ID。これは、プロパティの識別に使用されます。| いいえ | いいえ |

 

<a name="SingleValueLegacyExtendedPropertyResource"> </a>
**SingleValueLegacyExtendedProperty**

単一値が含まれる拡張プロパティ。 

型:**Microsoft.OutlookServices.SingleValueLegacyExtendedProperty**


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| 値 | String | プロパティ値。 | はい | いいえ |
| PropertyId | String | プロパティ ID。これは、プロパティの識別に使用されます。 | いいえ | はい |


拡張プロパティを作成する場合、様々な方法で **PropertyId** を指定できます。 詳細については、「[PropertyId 形式](..\api\extended-properties-rest-operations.md#ExtendedPropertyIdFormats)」を参照してください。

使用できる関連操作については、「[拡張プロパティ REST API リファレンス](..\api\extended-properties-rest-operations.md)」をご覧ください。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


エンティティのカスタム プロパティを、プロパティで対象とする値に応じて [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource_v2) または [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource_v2) として作成できます。  

<a name="MultiValueLegacyExtendedPropertyResource_v2"> </a>
**MultiValueLegacyExtendedProperty**

複数の値のコレクションを含めることができる拡張プロパティ。 

型:**Microsoft.OutlookServices.MultiValueLegacyExtendedProperty**


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| 値 | Collection(String) | プロパティ値のコレクション。 | はい | いいえ |
| PropertyId | String | プロパティ ID。これは、プロパティの識別に使用されます。| いいえ | いいえ |

 

<a name="SingleValueLegacyExtendedPropertyResource_v2"> </a>
**SingleValueLegacyExtendedProperty**

単一値が含まれる拡張プロパティ。 

型:**Microsoft.OutlookServices.SingleValueLegacyExtendedProperty**


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| 値 | String | プロパティ値。 | はい | いいえ |
| PropertyId | String | プロパティ ID。これは、プロパティの識別に使用されます。 | いいえ | はい |


拡張プロパティを作成する場合、様々な方法で **PropertyId** を指定できます。 詳細については、「[PropertyId 形式](..\api\extended-properties-rest-operations.md#ExtendedPropertyIdFormats)」を参照してください。

使用できる関連操作については、「[拡張プロパティ REST API リファレンス](..\api\extended-properties-rest-operations.md)」をご覧ください。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版および v2.0 で利用できます。 詳細については、記事の右上隅のコントロールを使用して、バージョンを選びます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="FileAttachmentResource"> </a>
### FileAttachment

メッセージまたはイベントに添付されたファイル (テキスト ファイルや Word 文書など)。**ContentBytes** プロパティには、base64 でエンコードされたファイルの内容が含まれています。[Attachment](#Attachment) エンティティから派生します。

型:**Microsoft.OutlookServices.FileAttachment**

基本型： **Microsoft.OutlookServices.Attachment**


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ContentBytes| binary|ファイルのバイナリ コンテンツです。|いいえ|
|ContentId| String|Exchange ストア内の添付ファイルの ID。|いいえ|
|ContentLocation| String|添付ファイルのコンテンツの場所に対応する Uniform Resource Identifier (URI)。|いいえ|
|ContentType| String|添付ファイルのコンテンツ タイプ。 |はい|
|LastModifiedDateTime| datetimeoffset|添付ファイルが最後に変更された日時です。|いいえ|
|Id| String|添付ファイル ID。|いいえ|
|IsInline| boolean|インライン添付ファイルの場合、true に設定します。|はい|
|名前| String|埋め込み添付ファイルを表すアイコンの下に表示されるテキストを表す名前。これは、実際のファイル名にする必要はありません。|はい|
|Size| Int32|添付ファイルのバイト単位のサイズ。|いいえ|


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ContentBytes| binary|ファイルのバイナリ コンテンツです。|いいえ|
|ContentId| String|Exchange ストア内の添付ファイルの ID。|いいえ|
|ContentLocation| String|添付ファイルのコンテンツの場所に対応する Uniform Resource Identifier (URI)。|いいえ|
|ContentType| String|添付ファイルのコンテンツ タイプ。 |はい|
|LastModifiedDateTime| datetimeoffset|添付ファイルが最後に変更された日時です。|いいえ|
|Id| String|添付ファイル ID。|いいえ|
|IsInline| boolean|インライン添付ファイルの場合、true に設定します。|はい|
|名前| String|埋め込み添付ファイルを表すアイコンの下に表示されるテキストを表す名前。これは、実際のファイル名にする必要はありません。|はい|
|Size| Int32|添付ファイルのバイト単位のサイズ。|いいえ|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ContentBytes| binary|ファイルのバイナリ コンテンツです。|いいえ|
|ContentId| String|Exchange ストア内の添付ファイルの ID。|いいえ|
|ContentLocation| String|添付ファイルのコンテンツの場所に対応する Uniform Resource Identifier (URI)。|いいえ|
|ContentType| String|添付ファイルのコンテンツ タイプ。 |はい|
|DateTimeLastModified| datetimeoffset|添付ファイルが最後に変更された日時です。|いいえ|
|Id| String|添付ファイル ID。|いいえ|
|IsContactPhoto| boolean|現在は使用されていません。|はい|
|IsInline| boolean|インライン添付ファイルの場合、true に設定します。|はい|
|名前| String|埋め込み添付ファイルを表すアイコンの下に表示されるテキストを表す名前。これは、実際のファイル名にする必要はありません。|はい|
|Size| Int32|添付ファイルのバイト単位のサイズ。|いいえ|



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




<a name="FolderResource"> </a>
### Folder / MailFolder   <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

**注** ベータ版では、以前は **Folder** という名前のエンティティと型が **MailFolder** という名前に変更されました。

受信トレイ、下書き、送信済みアイテムなどのユーザーのメールボックス内のフォルダーです。 フォルダーにはメッセージと他のフォルダーを含めることができます。

型:**Microsoft.OutlookServices.MailFolder**

MailFolder コレクションは、OData 応答の **value** プロパティでフォルダーの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/folders/$count`) を取得します。

サポートされているアクションについては、「[Folder の操作](..\api\mail-rest-operations.md#FolderOperations)」を参照してください。

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| ChildFolderCount| Int32|フォルダー内のフォルダー数。|いいえ|はい|
| ChildFolders| Collection([MailFolder](#FolderResource))|フォルダー内の子フォルダーのコレクション。 ナビゲーション プロパティです。 |いいえ|いいえ|
| DisplayName| String|フォルダーの表示名。|はい|はい|
| Id| String|フォルダーの一意の識別子。 次の既知の名前を使用して対応するフォルダーにアクセスできます。**Inbox**、**Drafts**、**SentItems**、**DeletedItems**。|いいえ|いいえ|
| Messages| Collection([Message](#MessageResource))|フォルダー内のメッセージのコレクション。 ナビゲーション プロパティです。|いいえ|いいえ|
| ParentFolderId| String|フォルダーの親フォルダーの一意の識別子。|いいえ|いいえ|
| TotalItemCount | Int32 | フォルダーに含まれるアイテムの数。 | いいえ | はい |
| UnreadItemCount | Int32 | フォルダー内で未読としてマークされているアイテム数。 | いいえ | はい |
| WellKnownName | String | `clutter`、`deleteditems`、`drafts`、`inbox`、`junkemail`、`outbox`、`sentitems` などの、フォルダーの既知の名前。 | いいえ | はい |
| MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |
| SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |


**アイテム数の効率的な取得**

フォルダーの **TotalItemCount** プロパティと **UnreadItemCount** プロパティを使用すると、ファイル内の既読アイテム数を簡単に算出できます。  
これにより、大幅な遅延が発生する可能性がある次のようなクエリを回避できます。

```
https://outlook.office.com/api/beta/me/mailfolders/inbox/messages?$count=true&$filter=isread%20eq%20false
```
Outlook 内のフォルダーには、複数の種類のアイテムを含めることができます。たとえば、受信トレイには、メール アイテムとは異なる会議出席依頼アイテムを入れることができます。**TotalItemCount** と **UnreadItemCount** には、アイテムの種類に関係なく、フォルダー内のアイテムが含まれます。




[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

**注** V2.0 では、以前は **Folder** という名前のエンティティと型が **MailFolder** という名前に変更されました。

受信トレイ、下書き、送信済みアイテムなどのユーザーのメールボックス内のフォルダーです。 フォルダーにはメッセージと他のフォルダーを含めることができます。

型:**Microsoft.OutlookServices.MailFolder**

MailFolder コレクションは、OData 応答の **value** プロパティでフォルダーの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/folders/$count`) を取得します。

サポートされているアクションについては、「[Folder の操作](..\api\mail-rest-operations.md#FolderOperations)」を参照してください。

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| ChildFolderCount| Int32|フォルダー内のフォルダー数。|いいえ|はい|
| ChildFolders| Collection([MailFolder](#FolderResource))|フォルダー内の子フォルダーのコレクション。 ナビゲーション プロパティです。 |いいえ|いいえ|
| DisplayName| String|フォルダーの表示名。|はい|はい|
| Id| String|フォルダーの一意の識別子。 次の既知の名前を使用して対応するフォルダーにアクセスできます。**Inbox**、**Drafts**、**SentItems**、**DeletedItems**。|いいえ|いいえ|
| Messages| Collection([Message](#MessageResource))|フォルダー内のメッセージのコレクション。 ナビゲーション プロパティです。|いいえ|いいえ|
| ParentFolderId| String|フォルダーの親フォルダーの一意の識別子。|いいえ|いいえ|
| TotalItemCount | Int32 | フォルダーに含まれるアイテムの数。 | いいえ | はい |
| UnreadItemCount | Int32 | フォルダー内で未読としてマークされているアイテム数。 | いいえ | はい |
| MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource_v2) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |
| SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource_v2) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい |

**アイテム数の効率的な取得**

フォルダーの **TotalItemCount** プロパティと **UnreadItemCount** プロパティを使用すると、ファイル内の既読アイテム数を簡単に算出できます。  
これにより、大幅な遅延が発生する可能性がある次のようなクエリを回避できます。

```
https://outlook.office.com/api/v2.0/me/mailfolders/inbox/messages?$count=true&$filter=isread%20eq%20false
```
Outlook 内のフォルダーには、複数の種類のアイテムを含めることができます。たとえば、受信トレイには、メール アイテムとは異なる会議出席依頼アイテムを入れることができます。**TotalItemCount** と **UnreadItemCount** には、アイテムの種類に関係なく、フォルダー内のアイテムが含まれます。



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

**注** バージョン 1.0 以降では、**Folder** エンティティと型が **MailFolder** という名前に変更されました。 

受信トレイ、下書き、送信済みアイテムなどのユーザーのメールボックス内のフォルダーです。 フォルダーにはメッセージと他のフォルダーを含めることができます。

型:**Microsoft.OutlookServices.Folder**

Folder コレクションは、OData 応答の **value** プロパティでフォルダーの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/folders/$count`) を取得します。

サポートされているアクションについては、「[Folder の操作](..\api\mail-rest-operations.md#FolderOperations)」を参照してください。

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| ChildFolderCount| Int32|フォルダー内のフォルダー数。|いいえ|はい|
| ChildFolders| Collection([Folder](#FolderResource))|フォルダー内の子フォルダーのコレクション。 ナビゲーション プロパティです。 |いいえ|いいえ|
| DisplayName| String|フォルダーの表示名。|はい|はい|
| Id| String|フォルダーの一意の識別子。 次の既知の名前を使用して対応するフォルダーにアクセスできます。**Inbox**、**Drafts**、**SentItems**、**DeletedItems**。|いいえ|いいえ|
| Messages| Collection([Message](#MessageResource))|フォルダー内のメッセージのコレクション。 ナビゲーション プロパティです。|いいえ|いいえ|
| ParentFolderId| String|フォルダーの親フォルダーの一意の識別子。|いいえ|いいえ|
| TotalItemCount | Int32 | フォルダーに含まれるアイテムの数。 | いいえ | はい |
| UnreadItemCount | Int32 | フォルダー内で未読としてマークされているアイテム数。 | いいえ | はい |


**アイテム数の効率的な取得**

フォルダーの **TotalItemCount** プロパティと **UnreadItemCount** プロパティを使用すると、ファイル内の既読アイテム数を簡単に算出できます。  
これにより、大幅な遅延が発生する可能性がある次のようなクエリを回避できます。

```
https://outlook.office.com/api/v1.0/me/folders/inbox/messages?$count=true&$filter=isread%20eq%20false
```
Outlook 内のフォルダーには、複数の種類のアイテムを含めることができます。たとえば、受信トレイには、メール アイテムとは異なる会議出席依頼アイテムを入れることができます。**TotalItemCount** と **UnreadItemCount** には、アイテムの種類に関係なく、フォルダー内のアイテムが含まれます。



[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="InferenceClassificationResource"> </a>
### InferenceClassification  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

ユーザーにとって、より関連性や重要性があるメッセージに注意が向けられるようにするためのユーザー メッセージの分類です。 

型:**Microsoft.OutlookServices.InferenceClassification**

|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|Overrides| Collection([InferenceClassificationOverride](#InferenceClassificationOverrideResource))|ユーザーが、[InferenceClassificationType](#InferenceClassificationTypeBeta) でサポートされている特定の方法で特定の差出人からのメッセージを常時分類するための一連のオーバーライド。 ナビゲーション プロパティです。|はい|

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

ユーザーにとって、より関連性や重要性があるメッセージに注意が向けられるようにするためのユーザー メッセージの分類です。 

型:**Microsoft.OutlookServices.InferenceClassification**

|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|Overrides| Collection([InferenceClassificationOverride](#InferenceClassificationOverrideResource))|ユーザーが、[InferenceClassificationType](#InferenceClassificationTypeV2) でサポートされている特定の方法で特定の差出人からのメッセージを常時分類するための一連のオーバーライド。 ナビゲーション プロパティです。|はい|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在 v2.0 とベータ版で利用できます。詳細については、記事の右上隅にコントロールを使用して、どちらかのバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="InferenceClassificationOverrideResource"> </a>
### InferenceClassificationOverride  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

特定の差出人からの着信メッセージを常時分類するためのユーザーのオーバーライドを表します。

型:**Microsoft.OutlookServices.InferenceClassificationOverride**


|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ClassifyAs| [InferenceClassificationType](#InferenceClassificationTypeBeta)|特定の差出人からの着信メッセージを常時分類する方法を指定します。 優先=0、その他=1。 |はい|
|Id| String|オーバーライドの一意識別子。|いいえ|
|SenderEmailAddress| [EmailAddress](#EmailAddress)|オーバーライドを作成する対象の差出人のメール アドレス情報。|はい|

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

特定の差出人からの着信メッセージを常時分類するためのユーザーのオーバーライドを表します。

型:**Microsoft.OutlookServices.InferenceClassificationOverride**


|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ClassifyAs| [InferenceClassificationType](#InferenceClassificationTypeV2)|特定の差出人からの着信メッセージを常時分類する方法を指定します。 優先=0、その他=1。 |はい|
|Id| String|オーバーライドの一意識別子。|いいえ|
|SenderEmailAddress| [EmailAddress](#EmailAddress)|オーバーライドを作成する対象の差出人のメール アドレス。|はい|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在 v2.0 とベータ版で利用できます。詳細については、記事の右上隅にコントロールを使用して、どちらかのバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="ItemAttachmentResource"> </a>
### ItemAttachment

別のメッセージまたはイベントに添付されたメッセージ、連絡先、またはイベントです。 [Attachment](#Attachment) エンティティから派生します。

型:**Microsoft.OutlookServices.ItemAttachment**

基本型： **Microsoft.OutlookServices.Attachment**


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ContentType| String|添付ファイルのコンテンツ タイプ。|はい|
|LastModifiedDateTime| datetimeoffset|添付ファイルが変更された最後の日時です。|いいえ|
|Id| String|添付ファイル ID。|いいえ|
|アイテム| アイテム|添付されたメッセージまたはイベントです。ナビゲーション プロパティです。|はい|
|IsInline| boolean|添付ファイルがインライン (アイテムの本文に埋め込まれた画像など) の場合に、true に設定します。|はい|
|名前| String|添付ファイルの表示名。|はい|
|Size| Int32|添付ファイルのバイト単位のサイズ。|はい|


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ContentType| String|添付ファイルのコンテンツ タイプ。|はい|
|LastModifiedDateTime| datetimeoffset|添付ファイルが変更された最後の日時です。|いいえ|
|Id| String|添付ファイル ID。|いいえ|
|アイテム| アイテム|添付されたメッセージまたはイベントです。ナビゲーション プロパティです。|はい|
|IsInline| boolean|添付ファイルがインライン (アイテムの本文に埋め込まれた画像など) の場合に、true に設定します。|はい|
|名前| String|添付ファイルの表示名。|はい|
|Size| Int32|添付ファイルのバイト単位のサイズ。|はい|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|
|:-----|:-----|:-----|:-----|
|ContentType| String|添付ファイルのコンテンツ タイプ。|はい|
|DateTimeLastModified| datetimeoffset|添付ファイルが変更された最後の日時です。|いいえ|
|Id| String|添付ファイル ID。|いいえ|
|アイテム| アイテム|添付されたメッセージまたはイベントです。ナビゲーション プロパティです。|はい|
|IsInline| boolean|添付ファイルがインライン (アイテムの本文に埋め込まれた画像など) の場合に、true に設定します。|はい|
|名前| String|添付ファイルの表示名。|はい|
|Size| Int32|添付ファイルのバイト単位のサイズ。|はい|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="MentionResource"> </a>

<!-- Comment out for Mentions API

### Mention (preview)

Comment out for Mentions API -->

<!-- ==================================== Start beta content ==================================================== -->

<!-- Comment out for Mentions API
Add second row back to table to format table columns

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Represents a notification to a person based on the person's email address. This type of notifications is also known as 
@-mentions.

The [Event](#EventResource) and [Message](#MessageResource) resources support **Mention**. These resources include a
**MentionsPreview** property which indicates whether the signed-in user is mentioned in that instance, and the
**Mentions** navigation property which supports getting details of any mention in that instance.

When creating an event or message, an app can create a mention in the same `POST` request by including the mention as part of 
the **Mentions** property. In one `GET` request with the `$filter` query parameter, an app can return 
all the events or messages in the signed-in user's mailbox that have included the user as a mention. A `GET` request with
the `$expand` query parameter lets the app expand all mentions in a specific event or message.

This mechanism of letting an app set and get mentions in events and messages enables light-weight notifications, where the
user making the mention can remain in the existing context (such as composing a message body) while the app sets 
the underlying **Mentions** property, and the mentioned persons can easily find out if and where they are mentioned 
as soon as the app makes `GET` requests with the `$filter` or `$expand` query parameter.  

An example use case of the Mentions API is in Outlook, the mail client itself. When a user types `@` while writing a message or 
meeting request, Outlook lets the user simply select or enter a name to complete the @-mention, without having to step out of 
the context to call the person's attention. Beneath the covers, Outlook sets the **Mentions** property before actually
creating and sending the message or event. Outlook also uses `GET` operations with `$filter` and `$expand` to let the 
signed-in user conveniently look up if and which messages or events mention the user, alerting the user to action items 
or discussions for quicker response.

Type:  **Microsoft.OutlookServices.Mention**



|**Property**|**Type**|**Description**|**Writable?**|**Filterable?**|

|Application | String | The name of the application where the mention is created. Optional. Not used and defaulted as null for **Message** and **Event**. | Yes | No |
|ClientReference | String | A unique identifier that represents a parent of the resource instance. Optional. Not used and defaulted as null for **Message** and **Event**. | Yes | Yes |
|CreatedBy  | [EmailAddress](#EmailAddress) | The email information of the user who made the mention. Required. |Yes |No |
|CreatedDateTime  |DateTimeOffset |The date and time that the mention is created on the client. | No | No |
|DeepLink | String | A deep web link to the context of the mention in the resource instance. Optional. Not used and defaulted as null for **Message** and **Event**. | Yes | No |
|Id | String| The unique identifier of a mention in a resource instance.|No|No|
|Mentioned | [EmailAddress](#EmailAddress) | The email information of the mentioned person. Required. |Yes |No |
|MentionText | String | Content for the mention. Optional. Not used and defaulted as null for **Message** and **Event**. | Yes | No |
|ServerCreatedDateTime | DateTimeOffset | The date and time that the mention is created on the server. Optional. Not used and defaulted as null for **Message** and **Event**. | No | No |


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

Comment out for Mentions API -->

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

<!-- Comment out for Mentions API

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

This feature is currently available in only beta. To find out more, use the control in the top right corner of
the article and select **beta**.


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

Comment out for Mentions API -->

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

<!-- Comment out for Mentions API

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

This feature is currently available in only beta. To find out more, use the control in the top right corner of
the article and select **beta**.

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

Comment out for Mentions API -->

<!-- ==================================== End v1 content ======================================================== -->






<a name="MessageResource"> </a>
### Message

メールボックス フォルダー内のメッセージです。

型:**Microsoft.OutlookServices.Message**

Message コレクションは、OData 応答の **value** プロパティでメッセージの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/messages/$count`) を取得します。

サポートされているアクションについては、「[Message の操作](..\api\mail-rest-operations.md#MessageOperations)」を参照してください。

<a name="MessageProperties"></a>

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|**検索可能**|
|:-----|:-----|:-----|:-----|:-----|:-----|
| 添付ファイル| Collection(Attachment)|メッセージの [FileAttachment](#FileAttachment) 添付ファイルと [ItemAttachment](#ItemAttachmentResource) 添付ファイル。 ナビゲーション プロパティです。|はい|いいえ|はい|
| BccRecipients| Collection ([Recipient](#Recipient))|メッセージの BCC 受信者。|はい|いいえ|はい|
| <a name="MessageBody">Body</a> | [ItemBody](#ItemBody)|メッセージの本文。|はい|いいえ|既定|
| BodyPreview| String|メッセージ本文の内容の最初の 255 文字。|いいえ|いいえ|はい|
| Categories| Collection (String)|メッセージに関連付けられたカテゴリ。|はい|はい|はい|
| CcRecipients| Collection ([Recipient](#Recipient))|メッセージの CC 受信者。|はい|いいえ|はい|
| ChangeKey| String|メッセージのバージョン。|いいえ|いいえ|いいえ|
| ConversationId| String|電子メールが属している会話の ID。|いいえ|はい|いいえ|
| ConversationIndex| Binary|電子メールが属している会話内の項目の相対位置を示します。|いいえ|いいえ|いいえ|
| CreatedDateTime| datetimeoffset|メッセージが作成された日時です。|いいえ|はい|いいえ|
| Extensions| Collection(Extension) |メッセージに対して定義されているオープン型のデータ拡張機能のコレクション。ナビゲーション プロパティです。 |いいえ |はい |いいえ|
| フラグ| [FollowupFlag](#FollowupFlag) | メッセージの任意のフォローアップに関する情報。 | はい | はい | はい |
| From|[Recipient](#Recipient)|メッセージのメールボックス所有者と送信者。|はい|はい|はい|
| HasAttachments| boolean|メッセージに添付ファイルがあるかどうかを示します。|いいえ|はい|はい|
| Id| String| メッセージの一意識別子。|いいえ|いいえ|いいえ|
| Importance| Importance|次のようなメッセージの重要度があります。低 = 0、標準 = 1、高 = 2。|はい|はい|はい|
| InferenceClassification | [InferenceClassificationType](#InferenceClassificationTypeBeta) | 推定される関連性や重要性、または明示的なオーバーライドに基づく、ユーザーの対象メッセージの分類。 |はい|はい|いいえ|
| InternetMessageId |String |[RFC2822](http://www.ietf.org/rfc/rfc2822.txt) によって指定された形式のメッセージ ID。 |いいえ|いいえ|いいえ|
| IsDeliveryReceiptRequested| boolean|メッセージの開封応答が要求されているかどうかを示します。|はい|はい|いいえ|
| IsDraft| ブール値|メッセージが下書きかどうかを示します。メッセージがまだ送信されていなければ下書きです。|いいえ|はい|いいえ|
| IsRead| boolean|メッセージが読み取られたかどうかを示します。|はい|はい|いいえ|
| IsReadReceiptRequested| boolean|メッセージの開封応答が要求されているかどうかを示します。|はい|はい|いいえ|
| LastModifiedDateTime| datetimeoffset|メッセージが最後に変更された日時です。|いいえ|はい|いいえ|
| MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい | いいえ |
| ParentFolderId| String|メッセージの親フォルダーの一意の識別子。|いいえ|いいえ|いいえ|
| ReceivedDateTime| datetimeoffset|メッセージが受信された日時です。|いいえ|はい|はい|
| ReplyTo| Collection ([Recipient](#Recipient))|返信時に使用される電子メール アドレス。|いいえ|いいえ|いいえ|
| Sender| [Recipient](#Recipient)|メッセージを生成するために実際に使用されるアカウント。|はい|はい|既定|
| SentDateTime| datetimeoffset|メッセージが送信された日時です。|いいえ|はい|いいえ|
| SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい | いいえ |
| Subject| String|メッセージの件名を指定します。|はい|はい|既定|
| ToRecipients| Collection ([Recipient](#Recipient))|メッセージの**宛先**。|はい|いいえ|はい|
| UniqueBody| [ItemBody](#ItemBody)|会話に特有のメッセージの本文。|いいえ|いいえ|いいえ|
| UnsubscribeData | Collection (String) |`List-Unsubscribe` ヘッダーに基づいて解析された有効なエントリが含まれます。 **UnsubscribeEnabled** プロパティが `true` の場合、`List-Unsubscribe` ヘッダーの `mailto:` コマンドのデータが含まれます。 このデータは、[RFC-2369](http://www.faqs.org/rfcs/rfc2369.html) に準拠している必要があります。 対応する配布リストにメッセージがそれ以上送信されないようにするには、[購読取り消し](..\api\mail-rest-operations.md#Unsubscribe)アクションを使用します。 |いいえ |いいえ |いいえ |
| UnsubscribeEnabled | ブール値 | 対象メッセージで受信者が購読取り消しを行い、電子メール配布リストに基づいて今後メッセージが送信されないようにできるかどうかを示します。 `List-Unsubscribe` ヘッダーが [RFC-2369](http://www.faqs.org/rfcs/rfc2369.html) に準拠している場合には、`True` です。 | いいえ | いいえ | いいえ |
| WebLink | String|Outlook Web App でメッセージを開く URL。<br/><br/>URL の末尾に ispopout 引数を付加して、メッセージの表示方法を変更できます。 ispopout が存在しない、または 1 に設定されている場合は、メッセージがポップアウト ウィンドウに表示されます。 ispopout が 0 に設定されている場合、ブラウザーの Outlook Web App レビュー ウィンドウにメッセージが表示されます。<br/><br/>Outlook Web App のメールボックスにログインしている場合、ブラウザーでメッセージが開きます。 まだブラウザーでログインしていない場合、ログインするように求められます。<br/><br/>この URL には、iFrame 内からアクセスできます。 |いいえ|はい|いいえ|

<!-- Comment out for Mentions API 

Add these lines back to above table
| Mentions| Collection ([Mention](#MentionResource)) | A collection of mentions in the message, ordered by the **CreatedDateTime** from the newest to the oldest. By default, a `GET` message does not return this property unless you apply `$expand` on the property. Navigation property.|Yes|No|No|
| MentionsPreview| [MentionsPreview](#MentionsPreview) | Information about mentions in the message. When processing a `GET /messages` request, the server sets this property and includes it in the response by default. The server returns `null` if there are no mentions in the message. Optional. |No|No|No| 

Comment out for Mentions API -->



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|**検索可能**|
|:-----|:-----|:-----|:-----|:-----|:-----|
| 添付ファイル| Collection(Attachment)|メッセージの [FileAttachment](#FileAttachment) 添付ファイルと [ItemAttachment](#ItemAttachmentResource) 添付ファイル。 ナビゲーション プロパティです。|はい|いいえ|はい|
| BccRecipients| Collection ([Recipient](#Recipient))|メッセージの BCC 受信者。|はい|いいえ|はい|
| <a name="MessageBody">Body</a> | [ItemBody](#ItemBody)|メッセージの本文。|はい|いいえ|既定|
| BodyPreview| String|メッセージ本文の内容の最初の 255 文字。|いいえ|いいえ|はい|
| Categories| Collection (String)|メッセージに関連付けられたカテゴリ。|はい|はい|はい|
| CcRecipients| Collection ([Recipient](#Recipient))|メッセージの CC 受信者。|はい|いいえ|はい|
| ChangeKey| String|メッセージのバージョン。|いいえ|いいえ|いいえ|
| ConversationId| String|電子メールが属している会話の ID。|いいえ|はい|いいえ|
| CreatedDateTime| datetimeoffset|メッセージが作成された日時です。|いいえ|はい|いいえ|
| Extensions| Collection(Extension) |メッセージに対して定義されているオープン型のデータ拡張機能のコレクション。ナビゲーション プロパティです。 |いいえ |はい |いいえ|
| From|[Recipient](#Recipient)|メッセージのメールボックス所有者と送信者。|はい|はい|はい|
| HasAttachments| boolean|メッセージに添付ファイルがあるかどうかを示します。|いいえ|はい|はい|
| Id| String| メッセージの一意識別子。|いいえ|いいえ|いいえ|
| Importance| Importance|次のようなメッセージの重要度があります。低 = 0、標準 = 1、高 = 2。|はい|はい|はい|
| InferenceClassification | [InferenceClassificationType](#InferenceClassificationTypeV2) | 推定される関連性や重要性、または明示的なオーバーライドに基づく、ユーザーの対象メッセージの分類。 |はい|はい|はい|
| IsDeliveryReceiptRequested| boolean|メッセージの開封応答が要求されているかどうかを示します。|はい|はい|いいえ|
| IsDraft| ブール値|メッセージが下書きかどうかを示します。メッセージがまだ送信されていなければ下書きです。|いいえ|はい|いいえ|
| IsRead| boolean|メッセージが読み取られたかどうかを示します。|はい|はい|いいえ|
| IsReadReceiptRequested| boolean|メッセージの開封応答が要求されているかどうかを示します。|はい|はい|いいえ|
| LastModifiedDateTime| datetimeoffset|メッセージが最後に変更された日時です。|いいえ|はい|いいえ|
| MultiValueExtendedProperties | collection |  [MultiValueLegacyExtendedProperty](#MultiValueLegacyExtendedPropertyResource_v2) 型の複数値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい | いいえ |
| ParentFolderId| String|メッセージの親フォルダーの一意の識別子。|いいえ|いいえ|いいえ|
| ReceivedDateTime| datetimeoffset|メッセージが受信された日時です。|いいえ|はい|はい|
| ReplyTo| Collection ([Recipient](#Recipient))|返信時に使用される電子メール アドレス。|いいえ|いいえ|いいえ|
| Sender| [Recipient](#Recipient)|メッセージを生成するために実際に使用されるアカウント。|はい|はい|既定|
| SingleValueExtendedProperties | collection | [SingleValueLegacyExtendedProperty](#SingleValueLegacyExtendedPropertyResource_v2) 型の単一値拡張プロパティのコレクション。 これはナビゲーションのプロパティです。 詳細については、「[拡張プロパティ](#ExtendedProperties)」を参照してください。 | はい | はい | いいえ |
| SentDateTime| datetimeoffset|メッセージが送信された日時です。|いいえ|はい|いいえ|
| Subject| String|メッセージの件名を指定します。|はい|はい|既定|
| ToRecipients| Collection ([Recipient](#Recipient))|メッセージの**宛先**。|はい|いいえ|はい|
| UniqueBody| [ItemBody](#ItemBody)|会話に特有のメッセージの本文。|いいえ|いいえ|いいえ|
| WebLink | String|Outlook Web App でメッセージを開く URL。<br/><br/>URL の末尾に ispopout 引数を付加して、メッセージの表示方法を変更できます。 ispopout が存在しない、または 1 に設定されている場合は、メッセージがポップアウト ウィンドウに表示されます。 ispopout が 0 に設定されている場合、ブラウザーの Outlook Web App レビュー ウィンドウにメッセージが表示されます。<br/><br/>Outlook Web App のメールボックスにログインしている場合、ブラウザーでメッセージが開きます。 まだブラウザーでログインしていない場合、ログインするように求められます。<br/><br/>この URL には、iFrame 内からアクセスできます。 |いいえ|はい|いいえ|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|**検索可能**|
|:-----|:-----|:-----|:-----|:-----|:-----|
| 添付ファイル| Collection(Attachment)|メッセージの [FileAttachment](#FileAttachment) 添付ファイルと [ItemAttachment](#ItemAttachmentResource) 添付ファイル。 ナビゲーション プロパティです。|はい|いいえ|はい|
| BccRecipients| Collection ([Recipient](#Recipient))|メッセージの BCC 受信者。|はい|いいえ|はい|
| <a name="MessageBody">Body</a> | [ItemBody](#ItemBody)|メッセージの本文。|はい|いいえ|既定|
| BodyPreview| String|メッセージ本文の内容の最初の 255 文字。|いいえ|いいえ|はい|
| Categories| Collection (String)|メッセージに関連付けられたカテゴリ。|はい|はい|はい|
| CcRecipients| Collection ([Recipient](#Recipient))|メッセージの CC 受信者。|はい|いいえ|はい|
| ChangeKey| String|メッセージのバージョン。|いいえ|いいえ|いいえ|
| ConversationId| String|電子メールが属している会話の ID。|いいえ|はい|いいえ|
| DateTimeCreated| datetimeoffset|メッセージが作成された日時です。|いいえ|はい|いいえ|
| DateTimeLastModified| datetimeoffset|メッセージが最後に変更された日時です。|いいえ|はい|いいえ|
| DateTimeReceived| datetimeoffset|メッセージが受信された日時です。|いいえ|はい|はい|
| DateTimeSent| datetimeoffset|メッセージが送信された日時です。|いいえ|はい|いいえ|
| From|[Recipient](#Recipient)|メッセージのメールボックス所有者と送信者。|はい|はい|はい|
| HasAttachments| boolean|メッセージに添付ファイルがあるかどうかを示します。|はい|はい|はい|
| Id| String| メッセージの一意識別子。|いいえ|いいえ|いいえ|
| Importance| Importance|次のようなメッセージの重要度があります。低 = 0、標準 = 1、高 = 2。|はい|はい|はい|
| IsDeliveryReceiptRequested| boolean|メッセージの開封応答が要求されているかどうかを示します。|はい|はい|いいえ|
| IsDraft| ブール値|メッセージが下書きかどうかを示します。メッセージがまだ送信されていなければ下書きです。|いいえ|はい|いいえ|
| IsRead| boolean|メッセージが読み取られたかどうかを示します。|はい|はい|いいえ|
| IsReadReceiptRequested| boolean|メッセージの開封応答が要求されているかどうかを示します。|はい|はい|いいえ|
| ParentFolderId| String|メッセージの親フォルダーの一意の識別子。|いいえ|いいえ|いいえ|
| ReplyTo| Collection ([Recipient](#Recipient))|返信時に使用される電子メール アドレス。|はい|いいえ|いいえ|
| Sender| [Recipient](#Recipient)|メッセージを生成するために実際に使用されるアカウント。|はい|はい|既定|
| Subject| String|メッセージの件名を指定します。|はい|はい|既定|
| ToRecipients| Collection ([Recipient](#Recipient))|メッセージの**宛先**。|はい|いいえ|はい|
| UniqueBody| [ItemBody](#ItemBody)|会話に特有のメッセージの本文。|いいえ|いいえ|いいえ|
| WebLink | String|Outlook Web App でメッセージを開く URL。<br/><br/>URL の末尾に ispopout 引数を付加して、メッセージの表示方法を変更できます。 ispopout が存在しない、または 1 に設定されている場合は、メッセージがポップアウト ウィンドウに表示されます。 ispopout が 0 に設定されている場合、ブラウザーの Outlook Web App レビュー ウィンドウにメッセージが表示されます。<br/><br/>Outlook Web App のメールボックスにログインしている場合、ブラウザーでメッセージが開きます。 まだブラウザーでログインしていない場合、ログインするように求められます。<br/><br/>この URL には、iFrame 内からアクセスできます。 |いいえ|はい|いいえ|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


**Body プロパティからのスクリプトの削除**

メッセージ本文は、HTML またはテキストのいずれかにできます。 本文が HTML の場合、既定では、[Body](..\api\complex-types-for-mail-contacts-calendar.md#MessageBody) プロパティに組み込まれている安全ではない可能性がある HTML (たとえば、JavaScript など) が、本文の内容が REST 応答で返される前に削除されます。 

元の HTML コンテンツ全体を取得するには、次の HTTP 要求ヘッダーを含めます。
```
Prefer: outlook.allow-unsafe-html
```

**From プロパティと Sender プロパティの設定**

メッセージが作成されるとき、ほとんどの場合には **From** プロパティと **Sender** プロパティは、同じサインイン ユーザーを示します。ただし、次のシナリオで説明されているように、どちらかが更新される場合は例外です。
- **From** プロパティは、Exchange 管理者がメールボックスの **SendAs** 権限を他のユーザーに割り当てた場合には変更が可能です。 管理者は、Azure 管理ポータルでメールボックス所有者の**メールボックスのアクセス許可**を選択するか、[Exchange 管理センター](https://technet.microsoft.com/en-us/library/jj200743%28v=exchg.150%29.aspx)または [Windows PowerShell Add-ADPermission コマンドレット](https://technet.microsoft.com/en-us/library/bb124403%28v=exchg.150%29.aspx)を使用してこれを行えます。 その後、プログラムを使用して、**From** プロパティを、対象メールボックスの **SendAs** 権限を持ついずれかのユーザーに自動的に設定できます。

- **Sender** プロパティは、メールボックス所有者が 1 人以上のユーザーにそのメールボックスからメッセージを送信する権限を委任すると、変更できます。 メールボックス所有者は、Outlook で委任できます。 代理人がメールボックス所有者に代わってメッセージを送信する場合、**Sender** プロパティは代理人のアカウントに設定され、**From** プロパティはメールボックス所有者のままになります。 プログラムを使用して、**Sender** プロパティは、対象メールボックスの委任権限を取得したユーザーに設定できます。





<a name="PhotoResource"> </a>
### Photo


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

型:**Microsoft.OutlookServices.Photo**

Exchange Online からアクセスされる写真。 base 64 でエンコードされていないバイナリ データです。

|プロパティ|型|説明|書き込み可能|フィルター処理可能|
|:-----|:-----|:-----|:-----|:-----|
|Height| int|写真の高さ|いいえ|いいえ|
|Id| String| 写真の一意識別子。|いいえ|いいえ|
|Width| int|写真の幅。|いいえ|いいえ|




[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

型:**Microsoft.OutlookServices.Photo**

Exchange Online からアクセスされる写真。 base 64 でエンコードされていないバイナリ データです。

|プロパティ|型|説明|書き込み可能|フィルター処理可能|
|:-----|:-----|:-----|:-----|:-----|
|Height| int|写真の高さ|いいえ|いいえ|
|Id| String| 写真の一意識別子。|いいえ|いいえ|
|Width| int|写真の幅。|いいえ|いいえ|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在 v2.0 とベータ版で利用できます。詳細については、記事の右上隅にコントロールを使用して、どちらかのバージョンを選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="ReferenceAttachmentResource"></a>
### ReferenceAttachment (プレビュー)  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

型:**Microsoft.OutlookServices.ReferenceAttachment**

基本型： **Microsoft.OutlookServices.Attachment**

メッセージまたはイベントに添付されているファイルまたはフォルダーへのリンク。ファイルまたはフォルダーの場所として、OneDrive、OneDrive for Business、DropBox が含まれます。[Attachment](#Attachment) エンティティから派生します。

|プロパティ|型|説明|書き込み可能|フィルター処理可能|
|:-----|:-----|:-----|:-----|:-----|
| ContentType  | String | 添付ファイルの MIME タイプ。省略可能。| はい | いいえ |
| Id| String| 参照添付ファイルの一意の識別子。|いいえ|いいえ|
| IsFolder|ブール型|添付ファイルがフォルダーへのリンクであるかどうかを指定します。**SourceUrl** がフォルダーへのリンクの場合、true に設定する必要があります。省略可能。| はい | いいえ |
| IsInline | ブール型 | 添付ファイルがインライン添付ファイルの場合は `true`、それ以外の場合は `false`。 省略可能。 | はい | はい |
| LastModifiedDateTime | DateTimeOffset | 添付ファイルが最後に変更された日時。Timestamp 型は、ISO 8601 形式を使用して日付と時刻の情報を表し、必ず UTC 時間です。たとえば、2014 年 1 月 1 日午前 0 時 (UTC) は、次のようになります。'2014-01-01T00:00:00Z'。省略可能。 | いいえ | はい |
| 名前 | String | 添付ファイルの表示名。実際のファイル名である必要はありません。必須。 | はい | はい |
| アクセス許可|[ReferenceAttachmentPermissions](#ReferenceAttachmentPermissions)|**ProviderType** のプロバイダーの種類によって、添付ファイルに付与されるアクセス許可を指定します。 可能な値は、`Other`、`View`、`Edit`、`AnonymousView`、`AnonymousEdit`、`OrganizationView`、`OrganizationEdit` です。 省略可能。| はい | いいえ | 
| PreviewUrl|String|プレビュー イメージを取得するための、イメージの URL の参照添付ファイルのみに適用されます。**SourceUrl** によってイメージ ファイルが識別される場合にのみ、**ThumbnailUrl** と **PreviewUrl** を使用します。省略可能。| はい | いいえ |
| ProviderType|[ReferenceAttachmentProviders](#ReferenceAttachmentProviders)|この **ContentType** の添付ファイルをサポートするプロバイダーの種類。 可能な値は、`Other`、`OneDriveBusiness`、`OneDriveConsumer`、`Dropbox` です。 省略可能。| はい | はい |
| Size | Int32 | 添付ファイルの長さ (バイト単位)。省略可能。| いいえ | いいえ |
| SourceUrl|String|添付ファイルの内容を取得するための URL。フォルダーへの URL の場合、Outlook または Outlook on the web 上でフォルダーが正しく表示されるために、**IsFolder** を true に設定します。必須。| はい | いいえ |
| ThumbnailUrl|String|サムネイル イメージを取得するための、イメージの URL の参照添付ファイルのみに適用されます。**SourceUrl** によってイメージ ファイルが識別される場合にのみ、**ThumbnailUrl** と **PreviewUrl** を使用します。省略可能。| はい | いいえ |




[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="TaskResource"> </a>
### Task (プレビュー)   <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

型:**Microsoft.OutlookServices.Task**

作業アイテムを追跡することができる Outlook アイテム。タスクを使用すると、開始日時、期限日時、実際の完了日時、進行状況や状態、定期的かどうか、通知が必要かどうかを追跡できます。

**注意**

日付に関連する次の各プロパティについて:
- **CompletedDateTime**
- **CreatedDateTime**
- **DueDateTime**
- **LastModifiedDateTime**
- **ReminderDateTime**
- **StartDateTime**

プロパティが設定されている場合、既定で Task REST API の REST 応答では UTC で返されます。
詳細については、「[StartDateTime と DueDateTime の設定](..\api\task-rest-operations.md#NoteAboutStartDueDateTimes)」と「[カスタム タイム ゾーンの日付関連プロパティを返す](..\api\task-rest-operations.md#NoteAboutPreferHeader)」を参照してください。


|プロパティ|型|説明|書き込み可能|フィルター処理可能|
|:-----|:-----|:-----|:-----|:-----|
|AssignedTo |String |タスクが割り当てられているユーザーの名前。 |いいえ |はい |
|Body|[ItemBody](#ItemBody)|通常はタスクに関する情報を含むタスク本体。 HTML 型のみがサポートされていることに注意してください。|はい|いいえ|
|Categories |Collection(String) |タスクに関連付けられたカテゴリ。|はい |はい|
|ChangeKey|String |タスクのバージョン。|いいえ|いいえ|
|CompletedDateTime| [DateTimeTimeZone](#DateTimeTimeZoneBeta)|タスクが終了した日付 (指定のタイム ゾーン)。|はい|はい|
|CreatedDateTime |DateTimeOffset |タスクが作成された日時。既定では、UTC 時間です。要求ヘッダーでカスタム タイム ゾーンを使用できます。 |いいえ |はい |
|DueDateTime|[DateTimeTimeZone](#DateTimeTimeZoneBeta) |タスクが終了する予定の日時 (指定のタイム ゾーン)。 |はい|はい|
|Id| String|タスクの一意識別子。|いいえ|いいえ|
|Importance |Importance |次のようなイベントの重要度があります。低 = 0、標準 = 1、高 = 2。 |はい |はい |
|IsReminderOn| boolean|ユーザーにタスクを通知するアラートを設定する場合は、true に設定します。|はい|いいえ|
|LastModifiedDateTime |DateTimeOffset |タスクが最後に変更された日時。既定では、UTC 時間です。要求ヘッダーでカスタム タイム ゾーンを使用できます。 |いいえ |はい |
|Owner |String |タスクを作成したユーザーの名前。 |いいえ |はい |
|ParentFolderId| String|タスクの親フォルダーの一意の識別子。|いいえ|いいえ|
|Recurrence|[PatternedRecurrence](#PatternedRecurrence) |タスクの繰り返しパターン。|はい|いいえ|
|ReminderDateTime| [DateTimeTimeZone](#DateTimeTimeZoneBeta)|タスクのアラーム通知を行う日時。 |はい|いいえ|
|StartDateTime |[DateTimeTimeZone](#DateTimeTimeZoneBeta) |タスクを開始する日付 (指定のタイム ゾーン)。 |はい |はい |
|Status|[TaskStatus](#TaskStatus) |タスクの状態または進行状況を示します。未開始 = 0、進行中 = 1、完了 = 2、他を待機中 = 3、遅延 = 4。|はい|はい|
|Subject| String|タスクのタイトルまたは簡単な説明。|はい|はい|



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="TaskFolderResource"> </a>
### TaskFolder (プレビュー)   <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

型:**Microsoft.OutlookServices.TaskFolder**

タスクを格納するフォルダー。 Outlook では、既定のタスク グループ `My Tasks` には、ユーザーのメールボックス用の既定のタスク フォルダー `Tasks` が含まれています。 これらの既定のタスク グループとフォルダーの名前を変更したり削除したりすることはできませんが、タスク グループとタスク フォルダーを作成することはできます。

|プロパティ|型|説明|書き込み可能|フィルター処理可能|
|:-----|:-----|:-----|:-----|:-----|
|ChangeKey|String |タスク フォルダーのバージョン。|いいえ|いいえ|
|Id| String|タスク フォルダーの一意識別子。|いいえ|いいえ|
|IsDefaultFolder|ブール型|フォルダーが既定のタスク フォルダーである場合は true。|いいえ|はい| 
|名前| String|タスク フォルダーの名前。|はい|はい|
|ParentGroupKey| Edm.Guid|タスク フォルダーの親グループの一意の GUID 識別子。|いいえ|いいえ|
|Tasks|Collection([Task](#TaskResource)) |対象タスク フォルダー内のタスク。 ナビゲーション プロパティです。|いいえ|いいえ|



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="TaskGroupResource"> </a>
### TaskGroup (プレビュー)   <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

型:**Microsoft.OutlookServices.TaskGroup**

タスクを格納するフォルダーのグループ。 Outlook には、名前を変更または削除することができない既定のタスク グループ `My Tasks` があります。 ただし、タスク グループを作成することはできます。  

|プロパティ|型|説明|書き込み可能|フィルター処理可能
|:-----|:-----|:-----|:-----|:-----|
|ChangeKey|String |タスク グループのバージョン。|いいえ|いいえ|
|GroupKey| Edm.Guid|タスク グループの一意の GUID 識別子。|いいえ|いいえ|
|Id| String|タスク グループの一意の識別子。|いいえ|いいえ|
|IsDefaultGroup|ブール型|タスク グループが既定のタスク グループの場合は true。|いいえ|はい| 
|名前| String|タスク グループの名前。|はい|はい|
|TaskFolders|Collection([TaskFolder](#TaskFolderResource)) |対象タスク グループ内のタスク フォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

この機能は現在ベータ版として利用できます。詳細については、記事の右上隅にコントロールを使用して、**[ベータ版]** を選択します。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="UserResource"> </a>
### ユーザー

システム内のユーザーです。 **Me** エンドポイントが、現在のユーザーを SMTP アドレスで指定するためのショートカット (`users/sadie@contoso.com`) として提供されます。

型:**Microsoft.OutlookServices.User**

`Users` コレクションは、OData 応答の **value** プロパティでユーザーの配列を返します。
`$count` を使用してコレクション内のエンティティ数 (`.../me/users/$count`) を取得します。

**注** **User** エンティティには、多くのプロパティとリレーションシップ (ナビゲーション プロパティ) が含まれていて、頻繁に拡張されます。 次のセクションでは、その一部のみを取り上げます。 最新の情報については、ご使用のバージョンの[メタデータ ファイル](#Resources)にある **User** 定義を参照してください。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| Alias| String|ユーザーのエイリアス。通常は、ユーザーの SMTP アドレスです。|はい|はい|
| Calendar|[Calendar](#CalendarResource)|ユーザーの標準予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
| CalendarGroups| Collection([CalendarGroup](#CalendarGroupResource))|ユーザーの予定表グループ。 ナビゲーション プロパティです。|いいえ|いいえ|
| Calendars| Collection([Calendar](#CalendarResource))|ユーザーの予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
| CalendarView| Collection([Event](#EventResource))|予定表のカレンダー ビュー。 ナビゲーション プロパティです。|いいえ|いいえ|
| ContactFolders| Collection([ContactFolder](#ContactFolderResource))|ユーザーの連絡先フォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|
| Contacts| Collection([Contact](#ContactResource))|ユーザーの連絡先。 ナビゲーション プロパティです。|いいえ|いいえ|
| DisplayName| String|ユーザーの表示名。|はい|はい|
| Events| Collection([Event](#EventResource))|ユーザーのイベント。 既定は、既定の予定表の下にイベントを表示することです。 ナビゲーション プロパティです。|いいえ|いいえ|
| Id| String|ユーザーの一意の識別子。|いいえ|いいえ|
| InferenceClassification | [InferenceClassification](#InferenceClassificationResource) | 明示的な指定に基づく、ユーザーのメッセージの関連性の分類。明示的な指定は、推定される関連性や重要性より優先されます。 ナビゲーション プロパティです。|はい|はい|
| MailboxGuid| guid|ユーザーのメールボックスに割り当てられた GUID です。|いいえ|はい|
| MailboxSettings | [MailboxSettings](#MailboxSettings) | サインイン ユーザーのプライマリ メールボックスの設定。 | はい | いいえ |
| MailFolders| Collection([MailFolder](#FolderResource))|メールボックス内のフォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|
| Messages| Collection([Message](#MessageResource))|メールボックスまたはフォルダー内のメッセージ。 ナビゲーション プロパティです。|いいえ|いいえ|
| RootFolder| [MailFolder](#FolderResource)|ユーザーのメールボックスのルート フォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| Alias| String|ユーザーのエイリアス。通常は、ユーザーの SMTP アドレスです。|はい|はい|
| Calendar|[Calendar](#CalendarResource)|ユーザーの標準予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
| CalendarGroups| Collection([CalendarGroup](#CalendarGroupResource))|ユーザーの予定表グループ。 ナビゲーション プロパティです。|いいえ|いいえ|
| Calendars| Collection([Calendar](#CalendarResource))|ユーザーの予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
| CalendarView| Collection([Event](#EventResource))|予定表のカレンダー ビュー。 ナビゲーション プロパティです。|いいえ|いいえ|
| ContactFolders| Collection([ContactFolder](#ContactFolderResource))|ユーザーの連絡先フォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|
| Contacts| Collection([Contact](#ContactResource))|ユーザーの連絡先。 ナビゲーション プロパティです。|いいえ|いいえ|
| DisplayName| String|ユーザーの表示名。|はい|はい|
| Events| Collection([Event](#EventResource))|ユーザーのイベント。 既定は、既定の予定表の下にイベントを表示することです。 ナビゲーション プロパティです。|いいえ|いいえ|
| Id| String|ユーザーの一意の識別子。|いいえ|いいえ|
| InferenceClassification | [InferenceClassification](#InferenceClassificationResource) | 明示的な指定に基づく、ユーザーのメッセージの関連性の分類。明示的な指定は、推定される関連性や重要性より優先されます。 ナビゲーション プロパティです。|はい|はい|
| MailboxGuid| guid|ユーザーのメールボックスに割り当てられた GUID です。|いいえ|はい|
| MailFolders| Collection([MailFolder](#FolderResource))|メールボックス内のフォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|
| Messages| Collection([Message](#MessageResource))|メールボックスまたはフォルダー内のメッセージ。 ナビゲーション プロパティです。|いいえ|いいえ|
| RootFolder| [MailFolder](#FolderResource)|ユーザーのメールボックスのルート フォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|



[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|**書き込み可能**|**フィルター処理可能**|
|:-----|:-----|:-----|:-----|:-----|
| Alias| String|ユーザーのエイリアス。通常は、ユーザーの SMTP アドレスです。|はい|はい|
| Calendar|[Calendar](#CalendarResource)|ユーザーの標準予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
| CalendarGroups| Collection([CalendarGroup](#CalendarGroupResource))|ユーザーの予定表グループ。 ナビゲーション プロパティです。|いいえ|いいえ|
| Calendars| Collection([Calendar](#CalendarResource))|ユーザーの予定表。 ナビゲーション プロパティです。|いいえ|いいえ|
| CalendarView| Collection([Event](#EventResource))|予定表のカレンダー ビュー。 ナビゲーション プロパティです。|いいえ|いいえ|
| ContactFolders| Collection([ContactFolder](#ContactFolderResource))|ユーザーの連絡先フォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|
| Contacts| Collection([Contact](#ContactResource))|ユーザーの連絡先。 ナビゲーション プロパティです。|いいえ|いいえ|
| DisplayName| String|ユーザーの表示名。|はい|はい|
| Events| Collection([Event](#EventResource))|ユーザーのイベント。 既定は、既定の予定表の下にイベントを表示することです。 ナビゲーション プロパティです。|いいえ|いいえ|
| Folders| Collection([Folder](#FolderResource))|メールボックスまたはフォルダー内のフォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|
| Id| String|ユーザーの一意の識別子。|いいえ|いいえ|
| MailboxGuid| guid|ユーザーのメールボックスに割り当てられた GUID です。|いいえ|はい|
| Messages| Collection([Message](#MessageResource))|メールボックスまたはフォルダー内のメッセージ。 ナビゲーション プロパティです。|いいえ|いいえ|
| RootFolder| [Folder](#FolderResource)|ユーザーのメールボックスのルート フォルダー。 ナビゲーション プロパティです。|いいえ|いいえ|


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->







<a name="ComplexTypes"> </a>
### 複合型  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


[Attendee](#Attendee) (プレビュー) | [AttendeeBase](#AttendeeBase) (プレビュー) | [AttendeeAvailability](#AttendeeAvailability) (プレビュー) | [AutomaticRepliesSetting](#AutomaticRepliesSetting) (プレビュー) |[DateTimeTimeZone](#DateTimeTimeZoneBeta) | [EmailAddress](#EmailAddress) | 
[FollowUpFlag](#FollowUpFlag) (プレビュー) | [GeoCoordinates](#GeoCoordinates) | [ItemBody](#ItemBody)  | [LocaleInfo](#LocaleInfo) | 
[Location](#Location) (プレビュー) | [LocationConstraint](#LocationConstraint) (プレビュー) | [MailboxSettings](#MailboxSettings) (プレビュー) | [MeetingTimeCandidate](#MeetingTimeCandidate) (プレビュー) | [MeetingTimeCandidatesResult](#MeetingTimeCandidatesResult) (プレビュー) | 
<!-- Comment out for Mentions API
[MentionsPreview](#MentionsPreview) (preview) | 
Comment out for Mentions API -->
[PatternedRecurrence](#PatternedRecurrence) | [Phone](#Phone)  (プレビュー)| [PhysicalAddress](#PhysicalAddress) | 
[Recipient](#Recipient) | [RecurrencePattern](#RecurrencePattern) | [RecurrenceRange](#RecurrenceRange) | [ResponseStatus](#ResponseStatus) | [TimeConstraint](#TimeConstraint) (プレビュー) | [TimeSlot](#TimeSlot) (プレビュー) | [TimeStamp](#TimeStamp) (プレビュー) | [Website](#Website)  (プレビュー)


<a name="Attendee"> </a>
 **Attendee (プレビュー)**

イベントの参加者です。

型:**Microsoft.OutlookServices.AttendeeBase**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Status| [ResponseStatus](#ResponseStatus)|応答 (なし、承諾、辞退など) と時刻。|


<a name="AttendeeBase"></a>
**AttendeeBase (プレビュー)**

出席者の種類です。

型:**Microsoft.OutlookServices.Recipient**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|種類| AttendeeType |出席者の種類です。必須 = 0、リソース = 2。 現時点では、出席者が 1 人である場合、[FindMeetingTimes](..\api\calendar-rest-operations.md#FindMeetingTimesPreview) では常にその人は `Required` 型と見なされます。|



<a name="AttendeeAvailability"></a>
**AttendeeAvailability (プレビュー)**

出席者の種類と空き時間情報。

型:**Microsoft.OutlookServices.AttendeeAvailability**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Attendee| [AttendeeBase](#AttendeeBase) |出席者の種類 (人またはリソースのいずれか)。 |
| Availability | [FreeBusyStatus](#FreeBusyStatus) | 出席者の空き時間の状態。 |


<a name="AutomaticRepliesSetting"></a>
**AutomaticRepliesSetting (プレビュー)** 

サインイン ユーザーからのメッセージを使用して、着信メールの送信者に自動的に通知する構成設定。たとえば、サインイン ユーザーが電子メールに返信できないことを通知する自動返信などです。 

型:**Microsoft.OutlookServices.AutomaticRepliesSetting**

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| ExternalAudience | [ExternalAudienceScope](#ExternalAudienceScope) | **Status** が `AlwaysEnabled` または `Scheduled` の場合に、**ExternalReplyMessage** を受信する、サインイン ユーザーの組織外の一連の対象ユーザー。 値は、`None` = 0、`ContactsOnly` = 1、または `All` = 2 です。 |
| ExternalReplyMessage | String | **Status** が `AlwaysEnabled` または `Scheduled` の場合、指定の外部対象ユーザーに送信される自動応答。 |
| InternalReplyMessage | String | **Status** が `AlwaysEnabled` または `Scheduled` の場合、サインイン ユーザーの組織内の対象ユーザーに送信される自動応答。 |
| ScheduledEndDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta) | **Status** が `Scheduled` に設定されている場合に、自動応答を終了する日時。 `Prefer: outlook.timezone` HTTP ヘッダーで [Get](../api/mail-rest-operations.md#GetOnlyAutoReplySettings) 操作を使用して、タイム ゾーンを設定できます。|
| ScheduledStartDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta) | **Status** が `Scheduled` に設定されている場合に、自動応答を開始する日時。 `Prefer: outlook.timezone` HTTP ヘッダーで [Get](../api/mail-rest-operations.md#GetOnlyAutoReplySettings) 操作を使用して、タイム ゾーンを設定できます。 |
| Status | [AutomaticRepliesStatus](#AutomaticRepliesStatus) | 自動応答の構成状態。`Disabled` = 0、`AlwaysEnabled` = 1、`Scheduled` = 2 です。 |



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


[Attendee](#Attendeev10) | [DateTimeTimeZone](#DateTimeTimeZoneV2)| [EmailAddress](#EmailAddress) | 
[GeoCoordinates](#GeoCoordinates) | [ItemBody](#ItemBody)  | [Location](#Locationv10) | 
[PatternedRecurrence](#PatternedRecurrence) | [PhysicalAddress](#PhysicalAddress) | 
[Recipient](#Recipient) | [RecurrencePattern](#RecurrencePattern) | [RecurrenceRange](#RecurrenceRange) | [ResponseStatus](#ResponseStatus) 

<a name="Attendeev10"> </a>
 **Attendee**

イベントの参加者です。

型:**Microsoft.OutlookServices.Recipient**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Status| [ResponseStatus](#ResponseStatus)|応答 (なし、承諾、辞退など) と時刻。|
|Type| AttendeeType |出席者の種類です。必須 = 0、任意 = 1、リソース = 2。 |





[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

[Attendee](#Attendeev10) | [EmailAddress](#EmailAddress) | 
[GeoCoordinates](#GeoCoordinates) | [ItemBody](#ItemBody)  | [Location](#Locationv10) | 
[PatternedRecurrence](#PatternedRecurrence) | [PhysicalAddress](#PhysicalAddress) | 
[Recipient](#Recipient) | [RecurrencePattern](#RecurrencePattern) | [RecurrenceRange](#RecurrenceRange) | [ResponseStatus](#ResponseStatus) 


<a name="Attendeev10"> </a>
 **Attendee**

イベントの参加者です。

型:**Microsoft.OutlookServices.Recipient**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Status| [ResponseStatus](#ResponseStatus)|応答 (なし、承諾、辞退など) と時刻。|
|Type| AttendeeType |出席者の種類です。必須 = 0、任意 = 1、リソース = 2。 |

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<a name="DateTimeTimeZoneBeta"> </a>
**DateTimeTimeZone**

特定時点の日付、時刻、およびタイム ゾーンを記述します。

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|DateTime|DateTime|ISO 8601 形式に従って、特定時点の日付と時刻を組み合わせた表現 (`<date>T<time>`)|
|TimeZone|String|次のいずれかのタイム ゾーン名。|

_TimeZone_ プロパティは、Windows でサポートされている任意のタイム ゾーン、および次のタイム ゾーン名に設定できます。
詳細については、「<a href="https://technet.microsoft.com/en-us/library/cc749073(v=ws.10).aspx">TimeZone</a>」を参照してください。
 
Etc/GMT+12

Etc/GMT+11

太平洋/ホノルル

アメリカ/アンカレッジ

アメリカ/サンタイサベル

アメリカ/ロサンゼルス

アメリカ/フェニックス

アメリカ/チワワ

アメリカ/デンバー

アメリカ/グアテマラ

アメリカ/シカゴ

アメリカ/メキシコシティ

アメリカ/リジャイナ

アメリカ/ボゴタ

アメリカ/ニューヨーク

アメリカ/インディアナ/インディアナポリス

アメリカ/カラカス

アメリカ/アスンシオン

アメリカ/ハリファックス

アメリカ/クイアバ

アメリカ/ラパス

アメリカ/サンティアゴ

アメリカ/セントジョンズ

アメリカ/サンパウロ

アメリカ/アルゼンチン/ブエノスアイレス

アメリカ/カイエンヌ

アメリカ/ゴットホープ

アメリカ/モンテビデオ

アメリカ/バイア

Etc/GMT+2

大西洋/アゾレス諸島

大西洋/カーボベルデ

アフリカ/カサブランカ

Etc/GMT

ヨーロッパ/ロンドン

大西洋/レイキャビク

ヨーロッパ/ベルリン

ヨーロッパ/ブダペスト

ヨーロッパ/パリ

ヨーロッパ/ワルシャワ

アフリカ/ラゴス

アフリカ/ウィントフック

ヨーロッパ/ブカレスト

アジア/ベイルート

アフリカ/カイロ

アジア/ダマスカス

アフリカ/ヨハネスブルグ

ヨーロッパ/キエフ

ヨーロッパ/イスタンブール

アジア/エルサレム

アジア/アンマン

アジア/バグダッド

ヨーロッパ/カリーニングラード

アジア/リャド

アフリカ/ナイロビ

アジア/テヘラン

アジア/ドバイ

アジア/バクー

ヨーロッパ/モスクワ

インド/モーリシャス

アジア/トビリシ

アジア/エレバン

アジア/カブール

アジア/カラチ

アジア/タシケント

アジア/カルカッタ

アジア/コロンボ

アジア/カトマンズ

アジア/アルマトイ

アジア/ダッカ

アジア/エカテリンブルク

アジア/ヤンゴン

アジア/バンコク

アジア/ノボシビルスク

アジア/上海

アジア/クラスノヤルスク

アジア/シンガポール

オーストラリア/パース

アジア/台北

アジア/ウランバートル

アジア/イルクーツク

アジア/東京

アジア/ソウル

オーストラリア/アデレード

オーストラリア/ダーウィン

オーストラリア/ブリスベン

オーストラリア/シドニー

太平洋/ポートモレスビー

オーストラリア/ホバート

アジア/ヤクーツク

太平洋/ガダルカナル島

アジア/ウラジオストク

太平洋/オークランド

Etc/GMT-12

太平洋/フィジー

アジア/マガダン

太平洋/トンガタプ

太平洋/アピア

太平洋/クリスマス島

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<a name="DateTimeTimeZoneV2"> </a>
**DateTimeTimeZone**

特定時点の日付、時刻、およびタイム ゾーンを記述します。

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|DateTime|DateTime|ISO 8601 形式に従って、特定時点の日付と時刻を組み合わせた表現 (`<date>T<time>`)。|
|TimeZone|String|次のいずれかのタイム ゾーン名。|

_TimeZone_ プロパティは、Windows でサポートされている任意のタイム ゾーン、および次のタイム ゾーン名に設定できます。
詳細については、「<a href="https://technet.microsoft.com/en-us/library/cc749073(v=ws.10).aspx">TimeZone</a>」を参照してください。
 
Etc/GMT+12

Etc/GMT+11

太平洋/ホノルル

アメリカ/アンカレッジ

アメリカ/サンタイサベル

アメリカ/ロサンゼルス

アメリカ/フェニックス

アメリカ/チワワ

アメリカ/デンバー

アメリカ/グアテマラ

アメリカ/シカゴ

アメリカ/メキシコシティ

アメリカ/リジャイナ

アメリカ/ボゴタ

アメリカ/ニューヨーク

アメリカ/インディアナ/インディアナポリス

アメリカ/カラカス

アメリカ/アスンシオン

アメリカ/ハリファックス

アメリカ/クイアバ

アメリカ/ラパス

アメリカ/サンティアゴ

アメリカ/セントジョンズ

アメリカ/サンパウロ

アメリカ/アルゼンチン/ブエノスアイレス

アメリカ/カイエンヌ

アメリカ/ゴットホープ

アメリカ/モンテビデオ

アメリカ/バイア

Etc/GMT+2

大西洋/アゾレス諸島

大西洋/カーボベルデ

アフリカ/カサブランカ

Etc/GMT

ヨーロッパ/ロンドン

大西洋/レイキャビク

ヨーロッパ/ベルリン

ヨーロッパ/ブダペスト

ヨーロッパ/パリ

ヨーロッパ/ワルシャワ

アフリカ/ラゴス

アフリカ/ウィントフック

ヨーロッパ/ブカレスト

アジア/ベイルート

アフリカ/カイロ

アジア/ダマスカス

アフリカ/ヨハネスブルグ

ヨーロッパ/キエフ

ヨーロッパ/イスタンブール

アジア/エルサレム

アジア/アンマン

アジア/バグダッド

ヨーロッパ/カリーニングラード

アジア/リャド

アフリカ/ナイロビ

アジア/テヘラン

アジア/ドバイ

アジア/バクー

ヨーロッパ/モスクワ

インド/モーリシャス

アジア/トビリシ

アジア/エレバン

アジア/カブール

アジア/カラチ

アジア/タシケント

アジア/カルカッタ

アジア/コロンボ

アジア/カトマンズ

アジア/アルマトイ

アジア/ダッカ

アジア/エカテリンブルク

アジア/ヤンゴン

アジア/バンコク

アジア/ノボシビルスク

アジア/上海

アジア/クラスノヤルスク

アジア/シンガポール

オーストラリア/パース

アジア/台北

アジア/ウランバートル

アジア/イルクーツク

アジア/東京

アジア/ソウル

オーストラリア/アデレード

オーストラリア/ダーウィン

オーストラリア/ブリスベン

オーストラリア/シドニー

太平洋/ポートモレスビー

オーストラリア/ホバート

アジア/ヤクーツク

太平洋/ガダルカナル島

アジア/ウラジオストク

太平洋/オークランド

Etc/GMT-12

太平洋/フィジー

アジア/マガダン

太平洋/トンガタプ

太平洋/アピア

太平洋/クリスマス島

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->

<a name="EmailAddress"> </a>
**EmailAddress**

連絡先またはメッセージ受信者の名前と電子メール アドレスです。

型:**Microsoft.OutlookServices.EmailAddress**



<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|名前| String|個人またはエンティティの表示名。|
|Address| String|個人またはエンティティのメール アドレス。|

<!-- Comment out for Mentions API

Add this line back to table above

|ExternalId| String|Unique identifier for a person. Optional. In the context of mentions, set to the Azure Active Directory ID of a person who makes a mention or is mentioned. Supports filtering.|

Comment out for Mentions API -->

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|名前| String|個人またはエンティティの表示名。|
|Address| String|個人またはエンティティのメール アドレス。|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|名前| String|個人またはエンティティの表示名。|
|Address| String|個人またはエンティティの電子メール アドレス。|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->




<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<a name="FollowUpFlag"></a>
**FollowUpFlag (プレビュー)**

リソース インスタンスに関するフォローアップ情報です。

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| CompletedDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta) | フォローアップが終了した日時。 |
| DueDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta) | フォローアップが終了する予定の日時。 |
| StartDateTime | [DateTimeTimeZone](#DateTimeTimeZoneBeta) | フォローアップを開始する日時。 |
| FlagStatus | [FollowupFlagStatus](#FollowupFlagStatus) | 親リソース インスタンスにフォローアップのフラグを設定するか、またはフォローアップが終了したかどうかを表します。 |

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<a name="GeoCoordinates"> </a>
**GeoCoordinates**

場所の地理的座標と標高です。

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| Altitude | double | 場所の標高です。  |
| Latitude | double | 場所の緯度です。 |
| Longitude | double | 場所の経度です。 |
| Accuracy | double | 緯度と経度を提供するセンサーの精度です。 |
| AltitudeAccuracy | double | 標高を提供するセンサーの精度です。 |

<a name="ItemBody"> </a>
**ItemBody**

メッセージまたはイベントの本文の内容です。

型:**Microsoft.OutlookServices.ItemBody**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|ContentType| BodyType|次のようなコンテンツ タイプがあります。テキスト = 0、HTML = 1。|
|コンテンツ| String|テキストまたは HTML コンテンツ。|


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


<a name="LocaleInfo"> </a>
**LocaleInfo (プレビュー)**

サインインしているユーザーの優先言語および国/地域を含むロケールに関する情報。

型:**Microsoft.OutlookServices.LocaleInfo**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|DisplayName| String|たとえば "English (United States)" のように、ユーザーのロケールを自然言語で表す名前。|
|ロケール| String| ユーザーのロケールを表します。ユーザーの優先言語および国/地域が含まれます。 たとえば、"en-us"。 言語のコンポーネントは [ISO 639-1](http://www.iso.org/iso/home/standards/language_codes.htm) で定義されている 2 文字のコードに従い、国のコンポーネントは [ISO 3166-1 alpha-2](http://www.iso.org/iso/country_codes.htm) で定義されている 2 文字のコードに従います。|


<a name="Location"> </a>
 **Location (プレビュー)**

イベントの場所です。

型:**Microsoft.OutlookServices.Location**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Address|[PhysicalAddress](#PhysicalAddress)|場所の物理アドレス。|
|Coordinates|[GeoCoordinates](#GeoCoordinates)|場所の地理的座標と標高です。|
|DisplayName| String|場所に関連付けられた名前。|
|LocationEmailAddress| String | 場所の電子メール アドレス (省略可能)。 |
|LocationUri |String | 場所を表す URI (省略可能)。 |



<a name="LocationConstraint"> </a>
 **LocationConstraint (プレビュー)**

会議の場所に関して、クライアントが表明している条件です。

型:**Microsoft.OutlookServices.LocationConstraint**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|IsRequired| boolean |クライアントは、応答に会議の場所を含めるようにサービスに要求します。|
|SuggestLocation| boolean | クライアントは、1 つ以上の会議場所を提案するようサービスに要求します。 |
|Locations|Collection([Location](#Location)) | クライアントが会議のために要求する 1 つ以上の場所。|


<a name="MailboxSettings"></a>
**MailboxSettings (プレビュー)**

サインイン ユーザーのプライマリ メールボックスの設定。 着信メッセージに対する自動返信を送信するための設定が含まれます。

型:Microsoft.OutlookServices.MailboxSettings


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| AutomaticRepliesSetting | [AutomaticRepliesSetting](#AutomaticRepliesSetting) | 着信メッセージに対する返信を自動的に送信する構成の設定。 |
| TimeZone | String | ユーザーのメールボックスの既定のタイム ゾーン。 |
| 言語 | [LocaleInfo](#LocaleInfo) | 優先言語および国/地域を含むユーザーのロケール情報。 |


<a name="MeetingTimeCandidate"> </a>
**MeetingTimeCandidate (プレビュー)**

会議時間、出席の可能性、各出席者の空き時間情報、利用可能な会議場所を含む、会議の提案です。

型:**Microsoft.OutlookServices.MeetingTimeCandidate**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| MeetingTimeSlot | [TimeSlot](#TimeSlot) | 会議の提案されている期間。 |
| [Confidence](#ConfidenceScoreDetails) | double | すべての出席者が出席する見込みを表すパーセンテージ。 |
| OrganizerAvailability | [FreeBusyStatus](#FreeBusyStatus) | この提案されている会議の開催者の空き時間情報。`Free`、`Tentative`、`Busy`、`Oof`、`WorkingElsewhere`、`Unknown`。 |
| AttendeeAvailability | Collection([AttendeeAvailability](#AttendeeAvailability)) | この提案された会議の各出席者の空き時間情報の状態を示す配列。 |
| Locations | Collection([Location](#Location)) | この提案された会議の各会議場所の名前と地理的な場所を指定する配列。 |
| SuggestionHint | String |会議時間を提案する理由について記述します。 |


<a name="ConfidenceScoreDetails"></a>

**会議の確実性**

[MeetingTimeCandidate](#MeetingTimeCandidate) の **Confidence** プロパティの範囲は 0% ～ 100% で、各個人の空き時間状態に基づいて会議に出席するすべての出席者の見込みを表します。
- 各出席者に関する、指定の会議期間の空き状態で、確実に出席する場合は 100%、不明な状態は 50%、忙しい場合には 0% です。
- 会議時間の候補の確実性は、指定された対象会議のすべての出席者の出席見込みの平均によって算出されます。
- [FindMeetingTimes](..\api\calendar-rest-operations.md#FindMeetingTimesPreview) アクションは、候補の確実性が 50% 以上の会議時間候補を返します。
- 会議時間候補が複数ある場合、**FindMeetingTimes** アクションは、算出した確実性が高い方から順番に候補を並べて示します。確実性が同じ候補がある場合には、時系列で候補を並べて示します。


<a name="MeetingTimeCandidatesResult"> </a>
**MeetingTimeCandidatesResult (プレビュー)**

会議の提案がある場合にはそのコレクションを、ない場合にはその理由を示します。

型:**Microsoft.OutlookServices.MeetingTimeCandidatesResult**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| MeetingTimeSlots | Collection([MeetingTimeCandidate](#MeetingTimeCandidate)) | 会議提案の配列。 |
| EmptySuggestionsHint | String | 会議提案が 1 つも返されない理由。 可能な値は、`AttendeesUnavailable`、`LocationsUnavailable`、`OrganizerUnavailable`、`AttendeesUnavailableOrUnknown`、`Unknown` です。|

<a name="ReasonsNoMeetingSuggestion"></a>

**会議提案が 1 つも返されない理由**

**EmptySuggestionsHint** プロパティは、[FindMeetingTimes](..\api\calendar-rest-operations.md#FindMeetingTimesPreview) アクションによって会議提案が 1 つも返されない理由として次のいずれかを示します。**FindMeetingTimes** が何らかの会議提案を返す場合には、このプロパティは空の文字列になります。

|**値**|**理由**|
|:-----|:-----|
| AttendeesUnavailable | すべての出席者の空き時間情報を把握していますが、[会議の確実性](#ConfidenceScoreDetails)しきい値 (既定では 50%) に達するには出席者が足りません。 |
| AttendeesUnavailableOrUnknown | 一部またはすべての出席者の空き時間情報が不明なため、会議の確実性が設定されているしきい値 (既定では 50%) を下回っています。出席者が組織外の場合、または空き時間情報の取得でエラーが生じる場合には、出席者の空き時間情報が不明になることがあります。|
| LocationsUnavailable | [LocationConstraint](#LocationConstraint) の **IsRequired** プロパティが必須に指定されているものの、算出された時間範囲で利用可能な場所がありません。 |
| OrganizerUnavailable | **IsOrganizerOptional** パラメーターが false で、要求された期間中、主催者が現時点で出席可能ではありません。 |
| Unknown | 会議提案が 1 つも返されない理由が不明です。|



<a name="MentionsPreview"></a>

<!-- Comment out for Mentions API
Add second row back to table to format columns

**MentionsPreview (preview)**

Represents information about mentions in a resource instance.

Type: Microsoft.OutlookServices.MentionsPreview


|**Property**|**Type**|**Description**|

| IsMentioned | Boolean | True if the signed-in user is mentioned in the parent resource instance. Read-only. Supports filter. |


Comment out for Mentions API -->

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


<a name="Locationv10"> </a>
 **Location**

イベントの場所です。

型:**Microsoft.OutlookServices.Location**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|DisplayName| String|場所に関連付けられた名前。|
|Address|[PhysicalAddress](#PhysicalAddress)|場所の物理アドレス。|
|Coordinates|[GeoCoordinates](#GeoCoordinates)|場所の地理的座標と標高です。|


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<a name="Locationv10"> </a>
 **Location**

イベントの場所です。

型:**Microsoft.OutlookServices.Location**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|DisplayName| String|場所に関連付けられた名前。|
|Address|[PhysicalAddress](#PhysicalAddress)|場所の物理アドレス。|
|Coordinates|[GeoCoordinates](#GeoCoordinates)|場所の地理的座標と標高です。|


[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="PatternedRecurrence"> </a>
**PatternedRecurrence**

繰り返しのパターンと範囲です。

型:**Microsoft.OutlookServices.PatternedRecurrence**

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Pattern| [RecurrencePattern](#RecurrencePattern)|イベントの頻度。|
|RecurrenceTimeZone|String|開始時刻と終了時刻のタイム ゾーン。 サポートされているタイム ゾーンの一覧については [DateTimeTimeZone](#DateTimeTimeZoneBeta) 複合型をご覧ください。|
|Range| [RecurrenceRange](#RecurrenceRange)|イベントの期間。|

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Pattern| [RecurrencePattern](#RecurrencePattern)|イベントの頻度。|
|Range| [RecurrenceRange](#RecurrenceRange)|イベントの期間。|

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Pattern| [RecurrencePattern](#RecurrencePattern)|イベントの頻度。|
|Range| [RecurrenceRange](#RecurrenceRange)|イベントの期間。|

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<a name="Phone"> </a>
**Phone**

電話番号を表します。

型:**Microsoft.OutlookServices.Phone**

| プロパティ     | 型   |説明|
|:---------------|:--------|:----------|
|Number|string|電話番号。|
|Type|String|電話番号の種類。 可能な値は、`Home`、`Business`、`Mobile`、`Other`、`Assistant`、`HomeFax`、`BusinessFax`、`OtherFax`、`Pager`、`Radio` です。|

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<a name="PhysicalAddress"> </a>
**PhysicalAddress**

連絡先の住所です。

型:**Microsoft.OutlookServices.PhysicalAddress**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| Street| String|番地。|
| City| String|市区町村。|
| State| String|都道府県。|
| CountryOrRegion| String|国または地域。|
| PostalCode| String|郵便番号。|



<a name="Recipient"> </a>
 **Recipient**

イベントまたはメッセージの送信側または受信側のユーザーに関する情報を表します。

型:**Microsoft.OutlookServices.Recipient**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|EmailAddress|[EmailAddress](#EmailAddress)|受信者の電子メール アドレス。|



<a name="RecurrencePattern"> </a>
**RecurrencePattern**

イベントの頻度。

型:**Microsoft.OutlookServices.RecurrencePattern**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|種類|RecurrencePatternType|次のような繰り返しパターン タイプがあります。日単位 = 0、週単位 = 1、絶対月単位 = 2、相対月単位 = 3、絶対年単位 = 4、相対年単位 = 5。|
|Interval| Int32|発生と発生の間の指定された繰り返しタイプの単位数。|
|DayOfMonth| Int32|アイテムが発生する月の日付。|
|Month| Int32|アイテムが発生する月。これは、1 から 12 までの数字です。|
|DaysOfWeek| Collection(DayOfWeek)|次のような曜日のコレクションです。日曜日 = 0、月曜日 = 1、火曜日 = 2、水曜日 = 3、木曜日 = 4、金曜日 = 5、土曜日 = 6。|
|FirstDayOfWeek| DayOfWeek|次のような曜日があります。日曜日 = 0、月曜日 = 1、火曜日 = 2、水曜日 = 3、木曜日 = 4、金曜日 = 5、土曜日 = 6。|
|インデックス| WeekIndex|次のような週インデックスです。第 1 週 = 0、第 2 週 = 1、第 3 週 = 2、第 4 週 = 3、最後 = 4。|



<a name="RecurrenceRange"> </a>
 **RecurrenceRange**

イベントの期間。

型:**Microsoft.OutlookServices.RecurrenceRange**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|種類| RecurrenceRangeType|次のような繰り返し範囲があります。終了日 = 0、無制限 = 1、番号順 = 2。|
|StartDate| datetimeoffset|繰り返しの開始日です。|
|EndDate| datetimeoffset|繰り返しの終了日です。|
|NumberOfOccurrences| Int32|イベントを繰り返す回数。|



<a name="ResponseStatus"> </a>
 **ResponseStatus**

会議出席依頼の応答状態です。

型:**Microsoft.OutlookServices.ResponseStatus**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Response| ResponseType|次のような応答タイプがあります。なし = 0、主催者 = 1、一時承諾 = 2、承諾 = 3、辞退 = 4、無応答 = 5。|
|時刻| datetimeoffset|応答が返された日時。|




<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<a name="TimeConstraint"> </a>
 **TimeConstraint (プレビュー)**

指定した性質の活動の期間です。

型:**Microsoft.OutlookServices.TimeConstraint**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|ActivityDomain| ActivityDomain |会議の性質 (省略可能)。不明 = 0、仕事 = 1、私用 = 2。 現在、[FindMeetingTimes](..\api\calendar-rest-operations.md#FindMeetingTimesPreview) では、この値は常に `Work` であると想定し、開催者または出席者の勤務時間中の会議提案のみを返します。|
|Timeslots| Collection([TimeSlot](#TimeSlot)) | 期間の配列。 |



<a name="TimeSlot"> </a>
 **TimeSlot (プレビュー)**

期間です。

型:**Microsoft.OutlookServices.TimeSlot**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Start| [TimeStamp](#TimeStamp) |期間の開始時間。|
|End| TimeStamp|期間の終了時間。|


<a name="TimeStamp"> </a>
 **TimeStamp (プレビュー)**

特定の日時を表します。

型:**Microsoft.OutlookServices.TimeStamp**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
|Date| Edm.Date|タイムスタンプの日付部分。|
|Time| Edm.TimeOfDay|タイムスタンプの時刻部分。|
|TimeZone| String | タイムスタンプのタイム ゾーン部分。世界を 24 個の縦方向の領域に分けたうちのいずれかの領域です。 |



<a name="Website"></a>

Web サイトを表します。

型:**Microsoft.OutlookServices.Website**

|**プロパティ**|**型**|**説明**|
|:---------------|:--------|:----------|
|Address |String |Web サイトの URL。 |
|DisplayName |String |Web サイトの表示名。|
|Type|[WebsiteType](#WebsiteType)]| 連絡先に通常関連付けられる Web サイトの種類。 可能な値は、`Blog`、`Home`、`Other`、`Profile`、`Work` です。|



[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->

<a name="EnumTypes"> </a>
### 列挙型  <!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

[FreeBusyStatus](#FreeBusyStatus) | [AutomaticRepliesStatus](#AutomaticRepliesStatus) (プレビュー) | [ExternalAudienceScope](#ExternalAudienceScope) (プレビュー) | [FollowupFlagStatus](#FollowupFlagStatus) (プレビュー) | [InferenceClassificationType](#InferenceClassificationTypeBeta) | [PhoneType](#PhoneType)  (プレビュー) | [ReferenceAttachmentPermissions](#ReferenceAttachmentPermissions) (プレビュー) | [ReferenceAttachmentProviders](#ReferenceAttachmentProviders) (プレビュー) | [TaskStatus](#TaskStatus) (プレビュー) | [WebsiteType](#WebsiteType)

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

[FreeBusyStatus](#FreeBusyStatus) | [InferenceClassificationType](#InferenceClassificationTypeV2)  

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<a name="FreeBusyStatus"></a>
**FreeBusyStatus**

会議の出席者の空き時間状態を示します。

サポートされている値: 

- Busy
- Free
- Oof 
- Tentative
- Unknown 
- WorkingElsewhere


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<a name="InferenceClassificationTypeV2"></a>
**InferenceClassificationType**

注目するユーザーのメッセージの推定関連性を表します。

サポートされている値: 

- Focused
- Other 


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<a name="AutomaticRepliesStatus"></a>
**AutomaticRepliesStatus (プレビュー)**

ユーザーのメールボックスでメッセージを受信すると自動返信するための構成の状態です。

サポートされている値: 

- AlwaysEnabled
- Disabled
- Scheduled


<a name="ExternalAudienceScope"></a>
**ExternalAudienceScope (プレビュー)**

**ExternalReplyMessage** を送信する、外部出席者のセット。

サポートされている値: 

- All
- ContactsOnly
- None


<a name="FollowupFlagStatus"></a>
**FollowupFlagStatus (プレビュー)**

リソース インスタンスのフローアップ フラグの状態を表します。

サポートされている値: 

- Complete
- Flagged
- NotFlagged


<a name="InferenceClassificationTypeBeta"></a>
**InferenceClassificationType**

注目するユーザーのメッセージの推定関連性を表します。

サポートされている値: 

- Focused
- Other


<a name="PhoneType"></a>

**PhoneType (プレビュー)**

連絡先に通常関連付けられる電話番号の種類。

サポートされている値:

- Assistant
- Business
- BusinessFax
- Home
- HomeFax
- Mobile
- Other
- OtherFax
- Pager
- Radio



<a name="ReferenceAttachmentPermissions"></a>
**ReferenceAttachmentPermissions (プレビュー)**

参照添付ファイルのファイルまたはフォルダーのアクセス許可。

サポートされている値: 

- Other
- 表示
- Edit
- AnonymousView
- AnonymousEdit
- OrganizationView
- OrganizationEdit


<a name="ReferenceAttachmentProviders"></a>
**ReferenceAttachmentProviders (プレビュー)**

参照添付ファイルの可能なファイル ストレージ プロバイダー。 

サポートされている値: 

- Dropbox
- OneDriveBusiness
- OneDriveConsumer
- Other



<a name="TaskStatus"></a>
**TaskStatus (プレビュー)**

タスクの状態または進行状況を示します。

サポートされている値: 

- Completed
- Deferred
- InProgress
- NotStarted
- WaitingOnOthers
 


<a name="WebsiteType"></a>
**WebsiteType (プレビュー)**

連絡先に通常関連付けられる Web サイトの種類を指定します。

サポートされている値:

- Blog
- Home
- Other
- Profile
- Work


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<a name="UseOdataQueryParameters"> </a>
## OData クエリ パラメーターの使用

メール、予定表、および連絡先 API を操作するときに、標準の [OData v4.0](http://docs.oasis-open.org/odata/odata/v4.0/odata-v4.0-part1-protocol.html) クエリ パラメーターを使用してデータ要求をフィルター処理したり、結果を並べ替えてページングしたりすることができます。 クエリ パラメーターを指定するとき、[URI で特別な意味のために予約されている](https://tools.ietf.org/html/rfc3986#section-2.2)文字が適切にエンコードされていることを確認してください。 

-  **$search**: 特定の条件で検索します。

-  **$filter**: 特定の条件でフィルター処理します。

-  **$select**: 特定のプロパティを要求します。

-  **$orderby**: 結果を並び替えます。

-  **$top** および **$skip**: 結果をページングします。

-  **$expand**: メッセージの添付ファイルとイベントの添付ファイルを展開します。

-  **$count**: コレクション内のエンティティ数を取得します。 このパラメーターは URL パスに追加されます (`.../me/calendars/$count`)。

メール、予定表、および連絡先 API を使用した問い合わせでは、必ず、浅いスコープが使用されます。現在のフォルダー内のアイテムのみが返されます。深い検索はサポートされていません。

### 検索要求
<a name="Search"></a>

**$search** パラメーターを使用すれば、要求の結果を検索式と一致するメッセージに制限することができます。検索文字列は、高度な検索テクニック (AQS) を使用して表現されます。結果は、メッセージが送信された日時で並べ替えられます。

**メモ****$search** 要求から最大で 250 の結果を取得できます。メッセージにのみ **$search** を使用できます。連絡先と予定表イベントの検索はサポートされていません。

検索要求内では **$filter** も **$orderby** も使用できません。 使用した場合は、次のようなエラー メッセージが表示されます。

    {
      "error":
      {
        "code":"ErrorInvalidUrlQuery",
        "message":"The query parameter 'OrderBy' is invalid."
      }
    }

|プロパティ|説明|
|:-----|:-----|
| Attachment      | 指定された添付ファイルをタイトルで検索します。  |
| BCC             | [**Bcc**] フィールドを検索します。 |
| Body または Content | [**本文**] フィールドを検索します。 既定の検索でのみサポートされています。 |
| Category        | [**カテゴリ**] フィールドを検索します。 |
| CC              | [**CC**] フィールドを検索します。 |
| From            | [**差出人**] フィールドを検索します。 |
| Has             | [**添付ファイルあり**] フィールドを検索します。|
| Participants    | [**宛先**]、[**CC**]、[**BCC**] の各フィールドを検索します。 |
| Received        | MM/DD/YYYY として表現された特定の日付を [**受信日時**] フィールドで検索します。 |
| Sender          | [**送信者**] フィールドを検索します。 |
| Subject         | [**件名**] フィールドを検索します。 |
| To              | [**宛先**] フィールドを検索します。 |

共通フィールドは、プロパティを指定せずに **$search** クエリ パラメーターを使用して検索します。既定の検索では、**Body**、**Sender**、および **Subject** プロパティが検索されます。次の検索は、3 つの既定のプロパティのいずれかに「pizza」が含まれる受信トレイ内のすべてのメッセージを返します。

いくつかの例を見てみましょう。読みやすくするために、例内の URL はエンコードされていません。ただし、これらの例を試す場合は、URL をサーバーに送信する前にエンコードする必要があります。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


**From**、**Subject**、または **Body** プロパティ内に「Pizza」という単語を含む受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$search="pizza"
```

**Subject** プロパティ内に「Pizza」という単語を含む受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$search="subject:pizza"
```

特定の人から送信された受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。
```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$search="from:help@contoso.com"
```

上の例では、URL エンコードが含まれていませんでしたが、URL エンコードされ、サーバーに送信する準備ができた同じ例を以下に示します。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$search=%27pizza%27
GET https://outlook.office.com/api/beta/me/messages?$search=%27subject:pizza%27
GET https://outlook.office.com/api/beta/me/messages?$search=%27from:help@contoso.com%27
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


**From**、**Subject**、または **Body** プロパティ内に「Pizza」という単語を含む受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$search="pizza"
```

**Subject** プロパティ内に「Pizza」という単語を含む受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$search="subject:pizza"
```

特定の人から送信された受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。
```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$search="from:help@contoso.com"
```

上の例では、URL エンコードが含まれていませんでしたが、URL エンコードされ、サーバーに送信する準備ができた同じ例を以下に示します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$search=%27pizza%27
GET https://outlook.office.com/api/v2.0/me/messages?$search=%27subject:pizza%27
GET https://outlook.office.com/api/v2.0/me/messages?$search=%27from:help@contoso.com%27
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

**From**、**Subject**、または **Body** プロパティ内に「Pizza」という単語を含む受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$search="pizza"
```

**Subject** プロパティ内に「Pizza」という単語を含む受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$search="subject:pizza"
```

特定の人から送信された受信トレイ内のすべてのメッセージを取得する場合は、この要求を使用することができます。
```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$search="from:help@contoso.com"
```

上の例では、URL エンコードが含まれていませんでしたが、URL エンコードされ、サーバーに送信する準備ができた同じ例を以下に示します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$search=%27pizza%27
GET https://outlook.office.com/api/v1.0/me/messages?$search=%27subject:pizza%27
GET https://outlook.office.com/api/v1.0/me/messages?$search=%27from:help@contoso.com%27
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="Filter"> </a>
### フィルター処理要求

**$filter** クエリ パラメーターを使用すれば、次のフィルター演算子を使用して検索条件を指定することができます。

すべてのプロパティがフィルタリングをサポートしているわけではありません。前述した対応する表の [フィルター処理可能] 列が [はい] になっているリソース プロパティのみを使用できます。プロパティがフィルター処理できない場合は、**ChangeKey** プロパティでフィルター処理しようとしたときに返される次のようなエラー メッセージが応答で返されます。

    {
      "error":
      {
        "code":"ErrorInvalidProperty",
        "message":"The property 'ChangeKey' does not support filtering."
      }
    }

サポートされていないフィルタリング メソッドを使用した場合は、**Subject** プロパティに対して `startswith` フィルター メソッドを使用したときに返される次のようなエラー メッセージが表示されます。

    {
      "error":
      {
        "code":"ErrorInvalidUrlQueryFilter",
        "message":"'contains' and 'startswith' are not supported for filtering.  Use Search instead."
      }
    }


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

|**演算子**|**種類**|**例**|
|:-----|:-----|:-----|
|および|論理積 (複数の条件を結合するために使用する)| `TotalCount gt 0 and ChildFolderCount eq 0`|
|または|論理和 (複数の条件を結合するために使用する)| `TotalCount gt 0 or ChildFolderCount eq 0`|
|eq|等しい| `IsRead eq false`|
|ne|等しくない| `Importance ne Microsoft.Exchange.Services.OData.Model.Importance'High'`|
|gt|より大きい| `ReceivedDateTime gt 2014-09-01T00:00:00Z`|
|ge|以上| `LastModifiedDateTime ge 2014-09-01T00:00:00Z`|
|lt|より少ない| `ReceivedDateTime lt 2014-09-01T00:00:00Z`|
|le|以下| `LastModifiedDateTime le 2014-09-01T00:00:00Z`|

フィルター条件で任意の文字列値を区切るには、単一引用符 (') を使用します。 URL で単一引用符をエンコードするには、`%27` を使用します。 文字列自体では大文字と小文字の区別はありません。 

いくつかの例を見てみましょう。読みやすくするために、例内の URL はエンコードされていません。ただし、これらの例を試す場合は、URL をサーバーに送信する前にエンコードする必要があります。

ユーザーの既定の予定表で、指定した日付以降に開始されるすべてのイベントを取得するには、**Start** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/beta/me/events?$filter=Start/DateTime ge '2016-04-01T08:00'
```


ユーザーの予定表に含まれる特定の件名のイベントすべてを取得するには、**Subject** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/beta/me/events?$filter=Subject eq 'Mega Charity Bash'
```

受信トレイ内のすべての未読メッセージを取得する場合は、**IsRead** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$filter=IsRead eq false
```

受信トレイ内のすべての添付ファイル付きメッセージを取得する場合は、**HasAttachments** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$filter=HasAttachments eq true
```

2014 年 9 月 1 日以降に受信された受信トレイ内のすべてのメッセージを取得する場合は、**ReceivedDateTime** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$filter=ReceivedDateTime ge 2014-09-01
```

"hr@contoso.com" から送信された受信トレイ内のすべてのメッセージを取得する場合は、**Sender** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$filter=From/EmailAddress/Address eq 'hr@contoso.com'
```

上の例では、URL エンコードが含まれていませんでしたが、URL エンコードされ、サーバーに送信する準備ができた同じ例を以下に示します。

```no-highlight
GET https://outlook.office.com/api/beta/me/events?$filter=Start/DateTime%20ge%20%272016-04-01T08:00%27
GET https://outlook.office.com/api/beta/me/events?$filter=Subject%20eq%20%27Mega%20Charity%20Bash%27
GET https://outlook.office.com/api/beta/me/messages?$filter=IsRead%20eq%20false
GET https://outlook.office.com/api/beta/me/messages?$filter=HasAttachments%20eq%20true
GET https://outlook.office.com/api/beta/me/messages?$filter=ReceivedDateTime%20ge%202014-09-01
GET https://outlook.office.com/api/beta/me/messages?$filter=From/EmailAddress/Address%20eq%20%27hr@contoso.com%27
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

|**演算子**|**種類**|**例**|
|:-----|:-----|:-----|
|および|論理積 (複数の条件を結合するために使用する)| `TotalCount gt 0 and ChildFolderCount eq 0`|
|または|論理和 (複数の条件を結合するために使用する)| `TotalCount gt 0 or ChildFolderCount eq 0`|
|eq|等しい| `IsRead eq false`|
|ne|等しくない| `Importance ne Microsoft.Exchange.Services.OData.Model.Importance'High'`|
|gt|より大きい| `ReceivedDateTime gt 2014-09-01T00:00:00Z`|
|ge|以上| `LastModifiedDateTime ge 2014-09-01T00:00:00Z`|
|lt|より少ない| `ReceivedDateTime lt 2014-09-01T00:00:00Z`|
|le|以下| `LastModifiedDateTime le 2014-09-01T00:00:00Z`|

フィルター条件で任意の文字列値を区切るには、単一引用符 (') を使用します。 URL で単一引用符をエンコードするには、`%27` を使用します。 文字列自体では大文字と小文字の区別はありません。 

いくつかの例を見てみましょう。読みやすくするために、例内の URL はエンコードされていません。ただし、これらの例を試す場合は、URL をサーバーに送信する前にエンコードする必要があります。

ユーザーの既定の予定表で、指定した日付以降に開始されるすべてのイベントを取得するには、**Start** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/events?$filter=Start/DateTime ge '2016-04-01T08:00'
```


ユーザーの予定表に含まれる特定の件名のイベントすべてを取得するには、**Subject** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/events?$filter=Subject eq 'Mega Charity Bash'
```

受信トレイ内のすべての未読メッセージを取得する場合は、**IsRead** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$filter=IsRead eq false
```

受信トレイ内のすべての添付ファイル付きメッセージを取得する場合は、**HasAttachments** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$filter=HasAttachments eq true
```

2014 年 9 月 1 日以降に受信された受信トレイ内のすべてのメッセージを取得する場合は、**ReceivedDateTime** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$filter=ReceivedDateTime ge 2014-09-01
```

"hr@contoso.com" から送信された受信トレイ内のすべてのメッセージを取得する場合は、**Sender** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$filter=From/EmailAddress/Address eq 'hr@contoso.com'
```

上の例では、URL エンコードが含まれていませんでしたが、URL エンコードされ、サーバーに送信する準備ができた同じ例を以下に示します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/events?$filter=Start/DateTime%20ge%20%272016-04-01T08:00%27
GET https://outlook.office.com/api/v2.0/me/events?$filter=Subject%20eq%20%27Mega%20Charity%20Bash%27
GET https://outlook.office.com/api/v2.0/me/messages?$filter=IsRead%20eq%20false
GET https://outlook.office.com/api/v2.0/me/messages?$filter=HasAttachments%20eq%20true
GET https://outlook.office.com/api/v2.0/me/messages?$filter=ReceivedDateTime%20ge%202014-09-01
GET https://outlook.office.com/api/v2.0/me/messages?$filter=From/EmailAddress/Address%20eq%20%27hr@contoso.com%27
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

|**演算子**|**種類**|**例**|
|:-----|:-----|:-----|
|および|論理積 (複数の条件を結合するために使用する)| `TotalCount gt 0 and ChildFolderCount eq 0`|
|または|論理和 (複数の条件を結合するために使用する)| `TotalCount gt 0 or ChildFolderCount eq 0`|
|eq|等しい| `IsRead eq false`|
|ne|等しくない| `Importance ne Microsoft.Exchange.Services.OData.Model.Importance'High'`|
|gt|より大きい| `DateTimeReceived gt 2014-09-01T00:00:00Z`|
|ge|以上| `DateTimeLastModified  ge 2014-09-01T00:00:00Z`|
|lt|より少ない| `DateTimeReceived lt 2014-09-01T00:00:00Z`|
|le|以下| `DateTimeLastModified  le 2014-09-01T00:00:00Z`|

フィルター条件で任意の文字列値を区切るには、単一引用符 (') を使用します。 URL で単一引用符をエンコードするには、`%27` を使用します。 文字列自体では大文字と小文字の区別はありません。 

いくつかの例を見てみましょう。読みやすくするために、例内の URL はエンコードされていません。ただし、これらの例を試す場合は、URL をサーバーに送信する前にエンコードする必要があります。

ユーザーの予定表に含まれる特定の件名のイベントすべてを取得するには、**Subject** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/events?$filter=Subject eq 'Mega Charity Bash'
```

受信トレイ内のすべての未読メッセージを取得する場合は、**IsRead** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$filter=IsRead eq false
```

受信トレイ内のすべての添付ファイル付きメッセージを取得する場合は、**HasAttachments** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$filter=HasAttachments eq true
```

2014 年 9 月 1 日以降に受信された受信トレイ内のすべてのメッセージを取得する場合は、**DateTimeReceived** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$filter=DateTimeReceived ge 2014-09-01
```

"hr@contoso.com" から送信された受信トレイ内のすべてのメッセージを取得する場合は、**Sender** プロパティでフィルター処理できます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$filter=From/EmailAddress/Address eq 'hr@contoso.com'
```

上の例では、URL エンコードが含まれていませんでしたが、URL エンコードされ、サーバーに送信する準備ができた同じ例を以下に示します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/events?$filter=Subject%20eq%20%27Mega%20Charity%20Bash%27
GET https://outlook.office.com/api/v1.0/me/messages?$filter=IsRead%20eq%20false
GET https://outlook.office.com/api/v1.0/me/messages?$filter=HasAttachments%20eq%20true
GET https://outlook.office.com/api/v1.0/me/messages?$filter=DateTimeReceived%20ge%202014-09-01
GET https://outlook.office.com/api/v1.0/me/messages?$filter=From/EmailAddress/Address%20eq%20%27hr@contoso.com%27
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->





<a name="Select"> </a>
### 返される特定のプロパティの選択

**$select** クエリ パラメーターを使用すれば、アプリに必要なプロパティのみを指定することができます。

**注** メール、予定表、および連絡先アイテムを取得する場合は、必ず、**$select** を使用して応答ペイロードから不要なプロパティを除外することによって、適切なアプリ パフォーマンスを維持してください。 **$select** パラメーターを省略した場合は、アイテムのすべてのプロパティが返されます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

次の例は、受信トレイ内のすべてのメッセージの **Subject**、**Sender**、および **ReceivedDateTime** プロパティを取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$select=Subject,Sender,ReceivedDateTime
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

次の例は、受信トレイ内のすべてのメッセージの **Subject**、**Sender**、および **ReceivedDateTime** プロパティを取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$select=Subject,Sender,ReceivedDateTime
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

次の例は、受信トレイ内のすべてのメッセージの **Subject**、**Sender**、および **DateTimeReceived** プロパティを取得します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$select=Subject,Sender,DateTimeReceived
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->



<a name="OrderBy"> </a>
### 結果の並べ替え

**$orderby** クエリ パラメーターを使用すれば、結果を並べ替えることができます。このパラメーターの値をプロパティ名に設定し、必要に応じて、昇順 (既定) または降順を指定します。**$orderby** クエリ パラメーターは **$search.** と一緒に使用できないことを思い出してください。


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

URL エンコードを使用しない次の例では、**ReceivedDateTime** プロパティによって受信トレイ内のすべてのメッセージが降順に並べ替えられます。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$orderby=ReceivedDateTime desc
```

URL エンコードを使用した同じ例を以下に示します。
```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$orderby=ReceivedDateTime%20desc
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

URL エンコードを使用しない次の例では、**ReceivedDateTime** プロパティによって受信トレイ内のすべてのメッセージが降順に並べ替えられます。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$orderby=ReceivedDateTime desc
```

URL エンコードを使用した同じ例を以下に示します。
```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$orderby=ReceivedDateTime%20desc
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

URL エンコードを使用しない次の例では、**DateTimeReceived** プロパティによって受信トレイ内のすべてのメッセージが降順に並べ替えられます。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$orderby=DateTimeReceived desc
```

URL エンコードを使用した同じ例を以下に示します。
```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$orderby=DateTimeReceived%20desc
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="TopSkip"> </a>
### 結果のページング

既定で、**Messages** または **ChildFolders** プロパティ、コレクション、または **CalendarView** に対する GET 要求は 10 個のエントリ (最大 50) を返します。この動作は、**$top** クエリ パラメーターを使用して最大数を設定することによって変更できます。次の例は、受信トレイ内の最初の 5 つのメッセージを取得します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$top=5
```

受信トレイ内に 6 つ以上のメッセージが存在していた場合は、応答に **odata.nextLink** プロパティが含まれます。このプロパティの存在は、サーバー上に使用可能なアイテムがまだ存在することを意味します。このプロパティの値は、次の 5 つのアイテムを取得するために使用可能な URI です。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages?$top=5&$skip=5
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$top=5
```

受信トレイ内に 6 つ以上のメッセージが存在していた場合は、応答に **odata.nextLink** プロパティが含まれます。このプロパティの存在は、サーバー上に使用可能なアイテムがまだ存在することを意味します。このプロパティの値は、次の 5 つのアイテムを取得するために使用可能な URI です。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages?$top=5&$skip=5
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$top=5
```

受信トレイ内に 6 つ以上のメッセージが存在していた場合は、応答に **odata.nextLink** プロパティが含まれます。このプロパティの存在は、サーバー上に使用可能なアイテムがまだ存在することを意味します。このプロパティの値は、次の 5 つのアイテムを取得するために使用可能な URI です。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages?$top=5&$skip=5
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


ページングは、**$top** パラメーターでページ サイズを指定し、**$skip** パラメーターをページ サイズの倍数として使用することによって実現されます。**$skip** パラメーター値をページ サイズ分だけ増やすことによって、結果セットに次のページを要求できます。


<a name="Count"> </a>
### コレクション内のエンティティのカウント

**$count** パラメーターを使用すれば、コレクション内のエンティティ数を取得することができます。 カウント要求をフィルター処理することもできます。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]


次の例は、受信トレイ内のメッセージの数を取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages/$count
```

また、URL エンコードを使用しないこの例は、受信トレイ内の未読メッセージの数も取得します。

```no-highlight
GET https://outlook.office.com/api/beta/me/messages/$count?$filter=IsRead eq false
```

URL エンコードを使用した同じ例を以下に示します。
```no-highlight
GET https://outlook.office.com/api/beta/me/messages/$count?$filter=IsRead%20eq%20false
```


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

次の例は、受信トレイ内のメッセージの数を取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages/$count
```

また、URL エンコードを使用しないこの例は、受信トレイ内の未読メッセージの数も取得します。

```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages/$count?$filter=IsRead eq false
```

URL エンコードを使用した同じ例を以下に示します。
```no-highlight
GET https://outlook.office.com/api/v2.0/me/messages/$count?$filter=IsRead%20eq%20false
```

[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

次の例は、受信トレイ内のメッセージの数を取得します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages/$count
```

また、URL エンコードを使用しないこの例は、受信トレイ内の未読メッセージの数も取得します。

```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages/$count?$filter=IsRead eq false
```

URL エンコードを使用した同じ例を以下に示します。
```no-highlight
GET https://outlook.office.com/api/v1.0/me/messages/$count?$filter=IsRead%20eq%20false
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->


<a name="PutItTogether"> </a>
### すべてをまとめる

パラメーターを組み合わせて複雑なクエリを作成することができます。次の例は、受信トレイ内のメッセージのクエリを次のように精緻化します。

<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

- **Importance** が High に設定されたアイテムのみを返す。

- **Subject**、**Sender**、および **ReceivedDateTime** プロパティのみを返す。

- 最初の 5 つのメッセージのみを返す。

**メモ**: 読みやすくするために、URL エンコードが省略され、改行が追加されています。

```no-highlight
https://outlook.office.com/api/beta/me/messages?
    $filter=Importance eq 'High'
    &$select=Subject,Sender,ReceivedDateTime
    &$top=5
```

**$filter** を指定した場合は、サーバーで結果の並べ替え順序が考慮されます。 **$filter** と **$orderby** の両方を使用する場合は、**$filter** 内のプロパティを他のプロパティよりも先に **$orderby** 内に配置して、**$filter** パラメーター内の順序で並べる必要があります。 次の例は、**Subject** と **Importance** の両方のプロパティでフィルター処理されてから、**Subject**、**Importance**、および **Sender** の各プロパティで並べ替えられたクエリを示しています。

```no-highlight
https://outlook.office.com/api/beta/me/messages?
    $filter=Subject eq 'Good Times' AND Importance eq 'High'&
    $orderby=Subject,Importance,Sender
```

URL エンコードを使用し、改行を除外した同じ例を以下に示します。

```no-highlight
https://outlook.office.com/api/beta/me/messages?$filter=Importance%20eq%20%27High%27&select=Subject,Sender,ReceivedDateTime&$top=5

https://outlook.office.com/api/beta/me/messages?$filter=Subject%20eq%20%27Good%20Times%27%20AND%20Importance%20eq%20%27High%27&$orderby=Subject,Importance,Sender
```

[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->



<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]


- **Importance** が High に設定されたアイテムのみを返す。

- **Subject**、**Sender**、および **ReceivedDateTime** プロパティのみを返す。

- 最初の 5 つのメッセージのみを返す。

**メモ**: 読みやすくするために、URL エンコードが省略され、改行が追加されています。

```no-highlight
https://outlook.office.com/api/v2.0/me/messages?
    $filter=Importance eq 'High'
    &$select=Subject,Sender,ReceivedDateTime
    &$top=5
```

**$filter** を指定した場合は、サーバーで結果の並べ替え順序が考慮されます。 **$filter** と **$orderby** の両方を使用する場合は、**$filter** 内のプロパティを他のプロパティよりも先に **$orderby** 内に配置して、**$filter** パラメーター内の順序で並べる必要があります。 次の例は、**Subject** と **Importance** の両方のプロパティでフィルター処理されてから、**Subject**、**Importance**、および **Sender** の各プロパティで並べ替えられたクエリを示しています。

```no-highlight
https://outlook.office.com/api/v2.0/me/messages?
    $filter=Subject eq 'Good Times' AND Importance eq 'High'&
    $orderby=Subject,Importance,Sender
```

URL エンコードを使用し、改行を除外した同じ例を以下に示します。

```no-highlight
https://outlook.office.com/api/v2.0/me/messages?$filter=Importance%20eq%20%27High%27&select=Subject,Sender,ReceivedDateTime&$top=5

https://outlook.office.com/api/v2.0/me/messages?$filter=Subject%20eq%20%27Good%20Times%27%20AND%20Importance%20eq%20%27High%27&$orderby=Subject,Importance,Sender
```


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->


[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

- **Importance** が High に設定されたアイテムのみを返す。

- **Subject**、**Sender**、および **DateTimeReceived** プロパティのみを返す。

- 最初の 5 つのメッセージのみを返す。

**メモ**: 読みやすくするために、URL エンコードが省略され、改行が追加されています。

```no-highlight
https://outlook.office.com/api/v1.0/me/messages?
    $filter=Importance eq 'High'
    &$select=Subject,Sender,DateTimeReceived
    &$top=5
```

**$filter** を指定した場合は、サーバーで結果の並べ替え順序が考慮されます。 **$filter** と **$orderby** の両方を使用する場合は、**$filter** 内のプロパティを他のプロパティよりも先に **$orderby** 内に配置して、**$filter** パラメーター内の順序で並べる必要があります。 次の例は、**Subject** と **Importance** の両方のプロパティでフィルター処理されてから、**Subject**、**Importance**、および **Sender** の各プロパティで並べ替えられたクエリを示しています。

```no-highlight
https://outlook.office.com/api/v1.0/me/messages?
    $filter=Subject eq 'Good Times' AND Importance eq 'High'&
    $orderby=Subject,Importance,Sender
```

URL エンコードを使用し、改行を除外した同じ例を以下に示します。

```no-highlight
https://outlook.office.com/api/v1.0/me/messages?$filter=Importance%20eq%20%27High%27&select=Subject,Sender,DateTimeReceived&$top=5

https://outlook.office.com/api/v1.0/me/messages?$filter=Subject%20eq%20%27Good%20Times%27%20AND%20Importance%20eq%20%27High%27&$orderby=Subject,Importance,Sender
```

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->





##関連記事

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)

- [Outlook メール REST API リファレンス](..\api\mail-rest-operations.md)

- [Outlook 予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [Outlook の連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [Outlook タスク REST API リファレンス](..\api\task-rest-operations.md)


[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]


