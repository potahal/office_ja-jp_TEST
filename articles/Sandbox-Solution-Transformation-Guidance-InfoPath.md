# サンドボックス ソリューションの変換ガイダンス - InfoPath
分離コードを含む InfoPath フォームを使用している場合、その InfoPath フォームは分離コードを実行するためにコードベースのサンドボックス ソリューションに依存します。 この記事では、サンドボックス ソリューションの依存関係がなくなるように InfoPath フォームを修正または変換する方法について説明します。

_
            **適用対象:**SharePoint Online の InfoPath フォーム | SharePoint 2013 | SharePoint 2016_

コードベースのサンドボックス ソリューションは、2014 年以降[非推奨になっています](https://blogs.msdn.microsoft.com/sharepointdev/2014/01/14/deprecation-of-custom-code-in-sandboxed-solutions/)。また、SharePoint Online では、この機能を完全に削除するためのプロセスが始まっています。 コードベースのサンドボックス ソリューションは、SharePoint 2013 および SharePoint 2016 でも非推奨になっています。

## InfoPath フォームを分析して可能な場合は修正する
<a name="sectionSection1"> </a>

このセクションでは、InfoPath フォームに適用できる分析および修正モデルを示します。 フォームによっては、単にフォームを**修正**するだけで再展開できるものもありますが、**InfoPath からの移行**が必要になるものもあります。その場合は、必要とされる機能を実現するための代替方法を使用することになります。 ただし、そうした対策を講じる前に、**フォームのビジネス ニーズを評価することが重要です**。ビジネスには不要になった大量の古いフォームを見かけることがよくあります。このような場合は、簡単にフォームを破棄できます。 

### 分離コードを使用した InfoPath フォームを見分ける方法
PnP 構想が提供する**[詳細なサンドボックス ソリューション インベントリ スクリプト](https://github.com/OfficeDev/PnP-Tools/tree/master/Scripts/SharePoint.Sandbox.ListSolutionsFromTenant)**を使用すると、テナント内にインストールされたサンドボックス ソリューションの一覧を取得できます。 このリストを取得したら、**WSPName 列**を調べます。一般に、**先頭が "InfoPath Form_"** の WSP は、分離コードを使用して展開された InfoPath フォームによるものです。

### フォームは現在でも有用であるか
修復/変換作業に取り組む前に、「このフォームは、現在もビジネスに不可欠であるか」と問いかけることが重要です。 そのとおり不可欠であれば、次の章に進んでください。そうではない場合、このフォームを使用して作成されるデータについて考える必要はありません。 一般に、データは InfoPath XML ファイルとして作成されます。このファイルは、SharePoint リスト内に存在します。 フォームを削除すると、データを視覚化できなくなります。フォームとデータが不要になっているため、これが問題にならないこともありますが、InfoPath XML ファイルを SharePoint リスト (アイテム) データに変換するためにデータにアクセスできるようにしたいこともあります。 [PnP - 変換リポジトリには、これを実現可能にする方法を示すサンプルがあります](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Migration/EmpRegConsole "サンプルでは InfoPath XML からリスト データへの変換方法を示しています")。

### 調査のために InfoPath フォーム (XSN ファイル) をダウンロードする
前のセクションの手順では、作業が必要な InfoPath フォームがあることを確認しました。このセクションでは、作業が必要なフォームをダウンロードする方法について説明します。 分離コードを使用した InfoPath フォームは、「フォーム ライブラリ」または「サイト コンテンツ タイプ」のどちらかとして展開されています。 選択した発行モデルに応じて、次に示すようにフォームをダウンロードできます。

#### 「フォーム ライブラリ」として展開された InfoPath フォームのダウンロード
この場合、XSN ファイルは、InfoPath フォームの展開先にしたフォーム ライブラリの [Forms] フォルダーにあります。 WSP パッケージの名前は "InfoPath Form_**ライブラリ名**_id" という規則に従っているため、その名前を調べることで目的のフォーム ライブラリがわかります。 目的のライブラリがわかったら、そのライブラリの [Forms] フォルダーから template.xsn ファイルをダウンロードする必要があります そうするために、**ライブラリの URL + "Forms/template.xsn"** という URL (この例では、`https://contoso.sharepoint.com/sites/infopath1/IHaveCodeBehind/Forms/template.xsn`) を作成します。ブラウザーを使用して、この URL からファイルをダウンロードします。

#### 「サイト コンテンツ タイプ」として展開された InfoPath フォームのダウンロード
コンテンツ タイプとして展開された InfoPath フォームは、それぞれにフォーム ライブラリに保存された XSN ファイルがあります。このフォーム ライブラリは、フォームの展開時にコンテンツ タイプに結び付けられています。 前述のセクションと同様に、WSP パッケージの名前からライブラリの名前がわかります。 前述の場合と異なり、このフォームは探し当てたライブラリに実際にファイルとして保存されています。そのため、そのフォームを目的のフォーム ライブラリから単にダウンロードするだけで済みます。

### InfoPath フォームの修正
前述のセクションでは、分離コードを使用した InfoPath フォームを示して取得しましたが、そのフォームには本当に有用な分離コードが含まれているのでしょうか。 フォームの作成者が、InfoPath の **[開発者]** リボンにある **[コード エディター]** ボタンを誤ってクリックしてしまったフォームが多数あることがわかっています。

![InfoPath の分離コード](media/Sandbox-Solution-Transformation-Guidance-InfoPath/InfoPathCodeBehind.png)

このようにすると、何も実行しない分離コードが含まれてしまいます。この分離コードを削除することで、分離コードを使用した InfoPath フォームを通常の InfoPath フォーム (分離コードを使用しないフォーム) に変換できます。これにより、サンドボックス ソリューションの依存関係がなくなります。

#### 分離コードが "不要" なことを調べる方法
不要な分離コードの修正のみが可能なときに、分離コードが不要か必要かどうかを見分ける方法がわからないかもしれません。 展開済みのフォームのオリジナル (前述の手順でダウンロードしたフォームではないもの) が残っている場合は、コードを見るだけでわかります。 次に示すコードは、既定の空のコードです。これと同様のコードがある場合は、そのコードを削除することでフォームを修正できます。

```C#
using Microsoft.Office.InfoPath;
using System;
using System.Xml;
using System.Xml.XPath;

namespace Form1
{
    public partial class FormCode
    {
        // Member variables are not supported in browser-enabled forms.
        // Instead, write and read these values from the FormState
        // dictionary using code such as the following:
        //
        // private object _memberVariable
        // {
        //     get
        //     {
        //         return FormState["_memberVariable"];
        //     }
        //     set
        //     {
        //         FormState["_memberVariable"] = value;
        //     }
        // }

        // NOTE: The following procedure is required by Microsoft InfoPath.
        // It can be modified using Microsoft InfoPath.
        public void InternalStartup()
        {
        }
    }
}
```

前述の手順でダウンロードした XSN ファイルのみがある場合は、XSN ファイルの名前を cab ファイル (たとえば、template.cab) に変更し、アセンブリを抽出し、.Net のリフレクション ツール (たとえば、オープン ソースの [ILSpy](http://ilspy.net/ "ILSpy は、オープン ソースの .NET アセンブリ ブラウザーおよびデコンパイラです")) を使用してコードを調べます。 一般に、ILSpy では、不要な分離コードは次のように表示されます。

![ILSpy に表示された不要な分離コード](media/Sandbox-Solution-Transformation-Guidance-InfoPath/ilspyoutput.png)

#### InfoPath フォームから分離コードを削除して修正する
分離コードが不要であることを確認したら、次の手順を実行することで簡単に分離コードを削除できます。
- **[InfoPath Designer]** でフォームを開きます (右クリックして、[デザイン] を選択します)
- **[フォームのオプション]** に移動します ([ファイル] > [情報])
- **[プログラミング]** カテゴリを選択して、**[コードの削除]** をクリックします。
- 
            **フォームを再発行します** ([ファイル] > [情報] > [クイック発行])
- 
            **リンクされたサンドボックス ソリューションを非アクティブ化します** ([サイト設定] > [ソリューション])
- フォームが正常に動作することを**確認します**
- サンドボックス ソリューションを**削除します**

## InfoPath フォームの移行

                <a name="sectionSection2"> </a> 前述の章で示したガイダンスが適用できない InfoPath フォームがある場合、そのフォームは現在もビジネスに有用であり、削除できない分離コードを含んでいることを意味します。 その場合の一般的な解決策は、InfoPath からの移行になります。これは、さまざまな方法で実現できます。
- [Azure PowerApps](https://powerapps.microsoft.com/en-us/ "Azure PowerApps") または [Microsoft Flow](https://flow.microsoft.com/en-us/search/?q=sharepoint "Microsoft Flow") を使用してアプリを作成する 

![Azure PowerApps と Microsoft Flow](media/Sandbox-Solution-Transformation-Guidance-InfoPath/powerappsflow.png)

- SharePoint データの読み取り/書き込みにリモート API を利用する SharePoint アドインを作成する

### InfoPath フォームを置き換える SharePoint アドインの作成
InfoPath フォームを置き換えるために通常の SharePoint アドインを使用する場合には、いくつかのオプションがあります。 次に示す 3 つのオプションについては精査していますが、これらのバリエーションを使用してもまったくかまいません。 次の 3 つのオプションについて、詳しく説明します。
- 
            [knockout.js に基づいた単一ページ アプリケーション (SPA)](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Samples/EmployeeRegistration.KnockOut.SinglePageApp "knockout.js を使用した SPA サンプル")

![Knockout サンプル](media/Sandbox-Solution-Transformation-Guidance-InfoPath/knockoutsample.png)

- 
            [ASP.Net MVC SharePoint アドイン](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Samples/EmployeeRegistration.MVC "ASP.Net MVC を使用した SharePoint アドイン")
- 
            [ASP.Net フォーム SharePoint アドイン](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Samples/EmployeeRegistration.Forms "ASP.Net フォームを使用した SharePoint アドイン")

InfoPath フォームの変換を適切に支援できるように、**一般的な 11 通りの InfoPath コーディング パターンを一覧に示し、これらのパターンを上記の 3 つの SharePoint アドインオプションで実装する方法について説明しています**。 そのために、[参考用の InfoPath フォーム](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Reference/EmployeeRegistration "参考用 InfoPath フォーム")を最初に開発しました。このフォームでは最も共通する InfoPath コーディング パターンを使用しています。また、そのフォームを 3 種類の SharePoint アドインに移行しています。 次の各リンクは、これらの共通のパターンを示しています。
- 
            [フォームの読み込み時にフィールドを設定する - ユーザー情報の設定](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Populating%20fields%20on%20form%20load-set%20user%20information.md "フォームの読み込み時にフィールドを設定する - ユーザー情報の設定")
- 
            [フォームの読み込み時にフィールドを設定する - リスト情報の読み込み](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Populating%20fields%20on%20form%20load-read%20list%20information.md "フォームの読み込み時にフィールドを設定する - リスト情報の読み込み")
- 
            [フォームの読み込み時にフィールドを設定する - リスト データの読み込み](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Populating%20fields%20on%20form%20load-read%20list%20data.md "フォームの読み込み時にフィールドを設定する - リスト データの読み込み")
- 
            [コードによるフォームの送信](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Submit%20the%20form%20via%20code.md "コードによるフォームの送信")
- 
            [フォーム送信後のビューの切り替え](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Switching%20view%20after%20form%20submission.md "フォーム送信後のビューの切り替え")
- 
            [ユーザー データの取得](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Retrieving%20user%20data.md "ユーザー データの取得")
- 
            [データ コレクションの読み取りと複数のコントロールの設定](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Read%20data%20collection%20and%20set%20multiple%20controls.md "データ コレクションの読み取りと複数のコントロールの設定")
- 
            [カスケード データの読み込み](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Cascading%20data%20load.md "カスケード データの読み込み")
- 
            [添付ファイルの更新または削除](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Upload%20or%20Delete%20Attachments.md "添付ファイルの更新または削除")
- 
            [サイト グループに対するユーザーの追加または削除](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Add%20or%20remove%20user%20from%20site%20groups.md "サイト グループに対するユーザーの追加または削除")
- 
            [フォームに存在するアイテムの読み込み](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Load%20existing%20item%20in%20form.md "フォームに存在するアイテムの読み込み")


### InfoPath データの移行
InfoPath フォームを新しいソリューションに移行すると、InfoPath XML のデータを通常の SharePoint リスト データまたは任意のデータ層に移行することが必要になります。 InfoPath のファイルは XML ファイルであるため、読み取りと変換は非常に簡単です。 [PnP - 変換リポジトリには、これを実現可能にする方法を示すサンプルがあります](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Migration/EmpRegConsole "サンプルでは InfoPath XML からリスト データへの変換方法を示しています")。