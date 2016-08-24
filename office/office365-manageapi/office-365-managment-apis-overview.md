
# Office 365 管理 API の概要


 _**適用対象:** Office 365_



The Office 365 Management APIs will provide a single extensibility platform for all Office 365 customers' and partners' management tasks, including service communications, security, compliance, reporting, and auditing.
All of the Office 365 Management APIs will be consistent in design and implementation with the current suite of Office 365 REST APIs, using common industry-standard approaches, including OAuth v2, OData v4, and JSON. Like the other Office 365 APIs, applications are registered in Azure Active Directory, giving developers a consistent way to authenticate and authorize their apps.
If you have questions about the Office 365 Management APIs, post your question on [Stack Overflow](http://stackoverflow.com/tags/office365), using the [office365] tag.

## Office 365 サービス通信 API (プレビュー)

Office 365 サービス通信 API は、プレビュー モードでリリースされています。これは現在の [Office 365 サービス通信 API](https://msdn.microsoft.com/library/office/dn776043.aspx) に置き換わるものであり、テナント管理者およびパートナーにサービス正常性の情報を提供します。以前のバージョンとは異なり、新しいサービス通信 API は、URL の名前付け、データ形式、および認証を含め、一貫した方式で構築された REST API により、まとまりのよいプラットフォーム操作性を提供します。

新機能は、新しいバージョンの API にのみ追加されます。旧バージョンをご使用のお客様は、早期の導入をお勧めします。Office 365 サービスの通信 API の通常の製品発表が行われると、サービス通信 API の旧バージョンの廃止に向けた期間が開始されます。操作のリファレンスについては、「[Office 365 サービス通信 API リファレンス (プレビュー)](https://msdn.microsoft.com/EN-US/library/dn707386.aspx)」を参照してください。


## Office 365 管理アクティビティ API

Office 365 管理アクティビティ API は、Office 365 と Azure Active Directory のアクティビティ ログからの、ユーザー、管理者、システム、およびポリシー アクションとポリシー イベントについての情報を提供します。お客様とパートナーは、この情報を使用して、操作、セキュリティ、およびコンプライアンス監視の新しい企業向けソリューションを作成したり、既存のソリューションを拡張したりできます。操作のリファレンスについては、「[Office 365 管理アクティビティ API リファレンス](https://msdn.microsoft.com/EN-US/library/dn707386.aspx)」を参照してください。

