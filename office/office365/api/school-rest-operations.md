---
ms.Toctitle: Schools REST API reference
title: "教育機関 REST API リファレンス"
description: "Office 365 Education テナントの教育機関へのアクセスを提供する教育機関 REST API と対話する方法に関するリファレンスです。"
ms.ContentId: 35003e02-c4b1-4702-9d86-b6a7718e9fa8

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]




# 教育機関 REST API リファレンス
    
 _**適用対象:**Office 365 Education_
<p class="previewnote">このドキュメントで取り上げる機能は、現時点ではプレビュー段階にあります。</p>


<a name="Overview"> </a> Office 365 Education API は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントから、データを抽出する際に役立ちます。 その結果により、教育機関、セクション、教師、学生および名簿に関する情報が得られます。 教育機関 REST API は、Education テナント用の Office 365 に含まれる教育機関エンティティへのアクセスを提供します。

API は Microsoft Azure Active Directory と OAuth を使用して、[アプリケーション要求を認証します](..\howto\common-app-authentication-tasks.md)。
 
アプリケーションから教育機関 REST API にアクセスするには、[そのアプリケーションを Azure Active Directory に登録する](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/#adding-an-application)ことが必要になります。また、認証トークンを管理し、ニーズを満たす正しい URL とクエリを構築する必要もあります。次に示す各例は、クエリを作成する際に利用できます。

<!-- Add Extension Properties and Data-Model-->

## すべての教育機関 REST API の操作


  <a name="SchoolOperations"> </a> Azure Active Directory では、教育機関が[管理単位](https://msdn.microsoft.com/en-us/library/azure/dn832057.aspx)として表されます。 テナントで使用可能な教育機関を取得できます。また、教育機関エンティティに含まれるセクション、学生、および教師も取得できます。


[教育機関を取得する](#GetSchools) | [教育機関内のセクションを取得する](#GetSchoolSections) | [教育機関内の学生を取得する](#GetSchoolStudents) | [教育機関内の教師を取得する](#GetSchoolTeachers) 


## 教育機関 REST API の使用

教育機関 REST API と対話する場合は、HTTP GET 要求を送信します。

すべての教育機関 REST API 要求で次のルート URL を使用します。

`https://graph.windows.net/{tenant_id}/administrativeUnits`

`{tenant_id}` は、Azure Active Directory テナントの一意の ID またはドメインです。 tenant_id は、次のいずれかの方法で指定できます。
- テナントのオブジェクト ID (GUID)。例: `https://graph.windows.net/95b43ae0-0554-4cc5-8c22-fe219dc31156/`。
- テナントの検証済みドメイン名。例: `https://graph.windows.net/contoso.onmicrosoft.com/`。
- `myOrganization` のエイリアス。 このエイリアスは、サインインしているユーザーのテナントに解決されます (要求で送信されるベアラー トークンに基づきます)。例: `https://graph.windows.net/myorganization/`。 

後述する各例では、`tenant_id = 95b43ae0-0554-4cc5-8c22-fe219dc31156` を使用します。   

Azure Active Directory では、教育機関が管理単位として表されます。 管理単位の拡張属性により、教育機関固有の情報が追加されます。  
たとえば、`extension_fe2174665583431c953114ff7268b7b3_Education_HighestGrade` 属性には、その教育機関内での最高学年が格納されています。

**注**: すべての要求について、URL クエリ文字列では `api-version` を指定する必要があります。 教育機関 REST API には、バージョン `beta` が必要です。 例: `https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta`。   


## 教育機関の属性

教育機関に関する情報を特定する際に役立つ属性の説明については、[教育機関の属性](education-rest-attributes.md#SchoolAttributes)

****

<a name="GetSchools"> </a>
##教育機関の取得

すべての教育機関を取得することも、`object_id` ごとに 1 つの教育機関を取得することも、クエリのフィルター セットに一致する教育機関のコレクションを取得することもできます。

<a name="GetAllSchools"> </a>
###すべての教育機関の取得

Azure Active Directory テナント内に存在する、すべての教育機関を取得します。

```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta&$filter=extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType%20eq%20'School'
```

|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|

[!code-REST-i[schools_api_get_schools.json](./_data/schools_api_get_schools.json)]


 **応答の種類**

教育機関エンティティのコレクション。


****


<a name="GetSchool"> </a>
###1 つの教育機関の取得

`object_id` を使用して、1 つの教育機関を取得します。

```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits/{object_id}?api-version=beta
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory の教育機関管理単位のオブジェクト ID。|   

[!code-REST-i[schools_api_get_school_by_id.json](./_data/schools_api_get_school_by_id.json)]


 **応答の種類**

要求された教育機関エンティティ。


****

<a name="GetSchoolSections"> </a>
##教育機関内のセクションの取得

Azure Active Directory では、セクションが[統合グループ](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations)として表されます。 統合グループについての拡張属性により、セクション固有の情報が追加されます。 たとえば、`extension_fe2174665583431c953114ff7268b7b3_Education_CourseName` 属性には、セクションの講座名が格納されます。

特定の教育機関のセクションは、教育機関 ID に基づいたグループにクエリを実行することで取得できます。このとき、クエリでは `extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType` 属性と `extension_fe2174665583431c953114ff7268b7b3_Education_SyncSource_SchoolId` 属性を同時に使用します。


```no-highlight
GET https://graph.windows.net/{tenant_id}/groups?api-version=1.5
        &$filter=extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType%20eq%20'Section'%20
        and%20extension_fe2174665583431c953114ff7268b7b3_Education_SyncSource_SchoolId%20eq%20'{school_id}'
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|school_id|文字列|School Information System (SIS) での教育機関の ID。|   


[!code-REST-i[schools_api_get_sections_by_school_id.json](./_data/schools_api_get_sections_by_school_id.json)]


 **応答の種類**

セクション エンティティのコレクション。

**** 

<a name="GetSchoolStudents"> </a>
##教育機関内の学生の取得

Azure Active Directory では、学生がユーザーとして表されます。 ユーザーについての拡張属性により、学生固有の情報が追加されます。 たとえば、`extension_fe2174665583431c953114ff7268b7b3_Education_Grade` 属性には学生の学年が格納されます。  

特定の教育機関の学生は、教育機関管理単位のメンバーになり、アプリケーションで結果のコレクションから学生以外のユーザーをフィルターで除外することで取得できます。

###教育機関のメンバーの取得
```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits/{object_id}/members?api-version=beta
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory の教育機関管理単位のオブジェクト ID。|   


[!code-REST-i[schools_api_get_students_by_school_id.json](./_data/schools_api_get_students_by_school_id.json)]


 **応答の種類**

ユーザーのコレクション。

###学生の検索

ユーザーのコレクションには、学生、教職員、および教育に関わらない (管理スタッフなどの) ユーザーが含まれていることがあります。 アプリケーションでは、学生を抽出するフィルターをコレクションに適用しできます。 `Student` と等しくなる `Education_ObjectType` 拡張属性を検索します。

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Student'
```

**** 

<a name="GetSchoolTeachers"> </a>
##教育機関内の教師の取得

Azure Active Directory では、教職員はユーザーとして表されます。 ユーザーについての拡張属性により、教職員固有の属性が追加されます。 たとえば、`extension_fe2174665583431c953114ff7268b7b3_Education_TeacherNumber` 属性には教師の教師番号が格納されます。 

特定の教育機関の教職員は、教育機関管理単位のメンバーになり、アプリケーションで結果のコレクションから教職員以外のユーザーをフィルターで除外することで取得できます。

###教育機関のメンバーの取得
```no-highlight
GET https://graph.windows.net/{tenant_id}/administrativeUnits/{object_id}/members?api-version=beta
```


|**必須パラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|_URL パラメーター_|
|tenant_id|文字列|Azure Active Directory のテナント ID、またはドメイン名。|
|object_id|文字列|Azure Active Directory の教育機関管理単位のオブジェクト ID。|   


[!code-REST-i[schools_api_get_teachers_by_school_id.json](./_data/schools_api_get_teachers_by_school_id.json)]


 **応答の種類**

ユーザーのコレクション。

###教職員の検索

ユーザーのコレクションには、学生、教職員、および教育に関わらない (管理スタッフなどの) ユーザーが含まれていることがあります。 アプリケーションでは、教職員を抽出するフィルターをコレクションに適用できます。 `Teacher` と等しくなる `Education_ObjectType` 拡張属性を検索します。

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Teacher'
```


**** 
<a name="NextSteps"> </a>
## 次の手順

次に、興味の対象になると考えられる、その他の Education に関連するリソースを示します。

- [セクション REST 操作](..\api\section-rest-operations.md)を使用した、セクション情報へのアクセス

- [学生 Rest 操作](..\api\student-rest-operations.md)を使用した、学生情報へのアクセス

- [教師 Rest 操作](..\api\teacher-rest-operations.md)を使用した、教師情報へのアクセス 

- 属性の説明については、「[Education の属性](..\api\education-rest-attributes.md)」にあります


アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。


- 開始する準備ができている場合は、 初めに[環境をセットアップし](..\howto\setup-development-environment.md)、次に[最初のアプリのウォークスルー](..\howto\getting-started-Office-365-APIs.md)をご覧ください。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](http://apisandbox.msdn.microsoft.com/)をお使いください。
    
- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。
    

Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)
    
- [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)
    
- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)
  
- [Mail API リファレンス](..\api\mail-rest-operations.md)
  
- [Calendar API リファレンス](..\api\calendar-rest-operations.md)

- [Files API リファレンス](..\api\files-rest-operations.md)

- [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)

- [メール、予定表、連絡先 REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)
    
