# 作成時に SharePoint ホスト Web のリストを変更する

リストの作成時にホスト Web で作成された SharePoint リストを変更します。

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

ホスト Web に新しいリストを作成するとき、**ListAdded** リモート イベント レシーバーを使用してそのリストを変更できます。たとえば、バージョン管理を有効にすること、リストにコンテンツ タイプを追加すること、クライアント オブジェクト モデル (CSOM) によって実装されるその他の変更を行うことなどが可能です。

リストがホスト Web に追加されるときには、イベント レシーバーをプログラムを使用して関連付ける必要があります。[Core.EventReceiversBasedModifications](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.EventReceiversBasedModifications) サンプルは、プロバイダー ホスト型アドインを使用してこれを行う方法を示しています。アドインがインストールされると、**AppInstalled** イベントが発生し、このイベントを使用して **ListAdded** イベントを関連付けることができます。

## 始める前に

まず、[Core.EventReceiversBasedModifications](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.EventReceiversBasedModifications) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## ListAdded イベントを関連付ける

アドインのイベント ハンドラーを実装するには、SharePoint プロジェクトのプロパティを表示して、**[アドインのインストールの処理]** と **[アドインのアンインストールの処理]** の両方を **[True]** に設定します。

次のコード例では、**AppInstalled** イベント レシーバーを変更して **ListAdded** イベント レシーバーを関連付ける方法を示しています。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
bool rerExists = false;
cc.Load(cc.Web.EventReceivers);
cc.ExecuteQuery();

foreach (var rer in cc.Web.EventReceivers)
{
  if (rer.ReceiverName == RECEIVER_NAME)
  {
    rerExists = true;
  }
}

if (!rerExists)
{
  EventReceiverDefinitionCreationInformation receiver = new EventReceiverDefinitionCreationInformation();
  receiver.EventType = EventReceiverType.ListAdded;

  // Get WCF URL where this message was handled.
  OperationContext op = OperationContext.Current;
  Message msg = op.RequestContext.RequestMessage;
  receiver.ReceiverUrl = msg.Headers.To.ToString();
  receiver.ReceiverName = RECEIVER_NAME;
  receiver.Synchronization = EventReceiverSynchronization.Synchronous;
  cc.Web.EventReceivers.Add(receiver);
  cc.ExecuteQuery();
}
```

## 追加のリストをカスタマイズする

**ListAdded** イベント ハンドラーが起動すると、次のコードが実行されます。

```C#
private void HandleListAdded(SPRemoteEventProperties properties)
{
  using (ClientContext cc = TokenHelper.CreateRemoteEventReceiverClientContext(properties))
  {
    if (cc != null)
    {
      try
        {
          if (properties.ListEventProperties.TemplateId == (int)ListTemplateType.DocumentLibrary)
          {
          //set versioning 
   cc.Web.GetListByTitle(properties.ListEventProperties.ListTitle).UpdateListVersioning(true, true);
          }
        }
         catch (Exception ex)
         {
           System.Diagnostics.Trace.WriteLine(ex.Message);
         }
       }
    }
  }
}
```

## イベント レシーバーをアンインストールする

アドインをアンインストールするときには、イベント レシーバーも削除する必要があります。デバッグ中にこれが行われるように、**テスト用ライブラリのアドイン**に移動して、アドインの **[削除]** オプションを使用します。これにより、**AppUninstalling** イベントが適切な許可と共にトリガーされて、作成されたリモート イベント ハンドラーが削除されます。単にブラウザーを閉じた場合や**サイト コンテンツ**からアドインをアンインストールした場合には、イベント レシーバーが起動しないか、**ListAdded** イベント レシーバーを削除するには不十分な許可で **AppUninstalling** イベント レシーバーが実行されます。これは、アドインがサイドロードされるときに別個に展開されるためで、Visual Studio で F5 を押すとこのようになります。

**メモ:** このサンプルは、クリーンな開発者向けサイトでテストすることをお勧めします。

## その他の技術情報
<a name="bk_addresources"> </a>

- [SharePoint サイト プロビジョニング ソリューション](sharepoint-site-provisioning-solutions.md)
    
- [Core.EventReceiversBasedModifications サンプル](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.EventReceiversBasedModifications)
