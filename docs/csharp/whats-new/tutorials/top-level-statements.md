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
# <a name="tutorial-explore-ideas-using-top-level-statements-to-build-code-as-you-learn"></a><span data-ttu-id="2fa9f-103">教程：在学习过程中，探索使用顶级语句生成代码的想法</span><span class="sxs-lookup"><span data-stu-id="2fa9f-103">Tutorial: Explore ideas using top-level statements to build code as you learn</span></span>

<span data-ttu-id="2fa9f-104">本教程介绍以下操作：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-104">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="2fa9f-105">了解控制顶层语句使用的规则。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-105">Learn the rules governing your use of top-level statements.</span></span>
> - <span data-ttu-id="2fa9f-106">使用顶级语句了解算法。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-106">Use top-level statements to explore algorithms.</span></span>
> - <span data-ttu-id="2fa9f-107">重构对可重用组件的探索。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-107">Refactor explorations into reusable components.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2fa9f-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="2fa9f-108">Prerequisites</span></span>

<span data-ttu-id="2fa9f-109">需要将计算机设置为运行 .NET 5，其中包括 C# 9 编译器。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-109">You'll need to set up your machine to run .NET 5, which includes the C# 9 compiler.</span></span> <span data-ttu-id="2fa9f-110">自 [Visual Studio 2019 版本 16.9 预览 1](https://visualstudio.microsoft.com/vs/preview/) 或 [.NET 5.0 SDK](https://dot.net/get-dotnet5) 起，开始随附 C# 9 编译器。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-110">The C# 9 compiler is available starting with [Visual Studio 2019 version 16.9 preview 1](https://visualstudio.microsoft.com/vs/preview/) or [.NET 5.0 SDK](https://dot.net/get-dotnet5).</span></span>

<span data-ttu-id="2fa9f-111">本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-111">This tutorial assumes you're familiar with C# and .NET, including either Visual Studio or the .NET Core CLI.</span></span>

## <a name="start-exploring"></a><span data-ttu-id="2fa9f-112">开始探索</span><span class="sxs-lookup"><span data-stu-id="2fa9f-112">Start exploring</span></span>

<span data-ttu-id="2fa9f-113">借助顶级语句，你可以将程序的入口点置于类的静态方法中，以避免额外的工作。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-113">Top-level statements enable you to avoid the extra ceremony required by placing your program's entry point in a static method in a class.</span></span> <span data-ttu-id="2fa9f-114">新控制台应用程序的典型起点类似于以下代码：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-114">The typical starting point for a new console application looks like the following code:</span></span>

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

<span data-ttu-id="2fa9f-115">上面的代码是运行 `dotnet new console` 命令并创建新控制台应用程序的结果。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-115">The preceding code is the result of running the `dotnet new console` command and creating a new console application.</span></span> <span data-ttu-id="2fa9f-116">这 11 行只包含一行可执行代码。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-116">Those 11 lines contain only one line of executable code.</span></span> <span data-ttu-id="2fa9f-117">可以通过新的顶级语句功能简化该程序。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-117">You can simplify that program with the new top-level statements feature.</span></span> <span data-ttu-id="2fa9f-118">由此，你可以删除此程序中除两行以外的所有行：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-118">That enables you to remove all but two of the lines in this program:</span></span>

```csharp
using System;

Console.WriteLine("Hello World!");
```

<span data-ttu-id="2fa9f-119">此功能简化了开始探索新想法所需的操作。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-119">This feature simplifies what's needed to begin exploring new ideas.</span></span> <span data-ttu-id="2fa9f-120">你可以将顶级语句用于脚本编写场景，或用于探索。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-120">You can use top-level statements for scripting scenarios, or to explore.</span></span> <span data-ttu-id="2fa9f-121">掌握基础知识后，便可以开始重构代码，并为生成的可重用组件创建方法、类或其他程序集。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-121">Once you've got the basics working, you can start refactoring the code and create methods, classes, or other assemblies for reusable components you've built.</span></span> <span data-ttu-id="2fa9f-122">顶级语句支持快速试验和初学者教程。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-122">Top-level statements do enable quick experimentation and beginner tutorials.</span></span> <span data-ttu-id="2fa9f-123">它们还提供一条从试验到完整程序的平滑路径。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-123">They also provide a smooth path from experimentation to full programs.</span></span>

<span data-ttu-id="2fa9f-124">顶级语句按照其在文件中出现的顺序执行。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-124">Top-level statements are executed in the order they appear in the file.</span></span> <span data-ttu-id="2fa9f-125">顶级语句只能用在应用程序的一个源文件中。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-125">Top-level statements can only be used in one source file in your application.</span></span> <span data-ttu-id="2fa9f-126">如果将其用于多个文件，编译器将生成错误。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-126">The compiler generates an error if you use them in more than one file.</span></span>

## <a name="build-a-magic-net-answer-machine"></a><span data-ttu-id="2fa9f-127">构建神奇的 .NET 应答机</span><span class="sxs-lookup"><span data-stu-id="2fa9f-127">Build a magic .NET answer machine</span></span>

<span data-ttu-id="2fa9f-128">对于本教程，让我们构建一个控制台应用程序，该应用程序使用随机答案回答“是”或“否”问题。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-128">For this tutorial, let's build a console application that answers a "yes" or "no" question with a random answer.</span></span> <span data-ttu-id="2fa9f-129">你将逐步生成功能。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-129">You'll build out the functionality step by step.</span></span> <span data-ttu-id="2fa9f-130">你可以专注于你的任务，而不是典型程序结构所需的工作。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-130">You can focus on your task rather than ceremony needed for the structure of a typical program.</span></span> <span data-ttu-id="2fa9f-131">然后，当你满意该功能后，可根据自己的情况重构应用程序。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-131">Then, once you're happy with the functionality, you can refactor the application as you see fit.</span></span>

<span data-ttu-id="2fa9f-132">将问题写回控制台是一个良好的起点。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-132">A good starting point is to write the question back to the console.</span></span> <span data-ttu-id="2fa9f-133">首先，可以编写以下代码：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-133">You can start by writing the following code:</span></span>

```csharp
using System;

Console.WriteLine(args);
```

<span data-ttu-id="2fa9f-134">不声明 `args` 变量。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-134">You don't declare an `args` variable.</span></span> <span data-ttu-id="2fa9f-135">对于包含顶级语句的单个源文件，编译器会将 `args` 识别为表示命令行参数。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-135">For the single source file that contains your top-level statements, the compiler recognizes `args` to mean the command-line arguments.</span></span> <span data-ttu-id="2fa9f-136">参数的类型是一个 `string[]`，就像在所有 C# 程序中一样。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-136">The type of args is a `string[]`, as in all C# programs.</span></span>

<span data-ttu-id="2fa9f-137">你可以运行以下 `dotnet run` 命令对代码进行测试：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-137">You can test your code by running the following `dotnet run` command:</span></span>

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?
```

<span data-ttu-id="2fa9f-138">命令行上 `--` 后面的参数将传递给程序。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-138">The arguments after the `--` on the command line are passed to the program.</span></span> <span data-ttu-id="2fa9f-139">你可以查看 `args` 变量的类型，因为该类型会输出到控制台：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-139">You can see the type of the `args` variable, because that's what's printed to the console:</span></span>

```console
System.String[]
```

<span data-ttu-id="2fa9f-140">若要将问题写入控制台，需要枚举参数，并使用空格分隔参数。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-140">To write the question to the console, you'll need to enumerate the arguments and separate them with a space.</span></span> <span data-ttu-id="2fa9f-141">将 `WriteLine` 调用替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-141">Replace the `WriteLine` call with the following code:</span></span>

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="EchoInput":::

<span data-ttu-id="2fa9f-142">现在，运行该程序时，它会将问题正确地显示为参数字符串。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-142">Now, when you run the program, it will correctly display the question as a string of arguments.</span></span>

## <a name="respond-with-a-random-answer"></a><span data-ttu-id="2fa9f-143">使用随机答案响应</span><span class="sxs-lookup"><span data-stu-id="2fa9f-143">Respond with a random answer</span></span>

<span data-ttu-id="2fa9f-144">在回显问题后，你可以添加代码以生成随机答案。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-144">After echoing the question, you can add the code to generate the random answer.</span></span> <span data-ttu-id="2fa9f-145">首先添加可能的答案的数组：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-145">Start by adding an array of possible answers:</span></span>

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="Answers":::

<span data-ttu-id="2fa9f-146">此数组有 10 个肯定答案，5 个态度不明确的答案，以及 5 个否定答案。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-146">This array has ten answers that are affirmative, five that are non-committal, and five that are negative.</span></span> <span data-ttu-id="2fa9f-147">接下来，添加以下代码以生成并显示来自数组的随机答案：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-147">Next, add the following code to generate and display a random answer from the array:</span></span>

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="GenerateAnswer":::

<span data-ttu-id="2fa9f-148">你可以再次运行该应用程序以查看结果。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-148">You can run the application again to see the results.</span></span> <span data-ttu-id="2fa9f-149">显示的内容应与以下输出类似：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-149">You should see something like the following output:</span></span>

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?

Should I use top level statements in all my programs?
Better not tell you now.
```

<span data-ttu-id="2fa9f-150">此代码回答了这些问题，但我们再添加一个功能。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-150">This code answers the questions, but let's add one more feature.</span></span> <span data-ttu-id="2fa9f-151">你想让你的问题应用模拟对答案的思考。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-151">You'd like your question app to simulate thinking about the answer.</span></span> <span data-ttu-id="2fa9f-152">可添加一小段 ASCII 动画并在工作时暂停，以实现此目的。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-152">You can do that by adding a bit of ASCII animation, and pausing while working.</span></span>  <span data-ttu-id="2fa9f-153">在回显问题的行后添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-153">Add the following code after the line that echoes the question:</span></span>

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="AnimationFirstPass":::

<span data-ttu-id="2fa9f-154">还需要将 `using` 语句添加到源文件顶部：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-154">You'll also need to add a `using` statement to the top of the source file:</span></span>

```csharp
using System.Threading.Tasks;
```

<span data-ttu-id="2fa9f-155">`using` 语句必须位于文件中的任何其他语句之前。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-155">The `using` statements must be before any other statements in the file.</span></span> <span data-ttu-id="2fa9f-156">否则，会出现编译器错误。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-156">Otherwise, it's a compiler error.</span></span> <span data-ttu-id="2fa9f-157">可以再次运行该程序，并查看动画。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-157">You can run the program again and see the animation.</span></span> <span data-ttu-id="2fa9f-158">这样可以获得更好的体验。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-158">That makes a better experience.</span></span> <span data-ttu-id="2fa9f-159">试验一下延迟的时长，使其与你的喜好匹配。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-159">Experiment with the length of the delay to match your taste.</span></span>

<span data-ttu-id="2fa9f-160">上面的代码创建一组由空格分隔的旋转行。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-160">The preceding code creates a set of spinning lines separated by a space.</span></span> <span data-ttu-id="2fa9f-161">添加 `await` 关键字可指示编译器生成程序入口点作为具有 `async` 修饰符的方法，并返回 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-161">Adding the `await` keyword instructs the compiler to generate the program entry point as a method that has the `async` modifier, and returns a <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>.</span></span> <span data-ttu-id="2fa9f-162">此程序不返回值，因此程序入口点返回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-162">This program does not return a value, so the program entry point returns a `Task`.</span></span> <span data-ttu-id="2fa9f-163">如果程序返回整数值，你可将 return 语句添加到顶级语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-163">If your program returns an integer value, you would add a return statement to the end of your top-level statements.</span></span> <span data-ttu-id="2fa9f-164">该 return 语句将指定要返回的整数值。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-164">That return statement would specify the integer value to return.</span></span> <span data-ttu-id="2fa9f-165">如果顶级语句包含 `await` 表达式，返回类型将变为 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-165">If your top-level statements include an `await` expression, the return type becomes <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>.</span></span>

## <a name="refactoring-for-the-future"></a><span data-ttu-id="2fa9f-166">为满足未来需要而重构</span><span class="sxs-lookup"><span data-stu-id="2fa9f-166">Refactoring for the future</span></span>

<span data-ttu-id="2fa9f-167">程序应类似于以下代码：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-167">Your program should look like the following code:</span></span>

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

<span data-ttu-id="2fa9f-168">前面的代码合理。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-168">The preceding code is reasonable.</span></span> <span data-ttu-id="2fa9f-169">有效。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-169">It works.</span></span> <span data-ttu-id="2fa9f-170">但它无法重用。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-170">But it isn't reusable.</span></span> <span data-ttu-id="2fa9f-171">现在，应用程序正在运行，可以提取可重用的部分。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-171">Now that you have the application working, it's time to pull out reusable parts.</span></span>

<span data-ttu-id="2fa9f-172">一个候选项是显示等待动画的代码。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-172">One candidate is the code that displays the waiting animation.</span></span> <span data-ttu-id="2fa9f-173">该代码片段可以成为一种方法：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-173">That snippet can become a method:</span></span>

<span data-ttu-id="2fa9f-174">首先，可以在文件中创建本地函数。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-174">You can start by creating a local function in your file.</span></span> <span data-ttu-id="2fa9f-175">将当前动画替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-175">Replace the current animation with the following code:</span></span>

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

<span data-ttu-id="2fa9f-176">前面的代码在 main 方法中创建本地函数。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-176">The preceding code creates a local function inside your main method.</span></span> <span data-ttu-id="2fa9f-177">这仍不可重用。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-177">That's still not reusable.</span></span> <span data-ttu-id="2fa9f-178">因此，将该代码提取到类中。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-178">So, extract that code into a class.</span></span> <span data-ttu-id="2fa9f-179">创建名为 utilities.cs 的新文件，并添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-179">Create a new file named *utilities.cs* and add the following code:</span></span>

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="SnippetUtilities":::

<span data-ttu-id="2fa9f-180">具有顶级语句的文件还可以在顶级语句后，在文件末尾包含命名空间和类型。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-180">A file that has top-level statements can also contain namespaces and types at the end of the file, after the top-level statements.</span></span> <span data-ttu-id="2fa9f-181">但对于本教程，请将动画方法放在一个单独的文件中，使其更易于重复使用。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-181">But for this tutorial you put the animation method in a separate file to make it more readily reusable.</span></span>

<span data-ttu-id="2fa9f-182">最后，你可以清理动画代码以删除一些重复项：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-182">Finally, you can clean the animation code to remove some duplication:</span></span>

:::code language="csharp" source="snippets/top-level-statements/Utilities.cs" ID="Animation":::

<span data-ttu-id="2fa9f-183">现在你有了一个完整的应用程序，并且已经重构了可重用部分供以后使用。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-183">Now you have a complete application, and you've refactored the reusable parts for later use.</span></span> <span data-ttu-id="2fa9f-184">可以从顶级语句中调用新的实用工具方法，如下面主程序的最终版本所示：</span><span class="sxs-lookup"><span data-stu-id="2fa9f-184">You can call the new utility method from your top-level statements, as shown below in the finished version of the main program:</span></span>

:::code language="csharp" source="snippets/top-level-statements/Program.cs":::

<span data-ttu-id="2fa9f-185">这样就增加了对 `Utilities.ShowConsoleAnimation` 的调用，并增加了一条 `using` 语句。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-185">This adds the call to `Utilities.ShowConsoleAnimation`, and adds an additional `using` statement.</span></span>

## <a name="summary"></a><span data-ttu-id="2fa9f-186">摘要</span><span class="sxs-lookup"><span data-stu-id="2fa9f-186">Summary</span></span>

<span data-ttu-id="2fa9f-187">使用顶级语句，可以更轻松地创建简单的程序来探索新的算法。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-187">Top-level statements make it easier to create simple programs for use to explore new algorithms.</span></span> <span data-ttu-id="2fa9f-188">可以尝试使用不同的代码片段来试验算法。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-188">You can experiment with algorithms by trying different snippets of code.</span></span> <span data-ttu-id="2fa9f-189">了解了哪些可用后，可以重构代码，使其更易于维护。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-189">Once you've learned what works, you can refactor the code to be more maintainable.</span></span>

<span data-ttu-id="2fa9f-190">顶级语句可简化基于控制台应用程序的程序。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-190">Top-level statements simplify programs that are based on console applications.</span></span> <span data-ttu-id="2fa9f-191">其中包括 Azure Functions、GitHub 操作和其他小实用工具。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-191">These include Azure functions, GitHub actions, and other small utilities.</span></span> <span data-ttu-id="2fa9f-192">有关详细信息，请参阅[顶级语句（C# 编程指南）](../../programming-guide/main-and-command-args/top-level-statements.md)。</span><span class="sxs-lookup"><span data-stu-id="2fa9f-192">For more information, see [Top-level statements (C# Programming Guide)](../../programming-guide/main-and-command-args/top-level-statements.md).</span></span>
