---
title: 教程：编写第一个分析器和代码修补程序
description: 本教程提供了有关使用 .NET 编译器 SDK (Roslyn API) 生成分析器和代码修补程序的分步说明。
ms.date: 03/02/2021
ms.custom: mvc
ms.openlocfilehash: 040a91c5755c736a9729e6796d2e73d2f956a8dd
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514458"
---
# <a name="tutorial-write-your-first-analyzer-and-code-fix"></a>教程：编写第一个分析器和代码修补程序

.NET Compiler Platform SDK 提供面向 C# 或 Visual Basic 代码创建自定义诊断（分析器）、代码修补程序、代码重构和诊断抑制器所需的工具。 分析器包含可识别规则冲突的代码。 代码修补程序包含修复冲突的代码。 实现的规则可以是从代码结构到编码样式再到命名约定之类的任何内容。 .NET Compiler Platform 在开发人员编写代码时提供运行分析的框架，以及用于修复代码的所有 Visual Studio UI 功能：显示编辑器中的波形曲线、填充 Visual Studio 错误列表、创建“灯泡”建议，并显示建议修补程序的丰富预览。

在本教程中，将探讨使用 Roslyn API 创建分析器以及随附的代码修补程序。 分析器是一种执行源代码分析并向用户报告问题的方法。 可以选择将代码修补程序与分析器相关联，来表示对用户源代码的修改。 本教程将创建一个分析器，用于查找可以使用 `const` 修饰符声明的但未执行此操作的局部变量声明。 随附的代码修补程序修改这些声明来添加 `const` 修饰符。

## <a name="prerequisites"></a>先决条件

- [Visual Studio 2019](https://www.visualstudio.com/downloads) 版本 16.8 或更高版本

必须通过 Visual Studio 安装程序安装 .NET 编译器平台 SDK：

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

可以通过几个步骤创建和验证分析器：

1. 创建解决方案。
1. 注册分析器名称和描述。
1. 报告分析器警告和建议。
1. 实现代码修复以接受建议。
1. 通过单元测试改进分析。

## <a name="create-the-solution"></a>创建解决方案

- 在 Visual Studio 中，选择“文件”>“新建”>“项目...”，显示“新建项目”对话框。
- 在“Visual C#”>“扩展性”下，选择“随附代码修补程序的分析器 (.NET Standard)”。
- 给项目“MakeConst”命名，然后单击“确定”。

## <a name="explore-the-analyzer-template"></a>探索分析器模板

随附代码修补程序的分析器模板会创建五个项目：

- MakeConst，其中包含分析器。
- MakeConst.CodeFixes，其中包含代码修补程序。
- MakeConst.Package，用于生成分析器和代码修补程序的 NuGet 包。
- MakeConst.Test，这是一个单元测试项目。
- MakeConst.Vsix，这是默认的启动项目，它将启动加载了新分析器的第二个 Visual Studio 实例。 按 F5<kbd></kbd> 启动 VSIX 项目。

> [!TIP]
> 在运行分析器时，请启动 Visual Studio 的第二个副本。 此第二个副本使用不同的注册表配置单元来存储设置。 这样便可以将 Visual Studio 两个副本中的可视化设置区分开来。 可以选择 Visual Studio 实验性运行的不同主题。 此外，不要在设置中漫游，也不要使用 Visual Studio 的实验性运行登录到 Visual Studio 帐户。 这样可以使设置保持不同。
>
> 该配置单元不仅包括正在开发的分析器，而且还包括任何以前打开的分析器。 若要重置 Roslyn 配置单元，需要从 % LocalAppData%\\Microsoft\\VisualStudio 中手动将其删除。 Roslyn 配置单元的文件夹名称将以 `Roslyn` 结尾，例如 `16.0_9ae182f9Roslyn`。 请注意，你可能需要在删除配置单元后清除解决方案并重新生成。

在刚刚启动的第二个 Visual Studio 实例中，创建一个新的 C# 控制台应用程序项目（任何目标框架都可用 -- 分析器在源级别工作。）悬停在带波浪下划线的标记上，将显示分析器提供的警告文本。

该模板创建一个分析器，它报告有关类型名称包含小写字母的每种类型声明的警告，如下图所示：

![分析器报告警告](media/how-to-write-csharp-analyzer-code-fix/report-warning.png)

该模板还提供了一种代码修补程序，它可以将包含小写字符的任何类型名称更改为大写字母。 可以单击显示警告的灯泡，以查看建议的更改。 接受建议的更改会更新解决方案中的类型名称和所有对该类型的引用。 现在你已了解初始分析器的操作，关闭第二个 Visual Studio 实例，并返回到分析器项目。

无需启动 Visual Studio 的第二个副本和创建新代码来测试分析器中的每一项更改。 该模板还为你创建了单元测试项目。 该项目包含两个测试。 `TestMethod1` 显示了在不触发诊断的情况下分析代码的典型测试格式。 `TestMethod2` 显示了先触发诊断然后应用建议的代码修补程序的测试格式。 在构建分析器和代码修补程序时，为不同的代码结构编写测试，以验证你的工作。 分析器的单元测试比使用 Visual Studio 以交互方式进行测试的速度更快。

> [!TIP]
> 当你知道哪些代码构造应触发和不应触发分析器时，分析器单元测试是一个很好的工具。 在 Visual Studio 的另一个副本加载分析器是用于浏览并找到你可能未曾想到的构造的绝佳工具。

在本教程中，你将编写一个分析器，用于向用户报告可以转换为局部常量的任何局部变量声明。 例如，考虑以下代码：

```csharp
int x = 0;
Console.WriteLine(x);
```

在上面的代码中，会向 `x` 分配常量值，并且永远不会被修改。 可以使用 `const` 修饰符声明：

```csharp
const int x = 0;
Console.WriteLine(x);
```

涉及到确定变量是否可以保持不变的分析，需要进行句法分析、初始值设定项的常量分析和数据流分析，以确保永远不会写入该变量。 .NET Compiler Platform 提供了 API，以便更轻松地执行此分析。

## <a name="create-analyzer-registrations"></a>创建分析器注册

该模板将在 MakeConstAnalyzer.cs 文件中创建初始 `DiagnosticAnalyzer` 类。 此初始分析器显示每个分析器的两个重要属性。

- 每个诊断分析器必须提供 `[DiagnosticAnalyzer]` 属性，用于描述其操作所用的语言。
- 每个诊断分析器都必须是（直接或间接地）从 <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer> 类派生的。

该模板还显示属于任何分析器的基本功能：

1. 注册操作。 此操作表示应触发分析器以检查存在冲突的代码的代码更改。 当 Visual Studio 检测到匹配注册操作的代码编辑时，它调用分析器的注册方法。
1. 创建诊断。 当分析器检测到冲突，它会创建一个诊断对象，Visual Studio 使用该对象来通知用户有关冲突的信息。

在重写的 <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer.Initialize(Microsoft.CodeAnalysis.Diagnostics.AnalysisContext)?displayProperty=nameWithType> 方法中注册操作。 在本教程中，你将访问“语法节点”寻找局部声明，并查看哪些具有常量值。 如果声明可以是常量，分析器将创建并报告诊断。

第一步是更新注册常量和 `Initialize` 方法，以便这些常量指示“Make Const”分析器。 大多数字符串常量在字符串资源文件中定义。 应遵循此做法，以便更轻松地实现本地化。 打开“MakeConst”分析器项目的“Resources.resx”。 将显示资源编辑器。 更新字符串资源，如下所示：

- 将 `AnalyzerDescription` 更改为“:::no-loc text="Variables that are not modified should be made constants.":::”。
- 将 `AnalyzerMessageFormat` 更改为“:::no-loc text="Variable '{0}' can be made constant":::”。
- 将 `AnalyzerTitle` 更改为“:::no-loc text="Variable can be made constant":::”。

完成后，资源编辑器应如下图所示：

![更新字符串资源](media/how-to-write-csharp-analyzer-code-fix/update-string-resources.png)

剩余的更改在分析器文件中。 在 Visual Studio 中打开“MakeConstAnalyzer.cs”。 将注册操作从作用于符号的操作更改为作用于语法的操作。 在 `MakeConstAnalyzerAnalyzer.Initialize` 方法中，找到在符号上注册操作的行：

```csharp
context.RegisterSymbolAction(AnalyzeSymbol, SymbolKind.NamedType);
```

使用下面的行替换它：

[!code-csharp[Register the node action](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#RegisterNodeAction "Register a node action")]

完成此更改后，可以删除 `AnalyzeSymbol` 方法。 此分析器检查 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.LocalDeclarationStatement?displayProperty=nameWithType>，而不是 <xref:Microsoft.CodeAnalysis.SymbolKind.NamedType?displayProperty=nameWithType> 语句。 请注意，`AnalyzeNode` 下面有红色波浪线。 刚添加的代码引用未声明的 `AnalyzeNode` 方法。 使用以下代码声明该方法：

```csharp
private void AnalyzeNode(SyntaxNodeAnalysisContext context)
{
}
```

将 `Category` 更改为 MakeConstAnalyzer.cs 中的“:::no-loc text="Usage":::”，如以下代码所示：

[!code-csharp[Category constant](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#Category  "Change category to Usage")]

## <a name="find-local-declarations-that-could-be-const"></a>查找可以是常量的局部声明

可以开始编写 `AnalyzeNode` 方法的第一个版本了。 应查找可以是 `const` 但实际不是的单个局部声明，如以下代码所示：

```csharp
int x = 0;
Console.WriteLine(x);
```

第一步是查找局部声明。 将以下代码添加到 MakeConstAnalyzer.cs 中的 `AnalyzeNode`：

[!code-csharp[localDeclaration variable](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#LocalDeclaration  "Add localDeclaration variable")]

此强制转换始终会成功，因为分析器注册了对局部声明的更改，并且只注册了局部声明。 没有其他节点类型会触发对 `AnalyzeNode` 方法的调用。 接下来，检查任何 `const` 修饰符的声明。 一旦找到，请立即返回。 以下代码用于查找局部声明上的任何 `const` 修饰符：

[!code-csharp[bail-out on const keyword](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#BailOutOnConst  "bail-out on const keyword")]

最后，需要检查变量是否可能是 `const`。 这意味着确保在其初始化后永远不会对其赋值。

将使用 <xref:Microsoft.CodeAnalysis.Diagnostics.SyntaxNodeAnalysisContext> 执行一些语义分析。 使用 `context` 参数确定局部变量声明是否可为 `const`。 <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> 表示单个源文件中的所有语义信息。 可参阅涵盖了[语义模型](../work-with-semantics.md)的文章了解详细信息。 将使用 <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> 在局部声明语句上执行数据流分析。 然后，使用此数据流分析的结果确保局部变量不会在任何其他位置用新值来编写。 调用 <xref:Microsoft.CodeAnalysis.ModelExtensions.GetDeclaredSymbol%2A> 扩展方法来检索变量的 <xref:Microsoft.CodeAnalysis.ILocalSymbol>，并检查确认它不包含在数据流分析的 <xref:Microsoft.CodeAnalysis.DataFlowAnalysis.WrittenOutside%2A?displayProperty=nameWithType> 集中。 在 `AnalyzeNode` 方法的末尾添加以下代码：

```csharp
// Perform data flow analysis on the local declaration.
DataFlowAnalysis dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

// Retrieve the local symbol for each variable in the local declaration
// and ensure that it is not written outside of the data flow analysis region.
VariableDeclaratorSyntax variable = localDeclaration.Declaration.Variables.Single();
ISymbol variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable, context.CancellationToken);
if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
{
    return;
}
```

刚添加的代码可确保变量不会修改，并因此可以进行 `const` 操作。 现在可以引发诊断了。 将以下代码添加为 `AnalyzeNode` 的最后一行：

[!code-csharp[Call ReportDiagnostic](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#ReportDiagnostic  "Call ReportDiagnostic")]

可以通过按 F5<kbd></kbd> 运行分析器来检查进度。 可以加载前面创建的控制台应用程序，然后添加以下测试代码：

```csharp
int x = 0;
Console.WriteLine(x);
```

应显示灯泡，且分析器应报告诊断。 但是，灯泡仍使用模板生成的代码修补程序，并告知你可以用大写。 下一部分将说明如何编写代码修补程序。

## <a name="write-the-code-fix"></a>编写代码修补程序

分析器可以提供一个或多个代码修补程序。 代码修补程序定义解决报告问题的编辑。 对于你创建的分析器，可以提供将插入 const 关键字的代码修补程序：

```diff
- int x = 0;
+ const int x = 0;
Console.WriteLine(x);
```

用户从编辑器的灯泡 UI 中选择它，Visual Studio 更改代码。

打开 CodeFixResources.resx 文件，并将 `CodeFixTitle` 更改为“:::no-loc text="Make constant":::”。

打开由模板添加的“MakeConstCodeFixProvider.cs”文件。 此代码修补程序已绑定到由诊断分析器生成的诊断 ID，但它尚没有实施正确的代码转换。

接下来，删除 `MakeUppercaseAsync` 方法。 它不再适用。

所有代码修复提供程序都派生自 <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider>。 它们都重写 <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider.RegisterCodeFixesAsync(Microsoft.CodeAnalysis.CodeFixes.CodeFixContext)?displayProperty=nameWithType> 以报告可用的代码修补程序。 在 `RegisterCodeFixesAsync` 中，将正在搜索的上级节点类型更改为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> 以匹配诊断：

[!code-csharp[Find local declaration node](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#FindDeclarationNode  "Find the local declaration node that raised the diagnostic")]

接下来，更改用于注册代码修补程序的最后一行。 修补程序将创建新的文档，该文档通过将 `const` 修饰符添加到现有声明生成：

[!code-csharp[Register the new code fix](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#RegisterCodeFix  "Register the new code fix")]

你会注意到刚在符号 `MakeConstAsync` 上添加的代码中的红色波浪线。 添加的 `MakeConstAsync` 声明如以下代码所示：

```csharp
private static async Task<Document> MakeConstAsync(Document document,
    LocalDeclarationStatementSyntax localDeclaration,
    CancellationToken cancellationToken)
{
}
```

新的 `MakeConstAsync` 方法会将表示用户源文件的 <xref:Microsoft.CodeAnalysis.Document> 转换到现包含 `const` 声明的新 <xref:Microsoft.CodeAnalysis.Document>。

创建一个新的 `const` 关键字标记，以在声明语句的开头处插入。 请注意，首先从声明语句的第一个标记中删除任何前导琐碎内容，然后将其附加到 `const` 标记。 将以下代码添加到 `MakeConstAsync` 方法中：

[!code-csharp[Create a new const keyword token](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#CreateConstToken  "Create the new const keyword token")]

接下来，使用以下代码向声明添加 `const` 标记：

```csharp
// Insert the const token into the modifiers list, creating a new modifiers list.
SyntaxTokenList newModifiers = trimmedLocal.Modifiers.Insert(0, constToken);
// Produce the new local declaration.
LocalDeclarationStatementSyntax newLocal = trimmedLocal
    .WithModifiers(newModifiers)
    .WithDeclaration(localDeclaration.Declaration);
```

接下来，设置要匹配 C# 格式设置规则的新声明的格式。 对所做的更改进行格式设置以匹配现有代码，这可创建更好的体验。 紧接着在现有代码后面添加以下语句：

[!code-csharp[Format the new declaration](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#FormatLocal  "Format the new declaration")]

此代码需要新命名空间。 将下面的 `using` 指令添加到文件的顶部：

```csharp
using Microsoft.CodeAnalysis.Formatting;
```

最后一步是进行编辑。 此过程包括三个步骤：

1. 获取现有文档的句柄。
1. 通过将现有声明替换为新声明来创建一个新文档。
1. 返回新文档。

在 `MakeConstAsync` 方法的末尾添加以下代码：

[!code-csharp[replace the declaration](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#ReplaceDocument  "Generate a new document by replacing the declaration")]

代码修补程序已准备就绪。  按 <kbd> F5 </kbd> 在第二个 Visual Studio 实例中运行分析器项目。 在第二个 Visual Studio 实例中，创建一个新的 C# 控制台应用程序项目并向 Main 方法添加使用常量值初始化的几个局部变量声明。 你将看到它们被报告为警告，如下所示。

![可以发出 const 警告](media/how-to-write-csharp-analyzer-code-fix/make-const-warning.png)

现在已经有了很大的进展。 可以进行 `const` 操作的声明下具有波浪线。 但仍有工作要做。 如果将 `const` 添加到依次以 `i`、`j` 和 `k` 开头的声明，该过程会很有效。 不过，如果以从 `k` 开始的不同顺序添加 `const` 修饰符，分析器会生成错误：`k` 无法声明为 `const`，除非 `i` 和 `j` 均已进行 `const` 处理。 必须执行详细分析，以确保处理可以声明和初始化变量的不同方式。

## <a name="build-unit-tests"></a>生成单元测试

分析器和代码修补程序在简单的单个声明情况下工作，可以对其进行 const 处理。 在许多可能的声明语句中，该实现会出错。 可以通过使用模板编写的单元测试库来处理这种情况。 它要比反复打开 Visual Studio 的第二个副本快得多。

打开单元测试项目中的“MakeConstUnitTests.cs”文件。 该模板会创建两个测试，这些测试遵循分析器和代码修补程序单元测试的两种常见模式。 `TestMethod1` 显示测试模式，确保分析器在不应报告诊断的情况下不会执行此操作。 `TestMethod2` 演示用于报告诊断和运行代码修补程序的模式。

该模板使用 [Microsoft.CodeAnalysis.Testing](https://github.com/dotnet/roslyn-sdk/blob/main/src/Microsoft.CodeAnalysis.Testing/README.md) 包进行单元测试。

> [!TIP]
> 测试库支持特殊标记语法，其中包括以下内容：
>
> - `[|text|]`：表示报告 `text` 的诊断信息。 默认情况下，此格式只可用于测试由 `DiagnosticAnalyzer.SupportedDiagnostics` 提供了正好一个 `DiagnosticDescriptor` 的分析器。
> - `{|ExpectedDiagnosticId:text|}`：表示针对 `text` 报告 <xref:Microsoft.CodeAnalysis.Diagnostic.Id> 为 `ExpectedDiagnosticId` 的诊断信息。

将 `MakeConstUnitTest` 类中的模板测试替换为以下测试方法：

[!code-csharp[test method for fix test](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#FirstFixTest "test method for fix test")]

运行此测试，确保测试通过。 在 Visual Studio 中，通过选择“测试” > “Windows” > “测试资源管理器”来打开“测试资源管理器”。 然后，选择“全部运行”。

## <a name="create-tests-for-valid-declarations"></a>为有效声明创建测试

作为一般规则，分析器应尽可能使工作量最小化以快速退出。 Visual Studio 调用注册分析器作为用户编辑代码。 响应能力是一项关键要求。 有多个代码测试用例，不应引发诊断。 分析器已处理这些测试中的一个，其中变量在初始化后进行了分配。 添加以下测试方法来表示这种情况：

[!code-csharp[variable assigned](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VariableAssigned "a variable that is assigned after being initialized won't raise the diagnostic")]

此测试也通过了。 接下来，为尚未处理的情况添加测试方法：

- 已经是 `const` 的声明，因为它们已为 const 类型：

   [!code-csharp[already const declaration](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#AlreadyConst "a declaration that is already const should not raise the diagnostic")]

- 没有初始值设定项的声明，因为没有要使用的值：

   [!code-csharp[declarations that have no initializer](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#NoInitializer "a declaration that has no initializer should not raise the diagnostic")]

- 初始值设定项不是常量的声明，因为它们不能是编译时常量：

   [!code-csharp[declarations where the initializer isn't const](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#InitializerNotConstant "a declaration where the initializer is not a compile-time constant should not raise the diagnostic")]

甚至可能更加复杂，因为 C# 允许多个声明作为一条语句。 请考虑以下测试用例字符串常量：

[!code-csharp[multiple initializers](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#MultipleInitializers "A declaration can be made constant only if all variables in that statement can be made constant")]

变量 `i` 可以常量化，但变量 `j` 不能。 因此，此语句不能成为 const 声明。

再次运行测试，将看到这些新测试用例失败。

## <a name="update-your-analyzer-to-ignore-correct-declarations"></a>更新分析器以忽略正确声明

需要对分析器的 `AnalyzeNode` 方法进行一些增强，以筛选出匹配这些条件的代码。 它们是所有相关条件，因此类似的更改将解决所有这些条件。 对 `AnalyzeNode` 进行以下更改：

- 语义分析检查单个变量声明。 此代码必须位于 `foreach` 循环中，以检查同一语句中声明的所有变量。
- 每个声明变量需要有初始值设定项。
- 每个声明的变量的初始值设定项必须是编译时常量。

在 `AnalyzeNode` 方法中，替换原始语义分析：

```csharp
// Perform data flow analysis on the local declaration.
DataFlowAnalysis dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

// Retrieve the local symbol for each variable in the local declaration
// and ensure that it is not written outside of the data flow analysis region.
VariableDeclaratorSyntax variable = localDeclaration.Declaration.Variables.Single();
ISymbol variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable, context.CancellationToken);
if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
{
    return;
}
```

使用以下代码片段：

```csharp
// Ensure that all variables in the local declaration have initializers that
// are assigned with constant values.
foreach (VariableDeclaratorSyntax variable in localDeclaration.Declaration.Variables)
{
    EqualsValueClauseSyntax initializer = variable.Initializer;
    if (initializer == null)
    {
        return;
    }

    Optional<object> constantValue = context.SemanticModel.GetConstantValue(initializer.Value, context.CancellationToken);
    if (!constantValue.HasValue)
    {
        return;
    }
}

// Perform data flow analysis on the local declaration.
DataFlowAnalysis dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

foreach (VariableDeclaratorSyntax variable in localDeclaration.Declaration.Variables)
{
    // Retrieve the local symbol for each variable in the local declaration
    // and ensure that it is not written outside of the data flow analysis region.
    ISymbol variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable, context.CancellationToken);
    if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
    {
        return;
    }
}
```

第一个 `foreach` 循环将使用语法分析检查每个变量声明。 第一次检查可保证该变量具有初始值设定项。 第二次检查可保证初始值设定项是一个常量。 第二个循环具有原始语义分析。 语义检查是在一个单独循环中，因为它对性能具有更大的影响。 再次运行测试，应看到它们全部通过。

## <a name="add-the-final-polish"></a>添加最后的润饰

即将完成。 分析器还要处理一些其他条件。 用户编写代码时，Visual Studio 将调用分析器。 通常情况下，分析器将针对无法进行编译的代码进行调用。 诊断分析器的 `AnalyzeNode` 方法不会检查以查看常量值是否可转换为变量类型。 因此，当前实现会不假思索地将不正确的声明（如 int i = “abc”）转换为局部常量。 为这种情况添加测试方法：

[!code-csharp[Mismatched types don't raise diagnostics](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsInvalid "When the variable type and the constant type don't match, there's no diagnostic")]

此外，无法正确处理引用类型。 允许用于引用类型的唯一常量值为 `null`， <xref:System.String?displayProperty=nameWithType> 这种情况除外，后者允许字符串。 换而言之，`const string s = "abc"` 是合法的，但 `const object s = "abc"` 不是。 此代码片段验证以下条件：

[!code-csharp[Reference types don't raise diagnostics](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsntString "When the variable type is a reference type other than string, there's no diagnostic")]

为全面起见，需要添加另一个测试以确保你可以为字符串创建常量声明。 以下代码片段定义引发诊断的代码和在应用修补程序后的代码：

[!code-csharp[string reference types raise diagnostics](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#ConstantIsString "When the variable type is string, it can be constant")]

最后，如使用关键字 `var` 声明变量，代码修补程序将执行错误操作，并生成 `const var` 声明，C# 语言不支持该声明。 若要修复此 bug，代码修补程序必须将 `var` 关键字替换为推断类型的名称：

[!code-csharp[var references need to use the inferred types](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VarDeclarations "Declarations made using var must have the type replaced with the inferred type")]

幸运的是，所有上述 bug 可以使用你刚刚了解的相同技术解决。

若要修复第一个 bug，请先打开“MakeConstAnalyzer.cs”，并找到 foreach 循环，将检查其中每个局部声明的初始值设定项以确保向其分配常量值。 在第一个 foreach 循环之前，立即调用 `context.SemanticModel.GetTypeInfo()` 来检索有关局部声明的声明类型的详细信息：

[!code-csharp[Retrieve type information](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#VariableConvertedType "Retrieve type information")]

然后，在 `foreach` 循环中，检查每个初始值设定项，以确保它可以转换为变量类型。 确保初始值设定项为常量后，添加以下检查：

[!code-csharp[Ensure non-user-defined conversion](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#BailOutOnUserDefinedConversion "Bail-out on user-defined conversion")]

下一次更改建立在最后一次更改之上。 在第一个 foreach 循环的右大括号前，添加以下代码以检查当常量为字符串或 NULL 时局部声明的类型。

[!code-csharp[Handle special cases](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#HandleSpecialCases "Handle special cases")]

必须在代码修复提供程序中编写更多代码以将 `var` 关键字替换为正确类型名称。 返回到 MakeConstCodeFixProvider.cs。 要添加的代码将执行以下步骤：

- 检查声明是否为 `var` 声明，如果它是：
- 创建新类型的推断类型。
- 确保类型声明不是别名。 如果是这样，则声明 `const var` 是合法的。
- 确保 `var` 不是此程序中的类型名称。 （如果是这样，则 `const var` 是合法的）。
- 简化完整类型名称

这听起来好像有很多代码。 其实不然。 将声明和初始化 `newLocal` 的行替换为以下代码。 在初始化 `newModifiers` 之后立即进行：

[!code-csharp[Replace Var designations](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#ReplaceVar "Replace a var designation with the explicit type")]

需要添加一个 `using` 指令才能使用 <xref:Microsoft.CodeAnalysis.Simplification.Simplifier> 类型：

```csharp
using Microsoft.CodeAnalysis.Simplification;
```

运行测试，它们应全部通过。 通过运行已完成的分析器自行庆祝。 按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 在加载了 Roslyn Preview 扩展的第二个 Visual Studio 实例中运行分析器项目。

- 在第二个 Visual Studio 实例，创建一个新的 C# 控制台应用程序项目并将 `int x = "abc";` 添加到 Main 方法。 由于第一个 bug 已修复，应不会报告针对此局部变量声明的警告（尽管像预期那样出现了编译器错误）。
- 接下来，将 `object s = "abc";` 添加到 Main 方法。 由于第二个 bug 已修复，应不会报告任何警告。
- 最后，添加另一个使用 `var` 关键字的局部变量。 你将看到一个警告和显示在左下方的一个建议。
- 将编辑器插入点移到波浪下划线，然后按 <kbd>Ctrl</kbd>+<kbd>。</kbd> 显示建议的代码修补程序。 选择代码修补程序，请注意，`var` 关键字现已正确处理。

最后，添加以下代码：

```csharp
int i = 2;
int j = 32;
int k = i + j;
```

完成这些更改后，仅在前两个变量上有红色波浪线。 将 `const` 同时添加到 `i` 和 `j`，你将获得一个有关 `k` 的新警告，因为它现在可以是 `const`。

祝贺你！ 你已创建第一个 .NET Compiler Platform 扩展来执行即时代码分析，以便检测问题，并提供了用于快速更正的修补程序。 在此过程中，你已了解很多代码 API 是 .NET Compiler Platform SDK (Roslyn API) 的一部分。 可以在我们的示例 GitHub 存储库中根据[完成的示例](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/Tutorials/MakeConst)来检查工作。

## <a name="other-resources"></a>其他资源

- [语法分析入门](../get-started/syntax-analysis.md)
- [语义分析入门](../get-started/semantic-analysis.md)
