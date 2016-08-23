# SharePoint のユーザーとグループを管理する

SharePoint サイト コレクション内のユーザー、グループ、および許可を管理します。 

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

この記事では、特定のサイト コレクション内のグループやユーザーを追加または削除する方法を示します。この記事のコード例は、ユーザーとグループを追加してから、それらに SharePoint にアクセスするための許可レベルを付与します。それらのユーザーとグループのアクセス許可レベルのアクションは、 [Core.GroupManagement](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.GroupManagement) PnP サンプルの拡張メソッドによって実装されます。

## 始める前に

まず、 [Core.GroupManagement](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.GroupManagement) のサンプル アドインを GitHub の [Office 365 Developer パターンおよびプラクティス](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードしてください。

## グループやユーザーを追加および削除する

次の例は、グループを追加する方法、およびユーザーをグループに追加する方法を示しています。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
cc.Load(cc.Web, web => web.CurrentUser);
cc.ExecuteQuery();
Microsoft.SharePoint.Client.User currentUser = cc.Web.CurrentUser;

if (!cc.Web.GroupExists("Test"))
{
  Group group = cc.Web.AddGroup("Test", "Test group", true);
  cc.Web.AddUserToGroup("Test", currentUser.LoginName);
}
```

次の例では、グループを削除します。

```C#
if (cc.Web.GroupExists("Test"))
{
  cc.Web.RemoveGroup("Test");
}
```

次の例では、ユーザーをグループから削除します。

```C#
cc.Load(cc.Web, web => web.CurrentUser);
cc.ExecuteQuery();
Microsoft.SharePoint.Client.User currentUser = cc.Web.CurrentUser;
if (cc.Web.GroupExists("Test"))
{
  if (cc.Web.IsUserInGroup("Test", currentUser.LoginName))
  {
    cc.Web.RemoveUserFromGroup("Test", currentUser.LoginName);
  }
}
```

## グループやユーザーに許可レベルを追加する

次の例では、グループに許可レベルを追加します。

```C#
if (cc.Web.GroupExists("Test"))
{
  cc.Web.AddPermissionLevelToGroup("Test", RoleType.Contributor);
}
```

次の例では、ユーザーに許可レベルを追加します。

```C#
cc.Load(cc.Web, web => web.CurrentUser);
cc.ExecuteQuery();
Microsoft.SharePoint.Client.User currentUser = cc.Web.CurrentUser;
cc.Web.AddPermissionLevelToUser(currentUser.LoginName, RoleType.Reader);
```

## グループやユーザーから許可レベルを削除する

次の例では、グループから許可レベルを削除します。

```C#
if (cc.Web.GroupExists("Test"))
{
  cc.Web.RemovePermissionLevelFromGroup("Test", RoleType.Reader);
}

```
次の例では、ユーザーから許可レベルを削除します。

```C#
cc.Load(cc.Web, web => web.CurrentUser);
cc.ExecuteQuery();
Microsoft.SharePoint.Client.User currentUser = cc.Web.CurrentUser;
cc.Web.RemovePermissionLevelFromUser(currentUser.LoginName, RoleType.Reader);
```

## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint サイト プロビジョニング ソリューション](sharepoint-site-provisioning-solutions.md)
    
- [SharePoint 2013 および SharePoint Online のブランディングおよびサイト プロビジョニング ソリューション](Branding-and-site-provisioning-solutions-for-SharePoint.md)
