---
ms.Toctitle: HTTP response status codes
title: "Office 365 HTTP response status codes"
description: "Locate error codes and status information for Office 365 APIs."
ms.date: May 26, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]


# HTTP 応答状態コード

_**適用対象:** Office 365_

要求を処理する際に、Microsoft Web サーバーでは応答の通信に HTTP 応答状態コードを使用します。 

HTTP 状態コードの詳細については、「[HTTP 状態コードの一覧](http://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml#http-status-codes-1)」を参照してください。

次の表に、Microsoft Web サーバーから返される最も一般的な状態コードを示します。

|HTTP ステータス コード| HTTP 状態の説明| 説明|
|:-----|:-----|:-----|
|200|   OK| エラーなし。要求は成功しました|
|201|   Created|    エラーなし。新しいエンティティが作成されました|
|202|   Accepted|   エラーなし。処理の要求が受理されました|
|204|   No Content| 要求は成功しましたが、応答がありません|
|400|   Bad Request|    無効なパラメーターが渡されました|
|401|   Unauthorized|   要求にユーザー認証が必要です|
|403|   Forbidden|  試行した操作に対するアクセス権がありません| 
|404|   Not Found|  無効な URI です|
|405|   MethodNotAllowed|   メソッドまたはコンテンツの種類がサポートされていません|
|406|   NotAcceptable|  要求されたコンテンツの種類がサポートされていません|
|410|   Gone|   要求されたエンティティが存在しません|
|415|   UnsupportedMediaType|   要求されたコンテンツの種類がサポートされていません|
|429|   Too Many Requests|  レートの制限を超えました|
|500|   Internal| Server Error	トランザクションが失敗しました|
|501|   NotImplemented| 要求に失敗しました。将来実装される可能性があります|
|503|   Service Unavailable|    システムを利用できません。後でもう一度試してください|

## JSON 形式の応答

JSON 形式で応答する場合に Microsoft が使用している書式については、「[OData JSON Format Version 4.0 Plus Errata 02 でのエラー応答](http://docs.oasis-open.org/odata/odata-json-format/v4.0/errata02/os/odata-json-format-v4.0-errata02-os-complete.html#_Toc403940655)」を参照してください。

## 応答の例

An error response might contain [annotations](http://docs.oasis-open.org/odata/odata-json-format/v4.0/errata02/os/odata-json-format-v4.0-errata02-os-complete.html#_Instance_Annotations) in any of its JSON objects.
```json
{
  "error": {
    "code": "501",
    "message": "Unsupported functionality",
    "target": "query",
    "details": [
      {
       "code": "301",
       "target": "$search" 
       "message": "$search query option not supported",
      }
    ],
    "innererror": {
      "trace": [...],
      "context": {...}
    }
  }
}

```

