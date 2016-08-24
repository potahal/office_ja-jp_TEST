---
ms.Toctitle: Education Attributes
title: "Education の属性"
description: "Office 365 Education テナント内の教育機関、セクション、学生、および教師へのアクセスを提供する Education REST API に求められる属性についてのリファレンスです。"
ms.ContentId: 1f51d3a9-0ab7-4654-a15d-689e0880ebfb

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Education の属性
    
 _**適用対象:**Office 365 Education_
<p class="previewnote">このドキュメントで取り上げる機能は、現時点ではプレビュー段階にあります。</p>


<a name="Overview"> </a> Office 365 Education API は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントから、データを抽出する際に役立ちます。 その結果により、教育機関、セクション、教師、学生および名簿に関する情報が得られます。

[教育機関の属性](#SchoolAttributes) | [セクションの属性](#SectionAttributes) | [学生の属性](#StudentAttributes) | [教師の属性](#TeacherAttributes)

## 教育機関の属性


  <a name="SchoolAttributes"> </a> Azure Active Directory では、教育機関が[管理単位](https://msdn.microsoft.com/en-us/library/azure/dn832057.aspx)として表されます。 テナントで使用可能な教育機関を取得できます。また、教育機関エンティティに含まれるセクション、学生、および教師も取得できます。
管理単位のネイティブ属性と拡張属性によって、教育機関について必要な情報がすべて得られます。 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**種類**|**説明**|
|:-----|:-----|:-----|
|`displayName`|必須|教育機関の名前|
|`{prefix}_SyncSource_SchoolId`|必須|教育機関の一意の ID (SIS によって割り当てられる)|
|`{prefix}_SchoolNumber`|省略可|通常、レポートで示される ID|
|`{prefix}_SchoolNationalCenterFor EducationStatisticsId`|省略可|NCES によって割り当てられる ID|
|`{prefix}_StateId`|省略可|教育機関の ID (都道府県によって割り当てられる)|
|`{prefix}_LowestGrade`|省略可能|教育機関の最低学年 {1 ～ 12、Pre-K (保育園)、K (幼稚園)、Other (その他)}|
|`{prefix}_HighestGrade`|省略可|教育機関の最高学年 {1 ～ 12、Pre-K (保育園)、K (幼稚園)、Other (その他)}|
|`{prefix}_SchoolPrincipalSyncSourceId`|省略可能|校長の SIS ID|
|`{prefix}_SchoolPrincipalName`|省略可|校長の名前|
|`{prefix}_SchoolPrincipalEmail`|省略可|校長の電子メール|
|`{prefix}_Address`|省略可|教育機関の番地|
|`{prefix}_City`|省略可|教育機関の市区町村|
|`{prefix}_State`|省略可|教育機関の都道府県|
|`{prefix}_Country`|省略可|教育機関の国|
|`{prefix}_Zip`|省略可|教育機関の郵便番号|
|`{prefix}_Phone`|省略可能|教育機関の連絡先電話番号|
|`{prefix}_SchoolZone`|省略可|教育機関の地域/地区|
|`{prefix}_AnchorId`|内部|アンカー ID。各教育機関を一意に特定するために内部的に使用される|
|`{prefix}_ObjectType`|内部|教育機関のオブジェクトの種類は 'School' です|
|`{prefix}_SyncSource`|内部|この教育機関についての情報のソース|


**** 


## セクションの属性

<a name="SectionAttributes"> </a> Azure Active Directory では、セクションが[統合グループ](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations)として表されます。 テナントで使用可能なセクションを取得できます。また、各セクションに含まれる学生と教師、およびセクションが属する教育機関の詳細も取得できます。 統合グループのネイティブ属性と拡張属性によって、セクションについて必要な情報がすべて得られます。 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**種類**|**説明**|
|:-----|:-----|:-----|
|`displayName`|必須|わかりやすいセクションの名前|
|`{prefix}_SectionId`|必須|このセクションの一意の ID (SIS によって割り当てられる)|
|`{prefix}_SectionNumber`|省略可|このセクションの番号 (地方自治体/教育機関によって割り当てられる)|
|`{prefix}_SectionName`|必須|セクションの名前|
|`{prefix}_TermId`|省略可|セクションの学期 SIS ID|
|`{prefix}_TermName`|省略可|このセクションに関連付けられた学期名 (秋期、冬期、春期、夏期など)|
|`{prefix}_TermStartDate`|省略可能|セクションの学期の開始日|
|`{prefix}_TermEndDate`|省略可|セクションの学期の終了日|
|`{prefix}_SchoolId`|必須|セクションの教育機関 SIS ID|
|`{prefix}_CourseId`|省略可|この講座の ID (SIS によって割り当てられる)|
|`{prefix}_CourseName`|省略可能|講座の名前|
|`{prefix}_CourseDescription`|省略可|この講座の説明|
|`{prefix}_CourseNumber`|省略可|講座の番号 (地方自治体/教育機関によって割り当てられる)|
|`{prefix}_CourseSubject`|省略可|このセクションの科目|
|`{prefix}_Period`|省略可|セクションのすべての期間を結合した文字列|
|`{prefix}_AnchorId`|内部|アンカー ID。各セクションを一意に特定するために内部的に使用される|
|`{prefix}_ObjectType`|内部|教育機関のオブジェクトの種類は 'Section' です|
|`{prefix}_SyncSource`|内部|このセクションについての情報のソース|


**** 


## 学生の属性

<a name="StudentAttributes"> </a> Azure Active Directory では、学生がユーザーとして表されます。 ログインしている現在の学生の情報を取得できます。また、学生が属している教育機関とセクションも取得できます。 ユーザーのネイティブ属性と拡張属性によって、学生について必要な情報がすべて得られます。 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**種類**|**説明**|
|:-----|:-----|:-----|
|`mailNickname`|必須|名簿に記載された学生の一意の別名|
|`userPrincipalName`|必須|学生の正式な電子メール アドレス|
|`givenName`|必須|学生の名前 (名)|
|`surName`|必須|学生の名前 (姓)|
|`{prefix}_MiddleName`|省略可|学生のミドル ネーム|
|`{prefix}_SyncSource_StudentId`|必須|学生の一意の ID (SIS によって割り当てられる)|
|`{prefix}_SyncSource_SchoolId`|必須|学生の教育機関の ID|
|`{prefix}_Email`|省略可|学生の個人用電子メール アドレス|
|`{prefix}_StateId`|省略可能|学生の番号 (都道府県によって割り当てられる)|
|`{prefix}_StudentNumber`|省略可|学生の番号 (地方自治体/教育機関によって割り当てられる)|
|`{prefix}_MailingAddress`|省略可能|学生の郵送先住所|
|`{prefix}_MailingCity`|省略可|学生の郵送先市区町村|
|`{prefix}_MailingState`|省略可|学生の郵送先都道府県|
|`{prefix}_MailingZip`|省略可|学生の郵便番号|
|`{prefix}_MailingLatitude`|省略可能|郵送先住所の緯度|
|`{prefix}_MailingLongitude`|省略可|郵送先住所の経度|
|`{prefix}_MailingCountry`|省略可能|学生の郵送先の国|
|`{prefix}_ResidenceAddress`|省略可|学生の現住所|
|`{prefix}_ResidenceCity`|省略可能|学生の現住所の市区町村|
|`{prefix}_ResidenceState`|省略可能|学生の現住所の都道府県|
|`{prefix}_ResidenceZip`|省略可|学生の現住所の郵便番号|
|`{prefix}_ResidenceLatitude`|省略可|現住所の緯度|
|`{prefix}_ResidenceLongitude`|省略可|現住所の経度|
|`{prefix}_ResidenceCountry`|省略可|学生の現住所の国|
|`{prefix}_Gender`|省略可|学生の性別|
|`{prefix}_DateOfBirth`|省略可|学生の生年月日|
|`{prefix}_Grade`|省略可能|学生の学年|
|`{prefix}_EnglishLanguageLearnersStatus`|省略可能|英語学習者の状態|
|`{prefix}_FederalRace`|省略可能|学生の全米学力基準|
|`{prefix}_GraduationYear`|省略可|学生の卒業年|
|`{prefix}_StudentStatus`|省略可|学生の状態 {Pre-Registered (事前登録済み)、Active (アクティブ)、Transferred-Out (転校済み)、休学 (Suspended)、Graduated (卒業済み)、Historical (過去に在席)、Inactive (非アクティブ) など}|
|`{prefix}_AnchorId`|内部|アンカー ID。各学生を一意に特定するために内部的に使用される|
|`{prefix}_ObjectType`|内部|教育機関のオブジェクトの種類は 'Student' です|


**** 


## 教師の属性

<a name="TeacherAttributes"> </a> Azure Active Directory では、教師がユーザーとして表されます。 ログインしている現在の教師の情報を取得できます。また、教師が属している教育機関とセクションも取得できます。 ユーザーのネイティブ属性と拡張属性によって、教師について必要な情報がすべて得られます。 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**種類**|**説明**|
|:-----|:-----|:-----|
|`mailNickname`|必須|名簿に記載された教職員の一意の別名|
|`userPrincipalName`|必須|教職員の正式な電子メール アドレス|
|`givenName`|必須|教職員の名前 (名)|
|`surName`|必須|教職員の名前 (姓)|
|`{prefix}_MiddleName`|省略可|教職員のミドル ネーム|
|`{prefix}_SyncSource_TeacherId`|必須|教職員の一意の ID (SIS によって割り当てられる)|
|`{prefix}_SyncSource_SchoolId`|必須|教職員の教育機関の ID|
|`{prefix}_StateId`|省略可能|教職員の身分証明書番号/認定番号|
|`{prefix}_TeacherNumber`|省略可|教職員の番号 (地方自治体/教育機関によって割り当てられる)|
|`{prefix}_TeacherStatus`|省略可能|教職員の状態 {Active (アクティブ)、On Leave (休暇中)、No Longer Here (退職) など}|
|`{prefix}_Email`|省略可|教職員の個人用電子メール|
|`{prefix}_Title`|省略可能|教職員の役職|
|`{prefix}_TeacherQualification`|省略可|教職員の資格|
|`{prefix}_AnchorId`|内部|アンカー ID。各教職員を一意に特定するために内部的に使用される|
|`{prefix}_ObjectType`|内部|教育機関のオブジェクトの種類は 'Teacher' です|


**** 
<a name="NextSteps"> </a>
## 次の手順

関心がおありかもしれないその他の教育に関連するリソース

- [教育機関 REST 操作](..\api\school-rest-operations.md)を使用した、教育機関情報へのアクセス

- [セクション REST 操作](..\api\section-rest-operations.md)を使用した、セクション情報へのアクセス

- [学生 Rest 操作](..\api\student-rest-operations.md)を使用した、学生情報へのアクセス

- [教職員 Rest 操作](..\api\teacher-rest-operations.md)を使用した、教職員情報へのアクセス 


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
    
