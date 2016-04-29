
# LabsJS.Labs
LabsJS.Labs モジュール内の API の概要について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Office Mix?| PowerPoint_

LabsJS.Labs モジュールには、Office アドイン (ラボ) の作成に使うことのできる主要 JavaScript API のセットが含まれます。API は、ラボの開発用のエントリ ポイントを提供します。

## LabsJS.Labs API モジュール

ラボ モジュールには次の種類が含まれます。


### 変数


|||
|:-----|:-----|
|[Labs.DefaultHostBuilder](../../reference/office-mix/labs.defaulthostbuilder.md)|このオブジェクトを使って、既定の [Labs.Core.ILabHost](../../reference/office-mix/labs.core.ilabhost.md) インスタンスを生成します。|

### 関数


|||
|:-----|:-----|
|[Labs.Connect](../../reference/office-mix/labs.connect.md)|ホストとの接続を初期化します。|
|[Labs.connect (overload)](../../reference/office-mix/labs.connect-overload.md)|ホストとの接続を初期化し、入力パラメーターを提供します。|
|[Labs.isConnected](../../reference/office-mix/labs.isconnected.md)|ホストとの接続を初期化します。|
|[Labs.getConnectionInfo](../../reference/office-mix/labs.getconnectioninfo.md)|指定された接続に関連付けられている構成情報を取得します。|
|[Labs.disconnect](../../reference/office-mix/labs.disconnect.md)|ホストからラボを切断し、ラボの完了の状態を提供します。|
|[Labs.editLab](../../reference/office-mix/labs.editlab.md)|指定されたラボを編集用に開きます。編集モードのままラボの構成データを指定できます。ただし、実行中にラボを編集することはできません。|
|[Labs.takeLab](../../../reference/office-mix/labs.takelab.md)|指定されたラボを実行し、サーバーへのラボの結果の送信を有効にします。編集中にラボを実行することはできないことに注意してください。|
|[Labs.on](../../reference/office-mix/labs.on.md)|指定されたイベント用の新しいハンドラーを追加します。|
|[Labs.off](../../reference/office-mix/labs.off.md)|指定されたイベントのイベント ハンドラーを削除します。|
|[Labs.getTimeline](../../reference/office-mix/labs.gettimeline.md)|ホスト プレーヤー コントロールを制御するのに使用できる [Labs.Timeline](../../reference/office-mix/labs.timeline.md) オブジェクト インスタンスを取得します。|
|[Labs.registerDeserializer](../../reference/office-mix/labs.registerdeserializer.md)|指定された JSON オブジェクトをオブジェクトに逆シリアル化します。使用できるのは、コンポーネントの作成者のみです。|

### クラス


|||
|:-----|:-----|
|[Labs.ComponentInstanceBase](../../../reference/office-mix/labs.componentinstancebase.md)|コンポーネント インスタンスの初期化用の基本クラスです。|
|[Labs.ComponentInstance](../../../reference/office-mix/labs.componentinstance.md)|実行時にユーザーに指定されるコンポーネントのインスタンス化である、コンポーネントのインスタンスを表します。オブジェクトには、ラボの特定の実行のためのコンポーネントの翻訳されたビューが含まれます。|
|[Labs.Command](../../reference/office-mix/labs.command.md)|クライアントとホスト間でメッセージを渡すのに使用される一般的なコマンドです。|
|||
|[Labs.LabEditor](../../../reference/office-mix/labs.labeditor.md)|**LabEditor** オブジェクトを使うと、指定されたラボの編集に加えて、ラボに関連付けられている構成データを取得し、設定できます。|
|[Labs.LabInstance](../../../reference/office-mix/labs.labinstance.md)|現在のユーザーに対して構成されているラボのインスタンスです。このオブジェクトを使用してユーザー用にラボのデータを記録し、取得します。|
|||
|||
|[Labs.Timeline](../../reference/office-mix/labs.timeline.md)|Labs.js タイムライン機能へのアクセスを提供します。|
|[Labs.ValueHolder](../../reference/office-mix/labs.valueholder.md)|指定されたラボの値を保持し、追跡するコンテナ オブジェクトです。値は、ローカルまたはサーバーに格納できます。|

### インターフェイス


|||
|:-----|:-----|
|[Labs.GetActionsCommandData](../../reference/office-mix/labs.getactionscommanddata.md)|[LabsJS.Labs.Core.GetActions](../../reference/office-mix/labsjs.labs.core.getactions.md) コマンドに関連付けられているデータを取得できるようにします。|
|[Labs.IMessageHandler](../../reference/office-mix/labs.imessagehandler.md)|イベント ハンドラーを定義できるようにするインターフェイスです。|
|[Labs.ITimelineNextMessage](../../reference/office-mix/labs.itimelinenextmessage.md)|[Labs.Core.IMessage](https://msdn.microsoft.com/library/office/mt599680.aspx) オブジェクトを操作する方法を提供します。|
|[Labs.SendMessageCommandData](../../reference/office-mix/labs.sendmessagecommanddata.md)|[Labs.CommandType.TakeAction](https://msdn.microsoft.com/library/office/mt599680.aspx) コマンドに関連付けられているデータです。|
|[Labs.TakeActionCommandData](../../reference/office-mix/labs.takeactioncommanddata.md)|操作の実行コマンドに関連付けられているデータです。|

### 列挙体


|||
|:-----|:-----|
|[Labs.ConnectionState](../../reference/office-mix/labs.connectionstate.md)|ホストするラボの接続状態を列挙します。|
|[Labs.ProblemState](../../reference/office-mix/labs.problemstate.md)||
