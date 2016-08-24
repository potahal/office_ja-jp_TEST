---
ms.Toctitle: Office 365 API reference
title: "Office 365 API リファレンス"
description: "Find Office 365 API references and info about REST and client library APIs."
ms.ContentId: 6736150c-641e-4e1b-bcc0-6cce0996779d
ms.date: July 26, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Office 365 API リファレンス 
    
 _**Applies to:** Exchange Online | Office 365 | OneDrive for Business_

[!INCLUDE [Use Microsoft Graph](../includes/use-msgraph-note.txt)]

Office 365 API では、メール、予定表、連絡先、ユーザーおよびグループ、ファイル、およびフォルダーなど、お客様が最も大事にしている Office 365 データにアプリ本体から直接アクセスできるようになりました。 
            
Office 365 API へは、モバイル、Web、およびデスクトップ プラットフォームのすべてのソリューションからアクセスできます。開発プラットフォームやツールには依存しません。つまり、.NET、PHP、Java、Python、Ruby on Rails を使用して Web アプリケーションを構築しても、Windows ユニバーサル アプリ、iOS、Android や他のデバイス プラットフォーム用のアプリを作成してもかまいません。
    
### アプリ開発を簡単にする Office 365 SDK

REST API を直接プログラミングして、Office 365 と通信するようにできます。このようにすると、認証トークンの管理、アクセスする API 用の正しい URL やクエリの構築、その他のタスクなどを実行するコードを記述および維持する必要があります。Visual Studio、Eclipse および Android Studio、Xcode 用の Office 365 SDK では、Office 365 API にアクセスする複雑なコードが単純になります。 The Office 365 SDKs for Visual Studio, Eclipse and Android Studio, or Xcode, help reduce the complexity of the code you need to write to access the Office 365 APIs.

[Visual Studio の Office 開発ツール](http://aka.ms/OfficeDevToolsForVS2013)には、Office 365 の REST サービスをラップする Office 365 データへの接続を容易にする .NET や JavaScript のライブラリとなる SDK が含まれています。Visual Studio の SDK は、ASP.NET MVC、ASP.NET Web フォーム、WPF、Win フォーム、ユニバーサル アプリ、Cordova、Xamarin プロジェクトの種類用に用意されています。 

また、Android 開発者用に、Android SDK for Eclipse と Android Studio が一般に利用できるようになりました。「Android 用の Office 365 SDK を使用した Office 365 アプリの開発」を参照してください。  [Office 365 SDK for Android](https://github.com/OfficeDev/Office-365-SDK-for-Android)。 

iOS アプリ用には、[Xcode 6 内で Xcode 用 iOS SDK (現在プレビュー中) が Objective C と Swift 言語の両方でサポートされています。「iOS 用の Office 365 SDK](https://developer.apple.com/xcode/)」を参照してください。 See the [Office 365 SDK for iOS](https://github.com/OfficeDev/Office-365-SDK-for-iOS). 


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

**Task operations** &nbsp;
[Create tasks](..\api\task-rest-operations.md#CreateTasks) | [Get tasks](..\api\task-rest-operations.md#GetTasks) | [Update tasks](..\api\task-rest-operations.md#UpdateTasks) | [Delete tasks](..\api\task-rest-operations.md#DeleteTasks) | 
[Complete tasks](..\api\task-rest-operations.md#CompleteTasks) | [Synchronize tasks or task folders](..\api\task-rest-operations.md#SyncTasks) 


**Task folder operations** &nbsp;
[Create task folders](..\api\task-rest-operations.md#CreateTaskFolders) | [Get task folders](..\api\task-rest-operations.md#GetTaskFolders) | [Update task folders](..\api\task-rest-operations.md#UpdateTaskFolders) | 
[Delete task folders](..\api\task-rest-operations.md#DeleteTaskFolders) | [Synchronize tasks or task folders](..\api\task-rest-operations.md#SyncTasks) 


**Task group operations** &nbsp;
[Create task groups](..\api\task-rest-operations.md#CreateTaskGroups) | [Get task groups](..\api\task-rest-operations.md#GetTaskGroups) | [Update task groups](..\api\task-rest-operations.md#UpdateTaskGroups) | 
[Delete task groups](..\api\task-rest-operations.md#DeleteTaskGroups)


<a name="bk_people"> </a>

##Outlook People (プレビュー)

[People API](..\api\people-rest-operations.md)

**Browse:** &nbsp; 
[Browsing](..\api\people-rest-operations.md#BrowsePeople) | [Paging](..\api\people-rest-operations.md#BrowsePaging) | 
[Sorting](..\api\people-rest-operations.md#BrowseSort) | [Limiting](..\api\people-rest-operations.md#BrowsePageSize) |
[Selecting](..\api\people-rest-operations.md#BrowseSelecting) | [Filtering](..\api\people-rest-operations.md#BrowseFiltering) |
[Filtering and selecting](..\api\people-rest-operations.md#BrowseSelectingAndFiltering)

**Search:** &nbsp;
[Select topic](..\api\people-rest-operations.md#SearchTopic) |
[Filter a search](..\api\people-rest-operations.md#SearchFilter) |
[Fuzzy search](..\api\people-rest-operations.md#FuzzySearch)



## Office 365 のデータ拡張機能

[データ拡張機能 API](..\api\extensions-rest-operations.md)

**Open Type Extensions:** &nbsp; [Create in existing item](..\api\extensions-rest-operations.md#CreateExtensionInExistingItem) | [Create with new item](..\api\extensions-rest-operations.md#CreateExtensionInNewItem) |
[Get](..\api\extensions-rest-operations.md#GetExtension) | [Get expanded item](..\api\extensions-rest-operations.md#GetExpandedExtension) |
[Update](..\api\extensions-rest-operations.md#UpdateExtension) | [Delete](..\api\extensions-rest-operations.md#DeleteExtension)


## Outlook の拡張プロパティ (プレビュー)

[拡張プロパティ API](..\api\extended-properties-rest-operations.md)

**Extended Properties**  &nbsp;  [Create in existing item](..\api\extended-properties-rest-operations.md#CreateExtendedPropertyInExistingItem) | 
[Create with new item](..\api\extended-properties-rest-operations.md#CreateExtendedPropertyInNewItem) | 
[Get expanded item](..\api\extended-properties-rest-operations.md#GetExpandedExtendedProperty) | 
[Filter](..\api\extended-properties-rest-operations.md#GetItemByFilteringExtendedProperty)



<a name="bk_mail"> </a>

##Outlook のメール

[メール API](..\api\mail-rest-operations.md) 

**Messages:** &nbsp; [Get](..\api\mail-rest-operations.md#Getmessages) | [Create and send](..\api\mail-rest-operations.md#Sendmessages) |
 [Reply to](..\api\mail-rest-operations.md#Replytomessages) | [Forward](..\api\mail-rest-operations.md#Forwardmessages) |
 [Update](..\api\mail-rest-operations.md#Updatemessages) | [Delete](..\api\mail-rest-operations.md#DeleteMessages) |
 [Move or copy](..\api\mail-rest-operations.md#MoveCopymessages)

**Attachments:**&nbsp;  [Get](..\api\mail-rest-operations.md#GetAttachments) |
 [Create](..\api\mail-rest-operations.md#CreateAttachments) | [Delete](..\api\mail-rest-operations.md#DeleteAttachments)

**Folders:**&nbsp;  [Get](..\api\mail-rest-operations.md#GetFolders) | [Create](..\api\mail-rest-operations.md#CreateFolders) | [Update](..\api\mail-rest-operations.md#UpdateFolders) |
 [Delete](..\api\mail-rest-operations.md#DeleteFolders) | [Move or copy](..\api\mail-rest-operations.md#MoveCopyFolders)


<a name="bk_contacts"> </a>

##Outlook の連絡先

[連絡先 API](..\api\contacts-rest-operations.md)

**Contacts:**&nbsp;  [Get](..\api\contacts-rest-operations.md#GetContacts) | [Create](..\api\contacts-rest-operations.md#CreateContacts) |
 [Update](..\api\contacts-rest-operations.md#UpdateContacts) | [Delete](..\api\contacts-rest-operations.md#DeleteContacts) 
 

**Contact folders:**&nbsp;  [Get](..\api\contacts-rest-operations.md#GetContactFolders)


<a name="bk_calendar"> </a>

##Outlook の予定表

[予定表 API](..\api\calendar-rest-operations.md)

**Calendar view:**  &nbsp;  [Get](..\api\calendar-rest-operations.md#GetCalendarView) | [Sync](..\api\calendar-rest-operations.md#SyncCalendarView)

**Events:** &nbsp;  [Get](..\api\calendar-rest-operations.md#GetEvents) | [Sync](..\api\calendar-rest-operations.md#SyncCalendarView) |
 [Create](..\api\calendar-rest-operations.md#CreateEvents) |
 [Update](..\api\calendar-rest-operations.md#UpdateEvents) | [Respond](..\api\calendar-rest-operations.md#RespondToEvents) | 
 [Delete](..\api\calendar-rest-operations.md#DeleteEvents)

**Attachments:** &nbsp; 
 [Get](..\api\calendar-rest-operations.md#GetAttachments) | [Create](..\api\calendar-rest-operations.md#reateAttachments) |
 [Delete](..\api\calendar-rest-operations.md#DeleteAttachments)
 
 **Reminders:** &nbsp;
 [Get](..\api\calendar-rest-operations.md#GetReminders) | 
 [Snooze](..\api\calendar-rest-operations.md#SnoozeReminders) | 
 [Dismiss](..\api\calendar-rest-operations.md#DismissReminders)

**Calendars:** &nbsp;  [Get](..\api\calendar-rest-operations.md#GetCalendars) |
 [Create](..\api\calendar-rest-operations.md#CreateCalendars) | [Update](..\api\calendar-rest-operations.md#UpdateCalendars) |
 [Delete](..\api\calendar-rest-operations.md#DeleteCalendars)

**Calendar groups:**&nbsp;   [Get](..\api\calendar-rest-operations.md#GetCalendarGroups) |
 [Create](..\api\calendar-rest-operations.md#CreateCalendarGroups) | [Update](..\api\calendar-rest-operations.md#UpdateCalendarGroups) |
 [Delete](..\api\calendar-rest-operations.md#DeleteCalendarGroups)



##メール API、予定表 API、連絡先 API、タスク API のリソース リファレンス

[リソース リファレンス](..\API\complex-types-for-mail-contacts-calendar.md) 
 
 **Entities:** &nbsp;  [Calendar](..\api\complex-types-for-mail-contacts-calendar.md#CalendarResource) |
 [CalendarGroup](..\api\complex-types-for-mail-contacts-calendar.md#CalendarGroupResource) | [Contact](..\api\complex-types-for-mail-contacts-calendar.md#ContactResource) |
 [ContactFolder](..\api\complex-types-for-mail-contacts-calendar.md#ContactFolderResource) | [Event](..\api\complex-types-for-mail-contacts-calendar.md#EventResource) |
 [EventMessage](..\api\complex-types-for-mail-contacts-calendar.md#EventMessageResource) | [Extended properties](..\api\complex-types-for-mail-contacts-calendar.md#ExtendedProperties) | 
 [FileAttachment](..\api\complex-types-for-mail-contacts-calendar.md#FileAttachmentResource) | [Folder](..\api\complex-types-for-mail-contacts-calendar.md#FolderResource) |
 [ItemAttachment](..\api\complex-types-for-mail-contacts-calendar.md#ItemAttachmentResource) | [Message](..\api\complex-types-for-mail-contacts-calendar.md#MessageResource) |
 [Task (preview)](..\api\complex-types-for-mail-contacts-calendar.md#TaskResource) | [TaskFolder (preview)](..\api\complex-types-for-mail-contacts-calendar.md#TaskFolderResource) | 
 [TaskGroup (preview)](..\api\complex-types-for-mail-contacts-calendar.md#TaskGroupResource) | 
 [User](..\api\complex-types-for-mail-contacts-calendar.md#UserResource) 
 
 **Complex types:** &nbsp;   [Attendee](..\api\complex-types-for-mail-contacts-calendar.md#Attendee) | 
 [AttendeeBase](..\api\complex-types-for-mail-contacts-calendar.md#AttendeeBase) (preview) |  [AttendeeAvailability](..\api\complex-types-for-mail-contacts-calendar.md#AttendeeAvailability) (preview) |  [DateTimeTimeZone](..\api\complex-types-for-mail-contacts-calendar.md#DateTimeTimeZoneBeta) | 
 [EmailAddress](..\api\complex-types-for-mail-contacts-calendar.md#EmailAddress) | 
 [GeoCoordinates](..\api\complex-types-for-mail-contacts-calendar.md#GeoCoordinates) | 
 [ItemBody](..\api\complex-types-for-mail-contacts-calendar.md#ItemBody)  | 
 [Location](..\api\complex-types-for-mail-contacts-calendar.md#Location) (preview) |  [LocationConstraint](..\api\complex-types-for-mail-contacts-calendar.md#LocationConstraint) (preview) |  [MeetingTimeCandidate](..\api\complex-types-for-mail-contacts-calendar.md#MeetingTimeCandidate) (preview) |  [PatternedRecurrence](..\api\complex-types-for-mail-contacts-calendar.md#PatternedRecurrence) | 
 [PhysicalAddress](..\api\complex-types-for-mail-contacts-calendar.md#PhysicalAddress) | 
 [Recipient](..\api\complex-types-for-mail-contacts-calendar.md#Recipient) | 
 [RecurrencePattern](..\api\complex-types-for-mail-contacts-calendar.md#RecurrencePattern) | 
 [RecurrenceRange](..\api\complex-types-for-mail-contacts-calendar.md#RecurrenceRange) | 
 [ResponseStatus](..\api\complex-types-for-mail-contacts-calendar.md#ResponseStatus) | 
 [TimeConstraint](..\api\complex-types-for-mail-contacts-calendar.md#TimeConstraint) (preview) |  [TimeSlot](..\api\complex-types-for-mail-contacts-calendar.md#TimeSlot) (preview) |  [TimeStamp](..\api\complex-types-for-mail-contacts-calendar.md#TimeStamp) (preview) 
 
 
 **OData query parameters:** &nbsp;  [$search](..\api\complex-types-for-mail-contacts-calendar.md#Search) | 
 [$filter](..\api\complex-types-for-mail-contacts-calendar.md#Filter) | [$select](..\api\complex-types-for-mail-contacts-calendar.md#Select) | 
 [$orderby](..\api\complex-types-for-mail-contacts-calendar.md#OrderBy) | [$top and $skip](..\api\complex-types-for-mail-contacts-calendar.md#TopSkip) | 
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

[Initial sign in](..\api\discovery-service-rest-operations.md#DiscoveryServiceoperationsInitialsignin) |
 [Discover specific services](..\api\discovery-service-rest-operations.md#DiscoveryServiceoperationsDiscoverspecificservices) | [Learn what services are discoverable](..\api\discovery-service-rest-operations.md#DiscoveryServiceoperationsLearnwhatservicesarediscoverable)


<a name="bk_files"> </a>

##ファイル

[OneDrive API](https://dev.onedrive.com/)


<a name="bk_video"> </a>

##ビデオ 

[ビデオ API](../api/video-rest-operations.md)

**Video portal:** &nbsp; 
  [Get information](..\api\video-rest-operations.md#GetPortalInformation)
  
**Channels:** &nbsp; 
  [Get information](..\api\video-rest-operations.md#GetChannelsInfo) 
  
**Video information:** &nbsp; 
  [Get](..\api\video-rest-operations.md#GetVideoInfo) | [Update video metadata](..\api\video-rest-operations.md#UpdateVideo) 
  
**Videos:** &nbsp; 
  [Upload](..\api\video-rest-operations.md#UploadVideos) | [Delete](..\api\video-rest-operations.md#DeleteVideos) 

##21Vianet が運用している Office 365 の API リソースおよびサービス エンドポイント
[21Vianet が運用している Office 365 の API エンドポイント](..\api\o365-china-endpoints.md)

##Office 365 管理 API
[Getting started](https://msdn.microsoft.com/library/office/dn707383.aspx) | [Service Communications API (preview)](https://msdn.microsoft.com/EN-US/library/dn707386.aspx) | [Management Activity API Reference (preview)](https://msdn.microsoft.com/library/office/mt227394.aspx) | [Reporting web service](https://msdn.microsoft.com/en-us/library/office/jj984325.aspx) 

##その他の技術情報

###API サンドボックス
[Office 365 の API を実際に試してみる場合は、この対話型コンソールを使用してください](http://apisandbox.msdn.microsoft.com/)

###Office 365 プラットフォームの概要

Office 365 プラットフォーム上での開発の概要  Office 365 開発環境のセットアップ 

[Getting Started with Office 365APIs](..\howto\getting-started-Office-365-APIs.md) 

###Office 365 のアクセス許可スコープ
[Office 365 のすべてのアクセス許可スコープの詳細](..\howto\application-manifest.md)

###Microsoft パートナー センターの API リファレンス
パートナーは、CSP Commerce REST API を使用して、Microsoft Commerce Platform での顧客アカウントの作成、顧客プロファイルの管理ができます。また、顧客向け Microsoft 製品の注文とサブスクリプションを購入および管理できます。 



