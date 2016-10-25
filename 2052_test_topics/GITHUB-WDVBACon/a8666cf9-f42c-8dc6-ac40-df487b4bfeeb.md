
# 确定 Application 属性是否必要

不使用 **应用程序** 对象识别符的情况下，可以使用的许多属性和方法的 **[应用程序](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** 对象。例如不 **应用程序** 对象识别符的情况下可以使用 **[ActiveDocument](c20a7c9f-f8a4-7913-f53f-10baa6807def.md)** 属性。而不是编写 `Application.ActiveDocument.PrintOut`，您可以编写 `ActiveDocument.PrintOut`。

不需要  **Application** 对象识别符的属性和方法被看作"全局"属性和方法。要在"对象浏览器"中查看全局属性和方法，可单击"类"框中列表顶部的"<全局>"。
