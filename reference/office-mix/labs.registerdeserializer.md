
# Labs.registerDeserializer

 _ **適用対象:** apps for Office?| Office Add-ins?| Office Mix?| PowerPoint_

Deserializes a specified JSON object into an object. Should be used by component authors only.

```
function?registerDeserializer(type:?string,?deserialize:?(json:?Core.ILabObject):?any):?void
```


## パラメーター


|||
|:-----|:-----|
|json|The [Labs.Core.ILabObject](../../reference/office-mix/labs.core.ilabobject.md) to deserialize.|

## Return value

Returns an [Labs.Core.ILabObject](../../reference/office-mix/labs.core.ilabobject.md) instance.

