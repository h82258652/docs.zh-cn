---
title: 教程：编写第一个分析器和代码修补程序
description: 本教程提供了有关使用 .NET 编译器 SDK (Roslyn API) 生成分析器和代码修补程序的分步说明。
ms.date: 03/02/2021
ms.custom: mvc
ms.openlocfilehash: b712cb4df5ab6dae825407212685cb1a08b2d189
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637242"
---
# <a name="tutorial-write-your-first-analyzer-and-code-fix"></a><span data-ttu-id="c44e6-103">教程：编写第一个分析器和代码修补程序</span><span class="sxs-lookup"><span data-stu-id="c44e6-103">Tutorial: Write your first analyzer and code fix</span></span>

<span data-ttu-id="c44e6-104">.NET Compiler Platform SDK 提供面向 C# 或 Visual Basic 代码创建自定义诊断（分析器）、代码修补程序、代码重构和诊断抑制器所需的工具。</span><span class="sxs-lookup"><span data-stu-id="c44e6-104">The .NET Compiler Platform SDK provides the tools you need to create custom diagnostics (analyzers), code fixes, code refactoring, and diagnostic suppressors that target C# or Visual Basic code.</span></span> <span data-ttu-id="c44e6-105">分析器包含可识别规则冲突的代码。</span><span class="sxs-lookup"><span data-stu-id="c44e6-105">An **analyzer** contains code that recognizes violations of your rule.</span></span> <span data-ttu-id="c44e6-106">代码修补程序包含修复冲突的代码。</span><span class="sxs-lookup"><span data-stu-id="c44e6-106">Your **code fix** contains the code that fixes the violation.</span></span> <span data-ttu-id="c44e6-107">实现的规则可以是从代码结构到编码样式再到命名约定之类的任何内容。</span><span class="sxs-lookup"><span data-stu-id="c44e6-107">The rules you implement can be anything from code structure to coding style to naming conventions and more.</span></span> <span data-ttu-id="c44e6-108">.NET Compiler Platform 在开发人员编写代码时提供运行分析的框架，以及用于修复代码的所有 Visual Studio UI 功能：显示编辑器中的波形曲线、填充 Visual Studio 错误列表、创建“灯泡”建议，并显示建议修补程序的丰富预览。</span><span class="sxs-lookup"><span data-stu-id="c44e6-108">The .NET Compiler Platform provides the framework for running analysis as developers are writing code, and all the Visual Studio UI features for fixing code: showing squiggles in the editor, populating the Visual Studio Error List, creating the "light bulb" suggestions and showing the rich preview of the suggested fixes.</span></span>

<span data-ttu-id="c44e6-109">在本教程中，将探讨使用 Roslyn API 创建分析器以及随附的代码修补程序。</span><span class="sxs-lookup"><span data-stu-id="c44e6-109">In this tutorial, you'll explore the creation of an **analyzer** and an accompanying **code fix** using the Roslyn APIs.</span></span> <span data-ttu-id="c44e6-110">分析器是一种执行源代码分析并向用户报告问题的方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-110">An analyzer is a way to perform source code analysis and report a problem to the user.</span></span> <span data-ttu-id="c44e6-111">可以选择将代码修补程序与分析器相关联，来表示对用户源代码的修改。</span><span class="sxs-lookup"><span data-stu-id="c44e6-111">Optionally, a code fix can be associated with the analyzer to represent a modification to the user's source code.</span></span> <span data-ttu-id="c44e6-112">本教程将创建一个分析器，用于查找可以使用 `const` 修饰符声明的但未执行此操作的局部变量声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-112">This tutorial creates an analyzer that finds local variable declarations that could be declared using the `const` modifier but are not.</span></span> <span data-ttu-id="c44e6-113">随附的代码修补程序修改这些声明来添加 `const` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="c44e6-113">The accompanying code fix modifies those declarations to add the `const` modifier.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c44e6-114">先决条件</span><span class="sxs-lookup"><span data-stu-id="c44e6-114">Prerequisites</span></span>

- <span data-ttu-id="c44e6-115">[Visual Studio 2019](https://www.visualstudio.com/downloads) 版本 16.8 或更高版本</span><span class="sxs-lookup"><span data-stu-id="c44e6-115">[Visual Studio 2019](https://www.visualstudio.com/downloads) version 16.8 or later</span></span>

<span data-ttu-id="c44e6-116">必须通过 Visual Studio 安装程序安装 .NET 编译器平台 SDK：</span><span class="sxs-lookup"><span data-stu-id="c44e6-116">You'll need to install the **.NET Compiler Platform SDK** via the Visual Studio Installer:</span></span>

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

<span data-ttu-id="c44e6-117">可以通过几个步骤创建和验证分析器：</span><span class="sxs-lookup"><span data-stu-id="c44e6-117">There are several steps to creating and validating your analyzer:</span></span>

1. <span data-ttu-id="c44e6-118">创建解决方案。</span><span class="sxs-lookup"><span data-stu-id="c44e6-118">Create the solution.</span></span>
1. <span data-ttu-id="c44e6-119">注册分析器名称和描述。</span><span class="sxs-lookup"><span data-stu-id="c44e6-119">Register the analyzer name and description.</span></span>
1. <span data-ttu-id="c44e6-120">报告分析器警告和建议。</span><span class="sxs-lookup"><span data-stu-id="c44e6-120">Report analyzer warnings and recommendations.</span></span>
1. <span data-ttu-id="c44e6-121">实现代码修复以接受建议。</span><span class="sxs-lookup"><span data-stu-id="c44e6-121">Implement the code fix to accept recommendations.</span></span>
1. <span data-ttu-id="c44e6-122">通过单元测试改进分析。</span><span class="sxs-lookup"><span data-stu-id="c44e6-122">Improve the analysis through unit tests.</span></span>

## <a name="create-the-solution"></a><span data-ttu-id="c44e6-123">创建解决方案</span><span class="sxs-lookup"><span data-stu-id="c44e6-123">Create the solution</span></span>

- <span data-ttu-id="c44e6-124">在 Visual Studio 中，选择“文件”>“新建”>“项目...”，显示“新建项目”对话框。</span><span class="sxs-lookup"><span data-stu-id="c44e6-124">In Visual Studio, choose **File > New > Project...** to display the New Project dialog.</span></span>
- <span data-ttu-id="c44e6-125">在“Visual C#”>“扩展性”下，选择“随附代码修补程序的分析器 (.NET Standard)”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-125">Under **Visual C# > Extensibility**, choose **Analyzer with code fix (.NET Standard)**.</span></span>
- <span data-ttu-id="c44e6-126">给项目“MakeConst”命名，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-126">Name your project "**MakeConst**" and click OK.</span></span>

## <a name="explore-the-analyzer-template"></a><span data-ttu-id="c44e6-127">探索分析器模板</span><span class="sxs-lookup"><span data-stu-id="c44e6-127">Explore the analyzer template</span></span>

<span data-ttu-id="c44e6-128">随附代码修补程序的分析器模板会创建五个项目：</span><span class="sxs-lookup"><span data-stu-id="c44e6-128">The analyzer with code fix template creates five projects:</span></span>

- <span data-ttu-id="c44e6-129">MakeConst，其中包含分析器。</span><span class="sxs-lookup"><span data-stu-id="c44e6-129">**MakeConst**, which contains the analyzer.</span></span>
- <span data-ttu-id="c44e6-130">MakeConst.CodeFixes，其中包含代码修补程序。</span><span class="sxs-lookup"><span data-stu-id="c44e6-130">**MakeConst.CodeFixes**, which contains the code fix.</span></span>
- <span data-ttu-id="c44e6-131">MakeConst.Package，用于生成分析器和代码修补程序的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="c44e6-131">**MakeConst.Package**, which is used to produce NuGet package for the analyzer and code fix.</span></span>
- <span data-ttu-id="c44e6-132">MakeConst.Test，这是一个单元测试项目。</span><span class="sxs-lookup"><span data-stu-id="c44e6-132">**MakeConst.Test**, which is a unit test project.</span></span>
- <span data-ttu-id="c44e6-133">MakeConst.Vsix，这是默认的启动项目，它将启动加载了新分析器的第二个 Visual Studio 实例。</span><span class="sxs-lookup"><span data-stu-id="c44e6-133">**MakeConst.Vsix**, which is the default startup project that starts a second instance of Visual Studio that has loaded your new analyzer.</span></span> <span data-ttu-id="c44e6-134">按 F5<kbd></kbd> 启动 VSIX 项目。</span><span class="sxs-lookup"><span data-stu-id="c44e6-134">Press <kbd>F5</kbd> to start the VSIX project.</span></span>

> [!TIP]
> <span data-ttu-id="c44e6-135">在运行分析器时，请启动 Visual Studio 的第二个副本。</span><span class="sxs-lookup"><span data-stu-id="c44e6-135">When you run your analyzer, you start a second copy of Visual Studio.</span></span> <span data-ttu-id="c44e6-136">此第二个副本使用不同的注册表配置单元来存储设置。</span><span class="sxs-lookup"><span data-stu-id="c44e6-136">This second copy uses a different registry hive to store settings.</span></span> <span data-ttu-id="c44e6-137">这样便可以将 Visual Studio 两个副本中的可视化设置区分开来。</span><span class="sxs-lookup"><span data-stu-id="c44e6-137">That enables you to differentiate the visual settings in the two copies of Visual Studio.</span></span> <span data-ttu-id="c44e6-138">可以选择 Visual Studio 实验性运行的不同主题。</span><span class="sxs-lookup"><span data-stu-id="c44e6-138">You can pick a different theme for the experimental run of Visual Studio.</span></span> <span data-ttu-id="c44e6-139">此外，不要在设置中漫游，也不要使用 Visual Studio 的实验性运行登录到 Visual Studio 帐户。</span><span class="sxs-lookup"><span data-stu-id="c44e6-139">In addition, don't roam your settings or login to your Visual Studio account using the experimental run of Visual Studio.</span></span> <span data-ttu-id="c44e6-140">这样可以使设置保持不同。</span><span class="sxs-lookup"><span data-stu-id="c44e6-140">That keeps the settings different.</span></span>

<span data-ttu-id="c44e6-141">在刚刚启动的第二个 Visual Studio 实例中，创建一个新的 C# 控制台应用程序项目（任何目标框架都可用 -- 分析器在源级别工作。）悬停在带波浪下划线的标记上，将显示分析器提供的警告文本。</span><span class="sxs-lookup"><span data-stu-id="c44e6-141">In the second Visual Studio instance that you just started, create a new C# Console Application project (any target framework will work -- analyzers work at the source level.) Hover over the token with a wavy underline, and the warning text provided by an analyzer appears.</span></span>

<span data-ttu-id="c44e6-142">该模板创建一个分析器，它报告有关类型名称包含小写字母的每种类型声明的警告，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="c44e6-142">The template creates an analyzer that reports a warning on each type declaration where the type name contains lowercase letters, as shown in the following figure:</span></span>

![分析器报告警告](media/how-to-write-csharp-analyzer-code-fix/report-warning.png)

<span data-ttu-id="c44e6-144">该模板还提供了一种代码修补程序，它可以将包含小写字符的任何类型名称更改为大写字母。</span><span class="sxs-lookup"><span data-stu-id="c44e6-144">The template also provides a code fix that changes any type name containing lower case characters to all upper case.</span></span> <span data-ttu-id="c44e6-145">可以单击显示警告的灯泡，以查看建议的更改。</span><span class="sxs-lookup"><span data-stu-id="c44e6-145">You can click on the light bulb displayed with the warning to see the suggested changes.</span></span> <span data-ttu-id="c44e6-146">接受建议的更改会更新解决方案中的类型名称和所有对该类型的引用。</span><span class="sxs-lookup"><span data-stu-id="c44e6-146">Accepting the suggested changes updates the type name and all references to that type in the solution.</span></span> <span data-ttu-id="c44e6-147">现在你已了解初始分析器的操作，关闭第二个 Visual Studio 实例，并返回到分析器项目。</span><span class="sxs-lookup"><span data-stu-id="c44e6-147">Now that you've seen the initial analyzer in action, close the second Visual Studio instance and return to your analyzer project.</span></span>

<span data-ttu-id="c44e6-148">无需启动 Visual Studio 的第二个副本和创建新代码来测试分析器中的每一项更改。</span><span class="sxs-lookup"><span data-stu-id="c44e6-148">You don't have to start a second copy of Visual Studio and create new code to test every change in your analyzer.</span></span> <span data-ttu-id="c44e6-149">该模板还为你创建了单元测试项目。</span><span class="sxs-lookup"><span data-stu-id="c44e6-149">The template also creates a unit test project for you.</span></span> <span data-ttu-id="c44e6-150">该项目包含两个测试。</span><span class="sxs-lookup"><span data-stu-id="c44e6-150">That project contains two tests.</span></span> <span data-ttu-id="c44e6-151">`TestMethod1` 显示了在不触发诊断的情况下分析代码的典型测试格式。</span><span class="sxs-lookup"><span data-stu-id="c44e6-151">`TestMethod1` shows the typical format of a test that analyzes code without triggering a diagnostic.</span></span> <span data-ttu-id="c44e6-152">`TestMethod2` 显示了先触发诊断然后应用建议的代码修补程序的测试格式。</span><span class="sxs-lookup"><span data-stu-id="c44e6-152">`TestMethod2` shows the format of a test that triggers a diagnostic, and then applies a suggested code fix.</span></span> <span data-ttu-id="c44e6-153">在构建分析器和代码修补程序时，为不同的代码结构编写测试，以验证你的工作。</span><span class="sxs-lookup"><span data-stu-id="c44e6-153">As you build your analyzer and code fix, you'll write tests for different code structures to verify your work.</span></span> <span data-ttu-id="c44e6-154">分析器的单元测试比使用 Visual Studio 以交互方式进行测试的速度更快。</span><span class="sxs-lookup"><span data-stu-id="c44e6-154">Unit tests for analyzers are much quicker than testing them interactively with Visual Studio.</span></span>

> [!TIP]
> <span data-ttu-id="c44e6-155">当你知道哪些代码构造应触发和不应触发分析器时，分析器单元测试是一个很好的工具。</span><span class="sxs-lookup"><span data-stu-id="c44e6-155">Analyzer unit tests are a great tool when you know what code constructs should and shouldn't trigger your analyzer.</span></span> <span data-ttu-id="c44e6-156">在 Visual Studio 的另一个副本加载分析器是用于浏览并找到你可能未曾想到的构造的绝佳工具。</span><span class="sxs-lookup"><span data-stu-id="c44e6-156">Loading your analyzer in another copy of Visual Studio is a great tool to explore and find constructs you may not have thought about yet.</span></span>

<span data-ttu-id="c44e6-157">在本教程中，你将编写一个分析器，用于向用户报告可以转换为局部常量的任何局部变量声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-157">In this tutorial, you write an analyzer that reports to the user any local variable declarations that can be converted to local constants.</span></span> <span data-ttu-id="c44e6-158">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="c44e6-158">For example, consider the following code:</span></span>

```csharp
int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="c44e6-159">在上面的代码中，会向 `x` 分配常量值，并且永远不会被修改。</span><span class="sxs-lookup"><span data-stu-id="c44e6-159">In the code above, `x` is assigned a constant value and is never modified.</span></span> <span data-ttu-id="c44e6-160">可以使用 `const` 修饰符声明：</span><span class="sxs-lookup"><span data-stu-id="c44e6-160">It can be declared using the `const` modifier:</span></span>

```csharp
const int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="c44e6-161">涉及到确定变量是否可以保持不变的分析，需要进行句法分析、初始值设定项的常量分析和数据流分析，以确保永远不会写入该变量。</span><span class="sxs-lookup"><span data-stu-id="c44e6-161">The analysis to determine whether a variable can be made constant is involved, requiring syntactic analysis, constant analysis of the initializer expression and dataflow analysis to ensure that the variable is never written to.</span></span> <span data-ttu-id="c44e6-162">.NET Compiler Platform 提供了 API，以便更轻松地执行此分析。</span><span class="sxs-lookup"><span data-stu-id="c44e6-162">The .NET Compiler Platform provides APIs that make it easier to perform this analysis.</span></span>

## <a name="create-analyzer-registrations"></a><span data-ttu-id="c44e6-163">创建分析器注册</span><span class="sxs-lookup"><span data-stu-id="c44e6-163">Create analyzer registrations</span></span>

<span data-ttu-id="c44e6-164">该模板将在 MakeConstAnalyzer.cs 文件中创建初始 `DiagnosticAnalyzer` 类。</span><span class="sxs-lookup"><span data-stu-id="c44e6-164">The template creates the initial `DiagnosticAnalyzer` class, in the *MakeConstAnalyzer.cs* file.</span></span> <span data-ttu-id="c44e6-165">此初始分析器显示每个分析器的两个重要属性。</span><span class="sxs-lookup"><span data-stu-id="c44e6-165">This initial analyzer shows two important properties of every analyzer.</span></span>

- <span data-ttu-id="c44e6-166">每个诊断分析器必须提供 `[DiagnosticAnalyzer]` 属性，用于描述其操作所用的语言。</span><span class="sxs-lookup"><span data-stu-id="c44e6-166">Every diagnostic analyzer must provide a `[DiagnosticAnalyzer]` attribute that describes the language it operates on.</span></span>
- <span data-ttu-id="c44e6-167">每个诊断分析器都必须是（直接或间接地）从 <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer> 类派生的。</span><span class="sxs-lookup"><span data-stu-id="c44e6-167">Every diagnostic analyzer must derive (directly or indirectly) from the <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer> class.</span></span>

<span data-ttu-id="c44e6-168">该模板还显示属于任何分析器的基本功能：</span><span class="sxs-lookup"><span data-stu-id="c44e6-168">The template also shows the basic features that are part of any analyzer:</span></span>

1. <span data-ttu-id="c44e6-169">注册操作。</span><span class="sxs-lookup"><span data-stu-id="c44e6-169">Register actions.</span></span> <span data-ttu-id="c44e6-170">此操作表示应触发分析器以检查存在冲突的代码的代码更改。</span><span class="sxs-lookup"><span data-stu-id="c44e6-170">The actions represent code changes that should trigger your analyzer to examine code for violations.</span></span> <span data-ttu-id="c44e6-171">当 Visual Studio 检测到匹配注册操作的代码编辑时，它调用分析器的注册方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-171">When Visual Studio detects code edits that match a registered action, it calls your analyzer's registered method.</span></span>
1. <span data-ttu-id="c44e6-172">创建诊断。</span><span class="sxs-lookup"><span data-stu-id="c44e6-172">Create diagnostics.</span></span> <span data-ttu-id="c44e6-173">当分析器检测到冲突，它会创建一个诊断对象，Visual Studio 使用该对象来通知用户有关冲突的信息。</span><span class="sxs-lookup"><span data-stu-id="c44e6-173">When your analyzer detects a violation, it creates a diagnostic object that Visual Studio uses to notify the user of the violation.</span></span>

<span data-ttu-id="c44e6-174">在重写的 <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer.Initialize(Microsoft.CodeAnalysis.Diagnostics.AnalysisContext)?displayProperty=nameWithType> 方法中注册操作。</span><span class="sxs-lookup"><span data-stu-id="c44e6-174">You register actions in your override of <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer.Initialize(Microsoft.CodeAnalysis.Diagnostics.AnalysisContext)?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="c44e6-175">在本教程中，你将访问“语法节点”寻找局部声明，并查看哪些具有常量值。</span><span class="sxs-lookup"><span data-stu-id="c44e6-175">In this tutorial, you'll visit **syntax nodes** looking for local declarations, and see which of those have constant values.</span></span> <span data-ttu-id="c44e6-176">如果声明可以是常量，分析器将创建并报告诊断。</span><span class="sxs-lookup"><span data-stu-id="c44e6-176">If a declaration could be constant, your analyzer will create and report a diagnostic.</span></span>

<span data-ttu-id="c44e6-177">第一步是更新注册常量和 `Initialize` 方法，以便这些常量指示“Make Const”分析器。</span><span class="sxs-lookup"><span data-stu-id="c44e6-177">The first step is to update the registration constants and `Initialize` method so these constants indicate your "Make Const" analyzer.</span></span> <span data-ttu-id="c44e6-178">大多数字符串常量在字符串资源文件中定义。</span><span class="sxs-lookup"><span data-stu-id="c44e6-178">Most of the string constants are defined in the string resource file.</span></span> <span data-ttu-id="c44e6-179">应遵循此做法，以便更轻松地实现本地化。</span><span class="sxs-lookup"><span data-stu-id="c44e6-179">You should follow that practice for easier localization.</span></span> <span data-ttu-id="c44e6-180">打开“MakeConst”分析器项目的“Resources.resx”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-180">Open the *Resources.resx* file for the **MakeConst** analyzer project.</span></span> <span data-ttu-id="c44e6-181">将显示资源编辑器。</span><span class="sxs-lookup"><span data-stu-id="c44e6-181">This displays the resource editor.</span></span> <span data-ttu-id="c44e6-182">更新字符串资源，如下所示：</span><span class="sxs-lookup"><span data-stu-id="c44e6-182">Update the string resources as follows:</span></span>

- <span data-ttu-id="c44e6-183">将 `AnalyzerDescription` 更改为“:::no-loc text="Variables that are not modified should be made constants.":::”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-183">Change `AnalyzerDescription` to ":::no-loc text="Variables that are not modified should be made constants.":::".</span></span>
- <span data-ttu-id="c44e6-184">将 `AnalyzerMessageFormat` 更改为“:::no-loc text="Variable '{0}' can be made constant":::”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-184">Change `AnalyzerMessageFormat` to ":::no-loc text="Variable '{0}' can be made constant":::".</span></span>
- <span data-ttu-id="c44e6-185">将 `AnalyzerTitle` 更改为“:::no-loc text="Variable can be made constant":::”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-185">Change `AnalyzerTitle` to ":::no-loc text="Variable can be made constant":::.</span></span>

<span data-ttu-id="c44e6-186">完成后，资源编辑器应如下图所示：</span><span class="sxs-lookup"><span data-stu-id="c44e6-186">When you have finished, the resource editor should appear as follow figure shows:</span></span>

![更新字符串资源](media/how-to-write-csharp-analyzer-code-fix/update-string-resources.png)

<span data-ttu-id="c44e6-188">剩余的更改在分析器文件中。</span><span class="sxs-lookup"><span data-stu-id="c44e6-188">The remaining changes are in the analyzer file.</span></span> <span data-ttu-id="c44e6-189">在 Visual Studio 中打开“MakeConstAnalyzer.cs”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-189">Open *MakeConstAnalyzer.cs* in Visual Studio.</span></span> <span data-ttu-id="c44e6-190">将注册操作从作用于符号的操作更改为作用于语法的操作。</span><span class="sxs-lookup"><span data-stu-id="c44e6-190">Change the registered action from one that acts on symbols to one that acts on syntax.</span></span> <span data-ttu-id="c44e6-191">在 `MakeConstAnalyzerAnalyzer.Initialize` 方法中，找到在符号上注册操作的行：</span><span class="sxs-lookup"><span data-stu-id="c44e6-191">In the `MakeConstAnalyzerAnalyzer.Initialize` method, find the line that registers the action on symbols:</span></span>

```csharp
context.RegisterSymbolAction(AnalyzeSymbol, SymbolKind.NamedType);
```

<span data-ttu-id="c44e6-192">使用下面的行替换它：</span><span class="sxs-lookup"><span data-stu-id="c44e6-192">Replace it with the following line:</span></span>

[!code-csharp[Register the node action](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#RegisterNodeAction "Register a node action")]

<span data-ttu-id="c44e6-193">完成此更改后，可以删除 `AnalyzeSymbol` 方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-193">After that change, you can delete the `AnalyzeSymbol` method.</span></span> <span data-ttu-id="c44e6-194">此分析器检查 <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.LocalDeclarationStatement?displayProperty=nameWithType>，而不是 <xref:Microsoft.CodeAnalysis.SymbolKind.NamedType?displayProperty=nameWithType> 语句。</span><span class="sxs-lookup"><span data-stu-id="c44e6-194">This analyzer examines <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.LocalDeclarationStatement?displayProperty=nameWithType>, not <xref:Microsoft.CodeAnalysis.SymbolKind.NamedType?displayProperty=nameWithType> statements.</span></span> <span data-ttu-id="c44e6-195">请注意，`AnalyzeNode` 下面有红色波浪线。</span><span class="sxs-lookup"><span data-stu-id="c44e6-195">Notice that `AnalyzeNode` has red squiggles under it.</span></span> <span data-ttu-id="c44e6-196">刚添加的代码引用未声明的 `AnalyzeNode` 方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-196">The code you just added references an `AnalyzeNode` method that hasn't been declared.</span></span> <span data-ttu-id="c44e6-197">使用以下代码声明该方法：</span><span class="sxs-lookup"><span data-stu-id="c44e6-197">Declare that method using the following code:</span></span>

```csharp
private void AnalyzeNode(SyntaxNodeAnalysisContext context)
{
}
```

<span data-ttu-id="c44e6-198">将 `Category` 更改为 MakeConstAnalyzer.cs 中的“:::no-loc text="Usage":::”，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="c44e6-198">Change the `Category` to ":::no-loc text="Usage":::" in *MakeConstAnalyzer.cs* as shown in the following code:</span></span>

[!code-csharp[Category constant](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#Category  "Change category to Usage")]

## <a name="find-local-declarations-that-could-be-const"></a><span data-ttu-id="c44e6-199">查找可以是常量的局部声明</span><span class="sxs-lookup"><span data-stu-id="c44e6-199">Find local declarations that could be const</span></span>

<span data-ttu-id="c44e6-200">可以开始编写 `AnalyzeNode` 方法的第一个版本了。</span><span class="sxs-lookup"><span data-stu-id="c44e6-200">It's time to write the first version of the `AnalyzeNode` method.</span></span> <span data-ttu-id="c44e6-201">应查找可以是 `const` 但实际不是的单个局部声明，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="c44e6-201">It should look for a single local declaration that could be `const` but is not, like the following code:</span></span>

```csharp
int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="c44e6-202">第一步是查找局部声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-202">The first step is to find local declarations.</span></span> <span data-ttu-id="c44e6-203">将以下代码添加到 MakeConstAnalyzer.cs 中的 `AnalyzeNode`：</span><span class="sxs-lookup"><span data-stu-id="c44e6-203">Add the following code to `AnalyzeNode` in *MakeConstAnalyzer.cs*:</span></span>

[!code-csharp[localDeclaration variable](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#LocalDeclaration  "Add localDeclaration variable")]

<span data-ttu-id="c44e6-204">此强制转换始终会成功，因为分析器注册了对局部声明的更改，并且只注册了局部声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-204">This cast always succeeds because your analyzer registered for changes to local declarations, and only local declarations.</span></span> <span data-ttu-id="c44e6-205">没有其他节点类型会触发对 `AnalyzeNode` 方法的调用。</span><span class="sxs-lookup"><span data-stu-id="c44e6-205">No other node type triggers a call to your `AnalyzeNode` method.</span></span> <span data-ttu-id="c44e6-206">接下来，检查任何 `const` 修饰符的声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-206">Next, check the declaration for any `const` modifiers.</span></span> <span data-ttu-id="c44e6-207">一旦找到，请立即返回。</span><span class="sxs-lookup"><span data-stu-id="c44e6-207">If you find them, return immediately.</span></span> <span data-ttu-id="c44e6-208">以下代码用于查找局部声明上的任何 `const` 修饰符：</span><span class="sxs-lookup"><span data-stu-id="c44e6-208">The following code looks for any `const` modifiers on the local declaration:</span></span>

[!code-csharp[bail-out on const keyword](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#BailOutOnConst  "bail-out on const keyword")]

<span data-ttu-id="c44e6-209">最后，需要检查变量是否可能是 `const`。</span><span class="sxs-lookup"><span data-stu-id="c44e6-209">Finally, you need to check that the variable could be `const`.</span></span> <span data-ttu-id="c44e6-210">这意味着确保在其初始化后永远不会对其赋值。</span><span class="sxs-lookup"><span data-stu-id="c44e6-210">That means making sure it is never assigned after it is initialized.</span></span>

<span data-ttu-id="c44e6-211">将使用 <xref:Microsoft.CodeAnalysis.Diagnostics.SyntaxNodeAnalysisContext> 执行一些语义分析。</span><span class="sxs-lookup"><span data-stu-id="c44e6-211">You'll perform some semantic analysis using the <xref:Microsoft.CodeAnalysis.Diagnostics.SyntaxNodeAnalysisContext>.</span></span> <span data-ttu-id="c44e6-212">使用 `context` 参数确定局部变量声明是否可为 `const`。</span><span class="sxs-lookup"><span data-stu-id="c44e6-212">You use the `context` argument to determine whether the local variable declaration can be made `const`.</span></span> <span data-ttu-id="c44e6-213"><xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> 表示单个源文件中的所有语义信息。</span><span class="sxs-lookup"><span data-stu-id="c44e6-213">A <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> represents all of semantic information in a single source file.</span></span> <span data-ttu-id="c44e6-214">可参阅涵盖了[语义模型](../work-with-semantics.md)的文章了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="c44e6-214">You can learn more in the article that covers [semantic models](../work-with-semantics.md).</span></span> <span data-ttu-id="c44e6-215">将使用 <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> 在局部声明语句上执行数据流分析。</span><span class="sxs-lookup"><span data-stu-id="c44e6-215">You'll use the <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> to perform data flow analysis on the local declaration statement.</span></span> <span data-ttu-id="c44e6-216">然后，使用此数据流分析的结果确保局部变量不会在任何其他位置用新值来编写。</span><span class="sxs-lookup"><span data-stu-id="c44e6-216">Then, you use the results of this data flow analysis to ensure that the local variable isn't written with a new value anywhere else.</span></span> <span data-ttu-id="c44e6-217">调用 <xref:Microsoft.CodeAnalysis.ModelExtensions.GetDeclaredSymbol%2A> 扩展方法来检索变量的 <xref:Microsoft.CodeAnalysis.ILocalSymbol>，并检查确认它不包含在数据流分析的 <xref:Microsoft.CodeAnalysis.DataFlowAnalysis.WrittenOutside%2A?displayProperty=nameWithType> 集中。</span><span class="sxs-lookup"><span data-stu-id="c44e6-217">Call the <xref:Microsoft.CodeAnalysis.ModelExtensions.GetDeclaredSymbol%2A> extension method to retrieve the <xref:Microsoft.CodeAnalysis.ILocalSymbol> for the variable and check that it isn't contained with the <xref:Microsoft.CodeAnalysis.DataFlowAnalysis.WrittenOutside%2A?displayProperty=nameWithType> collection of the data flow analysis.</span></span> <span data-ttu-id="c44e6-218">在 `AnalyzeNode` 方法的末尾添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="c44e6-218">Add the following code to the end of the `AnalyzeNode` method:</span></span>

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

<span data-ttu-id="c44e6-219">刚添加的代码可确保变量不会修改，并因此可以进行 `const` 操作。</span><span class="sxs-lookup"><span data-stu-id="c44e6-219">The code just added ensures that the variable isn't modified, and can therefore be made `const`.</span></span> <span data-ttu-id="c44e6-220">现在可以引发诊断了。</span><span class="sxs-lookup"><span data-stu-id="c44e6-220">It's time to raise the diagnostic.</span></span> <span data-ttu-id="c44e6-221">将以下代码添加为 `AnalyzeNode` 的最后一行：</span><span class="sxs-lookup"><span data-stu-id="c44e6-221">Add the following code as the last line in `AnalyzeNode`:</span></span>

[!code-csharp[Call ReportDiagnostic](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#ReportDiagnostic  "Call ReportDiagnostic")]

<span data-ttu-id="c44e6-222">可以通过按 F5<kbd></kbd> 运行分析器来检查进度。</span><span class="sxs-lookup"><span data-stu-id="c44e6-222">You can check your progress by pressing <kbd>F5</kbd> to run your analyzer.</span></span> <span data-ttu-id="c44e6-223">可以加载前面创建的控制台应用程序，然后添加以下测试代码：</span><span class="sxs-lookup"><span data-stu-id="c44e6-223">You can load the console application you created earlier and then add the following test code:</span></span>

```csharp
int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="c44e6-224">应显示灯泡，且分析器应报告诊断。</span><span class="sxs-lookup"><span data-stu-id="c44e6-224">The light bulb should appear, and your analyzer should report a diagnostic.</span></span> <span data-ttu-id="c44e6-225">但是，灯泡仍使用模板生成的代码修补程序，并告知你可以用大写。</span><span class="sxs-lookup"><span data-stu-id="c44e6-225">However, the light bulb still uses the template generated code fix, and tells you it can be made upper case.</span></span> <span data-ttu-id="c44e6-226">下一部分将说明如何编写代码修补程序。</span><span class="sxs-lookup"><span data-stu-id="c44e6-226">The next section explains how to write the code fix.</span></span>

## <a name="write-the-code-fix"></a><span data-ttu-id="c44e6-227">编写代码修补程序</span><span class="sxs-lookup"><span data-stu-id="c44e6-227">Write the code fix</span></span>

<span data-ttu-id="c44e6-228">分析器可以提供一个或多个代码修补程序。</span><span class="sxs-lookup"><span data-stu-id="c44e6-228">An analyzer can provide one or more code fixes.</span></span> <span data-ttu-id="c44e6-229">代码修补程序定义解决报告问题的编辑。</span><span class="sxs-lookup"><span data-stu-id="c44e6-229">A code fix defines an edit that addresses the reported issue.</span></span> <span data-ttu-id="c44e6-230">对于你创建的分析器，可以提供将插入 const 关键字的代码修补程序：</span><span class="sxs-lookup"><span data-stu-id="c44e6-230">For the analyzer that you created, you can provide a code fix that inserts the const keyword:</span></span>

```diff
- int x = 0;
+ const int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="c44e6-231">用户从编辑器的灯泡 UI 中选择它，Visual Studio 更改代码。</span><span class="sxs-lookup"><span data-stu-id="c44e6-231">The user chooses it from the light bulb UI in the editor and Visual Studio changes the code.</span></span>

<span data-ttu-id="c44e6-232">打开 CodeFixResources.resx 文件，并将 `CodeFixTitle` 更改为“:::no-loc text="Make constant":::”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-232">Open *CodeFixResources.resx* file and change `CodeFixTitle` to ":::no-loc text="Make constant":::".</span></span>

<span data-ttu-id="c44e6-233">打开由模板添加的“MakeConstCodeFixProvider.cs”文件。</span><span class="sxs-lookup"><span data-stu-id="c44e6-233">Open the *MakeConstCodeFixProvider.cs* file added by the template.</span></span> <span data-ttu-id="c44e6-234">此代码修补程序已绑定到由诊断分析器生成的诊断 ID，但它尚没有实施正确的代码转换。</span><span class="sxs-lookup"><span data-stu-id="c44e6-234">This code fix is already wired up to the Diagnostic ID produced by your diagnostic analyzer, but it doesn't yet implement the right code transform.</span></span>

<span data-ttu-id="c44e6-235">接下来，删除 `MakeUppercaseAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-235">Next, delete the `MakeUppercaseAsync` method.</span></span> <span data-ttu-id="c44e6-236">它不再适用。</span><span class="sxs-lookup"><span data-stu-id="c44e6-236">It no longer applies.</span></span>

<span data-ttu-id="c44e6-237">所有代码修复提供程序都派生自 <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider>。</span><span class="sxs-lookup"><span data-stu-id="c44e6-237">All code fix providers derive from <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider>.</span></span> <span data-ttu-id="c44e6-238">它们都重写 <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider.RegisterCodeFixesAsync(Microsoft.CodeAnalysis.CodeFixes.CodeFixContext)?displayProperty=nameWithType> 以报告可用的代码修补程序。</span><span class="sxs-lookup"><span data-stu-id="c44e6-238">They all override <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider.RegisterCodeFixesAsync(Microsoft.CodeAnalysis.CodeFixes.CodeFixContext)?displayProperty=nameWithType> to report available code fixes.</span></span> <span data-ttu-id="c44e6-239">在 `RegisterCodeFixesAsync` 中，将正在搜索的上级节点类型更改为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> 以匹配诊断：</span><span class="sxs-lookup"><span data-stu-id="c44e6-239">In `RegisterCodeFixesAsync`, change the ancestor node type you're searching for to a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> to match the diagnostic:</span></span>

[!code-csharp[Find local declaration node](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#FindDeclarationNode  "Find the local declaration node that raised the diagnostic")]

<span data-ttu-id="c44e6-240">接下来，更改用于注册代码修补程序的最后一行。</span><span class="sxs-lookup"><span data-stu-id="c44e6-240">Next, change the last line to register a code fix.</span></span> <span data-ttu-id="c44e6-241">修补程序将创建新的文档，该文档通过将 `const` 修饰符添加到现有声明生成：</span><span class="sxs-lookup"><span data-stu-id="c44e6-241">Your fix will create a new document that results from adding the `const` modifier to an existing declaration:</span></span>

[!code-csharp[Register the new code fix](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#RegisterCodeFix  "Register the new code fix")]

<span data-ttu-id="c44e6-242">你会注意到刚在符号 `MakeConstAsync` 上添加的代码中的红色波浪线。</span><span class="sxs-lookup"><span data-stu-id="c44e6-242">You'll notice red squiggles in the code you just added on the symbol `MakeConstAsync`.</span></span> <span data-ttu-id="c44e6-243">添加的 `MakeConstAsync` 声明如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="c44e6-243">Add a declaration for `MakeConstAsync` like the following code:</span></span>

```csharp
private static async Task<Document> MakeConstAsync(Document document,
    LocalDeclarationStatementSyntax localDeclaration,
    CancellationToken cancellationToken)
{
}
```

<span data-ttu-id="c44e6-244">新的 `MakeConstAsync` 方法会将表示用户源文件的 <xref:Microsoft.CodeAnalysis.Document> 转换到现包含 `const` 声明的新 <xref:Microsoft.CodeAnalysis.Document>。</span><span class="sxs-lookup"><span data-stu-id="c44e6-244">Your new `MakeConstAsync` method will transform the <xref:Microsoft.CodeAnalysis.Document> representing the user's source file into a new <xref:Microsoft.CodeAnalysis.Document> that now contains a `const` declaration.</span></span>

<span data-ttu-id="c44e6-245">创建一个新的 `const` 关键字标记，以在声明语句的开头处插入。</span><span class="sxs-lookup"><span data-stu-id="c44e6-245">You create a new `const` keyword token to insert at the front of the declaration statement.</span></span> <span data-ttu-id="c44e6-246">请注意，首先从声明语句的第一个标记中删除任何前导琐碎内容，然后将其附加到 `const` 标记。</span><span class="sxs-lookup"><span data-stu-id="c44e6-246">Be careful to first remove any leading trivia from the first token of the declaration statement and attach it to the `const` token.</span></span> <span data-ttu-id="c44e6-247">将以下代码添加到 `MakeConstAsync` 方法中：</span><span class="sxs-lookup"><span data-stu-id="c44e6-247">Add the following code to the `MakeConstAsync` method:</span></span>

[!code-csharp[Create a new const keyword token](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#CreateConstToken  "Create the new const keyword token")]

<span data-ttu-id="c44e6-248">接下来，使用以下代码向声明添加 `const` 标记：</span><span class="sxs-lookup"><span data-stu-id="c44e6-248">Next, add the `const` token to the declaration using the following code:</span></span>

```csharp
// Insert the const token into the modifiers list, creating a new modifiers list.
SyntaxTokenList newModifiers = trimmedLocal.Modifiers.Insert(0, constToken);
// Produce the new local declaration.
LocalDeclarationStatementSyntax newLocal = trimmedLocal
    .WithModifiers(newModifiers)
    .WithDeclaration(localDeclaration.Declaration);
```

<span data-ttu-id="c44e6-249">接下来，设置要匹配 C# 格式设置规则的新声明的格式。</span><span class="sxs-lookup"><span data-stu-id="c44e6-249">Next, format the new declaration to match C# formatting rules.</span></span> <span data-ttu-id="c44e6-250">对所做的更改进行格式设置以匹配现有代码，这可创建更好的体验。</span><span class="sxs-lookup"><span data-stu-id="c44e6-250">Formatting your changes to match existing code creates a better experience.</span></span> <span data-ttu-id="c44e6-251">紧接着在现有代码后面添加以下语句：</span><span class="sxs-lookup"><span data-stu-id="c44e6-251">Add the following statement immediately after the existing code:</span></span>

[!code-csharp[Format the new declaration](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#FormatLocal  "Format the new declaration")]

<span data-ttu-id="c44e6-252">此代码需要新命名空间。</span><span class="sxs-lookup"><span data-stu-id="c44e6-252">A new namespace is required for this code.</span></span> <span data-ttu-id="c44e6-253">将下面的 `using` 指令添加到文件的顶部：</span><span class="sxs-lookup"><span data-stu-id="c44e6-253">Add the following `using` directive to the top of the file:</span></span>

```csharp
using Microsoft.CodeAnalysis.Formatting;
```

<span data-ttu-id="c44e6-254">最后一步是进行编辑。</span><span class="sxs-lookup"><span data-stu-id="c44e6-254">The final step is to make your edit.</span></span> <span data-ttu-id="c44e6-255">此过程包括三个步骤：</span><span class="sxs-lookup"><span data-stu-id="c44e6-255">There are three steps to this process:</span></span>

1. <span data-ttu-id="c44e6-256">获取现有文档的句柄。</span><span class="sxs-lookup"><span data-stu-id="c44e6-256">Get a handle to the existing document.</span></span>
1. <span data-ttu-id="c44e6-257">通过将现有声明替换为新声明来创建一个新文档。</span><span class="sxs-lookup"><span data-stu-id="c44e6-257">Create a new document by replacing the existing declaration with the new declaration.</span></span>
1. <span data-ttu-id="c44e6-258">返回新文档。</span><span class="sxs-lookup"><span data-stu-id="c44e6-258">Return the new document.</span></span>

<span data-ttu-id="c44e6-259">在 `MakeConstAsync` 方法的末尾添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="c44e6-259">Add the following code to the end of the `MakeConstAsync` method:</span></span>

[!code-csharp[replace the declaration](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#ReplaceDocument  "Generate a new document by replacing the declaration")]

<span data-ttu-id="c44e6-260">代码修补程序已准备就绪。</span><span class="sxs-lookup"><span data-stu-id="c44e6-260">Your code fix is ready to try.</span></span>  <span data-ttu-id="c44e6-261">按 <kbd> F5 </kbd> 在第二个 Visual Studio 实例中运行分析器项目。</span><span class="sxs-lookup"><span data-stu-id="c44e6-261">Press <kbd>F5</kbd> to run the analyzer project in a second instance of Visual Studio.</span></span> <span data-ttu-id="c44e6-262">在第二个 Visual Studio 实例中，创建一个新的 C# 控制台应用程序项目并向 Main 方法添加使用常量值初始化的几个局部变量声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-262">In the second Visual Studio instance, create a new C# Console Application project and add a few local variable declarations initialized with constant values to the Main method.</span></span> <span data-ttu-id="c44e6-263">你将看到它们被报告为警告，如下所示。</span><span class="sxs-lookup"><span data-stu-id="c44e6-263">You'll see that they are reported as warnings as below.</span></span>

![可以发出 const 警告](media/how-to-write-csharp-analyzer-code-fix/make-const-warning.png)

<span data-ttu-id="c44e6-265">现在已经有了很大的进展。</span><span class="sxs-lookup"><span data-stu-id="c44e6-265">You've made a lot of progress.</span></span> <span data-ttu-id="c44e6-266">可以进行 `const` 操作的声明下具有波浪线。</span><span class="sxs-lookup"><span data-stu-id="c44e6-266">There are squiggles under the declarations that can be made `const`.</span></span> <span data-ttu-id="c44e6-267">但仍有工作要做。</span><span class="sxs-lookup"><span data-stu-id="c44e6-267">But there is still work to do.</span></span> <span data-ttu-id="c44e6-268">如果将 `const` 添加到依次以 `i`、`j` 和 `k` 开头的声明，该过程会很有效。</span><span class="sxs-lookup"><span data-stu-id="c44e6-268">This works fine if you add `const` to the declarations starting with `i`, then `j` and finally `k`.</span></span> <span data-ttu-id="c44e6-269">不过，如果以从 `k` 开始的不同顺序添加 `const` 修饰符，分析器会生成错误：`k` 无法声明为 `const`，除非 `i` 和 `j` 均已进行 `const` 处理。</span><span class="sxs-lookup"><span data-stu-id="c44e6-269">But, if you add the `const` modifier in a different order, starting with `k`, your analyzer creates errors: `k` can't be declared `const`, unless `i` and `j` are both already `const`.</span></span> <span data-ttu-id="c44e6-270">必须执行详细分析，以确保处理可以声明和初始化变量的不同方式。</span><span class="sxs-lookup"><span data-stu-id="c44e6-270">You've got to do more analysis to ensure you handle the different ways variables can be declared and initialized.</span></span>

## <a name="build-unit-tests"></a><span data-ttu-id="c44e6-271">生成单元测试</span><span class="sxs-lookup"><span data-stu-id="c44e6-271">Build unit tests</span></span>

<span data-ttu-id="c44e6-272">分析器和代码修补程序在简单的单个声明情况下工作，可以对其进行 const 处理。</span><span class="sxs-lookup"><span data-stu-id="c44e6-272">Your analyzer and code fix work on a simple case of a single declaration that can be made const.</span></span> <span data-ttu-id="c44e6-273">在许多可能的声明语句中，该实现会出错。</span><span class="sxs-lookup"><span data-stu-id="c44e6-273">There are numerous possible declaration statements where this implementation makes mistakes.</span></span> <span data-ttu-id="c44e6-274">可以通过使用模板编写的单元测试库来处理这种情况。</span><span class="sxs-lookup"><span data-stu-id="c44e6-274">You'll address these cases by working with the unit test library written by the template.</span></span> <span data-ttu-id="c44e6-275">它要比反复打开 Visual Studio 的第二个副本快得多。</span><span class="sxs-lookup"><span data-stu-id="c44e6-275">It's much faster than repeatedly opening a second copy of Visual Studio.</span></span>

<span data-ttu-id="c44e6-276">打开单元测试项目中的“MakeConstUnitTests.cs”文件。</span><span class="sxs-lookup"><span data-stu-id="c44e6-276">Open the *MakeConstUnitTests.cs* file in the unit test project.</span></span> <span data-ttu-id="c44e6-277">该模板会创建两个测试，这些测试遵循分析器和代码修补程序单元测试的两种常见模式。</span><span class="sxs-lookup"><span data-stu-id="c44e6-277">The template created two tests that follow the two common patterns for an analyzer and code fix unit test.</span></span> <span data-ttu-id="c44e6-278">`TestMethod1` 显示测试模式，确保分析器在不应报告诊断的情况下不会执行此操作。</span><span class="sxs-lookup"><span data-stu-id="c44e6-278">`TestMethod1` shows the pattern for a test that ensures the analyzer doesn't report a diagnostic when it shouldn't.</span></span> <span data-ttu-id="c44e6-279">`TestMethod2` 演示用于报告诊断和运行代码修补程序的模式。</span><span class="sxs-lookup"><span data-stu-id="c44e6-279">`TestMethod2` shows the pattern for reporting a diagnostic and running the code fix.</span></span>

<span data-ttu-id="c44e6-280">该模板使用 [Microsoft.CodeAnalysis.Testing](https://github.com/dotnet/roslyn-sdk/blob/main/src/Microsoft.CodeAnalysis.Testing/README.md) 包进行单元测试。</span><span class="sxs-lookup"><span data-stu-id="c44e6-280">The template uses [Microsoft.CodeAnalysis.Testing](https://github.com/dotnet/roslyn-sdk/blob/main/src/Microsoft.CodeAnalysis.Testing/README.md) packages for unit testing.</span></span>

> [!TIP]
> <span data-ttu-id="c44e6-281">测试库支持特殊标记语法，其中包括以下内容：</span><span class="sxs-lookup"><span data-stu-id="c44e6-281">The testing library supports a special markup syntax, including the following:</span></span>
>
> - <span data-ttu-id="c44e6-282">`[|text|]`：表示报告 `text` 的诊断信息。</span><span class="sxs-lookup"><span data-stu-id="c44e6-282">`[|text|]`: indicates that a diagnostic is reported for `text`.</span></span> <span data-ttu-id="c44e6-283">默认情况下，此格式只可用于测试由 `DiagnosticAnalyzer.SupportedDiagnostics` 提供了正好一个 `DiagnosticDescriptor` 的分析器。</span><span class="sxs-lookup"><span data-stu-id="c44e6-283">By default, this form may only be used for testing analyzers with exactly one `DiagnosticDescriptor` provided by `DiagnosticAnalyzer.SupportedDiagnostics`.</span></span>
> - <span data-ttu-id="c44e6-284">`{|ExpectedDiagnosticId:text|}`：表示针对 `text` 报告 <xref:Microsoft.CodeAnalysis.Diagnostic.Id> 为 `ExpectedDiagnosticId` 的诊断信息。</span><span class="sxs-lookup"><span data-stu-id="c44e6-284">`{|ExpectedDiagnosticId:text|}`: indicates that a diagnostic with <xref:Microsoft.CodeAnalysis.Diagnostic.Id> `ExpectedDiagnosticId` is reported for `text`.</span></span>

<span data-ttu-id="c44e6-285">将 `MakeConstUnitTest` 类中的模板测试替换为以下测试方法：</span><span class="sxs-lookup"><span data-stu-id="c44e6-285">Replace the template tests in the `MakeConstUnitTest` class with the following test method:</span></span>

[!code-csharp[test method for fix test](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#FirstFixTest "test method for fix test")]

<span data-ttu-id="c44e6-286">运行此测试，确保测试通过。</span><span class="sxs-lookup"><span data-stu-id="c44e6-286">Run this test to make sure it passes.</span></span> <span data-ttu-id="c44e6-287">在 Visual Studio 中，通过选择“测试” > “Windows” > “测试资源管理器”来打开“测试资源管理器”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-287">In Visual Studio, open the **Test Explorer** by selecting **Test** > **Windows** > **Test Explorer**.</span></span> <span data-ttu-id="c44e6-288">然后，选择“全部运行”。</span><span class="sxs-lookup"><span data-stu-id="c44e6-288">Then select **Run All**.</span></span>

## <a name="create-tests-for-valid-declarations"></a><span data-ttu-id="c44e6-289">为有效声明创建测试</span><span class="sxs-lookup"><span data-stu-id="c44e6-289">Create tests for valid declarations</span></span>

<span data-ttu-id="c44e6-290">作为一般规则，分析器应尽可能使工作量最小化以快速退出。</span><span class="sxs-lookup"><span data-stu-id="c44e6-290">As a general rule, analyzers should exit as quickly as possible, doing minimal work.</span></span> <span data-ttu-id="c44e6-291">Visual Studio 调用注册分析器作为用户编辑代码。</span><span class="sxs-lookup"><span data-stu-id="c44e6-291">Visual Studio calls registered analyzers as the user edits code.</span></span> <span data-ttu-id="c44e6-292">响应能力是一项关键要求。</span><span class="sxs-lookup"><span data-stu-id="c44e6-292">Responsiveness is a key requirement.</span></span> <span data-ttu-id="c44e6-293">有多个代码测试用例，不应引发诊断。</span><span class="sxs-lookup"><span data-stu-id="c44e6-293">There are several test cases for code that should not raise your diagnostic.</span></span> <span data-ttu-id="c44e6-294">分析器已处理这些测试中的一个，其中变量在初始化后进行了分配。</span><span class="sxs-lookup"><span data-stu-id="c44e6-294">Your analyzer already handles one of those tests, the case where a variable is assigned after being initialized.</span></span> <span data-ttu-id="c44e6-295">添加以下测试方法来表示这种情况：</span><span class="sxs-lookup"><span data-stu-id="c44e6-295">Add the following test method to represent that case:</span></span>

[!code-csharp[variable assigned](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VariableAssigned "a variable that is assigned after being initialized won't raise the diagnostic")]

<span data-ttu-id="c44e6-296">此测试也通过了。</span><span class="sxs-lookup"><span data-stu-id="c44e6-296">This test passes as well.</span></span> <span data-ttu-id="c44e6-297">接下来，为尚未处理的情况添加测试方法：</span><span class="sxs-lookup"><span data-stu-id="c44e6-297">Next, add test methods for conditions you haven't handled yet:</span></span>

- <span data-ttu-id="c44e6-298">已经是 `const` 的声明，因为它们已为 const 类型：</span><span class="sxs-lookup"><span data-stu-id="c44e6-298">Declarations that are already `const`, because they are already const:</span></span>

   [!code-csharp[already const declaration](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#AlreadyConst "a declaration that is already const should not raise the diagnostic")]

- <span data-ttu-id="c44e6-299">没有初始值设定项的声明，因为没有要使用的值：</span><span class="sxs-lookup"><span data-stu-id="c44e6-299">Declarations that have no initializer, because there is no value to use:</span></span>

   [!code-csharp[declarations that have no initializer](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#NoInitializer "a declaration that has no initializer should not raise the diagnostic")]

- <span data-ttu-id="c44e6-300">初始值设定项不是常量的声明，因为它们不能是编译时常量：</span><span class="sxs-lookup"><span data-stu-id="c44e6-300">Declarations where the initializer is not a constant, because they can't be compile-time constants:</span></span>

   [!code-csharp[declarations where the initializer isn't const](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#InitializerNotConstant "a declaration where the initializer is not a compile-time constant should not raise the diagnostic")]

<span data-ttu-id="c44e6-301">甚至可能更加复杂，因为 C# 允许多个声明作为一条语句。</span><span class="sxs-lookup"><span data-stu-id="c44e6-301">It can be even more complicated because C# allows multiple declarations as one statement.</span></span> <span data-ttu-id="c44e6-302">请考虑以下测试用例字符串常量：</span><span class="sxs-lookup"><span data-stu-id="c44e6-302">Consider the following test case string constant:</span></span>

[!code-csharp[multiple initializers](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#MultipleInitializers "A declaration can be made constant only if all variables in that statement can be made constant")]

<span data-ttu-id="c44e6-303">变量 `i` 可以常量化，但变量 `j` 不能。</span><span class="sxs-lookup"><span data-stu-id="c44e6-303">The variable `i` can be made constant, but the variable `j` cannot.</span></span> <span data-ttu-id="c44e6-304">因此，此语句不能成为 const 声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-304">Therefore, this statement cannot be made a const declaration.</span></span>

<span data-ttu-id="c44e6-305">再次运行测试，将看到这些新测试用例失败。</span><span class="sxs-lookup"><span data-stu-id="c44e6-305">Run your tests again, and you'll see these new test cases fail.</span></span>

## <a name="update-your-analyzer-to-ignore-correct-declarations"></a><span data-ttu-id="c44e6-306">更新分析器以忽略正确声明</span><span class="sxs-lookup"><span data-stu-id="c44e6-306">Update your analyzer to ignore correct declarations</span></span>

<span data-ttu-id="c44e6-307">需要对分析器的 `AnalyzeNode` 方法进行一些增强，以筛选出匹配这些条件的代码。</span><span class="sxs-lookup"><span data-stu-id="c44e6-307">You need some enhancements to your analyzer's `AnalyzeNode` method to filter out code that matches these conditions.</span></span> <span data-ttu-id="c44e6-308">它们是所有相关条件，因此类似的更改将解决所有这些条件。</span><span class="sxs-lookup"><span data-stu-id="c44e6-308">They are all related conditions, so similar changes will fix all these conditions.</span></span> <span data-ttu-id="c44e6-309">对 `AnalyzeNode` 进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="c44e6-309">Make the following changes to `AnalyzeNode`:</span></span>

- <span data-ttu-id="c44e6-310">语义分析检查单个变量声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-310">Your semantic analysis examined a single variable declaration.</span></span> <span data-ttu-id="c44e6-311">此代码必须位于 `foreach` 循环中，以检查同一语句中声明的所有变量。</span><span class="sxs-lookup"><span data-stu-id="c44e6-311">This code needs to be in a `foreach` loop that examines all the variables declared in the same statement.</span></span>
- <span data-ttu-id="c44e6-312">每个声明变量需要有初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="c44e6-312">Each declared variable needs to have an initializer.</span></span>
- <span data-ttu-id="c44e6-313">每个声明的变量的初始值设定项必须是编译时常量。</span><span class="sxs-lookup"><span data-stu-id="c44e6-313">Each declared variable's initializer must be a compile-time constant.</span></span>

<span data-ttu-id="c44e6-314">在 `AnalyzeNode` 方法中，替换原始语义分析：</span><span class="sxs-lookup"><span data-stu-id="c44e6-314">In your `AnalyzeNode` method, replace the original semantic analysis:</span></span>

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

<span data-ttu-id="c44e6-315">使用以下代码片段：</span><span class="sxs-lookup"><span data-stu-id="c44e6-315">with the following code snippet:</span></span>

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

<span data-ttu-id="c44e6-316">第一个 `foreach` 循环将使用语法分析检查每个变量声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-316">The first `foreach` loop examines each variable declaration using syntactic analysis.</span></span> <span data-ttu-id="c44e6-317">第一次检查可保证该变量具有初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="c44e6-317">The first check guarantees that the variable has an initializer.</span></span> <span data-ttu-id="c44e6-318">第二次检查可保证初始值设定项是一个常量。</span><span class="sxs-lookup"><span data-stu-id="c44e6-318">The second check guarantees that the initializer is a constant.</span></span> <span data-ttu-id="c44e6-319">第二个循环具有原始语义分析。</span><span class="sxs-lookup"><span data-stu-id="c44e6-319">The second loop has the original semantic analysis.</span></span> <span data-ttu-id="c44e6-320">语义检查是在一个单独循环中，因为它对性能具有更大的影响。</span><span class="sxs-lookup"><span data-stu-id="c44e6-320">The semantic checks are in a separate loop because it has a greater impact on performance.</span></span> <span data-ttu-id="c44e6-321">再次运行测试，应看到它们全部通过。</span><span class="sxs-lookup"><span data-stu-id="c44e6-321">Run your tests again, and you should see them all pass.</span></span>

## <a name="add-the-final-polish"></a><span data-ttu-id="c44e6-322">添加最后的润饰</span><span class="sxs-lookup"><span data-stu-id="c44e6-322">Add the final polish</span></span>

<span data-ttu-id="c44e6-323">即将完成。</span><span class="sxs-lookup"><span data-stu-id="c44e6-323">You're almost done.</span></span> <span data-ttu-id="c44e6-324">分析器还要处理一些其他条件。</span><span class="sxs-lookup"><span data-stu-id="c44e6-324">There are a few more conditions for your analyzer to handle.</span></span> <span data-ttu-id="c44e6-325">用户编写代码时，Visual Studio 将调用分析器。</span><span class="sxs-lookup"><span data-stu-id="c44e6-325">Visual Studio calls analyzers while the user is writing code.</span></span> <span data-ttu-id="c44e6-326">通常情况下，分析器将针对无法进行编译的代码进行调用。</span><span class="sxs-lookup"><span data-stu-id="c44e6-326">It's often the case that your analyzer will be called for code that doesn't compile.</span></span> <span data-ttu-id="c44e6-327">诊断分析器的 `AnalyzeNode` 方法不会检查以查看常量值是否可转换为变量类型。</span><span class="sxs-lookup"><span data-stu-id="c44e6-327">The diagnostic analyzer's `AnalyzeNode` method does not check to see if the constant value is convertible to the variable type.</span></span> <span data-ttu-id="c44e6-328">因此，当前实现会不假思索地将不正确的声明（如 int i = “abc”）转换为局部常量。</span><span class="sxs-lookup"><span data-stu-id="c44e6-328">So, the current implementation will happily convert an incorrect declaration such as int i = "abc"' to a local constant.</span></span> <span data-ttu-id="c44e6-329">为这种情况添加测试方法：</span><span class="sxs-lookup"><span data-stu-id="c44e6-329">Add a test method for this case:</span></span>

[!code-csharp[Mismatched types don't raise diagnostics](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsInvalid "When the variable type and the constant type don't match, there's no diagnostic")]

<span data-ttu-id="c44e6-330">此外，无法正确处理引用类型。</span><span class="sxs-lookup"><span data-stu-id="c44e6-330">In addition, reference types are not handled properly.</span></span> <span data-ttu-id="c44e6-331">允许用于引用类型的唯一常量值为 `null`， <xref:System.String?displayProperty=nameWithType> 这种情况除外，后者允许字符串。</span><span class="sxs-lookup"><span data-stu-id="c44e6-331">The only constant value allowed for a reference type is `null`, except in this case of <xref:System.String?displayProperty=nameWithType>, which allows string literals.</span></span> <span data-ttu-id="c44e6-332">换而言之，`const string s = "abc"` 是合法的，但 `const object s = "abc"` 不是。</span><span class="sxs-lookup"><span data-stu-id="c44e6-332">In other words, `const string s = "abc"` is legal, but `const object s = "abc"` is not.</span></span> <span data-ttu-id="c44e6-333">此代码片段验证以下条件：</span><span class="sxs-lookup"><span data-stu-id="c44e6-333">This code snippet verifies that condition:</span></span>

[!code-csharp[Reference types don't raise diagnostics](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsntString "When the variable type is a reference type other than string, there's no diagnostic")]

<span data-ttu-id="c44e6-334">为全面起见，需要添加另一个测试以确保你可以为字符串创建常量声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-334">To be thorough, you need to add another test to make sure that you can create a constant declaration for a string.</span></span> <span data-ttu-id="c44e6-335">以下代码片段定义引发诊断的代码和在应用修补程序后的代码：</span><span class="sxs-lookup"><span data-stu-id="c44e6-335">The following snippet defines both the code that raises the diagnostic, and the code after the fix has been applied:</span></span>

[!code-csharp[string reference types raise diagnostics](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#ConstantIsString "When the variable type is string, it can be constant")]

<span data-ttu-id="c44e6-336">最后，如使用关键字 `var` 声明变量，代码修补程序将执行错误操作，并生成 `const var` 声明，C# 语言不支持该声明。</span><span class="sxs-lookup"><span data-stu-id="c44e6-336">Finally, if a variable is declared with the `var` keyword, the code fix does the wrong thing and generates a `const var` declaration, which is not supported by the C# language.</span></span> <span data-ttu-id="c44e6-337">若要修复此 bug，代码修补程序必须将 `var` 关键字替换为推断类型的名称：</span><span class="sxs-lookup"><span data-stu-id="c44e6-337">To fix this bug, the code fix must replace the `var` keyword with the inferred type's name:</span></span>

[!code-csharp[var references need to use the inferred types](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VarDeclarations "Declarations made using var must have the type replaced with the inferred type")]

<span data-ttu-id="c44e6-338">幸运的是，所有上述 bug 可以使用你刚刚了解的相同技术解决。</span><span class="sxs-lookup"><span data-stu-id="c44e6-338">Fortunately, all of the above bugs can be addressed using the same techniques that you just learned.</span></span>

<span data-ttu-id="c44e6-339">若要修复第一个 bug，请先打开“MakeConstAnalyzer.cs”，并找到 foreach 循环，将检查其中每个局部声明的初始值设定项以确保向其分配常量值。</span><span class="sxs-lookup"><span data-stu-id="c44e6-339">To fix the first bug, first open *MakeConstAnalyzer.cs* and locate the foreach loop where each of the local declaration's initializers are checked to ensure that they're assigned with constant values.</span></span> <span data-ttu-id="c44e6-340">在第一个 foreach 循环之前，立即调用 `context.SemanticModel.GetTypeInfo()` 来检索有关局部声明的声明类型的详细信息：</span><span class="sxs-lookup"><span data-stu-id="c44e6-340">Immediately _before_ the first foreach loop, call `context.SemanticModel.GetTypeInfo()` to retrieve detailed information about the declared type of the local declaration:</span></span>

[!code-csharp[Retrieve type information](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#VariableConvertedType "Retrieve type information")]

<span data-ttu-id="c44e6-341">然后，在 `foreach` 循环中，检查每个初始值设定项，以确保它可以转换为变量类型。</span><span class="sxs-lookup"><span data-stu-id="c44e6-341">Then, inside your `foreach` loop, check each initializer to make sure it's convertible to the variable type.</span></span> <span data-ttu-id="c44e6-342">确保初始值设定项为常量后，添加以下检查：</span><span class="sxs-lookup"><span data-stu-id="c44e6-342">Add the following check after ensuring that the initializer is a constant:</span></span>

[!code-csharp[Ensure non-user-defined conversion](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#BailOutOnUserDefinedConversion "Bail-out on user-defined conversion")]

<span data-ttu-id="c44e6-343">下一次更改建立在最后一次更改之上。</span><span class="sxs-lookup"><span data-stu-id="c44e6-343">The next change builds upon the last one.</span></span> <span data-ttu-id="c44e6-344">在第一个 foreach 循环的右大括号前，添加以下代码以检查当常量为字符串或 NULL 时局部声明的类型。</span><span class="sxs-lookup"><span data-stu-id="c44e6-344">Before the closing curly brace of the first foreach loop, add the following code to check the type of the local declaration when the constant is a string or null.</span></span>

[!code-csharp[Handle special cases](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst/MakeConstAnalyzer.cs#HandleSpecialCases "Handle special cases")]

<span data-ttu-id="c44e6-345">必须在代码修复提供程序中编写更多代码以将 `var` 关键字替换为正确类型名称。</span><span class="sxs-lookup"><span data-stu-id="c44e6-345">You must write a bit more code in your code fix provider to replace the `var` keyword with the correct type name.</span></span> <span data-ttu-id="c44e6-346">返回到 MakeConstCodeFixProvider.cs。</span><span class="sxs-lookup"><span data-stu-id="c44e6-346">Return to *MakeConstCodeFixProvider.cs*.</span></span> <span data-ttu-id="c44e6-347">要添加的代码将执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="c44e6-347">The code you'll add does the following steps:</span></span>

- <span data-ttu-id="c44e6-348">检查声明是否为 `var` 声明，如果它是：</span><span class="sxs-lookup"><span data-stu-id="c44e6-348">Check if the declaration is a `var` declaration, and if it is:</span></span>
- <span data-ttu-id="c44e6-349">创建新类型的推断类型。</span><span class="sxs-lookup"><span data-stu-id="c44e6-349">Create a new type for the inferred type.</span></span>
- <span data-ttu-id="c44e6-350">确保类型声明不是别名。</span><span class="sxs-lookup"><span data-stu-id="c44e6-350">Make sure the type declaration is not an alias.</span></span> <span data-ttu-id="c44e6-351">如果是这样，则声明 `const var` 是合法的。</span><span class="sxs-lookup"><span data-stu-id="c44e6-351">If so, it is legal to declare `const var`.</span></span>
- <span data-ttu-id="c44e6-352">确保 `var` 不是此程序中的类型名称。</span><span class="sxs-lookup"><span data-stu-id="c44e6-352">Make sure that `var` isn't a type name in this program.</span></span> <span data-ttu-id="c44e6-353">（如果是这样，则 `const var` 是合法的）。</span><span class="sxs-lookup"><span data-stu-id="c44e6-353">(If so, `const var` is legal).</span></span>
- <span data-ttu-id="c44e6-354">简化完整类型名称</span><span class="sxs-lookup"><span data-stu-id="c44e6-354">Simplify the full type name</span></span>

<span data-ttu-id="c44e6-355">这听起来好像有很多代码。</span><span class="sxs-lookup"><span data-stu-id="c44e6-355">That sounds like a lot of code.</span></span> <span data-ttu-id="c44e6-356">其实不然。</span><span class="sxs-lookup"><span data-stu-id="c44e6-356">It's not.</span></span> <span data-ttu-id="c44e6-357">将声明和初始化 `newLocal` 的行替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="c44e6-357">Replace the line that declares and initializes `newLocal` with the following code.</span></span> <span data-ttu-id="c44e6-358">在初始化 `newModifiers` 之后立即进行：</span><span class="sxs-lookup"><span data-stu-id="c44e6-358">It goes immediately after the initialization of `newModifiers`:</span></span>

[!code-csharp[Replace Var designations](snippets/how-to-write-csharp-analyzer-code-fix/MakeConst/MakeConst.CodeFixes/MakeConstCodeFixProvider.cs#ReplaceVar "Replace a var designation with the explicit type")]

<span data-ttu-id="c44e6-359">需要添加一个 `using` 指令才能使用 <xref:Microsoft.CodeAnalysis.Simplification.Simplifier> 类型：</span><span class="sxs-lookup"><span data-stu-id="c44e6-359">You'll need to add one `using` directive to use the <xref:Microsoft.CodeAnalysis.Simplification.Simplifier> type:</span></span>

```csharp
using Microsoft.CodeAnalysis.Simplification;
```

<span data-ttu-id="c44e6-360">运行测试，它们应全部通过。</span><span class="sxs-lookup"><span data-stu-id="c44e6-360">Run your tests, and they should all pass.</span></span> <span data-ttu-id="c44e6-361">通过运行已完成的分析器自行庆祝。</span><span class="sxs-lookup"><span data-stu-id="c44e6-361">Congratulate yourself by running your finished analyzer.</span></span> <span data-ttu-id="c44e6-362">按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 在加载了 Roslyn Preview 扩展的第二个 Visual Studio 实例中运行分析器项目。</span><span class="sxs-lookup"><span data-stu-id="c44e6-362">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the analyzer project in a second instance of Visual Studio with the Roslyn Preview extension loaded.</span></span>

- <span data-ttu-id="c44e6-363">在第二个 Visual Studio 实例，创建一个新的 C# 控制台应用程序项目并将 `int x = "abc";` 添加到 Main 方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-363">In the second Visual Studio instance, create a new C# Console Application project and add `int x = "abc";` to the Main method.</span></span> <span data-ttu-id="c44e6-364">由于第一个 bug 已修复，应不会报告针对此局部变量声明的警告（尽管像预期那样出现了编译器错误）。</span><span class="sxs-lookup"><span data-stu-id="c44e6-364">Thanks to the first bug fix, no warning should be reported for this local variable declaration (though there's a compiler error as expected).</span></span>
- <span data-ttu-id="c44e6-365">接下来，将 `object s = "abc";` 添加到 Main 方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-365">Next, add `object s = "abc";` to the Main method.</span></span> <span data-ttu-id="c44e6-366">由于第二个 bug 已修复，应不会报告任何警告。</span><span class="sxs-lookup"><span data-stu-id="c44e6-366">Because of the second bug fix, no warning should be reported.</span></span>
- <span data-ttu-id="c44e6-367">最后，添加另一个使用 `var` 关键字的局部变量。</span><span class="sxs-lookup"><span data-stu-id="c44e6-367">Finally, add another local variable that uses the `var` keyword.</span></span> <span data-ttu-id="c44e6-368">你将看到一个警告和显示在左下方的一个建议。</span><span class="sxs-lookup"><span data-stu-id="c44e6-368">You'll see that a warning is reported and a suggestion appears beneath to the left.</span></span>
- <span data-ttu-id="c44e6-369">将编辑器插入点移到波浪下划线，然后按 <kbd>Ctrl</kbd>+<kbd>。</kbd></span><span class="sxs-lookup"><span data-stu-id="c44e6-369">Move the editor caret over the squiggly underline and press <kbd>Ctrl</kbd>+<kbd>.</kbd>.</span></span> <span data-ttu-id="c44e6-370">显示建议的代码修补程序。</span><span class="sxs-lookup"><span data-stu-id="c44e6-370">to display the suggested code fix.</span></span> <span data-ttu-id="c44e6-371">选择代码修补程序，请注意，`var` 关键字现已正确处理。</span><span class="sxs-lookup"><span data-stu-id="c44e6-371">Upon selecting your code fix, note that the `var` keyword is now handled correctly.</span></span>

<span data-ttu-id="c44e6-372">最后，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="c44e6-372">Finally, add the following code:</span></span>

```csharp
int i = 2;
int j = 32;
int k = i + j;
```

<span data-ttu-id="c44e6-373">完成这些更改后，仅在前两个变量上有红色波浪线。</span><span class="sxs-lookup"><span data-stu-id="c44e6-373">After these changes, you get red squiggles only on the first two variables.</span></span> <span data-ttu-id="c44e6-374">将 `const` 同时添加到 `i` 和 `j`，你将获得一个有关 `k` 的新警告，因为它现在可以是 `const`。</span><span class="sxs-lookup"><span data-stu-id="c44e6-374">Add `const` to both `i` and `j`, and you get a new warning on `k` because it can now be `const`.</span></span>

<span data-ttu-id="c44e6-375">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="c44e6-375">Congratulations!</span></span> <span data-ttu-id="c44e6-376">你已创建第一个 .NET Compiler Platform 扩展来执行即时代码分析，以便检测问题，并提供了用于快速更正的修补程序。</span><span class="sxs-lookup"><span data-stu-id="c44e6-376">You've created your first .NET Compiler Platform extension that performs on-the-fly code analysis to detect an issue and provides a quick fix to correct it.</span></span> <span data-ttu-id="c44e6-377">在此过程中，你已了解很多代码 API 是 .NET Compiler Platform SDK (Roslyn API) 的一部分。</span><span class="sxs-lookup"><span data-stu-id="c44e6-377">Along the way, you've learned many of the code APIs that are part of the .NET Compiler Platform SDK (Roslyn APIs).</span></span> <span data-ttu-id="c44e6-378">可以在我们的示例 GitHub 存储库中根据[完成的示例](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/Tutorials/MakeConst)来检查工作。</span><span class="sxs-lookup"><span data-stu-id="c44e6-378">You can check your work against the [completed sample](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/Tutorials/MakeConst) in our samples GitHub repository.</span></span>

## <a name="other-resources"></a><span data-ttu-id="c44e6-379">其他资源</span><span class="sxs-lookup"><span data-stu-id="c44e6-379">Other resources</span></span>

- [<span data-ttu-id="c44e6-380">语法分析入门</span><span class="sxs-lookup"><span data-stu-id="c44e6-380">Get started with syntax analysis</span></span>](../get-started/syntax-analysis.md)
- [<span data-ttu-id="c44e6-381">语义分析入门</span><span class="sxs-lookup"><span data-stu-id="c44e6-381">Get started with semantic analysis</span></span>](../get-started/semantic-analysis.md)
