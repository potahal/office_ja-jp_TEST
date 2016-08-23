# 指数関数的バックオフを使用して SharePoint Online の調整を処理する

指数関数的バックオフの手法を使用して SharePoint Online の調整を処理する方法について説明します。 
    
_
            **適用対象:**Office 365 | SharePoint Online | SharePoint Server 2013_

SharePoint Online は調整を利用して、ユーザーがリソースを過剰に消費しないようにします。ユーザーが CSOM コードや REST コードを実行する際に使用量の限度を超えると、SharePoint Online は一定期間、ユーザーからの追加の要求を調整します。 
    
[Office 365 開発者向けのパターンとプラクティス リポジトリ](https://github.com/OfficeDev/PnP)の [Core.Throttling](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.Throttling) コード サンプルは、指数関数的バックオフの手法を実装して SharePoint Online で調整を処理する方法を示します。SharePoint Online で調整が生じると、指数関数的バックオフの手法では、調整されたコードが再試行されるまで待機する時間は徐々に長くなります。
    
SharePoint Online での調整に関する詳細 (原因や限度など) と、Core.Throttling コード サンプルの説明については、「[方法: SharePoint Online で調整またはブロックを回避する](https://msdn.microsoft.com/library/office/dn889829.aspx)」を参照してください。 

また、[ClientContextExtensions.cs](https://github.com/OfficeDev/PnP-Sites-Core/tree/master/Core/OfficeDevPnP.Core/AppModelExtensions/ClientContextExtensions.cs) サンプルで、ExecuteQueryImplementation の拡張メソッドを確認してください。ExecuteQueryImplementation は [OfficeDevPnP.Core](https://github.com/OfficeDev/PnP-Sites-Core/tree/master/Core/OfficeDevPnP.Core) に含まれています。    

## その他のリソース
<a name="bk_addresources"> </a>

-  [ソリューション ガイダンス](Office-365-development-patterns-and-practices-solution-guidance.md)
    
-  [ClientContextExtensions.cs サンプル](https://github.com/OfficeDev/PnP-Sites-Core/tree/master/Core/OfficeDevPnP.Core/AppModelExtensions/ClientContextExtensions.cs)
    
-  [方法:SharePoint Online で調整またはブロックを回避する](https://msdn.microsoft.com/library/office/dn889829.aspx)
