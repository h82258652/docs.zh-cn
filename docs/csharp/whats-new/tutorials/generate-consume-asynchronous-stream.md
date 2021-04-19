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
# <a name="tutorial-generate-and-consume-async-streams-using-c-80-and-net-core-30"></a><span data-ttu-id="fa1f3-104">教程：使用 C# 8.0 和 .NET Core 3.0 生成和使用异步流</span><span class="sxs-lookup"><span data-stu-id="fa1f3-104">Tutorial: Generate and consume async streams using C# 8.0 and .NET Core 3.0</span></span>

<span data-ttu-id="fa1f3-105">C# 8.0 引入了异步流，这可针对流式处理数据源建模  。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-105">C# 8.0 introduces **async streams**, which model a streaming source of data.</span></span> <span data-ttu-id="fa1f3-106">数据流经常异步检索或生成元素。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-106">Data streams often retrieve or generate elements asynchronously.</span></span> <span data-ttu-id="fa1f3-107">异步流依赖于 .NET Standard 2.1 中引入的新接口。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-107">Async streams rely on new interfaces introduced in .NET Standard 2.1.</span></span> <span data-ttu-id="fa1f3-108">.NET Core 3.0 以及更高版本支持这些接口。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-108">These interfaces are supported in .NET Core 3.0 and later.</span></span> <span data-ttu-id="fa1f3-109">它们为异步流式处理数据源提供了自然编程模型。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-109">They provide a natural programming model for asynchronous streaming data sources.</span></span>

<span data-ttu-id="fa1f3-110">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-110">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="fa1f3-111">创建以异步方式生成数据元素序列的数据源。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-111">Create a data source that generates a sequence of data elements asynchronously.</span></span>
> - <span data-ttu-id="fa1f3-112">以异步方式使用该数据源。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-112">Consume that data source asynchronously.</span></span>
> - <span data-ttu-id="fa1f3-113">支持异步流的取消和捕获的上下文。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-113">Support cancellation and captured contexts for asynchronous streams.</span></span>
> - <span data-ttu-id="fa1f3-114">识别新接口和数据源何时优先于先前的同步数据序列。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-114">Recognize when the new interface and data source are preferred to earlier synchronous data sequences.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa1f3-115">先决条件</span><span class="sxs-lookup"><span data-stu-id="fa1f3-115">Prerequisites</span></span>

<span data-ttu-id="fa1f3-116">需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-116">You'll need to set up your machine to run .NET Core, including the C# 8.0 compiler.</span></span> <span data-ttu-id="fa1f3-117">自 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 起，开始随附 C# 8 编译器。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-117">The C# 8 compiler is available starting with [Visual Studio 2019 version 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download).</span></span>

<span data-ttu-id="fa1f3-118">将需要创建 [GitHub 访问令牌](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token)，以便可以访问 GitHub GraphQL 终结点。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-118">You'll need to create a [GitHub access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token) so that you can access the GitHub GraphQL endpoint.</span></span> <span data-ttu-id="fa1f3-119">为 GitHub 访问令牌选择以下权限：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-119">Select the following permissions for your GitHub Access Token:</span></span>

- <span data-ttu-id="fa1f3-120">repo:status</span><span class="sxs-lookup"><span data-stu-id="fa1f3-120">repo:status</span></span>
- <span data-ttu-id="fa1f3-121">public_repo</span><span class="sxs-lookup"><span data-stu-id="fa1f3-121">public_repo</span></span>

<span data-ttu-id="fa1f3-122">将访问令牌保存在安全位置，以便可以使用它来访问 GitHub API 终结点。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-122">Save the access token in a safe place so you can use it to gain access to the GitHub API endpoint.</span></span>

> [!WARNING]
> <span data-ttu-id="fa1f3-123">保护个人访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-123">Keep your personal access token secure.</span></span> <span data-ttu-id="fa1f3-124">任何带有你的个人访问令牌的软件都可以使用你的访问权限进行 GitHub API 调用。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-124">Any software with your personal access token could make GitHub API calls using your access rights.</span></span>

<span data-ttu-id="fa1f3-125">本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-125">This tutorial assumes you're familiar with C# and .NET, including either Visual Studio or the .NET Core CLI.</span></span>

## <a name="run-the-starter-application"></a><span data-ttu-id="fa1f3-126">运行初学者应用程序</span><span class="sxs-lookup"><span data-stu-id="fa1f3-126">Run the starter application</span></span>

<span data-ttu-id="fa1f3-127">可以从 [csharp/whats-new/tutorials](https://github.com/dotnet/docs/tree/main/docs/csharp/whats-new/tutorials/snippets/generate-consume-asynchronous-streams/start) 文件夹中的 [dotnet/docs](https://github.com/dotnet/docs) 存储库获得本教程中使用的初学者应用程序代码。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-127">You can get the code for the starter application used in this tutorial from the [dotnet/docs](https://github.com/dotnet/docs) repository in the [csharp/whats-new/tutorials](https://github.com/dotnet/docs/tree/main/docs/csharp/whats-new/tutorials/snippets/generate-consume-asynchronous-streams/start) folder.</span></span>

<span data-ttu-id="fa1f3-128">初学者应用程序是一个控制台应用程序，它使用 [GitHub GraphQL](https://developer.github.com/v4/) 接口检索最近在 [dotnet/docs](https://github.com/dotnet/docs) 存储库中编写的问题。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-128">The starter application is a console application that uses the [GitHub GraphQL](https://developer.github.com/v4/) interface to retrieve recent issues written in the [dotnet/docs](https://github.com/dotnet/docs) repository.</span></span> <span data-ttu-id="fa1f3-129">首先来看一下以下初学者应用 `Main` 方法的代码：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-129">Start by looking at the following code for the starter app `Main` method:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetStarterAppMain" :::

<span data-ttu-id="fa1f3-130">可以将 `GitHubKey` 环境变量设置为个人访问令牌，也可以将对 `GetEnvVariable` 的调用中的最后一个参数替换为个人访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-130">You can either set a `GitHubKey` environment variable to your personal access token, or you can replace the last argument in the call to `GetEnvVariable` with your personal access token.</span></span> <span data-ttu-id="fa1f3-131">如果要与其他人共享源，请不要将访问代码放在源代码中。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-131">Don't put your access code in source code if you'll be sharing the source with others.</span></span> <span data-ttu-id="fa1f3-132">不要将访问代码上传到共享源存储库。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-132">Never upload access codes to a shared source repository.</span></span>

<span data-ttu-id="fa1f3-133">在创建 GitHub 客户端后，`Main` 中的代码将创建一个进度报告对象和一个取消令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-133">After creating the GitHub client, the code in `Main` creates a progress reporting object and a cancellation token.</span></span> <span data-ttu-id="fa1f3-134">创建这些对象之后，`Main` 调用 `runPagedQueryAsync` 来检索最近创建的 250 个问题。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-134">Once those objects are created, `Main` calls `runPagedQueryAsync` to retrieve the most recent 250 created issues.</span></span> <span data-ttu-id="fa1f3-135">任务完成后，将显示结果。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-135">After that task has finished, the results are displayed.</span></span>

<span data-ttu-id="fa1f3-136">在运行初学者应用程序时，可以对该应用程序的运行方式进行一些重要观察。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-136">When you run the starter application, you can make some important observations about how this application runs.</span></span>  <span data-ttu-id="fa1f3-137">将看到从 GitHub 返回的每个页面的进度报告。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-137">You'll see progress reported for each page returned from GitHub.</span></span> <span data-ttu-id="fa1f3-138">在 GitHub 返回问题的每个新页面之前，可以观察到明显的停顿。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-138">You can observe a noticeable pause before GitHub returns each new page of issues.</span></span> <span data-ttu-id="fa1f3-139">最后，只有在从 GitHub 检索到所有 10 个页面之后，问题才会显示出来。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-139">Finally, the issues are displayed only after all 10 pages have been retrieved from GitHub.</span></span>

## <a name="examine-the-implementation"></a><span data-ttu-id="fa1f3-140">检查实现情况</span><span class="sxs-lookup"><span data-stu-id="fa1f3-140">Examine the implementation</span></span>

<span data-ttu-id="fa1f3-141">该实现揭示了你观察到上一部分中讨论的行为的原因。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-141">The implementation reveals why you observed the behavior discussed in the previous section.</span></span> <span data-ttu-id="fa1f3-142">检查 `runPagedQueryAsync` 的代码：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-142">Examine the code for `runPagedQueryAsync`:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetRunPagedQuery" :::

<span data-ttu-id="fa1f3-143">让我们集中讨论前面代码的分页算法和异步结构。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-143">Let's concentrate on the paging algorithm and async structure of the preceding code.</span></span> <span data-ttu-id="fa1f3-144">（有关 GitHub GraphQL API 的详细信息，可以参考 [GitHub GraphQL 文档](https://developer.github.com/v4/guides/)。）`runPagedQueryAsync` 方法按从最新到最旧的顺序枚举问题。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-144">(You can consult the [GitHub GraphQL documentation](https://developer.github.com/v4/guides/) for details on the GitHub GraphQL API.) The `runPagedQueryAsync` method enumerates the issues from most recent to oldest.</span></span> <span data-ttu-id="fa1f3-145">它每页请求 25 个问题，并检查响应的 `pageInfo` 结构以继续上一页的操作。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-145">It requests 25 issues per page and examines the `pageInfo` structure of the response to continue with the previous page.</span></span> <span data-ttu-id="fa1f3-146">这遵循了 GraphQL 对多页响应的标准分页支持。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-146">That follows GraphQL's standard paging support for multi-page responses.</span></span> <span data-ttu-id="fa1f3-147">响应包括 `pageInfo` 对象，该对象包含用于请求上一页的 `hasPreviousPages` 值和 `startCursor` 值。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-147">The response includes a `pageInfo` object that includes a `hasPreviousPages` value and a `startCursor` value used to request the previous page.</span></span> <span data-ttu-id="fa1f3-148">问题在 `nodes` 数组中。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-148">The issues are in the `nodes` array.</span></span> <span data-ttu-id="fa1f3-149">`runPagedQueryAsync` 方法将这些节点追加到一个数组中，其中包含所有页面的所有结果。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-149">The `runPagedQueryAsync` method appends these nodes to an array that contains all the results from all pages.</span></span>

<span data-ttu-id="fa1f3-150">在检索和还原结果页之后，`runPagedQueryAsync` 将报告进度并检查是否取消。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-150">After retrieving and restoring a page of results, `runPagedQueryAsync` reports progress and checks for cancellation.</span></span> <span data-ttu-id="fa1f3-151">如果已请求取消，`runPagedQueryAsync` 将引发 <xref:System.OperationCanceledException>。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-151">If cancellation has been requested, `runPagedQueryAsync` throws an <xref:System.OperationCanceledException>.</span></span>

<span data-ttu-id="fa1f3-152">此代码中有几个可以改进的元素。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-152">There are several elements in this code that can be improved.</span></span> <span data-ttu-id="fa1f3-153">最重要的是，`runPagedQueryAsync` 必须为返回的所有问题分配存储空间。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-153">Most importantly, `runPagedQueryAsync` must allocate storage for all the issues returned.</span></span> <span data-ttu-id="fa1f3-154">该示例在 250 个问题处停止，因为检索所有未决问题需要更多的内存来存储所有检索到的问题。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-154">This sample stops at 250 issues because retrieving all open issues would require much more memory to store all the retrieved issues.</span></span> <span data-ttu-id="fa1f3-155">支持进度报告和取消的协议使得算法在第一次读取时更加难以理解。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-155">The protocols for supporting progress reports and cancellation make the algorithm harder to understand on its first reading.</span></span> <span data-ttu-id="fa1f3-156">涉及更多类型和 API。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-156">More types and APIs are involved.</span></span> <span data-ttu-id="fa1f3-157">必须通过 <xref:System.Threading.CancellationTokenSource> 及其关联的 <xref:System.Threading.CancellationToken> 跟踪通信，以了解在何处请求取消，以及在何处授予取消。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-157">You must trace the communications through the <xref:System.Threading.CancellationTokenSource> and its associated <xref:System.Threading.CancellationToken> to understand where cancellation is requested and where it's granted.</span></span>

## <a name="async-streams-provide-a-better-way"></a><span data-ttu-id="fa1f3-158">异步流可提供更好的方法</span><span class="sxs-lookup"><span data-stu-id="fa1f3-158">Async streams provide a better way</span></span>

<span data-ttu-id="fa1f3-159">异步流和关联语言支持解决了所有这些问题。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-159">Async streams and the associated language support address all those concerns.</span></span> <span data-ttu-id="fa1f3-160">生成序列的代码现在可以使用 `yield return` 返回用 `async` 修饰符声明的方法中的元素。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-160">The code that generates the sequence can now use `yield return` to return elements in a method that was declared with the `async` modifier.</span></span> <span data-ttu-id="fa1f3-161">可以通过 `await foreach` 循环来使用异步流，就像通过 `foreach` 循环使用任何序列一样。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-161">You can consume an async stream using an `await foreach` loop just as you consume any sequence using a `foreach` loop.</span></span>

<span data-ttu-id="fa1f3-162">这些新语言功能依赖于添加到 .NET Standard 2.1 并在 .NET Core 3.0 中实现的三个新接口：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-162">These new language features depend on three new interfaces added to .NET Standard 2.1 and implemented in .NET Core 3.0:</span></span>

- <xref:System.Collections.Generic.IAsyncEnumerable%601?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IAsyncEnumerator%601?displayProperty=nameWithType>
- <xref:System.IAsyncDisposable?displayProperty=nameWithType>

<span data-ttu-id="fa1f3-163">大多数 C# 开发人员都应该熟悉这三个接口。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-163">These three interfaces should be familiar to most C# developers.</span></span> <span data-ttu-id="fa1f3-164">它们的行为方式类似于其对应的同步对象：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-164">They behave in a manner similar to their synchronous counterparts:</span></span>

- <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IEnumerator%601?displayProperty=nameWithType>
- <xref:System.IDisposable?displayProperty=nameWithType>

<span data-ttu-id="fa1f3-165">可能不熟悉的一种类型是 <xref:System.Threading.Tasks.ValueTask?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-165">One type that may be unfamiliar is <xref:System.Threading.Tasks.ValueTask?displayProperty=nameWithType>.</span></span> <span data-ttu-id="fa1f3-166">`ValueTask` 结构提供了与 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 类类似的 API。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-166">The `ValueTask` struct provides a similar API to the <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> class.</span></span> <span data-ttu-id="fa1f3-167">出于性能方面的原因，这些接口中使用了 `ValueTask`。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-167">`ValueTask` is used in these interfaces for performance reasons.</span></span>

## <a name="convert-to-async-streams"></a><span data-ttu-id="fa1f3-168">转换为异步流</span><span class="sxs-lookup"><span data-stu-id="fa1f3-168">Convert to async streams</span></span>

<span data-ttu-id="fa1f3-169">接下来，转换 `runPagedQueryAsync` 方法以生成异步流。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-169">Next, convert the `runPagedQueryAsync` method to generate an async stream.</span></span> <span data-ttu-id="fa1f3-170">首先，更改 `runPagedQueryAsync` 的签名以返回 `IAsyncEnumerable<JToken>`，并从参数列表删除取消令牌和进度对象，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-170">First, change the signature of `runPagedQueryAsync` to return an `IAsyncEnumerable<JToken>`, and remove the cancellation token and progress objects from the parameter list as shown in the following code:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetUpdateSignature" :::

<span data-ttu-id="fa1f3-171">起始代码在检索页面时处理每个页面，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-171">The starter code processes each page as the page is retrieved, as shown in the following code:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetProcessPage" :::

<span data-ttu-id="fa1f3-172">将这三行替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-172">Replace those three lines with the following code:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetYieldReturnPage" :::

<span data-ttu-id="fa1f3-173">还可以在此方法中删除前面的 `finalResults` 声明以及你修改的循环之后的 `return` 语句。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-173">You can also remove the declaration of `finalResults` earlier in this method and the `return` statement that follows the loop you modified.</span></span>

<span data-ttu-id="fa1f3-174">已完成更改以生成异步流。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-174">You've finished the changes to generate an async stream.</span></span> <span data-ttu-id="fa1f3-175">已完成的方法应与以下代码类似：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-175">The finished method should resemble the following code:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetGenerateAsyncStream" :::

<span data-ttu-id="fa1f3-176">接下来，将使用集合的代码更改为使用异步流。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-176">Next, you change the code that consumes the collection to consume the async stream.</span></span> <span data-ttu-id="fa1f3-177">在 `Main` 中找到以下处理问题集合的代码：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-177">Find the following code in `Main` that processes the collection of issues:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/start/Program.cs" id="SnippetEnumerateOldStyle" :::

<span data-ttu-id="fa1f3-178">将该代码替换为以下 `await foreach` 循环：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-178">Replace that code with the following `await foreach` loop:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetEnumerateAsyncStream" :::

<span data-ttu-id="fa1f3-179">新接口 <xref:System.Collections.Generic.IAsyncEnumerator%601> 派生自 <xref:System.IAsyncDisposable>。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-179">The new interface <xref:System.Collections.Generic.IAsyncEnumerator%601> derives from <xref:System.IAsyncDisposable>.</span></span> <span data-ttu-id="fa1f3-180">这意味着在循环完成时，前面的循环会以异步方式释放流。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-180">That means the preceding loop will asynchronously dispose the stream when the loop finishes.</span></span> <span data-ttu-id="fa1f3-181">可以假设循环类似于以下代码：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-181">You can imagine the loop looks like the following code:</span></span>

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

<span data-ttu-id="fa1f3-182">默认情况下，在捕获的上下文中处理流元素。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-182">By default, stream elements are processed in the captured context.</span></span> <span data-ttu-id="fa1f3-183">如果要禁用上下文捕获，请使用 <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> 扩展方法。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-183">If you want to disable capturing of the context, use the <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> extension method.</span></span> <span data-ttu-id="fa1f3-184">有关同步上下文并捕获当前上下文的详细信息，请参阅有关[使用基于任务的异步模式](../../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)的文章。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-184">For more information about synchronization contexts and capturing the current context, see the article on [consuming the Task-based asynchronous pattern](../../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md).</span></span>

<span data-ttu-id="fa1f3-185">异步流支持使用与其他 `async` 方法相同的协议的取消。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-185">Async streams support cancellation using the same protocol as other `async` methods.</span></span> <span data-ttu-id="fa1f3-186">要支持取消，请按如下所示修改异步迭代器方法的签名：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-186">You would modify the signature for the async iterator method as follows to support cancellation:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetGenerateWithCancellation" :::

<span data-ttu-id="fa1f3-187"><xref:System.Runtime.CompilerServices.EnumeratorCancellationAttribute?dipslayProperty=nameWithType> 属性导致编译器生成 <xref:System.Collections.Generic.IAsyncEnumerator%601> 的代码，该代码使传递给 `GetAsyncEnumerator` 的令牌对作为该参数的异步迭代器的主体可见。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-187">The <xref:System.Runtime.CompilerServices.EnumeratorCancellationAttribute?dipslayProperty=nameWithType> attribute causes the compiler to generate code for the <xref:System.Collections.Generic.IAsyncEnumerator%601> that makes the token passed to `GetAsyncEnumerator` visible to the body of the async iterator as that argument.</span></span> <span data-ttu-id="fa1f3-188">在 `runQueryAsync` 中，可以检查令牌的状态，并在请求时取消进一步的工作。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-188">Inside `runQueryAsync`, you could examine the state of the token and cancel further work if requested.</span></span>

<span data-ttu-id="fa1f3-189">使用另一个扩展方法 <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.WithCancellation%2A>，将取消标记传递给异步流。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-189">You use another extension method, <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.WithCancellation%2A>, to pass the cancellation token to the async stream.</span></span> <span data-ttu-id="fa1f3-190">可以按如下所示修改枚举问题的循环：</span><span class="sxs-lookup"><span data-stu-id="fa1f3-190">You would modify the loop enumerating the issues as follows:</span></span>

:::code language="csharp" source="snippets/generate-consume-asynchronous-streams/finished/Program.cs" id="SnippetEnumerateWithCancellation" :::

<span data-ttu-id="fa1f3-191">可以从 [csharp/whats-new/tutorials](https://github.com/dotnet/docs/tree/main/docs/csharp/whats-new/tutorials/snippets/generate-consume-asynchronous-streams/finished) 文件夹中的 [dotnet/docs](https://github.com/dotnet/docs) 存储库获得完成教程的代码。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-191">You can get the code for the finished tutorial from the [dotnet/docs](https://github.com/dotnet/docs) repository in the [csharp/whats-new/tutorials](https://github.com/dotnet/docs/tree/main/docs/csharp/whats-new/tutorials/snippets/generate-consume-asynchronous-streams/finished) folder.</span></span>

## <a name="run-the-finished-application"></a><span data-ttu-id="fa1f3-192">运行完成的应用程序</span><span class="sxs-lookup"><span data-stu-id="fa1f3-192">Run the finished application</span></span>

<span data-ttu-id="fa1f3-193">再次运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-193">Run the application again.</span></span> <span data-ttu-id="fa1f3-194">将其行为与初学者应用程序的行为进行对比。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-194">Contrast its behavior with the behavior of the starter application.</span></span> <span data-ttu-id="fa1f3-195">会在结果的第一页可用立即对其进行枚举。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-195">The first page of results is enumerated as soon as it's available.</span></span> <span data-ttu-id="fa1f3-196">在请求和检索每个新页面时都会有一个可观察到的暂停，然后快速枚举下一页结果。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-196">There's an observable pause as each new page is requested and retrieved, then the next page's results are quickly enumerated.</span></span> <span data-ttu-id="fa1f3-197">不需要 `try` / `catch` 块来处理取消：调用者可以停止枚举集合。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-197">The `try` / `catch` block isn't needed to handle cancellation: the caller can stop enumerating the collection.</span></span> <span data-ttu-id="fa1f3-198">由于异步流在下载每个页面时生成结果，因此可以清楚地报告进度。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-198">Progress is clearly reported because the async stream generates results as each page is downloaded.</span></span> <span data-ttu-id="fa1f3-199">返回的每个问题的状态都无缝包含在 `await foreach` 循环中。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-199">The status for each issue returned is seamlessly included in the `await foreach` loop.</span></span> <span data-ttu-id="fa1f3-200">不需要回调对象即可跟踪进度。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-200">You don't need a callback object to track progress.</span></span>

<span data-ttu-id="fa1f3-201">通过检查代码，可以看到内存使用方面的改进。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-201">You can see improvements in memory use by examining the code.</span></span> <span data-ttu-id="fa1f3-202">不再需要在枚举所有结果之前分配一个集合来存储它们。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-202">You no longer need to allocate a collection to store all the results before they're enumerated.</span></span> <span data-ttu-id="fa1f3-203">调用者可以决定如何使用结果，以及是否需要存储集合。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-203">The caller can determine how to consume the results and if a storage collection is needed.</span></span>

<span data-ttu-id="fa1f3-204">运行初学者应用程序和已完成的应用程序，可以自行观察实现之间的差异。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-204">Run both the starter and finished applications and you can observe the differences between the implementations for yourself.</span></span> <span data-ttu-id="fa1f3-205">可以在完成本教程后删除在开始学习本教程时创建的 GitHub 访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-205">You can delete the GitHub access token you created when you started this tutorial after you've finished.</span></span> <span data-ttu-id="fa1f3-206">如果攻击者获得了对该令牌的访问权限，他们可以使用你的凭据来访问 GitHub API。</span><span class="sxs-lookup"><span data-stu-id="fa1f3-206">If an attacker gained access to that token, they could access GitHub APIs using your credentials.</span></span>
