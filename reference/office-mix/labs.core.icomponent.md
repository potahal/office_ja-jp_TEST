
# Labs.Core.IComponent

 _ **適用対象:** apps for Office?| Office Add-ins?| Office Mix?| PowerPoint_

Base class for representing components of a lab.

```
interface IComponent extends Core.ILabObject, Core.IUserData
```


## Properties


|||
|:-----|:-----|
| `name:?string`|Name of the component.|
| `values: {[type:string]: Core.IValue[]}`|The value property map associated with the component.|
