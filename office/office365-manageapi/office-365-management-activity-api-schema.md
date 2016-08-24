
# Office 365 管理アクティビティ API のスキーマ
 
 _**適用対象:** Office 365_
 

Office 365 管理アクティビティ API のスキーマは、次の 2 つのレイヤーでデータ サービスとして提供されます。


-  **共通スキーマ** - レコードの種類、作成時刻、ユーザーの種類、およびアクションなどの中心的な Office 365 監査概念にアクセスし、コア ディメンション (ユーザー ID など)、場所の詳細情報 (クライアント IP アドレスなど)、および製品固有のプロパティ (オブジェクト ID など) を提供するインターフェイスです。これにより一貫性がある統一ビューが確立され、いくつかの最上位ビューで適切なパラメーターを指定して、すべての Office 365 監査データを抽出できます。さらに、学習のコストを大幅に削減する、すべてのデータ ソース向けの固定スキーマも提供されます。共通スキーマのデータは、Exchange、SharePoint、Azure Active Directory、Yammer、および OneDrive for Business などの各製品チームが所有する製品データから調達されます。製品チームは [オブジェクト ID] フィールドを、製品固有のプロパティを追加するために拡張できます。
    
-  **製品固有スキーマ** - 共通スキーマ上に構築され、製品固有の属性セット (Sway スキーマ、SharePoint スキーマ、OneDrive for Business スキーマ、および Exchange 管理者スキーマなど) を提供します。
    
 **各自のシナリオでどのレイヤーを使用するべきですか?** 一般に、データが上位レイヤーで使用可能であれば、下位レイヤーには戻らないでください。つまり、データ要件が製品固有のスキーマに適合できるのであれば、共通スキーマに戻る必要はありません。 

## Office 365 管理 API のスキーマ

この記事では、共通スキーマと、製品固有の各スキーマについて詳細に説明します。次の表では、使用可能なスキーマについて説明しています。 



|**スキーマ名**|**説明**|
|:-----|:-----|
|共通スキーマ|レコードの種類、ユーザー ID、クライアント IP、ユーザーの種類、およびアクションと、ユーザーのプロパティ (ユーザー ID など)、場所のプロパティ (クライアント IP など)、および製品固有のプロパティ (オブジェクト ID など) といったコア ディメンションを抽出するためのビューです。|
|SharePoint 基本スキーマ|共通スキーマを、すべての SharePoint 監査データに固有のプロパティで拡張します。|
|SharePoint ファイルの操作|SharePoint 基本スキーマを、ファイル・アクセスと SharePoint での操作に固有のプロパティで拡張します。|
|SharePoint 共有スキーマ|SharePoint 基本スキーマを、ファイル共有に固有のプロパティで拡張します。|
|SharePoint スキーマ|SharePoint 基本スキーマを、SharePoint に固有であるものの、ファイル アクセスと操作には関連がないプロパティで拡張します。|
|Exchange 管理者スキーマ|共通スキーマを、すべての Exchange 管理者監査スキーマに固有のプロパティで拡張します。|
|Exchange メールボックス スキーマ|共通スキーマを、すべての Exchange メールボックス監査データに固有のプロパティで拡張します。|
|Azure Active Directory 基本スキーマ|共通スキーマを、すべての Azure Active Directory 監査データに固有のプロパティで拡張します。|
|Azure Active Directory アカウント ログオン スキーマ|Azure Active Directory 基本スキーマを、すべての Azure Active Directory ログオン イベントに固有のプロパティで拡張します。|
|Azure Active Directory スキーマ|共通スキーマを、すべての Azure Active Directory 監査データに固有のプロパティで拡張します。|
|DLP Schema|Extends the Common Schema with properties specific to Data Loss Prevention events.|
|Security & Compliance Center Schema|Extends the Common Schema with properties specific to all Security & Compliance Center events.|
|Sway スキーマ|共通スキーマを、すべての spnv に固有のプロパティで拡張します。|
|データ センター セキュリティ基本スキーマ|Azure Active Directory 基本スキーマを、すべての Azure Active Directory STS ログオン イベントに固有のプロパティで拡張します。|
|データ センター セキュリティ基本スキーマ|共通スキーマを、すべてのデータ センター セキュリティ監査データに固有のプロパティで拡張します。|
|データ センター セキュリティ コマンドレット スキーマ|データ センター セキュリティ基本スキーマを、すべてのデータ センター セキュリティ コマンドレット監査データに固有のプロパティで拡張します。|

## 共通スキーマ

EntityType の名前:AuditRecord



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Id|Combination GUIDEdm.Guid|はい|監査レコードの一意識別子。|
|RecordType|Self.AuditLogRecordType|はい|レコードによって示される操作の種類。監査ログ レコードの種類の詳細については、**AuditLogRecordType** の表を参照してください。|
|CreationTime|Edm.Date|はい|ユーザーがアクティビティを実行した、世界協定時刻 (UTC) での日時。|
|Operation|Edm.String|はい|The name of the user or admin activity. ユーザーまたは管理者アクティビティの名前。一般的な操作/アクティビティの説明については、「[Office 365 プロテクション センターでの監査ログの検索](http://go.microsoft.com/fwlink/p/?LinkId=708432)」を参照してください。Exchange 管理者アクティビティでは、このプロパティは、実行されたコマンドレットの名前を識別します。 For Exchange admin activity, this property identifies the name of the cmdlet that was run. For Dlp events, this can be "DlpRuleMatch", "DlpRuleUndo" or "DlpInfo".|
|OrganizationId|Edm.Guid|はい|イベントが発生した、組織の Office 365 サービスの GUID。たとえば、SharePoint のイベントは、Exchange のイベントとは OrganizationId が異なります。イベントが発生したサービスは、EventSource プロパティにより識別されます。|
|UserType|Self.User Type|はい|操作を実行したユーザーの種類。ユーザーの種類の詳細については、「**ユーザーの種類**」の表を参照してください。|
|UserKey|Edm.String|はい|UserId プロパティで識別されるユーザーの別の ID。たとえば、このプロパティには、SharePoint、OneDrive for Business、および Exchange のユーザーにより実行されたイベントの Passport 固有 ID (PUID) が格納されます。このプロパティは、他のサービスで発生するイベントや、システム アカウントで実行されるイベントの UseID プロパティと同じ値を指定することもできます。|
|Workload|Edm.String|いいえ|Workload 文字列でアクティビティが発生した Office 365 サービス。このプロパティに指定可能な値は次のとおりです。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Exchange</p></li><li><p>SharePoint</p></li><li><p>OneDrive for Business</p></li><li><p>Azure Active Directory</p></li><li><p>SecurityComplianceCenter</p></li><li><p>Sway</p></li></ul>|
|ResultStatus|Edm.String|いいえ|(Operation プロパティで指定された) アクションが正常に終了したかどうかどうかを示します。指定可能な値は、**Succeeded**、**PartiallySucceded**、または **Failed** です。Exchange 管理者アクティビティでは、値は **True** または **False** のいずれかです。|
|ObjectId|Edm.string|いいえ|SharePoint および OneDrive for Business アクティビティの場合、ユーザーがアクセスするファイルまたはフォルダーの完全なパス名。Exchange 管理者監査ログの場合、コマンドレットによって変更されたオブジェクトの名前。|
|UserId|Edm.string|はい|レコードがログに記録されることになった、(Operation プロパティで指定された) アクションを実行したユーザーの UPN (ユーザー プリンシパル名)。たとえば、my_name@my_domain_name などです。システム アカウント (SHAREPOINT\system または NT AUTHORITY\SYSTEM など) で実行されるアクティビティのレコードも含まれることに注意してください。|
|ClientIp|Edm.String|いいえ|アクティビティがログに記録されたときに使用されたデバイスの IP アドレス。IP アドレスは、IPv4 または IPv6 アドレスの形式で表示されます。|

### 列挙:AuditLogRecordType - タイプ:Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|1|ExchangeAdmin|Exchange 管理者監査ログからのイベント。|
|2|ExchangeItem|単一のアイテムに対して実行されるアクション (単一の電子メール メッセージの作成や受信など) の、Exchange メールボックス監査ログからのイベント。|
|3|ExchangeItemGroup|複数のアイテムに対して実行できるアクション (1 つ以上の電子メール メッセージの移動や削除など) の、Exchange メールボックス監査ログからのイベント。|
|4|SharePoint|SharePoint イベント。|
|6|SharePointFileOperation|SharePoint ファイル操作イベント。|
|8|AzureActiveDirectory|Azure Active Directory イベント。|
|9|AzureActiveDirectoryAccountLogon|Azure Active Directory OrgId ログオン イベント (非推奨)。|
|10|DataCenterSecurityCmdlet|データ センター セキュリティ コマンドレット イベント。|
|11|ComplianceDLPSharePoint|Data loss protection (DLP) events in SharePoint and OneDrive for Business.|
|12|Sway|Sway サービスおよびクライアントからのイベント。|
|13|ComplianceDLPExchange|SharePoint のデータ損失防止 (DLP) イベント。|
|14|SharePointSharingOperation|SharePoint 共有イベント。|
|15|AzureActiveDirectoryStsLogon|Azure Active Directory の Secure Token Service (STS) ログオン イベント。|
|18|SecurityComplianceCenterEOPCmdlet|Admin actions from the Security & Compliance Center.|


### 列挙:User Type - タイプ:Edm.Int32


|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|Regular|正規ユーザー。|
|1|Reserved|予約済みユーザー。|
|2|Admin|管理者。|
|3|DcAdmin|Microsoft データセンターのオペレーター。|
|4|System|システム アカウント。|
|5|Application|アプリケーション。|
|6|ServicePrincipal|サービス プリンシパル。|

 **注** ユーザーの種類が含まれるのは Exchange の操作のみです。SharePoint の操作では、ユーザーの種類は指定しません。 注 ユーザーの種類が含まれるのは Exchange の操作のみです。SharePoint の操作では、ユーザーの種類は指定しません。 


## SharePoint 基本スキーマ




|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Site|Edm.Guid|いいえ|ユーザーによりアクセスされるファイルまたはフォルダーが置かれているサイトの GUID。|
|ItemType|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.ItemType"|いいえ|アクセスまたは変更されたオブジェクトの種類。オブジェクトの種類の詳細については、「**ItemType**」の表を参照してください。|
|EventSource|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.EventSource"|いいえ|SharePoint でイベントが発生したことを示します。指定可能な値は、**SharePoint** または **ObjectModel** です。|
|SourceName|Edm.String|いいえ|監査対象の操作をトリガーしたエンティティ。指定可能な値は、SharePoint または **ObjectModel** です。|
|UserAgent|Edm.String|いいえ|ユーザーのクライアントまたはブラウザーに関する情報。この情報は、クライアントまたはブラウザーによって提供されます。|
|MachineDomainInfo|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|いいえ|デバイスの同期操作に関する情報。この情報は、要求で指定されている場合にのみ報告されます。|
|MachineId|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|いいえ|デバイスの同期操作に関する情報。この情報は、要求で指定されている場合にのみ報告されます。|


### 列挙:ItemType - タイプ:Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|Invalid|アイテムは、その他の種類のアイテム (この表にリストされている) のいずれでもありません。|
|1|File|アイテムはファイルです。|
|5|Folder|アイテムはフォルダーです。|
|6|Web|アイテムは Web ページです。|
|7|Site|アイテムはサイトです。|
|8|Tenant|アイテムはテナントです。|
|9|DocumentLibrary|アイテムはドキュメント ライブラリです。|

### 列挙:EventSource - タイプ:Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|SharePoint|イベント ソースは SharePoint です。|
|1|ObjectModel|イベント ソースは ObjectModel です。|


### 列挙:SharePointAuditOperation - タイプ:Edm.Int32



|**メンバー名**|**説明**|
|:-----|:-----|
|AccessInvitationAccepted*|共有ファイル (またはフォルダー) の表示または編集への招待の受信者が、招待のリンクをクリックして共有ファイルにアクセスしました。|
|AccessInvitationCreated*|ユーザーは、SharePoint または OneDrive for Business サイトにある共有ファイルまたはフォルダーの表示または編集への招待を、(組織の内外の) 他のユーザーに送信します。イベント エントリの詳細は、共有されたファイルの名前、招待状の送付先ユーザー、および招待を送信したユーザーが選択した共有アクセス許可の種類を示します。|
|AccessInvitationExpired*|外部ユーザーに送信する招待に有効期限を設定します。既定では、組織外のユーザーに送信する招待は、招待が承諾されない場合には 7 日後に有効期限が切れます。|
|AccessInvitationRevoked*|SharePoint または OneDrive for Business のサイト管理者、サイト所有者、またはドキュメント所有者は、組織外のユーザーに送信された招待を取り下げます。招待は、承諾前にのみ取り下げることができます。|
|AccessInvitationUpdated*|SharePoint または OneDrive for Business サイトにある共有ファイルまたはフォルダーの表示または編集への招待を作成して送信したユーザーが、招待を再送します。|
|AccessRequestApproved|SharePoint または OneDrive for Business のサイト管理者、サイト所有者、またはドキュメント所有者は、サイトまたはドキュメントにアクセスするユーザー要求を承認します。|
|AccessRequestCreated*|ユーザーは、アクセス許可がない SharePoint または OneDrive for Business のサイトまたはドキュメントへのアクセス権限を要求します。 |
|AccessRequestRejected*|SharePoint のサイト管理者、サイト所有者、またはドキュメント所有者は、サイトまたはドキュメントへのアクセスを求めるユーザー要求を拒否します。|
|ActivationEnabled*|ユーザーは、フォーム コードが含まれていないフォーム テンプレートをブラウザー対応にする、完全な信頼を要求する、モバイル デバイスでのレンダリングを有効にする、サーバー管理者が管理するデータ接続を使用する、などができます。|
|AdministratorAddedToTermStore*|用語ストア管理者が追加されました。|
| AdministratorDeletedFromTermStore*|用語ストア管理者が削除されました。|
|AllowGroupCreationSet*|サイト管理者またはサイト所有者は、許可が割り当てられたユーザーが SharePoint または OneDrive for Business サイトのグループを作成できる許可レベルを、それらのサイトに追加します。|
| AppCatalogCreated*|SharePoint 環境でカスタム ビジネス アプリを利用できるように、アプリ カタログが作成されました。|
|AuditPolicyRemoved*|サイト コレクションのドキュメント ライフサイクル ポリシーが削除されました。|
|AuditPolicyUpdate*|サイト コレクションのドキュメント ライフサイクル ポリシーが更新されました。|
| AzureStreamingEnabledSet*|ビデオ ポータルの所有者が、Azure からのビデオ ストリーミングを許可されました。|
|CollaborationTypeModified*|サイト (イントラネット、エクストラネット、またはパブリックなど) で許可されているコラボレーションの種類が変更されました。|
|CreateSSOApplication*|ターゲット アプリケーションが Secure Store Service で作成されました。|
|CustomizeExemptUsers*|グローバル管理者が、SharePoint 管理センターで適用除外ユーザー エージェントのリストをカスタマイズしました。インデックスを作成する Web ページ全体を受け取らせないように除外するユーザー エージェントを指定できます。つまり、除外対象として指定したユーザー エージェントが InfoPath フォームを検出すると、フォームは、Web ページ全体ではなく XML ファイルとして返されます。これにより InfoPath フォームのインデックス作成の速度が上がります。|
|DefaultLanguageChangedInTermStore*|用語ストアで変更された言語設定。|
|DeleteSSOApplication*|SSO アプリケーションが削除されました。|
|eDiscoveryHoldApplied*|インプレース ホールドが、コンテンツ ソースに置かれました。インプレース ホールドは、SharePoint で (電子情報開示センターなどの) 電子情報開示サイト コレクションを使用して管理されます。|
|eDiscoveryHoldRemoved*|インプレース ホールドが、コンテンツ ソースから削除されました。インプレース ホールドは、SharePoint で (電子情報開示センターなどの) 電子情報開示サイト コレクションを使用して管理されます。|
|eDiscoverySearchPerformed*|電子情報開示検索は、SharePoint の電子情報開示サイト コレクションを使用して実行されました。|
|ExemptUserAgentSet*|グローバル管理者が、SharePoint 管理センターで適用除外ユーザー エージェントのリストにユーザー エージェントを追加しました。|
|FileAccessed|ユーザーまたはシステム アカウントは、SharePoint または OneDrive for Business サイトにあるファイルにアクセスします。システム アカウントが FileAccessed イベントを生成する場合もあります。|
|FileCheckOutDiscarded*|ユーザーは、チェックアウトしたファイルを破棄します (または元に戻します)。つまり、チェックアウト時にファイルに加えた変更はすべて破棄され、ドキュメント ライブラリ内のドキュメントのバージョンには保存されないということです。|
|FileCheckedIn*|ユーザーは、SharePoint または OneDrive for Business ドキュメント ライブラリからチェックアウトしたドキュメントをチェックインします。|
|FileCheckedOut*|ユーザーは、SharePoint または OneDrive for Business ドキュメント ライブラリにあるドキュメントをチェックアウトします。共有しているドキュメントは、複数のユーザーがチェックアウトし、変更を加えることができます。|
|FileCopied|ユーザーは、SharePoint または OneDrive for Business サイトからドキュメントをコピーします。コピーしたファイルは、サイト上の別のフォルダーに保存できます。|
|FileDeleted|ユーザーは、SharePoint または OneDrive for Business サイトからドキュメントを削除します。|
|FileDownloaded|ユーザーは、SharePoint または OneDrive for Business サイトからドキュメントをダウンロードします。|
|FileFetched|このイベントは、FileAccessed イベントに置き換えられており、推奨されていません。|
|FileModified|ユーザーまたはシステム アカウントは、SharePoint あるいは OneDrive for Business サイトにあるドキュメントの内容またはプロパティを変更します。|
|FileMoved|ユーザーは、SharePoint または OneDrive for Business サイトの現在の場所から新しい場所にドキュメントを移動します。|
|FileRenamed|ユーザーは、SharePoint または OneDrive for Business サイトにあるドキュメントの名前を変更します。|
|FileRestored|ユーザーは、SharePoint または OneDrive for Business サイトのごみ箱からドキュメントを復元します。 |
|FileSyncDownloadedFull|ユーザーは、同期関係を確立し、SharePoint または OneDrive for Business ドキュメント ライブラリから自分のコンピューターに初めてファイルを正常にダウンロードします。|
|FileSyncDownloadedPartial|ユーザーは、SharePoint または OneDrive for Business ドキュメント ライブラリから、ファイルに加えたすべての変更内容を正常にダウンロードします。このイベントは、ドキュメント ライブラリ内のファイルに加えたすべての変更内容がユーザーのコンピューターにダウンロードされたことを示します。ドキュメント ライブラリは (FileSyncDownloadedFull イベントが示すとおり) ユーザーによって以前ダウンロードされているため、変更内容のみがダウンロードされました。|
|FileSyncUploadedFull*|ユーザーは、同期関係を確立し、自分のコンピューターから SharePoint または OneDrive for Business ドキュメント ライブラリに初めてファイルを正常にアップロードします。|
|FileSyncUploadedPartial*|ユーザーは、SharePoint または OneDrive for Business ドキュメント ライブラリにあるファイルに、変更内容を正常にアップロードします。このイベントは、ドキュメント ライブラリからダウンロードしたローカル バージョンのファイルに加えた変更内容が、ドキュメント ライブラリに正常にアップロードされたことを示します。ファイルは (FileSyncUploadedFull イベントが示すとおり) ユーザーによって以前にアップロードされているため、変更内容のみがアップロードされます。|
|FileUploaded|ユーザーは、SharePoint または OneDrive for Business サイトにあるフォルダーにドキュメントをアップロードします。 |
|FileViewed|このイベントは、FileAccessed イベントに置き換えられており、推奨されていません。|
|GroupAdded*|サイト管理者または所有者は、SharePoint または OneDrive for Business のグループを作成するか、グループが作成されることになるタスクを実行します。たとえば、ユーザーがファイルを共有するためのリンクを初めて作成すると、システム グループがユーザーの OneDrive for Business に追加されます。ユーザーが共有ファイルの編集許可があるリンクを作成したときにも、このイベントになる場合があります。|
|GroupRemoved*|ユーザーは、SharePoint または OneDrive for Business サイトからグループを削除します。 |
|GroupUpdated*|サイト管理者または所有者は、SharePoint または OneDrive for Business サイトのグループの設定を変更します。これには、グループの名前、グループ メンバーシップを表示または編集できるユーザー、およびメンバーシップ要求を処理する方法の変更を含めることができます。|
|LanguageAddedToTermStore*|用語ストアに言語が追加されました。|
|LanguageRemovedFromTermStore*|用語ストアから言語が削除されました。|
|LegacyWorkflowEnabledSet*|サイト管理者または所有者は、SharePoint ワークフロー タスク コンテンツ タイプをサイトに追加します。グローバル管理者は、SharePoint 管理センターで組織全体のワークフローを有効にすることもできます。|
|ManagedSyncClientAllowed|ユーザーは、SharePoint または OneDrive for Business サイトとの同期関係を正常に確立します。ユーザーのコンピューターが、組織内のドキュメント ライブラリにアクセスできるドメインのリスト (宛先セーフ リスト) に追加されているドメインのメンバーであるため、同期関係は成功しました。この機能の詳細については、「[Windows PowerShell コマンドレットを使用して宛先セーフ リスト上のドメインに対して OneDrive 同期を有効にする](http://go.microsoft.com/fwlink/p/?LinkID=534609)」を参照してください。|
|MaxQuotaModified*|サイトの最大クォータは変更されています。|
|MaxResourceUsageModified*|サイトの最大許容リソース配分状況は変更されています。|
|MySitePublicEnabledSet*|SharePoint 管理者によって、ユーザーが公開 MySite を持てるようにするフラグが設定されています。|
|NewsFeedEnabledSet*|サイト管理者または所有者は、SharePoint または OneDrive for Business サイトの RSS フィードを有効にします。グローバル管理者は、SharePoint 管理センターで組織全体の RSS フィードを有効にできます。|
|ODBNextUXSettings*|OneDrive for Business 用の新しい UI が使用可能になっています。|
|OfficeOnDemandSet*|サイト管理者は、Office オンデマンドを有効にします。これによりユーザーは、Office デスクトップ アプリケーションの最新バージョンにアクセスできます。Office オンデマンドは Office SharePoint 管理センターで有効にされ、インストール済みのすべての Office アプリケーションを含む Office 365 サブスクリプションを必要とします。|
|PeopleResultsScopeSet*|サイト管理者は、SharePoint サイトの人の検索の結果ソースを作成または変更します。|
|PreviewModeEnabledSet*|サイト管理者は、SharePoint サイトのドキュメント プレビューを有効にします。|
|QuotaWarningEnabledModified*|ストレージ クォータ警告が変更されました。|
|RenderingEnabled*|ブラウザー対応フォーム テンプレートが、InfoPath フォーム サービスでレンダリングされます。|
|ResourceWarningEnabledModified*|リソース クォータ警告が変更されました。|
|SSOGroupCredentialsSet*|Secure Store Service でグループ資格情報が設定されました。|
|SSOUserCredentialsSet*|Secure Store Service でユーザー資格情報が設定されました。|
|SearchCenterUrlSet*|検索センターの URL が設定されました。|
|SecondaryMySiteOwnerSet*|ユーザーがその MySite に代理所有者を追加しました。|
|SendToConnectionAdded*|グローバル管理者は、SharePoint 管理センターの [レコード管理] ページで、新しい [送信先] 接続を作成します。[送信先] 接続は、ドキュメント リポジトリまたはレコード センターの設定を指定します。[送信先] 接続を作成すると、コンテンツ オーガナイザーにより、指定された場所にドキュメントを送信できます。|
|SendToConnectionRemoved*|グローバル管理者は、SharePoint 管理センターの [レコード管理] ページで、[送信先] 接続を削除します。|
|SharedLinkCreated|ユーザーは、SharePoint または OneDrive for Business で共有ファイルへのリンクを作成します。このリンクは、共有ファイルにアクセスできるように他のユーザーに送信できます。ユーザーは、共有ファイルの表示と編集を許可するリンク、またはファイルの表示だけを許可するリンクの、2 種類のリンクを作成できます。|
|SharedLinkDisabled*|ユーザーは、ファイルを共有するために作成されたリンクを (完全に) 無効にします。|
|SharingInvitationAccepted*|User accepts an invitation to share a file or folder. This event is logged when a user shares a file with other users.|
|SharingRevoked*|ユーザーは、他のユーザーと以前に共有していたファイルまたはフォルダーを共有解除します。このイベントは、ユーザーが他のユーザーとのファイルの共有を停止すると記録されます。|
|SharingSet|ユーザーは、SharePoint または OneDrive for Business にあるファイルまたはフォルダーを、組織内の他のユーザーと共有します。|
|SiteAdminChangeRequest*|ユーザーが、SharePoint サイト コレクションのサイト コレクション管理者として追加されるように要求します。サイト コレクション管理者は、サイト コレクションとすべてのサブサイトのフル コントロール権限を持ちます。|
|SiteCollectionAdminAdded*|サイト コレクション管理者または所有者は、ユーザーを SharePoint または OneDrive for Business サイトのサイト コレクション管理者として追加します。サイト コレクション管理者は、サイト コレクションとすべてのサブサイトのフル コントロール権限を持ちます。|
|SiteCollectionCreated*| グローバル管理者は、SharePoint 組織に新しいサイト コレクションを作成します。|
|SitePermissionsModified*|サイト管理者または所有者 (あるいはシステム アカウント) は、SharePoint または OneDrive for Business サイトのグループに割り当てられているアクセス許可レベルを変更します。このイベントは、グループからすべてのアクセス許可が削除された場合にもログに記録されます。|
|SiteRenamed*|サイト管理者または所有者は、SharePoint または OneDrive for Business サイトの名前を変更します。|
|SyncGetChanges*|ユーザーは、ドキュメント ライブラリ内のファイルに加えたすべての変更内容を自分のコンピューターに同期するために、SharePoint または OneDrive for Business のアクション トレイにある **[同期]** をクリックします。|
|UnmanagedSyncClientBlocked|ユーザーは、組織のドメインのメンバーではないコンピューター、または組織のドキュメント ライブラリにアクセスできるものの、ドメインのリスト (宛先セーフ リスト) に追加されていないドメインのメンバーであるコンピューターから、SharePoint または OneDrive for Business サイトとの同期関係を確立しようとします。同期関係は許可されておらず、ユーザーのコンピューターは、ドキュメント ライブラリにあるファイルの同期、ダウンロード、またはアップロードができないようにブロックされています。この機能の詳細については、「[Windows PowerShell コマンドレットを使用して宛先セーフ リスト上のドメインに対して OneDrive 同期を有効にする](http://go.microsoft.com/fwlink/p/?LinkID=534609)」を参照してください。|
|UpdateSSOApplication*|ターゲット アプリケーションが Secure Store Service で更新されました。|
|UserAddedToGroup*|サイト管理者または所有者は、SharePoint または OneDrive for Business サイトにあるグループにユーザーを追加します。ユーザーをグループに追加すると、そのユーザーにはグループに割り当てられたアクセス許可が付与されます。 |
|UserRemovedFromGroup*|サイト管理者または所有者は、SharePoint または OneDrive for Business サイトにあるグループからユーザーを削除します。削除されると、そのユーザーにはグループに割り当てられたアクセス許可は付与されなくなります。 |

 **注** * この操作はプレビュー段階です。



## SharePoint ファイルの操作

「[Office 365 プロテクション センターでの監査ログの検索](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US)」の「ファイルとフォルダーのアクティビティ」セクションにリストされているファイル関連 SharePoint イベントは、このスキーマを使用します。



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|SiteUrl|Edm.String|はい|ユーザーによりアクセスされるファイルまたはフォルダーが置かれているサイトの URL。|
|SourceRelativeUrl|Edm.String|いいえ|ユーザーがアクセスするファイルが含まれているフォルダーの URL。_SiteURL_、_SourceRelativeURL_、および _SourceFileName_ パラメーターの値の組み合わせは、**ObjectID** プロパティの値と同じであり、ユーザーがアクセスするファイルの完全パス名です。|
|SourceFileName|Edm.String|はい|ユーザーによりアクセスされるファイルまたはフォルダーの名前。|
|SourceFileExtension|Edm.String|いいえ|ユーザーがアクセスしたファイルのファイル拡張子。このプロパティは、アクセスされたオブジェクトがフォルダーの場合には、空白になります。|
|DestinationRelativeUrl|Edm.String|いいえ|ファイルのコピー先または移動先フォルダーの URL。_SiteURL_、_SourceRelativeURL_、および _SourceFileName_ パラメーターの値の組み合わせは、**ObjectID** プロパティの値と同じであり、コピーされたファイルの完全パス名です。このプロパティは、FileCopied および FileMoved イベントに対してのみ表示されます。|
|DestinationFileName|Edm.String|いいえ|コピーまたは移動したファイルの名前。このプロパティは、FileCopied および FileMoved イベントに対してのみ表示されます。|
|DestinationFileExtension|Edm.String|いいえ|コピーまたは移動したファイルのファイル拡張子。このプロパティは、FileCopied および FileMoved イベントに対してのみ表示されます。|
|UserSharedWith|Edm.String|いいえ|リソースを共有したユーザー。|
|SharingType|Edm.String|いいえ|リソースを共有したユーザーに割り当てられているアクセス許可の種類。このユーザーは、_UserSharedWith_ パラメーターにより識別されます。|



## SharePoint 共有スキーマ

 The file share-related SharePoint events. ファイル共有関連の SharePoint イベント。これは、ユーザーが別のユーザーに何らかの影響があるアクションを取るという点で、ファイル関連イベントやフォルダー関連イベントとは異なります。SharePoint 共有スキーマについては、「Office 365 監査ログで共有監査を使う」を参照してください。 For information about the SharePoint Sharing Schema, see [Use sharing auditing in the Office 365 audit log](https://support.office.com/en-us/article/Use-sharing-auditing-in-the-Office-365-audit-log-50bbf89f-7870-4c2a-ae14-42635e0cfc01?ui=en-US&amp;rs=en-US&amp;ad=US).



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|||||
|||||
|TargetUserOrGroupName |Edm.String|いいえ|リソースを共有していたターゲット ユーザーまたはグループの UPN または名前を格納します。|
|TargetUserOrGroupType|Edm.String|いいえ|ターゲット ユーザーまたはグループが、メンバー、ゲスト、グループ、またはパートナーであるかどうかを示します。 |
|EventData|XML コード|いいえ|グループへのユーザーの追加や編集アクセス許可の付与などの、発生した共有アクションに関する追加情報を伝達します。|


## SharePoint スキーマ

「[Office 365 プロテクション センターでの監査ログの検索](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US)」にリストされている SharePoint イベント (ファイルおよびフォルダー イベントを除く) は、このスキーマを使用します。



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|CustomEvent|Edm.String|いいえ|カスタム イベントのオプション文字列。|
|EventData|Edm.String|いいえ|カスタム イベントのオプション ペイロード。|
|ModifiedProperties|Collection(ModifiedProperty)|いいえ|このプロパティは、サイトまたはサイト コレクション管理者グループのメンバーとしてユーザーを追加するなどの、管理イベントの場合に含まれます。プロパティには、変更されたプロパティの名前 (サイト管理者グループなど)、変更されたプロパティの新しい値 (サイト管理者として追加されたユーザーなど)、および変更されたオブジェクトの以前の値が含まれます。|


## Exchange 管理者スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|ModifiedObjectResolvedName|Edm.String|いいえ|これはコマンドレットによって変更されたオブジェクトのユーザー フレンドリ名です。これはコマンドレットによるオブジェクトの変更の場合にのみ記録されます。|
|Parameters|Collection(Common.NameValuePair)|いいえ|Operations プロパティで示されているコマンドレットで使用された、すべてのパラメーターの名前と値。|
|ModifiedProperties|Collection(Common.ModifiedProperty)|いいえ|このプロパティは、管理イベント用に組み込まれています。プロパティには、変更されたプロパティの名前、変更されたプロパティの新しい値、および変更されたオブジェクトの以前の値が含まれます。|
|ExternalAccess|Edm.Boolean|はい|組織内のユーザー、Microsoft データセンター担当者またはデータセンター サービス アカウント、あるいは代理管理者により、コマンドレットが実行されたかどうかを示します。値 **False** は、コマンドレットが組織内のユーザーによって実行されたことを示します。値 **True** は、コマンドレットがデータセンターの担当者、データセンターのサービス アカウント、または代理管理者によって実行されたことを示します。|
|OriginatingServer|Edm.String|いいえ|コマンドレット実行元のサーバーの名前。|
|OrganizationName|Edm.String|いいえ|テナントの名前。|


## Exchange メールボックス スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|LogonType|Self.LogonType|いいえ| メールボックスにアクセスして、ログに記録された操作を実行したユーザーの種類を示します。|
|InternalLogonType|Self.LogonType|いいえ|内部使用のため予約済みです。|
|MailboxGuid|Edm.String|いいえ|アクセスされたメールボックスの Exchange GUID。|
|MailboxOwnerUPN|Edm.String|いいえ|アクセスされたメールボックスの所有者の電子メール アドレス。|
|MailboxOwnerSid|Edm.String|いいえ|メールボックス所有者の SID。|
|MailboxOwnerMasterAccountSid|Edm.String|いいえ|メールボックス所有者のアカウントのマスター アカウント SID。|
|LogonUserSid|Edm.String|いいえ|操作を実行したユーザーの SID。|
|LogonUserDisplayName|Edm.String|いいえ|操作を実行したユーザーのユーザー フレンドリ名。|
|ExternalAccess|Edm.Boolean|はい|これは、ログオン ユーザーのドメインがメールボックス所有者のドメインと異なる場合は、true です。|
|OriginatingServer |Edm.String|いいえ|これは、操作の発生場所です。|
|OrganizationName|Edm.String|いいえ|テナントの名前。|
|ClientInfoString|Edm.String|いいえ|操作を実行するために使用された電子メール クライアントについての情報 (ブラウザーのバージョン、Outlook のバージョン、およびモバイル デバイスの情報など)。|
|ClientIPAddress|Edm.String|いいえ|操作がログに記録されたときに使用されたデバイスの IP アドレス。IP アドレスは、IPv4 または IPv6 アドレスの形式で表示されます。|
|ClientMachineName|Edm.String|いいえ|Outlook クライアントをホストしているマシン名。|
|ClientProcessName|Edm.String|いいえ|メールボックスへのアクセスに使用された電子メール クライアント。 |
|ClientVersion|Edm.String|いいえ|電子メール クライアントのバージョン。|


### 列挙:LogonType - タイプ:Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|Owner|メールボックス所有者。|
|1|Admin|他のユーザーのメールボックスに対する管理者特権を持つユーザー。|
|2|Delegated|他のユーザーのメールボックスに対する代理人特権を持つユーザー。|
|3|Transport|Microsoft データセンターのトランスポート サービス。|
|4|SystemService|Microsoft データセンターのサービス アカウント。|
|5|BestAccess|内部使用のため予約済みです。|
|6|DelegatedAdmin|代理管理者。|


### ExchangeMailboxAuditGroupRecord スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Folder|Self.ExchangeFolder|いいえ|アイテムのグループが存在するフォルダー。|
|CrossMailboxOperations|Edm.Boolean|いいえ|操作に複数のメールボックスが関係したかどうかを示します。|
|DestMailboxId|Edm.Guid|いいえ|CrossMailboxOperations パラメーターが **True** の場合にのみ設定します。ターゲット メールボックスの GUID を指定します。|
|DestMailboxOwnerUPN|Edm.String|いいえ|CrossMailboxOperations パラメーターが **True** の場合にのみ設定します。ターゲット メールボックス所有者の UPN を指定します。|
|DestMailboxOwnerSid|Edm.String|いいえ|CrossMailboxOperations パラメーターが **True** の場合にのみ設定します。ターゲット メールボックスの SID を指定します。|
|DestMailboxOwnerMasterAccountSid|Edm.String|いいえ|CrossMailboxOperations パラメーターが **True** の場合にのみ設定します。ターゲット メールボックス所有者のマスター アカウント SID の SID を指定します。|
|DestFolder|Self.ExchangeFolder|いいえ|移動などの操作の場合の、移動先フォルダー。|
|Folders|Collection(Self.ExchangeFolder)|いいえ|操作に関連したソース フォルダーに関する情報 (フォルダーが選択されて削除された場合など)。|
|AffectedItems|Collection(Self.ExchangeItem)|いいえ|グループ内の各項目についての情報。|



### ExchangeMailboxAuditRecord スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Item|Self.ExchangeItem|いいえ|操作の実行対象のアイテムを表します。|
|ModifiedProperties|Collection(Edm.String)|いいえ|TBD|
|SendAsUserSmtp|Edm.String|いいえ|偽装しているユーザーの SMTP アドレス。|
|SendAsUserMailboxGuid|Edm.Guid|いいえ|電子メールを偽装ユーザーとして送信するためにアクセスされたメールボックスの Exchange GUID。|
|SendOnBehalfOfUserSmtp|Edm.String|いいえ|電子メールが代理で送信されたユーザーの SMTP アドレス。|
|SendonBehalfOfUserMailboxGuid|Edm.Guid|いいえ|電子メールを代理で送信するためにアクセスされたメールボックスの Exchange GUID。|


### ExchangeItem 複合型



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|はい|ストア ID。|
|Subject|Edm.String|いいえ|アクセスされたメッセージの件名。|
|ParentFolder|Edm.ExchangeFolder|いいえ|アイテムが置かれているフォルダーの名前。|
|Attachments|Edm.String|いいえ|メッセージに添付されているすべてのアイテムの名前とファイル サイズのリスト。|

### ExchangeFolder 複合型



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|はい|フォルダー オブジェクトのストアの ID。|
|Path|Edm.String|いいえ|アクセスされたメッセージが置かれているメールボックス フォルダーの名前。|



## Azure Active Directory 基本スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|AzureActiveDirectoryEventType|Self.AzureActiveDirectoryEventType|はい|Azure AD イベントの種類。 |
|ExtendedProperties|Collection(Common.NameValuePair)|いいえ|Azure AD イベントの拡張プロパティ。|
|ModifiedProperties|Collection(Common.ModifiedProperty)|いいえ|このプロパティは、管理イベント用に組み込まれています。プロパティには、変更されたプロパティの名前、変更されたプロパティの新しい値、および変更されたプロパティの以前の値が含まれています。|

### 列挙:AzureActiveDirectoryEventType - タイプ: -Edm.Int32



|**メンバー名**|**説明**|
|:-----|:-----|
|AccountLogon|アカウント ログイン イベント。|
|AzureApplicationAuditEvent|Azure アプリケーション セキュリティ イベント。|

## Azure Active Directory アカウント ログオン スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Application|Edm.String|いいえ|Office 15 などの、アカウント ログイン イベントをトリガーするアプリケーション。|
|Client|Edm.String|いいえ|クライアント デバイス、デバイスの OS、およびアカウント ログイン イベントに使用したデバイスのブラウザーに関する詳細。|
|LoginStatus|Edm.Int32|はい|このプロパティは、OrgIdLogon.LoginStatus に直接由来しています。関心の対象となるさまざまなログオン失敗のマッピングは、警告アルゴリズムによって行うことができます。|
|UserDomain|Edm.String|はい|テナント ID 情報 (TII)。|



### 列挙:CredentialType - タイプ:Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|-1|Other|その他の認証。|
|0|Password|ユーザー資格情報は、ユーザー名とパスワードです。|
|1|MobilePhone|ユーザー資格情報は、携帯電話です。|
|2|SecretQuestion|ユーザー資格情報は、秘密の質問です。|
|3|SecurePin|ユーザー資格情報は、セキュア PIN です。|
|4|SecurePinReset|ユーザー資格情報は、セキュア PIN リセットです。|
|11|EasyID|ユーザー資格情報は、EasyID です。|
|14|PasswordIndexCredentialType|ユーザー資格情報は、PasswordIndexCredentialType です。|
|16|Device|ユーザー資格情報は、デバイスです。|
|17|ForeignRealmIndex|ユーザーの資格情報は、ForeignRealmIndex です。|

### 列挙:LoginType - タイプ:Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|-1|Other|その他の i タイプ。|
|1|InitialAuth|最初の認証でログイン。|
|2|CookieCopy|Cookie を使用してログイン。|
|3|SilentReAuth|自動再認証でログイン。|


### 列挙:AuthenticationMethod - タイプ:Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|Min|認証方法は、Min です。|
|1|Password|認証方法は、パスワードです。|
|2|Digest|認証方法は、ダイジェストです。|
|3|ProxyAuth|認証方法は、ProxyAuth です。|
|4|InfoCard|認証方法は、InfoCard です。|
|5|DAToken|認証方法は、DAToken です。|
|6|Sha1RememberMyPassword|認証方法は、Sha1RememberMyPassword です。|
|7|LMPasswordHash|認証方法は、LMPasswordHash です。|
|8|ADFSFederatedToken|認証方法は、ADFSFederatedToken です。|
|9|EID|認証方法は、EID です。|
|10|DeviceID|認証方法は、DeviceID です。 |
|11|MD5|認証方法は、MD5 です。|
|12|EncProxyPasswordHash|認証方法は、EncProxyPasswordHash です。|
|13|LWAFederation|認証方法は、LWAFederation です。|
|14|Sha1HashedPassword|認証方法は、Sha1HashedPassword です。|
|15|SecurePin|認証方法は、セキュア PIN です。|
|16|SecurePinReset|認証方法は、セキュア PIN リセットです。|
|17|SAML20PostSimpleSign|認証方法は、SAML20PostSimpleSign です。|
|18|SAML20Post|認証方法は、SAML20Post です。|
|19|OneTimeCode|認証方法は、ワンタイム・コードです。|



## Azure Active Directory スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Actor|Collection(Self.IdentityTypeValuePair)|いいえ|アクションを実行したユーザーまたはサービス プリンシパル。|
|ActorContextId|Edm.String|いいえ|アクターが属する組織の GUID。|
|ActorIpAddress|Edm.String|いいえ|IPV4 または IPV6 アドレス形式の、アクターの IP アドレス。|
|InterSystemsId|Edm.String|いいえ|Office 365 サービス内の複数のコンポーネント間でアクションを追跡する GUID。|
|IntraSystemsId|Edm.String|いいえ|アクションを追跡するために Azure Active Directory で生成された GUID。|
|SupportTicketId|Edm.String|いいえ|「代理人」状態でのアクションのカスタマー サポート チケット ID。|
|Target|Collection(Self.IdentityTypeValuePair)|いいえ|アクション (Operation プロパティで示される) の実行対象であったユーザー。|
|TargetContextId|Edm.String|いいえ|対象ユーザーが属する組織の GUID。|



### 複合型:IdentityTypeValuePair



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|ID|Edm.String|はい|種類を指定した ID の値。|
|Type|Self.IdentityType|はい|ID の種類。|

### 列挙:IdentityType - タイプ:Edm.Int32



|**メンバー名**|**説明**|
|:-----|:-----|
|Claim|ID は、認証目的の要求です。|
|Name|監査アクション アクターまたはターゲット ID の表示名。|
|Other|アクターの ID は、Office 365 サービスによって生成された GUID の ObjectId などの、他の種類です。|
|PUID|監査アクション アクターまたはターゲット パスポートの固有 ID (PUID)。|
|SPN|Office 365 サービスでアクションが実行される場合、サービス プリンシパルの ID。|
|UPN|ユーザー プリンシパル名。|


## Azure Active Directory STS ログオン スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.String|いいえ|ログインを要求しているアプリケーションを表す GUID。表示名は、Azure Active Directory グラフ API を使用して検索できます。|
|Client|Edm.String|いいえ|ログインを実行するブラウザーが提供する、クライアント デバイス情報。|
|LogonError|Edm.String|いいえ|失敗したログインの場合、ログインが失敗した理由が含まれます。|




## DLP Schema
DLP events will always have UserKey="DlpAgent" in the common schema.  There are 3 types of DlpEvents which which are stored as the value of the Operation property of the common schema:
- "DlpRuleMatch":  This indicates a rule was matched. These events exist in both SharePoint/OneDrive and Exchange. For Exchange it includes false positive and override information. For SharePoint, false positive and overrides generate separate events.
- "DlpRuleUndo": These only exist in SharePoint/OneDrive, and indicate a previously applied policy action has been “undone” – either because of false positive/override designation by user, or because the document is no longer subject to policy (either due to policy change or doc change).
- "DlpInfo": These only exist in SharePoint/OneDrive and indicate a false positive designation but no action was “undone.”



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|SharePointMetaData|Self.SharePointMetadata|いいえ|Describes metadata about the document in SharePoint or OneDrive for Business that contained the sensitive information.|
|ExchangeMetaData|Self.ExchangeMetadata|いいえ|Describes metadata about the email message that contained the sensitive information.|
|ExceptionInfo|Edm.String|いいえ|Identifies reasons why a policy no longer applies and/or any information about false positive and/or override noted by the end user.|
|PolicyDetails|Collection(Self.PolicyDetails)|はい|Information about 1 or more policies that triggered the DLP event.|
|SensitiveInfoDetectionIsIncluded|ブール型 (Boolean)|はい|Indicates whether the event contains the value of the sensitive data type and surrounding context from the source content. Accessing sensitive data requires additional permission in Azure Active Directory.|




### SharePointMetadata complex type

|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|From|Edm.String|はい|イベントを実行したユーザーの ID。 This will be either the FileOwner, LastModifier, or LastSharer.|
|itemCreationTime|Edm.Date|はい|Datetimestamp in UTC of when event logged.|
|SiteCollectionGuid|Edm.Guid|はい|サイト コレクションの wssversion3 GUID を取得します。 |
|SiteCollectionUrl|Edm.String|はい|SharePoint サイトの URL を指定します。|
|FileName|Edm.String|はい|Name of the path.|
|FileOwner|Edm.String|はい|The document owner.|
|FilePathUrl|Edm.ExchangeFolder|はい|ドキュメント ライブラリの URL。|
|DocumentLastModifier|Edm.String|はい|セクションを最後に変更したユーザー。|
|DocumentSharer|Edm.String|はい|The user who last modiifed sharing of the document.|
|UniqueId|Edm.String|はい|ファイルを識別する GUID。|
|LastModifiedTime|Edm.Date|はい|Timestamp in UTC for when doc was last modified.|


### ExchangeMetadata complex type

|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|MessageID|Edm.String|はい|The message ID of the email that triggered the event.|
|From|Edm.String|はい|The user who sent the email.|
|To|Collection(Edm.String)|いいえ|A collection of email addresses that were on the To line of the message.|
|CC|Collection(Edm.String)d|いいえ|A collection of email addresses that were on the CC line of the message.|
|BCC|Collection(Edm.String)|いいえ|A collection of email addresses that were on the BCC line of the message.|
|Subject|Edm.String|はい|電子メール メッセージのヘッダー。 |
|Sent|Edm.Date|はい|The time in UTC of when the email was sent.|
|RecipientCount|Edm.Int32|はい|The total number of all recipients on the TO, CC, and BCC lines of the message.|


### PolicyDetails complex type

|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|PolicyId|Edm.Guid|はい|The guid of the DLP policy for this event.|
|PolicyName|Edm.String|はい|The friendly name of the DLP policy for this event.|
|ルール|Collection(Self.Rules)|はい|Information about the rules within the policy that were matched for this event.|

### Rules complex type

|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|RuleId|Edm.Guid|はい|The guid of the DLP rule for this event.|
|RuleName|Edm.String|はい|The friendly name of the DLP rule for this event.|
|アクション|Collection(Edm.String)|いいえ|A list of actions taken as a result of a DLP RuleMatch event.|
|OverriddenActions|Collection(Edm.String)|いいえ|A list of actions previously taken that were now undone as a result of a DLPRuleUndo event. |
|重大度|Edm.String|いいえ|The severity of the rule match.|
|RuleMode|Edm.String|はい|Indicate whether the DLP Rule was set to Enforce, Audit with Notify, or Audit only.|
|ConditionsMatched|Self.ConditionsMatched|いいえ|Details about what conditions of the rule were matched for this event.|

### ConditionsMatched complex type

|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|SensitiveInformation|Collection(Self.SensitiveInformation)|いいえ|Information about the type of sensitive information detected.|
|DocumentProperties|Collection(Edm.FIX)|いいえ|Information about document properties that triggered a rule match.|
|OtherConditions|Collection(Edm.FIX)|いいえ|A list of key value pairs describing any other conditions that were matched.|

### SensitiveInformation complex type

|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.Int|はい|The confidence of pattern that matched the detection.|
|Count|Edm.Int|はい|The number of sensitive instances detected.|
|SensitiveType|Edm.Guid|はい|A guid that indentifies the type of sensitive data detected.|

### ExceptionInfo complex type

|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|理由|Edm.String|いいえ|For a DLPRuleUndo event, this indicates why the rule no longer applies, which can be one of 3 reasons: Override, Document Change, or Policy Change|
|FalsePositive|Edm.Boolean|いいえ|Indicates whether the user designated this event as a false positive.|
|Justification プロパティ|Edm.String|いいえ|If the user chose to override policy, any user-specified justification is captured here.|
|ルール|Collection(Edm.Guid)|いいえ|A collection of guids for each rule that was designated as a false positive or override, or for which an action was undone.|


## Security & Compliance Center Schema



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|いいえ|The date and time at which the cmdlet was executed.|
|ClientRequestId|Edm.String|いいえ|A GUID that can be used to correlate this cmdlet with the Security & Compliance Center UX operations. This information is only used by Microsoft support.|
|CmdletVersion|Edm.String|いいえ|The build version of the cmdlet when it was executed.|
|EffectiveOrganization|Edm.String|いいえ|The GUID for the organization impacted by the cmdlet. (Deprecated: This parameter will stop appearing in the future.)|
|UserServicePlan|Edm.String|いいえ|The Exchange Online Protection service plan assigned to the user that executed the cmdlet.|
|ClientApplication|Edm.String|いいえ|If the cmdlet was executed by an application, as opposed to remote powershell, this field contains that application’s name.|
|パラメーター|Edm.String|いいえ|The name and value for parameters that were used with the cmdlet that do not include Personally Identifiable Information.|
|NonPiiParameters|Edm.String|いいえ|The name and value for parameters that were used with the cmdlet that include Personally Identifiable Information. (Deprecated: This field will stop appearing in the future and its content merged with the Parameters field.)|




## Sway スキーマ

「[Office 365 プロテクション センターでの監査ログの検索](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US)」にリストされている Sway イベント (ファイルおよびフォルダー イベントを除く) は、このスキーマを使用します。



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|ObjectType|Self.ObjectType|いいえ|トリガーされたイベントのアクセス ポイント。アクセス ポイントには以下が含まれます。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Sway</p></li><li><p>ホスト内に埋め込まれた Sway。</p></li><li><p>Office 365 管理ポータル内の Sway 設定。</p></li></ul>|
|Endpoint|Self.Endpoint|いいえ|トリガーされたイベントの Sway クライアント エンドポイント。Sway クライアント エンドポイントには、Web、iOS、Windows、または Android を使用できます。 |
|BrowserName|Edm.String|いいえ|トリガーされたイベントの Sway にアクセスするために使用するブラウザー。 |
|DeviceType|Self.DeviceType|いいえ|トリガーされたイベントの Sway にアクセスするために使用するデバイスの種類。デバイスの種類は、デスクトップ、モバイル、またはタブレットのいずれかにできます。|
|SwayLookupId|Edm.String|いいえ|Sway ID。 |
|SiteUrl|Edm.String|いいえ|Sway の URL。|
|OperationResult|Self.OperationResult|いいえ|成功または失敗のいずれか。|


### 列挙:ObjectType - タイプ: Edm.Int32


|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|Sway|Sway からトリガーされたイベント。|
|1|SwayEmbedded|ホストに埋め込まれている、Sway からトリガーされたイベント。|
|2|SwayAdminPortal|イベントは、Office 365 管理ポータルの Sway サービス設定からトリガーされました。|


### 列挙:OperationResult - タイプ: Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|Succeeded|イベントは成功しました。|
|1|Failed|イベントは失敗しました。|


### 列挙:Endpoint - タイプ: Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|SwayWeb|Sway の Web クライアントを使用してイベントがトリガーされました。|
|1|SwayIOS|Sway の iOS クライアントを使用してイベントがトリガーされました。|
|2|SwayWindows|Sway の Windows クライアントを使用してイベントがトリガーされました。|
|3|SwayAndroid|Sway の Android クライアントを使用してイベントがトリガーされました。|



### 列挙:DeviceType - タイプ: Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|0|Desktop|イベントはデスクトップを使用してトリガーされました。|
|1|Mobile|イベントはモバイル デバイスを使用してトリガーされました。|
|2|Tablet|イベントはタブレット デバイスを使用してトリガーされました。|



### 列挙:SwayAuditOperation - タイプ: Edm.Int32



|**値**|**メンバー名**|**説明**|
|:-----|:-----|:-----|
|1|Create|ユーザーは Sway を作成します。|
|2|Delete|ユーザーは Sway を削除します。|
|3|View|ユーザーは Sway を表示します。|
|4|Edit|ユーザーは Sway を編集します。|
|5|Duplicate|ユーザーは Sway を複製します。|
|7|Share|ユーザーは Sway の共有を開始します。このイベントは、Sway 共有メニュー内で特定の共有先をクリックするユーザー アクションをキャプチャします。イベントでは、ユーザーが実際に共有アクションを最後まで実行して完了させるかどうかは示されません。|
|8|ChangeShareLevel|ユーザーは Sway の共有のレベルを変更します。このイベントは、ユーザーによる、Sway に関連付けられている共有のスコープの変更をキャプチャします。たとえば、「組織内から」と比較して「パブリック」などへの変更です。|
|9|RevokeShare|ユーザーは、アクセス権限の取り消しによって、Sway の共有を停止します。アクセス権限を取り消すと、Sway に関連付けられているリンクは変更されます。|
|10|EnableDuplication|ユーザーは Sway を複製できます (既定ではオン)。|
|11|DisableDuplication|ユーザーは Sway を複製できません (既定ではオフ)。|
|12|ServiceOn|ユーザーは、Office 365 管理センターを使用して、組織全体で Sway を有効にします (既定ではオン)。|
|13|ServiceOff|ユーザーは、Office 365 管理センターを使用して、組織全体で Sway を無効にします (既定ではオフ)。|
|14|ExternalSharingOn|ユーザーは、Office 365 管理センターを使用して、組織全体で外部共有を有効にします。|
|15|ExternalSharingOff|ユーザーは、Office 365 管理センターを使用して、組織全体で外部共有を無効にします。|

## データ センター セキュリティ基本スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|DataCenterSecurityEventType|Self.DataCenterSecurityEventType|はい|ロック ボックス内のコマンドレット イベントのタイプ。|

### 列挙:DataCenterSecurityEventType - タイプ:Edm.Int32



|**メンバー名**|**説明**|
|:-----|:-----|
|DataCenterSecurityCmdletAuditEvent|これは、コマンドレット監査タイプ イベントの列挙型の値です。|



## データ センター セキュリティ コマンドレット スキーマ



|**パラメーター**|**タイプ**|**必須かどうか?**|**説明**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|はい|コマンドレット実行の開始時刻。|
|EffectiveOrganization|Edm.String|はい|昇格/コマンドレットの対象とされたテナントの名前。|
|ElevationTime|Edm.Date|はい|イベントの開始時刻。|
|ElevationApprover|Edm.String|はい|Microsoft 管理者の名前。|
|ElevationApprovedTime|Edm.Date|いいえ|昇格が承認されたときのタイムスタンプ。|
|ElevationRequestId|Edm.Guid|はい|昇格要求の一意識別子。|
|ElevationRole|Edm.String|いいえ|昇格が要求されたロール。|
|ElevationDuration|Edm.Int32|はい|昇格がアクティブであった期間。|
|GenericInfo|Edm.String|いいえ|コメントやその他の一般的な情報に使用されます。|


