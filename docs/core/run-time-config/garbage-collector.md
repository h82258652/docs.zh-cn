---
title: 垃圾回收器配置设置
description: 了解用于配置垃圾回收器如何为 .NET Core 应用管理内存的运行时设置。
ms.date: 07/10/2020
ms.topic: reference
ms.openlocfilehash: 1bae00d2fc6e80811986bbb111f38582720f8487
ms.sourcegitcommit: 872ca41d1c26f39d0aef57cc365d09503bac780d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106288066"
---
# <a name="run-time-configuration-options-for-garbage-collection"></a>用于垃圾回收的运行时配置选项

此页包含有关可在运行时更改的垃圾回收器 (GC) 设置的信息。 如果你要尝试让正在运行的应用达到最佳性能，请考虑使用这些设置。 然而，在特定情况下，默认值为大多数应用程序提供最佳性能。

设置在此页上被排入组中。 每个组内的设置通常彼此结合使用以实现特定的结果。

> [!NOTE]
>
> - 这些设置也可以在应用运行时由应用动态地进行更改，因此，你设置的任何运行时设置都可能会被覆盖。
> - 某些设置（如[延迟级别](../../standard/garbage-collection/latency.md)）通常仅在设计时通过 API 进行设置。 此页面省略了此类设置。
> - 对于数值，请对 runtimeconfig.json 文件中的设置使用十进制表示法，而对环境变量设置使用十六进制表示法。 对于十六进制值，可以使用或不使用“0x”前缀来指定它们。

## <a name="flavors-of-garbage-collection"></a>垃圾回收的风格

垃圾回收的两种主要风格是工作站 GC 和服务器 GC。 有关两者之间的差异的详细信息，请参阅[工作站和服务器垃圾回收](../../standard/garbage-collection/workstation-server-gc.md)。

垃圾回收的次要风格是后台垃圾回收和非并发垃圾回收。

使用以下设置，选择垃圾回收的风格：

- [工作站与服务器 GC](#workstation-vs-server)
- [后台垃圾回收](#background-gc)

### <a name="workstation-vs-server"></a>工作站与服务器

- 配置应用程序是使用工作站垃圾回收还是服务器垃圾回收。
- 默认：工作站垃圾回收。 它等效于将值设置为 `false`。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.Server` | `false` - 工作站<br/>`true` - 服务器 | .NET Core 1.0 |
| MSBuild 属性 | `ServerGarbageCollection` | `false` - 工作站<br/>`true` - 服务器 | .NET Core 1.0 |
| **环境变量** | `COMPlus_gcServer` | `0` - 工作站<br/>`1` - 服务器 | .NET Core 1.0 |
| **.NET Framework 的 app.config** | [GCServer](../../framework/configure-apps/file-schema/runtime/gcserver-element.md) | `false` - 工作站<br/>`true` - 服务器 |  |

#### <a name="examples"></a>示例

runtimeconfig.json 文件：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Server": true
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ServerGarbageCollection>true</ServerGarbageCollection>
  </PropertyGroup>

</Project>
```

### <a name="background-gc"></a>后台垃圾回收

- 配置是否启用后台（并发）垃圾回收。
- 默认：使用后台垃圾回收。 它等效于将值设置为 `true`。
- 有关详细信息，请参阅[后台垃圾回收](../../standard/garbage-collection/background-gc.md)。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.Concurrent` | `true` - 后台 GC<br/>`false` - 非并发 GC | .NET Core 1.0 |
| MSBuild 属性 | `ConcurrentGarbageCollection` | `true` - 后台 GC<br/>`false` - 非并发 GC | .NET Core 1.0 |
| **环境变量** | `COMPlus_gcConcurrent` | `1` - 后台 GC<br/>`0` - 非并发 GC | .NET Core 1.0 |
| **.NET Framework 的 app.config** | [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) | `true` - 后台 GC<br/>`false` - 非并发 GC |  |

#### <a name="examples"></a>示例

runtimeconfig.json 文件：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Concurrent": false
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="manage-resource-usage"></a>管理资源使用情况

使用以下设置来管理垃圾回收器的内存和处理器使用情况：

- [关联](#affinitize)
- [关联掩码](#affinitize-mask)
- [关联范围](#affinitize-ranges)
- [CPU 组](#cpu-groups)
- [堆计数](#heap-count)
- [堆限制](#heap-limit)
- [堆限制百分比](#heap-limit-percent)
- [高内存百分比](#high-memory-percent)
- [每对象堆限制](#per-object-heap-limits)
- [每对象堆限制百分比](#per-object-heap-limit-percents)
- [保留 VM](#retain-vm)

有关其中某些设置的详细信息，请参阅 [Middle ground between workstation and server GC](https://devblogs.microsoft.com/dotnet/middle-ground-between-server-and-workstation-gc/)（服务器和工作站 GC 之间的中间地带）博客条目。

### <a name="heap-count"></a>堆计数

- 限制通过垃圾回收器创建的堆数。
- 仅适用于服务器垃圾回收。
- 如果启用了默认的 [GC 处理器关联](#affinitize)，堆计数设置会将 `n` 个 GC 堆/线程关联到前 `n` 个处理器。 （使用[关联掩码](#affinitize-mask)或[关联范围](#affinitize-ranges)设置可精确指定要关联的处理器。）
- 如果禁用了 [GC 处理器关联](#affinitize)，则此设置会限制 GC 堆的数量。
- 有关详细信息，请参阅 [GCHeapCount 备注](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md#remarks)。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapCount` | 十进制值 | .NET Core 3.0 |
| **环境变量** | `COMPlus_GCHeapCount` | 十六进制值 | .NET Core 3.0 |
| **.NET Framework 的 app.config** | [GCHeapCount](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md) | 十进制值 | .NET Framework 4.6.2 |

示例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapCount": 16
      }
   }
}
```

> [!TIP]
> 如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。 如果要将选项设置为一个环境变量，请指定一个十六进制值。 例如，若要将堆数限制为 16，则该值对于 JSON 文件为 16，对于环境变量则为 0x10 或 10。

### <a name="affinitize-mask"></a>关联掩码

- 指定垃圾回收器线程应使用的确切处理器数。
- 如果禁用了 [GC 处理器关联](#affinitize)，则忽略此设置。
- 仅适用于服务器垃圾回收。
- 该值是一个位掩码，用于定义可用于该进程的处理器。 例如，十进制值 1023 或十六进制值 0x3FF 或 3FF（如果使用环境变量）在二进制记数法中为 0011 1111 1111。 这指定将使用前 10 个处理器。 若要指定接下来使用的 10 个处理器（即处理器 10-19），请指定一个十进制值 1047552（或十六进制值 0xFFC00 或 FFC00），它等效于二进制值 1111 1111 1100 0000 0000。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapAffinitizeMask` | 十进制值 | .NET Core 3.0 |
| **环境变量** | `COMPlus_GCHeapAffinitizeMask` | 十六进制值 | .NET Core 3.0 |
| **.NET Framework 的 app.config** | [GCHeapAffinitizeMask](../../framework/configure-apps/file-schema/runtime/gcheapaffinitizemask-element.md) | 十进制值 | .NET Framework 4.6.2 |

示例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapAffinitizeMask": 1023
      }
   }
}
```

### <a name="affinitize-ranges"></a>关联范围

- 指定用于垃圾回收器线程的处理器列表。
- 此设置与 [System.GC.HeapAffinitizeMask](#affinitize-mask) 类似，只是它允许你指定超过 64 个的处理器。
- 对于 Windows 操作系统，请为处理器编号或范围加上相应的 [CPU 组](/windows/win32/procthread/processor-groups)作为前缀，例如“0:1-10,0:12,1:50-52,1:70”。
- 如果禁用了 [GC 处理器关联](#affinitize)，则忽略此设置。
- 仅适用于服务器垃圾回收。
- 有关详细信息，请参阅 Maoni Stephens 的博客文章 [Making CPU configuration better for GC on machines with > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)（在 CPU 大于 64 个的计算机上，为 GC 提供更好的 CPU 配置）。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.GCHeapAffinitizeRanges` | 以逗号分隔的处理器编号列表或处理器编号范围。<br/>Unix 示例：“1-10,12,50-52,70”<br/>Windows 示例：“0:1-10,0:12,1:50-52,1:70” | .NET Core 3.0 |
| **环境变量** | `COMPlus_GCHeapAffinitizeRanges` | 以逗号分隔的处理器编号列表或处理器编号范围。<br/>Unix 示例：“1-10,12,50-52,70”<br/>Windows 示例：“0:1-10,0:12,1:50-52,1:70” | .NET Core 3.0 |

示例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.GCHeapAffinitizeRanges": "0:1-10,0:12,1:50-52,1:70"
      }
   }
}
```

### <a name="cpu-groups"></a>CPU 组

- 配置垃圾回收器是否使用 [CPU 组](/windows/win32/procthread/processor-groups)。

  当 64 位 Windows 计算机具有多个 CPU 组（即，有超过 64 个处理器）时，通过启用此元素，可跨所有 CPU 组扩展垃圾回收。 垃圾回收器使用所有核心来创建和平衡堆。

- 仅适用于 64 位 Windows 操作系统上的服务器垃圾回收。
- 默认：垃圾回收不会跨 CPU 组扩展。 它等效于将值设置为 `0`。
- 有关详细信息，请参阅 Maoni Stephens 的博客文章 [Making CPU configuration better for GC on machines with > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)（在 CPU 大于 64 个的计算机上，为 GC 提供更好的 CPU 配置）。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.CpuGroup` | `0` - 禁用<br/>`1` - 启用 | .NET 5.0 |
| **环境变量** | `COMPlus_GCCpuGroup` | `0` - 禁用<br/>`1` - 启用 | .NET Core 1.0 |
| **.NET Framework 的 app.config** | [GCCpuGroup](../../framework/configure-apps/file-schema/runtime/gccpugroup-element.md) | `false` - 禁用<br/>`true` - 启用 |  |

> [!NOTE]
> 若要配置公共语言运行时 (CLR)，使其也在所有 CPU 组之间分配线程池中的线程，请启用 [Thread_UseAllCpuGroups 元素](../../framework/configure-apps/file-schema/runtime/thread-useallcpugroups-element.md)选项。 对于 .NET Core 应用，可以通过将 `COMPlus_Thread_UseAllCpuGroups` 环境变量的值设置为 `1` 以启用此选项。

### <a name="affinitize"></a>关联

- 指定是否将垃圾回收线程与处理器关联。 若要关联一个 GC 线程，则意味着它只能在其特定的 CPU 上运行。 为每个 GC 线程创建一个堆。
- 仅适用于服务器垃圾回收。
- 默认：将垃圾回收线程与处理器关联。 它等效于将值设置为 `false`。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.NoAffinitize` | `false` - 关联<br/>`true` - 不关联 | .NET Core 3.0 |
| **环境变量** | `COMPlus_GCNoAffinitize` | `0` - 关联<br/>`1` - 不关联 | .NET Core 3.0 |
| **.NET Framework 的 app.config** | [GCNoAffinitize](../../framework/configure-apps/file-schema/runtime/gcnoaffinitize-element.md) | `false` - 关联<br/>`true` - 不关联 | .NET Framework 4.6.2 |

示例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.NoAffinitize": true
      }
   }
}
```

### <a name="heap-limit"></a>堆限制

- 指定 GC 堆和 GC 簿记的最大提交大小（以字节为单位）。
- 此设置仅适用于 64 位计算机。
- 如果已配置[每对象堆限制](#per-object-heap-limits)，则忽略此设置。
- 默认值（仅在某些情况下适用）是 20 MB 或容器内存限制的 75%（以较大者为准）。 此默认值在以下情况下适用：

  - 进程正在具有指定内存限制的容器中运行。
  - [HeapHardLimitPercent](#heap-limit-percent) 未设置。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimit` | 十进制值 | .NET Core 3.0 |
| **环境变量** | `COMPlus_GCHeapHardLimit` | 十六进制值 | .NET Core 3.0 |

示例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimit": 209715200
      }
   }
}
```

> [!TIP]
> 如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。 如果要将选项设置为一个环境变量，请指定一个十六进制值。 例如，若要将堆硬限制指定为 200 个兆字节 (MiB)，则该值对于 JSON 文件为 209715200，对于环境变量则为 0xC800000 或 C800000。

### <a name="heap-limit-percent"></a>堆限制百分比

- 指定允许的 GC 堆使用量占总物理内存的百分比。
- 如果还设置了 [System.GC.heaphdlimit](#heap-limit)，则忽略此设置。
- 此设置仅适用于 64 位计算机。
- 如果进程正在具有指定内存限制的容器中运行，则百分比的计算结果将为该内存限制的百分比。
- 如果已配置[每对象堆限制](#per-object-heap-limits)，则忽略此设置。
- 默认值（仅在某些情况下适用）是 20 MB 或容器内存限制的 75%（以较大者为准）。 此默认值在以下情况下适用：

  - 进程正在具有指定内存限制的容器中运行。
  - [System.GC.HeapHardLimit](#heap-limit) 未设置。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitPercent` | 十进制值 | .NET Core 3.0 |
| **环境变量** | `COMPlus_GCHeapHardLimitPercent` | 十六进制值 | .NET Core 3.0 |

示例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimitPercent": 30
      }
   }
}
```

> [!TIP]
> 如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。 如果要将选项设置为一个环境变量，请指定一个十六进制值。 例如，若要将堆使用率限制为 30%，则该值对于 JSON 文件为 30，对于环境变量则为 0x1E 或 1E。

### <a name="per-object-heap-limits"></a>每对象堆限制

可以根据每个对象堆指定 GC 的允许堆使用量。 不同的堆包括大型对象堆 (LOH)、小型对象堆 (SOH) 和固定对象堆 (POH)。

- 如果为任何 `COMPLUS_GCHeapHardLimitSOH`、`COMPLUS_GCHeapHardLimitLOH` 或 `COMPLUS_GCHeapHardLimitPOH` 设置指定值，则还必须为 `COMPLUS_GCHeapHardLimitSOH` 和 `COMPLUS_GCHeapHardLimitLOH` 指定值。 否则，运行时将无法初始化。
- `COMPLUS_GCHeapHardLimitPOH` 的默认值为 0。 `COMPLUS_GCHeapHardLimitSOH` 和 `COMPLUS_GCHeapHardLimitLOH` 没有默认值。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitSOH` | 十进制值 | .NET 5.0 |
| **环境变量** | `COMPLUS_GCHeapHardLimitSOH` | 十六进制值 | .NET 5.0 |

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitLOH` | 十进制值 | .NET 5.0 |
| **环境变量** | `COMPLUS_GCHeapHardLimitLOH` | 十六进制值 | .NET 5.0 |

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitPOH` | 十进制值 | .NET 5.0 |
| **环境变量** | `COMPLUS_GCHeapHardLimitPOH` | 十六进制值 | .NET 5.0 |

> [!TIP]
> 如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。 如果要将选项设置为一个环境变量，请指定一个十六进制值。 例如，若要将堆硬限制指定为 200 个兆字节 (MiB)，则该值对于 JSON 文件为 209715200，对于环境变量则为 0xC800000 或 C800000。

### <a name="per-object-heap-limit-percents"></a>每对象堆限制百分比

可以根据每个对象堆指定 GC 的允许堆使用量。 不同的堆包括大型对象堆 (LOH)、小型对象堆 (SOH) 和固定对象堆 (POH)。

- 如果为任何 `COMPLUS_GCHeapHardLimitSOHPercent`、`COMPLUS_GCHeapHardLimitLOHPercent` 或 `COMPLUS_GCHeapHardLimitPOHPercent` 设置指定值，则还必须为 `COMPLUS_GCHeapHardLimitSOHPercent` 和 `COMPLUS_GCHeapHardLimitLOHPercent` 指定值。 否则，运行时将无法初始化。
- 如果指定了 `COMPLUS_GCHeapHardLimitSOH`、`COMPLUS_GCHeapHardLimitLOH` 和 `COMPLUS_GCHeapHardLimitPOH`，则忽略这些设置。
- 值为 1 表示 GC 使用该对象堆的总物理内存的 1%。
- 每个值都必须大于 0 并小于 100。 此外，3 个百分比值的总和必须小于 100。 否则，运行时将无法初始化。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitSOHPercent` | 十进制值 | .NET 5.0 |
| **环境变量** | `COMPLUS_GCHeapHardLimitSOHPercent` | 十六进制值 | .NET 5.0 |

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitLOHPercent` | 十进制值 | .NET 5.0 |
| **环境变量** | `COMPLUS_GCHeapHardLimitLOHPercent` | 十六进制值 | .NET 5.0 |

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitPOHPercent` | 十进制值 | .NET 5.0 |
| **环境变量** | `COMPLUS_GCHeapHardLimitPOHPercent` | 十六进制值 | .NET 5.0 |

> [!TIP]
> 如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。 如果要将选项设置为一个环境变量，请指定一个十六进制值。 例如，若要将堆使用率限制为 30%，则该值对于 JSON 文件为 30，对于环境变量则为 0x1E 或 1E。

### <a name="high-memory-percent"></a>高内存百分比

内存负载由正在使用的物理内存的百分比表示。 默认情况下，当物理内存负载达到 90%时，垃圾回收对于执行完整的压缩垃圾回收变得更加积极，以避免分页。 当内存负载低于 90% 时，GC 优先使用后台回收进行完整的垃圾回收，这种方法的暂停时间较短，但不会使堆的总大小减少太多。 在具有大量内存（80 GB 或更多）的计算机上，默认负载阈值介于 90% 到 97% 之间。

可以通过 `COMPlus_GCHighMemPercent` 环境变量或 `System.GC.HighMemoryPercent` JSON 配置设置来调整高内存负载阈值。 如果要控制堆大小，请考虑调整阈值。 例如，对于具有 64 GB 内存的计算机上的主要进程，当有 10% 的可用内存时，GC 开始响应是合理的。 但是对于较小的进程（例如，仅消耗 1GB 内存的进程），GC 可以在可用内存少于 10% 的情况下轻松地运行。 对于这些较小的进程，请考虑将阈值设置得更高。 另一方面，如果你希望较大的进程具有较小的堆大小（即使有足够的物理内存可用），要使 GC 更快做出反应以缩小堆大小，则降低此阈值是一种有效的方法。

> [!NOTE]
> 对于在容器中运行的进程，GC 将根据容器限制考虑物理内存。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HighMemoryPercent` | 十进制值 | .NET 5.0 |
| **环境变量** | `COMPlus_GCHighMemPercent` | 十六进制值 | |

> [!TIP]
> 如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。 如果要将选项设置为一个环境变量，请指定一个十六进制值。 例如，要将高内存阈值设置为 75%，JSON 文件的值将为 75，而环境变量的值为 0x4B 或 4B。

### <a name="retain-vm"></a>保留 VM

- 配置是将应删除的段置于备用列表上供将来使用，还是将其释放回操作系统 (OS)。
- 默认：将段释放回操作系统。 它等效于将值设置为 `false`。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.RetainVM` | `false` - 释放到 OS<br/>`true` - 置于备用列表上 | .NET Core 1.0 |
| MSBuild 属性 | `RetainVMGarbageCollection` | `false` - 释放到 OS<br/>`true` - 置于备用列表上 | .NET Core 1.0 |
| **环境变量** | `COMPlus_GCRetainVM` | `0` - 释放到 OS<br/>`1` - 置于备用列表上 | .NET Core 1.0 |

#### <a name="examples"></a>示例

runtimeconfig.json 文件：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.RetainVM": true
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="large-pages"></a>大型页面

- 指定设置堆硬限制时是否应使用大型页面。
- 默认：设置堆硬限制时不要使用大页面。 它等效于将值设置为 `0`。
- 这是一项实验性设置。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | 不可用 | 空值 | 不可用 |
| **环境变量** | `COMPlus_GCLargePages` | `0` - 禁用<br/>`1` - 启用 | .NET Core 3.0 |

## <a name="allow-large-objects"></a>允许大型对象

- 在 64 位平台上，为总大小大于 2 千兆字节 (GB) 的数组配置垃圾回收器支持。
- 默认：垃圾回收支持大于 2 GB 的数组。 它等效于将值设置为 `1`。
- 在未来的 .NET 版本中，此选项可能会过时。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | 不可用 | 空值 | 不可用 |
| **环境变量** | `COMPlus_gcAllowVeryLargeObjects` | `1` - 启用<br/> `0` - 禁用 | .NET Core 1.0 |
| **.NET Framework 的 app.config** | [gcAllowVeryLargeObjects](../../framework/configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md) | `1` - 启用<br/> `0` - 禁用 | .NET Framework 4.5 |

## <a name="large-object-heap-threshold"></a>大型对象堆阈值

- 指定导致对象进入大型对象堆 (LOH) 的阈值大小（以字节为单位）。
- 默认阈值为 85,000 字节。
- 指定的值必须大于默认阈值。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.LOHThreshold` | 十进制值 | .NET Core 1.0 |
| **环境变量** | `COMPlus_GCLOHThreshold` | 十六进制值 | .NET Core 1.0 |
| **.NET Framework 的 app.config** | [GCLOHThreshold](../../framework/configure-apps/file-schema/runtime/gclohthreshold-element.md) | 十进制值 | .NET Framework 4.8 |

示例：

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.LOHThreshold": 120000
      }
   }
}
```

> [!TIP]
> 如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。 如果要将选项设置为一个环境变量，请指定一个十六进制值。 例如，若要将阙值大小设置为 120,000 个字节，则该值对于 JSON 文件为 120,000，对于环境变量则为 0x1D4C0 或 1D4C0。

## <a name="standalone-gc"></a>独立 GC

- 指定库的路径，该库包含运行时打算加载的垃圾回收器。
- 有关详细信息，请参阅 [Standalone GC loader design](https://github.com/dotnet/runtime/blob/main/docs/design/features/standalone-gc-loading.md)（独立 GC 加载程序设计）。

| | 设置名 | 值 | 引入的版本 |
| - | - | - | - |
| **runtimeconfig.json** | 不可用 | 空值 | 不可用 |
| **环境变量** | `COMPlus_GCName` | string_path | .NET Core 2.0 |
