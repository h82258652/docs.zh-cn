---
title: 运行时分析
description: 探索 .NET 中的运行时分析，这是在任何开发或部署方案中收集性能数据的方法。
ms.date: 03/30/2017
helpviewer_keywords:
- performance counters
- common language runtime, profiling
- Perfmon.exe
- System Monitor
- profiling
- runtime, profiling
- profiling applications
- Performance Console
ms.assetid: ccd68284-f3a8-47b8-bc3f-92e5fe3a1640
ms.openlocfilehash: a1ee34ef5e99f95dc7037c0ed5aebc074aa9db7a
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494867"
---
# <a name="runtime-profiling"></a><span data-ttu-id="76079-103">运行时分析</span><span class="sxs-lookup"><span data-stu-id="76079-103">Runtime Profiling</span></span>

<span data-ttu-id="76079-104">分析是用于在任何开发或部署方案中收集性能数据的方法。</span><span class="sxs-lookup"><span data-stu-id="76079-104">Profiling is a method of gathering performance data in any development or deployment scenario.</span></span> <span data-ttu-id="76079-105">本节面向想要收集有关应用程序性能的信息的开发人员和系统管理员。</span><span class="sxs-lookup"><span data-stu-id="76079-105">This section is for developers and system administrators who want to gather information about application performance.</span></span>  
  
## <a name="tracking-performance-using-the-performance-monitor-perfmonexe"></a><span data-ttu-id="76079-106">使用性能监视器 (Perfmon.exe) 跟踪性能</span><span class="sxs-lookup"><span data-stu-id="76079-106">Tracking Performance Using the Performance Monitor (Perfmon.exe)</span></span>  

 <span data-ttu-id="76079-107">性能监视器是用于分析 .NET Framework 应用程序的最简单的工具。</span><span class="sxs-lookup"><span data-stu-id="76079-107">The Performance Monitor is the easiest tool to use to profile your .NET Framework application.</span></span> <span data-ttu-id="76079-108">性能监视器以图形方式表示在与公共语言运行时一起安装的 .NET Framework 性能计数器和 Windows SDK 中找到的数据。</span><span class="sxs-lookup"><span data-stu-id="76079-108">The Performance Monitor graphically represents data found in the .NET Framework performance counters that are installed with the common language runtime and the Windows SDK.</span></span> <span data-ttu-id="76079-109">这些计数器可用于监视从内存管理到实时 (JIT) 编译器性能的方方面面。</span><span class="sxs-lookup"><span data-stu-id="76079-109">These counters can be used to monitor everything from memory management to just-in-time (JIT) compiler performance.</span></span> <span data-ttu-id="76079-110">它们告诉你应用程序所使用的资源的情况，这是了解应用程序性能的间接方法。</span><span class="sxs-lookup"><span data-stu-id="76079-110">They tell you about the resources your application uses, which is an indirect measure of your application's performance.</span></span> <span data-ttu-id="76079-111">使用这些计数器可了解应用程序在内部的工作方式。</span><span class="sxs-lookup"><span data-stu-id="76079-111">Use these counters to understand how your application works internally.</span></span>  
  
#### <a name="to-run-perfmonexe-on-windows-vista-and-later-versions"></a><span data-ttu-id="76079-112">在 Windows Vista 和更高版本上运行 Perfmon.exe</span><span class="sxs-lookup"><span data-stu-id="76079-112">To run Perfmon.exe on Windows Vista and later versions</span></span>  
  
1. <span data-ttu-id="76079-113">在命令提示符处，键入 **perfmon**。</span><span class="sxs-lookup"><span data-stu-id="76079-113">At the command prompt, type **perfmon**.</span></span> <span data-ttu-id="76079-114">将出现“性能监视器”  控制台。</span><span class="sxs-lookup"><span data-stu-id="76079-114">The **Performance Monitor** console appears.</span></span>  
  
2. <span data-ttu-id="76079-115">在“监视工具”  文件夹中，单击“性能监视器” 。</span><span class="sxs-lookup"><span data-stu-id="76079-115">In the **Monitoring Tools** folder, click **Performance Monitor**.</span></span>  
  
3. <span data-ttu-id="76079-116">在“性能监视器”工具栏上，如果有“添加”  图标（加号），请单击该图标。</span><span class="sxs-lookup"><span data-stu-id="76079-116">In the Performance Monitor toolbar, click the **Add** icon (the plus sign), if it is present.</span></span> <span data-ttu-id="76079-117">如果没有，请在监视器窗口中单击右键，然后选择“添加计数器”  选项。</span><span class="sxs-lookup"><span data-stu-id="76079-117">If it is not present, right-click in the monitor window and select the **Add Counters** option.</span></span>  
  
     <span data-ttu-id="76079-118">这将打开“添加计数器”  对话框。</span><span class="sxs-lookup"><span data-stu-id="76079-118">This opens the **Add Counters** dialog box.</span></span> <span data-ttu-id="76079-119">“可用计数器”  列表框中显示可用的性能对象。</span><span class="sxs-lookup"><span data-stu-id="76079-119">The **Available counters** list box displays the available performance objects.</span></span> <span data-ttu-id="76079-120">.NET Framework 应用程序有许多预定义的对象，包括用于内存管理、互操作性、异常处理以及多线程处理的计数器，它们分别是“.NET CLR Memory”**.**、“.NET CLR Interop”、“.NET CLR Exceptions”和“.NET CLR LocksAndThreads”。</span><span class="sxs-lookup"><span data-stu-id="76079-120">There are a number of predefined objects for .NET Framework applications, including those for memory management (**.NET CLR Memory**), interoperability (**.NET CLR Interop**), exception handling (**.NET CLR Exceptions**), and multithreading (**.NET CLR LocksAndThreads**).</span></span> <span data-ttu-id="76079-121">每个性能对象均包含多个单一性能计数器。</span><span class="sxs-lookup"><span data-stu-id="76079-121">Each performance object includes a number of individual performance counters.</span></span> <span data-ttu-id="76079-122">在性能监视器中的可用性能计数器列表，请参阅 [Performance Counters](performance-counters.md)一起安装的.NET Framework 性能计数器中的数据。</span><span class="sxs-lookup"><span data-stu-id="76079-122">For a list of the performance counters available in Performance Monitor, see [Performance Counters](performance-counters.md).</span></span>  
  
4. <span data-ttu-id="76079-123">选择性能对象的名称旁边的复选框，以查看它所支持的各个性能计数器的列表。</span><span class="sxs-lookup"><span data-stu-id="76079-123">Select the check box next to a performance object's name to view the list of individual performance counters that it supports.</span></span>  
  
5. <span data-ttu-id="76079-124">单击要查看的性能计数器。</span><span class="sxs-lookup"><span data-stu-id="76079-124">Click the performance counter you want to view.</span></span>  
  
6. <span data-ttu-id="76079-125">在 " **所选对象的实例** " 列表框中，单击 **\<All instances>** 以指定您想要在整个系统范围) 上监视公共语言运行时的性能计数器，全局 (。</span><span class="sxs-lookup"><span data-stu-id="76079-125">In the **Instances of selected object** list box, click **\<All instances>** to specify that you want to monitor the performance counter for the common language runtime globally (that is, on a system-wide basis).</span></span>  
  
     <span data-ttu-id="76079-126">-或-</span><span class="sxs-lookup"><span data-stu-id="76079-126">-or-</span></span>  
  
     <span data-ttu-id="76079-127">在“选定对象的实例”  列表框中，单击要监视该应用程序的性能计数器的应用程序的名称。</span><span class="sxs-lookup"><span data-stu-id="76079-127">In the **Instances of selected object** list box, click an application name to monitor the performance counter for that application.</span></span>  
  
     <span data-ttu-id="76079-128">若要区分运行时的多个版本，或消除具有相同名称的多个应用程序的歧义，还必须修改注册表项。</span><span class="sxs-lookup"><span data-stu-id="76079-128">To differentiate multiple versions of the runtime, or to disambiguate multiple applications with the same name, you must also modify a registry key.</span></span> <span data-ttu-id="76079-129">有关详细信息，请参阅 [Performance Counters and In-Process Side-By-Side Applications](performance-counters-and-in-process-side-by-side-applications.md)。</span><span class="sxs-lookup"><span data-stu-id="76079-129">For more information, see [Performance Counters and In-Process Side-By-Side Applications](performance-counters-and-in-process-side-by-side-applications.md).</span></span>  
  
> [!NOTE]
> <span data-ttu-id="76079-130">如果在性能控制台正在运行时安装新的性能计数器，请在停止后再重启性能控制台，以便显示新的计数器。</span><span class="sxs-lookup"><span data-stu-id="76079-130">When new performance counters are installed while the Performance console is running, stop and restart the Performance console to make the new counters visible.</span></span>  
  
 <span data-ttu-id="76079-131">要分析位于某一区域或远程共享中的程序集，请确保该远程程序集在运行性能计数器的计算机上完全受信任。</span><span class="sxs-lookup"><span data-stu-id="76079-131">If you want to profile an assembly that exists in a zone or on a remote share, make sure that the remote assembly has full trust on the computer that runs the performance counters.</span></span> <span data-ttu-id="76079-132">如果该程序集不具有足够的信任，则性能计数器将不工作。</span><span class="sxs-lookup"><span data-stu-id="76079-132">If the assembly does not have sufficient trust, the performance counters will not work.</span></span> <span data-ttu-id="76079-133">有关向不同区域授予信任的信息，请参阅 [Caspol.exe（代码访问安全策略工具）](../tools/caspol-exe-code-access-security-policy-tool.md)。</span><span class="sxs-lookup"><span data-stu-id="76079-133">For information about granting trust to different zones, see [Caspol.exe (Code Access Security Policy Tool)](../tools/caspol-exe-code-access-security-policy-tool.md).</span></span>  
  
> [!NOTE]
> <span data-ttu-id="76079-134">在安装了 .NET Framework 4 的系统上，对于使用 .NET Framework 1.1 开发的应用程序，性能监视器可能不会显示某些类别（如 **.NET Clr 数据** 和 **.net clr 网络**）中性能计数器的数据。</span><span class="sxs-lookup"><span data-stu-id="76079-134">On systems on which the .NET Framework 4 is installed, the Performance Monitor may not display data for performance counters in some categories, such as **.NET CLR Data** and **.NET CLR Networking**, for applications that were developed using the .NET Framework 1.1.</span></span> <span data-ttu-id="76079-135">如果是这种情况，则可以通过将 [\<forcePerformanceCounterUniqueSharedMemoryReads>](../configure-apps/file-schema/runtime/forceperformancecounteruniquesharedmemoryreads-element.md) 元素添加到应用程序的配置文件中，将性能监视器配置为显示此数据。</span><span class="sxs-lookup"><span data-stu-id="76079-135">If this is the case, you can configure Performance Monitor to display this data by adding the [\<forcePerformanceCounterUniqueSharedMemoryReads>](../configure-apps/file-schema/runtime/forceperformancecounteruniquesharedmemoryreads-element.md) element to the application's configuration file.</span></span>  
  
## <a name="reading-and-creating-performance-counters-programmatically"></a><span data-ttu-id="76079-136">以编程方式读取和创建性能计数器</span><span class="sxs-lookup"><span data-stu-id="76079-136">Reading and Creating Performance Counters Programmatically</span></span>  

 <span data-ttu-id="76079-137">.NET Framework 提供了一些类，可用于以编程方式访问性能控制台中提供的相同性能信息。</span><span class="sxs-lookup"><span data-stu-id="76079-137">The .NET Framework provides classes you can use to programmatically access the same performance information that is available in the Performance console.</span></span> <span data-ttu-id="76079-138">另外，还可以使用这些类创建自定义性能计数器。</span><span class="sxs-lookup"><span data-stu-id="76079-138">You can also use these classes to create custom performance counters.</span></span> <span data-ttu-id="76079-139">下表描述了 .NET Framework 中提供的某些性能监视类。</span><span class="sxs-lookup"><span data-stu-id="76079-139">The following table describes some of the performance monitoring classes that are provided in the .NET Framework.</span></span>  
  
|<span data-ttu-id="76079-140">类</span><span class="sxs-lookup"><span data-stu-id="76079-140">Class</span></span>|<span data-ttu-id="76079-141">描述</span><span class="sxs-lookup"><span data-stu-id="76079-141">Description</span></span>|  
|-----------|-----------------|  
|<xref:System.Diagnostics.PerformanceCounter?displayProperty=nameWithType>|<span data-ttu-id="76079-142">表示 Windows 性能计数器组件。</span><span class="sxs-lookup"><span data-stu-id="76079-142">Represents a Windows performance counter component.</span></span> <span data-ttu-id="76079-143">使用此类可读取现有预定义或自定义计数器，并向自定义计数器发布（写入）性能数据。</span><span class="sxs-lookup"><span data-stu-id="76079-143">Use this class to read existing predefined or custom counters and publish (write) performance data to custom counters.</span></span>|  
|<xref:System.Diagnostics.PerformanceCounterCategory?displayProperty=nameWithType>|<span data-ttu-id="76079-144">提供与计数器交互的几种方法以及计算机上计数器的类别。</span><span class="sxs-lookup"><span data-stu-id="76079-144">Provides several methods for interacting with counters and categories of counters on the computer.</span></span>|  
|<xref:System.Diagnostics.PerformanceCounterInstaller?displayProperty=nameWithType>|<span data-ttu-id="76079-145">指定 `PerformanceCounter` 组件的安装程序。</span><span class="sxs-lookup"><span data-stu-id="76079-145">Specifies an installer for the `PerformanceCounter` component.</span></span>|  
|<xref:System.Diagnostics.PerformanceCounterType?displayProperty=nameWithType>|<span data-ttu-id="76079-146">为 `NextValue` 指定用于计算 `PerformanceCounter`的方法。</span><span class="sxs-lookup"><span data-stu-id="76079-146">Specifies the formula to calculate the `NextValue` method for a `PerformanceCounter`.</span></span>|  
  
## <a name="see-also"></a><span data-ttu-id="76079-147">另请参阅</span><span class="sxs-lookup"><span data-stu-id="76079-147">See also</span></span>

- [<span data-ttu-id="76079-148">性能计数器</span><span class="sxs-lookup"><span data-stu-id="76079-148">Performance Counters</span></span>](performance-counters.md)
