# PnP プロビジョニング スキーマ

「 [PnP プロビジョニング フレームワーク](pnp-provisioning-framework.md)」などですでに学んでこられたように、プロビジョニング テンプレートの形式は永続化形式とは切り離されているため、任意の形式を使用できます。 とはいえ、XML のプロビジョニング スキーマを使用してテンプレートを永続化することはよく行われることなので、ここでは、XML スキーマを使用してプロビジョニング テンプレートをシリアル化して保存する方法に関する追加情報を提供します。

**重要:** 言うまでもなく、プロビジョニング スキーマはプロビジョニング テンプレートの XML シリアル化をサポートしていますが、それに加え、JSON 形式のシリアル化構造も備えています。すなわち、このスキーマはプロビジョニング構造を定義するためのモデルを備えています。

## プロビジョニング スキーマのリソース

プロビジョニング スキーマ、およびそのサポート ファイルは、GitHub の「 [PnP-Provisioning-Schema](https://github.com/OfficeDev/PnP-Provisioning-Schema)」で入手できます。

プロビジョニング スキーマについて取り上げた、「 [PnP プロビジョニング エンジン スキーマの詳細](https://channel9.msdn.com/blogs/OfficeDevPnP/Deep-dive-to-PnP-provisioning-engine-schema)」という 20 分間の Channel 9 ビデオがあります。

サンプル スキーマは、GitHub の「 [PnP-Provisioning-Schema/Samples](https://github.com/OfficeDev/PnP-Provisioning-Schema/tree/master/Samples)」から入手できます。

以下のコード ブロックは、このスキーマのルート要素と、ルートの直接の子要素を示しています。GitHub の「 [PnP プロビジョニング スキーマ](https://github.com/OfficeDev/PnP-Sites-Core/blob/dev/Core/Tools/OfficeDevPnP.Core.Tools.DocsGenerator/OfficeDevPnP.Core.Tools.DocsGenerator/ProvisioningSchema-2015-08.md)」にはプロビジョニング スキーマに関するドキュメントがあります。

```
<pnp:ProvisioningTemplate
           xmlns:pnp="http://schemas.dev.office.com/PnP/2015/08/ProvisioningSchema"
           ID="xsd:ID"
           Version="xsd:decimal"
           ImagePreviewUrl="xsd:string"
           DisplayName="xsd:string"
           Description="xsd:string">
   <pnp:Properties />
   <pnp:SitePolicy />
   <pnp:RegionalSettings />
   <pnp:SupportedUILanguages />
   <pnp:AuditSettings />
   <pnp:PropertyBagEntries />
   <pnp:Security />
   <pnp:SiteFields />
   <pnp:ContentTypes />
   <pnp:Lists />
   <pnp:Features />
   <pnp:CustomActions />
   <pnp:Files />
   <pnp:Pages />
   <pnp:TermGroups />
   <pnp:ComposedLook />
   <pnp:Workflows />
   <pnp:SearchSettings />
   <pnp:Publishing />
   <pnp:AddIns />
   <pnp:Providers />
</pnp:ProvisioningTemplate>
```

## その他のリソース
<a name="bk_addresources"> </a>

- [PnP プロビジョニング エンジン スキーマの詳細](https://channel9.msdn.com/blogs/OfficeDevPnP/Deep-dive-to-PnP-provisioning-engine-schema)
    
- [PnP プロビジョニング スキーマ](https://github.com/OfficeDev/PnP-Sites-Core/blob/dev/Core/Tools/OfficeDevPnP.Core.Tools.DocsGenerator/OfficeDevPnP.Core.Tools.DocsGenerator/ProvisioningSchema-2015-08.md)
    
- [GitHub の PnP-Provisioning-Schema/Samples](https://github.com/OfficeDev/PnP-Provisioning-Schema/tree/master/Samples)
