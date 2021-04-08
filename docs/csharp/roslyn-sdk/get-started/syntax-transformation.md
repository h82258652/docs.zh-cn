---
title: 语法转换 (Roslyn API) 入门
description: 介绍如何遍历、查询及浏览语法树。
ms.date: 06/01/2018
ms.custom: mvc
ms.openlocfilehash: f12727332256acd9bbe828b9149376080facd156
ms.sourcegitcommit: 652f62fc8f3ab6a264681b6eb5211ac7539bd115
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "105964816"
---
# <a name="get-started-with-syntax-transformation"></a>语法转换入门

本教程基于[语法分析入门](syntax-analysis.md)和[语义分析入门](semantic-analysis.md)快速入门中介绍的概念和技巧。 如果尚未执行此操作，应在开始之前完成这些快速入门。

在本快速入门教程，你将了解用于创建和转换语法树的技巧。 结合你在前面的快速入门中了解的技巧，可以创建第一个命令行重构！

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="immutability-and-the-net-compiler-platform"></a>不可变性和 .NET 编译器平台

不可变性是 .NET 编译器平台的基本原则。 在创建不可变数据结构后，不能对其进行更改。 不可变数据结构可以由多个使用者同时安全地共享和分析。 一个使用者以不可预测的方式影响另一个使用者不存在任何风险。 分析器不需要锁或其他并发度量值。 此规则适用于语法树、编译、符号、语义模型，以及你所遇到的所有其他数据结构。 API 不是修改现有结构，而是基于旧结构的特定差异创建新对象。 将此概念应用到语法树中，以使用转换来创建新树。

## <a name="create-and-transform-trees"></a>创建和转换树

选择两个策略之一进行语法转换。 当你在寻找要替换的特定节点时，或者想要在其中插入新代码的特定位置时，最好使用工厂方法。 当你想要扫描一个你想要替换的代码模式的整个项目时，最好使用重写工具。

### <a name="create-nodes-with-factory-methods"></a>使用工厂方法创建节点

第一个语法转换演示工厂方法。 将 `using System.Collections;` 语句替换为 `using System.Collections.Generic;` 语句。 此示例演示如何使用 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory?displayProperty=nameWithType> 工厂方法创建 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxNode?displayProperty=nameWithType> 对象。 对于每一类节点、令牌或琐事，都有创建该类型实例的工厂方法。 可以通过以自下而上的方式按层次结构组合节点来创建语法树。 然后，转换现有程序，用你所创建的新树替换现有节点。

启动 Visual Studio，并新建 C#“独立代码分析工具”项目。 在 Visual Studio 中，选择“文件”   > “新建”   > “项目”  ，显示新建项目对话框。 在“Visual C#” > “扩展性”下，选择“独立代码分析工具”。 本快速入门教程有两个示例项目，因此将解决方案命名为“SyntaxTransformationQuickStart”，并将项目命名为“ConstructionCS”。 单击 **“确定”** 。

此项目使用 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory?displayProperty=nameWithType> 类方法构造 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType> 来表示 `System.Collections.Generic` 命名空间。

将以下 using 指令添加到 `Program.cs` 顶部。

[!code-csharp[import the SyntaxFactory class](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#StaticUsings "import the Syntax Factory class and the System.Console class")]

创建 **命名语法节点** 以创建表示 `using System.Collections.Generic;` 语句的树。 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax> 是在 C# 中显示的四种类型名称的基类。 将这四种类型名称组合在一起，以创建任何可通过 C# 语言显示的名称：

* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType>，表示简单的单个标识符名称，如 `System` 和 `Microsoft`。
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.GenericNameSyntax?displayProperty=nameWithType>，表示泛型类型或方法名称，如 `List<int>`。
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax?displayProperty=nameWithType>，表示窗体 `<left-name>.<right-identifier-or-generic-name>` 的限定名称，如 `System.IO`。
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.AliasQualifiedNameSyntax?displayProperty=nameWithType>，表示使用程序集外部别名的名称，如 `LibraryV2::Foo`。

若要创建 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax> 节点，请使用 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory.IdentifierName(System.String)> 方法。 在 `Program.cs` 的 `Main` 方法中添加以下代码：

[!code-csharp[create the system identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateIdentifierName "Create and display the system name identifier")]

前面的代码创建 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.IdentifierNameSyntax> 对象，并将其分配给变量 `name`。 许多 Roslyn API 返回基类，使其更轻松地处理相关类型。 变量 `name`，即 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax>，可以在生成 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> 时重用。 在生成示例时，不要使用类型推理。 你将自动执行此项目中的这一步。

你已创建名称。 现在，可以通过构建 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> 在树中生成更多节点。 新树使用 `name` 作为左侧名称，并使用 `Collections` 命名空间新的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.IdentifierNameSyntax> 作为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> 的右侧。 将下列代码添加到 `program.cs`：

[!code-csharp[create the collections identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateQualifiedIdentifierName "Build the System.Collections identifier")]

再次运行代码并查看结果。 你将构建一个表示代码的节点树。 你将继续运行此模式，以便生成命名空间 `System.Collections.Generic` 的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax>。 将下列代码添加到 `Program.cs`：

[!code-csharp[create the full identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateFullNamespace "Build the System.Collections.Generic identifier")]

再次运行此程序，以查看你已为要添加的代码生成的树。

### <a name="create-a-modified-tree"></a>创建修改后的树

你已构建一个小型语法树，其中包含一个语句。 用于创建新节点的 API 是创建单个语句或其他小代码块的正确选择。 但是，若要生成更大的代码块，应使用替换节点或将节点插入到现有树的方法。 请记住语法树不可变。 语法 API 不提供用于完成构造后修改现有语法树的任何机制。 相反，它提供基于对现有数的更改生成新树的方法。 `With*` 方法在派生自 <xref:Microsoft.CodeAnalysis.SyntaxNode> 的具体类中定义，或者在 <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions> 类中声明的扩展方法中定义。 这些方法通过将更改应用于现有节点的子属性来创建一个新节点。 此外，<xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> 扩展方法可以用来替换子树中的子代节点。 此方法还会更新父结点，以指向新创建的子节点，并在整个树中重复这一过程，该过程称为“重新遍历树”。

下一步是创建一个表示整个（小型）程序的树，然后修改它。 将以下代码添加到 `Program` 类的开头：

[!code-csharp[create a parse tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#DeclareSampleCode "Create a tree that represents a small program")]

> [!NOTE]
> 该示例代码使用 `System.Collections` 命名空间而不是 `System.Collections.Generic` 命名空间。

接下来，将以下代码添加到 `Main` 方法的底部来分析文本，并创建树：

[!code-csharp[create a parse tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateParseTree "Create a tree that represents a small program")]

此示例使用 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.WithName(Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax)?displayProperty=NameWithType> 方法将 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> 节点中的名称替换为在前面代码中构造的名称。

使用 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.WithName(Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax)> 方法创建一个新的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> 节点，将 `System.Collections` 名称更新为在前面代码中创建的名称。 将以下代码添加到 `Main` 方法底部：

[!code-csharp[create a new subtree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#BuildNewUsing "Create the subtree with the replaced namespace")]

运行程序，并仔细查看输出。 `newUsing` 尚未置于根树中。 原始树尚未更改。

使用 <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> 扩展方法添加以下代码以创建新树。 新树是将现有导入替换为更新后的 `newUsing` 节点的结果。 将此新树分配给现有 `root` 变量：

[!code-csharp[create a new root tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#TransformTree "Create the transformed root tree with the replaced namespace")]

再次运行程序。 现在，树正确地导入了 `System.Collections.Generic` 命名空间。

### <a name="transform-trees-using-syntaxrewriters"></a>使用 `SyntaxRewriters` 转换树

`With*` 和 <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> 方法提供了方便的方法来转换语法树的单独分支。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter?displayProperty=nameWithType> 类在语法树上执行多个转换。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter?displayProperty=nameWithType> 类是 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor%601?displayProperty=nameWithType> 的一个子类。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> 将转换应用于特定类型的 <xref:Microsoft.CodeAnalysis.SyntaxNode>。 你可以将转换应用于多个类型的 <xref:Microsoft.CodeAnalysis.SyntaxNode> 对象，只要它们显示在语法树中。 本快速入门教程中的第二个项目创建命令行重构，以便在可以使用类型推理的任何位置删除本地变量声明中的显式类型。

新建 C#“独立代码分析工具”项目。 在 Visual Studio 中，右键单击 `SyntaxTransformationQuickStart` 解决方案节点。 选择“添加” > “新项目”以显示“新项目对话框”。 在“Visual C#”   > “扩展性”  下，选择“独立代码分析工具”  。 给项目 `TransformationCS` 命名，然后单击“确定”。

第一步是创建一个派生自 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> 的类，以执行转换。 向项目添加一个新类文件。 在 Visual Studio 中，依次选择“项目” > “添加类...”。在“添加新项”对话框中键入 `TypeInferenceRewriter.cs` 作为文件名。

使用指令将以下内容添加到 `TypeInferenceRewriter.cs` 文件：

[!code-csharp[add necessary usings](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#AddUsings "Add required usings")]

接下来，使 `TypeInferenceRewriter` 类扩展 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> 类：

[!code-csharp[add base class](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#BaseClass "Add base class")]

添加以下代码以声明一个私有只读字段，以保存 <xref:Microsoft.CodeAnalysis.SemanticModel> 并在构造函数中将其初始化。 稍后你将需要此字段以确定可以使用类型推理的位置：

[!code-csharp[initialize members](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#Construction "Declare and initialize member variables")]

重写 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter.VisitLocalDeclarationStatement(Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax)> 方法：

```csharp
public override SyntaxNode VisitLocalDeclarationStatement(LocalDeclarationStatementSyntax node)
{

}
```

> [!NOTE]
> 许多 Roslyn API 声明返回类型，它们是返回的实际运行时类型的基类。 在许多情况下，一种类型的节点可能会被另一种节点完全替换，甚至删除。 在此示例中，<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter.VisitLocalDeclarationStatement(Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax)> 方法返回 <xref:Microsoft.CodeAnalysis.SyntaxNode>，而不是派生类型的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax>。 此重写工具根据现有节点返回一个新的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax>。

本快速入门教程处理本地变量声明。 你无法将其扩展到其他声明，如 `foreach` 循环、`for` 循环、LINQ 表达式和 lambda 表达式。 此外，此重写工具仅转换最简单形式的声明：

```csharp
Type variable = expression;
```

如果想要自行浏览，请考虑扩展这些类型变量声明的已完成示例：

```csharp
// Multiple variables in a single declaration.
Type variable1 = expression1,
     variable2 = expression2;
// No initializer.
Type variable;
```

将以下代码添加到 `VisitLocalDeclarationStatement` 方法主体以跳过重写这些形式的声明：

[!code-csharp[exclude other declarations](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#Exclusions "Exclude variables declarations not processed by this sample")]

该方法表明，通过返回未修改的 `node` 参数没有发生重写。 如果上述两个 `if` 表达式都为 true，则该节点表示一个可能的初始化声明。 添加这些语句以提取声明中指定的类型名称，并使用 <xref:Microsoft.CodeAnalysis.SemanticModel> 字段将其绑定来获取类型符号：

[!code-csharp[extract type name](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#ExtractTypeSymbol "Extract the type name specified by the declaration")]

现在，添加此语句以绑定初始值设定项表达式：

[!code-csharp[bind initializer](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#BindInitializer "Bind the initializer expressions")]

最后，如果初始值设定项表达式的类型与指定类型相匹配，则添加以下 `if` 语句，将现有类型名称替换为 `var` 关键字：

[!code-csharp[ReplaceNode](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#ReplaceNode "Replace the initializer node")]

此条件是必需的，因为声明可能将初始值设定项表达式转换为基类或接口。 如果这是需要的，则分配左侧和右侧的类型不匹配。 在这些情况下删除显式类型将更改程序语义。 `var` 指定为标识符而不是关键字，因为 `var` 是上下文关键字。 前导和尾随琐事（空白）从旧类型名转换为 `var` 关键字以保持垂直空白和缩进。 使用 `ReplaceNode`（而非 `With*`来转换 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> 更为简单，因为类型名称实际上是声明语句的孙级。

你已完成 `TypeInferenceRewriter`。 现在返回到 `Program.cs` 文件来完成该示例。 创建测试 <xref:Microsoft.CodeAnalysis.Compilation> 并从中获取 <xref:Microsoft.CodeAnalysis.SemanticModel>。 使用该 <xref:Microsoft.CodeAnalysis.SemanticModel> 尝试 `TypeInferenceRewriter`。 你将在最后执行此步骤。 在此期间，声明一个表示测试编译的占位符变量：

[!code-csharp[DeclareCompilation](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#DeclareTestCompilation "Declare the test compilation")]

暂停一段时间后，应会看到错误波形曲线，报告不存在 `CreateTestCompilation` 方法。 按 Ctrl+句点打开灯泡，然后按 Enter 以调用“生成方法存根(Stub)”命令。 此命令将在 `Program` 类中生成 `CreateTestCompilation` 方法的方法存根(Stub)。 稍后你将返回填写此方法：

![使用中的 C# 生成方法](./media/syntax-transformation/generate-from-usage.png)

编写以下代码以循环访问测试 <xref:Microsoft.CodeAnalysis.Compilation> 中的每个 <xref:Microsoft.CodeAnalysis.SyntaxTree>。 对于每一个，都使用 <xref:Microsoft.CodeAnalysis.SemanticModel> 初始化该树的新 `TypeInferenceRewriter`：

[!code-csharp[IterateTrees](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#IterateTrees "Iterate all the source trees in the test compilation")]

在你创建的 `foreach` 语句中，添加以下代码以在每个源树上执行转换。 如果进行了任何编辑，这段代码将有条件地写出新的转换树。 如果遇到一个或多个可以使用类型推理进行简化的本地变量声明，则重写工具应该只修改一个树：

[!code-csharp[TransformTrees](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#TransformTrees "Transform and save any trees that are modified by the rewriter")]

应看到 `File.WriteAllText` 代码下的波形曲线。 选择灯泡，并添加所需的 `using System.IO;` 语句。

即将完成！ 还剩一步，即创建测试 <xref:Microsoft.CodeAnalysis.Compilation>。 因为你在本快速入门教程期间尚未使用类型推理，所以它将是一个完美的测试用例。 遗憾的是，从 C# 项目文件中创建编译不在本演练范围内。 但幸运的是，如果你已仔细按照说明进行操作，那还是有希望的。 将 `CreateTestCompilation` 方法的内容替换为以下代码。 它将创建一个与本快速入门教程所述的项目相匹配的测试编译：

[!code-csharp[CreateTestCompilation](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#CreateTestCompilation "Create a test compilation using the code written for this quickstart.")]

运行项目，祈求好运吧。 在 Visual Studio 中，选择“调试” > “启动调试”。 应该会收到 Visual Studio 的提醒，指示项目中的文件已更改。 单击“全部同意”以重载已修改的文件。 检查这些文件以观察效果。 请注意，如果没有所有那些显式和冗余类型的说明符，代码会看起来更加简洁。

祝贺你！ 你已使用编译器 API 编写你自己的重构，以便在 C# 项目的所有文件中搜索某些语法模式、分析匹配这些模式的源代码语义，并对其进行转换。 现在，你已正式成为重构作者了！
