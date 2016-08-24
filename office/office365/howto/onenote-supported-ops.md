---
ms.Toctitle: Supported operations
title: "サポート対象の REST 操作" 
description: "OneNote API でサポートされているすべての REST 操作の一覧を示します。"
ms.ContentId: 55d9973a-316a-4ccf-8c47-59a36ad7b4f1
ms.date: April 5, 2015
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

<style>#operation {margin:5px 0px 0px 10px;} #description {margin:-11px 0px 5px 50px} .small {font-size:90%;}</style>


# サポート対象の REST 操作

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

この記事では、OneNote API で使用できる REST 操作の一覧を示します。操作をクリックして、[対話型の Apigee コンソール](../howto/onenote-landing.md#console)から自分の所有する OneDrive のコンシューマー ノートブックで、その操作を試してください。

<a name="pages"></a>
## ページの操作

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/pages`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getpages%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">すべてのページを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/pages?search`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22searchpages%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">ページを検索します。 <span class="small">(*コンシューマー向け OneDrive のみ*)</span></p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sections/{id}/pages`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getsectionpages%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のセクションに含まれるすべてのページを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sections/{id}/pages?pagelevel`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getpagelevelandorder%22%2C%22params%22%3A%7B%22query%22%3A%7B%22pagelevel%22%3A%22true%22%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">セクションに含まれるページの順序とインデント レベルを取得します。 [`GET /pages/{id}?pagelevel`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getpage%22%2C%22params%22%3A%7B%22query%22%3A%7B%22pagelevel%22%3A%22true%22%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22undefined%22%3A%22%5Cu0001%5Cu0001%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D) もサポートされます。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/pages/{id}`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getpage%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のページを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/pages/{id}/preview`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getpagepreview%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22application%2Fjson%22%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のページのテキストとイメージのプレビュー コンテンツを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/pages/{id}/content`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getpagecontent%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Accept%22%3A%22text%2Fhtml%22%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のページの HTML コンテンツを取得します。</p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/pages`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createpage%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22multipart%2Fform-data%3B%20boundary%3DNewPart%22%7D%2C%22body%22%3A%7B%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">既定のセクションにページを作成します。 <span class="small">(*OneDrive の個人用ノートブックまたは OneDrive for Business のみ*)</span></p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/pages?sectionName`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createpage%22%2C%22params%22%3A%7B%22query%22%3A%7B%22sectionName%22%3A%22name%22%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22multipart%2Fform-data%3B%20boundary%3DNewPart%22%7D%2C%22body%22%3A%7B%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">既定のノートブックの名前付きセクションにページを作成します。 <span class="small">(*OneDrive の個人用ノートブックまたは OneDrive for Business のみ*)</span></p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/sections/{id}/pages`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createpageinsection%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22multipart%2Fform-data%3B%20boundary%3DNewPart%22%7D%2C%22body%22%3A%7B%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">特定のセクションにページを作成します。</p>

<p id="operation">![POST](images\onenote\post.png)&nbsp;`/pages/{id}/copyToSection`</p>
<p id="description">セクションにページをコピーします。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![PATCH](images\onenote\patch.png)&nbsp;`/pages/{id}/content`</p>
<p id="description">ページの HTML コンテンツを更新します。</p><!--POST /pages/{id}/patchcontent is also supported.-->

<p id="operation">[![DELETE](images\onenote\delete.png)&nbsp;`/pages/{id}`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22deletepage%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22delete%22%7D)</p>
<p id="description">特定のページを削除します。</p>
<p id="description">**警告** OneNote API を使用したページの削除は永続的です。 削除したページは復元できません。</p>

<br />
[*GET* 要求](../howto/onenote-get-content.md)の詳細 (サポートされるクエリ文字列のオプションを含む) と、[ページの作成](../howto/onenote-create-page.md)方法、[ページ コンテンツの更新](../howto/onenote-update-page.md)方法、および [ページのコピー](../howto/onenote-copy.md)方法を習得してください。

<a name="sections"></a>
##セクションの操作

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sections`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getallsections%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">すべてのセクションを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/notebooks/{id}/sections`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getnotebooksections%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のノートブックに含まれるすべてのセクションを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sectionGroups/{id}/sections`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getsectiongroupssections%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のセクション グループに含まれるすべてのセクションを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sections/{id}`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getsection%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のセクションを取得します。</p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/notebooks/{id}/sections`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createsectioninnotebook%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22application%2Fjson%22%7D%2C%22body%22%3A%7B%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">特定のノートブックにセクションを作成します。</p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/sectionGroups/{id}/sections`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createsectioninsectiongroup%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22application%2Fjson%22%7D%2C%22body%22%3A%7B%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">特定のセクション グループにセクションを作成します。</p>

<p id="operation">![PATCH](images\onenote\patch.png)&nbsp;`/sections/{id}`</p>
<p id="description">セクションの名前を変更します。 *application/json* コンテンツ タイプを使用して、新しい名前をメッセージ本文で送信します。例: `{ "name": "New section name" }`</p>

<p id="operation">![POST](images\onenote\post.png)&nbsp;`/sections/{id}/copyToNotebook`</p>
<p id="description">ノートブックにセクションをコピーします。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![POST](images\onenote\post.png)&nbsp;`/sections/{id}/copyToSectionGroup`</p>
<p id="description">セクション グループにセクションをコピーします。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![GET](images\onenote\get.png)&nbsp;`/sections/{id}/permissions`</p>
<p id="description">セクションの[アクセス許可](../howto/onenote-manage-perms.md)を取得します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![GET](images\onenote\get.png)&nbsp;`/sections/{id}/permissions/{id}`</p>
<p id="description">セクションの特定の[アクセス許可](../howto/onenote-manage-perms.md)を取得します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![POST](images\onenote\post.png)&nbsp;`/sections/{id}/permissions`</p>
<p id="description">セクションの[アクセス許可](../howto/onenote-manage-perms.md)を作成または更新します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![DELETE](images\onenote\delete.png)&nbsp;`/sections/{id}/permissions/{id}`</p>
<p id="description">セクションの[アクセス許可](../howto/onenote-manage-perms.md)を削除します。 <span class="small">(*Office 365 のみ*)</span></p>

<br />
[*GET* 要求](../howto/onenote-get-content.md)の詳細 (サポートされるクエリ文字列のオプションを含む) と、[セクションのコピー](../howto/onenote-copy.md)方法を習得してください。


<a name="section-groups"></a>
##セクション グループの操作

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sectionGroups`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getallsectiongroups%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">すべてのセクション グループを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/notebooks/{id}/sectionGroups`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getnotebooksectiongroups%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のノートブックに含まれるすべてのセクション グループを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sectionGroups/{id}/sectionGroups`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getsectiongroupssectiongroups%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のセクション グループに含まれるすべてのセクション グループを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/sectionGroups/{id}`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getsectiongroup%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のセクション グループを取得します。</p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/notebooks/{id}/sectionGroups`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createsectiongroupinnotebook%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22application%2Fjson%22%7D%2C%22body%22%3A%7B%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">特定のノートブックにセクション グループを作成します。</p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/sectionGroups/{id}/sectionGroups`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createsectiongroupinsectiongroup%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22application%2Fjson%22%7D%2C%22body%22%3A%7B%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">特定のセクション グループにセクション グループを作成します。</p>

<p id="operation">![GET](images\onenote\get.png)&nbsp;`/sectiongroups/{id}/permissions`</p>
<p id="description">セクション グループの[アクセス許可](../howto/onenote-manage-perms.md)を取得します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![GET](images\onenote\get.png)&nbsp;`/permissions/{id}`</p>
<p id="description">セクション グループの特定の[アクセス許可](../howto/onenote-manage-perms.md)を取得します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![POST](images\onenote\post.png)&nbsp;`/permissions`</p>
<p id="description">セクション グループの[アクセス許可](../howto/onenote-manage-perms.md)を作成または更新します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![DELETE](images\onenote\delete.png)&nbsp;`/permissions/{id}`</p>
<p id="description">セクション グループの[アクセス許可](../howto/onenote-manage-perms.md)を削除します。 <span class="small">(*Office 365 のみ*)</span></p>

<br />
[*GET* 要求](../howto/onenote-get-content.md)の詳細 (サポートされるクエリ文字列のオプションを含む) を習得してください。


<a name="notebooks"></a>
##ノートブックの操作

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/notebooks`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getallnotebooks%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">すべてのノートブックを取得します。</p>

<p id="operation">[![GET](images\onenote\get.png)&nbsp;`/notebooks/{id}`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22getnotebook%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%7D%2C%22body%22%3A%7B%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%7D%7D%2C%22verb%22%3A%22get%22%7D)</p>
<p id="description">特定のノートブックを取得します。</p>

<p id="operation">[![POST](images\onenote\post.png)&nbsp;`/notebooks`](https://apigee.com/onenote/embed/console/onenote/?req=%7B%22resource%22%3A%22createnotebook%22%2C%22params%22%3A%7B%22query%22%3A%7B%7D%2C%22template%22%3A%7B%7D%2C%22headers%22%3A%7B%22Content-Type%22%3A%22application%2Fjson%22%7D%2C%22body%22%3A%7B%22undefined%22%3A%22%5Cu0001%5Cu0001%22%2C%22attachmentParamName%22%3A%22file%20attachment%22%2C%22attachmentFormat%22%3A%22mime%22%2C%22attachmentContentDisposition%22%3A%22form-data%22%2C%22bodyText%22%3A%22%7B%20name%3A%20%5C%22name%5C%22%20%7D%22%7D%7D%2C%22verb%22%3A%22post%22%7D)</p>
<p id="description">ノートブックを作成します。</p>

<p id="operation">![POST](images\onenote\post.png)&nbsp;`/notebooks/{id}/copyNotebook`</p>
<p id="description">ノートブックをコピーします。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![GET](images\onenote\get.png)&nbsp;`/notebooks/{id}/permissions`</p>
<p id="description">ノートブックの[アクセス許可](../howto/onenote-manage-perms.md)を取得します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![GET](images\onenote\get.png)&nbsp;`/permissions/{id}`</p>
<p id="description">ノートブックの特定の[アクセス許可](../howto/onenote-manage-perms.md)を取得します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![POST](images\onenote\post.png)&nbsp;`/permissions`</p>
<p id="description">ノートブックの[アクセス許可](../howto/onenote-manage-perms.md)を作成または更新します。 <span class="small">(*Office 365 のみ*)</span></p>

<p id="operation">![DELETE](images\onenote\delete.png)&nbsp;`/permissions/{id}`</p>
<p id="description">ノートブックの[アクセス許可](../howto/onenote-manage-perms.md)を削除します。 <span class="small">(*Office 365 のみ*)</span></p>

<br />
[*GET* 要求](../howto/onenote-get-content.md)の詳細 (サポートされるクエリ文字列のオプションを含む) と、[ノートブックのコピー](../howto/onenote-copy.md)方法を習得してください。

>`classNotebooks` エンドポイントを使用して、[クラス ノートブックを操作](../howto/onenote-classnotebook.md)し、`staffNotebooks` エンドポイントを使用して、[スタッフ ノートブックを操作](../howto/onenote-staffnotebook.md)します。


<a name="resources"></a>
## リソースの操作

<p id="operation">![GET](images\onenote\get.png)&nbsp;`/resources/{id}/content`</p>
<p id="description">イメージまたはファイル リソースのバイナリ コンテンツを取得します。</p>

<br />
[*GET* 要求](../howto/onenote-get-content.md)の詳細と、[イメージおよびファイルをページに追加する](../howto/onenote-images-files.md)方法を習得してください。



<a name="see-also"></a>
## その他のリソース

- [OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)
- [OneNote ページの作成](../howto/onenote-create-page.md)
- [OneNote ページ コンテンツを更新する](../howto/onenote-update-page.md)
- [OneNote ページにイメージとファイルを追加する](../howto/onenote-images-files.md)
- [ノートブック、セクション、およびページのコピー](../howto/onenote-copy.md)
- [OneNote ページの入出力 HTML](../howto/onenote-input-output-html.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182)
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  

