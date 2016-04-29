
# LabsJS.Labs.Components
Labs.JS Labs.Components JavaScript API の概要について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Office Mix?| PowerPoint_

Labs.Components モジュール内の API は、現在ラボの開発用に使うことのできる 4 つの既定のコンポーネント (アクティビティ、選択、入力、動的コンポーネント) を表します。

## Labs.Components モジュール

Labs.Components の種類を次に示します。


### クラス


|||
|:-----|:-----|
|[Labs.Components.ComponentAttempt](../../reference/office-mix/labs.components.componentattempt.md)|コンポーネントでの試行の基本クラス。|
|[Labs.Components.ActivityComponentAttempt](../../reference/office-mix/labs.components.activitycomponentattempt.md)|アクティビティ コンポーネントの完了の試行を表す。|
|[Labs.Components.ActivityComponentInstance](../../../reference/office-mix/labs.components.activitycomponentinstance.md)|アクティビティ コンポーネントの現在のインスタンスを表す。|
|[Labs.Components.ChoiceComponentAnswer](../../reference/office-mix/labs.components.choicecomponentanswer.md)|選択コンポーネントで発生した問題への回答。|
|[Labs.Components.ChoiceComponentAttempt](../../reference/office-mix/labs.components.choicecomponentattempt.md)|選択コンポーネントでの試行を表す。|
|[Labs.Components.ChoiceComponentInstance](../../../reference/office-mix/labs.components.choicecomponentinstance.md)|選択コンポーネントのインスタンスを表す。|
|[Labs.Components.ChoiceComponentResult](../../reference/office-mix/labs.components.choicecomponentresult.md)|選択コンポーネントの送信の結果。|
|[Labs.Components.ChoiceComponentSubmission](../../reference/office-mix/labs.components.choicecomponentsubmission.md)|選択コンポーネントに関連付けられている送信を表す。|
|[Labs.Components.DynamicComponentInstance](../../../reference/office-mix/labs.components.dynamiccomponentinstance.md)|動的コンポーネントのインスタンスを表す。|
|[Labs.Components.InputComponentAnswer](../../reference/office-mix/labs.components.inputcomponentanswer.md)|入力コンポーネントの問題への回答を表す。|
|[Labs.Components.InputComponentAttempt](../../reference/office-mix/labs.components.inputcomponentattempt.md)|入力コンポーネントとの対話の試行を表す。|
|[Labs.Components.InputComponentInstance](../../../reference/office-mix/labs.components.inputcomponentinstance.md)|入力コンポーネントのインスタンスを表す。|
|[Labs.Components.InputComponentResult](../../reference/office-mix/labs.components.inputcomponentresult.md)|入力コンポーネントの送信の結果。|
|[Labs.Components.InputComponentSubmission](../../reference/office-mix/labs.components.inputcomponentsubmission.md)|入力コンポーネントへの送信を表す。|

### インターフェイス


|||
|:-----|:-----|
|[Labs.Components.IActivityComponent](../../reference/office-mix/labs.components.iactivitycomponent.md)|アクティビティ コンポーネントを表す。 [Labs.Core.IComponent](../../reference/office-mix/labs.core.icomponent.md) を拡張する。|
|[Labs.Components.IActivityComponentInstance](../../reference/office-mix/labs.components.iactivitycomponentinstance.md)|アクティビティ コンポーネントの特定のインスタンスを表す。 [Labs.Core.IComponentInstance](../../reference/office-mix/labs.core.icomponentinstance.md) を拡張する。|
|[Labs.Components.IChoice](../../reference/office-mix/labs.components.ichoice.md)|特定の問題に対して利用可能な選択肢。|
|[Labs.Components.IChoiceComponent](../../reference/office-mix/labs.components.ichoicecomponent.md)|選択コンポーネントとの対話を有効にする。|
|[Labs.Components.IChoiceComponentInstance](../../reference/office-mix/labs.components.ichoicecomponentinstance.md)|選択コンポーネントのインスタンス。|
|[Labs.Components.IDynamicComponent](../../reference/office-mix/labs.components.idynamiccomponent.md)|動的コンポーネントとの対話を有効にする。|
|[Labs.Components.IDynamicComponentInstance](../../reference/office-mix/labs.components.idynamiccomponentinstance.md)|動的コンポーネントのインスタンス。|
|[Labs.Components.IHint](../../reference/office-mix/labs.components.ihint.md)|ラボの問題のヒント。|
|[Labs.Components.IInputComponent](../../reference/office-mix/labs.components.iinputcomponent.md)|入力コンポーネントとの対話を有効にする。|
|[Labs.Components.IInputComponentInstance](../../reference/office-mix/labs.components.iinputcomponentinstance.md)|入力コンポーネントのインスタンス。|
