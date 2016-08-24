# 沙盒解决方案转换指南 - InfoPath
使用具有隐藏代码的 InfoPath 表单时，这些 InfoPath 表单的确取决于基于代码的沙盒解决方案，以执行隐藏的代码。 本文将帮助你修复或转换你的 InfoPath 表单，以便再也没有对沙盒解决方案的依赖性。

_**适用于：**SharePoint Online 的 InfoPath 表单 SharePoint 2013 | SharePoint 2016_

基于代码的沙盒解决方案在 2014 年被 [弃用](https://blogs.msdn.microsoft.com/sharepointdev/2014/01/14/deprecation-of-custom-code-in-sandboxed-solutions/)(#弃用)，而 SharePoint online 开启了完全删除此功能的过程。 基于代码的沙盒解决方案在 SharePoint 2013 和 SharePoint 2016 中也同样被弃用。

## 分析并尽可能修复你的 InfoPath 表单
<a name="sectionSection1"> </a>

在本节中，我们向你介绍可应用于 InfoPath 表单的分析和修复模型。 根据表单，你可以仅**修复**表单并将其重新部署，或需要**从 InfoPath 移走**，并使用替代方法来实现所需功能。 但是在执行这些操作前，**为你的表单评估业务需求非常重要**：我们经常看到很多旧表单已不再和业务需求相关，在这种情况下，只需删除表单即可。 

### 如何知道我拥有隐藏代码的 InfoPath 表单？
可以使用 PnP 计划提供的 **[特定沙盒解决方案清单脚本](https://github.com/OfficeDev/PnP-Tools/tree/master/Scripts/SharePoint.Sandbox.ListSolutionsFromTenant)**(#特定沙盒解决方案清单脚本) 以获取租户中已安装沙盒解决方案的列表。 拥有该列表后，可以查看 **WSPName 列**：通常情况下，以**“InfoPath Form_”开头**的 WSP 列来自具有隐藏代码的部署的 InfoPath 表单。

### 我的表单是否仍然相关？
深入探究修正/转换工作前，考虑这样一个问题很重要：此表单是否对我的业务仍然很关键？ 如果确实如此，请继续阅读下一章节，如果并非如此，则需要考虑使用该表单所创建的数据。 通常，数据被创建为 InfoPath XML 文件，位于 SharePoint 列表中。 如果删除表单，则不能再将数据可视化：有时候该操作是可行的，因为表单和数据不再相关，但是在想要确保访问数据的情况下，则可以将 InfoPath XML 文件转换为 SharePoint 列表（项目）数据。[PnP 转换存储库包含如何实现此操作的示例](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Migration/EmpRegConsole "介绍如何从 InfoPath XML 转换到列表数据的示例")。

### 下载供检查的 InfoPath 表单（XSN 文件）
在本节的步骤中已确认了需要进行操作的 InfoPath 表单，在本节中，将了解如何下载这些表单。 具有隐藏代码的 InfoPath 表单作为“表单库”或“网站内容类型”进行部署。 这取决于可下载表单可选的发布模型，如下所示。

#### 下载以“表单库”部署的 InfoPath 表单
在这种情况下，XSN 文件位于 InfoPath 表单被部署的表单库的表单文件夹内。 若要了解表单库，可以查看遵循此约定的 WSP 包名称：“InfoPath Form_**LibName**_id”。 因此，一旦拥有库，便需要从库的“表单”文件夹下载 template.xsn 文件。 为此，可以构造如本示例中所示的 url：** url + "Forms/template.xsn"**`https://contoso.sharepoint.com/sites/infopath1/IHaveCodeBehind/Forms/template.xsn`并使用浏览器下载文件。

#### 下载以“网站内容类型”部署的 InfoPath 表单
作为内容类型部署的 InfoPath 表单的确拥有存储在表单库中的 XSN 文件，且将在表单部署时连接至内容类型。 如上一节所述，可以从 WSP 包名称获取库名称。 不同之处在于，表单实际上作为文件存储在已找到的库中，因此只需从表单库对其下载。

### 修复你的 InfoPath 表单
上一节我们已经介绍并给出具有隐藏代码的 InfoPath 表单，但是它们是否包含有用的隐藏代码呢？ 我们所看到的是，表单作者在 InfoPath 的“**开发者**”功能区的“**代码编辑器**”按钮上无意中单击出了许多表单。

![隐藏代码的 InfoPath](media/Sandbox-Solution-Transformation-Guidance-InfoPath/InfoPathCodeBehind.png)

执行此操作后，将拥有隐藏代码，但是该隐藏代码是无用的，通过将其删除，可以将具有隐藏代码的 InfoPath 表单转换为没有隐藏代码的常规 InfoPath 表单，从而不会再对沙盒解决方案具有依赖性！

#### 如何知道隐藏代码“无用”？
你可能想知道如何区分隐藏代码是无用的还是所需要的，因为你仅能修复第一种类别。 如果你仍然具有原始部署的表单（而非在前面的步骤中下载的表单），只需扫视一下代码即可。 默认为空的代码显示在下方，如果有类似代码，则可通过删除代码来修复该表单：

```C#
using Microsoft.Office.InfoPath;
using System;
using System.Xml;
using System.Xml.XPath;

namespace Form1
{
    public partial class FormCode
    {
        // Member variables are not supported in browser-enabled forms.
        // Instead, write and read these values from the FormState
        // dictionary using code such as the following:
        //
        // private object _memberVariable
        // {
        //     get
        //     {
        //         return FormState["_memberVariable"];
        //     }
        //     set
        //     {
        //         FormState["_memberVariable"] = value;
        //     }
        // }

        // NOTE: The following procedure is required by Microsoft InfoPath.
        // It can be modified using Microsoft InfoPath.
        public void InternalStartup()
        {
        }
    }
}
```

假使你仅有从前面的步骤中下载的 XSN 文件，则可以将 XSN 文件重命名为 cab 文件（例如 template.cab）、提取程序集并使用 .Net 反射工具（如开源 [ILSpy](http://ilspy.net/ " ILSpy 是开源 .NET 程序集浏览器和反编译程序")）以检查代码。 无用的隐藏代码的典型视图在 ILSpy 中如下所示：

![无用的代码在 ILSpy 中如下所示](media/Sandbox-Solution-Transformation-Guidance-InfoPath/ilspyoutput.png)

#### 将隐藏的代码从 InfoPath 表单删除以对其修复
如果已确认隐藏代码是无用的，则可通过以下操作轻松地将其删除：
- 打开“**InfoPath 设计器**”中的表单（右键单击 -“设计”）
- 通过“文件”-“信息”转到“**表单选项**”
- 选择“**编程**”类别并在“**删除代码**”上单击
- 通过“文件”-“信息”-“快速发布”以“**重新发布表单**”
- 通过“网站设置”-“解决方案”以“**停用链接的沙盒解决方案**”
- **确认**表单按预期方式运行
- **删除**沙盒解决方案

## 迁移你的 InfoPath 表单
<a name="sectionSection2"> </a>
如果前面章节的指南对你的 InfoPath 表单不适用，这实际上意味着你的表单仍与业务相关，且包含无法删除的隐藏代码。 如果是此种情况，典型的解决方案是从 InfoPath 移走，可通过以下方法实现：
- 使用 [Azure PowerApps](https://powerapps.microsoft.com/en-us/ "Azure PowerApps") 或 [Microsoft 流](https://flow.microsoft.com/en-us/search/?q=sharepoint "Microsoft 流") 创建应用 

![Azure PowerApps 和 Microsoft 流](media/Sandbox-Solution-Transformation-Guidance-InfoPath/powerappsflow.png)

- 生成利用远程 API 的读/写 SharePoint 数据的 SharePoint 外接程序

### 生成替换 InfoPath 表单的 SharePoint 外接程序
如果你倾向于使用常规的 SharePoint 外接程序以替换 InfoPath 表单，则有多种选项。 下面是我们提供了更多详细信息的三种选项，但是正如我们所说，你可以使用这些选项的变体。 我们想要深入探究的三种选项是：
- [基于 knockout.js 的单页面应用程序 (SPA)](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Samples/EmployeeRegistration.KnockOut.SinglePageApp "使用 knockout.js 的 SPA 示例")

![挖空示例](media/Sandbox-Solution-Transformation-Guidance-InfoPath/knockoutsample.png)

- [ASP.Net MVC SharePoint 外接程序](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Samples/EmployeeRegistration.MVC "使用 ASP.Net MVC 的 SharePoint 外接程序")
- [ASP.Net 表单 SharePoint 外接程序](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Samples/EmployeeRegistration.Forms "使用 ASP.Net 表单的 SharePoint 外接程序")

为了更好地帮助你转换 InfoPath 表单，**我们列出了 11 种常见的 InfoPath 编码模式，并介绍如何使用上面提到的三种 SharePoint 外接程序选项来实现这些模式**。 为此，我们首先开发了 [引用 InfoPath 表单](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Reference/EmployeeRegistration "引用 InfoPath 表单")，它使用最常见的 InfoPath 编码模式，然后我们将该表单迁移到三种 SharePoint 外接程序类型。 以下链接介绍这些常见模式：
- [在表单加载上填充字段 - 设置用户信息](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Populating%20fields%20on%20form%20load-set%20user%20information.md "在表单加载上填充字段 - 设置用户信息")
- [在表单加载上填充字段 - 读取列表信息](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Populating%20fields%20on%20form%20load-read%20list%20information.md "在表单加载上填充字段 - 读取列表信息")
- [在表单加载上填充字段 - 读取列表数据](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Populating%20fields%20on%20form%20load-read%20list%20data.md "在表单加载上填充字段 - 读取列表数据")
- [通过代码提交表单](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Submit%20the%20form%20via%20code.md "通过代码提交表单")
- [表单提交后切换视图](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Switching%20view%20after%20form%20submission.md "表单提交后切换视图")
- [检索用户数据](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Retrieving%20user%20data.md "检索用户数据")
- [读取数据集合并设置多个控件](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Read%20data%20collection%20and%20set%20multiple%20controls.md "读取数据集合并设置多个控件")
- [级联数据加载](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Cascading%20data%20load.md "级联数据加载")
- [上载或删除附件](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Upload%20or%20Delete%20Attachments.md "上载或删除附件")
- [从网站组添加或删除用户](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Add%20or%20remove%20user%20from%20site%20groups.md "从网站组添加或删除用户")
- [加载表单中的现有项](https://github.com/OfficeDev/PnP-Transformation/blob/master/InfoPath/Guidance/Patterns/Load%20existing%20item%20in%20form.md "加载表单中的现有项")


### 迁移你的 InfoPath 数据
将 InfoPath 表单移至新的解决方案后，你可能也想要将你的数据从 InfoPath XML 迁移至常规的 SharePoint 列表数据或所选的数据层。 因为 InfoPath 文件是 XML 文件，所以对其读取和转换非常简单。[PnP 转换存储库包含如何实现此操作的示例](https://github.com/OfficeDev/PnP-Transformation/tree/master/InfoPath/Migration/EmpRegConsole "介绍如何从 InfoPath XML 转换到列表数据的示例")。