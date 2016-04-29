
# LabsJS JavaScript API リファレンス
LabsJS JavaScript オブジェクト モデルの概要について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Office Mix?| PowerPoint_

LabsJS リファレンスは、 [TypeScript](http://www.typescriptlang.org/) ファイル **labs-1.0.42.d.ts** を文書化したものです。このファイルでは、LabsJS オブジェクト モデルがモジュールに分類されています。

## LabsJS オブジェクト モデル

LabsJS オブジェクト モデルは次の 5 つのモジュールにまとめられています。


- [LabsJS.Labs](../../reference/office-mix/labsjs.labs.md)。ラボ モジュールには、ラボそのものを作成するための主要 API のセットが含まれています。これらの API がラボの開発のエントリ ポイントになります。
    
- [LabsJS.Labs.Core](../../reference/office-mix/labsjs.labs.core.md)。LabsJS とプレゼンテーション ドライバー (この場合は Office Mix) の間で共有され、この 2 者間の橋渡しをする、中心的なインターフェイス、データ構造、およびクラス。
    
- [LabsJS.Labs.Core.Actions](../../reference/office-mix/labsjs.labs.core.actions.md)。この API は、ラボの操作を表し、その現在の動作を示します。(既定のコンポーネント以外の) 新しいコンポーネントを作成する開発者や、(Office Mix 以外の) 新しいドライバーを使用した接続を開発する開発者にとって有用です。
    
- [LabsJS.Labs.Core.GetActions](../../reference/office-mix/labsjs.labs.core.getactions.md)。この API を使用すると、サーバーで以前発生した操作についてのクエリを実行できます。
    
- [LabsJS.Labs.Components](../../reference/office-mix/labsjs.labs.components.md)。この API は、現在ラボで使用可能な 4 つの既定のコンポーネント (アクティビティ、選択、入力、および動的) を表します。
    
各モジュールには、次のメンバーの種類のうちの 1 つ以上からなるメンバーのセットが含まれています。


- クラス
    
- インターフェイス
    
- 関数
    
- 列挙体
    
- 変数
    



## その他の技術情報



- [TypeScript](http://www.typescriptlang.org/)
    
