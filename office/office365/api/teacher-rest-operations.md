---
ms.Toctitle: Teachers REST API reference
title: "教師 REST API リファレンス"
description: "ms.TocTitle:教師 REST API リファレンスTitle:教師 REST API リファレンスDescription:Office 365 Education 内の教師へのアクセスを提供する教師 REST API と対話する方法に関するリファレンスms.ContentId:80124eb7-1af4-44d3-94d4-ec60afe0f254 ms.topic: リファレンス (API)"
ms.ContentId: 80124eb7-1af4-44d3-94d4-ec60afe0f254

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# 教師 REST API リファレンス
    
 __**適用対象:**Office 365 Education__
<p class="previewnote">This documentation covers features that are currently in preview.</p>


<a name="Overview"> </a>Office 365 Education API は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントから、データを抽出する際に役立ちます。その結果により、教育機関、セクション、教職員、学生および名簿に関する情報が得られます。 These results provide information about schools, sections, teachers, students and roster information. The Teachers REST API provides access to teacher entities in Office 365 Education.

API は Microsoft Azure Active Directory と OAuth を使用して、[アプリケーション要求を認証します](..\howto\common-app-authentication-tasks.md)。
 
アプリケーションから教師 REST API にアクセスするには、[そのアプリケーションを Azure Active Directory に登録する](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/#adding-an-application)ことが必要になります。また、認証トークンを管理し、ニーズを満たす正しい URL とクエリを構築する必要もあります。次に示す各例は、クエリを作成する際に利用できます。

<!-- Add Extension Properties and Data-Model-->

## すべての教師 REST API 操作

<a name="TeacherOperations"> </a> Teachers are represented as Users in Azure Active Directory. Azure Active Directory では、教師はユーザーとして表されます。ログオンしている現在の教師の属性、およびその教師がメンバーである学校とセクションの属性を取得できます。


現在のユーザーの取得  教師の学校の取得  教師のセクションの取得 


## 教師 REST API の使用

教師 REST API と対話する場合は、HTTP GET 要求を送信します。

ログイン ユーザーが教師である場合、すべての教師 REST API 要求は次のルート URL を使用します。

`https://graph.windows.net/me`

Teachers are represented in Azure Active Directory as Users. Extension attributes on the users add teacher-specific information.  
For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_TeacherNumber` attribute contains the Teacher Number of the teacher.

**注** すべての要求について、URL クエリ文字列では api-version`api-version` を指定する必要があります。教育機関 REST API には、バージョン beta が必要です。例: https://graph.windows.net/{tenant_id}/administrativeUnits?api-version=beta。 The Teachers REST API requires version 1.5 or greater. 例: `https://graph.windows.net/me?api-version=1.5`   

## 教育機関のオブジェクトの種類は 'Student' です

教師に関する情報を特定する際に役立つ属性の説明については、「[教師属性](education-rest-attributes.md#TeacherAttributes)」を参照してください。

****

<a name="GetCurrentUser"> </a>
##現在のユーザーの取得

現在ログインしているユーザーを取得し、そのユーザーが教師かどうかを確認することができます。

```no-highlight
GET https://graph.windows.net/me?api-version=1.5
```

[!code-REST-i[teachers_api_get_current_user.json](./_data/teachers_api_get_current_user.json)]


 **応答の種類**

現在ログインしているユーザー。

###教師かどうかを確認する

現在ログインしているユーザーは、学生、教師、または教育に関わらない (管理スタッフなどの) ユーザーである可能性があります。ユーザーが教師であるかどうかは、アプリケーションで確認できます。Teacher と等しくなる Education_ObjectType 拡張属性を検索します。 You can check if the user is a teacher within your application. Look for the `Education_ObjectType` extension attribute to equal `Teacher`.

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Teacher'
```

****

<a name="GetTeacherSchool"> </a>
##教師の教育機関の取得

Schools are represented in Azure Active Directory as [Administrative Units](https://msdn.microsoft.com/en-us/library/azure/dn832057.aspx). Extension attributes on these administrative units add school-specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_HighestGrade` attribute contains the highest grade for that school.

その教師のメンバーシップを取得して、extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'School'`extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'School'` と objectType == 'AdministrativeUnit'`objectType == 'AdministrativeUnit'` に対するフィルターを適用することで、ログインしている教師の教育機関を取得できます。

###教師のメンバーシップの取得

ログインしている教師のメンバーシップは、次に示すクエリを使用することで取得できます。


```no-highlight
GET https://graph.windows.net/me/memberOf?api-version=1.5
```


 **応答の種類**

応答には、教師がメンバーになっている複数の Group や DirectoryRole、AdministrativeUnit が含まれています。

###教育機関のフィルター

教師の教育機関は、extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'School'`extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'School'` と objectType == 'AdministrativeUnit'`objectType == 'AdministrativeUnit'` に関するフィルター処理によって取得できます。


**** 

<a name="GetTeacherSection"> </a>
##教師のセクションの取得

Sections are represented in Azure Active Directory as [Unified Groups](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations). Extension attributes on the unified groups add section-specific information. For example, the `extension_fe2174665583431c953114ff7268b7b3_Education_CourseName` attribute contains the course name for the section.

###教師のメンバーシップの取得

教師のメンバーシップは、次に示すクエリを使用することで取得できます。


```no-highlight
GET https://graph.windows.net/me/memberOf?api-version=1.5
```

 **応答の種類**

応答には、教師がメンバーになっている複数の Group や DirectoryRole、AdministrativeUnit が含まれています。

###セクションのフィルター

教師のセクションは、extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Section'`extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Section'` と objectType == 'Group'`objectType == 'Group'` に関するフィルター処理によって取得できます。

**** 

<a name="NextSteps"> </a>
## 次のステップ

次に、興味の対象になると考えられる、その他の Education に関連するリソースを示します。

- [教育機関 REST 操作](..\api\school-rest-operations.md)を使用した、教育機関情報へのアクセス

- [セクション REST 操作](..\api\section-rest-operations.md)を使用した、セクション情報へのアクセス

- [学生 Rest 操作](..\api\student-rest-operations.md)を使用した、学生情報へのアクセス

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
    
