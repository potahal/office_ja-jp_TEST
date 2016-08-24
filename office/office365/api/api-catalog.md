---
ms.Toctitle: Office 365 API reference
title: "Office 365 API リファレンス"
description: "Office 365 API リファレンスと、REST およびクライアント ライブラリ API に関する情報を参照してください。"
ms.ContentId: 6736150c-641e-4e1b-bcc0-6cce0996779d
ms.date: July 26, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Office 365 API リファレンス 
    
 _**適用対象:**Exchange Online | Office 365 | OneDrive for Business_

[!INCLUDE [Use Microsoft Graph](../includes/use-msgraph-note.txt)]

Office 365 API では、メール、予定表、連絡先、ユーザーおよびグループ、ファイル、およびフォルダーなど、お客様が最も大事にしている Office 365 データにアプリ本体から直接アクセスできるようになりました。 
            
Office 365 API へは、モバイル、Web、およびデスクトップ プラットフォームのすべてのソリューションからアクセスできます。開発プラットフォームやツールには依存しません。つまり、.NET、PHP、Java、Python、Ruby on Rails を使用して Web アプリケーションを構築しても、Windows ユニバーサル アプリ、iOS、Android や他のデバイス プラットフォーム用のアプリを作成してもかまいません。
    
### アプリ開発を簡単にする Office 365 SDK

直接 REST API を使用してプログラミングすれば、Office 365 を操作でき、認証トークンの管理、アクセスする API 用の正しい URL やクエリの構築、その他のタスクなどを実行するコードを記述および維持できます。 Visual Studio、Eclipse および Android Studio、Xcode 用の Office 365 SDK では、Office 365 API にアクセスする複雑なコードが単純になります。

[Visual Studio の Office 開発ツール](http://aka.ms/OfficeDevToolsForVS2013)には、Office 365 の REST サービスをラップする Office 365 データへの接続を容易にする .NET や JavaScript のライブラリとなる SDK が含まれています。Visual Studio の SDK は、ASP.NET MVC、ASP.NET Web フォーム、WPF、Win フォーム、ユニバーサル アプリ、Cordova、Xamarin プロジェクトの種類用に用意されています。 

また、Android 開発者用に、Android SDK for Eclipse と Android Studio が一般に利用できるようになりました。  「[Android 用の Office 365 SDK](https://github.com/OfficeDev/Office-365-SDK-for-Android)」をご覧ください。 

iOS アプリ用には、[Xcode 6](https://developer.apple.com/xcode/) 内で Xcode 用 iOS SDK (プレビュー) が Objective C と Swift 言語の両方でサポートされています。 「[iOS 用の Office 365 SDK](https://github.com/OfficeDev/Office-365-SDK-for-iOS)」をご覧ください。 


<!--
<a href="#bk_groups"><img alt="Groups API (preview) icon" src="office/office365/api/images/O365APIs_REST_Reference_icons_groups.png" /></a>
-->

<!--
<a href="#bk_mail" id="Mail">
    <img title="Mail API" src="http://i.imgur.com/qhjA7Qy.png" onmouseover="this.src='http://i.imgur.com/s5rJ51y.png'" onmouseout="this.src='http://i.imgur.com/qhjA7Qy.png'" />
</a>
<a href="#bk_contacts" id="Contacts">
    <img title="Contacts API" src="http://i.imgur.com/xJbLQcp.png" onmouseover="this.src='http://i.imgur.com/idZpJmq.png'" onmouseout="this.src='http://i.imgur.com/xJbLQcp.png'" />
</a>
<a href="#bk_calendar" id="Calendar">
    <img title="Calendar API" src="http://i.imgur.com/dKrWDZ2.png" onmouseover="this.src='http://i.imgur.com/9D0eQGe.png'" onmouseout="this.src='http://i.imgur.com/dKrWDZ2.png'" />
</a>
-->


## Outlook REST 要求のバッチ処理 (プレビュー)

[バッチでの Outlook REST 要求の送信](..\api\batch-outlook-rest-requests.md)


## Outlook タスク (プレビュー)
[タスク API](..\api\task-rest-operations.md)

**タスクの操作** &nbsp;
[タスクの作成](..\api\task-rest-operations.md#CreateTasks) | [タスクの取得](..\api\task-rest-operations.md#GetTasks) | [タスクの更新](..\api\task-rest-operations.md#UpdateTasks) | [タスクの削除](..\api\task-rest-operations.md#DeleteTasks) | 
[タスクの完了](..\api\task-rest-operations.md#CompleteTasks) | [タスクまたはタスク フォルダーの同期](..\api\task-rest-operations.md#SyncTasks) 


**タスク フォルダーの操作** &nbsp;
[タスク フォルダーの作成](..\api\task-rest-operations.md#CreateTaskFolders) | [タスク フォルダーの取得](..\api\task-rest-operations.md#GetTaskFolders) | [タスク フォルダーの更新](..\api\task-rest-operations.md#UpdateTaskFolders) | 
[タスク フォルダーの削除](..\api\task-rest-operations.md#DeleteTaskFolders) | [タスクまたはタスク フォルダーの同期](..\api\task-rest-operations.md#SyncTasks) 


**タスク グループの操作** &nbsp;
[タスク グループの作成](..\api\task-rest-operations.md#CreateTaskGroups) | [タスク グループの取得](..\api\task-rest-operations.md#GetTaskGroups) | [タスク グループの更新](..\api\task-rest-operations.md#UpdateTaskGroups) | 
[タスク グループの削除](..\api\task-rest-operations.md#DeleteTaskGroups)


<a name="bk_people"> </a>

##Outlook People (プレビュー)

[People API](..\api\people-rest-operations.md)

**参照:** &nbsp; 
[参照](..\api\people-rest-operations.md#BrowsePeople) | [ページング](..\api\people-rest-operations.md#BrowsePaging) | 
[並べ替え](..\api\people-rest-operations.md#BrowseSort) | [制限](..\api\people-rest-operations.md#BrowsePageSize) |
[選択](..\api\people-rest-operations.md#BrowseSelecting) | [フィルター処理](..\api\people-rest-operations.md#BrowseFiltering) |
[フィルター処理と選択](..\api\people-rest-operations.md#BrowseSelectingAndFiltering)

**検索:**&nbsp;
[トピックの選択](..\api\people-rest-operations.md#SearchTopic) |
[検索のフィルター処理](..\api\people-rest-operations.md#SearchFilter) |
[あいまい検索](..\api\people-rest-operations.md#FuzzySearch)



## Office 365 のデータ拡張機能

[データ拡張機能 API](..\api\extensions-rest-operations.md)

**オープン型の拡張機能:**&nbsp; [既存のアイテムに作成](..\api\extensions-rest-operations.md#CreateExtensionInExistingItem) | [新しいアイテムで作成](..\api\extensions-rest-operations.md#CreateExtensionInNewItem) |
[取得](..\api\extensions-rest-operations.md#GetExtension) | [拡張アイテムの取得](..\api\extensions-rest-operations.md#GetExpandedExtension) |
[更新](..\api\extensions-rest-operations.md#UpdateExtension) | [削除](..\api\extensions-rest-operations.md#DeleteExtension)


## Outlook の拡張プロパティ

[拡張プロパティ API](..\api\extended-properties-rest-operations.md)

**拡張プロパティ**  &nbsp;  [既存のアイテムに作成](..\api\extended-properties-rest-operations.md#CreateExtendedPropertyInExistingItem) | 
[新しいアイテムで作成](..\api\extended-properties-rest-operations.md#CreateExtendedPropertyInNewItem) | 
[拡張アイテムの取得](..\api\extended-properties-rest-operations.md#GetExpandedExtendedProperty) | 
[フィルター処理](..\api\extended-properties-rest-operations.md#GetItemByFilteringExtendedProperty)



<a name="bk_mail"> </a>

##Outlook のメール

[メール API](..\api\mail-rest-operations.md) 

**メッセージ:**&nbsp; [取得](..\api\mail-rest-operations.md#Getmessages) | [作成と送信](..\api\mail-rest-operations.md#Sendmessages) |
 [返信](..\api\mail-rest-operations.md#Replytomessages) | [転送](..\api\mail-rest-operations.md#Forwardmessages) |
 [更新](..\api\mail-rest-operations.md#Updatemessages) | [削除](..\api\mail-rest-operations.md#DeleteMessages) |
 [移動またはコピー](..\api\mail-rest-operations.md#MoveCopymessages)

**添付ファイル:**&nbsp;[取得](..\api\mail-rest-operations.md#GetAttachments) |
 [作成](..\api\mail-rest-operations.md#CreateAttachments) | [削除](..\api\mail-rest-operations.md#DeleteAttachments)

**フォルダー:**&nbsp;[取得](..\api\mail-rest-operations.md#GetFolders) | [作成](..\api\mail-rest-operations.md#CreateFolders) | [更新](..\api\mail-rest-operations.md#UpdateFolders) |
 [削除](..\api\mail-rest-operations.md#DeleteFolders) | [移動またはコピー](..\api\mail-rest-operations.md#MoveCopyFolders)


<a name="bk_contacts"> </a>

##Outlook の連絡先

[連絡先 API](..\api\contacts-rest-operations.md)

**連絡先:**&nbsp; [取得](..\api\contacts-rest-operations.md#GetContacts) | [作成](..\api\contacts-rest-operations.md#CreateContacts) |
 [更新](..\api\contacts-rest-operations.md#UpdateContacts) | [削除](..\api\contacts-rest-operations.md#DeleteContacts) 
 

**連絡先フォルダー:**&nbsp; [取得](..\api\contacts-rest-operations.md#GetContactFolders)


<a name="bk_calendar"> </a>

##Outlook の予定表

[予定表 API](..\api\calendar-rest-operations.md)

**予定表ビュー:**&nbsp;  [取得](..\api\calendar-rest-operations.md#GetCalendarView) | [同期](..\api\calendar-rest-operations.md#SyncCalendarView)

**イベント:**&nbsp;  [取得](..\api\calendar-rest-operations.md#GetEvents) | [同期](..\api\calendar-rest-operations.md#SyncCalendarView) |
 [作成](..\api\calendar-rest-operations.md#CreateEvents) |
 [更新](..\api\calendar-rest-operations.md#UpdateEvents) | [応答](..\api\calendar-rest-operations.md#RespondToEvents) | 
 [削除](..\api\calendar-rest-operations.md#DeleteEvents)

**添付ファイル:**&nbsp; 
 [取得](..\api\calendar-rest-operations.md#GetAttachments) | [作成](..\api\calendar-rest-operations.md#reateAttachments) |
 [削除](..\api\calendar-rest-operations.md#DeleteAttachments)
 
 **アラーム:** &nbsp;
 [取得](..\api\calendar-rest-operations.md#GetReminders) | 
 [再通知](..\api\calendar-rest-operations.md#SnoozeReminders) | 
 [解除](..\api\calendar-rest-operations.md#DismissReminders)

**予定表:**&nbsp;  [取得](..\api\calendar-rest-operations.md#GetCalendars) |
 [作成](..\api\calendar-rest-operations.md#CreateCalendars) | [更新](..\api\calendar-rest-operations.md#UpdateCalendars) |
 [削除](..\api\calendar-rest-operations.md#DeleteCalendars)

**予定表グループ:**&nbsp; [取得](..\api\calendar-rest-operations.md#GetCalendarGroups) |
 [作成](..\api\calendar-rest-operations.md#CreateCalendarGroups) | [更新](..\api\calendar-rest-operations.md#UpdateCalendarGroups) |
 [削除](..\api\calendar-rest-operations.md#DeleteCalendarGroups)



##メール API、予定表 API、連絡先 API、タスク API のリソース リファレンス

[リソース リファレンス](..\API\complex-types-for-mail-contacts-calendar.md) 
 
 **エンティティ:**&nbsp;  [Calendar](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource) |
 [CalendarGroup](..\api\complex-types-for-mail-contacts-calendar.md#CalendarGroupResource) | [Contact](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource) |
 [ContactFolder](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource) | [Event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) |
 [EventMessage](..\api\complex-types-for-mail-contacts-calendar.md#EventMessageResource) | [Extended properties](..\api\complex-types-for-mail-contacts-calendar.md#ExtendedProperties) | 
 [FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource) | [Folder](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource) |
 [ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource) | [Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) |
 [Task (プレビュー)](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource) | [TaskFolder (プレビュー)](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource) | 
 [TaskGroup (プレビュー)](..\api\complex-types-for-mail-contacts-calendar.md#TaskGroupResource) | 
 [User](..\api\complex-types-for-mail-contacts-calendar.md#UserResource) 
 
 **複合型:**&nbsp;   [Attendee](..\api\complex-types-for-mail-contacts-calendar.md#Attendee) | 
 [AttendeeBase](..\api\complex-types-for-mail-contacts-calendar.md#AttendeeBase) (プレビュー) |  [AttendeeAvailability](..\api\complex-types-for-mail-contacts-calendar.md#AttendeeAvailability) (プレビュー) |  [DateTimeTimeZone](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZoneBeta) | 
 [EmailAddress](..\api\complex-types-for-mail-contacts-calendar.md#EmailAddress) | 
 [GeoCoordinates](..\api\complex-types-for-mail-contacts-calendar.md#GeoCoordinates) | 
 [ItemBody](..\api\complex-types-for-mail-contacts-calendar.md#ItemBody)  | 
 [Location](..\api\complex-types-for-mail-contacts-calendar.md#Location) (プレビュー) |  [LocationConstraint](..\api\complex-types-for-mail-contacts-calendar.md#LocationConstraint) (プレビュー) |  [MeetingTimeCandidate](..\api\complex-types-for-mail-contacts-calendar.md#MeetingTimeCandidate) (プレビュー) |  [PatternedRecurrence](..\api\complex-types-for-mail-contacts-calendar.md#PatternedRecurrence) | 
 [PhysicalAddress](..\api\complex-types-for-mail-contacts-calendar.md#PhysicalAddress) | 
 [Recipient](..\api\complex-types-for-mail-contacts-calendar.md#Recipient) | 
 [RecurrencePattern](..\api\complex-types-for-mail-contacts-calendar.md#RecurrencePattern) | 
 [RecurrenceRange](..\api\complex-types-for-mail-contacts-calendar.md#RecurrenceRange) | 
 [ResponseStatus](..\api\complex-types-for-mail-contacts-calendar.md#ResponseStatus) | 
 [TimeConstraint](..\api\complex-types-for-mail-contacts-calendar.md#TimeConstraint) (プレビュー) |  [TimeSlot](..\api\complex-types-for-mail-contacts-calendar.md#TimeSlot) (プレビュー) |  [TimeStamp](..\api\complex-types-for-mail-contacts-calendar.md#TimeStamp) (プレビュー) 
 
 
 **OData クエリ パラメーター:** &nbsp;  [$search](..\api\complex-types-for-mail-contacts-calendar.md#Search) | 
 [$filter](..\api\complex-types-for-mail-contacts-calendar.md#Filter) | [$select](..\api\complex-types-for-mail-contacts-calendar.md#Select) | 
 [$orderby](..\api\complex-types-for-mail-contacts-calendar.md#OrderBy) | [$top および $skip](..\api\complex-types-for-mail-contacts-calendar.md#TopSkip) | 
 $expand | [$count](..\api\complex-types-for-mail-contacts-calendar.md#Count) 

<a name="bk_notify"> </a>

##Outlook の通知

[通知 API](../api/notify-rest-operations.md)


<a name="bk_photo"> </a>

##Outlook のユーザーの写真

[ユーザー写真 API](../api/photo-rest-operations.md)
 

<a name="bk_discovery"> </a>

##探索サービス

[探索サービス API](..\api\discovery-service-rest-operations.md)

[最初のサインイン](..\api\discovery-service-rest-operations.md#DiscoveryServiceoperationsInitialsignin) |
 [特定のサービスの検出](..\api\discovery-service-rest-operations.md#DiscoveryServiceoperationsDiscoverspecificservices) | [検出可能なサービスについて](..\api\discovery-service-rest-operations.md#DiscoveryServiceoperationsLearnwhatservicesarediscoverable)


<a name="bk_files"> </a>

##ファイル

[OneDrive API](https://dev.onedrive.com/)


<a name="bk_video"> </a>

##ビデオ 

[ビデオ API](../api/video-rest-operations.md)

**ビデオ ポータル:**&nbsp; 
  [情報の取得](..\api\video-rest-operations.md#GetPortalInformation)
  
**チャネル:**&nbsp; 
  [情報の取得](..\api\video-rest-operations.md#GetChannelsInfo) 
  
**ビデオの情報:**&nbsp; 
  [取得](..\api\video-rest-operations.md#GetVideoInfo) | [ビデオ メタデータの更新](..\api\video-rest-operations.md#UpdateVideo) 
  
**ビデオ:** &nbsp; 
  [アップロード](..\api\video-rest-operations.md#UploadVideos) | [削除](..\api\video-rest-operations.md#DeleteVideos) 

##21Vianet が運用している Office 365 の API リソースおよびサービス エンドポイント
[21Vianet が運用している Office 365 の API エンドポイント](..\api\o365-china-endpoints.md)

##Office 365 管理 API

  [はじめに](https://msdn.microsoft.com/library/office/dn707383.aspx) | [サービス通信 API (プレビュー)](https://msdn.microsoft.com/EN-US/library/dn707386.aspx) | [管理アクティビティ API リファレンス (プレビュー)](https://msdn.microsoft.com/library/office/mt227394.aspx) | [レポート Web サービス](https://msdn.microsoft.com/en-us/library/office/jj984325.aspx) 

##その他の技術情報

###API サンドボックス
[Office 365 の API を実際に試してみる場合は、この対話型コンソールを使用してください](http://apisandbox.msdn.microsoft.com/)

###Office 365 プラットフォームの概要

[Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md) | [Office 365 開発環境のセットアップ](..\howTo\setup-development-environment.md) 

[Office 365 API を使う](..\howto\getting-started-Office-365-APIs.md) 

###Office 365 のアクセス許可スコープ
[Office 365 のすべてのアクセス許可スコープの詳細](..\howto\application-manifest.md)

###Microsoft パートナー センターの API リファレンス
パートナーは、[CSP Commerce REST API](https://msdn.microsoft.com/en-us/library/partnercenter/dn974944.aspx) を使用して、Microsoft Commerce Platform での顧客アカウントの作成、顧客プロファイルの管理ができます。また、顧客向け Microsoft 製品の注文とサブスクリプションを購入および管理できます。 



