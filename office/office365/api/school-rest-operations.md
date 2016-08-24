---
ms.Toctitle: Schools REST API reference
title: "教育機関 REST API リファレンス"
description: "ms.TocTitle:教育機関 REST API リファレンス Title:教育機関 REST API リファレンス Description:Office 365 Education テナントの教育機関へのアクセスを提供する 教育機関 REST API と対話する方法に関するリファレンスです。ms.ContentId:35003e02-c4b1-4702-9d86-b6a7718e9fa8 ms.topic: リファレンス (API)"
ms.ContentId: 35003e02-c4b1-4702-9d86-b6a7718e9fa8

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]




# 教育機関 REST API リファレンス
    
 __**適用対象:**Office 365 Education__
<p class="previewnote">This documentation covers features that are currently in preview.</p>


<a name="Overview"> </a>Office 365 Education API は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントから、データを抽出する際に役立ちます。その結果により、教育機関、セクション、教職員、学生および名簿に関する情報が得られます。 These results provide information about schools, sections, teachers, students and rosters. The Schools REST API provides access to school entities in Office 365 for Education tenants.

API は Microsoft Azure Active Directory と OAuth を使用して、[アプリケーション要求を認証します](..\howto\common-app-authentication-tasks.md)。
 
アプリケーションから教育機関 REST API にアクセスするには、[そのアプリケーションを Azure Active Directory に登録する](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/#adding-an-application)ことが必要になります。また、認証トークンを管理し、ニーズを満たす正しい URL とクエリを構築する必要もあります。次に示す各例は、クエリを作成する際に利用できます。

<!-- Add Extension Properties and Data-Model-->

## すべての教育機関 REST API 操作

<a name="SchoolOperations"> </a> Schools are represented as [Administrative Units](https://msdn.microsoft.com/en-us/library/azure/dn832057.aspx) in Azure Active Directory. Azure Active Directory では、教育機関が管理単位として表されます。テナントで使用可能な教育機関を取得できます。また、教育機関エンティティに含まれるセクション、学生、および教職員も取得できます。


教育機関を取得する  教育機関内のセクションを取得する  教育機関内の学生を取得する  教育機関内の教職員を取得する 


## 教育機関 REST API の使用

教育機関 REST API と対話する場合は、HTTP GET 要求を送信します。

すべての教育機関 REST API 要求で次のルート URL を使用します。

`https://graph.windows.net/{tenant_id}/administrativeUnits`

{tenant_id}`{tenant_id}` は、Azure Active Directory テナントの一意の ID または ドメインです。tenant_id は、次のいずれかの方法で指定できます。 {tenant_id} は、Azure Active Directory テナントの一意の ID または ドメインです。tenant_id は、次のいずれかの方法で指定できます。
- テナントのオブジェクト ID (GUID)。例: https://graph.windows.net/95b43ae0-0554-4cc5-8c22-fe219dc31156/`https://graph.windows.net/95b43ae0-0554-4cc5-8c22-fe219dc31156/`。
- テナントの検証済みドメイン名。例: https://graph.windows.net/contoso.onmicrosoft.com/`https://graph.windows.net/contoso.onmicrosoft.com/`。
- The `myOrganization` alias. myOrganization`https://graph.windows.net/myorganization/` のエイリアス。このエイリアスは、サインインしているユーザーのテナントに解決されます (要求で送信されるベアラー トークンに基づきます)。例: https://graph.windows.net/myorganization/。 

後述する各例では、tenant_id = 95b43ae0-0554-4cc5-8c22-fe219dc31156`tenant_id = 95b43ae0-0554-4cc5-8c22-fe219dc31156` を使用します。   

Schools are represented in Azure Active Directory as administrative units. Extension attributes on the administrative units add education-specific information.  
For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_HighestGrade` attribute contains the highest grade level within the school.

**注** すべての要求について、URL クエリ文字列では api-version`api-version` を指定する必要があります。教育機関 REST API には、バージョン beta が必要です。例: https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta。 The Schools REST API requires version `beta`. 例: `https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta`   


## 教育機関の属性

教育機関に関する情報を特定する際に役立つ属性の説明については、「[教育機関の属性](education-rest-attributes.md#SchoolAttributes)」を参照してください。

****

<a name="GetSchools"> </a>
##教育機関の取得

すべての教育機関を取得することも、object_id`object_id` ごとに 1 つの教育機関を取得することも、クエリのフィルター セットに一致する教育機関のコレクションを取得することもできます。

<a name="GetAllSchools"> </a>
###すべての教育機関の取得

Azure Active Directory テナント内に存在する、すべての教育機関を取得します。

```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta&$filter=extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType%20eq%20'School'
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|

[!code-REST-i[schools_api_get_schools.json](./_data/schools_api_get_schools.json)]


 **応答の種類**

教育機関エンティティのコレクション。


****


<a name="GetSchool"> </a>
###1 つの教育機関の取得

object_id`object_id` を使用して、1 つの教育機関を取得します。

```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits/{object_id}?api-version=beta
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory の教育機関管理単位のオブジェクト ID。|   

[!code-REST-i[schools_api_get_school_by_id.json](./_data/schools_api_get_school_by_id.json)]


 **応答の種類**

要求された教育機関エンティティ。


****

<a name="GetSchoolSections"> </a>
##教育機関内のセクションの取得

Sections are represented in Azure Active Directory as [Unified Groups](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations). Extension attributes on the unified groups add section specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_CourseName` attribute contains the course name for the section.

特定の教育機関のセクションは、スクール ID に基づいたグループにクエリを実行することで取得できます。このとき、クエリでは extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType`extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType` 属性と extension_fe2174665583431c953114ff7268b7b3_Education_SyncSource_SchoolId`extension_fe2174665583431c953114ff7268b7b3_Education_SyncSource_SchoolId` 属性を同時に使用します。


```no-highlight
GET https://graph.windows.net/{tenant_id}/groups?api-version=1.5
        &$filter=extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType%20eq%20'Section'%20
        and%20extension_fe2174665583431c953114ff7268b7b3_Education_SyncSource_SchoolId%20eq%20'{school_id}'
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|school_id|文字列|School Information System (SIS) での教育機関の ID。|   


[!code-REST-i[schools_api_get_sections_by_school_id.json](./_data/schools_api_get_sections_by_school_id.json)]


 **応答の種類**

セクション エンティティのコレクション。

**** 

<a name="GetSchoolStudents"> </a>
##教育機関内の学生の取得

Students are represented in Azure Active Directory as users. Extension attributes on the users add student specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_Grade` attribute contains the student's grade level.  

特定の教育機関の学生は、教育機関管理単位のメンバーになり、アプリケーションで結果のコレクションから学生以外のユーザーをフィルターで除外することで取得できます。

###教育機関のメンバーの取得
```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits/{object_id}/members?api-version=beta
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory の教育機関管理単位のオブジェクト ID。|   


[!code-REST-i[schools_api_get_students_by_school_id.json](./_data/schools_api_get_students_by_school_id.json)]


 **応答の種類**

ユーザーのコレクション。

###学生の検索

ユーザーのコレクションには、学生、教職員、および教育に関わらない (管理スタッフなどの) ユーザーが含まれていることがあります。アプリケーションでは、学生を抽出するフィルターをコレクションに適用しできます。Student と等しくなる Education_ObjectType 拡張属性を検索します。 You can filter the collection down to students within your application. Look for the `Education_ObjectType` extension attribute to equal `Student`.

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Student'
```

**** 

<a name="GetSchoolTeachers"> </a>
##教育機関内の教職員の取得

Teachers are represented in Azure Active Directory as users. Extension attributes on the users add teacher specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_TeacherNumber` attribute contains the teacher's teacher number. 

特定の教育機関の教職員は、教育機関管理単位のメンバーになり、アプリケーションで結果のコレクションから教職員以外のユーザーをフィルターで除外することで取得できます。

###教育機関のメンバーの取得
```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits/{object_id}/members?api-version=beta
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL のパラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory の教育機関管理単位のオブジェクト ID。|   


[!code-REST-i[schools_api_get_teachers_by_school_id.json](./_data/schools_api_get_teachers_by_school_id.json)]


 **応答の種類**

ユーザーのコレクション。

###教職員の検索

ユーザーのコレクションには、学生、教職員、および教育に関わらない (管理スタッフなどの) ユーザーが含まれていることがあります。アプリケーションでは、教職員のみを抽出するフィルターをコレクションに適用できます。Teacher と等しくなる Education_ObjectType 拡張属性についてのクエリを実行します。 You can filter the collection down to teachers within your application. Look for the `Education_ObjectType` extension attribute to equal `Teacher`.

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Teacher'
```


**** 
<a name="NextSteps"> </a>
## 次のステップ

次に、興味の対象になると考えられる、その他の Education に関連するリソースを示します。

- [セクション REST 操作](..\api\section-rest-operations.md)を使用した、セクション情報へのアクセス

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
    
