---
title: 异步编程 - C#
description: 了解 .NET Core 提供的 C# 语言级别异步编程模式。
author: cartermp
ms.date: 05/20/2020
ms.technology: csharp-async
ms.assetid: b878c34c-a78f-419e-a594-a2b44fa521a4
ms.openlocfilehash: bb333e5eb5dfadb1ff7a1fac64627b74bca8d981
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637255"
---
# <a name="asynchronous-programming"></a><span data-ttu-id="fdb66-103">异步编程</span><span class="sxs-lookup"><span data-stu-id="fdb66-103">Asynchronous programming</span></span>

<span data-ttu-id="fdb66-104">如果需要 I/O 绑定（例如从网络请求数据、访问数据库或读取和写入到文件系统），则需要利用异步编程。</span><span class="sxs-lookup"><span data-stu-id="fdb66-104">If you have any I/O-bound needs (such as requesting data from a network, accessing a database, or reading and writing to a file system), you'll want to utilize asynchronous programming.</span></span> <span data-ttu-id="fdb66-105">还可以使用 CPU 绑定代码（例如执行成本高昂的计算），对编写异步代码而言，这是一个不错的方案。</span><span class="sxs-lookup"><span data-stu-id="fdb66-105">You could also have CPU-bound code, such as performing an expensive calculation, which is also a good scenario for writing async code.</span></span>

<span data-ttu-id="fdb66-106">C# 拥有语言级别的异步编程模型，让你能轻松编写异步代码，而无需应付回调或受限于支持异步的库。</span><span class="sxs-lookup"><span data-stu-id="fdb66-106">C# has a language-level asynchronous programming model, which allows for easily writing asynchronous code without having to juggle callbacks or conform to a library that supports asynchrony.</span></span> <span data-ttu-id="fdb66-107">它遵循[基于任务的异步模式 (TAP)](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)。</span><span class="sxs-lookup"><span data-stu-id="fdb66-107">It follows what is known as the [Task-based Asynchronous Pattern (TAP)](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md).</span></span>

## <a name="overview-of-the-asynchronous-model"></a><span data-ttu-id="fdb66-108">异步模型概述</span><span class="sxs-lookup"><span data-stu-id="fdb66-108">Overview of the asynchronous model</span></span>

<span data-ttu-id="fdb66-109">异步编程的核心是 `Task` 和 `Task<T>` 对象，这两个对象对异步操作建模。</span><span class="sxs-lookup"><span data-stu-id="fdb66-109">The core of async programming is the `Task` and `Task<T>` objects, which model asynchronous operations.</span></span> <span data-ttu-id="fdb66-110">它们受关键字 `async` 和 `await` 的支持。</span><span class="sxs-lookup"><span data-stu-id="fdb66-110">They are supported by the `async` and `await` keywords.</span></span> <span data-ttu-id="fdb66-111">在大多数情况下模型十分简单：</span><span class="sxs-lookup"><span data-stu-id="fdb66-111">The model is fairly simple in most cases:</span></span>

- <span data-ttu-id="fdb66-112">对于 I/O 绑定代码，等待一个在 `async` 方法中返回 `Task` 或 `Task<T>` 的操作。</span><span class="sxs-lookup"><span data-stu-id="fdb66-112">For I/O-bound code, you await an operation that returns a `Task` or `Task<T>` inside of an `async` method.</span></span>
- <span data-ttu-id="fdb66-113">对于 CPU 绑定代码，等待一个使用 <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> 方法在后台线程启动的操作。</span><span class="sxs-lookup"><span data-stu-id="fdb66-113">For CPU-bound code, you await an operation that is started on a background thread with the <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="fdb66-114">`await` 关键字有这奇妙的作用。</span><span class="sxs-lookup"><span data-stu-id="fdb66-114">The `await` keyword is where the magic happens.</span></span> <span data-ttu-id="fdb66-115">它控制执行 `await` 的方法的调用方，且它最终允许 UI 具有响应性或服务具有灵活性。</span><span class="sxs-lookup"><span data-stu-id="fdb66-115">It yields control to the caller of the method that performed `await`, and it ultimately allows a UI to be responsive or a service to be elastic.</span></span> <span data-ttu-id="fdb66-116">虽然[有方法](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)可处理 `async` 和 `await` 以外的异步代码，但本文重点介绍语言级构造。</span><span class="sxs-lookup"><span data-stu-id="fdb66-116">While [there are ways](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md) to approach async code other than `async` and `await`, this article focuses on the language-level constructs.</span></span>

### <a name="io-bound-example-download-data-from-a-web-service"></a><span data-ttu-id="fdb66-117">I/O 绑定示例：从 Web 服务下载数据</span><span class="sxs-lookup"><span data-stu-id="fdb66-117">I/O-bound example: Download data from a web service</span></span>

<span data-ttu-id="fdb66-118">你可能需要在按下按钮时从 Web 服务下载某些数据，但不希望阻止 UI 线程。</span><span class="sxs-lookup"><span data-stu-id="fdb66-118">You may need to download some data from a web service when a button is pressed but don't want to block the UI thread.</span></span> <span data-ttu-id="fdb66-119">可执行如下操作来实现：</span><span class="sxs-lookup"><span data-stu-id="fdb66-119">It can be accomplished like this:</span></span>

```csharp
private readonly HttpClient _httpClient = new HttpClient();

downloadButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI as the request
    // from the web service is happening.
    //
    // The UI thread is now free to perform other work.
    var stringData = await _httpClient.GetStringAsync(URL);
    DoSomethingWithData(stringData);
};
```

<span data-ttu-id="fdb66-120">代码表示目的（异步下载数据），而不会在与 `Task` 对象的交互中停滞。</span><span class="sxs-lookup"><span data-stu-id="fdb66-120">The code expresses the intent (downloading data asynchronously) without getting bogged down in interacting with `Task` objects.</span></span>

### <a name="cpu-bound-example-perform-a-calculation-for-a-game"></a><span data-ttu-id="fdb66-121">CPU 绑定示例：为游戏执行计算</span><span class="sxs-lookup"><span data-stu-id="fdb66-121">CPU-bound example: Perform a calculation for a game</span></span>

<span data-ttu-id="fdb66-122">假设你正在编写一个移动游戏，在该游戏中，按下某个按钮将会对屏幕中的许多敌人造成伤害。</span><span class="sxs-lookup"><span data-stu-id="fdb66-122">Say you're writing a mobile game where pressing a button can inflict damage on many enemies on the screen.</span></span> <span data-ttu-id="fdb66-123">执行伤害计算的开销可能极大，而且在 UI 线程中执行计算有可能使游戏在计算执行过程中暂停！</span><span class="sxs-lookup"><span data-stu-id="fdb66-123">Performing the damage calculation can be expensive, and doing it on the UI thread would make the game appear to pause as the calculation is performed!</span></span>

<span data-ttu-id="fdb66-124">此问题的最佳解决方法是启动一个后台线程，它使用 `Task.Run` 执行工作，并使用 `await` 等待其结果。</span><span class="sxs-lookup"><span data-stu-id="fdb66-124">The best way to handle this is to start a background thread, which does the work using `Task.Run`, and await its result using `await`.</span></span> <span data-ttu-id="fdb66-125">这可确保在执行工作时 UI 能流畅运行。</span><span class="sxs-lookup"><span data-stu-id="fdb66-125">This allows the UI to feel smooth as the work is being done.</span></span>

```csharp
private DamageResult CalculateDamageDone()
{
    // Code omitted:
    //
    // Does an expensive calculation and returns
    // the result of that calculation.
}

calculateButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI while CalculateDamageDone()
    // performs its work. The UI thread is free to perform other work.
    var damageResult = await Task.Run(() => CalculateDamageDone());
    DisplayDamage(damageResult);
};
```

<span data-ttu-id="fdb66-126">此代码清楚地表达了按钮的单击事件的目的，它无需手动管理后台线程，而是通过非阻止性的方式来实现。</span><span class="sxs-lookup"><span data-stu-id="fdb66-126">This code clearly expresses the intent of the button's click event, it doesn't require managing a background thread manually, and it does so in a non-blocking way.</span></span>

### <a name="what-happens-under-the-covers"></a><span data-ttu-id="fdb66-127">内部原理</span><span class="sxs-lookup"><span data-stu-id="fdb66-127">What happens under the covers</span></span>

<span data-ttu-id="fdb66-128">异步操作涉及许多移动部分。</span><span class="sxs-lookup"><span data-stu-id="fdb66-128">There are many moving pieces where asynchronous operations are concerned.</span></span> <span data-ttu-id="fdb66-129">若要了解 `Task` 和 `Task<T>` 的内部原理，请参阅[深入了解异步](../standard/async-in-depth.md)一文，以获取详细信息。</span><span class="sxs-lookup"><span data-stu-id="fdb66-129">If you're curious about what's happening underneath the covers of `Task` and `Task<T>`, see the [Async in-depth](../standard/async-in-depth.md) article for more information.</span></span>

<span data-ttu-id="fdb66-130">在 C# 方面，编译器将代码转换为状态机，它将跟踪类似以下内容：到达 `await` 时暂停执行以及后台作业完成时继续执行。</span><span class="sxs-lookup"><span data-stu-id="fdb66-130">On the C# side of things, the compiler transforms your code into a state machine that keeps track of things like yielding execution when an `await` is reached and resuming execution when a background job has finished.</span></span>

<span data-ttu-id="fdb66-131">从理论上讲，这是[异步的承诺模型](https://en.wikipedia.org/wiki/Futures_and_promises)的实现。</span><span class="sxs-lookup"><span data-stu-id="fdb66-131">For the theoretically inclined, this is an implementation of the [Promise Model of asynchrony](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span>

## <a name="key-pieces-to-understand"></a><span data-ttu-id="fdb66-132">需了解的要点</span><span class="sxs-lookup"><span data-stu-id="fdb66-132">Key pieces to understand</span></span>

* <span data-ttu-id="fdb66-133">异步代码可用于 I/O 绑定和 CPU 绑定代码，但在每个方案中有所不同。</span><span class="sxs-lookup"><span data-stu-id="fdb66-133">Async code can be used for both I/O-bound and CPU-bound code, but differently for each scenario.</span></span>
* <span data-ttu-id="fdb66-134">异步代码使用 `Task<T>` 和 `Task`，它们是对后台所完成的工作进行建模的结构。</span><span class="sxs-lookup"><span data-stu-id="fdb66-134">Async code uses `Task<T>` and `Task`, which are constructs used to model work being done in the background.</span></span>
* <span data-ttu-id="fdb66-135">`async` 关键字将方法转换为异步方法，这使你能在其正文中使用 `await` 关键字。</span><span class="sxs-lookup"><span data-stu-id="fdb66-135">The `async` keyword turns a method into an async method, which allows you to use the `await` keyword in its body.</span></span>
* <span data-ttu-id="fdb66-136">应用 `await` 关键字后，它将挂起调用方法，并将控制权返还给调用方，直到等待的任务完成。</span><span class="sxs-lookup"><span data-stu-id="fdb66-136">When the `await` keyword is applied, it suspends the calling method and yields control back to its caller until the awaited task is complete.</span></span>
* <span data-ttu-id="fdb66-137">仅允许在异步方法中使用 `await`。</span><span class="sxs-lookup"><span data-stu-id="fdb66-137">`await` can only be used inside an async method.</span></span>

## <a name="recognize-cpu-bound-and-io-bound-work"></a><span data-ttu-id="fdb66-138">识别 CPU 绑定和 I/O 绑定工作</span><span class="sxs-lookup"><span data-stu-id="fdb66-138">Recognize CPU-bound and I/O-bound work</span></span>

<span data-ttu-id="fdb66-139">本指南的前两个示例演示如何将 `async` 和 `await` 用于 I/O 绑定和 CPU 绑定工作。</span><span class="sxs-lookup"><span data-stu-id="fdb66-139">The first two examples of this guide showed how you could use `async` and `await` for I/O-bound and CPU-bound work.</span></span> <span data-ttu-id="fdb66-140">确定所需执行的操作是 I/O 绑定或 CPU 绑定是关键，因为这会极大影响代码性能，并可能导致某些构造的误用。</span><span class="sxs-lookup"><span data-stu-id="fdb66-140">It's key that you can identify when a job you need to do is I/O-bound or CPU-bound because it can greatly affect the performance of your code and could potentially lead to misusing certain constructs.</span></span>

<span data-ttu-id="fdb66-141">以下是编写代码前应考虑的两个问题：</span><span class="sxs-lookup"><span data-stu-id="fdb66-141">Here are two questions you should ask before you write any code:</span></span>

1. <span data-ttu-id="fdb66-142">你的代码是否会“等待”某些内容，例如数据库中的数据？</span><span class="sxs-lookup"><span data-stu-id="fdb66-142">Will your code be "waiting" for something, such as data from a database?</span></span>

   <span data-ttu-id="fdb66-143">如果答案为“是”，则你的工作是 **I/O 绑定**。</span><span class="sxs-lookup"><span data-stu-id="fdb66-143">If your answer is "yes", then your work is **I/O-bound**.</span></span>

1. <span data-ttu-id="fdb66-144">你的代码是否要执行开销巨大的计算？</span><span class="sxs-lookup"><span data-stu-id="fdb66-144">Will your code be performing an expensive computation?</span></span>

   <span data-ttu-id="fdb66-145">如果答案为“是”，则你的工作是 **CPU 绑定**。</span><span class="sxs-lookup"><span data-stu-id="fdb66-145">If you answered "yes", then your work is **CPU-bound**.</span></span>

<span data-ttu-id="fdb66-146">如果你的工作为 I/O 绑定，请使用 `async` 和 `await`（而不使用 `Task.Run`）。</span><span class="sxs-lookup"><span data-stu-id="fdb66-146">If the work you have is **I/O-bound**, use `async` and `await` *without* `Task.Run`.</span></span> <span data-ttu-id="fdb66-147">不应使用任务并行库。</span><span class="sxs-lookup"><span data-stu-id="fdb66-147">You *should not* use the Task Parallel Library.</span></span> <span data-ttu-id="fdb66-148">相关原因在[深入了解异步](../standard/async-in-depth.md)中说明。</span><span class="sxs-lookup"><span data-stu-id="fdb66-148">The reason for this is outlined in [Async in Depth](../standard/async-in-depth.md).</span></span>

<span data-ttu-id="fdb66-149">如果你的工作属于 CPU 绑定，并且你重视响应能力，请使用 `async` 和 `await`，但在另一个线程上使用 `Task.Run` 生成工作。</span><span class="sxs-lookup"><span data-stu-id="fdb66-149">If the work you have is **CPU-bound** and you care about responsiveness, use `async` and `await`, but spawn off the work on another thread *with* `Task.Run`.</span></span> <span data-ttu-id="fdb66-150">如果该工作同时适用于并发和并行，还应考虑使用[任务并行库](../standard/parallel-programming/task-parallel-library-tpl.md)。</span><span class="sxs-lookup"><span data-stu-id="fdb66-150">If the work is appropriate for concurrency and parallelism, also consider using the [Task Parallel Library](../standard/parallel-programming/task-parallel-library-tpl.md).</span></span>

<span data-ttu-id="fdb66-151">此外，应始终对代码的执行进行测量。</span><span class="sxs-lookup"><span data-stu-id="fdb66-151">Additionally, you should always measure the execution of your code.</span></span> <span data-ttu-id="fdb66-152">例如，你可能会遇到这样的情况：多线程处理时，上下文切换的开销高于 CPU 绑定工作的开销。</span><span class="sxs-lookup"><span data-stu-id="fdb66-152">For example, you may find yourself in a situation where your CPU-bound work is not costly enough compared with the overhead of context switches when multithreading.</span></span> <span data-ttu-id="fdb66-153">每种选择都有折衷，应根据自身情况选择正确的折衷方案。</span><span class="sxs-lookup"><span data-stu-id="fdb66-153">Every choice has its tradeoff, and you should pick the correct tradeoff for your situation.</span></span>

## <a name="more-examples"></a><span data-ttu-id="fdb66-154">更多示例</span><span class="sxs-lookup"><span data-stu-id="fdb66-154">More examples</span></span>

<span data-ttu-id="fdb66-155">下列示例演示了多种使用 C# 编写异步代码的方法。</span><span class="sxs-lookup"><span data-stu-id="fdb66-155">The following examples demonstrate various ways you can write async code in C#.</span></span> <span data-ttu-id="fdb66-156">它们涉及你可能会遇到的一些不同方案。</span><span class="sxs-lookup"><span data-stu-id="fdb66-156">They cover a few different scenarios you may come across.</span></span>

### <a name="extract-data-from-a-network"></a><span data-ttu-id="fdb66-157">从网络提取数据</span><span class="sxs-lookup"><span data-stu-id="fdb66-157">Extract data from a network</span></span>

<span data-ttu-id="fdb66-158">此代码片段从 <https://dotnetfoundation.org> 主页下载 HTML，并计算 HTML 中字符串“.NET”的出现次数。</span><span class="sxs-lookup"><span data-stu-id="fdb66-158">This snippet downloads the HTML from the homepage at <https://dotnetfoundation.org> and counts the number of times the string ".NET" occurs in the HTML.</span></span> <span data-ttu-id="fdb66-159">它使用 ASP.NET 定义 Web API 控制器方法，该方法将执行此任务并返回数字。</span><span class="sxs-lookup"><span data-stu-id="fdb66-159">It uses ASP.NET to define a Web API controller method, which performs this task and returns the number.</span></span>

> [!NOTE]
> <span data-ttu-id="fdb66-160">如果打算在生产代码中进行 HTML 分析，则不要使用正则表达式。</span><span class="sxs-lookup"><span data-stu-id="fdb66-160">If you plan on doing HTML parsing in production code, don't use regular expressions.</span></span> <span data-ttu-id="fdb66-161">改为使用分析库。</span><span class="sxs-lookup"><span data-stu-id="fdb66-161">Use a parsing library instead.</span></span>

```csharp
private readonly HttpClient _httpClient = new HttpClient();

[HttpGet, Route("DotNetCount")]
public async Task<int> GetDotNetCount()
{
    // Suspends GetDotNetCount() to allow the caller (the web server)
    // to accept another request, rather than blocking on this one.
    var html = await _httpClient.GetStringAsync("https://dotnetfoundation.org");

    return Regex.Matches(html, @"\.NET").Count;
}
```

<span data-ttu-id="fdb66-162">以下是为通用 Windows 应用编写的相同方案，当按下按钮时，它将执行相同的任务：</span><span class="sxs-lookup"><span data-stu-id="fdb66-162">Here's the same scenario written for a Universal Windows App, which performs the same task when a Button is pressed:</span></span>

```csharp
private readonly HttpClient _httpClient = new HttpClient();

private async void OnSeeTheDotNetsButtonClick(object sender, RoutedEventArgs e)
{
    // Capture the task handle here so we can await the background task later.
    var getDotNetFoundationHtmlTask = _httpClient.GetStringAsync("https://dotnetfoundation.org");

    // Any other work on the UI thread can be done here, such as enabling a Progress Bar.
    // This is important to do here, before the "await" call, so that the user
    // sees the progress bar before execution of this method is yielded.
    NetworkProgressBar.IsEnabled = true;
    NetworkProgressBar.Visibility = Visibility.Visible;

    // The await operator suspends OnSeeTheDotNetsButtonClick(), returning control to its caller.
    // This is what allows the app to be responsive and not block the UI thread.
    var html = await getDotNetFoundationHtmlTask;
    int count = Regex.Matches(html, @"\.NET").Count;

    DotNetCountLabel.Text = $"Number of .NETs on dotnetfoundation.org: {count}";

    NetworkProgressBar.IsEnabled = false;
    NetworkProgressBar.Visibility = Visibility.Collapsed;
}
```

### <a name="wait-for-multiple-tasks-to-complete"></a><span data-ttu-id="fdb66-163">等待多个任务完成</span><span class="sxs-lookup"><span data-stu-id="fdb66-163">Wait for multiple tasks to complete</span></span>

<span data-ttu-id="fdb66-164">你可能发现自己处于需要并行检索多个数据部分的情况。</span><span class="sxs-lookup"><span data-stu-id="fdb66-164">You may find yourself in a situation where you need to retrieve multiple pieces of data concurrently.</span></span> <span data-ttu-id="fdb66-165">`Task` API 包含两种方法（即 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>），这些方法允许你编写在多个后台作业中执行非阻止等待的异步代码。</span><span class="sxs-lookup"><span data-stu-id="fdb66-165">The `Task` API contains two methods, <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> and <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>, that allow you to write asynchronous code that performs a non-blocking wait on multiple background jobs.</span></span>

<span data-ttu-id="fdb66-166">此示例演示如何为一组 `User` 捕捉 `userId` 数据。</span><span class="sxs-lookup"><span data-stu-id="fdb66-166">This example shows how you might grab `User` data for a set of `userId`s.</span></span>

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<IEnumerable<User>> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = new List<Task<User>>();
    foreach (int userId in userIds)
    {
        getUserTasks.Add(GetUserAsync(userId));
    }

    return await Task.WhenAll(getUserTasks);
}
```

<span data-ttu-id="fdb66-167">以下是另一种更简洁的使用 LINQ 进行编写的方法：</span><span class="sxs-lookup"><span data-stu-id="fdb66-167">Here's another way to write this more succinctly, using LINQ:</span></span>

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<User[]> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = userIds.Select(id => GetUserAsync(id));
    return await Task.WhenAll(getUserTasks);
}
```

<span data-ttu-id="fdb66-168">尽管它的代码较少，但在混合 LINQ 和异步代码时需要谨慎使用。</span><span class="sxs-lookup"><span data-stu-id="fdb66-168">Although it's less code, use caution when mixing LINQ with asynchronous code.</span></span> <span data-ttu-id="fdb66-169">因为 LINQ 使用延迟的执行，因此异步调用将不会像在 `foreach` 循环中那样立刻发生，除非强制所生成的序列通过对 `.ToList()` 或 `.ToArray()` 的调用循环访问。</span><span class="sxs-lookup"><span data-stu-id="fdb66-169">Because LINQ uses deferred (lazy) execution, async calls won't happen immediately as they do in a `foreach` loop unless you force the generated sequence to iterate with a call to `.ToList()` or `.ToArray()`.</span></span>

## <a name="important-info-and-advice"></a><span data-ttu-id="fdb66-170">重要信息和建议</span><span class="sxs-lookup"><span data-stu-id="fdb66-170">Important info and advice</span></span>

<span data-ttu-id="fdb66-171">对于异步编程，有一些细节需要注意，以防止意外行为。</span><span class="sxs-lookup"><span data-stu-id="fdb66-171">With async programming, there are some details to keep in mind that can prevent unexpected behavior.</span></span>

* <span data-ttu-id="fdb66-172">`async` 方法需要在主体中有 `await` 关键字，否则它们将永不暂停！</span><span class="sxs-lookup"><span data-stu-id="fdb66-172">`async` **methods need to have an** `await` **keyword in their body or they will never yield!**</span></span>

  <span data-ttu-id="fdb66-173">这一点需牢记在心。</span><span class="sxs-lookup"><span data-stu-id="fdb66-173">This is important to keep in mind.</span></span> <span data-ttu-id="fdb66-174">如果 `await` 未用在 `async` 方法的主体中，C# 编译器将生成一个警告，但此代码将会以类似普通方法的方式进行编译和运行。</span><span class="sxs-lookup"><span data-stu-id="fdb66-174">If `await` is not used in the body of an `async` method, the C# compiler generates a warning, but the code compiles and runs as if it were a normal method.</span></span> <span data-ttu-id="fdb66-175">这种方式非常低效，因为由 C# 编译器为异步方法生成的状态机将不会完成任何任务。</span><span class="sxs-lookup"><span data-stu-id="fdb66-175">This is incredibly inefficient, as the state machine generated by the C# compiler for the async method is not accomplishing anything.</span></span>

* <span data-ttu-id="fdb66-176">添加“Async”作为编写的每个异步方法名称的后缀。</span><span class="sxs-lookup"><span data-stu-id="fdb66-176">**Add "Async" as the suffix of every async method name you write.**</span></span>

  <span data-ttu-id="fdb66-177">这是 .NET 中的惯例，以便更为轻松地区分同步和异步方法。</span><span class="sxs-lookup"><span data-stu-id="fdb66-177">This is the convention used in .NET to more easily differentiate synchronous and asynchronous methods.</span></span> <span data-ttu-id="fdb66-178">未由代码显式调用的某些方法（如事件处理程序或 Web 控制器方法）并不一定适用。</span><span class="sxs-lookup"><span data-stu-id="fdb66-178">Certain methods that aren't explicitly called by your code (such as event handlers or web controller methods) don't necessarily apply.</span></span> <span data-ttu-id="fdb66-179">由于它们未由代码显式调用，因此对其显式命名并不重要。</span><span class="sxs-lookup"><span data-stu-id="fdb66-179">Because they are not explicitly called by your code, being explicit about their naming isn't as important.</span></span>

* <span data-ttu-id="fdb66-180">`async void` 应仅用于事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="fdb66-180">`async void` **should only be used for event handlers.**</span></span>

  <span data-ttu-id="fdb66-181">`async void` 是允许异步事件处理程序工作的唯一方法，因为事件不具有返回类型（因此无法利用 `Task` 和 `Task<T>`）。</span><span class="sxs-lookup"><span data-stu-id="fdb66-181">`async void` is the only way to allow asynchronous event handlers to work because events do not have return types (thus cannot make use of `Task` and `Task<T>`).</span></span> <span data-ttu-id="fdb66-182">其他任何对 `async void` 的使用都不遵循 TAP 模型，且可能存在一定使用难度，例如：</span><span class="sxs-lookup"><span data-stu-id="fdb66-182">Any other use of `async void` does not follow the TAP model and can be challenging to use, such as:</span></span>

  * <span data-ttu-id="fdb66-183">`async void` 方法中引发的异常无法在该方法外部被捕获。</span><span class="sxs-lookup"><span data-stu-id="fdb66-183">Exceptions thrown in an `async void` method can't be caught outside of that method.</span></span>
  * <span data-ttu-id="fdb66-184">`async void` 方法很难测试。</span><span class="sxs-lookup"><span data-stu-id="fdb66-184">`async void` methods are difficult to test.</span></span>
  * <span data-ttu-id="fdb66-185">`async void` 方法可能会导致不良副作用（如果调用方不希望方法是异步的话）。</span><span class="sxs-lookup"><span data-stu-id="fdb66-185">`async void` methods can cause bad side effects if the caller isn't expecting them to be async.</span></span>

* <span data-ttu-id="fdb66-186">**在 LINQ 表达式中使用异步 lambda 时请谨慎**</span><span class="sxs-lookup"><span data-stu-id="fdb66-186">**Tread carefully when using async lambdas in LINQ expressions**</span></span>

  <span data-ttu-id="fdb66-187">LINQ 中的 Lambda 表达式使用延迟执行，这意味着代码可能在你并不希望结束的时候停止执行。</span><span class="sxs-lookup"><span data-stu-id="fdb66-187">Lambda expressions in LINQ use deferred execution, meaning code could end up executing at a time when you're not expecting it to.</span></span> <span data-ttu-id="fdb66-188">如果编写不正确，将阻塞任务引入其中时可能很容易导致死锁。</span><span class="sxs-lookup"><span data-stu-id="fdb66-188">The introduction of blocking tasks into this can easily result in a deadlock if not written correctly.</span></span> <span data-ttu-id="fdb66-189">此外，此类异步代码嵌套可能会对推断代码的执行带来更多困难。</span><span class="sxs-lookup"><span data-stu-id="fdb66-189">Additionally, the nesting of asynchronous code like this can also make it more difficult to reason about the execution of the code.</span></span> <span data-ttu-id="fdb66-190">Async 和 LINQ 的功能都十分强大，但在结合使用两者时应尽可能小心。</span><span class="sxs-lookup"><span data-stu-id="fdb66-190">Async and LINQ are powerful but should be used together as carefully and clearly as possible.</span></span>

* <span data-ttu-id="fdb66-191">**采用非阻止方式编写等待任务的代码**</span><span class="sxs-lookup"><span data-stu-id="fdb66-191">**Write code that awaits Tasks in a non-blocking manner**</span></span>

  <span data-ttu-id="fdb66-192">通过阻止当前线程来等待 `Task` 完成的方法可能导致死锁和已阻止的上下文线程，且可能需要更复杂的错误处理方法。</span><span class="sxs-lookup"><span data-stu-id="fdb66-192">Blocking the current thread as a means to wait for a `Task` to complete can result in deadlocks and blocked context threads and can require more complex error-handling.</span></span> <span data-ttu-id="fdb66-193">下表提供了关于如何以非阻止方式处理等待任务的指南：</span><span class="sxs-lookup"><span data-stu-id="fdb66-193">The following table provides guidance on how to deal with waiting for tasks in a non-blocking way:</span></span>

  | <span data-ttu-id="fdb66-194">使用以下方式...</span><span class="sxs-lookup"><span data-stu-id="fdb66-194">Use this...</span></span>          | <span data-ttu-id="fdb66-195">而不是…</span><span class="sxs-lookup"><span data-stu-id="fdb66-195">Instead of this...</span></span>           | <span data-ttu-id="fdb66-196">若要执行此操作...</span><span class="sxs-lookup"><span data-stu-id="fdb66-196">When wishing to do this...</span></span>                 |
  |----------------------|------------------------------|--------------------------------------------|
  | `await`              | <span data-ttu-id="fdb66-197">`Task.Wait` 或 `Task.Result`</span><span class="sxs-lookup"><span data-stu-id="fdb66-197">`Task.Wait` or `Task.Result`</span></span> | <span data-ttu-id="fdb66-198">检索后台任务的结果</span><span class="sxs-lookup"><span data-stu-id="fdb66-198">Retrieving the result of a background task</span></span> |
  | `await Task.WhenAny` | `Task.WaitAny`               | <span data-ttu-id="fdb66-199">等待任何任务完成</span><span class="sxs-lookup"><span data-stu-id="fdb66-199">Waiting for any task to complete</span></span>           |
  | `await Task.WhenAll` | `Task.WaitAll`               | <span data-ttu-id="fdb66-200">等待所有任务完成</span><span class="sxs-lookup"><span data-stu-id="fdb66-200">Waiting for all tasks to complete</span></span>          |
  | `await Task.Delay`   | `Thread.Sleep`               | <span data-ttu-id="fdb66-201">等待一段时间</span><span class="sxs-lookup"><span data-stu-id="fdb66-201">Waiting for a period of time</span></span>               |

* <span data-ttu-id="fdb66-202">如果可能，请考虑使用 `ValueTask`</span><span class="sxs-lookup"><span data-stu-id="fdb66-202">**Consider using** `ValueTask` **where possible**</span></span>

  <span data-ttu-id="fdb66-203">从异步方法返回 `Task` 对象可能在某些路径中导致性能瓶颈。</span><span class="sxs-lookup"><span data-stu-id="fdb66-203">Returning a `Task` object from async methods can introduce performance bottlenecks in certain paths.</span></span> <span data-ttu-id="fdb66-204">`Task` 是引用类型，因此使用它意味着分配对象。</span><span class="sxs-lookup"><span data-stu-id="fdb66-204">`Task` is a reference type, so using it means allocating an object.</span></span> <span data-ttu-id="fdb66-205">如果使用 `async` 修饰符声明的方法返回缓存结果或以同步方式完成，那么额外的分配在代码的性能关键部分可能要耗费相当长的时间。</span><span class="sxs-lookup"><span data-stu-id="fdb66-205">In cases where a method declared with the `async` modifier returns a cached result or completes synchronously, the extra allocations can become a significant time cost in performance critical sections of code.</span></span> <span data-ttu-id="fdb66-206">如果这些分配发生在紧凑循环中，则成本会变高。</span><span class="sxs-lookup"><span data-stu-id="fdb66-206">It can become costly if those allocations occur in tight loops.</span></span> <span data-ttu-id="fdb66-207">有关详细信息，请参阅[通用的异步返回类型](whats-new/csharp-7.md#generalized-async-return-types)。</span><span class="sxs-lookup"><span data-stu-id="fdb66-207">For more information, see [generalized async return types](whats-new/csharp-7.md#generalized-async-return-types).</span></span>

* <span data-ttu-id="fdb66-208">**考虑使用** `ConfigureAwait(false)`</span><span class="sxs-lookup"><span data-stu-id="fdb66-208">**Consider using** `ConfigureAwait(false)`</span></span>

  <span data-ttu-id="fdb66-209">常见的问题是“应何时使用 <xref:System.Threading.Tasks.Task.ConfigureAwait(System.Boolean)?displayProperty=nameWithType> 方法？”。</span><span class="sxs-lookup"><span data-stu-id="fdb66-209">A common question is, "when should I use the <xref:System.Threading.Tasks.Task.ConfigureAwait(System.Boolean)?displayProperty=nameWithType> method?".</span></span> <span data-ttu-id="fdb66-210">该方法允许 `Task` 实例配置其 awaiter。</span><span class="sxs-lookup"><span data-stu-id="fdb66-210">The method allows for a `Task` instance to configure its awaiter.</span></span> <span data-ttu-id="fdb66-211">这是一个重要的注意事项，如果设置不正确，可能会影响性能，甚至造成死锁。</span><span class="sxs-lookup"><span data-stu-id="fdb66-211">This is an important consideration and setting it incorrectly could potentially have performance implications and even deadlocks.</span></span> <span data-ttu-id="fdb66-212">有关 `ConfigureAwait` 的详细信息，请参阅 [ConfigureAwait 常见问题解答](https://devblogs.microsoft.com/dotnet/configureawait-faq)。</span><span class="sxs-lookup"><span data-stu-id="fdb66-212">For more information on `ConfigureAwait`, see the [ConfigureAwait FAQ](https://devblogs.microsoft.com/dotnet/configureawait-faq).</span></span>

* <span data-ttu-id="fdb66-213">**编写状态欠缺的代码**</span><span class="sxs-lookup"><span data-stu-id="fdb66-213">**Write less stateful code**</span></span>

  <span data-ttu-id="fdb66-214">请勿依赖全局对象的状态或某些方法的执行。</span><span class="sxs-lookup"><span data-stu-id="fdb66-214">Don't depend on the state of global objects or the execution of certain methods.</span></span> <span data-ttu-id="fdb66-215">请仅依赖方法的返回值。</span><span class="sxs-lookup"><span data-stu-id="fdb66-215">Instead, depend only on the return values of methods.</span></span> <span data-ttu-id="fdb66-216">为什么？</span><span class="sxs-lookup"><span data-stu-id="fdb66-216">Why?</span></span>

  * <span data-ttu-id="fdb66-217">这样更容易推断代码。</span><span class="sxs-lookup"><span data-stu-id="fdb66-217">Code will be easier to reason about.</span></span>
  * <span data-ttu-id="fdb66-218">这样更容易测试代码。</span><span class="sxs-lookup"><span data-stu-id="fdb66-218">Code will be easier to test.</span></span>
  * <span data-ttu-id="fdb66-219">混合异步和同步代码更简单。</span><span class="sxs-lookup"><span data-stu-id="fdb66-219">Mixing async and synchronous code is far simpler.</span></span>
  * <span data-ttu-id="fdb66-220">通常可完全避免争用条件。</span><span class="sxs-lookup"><span data-stu-id="fdb66-220">Race conditions can typically be avoided altogether.</span></span>
  * <span data-ttu-id="fdb66-221">通过依赖返回值，协调异步代码可变得简单。</span><span class="sxs-lookup"><span data-stu-id="fdb66-221">Depending on return values makes coordinating async code simple.</span></span>
  * <span data-ttu-id="fdb66-222">（好处）它非常适用于依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="fdb66-222">(Bonus) it works really well with dependency injection.</span></span>

<span data-ttu-id="fdb66-223">建议的目标是实现代码中完整或接近完整的[引用透明度](https://en.wikipedia.org/wiki/Referential_transparency_%28computer_science%29)。</span><span class="sxs-lookup"><span data-stu-id="fdb66-223">A recommended goal is to achieve complete or near-complete [Referential Transparency](https://en.wikipedia.org/wiki/Referential_transparency_%28computer_science%29) in your code.</span></span> <span data-ttu-id="fdb66-224">这么做能获得可预测、可测试和可维护的代码库。</span><span class="sxs-lookup"><span data-stu-id="fdb66-224">Doing so will result in a predictable, testable, and maintainable codebase.</span></span>

## <a name="other-resources"></a><span data-ttu-id="fdb66-225">其他资源</span><span class="sxs-lookup"><span data-stu-id="fdb66-225">Other resources</span></span>

* <span data-ttu-id="fdb66-226">[深入了解异步](../standard/async-in-depth.md)提供了关于任务如何工作的详细信息。</span><span class="sxs-lookup"><span data-stu-id="fdb66-226">[Async in-depth](../standard/async-in-depth.md) provides more information about how Tasks work.</span></span>
* <span data-ttu-id="fdb66-227">[基于任务的异步编程模型 (C#)](./programming-guide/concepts/async/task-asynchronous-programming-model.md)。</span><span class="sxs-lookup"><span data-stu-id="fdb66-227">[The Task asynchronous programming model (C#)](./programming-guide/concepts/async/task-asynchronous-programming-model.md).</span></span>
* <span data-ttu-id="fdb66-228">由 Lucian Wischik 所著的 [Six Essential Tips for Async](https://channel9.msdn.com/Series/Three-Essential-Tips-for-Async)（关于异步的六个要点）是有关异步编程的绝佳资源。</span><span class="sxs-lookup"><span data-stu-id="fdb66-228">Lucian Wischik's [Six Essential Tips for Async](https://channel9.msdn.com/Series/Three-Essential-Tips-for-Async) is a wonderful resource for async programming.</span></span>
