---
title: 确定要迁移的项目序列
description: 大型应用程序通常不会同时迁移到新的平台，而是一系列较小的步骤。 了解如何规划将 ASP.NET MVC 应用程序迁移到 ASP.NET Core 的步骤。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 34bcfbf4ea3941329419ba64253a2177247df7b8
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122906"
---
# <a name="identify-sequence-of-projects-to-migrate"></a><span data-ttu-id="ab013-104">确定要迁移的项目序列</span><span class="sxs-lookup"><span data-stu-id="ab013-104">Identify sequence of projects to migrate</span></span>

<span data-ttu-id="ab013-105">对于涉及多个前端应用的解决方案，最好逐个迁移应用。</span><span class="sxs-lookup"><span data-stu-id="ab013-105">For solutions that involve multiple front-end apps, it's best to migrate the apps one by one.</span></span> <span data-ttu-id="ab013-106">例如，创建一个仅包含一个前端应用及其依赖项的解决方案，以便你可以轻松地确定所涉及的工作范围。</span><span class="sxs-lookup"><span data-stu-id="ab013-106">For example, create a solution that only includes one front-end app and its dependencies so you can easily identify the scope of work involved.</span></span> <span data-ttu-id="ab013-107">解决方案是轻型的，如果需要，可以在多个解决方案中包含项目。</span><span class="sxs-lookup"><span data-stu-id="ab013-107">Solutions are lightweight, and you can include projects in multiple solutions if needed.</span></span> <span data-ttu-id="ab013-108">迁移时，可使用解决方案作为组织工具。</span><span class="sxs-lookup"><span data-stu-id="ab013-108">Take advantage of solutions as an organizational tool when migrating.</span></span>

<span data-ttu-id="ab013-109">确定要迁移的 ASP.NET 应用并使其相关项目 (理想情况下，在解决方案) 中，下一步是确定框架和 NuGet 依赖项。</span><span class="sxs-lookup"><span data-stu-id="ab013-109">Once you've identified the ASP.NET app to migrate and have its dependent projects located with it (ideally in a solution), the next step is to identify framework and NuGet dependencies.</span></span> <span data-ttu-id="ab013-110">识别所有依赖项后，最简单的迁移方法是 "自下而上的" 方法。</span><span class="sxs-lookup"><span data-stu-id="ab013-110">Having identified all dependencies, the simplest migration approach is a "bottom up" approach.</span></span> <span data-ttu-id="ab013-111">利用这种方法，首先迁移最低级别的依赖项。</span><span class="sxs-lookup"><span data-stu-id="ab013-111">With this approach, the lowest level of dependencies is migrated first.</span></span> <span data-ttu-id="ab013-112">然后，将迁移下一级别的依赖项，直到最终只有一个前端应用。</span><span class="sxs-lookup"><span data-stu-id="ab013-112">Then the next level of dependencies is migrated, until eventually the only thing left is the front-end app.</span></span> <span data-ttu-id="ab013-113">图3-1 显示了一组构成应用程序的项目示例。</span><span class="sxs-lookup"><span data-stu-id="ab013-113">Figure 3-1 shows an example set of projects composing an app.</span></span> <span data-ttu-id="ab013-114">低级别类库位于底部，而 ASP.NET MVC 项目位于顶部。</span><span class="sxs-lookup"><span data-stu-id="ab013-114">The low-level class libraries are at the bottom, and the ASP.NET MVC project is at the top.</span></span>

![项目依赖项](./media/Figure3-1.png)

<span data-ttu-id="ab013-116">**图 3-1。**</span><span class="sxs-lookup"><span data-stu-id="ab013-116">**Figure 3-1.**</span></span> <span data-ttu-id="ab013-117">项目依赖项关系图。</span><span class="sxs-lookup"><span data-stu-id="ab013-117">Project dependencies graph.</span></span>

<span data-ttu-id="ab013-118">选择一个特定的前端应用，即一个 ASP.NET MVC 5/Web API 2 项目。</span><span class="sxs-lookup"><span data-stu-id="ab013-118">Choose a particular front-end app, an ASP.NET MVC 5 / Web API 2 project.</span></span> <span data-ttu-id="ab013-119">确定其在解决方案中的依赖关系，并映射其依赖项，直至获得完整列表。</span><span class="sxs-lookup"><span data-stu-id="ab013-119">Identify its dependencies in the solution, and map out their dependencies until you have a complete list.</span></span> <span data-ttu-id="ab013-120">映射项目依赖项时，图3-1 中所示的关系图可能很有用。</span><span class="sxs-lookup"><span data-stu-id="ab013-120">A diagram like the one shown in Figure 3-1 may be useful when mapping out project dependencies.</span></span> <span data-ttu-id="ab013-121">Visual Studio 可以根据你使用的版本， [为你的解决方案生成依赖关系图](/visualstudio/modeling/create-layer-diagrams-from-your-code)。</span><span class="sxs-lookup"><span data-stu-id="ab013-121">Visual Studio can produce a [dependency diagram for your solution](/visualstudio/modeling/create-layer-diagrams-from-your-code), depending on which edition you're using.</span></span> <span data-ttu-id="ab013-122">[.Net 可移植性分析器](../../standard/analyzers/portability-analyzer.md) 还可以生成依赖关系图。</span><span class="sxs-lookup"><span data-stu-id="ab013-122">[The .NET Portability Analyzer](../../standard/analyzers/portability-analyzer.md) can also produce dependency diagrams.</span></span>

<span data-ttu-id="ab013-123">图3-2 显示了 ".Net 可 [移植性分析器" Visual Studio 扩展](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)的安装程序：</span><span class="sxs-lookup"><span data-stu-id="ab013-123">Figure 3-2 shows the installer for the [.NET Portability Analyzer Visual Studio extension](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer):</span></span>

![安装 .NET 可移植性分析器扩展](./media/Figure3-2.png)

<span data-ttu-id="ab013-125">**图3-2。**</span><span class="sxs-lookup"><span data-stu-id="ab013-125">**Figure 3-2.**</span></span> <span data-ttu-id="ab013-126">.NET 可移植性分析器安装程序。</span><span class="sxs-lookup"><span data-stu-id="ab013-126">.NET Portability Analyzer installer.</span></span>

<span data-ttu-id="ab013-127">此扩展插件支持 Visual Studio 2017 及更高版本。</span><span class="sxs-lookup"><span data-stu-id="ab013-127">The extension supports Visual Studio 2017 and later.</span></span> <span data-ttu-id="ab013-128">安装完成后，可在 "**分析** 可  >  **移植性分析器设置**" 菜单中进行配置，如图3-3 所示。</span><span class="sxs-lookup"><span data-stu-id="ab013-128">Once installed, you configure it from the **Analyze** > **Portability Analyzer Settings** menu, as shown in Figure 3-3.</span></span>

![配置 .NET 可移植性分析器扩展](./media/Figure3-3.png)

<span data-ttu-id="ab013-130">**图3-3。**</span><span class="sxs-lookup"><span data-stu-id="ab013-130">**Figure 3-3.**</span></span> <span data-ttu-id="ab013-131">配置 .NET 可移植性分析器。</span><span class="sxs-lookup"><span data-stu-id="ab013-131">Configure the .NET Portability Analyzer.</span></span>

<span data-ttu-id="ab013-132">分析器生成每个程序集的详细报表。</span><span class="sxs-lookup"><span data-stu-id="ab013-132">The analyzer produces a detailed report for each assembly.</span></span> <span data-ttu-id="ab013-133">报表：</span><span class="sxs-lookup"><span data-stu-id="ab013-133">The report:</span></span>

* <span data-ttu-id="ab013-134">描述每个项目与给定目标框架（如 .NET Core 3.1 或 .NET Standard 2.0）的兼容性。</span><span class="sxs-lookup"><span data-stu-id="ab013-134">Describes how compatible each project is with a given target framework, such as .NET Core 3.1 or .NET Standard 2.0.</span></span>
* <span data-ttu-id="ab013-135">帮助团队评估将特定项目移植到特定目标框架所需的工作量。</span><span class="sxs-lookup"><span data-stu-id="ab013-135">Helps teams assess the effort required to port a particular project to a particular target framework.</span></span>

<span data-ttu-id="ab013-136">下一节将介绍此分析的详细信息。</span><span class="sxs-lookup"><span data-stu-id="ab013-136">The details of this analysis are covered in the next section.</span></span>

<span data-ttu-id="ab013-137">映射出项目及其相互关系后，就可以确定迁移项目的顺序了，这是一项工作。</span><span class="sxs-lookup"><span data-stu-id="ab013-137">Once you've mapped out the projects and their relationships with one another, you're ready to determine the order in which you'll migrate the projects.</span></span> <span data-ttu-id="ab013-138">从没有依赖项的项目开始。</span><span class="sxs-lookup"><span data-stu-id="ab013-138">Begin with projects that have no dependencies.</span></span> <span data-ttu-id="ab013-139">然后，将树的工作方式设置为依赖于这些项目的项目。</span><span class="sxs-lookup"><span data-stu-id="ab013-139">Then, work your way up the tree to the projects that depend on these projects.</span></span>

<span data-ttu-id="ab013-140">在图3-1 所示的示例中，你将从 *Utils* 项目开始，因为它不依赖于任何其他项目。</span><span class="sxs-lookup"><span data-stu-id="ab013-140">In the example shown in Figure 3-1, you would start with the *Contoso.Utils* project, since it doesn't depend on any other projects.</span></span> <span data-ttu-id="ab013-141">接下来， *Contoso. 数据* 仅依赖于 "Utils"。</span><span class="sxs-lookup"><span data-stu-id="ab013-141">Next, *Contoso.Data* since it only depends on "Utils".</span></span> <span data-ttu-id="ab013-142">然后迁移 "Businesslogic.dll" 库，最后迁移前端 ASP.NET "Web" 项目。</span><span class="sxs-lookup"><span data-stu-id="ab013-142">Then migrate the "BusinessLogic" library, and finally the front-end ASP.NET "Web" project.</span></span> <span data-ttu-id="ab013-143">下面的 "自下而上" 方法非常适用于相对较小且分解良好的应用程序，这些应用程序的所有项目都已迁移后可作为一个单元进行迁移。</span><span class="sxs-lookup"><span data-stu-id="ab013-143">Following this "bottom up" approach works well for relatively small and well-factored apps that can be migrated as a unit once all of their projects have migrated.</span></span> <span data-ttu-id="ab013-144">具有更多复杂性或更长时间迁移的更多代码的较大应用可能需要考虑更多增量策略。</span><span class="sxs-lookup"><span data-stu-id="ab013-144">Larger apps with more complexity, or more code that will take longer to migrate, may need to consider more incremental strategies.</span></span>

## <a name="unit-tests"></a><span data-ttu-id="ab013-145">单元测试</span><span class="sxs-lookup"><span data-stu-id="ab013-145">Unit tests</span></span>

<span data-ttu-id="ab013-146">前面的关系图中缺少的是单元测试项目。</span><span class="sxs-lookup"><span data-stu-id="ab013-146">Missing from the previous diagrams are unit test projects.</span></span> <span data-ttu-id="ab013-147">但愿有些测试至少涵盖了正在移植的库的某些现有行为。</span><span class="sxs-lookup"><span data-stu-id="ab013-147">Hopefully there are tests covering at least some of the existing behavior of the libraries being ported.</span></span>

<span data-ttu-id="ab013-148">如果你有单元测试，最好先转换这些项目。</span><span class="sxs-lookup"><span data-stu-id="ab013-148">If you have unit tests, it's best to convert those projects first.</span></span> <span data-ttu-id="ab013-149">你需要继续测试正在处理的项目中的更改。</span><span class="sxs-lookup"><span data-stu-id="ab013-149">You'll want to continue testing changes in the project you're working on.</span></span> <span data-ttu-id="ab013-150">请记住，移植到 .NET Core 对代码库进行了重大更改。</span><span class="sxs-lookup"><span data-stu-id="ab013-150">Remember that porting to .NET Core is a significant change to your codebase.</span></span>

<span data-ttu-id="ab013-151">MSTest、xUnit 和 NUnit 都适用于 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="ab013-151">MSTest, xUnit, and NUnit all work on .NET Core.</span></span> <span data-ttu-id="ab013-152">如果当前没有适用于你的应用程序的测试，请考虑构建一些用于验证系统当前行为的特性测试。</span><span class="sxs-lookup"><span data-stu-id="ab013-152">If you don't currently have tests for your app, consider building some characterization tests that verify the system's current behavior.</span></span> <span data-ttu-id="ab013-153">优点是迁移完成后，可以确认应用的行为保持不变。</span><span class="sxs-lookup"><span data-stu-id="ab013-153">The benefit is that once the migration is complete, you can confirm the app's behavior remains unchanged.</span></span>

## <a name="considerations-for-migrating-many-apps"></a><span data-ttu-id="ab013-154">迁移多个应用时的注意事项</span><span class="sxs-lookup"><span data-stu-id="ab013-154">Considerations for migrating many apps</span></span>

<span data-ttu-id="ab013-155">某些组织将有许多不同的应用程序进行迁移，手动迁移每个应用可能需要 tenable 过多的资源。</span><span class="sxs-lookup"><span data-stu-id="ab013-155">Some organizations will have many different apps to migrate, and migrating each one by hand may require too many resources to be tenable.</span></span> <span data-ttu-id="ab013-156">在这些情况下，建议一定程度的自动化。</span><span class="sxs-lookup"><span data-stu-id="ab013-156">In these situations, some degree of automation is recommended.</span></span> <span data-ttu-id="ab013-157">本章后面的步骤可自动执行。</span><span class="sxs-lookup"><span data-stu-id="ab013-157">The steps followed in this chapter can be automated.</span></span> <span data-ttu-id="ab013-158">结构更改（如项目文件差异和常见包的更新）可通过脚本来应用。</span><span class="sxs-lookup"><span data-stu-id="ab013-158">Structural changes, like project file differences and updates to common packages, can be applied by scripts.</span></span> <span data-ttu-id="ab013-159">可以优化这些脚本，因为它们在多个项目上以迭代方式运行。</span><span class="sxs-lookup"><span data-stu-id="ab013-159">These scripts can be refined as they're run iteratively on more projects.</span></span> <span data-ttu-id="ab013-160">每次运行时，都会检查每个项目是否需要任何手动步骤。</span><span class="sxs-lookup"><span data-stu-id="ab013-160">On each run, examine whatever manual steps are required for each project.</span></span> <span data-ttu-id="ab013-161">如果可能，自动执行这些手动步骤。</span><span class="sxs-lookup"><span data-stu-id="ab013-161">Automate those manual steps, if possible.</span></span> <span data-ttu-id="ab013-162">使用此方法时，组织应在一段时间内迁移其应用时更快、更好地增长，并改进了自动化支持的每个步骤。</span><span class="sxs-lookup"><span data-stu-id="ab013-162">Using this approach, the organization should grow faster and better at porting their apps over time, with improved automation support each step of the way.</span></span>

<span data-ttu-id="ab013-163">观看有关如何在此 [dotNetConf 演示文稿中通过 Lizzy Gallagher Of Mastercard](https://www.youtube.com/watch?v=C-2haqb60No)使用此方法的概述。</span><span class="sxs-lookup"><span data-stu-id="ab013-163">Watch an overview of how to employ this approach in this [dotNetConf presentation by Lizzy Gallagher of Mastercard](https://www.youtube.com/watch?v=C-2haqb60No).</span></span> <span data-ttu-id="ab013-164">此演示中采用的五个阶段包括：</span><span class="sxs-lookup"><span data-stu-id="ab013-164">The five phases employed in this presentation included:</span></span>

- <span data-ttu-id="ab013-165">迁移第三方 NuGet 依赖项</span><span class="sxs-lookup"><span data-stu-id="ab013-165">Migrate third-party NuGet dependencies</span></span>
- <span data-ttu-id="ab013-166">迁移应用以使用新 *的 .csproj* 文件格式</span><span class="sxs-lookup"><span data-stu-id="ab013-166">Migrate apps to use new *.csproj* file format</span></span>
- <span data-ttu-id="ab013-167">将内部 NuGet 依赖项更新到 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="ab013-167">Update internal NuGet dependencies to .NET Standard</span></span>
- <span data-ttu-id="ab013-168">将应用迁移到 ASP.NET Core (目标 .NET Framework) </span><span class="sxs-lookup"><span data-stu-id="ab013-168">Migrate apps to ASP.NET Core (targeting .NET Framework)</span></span>
- <span data-ttu-id="ab013-169">将所有应用更新为面向 .NET Core 3。1</span><span class="sxs-lookup"><span data-stu-id="ab013-169">Update all apps to target .NET Core 3.1</span></span>

<span data-ttu-id="ab013-170">当应用程序自动执行时，如果应用程序遵循一致的编码准则和项目组织，则会很有帮助。</span><span class="sxs-lookup"><span data-stu-id="ab013-170">When automating a large suite of apps, it helps significantly if they follow consistent coding guidelines and project organization.</span></span> <span data-ttu-id="ab013-171">自动化工作依赖于此一致性才能有效。</span><span class="sxs-lookup"><span data-stu-id="ab013-171">Automation efforts rely on this consistency to be effective.</span></span> <span data-ttu-id="ab013-172">除了分析和迁移项目文件外，还可以自动迁移常见的代码模式。</span><span class="sxs-lookup"><span data-stu-id="ab013-172">In addition to parsing and migrating project files, common code patterns can be migrated automatically.</span></span> <span data-ttu-id="ab013-173">一些代码模式示例包括如何声明控制器操作的方式，或它们如何返回结果。</span><span class="sxs-lookup"><span data-stu-id="ab013-173">Some code pattern examples include differences in how controller actions are declared or how they return results.</span></span>

<span data-ttu-id="ab013-174">例如，迁移脚本可以在与以下模式之一 *匹配的* 代码行中搜索匹配的文件：</span><span class="sxs-lookup"><span data-stu-id="ab013-174">For example, a migration script could search files matching *Controller.cs* for lines of code matching one of these patterns:</span></span>

```csharp
   return new HttpStatusCodeResult(200);
   // or
   return new HttpStatusCodeResult(HttpStatusCode.OK);
```

<span data-ttu-id="ab013-175">在 ASP.NET Core 中，前面的代码行都可以替换为：</span><span class="sxs-lookup"><span data-stu-id="ab013-175">In ASP.NET Core, either of the preceding lines of code can be replaced with:</span></span>

```csharp
    return Ok();
```

## <a name="summary"></a><span data-ttu-id="ab013-176">总结</span><span class="sxs-lookup"><span data-stu-id="ab013-176">Summary</span></span>

<span data-ttu-id="ab013-177">将大型 .NET Framework 应用移植到 .NET Core 的最佳方法是：</span><span class="sxs-lookup"><span data-stu-id="ab013-177">The best approach to porting a large .NET Framework app to .NET Core is to:</span></span>

1. <span data-ttu-id="ab013-178">标识项目依赖项。</span><span class="sxs-lookup"><span data-stu-id="ab013-178">Identify project dependencies.</span></span>
1. <span data-ttu-id="ab013-179">分析每个项目的端口所需的内容。</span><span class="sxs-lookup"><span data-stu-id="ab013-179">Analyze what's required to port each project.</span></span>
1. <span data-ttu-id="ab013-180">从下向上启动。</span><span class="sxs-lookup"><span data-stu-id="ab013-180">Start from the bottom up.</span></span>

<span data-ttu-id="ab013-181">可以使用 ".NET 可移植性分析器" 来确定兼容现有库与目标平台之间的兼容性。</span><span class="sxs-lookup"><span data-stu-id="ab013-181">You can use the .NET Portability Analyzer to determine how compatible existing libraries may be with target platforms.</span></span> <span data-ttu-id="ab013-182">自动测试可帮助确保在应用程序移植时不会引入重大更改。</span><span class="sxs-lookup"><span data-stu-id="ab013-182">Automated tests will help ensure no breaking changes are introduced as the app is ported.</span></span> <span data-ttu-id="ab013-183">这些测试项目应位于第一个移植的项目中。</span><span class="sxs-lookup"><span data-stu-id="ab013-183">These test projects should be among the first projects ported.</span></span>

## <a name="references"></a><span data-ttu-id="ab013-184">参考</span><span class="sxs-lookup"><span data-stu-id="ab013-184">References</span></span>

- [<span data-ttu-id="ab013-185">从 .NET Framework 移植到 .NET Core</span><span class="sxs-lookup"><span data-stu-id="ab013-185">Porting from .NET Framework to .NET Core</span></span>](../../core/porting/index.md)
- [<span data-ttu-id="ab013-186">.NET 可移植性分析器</span><span class="sxs-lookup"><span data-stu-id="ab013-186">The .NET Portability Analyzer</span></span>](../../standard/analyzers/portability-analyzer.md)
- [<span data-ttu-id="ab013-187">第9频道： ".NET 可移植性分析器" (视频) </span><span class="sxs-lookup"><span data-stu-id="ab013-187">Channel 9: A Brief Look at the .NET Portability Analyzer (Video)</span></span>](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)
- [<span data-ttu-id="ab013-188">2年，200应用：大规模的 .NET Core 迁移 (视频) </span><span class="sxs-lookup"><span data-stu-id="ab013-188">2 Years, 200 Apps: A .NET Core Migration at Scale (Video)</span></span>](https://www.youtube.com/watch?v=C-2haqb60No)

>[!div class="step-by-step"]
><span data-ttu-id="ab013-189">[上一页](migrate-large-solutions.md)
>[下一页](understand-update-dependencies.md)</span><span class="sxs-lookup"><span data-stu-id="ab013-189">[Previous](migrate-large-solutions.md)
[Next](understand-update-dependencies.md)</span></span>
