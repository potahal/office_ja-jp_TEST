SharePoint アドイン モデルのユーザー プロファイルの操作
========================================================

概要
-------

新しい SharePoint アドイン モデルにおいて、ユーザー プロファイル サービス (UPS) の作成、読み取り、更新、および削除 (CRUD) 操作の実行方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、UPS の CRUD　操作は SharePoint サーバー側オブジェクト モデル コードまたはユーザー プロファイル サービスを使用して実行され、ファーム ソリューション経由で展開されていました。 

SharePoint アドイン モデル シナリオでは、UPS の CRUD 操作は、クライアント側オブジェクト モデル (CSOM) またはユーザー プロファイル Web サービスで実行されます。  

基本ガイドライン
---------------------

UPS の CRUD 操作の実行については、大まかに次のような基本ガイドラインが提供されています。

- 次の表で、CSOM およびユーザー プロファイル Web サービス API に対し、Office 365 とオンプレミスの SharePoint 環境でサポートされる操作の種類について説明します。

    操作  | API | オンプレミス | Office 365
    ---------| -----| ---------| ------
    READ | CSOM | サポートされている | サポートされている
    CREATE | CSOM | サポート対象外 | サポートされている
    UPDATE | CSOM | サポート対象外 | サポートされている
    DELETE | CSOM | サポート対象外 | サポートされている
    READ | ユーザー プロファイル Web サービス | サポートされている | サポート対象外
    CREATE | ユーザー プロファイル Web サービス | サポートされている | サポート対象外
    UPDATE | ユーザー プロファイル Web サービス | サポートされている | サポート対象外
    DELETE | ユーザー プロファイル Web サービス | サポートされている | サポート対象外

- 一般的に、UPS データ のコピーや同期化を処理するためにアドインを SharePoint テナンシーに展開することはありません。通常アドインは、スケジュールされたタスクや、長時間実行されるクラウド サービス (Azure Web ジョブなど) として実行されるコンソール アプリケーションの形式をとります。
    + これらのテクノロジと、それらを SharePoint アドイン モデルで使用する方法の詳細については、「[リモート タイマー ジョブ (SharePoint アドイン モデルのレシピ)](remote-timer-jobs-sharepoint-add-in.md)」を参照してください。
- AppOnly 認証の使用は、どのユーザー プロファイル サービス操作でもサポートされません。
- UPS の CRUD 操作を実行するには、適切なアクセス許可を持つアカウントを使用して、CSOM のコードを実行します。
- Active Directory をユーザー プロファイル サービスに同期する場合、既定でいくつかの属性が同期されます。
    + 「[SharePoint Server 2013 の既定のユーザー プロファイル プロパティ マッピング (TechNet 記事)](https://technet.microsoft.com/en-us/library/hh147510.aspx)」には、既定で同期される属性の一覧が含まれています。
    + 追加の属性を同期する必要がある場合は、この記事で説明した方法のいずれかを使用してカスタム ツールを作成する必要があります。

UPS データをコピーして同期するオプション
----------------------------------------

UPS のデータをコピーおよび同期するには、いくつかのオプションがあります。

- 社内
    + データベースのコピー
    + ユーザー プロファイル Web サービスを使用してデータをコピーする
    + ユーザー プロファイル Web サービスを使用してデータを同期する
- Office 365
    + CSOM を使用してデータをコピーする
    + CSOM を使用してデータを同期する

オンプレミス - データベースのコピー
---------------------------
オンプレミスの SharePoint 環境の場合は、UPS データベースをあるファームから別のファームにコピーし、用語をすばやくレプリケートできます。

**適切な場合**

オンプレミスの SharePoint 環境で、プロファイル属性を一方向にコピーするときは、コードを記述することがなく迅速かつ容易に実装できる場合があるこのオプションが適しています。

オンプレミス - ユーザー プロファイル Web サービスを使用してデータをコピーする
-----------------------------------------------------------
オンプレミスの SharePoint 環境の場合は、ユーザー プロファイル Web サービスを使用してあるファームから別のファームに UPS データをコピーできます。

**適切な場合**

オンプレミスの SharePoint 環境で 2 つ以上の SharePoint ファーム間で UPS データをコピーする場合、あるファームから別のファームに UPS データを柔軟にコピーできるこのオプションが適しています。

**作業の開始**

次のサンプルでは、ユーザー プロファイル Web サービスを使用して UPS の CRUD 操作を実行する方法について説明します。

- [Core.UserProfilePropertyUpdater (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.UserProfilePropertyUpdater)

オンプレミス - ユーザー プロファイル Web サービスを使用してデータを同期する
-----------------------------------------------------------
オンプレミスの SharePoint 環境の場合は、ユーザー プロファイル Web サービスを使用してファーム間で UPS データを同期できます。

**適切な場合**

オンプレミスの SharePoint 環境で 2 つ以上の SharePoint ファーム間で UPS データ同期する場合、真の同期化を柔軟に実行でき、必要なだけソースを含めることができるこのオプションが適しています。

**作業の開始**

「[Core.UserProfilePropertyUpdater (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.UserProfilePropertyUpdater)」は、ユーザー プロファイル Web サービスを使用して UPS の CRUD 操作を実行する方法について説明します。

「[Core.MMSSync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.MMSSync)」は、管理メタデータ サービス (MMS のデータ) 用同期ツールの構築について説明します。このサンプルでは、MMS の API に重点を置いていますが、同期に使用している全体的なパターンは UPS のデータにも適用できます。

Office 365 - CSOM を使用してデータをコピーする
----------------------------------
Office 365 SharePoint 環境の場合は、CSOM を使用して UPS のデータをあるテナンシーから別のテナンシーにコピーできます。  

**適切な場合**

Office 365 環境を使用し、2 つ以上の SharePoint テナンシー間で UPS データをコピーする場合、1 つのテナンシーから別のテナンシーへ UPS データを柔軟にコピーできるこのオプションが適しています。

**作業の開始**

「[UserProfile.Manipulation.CSOM (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/UserProfile.Manipulation.CSOM)」は、CSOM を使用して UPS の CRUD 操作を実行する方法について説明します。

「[読み取りおよび更新用ユーザー プロファイル CSOM (O365 PnP ビデオ) ](https://channel9.msdn.com/blogs/OfficeDevPnP/User-profile-CSOM-for-reading-and-updates)「[UserProfile.Manipulation.CSOM (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/UserProfile.Manipulation.CSOM) について説明します。

Office 365 - CSOM を使用してデータを同期する
----------------------------------
Office 365 SharePoint 環境を使用し、2 つ以上の SharePoint テナンシー間で UPS データをコピーする場合、真の同期化を柔軟に実行でき、必要なだけソースを含めることができるこのオプションが適しています。

**適切な場合**

Office 365 環境を使用し、2 つ以上の SharePoint テナンシー間で UPS データをコピーする場合、真の同期化を柔軟に実行でき、必要なだけソースを含めることができるこのオプションが適しています。

**作業の開始**

「[Core.UserProfiles.Sync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.UserProfiles.Sync)」は、ユーザー プロファイル サービス データ用の同期ツールを構築する方法について説明します。

ハイブリッド環境
-------------------

オンプレミスと Office 365 SharePoint 環境の両方があり、両方の環境でユーザー プロファイル情報を維持する必要がある場合は、ユーザー プロファイル Web サービスと CSOM を組み合わせることで、UPS データで CUD 操作を実行することができます。

関連リンク
=============
- [読み取りおよび更新用ユーザー プロファイル CSOM (O365 PnP ビデオ)](https://channel9.msdn.com/blogs/OfficeDevPnP/User-profile-CSOM-for-reading-and-updates)
- [リモート タイマー ジョブ (SharePoint アドイン モデル レシピ)](remote-timer-jobs-sharepoint-add-in.md)
- [SharePoint Server 2013 の既定のユーザー プロファイル プロパティ マッピング (TechNet 記事)](https://technet.microsoft.com/en-us/library/hh147510.aspx)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================
- [Core.UserProfilePropertyUpdater (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.UserProfilePropertyUpdater)
- [Core.MMSSync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.MMSSync)
- [UserProfile.Manipulation.CSOM (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/UserProfile.Manipulation.CSOM)
- [Core.UserProfiles.Sync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.UserProfiles.Sync) 
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
