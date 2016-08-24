---
ms.Toctitle: Sections REST API reference
title: "セクション REST API リファレンス"
description: "ms.TocTitle:セクション REST API リファレンス Title:セクション REST API リファレンス Description:Office 365 Education テナントのセクションへのアクセスを提供する セクション REST API と対話する方法に関するリファレンスです。ms.ContentId:3805c218-e0c7-48e9-b458-448563ff7ae4 ms.topic: リファレンス (API)"
ms.ContentId: 3805c218-e0c7-48e9-b458-448563ff7ae4

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]


# セクション REST API リファレンス
    
 __**適用対象:** Office 365 Education__
<p class="previewnote">This documentation covers features that are currently in preview.</p>



<a name="Overview"> </a>Office 365 Education API は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントから、データを抽出する際に役立ちます。その結果により、教育機関、セクション、教職員、学生および名簿に関する情報が得られます。 These results provide information about schools, sections, teachers, students and rosters. Each class in the school is represented as a Section. The Sections REST API provides access to section entities in Office 365 for Education tenants.

API は Microsoft Azure Active Directory と OAuth を使用して、[アプリケーション要求を認証します](..\howto\common-app-authentication-tasks.md)。
 
アプリケーションからセクション REST API にアクセスするには、[そのアプリケーションを Azure Active Directory に登録する](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/#adding-an-application)ことが必要になります。また、認証トークンを管理し、ニーズを満たす正しい URL とクエリを構築する必要もあります。次に示す各例は、クエリを作成する際に利用できます。

<!-- Add Extension Properties and Data-Model-->

## すべてのセクション REST API 操作

<a name="SectionOperations"> </a> Sections are represented as [Unified Groups](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations) in Azure Active Directory. Azure Active Directory では、セクションが統合グループとして表されます。テナントで使用可能なセクションを取得できます。また、各セクションに含まれる学生と教職員、およびセクションが属する教育機関の詳細も取得できます。


セクションを取得する  セクションの教育機関を取得する  セクション内の学生を取得する  セクション内の教職員を取得する 


## セクション REST API の使用

セクション REST API と対話する場合は、HTTP GET 要求を送信します。

すべてのセクション REST API 要求で次のルート URL を使用します。

`https://graph.windows.net/{tenant_id}/groups`

{tenant_id}`{tenant_id}` は、Azure Active Directory テナントの一意の ID または ドメインです。tenant_id は、次のいずれかの方法で指定できます。 {tenant_id} は、Azure Active Directory テナントの一意の ID または ドメインです。tenant_id は、次のいずれかの方法で指定できます。
- テナントのオブジェクト ID (GUID)。例: https://graph.windows.net/95b43ae0-0554-4cc5-8c22-fe219dc31156/`https://graph.windows.net/95b43ae0-0554-4cc5-8c22-fe219dc31156/`。
- テナントの検証済みドメイン名。例: https://graph.windows.net/contoso.onmicrosoft.com/`https://graph.windows.net/contoso.onmicrosoft.com/`。
- The `myOrganization` alias. myOrganization`https://graph.windows.net/myorganization/` のエイリアス。このエイリアスは、サインインしているユーザーのテナントに解決されます (要求で送信されるベアラー トークンに基づきます)。例: https://graph.windows.net/myorganization/。 

後述する各例では、tenant_id = 95b43ae0-0554-4cc5-8c22-fe219dc31156`tenant_id = 95b43ae0-0554-4cc5-8c22-fe219dc31156` を使用します。

Sections are represented in Azure Active Directory as [Unified Groups](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations). Extension attributes on the unified groups add education-specific information.  
For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_CourseNumber` attribute contains the course number of a section.

**注** すべての要求について、URL クエリ文字列では api-version`api-version` を指定する必要があります。教育機関 REST API には、バージョン beta が必要です。例: https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta。 注 すべての要求について、URL クエリ文字列では api-version`beta` を指定する必要があります。セクション REST API には、バージョン 1.5 以上が必要になります。ただし、beta のままのバージョンを必要とするセクションの教育機関を取得する場合を除きます。例: https://graph.windows.net/{tenant_id}/groups?api-version=1.5。 例: `https://graph.windows.net/{tenant_id}/groups?api-version=1.5`   

## この教育機関についての情報のソース

セクションに関する情報を特定する際に役立つ属性の説明については、「[セクションの属性](education-rest-attributes.md#SectionAttributes)」を参照してください。

****

<a name="GetSections"> </a>
##セクションの取得

すべてのセクションを取得することも、object_id`object_id` ごとに 1 つのセクションを取得することも、クエリのフィルター セットに一致するセクションのコレクションを取得することもできます。

<a name="GetAllSections"> </a>
###すべてのセクションの取得

Azure Active Directory テナント内に存在する、すべてのセクションを取得します。

```no-highlight
GET https://graph.windows.net/{tenant_id}/groups?api-version=1.5&$filter=extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType%20eq%20'Section'
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|

[!code-REST-i[sections_api_get_sections.json](./_data/sections_api_get_sections.json)]


 **応答の種類**

セクション エンティティのコレクション。


****


<a name="GetSection"> </a>
###1 つのセクションの取得

object_id`object_id` を使用してセクションを取得します。

```no-highlight
GET https://graph.windows.net/{tenant_id}/groups/{object_id}?api-version=1.5
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory のセクション グループのオブジェクト ID。|   


[!code-REST-i[sections_api_get_section_by_id.json](./_data/sections_api_get_section_by_id.json)]


 **応答の種類**

要求されたセクション エンティティ。


****

<a name="GetSectionsSchool"> </a>
##セクションの教育機関の取得

Schools are represented in Azure Active Directory as administrative units. Extension attributes on these administrative units add school-specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_HighestGrade` attribute contains the highest grade for that school.

セクションに関連付けられた教育機関は、「[セクションの取得](#GetSection)」の extension_fe2174665583431c953114ff7268b7b3_Education_SchoolId`SchoolId` 応答から得られる SchoolId`extension_fe2174665583431c953114ff7268b7b3_Education_SchoolId` プロパティで管理単位にクエリを実行することで取得できます。


```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta
        &$filter=extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType%20eq%20'School'%20
        and%20extension_fe2174665583431c953114ff7268b7b3_Education_SchoolId%20eq%20'{school_id}'
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|school_id|文字列|School Information System (SIS) での教育機関の ID。|   


[!code-REST-i[sections_api_get_school_for_section_id.json](./_data/sections_api_get_school_for_section_id.json)]


 **応答の種類**

教育機関エンティティ。

**** 

<a name="GetSectionStudents"> </a>
##セクション内の学生の取得

Students are represented in Azure Active Directory as users. User Extension Attributes add student-specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_Grade` attribute contains the student's grade level.  

特定のセクションの学生は、セクションの統合グループのメンバーになり、アプリケーションで結果のコレクションから学生以外のユーザーをフィルターで除外することで取得できます。

###セクションのメンバーの取得
```no-highlight
GET https://graph.windows.net/{tenant_id}/groups/{object_id}/members?api-version=1.5
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory のセクション グループのオブジェクト ID。|   


[!code-REST-i[sections_api_get_students_by_section_id.json](./_data/sections_api_get_students_by_section_id.json)]


 **応答の種類**

ユーザーのコレクション。

###学生の検索

ユーザーのコレクションには、学生、教職員、および教育に関わらない (管理スタッフなどの) ユーザーが含まれていることがあります。アプリケーションでは、学生を抽出するフィルターをコレクションに適用しできます。Student と等しくなる Education_ObjectType 拡張属性を検索します。 You can filter a collection down to students only, within your application. Query for the `Education_ObjectType` extension attribute equal to `Student`.

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Student'
```

**** 

<a name="GetSectionTeachers"> </a>
##セクション内の教職員の取得

Teachers are represented in Azure Active Directory as users. User extension attributes add teacher-specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_TeacherNumber` attribute contains the teacher's teacher number. 

特定のセクションの教職員は、セクションの統合グループのメンバーになり、アプリケーションで結果のコレクションから教職員以外のユーザーをフィルターで除外することで取得できます。

###セクションのメンバーの取得
```no-highlight
GET https://graph.windows.net/{tenant_id}/groups/{object_id}/members?api-version=1.5
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory のセクション グループのオブジェクト ID。|   


[!code-REST-i[sections_api_get_teachers_by_section_id.json](./_data/sections_api_get_teachers_by_section_id.json)]


 **応答の種類**

ユーザーのコレクション。

###教職員の検索

ユーザーのコレクションには、学生、教職員、および教育に関わらない (管理スタッフなどの) ユーザーが含まれていることがあります。アプリケーションでは、教職員のみを抽出するフィルターをコレクションに適用できます。Teacher と等しくなる Education_ObjectType 拡張属性についてのクエリを実行します。 You can filter the collection down to only teachers, within your application. Query for the `Education_ObjectType` extension attribute equal to `Teacher`.

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Teacher'
```


**** 
<a name="NextSteps"> </a>
## 次のステップ

次に、興味の対象になると考えられる、その他の Education に関連するリソースを示します。

- [教育機関 REST 操作](..\api\school-rest-operations.md)を使用した、教育機関情報へのアクセス

- [学生 Rest 操作](..\api\student-rest-operations.md)を使用した、学生情報へのアクセス

- [教職員 Rest 操作](..\api\teacher-rest-operations.md)を使用した、教職員情報へのアクセス 

- [教育属性](..\api\education-rest-attributes.md)で使用可能な属性の説明


アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。


- 開始する準備ができている場合は、 初めに[環境をセットアップし](..\howto\setup-development-environment.md)、次に[最初のアプリのウォークスルー](..\howto\getting-started-Office-365-APIs.md)をご覧ください。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](http://apisandbox.msdn.microsoft.com/)をお使いください。
    
- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。
    

Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)
    
- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)
    
- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)
  
- [メール API リファレンス](..\api\mail-rest-operations.md)
  
- [予定表 API リファレンス](..\api\calendar-rest-operations.md)

- [Files API リファレンス](..\api\files-rest-operations.md)

- [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)

- [メール、予定表、連絡先 REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)
    
