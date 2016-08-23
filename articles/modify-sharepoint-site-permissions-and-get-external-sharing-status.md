# SharePoint のサイト許可を変更して外部共有ステータスを取得する

CSOM を使用してサイト コレクションの管理者のプロパティを変更します。外部の共有状態と、サイト コレクションまたはテナントの外部ユーザーを取得します。

_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

[Core.SitePermissions](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.SitePermissions) サンプルを使用すると、サイト コレクション上のサイト コレクション管理者 (Office 365 テナント上の OneDrive for Business の管理者を含みます) を変更できます。またこのサンプルでは、Office 365 マルチテナント インストールの外部共有状態を取得する方法も示されています。

コンソール アプリケーションを使用して **ClientContext** オブジェクトを作成し、管理者をリストまたは変更したり、外部共有状態を取得したりするための許可を得ます。OAuth トークンを使用して登録済みアドインも作成します。

## 始める前に

まず、[Core.SitePermissions](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Core.SitePermissions) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## サイト コレクション管理者の処理

サイト コレクションの管理者を更新するには、サイト コレクションの管理者である必要があります。最初の手順は、適切なアクセス許可を持つユーザーとして **ClientContext** オブジェクトを作成することです。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
ClientContext cc = new AuthenticationManager().GetSharePointOnlineAuthenticatedContextTenant(String.Format("https://{0}.sharepoint.com/sites/{1}", tenantName, siteName), String.Format("{0}@{1}.onmicrosoft.com", userName, tenantName), password); 
```

次の例に示されているように、**ClientContext** オブジェクトを使用すると、現在のサイト コレクション管理者の一覧を取得したり、サイト コレクション管理者を更新したりできます。

```C#
List<UserEntity> admins = cc.Web.GetAdministrators();

List<UserEntity> adminsToAdd = new List<UserEntity>();
adminsToAdd.Add(new UserEntity() { LoginName = "i:0#.f|membership|user@domain" });

cc.Web.AddAdministrators(adminsToAdd);

UserEntity adminToRemove = new UserEntity() { LoginName = "i:0#.f|membership|user@domain" };
cc.Web.RemoveAdministrator(adminToRemove);
```

登録済みのアドインを使用して **ClientContext** オブジェクトを作成すると、サイト コレクション管理者にまだなっていないサイト コレクションに関してもサイト コレクション管理者を設定できます。この場合の **ClientContext** オブジェクトは、テナント レベルのアクセス許可が設定されている OAuth トークンに基づいています。

```C#
// Use (Get-MsolCompanyInformation).ObjectID to obtain Target/Tenant realm: <guid>
//
// Manually register an add-in via the appregnew.aspx page and generate an add-in ID and 
// add-in secret. The add-in title and add-in domain can be a simple string like "MyAddin".
//
// Update the add-in ID in your worker role settings.
//
// Add the add-in secret in your worker role settings. 
//
// Manually set the permission XML for you add-in via the appinv.aspx page:
// 1. Look up your add-in via its add-in ID.
// 2. Paste the permission XML and choose create.
//
// Sample permission XML:
// <AppPermissionRequests AllowAppOnlyPolicy="true">
//   <AppPermissionRequest Scope="http://sharepoint/content/tenant" Right="FullControl" />
// </AppPermissionRequests>
//
// Because you're granting tenant-wide full control to an add-in, the add-in secret is as important
// as the password from your SharePoint administration account.
```
この操作の実行後、次のコードを使用すると、このアドインの **ClientContext** オブジェクトを取得できます。

```C#
ClientContext cc = new AuthenticationManager().GetAppOnlyAuthenticatedContext("https://tenantname-my.sharepoint.com/personal/user2", "<your tenant realm>", "<appID>", "<appsecret>");
```

## 外部共有の処理 (Office 365 MT のみ)

このシナリオは、サイト コレクションの外部共有状態を取得し、特定のサイト コレクションまたはテナント全体の外部ユーザーの一覧を取得する方法を示しています。このためにはテナント CSOM ライブラリが必要なので、テナント管理者のサイト コレクションに対して **ClientContext** を作成しなければなりません。このユーザー アカウントは、テナント管理者アカウントである必要があります。

```C#
ClientContext ccTenant = new AuthenticationManager().GetSharePointOnlineAuthenticatedContextTenant(String.Format("https://{0}-admin.sharepoint.com/", tenantName), String.Format("{0}@{1}.onmicrosoft.com", userName, tenantName), password);
```

**ClientContext** の準備を整えてから、次のコードを使用すると、外部共有状態および外部ユーザーの一覧を取得できます。

```C#
ccTenant.Web.GetSharingCapabilitiesTenant(new Uri(String.Format("https://{0}.sharepoint.com/sites/{1}", tenantName, siteName)))

List<ExternalUserEntity> externalUsers = ccTenant.Web.GetExternalUsersForSiteTenant(new Uri(String.Format("https://{0}.sharepoint.com/sites/{1}", tenantName, siteName)));

List<ExternalUserEntity> externalUsers = ccTenant.Web.GetExternalUsersTenant();

```

## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint サイト プロビジョニング ソリューション](sharepoint-site-provisioning-solutions.md)
    
- [Provisioning.Pages サンプル](https://github.com/OfficeDev/PnP/tree/dev/Scenarios/Provisioning.Pages)
