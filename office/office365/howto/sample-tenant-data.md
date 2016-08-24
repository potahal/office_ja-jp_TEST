---
ms.Toctitle: Office 365 sample tenant data
title: "Office 365 のサンプル テナント データ"
description: "対話型の REST API リファレンスと Office 365 API の API サンドボックスに関連付けられたサンプル ユーザーのメール、連絡先、カレンダー、およびドキュメントのデータを検索します。"
ms.ContentId: f14745a4-1763-43a4-bcc4-ffd9b2df92f6
ms.date: April 29, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 のサンプル テナント データ

_**適用対象:**Office 365_

一部の REST 操作に表示される対話型の [Try] ボタンを使用した場合、Office 365 ではサンプル データで作成されたライブ テナントに要求を送信します。


![REST API リファレンス内の対話型の [Try] ボタン。](images/SampleTenantDataTryButton.png)

[Office 365 API の API サンド ボックス](https://apisandbox.msdn.microsoft.com/)も同じテナントに接続します。サンプル テナント内のサンプル ユーザーに関連付けられているデータの一覧を次に示します。

##メール

|**フォルダー**|**電子メールの件名**|
|:-----|:-----|
|送信済みメール|会議ノート <br/> 契約の署名 <br/> Rob:Alex 1:1 <br/> 日次チーム会議|
|受信トレイ|会議ノート (添付ファイル付き、Alex D が以前に送信した電子メールへの返信) <br/> 明日のイベント - アトリウム閉鎖|

##連絡先

|**名前**|**エイリアス**|
|:-----|:-----|
|Garth Fort|garthf|
|Janet Schorr|janets|
|Katie Jordan ("Finance" 連絡先フォルダーでも重複しています)|katiej|
|Pavel Bansky|pavelb|
|Rob Young|roby|

##カレンダー
- タイム ゾーン:太平洋標準時

|**会議のタイトル**|**定期的なイベントまたは日付/時刻**|
|:-----|:-----|
|日次チーム会議|平日の毎日、午前 11 時 00 分～ 11 時 30 分|
|Contoso プロジェクトの週次ミーティング|毎週月曜日、午後 2 時 00 分|
|Rob:Alex 1:1|毎月第 3 水曜日、午前 9 時 30 分|
|感謝祭の祝日|2014 年 11 月 27 日、終日|
|クリスマスの祝日|2014 年 12 月 25 日、終日|
|元日の祝日|2015 年 1 月 1 日、終日|
|感謝祭の祝日|2015 年 11 月 26 日、終日|


##ドキュメント

|**フォルダー**|**ファイル**|
|:-----|:-----|
|ルート フォルダー|Ad Goals Presentation.pptx <br/> Ad Goals.docx <br/> Annual Party Planning.docx <br/> Ideas for XT1000 Series.docx <br/> Marketing.pptx <br/> Meeting Notes.docx <br/> Proposal for Ad Campaign.docx <br/> Quick notes.txt <br/> Shopping List.xlsx <br/> Slogan Suggestions.docx <br/> Timesheet_Alexd.xlsx <br/> XT1000 Series.pptx |
|全員と共有|Denver Data.xlsx|


##ユーザーとグループ


Office 365 テナントのサンプルには、50 人のユーザーが定義されています。各ユーザーは、サンプル データ内に定義された 4 つのグループのうち 2 つのグループに属しています。 従業員またはマネージャー、およびマーケティングまたは営業。Josephine McNeil (demo1) を除く、すべての従業員にマネージャーがいます。

| **givenName** | **surname** | **UPN** | **性別** | **telephoneNumber** | **マネージャー** | **グループ**  | **グループ** | **グループ** |
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
| Josephine | McNeil | demo1 | 女 | 4255550100 |  | マネージャー |  |  |
| Kelli | Leach | demo2 | 女 | 4255550100 | Autumn Webster | 従業員 | マーケティング |  |
| Tamika | Carroll | demo3 | 女 | 4255550100 | Callie Dillard | 従業員 | マーケティング |  |
| Gilbert | Shelly | demo4 | 男 | 4255550100 | Zelma Huff | 従業員 | 営業 |  |
| Elisabeth | Dunn | demo5 | 女 | 4255550100 | Diana Hewitt | 従業員 | 営業 |  |
| Penelope | Logan | demo6 | 女 | 4255550100 | Greg Toscano | 従業員 | 営業 |  |
| Donovan | Carbajal | demo7 | 男 | 4255550100 | Greg Toscano | 従業員 | 営業 |  |
| Sallie | Payne | demo8 | 女 | 4255550100 | Donna Sweet | 従業員 | 営業 |  |
| Evangelina | Higgins | demo9 | 女 | 4255550100 | Donna Sweet | 従業員 | 営業 |  |
| Chasity | Mullins | demo10 | 女 | 4255550100 | Laura Brennan | 従業員 | マーケティング |  |
| Carlton | Fiore | demo11 | 男 | 4255550100 | Laura Brennan |  | マーケティング |  |
| Autumn | Webster | demo12 | 女 | 4255550100 | Laura Brennan | マネージャー | マーケティング |  |
| Emmanuel | Wolcott | demo13 | 男 | 4255550100 | Laura Brennan | 従業員 | マーケティング |  |
| Callie | Dillard | demo14 | 女 | 4255550100 | Laura Brennan | マネージャー | マーケティング |  |
| Darlene | Hale | demo15 | 女 | 4255550100 | Betty Carroll | 従業員 | マーケティング |  |
| Evelyn | Watts | demo16 | 女 | 4255550100 | Betty Carroll | マネージャー | マーケティング |  |
| Gail | Steele | demo17 | 女 | 4255550100 | Betty Carroll | 従業員 | マーケティング |  |
| Theodore | Coppola | demo18 | 男 | 4255550100 | Betty Carroll | 従業員 | マーケティング |  |
| Bennie | Coggins | demo19 | 男 | 4255550100 | Betty Carroll | 従業員 | マーケティング |  |
| Laura | Bishop | demo20 | 女 | 4255550100 | Autumn Webster | 従業員 | マーケティング |  |
| Alexander | Yoo | demo21 | 男 | 4255550100 | Autumn Webster | マネージャー | マーケティング |  |
| Chasity | Salinas | demo22 | 女 | 4255550100 | Autumn Webster | 従業員 | マーケティング |  |
| Al | Mota | demo23 | 男 | 4255550100 | Autumn Webster | 従業員 | マーケティング |  |
| Zelma | Huff | demo24 | 女 | 4255550100 | Autumn Webster | マネージャー | マーケティング |  |
| Dane | Clemente | demo25 | 男 | 4255550100 | Autumn Webster | 従業員 | マーケティング |  |
| Donna | Sweet | demo26 | 女 | 4255550100 | Autumn Webster | マネージャー | マーケティング |  |
| Sheila | Griffin | demo27 | 女 | 4255550100 | Callie Dillard | マネージャー | マーケティング |  |
| Beth | Hardy | demo28 | 女 | 4255550100 | Callie Dillard | 従業員 | マーケティング |  |
| Elsa | Hansen | demo29 | 女 | 4255550100 | Callie Dillard | 従業員 | マーケティング |  |
| Laura | Brennan | demo30 | 女 | 4255550100 | Josephine McNeil | マネージャー | マーケティング | 従業員 |
| Betty | Carroll | demo31 | 女 | 4255550100 | Josephine McNeil | マネージャー | マーケティング | 従業員 |
| Jan | Madden | demo32 | 女 | 4255550100 | Evelyn Watts | 従業員 | マーケティング |  |
| Lorena | Bird | demo33 | 女 | 4255550100 | Evelyn Watts | 従業員 | 営業 |  |
| Ada | Lang | demo34 | 女 | 4255550100 | Evelyn Watts | 従業員 | 営業 |  |
| Katelyn | Nielsen | demo35 | 女 | 4255550100 | Alexander Yoo | 従業員 | 営業 |  |
| Jillian | Gonzalez | demo36 | 女 | 4255550100 | Alexander Yoo | 従業員 | 営業 |  |
| Joe | Swearingen | demo37 | 男 | 4255550100 | Alexander Yoo | 従業員 | 営業 |  |
| Mitchell | Rodman | demo38 | 男 | 4255550100 | Zelma Huff | 従業員 | 営業 |  |
| Ester | O'Neil | demo39 | 女 | 4255550100 | Zelma Huff | 従業員 | 営業 |  |
| Vicky | Booth | demo40 | 女 | 4255550100 | Zelma Huff | マネージャー | 営業 |  |
| Diana | Hewitt | demo41 | 女 | 4255550100 | Sheila Griffin | マネージャー | 営業 |  |
| Eileen | Klein | demo42 | 女 | 4255550100 | Sheila Griffin | 従業員 | 営業 |  |
| Greg | Toscano | demo43 | 男 | 4255550100 | Sheila Griffin | マネージャー | 営業 |  |
| Keisha | Stout | demo44 | 女 | 4255550100 | Vicky Booth | 従業員 | 営業 |  |
| Bettie | Santana | demo45 | 女 | 4255550100 | Vicky Booth | 従業員 | 営業 |  |
| Evelyn | Rodgers | demo46 | 女 | 4255550100 | Vicky Booth | 従業員 | 営業 |  |
| Imelda | Miranda | demo47 | 女 | 4255550100 | Vicky Booth | 従業員 | 営業 |  |
| Cathleen | Silva | demo48 | 女 | 4255550100 | Diana Hewitt | 従業員 | 営業 |  |
| Tami | Glover | demo49 | 女 | 4255550100 | Diana Hewitt | 従業員 | 営業 |  |
| Lynne  | Deleon | demo50 | 女 | 4255550100 | Greg Toscano | 従業員 | 営業 |  | |



### マネージャー別の従業員

| **マネージャー** | **ManagerUPN** | **名前** | **UPN** |
|:-----|:-----|:-----|:-----|
|  |  | Josephine McNeil | demo01 |
| Josephine McNeil | demo1 | Laura Brennan | demo30 |
|  |  | Betty Carroll | demo31 |
| Autumn Webster | demo12 | Kelli Leach | demo02 |
|  |  | Laura Bishop | demo20 |
|  |  | Alexander Yoo | demo21 |
|  |  | Chasity Salinas | demo22 |
|  |  | Al Mota | demo23 |
|  |  | Zelma Huff | demo24 |
|  |  | Dane Clemente | demo25 |
|  |  | Donna Sweet | demo26 |
| Callie Dillard | demo14 | Tamika Carroll | demo03 |
|  |  | Sheila Griffin | demo27 |
|  |  | Beth Hardy | demo28 |
|  |  | Elsa Hansen | demo29 |
| Evelyn Watts | demo16 | Jan Madden | demo32 |
|  |  | Lorena Bird | demo33 |
|  |  | Ada Lang | demo34 |
| Alexander Yoo | demo21 | Katelyn Nielsen | demo35 |
|  |  | Jillian Gonzalez | demo36 |
|  |  | Joe Swearingen | demo37 |
| Zelma Huff | demo24 | Gilbert Shelly | demo04 |
|  |  | Mitchell Rodman | demo38 |
|  |  | Ester O'Neil | demo39 |
|  |  | Vicky Booth | demo40 |
| Donna Sweet | demo26 | Sallie Payne | demo08 |
|  |  | Evangelina Higgins | demo09 |
| Sheila Griffin | demo27 | Diana Hewitt | demo41 |
|  |  | Eileen Klein | demo42 |
|  |  | Greg Toscano | demo43 |
| Laura Brennan | demo30 | Chasity Mullins | demo10 |
|  |  | Carlton Fiore | demo11 |
|  |  | Autumn Webster | demo12 |
|  |  | Emmanuel Wolcott | demo13 |
|  |  | Callie Dillard | demo14 |
| Betty Carroll | demo31 | Darlene Hale | demo15 |
|  |  | Evelyn Watts | demo16 |
|  |  | Gail Steele | demo17 |
|  |  | Theodore Coppola | demo18 |
|   |  | Bennie Coggins | demo19 |
| Vicky Booth | demo40 | Keisha Stout | demo44 |
|   |  | Bettie Santana | demo45 |
|   |  | Evelyn Rodgers | demo46 |
|   |  | Imelda Miranda | demo47 |
| Diana Hewitt | demo41 | Elisabeth Dunn | demo05 |
|   |  | Cathleen Silva | demo48 |
|   |  | Tami Glover | demo49 |
| Greg Toscano | demo43 | Penelope Logan | demo06 |
|   |  | Donovan Carbajal | demo07 |
|   |  | Lynne Deleon | demo50 |


### グループ別の従業員

| **グループ**  | **説明** | **givenName** | **surname** | **UPN** |
|:-----|:-----|:-----|:-----|:-----|
| 従業員 | 管理者ではないすべてのユーザー | Kelli | Leach | demo2 |
|  |  | Tamika | Carroll | demo3 |
|  |  | Gilbert | Shelly | demo4 |
|  |  | Elisabeth | Dunn | demo5 | 
|  |  | Penelope | Logan | demo6 |
|  |  | Donovan | Carbajal | demo7 |
|  |  | Sallie | Payne | demo8 |
|  |  | Evangelina | Higgins | demo9 |
|  |  | Chasity | Mullins | demo10 |
|  |  | Emmanuel | Wolcott | demo13 |
|  |  | Darlene | Hale | demo15 | 
|  |  | Gail | Steele | demo17 |
|  |  | Theodore | Coppola | demo18 |
|  |  | Bennie | Coggins | demo19 |
|  |  | Laura | Bishop | demo20 |
|  |  | Chasity | Salinas | demo22 |
|  |  | Al | Mota | demo23 |
|  |  | Dane | Clemente | demo25 |
|  |  | Beth | Hardy | demo28 |
|  |  | Elsa | Hansen | demo29 |
|  |  | Laura | Brennan | demo30 |
|  |  | Betty | Carroll | demo31 |
|  |  | Jan | Madden | demo32 |
|  |  | Lorena | Bird | demo33 |
|  |  | Ada | Lang | demo34 |
|  |  | Katelyn | Nielsen | demo35 |
|  |  | Jillian | Gonzalez | demo36 |
|  |  | Joe | Swearingen | demo37 |
|  |  | Mitchell | Rodman | demo38 |
|  |  | Ester | O'Neil | demo39 |
|  |  | Eileen | Klein | demo42 |
|  |  | Keisha | Stout | demo44 |
|  |  | Bettie | Santana | demo45 |
|  |  | Evelyn | Rodgers | demo46 |
|  |  | Imelda | Miranda | demo47 |
|  |  | Cathleen | Silva | demo48 |
|  |  | Tami | Glover | demo49 |
|  |  | Lynne  | Deleon | demo50 |
| マネージャー | マネージャーではないすべてのユーザー | Josephine | McNeil | demo1 |
|  |  | Autumn | Webster | demo12 |
|  |  | Callie | Dillard | demo14 |
|  |  | Evelyn | Watts | demo16 |
|  |  | Alexander | Yoo | demo21 |
|  |  | Zelma | Huff | demo24 |
|  |  | Donna | Sweet | demo26 |
|  |  | Sheila | Griffin | demo27 |
|  |  | Laura | Brennan | demo30 |
|  |  | Betty | Carroll | demo31 |
|  |  | Vicky | Booth | demo40 |
|  |  | Diana | Hewitt | demo41 |
|  |  | Greg | Toscano | demo43 |
| マーケティング | マーケティング部門 | Kelli | Leach | demo2 |
|  |  | Tamika | Carroll | demo3 |
|  |  | Chasity | Mullins | demo10 |
|  |  | Carlton | Fiore | demo11 |
|  |  | Autumn | Webster | demo12 |
|  |  | Emmanuel | Wolcott | demo13 |
|  |  | Callie | Dillard | demo14 |
|  |  | Darlene | Hale | demo15 |
|  |  | Evelyn | Watts | demo16
|  |  | Gail | Steele | demo17 |
|  |  | Theodore | Coppola | demo18 |
|  |  | Bennie | Coggins | demo19 |
|  |  | Laura | Bishop | demo20 |
|  |  | Alexander | Yoo | demo21 |
|  |  | Chasity | Salinas | demo22 |
|  |  | Al | Mota | demo23 |
|  |  | Zelma | Huff | demo24 |
|  |  | Dane | Clemente | demo25 |
|  |  | Donna | Sweet | demo26 |
|  |  | Sheila | Griffin | demo27 |
|  |  | Beth | Hardy | demo28 |
|  |  | Elsa | Hansen | demo29 |
|  |  | Laura | Brennan | demo30 |
|  |  | Betty | Carroll | demo31 |
|  |  | Jan | Madden | demo32 |
| 営業 | 営業部 | Gilbert | Shelly | demo4 |
|  |  | Elisabeth | Dunn | demo5 |
|  |  | Penelope | Logan | demo6 |
|  |  | Donovan | Carbajal | demo7 |
|  |  | Sallie | Payne | demo8 |
|  |  | Evangelina | Higgins | demo9 |
|  |  | Lorena | Bird | demo33 | 
|  |  | Ada | Lang | demo34 |
|  |  | Katelyn | Nielsen | demo35 |
|  |  | Jillian | Gonzalez | demo36 |
|  |  | Joe | Swearingen | demo37 |
|  |  | Mitchell | Rodman | demo38 |
|  |  | Ester | O'Neil | demo39 |
|  |  | Vicky | Booth | demo40 |
|  |  | Diana | Hewitt | demo41 |
|  |  | Eileen | Klein | demo42 |
|  |  | Greg | Toscano | demo43 |
|  |  | Keisha | Stout | demo44 |
|  |  | Bettie | Santana | demo45 |
|  |  | Evelyn | Rodgers | demo46 |
|  |  | Imelda | Miranda | demo47 |
|  |  | Cathleen | Silva | demo48 |
|  |  | Tami | Glover | demo49 | 
|  |  | Lynne  | Deleon | demo50 |
| すべてのユーザー | 会社のすべてのユーザー | マネージャー | 従業員 | demo51 | 



