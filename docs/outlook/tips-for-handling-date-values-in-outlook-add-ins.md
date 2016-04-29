
# Outlook アドインで日付値を扱うためのヒント
Outlook アドインが JavaScript API for Office のさまざまな日付関連メソッドで入力および出力日付値を処理する方法のヒントを説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 一般的な JavaScript の日付と時刻のサポート


JavaScript API for Office では、ほとんどの場合、JavaScript の [Date](http://www.w3schools.com/jsref/jsref_obj_date.asp) オブジェクトを使用して、日付と時刻の保存および取得を行います。この **Date** オブジェクトには、 [getUTCDate](http://www.w3schools.com/jsref/jsref_getutcdate.asp)、 [getUTCHour](http://www.w3schools.com/jsref/jsref_getutchours.asp)、 [getUTCMinutes](http://www.w3schools.com/jsref/jsref_getutcminutes.asp)、 [toUTCString](http://www.w3schools.com/jsref/jsref_toutcstring.asp) などのメソッドがあります。これらのメソッドは、要求された日付または時刻の値を協定世界時 (UTC) に従って返します。

また、 **Date** オブジェクトには、 [getDate](http://www.w3schools.com/jsref/jsref_getutcdate.asp)、 [getHour](http://www.w3schools.com/jsref/jsref_getutchours.asp)、 [getMinutes](http://www.w3schools.com/jsref/jsref_getminutes.asp)、 [toString](http://www.w3schools.com/jsref/jsref_tostring_date.asp) などのメソッドもあります。これらのメソッドは、要求された日付または時刻を "現地時刻" に従って返します。

"現地時刻" の概念は、主にクライアント コンピューター上のブラウザーおよびオペレーティング システムで決まります。たとえば、Windows ベースのクライアント コンピューター上で動作している大部分のブラウザーでは、JavaScript で  **getDate** を呼び出すと、クライアント コンピューター上の Windows に設定されているタイム ゾーンに基づく日付が返されます。

次の例では、 `myLocalDate` という名前の **Date** オブジェクトを現地時刻で作成し、 **toUTCString** を呼び出して、その日付を UTC の日付文字列に変換します。




```
// Create and get the current date represented 
// in the client computer time zone.
var myLocalDate = new Date (); 

// Convert the Date value in the client computer time zone
// to a date string in UTC, and display the string.
document.write ("The current UTC time is " + 
    myLocalDate.toUTCString());
```

JavaScript の  **Date** オブジェクトを使用して、UTC またはクライアント コンピューターのタイム ゾーンに基づく日付や時刻の値を取得することはできますが、 **Date** オブジェクトには次の点で制限があります。すなわち、他のタイム ゾーンを指定して日付や時刻の値を返すメソッドはありません。たとえば、東部標準時 (EST) に設定されているクライアント コンピューターの場合、太平洋標準時 (PST) など、EST または UTC 以外の時刻値を取得できる **Date** メソッドはありません。


## Outlook アドインの日付関連機能


前述の JavaScript の制限は、JavaScript API for Office を使用して、Outlook リッチ クライアントおよび Outlook Web App または デバイス用 OWA 上で動作する Outlook アドインの日付または時刻の値を処理する場合にも影響します。


### Outlook クライアントのタイム ゾーン

わかりやすくするため、問題のタイム ゾーンを定義します。



|**タイム ゾーン**|**説明**|
|:-----|:-----|
|クライアント コンピューターのタイム ゾーン|これは、クライアント コンピューターのオペレーティング システムで設定されています。ほとんどのブラウザーは、クライアント コンピューターのタイム ゾーンを使用して、JavaScript の  **Date** オブジェクトの日付または時刻の値を表示します。Outlook リッチ クライアントでは、このタイム ゾーンを使用して、ユーザー インターフェイスの日付または時刻の値を表示します。 たとえば、Windows を実行しているクライアント コンピューター上の Outlook では、Windows 上で設定されているタイム ゾーンをローカル タイム ゾーンとして使用します。Mac の場合、ユーザーがクライアント コンピューター上のタイム ゾーンを変更すると、Outlook for Mac によってタイム ゾーンを更新するように求めるメッセージが、Outlook と同様に表示されます。|
|Exchange 管理センター (EAC) のタイム ゾーン|ユーザーは、最初に Outlook Web App または デバイス用 OWA にログオンするとき、このタイム ゾーンの値 (および優先する言語) を設定します。 Outlook Web App および デバイス用 OWA では、このタイム ゾーンを使用して、ユーザー インターフェイスの日付または時刻の値を表示します。|
Outlook リッチ クライアントはクライアント コンピューターのタイム ゾーンを使用し、Outlook Web App と デバイス用 OWA のユーザー インターフェイスは EAC タイム ゾーンを使用するので、インストールされている同じ Outlook アドインの、同じメールボックスに対する現地時刻が、Outlook リッチ クライアントで実行しているときと、Outlook Web App または デバイス用 OWA で実行しているときとで、異なる場合があります。Outlook アドイン開発者としては、日付値の入出力を適切に行い、その値が、対応するクライアント上で期待されるタイム ゾーンに常に合っているようにする必要があります。


### 日付関連の API

日付関連機能をサポートする JavaScript API for Office のプロパティおよびメソッドを以下に示します。



|**API メンバー**|**タイム ゾーン表現**|**Outlook リッチ クライアントの例**|**Outlook Web App または デバイス用 OWA の例**|
|:-----|:-----|:-----|:-----|
|[Office.context.mailbox.userProfile.timeZone](../reference/outlook/Office.context.mailbox.userProfile.md%28Office.15%29.md)|Outlook リッチ クライアントでは、このプロパティはクライアント コンピューターのタイム ゾーンを返します。Outlook Web App および デバイス用 OWA では、このプロパティは EAC タイム ゾーンを返します。 |EST|PST|
|[Office.context.mailbox.item.dateTimeCreated](../reference/outlook/Office.context.mailbox.item.html%28Office.15%29.md) および [Office.context.mailbox.item.dateTimeModified](https://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.md%28Office.15%29.md)|これらのプロパティはどちらも、JavaScript の  **Date** オブジェクトを返します。この **Date** の値は正しい UTC 時刻です。すなわち、次の例で、 `myUTCDate` には、Outlook リッチ クライアントと Outlook Web App、およびデバイス用 OWA で同じ値が入ります。
```
var myDate = Office.mailbox.item.dateTimeCreated;
var myUTCDate = myDate.getUTCDate;
```

ただし、 `myDate.getDate` を呼び出すと、クライアント コンピューターのタイム ゾーンで日付値が返され。この値は、Outlook リッチ クライアントのインターフェイスで日時値を表示する際に使用されるタイム ゾーンと一致しますが、Outlook Web App および デバイス用 OWA のユーザー インターフェイスで使用される EAC タイム ゾーンとは異なる場合があります。|アイテムが午前 9 時 (UTC) に作成された場合:  `Office.mailbox.item.dateTimeCreated.getHours` は、午前 4 時 (EST) を返します。アイテムが午前 11 時 (UTC) に変更された場合:  `Office.mailbox.item.dateTimeModified.getHours` は、午前 6 時 (EST) を返します。|アイテムの作成時刻が午前 9 時 (UTC) の場合:  `Office.mailbox.item.dateTimeCreated.getHours` は、午前 4 時 (EST) を返します。アイテムが午前 11 時 (UTC) に変更された場合:  `Office.mailbox.item.dateTimeModified.getHours` は、午前 6 時 (EST) を返します。ユーザー インターフェイスで作成時刻または変更時刻を表示する場合、他のユーザー インターフェイスと合うように、最初に時刻を PST に変換するとよいでしょう。|
|[Office.context.mailbox.displayNewAppointmentForm](../reference/outlook/Office.context.mailbox.md%28Office.15%29.md)| _Start_ パラメーターおよび _End_ パラメーターにはそれぞれ、JavaScript の **Date** オブジェクトが必要です。引数は、Outlook リッチ クライアント、Outlook Web App、または デバイス用 OWA のユーザー インターフェイスで使用されているタイム ゾーンにかかわらず、正しい UTC である必要があります。|予定フォームの開始時刻と終了時刻が午前 9 時 (UTC) および午前 11 時 (UTC) の場合、 `start` および `end` の引数は、次に示すように、正しい UTC 時刻であることが必要です。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="code">start.getUTCHours</span> は午前 9 時 (UTC) を返します。</p></li><li><p><span class="code">end.getUTCHours</span> は午前 11 時 (UTC) を返します。</p></li></ul>|予定フォームの開始時刻と終了時刻が午前 9 時 (UTC) および午前 11 時 (UTC) の場合、 `start` および `end` の引数は、次に示すように、正しい UTC 時刻であることが必要です。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="code">start.getUTCHours</span> は午前 9 時 (UTC) を返します。</p></li><li><p><span class="code">end.getUTCHours</span> は午前 11 時 (UTC) を返します。</p></li></ul>|

## 日付関連のシナリオ向けのヘルパー メソッド


前のセクションで説明したように、Outlook Web App または デバイス用 OWA のユーザーの "現地時刻" は Outlook リッチ クライアント上で異なる場合があるのに対し、JavaScript の  **Date** オブジェクトではクライアント コンピューターのタイム ゾーンまたは UTC への変換のみサポートされるので、JavaScript API for Office には、 [Office.context.mailbox.convertToLocalClientTime](../reference/outlook/Office.context.mailbox.html%28Office.15%29.md) と [Office.context.mailbox.convertToUtcClientTime](https://dev.outlook.com/reference/add-ins/Office.context.mailbox.md%28Office.15%29.md) の 2 つのヘルパー メソッドがあります。

このヘルパー メソッドを使用すると、次の 2 つの日付関連のシナリオで、Outlook リッチ クライアント、Outlook Web App、および デバイス用 OWA で、日付または時刻に異なる処理が必要な場合に対処できます。つまり、このヘルパー メソッドは、アドインの異なるクライアントごとの "ライト ワンス" を強化するものです。


### シナリオ A: アイテムの作成時刻または変更時刻を表示する

ユーザー インターフェイスにアイテムの作成時刻 ( **Item.dateTimeCreated**) または変更時刻 ( **Item.dateTimeModified**) を表示している場合、まず  **convertToLocalClientTime** を使用して、これらのプロパティで提供される **Date** オブジェクトを変換し、適切な現地時刻の辞書表現を取得します。その後、辞書の日付部分を表示します。以下のコードは、このシナリオの例を示しています。


```
// This date is UTC-correct.
var myDate = Office.context.mailbox.item.dateTimeCreated;

// Call helper method to get date in dictionary format, 
// represented in the appropriate local time.
// In an Outlook rich client, this is dictionary format 
// in client computer time zone.
// In Outlook web app or OWA for Devices, this dictionary 
// format is in EAC time zone.
var myLocalDictionaryDate = Office.context.mailbox.convertToLocalClientTime(myDate);

// Display different parts of the dictionary date.
document.write ("The item was created at " + myLocalDictionaryDate["hours"] + 
    ":" + myLocalDictionaryDate["minutes"]);)
```

 **convertToLocalClientTime** では、Outlook リッチ クライアントと、Outlook Web App またはデバイス用 OWA の相違に、次のように対処します。


-  **convertToLocalClientTime** メソッドでは、現在のホストがリッチ クライアントであることを検出すると、 **Date** 表現を、同じクライアント コンピューター タイム ゾーンの辞書表現に変換し、他のリッチ クライアント ユーザー インターフェイスとの一貫性を保ちます。
    
-  **convertToLocalClientTime** メソッドでは、現在のホストが Outlook Web App または デバイス用 OWA であることを検出すると、正しい UTC の **Date** 表現を、EAC タイム ゾーンの辞書形式に変換し、他の Outlook Web App または デバイス用 OWA ユーザー インターフェイスとの一貫性を保ちます。
    

### シナリオ B: 新しい予定フォームの開始日付と終了日付を表示する

現地時刻で表された日付と時刻の値の異なる各部分を入力として取得していて、この辞書入力値を予定フォームの開始時刻または終了時刻として提供する場合、最初に、 **convertToUtcClientTime** ヘルパー メソッドを使用して、ディクショナリ値を正しい UTC の **Date** オブジェクトに変換します。

次の例で、 `myLocalDictionaryStartDate` および `myLocalDictionaryEndDate` は、ユーザーから取得した辞書形式の日付と時刻の値とします。これらの値は、ホスト アプリケーションに依存する、現地時刻に基づいています。




```
var myUTCCorrectStartDate = Office.context.mailbox.convertToUtcClientTime(myLocalDictionaryStartDate);
var myUTCCorrectEndDate = Office.context.mailbox.convertToUtcClientTime(myLocalDictionaryEndDate);

```

出力結果の値  `myUTCCorrectStartDate` および `myUTCCorrectEndDate` は、正しい UTC です。次に、これらの **Date** オブジェクトを **Mailbox.displayNewAppointmentForm** メソッドの _Start_ パラメーターおよび _End_ パラメーターの引数として渡し、新しい予定フォームを表示します。

 **convertToUtcClientTime** では、Outlook リッチ クライアントと、Outlook Web App またはデバイス用 OWA の相違に、次のように対処します。


-  **convertToUtcClientTime** では、現在のホストが Outlook リッチ クライアントであることを検出すると、単純に辞書表現を **Date** オブジェクトに変換します。この **Date** オブジェクトは、 **displayNewAppointmentForm** で想定される正しい UTC です。
    
-  **convertToUtcClientTime** メソッドでは、現在のホストが Outlook Web App または デバイス用 OWA であることを検出すると、EAC タイム ゾーンで表される辞書形式の日付と時刻の値を、 **Date** オブジェクトに変換します。この **Date** オブジェクトは、 **displayNewAppointmentForm** で想定される正しい UTC です。
    

## その他の技術情報



- [テスト用に Outlook アドインを展開してインストールする](../outlook/testing-and-tips.md)
    


