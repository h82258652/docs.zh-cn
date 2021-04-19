---
title: 生成和使用异步流
description: 本高级教程介绍如何生成和使用异步流。 异步流提供了一种更自然的方式来处理可能以异步方式生成的数据序列。
ms.date: 02/10/2019
ms.technology: csharp-async
ms.custom: mvc
ms.openlocfilehash: 4e2264e88b0df9fe8c4e1ebf9bd154dddb390e4e
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497419"
---
# <a name="tutorial-generate-and-consume-async-streams-using-c-80-and-net-core-30"></a>教程：使用 C# 8.0 和 .NET Core 3.0 生成和使用异步流

C# 8.0 引入了异步流，这可针对流式处理数据源建模  。 数据流经常异步检索或生成元素。 异步流依赖于 .NET Standard 2.1 中引入的新接口。 .NET Core 3.0 以及更高版本支持这些接口。 它们为异步流式处理数据源提供了自然编程模型。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 创建以异步方式生成数据元素序列的数据源。
> - 以异步方式使用该数据源。
> - 支持异步流的取消和捕获的上下文。
> - 识别新接口和数据源何时优先于先前的同步数据序列。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。 自 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 起，开始随附 C# 8 编译器。

将需要创建 [GitHub 访问令牌](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token)，以便可以访问 GitHub GraphQL 终结点。 为 GitHub 访问令牌选择以下权限：

- repo:status
- public_repo

将访问令牌保存在安全位置，以便可以使用它来访问 GitHub API 终结点。

> [!WARNING]
> 保护个人访问令牌。 任何带有你的个人访问令牌的软件都可以使用你的访问权限进行 GitHub API 调用。

本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。

## <a name="run-the-starter-application"></a>运行初学者应用程序

可以从 [csharp/whats-new/tutorials](https://github.com/dotnet/docs/tree/main/docs/csharp/whats-new/tutorials/snippets/generate-consume-asynchronous-streams/start) 文件夹中的 [dotnet/docs](https://github.com/dotnet/docs) 存储库获得本教程中使用的初学者应用程序代码。

初学者应用程序是一个控制台应用程序，它使用 [GitHub GraphQL](https://developer.github.com/v4/) 接口检索最近在 [dotnet/docs](https://github.com/dotnet/docs) 存储库中编写的问题。 首先来看一下以下初学者应用 `Main` 方法的代码：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetStarterAppMain" :::

可以将 `GitHubKey` 环境变量设置为个人访问令牌，也可以将对 `GetEnvVariable` 的调用中的最后一个参数替换为个人访问令牌。 如果要与其他人共享源，请不要将访问代码放在源代码中。 不要将访问代码上传到共享源存储库。

在创建 GitHub 客户端后，`Main` 中的代码将创建一个进度报告对象和一个取消令牌。 创建这些对象之后，`Main` 调用 `runPagedQueryAsync` 来检索最近创建的 250 个问题。 任务完成后，将显示结果。

在运行初学者应用程序时，可以对该应用程序的运行方式进行一些重要观察。  将看到从 GitHub 返回的每个页面的进度报告。 在 GitHub 返回问题的每个新页面之前，可以观察到明显的停顿。 最后，只有在从 GitHub 检索到所有 10 个页面之后，问题才会显示出来。

## <a name="examine-the-implementation"></a>检查实现情况

该实现揭示了你观察到上一部分中讨论的行为的原因。 检查 `runPagedQueryAsync` 的代码：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetRunPagedQuery" :::

让我们集中讨论前面代码的分页算法和异步结构。 （有关 GitHub GraphQL API 的详细信息，可以参考 [GitHub GraphQL 文档](https://developer.github.com/v4/guides/)。）`runPagedQueryAsync` 方法按从最新到最旧的顺序枚举问题。 它每页请求 25 个问题，并检查响应的 `pageInfo` 结构以继续上一页的操作。 这遵循了 GraphQL 对多页响应的标准分页支持。 响应包括 `pageInfo` 对象，该对象包含用于请求上一页的 `hasPreviousPages` 值和 `startCursor` 值。 问题在 `nodes` 数组中。 `runPagedQueryAsync` 方法将这些节点追加到一个数组中，其中包含所有页面的所有结果。

在检索和还原结果页之后，`runPagedQueryAsync` 将报告进度并检查是否取消。 如果已请求取消，`runPagedQueryAsync` 将引发 <xref:System.OperationCanceledException>。

此代码中有几个可以改进的元素。 最重要的是，`runPagedQueryAsync` 必须为返回的所有问题分配存储空间。 该示例在 250 个问题处停止，因为检索所有未决问题需要更多的内存来存储所有检索到的问题。 支持进度报告和取消的协议使得算法在第一次读取时更加难以理解。 涉及更多类型和 API。 必须通过 <xref:System.Threading.CancellationTokenSource> 及其关联的 <xref:System.Threading.CancellationToken> 跟踪通信，以了解在何处请求取消，以及在何处授予取消。

## <a name="async-streams-provide-a-better-way"></a>异步流可提供更好的方法

异步流和关联语言支持解决了所有这些问题。 生成序列的代码现在可以使用 `yield return` 返回用 `async` 修饰符声明的方法中的元素。 可以通过 `await foreach` 循环来使用异步流，就像通过 `foreach` 循环使用任何序列一样。

这些新语言功能依赖于添加到 .NET Standard 2.1 并在 .NET Core 3.0 中实现的三个新接口：

- <xref:System.Collections.Generic.IAsyncEnumerable%601?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IAsyncEnumerator%601?displayProperty=nameWithType>
- <xref:System.IAsyncDisposable?displayProperty=nameWithType>

大多数 C# 开发人员都应该熟悉这三个接口。 它们的行为方式类似于其对应的同步对象：

- <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IEnumerator%601?displayProperty=nameWithType>
- <xref:System.IDisposable?displayProperty=nameWithType>

可能不熟悉的一种类型是 <xref:System.Threading.Tasks.ValueTask?displayProperty=nameWithType>。 `ValueTask` 结构提供了与 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 类类似的 API。 出于性能方面的原因，这些接口中使用了 `ValueTask`。

## <a name="convert-to-async-streams"></a>转换为异步流

接下来，转换 `runPagedQueryAsync` 方法以生成异步流。 首先，更改 `runPagedQueryAsync` 的签名以返回 `IAsyncEnumerable<JToken>`，并从参数列表删除取消令牌和进度对象，如以下代码所示：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetUpdateSignature" :::

起始代码在检索页面时处理每个页面，如以下代码所示：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetProcessPage" :::

将这三行替换为以下代码：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetYieldReturnPage" :::

还可以在此方法中删除前面的 `finalResults` 声明以及你修改的循环之后的 `return` 语句。

已完成更改以生成异步流。 已完成的方法应与以下代码类似：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetGenerateAsyncStream" :::

接下来，将使用集合的代码更改为使用异步流。 在 `Main` 中找到以下处理问题集合的代码：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetEnumerateOldStyle" :::

将该代码替换为以下 `await foreach` 循环：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetEnumerateAsyncStream" :::

新接口 <xref:System.Collections.Generic.IAsyncEnumerator%601> 派生自 <xref:System.IAsyncDisposable>。 这意味着在循环完成时，前面的循环会以异步方式释放流。 可以假设循环类似于以下代码：

```csharp
int num = 0;
var enumerator = runPagedQueryAsync(client, PagedIssueQuery, "docs").GetEnumeratorAsync();
try
{
    while (await enumerator.MoveNextAsync())
    {
        var issue = enumerator.Current;
        Console.WriteLine(issue);
        Console.WriteLine($"Received {++num} issues in total");
    }
} finally
{
    if (enumerator != null)
        await enumerator.DisposeAsync();
}
```

默认情况下，在捕获的上下文中处理流元素。 如果要禁用上下文捕获，请使用 <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> 扩展方法。 有关同步上下文并捕获当前上下文的详细信息，请参阅有关[使用基于任务的异步模式](../../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)的文章。

异步流支持使用与其他 `async` 方法相同的协议的取消。 要支持取消，请按如下所示修改异步迭代器方法的签名：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetGenerateWithCancellation" :::

<xref:System.Runtime.CompilerServices.EnumeratorCancellationAttribute?dipslayProperty=nameWithType> 属性导致编译器生成 <xref:System.Collections.Generic.IAsyncEnumerator%601> 的代码，该代码使传递给 `GetAsyncEnumerator` 的令牌对作为该参数的异步迭代器的主体可见。 在 `runQueryAsync` 中，可以检查令牌的状态，并在请求时取消进一步的工作。

使用另一个扩展方法 <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.WithCancellation%2A>，将取消标记传递给异步流。 可以按如下所示修改枚举问题的循环：

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetEnumerateWithCancellation" :::

可以从 [csharp/whats-new/tutorials](https://github.com/dotnet/docs/tree/main/docs/csharp/whats-new/tutorials/snippets/generate-consume-asynchronous-streams/finished) 文件夹中的 [dotnet/docs](https://github.com/dotnet/docs) 存储库获得完成教程的代码。

## <a name="run-the-finished-application"></a>运行完成的应用程序

再次运行该应用程序。 将其行为与初学者应用程序的行为进行对比。 会在结果的第一页可用立即对其进行枚举。 在请求和检索每个新页面时都会有一个可观察到的暂停，然后快速枚举下一页结果。 不需要 `try` / `catch` 块来处理取消：调用者可以停止枚举集合。 由于异步流在下载每个页面时生成结果，因此可以清楚地报告进度。 返回的每个问题的状态都无缝包含在 `await foreach` 循环中。 不需要回调对象即可跟踪进度。

通过检查代码，可以看到内存使用方面的改进。 不再需要在枚举所有结果之前分配一个集合来存储它们。 调用者可以决定如何使用结果，以及是否需要存储集合。

运行初学者应用程序和已完成的应用程序，可以自行观察实现之间的差异。 可以在完成本教程后删除在开始学习本教程时创建的 GitHub 访问令牌。 如果攻击者获得了对该令牌的访问权限，他们可以使用你的凭据来访问 GitHub API。
