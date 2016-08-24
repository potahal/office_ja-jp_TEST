---
ms.Toctitle: Education Attributes
title: "O365API リポジトリ スタイルの追加"
description: "ms.TocTitle:Education の属性Title:Education の属性Description:Office 365 Education テナント内の教育機関、セクション、学生、および教職員へのアクセスを提供する Education REST API に求められる属性についてのリファレンスです。ms.ContentId:1f51d3a9-0ab7-4654-a15d-689e0880ebfb ms.topic: リファレンス (API)"
ms.ContentId: 1f51d3a9-0ab7-4654-a15d-689e0880ebfb

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# O365API リポジトリ スタイルの追加
    
 __**適用対象:**Office 365 Education__
<p class="previewnote">This documentation covers features that are currently in preview.</p>


<a name="Overview"> </a>Office 365 Education API は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントから、データを抽出する際に役立ちます。その結果により、教育機関、セクション、教職員、学生および名簿に関する情報が得られます。 These results provide information about schools, sections, teachers, students and rosters.

[School Attributes](#SchoolAttributes) | [Section Attributes](#SectionAttributes) | [Student Attributes](#StudentAttributes) | [Teacher Attributes](#TeacherAttributes)

## 教育機関の属性

<a name="SchoolAttributes"> </a> Schools are represented as [Administrative Units](https://msdn.microsoft.com/en-us/library/azure/dn832057.aspx) in Azure Active Directory. Azure Active Directory では、教育機関が管理単位として表されます。テナントで使用可能な教育機関を取得できます。また、教育機関エンティティに含まれるセクション、学生、および教職員も取得できます。
The native attributes along with the extension attributes on the administrative unit provide all the necessary information about the school. 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**型**|**説明**|
|:-----|:-----|:-----|
|`displayName`|必須|必須|
|`{prefix}_SyncSource_SchoolId`|教育機関の名前|必須|
|`{prefix}_SchoolNumber`|教育機関の一意の ID (SIS によって割り当てられる)|省略可能|
|`{prefix}_SchoolNationalCenterFor EducationStatisticsId`|通常、レポートで示される ID|省略可能|
|`{prefix}_StateId`|NCES によって割り当てられる ID|省略可能|
|`{prefix}_LowestGrade`|教育機関の ID (都道府県によって割り当てられる)|省略可能|
|`{prefix}_HighestGrade`|教育機関の最低学年 {1 ～ 12、Pre-K (保育園)、K (幼稚園)、Other (その他)}|省略可能|
|`{prefix}_SchoolPrincipalSyncSourceId`|教育機関の最高学年 {1 ～ 12、Pre-K (保育園)、K (幼稚園)、Other (その他)}|省略可能|
|`{prefix}_SchoolPrincipalName`|校長の SIS ID|省略可能|
|`{prefix}_SchoolPrincipalEmail`|校長の名前|省略可能|
|`{prefix}_Address`|校長の電子メール|省略可能|
|`{prefix}_City`|教育機関の番地|省略可能|
|`{prefix}_State`|教育機関の市区町村|省略可能|
|`{prefix}_Country`|教育機関の都道府県|省略可能|
|`{prefix}_Zip`|教育機関の国|省略可能|
|`{prefix}_Phone`|教育機関の郵便番号|省略可能|
|`{prefix}_SchoolZone`|教育機関の連絡先電話番号|省略可能|
|`{prefix}_AnchorId`|教育機関の地域/地区|内部|
|`{prefix}_ObjectType`|アンカー ID。各教育機関を一意に特定するために内部的に使用される|内部|
|`{prefix}_SyncSource`|教育機関のオブジェクトの種類は 'School' です|内部|


**** 


## この教育機関についての情報のソース

<a name="SectionAttributes"> </a> Sections are represented as [Unified Groups](https://msdn.microsoft.com/en-us/office/office365/howto/groups-rest-operations) in Azure Active Directory. Azure Active Directory では、セクションが統合グループとして表されます。テナントで使用可能なセクションを取得できます。また、各セクションに含まれる学生と教職員、およびセクションが属する教育機関の詳細も取得できます。 The native attributes along with the extension attributes on the unified groups provide all the necessary information about the section. 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**型**|**説明**|
|:-----|:-----|:-----|
|`displayName`|必須|必須|
|`{prefix}_SectionId`|わかりやすいセクションの名前|必須|
|`{prefix}_SectionNumber`|このセクションの一意の ID (SIS によって割り当てられる)|省略可能|
|`{prefix}_SectionName`|このセクションの番号 (地方自治体/教育機関によって割り当てられる)|必須|
|`{prefix}_TermId`|セクションの名前|省略可能|
|`{prefix}_TermName`|セクションの学期 SIS ID|省略可能|
|`{prefix}_TermStartDate`|このセクションに関連付けられた学期名 (秋期、冬期、春期、夏期など)|省略可能|
|`{prefix}_TermEndDate`|セクションの学期の開始日|省略可能|
|`{prefix}_SchoolId`|セクションの学期の終了日|必須|
|`{prefix}_CourseId`|セクションの教育機関 SIS ID|省略可能|
|`{prefix}_CourseName`|この講座の ID (SIS によって割り当てられる)|省略可能|
|`{prefix}_CourseDescription`|講座の名前|省略可能|
|`{prefix}_CourseNumber`|この講座の説明|省略可能|
|`{prefix}_CourseSubject`|講座の番号 (地方自治体/教育機関によって割り当てられる)|省略可能|
|`{prefix}_Period`|このセクションの科目|省略可能|
|`{prefix}_AnchorId`|セクションのすべての期間を結合した文字列|内部|
|`{prefix}_ObjectType`|アンカー ID。各セクションを一意に特定するために内部的に使用される|内部|
|`{prefix}_SyncSource`|教育機関のオブジェクトの種類は 'Section' です|内部|


**** 


## このセクションについての情報のソース

<a name="StudentAttributes"> </a> Students are represented as Users in Azure Active Directory. Azure Active Directory では、学生はユーザーとして表されます。現在ログインしている学生についての情報を取得できます。また、その学生を含む教育機関とセクションも取得できます。 The native attributes along with the extension attributes on the user provide all the necessary information about the student. 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**型**|**説明**|
|:-----|:-----|:-----|
|`mailNickname`|必須|必須|
|`userPrincipalName`|名簿に記載された学生の一意の別名|必須|
|`givenName`|学生の正式な電子メール アドレス|必須|
|`surName`|学生の名前 (名)|必須|
|`{prefix}_MiddleName`|学生の名前 (姓)|省略可能|
|`{prefix}_SyncSource_StudentId`|学生のミドル ネーム|必須|
|`{prefix}_SyncSource_SchoolId`|学生の一意の ID (SIS によって割り当てられる)|必須|
|`{prefix}_Email`|学生の教育機関の ID|省略可能|
|`{prefix}_StateId`|学生の個人用電子メール アドレス|省略可能|
|`{prefix}_StudentNumber`|学生の番号 (都道府県によって割り当てられる)|省略可能|
|`{prefix}_MailingAddress`|学生の番号 (地方自治体/教育機関によって割り当てられる)|省略可能|
|`{prefix}_MailingCity`|学生の郵送先住所|省略可能|
|`{prefix}_MailingState`|学生の郵送先市区町村|省略可能|
|`{prefix}_MailingZip`|学生の郵送先都道府県|省略可能|
|`{prefix}_MailingLatitude`|学生の郵便番号|省略可能|
|`{prefix}_MailingLongitude`|郵送先住所の緯度|省略可能|
|`{prefix}_MailingCountry`|郵送先住所の経度|省略可能|
|`{prefix}_ResidenceAddress`|学生の郵送先の国|省略可能|
|`{prefix}_ResidenceCity`|学生の現住所|省略可能|
|`{prefix}_ResidenceState`|学生の現住所の市区町村|省略可能|
|`{prefix}_ResidenceZip`|学生の現住所の都道府県|省略可能|
|`{prefix}_ResidenceLatitude`|学生の現住所の郵便番号|省略可能|
|`{prefix}_ResidenceLongitude`|現住所の緯度|省略可能|
|`{prefix}_ResidenceCountry`|現住所の経度|省略可能|
|`{prefix}_Gender`|学生の現住所の国|省略可能|
|`{prefix}_DateOfBirth`|学生の性別|省略可能|
|`{prefix}_Grade`|学生の生年月日|省略可能|
|`{prefix}_EnglishLanguageLearnersStatus`|学生の学年|省略可能|
|`{prefix}_FederalRace`|英語学習者の状態|省略可能|
|`{prefix}_GraduationYear`|学生の全米学力基準|省略可能|
|`{prefix}_StudentStatus`|学生の卒業年|省略可能|
|`{prefix}_AnchorId`|学生の状態 {Pre-Registered (事前登録済み)、Active (アクティブ)、Transferred-Out (転校済み)、休学 (Suspended)、Graduated (卒業済み)、Historical (過去に在席)、Inactive (非アクティブ) など}|内部|
|`{prefix}_ObjectType`|アンカー ID。各学生を一意に特定するために内部的に使用される|内部|


**** 


## 教育機関のオブジェクトの種類は 'Student' です

<a name="TeacherAttributes"> </a> Teachers are represented as Users in Azure Active Directory. Azure Active Directory では、教師はユーザーとして表されます。ログオンしている現在の教師の属性、およびその教師がメンバーである学校とセクションの属性を取得できます。 The native attributes along with the extension attributes on the user provide all the necessary information about the teacher. 

```no-highlight
{prefix} is 'extension_fe2174665583431c953114ff7268b7b3_Education'
```

|**属性**|**型**|**説明**|
|:-----|:-----|:-----|
|`mailNickname`|必須|必須|
|`userPrincipalName`|名簿に記載された教職員の一意の別名|必須|
|`givenName`|教職員の正式な電子メール アドレス|必須|
|`surName`|教職員の名前 (名)|必須|
|`{prefix}_MiddleName`|教職員の名前 (姓)|省略可能|
|`{prefix}_SyncSource_TeacherId`|教職員のミドル ネーム|必須|
|`{prefix}_SyncSource_SchoolId`|教職員の一意の ID (SIS によって割り当てられる)|必須|
|`{prefix}_StateId`|教職員の教育機関の ID|省略可能|
|`{prefix}_TeacherNumber`|教職員の身分証明書番号/認定番号|省略可能|
|`{prefix}_TeacherStatus`|教職員の番号 (地方自治体/教育機関によって割り当てられる)|省略可能|
|`{prefix}_Email`|教職員の状態 {Active (アクティブ)、On Leave (休暇中)、No Longer Here (退職) など}|省略可能|
|`{prefix}_Title`|教職員の個人用電子メール|省略可能|
|`{prefix}_TeacherQualification`|教職員の役職|省略可能|
|`{prefix}_AnchorId`|教職員の資格|内部|
|`{prefix}_ObjectType`|アンカー ID。各教職員を一意に特定するために内部的に使用される|内部|


**** 
<a name="NextSteps"> </a>
## 次のステップ

次に、興味の対象になると考えられる、その他の Education に関連するリソースを示します。

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
    
- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)
    
- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)
  
- [メール API リファレンス](..\api\mail-rest-operations.md)
  
- [予定表 API リファレンス](..\api\calendar-rest-operations.md)

- [Files API リファレンス](..\api\files-rest-operations.md)

- [検出サービス REST API 操作リファレンス](..\api\discovery-service-rest-operations.md)

- [メール、予定表、連絡先 REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)
    
