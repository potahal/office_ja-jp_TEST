
# iPad 用の Office アドインを設計する
Office アドインを Office for iPad で使用できるようにします。

 _ **適用対象:** apps for Office?| Office Add-ins?| Office for iPad_

Office アドインは、Office ホストのコンテキスト内で、ユーザーに追加機能を提供します。Office アドインを Office for iPad で使用できるようにするには、次のガイドラインを適用します。

- アドインが、以下の設計ガイドラインに従っており、Office ストア で使用可能なアドインの要件を満たしているようにします。詳細については、以下をご覧ください。
    
      - [Office ストアに提出されたアプリとアドインの検証ポリシー (バージョン 1.9)](http://msdn.microsoft.com/library/cd90836a-523e-42f5-ab02-5123cdf9fefe%28Office.15%29.aspx) (ポリシー 3.4、4.12、10.8)
    
  - [Office アドインの設計ガイドライン](../add-in-design.md)
    
  - [Office アドインの設計のベスト プラクティス](d455b76b-4d76-493d-a681-6b02ba1f38a8.md)
    
  - [iOS の設計](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/)
    
- バージョン 1.1 の JavaScript API for Office を使用します。詳細については、「 [JavaScript API for Office およびマニフェスト スキーマ ファイルのバージョンを更新する](../docs/develop/update-your-javascript-api-for-office-and-manifest-schema-version.md)」をご覧ください。API の変更点を確認するには、「 [JavaScript API for Office の変更点](../../reference/what's-changed-in-the-javascript-api-for-office.md)」をご覧ください。
    
- iPad では Office アドインを無料にします。iPad では無料の Office アドインのみが許可されています。Windows や Office 365 などの他のプラットフォーム版として同じアドインを販売することができます。無料の Office アドインを作成する理由は何ですか。それは、無料のアドインを通して、より多くのユーザーを獲得できるからです。
    
- アドインに商取引を含めないでください。つまり、以下を行うことはできません。
    
      - 試用版の提供。
    
  - 有料版のアドインへの UI 要素やリンクを含めること。
    
  - 追加のコンテンツ、アプリ、またはアドインを販売するオンライン ストアへの UI 要素やリンクを含めること。
    
  - [プライバシー ポリシー] や [使用条件] のページに UI 要素やストアのリンクを含めること。
    

     >**メモ**  このポリシーに違反すると、Microsoft はただちに iPad でのお客様のアドインの使用を無効にします。

    アドインやアドイン内のサービスに関する詳細を共有することができます。また、他のプラットフォームで実行するアドインに課金したり商取引を含めたりすることもできます。そのためには、アドインを実行するブラウザーまたはデバイスに応じ、以下のプロパティーを使用して各種の UI を表示します。
    
      - [Context.touchEnabled プロパティ (JavaScript API for Office)](http://msdn.microsoft.com/library/fd73f94b-7e4a-422c-afdb-fef6fba43766%28Office.15%29.aspx) ? アドインが実行するホスト アプリケーションがタッチに対応しているかどうかを検出します。
    
  - [Context.commerceAllowed プロパティ (JavaScript API for Office)](http://msdn.microsoft.com/library/fd3812ac-14c3-485f-8991-d12fcc99c450%28Office.15%29.aspx) ? アドインが商取引を制限するプラットフォームで実行するかどうかを判断します。
    
アドインがこれらの要件を満たしていることを確認したら、アドインを再度 Office ストアに発行します。詳細については、「 [Office ストアに Office アドインと SharePoint アドインおよび Office 365 Web アプリを提出する](http://msdn.microsoft.com/library/ff075782-1303-4517-91cc-b3d730e9b9ae%28Office.15%29.aspx)」および「 [アプリまたはアドインを更新する](http://msdn.microsoft.com/library/7313d32b-5345-4039-ac5d-a1ba0aef890b%28Office.15%29.aspx)」をご覧ください。販売者ダッシュボード では、iPad でアドインを使用できるようにするオプションを選択し、更新されたストア ポリシーに同意してから、Apple 開発者 ID を入力します。さらに、「 [Office ストアのアプリケーション プロバイダー契約](https://sellerdashboard.microsoft.com/Assets/Content/Agreements/en-US/Office_Store_Seller_Agreement_20120927.md)」も読んで理解しておいてください。 

## その他の技術情報



- [iPad と Mac で Office アドインをデバッグする](../../docs/testing/debug-office-add-ins-on-ipad-and-mac.md)
    
