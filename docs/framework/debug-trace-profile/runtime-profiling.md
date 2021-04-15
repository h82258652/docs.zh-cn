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
# <a name="runtime-profiling"></a>运行时分析

分析是用于在任何开发或部署方案中收集性能数据的方法。 本节面向想要收集有关应用程序性能的信息的开发人员和系统管理员。  
  
## <a name="tracking-performance-using-the-performance-monitor-perfmonexe"></a>使用性能监视器 (Perfmon.exe) 跟踪性能  

 性能监视器是用于分析 .NET Framework 应用程序的最简单的工具。 性能监视器以图形方式表示在与公共语言运行时一起安装的 .NET Framework 性能计数器和 Windows SDK 中找到的数据。 这些计数器可用于监视从内存管理到实时 (JIT) 编译器性能的方方面面。 它们告诉你应用程序所使用的资源的情况，这是了解应用程序性能的间接方法。 使用这些计数器可了解应用程序在内部的工作方式。  
  
#### <a name="to-run-perfmonexe-on-windows-vista-and-later-versions"></a>在 Windows Vista 和更高版本上运行 Perfmon.exe  
  
1. 在命令提示符处，键入 **perfmon**。 将出现“性能监视器”  控制台。  
  
2. 在“监视工具”  文件夹中，单击“性能监视器” 。  
  
3. 在“性能监视器”工具栏上，如果有“添加”  图标（加号），请单击该图标。 如果没有，请在监视器窗口中单击右键，然后选择“添加计数器”  选项。  
  
     这将打开“添加计数器”  对话框。 “可用计数器”  列表框中显示可用的性能对象。 .NET Framework 应用程序有许多预定义的对象，包括用于内存管理、互操作性、异常处理以及多线程处理的计数器，它们分别是“.NET CLR Memory”**.**、“.NET CLR Interop”、“.NET CLR Exceptions”和“.NET CLR LocksAndThreads”。 每个性能对象均包含多个单一性能计数器。 在性能监视器中的可用性能计数器列表，请参阅 [Performance Counters](performance-counters.md)一起安装的.NET Framework 性能计数器中的数据。  
  
4. 选择性能对象的名称旁边的复选框，以查看它所支持的各个性能计数器的列表。  
  
5. 单击要查看的性能计数器。  
  
6. 在 " **所选对象的实例** " 列表框中，单击 **\<All instances>** 以指定您想要在整个系统范围) 上监视公共语言运行时的性能计数器，全局 (。  
  
     -或-  
  
     在“选定对象的实例”  列表框中，单击要监视该应用程序的性能计数器的应用程序的名称。  
  
     若要区分运行时的多个版本，或消除具有相同名称的多个应用程序的歧义，还必须修改注册表项。 有关详细信息，请参阅 [Performance Counters and In-Process Side-By-Side Applications](performance-counters-and-in-process-side-by-side-applications.md)。  
  
> [!NOTE]
> 如果在性能控制台正在运行时安装新的性能计数器，请在停止后再重启性能控制台，以便显示新的计数器。  
  
 要分析位于某一区域或远程共享中的程序集，请确保该远程程序集在运行性能计数器的计算机上完全受信任。 如果该程序集不具有足够的信任，则性能计数器将不工作。 有关向不同区域授予信任的信息，请参阅 [Caspol.exe（代码访问安全策略工具）](../tools/caspol-exe-code-access-security-policy-tool.md)。  
  
> [!NOTE]
> 在安装了 .NET Framework 4 的系统上，对于使用 .NET Framework 1.1 开发的应用程序，性能监视器可能不会显示某些类别（如 **.NET Clr 数据** 和 **.net clr 网络**）中性能计数器的数据。 如果是这种情况，则可以通过将 [\<forcePerformanceCounterUniqueSharedMemoryReads>](../configure-apps/file-schema/runtime/forceperformancecounteruniquesharedmemoryreads-element.md) 元素添加到应用程序的配置文件中，将性能监视器配置为显示此数据。  
  
## <a name="reading-and-creating-performance-counters-programmatically"></a>以编程方式读取和创建性能计数器  

 .NET Framework 提供了一些类，可用于以编程方式访问性能控制台中提供的相同性能信息。 另外，还可以使用这些类创建自定义性能计数器。 下表描述了 .NET Framework 中提供的某些性能监视类。  
  
|类|说明|  
|-----------|-----------------|  
|<xref:System.Diagnostics.PerformanceCounter?displayProperty=nameWithType>|表示 Windows 性能计数器组件。 使用此类可读取现有预定义或自定义计数器，并向自定义计数器发布（写入）性能数据。|  
|<xref:System.Diagnostics.PerformanceCounterCategory?displayProperty=nameWithType>|提供与计数器交互的几种方法以及计算机上计数器的类别。|  
|<xref:System.Diagnostics.PerformanceCounterInstaller?displayProperty=nameWithType>|指定 `PerformanceCounter` 组件的安装程序。|  
|<xref:System.Diagnostics.PerformanceCounterType?displayProperty=nameWithType>|为 `NextValue` 指定用于计算 `PerformanceCounter`的方法。|  
  
## <a name="see-also"></a>另请参阅

- [性能计数器](performance-counters.md)
