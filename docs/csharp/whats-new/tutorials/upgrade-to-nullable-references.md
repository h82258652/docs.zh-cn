---
title: 升级为可为 null 的引用类型
description: 本高级教程演示如何使用可为 null 的引用类型迁移现有代码。
ms.date: 02/19/2019
ms.technology: csharp-null-safety
ms.custom: mvc
ms.openlocfilehash: dc254e8ca7b9451ab9374bc1ec650b8eaa9da861
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878883"
---
# <a name="tutorial-migrate-existing-code-with-nullable-reference-types"></a>教程：使用可为空引用类型迁移现有代码

C# 8 引入了可为空引用类型，它们以与可为空值类型补充值类型相同的方式补充引用类型。 通过将 `?` 追加到此类型，你可以将变量声明为  可为空引用类型。 例如，`string?` 表示可为空的 `string`。 可以使用这些新类型更清楚地表达你的设计意图：某些变量  必须始终具有值，其他变量可以缺少值  。 引用类型的任何现有变量都将被解释为不可为空引用类型。

本教程介绍以下操作：

> [!div class="checklist"]
>
> - 使用代码时启用空引用检查。
> - 诊断并更正与 Null 值相关的其他警告。
> - 管理可为空启用上下文和可为空禁用上下文之间的接口。
> - 控制可为空的批注上下文。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。 自 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 起，开始随附 C# 8 编译器。

本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。

## <a name="explore-the-sample-application"></a>浏览示例应用程序

即将迁移的示例应用程序是一个 RSS 源阅读器 Web 应用。 它从单个 RSS 源中进行读取并显示最新文章的摘要。 可以选择任何文章以访问网站。 应用程序相对较新，但却是在可以使用可为空引用类型前编写的。 应用程序的设计决策代表了合理的原则，但没有利用这一重要的语言功能。

示例应用程序包括验证应用主要功能的单元测试库。 如果根据生成的警告更改任何实现，该项目将使安全升级变得更容易。 若要下载起始代码，可以访问 [dotnet/samples](https://github.com/dotnet/samples/tree/main/csharp/tutorials/nullable-reference-migration/start) GitHub 存储库。

迁移项目的目标应该是利用新的语言功能，以便清楚地表达对变量的为 Null 性的意图，并以如下特定方式执行此操作，即在将可为空注释上下文和可为空警告上下文设置为 `enabled` 时，编译器不会生成警告。

## <a name="upgrade-the-projects-to-c-8"></a>将项目升级到 C#8

第一步建议确定迁移任务的范围。 首先将项目升级到 C# 8.0（或更高版本）。 将 `LangVersion` 元素同时添加到 Web 项目和单元测试项目的 csproj 文件中的 PropertyGroup：

```xml
<LangVersion>8.0</LangVersion>
```

升级语言版本选择 C# 8.0，但不启用可为空注释上下文或可为空警告上下文。 重新生成项目，以确保在没有警告的情况下进行生成。

下一步建议打开可为空注释上下文，看看生成了多少警告。 在解决方案中的两个 csproj 文件中（直接在 `LangVersion` 元素下面）添加以下元素：

```xml
<Nullable>enable</Nullable>
```

执行测试生成，并注意警告列表。 在此小型应用程序中，编译器将生成五个警告，因此可能会启用可为空注释上下文，并开始为整个项目修复警告。

该策略仅适用于较小的项目。 对于任何大型项目，通过为整个代码库启用可为空注释上下文而生成的警告数使得以系统方式修复警告变得更加困难。 对于大型企业项目，通常希望一次迁移一个项目。 在每个项目中，一次迁移一个类或文件。

## <a name="warnings-help-discover-original-design-intent"></a>警告有助于发现原始设计意图

有两个类生成多个警告。 从 `NewsStoryViewModel` 类开始。 从两个 csproj 文件中删除 `Nullable` 元素，以便将警告范围限制为正在处理的代码部分。 打开 NewsStoryViewModel.cs 文件并添加下列指令，以启用 `NewsStoryViewModel` 的可为空注释上下文，并按照类定义对其进行还原：

```csharp
#nullable enable
public class NewsStoryViewModel
{
    public DateTimeOffset Published { get; set; }
    public string Title { get; set; }
    public string Uri { get; set; }
}
#nullable restore
```

这两条指令可帮助你专注于迁移工作。 为你正在积极处理的代码区域生成可为空警告。 在做好准备为整个项目启用警告之前，将一直保留这些警告。 应使用 `restore` 而不是 `disable` 值，以便在稍后为整个项目启用可为空注释时不会意外禁用上下文。 一旦为整个项目启用了可为空注释上下文，就可以从该项目中删除所有 `#nullable` 杂注。

`NewsStoryViewModel` 类是一个数据传输对象 (DTO)，其中两个属性是读/写字符串：

[!code-csharp[InitialViewModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/ViewModels/NewsStoryViewModel.cs#StarterViewModel)]

这两个属性导致 `CS8618`，“不可为空属性未初始化”。 这很清楚：在构造 `NewsStoryViewModel` 时，两个 `string` 属性的默认值都是 `null`。 重要的是了解 `NewsStoryViewModel` 对象是如何构造的。 查看此类时，无法判断 `null` 值是否是设计的一部分，或者这些对象在创建时是否被设置为非 Null 值。 新闻故事在 `NewsService` 类的 `GetNews` 方法中创建：

[!code-csharp[StarterCreateNewsItem](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Services/NewsService.cs#CreateNewsItem)]

前面的代码块中有相当多的内容。 此应用程序使用 [AutoMapper](https://automapper.org/) NuGet 包从 `ISyndicationItem` 中构造新闻项。 你会发现，在这一条语句中构造了新闻故事项，并设置了属性。 这意味着 `NewsStoryViewModel` 的设计表明这些属性绝不应该有 `null` 值。 这些属性应是不可为空引用类型。 这样可以充分表达原始设计意图。 实际上，任何  都是`NewsStoryViewModel` 用非 Null 值正确实例化的。 这使得以下初始化代码成为一个有效的修复程序：

```csharp
public class NewsStoryViewModel
{
    public DateTimeOffset Published { get; set; }
    public string Title { get; set; } = default!;
    public string Uri { get; set; } = default!;
}
```

`Title` 和 `Uri` 赋值为 `default`（`string` 类型为 `null`）不会更改程序的运行时行为。 `NewsStoryViewModel` 仍然用 Null 值构造，但现在编译器不会报告任何警告。 Null 包容运算符，`default` 表达式后面的 `!` 字符指示编译器前面的表达式不为 Null。 当其他更改强制对代码库进行更大的更改时，这种方法可能会很方便，但在此应用程序中，有一个相对快速且更好的解决方案：使 `NewsStoryViewModel` 为不可变类型，其中所有属性均在构造函数中设置。 对 `NewsStoryViewModel` 进行以下更改：

[!code-csharp[FinishedViewModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/ViewModels/NewsStoryViewModel.cs#FinishedViewModel)]

完成更改之后，需要更新配置 AutoMapper 的代码，以便它使用构造函数而不是设置属性。 打开 `NewsService.cs`，在文件底部查找以下代码：

[!code-csharp[StarterAutoMapper](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Services/NewsService.cs#ConfigureAutoMapper)]

该代码将 `ISyndicationItem` 对象的属性映射到 `NewsStoryViewModel` 属性。 希望 AutoMapper 改用构造函数来提供映射。 请用以下 automapper 配置替换上面的代码：

[!code-csharp[FinishedViewModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#ConfigureAutoMapper)]

注意，因为此类很小，而且你已经仔细检查过，所以应打开此类声明上面的 `#nullable enable` 指令。 对构造函数的更改可能会破坏某些内容，因此有必要在继续之前运行所有测试并测试应用程序。

第一组更改展示了如何发现原始设计指示不应该将变量设置为 `null`。 该方法称为“通过构造更正”。 在构造对象时，声明该对象及其属性不能为 `null`。 编译器的流分析确保这些属性在构造之后不会被设置为 `null`。 注意，此构造函数由外部代码调用，而该代码无论是否可为空都能进行调用。 新语法不提供运行时检查。 外部代码可能会避开编译器的流分析。

其他情况下，类的结构提供了意图的不同线索。 打开“Pages”文件夹中的“Error.cshtml.cs”文件。 `ErrorViewModel` 包含以下代码：

[!code-csharp[StarterErrorModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Pages/Error.cshtml.cs#StartErrorModel)]

在类声明之前添加 `#nullable enable` 指令，在类声明之后添加 `#nullable restore` 指令。 会出现一个警告，指示 `RequestId` 未初始化。 通过查看类，应确定在某些情况下 `RequestId` 属性应为 Null。 `ShowRequestId` 属性的存在表明可能缺失某些值。 因为 `null` 有效，所以在 `string` 类型上添加 `?` 表示 `RequestId` 属性是可为空引用类型。 最终类如下所示：

[!code-csharp[FinishedErrorModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Error.cshtml.cs#ErrorModel)]

检查属性的使用情况，会看到在关联页中，在标记中呈现属性之前会检查其是否为 Null。 这是可为空引用类型的安全使用，因此已完成此类的检查。

## <a name="fixing-nulls-causes-change"></a>修复 Null 会导致更改

通常情况下，修复一组警告会在相关代码中创建新的警告。 让我们通过修复 `index.cshtml.cs` 类来查看实际警告。 打开 `index.cshtml.cs` 文件并检查代码。 此文件包含索引页的隐藏代码：

[!code-csharp[StarterIndexModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Pages/Index.cshtml.cs#IndexModelStart)]

添加 `#nullable enable` 指令，将会看到两个警告。 `ErrorText` 属性和 `NewsItems` 属性都不会初始化。 此类检查会让你认为这两个属性应为可为空引用类型：都具有专用的 setter。 在 `OnGet` 方法中正好分配了一个属性。 在更改之前，看看这两个属性的使用者。 在页面本身，在为任何错误生成标记之前，将检查 `ErrorText` 是否为 Null。 检查 `NewsItems` 集合是否为 `null`，并选中以确保集合具有项。 快速修复会使这两个属性成为可为空引用类型。 更好的解决方法是使集合成为不可为空引用类型，并在检索新闻时向现有集合添加项。 第一个解决方法是为 `ErrorText` 的 `string` 类型添加 `?`：

[!code-csharp[UpdateErrorText](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#UpdateErrorText)]

此更改不会影响其他代码，因为对 `ErrorText` 属性的任何访问均已通过 Null 检查进行保护。 接下来，初始化 `NewsItems` 列表并删除属性资源库，使其成为只读属性：

[!code-csharp[InitializeNewsItems](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#InitializeNewsItems)]

这修复了警告，但引入了错误。 `NewsItems` 列表现在是“通过构造更正，但在 `OnGet` 中设置列表的代码必须更改以匹配新的 API。 调用 `AddRange` 将新闻项添加到现有列表，而不是赋值：

[!code-csharp[AddRange](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#AddRange)]

使用 `AddRange` 而不是赋值意味着 `GetNews` 方法可以返回 `IEnumerable` 而不是 `List`。 这节省了一次分配。 更改方法的签名，并删除 `ToList` 调用，如下面的代码示例所示：

[!code-csharp[GetNews](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#GetNewsFinished)]

更改签名也会中断其中一个测试。 打开 `SimpleFeedReader.Tests` 项目 `Services` 文件夹中的 `NewsServiceTests.cs` 文件。 导航到 `Returns_News_Stories_Given_Valid_Uri` 测试并将 `result` 变量的类型更改为 `IEnumerable<NewsItem>`。 更改类型意味着 `Count` 属性不再可用，因此将 `Assert` 中的 `Count` 属性替换为对 `Any()` 的调用：

[!code-csharp[FixTests](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader.Tests/Services/NewsServiceTests.cs#FixTestSignature)]

还需要向文件开头添加一个 `using System.Linq` 语句。

这组更改强调了更新包含泛型实例化的代码时需要特别考虑的问题。 列表和不可为空类型列表中的元素。 其中一个或两者可以是可为空类型。 允许所有以下声明：

- `List<NewsStoryViewModel>`：不可为空视图模型的不可为空列表。
- `List<NewsStoryViewModel?>`：可为空视图模型的不可为空列表。
- `List<NewsStoryViewModel>?`：不可为空视图模型的可为空列表。
- `List<NewsStoryViewModel?>?`：可为空视图模型的可为空列表。

## <a name="interfaces-with-external-code"></a>使用外部代码的接口

已经对 `NewsService` 类进行了更改，因此请为该类启用 `#nullable enable` 注释。 这不会生成任何新的警告。 但是，仔细检查该类有助于说明编译器流分析的一些限制。 检查构造函数：

[!code-csharp[ServiceConstructor](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#ServiceConstructor)]

`IMapper` 参数类型为不可为空引用。 它由 ASP.NET Core 基础结构代码调用，因此编译器并不知道 `IMapper` 永远不会为 Null。 如果默认的 ASP.NET Core 依赖关系注入 (DI) 容器不能解析必要的服务，就会引发异常，因此代码是正确的。 编译器无法验证对公共 API 的所有调用，即使代码是在启用了可为空注释上下文的情况下编译的。 此外，尚未选择使用可为空引用类型的项目可能会使用你的库。 验证公共 API 的输入，即使已将它们声明为不可为空类型。

## <a name="get-the-code"></a>获取代码

已经修复了在初始测试编译中标识的警告，因此现在可以为两个项目启用可为空注释上下文。 重新生成项目；编译器不会报告任何警告。 可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/csharp/tutorials/nullable-reference-migration/finished) GitHub 存储库中获取已完成项目的代码。

支持可为空引用类型的新功能可以帮助你发现和修复代码中处理 `null` 值的方式中的潜在错误。 启用可为空注释上下文可以表达设计意图：某些变量永远不应为 Null，其他变量可以包含 Null 值。 借助这些功能，可以更轻松地声明设计意图。 同样，可为空警告上下文指示编译器在违背该意图时发出警告。 这些警告指导你进行更新，使代码更具弹性，并减少在执行期间引发 `NullReferenceException` 的可能性。 可以控制这些上下文的范围，以便专注于要迁移的代码的本地区域，同时保证其余代码库不受影响。 实际上，可以将此迁移任务作为类的常规维护的一部分。 本教程演示了迁移应用程序以使用可为空引用类型的过程。 可以通过检查 PR [Jon Skeet](https://github.com/jskeet) 来浏览此过程一个更大的实际示例，用于将可为空引用类型合并到 [NodaTime](https://github.com/nodatime/nodatime/pull/1240/commits)。 此外，还可以在 [Entity Framework Core - 使用可为 null 的引用类型](/ef/core/miscellaneous/nullable-reference-types)中学习将可为 null 的引用类型与 Entity Framework Core 结合使用的技术。
