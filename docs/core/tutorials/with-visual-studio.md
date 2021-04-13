---
title: 使用 Visual Studio 创建 .NET 控制台应用程序
description: 了解如何使用 Visual Studio 通过 C# 或 Visual Basic 创建 .NET 控制台应用程序。
ms.date: 03/26/2021
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet,contperf-fy21q3
ms.openlocfilehash: e55927080ab30e7a24c54656b7f11a94a023bd65
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105636800"
---
# <a name="tutorial-create-a-net-console-application-using-visual-studio"></a><span data-ttu-id="f5077-103">教程：使用 Visual Studio 创建 .NET 控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="f5077-103">Tutorial: Create a .NET console application using Visual Studio</span></span>

<span data-ttu-id="f5077-104">本教程演示如何在 Visual Studio 2019 中创建和运行 .NET 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="f5077-104">This tutorial shows how to create and run a .NET console application in Visual Studio 2019.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5077-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="f5077-105">Prerequisites</span></span>

- <span data-ttu-id="f5077-106">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019 版本 16.9.2 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。</span><span class="sxs-lookup"><span data-stu-id="f5077-106">[Visual Studio 2019 version 16.9.2 or a later version](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **.NET Core cross-platform development** workload installed.</span></span> <span data-ttu-id="f5077-107">选择此工作负载时，将自动安装 .NET 5.0 SDK。</span><span class="sxs-lookup"><span data-stu-id="f5077-107">The .NET 5.0 SDK is automatically installed when you select this workload.</span></span>

  <span data-ttu-id="f5077-108">有关详细信息，请参阅[使用 Visual Studio 安装 .NET SDK](../install/windows.md#install-with-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="f5077-108">For more information, see [Install the .NET SDK with Visual Studio](../install/windows.md#install-with-visual-studio).</span></span>

## <a name="create-the-app"></a><span data-ttu-id="f5077-109">创建应用</span><span class="sxs-lookup"><span data-stu-id="f5077-109">Create the app</span></span>

<span data-ttu-id="f5077-110">创建一个名为“HelloWorld”的 .NET 控制台应用项目。</span><span class="sxs-lookup"><span data-stu-id="f5077-110">Create a .NET console app project named "HelloWorld".</span></span>

1. <span data-ttu-id="f5077-111">启动 Visual Studio 2019。</span><span class="sxs-lookup"><span data-stu-id="f5077-111">Start Visual Studio 2019.</span></span>

1. <span data-ttu-id="f5077-112">在“开始”页上，选择“创建新项目”。</span><span class="sxs-lookup"><span data-stu-id="f5077-112">On the start page, choose **Create a new project**.</span></span>

   :::image type="content" source="./media/with-visual-studio/start-window.png" alt-text="在 Visual Studio“开始”页选择“创建新项目”按钮":::

1. <span data-ttu-id="f5077-114">在“创建新项目”页面，在搜索框中输入“控制台”。</span><span class="sxs-lookup"><span data-stu-id="f5077-114">On the **Create a new project** page, enter **console** in the search box.</span></span> <span data-ttu-id="f5077-115">接下来，从“语言”列表中选择“C#”或“Visual Basic”，然后从“平台”列表中选择“所有平台”  。</span><span class="sxs-lookup"><span data-stu-id="f5077-115">Next, choose **C#** or **Visual Basic** from the language list, and then choose **All platforms** from the platform list.</span></span> <span data-ttu-id="f5077-116">选择“控制台应用程序”模板，然后选择“下一步” 。</span><span class="sxs-lookup"><span data-stu-id="f5077-116">Choose the **Console Application** template, and then choose **Next**.</span></span>

   :::image type="content" source="./media/with-visual-studio/create-new-project.png" alt-text="使用所选筛选器创建新项目窗口":::

   > [!TIP]
   > <span data-ttu-id="f5077-118">如果看不到 .NET 模板，则可能缺少所需的工作负载。</span><span class="sxs-lookup"><span data-stu-id="f5077-118">If you don't see the .NET templates, you're probably missing the required workload.</span></span> <span data-ttu-id="f5077-119">在“找不到所需内容?”消息下，选择“安装更多工具和功能”链接。</span><span class="sxs-lookup"><span data-stu-id="f5077-119">Under the **Not finding what you're looking for?** message, choose the **Install more tools and features** link.</span></span> <span data-ttu-id="f5077-120">Visual Studio 安装程序随即打开。</span><span class="sxs-lookup"><span data-stu-id="f5077-120">The Visual Studio Installer opens.</span></span> <span data-ttu-id="f5077-121">确保安装了“.NET Core 跨平台开发”工作负载。</span><span class="sxs-lookup"><span data-stu-id="f5077-121">Make sure you have the **.NET Core cross-platform development** workload installed.</span></span>

1. <span data-ttu-id="f5077-122">在“配置新项目”对话框中，在“项目名称”框中输入“HelloWorld”。</span><span class="sxs-lookup"><span data-stu-id="f5077-122">In the **Configure your new project** dialog,  enter **HelloWorld** in the **Project name** box.</span></span> <span data-ttu-id="f5077-123">然后选择“下一步”  。</span><span class="sxs-lookup"><span data-stu-id="f5077-123">Then choose **Next**.</span></span>

   :::image type="content" source="./media/with-visual-studio/configure-new-project.png" alt-text="为新项目窗口配置“项目名称”、“位置”和“解决方案名称”字段":::

1. <span data-ttu-id="f5077-125">在“其他信息”对话框中，选择“.NET 5.0 (当前)”，然后选择“创建”  。</span><span class="sxs-lookup"><span data-stu-id="f5077-125">In the **Additional information** dialog, select **.NET 5.0 (Current)**, and then select **Create**.</span></span>

   :::image type="content" source="media/with-visual-studio/additional-info.png" alt-text="“其他信息”对话框":::

<span data-ttu-id="f5077-127">用于创建简单的“Hello World”应用程序的模板。</span><span class="sxs-lookup"><span data-stu-id="f5077-127">The template creates a simple "Hello World" application.</span></span> <span data-ttu-id="f5077-128">它会调用 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法来显示“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="f5077-128">It calls the <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> method to display "Hello World!"</span></span> <span data-ttu-id="f5077-129">显示文本字符串“Hello World!”。</span><span class="sxs-lookup"><span data-stu-id="f5077-129">in the console window.</span></span>

<span data-ttu-id="f5077-130">模板代码将定义类 `Program`，其中包含一个需要将 <xref:System.String> 数组用作参数的方法 `Main`：</span><span class="sxs-lookup"><span data-stu-id="f5077-130">The template code defines a class, `Program`, with a single method, `Main`, that takes a <xref:System.String> array as an argument:</span></span>

```csharp
using System;

namespace HelloWorld
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

```vb
Imports System

Module Program
    Sub Main(args As String())
        Console.WriteLine("Hello World!")
    End Sub
End Module
```

<span data-ttu-id="f5077-131">`Main` 是应用程序入口点，同时也是在应用程序启动时由运行时自动调用的方法。</span><span class="sxs-lookup"><span data-stu-id="f5077-131">`Main` is the application entry point, the method that's called automatically by the runtime when it launches the application.</span></span> <span data-ttu-id="f5077-132">*args* 数组中包含在应用程序启动时提供的所有命令行自变量。</span><span class="sxs-lookup"><span data-stu-id="f5077-132">Any command-line arguments supplied when the application is launched are available in the *args* array.</span></span>

<span data-ttu-id="f5077-133">如果未显示想要使用的语言，请更改页面顶部的语言选择器。</span><span class="sxs-lookup"><span data-stu-id="f5077-133">If the language you want to use is not shown, change the language selector at the top of the page.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="f5077-134">运行应用</span><span class="sxs-lookup"><span data-stu-id="f5077-134">Run the app</span></span>

1. <span data-ttu-id="f5077-135">按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 运行程序而不进行调试。</span><span class="sxs-lookup"><span data-stu-id="f5077-135">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the program without debugging.</span></span>

   <span data-ttu-id="f5077-136">此时将打开在屏幕上显示文本“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="f5077-136">A console window opens with the text "Hello World!"</span></span> <span data-ttu-id="f5077-137">。</span><span class="sxs-lookup"><span data-stu-id="f5077-137">printed on the screen.</span></span>

   :::image type="content" source="./media/with-visual-studio/hello-world-console.png" alt-text="控制台窗口，其中显示 Hello World Press any key to continue":::

1. <span data-ttu-id="f5077-139">按任意键关闭控制台窗口。</span><span class="sxs-lookup"><span data-stu-id="f5077-139">Press any key to close the console window.</span></span>

## <a name="enhance-the-app"></a><span data-ttu-id="f5077-140">增强应用</span><span class="sxs-lookup"><span data-stu-id="f5077-140">Enhance the app</span></span>

<span data-ttu-id="f5077-141">改进应用程序，使其提示用户输入名字，并将其与日期和时间一同显示。</span><span class="sxs-lookup"><span data-stu-id="f5077-141">Enhance the application to prompt the user for their name and display it along with the date and time.</span></span>

1. <span data-ttu-id="f5077-142">在 Program.cs 或 Program.vb 中，将 `Main` 方法的内容（当前只是调用 `Console.WriteLine` 的行）替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="f5077-142">In *Program.cs* or *Program.vb*, replace the contents of the `Main` method, which is the line that calls `Console.WriteLine`, with the following code:</span></span>

   :::code language="csharp" source="./snippets/with-visual-studio/csharp/Program.cs" id="MainMethod":::
   :::code language="vb" source="./snippets/with-visual-studio/vb/Program.vb" id="MainMethod":::

   <span data-ttu-id="f5077-143">此代码会在控制台窗口中显示一条提示，然后等待用户输入字符串并按 <kbd>Enter</kbd>。</span><span class="sxs-lookup"><span data-stu-id="f5077-143">This code displays a prompt in the console window and waits until the user enters a string followed by the <kbd>Enter</kbd> key.</span></span> <span data-ttu-id="f5077-144">它会将此字符串存储到名为 `name` 的变量中。</span><span class="sxs-lookup"><span data-stu-id="f5077-144">It stores this string in a variable named `name`.</span></span> <span data-ttu-id="f5077-145">它还会检索 <xref:System.DateTime.Now?displayProperty=nameWithType> 属性的值（其中包含当前的本地时间），并将此值赋给 `currentDate` 变量。</span><span class="sxs-lookup"><span data-stu-id="f5077-145">It also retrieves the value of the <xref:System.DateTime.Now?displayProperty=nameWithType> property, which contains the current local time, and assigns it to a variable named `currentDate`.</span></span> <span data-ttu-id="f5077-146">同时会在控制台窗口中显示这些值。</span><span class="sxs-lookup"><span data-stu-id="f5077-146">And it displays these values in the console window.</span></span> <span data-ttu-id="f5077-147">最后会在控制台窗口中显示一条提示，并调用 <xref:System.Console.ReadKey(System.Boolean)?displayProperty=nameWithType> 方法来等待用户输入。</span><span class="sxs-lookup"><span data-stu-id="f5077-147">Finally, it displays a prompt in the console window and calls the <xref:System.Console.ReadKey(System.Boolean)?displayProperty=nameWithType> method to wait for user input.</span></span>

   <span data-ttu-id="f5077-148"><xref:System.Environment.NewLine> 是一种独立于平台和语言的表示换行符的方式。</span><span class="sxs-lookup"><span data-stu-id="f5077-148"><xref:System.Environment.NewLine> is a platform-independent and language-independent way to represent a line break.</span></span> <span data-ttu-id="f5077-149">替代方法是在 C# 中使用 `\n` 和在 Visual Basic 中使用 `vbCrLf`。</span><span class="sxs-lookup"><span data-stu-id="f5077-149">Alternatives are `\n` in C# and `vbCrLf` in Visual Basic.</span></span>

   <span data-ttu-id="f5077-150">字符串前面的美元符号 (`$`) 使你可以将表达式（如变量名称）放入字符串中的大括号内。</span><span class="sxs-lookup"><span data-stu-id="f5077-150">The dollar sign (`$`) in front of a string lets you put expressions such as variable names in curly braces in the string.</span></span> <span data-ttu-id="f5077-151">表达式值将代替表达式插入到字符串中。</span><span class="sxs-lookup"><span data-stu-id="f5077-151">The expression value is inserted into the string in place of the expression.</span></span> <span data-ttu-id="f5077-152">此语法称为[内插字符串](../../csharp/language-reference/tokens/interpolated.md)。</span><span class="sxs-lookup"><span data-stu-id="f5077-152">This syntax is referred to as [interpolated strings](../../csharp/language-reference/tokens/interpolated.md).</span></span>

1. <span data-ttu-id="f5077-153">按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 运行程序而不进行调试。</span><span class="sxs-lookup"><span data-stu-id="f5077-153">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the program without debugging.</span></span>

1. <span data-ttu-id="f5077-154">出现提示时，输入名称并按 Enter<kbd></kbd> 键。</span><span class="sxs-lookup"><span data-stu-id="f5077-154">Respond to the prompt by entering a name and pressing the <kbd>Enter</kbd> key.</span></span>

   :::image type="content" source="./media/with-visual-studio/hello-world-update.png" alt-text="控制台窗口，含已修改程序的输出":::

1. <span data-ttu-id="f5077-156">按任意键关闭控制台窗口。</span><span class="sxs-lookup"><span data-stu-id="f5077-156">Press any key to close the console window.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f5077-157">其他资源</span><span class="sxs-lookup"><span data-stu-id="f5077-157">Additional resources</span></span>

- [<span data-ttu-id="f5077-158">当前版本和长期支持版本</span><span class="sxs-lookup"><span data-stu-id="f5077-158">Current releases and long-term support releases</span></span>](../releases-and-support.md#net-core-and-net-5-version-lifecycles)

## <a name="next-steps"></a><span data-ttu-id="f5077-159">后续步骤</span><span class="sxs-lookup"><span data-stu-id="f5077-159">Next steps</span></span>

<span data-ttu-id="f5077-160">在本教程中，你创建了一个 .NET 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="f5077-160">In this tutorial, you created a .NET console application.</span></span> <span data-ttu-id="f5077-161">在下一教程中，你将调试该应用。</span><span class="sxs-lookup"><span data-stu-id="f5077-161">In the next tutorial, you debug the app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f5077-162">使用 Visual Studio 调试 .NET 控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="f5077-162">Debug a .NET console application using Visual Studio</span></span>](debugging-with-visual-studio.md)
