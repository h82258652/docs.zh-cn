---
title: .NET 术语表
description: 了解 .NET 文档中所用的选定术语的含义。
ms.date: 11/16/2020
ms.openlocfilehash: 009ab0266a4479dfd8a37cb3261ca6fae7c78b8e
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873180"
---
# <a name="net-glossary"></a><span data-ttu-id="a0ea9-103">.NET 术语表</span><span class="sxs-lookup"><span data-stu-id="a0ea9-103">.NET Glossary</span></span>

<span data-ttu-id="a0ea9-104">此术语表的主要目的是阐明所选术语和缩写词的含义，这些词频繁出现在 .NET 文档中但没有定义。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-104">The primary goal of this glossary is to clarify meanings of selected terms and acronyms that appear frequently in the .NET documentation without definitions.</span></span>

## <a name="aot"></a><span data-ttu-id="a0ea9-105">AOT</span><span class="sxs-lookup"><span data-stu-id="a0ea9-105">AOT</span></span>

<span data-ttu-id="a0ea9-106">预编译器。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-106">Ahead-of-time compiler.</span></span>

<span data-ttu-id="a0ea9-107">与 [JIT](#jit) 类似，此编译器还可将 [IL](#il) 转换为机器代码。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-107">Similar to [JIT](#jit), this compiler also translates [IL](#il) to machine code.</span></span> <span data-ttu-id="a0ea9-108">与 JIT 编译相比，AOT 编译在应用程序执行前进行并且通常在不同计算机上执行。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-108">In contrast to JIT compilation, AOT compilation happens before the application is executed and is usually performed on a different machine.</span></span> <span data-ttu-id="a0ea9-109">AOT 工具不会在运行时进行编译，因此它们不需要最大程度地减少编译所花费的时间。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-109">Because AOT tool chains don't compile at run time, they don't have to minimize time spent compiling.</span></span> <span data-ttu-id="a0ea9-110">这意味着它们可花更多的时间进行优化。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-110">That means they can spend more time optimizing.</span></span> <span data-ttu-id="a0ea9-111">由于 AOT 的上下文是整个应用程序，因此 AOT 编译器还会执行跨模块链接和全程序分析，这意味着之后会进行所有引用并会生成单个可执行文件。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-111">Since the context of AOT is the entire application, the AOT compiler also performs cross-module linking and whole-program analysis, which means that all references are followed and a single executable is produced.</span></span>

<span data-ttu-id="a0ea9-112">请参阅 [CoreRT](#corert) 和 [.NET Native](#net-native)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-112">See [CoreRT](#corert) and [.NET Native](#net-native).</span></span>

## <a name="app-model"></a><span data-ttu-id="a0ea9-113">应用模型</span><span class="sxs-lookup"><span data-stu-id="a0ea9-113">app model</span></span>

<span data-ttu-id="a0ea9-114">特定于[工作负载](#workload)的 API。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-114">A [workload](#workload)-specific API.</span></span> <span data-ttu-id="a0ea9-115">下面是一些示例：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-115">Here are some examples:</span></span>

* <span data-ttu-id="a0ea9-116">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a0ea9-116">ASP.NET</span></span>
* <span data-ttu-id="a0ea9-117">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a0ea9-117">ASP.NET Web API</span></span>
* <span data-ttu-id="a0ea9-118">实体框架 (EF)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-118">Entity Framework (EF)</span></span>
* <span data-ttu-id="a0ea9-119">Windows Presentation Foundation (WPF)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-119">Windows Presentation Foundation (WPF)</span></span>
* <span data-ttu-id="a0ea9-120">Windows Communication Foundation (WCF)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-120">Windows Communication Foundation (WCF)</span></span>
* <span data-ttu-id="a0ea9-121">Windows Workflow Foundation (WF)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-121">Windows Workflow Foundation (WF)</span></span>
* <span data-ttu-id="a0ea9-122">Windows 窗体 (WinForms)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-122">Windows Forms (WinForms)</span></span>

## <a name="aspnet"></a><span data-ttu-id="a0ea9-123">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a0ea9-123">ASP.NET</span></span>

<span data-ttu-id="a0ea9-124">随 .NET Framework 一起提供的原始 ASP.NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-124">The original ASP.NET implementation that ships with the .NET Framework.</span></span>

<span data-ttu-id="a0ea9-125">有时 ASP.NET 是一个涵盖性术语，指包含 ASP.NET Core 在内的两个 ASP.NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-125">Sometimes ASP.NET is an umbrella term that refers to both ASP.NET implementations including ASP.NET Core.</span></span> <span data-ttu-id="a0ea9-126">该术语在任何给定实例中的含义取决于上下文。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-126">The meaning that the term carries in any given instance is determined by context.</span></span> <span data-ttu-id="a0ea9-127">若要指明不要使用 ASP.NET 来表示这两种实现，请参考 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-127">Refer to ASP.NET 4.x when you want to make it clear that you're not using ASP.NET to mean both implementations.</span></span>

<span data-ttu-id="a0ea9-128">请参阅 [ASP.NET 文档](/aspnet/#pivot=aspnet)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-128">See [ASP.NET documentation](/aspnet/#pivot=aspnet).</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="a0ea9-129">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0ea9-129">ASP.NET Core</span></span>

<span data-ttu-id="a0ea9-130">一种跨平台、高性能的开放源代码 ASP.NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-130">A cross-platform, high-performance, open-source implementation of ASP.NET.</span></span>

<span data-ttu-id="a0ea9-131">请参阅 [ASP.NET Core 文档](/aspnet/#pivot=core)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-131">See [ASP.NET Core documentation](/aspnet/#pivot=core).</span></span>

## <a name="assembly"></a><span data-ttu-id="a0ea9-132">程序集 (assembly)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-132">assembly</span></span>

<span data-ttu-id="a0ea9-133">.dll/.exe 文件，其中包含一组可由应用程序或其他程序集调用的 API。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-133">A *.dll*/*.exe* file that can contain a collection of APIs that can be called by applications or other assemblies.</span></span>

<span data-ttu-id="a0ea9-134">程序集可以包括接口、类、结构、枚举和委托等类型。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-134">An assembly may include types such as interfaces, classes, structures, enumerations, and delegates.</span></span> <span data-ttu-id="a0ea9-135">有时，项目的 bin 文件夹中的程序集被称为二进制文件。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-135">Assemblies in a project's *bin* folder are sometimes referred to as *binaries*.</span></span> <span data-ttu-id="a0ea9-136">另请参阅[库](#library)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-136">See also [library](#library).</span></span>

## <a name="bcl"></a><span data-ttu-id="a0ea9-137">BCL</span><span class="sxs-lookup"><span data-stu-id="a0ea9-137">BCL</span></span>

<span data-ttu-id="a0ea9-138">基类库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-138">Base Class Library.</span></span>

<span data-ttu-id="a0ea9-139">一组构成 System.\*（在一定的程度上构成 Microsoft.\*）命名空间的库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-139">A set of libraries that comprise the System.\* (and to a limited extent Microsoft.\*) namespaces.</span></span> <span data-ttu-id="a0ea9-140">BCL 是用于生成 ASP.NET Core 等较高级应用程序框架的较低级通用框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-140">The BCL is a general purpose, lower-level framework that higher-level application frameworks, such as ASP.NET Core, build on.</span></span>

<span data-ttu-id="a0ea9-141">[.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的 BCL 的源代码包含在 [.NET 运行时存储库](https://github.com/dotnet/runtime)中。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-141">The source code of the BCL for [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions) is contained in the [.NET runtime repository](https://github.com/dotnet/runtime).</span></span> <span data-ttu-id="a0ea9-142">这些 BCL API 中的大多数也可以在 .NET Framework 中获取，因此可将此源代码视为 .NET Framework BCL 源代码的一个分支。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-142">The majority of these BCL APIs are also available in .NET Framework, so you can think of this source code as a fork of the .NET Framework BCL source code.</span></span>

<span data-ttu-id="a0ea9-143">以下术语通常指 BCL 引用的相同 API 集合：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-143">The following terms often refer to the same collection of APIs that BCL refers to:</span></span>

- [<span data-ttu-id="a0ea9-144">核心 .NET 库</span><span class="sxs-lookup"><span data-stu-id="a0ea9-144">core .NET libraries</span></span>](../core/compatibility/corefx.md)
- [<span data-ttu-id="a0ea9-145">框架库</span><span class="sxs-lookup"><span data-stu-id="a0ea9-145">framework libraries</span></span>](#framework-libraries)
- [<span data-ttu-id="a0ea9-146">运行时库</span><span class="sxs-lookup"><span data-stu-id="a0ea9-146">runtime libraries</span></span>](#runtime)
- [<span data-ttu-id="a0ea9-147">共享框架</span><span class="sxs-lookup"><span data-stu-id="a0ea9-147">shared framework</span></span>](#shared-framework)

## <a name="clr"></a><span data-ttu-id="a0ea9-148">CLR</span><span class="sxs-lookup"><span data-stu-id="a0ea9-148">CLR</span></span>

<span data-ttu-id="a0ea9-149">公共语言运行时。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-149">Common Language Runtime.</span></span>

<span data-ttu-id="a0ea9-150">具体含义依赖于上下文。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-150">The exact meaning depends on the context.</span></span> <span data-ttu-id="a0ea9-151">公共语言运行时通常指 [.NET Framework](#net-framework) 或 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的运行时。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-151">Common Language Runtime usually refers to the runtime of [.NET Framework](#net-framework) or the runtime of [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions).</span></span>

<span data-ttu-id="a0ea9-152">CLR 处理内存分配和管理。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-152">A CLR handles memory allocation and management.</span></span> <span data-ttu-id="a0ea9-153">CLR 也是一种虚拟机，不仅可执行应用，还可使用 [JIT](#jit) 编译器快速生成和编译代码。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-153">A CLR is also a virtual machine that not only executes apps but also generates and compiles code on-the-fly using a [JIT](#jit) compiler.</span></span>

<span data-ttu-id="a0ea9-154">适用于 .NET Framework 的 CLR 实现仅限于 Windows。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-154">The CLR implementation for .NET Framework is Windows only.</span></span>

<span data-ttu-id="a0ea9-155">.NET 5 及更高版本的 CLR 实现（也称为 Core CLR）是基于与 .NET Framework CLR 相同的代码库而构建的。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-155">The CLR implementation for .NET 5 and later versions (also known as the Core CLR) is built from the same code base as the .NET Framework CLR.</span></span> <span data-ttu-id="a0ea9-156">Core CLR 最初是 Silverlight 的运行时，并且设计为在多个平台上运行，尤其是在 Windows 和 OS X 上运行。它仍是[跨平台](#cross-platform)运行时，现在包括对许多 Linux 分发的支持。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-156">Originally, the Core CLR was the runtime of Silverlight and was designed to run on multiple platforms, specifically Windows and OS X. It's still a [cross-platform](#cross-platform) runtime, now including support for many Linux distributions.</span></span>

<span data-ttu-id="a0ea9-157">另请参阅[运行时](#runtime)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-157">See also [runtime](#runtime).</span></span>

## <a name="core-clr"></a><span data-ttu-id="a0ea9-158">Core CLR</span><span class="sxs-lookup"><span data-stu-id="a0ea9-158">Core CLR</span></span>

<span data-ttu-id="a0ea9-159">[.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的公共语言运行时。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-159">The Common Language Runtime for [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions).</span></span>

<span data-ttu-id="a0ea9-160">请参阅 [CLR](#clr)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-160">See [CLR](#clr)</span></span>

## <a name="corert"></a><span data-ttu-id="a0ea9-161">CoreRT</span><span class="sxs-lookup"><span data-stu-id="a0ea9-161">CoreRT</span></span>

<span data-ttu-id="a0ea9-162">与 [CLR](#clr) 相比，CoreRT 不是虚拟机，这意味着它不包含用于快速生成并运行代码的功能，因为它不包括 [JIT](#jit)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-162">In contrast to the [CLR](#clr), CoreRT is not a virtual machine, which means it doesn't include the facilities to generate and run code on-the-fly because it doesn't include a [JIT](#jit).</span></span> <span data-ttu-id="a0ea9-163">但它包含 [GC](#gc) 以及运行时类型标识 (RTTI) 和反射功能。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-163">It does, however, include the [GC](#gc) and the ability for run-time type identification (RTTI) and reflection.</span></span> <span data-ttu-id="a0ea9-164">只是由于设计有类型系统，因此并不需要元数据反射功能。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-164">However, its type system is designed so that metadata for reflection isn't required.</span></span> <span data-ttu-id="a0ea9-165">不需要元数据使它具有 [AOT](#aot) 工具链，该工具链可去除多余的元数据，更重要的是可识别应用不使用的代码。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-165">Not requiring metadata enables having an [AOT](#aot) tool chain that can link away superfluous metadata and (more importantly) identify code that the app doesn't use.</span></span> <span data-ttu-id="a0ea9-166">CoreRT 正在开发中。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-166">CoreRT is in development.</span></span>

<span data-ttu-id="a0ea9-167">请参阅 [.NET Native 和 CoreRT 简介](https://github.com/dotnet/corert/blob/main/Documentation/intro-to-corert.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-167">See [Intro to .NET Native and CoreRT](https://github.com/dotnet/corert/blob/main/Documentation/intro-to-corert.md).</span></span>

## <a name="cross-platform"></a><span data-ttu-id="a0ea9-168">跨平台</span><span class="sxs-lookup"><span data-stu-id="a0ea9-168">cross-platform</span></span>

<span data-ttu-id="a0ea9-169">能够开发并执行可在多个不同操作系统（如 Linux、Windows 和 iOS）上使用的应用程序，而无需专门针对每个操作系统进行重新编写。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-169">The ability to develop and execute an application that can be used on multiple different operating systems, such as Linux, Windows, and iOS, without having to rewrite specifically for each one.</span></span> <span data-ttu-id="a0ea9-170">这样，可以在不同平台上的应用程序之间重复使用代码并保持一致性。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-170">This enables code reuse and consistency between applications on different platforms.</span></span>

## <a name="ecosystem"></a><span data-ttu-id="a0ea9-171">生态系统</span><span class="sxs-lookup"><span data-stu-id="a0ea9-171">ecosystem</span></span>

<span data-ttu-id="a0ea9-172">所有针对给定技术生成和运行应用程序的运行时软件、开发工具和社区资源。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-172">All of the runtime software, development tools, and community resources that are used to build and run applications for a given technology.</span></span>

<span data-ttu-id="a0ea9-173">在所包含的第三方应用和库方面，术语“.NET 生态系统”不同于类似的“.NET 堆栈”等术语。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-173">The term ".NET ecosystem" differs from similar terms such as ".NET stack" in its inclusion of third-party apps and libraries.</span></span> <span data-ttu-id="a0ea9-174">请看以下这一句示例：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-174">Here's an example in a sentence:</span></span>

- <span data-ttu-id="a0ea9-175">“推出 [.NET Standard](#net-standard) 的背后动机是要提高 .NET 生态系统中的一致性。”</span><span class="sxs-lookup"><span data-stu-id="a0ea9-175">"The motivation behind [.NET Standard](#net-standard) is to establish greater uniformity in the .NET ecosystem."</span></span>

## <a name="framework"></a><span data-ttu-id="a0ea9-176">框架</span><span class="sxs-lookup"><span data-stu-id="a0ea9-176">framework</span></span>

<span data-ttu-id="a0ea9-177">一般指一个综合 API 集合，便于开发和部署基于特定技术的应用程序。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-177">In general, a comprehensive collection of APIs that facilitates development and deployment of applications that are based on a particular technology.</span></span> <span data-ttu-id="a0ea9-178">从此常规意义上来说，ASP.NET Core 和 Windows 窗体都是示例应用程序框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-178">In this general sense, ASP.NET Core and Windows Forms are examples of application frameworks.</span></span> <span data-ttu-id="a0ea9-179">另请参阅[库](#library)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-179">See also [library](#library).</span></span>

<span data-ttu-id="a0ea9-180">“框架”一词在以下术语中有不同的含义：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-180">The word "framework" has a different meaning in the following terms:</span></span>

- [<span data-ttu-id="a0ea9-181">框架库</span><span class="sxs-lookup"><span data-stu-id="a0ea9-181">framework libraries</span></span>](#framework-libraries)
- [<span data-ttu-id="a0ea9-182">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="a0ea9-182">.NET Framework</span></span>](#net-framework)
- [<span data-ttu-id="a0ea9-183">共享框架</span><span class="sxs-lookup"><span data-stu-id="a0ea9-183">shared framework</span></span>](#shared-framework)
- [<span data-ttu-id="a0ea9-184">目标框架</span><span class="sxs-lookup"><span data-stu-id="a0ea9-184">target framework</span></span>](#target-framework)
- [<span data-ttu-id="a0ea9-185">TFM（目标框架名字对象）</span><span class="sxs-lookup"><span data-stu-id="a0ea9-185">TFM (target framework moniker)</span></span>](#tfm)
- [<span data-ttu-id="a0ea9-186">依赖于框架的应用</span><span class="sxs-lookup"><span data-stu-id="a0ea9-186">framework-dependent app</span></span>](../core/deploying/index.md#publish-framework-dependent)

<span data-ttu-id="a0ea9-187">有时，“框架”指的是 [.NET 实现](#implementation-of-net)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-187">Sometimes "framework" refers to an [implementation of .NET](#implementation-of-net).</span></span> <span data-ttu-id="a0ea9-188">例如，某文章可能会将 .NET 5 称为框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-188">For example, an article may call .NET 5 a framework.</span></span>

## <a name="framework-libraries"></a><span data-ttu-id="a0ea9-189">框架库</span><span class="sxs-lookup"><span data-stu-id="a0ea9-189">framework libraries</span></span>

<span data-ttu-id="a0ea9-190">含义取决于上下文。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-190">Meaning depends on context.</span></span> <span data-ttu-id="a0ea9-191">可指适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的框架库，在这种情况下，它指 [BCL](#bcl) 引用的相同的库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-191">May refer to the framework libraries for [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions), in which case it refers to the same libraries that [BCL](#bcl) refers to.</span></span> <span data-ttu-id="a0ea9-192">它也可指在 BCL 上构建并为 Web 应用提供其他 API 的 ASP.NET Core 框架库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-192">It may also refer to the ASP.NET Core framework libraries, which build on the BCL and provide additional APIs for web apps.</span></span>

## <a name="gc"></a><span data-ttu-id="a0ea9-193">GC</span><span class="sxs-lookup"><span data-stu-id="a0ea9-193">GC</span></span>

<span data-ttu-id="a0ea9-194">垃圾回收器。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-194">Garbage collector.</span></span>

<span data-ttu-id="a0ea9-195">垃圾回收器是自动内存管理的实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-195">The garbage collector is an implementation of automatic memory management.</span></span>  <span data-ttu-id="a0ea9-196">GC 可释放不再使用的对象占用的内存。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-196">The GC frees memory occupied by objects that are no longer in use.</span></span>

<span data-ttu-id="a0ea9-197">请参阅[垃圾回收](garbage-collection/index.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-197">See [Garbage Collection](garbage-collection/index.md).</span></span>

## <a name="il"></a><span data-ttu-id="a0ea9-198">IL</span><span class="sxs-lookup"><span data-stu-id="a0ea9-198">IL</span></span>

<span data-ttu-id="a0ea9-199">中间语言。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-199">Intermediate language.</span></span>

<span data-ttu-id="a0ea9-200">C# 等较高级的 .NET 语言编译为称为中间语言 (IL) 的硬件无关性指令集。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-200">Higher-level .NET languages, such as C#, compile down to a hardware-agnostic instruction set, which is called Intermediate Language (IL).</span></span> <span data-ttu-id="a0ea9-201">IL 有时被称为 MSIL (Microsoft IL) 或 CIL（通用 IL）。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-201">IL is sometimes referred to as MSIL (Microsoft IL) or CIL (Common IL).</span></span>

## <a name="jit"></a><span data-ttu-id="a0ea9-202">JIT</span><span class="sxs-lookup"><span data-stu-id="a0ea9-202">JIT</span></span>

<span data-ttu-id="a0ea9-203">实时编译器。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-203">Just-in-time compiler.</span></span>

<span data-ttu-id="a0ea9-204">与 [AOT](#aot) 类似，此编译器将 [IL](#il) 转换为处理器可理解的计算机代码。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-204">Similar to [AOT](#aot), this compiler translates [IL](#il) to machine code that the processor understands.</span></span> <span data-ttu-id="a0ea9-205">与 AOT 不同，JIT 编译在需要运行代码的同一台计算机上按需执行。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-205">Unlike AOT, JIT compilation happens on demand and is performed on the same machine that the code needs to run on.</span></span> <span data-ttu-id="a0ea9-206">由于 JIT 编译在应用程序的执行过程中发生，因此编译时是运行时的一部分。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-206">Since JIT compilation occurs during execution of the application, compile time is part of the run time.</span></span> <span data-ttu-id="a0ea9-207">因此，JIT 编译器需要平衡优化代码所花费的时间与生成代码时可节约的时间。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-207">Thus, JIT compilers have to balance time spent optimizing code against the savings that the resulting code can produce.</span></span> <span data-ttu-id="a0ea9-208">但 JIT 知道实际硬件，这样开发人员就无需提供不同的实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-208">But a JIT knows the actual hardware and can free developers from having to ship different implementations.</span></span>

## <a name="implementation-of-net"></a><span data-ttu-id="a0ea9-209">.NET 实现</span><span class="sxs-lookup"><span data-stu-id="a0ea9-209">implementation of .NET</span></span>

<span data-ttu-id="a0ea9-210">.NET 的实现包括：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-210">An implementation of .NET includes:</span></span>

- <span data-ttu-id="a0ea9-211">一个或多个运行时。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-211">One or more runtimes.</span></span> <span data-ttu-id="a0ea9-212">示例：[CLR](#clr)、[CoreRT](#corert)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-212">Examples: [CLR](#clr), [CoreRT](#corert).</span></span>
- <span data-ttu-id="a0ea9-213">实现 .NET Standard 的某版本并且可能包含其他 API 的类库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-213">A class library that implements a version of .NET Standard and may include additional APIs.</span></span> <span data-ttu-id="a0ea9-214">示例：[.NET Framework](#net-framework) 和 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的 [BCL](#bcl)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-214">Examples: the [BCLs](#bcl) for [.NET Framework](#net-framework) and [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions).</span></span>
- <span data-ttu-id="a0ea9-215">可选择包含一个或多个应用程序框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-215">Optionally, one or more application frameworks.</span></span> <span data-ttu-id="a0ea9-216">示例：[ASP.NET](#aspnet)、Windows 窗体及 WPF 包含在 .NET Framework 和 .NET 5 中。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-216">Examples: [ASP.NET](#aspnet), Windows Forms, and WPF are included in the .NET Framework and .NET 5.</span></span>
- <span data-ttu-id="a0ea9-217">可包含开发工具。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-217">Optionally, development tools.</span></span> <span data-ttu-id="a0ea9-218">某些开发工具在多个实现之间共享。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-218">Some development tools are shared among multiple implementations.</span></span>

<span data-ttu-id="a0ea9-219">.NET 实现的示例：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-219">Examples of .NET implementations:</span></span>

- [<span data-ttu-id="a0ea9-220">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="a0ea9-220">.NET Framework</span></span>](#net-framework)
- [<span data-ttu-id="a0ea9-221">.NET 5 及更高版本（包括 .NET Core 2.1-3.1）</span><span class="sxs-lookup"><span data-stu-id="a0ea9-221">.NET 5 and later versions (including .NET Core 2.1-3.1)</span></span>](#net-5-and-later-versions)
- [<span data-ttu-id="a0ea9-222">通用 Windows 平台 (UWP)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-222">Universal Windows Platform (UWP)</span></span>](#uwp)
- [<span data-ttu-id="a0ea9-223">Mono</span><span class="sxs-lookup"><span data-stu-id="a0ea9-223">Mono</span></span>](#mono)

## <a name="library"></a><span data-ttu-id="a0ea9-224">库</span><span class="sxs-lookup"><span data-stu-id="a0ea9-224">library</span></span>

<span data-ttu-id="a0ea9-225">可由应用或其他库调用的 API 集合。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-225">A collection of APIs that can be called by apps or other libraries.</span></span> <span data-ttu-id="a0ea9-226">.NET 库由一个或多个[程序集](#assembly)组成。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-226">A .NET library is composed of one or more [assemblies](#assembly).</span></span>

<span data-ttu-id="a0ea9-227">词库和[框架](#framework)通常作同义词使用。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-227">The words library and [framework](#framework) are often used synonymously.</span></span>

## <a name="mono"></a><span data-ttu-id="a0ea9-228">Mono</span><span class="sxs-lookup"><span data-stu-id="a0ea9-228">Mono</span></span>

<span data-ttu-id="a0ea9-229">Mono 是主要在需要小型运行时使用的开放源、[跨平台](#cross-platform) .NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-229">Mono is an open source, [cross-platform](#cross-platform) .NET implementation that is mainly used when a small runtime is required.</span></span> <span data-ttu-id="a0ea9-230">它是在 Android、Mac、iOS、tvOS 和 watchOS 上为 Xamarin 应用程序提供技术支持的运行时，主要针对内存占用少的应用程序。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-230">It is the runtime that powers Xamarin applications on Android, Mac, iOS, tvOS, and watchOS and is focused primarily on apps that require a small footprint.</span></span>

<span data-ttu-id="a0ea9-231">它支持所有当前已发布的 .NET Standard 版本。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-231">It supports all of the currently published .NET Standard versions.</span></span>

<span data-ttu-id="a0ea9-232">以前，Mono 实现更大的 .NET Framework API 并模拟一些 Unix 上最常用的功能。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-232">Historically, Mono implemented the larger API of the .NET Framework and emulated some of the most popular capabilities on Unix.</span></span> <span data-ttu-id="a0ea9-233">有时使用它运行依赖 Unix 上的这些功能的 .NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-233">It is sometimes used to run .NET applications that rely on those capabilities on Unix.</span></span>

<span data-ttu-id="a0ea9-234">Mono 通常与[实时编译器](#jit)一起使用，但它也提供在 iOS 之类的平台使用的完整[静态编译器（预先编译）](#aot)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-234">Mono is typically used with a [just-in-time compiler](#jit), but it also features a full [static compiler (ahead-of-time compilation)](#aot) that is used on platforms like iOS.</span></span>

<span data-ttu-id="a0ea9-235">请参阅 [Mono 文档](https://www.mono-project.com/docs/)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-235">See the [Mono documentation](https://www.mono-project.com/docs/).</span></span>

## <a name="net"></a><span data-ttu-id="a0ea9-236">.NET</span><span class="sxs-lookup"><span data-stu-id="a0ea9-236">.NET</span></span>

<span data-ttu-id="a0ea9-237">[.NET Standard](#net-standard) 和所有 [.NET 实现](#implementation-of-net)及工作负荷的涵盖性术语。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-237">The umbrella term for [.NET Standard](#net-standard) and all [.NET implementations](#implementation-of-net) and workloads.</span></span> <span data-ttu-id="a0ea9-238">始终采用全大写形式，请勿使用“.Net”。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-238">Always fully capitalized, never ".Net".</span></span>

<span data-ttu-id="a0ea9-239">当 [.NET5](#net-5-and-later-versions)（当前处于预览状态）发布时，它将是所有新 .NET 开发的建议 .NET 实现，因此在某些上下文中，“.NET”将表示“.NET 5 及更高版本”。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-239">When [.NET 5](#net-5-and-later-versions) (currently in preview) is released, it will be the recommended .NET implementation for all new .NET development, and so in some contexts ".NET" will imply ".NET 5 and later versions."</span></span>

<span data-ttu-id="a0ea9-240">请参阅 [.NET 基础知识](../fundamentals/index.yml)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-240">See [.NET fundamentals](../fundamentals/index.yml)</span></span>

## <a name="net-5-and-later-versions"></a><span data-ttu-id="a0ea9-241">.NET 5 及更高版本</span><span class="sxs-lookup"><span data-stu-id="a0ea9-241">.NET 5 and later versions</span></span>

<span data-ttu-id="a0ea9-242">一种跨平台、高性能的开放源 .NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-242">A cross-platform, high-performance, open-source implementation of .NET.</span></span> <span data-ttu-id="a0ea9-243">包括公共语言运行时 ([CLR](#clr))、[AOT](#aot) 运行时（正在开发中的 [CoreRT](#corert)）、基类库 ([BCL](#bcl)) 以及 [.NET SDK](#net-sdk)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-243">Includes a Common Language Runtime ([CLR](#clr)), an [AOT](#aot) runtime ([CoreRT](#corert), in development), a Base Class Library ([BCL](#bcl)), and the [.NET SDK](#net-sdk).</span></span>

<span data-ttu-id="a0ea9-244">此 .NET 实现的早期版本称为 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-244">Earlier versions of this .NET implementation are known as .NET Core.</span></span> <span data-ttu-id="a0ea9-245">.Net 5.0 是继 .NET Core 3.1 之后的下一版本。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-245">.NET 5.0 is the next version following .NET Core 3.1.</span></span> <span data-ttu-id="a0ea9-246">跳过了版本 4，以避免将此较新的 .NET 实现与称为 [.NET Framework](#net-framework) 的旧实现混淆。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-246">Version 4 was skipped to avoid confusing this newer implementation of .NET with the older implementation that is known as [.NET Framework](#net-framework).</span></span> <span data-ttu-id="a0ea9-247">.NET Framework 的当前版本为版本 4.8。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-247">The current version of .NET Framework is 4.8.</span></span>

<span data-ttu-id="a0ea9-248">请参阅 [.NET 基础知识](../fundamentals/index.yml)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-248">See [.NET fundamentals](../fundamentals/index.yml).</span></span>

## <a name="net-cli"></a><span data-ttu-id="a0ea9-249">.NET CLI</span><span class="sxs-lookup"><span data-stu-id="a0ea9-249">.NET CLI</span></span>

<span data-ttu-id="a0ea9-250">用于开发适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的应用程序和库的跨平台工具链。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-250">A cross-platform toolchain for developing applications and libraries for [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions).</span></span> <span data-ttu-id="a0ea9-251">也称为 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-251">Also known as the .NET Core CLI.</span></span>

<span data-ttu-id="a0ea9-252">请参阅 [.NET CLI](../core/tools/index.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-252">See [.NET CLI](../core/tools/index.md).</span></span>

## <a name="net-core"></a><span data-ttu-id="a0ea9-253">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0ea9-253">.NET Core</span></span>

<span data-ttu-id="a0ea9-254">请参阅 [.NET 5 及更高版本](#net-5-and-later-versions)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-254">See [.NET 5 and later versions](#net-5-and-later-versions).</span></span>

## <a name="net-framework"></a><span data-ttu-id="a0ea9-255">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="a0ea9-255">.NET Framework</span></span>

<span data-ttu-id="a0ea9-256">仅在 Windows 上运行的 .NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-256">An implementation of .NET that runs only on Windows.</span></span> <span data-ttu-id="a0ea9-257">包括公共语言运行时 ([CLR](#clr))、基类库 ([BCL](#bcl)) 以及应用程序框架库（例如 [ASP.NET](#aspnet)、Windows 窗体和 WPF）。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-257">Includes the Common Language Runtime ([CLR](#clr)), the Base Class Library ([BCL](#bcl)), and application framework libraries such as [ASP.NET](#aspnet), Windows Forms, and WPF.</span></span>

<span data-ttu-id="a0ea9-258">请查阅 [.NET Framework 指南](../framework/index.yml)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-258">See [.NET Framework Guide](../framework/index.yml).</span></span>

## <a name="net-native"></a><span data-ttu-id="a0ea9-259">.NET Native</span><span class="sxs-lookup"><span data-stu-id="a0ea9-259">.NET Native</span></span>

<span data-ttu-id="a0ea9-260">编译器工具链，可预先 ([AOT](#aot)) 生成，而非实时 ([JIT](#jit)) 生成本机代码。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-260">A compiler tool chain that produces native code ahead-of-time ([AOT](#aot)), as opposed to just-in-time ([JIT](#jit)).</span></span>

<span data-ttu-id="a0ea9-261">编译采用与 C++ 编译器和链接器类似的工作方式在开发人员计算机上进行。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-261">Compilation happens on the developer's machine similar to the way a C++ compiler and linker works.</span></span> <span data-ttu-id="a0ea9-262">它删除了未使用的代码，留出更多时间进行优化。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-262">It removes unused code and spends more time optimizing it.</span></span> <span data-ttu-id="a0ea9-263">它从库中提取代码，将它们合并到可执行文件中。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-263">It extracts code from libraries and merges them into the executable.</span></span> <span data-ttu-id="a0ea9-264">结果是表示整个应用的单个模块。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-264">The result is a single module that represents the entire app.</span></span>

<span data-ttu-id="a0ea9-265">UWP 是 .NET Native 支持的首个应用程序框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-265">UWP was the first application framework supported by .NET Native.</span></span> <span data-ttu-id="a0ea9-266">现在，我们支持为 Windows、macOS 和 Linux 生成本机控制台应用。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-266">Now, we support building native console apps for Windows, macOS, and Linux.</span></span>

<span data-ttu-id="a0ea9-267">请参阅 [.NET Native 和 CoreRT 简介](https://github.com/dotnet/corert/blob/main/Documentation/intro-to-corert.md)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-267">See [Intro to .NET Native and CoreRT](https://github.com/dotnet/corert/blob/main/Documentation/intro-to-corert.md)</span></span>

## <a name="net-sdk"></a><span data-ttu-id="a0ea9-268">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="a0ea9-268">.NET SDK</span></span>

<span data-ttu-id="a0ea9-269">一组库和工具，开发人员可用其创建适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的 .NET Core 应用程序和库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-269">A set of libraries and tools that allow developers to create .NET applications and libraries for [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions).</span></span> <span data-ttu-id="a0ea9-270">也称为 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-270">Also known as the .NET Core SDK.</span></span>

<span data-ttu-id="a0ea9-271">包括用于生成应用的 [.NET CLI](#net-cli)、用于生成和运行应用的 .NET 库以及用于运行 CLI 命令和运行应用程序的 dotnet 可执行文件 (dotnet.exe)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-271">Includes the [.NET CLI](#net-cli) for building apps, .NET libraries and runtime for building and running apps, and the dotnet executable (*dotnet.exe*) that runs CLI commands and runs applications.</span></span>

<span data-ttu-id="a0ea9-272">请参阅 [.NET SDK 概述](../core/sdk.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-272">See [.NET SDK Overview](../core/sdk.md).</span></span>

## <a name="net-standard"></a><span data-ttu-id="a0ea9-273">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="a0ea9-273">.NET Standard</span></span>

<span data-ttu-id="a0ea9-274">在每个 .NET 实现中都可用的 .NET API 正式规范。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-274">A formal specification of .NET APIs that are available in each .NET implementation.</span></span>

<span data-ttu-id="a0ea9-275">.NET Standard 规范有时被称为文档中的库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-275">The .NET Standard specification is sometimes called a library in the documentation.</span></span> <span data-ttu-id="a0ea9-276">由于库不仅包括规范（接口），还包括 API 实现，所以会误将 .NET Standard 称为“库”。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-276">Because a library includes API implementations, not only specifications (interfaces), it's misleading to call .NET Standard a "library."</span></span>

<span data-ttu-id="a0ea9-277">请参阅 [.NET Standard](net-standard.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-277">See [.NET Standard](net-standard.md).</span></span>

## <a name="ngen"></a><span data-ttu-id="a0ea9-278">NGEN</span><span class="sxs-lookup"><span data-stu-id="a0ea9-278">NGEN</span></span>

<span data-ttu-id="a0ea9-279">本机（映像）生成。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-279">Native (image) generation.</span></span>

<span data-ttu-id="a0ea9-280">可将此方法视为永久性 [JIT](#jit) 编译器。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-280">You can think of this technology as a persistent [JIT](#jit) compiler.</span></span> <span data-ttu-id="a0ea9-281">它通常在执行代码的计算机上编译该代码，但通常在安装时进行编译。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-281">It usually compiles code on the machine where the code is executed, but compilation typically occurs at install time.</span></span>

## <a name="package"></a><span data-ttu-id="a0ea9-282">包</span><span class="sxs-lookup"><span data-stu-id="a0ea9-282">package</span></span>

<span data-ttu-id="a0ea9-283">NuGet 包 &mdash; 或只是一个包 &mdash; 是一个 .zip 文件，其中具有一个或多个名称相同的程序集以及作者姓名等其他元数据。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-283">A NuGet package &mdash; or just a package &mdash; is a *.zip* file with one or more assemblies of the same name along with additional metadata such as the author name.</span></span>

<span data-ttu-id="a0ea9-284">.zip 文件的扩展名为 .nupkg，且可以包含在多个目标框架和版本中使用的资产（如 .dll 文件和 .xml 文件）。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-284">The *.zip* file has a *.nupkg* extension and may contain assets, such as *.dll* files and *.xml* files, for use with multiple target frameworks and versions.</span></span> <span data-ttu-id="a0ea9-285">在应用或库中安装时，会根据应用或库指定的目标框架选择相应的资产。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-285">When installed in an app or library, the appropriate assets are selected based on the target framework specified by the app or library.</span></span> <span data-ttu-id="a0ea9-286">定义接口的资产位于 ref 文件夹，而定义实现的资产位于 lib文件夹。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-286">The assets that define the interface are in the *ref* folder, and the assets that define the implementation are in the *lib* folder.</span></span>

## <a name="platform"></a><span data-ttu-id="a0ea9-287">平台</span><span class="sxs-lookup"><span data-stu-id="a0ea9-287">platform</span></span>

<span data-ttu-id="a0ea9-288">操作系统以及运行它的硬件，例如 Windows、macOS、Linux、iOS 和 Android。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-288">An operating system and the hardware it runs on, such as Windows, macOS, Linux, iOS, and Android.</span></span>

<span data-ttu-id="a0ea9-289">下面是在句子中使用的示例：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-289">Here are examples of usage in sentences:</span></span>

- <span data-ttu-id="a0ea9-290">“.NET Core 是一个跨平台 .NET 实现。”</span><span class="sxs-lookup"><span data-stu-id="a0ea9-290">".NET Core is a cross-platform implementation of .NET."</span></span>
- <span data-ttu-id="a0ea9-291">“PCL 配置文件代表 Microsoft 平台，而 .NET Standard 与平台无关。”</span><span class="sxs-lookup"><span data-stu-id="a0ea9-291">"PCL profiles represent Microsoft platforms, while .NET Standard is agnostic to platform."</span></span>

<span data-ttu-id="a0ea9-292">旧的 .NET 文档有时使用“.NET 平台”来表示一个 [ .NET 的实现](#implementation-of-net)或包括所有实现的 .NET [堆栈](#stack)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-292">Legacy .NET documentation sometimes uses ".NET platform" to mean either an [implementation of .NET](#implementation-of-net) or the .NET [stack](#stack) including all implementations.</span></span> <span data-ttu-id="a0ea9-293">这两种用法往往会与主（OS/硬件）含义混淆，因此我们要尽量避免这些用法。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-293">Both of these usages tend to get confused with the primary (OS/hardware) meaning, so we try to avoid these usages.</span></span>

<span data-ttu-id="a0ea9-294">“平台”在短语“开发人员平台”中有不同的含义，这里指的是提供用于生成和运行应用的工具和库的软件。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-294">"Platform" has a different meaning in the phrase "developer platform," which refers to software that provides tools and libraries for building and running apps.</span></span> <span data-ttu-id="a0ea9-295">.NET 是跨平台的开放源代码开发人员平台，用于构建多种不同类型的应用程序。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-295">.NET is a cross-platform, open source developer platform for building many different types of applications.</span></span>

## <a name="runtime"></a><span data-ttu-id="a0ea9-296">Runtime — 运行时</span><span class="sxs-lookup"><span data-stu-id="a0ea9-296">runtime</span></span>

<span data-ttu-id="a0ea9-297">通常是指用于托管程序的执行环境。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-297">In general, the execution environment for a managed program.</span></span> <span data-ttu-id="a0ea9-298">操作系统属于运行时环境，但不属于 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-298">The OS is part of the runtime environment but is not part of the .NET runtime.</span></span> <span data-ttu-id="a0ea9-299">下面是此情况下 .NET 运行时的一些示例：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-299">Here are some examples of .NET runtimes in this sense of the word:</span></span>

- <span data-ttu-id="a0ea9-300">公共语言运行时 ([CLR](#clr))</span><span class="sxs-lookup"><span data-stu-id="a0ea9-300">Common Language Runtime ([CLR](#clr))</span></span>
- <span data-ttu-id="a0ea9-301">.NET Native（适用于 UWP）</span><span class="sxs-lookup"><span data-stu-id="a0ea9-301">.NET Native (for UWP)</span></span>
- <span data-ttu-id="a0ea9-302">Mono 运行时</span><span class="sxs-lookup"><span data-stu-id="a0ea9-302">Mono runtime</span></span>

<span data-ttu-id="a0ea9-303">“运行时”一词在某些上下文中有不同的含义：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-303">The word "runtime" has a different meaning in some contexts:</span></span>

* <span data-ttu-id="a0ea9-304">[.NET 5.0 下载页](https://dotnet.microsoft.com/download/dotnet/5.0)上的 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-304">*.NET runtime* on the [.NET 5.0 download page](https://dotnet.microsoft.com/download/dotnet/5.0).</span></span>

  <span data-ttu-id="a0ea9-305">可下载 .NET 运行时或其他运行时，如 ASP.NET Core 运行时 。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-305">You can download the *.NET runtime* or other runtimes, such as the *ASP.NET Core runtime*.</span></span> <span data-ttu-id="a0ea9-306">在此用法中运行时是一组必须安装在计算机上的组件，这样才能在计算机上运行[依赖于框架](../core/deploying/index.md#publish-framework-dependent)的应用。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-306">A *runtime* in this usage is the set of components that must be installed on a machine to run a [framework-dependent](../core/deploying/index.md#publish-framework-dependent) app on the machine.</span></span> <span data-ttu-id="a0ea9-307">.NET 运行时包括 [CLR](#clr) 和 .NET [共享框架](#shared-framework)，后者提供 [BCL](#bcl)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-307">The .NET runtime includes the [CLR](#clr) and the .NET [shared framework](#shared-framework), which provides the [BCL](#bcl).</span></span>

* <span data-ttu-id="a0ea9-308">.NET 运行时库</span><span class="sxs-lookup"><span data-stu-id="a0ea9-308">*.NET runtime libraries*</span></span>

  <span data-ttu-id="a0ea9-309">指 [BCL](#bcl) 引用的相同的库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-309">Refers to the same libraries that [BCL](#bcl) refers to.</span></span> <span data-ttu-id="a0ea9-310">但是，其他运行时（例如 ASP.NET Core 运行时）具有不同的[共享框架](#shared-framework)，并具有在 BCL 上构建的其他库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-310">However, other runtimes, such as the ASP.NET Core runtime, have different [shared frameworks](#shared-framework), with additional libraries that build on the BCL.</span></span>

* <span data-ttu-id="a0ea9-311">[运行时标识符 (RID)](../core/rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-311">[Runtime Identifier (RID)](../core/rid-catalog.md).</span></span>

  <span data-ttu-id="a0ea9-312">此处的“运行时”表示运行 .NET 应用的 OS 平台和 CPU 体系结构，例如：`linux-x64`。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-312">*Runtime* here means the OS platform and CPU architecture that a .NET app runs on, for example: `linux-x64`.</span></span>

* <span data-ttu-id="a0ea9-313">有时在 [.NET 实现](#implementation-of-net)的意义上使用“运行时”，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="a0ea9-313">Sometimes "runtime" is used in the sense of an [implementation of .NET](#implementation-of-net), as in the following examples:</span></span>

  - <span data-ttu-id="a0ea9-314">“各种 .NET 运行时实现特定版本的 .NET Standard。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-314">"The various .NET runtimes implement specific versions of .NET Standard.</span></span> <span data-ttu-id="a0ea9-315">…</span><span class="sxs-lookup"><span data-stu-id="a0ea9-315">…</span></span> <span data-ttu-id="a0ea9-316">每个 .NET 运行时版本都将会公布它所支持的最高 .NET Standard 版本...”</span><span class="sxs-lookup"><span data-stu-id="a0ea9-316">Each .NET runtime version advertises the highest .NET Standard version it supports …"</span></span>
  - <span data-ttu-id="a0ea9-317">“计划在多个运行时上运行的库应将此框架作为目标。”</span><span class="sxs-lookup"><span data-stu-id="a0ea9-317">"Libraries that are intended to run on multiple runtimes should target this framework."</span></span> <span data-ttu-id="a0ea9-318">（参阅 .NET Standard）</span><span class="sxs-lookup"><span data-stu-id="a0ea9-318">(referring to .NET Standard)</span></span>

## <a name="shared-framework"></a><span data-ttu-id="a0ea9-319">共享框架</span><span class="sxs-lookup"><span data-stu-id="a0ea9-319">shared framework</span></span>

<span data-ttu-id="a0ea9-320">含义取决于上下文。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-320">Meaning depends on context.</span></span> <span data-ttu-id="a0ea9-321">.NET 共享框架指 [.NET 运行时](#runtime)中包含的库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-321">The *.NET shared framework* refers to the libraries included in the [.NET runtime](#runtime).</span></span> <span data-ttu-id="a0ea9-322">在这种情况下，适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的共享框架指 [BCL](#bcl) 引用的相同的库。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-322">In this case, the *shared framework* for [.NET 5 (and .NET Core) and later versions](#net-5-and-later-versions) refers to the same libraries that [BCL](#bcl) refers to.</span></span>

<span data-ttu-id="a0ea9-323">也存在其他共享框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-323">There are other shared frameworks.</span></span> <span data-ttu-id="a0ea9-324">ASP.NET Core 共享框架指 [ASP.NET Core 运行时](#runtime)中包含的库，其中包括 BCL 以及供 Web 应用使用的其他 API。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-324">The *ASP.NET Core shared framework* refers to the libraries included in the [ASP.NET Core runtime](#runtime), which includes the BCL plus additional APIs for use by web apps.</span></span>

<span data-ttu-id="a0ea9-325">对于[依赖于框架的应用](../core/deploying/index.md#publish-framework-dependent)，共享框架由库组成，这些库包含在运行应用的计算机上的文件夹中的程序集内。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-325">For [framework-dependent apps](../core/deploying/index.md#publish-framework-dependent), the shared framework consists of libraries that are contained in assemblies installed in a folder on the machine that runs the app.</span></span> <span data-ttu-id="a0ea9-326">对于[自包含应用](../core/deploying/index.md#publish-self-contained)，共享框架程序集包含在该应用中。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-326">For [self-contained apps](../core/deploying/index.md#publish-self-contained), the shared framework assemblies are included with the app.</span></span>

<span data-ttu-id="a0ea9-327">有关详细信息，请参阅[深入了解 .NET Core 基元，第 2 部分：共享框架](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-327">For more information, see [Deep-dive into .NET Core primitives, part 2: the shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

## <a name="stack"></a><span data-ttu-id="a0ea9-328">堆栈</span><span class="sxs-lookup"><span data-stu-id="a0ea9-328">stack</span></span>

<span data-ttu-id="a0ea9-329">一组编程方法，一起用于生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-329">A set of programming technologies that are used together to build and run applications.</span></span>

<span data-ttu-id="a0ea9-330">“.NET 堆栈”指 .NET Standard 和所有 .NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-330">"The .NET stack" refers to .NET Standard and all .NET implementations.</span></span> <span data-ttu-id="a0ea9-331">短语“一个 .NET 堆栈”可能指一种 .NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-331">The phrase "a .NET stack" may refer to one implementation of .NET.</span></span>

## <a name="target-framework"></a><span data-ttu-id="a0ea9-332">Target Framework — 目标 Framework</span><span class="sxs-lookup"><span data-stu-id="a0ea9-332">target framework</span></span>

<span data-ttu-id="a0ea9-333">.NET 应用或库依赖的 API 集合。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-333">The collection of APIs that a .NET app or library relies on.</span></span>

<span data-ttu-id="a0ea9-334">应用或库可将某版本的 .NET Standard（例如 .NET Standard 2.0）作为目标，这是所有 .NET 实现中一组标准化 API 的规范。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-334">An app or library can target a version of .NET Standard (for example, .NET Standard 2.0), which is a specification for a standardized set of APIs across all .NET implementations.</span></span> <span data-ttu-id="a0ea9-335">应用或库还能以特定 .NET 的某版本实现为目标，这样便可获得特定于实现的 API 的访问权限。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-335">An app or library can also target a version of a specific .NET implementation, in which case it gets access to implementation-specific APIs.</span></span> <span data-ttu-id="a0ea9-336">例如，面向 Xamarin.iOS 的应用有权访问 Xamarin 提供的 iOS API 包装器。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-336">For example, an app that targets Xamarin.iOS gets access to Xamarin-provided iOS API wrappers.</span></span>

<span data-ttu-id="a0ea9-337">对于某些目标框架（例如 .NET Framework），可用 API 由 .NET 实现在系统上安装的程序集定义，其中可能包括应用程序框架 API（例如 ASP.NET、WinForms）。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-337">For some target frameworks (for example, the .NET Framework) the available APIs are defined by the assemblies that a .NET implementation installs on a system, which may include application framework APIs (for example, ASP.NET, WinForms).</span></span> <span data-ttu-id="a0ea9-338">对于基于包的目标框架（例如 .NET Standard 和 .NET Core），框架 API 由安装在应用或库中的包定义。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-338">For package-based target frameworks (such as .NET Standard and .NET Core), the framework APIs are defined by the packages installed in the app or library.</span></span> <span data-ttu-id="a0ea9-339">在这种情况下，目标框架隐式指定一个包，该包引用一起构成框架的所有包。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-339">In that case, the target framework implicitly specifies a package that references all the packages that together make up the framework.</span></span>

<span data-ttu-id="a0ea9-340">请参阅[目标框架](frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-340">See [Target Frameworks](frameworks.md).</span></span>

## <a name="tfm"></a><span data-ttu-id="a0ea9-341">TFM</span><span class="sxs-lookup"><span data-stu-id="a0ea9-341">TFM</span></span>

<span data-ttu-id="a0ea9-342">目标框架名字对象。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-342">Target framework moniker.</span></span>

<span data-ttu-id="a0ea9-343">一个标准化令牌格式，用于指定 .NET 应用或库的目标框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-343">A standardized token format for specifying the target framework of a .NET app or library.</span></span> <span data-ttu-id="a0ea9-344">目标框架通常由短名称（如 `net462`）引用。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-344">Target frameworks are typically referenced by a short name, such as `net462`.</span></span> <span data-ttu-id="a0ea9-345">存在长格式的 TFM（如 .NETFramework,Version=4.6.2），但通常不用来指定目标框架。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-345">Long-form TFMs (such as .NETFramework,Version=4.6.2) exist but are not generally used to specify a target framework.</span></span>

<span data-ttu-id="a0ea9-346">请参阅[目标框架](frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-346">See [Target Frameworks](frameworks.md).</span></span>

## <a name="uwp"></a><span data-ttu-id="a0ea9-347">UWP</span><span class="sxs-lookup"><span data-stu-id="a0ea9-347">UWP</span></span>

<span data-ttu-id="a0ea9-348">通用 Windows 平台。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-348">Universal Windows Platform.</span></span>

<span data-ttu-id="a0ea9-349">用于为物联网 (IoT) 生成新式触控 Windows 应用程序和软件的 .NET 实现。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-349">An implementation of .NET that is used for building modern, touch-enabled Windows applications and software for the Internet of Things (IoT).</span></span> <span data-ttu-id="a0ea9-350">它旨在统一可能想要以其为目标的不同类型的设备，包括电脑、平板电脑、电话，甚至 Xbox。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-350">It's designed to unify the different types of devices that you may want to target, including PCs, tablets, phones, and even the Xbox.</span></span> <span data-ttu-id="a0ea9-351">UWP 提供许多服务，如集中式应用商店、执行环境 (AppContainer) 和一组 Windows API（用于代替 Win32 (WinRT)）。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-351">UWP provides many services, such as a centralized app store, an execution environment (AppContainer), and a set of Windows APIs to use instead of Win32 (WinRT).</span></span> <span data-ttu-id="a0ea9-352">应用可采用 C++、C#、Visual Basic 和 JavaScript 编写。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-352">Apps can be written in C++, C#, Visual Basic, and JavaScript.</span></span> <span data-ttu-id="a0ea9-353">使用 C# 和 Visual Basic 时，.NET API 由 .NET 5（和 .NET Core）及更高版本提供。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-353">When using C# and Visual Basic, the .NET APIs are provided by .NET 5 (and .NET Core) and later versions.</span></span>

## <a name="workload"></a><span data-ttu-id="a0ea9-354">workload</span><span class="sxs-lookup"><span data-stu-id="a0ea9-354">workload</span></span>

<span data-ttu-id="a0ea9-355">某人正在构建的一种类型的应用。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-355">A type of app someone is building.</span></span> <span data-ttu-id="a0ea9-356">比[应用模型](#app-model)更通用。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-356">More generic than [app model](#app-model).</span></span> <span data-ttu-id="a0ea9-357">例如，在每个 .NET 文档页（包括此页）的顶部都有一个“工作负载”下拉列表，你可以通过该列表切换到针对“Web”、“移动”、“云”、“桌面”和“机器学习 \& 数据”的文档     。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-357">For example, at the top of every .NET documentation page, including this one, is a drop-down list for **Workloads**, which lets you switch to documentation for **Web**, **Mobile**, **Cloud**, **Desktop**, and **Machine Learning \& Data**.</span></span>

<span data-ttu-id="a0ea9-358">在某些上下文中，工作负载是指 Visual Studio 功能的集合，可以选择安装这些功能以支持特定类型的应用。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-358">In some contexts, *workload* refers to a collection of Visual Studio features that you can choose to install to support a particular type of app.</span></span> <span data-ttu-id="a0ea9-359">有关示例，请参阅[选择工作负载](../core/install/windows.md#select-a-workload)。</span><span class="sxs-lookup"><span data-stu-id="a0ea9-359">For an example, see [Select a workload](../core/install/windows.md#select-a-workload).</span></span>

## <a name="see-also"></a><span data-ttu-id="a0ea9-360">请参阅</span><span class="sxs-lookup"><span data-stu-id="a0ea9-360">See also</span></span>

- [<span data-ttu-id="a0ea9-361">.NET 基础知识</span><span class="sxs-lookup"><span data-stu-id="a0ea9-361">.NET fundamentals</span></span>](../fundamentals/index.yml)
- [<span data-ttu-id="a0ea9-362">.NET Framework 指南</span><span class="sxs-lookup"><span data-stu-id="a0ea9-362">.NET Framework Guide</span></span>](../framework/index.yml)
- [<span data-ttu-id="a0ea9-363">ASP.NET 概述</span><span class="sxs-lookup"><span data-stu-id="a0ea9-363">ASP.NET Overview</span></span>](/aspnet/index#pivot=aspnet)
- [<span data-ttu-id="a0ea9-364"> 概述</span><span class="sxs-lookup"><span data-stu-id="a0ea9-364">ASP.NET Core Overview</span></span>](/aspnet/index#pivot=core)
