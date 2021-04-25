---
title: Ngen.exe（本机映像生成器）
description: 查看 Ngen.exe（本机映像生成器）。 通过创建本机映像并将其安装到本地本机映像缓存中来提高托管应用程序性能。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Native Image Generator
- images [.NET Framework], native
- side-by-side execution, native images
- assemblies [.NET Framework], native image
- Ngen.exe
- native image generation
- native image cache
- publisher policy applied for native images
- invalid images
- BypassNGenAttribute
- System.Runtime.BypassNGenAttribute
ms.assetid: 44bf97aa-a9a4-4eba-9a0d-cfaa6fc53a66
ms.openlocfilehash: 480d5172a7ccec0c58b5b615226c836a358cb603
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494696"
---
# <a name="ngenexe-native-image-generator"></a>Ngen.exe（本机映像生成器）

本机映像生成器 (Ngen.exe) 是一种提高托管应用程序性能的工具。 Ngen.exe 创建本机映像（包含经编译的特定于处理器的机器代码的文件），并将它们安装到本地计算机上的本机映像缓存中。 运行时可从缓存中使用本机映像，而不必使用实时 (JIT) 编译器编译原始程序集。

> [!NOTE]
> Ngen.exe 编译仅面向 .NET Framework 的程序集的本机映像。 适用于 .NET Core 的等效本机映像生成器为 [CrossGen](https://github.com/dotnet/runtime/blob/main/docs/workflow/building/coreclr/crossgen.md)。

.NET Framework 4 中对 Ngen.exe 进行的更改：

- Ngen.exe 现在按照完全信任的状态编译程序集，并且不再评估代码访问安全性 (CAS) 策略。

- 使用 Ngen.exe 生成的本机映像不再载入按照部分信任的状态运行的应用程序中。

.NET Framework 2.0 版中对 Ngen.exe 进行的更改：

- 安装程序集时还将安装其依赖项，从而简化了 Ngen.exe 的语法。

- 现在可以在应用程序域之间共享本机映像。

- 可利用新增操作 `update` 重新创建已经失效的映像。

- 操作可由计算机上使用空闲时间生成和安装映像的服务推迟执行。

- 消除了一些导致映像无效的因素。

在 Windows 8 中，请参阅[本机映像任务](#native-image-task)。

有关使用 Ngen.exe 和本机映像服务的其他信息，请参阅[本机映像服务](#native-image-service)。

> [!NOTE]
> 在[本机映像生成器 (Ngen.exe) 旧式语法](/previous-versions/dotnet/netframework-4.0/ms165073(v=vs.100))中可以找到 .NET Framework 1.0 和 1.1 版的 Ngen.exe 语法。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，键入以下内容：

## <a name="syntax"></a>语法

```console
ngen action [options]
```

```console
ngen /? | /help
```

## <a name="actions"></a>操作

下表显示了每个 `action` 的语法。 有关 `action` 的各个部分的说明，请参见[参数](#ArgumentTable)、[优先级别](#PriorityTable)、[方案](#ScenarioTable)和[配置](#ConfigTable)表。 [选项`options`表描述了 ](#OptionTable) 和帮助开关。

|操作|描述|
|------------|-----------------|
|`install` [`assemblyName` &#124; `assemblyPath`] [`scenarios`] [`config`] [`/queue`[`:`{`1`&#124;`2`&#124;`3`}]]|生成程序集及其依赖项的本机映像，并在本机映像缓存中安装这些映像。<br /><br /> 如果指定了 `/queue`，则操作将排队等待本机映像服务。 默认优先级是 3。 请参见[优先级级别](#PriorityTable)表。|
|`uninstall` [`assemblyName` &#124; `assemblyPath`] [`scenarios`] [`config`]|从本机映像缓存中删除程序集及其依赖项的本机映像。<br /><br /> 若要卸载单个映像及其依赖项，可使用与安装此映像时相同的命令行自变量。 **注意：** 从 .NET Framework 4 开始，不再支持操作 `uninstall` *。|
|`update` [`/queue`]|更新已无效的本机映像。<br /><br /> 如果指定了 `/queue`，则更新将排队等待本机映像服务。 更新的优先级总是设定为 3，因此它们在计算机空闲时运行。|
|`display` [`assemblyName` &#124; `assemblyPath`]|显示程序集及其依赖项的本机映像的状态。<br /><br /> 如果未提供自变量，则显示本机映像缓存中的所有内容。|
|`executeQueuedItems` [<code>1&#124;2&#124;3</code>]<br /><br /> \- 或 -<br /><br /> `eqi` [1&#124;2&#124;3]|执行排队的编译作业。<br /><br /> 如果指定了优先级，则执行具有较高或同等优先级的编译作业。 如果未指定优先级，则执行所有排队的编译作业。|
|`queue` {`pause` &#124; `continue` &#124; `status`}|暂停本机映像服务，允许暂停的服务继续，或查询服务状态。|

<a name="ArgumentTable"></a>

## <a name="arguments"></a>参数

|参数|说明|
|--------------|-----------------|
|`assemblyName`|程序集的完整显示名称。 例如 `"myAssembly, Version=2.0.0.0, Culture=neutral, PublicKeyToken=0038abc9deabfle5"`。 **注意：** 可以为 `myAssembly` 和 `display` 操作提供部分程序集名称（如 `uninstall`）。 <br /><br /> 每个 Ngen.exe 命令行只能指定一个程序集。|
|`assemblyPath`|程序集的显式路径。 可指定完整路径或相对路径。<br /><br /> 如果指定文件名而不指定路径，则程序集必须位于当前目录中。<br /><br /> 每个 Ngen.exe 命令行只能指定一个程序集。|

<a name="PriorityTable"></a>

## <a name="priority-levels"></a>优先级级别

|优先级|描述|
|--------------|-----------------|
|`1`|生成本机映像，并立即进行安装，而无需等待空闲时间。|
|`2`|生成本机映像，并立即进行安装，而无需等待空闲时间，但是在所有优先级为 1 的操作（及其依赖项）完成后。|
|`3`|在本机映像服务检测到计算机处于空闲状态时，安装本机映像。 请参阅[本机映像服务](#native-image-service)。|

<a name="ScenarioTable"></a>

## <a name="scenarios"></a>方案

|方案|描述|
|--------------|-----------------|
|`/Debug`|生成可在调试器下使用的本机映像。|
|`/Profile`|生成可在探查器下使用的本机映像。|
|`/NoDependencies`|生成指定方案选项所需的最小数目的本机映像。|

<a name="ConfigTable"></a>

## <a name="config"></a>配置

|配置|说明|
|-------------------|-----------------|
|`/ExeConfig:` `exePath`|使用指定的可执行程序集的配置。<br /><br /> 绑定到依赖项时，Ngen.exe 需要做出与加载程序相同的决策。 如果在运行时使用 <xref:System.Reflection.Assembly.Load%2A> 方法加载共享组件，则应用程序的配置文件将决定为该共享组件加载的依赖项 - 例如，所加载依赖项的版本。 `/ExeConfig` 开关就运行时将加载哪些依赖项为 Ngen.exe 提供了指导。|
|`/AppBase:` `directoryPath`|查找依赖项时，使用指定目录作为应用程序基础。|

<a name="OptionTable"></a>

## <a name="options"></a>选项

|选项|描述|
|------------|-----------------|
|`/nologo`|禁止显示 Microsoft 启动版权标志。|
|`/silent`|禁止显示成功消息。|
|`/verbose`|显示详细的调试信息。|
|`/help`, `/?`|显示当前版本的命令语法和选项。|

## <a name="remarks"></a>备注

若要运行 Ngen.exe，你必须具有管理特权。

> [!CAUTION]
> 不在未完全受信任的程序集上运行 Ngen.exe。 从 .NET Framework 4 开始，Ngen.exe 按照完全信任的状态编译程序集，并且不再评估代码访问安全性 (CAS) 策略。

从 .NET Framework 4 开始，使用 Ngen.exe 生成的本机映像不再载入到按照部分信任的状态运行的应用程序中。 而是，调用了实时 (JIT) 编译器。

Ngen.exe 为 `assemblyname` 参数对 `install` 操作指定的程序集及其所有依赖项生成本机映像。 依赖项是根据程序集清单中的引用来确定的。 仅在应用程序使用反射（例如，通过调用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法）来加载依赖项的情况下，你才需要单独安装依赖项。

> [!IMPORTANT]
> 不要将 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 方法用于本机映像。 使用此方法加载的映像不能由执行上下文中的其他程序集使用。

Ngen.exe 维护一个与依赖项有关的计数。 例如，假设本机映像缓存中同时安装了 `MyAssembly.exe` 和 `YourAssembly.exe`，而且它们都具有对 `OurDependency.dll` 的引用。 如果卸载了 `MyAssembly.exe`，则不会卸载 `OurDependency.dll`。 只有当 `YourAssembly.exe` 也被卸载时才会将其移除。

如果为全局程序集缓存中的程序集生成本机映像，请指定其显示名称。 请参阅 <xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType>。

Ngen.exe 生成的本机映像可以在应用程序域之间共享。 这意味着，在要求在应用程序域之间共享程序集的应用程序方案中，可以使用 Ngen.exe。 若要指定域非特定性，请执行以下操作：

- 将 <xref:System.LoaderOptimizationAttribute> 特性应用于应用程序。

- 为新的应用程序域创建安装信息时，请设置 <xref:System.AppDomainSetup.LoaderOptimization%2A?displayProperty=nameWithType> 属性。

将同一个程序集加载到多个应用程序域中时，总是使用非特定于域的代码。 如果本机映像在已加载到共享域之后又被加载到非共享的应用程序域中，则该映像将无法使用。

> [!NOTE]
> 非特定于域的代码无法卸载，并且性能可能会稍微降低，尤其是在访问静态成员时。

在此“注解”部分中：

- [为不同的方案生成映像](#Scenarios)

- [确定何时使用本机映像](#WhenToUse)

  - [改善内存使用情况](#Memory)

  - [更快的应用程序启动速度](#Startup)

  - [使用注意事项摘要](#UsageSummary)

- [程序集基址的重要性](#BaseAddresses)

- [硬绑定](#HardBinding)

  - [为依赖项指定绑定提示](#DependencyHint)

  - [为程序集指定默认绑定提示](#AssemblyHint)

- [推迟处理](#Deferred)

- [本机映像和 JIT 编译](#JITCompilation)

  - [无效映像](#InvalidImages)

- [疑难解答](#Troubleshooting)

  - [程序集绑定日志查看器](#Fusion)

  - [JITCompilationStart 托管调试助手](#MDA)

  - [选择退出本机映像生成](#OptOut)

<a name="Scenarios"></a>

## <a name="generating-images-for-----different-scenarios"></a>为不同的方案生成映像

生成一个程序集的本机映像后，每当运行时运行该程序集时，都会自动尝试找到并使用该本机映像。 根据使用方案的不同，可生成多个映像。

例如，如果你在调试或分析方案中运行程序集，则运行时将查找利用 `/Debug` 或 `/Profile` 选项生成的本机映像。 如果运行时无法找到匹配的本机映像，它将恢复为标准的 JIT 编译。 调试本机映像的唯一方式是使用 `/Debug` 选项创建本机映像。

`uninstall` 操作也能识别方案，因此你可以卸载所有方案或只卸载选择的方案。

<a name="WhenToUse"></a>

## <a name="determining-when-to-use-native-images"></a>确定何时使用本机映像

本机映像可从两方面提高性能：改善内存使用情况和减少启动时间。

> [!NOTE]
> 本机映像的性能取决于很多因素，这些因素使得分析难以进行，如代码和数据访问模式，有多少调用跨模块边界进行，以及多少依赖项已由其他应用程序加载。 确定本机映像是否对应用程序有利的唯一方式是在关键部署方案中仔细进行性能测量。

<a name="Memory"></a>

### <a name="improved-memory-use"></a>改善内存使用情况

当代码在进程间共享时，本机映像可显著改善内存使用情况。 本机映像为 Windows PE 文件，因此一个 .dll 文件的单个副本可由多个进程共享；而 JIT 编译器生成的本机代码存储在私有内存中，并且不可共享。

运行于终端服务下的应用程序也可从共享代码页中获益。

此外，不加载 JIT 编译器会为每个应用程序实例节省固定量的内存。

<a name="Startup"></a>

### <a name="faster-application-startup"></a>更快的应用程序启动速度

使用 Ngen.exe 预编译程序集可减少某些应用程序的启动时间。 通常，如果应用程序共享组件程序集，则可从中获益，因为在第一个应用程序启动之后，共享组件即已加载，可供后续应用程序使用。 而冷启动（应用程序中的所有程序集必须从硬盘上加载）则不会从本机映像中获得相同的益处，因为硬盘访问时间占了很大比重。

硬绑定可影响启动时间，因为硬绑定至主应用程序程序集的所有映像必须同时加载。

> [!NOTE]
> 在 .NET Framework 3.5 Service Pack 1 之前，你应该将共享且强命名的组件置于全局程序集缓存中，因为加载程序对未处于全局程序集缓存中的强命名程序集执行额外验证，实际抵消了使用本机映像在启动时间方面获得的任何改善。 在 .NET Framework 3.5 SP1 中引入的优化移除了多余的有效性。

<a name="UsageSummary"></a>

### <a name="summary-of-usage-considerations"></a>使用注意事项摘要

下面的常规注意事项和应用程序注意事项可能有助于你决定是否对应用程序的本机映像进行评估：

- 本机映像的加载速度比 MSIL 更快，因为本机映像不必执行很多启动操作，如 JIT 编译和类型安全验证。

- 本机映像需要较小的初始工作集，因为它不需要 JIT 编译器。

- 本机映像允许在进程间共享代码。

- 本机映像需要占用比 MSIL 程序集更大的硬盘空间，并且可能需要相当长的时间才能生成。

- 必须维护本机映像。

  - 在提供原始程序集或原始程序集的某个依赖项后，需要重新生成映像。

  - 一个程序集可能需要多个本机映像，分别用在不同的应用程序或不同的方案中。 例如，两个应用程序中的配置信息可能为同一个依赖程序集生成不同的绑定决策。

  - 必须由管理员（也就是 Administrators 组中的某个 Windows 帐户）来生成本机映像。

除了这些常规注意事项之外，在确定本机映像是否可提供性能益处时，必须考虑应用程序的性质：

- 如果应用程序运行于使用很多共享组件的环境中，则本机映像允许多个进程共享组件。

- 如果应用程序使用多个应用程序域，则本机映像允许跨域共享代码页。

    > [!NOTE]
    > 在 .NET Framework 1.0 和 1.1 版中，本机映像不能跨应用程序域共享。 而在 2.0 版或更高版本中则并非如此。

- 如果应用程序将在终端服务器下运行，则本机映像允许共享代码页。

- 编译为本机映像通常有利于大型应用程序。 小型应用程序通常不会获益。

- 对于长时间运行的应用程序，运行时 JIT 编译的性能略高于本机映像。 （硬绑定某种程度上可减少这一性能差别。）

<a name="BaseAddresses"></a>

## <a name="importance-of-assembly-base-addresses"></a>程序集基址的重要性

因为本机映像为 Windows PE 文件，所以它们和其他可执行文件一样有着相同的重定基址问题。 如果采用硬绑定，则重定位的性能开销甚至更显著。

若要设置本机映像的基址，请使用编译器的相应选项设置程序集的基址。 Ngen.exe 对本机映像使用此基址。

> [!NOTE]
> 本机映像大于创建它时所基于的托管程序集。 基址必须进行计算以允许使用这些更大的大小。

可使用 dumpbin.exe 之类的工具查看本机映像的首选基址。

<a name="HardBinding"></a>

## <a name="hard-binding"></a>硬绑定

硬绑定增加吞吐量并减少本机映像的工作集大小。 硬绑定的缺点是加载程序集时必须加载硬绑定到程序集的所有映像。 对于大型应用程序，这会大大增加启动时间。

硬绑定适合于在所有应用程序性能关键的方案中加载的依赖项。 与本机映像使用情况的任何方面一样，仔细测量性能是确定硬绑定是否可改善应用程序性能的唯一方式。

<xref:System.Runtime.CompilerServices.DependencyAttribute> 和 <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> 特性可使你向 Ngen.exe 提供硬绑定提示。

> [!NOTE]
> 这些特性是对 Ngen.exe 的提示，而不是命令。 使用这些提示不保证进行硬绑定。 这些特性的含义在将来版本中可能会更改。

<a name="DependencyHint"></a>

### <a name="specifying-a-binding-hint-for-a-dependency"></a>为依赖项指定绑定提示

将 <xref:System.Runtime.CompilerServices.DependencyAttribute> 应用于程序集可指示加载指定依赖项的可能性。 <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> 指示适合进行硬绑定，<xref:System.Runtime.CompilerServices.LoadHint.Default> 指示应使用依赖项的默认提示，而 <xref:System.Runtime.CompilerServices.LoadHint.Sometimes> 则指示不适合使用硬绑定。

下面的代码显示有两个依赖项的程序集的特性。 第一个依赖项 (Assembly1) 适合于进行硬绑定，而第二个 (Assembly2) 不适合进行硬绑定。

```vb
Imports System.Runtime.CompilerServices
<Assembly:DependencyAttribute("Assembly1", LoadHint.Always)>
<Assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)>
```

```csharp
using System.Runtime.CompilerServices;
[assembly:DependencyAttribute("Assembly1", LoadHint.Always)]
[assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)]
```

```cpp
using namespace System::Runtime::CompilerServices;
[assembly:DependencyAttribute("Assembly1", LoadHint.Always)];
[assembly:DependencyAttribute("Assembly2", LoadHint.Sometimes)];
```

程序集名称不包括文件扩展名。 可使用显示名称。

<a name="AssemblyHint"></a>

### <a name="specifying-a-default-binding-hint-for-an-assembly"></a>为程序集指定默认绑定提示

只有某些程序集需要默认绑定提示：这些程序集将由依赖于它们的任何应用程序直接并经常使用。 将 <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> 以及 <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> 应用于这样的程序集可指定应使用硬绑定。

> [!NOTE]
> 不应将 <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> 应用于不属于此类别的 .dll 程序集，因为对除 <xref:System.Runtime.CompilerServices.LoadHint.Always?displayProperty=nameWithType> 之外的其他值应用该特性的效果与根本不应用该特性的效果相同。

Microsoft 使用 <xref:System.Runtime.CompilerServices.DefaultDependencyAttribute> 指定硬绑定为 .NET Framework 中极少数程序集（如 mscorlib.dll）的默认绑定。

<a name="Deferred"></a>

## <a name="deferred-processing"></a>推迟处理

超大型应用程序的本机映像生成过程可能需要相当长的时间。 同样，更改共享组件或更改计算机设置可能需要更新很多本机映像。 `install` 和 `update` 操作有一个 `/queue` 选项，该选项将该操作排入队列，以便由本机映像服务推迟执行。 此外，Ngen.exe 具有 `queue` 和 `executeQueuedItems` 操作，这些操作提供了对本机映像服务的某些控制。 有关详细信息，请参阅[本机映像服务](#native-image-service)。

<a name="JITCompilation"></a>

## <a name="native-images-and-jit-compilation"></a>本机映像和 JIT 编译

如果 Ngen.exe 在程序集中遇到它无法生成的任何方法，则会将这些方法从本机映像中排除。 当运行时执行此程序集时，对于那些未包含在本机映像中的方法，它将恢复为 JIT 编译。

此外，如果程序集已升级，或者本机映像出于任何原因已失效，则不会使用本机映像。

<a name="InvalidImages"></a>

### <a name="invalid-images"></a>无效映像

当你使用 Ngen.exe 创建程序集的本机映像时，输出取决于你指定的命令行选项以及计算机上的某些设置。 这些设置包括：

- .NET Framework 的版本。

- 程序集的确切标识（重新编译将更改标识）。

- 程序集引用的所有程序集的确切标识（重新编译将更改标识）。

- 安全因素。

Ngen.exe 在生成本机映像时记录这些信息。 当你执行程序集时，运行时将查找用匹配计算机的当前环境的选项和设置生成的本机映像。 如果运行时无法找到匹配的本机映像，它将恢复为程序集的 JIT 编译。 对计算机的设置和环境进行以下更改会导致本机映像失效：

- .NET Framework 的版本。

     如果将更新应用于 .NET Framework，则使用 Ngen.exe 创建的所有原生映像都将失效。 因此，.NET Framework 的所有更新都执行 `Ngen Update` 命令，以确保重新生成所有的原生映像。 .NET Framework 为它安装的 .NET Framework 库自动创建新的原生映像。

- 程序集的确切标识。

     如果重新编译程序集，则程序集的相应本机映像将失效。

- 程序集引用的任何程序集的确切标识。

     如果更新一个托管程序集，则所有直接或间接依赖该程序集的本机映像都将失效，并需要重新生成。 这既包括一般引用，也包括硬绑定依赖项。 当应用软件更新时，安装程序应执行 `Ngen Update` 命令，以确保重新生成所有依赖的本机映像。

- 安全因素。

     更改计算机安全策略以限制先前授予某个程序集的权限，这样会导致该程序集的先前编译的本机映像失效。

     有关公共语言运行时如何管理代码访问安全性以及如何使用权限的详细信息，请参阅[代码访问安全](../misc/code-access-security.md)。

<a name="Troubleshooting"></a>

## <a name="troubleshooting"></a>疑难解答

可通过参阅以下疑难解答主题，了解应用程序正在使用和无法使用的本机映像，由此确定 JIT 编译器何时开始编译方法以及如何选择退出指定方法的本机映像编译。

<a name="Fusion"></a>

### <a name="assembly-binding-log-viewer"></a>程序集绑定日志查看器

若要确认应用程序正在使用本机映像，可使用 [Fuslogvw.exe（程序集绑定日志查看器）](fuslogvw-exe-assembly-binding-log-viewer.md)。 在绑定日志查看器窗口上，选择“日志类别”框中的“本机映像”。 Fuslogvw.exe 提供了有关本机映像被拒绝的原因的信息。

<a name="MDA"></a>

### <a name="the-jitcompilationstart-managed-debugging-assistant"></a>JITCompilationStart 托管调试助手

可使用 [jitCompilationStart](../debug-trace-profile/jitcompilationstart-mda.md) 托管调试助手 (MDA) 来确定 JIT 编译器何时开始编译函数。

<a name="OptOut"></a>

### <a name="opting-out-of-native-image-generation"></a>选择退出本机映像生成

在某些情况下，NGen.exe 可能难以为特定方法生成本机映像，或者操作者可能更愿意对方法进行 JIT 编译而不是将其编译为本机映像。 在这种情况下，可使用 `System.Runtime.BypassNGenAttribute` 特性阻止 NGen.exe 为特定方法生成本机映像。 必须分别将该特性应用于不想将其中代码包含在本机映像中的各个方法。 NGen.exe 可识别该特性，但不会在本机映像中为相应的方法生成代码。

但请注意，`BypassNGenAttribute` 不定义为 .NET Framework 类库中的类型。 为在代码中使用该特性，必须先按以下所示对其进行定义：

[!code-csharp[System.Runtime.BypassNGenAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/cs/Optout1.cs#1)]
[!code-vb[System.Runtime.BypassNGenAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/vb/Optout1.vb#1)]

然后就可对每个方法应用该特性。 以下示例指示本机映像生成器不应为 `ExampleClass.ToJITCompile` 方法生成本机映像。

[!code-csharp[System.Runtime.BypassNGenAttribute#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/cs/Optout1.cs#2)]
[!code-vb[System.Runtime.BypassNGenAttribute#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/System.Runtime.BypassNGenAttribute/vb/Optout1.vb#2)]

## <a name="examples"></a>示例

下面的命令为当前目录中的 `ClientApp.exe` 生成本机映像，并在本机映像缓存中安装该映像。 如果该程序集存在配置文件，Ngen.exe 将使用它。 此外，还会为 `ClientApp.exe` 引用的所有 .dll 文件生成本机映像。

```console
ngen install ClientApp.exe
```

使用 Ngen.exe 安装的映像也称为根。 根可以为应用程序或共享组件。

下面的命令生成具有指定路径的 `MyAssembly.exe` 的本机映像。

```console
ngen install c:\myfiles\MyAssembly.exe
```

当查找程序集及其依赖项时，Ngen.exe 使用与公共语言运行时所使用的相同的探查逻辑。 默认情况下，包含 `ClientApp.exe` 的目录用作应用程序基目录，所有程序集的探查均从此目录开始。 使用 `/AppBase` 选项可重写此行为。

> [!NOTE]
> 这是对 .NET Framework 1.0 和 1.1 版中的 Ngen.exe 行为的更改，在这些版本中，应用程序基目录设置为当前目录。

程序集可以具有不带引用的依赖项（例如，它使用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法加载 .dll 文件）。 你可以借助 `/ExeConfig` 使用应用程序程序集的配置信息来为这样的 .dll 文件创建本机映像。 下面的命令使用 `MyLib.dll,` 中的配置信息为 `MyApp.exe` 生成一个本机映像。

```console
ngen install c:\myfiles\MyLib.dll /ExeConfig:c:\myapps\MyApp.exe
```

在移除应用程序时将不移除以此方式安装的程序集。

若要卸载依赖项，请使用与安装时相同的命令行选项。 下面的命令卸载上一个示例中的 `MyLib.dll`。

```console
ngen uninstall c:\myfiles\MyLib.dll /ExeConfig:c:\myapps\MyApp.exe
```

若要在全局程序集缓存中为程序集创建本机映像，请使用该程序集的显示名称。 例如：

```console
ngen install "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL"
```

NGen.exe 会为你安装的每个方案生成一个单独的映像集。 例如，下面的命令为正常操作生成一个完整的本机映像集，为调试生成另一个完整的映像集，并为探测生成第三个映像集：

```console
ngen install MyApp.exe
ngen install MyApp.exe /debug
ngen install MyApp.exe /profile
```

### <a name="displaying-the-native-image-cache"></a>显示本机映像缓存

一旦本机映像安装到缓存中，就可使用 Ngen.exe 显示它们。 下面的命令显示本机映像缓存中的所有本机映像。

```console
ngen display
```

`display` 操作首先列出所有根程序集，然后列出计算机上所有本机映像的列表。

使用程序集的简单名称仅显示该程序集的信息。 下面的命令显示本机映像缓存中与部分名称 `MyAssembly` 匹配的所有本机映像、其依赖项以及所有依赖 `MyAssembly` 的根：

```console
ngen display MyAssembly
```

了解哪些根依赖于共享组件程序集对于在共享组件升级后确定 `update` 操作的影响非常有用。

如果指定了程序集的文件扩展名，则必须指定路径，或从包含该程序集的目录执行 Ngen.exe：

```console
ngen display c:\myApps\MyAssembly.exe
```

下面的命令显示本机映像缓存中名为 `MyAssembly`、版本为 1.0.0.0 的所有本机映像。

```console
ngen display "myAssembly, version=1.0.0.0"
```

### <a name="updating-images"></a>更新映像

映像通常是在共享组件升级之后进行更新的。 若要更新本身发生更改或者其依赖项发生了更改的所有本机映像，请使用不带任何参数的 `update` 操作。

```console
ngen update
```

更新所有映像可能会耗费很长时间。 使用 `/queue` 选项可对更新操作进行排队以等候本机映像服务执行。 有关 `/queue` 选项和安装优先级的详细信息，请参阅[本机映像服务](#native-image-service)。

```console
ngen update /queue
```

### <a name="uninstalling-images"></a>卸载映像

Ngen.exe 维护依赖项的列表，因此，只有当依赖于这些共享组件的所有程序集都被移除后，才会移除这些共享组件。 此外，已安装为根的共享组件不会被移除。

下面的命令卸载根 `ClientApp.exe` 的所有方案：

```console
ngen uninstall ClientApp
```

`uninstall` 操作可用于移除特定方案。 下面的命令卸载 `ClientApp.exe` 的所有调试方案：

```console
ngen uninstall ClientApp /debug
```

> [!NOTE]
> 卸载 `/debug` 方案不会卸载同时包含 `/profile` 和 `/debug.` 的方案

下面的命令卸载特定版本的 `ClientApp.exe` 的所有方案：

```console
ngen uninstall "ClientApp, Version=1.0.0.0"
```

下面的命令卸载 `"ClientApp, Version=1.0.0.0, Culture=neutral, PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL",` 的所有方案，或者只卸载该程序集的调试方案：

```console
ngen uninstall "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL"
ngen uninstall "ClientApp, Version=1.0.0.0, Culture=neutral,
  PublicKeyToken=3c7ba247adcd2081, processorArchitecture=MSIL" /debug
```

与 `install` 操作一样，如果提供了扩展名，则需要从包含该程序集的目录执行 Ngen.exe，或者指定完整路径。

有关本机映像服务的相关示例，请参阅[本机映像服务](#native-image-service)。

## <a name="native-image-task"></a>本机映像任务

本机映像任务是生成和维护本机映像的 Windows 任务。 本机映像任务为支持方案自动生成并回收本机映像。 它还使安装程序能使用 [Ngen.exe（本机映像生成器）](ngen-exe-native-image-generator.md)来创建和更新在延迟时间的本机映像。

本机映像任务对计算机上受支持的每个 CPU 体系结构进行注册后，允许对以每个体系结构为目标的应用程序进行编译：

|任务名称|32 位计算机|64 位计算机|
|---------------|----------------------|----------------------|
|NET Framework NGEN v4.0.30319|是|“是”|
|NET Framework NGEN v4.0.30319 64|否|是|

在运行 Windows 8 或更高版本时，本机映像任务在 .NET Framework 4.5 和更高版本中可用。 在 Windows 早期版本中，.NET Framework 使用 [本机映像服务](#native-image-service)。

### <a name="task-lifetime"></a>任务生存期

一般情况下，计算机处于空闲状态时，Windows 任务计划程序将每晚启动本机映像任务。 该任务检查由应用程序安装程序排列的任何延时工作、任何延时本机映像更新请求和任何自动化映像创建。 任务完成未完成的工作项，然后关闭。 如果计算机在任务运行时停止空闲状态，该任务将停止。

此外还可以通过任务计划程序 UI 或手动调用 NGen.exe 来启动本机映像任务。 如果通过上述任一方法启动了该任务，则计算机不处于空闲状态时，它将继续运行。 优先使用 NGen.exe 手动创建的映像，以启用应用程序安装程序的可预知行为。

## <a name="native-image-service"></a>本机映像服务

本机映像任务是生成和维护本机映像的 Windows 任务。 本机映像服务允许开发人员将本机映像的安装和更新延迟到计算机空闲的时段。

通常情况下，本机映像服务通过应用程序安装程序或更新安装程序（安装程序）启动的。 对于优先级 3 操作，该服务在计算机空闲时执行。 该服务将保存其状态，如有必要，能通过多次重新启动后继续运行。 可以对多个映像编译进行排队。

该服务还与手动 Ngen.exe 命令进行交互。 手动命令优先于后台活动。

> [!NOTE]
> 在 Windows Vista 上，为本机映像服务显示的名称为“Microsoft.NET Framework NGEN v2.0.50727_X86”或“Microsoft.NET Framework NGEN v2.0.50727_X64”。 在 Microsoft Windows 的所有早期版本上，名称为“.NET 运行时优化服务 v2.0.50727_X86”或“.NET 运行时优化服务 v2.0.50727_X64”。

### <a name="launching-deferred-operations"></a>启动推迟的操作

在开始安装或升级之前，建议暂停该服务。 这可确保该服务不会在安装程序正在复制文件或将程序集放在全局程序集缓存中时执行。 下面的 Ngen.exe 命令行可以暂停该服务：

```console
ngen queue pause
```

当已排队所有的延迟操作时，下面的命令会允许恢复服务：

```console
ngen queue continue
```

若要在安装新应用程序或更新共享组件时，延迟本机映像生成，请将 `/queue` 选项用于 `install` 或 `update` 操作 。 以下的 Ngen.exe 命令行安装共享组件的本机映像并执行可能已被影响的所有根的更新：

```console
ngen install MyComponent /queue
ngen update /queue
```

`update` 操作将重新生成所有失效的本机映像，而不仅仅是那些使用 `MyComponent` 的本机映像。

如果你的应用程序包含多个根，则可以控制延迟操作的优先级。 以下命令将三个根的安装进行排队。 首先安装 `Assembly1`，无需等待空闲时间。 此外还可以安装 `Assembly2`，无需等待空闲时间，但要在优先级为 1 的所有操作都完成后。 在服务检测到计算机处于空闲状态时安装 `Assembly3`。

```console
ngen install Assembly1 /queue:1
ngen install Assembly2 /queue:2
ngen install Assembly3 /queue:3
```

可以使用 `executeQueuedItems` 操作强制排队操作同步发生。 如果你提供可选优先级，此操作将仅影响具有相等或较低优先级的已排队操作。 默认优先级为 3，因此下面的 Ngen.exe 命令将立即处理所有排队的操作，并且在完成前不会返回：

```console
ngen executeQueuedItems
```

通过 Ngen.exe 执行同步命令，并且不使用本机映像服务。 本机映像服务运行时，可以使用 Ngen.exe 执行操作。

### <a name="service-shutdown"></a>服务关闭

通过执行包括 `/queue` 选项的 Ngen.exe 命令启动后，该服务会在后台运行，直到完成所有操作。 该服务将保存其状态，以便必要时能通过多次重新启动继续运行。 当服务检测到没有更多的排队操作时，它将重置其状态，以便它不会在下次计算机启动时重新启动，随后自动关闭。

### <a name="service-interaction-with-clients"></a>与客户端进行服务交互

在.NET Framework 2.0 版中，与本机映像服务的唯一交互是通过命令行工具 Ngen.exe 进行的。 使用安装脚本中的命令行工具对本机映像服务的操作进行排队，并与服务交互。

## <a name="see-also"></a>请参阅

- [工具](index.md)
- [托管执行过程](../../standard/managed-execution-process.md)
- [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
