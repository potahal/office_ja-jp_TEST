SharePoint アドイン モデルにおけるマスター ページ
===========================================

概要
-------

新しい SharePoint アドイン モデルで SharePoint サイトにカスタム マスター ページを実装する方法は、完全信頼コード / ファーム ソリューションの場合とは異なります。一般的な完全信頼コード (FTC) ファーム / ソリューション シナリオでは、カスタム マスター ページはカスタム ブランドを実装するよう作成されます。マスター ページは通常、宣言型コードと FTC / ファーム ソリューションを使用してマスター ページを展開し、それらを SharePoint サイトに登録します。

SharePoint アドイン モデル ブランド化のシナリオでは、カスタム マスター ページを使用する場合もあります。リモート プロビジョニング パターンを使用して SharePoint サイトでカスタム マスター ページを展開して登録できます。

カスタム マスター ページの基本ガイドライン
---------------------------------------------

カスタム マスター ページについては、大まかに次のような基本ガイドラインが提供されています。

- カスタム マスター ページを使用して SharePoint サイトをカスタマイズすることができますが、長期にわたる追加コストと将来の更新に関する課題が生じることに留意してください。
    + ほとんどの場合、テーマ、構成された外観、および代替 CSS により、一般的なすべてのブランド化シナリオを達成できます。
    
        SharePoint アドイン モデルについて、SharePoint サイトで利用できるさまざまなブランドのオプションの詳細については、「[SharePoint サイトのブランド化 (SharePoint アドイン レシピ)](branding-sharepoint-sites-sharepoint-add-in.md)」を参照してください。  このレシピは、カスタマイズの短期的および長期的な影響を運用とメンテナンスの観点から考慮するのに役立ちます。 特定のブランドの要件を実装するのにカスタム マスター ページは必要ではないことがわかります。 

    + カスタム マスター ページを使用する場合は、Office 365 に主要な機能更新が適用されたときにカスタム マスター ページに変更を適用する準備をしておいてください。
- カスタム マスター ページを SharePoint サイトに展開して登録するには、リモート プロビジョニングを使用します。
- マスター ページを SharePoint サイトに展開して登録する場合、宣言型コードまたはサンドボックス コードを使用しないでください。  

チーム サイトと発行サイト
-------------------------------

**どのような場合にカスタム マスター ページが必要となるでしょうか。**

SharePoint サイトにカスタム ブランド化を適用するときは、チーム サイトと発行サイトの両方をブランド化する必要があります。一般に、オンプレミスと Office 365 の両方のシナリオで、SharePoint 上に構築されたイントラネットにチーム サイトと発行サイトの組み合わせが使用されます。  

カスタム ブランド化の要件では、テーマや JavaScript 埋め込み手法では実現できない特定のレイアウト変更がしばしば要求されます。

そのようなシナリオでは、チーム サイトで要求されるカスタム ブランド化は発行サイトの場合ほど多くなく、通常はモバイル デバイス用の標準の SharePoint コンテンポラリ表示でチーム サイト用にのバイル デバイスを十分にサポートできます。そのようなわけで、発行サイトにはカスタム マスター ページだけを使用し、チーム サイトのブランド化には、構成された外観として定義されたカスタム SharePoint テーマ (.spcolor ファイル)、フォント スキーム (.spfont ファイル)、および背景画像を使用することをお勧めします。

**展開に関する考慮事項**

- カスタム マスター ページを*発行サイト*に展開するときには、ルート サイトにカスタム マスター ページを展開するだけで十分です。
    + [Provisioning.PublishingFeatures (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.PublishingFeatures) は、発行サイトにカスタム マスター ページを展開する方法を示しています。 
    + [ SharePoint の発行機能のプロビジョニング (O365 PnP ビデオ)](http://channel9.msdn.com/Blogs/Office-365-Dev/Provisioning-SharePoint-Publishing-Features-Office-365-Developer-Patterns-and-Practices) は、サンプルについて説明します。   
- カスタム マスター ページを*非発行サイト*に展開するときには、カスタム マスター ページを各サイトに展開する必要があります。

通常、カスタム マスター ページはサイトがプロビジョニングされるときに適用されます。リモート プロビジョニング プロセスは、この方法に最適です。通常、Web ブラウザーを使用して SharePoint ブランド化カスタマイズを手動で適用するのは、拡張して他のサイト コレクションやサブサイトを含める計画のない単一の SharePoint サイトを試作または変更するときだけです。 

+ その他の展開の詳細や追加サンプルについては、[ (SharePoint アドイン レシピ)](modules-sharepoint-add-in.md) および [サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md) を参照してください。

SharePoint サイトのカスタム マスター ページおよびページ レイアウトの詳細
----------------------------------------------------------------------------

カスタム ブランド化の要件を実装する唯一の方法がカスタム マスター ページであるシナリオでは、カスタム マスター ページとページ レイアウトを作成することができます。この記事の冒頭でこのアプローチに関連する長期的なメンテナンス コストに関して述べられた点に注意してください。

- SharePoint サイト用のカスタム マスター ページを使用すると、最高レベルのカスタマイズ (無制限) を実行できます。
- SharePoint サイト用のカスタム マスター ページを短期および長期間にわたって実装およびメンテナンスするには、大量の時間が必要です。
- サービス更新プログラムに付属する標準のマスター ページへの変更は、カスタム マスター ページには反映されません。
- カスタム マスター ページはサイトごとのレベルで適用できます。
- カスタム マスター ページを使用する場合、最初は標準のマスター ページをニーズに合わせて変更することをお勧めします。
    + カスタム マスター ページでカスタマイズする量は最小限にしてください。これにより、標準のマスター ページに Office 365 サービスが加えた変更をカスタム マスター ページに複製しなければならない場合に、カスタム マスター ページを容易に更新できます。  
- SharePoint マスター ページには必須のコンテンツ プレースホルダーが多数あり、削除するとページでエラーが発生します。必須のコンテンツ プレースホルダーを削除した場合は、展開してそのマスター ページをサイトに割り当てたときにエラーが表示されるため、エラーを把握できます。

**SharePoint サイト用のカスタム マスター ページとページ レイアウトが適しているケース**

このオプションが適しているのは、ブランド化のニーズが非常に具体的である場合や、発行サイトを使用している場合です。

**推奨される展開アプローチ**

- Web ブラウザーを使用してカスタム マスター ページを手動でアップロードし、構成された外観に手動で割り当てることができます。
- リモート プロビジョニング パターンを使用してカスタム マスター ページをアップロードし、SharePoint サイトに割り当てることもできます。
    + その他の展開の詳細や追加サンプルについては、[ (SharePoint アドイン レシピ)](modules-sharepoint-add-in.md) および [サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md) を参照してください。

関連リンク
=============
- [モジュール (SharePoint アドインのレシピ)](modules-sharepoint-add-in.md)
- [サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md)
- [SharePoint サイトのブランド化 (SharePoint アドインのレシピ)](branding-sharepoint-sites-sharepoint-add-in.md)
- Ignite 2015 - [反復可能なパターンとプラクティスによる Office 365 での安全な SharePoint ブランド化の詳細](https://channel9.msdn.com/Events/Ignite/2015/BRK3164)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [CSOM を使用したテーマ管理 (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.DeployCustomThemeWeb)
- [Web オブジェクトの AlternateCSSUrl および SiteLogoUrl (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.AlternateCSSAndSiteLogo)
- [サイトにテーマを設定する (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.SetThemeToSite)
- [SharePoint 用アプリでの SharePoint テーマの設定 (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.Themes)
- [標準の Seattle マスター応答の作成 (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.InjectResponsiveCSS)
- [https://github.com/OfficeDev/PnP](https://github.com/OfficeDev/PnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
