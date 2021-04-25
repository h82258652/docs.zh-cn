---
title: 顶级语句 - C# 教程
description: 本教程介绍如何使用顶级语句来试验和证明概念，同时探索你的想法
ms.date: 10/28/2020
ms.openlocfilehash: 60044b58fb52302cd559d7e8802657b602f1b6eb
ms.sourcegitcommit: 985c603cb21a085f8a8105f34ff5b87a44b76ab4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2021
ms.locfileid: "107564826"
---
# <a name="tutorial-explore-ideas-using-top-level-statements-to-build-code-as-you-learn"></a>教程：在学习过程中，探索使用顶级语句生成代码的想法

本教程介绍以下操作：

> [!div class="checklist"]
>
> - 了解控制顶层语句使用的规则。
> - 使用顶级语句了解算法。
> - 重构对可重用组件的探索。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET 5，其中包括 C# 9 编译器。 自 [Visual Studio 2019 版本 16.9 预览 1](https://visualstudio.microsoft.com/vs/preview/) 或 [.NET 5.0 SDK](https://dot.net/get-dotnet5) 起，开始随附 C# 9 编译器。

本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。

## <a name="start-exploring"></a>开始探索

借助顶级语句，你可以将程序的入口点置于类的静态方法中，以避免额外的工作。 新控制台应用程序的典型起点类似于以下代码：

```csharp
using System;

namespace Application
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

上面的代码是运行 `dotnet new console` 命令并创建新控制台应用程序的结果。 这 11 行只包含一行可执行代码。 可以通过新的顶级语句功能简化该程序。 由此，你可以删除此程序中除两行以外的所有行：

```csharp
using System;

Console.WriteLine("Hello World!");
```

此功能简化了开始探索新想法所需的操作。 你可以将顶级语句用于脚本编写场景，或用于探索。 掌握基础知识后，便可以开始重构代码，并为生成的可重用组件创建方法、类或其他程序集。 顶级语句支持快速试验和初学者教程。 它们还提供一条从试验到完整程序的平滑路径。

顶级语句按照其在文件中出现的顺序执行。 顶级语句只能用在应用程序的一个源文件中。 如果将其用于多个文件，编译器将生成错误。

## <a name="build-a-magic-net-answer-machine"></a>构建神奇的 .NET 应答机

对于本教程，让我们构建一个控制台应用程序，该应用程序使用随机答案回答“是”或“否”问题。 你将逐步生成功能。 你可以专注于你的任务，而不是典型程序结构所需的工作。 然后，当你满意该功能后，可根据自己的情况重构应用程序。

将问题写回控制台是一个良好的起点。 首先，可以编写以下代码：

```csharp
using System;

Console.WriteLine(args);
```

不声明 `args` 变量。 对于包含顶级语句的单个源文件，编译器会将 `args` 识别为表示命令行参数。 参数的类型是一个 `string[]`，就像在所有 C# 程序中一样。

你可以运行以下 `dotnet run` 命令对代码进行测试：

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?
```

命令行上 `--` 后面的参数将传递给程序。 你可以查看 `args` 变量的类型，因为该类型会输出到控制台：

```console
System.String[]
```

若要将问题写入控制台，需要枚举参数，并使用空格分隔参数。 将 `WriteLine` 调用替换为以下代码：

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="EchoInput":::

现在，运行该程序时，它会将问题正确地显示为参数字符串。

## <a name="respond-with-a-random-answer"></a>使用随机答案响应

在回显问题后，你可以添加代码以生成随机答案。 首先添加可能的答案的数组：

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="Answers":::

此数组有 10 个肯定答案，5 个态度不明确的答案，以及 5 个否定答案。 接下来，添加以下代码以生成并显示来自数组的随机答案：

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="GenerateAnswer":::

你可以再次运行该应用程序以查看结果。 显示的内容应与以下输出类似：

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?

Should I use top level statements in all my programs?
Better not tell you now.
```

此代码回答了这些问题，但我们再添加一个功能。 你想让你的问题应用模拟对答案的思考。 可添加一小段 ASCII 动画并在工作时暂停，以实现此目的。  在回显问题的行后添加以下代码：

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="AnimationFirstPass":::

还需要将 `using` 语句添加到源文件顶部：

```csharp
using System.Threading.Tasks;
```

`using` 语句必须位于文件中的任何其他语句之前。 否则，会出现编译器错误。 可以再次运行该程序，并查看动画。 这样可以获得更好的体验。 试验一下延迟的时长，使其与你的喜好匹配。

上面的代码创建一组由空格分隔的旋转行。 添加 `await` 关键字可指示编译器生成程序入口点作为具有 `async` 修饰符的方法，并返回 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>。 此程序不返回值，因此程序入口点返回 `Task`。 如果程序返回整数值，你可将 return 语句添加到顶级语句的末尾。 该 return 语句将指定要返回的整数值。 如果顶级语句包含 `await` 表达式，返回类型将变为 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>。

## <a name="refactoring-for-the-future"></a>为满足未来需要而重构

程序应类似于以下代码：

```csharp
using System;
using System.Threading.Tasks;

Console.WriteLine();
foreach(var s in args)
{
    Console.Write(s);
    Console.Write(' ');
}
Console.WriteLine();

for (int i = 0; i < 20; i++)
{
    Console.Write("| -");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("/ \\");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("- |");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("\\ /");
    await Task.Delay(50);
    Console.Write("\b\b\b");
}
Console.WriteLine();

string[] answers =
{
    "It is certain.",       "Reply hazy, try again.",     "Don't count on it.",
    "It is decidedly so.",  "Ask again later.",           "My reply is no.",
    "Without a doubt.",     "Better not tell you now.",   "My sources say no.",
    "Yes – definitely.",    "Cannot predict now.",        "Outlook not so good.",
    "You may rely on it.",  "Concentrate and ask again.", "Very doubtful.",
    "As I see it, yes.",
    "Most likely.",
    "Outlook good.",
    "Yes.",
    "Signs point to yes.",
};

var index = new Random().Next(answers.Length - 1);
Console.WriteLine(answers[index]);
```

前面的代码合理。 有效。 但它无法重用。 现在，应用程序正在运行，可以提取可重用的部分。

一个候选项是显示等待动画的代码。 该代码片段可以成为一种方法：

首先，可以在文件中创建本地函数。 将当前动画替换为以下代码：

```csharp
await ShowConsoleAnimation();

static async Task ShowConsoleAnimation()
{
    for (int i = 0; i < 20; i++)
    {
        Console.Write("| -");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("/ \\");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("- |");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("\\ /");
        await Task.Delay(50);
        Console.Write("\b\b\b");
    }
    Console.WriteLine();
}
```

前面的代码在 main 方法中创建本地函数。 这仍不可重用。 因此，将该代码提取到类中。 创建名为 utilities.cs 的新文件，并添加以下代码：

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="SnippetUtilities":::

具有顶级语句的文件还可以在顶级语句后，在文件末尾包含命名空间和类型。 但对于本教程，请将动画方法放在一个单独的文件中，使其更易于重复使用。

最后，你可以清理动画代码以删除一些重复项：

:::code language="csharp" source="snippets/top-level-statements/Utilities.cs" ID="Animation":::

现在你有了一个完整的应用程序，并且已经重构了可重用部分供以后使用。 可以从顶级语句中调用新的实用工具方法，如下面主程序的最终版本所示：

:::code language="csharp" source="snippets/top-level-statements/Program.cs":::

这样就增加了对 `Utilities.ShowConsoleAnimation` 的调用，并增加了一条 `using` 语句。

## <a name="summary"></a>摘要

使用顶级语句，可以更轻松地创建简单的程序来探索新的算法。 可以尝试使用不同的代码片段来试验算法。 了解了哪些可用后，可以重构代码，使其更易于维护。

顶级语句可简化基于控制台应用程序的程序。 其中包括 Azure Functions、GitHub 操作和其他小实用工具。 有关详细信息，请参阅[顶级语句（C# 编程指南）](../../programming-guide/main-and-command-args/top-level-statements.md)。
