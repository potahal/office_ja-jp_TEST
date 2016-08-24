---
ms.Toctitle: Students REST API reference
title: "学生 REST API リファレンス"
description: "Office 365 Education テナントの学生へのアクセスを提供する学生 REST API と対話する方法に関するリファレンスです。"
ms.ContentId: aec1e56a-081d-4214-b441-9039b8341a9e 

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# 学生 REST API リファレンス
    
 _**適用対象:**Office 365 Education_
<p class="previewnote">このドキュメントで取り上げる機能は、現時点ではプレビュー段階にあります。</p>


<a name="Overview"> </a> Office 365 Education API は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントから、データを抽出する際に役立ちます。 その結果には、教育機関、セクション、教師、学生および名簿に関する情報が含まれています。 学生 REST API は、Education テナント用の Office 365 に含まれる学生エンティティへのアクセスを提供します。

API は Microsoft Azure Active Directory と OAuth を使用して、[アプリケーション要求を認証します](..\howto\common-app-authentication-tasks.md)。
 
アプリケーションから学生 REST API にアクセスするには、[そのアプリケーションを Azure Active Directory に登録する](https://azure.microsoft.com/en-us/documentation/articles/active-directory-integrating-applications/#adding-an-application)ことが必要になります。また、認証トークンを管理し、ニーズを満たす正しい URL とクエリを構築する必要もあります。次に示す各例は、クエリを作成する際に利用できます。

<!-- Add Extension Properties and Data-Model-->

## すべての学生 REST API 操作

<a name="StudentOperations"> </a> Azure Active Directory では、学生がユーザーとして表されます。 ログインしている現在の学生の情報を取得できます。また、学生が属している教育機関とセクションも取得できます。


[現在のユーザーを取得する](#GetCurrentUser) | [学生の教育機関を取得する](#GetStudentSchool) | [学生のセクションを取得する](#GetStudentSections) 


## 学生 REST API の使用

学生 REST API と対話する場合は、HTTP GET 要求を送信します。

ログインしているユーザーが学生の場合、すべての学生 REST API 要求は次のルート URL を使用します。

`https://graph.windows.net/me`

Azure Active Directory では、学生がユーザーとして表されます。 ユーザーについての拡張属性により、学生固有の情報が追加されます。  
たとえば、`extension_fe2174665583431c953114ff7268b7b3_Education_Grade` 属性には学生の学年が格納されます。

**注**: すべての要求について、URL クエリ文字列では `api-version` を指定する必要があります。 学生 REST API には、バージョン 1.5 以上が必要です。 例: `https://graph.windows.net/me?api-version=1.5`。   

## 学生の属性

学生に関する情報を特定する際に役立つ属性の説明については、「[学生の属性](education-rest-attributes.md#StudentAttributes)」をご覧ください

****

<a name="GetCurrentUser"> </a>
##現在のユーザーの取得

現在ログインしているユーザーを取得し、そのユーザーが学生かどうかを確認することができます。

```no-highlight
GET https://graph.windows.net/me?api-version=1.5
```

[!code-REST-i[students_api_get_current_user.json](./_data/students_api_get_current_user.json)]


 **応答の種類**

現在ログインしているユーザー。

###学生であることの確認

現在ログインしているユーザーは、学生、教職員、または教育に関わらない (管理スタッフなどの) ユーザーである可能性があります。 ユーザーが学生であるかどうかは、アプリケーションで確認できます。 `Student` と等しくなる `Education_ObjectType` 拡張属性についてのクエリを実行します。

```no-highlight
extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Student'
```

****

<a name="GetStudentSchool"> </a>
##学生の教育機関の取得

Azure Active Directory では、教育機関が[管理単位](https://msdn.microsoft.com/en-us/library/azure/dn832057.aspx)として表されます。 これに該当する管理単位の拡張属性により、教育機関固有の情報が追加されます。 たとえば、`extension_fe2174665583431c953114ff7268b7b3_Education_HighestGrade` 属性には、その教育機関の最高学年が格納されています。

ログインしている学生の教育機関は、その学生のメンバーシップを取得して、`extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'School'` と `objectType == 'AdministrativeUnit'` に対するフィルターを適用することで取得できます。

###学生のメンバーシップの取得

ログインしている学生のメンバーシップは、次に示すクエリを使用することで取得できます。


```no-highlight
GET https://graph.windows.net/me/memberOf?api-version=1.5
```


 **応答の種類**

応答には、学生がメンバーになっている複数の Group や DirectoryRole、AdministrativeUnit が含まれています。

###教育機関のフィルター

学生の教育機関は、`extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'School'` と `objectType == 'AdministrativeUnit'` に関するフィルター処理によって取得できます。


**** 

<a name="GetStudentSection"> </a>
##学生のセクションの取得

Azure Active Directory では、セクションが[統合グループ](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations)として表されます。 統合グループについての拡張属性により、セクション固有の情報が追加されます。 たとえば、`extension_fe2174665583431c953114ff7268b7b3_Education_CourseName` 属性には、セクションの講座名が格納されます。

###学生のメンバーシップの取得

学生のメンバーシップは、次に示すクエリを使用することで取得できます。


```no-highlight
GET https://graph.windows.net/me/memberOf?api-version=1.5
```

 **応答の種類**

応答には、学生がメンバーになっている複数の Group や DirectoryRole、AdministrativeUnit が含まれています。

###セクションのフィルター

学生のセクションは、`extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType == 'Section'` と `objectType == 'Group'` に関するフィルター処理によって取得できます。

**** 

<a name="NextSteps"> </a>
## 次の手順

関心がおありかもしれないその他の教育に関連するリソース

- [教育機関 REST 操作](..\api\school-rest-operations.md)を使用した、教育機関情報へのアクセス

- [セクション REST 操作](..\api\section-rest-operations.md)を使用した、セクション情報へのアクセス

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
    
