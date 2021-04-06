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
# <a name="run-time-configuration-options-for-garbage-collection"></a><span data-ttu-id="c775c-103">用于垃圾回收的运行时配置选项</span><span class="sxs-lookup"><span data-stu-id="c775c-103">Run-time configuration options for garbage collection</span></span>

<span data-ttu-id="c775c-104">此页包含有关可在运行时更改的垃圾回收器 (GC) 设置的信息。</span><span class="sxs-lookup"><span data-stu-id="c775c-104">This page contains information about garbage collector (GC) settings that can be changed at run time.</span></span> <span data-ttu-id="c775c-105">如果你要尝试让正在运行的应用达到最佳性能，请考虑使用这些设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-105">If you're trying to achieve peak performance of a running app, consider using these settings.</span></span> <span data-ttu-id="c775c-106">然而，在特定情况下，默认值为大多数应用程序提供最佳性能。</span><span class="sxs-lookup"><span data-stu-id="c775c-106">However, the defaults provide optimum performance for most applications in typical situations.</span></span>

<span data-ttu-id="c775c-107">设置在此页上被排入组中。</span><span class="sxs-lookup"><span data-stu-id="c775c-107">Settings are arranged into groups on this page.</span></span> <span data-ttu-id="c775c-108">每个组内的设置通常彼此结合使用以实现特定的结果。</span><span class="sxs-lookup"><span data-stu-id="c775c-108">The settings within each group are commonly used in conjunction with each other to achieve a specific result.</span></span>

> [!NOTE]
>
> - <span data-ttu-id="c775c-109">这些设置也可以在应用运行时由应用动态地进行更改，因此，你设置的任何运行时设置都可能会被覆盖。</span><span class="sxs-lookup"><span data-stu-id="c775c-109">These settings can also be changed dynamically by the app as it's running, so any run-time settings you set may be overridden.</span></span>
> - <span data-ttu-id="c775c-110">某些设置（如[延迟级别](../../standard/garbage-collection/latency.md)）通常仅在设计时通过 API 进行设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-110">Some settings, such as [latency level](../../standard/garbage-collection/latency.md), are typically set only through the API at design time.</span></span> <span data-ttu-id="c775c-111">此页面省略了此类设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-111">Such settings are omitted from this page.</span></span>
> - <span data-ttu-id="c775c-112">对于数值，请对 runtimeconfig.json 文件中的设置使用十进制表示法，而对环境变量设置使用十六进制表示法。</span><span class="sxs-lookup"><span data-stu-id="c775c-112">For number values, use decimal notation for settings in the *runtimeconfig.json* file and hexadecimal notation for environment variable settings.</span></span> <span data-ttu-id="c775c-113">对于十六进制值，可以使用或不使用“0x”前缀来指定它们。</span><span class="sxs-lookup"><span data-stu-id="c775c-113">For hexadecimal values, you can specify them with or without the "0x" prefix.</span></span>

## <a name="flavors-of-garbage-collection"></a><span data-ttu-id="c775c-114">垃圾回收的风格</span><span class="sxs-lookup"><span data-stu-id="c775c-114">Flavors of garbage collection</span></span>

<span data-ttu-id="c775c-115">垃圾回收的两种主要风格是工作站 GC 和服务器 GC。</span><span class="sxs-lookup"><span data-stu-id="c775c-115">The two main flavors of garbage collection are workstation GC and server GC.</span></span> <span data-ttu-id="c775c-116">有关两者之间的差异的详细信息，请参阅[工作站和服务器垃圾回收](../../standard/garbage-collection/workstation-server-gc.md)。</span><span class="sxs-lookup"><span data-stu-id="c775c-116">For more information about differences between the two, see [Workstation and server garbage collection](../../standard/garbage-collection/workstation-server-gc.md).</span></span>

<span data-ttu-id="c775c-117">垃圾回收的次要风格是后台垃圾回收和非并发垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-117">The subflavors of garbage collection are background and non-concurrent.</span></span>

<span data-ttu-id="c775c-118">使用以下设置，选择垃圾回收的风格：</span><span class="sxs-lookup"><span data-stu-id="c775c-118">Use the following settings to select flavors of garbage collection:</span></span>

- [<span data-ttu-id="c775c-119">工作站与服务器 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-119">Workstation vs. server GC</span></span>](#workstation-vs-server)
- [<span data-ttu-id="c775c-120">后台垃圾回收</span><span class="sxs-lookup"><span data-stu-id="c775c-120">Background GC</span></span>](#background-gc)

### <a name="workstation-vs-server"></a><span data-ttu-id="c775c-121">工作站与服务器</span><span class="sxs-lookup"><span data-stu-id="c775c-121">Workstation vs. server</span></span>

- <span data-ttu-id="c775c-122">配置应用程序是使用工作站垃圾回收还是服务器垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-122">Configures whether the application uses workstation garbage collection or server garbage collection.</span></span>
- <span data-ttu-id="c775c-123">默认：工作站垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-123">Default: Workstation garbage collection.</span></span> <span data-ttu-id="c775c-124">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="c775c-124">This is equivalent to setting the value to `false`.</span></span>

| | <span data-ttu-id="c775c-125">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-125">Setting name</span></span> | <span data-ttu-id="c775c-126">值</span><span class="sxs-lookup"><span data-stu-id="c775c-126">Values</span></span> | <span data-ttu-id="c775c-127">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-127">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-128">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-128">**runtimeconfig.json**</span></span> | `System.GC.Server` | <span data-ttu-id="c775c-129">`false` - 工作站</span><span class="sxs-lookup"><span data-stu-id="c775c-129">`false` - workstation</span></span><br/><span data-ttu-id="c775c-130">`true` - 服务器</span><span class="sxs-lookup"><span data-stu-id="c775c-130">`true` - server</span></span> | <span data-ttu-id="c775c-131">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-131">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-132">MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="c775c-132">**MSBuild property**</span></span> | `ServerGarbageCollection` | <span data-ttu-id="c775c-133">`false` - 工作站</span><span class="sxs-lookup"><span data-stu-id="c775c-133">`false` - workstation</span></span><br/><span data-ttu-id="c775c-134">`true` - 服务器</span><span class="sxs-lookup"><span data-stu-id="c775c-134">`true` - server</span></span> | <span data-ttu-id="c775c-135">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-135">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-136">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-136">**Environment variable**</span></span> | `COMPlus_gcServer` | <span data-ttu-id="c775c-137">`0` - 工作站</span><span class="sxs-lookup"><span data-stu-id="c775c-137">`0` - workstation</span></span><br/><span data-ttu-id="c775c-138">`1` - 服务器</span><span class="sxs-lookup"><span data-stu-id="c775c-138">`1` - server</span></span> | <span data-ttu-id="c775c-139">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-139">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-140">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-140">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-141">GCServer</span><span class="sxs-lookup"><span data-stu-id="c775c-141">GCServer</span></span>](../../framework/configure-apps/file-schema/runtime/gcserver-element.md) | <span data-ttu-id="c775c-142">`false` - 工作站</span><span class="sxs-lookup"><span data-stu-id="c775c-142">`false` - workstation</span></span><br/><span data-ttu-id="c775c-143">`true` - 服务器</span><span class="sxs-lookup"><span data-stu-id="c775c-143">`true` - server</span></span> |  |

#### <a name="examples"></a><span data-ttu-id="c775c-144">示例</span><span class="sxs-lookup"><span data-stu-id="c775c-144">Examples</span></span>

<span data-ttu-id="c775c-145">runtimeconfig.json 文件：</span><span class="sxs-lookup"><span data-stu-id="c775c-145">*runtimeconfig.json* file:</span></span>

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Server": true
      }
   }
}
```

<span data-ttu-id="c775c-146">项目文件：</span><span class="sxs-lookup"><span data-stu-id="c775c-146">Project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ServerGarbageCollection>true</ServerGarbageCollection>
  </PropertyGroup>

</Project>
```

### <a name="background-gc"></a><span data-ttu-id="c775c-147">后台垃圾回收</span><span class="sxs-lookup"><span data-stu-id="c775c-147">Background GC</span></span>

- <span data-ttu-id="c775c-148">配置是否启用后台（并发）垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-148">Configures whether background (concurrent) garbage collection is enabled.</span></span>
- <span data-ttu-id="c775c-149">默认：使用后台垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-149">Default: Use background GC.</span></span> <span data-ttu-id="c775c-150">它等效于将值设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="c775c-150">This is equivalent to setting the value to `true`.</span></span>
- <span data-ttu-id="c775c-151">有关详细信息，请参阅[后台垃圾回收](../../standard/garbage-collection/background-gc.md)。</span><span class="sxs-lookup"><span data-stu-id="c775c-151">For more information, see [Background garbage collection](../../standard/garbage-collection/background-gc.md).</span></span>

| | <span data-ttu-id="c775c-152">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-152">Setting name</span></span> | <span data-ttu-id="c775c-153">值</span><span class="sxs-lookup"><span data-stu-id="c775c-153">Values</span></span> | <span data-ttu-id="c775c-154">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-154">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-155">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-155">**runtimeconfig.json**</span></span> | `System.GC.Concurrent` | <span data-ttu-id="c775c-156">`true` - 后台 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-156">`true` - background GC</span></span><br/><span data-ttu-id="c775c-157">`false` - 非并发 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-157">`false` - non-concurrent GC</span></span> | <span data-ttu-id="c775c-158">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-158">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-159">MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="c775c-159">**MSBuild property**</span></span> | `ConcurrentGarbageCollection` | <span data-ttu-id="c775c-160">`true` - 后台 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-160">`true` - background GC</span></span><br/><span data-ttu-id="c775c-161">`false` - 非并发 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-161">`false` - non-concurrent GC</span></span> | <span data-ttu-id="c775c-162">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-162">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-163">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-163">**Environment variable**</span></span> | `COMPlus_gcConcurrent` | <span data-ttu-id="c775c-164">`1` - 后台 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-164">`1` - background GC</span></span><br/><span data-ttu-id="c775c-165">`0` - 非并发 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-165">`0` - non-concurrent GC</span></span> | <span data-ttu-id="c775c-166">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-166">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-167">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-167">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-168">gcConcurrent</span><span class="sxs-lookup"><span data-stu-id="c775c-168">gcConcurrent</span></span>](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) | <span data-ttu-id="c775c-169">`true` - 后台 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-169">`true` - background GC</span></span><br/><span data-ttu-id="c775c-170">`false` - 非并发 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-170">`false` - non-concurrent GC</span></span> |  |

#### <a name="examples"></a><span data-ttu-id="c775c-171">示例</span><span class="sxs-lookup"><span data-stu-id="c775c-171">Examples</span></span>

<span data-ttu-id="c775c-172">runtimeconfig.json 文件：</span><span class="sxs-lookup"><span data-stu-id="c775c-172">*runtimeconfig.json* file:</span></span>

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Concurrent": false
      }
   }
}
```

<span data-ttu-id="c775c-173">项目文件：</span><span class="sxs-lookup"><span data-stu-id="c775c-173">Project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="manage-resource-usage"></a><span data-ttu-id="c775c-174">管理资源使用情况</span><span class="sxs-lookup"><span data-stu-id="c775c-174">Manage resource usage</span></span>

<span data-ttu-id="c775c-175">使用以下设置来管理垃圾回收器的内存和处理器使用情况：</span><span class="sxs-lookup"><span data-stu-id="c775c-175">Use the following settings to manage the garbage collector's memory and processor usage:</span></span>

- [<span data-ttu-id="c775c-176">关联</span><span class="sxs-lookup"><span data-stu-id="c775c-176">Affinitize</span></span>](#affinitize)
- [<span data-ttu-id="c775c-177">关联掩码</span><span class="sxs-lookup"><span data-stu-id="c775c-177">Affinitize mask</span></span>](#affinitize-mask)
- [<span data-ttu-id="c775c-178">关联范围</span><span class="sxs-lookup"><span data-stu-id="c775c-178">Affinitize ranges</span></span>](#affinitize-ranges)
- [<span data-ttu-id="c775c-179">CPU 组</span><span class="sxs-lookup"><span data-stu-id="c775c-179">CPU groups</span></span>](#cpu-groups)
- [<span data-ttu-id="c775c-180">堆计数</span><span class="sxs-lookup"><span data-stu-id="c775c-180">Heap count</span></span>](#heap-count)
- [<span data-ttu-id="c775c-181">堆限制</span><span class="sxs-lookup"><span data-stu-id="c775c-181">Heap limit</span></span>](#heap-limit)
- [<span data-ttu-id="c775c-182">堆限制百分比</span><span class="sxs-lookup"><span data-stu-id="c775c-182">Heap limit percent</span></span>](#heap-limit-percent)
- [<span data-ttu-id="c775c-183">高内存百分比</span><span class="sxs-lookup"><span data-stu-id="c775c-183">High memory percent</span></span>](#high-memory-percent)
- [<span data-ttu-id="c775c-184">每对象堆限制</span><span class="sxs-lookup"><span data-stu-id="c775c-184">Per-object-heap limits</span></span>](#per-object-heap-limits)
- [<span data-ttu-id="c775c-185">每对象堆限制百分比</span><span class="sxs-lookup"><span data-stu-id="c775c-185">Per-object-heap limit percents</span></span>](#per-object-heap-limit-percents)
- [<span data-ttu-id="c775c-186">保留 VM</span><span class="sxs-lookup"><span data-stu-id="c775c-186">Retain VM</span></span>](#retain-vm)

<span data-ttu-id="c775c-187">有关其中某些设置的详细信息，请参阅 [Middle ground between workstation and server GC](https://devblogs.microsoft.com/dotnet/middle-ground-between-server-and-workstation-gc/)（服务器和工作站 GC 之间的中间地带）博客条目。</span><span class="sxs-lookup"><span data-stu-id="c775c-187">For more information about some of these settings, see the [Middle ground between workstation and server GC](https://devblogs.microsoft.com/dotnet/middle-ground-between-server-and-workstation-gc/) blog entry.</span></span>

### <a name="heap-count"></a><span data-ttu-id="c775c-188">堆计数</span><span class="sxs-lookup"><span data-stu-id="c775c-188">Heap count</span></span>

- <span data-ttu-id="c775c-189">限制通过垃圾回收器创建的堆数。</span><span class="sxs-lookup"><span data-stu-id="c775c-189">Limits the number of heaps created by the garbage collector.</span></span>
- <span data-ttu-id="c775c-190">仅适用于服务器垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-190">Applies to server garbage collection only.</span></span>
- <span data-ttu-id="c775c-191">如果启用了默认的 [GC 处理器关联](#affinitize)，堆计数设置会将 `n` 个 GC 堆/线程关联到前 `n` 个处理器。</span><span class="sxs-lookup"><span data-stu-id="c775c-191">If [GC processor affinity](#affinitize) is enabled, which is the default, the heap count setting affinitizes `n` GC heaps/threads to the first `n` processors.</span></span> <span data-ttu-id="c775c-192">（使用[关联掩码](#affinitize-mask)或[关联范围](#affinitize-ranges)设置可精确指定要关联的处理器。）</span><span class="sxs-lookup"><span data-stu-id="c775c-192">(Use the [affinitize mask](#affinitize-mask) or [affinitize ranges](#affinitize-ranges) settings to specify exactly which processors to affinitize.)</span></span>
- <span data-ttu-id="c775c-193">如果禁用了 [GC 处理器关联](#affinitize)，则此设置会限制 GC 堆的数量。</span><span class="sxs-lookup"><span data-stu-id="c775c-193">If [GC processor affinity](#affinitize) is disabled, this setting limits the number of GC heaps.</span></span>
- <span data-ttu-id="c775c-194">有关详细信息，请参阅 [GCHeapCount 备注](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md#remarks)。</span><span class="sxs-lookup"><span data-stu-id="c775c-194">For more information, see the [GCHeapCount remarks](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md#remarks).</span></span>

| | <span data-ttu-id="c775c-195">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-195">Setting name</span></span> | <span data-ttu-id="c775c-196">值</span><span class="sxs-lookup"><span data-stu-id="c775c-196">Values</span></span> | <span data-ttu-id="c775c-197">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-197">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-198">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-198">**runtimeconfig.json**</span></span> | `System.GC.HeapCount` | <span data-ttu-id="c775c-199">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-199">*decimal value*</span></span> | <span data-ttu-id="c775c-200">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-200">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-201">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-201">**Environment variable**</span></span> | `COMPlus_GCHeapCount` | <span data-ttu-id="c775c-202">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-202">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-203">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-203">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-204">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-204">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-205">GCHeapCount</span><span class="sxs-lookup"><span data-stu-id="c775c-205">GCHeapCount</span></span>](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md) | <span data-ttu-id="c775c-206">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-206">*decimal value*</span></span> | <span data-ttu-id="c775c-207">.NET Framework 4.6.2</span><span class="sxs-lookup"><span data-stu-id="c775c-207">.NET Framework 4.6.2</span></span> |

<span data-ttu-id="c775c-208">示例：</span><span class="sxs-lookup"><span data-stu-id="c775c-208">Example:</span></span>

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
> <span data-ttu-id="c775c-209">如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-209">If you're setting the option in *runtimeconfig.json*, specify a decimal value.</span></span> <span data-ttu-id="c775c-210">如果要将选项设置为一个环境变量，请指定一个十六进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-210">If you're setting the option as an environment variable, specify a hexadecimal value.</span></span> <span data-ttu-id="c775c-211">例如，若要将堆数限制为 16，则该值对于 JSON 文件为 16，对于环境变量则为 0x10 或 10。</span><span class="sxs-lookup"><span data-stu-id="c775c-211">For example, to limit the number of heaps to 16, the values would be 16 for the JSON file and 0x10 or 10 for the environment variable.</span></span>

### <a name="affinitize-mask"></a><span data-ttu-id="c775c-212">关联掩码</span><span class="sxs-lookup"><span data-stu-id="c775c-212">Affinitize mask</span></span>

- <span data-ttu-id="c775c-213">指定垃圾回收器线程应使用的确切处理器数。</span><span class="sxs-lookup"><span data-stu-id="c775c-213">Specifies the exact processors that garbage collector threads should use.</span></span>
- <span data-ttu-id="c775c-214">如果禁用了 [GC 处理器关联](#affinitize)，则忽略此设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-214">If [GC processor affinity](#affinitize) is disabled, this setting is ignored.</span></span>
- <span data-ttu-id="c775c-215">仅适用于服务器垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-215">Applies to server garbage collection only.</span></span>
- <span data-ttu-id="c775c-216">该值是一个位掩码，用于定义可用于该进程的处理器。</span><span class="sxs-lookup"><span data-stu-id="c775c-216">The value is a bit mask that defines the processors that are available to the process.</span></span> <span data-ttu-id="c775c-217">例如，十进制值 1023 或十六进制值 0x3FF 或 3FF（如果使用环境变量）在二进制记数法中为 0011 1111 1111。</span><span class="sxs-lookup"><span data-stu-id="c775c-217">For example, a decimal value of 1023 (or a hexadecimal value of 0x3FF or 3FF if you're using the environment variable) is 0011 1111 1111 in binary notation.</span></span> <span data-ttu-id="c775c-218">这指定将使用前 10 个处理器。</span><span class="sxs-lookup"><span data-stu-id="c775c-218">This specifies that the first 10 processors are to be used.</span></span> <span data-ttu-id="c775c-219">若要指定接下来使用的 10 个处理器（即处理器 10-19），请指定一个十进制值 1047552（或十六进制值 0xFFC00 或 FFC00），它等效于二进制值 1111 1111 1100 0000 0000。</span><span class="sxs-lookup"><span data-stu-id="c775c-219">To specify the next 10 processors, that is, processors 10-19, specify a decimal value of 1047552 (or a hexadecimal value of 0xFFC00 or FFC00), which is equivalent to a binary value of 1111 1111 1100 0000 0000.</span></span>

| | <span data-ttu-id="c775c-220">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-220">Setting name</span></span> | <span data-ttu-id="c775c-221">值</span><span class="sxs-lookup"><span data-stu-id="c775c-221">Values</span></span> | <span data-ttu-id="c775c-222">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-222">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-223">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-223">**runtimeconfig.json**</span></span> | `System.GC.HeapAffinitizeMask` | <span data-ttu-id="c775c-224">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-224">*decimal value*</span></span> | <span data-ttu-id="c775c-225">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-225">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-226">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-226">**Environment variable**</span></span> | `COMPlus_GCHeapAffinitizeMask` | <span data-ttu-id="c775c-227">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-227">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-228">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-228">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-229">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-229">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-230">GCHeapAffinitizeMask</span><span class="sxs-lookup"><span data-stu-id="c775c-230">GCHeapAffinitizeMask</span></span>](../../framework/configure-apps/file-schema/runtime/gcheapaffinitizemask-element.md) | <span data-ttu-id="c775c-231">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-231">*decimal value*</span></span> | <span data-ttu-id="c775c-232">.NET Framework 4.6.2</span><span class="sxs-lookup"><span data-stu-id="c775c-232">.NET Framework 4.6.2</span></span> |

<span data-ttu-id="c775c-233">示例：</span><span class="sxs-lookup"><span data-stu-id="c775c-233">Example:</span></span>

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapAffinitizeMask": 1023
      }
   }
}
```

### <a name="affinitize-ranges"></a><span data-ttu-id="c775c-234">关联范围</span><span class="sxs-lookup"><span data-stu-id="c775c-234">Affinitize ranges</span></span>

- <span data-ttu-id="c775c-235">指定用于垃圾回收器线程的处理器列表。</span><span class="sxs-lookup"><span data-stu-id="c775c-235">Specifies the list of processors to use for garbage collector threads.</span></span>
- <span data-ttu-id="c775c-236">此设置与 [System.GC.HeapAffinitizeMask](#affinitize-mask) 类似，只是它允许你指定超过 64 个的处理器。</span><span class="sxs-lookup"><span data-stu-id="c775c-236">This setting is similar to [System.GC.HeapAffinitizeMask](#affinitize-mask), except it allows you to specify more than 64 processors.</span></span>
- <span data-ttu-id="c775c-237">对于 Windows 操作系统，请为处理器编号或范围加上相应的 [CPU 组](/windows/win32/procthread/processor-groups)作为前缀，例如“0:1-10,0:12,1:50-52,1:70”。</span><span class="sxs-lookup"><span data-stu-id="c775c-237">For Windows operating systems, prefix the processor number or range with the corresponding [CPU group](/windows/win32/procthread/processor-groups), for example, "0:1-10,0:12,1:50-52,1:70".</span></span>
- <span data-ttu-id="c775c-238">如果禁用了 [GC 处理器关联](#affinitize)，则忽略此设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-238">If [GC processor affinity](#affinitize) is disabled, this setting is ignored.</span></span>
- <span data-ttu-id="c775c-239">仅适用于服务器垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-239">Applies to server garbage collection only.</span></span>
- <span data-ttu-id="c775c-240">有关详细信息，请参阅 Maoni Stephens 的博客文章 [Making CPU configuration better for GC on machines with > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)（在 CPU 大于 64 个的计算机上，为 GC 提供更好的 CPU 配置）。</span><span class="sxs-lookup"><span data-stu-id="c775c-240">For more information, see [Making CPU configuration better for GC on machines with > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/) on Maoni Stephens' blog.</span></span>

| | <span data-ttu-id="c775c-241">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-241">Setting name</span></span> | <span data-ttu-id="c775c-242">值</span><span class="sxs-lookup"><span data-stu-id="c775c-242">Values</span></span> | <span data-ttu-id="c775c-243">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-243">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-244">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-244">**runtimeconfig.json**</span></span> | `System.GC.GCHeapAffinitizeRanges` | <span data-ttu-id="c775c-245">以逗号分隔的处理器编号列表或处理器编号范围。</span><span class="sxs-lookup"><span data-stu-id="c775c-245">Comma-separated list of processor numbers or ranges of processor numbers.</span></span><br/><span data-ttu-id="c775c-246">Unix 示例：“1-10,12,50-52,70”</span><span class="sxs-lookup"><span data-stu-id="c775c-246">Unix example: "1-10,12,50-52,70"</span></span><br/><span data-ttu-id="c775c-247">Windows 示例：“0:1-10,0:12,1:50-52,1:70”</span><span class="sxs-lookup"><span data-stu-id="c775c-247">Windows example: "0:1-10,0:12,1:50-52,1:70"</span></span> | <span data-ttu-id="c775c-248">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-248">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-249">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-249">**Environment variable**</span></span> | `COMPlus_GCHeapAffinitizeRanges` | <span data-ttu-id="c775c-250">以逗号分隔的处理器编号列表或处理器编号范围。</span><span class="sxs-lookup"><span data-stu-id="c775c-250">Comma-separated list of processor numbers or ranges of processor numbers.</span></span><br/><span data-ttu-id="c775c-251">Unix 示例：“1-10,12,50-52,70”</span><span class="sxs-lookup"><span data-stu-id="c775c-251">Unix example: "1-10,12,50-52,70"</span></span><br/><span data-ttu-id="c775c-252">Windows 示例：“0:1-10,0:12,1:50-52,1:70”</span><span class="sxs-lookup"><span data-stu-id="c775c-252">Windows example: "0:1-10,0:12,1:50-52,1:70"</span></span> | <span data-ttu-id="c775c-253">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-253">.NET Core 3.0</span></span> |

<span data-ttu-id="c775c-254">示例：</span><span class="sxs-lookup"><span data-stu-id="c775c-254">Example:</span></span>

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.GCHeapAffinitizeRanges": "0:1-10,0:12,1:50-52,1:70"
      }
   }
}
```

### <a name="cpu-groups"></a><span data-ttu-id="c775c-255">CPU 组</span><span class="sxs-lookup"><span data-stu-id="c775c-255">CPU groups</span></span>

- <span data-ttu-id="c775c-256">配置垃圾回收器是否使用 [CPU 组](/windows/win32/procthread/processor-groups)。</span><span class="sxs-lookup"><span data-stu-id="c775c-256">Configures whether the garbage collector uses [CPU groups](/windows/win32/procthread/processor-groups) or not.</span></span>

  <span data-ttu-id="c775c-257">当 64 位 Windows 计算机具有多个 CPU 组（即，有超过 64 个处理器）时，通过启用此元素，可跨所有 CPU 组扩展垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-257">When a 64-bit Windows computer has multiple CPU groups, that is, there are more than 64 processors, enabling this element extends garbage collection across all CPU groups.</span></span> <span data-ttu-id="c775c-258">垃圾回收器使用所有核心来创建和平衡堆。</span><span class="sxs-lookup"><span data-stu-id="c775c-258">The garbage collector uses all cores to create and balance heaps.</span></span>

- <span data-ttu-id="c775c-259">仅适用于 64 位 Windows 操作系统上的服务器垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-259">Applies to server garbage collection on 64-bit Windows operation systems only.</span></span>
- <span data-ttu-id="c775c-260">默认：垃圾回收不会跨 CPU 组扩展。</span><span class="sxs-lookup"><span data-stu-id="c775c-260">Default: GC does not extend across CPU groups.</span></span> <span data-ttu-id="c775c-261">它等效于将值设置为 `0`。</span><span class="sxs-lookup"><span data-stu-id="c775c-261">This is equivalent to setting the value to `0`.</span></span>
- <span data-ttu-id="c775c-262">有关详细信息，请参阅 Maoni Stephens 的博客文章 [Making CPU configuration better for GC on machines with > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)（在 CPU 大于 64 个的计算机上，为 GC 提供更好的 CPU 配置）。</span><span class="sxs-lookup"><span data-stu-id="c775c-262">For more information, see [Making CPU configuration better for GC on machines with > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/) on Maoni Stephens' blog.</span></span>

| | <span data-ttu-id="c775c-263">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-263">Setting name</span></span> | <span data-ttu-id="c775c-264">值</span><span class="sxs-lookup"><span data-stu-id="c775c-264">Values</span></span> | <span data-ttu-id="c775c-265">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-265">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-266">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-266">**runtimeconfig.json**</span></span> | `System.GC.CpuGroup` | <span data-ttu-id="c775c-267">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="c775c-267">`0` - disabled</span></span><br/><span data-ttu-id="c775c-268">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="c775c-268">`1` - enabled</span></span> | <span data-ttu-id="c775c-269">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-269">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-270">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-270">**Environment variable**</span></span> | `COMPlus_GCCpuGroup` | <span data-ttu-id="c775c-271">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="c775c-271">`0` - disabled</span></span><br/><span data-ttu-id="c775c-272">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="c775c-272">`1` - enabled</span></span> | <span data-ttu-id="c775c-273">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-273">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-274">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-274">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-275">GCCpuGroup</span><span class="sxs-lookup"><span data-stu-id="c775c-275">GCCpuGroup</span></span>](../../framework/configure-apps/file-schema/runtime/gccpugroup-element.md) | <span data-ttu-id="c775c-276">`false` - 禁用</span><span class="sxs-lookup"><span data-stu-id="c775c-276">`false` - disabled</span></span><br/><span data-ttu-id="c775c-277">`true` - 启用</span><span class="sxs-lookup"><span data-stu-id="c775c-277">`true` - enabled</span></span> |  |

> [!NOTE]
> <span data-ttu-id="c775c-278">若要配置公共语言运行时 (CLR)，使其也在所有 CPU 组之间分配线程池中的线程，请启用 [Thread_UseAllCpuGroups 元素](../../framework/configure-apps/file-schema/runtime/thread-useallcpugroups-element.md)选项。</span><span class="sxs-lookup"><span data-stu-id="c775c-278">To configure the common language runtime (CLR) to also distribute threads from the thread pool across all CPU groups, enable the [Thread_UseAllCpuGroups element](../../framework/configure-apps/file-schema/runtime/thread-useallcpugroups-element.md) option.</span></span> <span data-ttu-id="c775c-279">对于 .NET Core 应用，可以通过将 `COMPlus_Thread_UseAllCpuGroups` 环境变量的值设置为 `1` 以启用此选项。</span><span class="sxs-lookup"><span data-stu-id="c775c-279">For .NET Core apps, you can enable this option by setting the value of the `COMPlus_Thread_UseAllCpuGroups` environment variable to `1`.</span></span>

### <a name="affinitize"></a><span data-ttu-id="c775c-280">关联</span><span class="sxs-lookup"><span data-stu-id="c775c-280">Affinitize</span></span>

- <span data-ttu-id="c775c-281">指定是否将垃圾回收线程与处理器关联。</span><span class="sxs-lookup"><span data-stu-id="c775c-281">Specifies whether to *affinitize* garbage collection threads with processors.</span></span> <span data-ttu-id="c775c-282">若要关联一个 GC 线程，则意味着它只能在其特定的 CPU 上运行。</span><span class="sxs-lookup"><span data-stu-id="c775c-282">To affinitize a GC thread means that it can only run on its specific CPU.</span></span> <span data-ttu-id="c775c-283">为每个 GC 线程创建一个堆。</span><span class="sxs-lookup"><span data-stu-id="c775c-283">A heap is created for each GC thread.</span></span>
- <span data-ttu-id="c775c-284">仅适用于服务器垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="c775c-284">Applies to server garbage collection only.</span></span>
- <span data-ttu-id="c775c-285">默认：将垃圾回收线程与处理器关联。</span><span class="sxs-lookup"><span data-stu-id="c775c-285">Default: Affinitize garbage collection threads with processors.</span></span> <span data-ttu-id="c775c-286">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="c775c-286">This is equivalent to setting the value to `false`.</span></span>

| | <span data-ttu-id="c775c-287">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-287">Setting name</span></span> | <span data-ttu-id="c775c-288">值</span><span class="sxs-lookup"><span data-stu-id="c775c-288">Values</span></span> | <span data-ttu-id="c775c-289">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-289">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-290">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-290">**runtimeconfig.json**</span></span> | `System.GC.NoAffinitize` | <span data-ttu-id="c775c-291">`false` - 关联</span><span class="sxs-lookup"><span data-stu-id="c775c-291">`false` - affinitize</span></span><br/><span data-ttu-id="c775c-292">`true` - 不关联</span><span class="sxs-lookup"><span data-stu-id="c775c-292">`true` - don't affinitize</span></span> | <span data-ttu-id="c775c-293">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-293">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-294">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-294">**Environment variable**</span></span> | `COMPlus_GCNoAffinitize` | <span data-ttu-id="c775c-295">`0` - 关联</span><span class="sxs-lookup"><span data-stu-id="c775c-295">`0` - affinitize</span></span><br/><span data-ttu-id="c775c-296">`1` - 不关联</span><span class="sxs-lookup"><span data-stu-id="c775c-296">`1` - don't affinitize</span></span> | <span data-ttu-id="c775c-297">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-297">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-298">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-298">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-299">GCNoAffinitize</span><span class="sxs-lookup"><span data-stu-id="c775c-299">GCNoAffinitize</span></span>](../../framework/configure-apps/file-schema/runtime/gcnoaffinitize-element.md) | <span data-ttu-id="c775c-300">`false` - 关联</span><span class="sxs-lookup"><span data-stu-id="c775c-300">`false` - affinitize</span></span><br/><span data-ttu-id="c775c-301">`true` - 不关联</span><span class="sxs-lookup"><span data-stu-id="c775c-301">`true` - don't affinitize</span></span> | <span data-ttu-id="c775c-302">.NET Framework 4.6.2</span><span class="sxs-lookup"><span data-stu-id="c775c-302">.NET Framework 4.6.2</span></span> |

<span data-ttu-id="c775c-303">示例：</span><span class="sxs-lookup"><span data-stu-id="c775c-303">Example:</span></span>

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.NoAffinitize": true
      }
   }
}
```

### <a name="heap-limit"></a><span data-ttu-id="c775c-304">堆限制</span><span class="sxs-lookup"><span data-stu-id="c775c-304">Heap limit</span></span>

- <span data-ttu-id="c775c-305">指定 GC 堆和 GC 簿记的最大提交大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="c775c-305">Specifies the maximum commit size, in bytes, for the GC heap and GC bookkeeping.</span></span>
- <span data-ttu-id="c775c-306">此设置仅适用于 64 位计算机。</span><span class="sxs-lookup"><span data-stu-id="c775c-306">This setting only applies to 64-bit computers.</span></span>
- <span data-ttu-id="c775c-307">如果已配置[每对象堆限制](#per-object-heap-limits)，则忽略此设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-307">This setting is ignored if the [Per-object-heap limits](#per-object-heap-limits) are configured.</span></span>
- <span data-ttu-id="c775c-308">默认值（仅在某些情况下适用）是 20 MB 或容器内存限制的 75%（以较大者为准）。</span><span class="sxs-lookup"><span data-stu-id="c775c-308">The default value, which only applies in certain cases, is the greater of 20 MB or 75% of the memory limit on the container.</span></span> <span data-ttu-id="c775c-309">此默认值在以下情况下适用：</span><span class="sxs-lookup"><span data-stu-id="c775c-309">The default value applies if:</span></span>

  - <span data-ttu-id="c775c-310">进程正在具有指定内存限制的容器中运行。</span><span class="sxs-lookup"><span data-stu-id="c775c-310">The process is running inside a container that has a specified memory limit.</span></span>
  - <span data-ttu-id="c775c-311">[HeapHardLimitPercent](#heap-limit-percent) 未设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-311">[System.GC.HeapHardLimitPercent](#heap-limit-percent) is not set.</span></span>

| | <span data-ttu-id="c775c-312">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-312">Setting name</span></span> | <span data-ttu-id="c775c-313">值</span><span class="sxs-lookup"><span data-stu-id="c775c-313">Values</span></span> | <span data-ttu-id="c775c-314">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-314">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-315">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-315">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimit` | <span data-ttu-id="c775c-316">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-316">*decimal value*</span></span> | <span data-ttu-id="c775c-317">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-317">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-318">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-318">**Environment variable**</span></span> | `COMPlus_GCHeapHardLimit` | <span data-ttu-id="c775c-319">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-319">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-320">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-320">.NET Core 3.0</span></span> |

<span data-ttu-id="c775c-321">示例：</span><span class="sxs-lookup"><span data-stu-id="c775c-321">Example:</span></span>

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
> <span data-ttu-id="c775c-322">如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-322">If you're setting the option in *runtimeconfig.json*, specify a decimal value.</span></span> <span data-ttu-id="c775c-323">如果要将选项设置为一个环境变量，请指定一个十六进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-323">If you're setting the option as an environment variable, specify a hexadecimal value.</span></span> <span data-ttu-id="c775c-324">例如，若要将堆硬限制指定为 200 个兆字节 (MiB)，则该值对于 JSON 文件为 209715200，对于环境变量则为 0xC800000 或 C800000。</span><span class="sxs-lookup"><span data-stu-id="c775c-324">For example, to specify a heap hard limit of 200 mebibytes (MiB), the values would be 209715200 for the JSON file and 0xC800000 or C800000 for the environment variable.</span></span>

### <a name="heap-limit-percent"></a><span data-ttu-id="c775c-325">堆限制百分比</span><span class="sxs-lookup"><span data-stu-id="c775c-325">Heap limit percent</span></span>

- <span data-ttu-id="c775c-326">指定允许的 GC 堆使用量占总物理内存的百分比。</span><span class="sxs-lookup"><span data-stu-id="c775c-326">Specifies the allowable GC heap usage as a percentage of the total physical memory.</span></span>
- <span data-ttu-id="c775c-327">如果还设置了 [System.GC.heaphdlimit](#heap-limit)，则忽略此设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-327">If [System.GC.HeapHardLimit](#heap-limit) is also set, this setting is ignored.</span></span>
- <span data-ttu-id="c775c-328">此设置仅适用于 64 位计算机。</span><span class="sxs-lookup"><span data-stu-id="c775c-328">This setting only applies to 64-bit computers.</span></span>
- <span data-ttu-id="c775c-329">如果进程正在具有指定内存限制的容器中运行，则百分比的计算结果将为该内存限制的百分比。</span><span class="sxs-lookup"><span data-stu-id="c775c-329">If the process is running inside a container that has a specified memory limit, the percentage is calculated as a percentage of that memory limit.</span></span>
- <span data-ttu-id="c775c-330">如果已配置[每对象堆限制](#per-object-heap-limits)，则忽略此设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-330">This setting is ignored if the [Per-object-heap limits](#per-object-heap-limits) are configured.</span></span>
- <span data-ttu-id="c775c-331">默认值（仅在某些情况下适用）是 20 MB 或容器内存限制的 75%（以较大者为准）。</span><span class="sxs-lookup"><span data-stu-id="c775c-331">The default value, which only applies in certain cases, is the greater of 20 MB or 75% of the memory limit on the container.</span></span> <span data-ttu-id="c775c-332">此默认值在以下情况下适用：</span><span class="sxs-lookup"><span data-stu-id="c775c-332">The default value applies if:</span></span>

  - <span data-ttu-id="c775c-333">进程正在具有指定内存限制的容器中运行。</span><span class="sxs-lookup"><span data-stu-id="c775c-333">The process is running inside a container that has a specified memory limit.</span></span>
  - <span data-ttu-id="c775c-334">[System.GC.HeapHardLimit](#heap-limit) 未设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-334">[System.GC.HeapHardLimit](#heap-limit) is not set.</span></span>

| | <span data-ttu-id="c775c-335">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-335">Setting name</span></span> | <span data-ttu-id="c775c-336">值</span><span class="sxs-lookup"><span data-stu-id="c775c-336">Values</span></span> | <span data-ttu-id="c775c-337">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-337">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-338">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-338">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimitPercent` | <span data-ttu-id="c775c-339">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-339">*decimal value*</span></span> | <span data-ttu-id="c775c-340">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-340">.NET Core 3.0</span></span> |
| <span data-ttu-id="c775c-341">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-341">**Environment variable**</span></span> | `COMPlus_GCHeapHardLimitPercent` | <span data-ttu-id="c775c-342">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-342">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-343">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-343">.NET Core 3.0</span></span> |

<span data-ttu-id="c775c-344">示例：</span><span class="sxs-lookup"><span data-stu-id="c775c-344">Example:</span></span>

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
> <span data-ttu-id="c775c-345">如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-345">If you're setting the option in *runtimeconfig.json*, specify a decimal value.</span></span> <span data-ttu-id="c775c-346">如果要将选项设置为一个环境变量，请指定一个十六进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-346">If you're setting the option as an environment variable, specify a hexadecimal value.</span></span> <span data-ttu-id="c775c-347">例如，若要将堆使用率限制为 30%，则该值对于 JSON 文件为 30，对于环境变量则为 0x1E 或 1E。</span><span class="sxs-lookup"><span data-stu-id="c775c-347">For example, to limit the heap usage to 30%, the values would be 30 for the JSON file and 0x1E or 1E for the environment variable.</span></span>

### <a name="per-object-heap-limits"></a><span data-ttu-id="c775c-348">每对象堆限制</span><span class="sxs-lookup"><span data-stu-id="c775c-348">Per-object-heap limits</span></span>

<span data-ttu-id="c775c-349">可以根据每个对象堆指定 GC 的允许堆使用量。</span><span class="sxs-lookup"><span data-stu-id="c775c-349">You can specify the GC's allowable heap usage on a per-object-heap basis.</span></span> <span data-ttu-id="c775c-350">不同的堆包括大型对象堆 (LOH)、小型对象堆 (SOH) 和固定对象堆 (POH)。</span><span class="sxs-lookup"><span data-stu-id="c775c-350">The different heaps are the large object heap (LOH), small object heap (SOH), and pinned object heap (POH).</span></span>

- <span data-ttu-id="c775c-351">如果为任何 `COMPLUS_GCHeapHardLimitSOH`、`COMPLUS_GCHeapHardLimitLOH` 或 `COMPLUS_GCHeapHardLimitPOH` 设置指定值，则还必须为 `COMPLUS_GCHeapHardLimitSOH` 和 `COMPLUS_GCHeapHardLimitLOH` 指定值。</span><span class="sxs-lookup"><span data-stu-id="c775c-351">If you specify a value for any of the `COMPLUS_GCHeapHardLimitSOH`, `COMPLUS_GCHeapHardLimitLOH`, or `COMPLUS_GCHeapHardLimitPOH` settings, you must also specify a value for `COMPLUS_GCHeapHardLimitSOH` and `COMPLUS_GCHeapHardLimitLOH`.</span></span> <span data-ttu-id="c775c-352">否则，运行时将无法初始化。</span><span class="sxs-lookup"><span data-stu-id="c775c-352">If you don't, the runtime will fail to initialize.</span></span>
- <span data-ttu-id="c775c-353">`COMPLUS_GCHeapHardLimitPOH` 的默认值为 0。</span><span class="sxs-lookup"><span data-stu-id="c775c-353">The default value for `COMPLUS_GCHeapHardLimitPOH` is 0.</span></span> <span data-ttu-id="c775c-354">`COMPLUS_GCHeapHardLimitSOH` 和 `COMPLUS_GCHeapHardLimitLOH` 没有默认值。</span><span class="sxs-lookup"><span data-stu-id="c775c-354">`COMPLUS_GCHeapHardLimitSOH` and `COMPLUS_GCHeapHardLimitLOH` don't have default values.</span></span>

| | <span data-ttu-id="c775c-355">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-355">Setting name</span></span> | <span data-ttu-id="c775c-356">值</span><span class="sxs-lookup"><span data-stu-id="c775c-356">Values</span></span> | <span data-ttu-id="c775c-357">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-357">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-358">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-358">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimitSOH` | <span data-ttu-id="c775c-359">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-359">*decimal value*</span></span> | <span data-ttu-id="c775c-360">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-360">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-361">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-361">**Environment variable**</span></span> | `COMPLUS_GCHeapHardLimitSOH` | <span data-ttu-id="c775c-362">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-362">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-363">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-363">.NET 5.0</span></span> |

| | <span data-ttu-id="c775c-364">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-364">Setting name</span></span> | <span data-ttu-id="c775c-365">值</span><span class="sxs-lookup"><span data-stu-id="c775c-365">Values</span></span> | <span data-ttu-id="c775c-366">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-366">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-367">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-367">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimitLOH` | <span data-ttu-id="c775c-368">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-368">*decimal value*</span></span> | <span data-ttu-id="c775c-369">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-369">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-370">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-370">**Environment variable**</span></span> | `COMPLUS_GCHeapHardLimitLOH` | <span data-ttu-id="c775c-371">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-371">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-372">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-372">.NET 5.0</span></span> |

| | <span data-ttu-id="c775c-373">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-373">Setting name</span></span> | <span data-ttu-id="c775c-374">值</span><span class="sxs-lookup"><span data-stu-id="c775c-374">Values</span></span> | <span data-ttu-id="c775c-375">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-375">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-376">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-376">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimitPOH` | <span data-ttu-id="c775c-377">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-377">*decimal value*</span></span> | <span data-ttu-id="c775c-378">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-378">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-379">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-379">**Environment variable**</span></span> | `COMPLUS_GCHeapHardLimitPOH` | <span data-ttu-id="c775c-380">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-380">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-381">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-381">.NET 5.0</span></span> |

> [!TIP]
> <span data-ttu-id="c775c-382">如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-382">If you're setting the option in *runtimeconfig.json*, specify a decimal value.</span></span> <span data-ttu-id="c775c-383">如果要将选项设置为一个环境变量，请指定一个十六进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-383">If you're setting the option as an environment variable, specify a hexadecimal value.</span></span> <span data-ttu-id="c775c-384">例如，若要将堆硬限制指定为 200 个兆字节 (MiB)，则该值对于 JSON 文件为 209715200，对于环境变量则为 0xC800000 或 C800000。</span><span class="sxs-lookup"><span data-stu-id="c775c-384">For example, to specify a heap hard limit of 200 mebibytes (MiB), the values would be 209715200 for the JSON file and 0xC800000 or C800000 for the environment variable.</span></span>

### <a name="per-object-heap-limit-percents"></a><span data-ttu-id="c775c-385">每对象堆限制百分比</span><span class="sxs-lookup"><span data-stu-id="c775c-385">Per-object-heap limit percents</span></span>

<span data-ttu-id="c775c-386">可以根据每个对象堆指定 GC 的允许堆使用量。</span><span class="sxs-lookup"><span data-stu-id="c775c-386">You can specify the GC's allowable heap usage on a per-object-heap basis.</span></span> <span data-ttu-id="c775c-387">不同的堆包括大型对象堆 (LOH)、小型对象堆 (SOH) 和固定对象堆 (POH)。</span><span class="sxs-lookup"><span data-stu-id="c775c-387">The different heaps are the large object heap (LOH), small object heap (SOH), and pinned object heap (POH).</span></span>

- <span data-ttu-id="c775c-388">如果为任何 `COMPLUS_GCHeapHardLimitSOHPercent`、`COMPLUS_GCHeapHardLimitLOHPercent` 或 `COMPLUS_GCHeapHardLimitPOHPercent` 设置指定值，则还必须为 `COMPLUS_GCHeapHardLimitSOHPercent` 和 `COMPLUS_GCHeapHardLimitLOHPercent` 指定值。</span><span class="sxs-lookup"><span data-stu-id="c775c-388">If you specify a value for any of the `COMPLUS_GCHeapHardLimitSOHPercent`, `COMPLUS_GCHeapHardLimitLOHPercent`, or `COMPLUS_GCHeapHardLimitPOHPercent` settings, you must also specify a value for `COMPLUS_GCHeapHardLimitSOHPercent` and `COMPLUS_GCHeapHardLimitLOHPercent`.</span></span> <span data-ttu-id="c775c-389">否则，运行时将无法初始化。</span><span class="sxs-lookup"><span data-stu-id="c775c-389">If you don't, the runtime will fail to initialize.</span></span>
- <span data-ttu-id="c775c-390">如果指定了 `COMPLUS_GCHeapHardLimitSOH`、`COMPLUS_GCHeapHardLimitLOH` 和 `COMPLUS_GCHeapHardLimitPOH`，则忽略这些设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-390">These settings are ignored if `COMPLUS_GCHeapHardLimitSOH`, `COMPLUS_GCHeapHardLimitLOH`, and `COMPLUS_GCHeapHardLimitPOH` are specified.</span></span>
- <span data-ttu-id="c775c-391">值为 1 表示 GC 使用该对象堆的总物理内存的 1%。</span><span class="sxs-lookup"><span data-stu-id="c775c-391">A value of 1 means that GC uses 1% of total physical memory for that object heap.</span></span>
- <span data-ttu-id="c775c-392">每个值都必须大于 0 并小于 100。</span><span class="sxs-lookup"><span data-stu-id="c775c-392">Each value must be greater than zero and less than 100.</span></span> <span data-ttu-id="c775c-393">此外，3 个百分比值的总和必须小于 100。</span><span class="sxs-lookup"><span data-stu-id="c775c-393">Additionally, the sum of the three percentage values must be less than 100.</span></span> <span data-ttu-id="c775c-394">否则，运行时将无法初始化。</span><span class="sxs-lookup"><span data-stu-id="c775c-394">Otherwise, the runtime will fail to initialize.</span></span>

| | <span data-ttu-id="c775c-395">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-395">Setting name</span></span> | <span data-ttu-id="c775c-396">值</span><span class="sxs-lookup"><span data-stu-id="c775c-396">Values</span></span> | <span data-ttu-id="c775c-397">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-397">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-398">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-398">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimitSOHPercent` | <span data-ttu-id="c775c-399">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-399">*decimal value*</span></span> | <span data-ttu-id="c775c-400">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-400">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-401">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-401">**Environment variable**</span></span> | `COMPLUS_GCHeapHardLimitSOHPercent` | <span data-ttu-id="c775c-402">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-402">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-403">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-403">.NET 5.0</span></span> |

| | <span data-ttu-id="c775c-404">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-404">Setting name</span></span> | <span data-ttu-id="c775c-405">值</span><span class="sxs-lookup"><span data-stu-id="c775c-405">Values</span></span> | <span data-ttu-id="c775c-406">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-406">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-407">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-407">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimitLOHPercent` | <span data-ttu-id="c775c-408">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-408">*decimal value*</span></span> | <span data-ttu-id="c775c-409">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-409">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-410">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-410">**Environment variable**</span></span> | `COMPLUS_GCHeapHardLimitLOHPercent` | <span data-ttu-id="c775c-411">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-411">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-412">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-412">.NET 5.0</span></span> |

| | <span data-ttu-id="c775c-413">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-413">Setting name</span></span> | <span data-ttu-id="c775c-414">值</span><span class="sxs-lookup"><span data-stu-id="c775c-414">Values</span></span> | <span data-ttu-id="c775c-415">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-415">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-416">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-416">**runtimeconfig.json**</span></span> | `System.GC.HeapHardLimitPOHPercent` | <span data-ttu-id="c775c-417">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-417">*decimal value*</span></span> | <span data-ttu-id="c775c-418">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-418">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-419">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-419">**Environment variable**</span></span> | `COMPLUS_GCHeapHardLimitPOHPercent` | <span data-ttu-id="c775c-420">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-420">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-421">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-421">.NET 5.0</span></span> |

> [!TIP]
> <span data-ttu-id="c775c-422">如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-422">If you're setting the option in *runtimeconfig.json*, specify a decimal value.</span></span> <span data-ttu-id="c775c-423">如果要将选项设置为一个环境变量，请指定一个十六进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-423">If you're setting the option as an environment variable, specify a hexadecimal value.</span></span> <span data-ttu-id="c775c-424">例如，若要将堆使用率限制为 30%，则该值对于 JSON 文件为 30，对于环境变量则为 0x1E 或 1E。</span><span class="sxs-lookup"><span data-stu-id="c775c-424">For example, to limit the heap usage to 30%, the values would be 30 for the JSON file and 0x1E or 1E for the environment variable.</span></span>

### <a name="high-memory-percent"></a><span data-ttu-id="c775c-425">高内存百分比</span><span class="sxs-lookup"><span data-stu-id="c775c-425">High memory percent</span></span>

<span data-ttu-id="c775c-426">内存负载由正在使用的物理内存的百分比表示。</span><span class="sxs-lookup"><span data-stu-id="c775c-426">Memory load is indicated by the percentage of physical memory in use.</span></span> <span data-ttu-id="c775c-427">默认情况下，当物理内存负载达到 90%时，垃圾回收对于执行完整的压缩垃圾回收变得更加积极，以避免分页。</span><span class="sxs-lookup"><span data-stu-id="c775c-427">By default, when the physical memory load reaches **90%**, garbage collection becomes more aggressive about doing full, compacting garbage collections to avoid paging.</span></span> <span data-ttu-id="c775c-428">当内存负载低于 90% 时，GC 优先使用后台回收进行完整的垃圾回收，这种方法的暂停时间较短，但不会使堆的总大小减少太多。</span><span class="sxs-lookup"><span data-stu-id="c775c-428">When memory load is below 90%, GC favors background collections for full garbage collections, which have shorter pauses but don't reduce the total heap size by much.</span></span> <span data-ttu-id="c775c-429">在具有大量内存（80 GB 或更多）的计算机上，默认负载阈值介于 90% 到 97% 之间。</span><span class="sxs-lookup"><span data-stu-id="c775c-429">On machines with a significant amount of memory (80GB or more), the default load threshold is between 90% and 97%.</span></span>

<span data-ttu-id="c775c-430">可以通过 `COMPlus_GCHighMemPercent` 环境变量或 `System.GC.HighMemoryPercent` JSON 配置设置来调整高内存负载阈值。</span><span class="sxs-lookup"><span data-stu-id="c775c-430">The high memory load threshold can be adjusted by the `COMPlus_GCHighMemPercent` environment variable or `System.GC.HighMemoryPercent` JSON configuration setting.</span></span> <span data-ttu-id="c775c-431">如果要控制堆大小，请考虑调整阈值。</span><span class="sxs-lookup"><span data-stu-id="c775c-431">Consider adjusting the threshold if you want to control heap size.</span></span> <span data-ttu-id="c775c-432">例如，对于具有 64 GB 内存的计算机上的主要进程，当有 10% 的可用内存时，GC 开始响应是合理的。</span><span class="sxs-lookup"><span data-stu-id="c775c-432">For example, for the dominant process on a machine with 64GB of memory, it's reasonable for GC to start reacting when there's 10% of memory available.</span></span> <span data-ttu-id="c775c-433">但是对于较小的进程（例如，仅消耗 1GB 内存的进程），GC 可以在可用内存少于 10% 的情况下轻松地运行。</span><span class="sxs-lookup"><span data-stu-id="c775c-433">But for smaller processes, for example, a process that only consumes 1GB of memory, GC can comfortably run with less than 10% of memory available.</span></span> <span data-ttu-id="c775c-434">对于这些较小的进程，请考虑将阈值设置得更高。</span><span class="sxs-lookup"><span data-stu-id="c775c-434">For these smaller processes, consider setting the threshold higher.</span></span> <span data-ttu-id="c775c-435">另一方面，如果你希望较大的进程具有较小的堆大小（即使有足够的物理内存可用），要使 GC 更快做出反应以缩小堆大小，则降低此阈值是一种有效的方法。</span><span class="sxs-lookup"><span data-stu-id="c775c-435">On the other hand, if you want larger processes to have smaller heap sizes (even when there's plenty of physical memory available), lowering this threshold is an effective way for GC to react sooner to compact the heap down.</span></span>

> [!NOTE]
> <span data-ttu-id="c775c-436">对于在容器中运行的进程，GC 将根据容器限制考虑物理内存。</span><span class="sxs-lookup"><span data-stu-id="c775c-436">For processes running in a container, GC considers the physical memory based on the container limit.</span></span>

| | <span data-ttu-id="c775c-437">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-437">Setting name</span></span> | <span data-ttu-id="c775c-438">值</span><span class="sxs-lookup"><span data-stu-id="c775c-438">Values</span></span> | <span data-ttu-id="c775c-439">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-439">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-440">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-440">**runtimeconfig.json**</span></span> | `System.GC.HighMemoryPercent` | <span data-ttu-id="c775c-441">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-441">*decimal value*</span></span> | <span data-ttu-id="c775c-442">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="c775c-442">.NET 5.0</span></span> |
| <span data-ttu-id="c775c-443">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-443">**Environment variable**</span></span> | `COMPlus_GCHighMemPercent` | <span data-ttu-id="c775c-444">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-444">*hexadecimal value*</span></span> | |

> [!TIP]
> <span data-ttu-id="c775c-445">如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-445">If you're setting the option in *runtimeconfig.json*, specify a decimal value.</span></span> <span data-ttu-id="c775c-446">如果要将选项设置为一个环境变量，请指定一个十六进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-446">If you're setting the option as an environment variable, specify a hexadecimal value.</span></span> <span data-ttu-id="c775c-447">例如，要将高内存阈值设置为 75%，JSON 文件的值将为 75，而环境变量的值为 0x4B 或 4B。</span><span class="sxs-lookup"><span data-stu-id="c775c-447">For example, to set the high memory threshold to 75%, the values would be 75 for the JSON file and 0x4B or 4B for the environment variable.</span></span>

### <a name="retain-vm"></a><span data-ttu-id="c775c-448">保留 VM</span><span class="sxs-lookup"><span data-stu-id="c775c-448">Retain VM</span></span>

- <span data-ttu-id="c775c-449">配置是将应删除的段置于备用列表上供将来使用，还是将其释放回操作系统 (OS)。</span><span class="sxs-lookup"><span data-stu-id="c775c-449">Configures whether segments that should be deleted are put on a standby list for future use or are released back to the operating system (OS).</span></span>
- <span data-ttu-id="c775c-450">默认：将段释放回操作系统。</span><span class="sxs-lookup"><span data-stu-id="c775c-450">Default: Release segments back to the operating system.</span></span> <span data-ttu-id="c775c-451">它等效于将值设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="c775c-451">This is equivalent to setting the value to `false`.</span></span>

| | <span data-ttu-id="c775c-452">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-452">Setting name</span></span> | <span data-ttu-id="c775c-453">值</span><span class="sxs-lookup"><span data-stu-id="c775c-453">Values</span></span> | <span data-ttu-id="c775c-454">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-454">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-455">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-455">**runtimeconfig.json**</span></span> | `System.GC.RetainVM` | <span data-ttu-id="c775c-456">`false` - 释放到 OS</span><span class="sxs-lookup"><span data-stu-id="c775c-456">`false` - release to OS</span></span><br/><span data-ttu-id="c775c-457">`true` - 置于备用列表上</span><span class="sxs-lookup"><span data-stu-id="c775c-457">`true` - put on standby</span></span> | <span data-ttu-id="c775c-458">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-458">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-459">MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="c775c-459">**MSBuild property**</span></span> | `RetainVMGarbageCollection` | <span data-ttu-id="c775c-460">`false` - 释放到 OS</span><span class="sxs-lookup"><span data-stu-id="c775c-460">`false` - release to OS</span></span><br/><span data-ttu-id="c775c-461">`true` - 置于备用列表上</span><span class="sxs-lookup"><span data-stu-id="c775c-461">`true` - put on standby</span></span> | <span data-ttu-id="c775c-462">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-462">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-463">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-463">**Environment variable**</span></span> | `COMPlus_GCRetainVM` | <span data-ttu-id="c775c-464">`0` - 释放到 OS</span><span class="sxs-lookup"><span data-stu-id="c775c-464">`0` - release to OS</span></span><br/><span data-ttu-id="c775c-465">`1` - 置于备用列表上</span><span class="sxs-lookup"><span data-stu-id="c775c-465">`1` - put on standby</span></span> | <span data-ttu-id="c775c-466">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-466">.NET Core 1.0</span></span> |

#### <a name="examples"></a><span data-ttu-id="c775c-467">示例</span><span class="sxs-lookup"><span data-stu-id="c775c-467">Examples</span></span>

<span data-ttu-id="c775c-468">runtimeconfig.json 文件：</span><span class="sxs-lookup"><span data-stu-id="c775c-468">*runtimeconfig.json* file:</span></span>

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.RetainVM": true
      }
   }
}
```

<span data-ttu-id="c775c-469">项目文件：</span><span class="sxs-lookup"><span data-stu-id="c775c-469">Project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="large-pages"></a><span data-ttu-id="c775c-470">大型页面</span><span class="sxs-lookup"><span data-stu-id="c775c-470">Large pages</span></span>

- <span data-ttu-id="c775c-471">指定设置堆硬限制时是否应使用大型页面。</span><span class="sxs-lookup"><span data-stu-id="c775c-471">Specifies whether large pages should be used when a heap hard limit is set.</span></span>
- <span data-ttu-id="c775c-472">默认：设置堆硬限制时不要使用大页面。</span><span class="sxs-lookup"><span data-stu-id="c775c-472">Default: Don't use large pages when a heap hard limit is set.</span></span> <span data-ttu-id="c775c-473">它等效于将值设置为 `0`。</span><span class="sxs-lookup"><span data-stu-id="c775c-473">This is equivalent to setting the value to `0`.</span></span>
- <span data-ttu-id="c775c-474">这是一项实验性设置。</span><span class="sxs-lookup"><span data-stu-id="c775c-474">This is an experimental setting.</span></span>

| | <span data-ttu-id="c775c-475">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-475">Setting name</span></span> | <span data-ttu-id="c775c-476">值</span><span class="sxs-lookup"><span data-stu-id="c775c-476">Values</span></span> | <span data-ttu-id="c775c-477">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-477">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-478">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-478">**runtimeconfig.json**</span></span> | <span data-ttu-id="c775c-479">不可用</span><span class="sxs-lookup"><span data-stu-id="c775c-479">N/A</span></span> | <span data-ttu-id="c775c-480">空值</span><span class="sxs-lookup"><span data-stu-id="c775c-480">N/A</span></span> | <span data-ttu-id="c775c-481">不可用</span><span class="sxs-lookup"><span data-stu-id="c775c-481">N/A</span></span> |
| <span data-ttu-id="c775c-482">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-482">**Environment variable**</span></span> | `COMPlus_GCLargePages` | <span data-ttu-id="c775c-483">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="c775c-483">`0` - disabled</span></span><br/><span data-ttu-id="c775c-484">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="c775c-484">`1` - enabled</span></span> | <span data-ttu-id="c775c-485">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c775c-485">.NET Core 3.0</span></span> |

## <a name="allow-large-objects"></a><span data-ttu-id="c775c-486">允许大型对象</span><span class="sxs-lookup"><span data-stu-id="c775c-486">Allow large objects</span></span>

- <span data-ttu-id="c775c-487">在 64 位平台上，为总大小大于 2 千兆字节 (GB) 的数组配置垃圾回收器支持。</span><span class="sxs-lookup"><span data-stu-id="c775c-487">Configures garbage collector support on 64-bit platforms for arrays that are greater than 2 gigabytes (GB) in total size.</span></span>
- <span data-ttu-id="c775c-488">默认：垃圾回收支持大于 2 GB 的数组。</span><span class="sxs-lookup"><span data-stu-id="c775c-488">Default: GC supports arrays greater than 2-GB.</span></span> <span data-ttu-id="c775c-489">它等效于将值设置为 `1`。</span><span class="sxs-lookup"><span data-stu-id="c775c-489">This is equivalent to setting the value to `1`.</span></span>
- <span data-ttu-id="c775c-490">在未来的 .NET 版本中，此选项可能会过时。</span><span class="sxs-lookup"><span data-stu-id="c775c-490">This option may become obsolete in a future version of .NET.</span></span>

| | <span data-ttu-id="c775c-491">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-491">Setting name</span></span> | <span data-ttu-id="c775c-492">值</span><span class="sxs-lookup"><span data-stu-id="c775c-492">Values</span></span> | <span data-ttu-id="c775c-493">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-493">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-494">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-494">**runtimeconfig.json**</span></span> | <span data-ttu-id="c775c-495">不可用</span><span class="sxs-lookup"><span data-stu-id="c775c-495">N/A</span></span> | <span data-ttu-id="c775c-496">空值</span><span class="sxs-lookup"><span data-stu-id="c775c-496">N/A</span></span> | <span data-ttu-id="c775c-497">不可用</span><span class="sxs-lookup"><span data-stu-id="c775c-497">N/A</span></span> |
| <span data-ttu-id="c775c-498">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-498">**Environment variable**</span></span> | `COMPlus_gcAllowVeryLargeObjects` | <span data-ttu-id="c775c-499">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="c775c-499">`1` - enabled</span></span><br/> <span data-ttu-id="c775c-500">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="c775c-500">`0` - disabled</span></span> | <span data-ttu-id="c775c-501">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-501">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-502">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-502">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-503">gcAllowVeryLargeObjects</span><span class="sxs-lookup"><span data-stu-id="c775c-503">gcAllowVeryLargeObjects</span></span>](../../framework/configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md) | <span data-ttu-id="c775c-504">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="c775c-504">`1` - enabled</span></span><br/> <span data-ttu-id="c775c-505">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="c775c-505">`0` - disabled</span></span> | <span data-ttu-id="c775c-506">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="c775c-506">.NET Framework 4.5</span></span> |

## <a name="large-object-heap-threshold"></a><span data-ttu-id="c775c-507">大型对象堆阈值</span><span class="sxs-lookup"><span data-stu-id="c775c-507">Large object heap threshold</span></span>

- <span data-ttu-id="c775c-508">指定导致对象进入大型对象堆 (LOH) 的阈值大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="c775c-508">Specifies the threshold size, in bytes, that causes objects to go on the large object heap (LOH).</span></span>
- <span data-ttu-id="c775c-509">默认阈值为 85,000 字节。</span><span class="sxs-lookup"><span data-stu-id="c775c-509">The default threshold is 85,000 bytes.</span></span>
- <span data-ttu-id="c775c-510">指定的值必须大于默认阈值。</span><span class="sxs-lookup"><span data-stu-id="c775c-510">The value you specify must be larger than the default threshold.</span></span>

| | <span data-ttu-id="c775c-511">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-511">Setting name</span></span> | <span data-ttu-id="c775c-512">值</span><span class="sxs-lookup"><span data-stu-id="c775c-512">Values</span></span> | <span data-ttu-id="c775c-513">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-513">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-514">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-514">**runtimeconfig.json**</span></span> | `System.GC.LOHThreshold` | <span data-ttu-id="c775c-515">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-515">*decimal value*</span></span> | <span data-ttu-id="c775c-516">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-516">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-517">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-517">**Environment variable**</span></span> | `COMPlus_GCLOHThreshold` | <span data-ttu-id="c775c-518">十六进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-518">*hexadecimal value*</span></span> | <span data-ttu-id="c775c-519">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="c775c-519">.NET Core 1.0</span></span> |
| <span data-ttu-id="c775c-520">**.NET Framework 的 app.config**</span><span class="sxs-lookup"><span data-stu-id="c775c-520">**app.config for .NET Framework**</span></span> | [<span data-ttu-id="c775c-521">GCLOHThreshold</span><span class="sxs-lookup"><span data-stu-id="c775c-521">GCLOHThreshold</span></span>](../../framework/configure-apps/file-schema/runtime/gclohthreshold-element.md) | <span data-ttu-id="c775c-522">十进制值</span><span class="sxs-lookup"><span data-stu-id="c775c-522">*decimal value*</span></span> | <span data-ttu-id="c775c-523">.NET Framework 4.8</span><span class="sxs-lookup"><span data-stu-id="c775c-523">.NET Framework 4.8</span></span> |

<span data-ttu-id="c775c-524">示例：</span><span class="sxs-lookup"><span data-stu-id="c775c-524">Example:</span></span>

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
> <span data-ttu-id="c775c-525">如果要在 runtimeconfig.template.json 中设置该选项，请指定一个十进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-525">If you're setting the option in *runtimeconfig.json*, specify a decimal value.</span></span> <span data-ttu-id="c775c-526">如果要将选项设置为一个环境变量，请指定一个十六进制值。</span><span class="sxs-lookup"><span data-stu-id="c775c-526">If you're setting the option as an environment variable, specify a hexadecimal value.</span></span> <span data-ttu-id="c775c-527">例如，若要将阙值大小设置为 120,000 个字节，则该值对于 JSON 文件为 120,000，对于环境变量则为 0x1D4C0 或 1D4C0。</span><span class="sxs-lookup"><span data-stu-id="c775c-527">For example, to set a threshold size of 120,000 bytes, the values would be 120000 for the JSON file and 0x1D4C0 or 1D4C0 for the environment variable.</span></span>

## <a name="standalone-gc"></a><span data-ttu-id="c775c-528">独立 GC</span><span class="sxs-lookup"><span data-stu-id="c775c-528">Standalone GC</span></span>

- <span data-ttu-id="c775c-529">指定库的路径，该库包含运行时打算加载的垃圾回收器。</span><span class="sxs-lookup"><span data-stu-id="c775c-529">Specifies a path to the library containing the garbage collector that the runtime intends to load.</span></span>
- <span data-ttu-id="c775c-530">有关详细信息，请参阅 [Standalone GC loader design](https://github.com/dotnet/runtime/blob/main/docs/design/features/standalone-gc-loading.md)（独立 GC 加载程序设计）。</span><span class="sxs-lookup"><span data-stu-id="c775c-530">For more information, see [Standalone GC loader design](https://github.com/dotnet/runtime/blob/main/docs/design/features/standalone-gc-loading.md).</span></span>

| | <span data-ttu-id="c775c-531">设置名</span><span class="sxs-lookup"><span data-stu-id="c775c-531">Setting name</span></span> | <span data-ttu-id="c775c-532">值</span><span class="sxs-lookup"><span data-stu-id="c775c-532">Values</span></span> | <span data-ttu-id="c775c-533">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c775c-533">Version introduced</span></span> |
| - | - | - | - |
| <span data-ttu-id="c775c-534">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="c775c-534">**runtimeconfig.json**</span></span> | <span data-ttu-id="c775c-535">不可用</span><span class="sxs-lookup"><span data-stu-id="c775c-535">N/A</span></span> | <span data-ttu-id="c775c-536">空值</span><span class="sxs-lookup"><span data-stu-id="c775c-536">N/A</span></span> | <span data-ttu-id="c775c-537">不可用</span><span class="sxs-lookup"><span data-stu-id="c775c-537">N/A</span></span> |
| <span data-ttu-id="c775c-538">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="c775c-538">**Environment variable**</span></span> | `COMPlus_GCName` | <span data-ttu-id="c775c-539">string_path</span><span class="sxs-lookup"><span data-stu-id="c775c-539">*string_path*</span></span> | <span data-ttu-id="c775c-540">.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="c775c-540">.NET Core 2.0</span></span> |
