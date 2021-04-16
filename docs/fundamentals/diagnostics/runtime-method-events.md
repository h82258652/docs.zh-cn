---
title: 方法运行时事件
description: 查看收集特定于方法的诊断信息的 .NET 运行时事件，如 CLR 方法事件、CLR 方法标记或 CLR 方法详细事件，以及 MethodJittingStarted。
ms.date: 11/13/2020
helpviewer_keywords:
- Method events (CoreCLR)
- ETW, EventPipe, LTTng method events (CoreCLR)
ms.openlocfilehash: f9d08efa420670cf7a8c863f115ff270998f2dca
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591056"
---
# <a name="net-runtime-method-events"></a><span data-ttu-id="636f9-103">.NET 运行时方法事件</span><span class="sxs-lookup"><span data-stu-id="636f9-103">.NET runtime method events</span></span>

<span data-ttu-id="636f9-104">这些事件收集特定于方法的信息。</span><span class="sxs-lookup"><span data-stu-id="636f9-104">These events collect information that is specific to methods.</span></span> <span data-ttu-id="636f9-105">符号解析需要这些事件的负载。</span><span class="sxs-lookup"><span data-stu-id="636f9-105">The payload of these events is required for symbol resolution.</span></span> <span data-ttu-id="636f9-106">此外，这些事件还提供了一些有用的信息，例如已加载和已卸载的方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-106">In addition, these events provide helpful information such as methods that are loaded and unloaded.</span></span> <span data-ttu-id="636f9-107">有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="636f9-107">For more information about how to use these events for diagnostic purposes, see [logging and tracing .NET applications](../../core/diagnostics/logging-tracing.md)</span></span>

<span data-ttu-id="636f9-108">所有方法事件都具有“信息性 (4)”级别。</span><span class="sxs-lookup"><span data-stu-id="636f9-108">All method events have a level of "Informational (4)".</span></span> <span data-ttu-id="636f9-109">所有方法的详细事件都具有“详细级别 (5)”级别。</span><span class="sxs-lookup"><span data-stu-id="636f9-109">All method verbose events have a level of "Verbose (5)".</span></span>

<span data-ttu-id="636f9-110">所有方法事件都是由运行时提供程序下的 `JITKeyword` (0x10) 关键字或 `NGenKeyword` (0x20) 关键字引发的，或是由断开提供程序下的 `JitRundownKeyword` (0x10) 或 `NGENRundownKeyword` (0x20) 引发的。</span><span class="sxs-lookup"><span data-stu-id="636f9-110">All method events are raised by the `JITKeyword` (0x10) keyword or the `NGenKeyword` (0x20) keyword under the runtime provider, or `JitRundownKeyword` (0x10) or `NGENRundownKeyword` (0x20) under the rundown provider.</span></span>

<span data-ttu-id="636f9-111">这些事件的 V2 版本包括 ReJITID（V1 版本不包含）。</span><span class="sxs-lookup"><span data-stu-id="636f9-111">The V2 versions of these events include the ReJITID, the V1 versions do not.</span></span>

## <a name="methodload_v1-event"></a><span data-ttu-id="636f9-112">MethodLoad_V1 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-112">MethodLoad_V1 event</span></span>

<span data-ttu-id="636f9-113">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="636f9-113">The following table shows the event information:</span></span>

|<span data-ttu-id="636f9-114">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-114">Event</span></span>|<span data-ttu-id="636f9-115">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-115">Event ID</span></span>|<span data-ttu-id="636f9-116">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-116">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodLoad_V1`|<span data-ttu-id="636f9-117">141</span><span class="sxs-lookup"><span data-stu-id="636f9-117">141</span></span>|<span data-ttu-id="636f9-118">在实时加载（JIT 加载）方法或者加载 NGEN 映像时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-118">Raised when a method is just-in-time loaded (JIT-loaded) or an NGEN image is loaded.</span></span> <span data-ttu-id="636f9-119">动态和泛型方法不使用此版本进行方法加载。</span><span class="sxs-lookup"><span data-stu-id="636f9-119">Dynamic and generic methods do not use this version for method loads.</span></span> <span data-ttu-id="636f9-120">JIT 帮助器从不使用此版本。</span><span class="sxs-lookup"><span data-stu-id="636f9-120">JIT helpers never use this version.</span></span>|

|<span data-ttu-id="636f9-121">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-121">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-122">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-122">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-123">`JITKeyword` (0x10) 运行时提供程序</span><span class="sxs-lookup"><span data-stu-id="636f9-123">`JITKeyword` (0x10) runtime provider</span></span>|<span data-ttu-id="636f9-124">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-124">Informational (4)</span></span>|
|<span data-ttu-id="636f9-125">`NGenKeyword` (0x20) 运行时提供程序</span><span class="sxs-lookup"><span data-stu-id="636f9-125">`NGenKeyword` (0x20) runtime provider</span></span>|<span data-ttu-id="636f9-126">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-126">Informational (4)</span></span>|

|<span data-ttu-id="636f9-127">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-127">Field name</span></span>|<span data-ttu-id="636f9-128">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-128">Data type</span></span>|<span data-ttu-id="636f9-129">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-129">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-130">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-130">Unique identifier of a method.</span></span> <span data-ttu-id="636f9-131">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-131">For JIT helper methods, this is set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-132">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-132">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-133">方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-133">Start address of the method.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-134">方法的大小。</span><span class="sxs-lookup"><span data-stu-id="636f9-134">Size of the method.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-135">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-135">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-136">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-136">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-137">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-137">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-138">0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。</span><span class="sxs-lookup"><span data-stu-id="636f9-138">0x4: JIT-compiled code method (otherwise NGEN native image code).</span></span><br /><br /> <span data-ttu-id="636f9-139">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-139">0x8: Helper method.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-140">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-140">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodload_v2-event"></a><span data-ttu-id="636f9-141">MethodLoad_V2 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-141">MethodLoad_V2 event</span></span>

|<span data-ttu-id="636f9-142">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-142">Event</span></span>|<span data-ttu-id="636f9-143">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-143">Event ID</span></span>|<span data-ttu-id="636f9-144">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-144">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodLoad_V2`|<span data-ttu-id="636f9-145">141</span><span class="sxs-lookup"><span data-stu-id="636f9-145">141</span></span>|<span data-ttu-id="636f9-146">在实时加载（JIT 加载）方法或者加载 NGEN 映像时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-146">Raised when a method is just-in-time loaded (JIT-loaded) or an NGEN image is loaded.</span></span> <span data-ttu-id="636f9-147">动态和泛型方法不使用此版本进行方法加载。</span><span class="sxs-lookup"><span data-stu-id="636f9-147">Dynamic and generic methods do not use this version for method loads.</span></span> <span data-ttu-id="636f9-148">JIT 帮助器从不使用此版本。</span><span class="sxs-lookup"><span data-stu-id="636f9-148">JIT helpers never use this version.</span></span>|

|<span data-ttu-id="636f9-149">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-149">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-150">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-150">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-151">`JITKeyword` (0x10) 运行时提供程序</span><span class="sxs-lookup"><span data-stu-id="636f9-151">`JITKeyword` (0x10) runtime provider</span></span>|<span data-ttu-id="636f9-152">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-152">Informational (4)</span></span>|
|<span data-ttu-id="636f9-153">`NGenKeyword` (0x20) 运行时提供程序</span><span class="sxs-lookup"><span data-stu-id="636f9-153">`NGenKeyword` (0x20) runtime provider</span></span>|<span data-ttu-id="636f9-154">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-154">Informational (4)</span></span>|

|<span data-ttu-id="636f9-155">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-155">Field name</span></span>|<span data-ttu-id="636f9-156">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-156">Data type</span></span>|<span data-ttu-id="636f9-157">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-157">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-158">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-158">Unique identifier of a method.</span></span> <span data-ttu-id="636f9-159">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-159">For JIT helper methods, this is set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-160">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-160">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-161">方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-161">Start address of the method.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-162">方法的大小。</span><span class="sxs-lookup"><span data-stu-id="636f9-162">Size of the method.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-163">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-163">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-164">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-164">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-165">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-165">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-166">0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。</span><span class="sxs-lookup"><span data-stu-id="636f9-166">0x4: JIT-compiled code method (otherwise NGEN native image code).</span></span><br /><br /> <span data-ttu-id="636f9-167">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-167">0x8: Helper method.</span></span>|
|`ReJITID`|`win:UInt64`|<span data-ttu-id="636f9-168">方法的 ReJIT ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-168">ReJIT ID of the method.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-169">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-169">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodunload_v1-event"></a><span data-ttu-id="636f9-170">MethodUnLoad_V1 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-170">MethodUnLoad_V1 event</span></span>

|<span data-ttu-id="636f9-171">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-171">Event</span></span>|<span data-ttu-id="636f9-172">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-172">Event ID</span></span>|<span data-ttu-id="636f9-173">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-173">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodUnLoad_V1`|<span data-ttu-id="636f9-174">142</span><span class="sxs-lookup"><span data-stu-id="636f9-174">142</span></span>|<span data-ttu-id="636f9-175">在卸载模块或销毁应用程序域时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-175">Raised when a module is unloaded, or an application domain is destroyed.</span></span> <span data-ttu-id="636f9-176">动态方法从不使用此版本进行方法卸载。</span><span class="sxs-lookup"><span data-stu-id="636f9-176">Dynamic methods never use this version for method unloads.</span></span>|

|<span data-ttu-id="636f9-177">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-177">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-178">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-178">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-179">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-179">`JITKeyword` (0x10)</span></span>|<span data-ttu-id="636f9-180">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-180">Informational (4)</span></span>|
|<span data-ttu-id="636f9-181">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-181">`NGenKeyword` (0x20)</span></span>|<span data-ttu-id="636f9-182">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-182">Informational (4)</span></span>|

|<span data-ttu-id="636f9-183">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-183">Field name</span></span>|<span data-ttu-id="636f9-184">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-184">Data type</span></span>|<span data-ttu-id="636f9-185">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-185">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-186">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-186">Unique identifier of a method.</span></span> <span data-ttu-id="636f9-187">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-187">For JIT helper methods, this is set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-188">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-188">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-189">方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-189">Start address of the method.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-190">方法的大小。</span><span class="sxs-lookup"><span data-stu-id="636f9-190">Size of the method.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-191">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-191">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-192">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-192">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-193">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-193">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-194">0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。</span><span class="sxs-lookup"><span data-stu-id="636f9-194">0x4: JIT-compiled code method (otherwise NGEN native image code).</span></span><br /><br /> <span data-ttu-id="636f9-195">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-195">0x8: Helper method.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-196">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-196">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodunload_v2-event"></a><span data-ttu-id="636f9-197">MethodUnLoad_V2 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-197">MethodUnLoad_V2 event</span></span>

|<span data-ttu-id="636f9-198">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-198">Event</span></span>|<span data-ttu-id="636f9-199">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-199">Event ID</span></span>|<span data-ttu-id="636f9-200">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-200">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodUnLoad_V2`|<span data-ttu-id="636f9-201">142</span><span class="sxs-lookup"><span data-stu-id="636f9-201">142</span></span>|<span data-ttu-id="636f9-202">在卸载模块或销毁应用程序域时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-202">Raised when a module is unloaded, or an application domain is destroyed.</span></span> <span data-ttu-id="636f9-203">动态方法从不使用此版本进行方法卸载。</span><span class="sxs-lookup"><span data-stu-id="636f9-203">Dynamic methods never use this version for method unloads.</span></span>|

|<span data-ttu-id="636f9-204">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-204">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-205">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-205">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-206">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-206">`JITKeyword` (0x10)</span></span>|<span data-ttu-id="636f9-207">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-207">Informational (4)</span></span>|
|<span data-ttu-id="636f9-208">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-208">`NGenKeyword` (0x20)</span></span>|<span data-ttu-id="636f9-209">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-209">Informational (4)</span></span>|

|<span data-ttu-id="636f9-210">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-210">Field name</span></span>|<span data-ttu-id="636f9-211">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-211">Data type</span></span>|<span data-ttu-id="636f9-212">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-212">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-213">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-213">Unique identifier of a method.</span></span> <span data-ttu-id="636f9-214">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-214">For JIT helper methods, this is set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-215">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-215">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-216">方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-216">Start address of the method.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-217">方法的大小。</span><span class="sxs-lookup"><span data-stu-id="636f9-217">Size of the method.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-218">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-218">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-219">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-219">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-220">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-220">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-221">0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。</span><span class="sxs-lookup"><span data-stu-id="636f9-221">0x4: JIT-compiled code method (otherwise NGEN native image code).</span></span><br /><br /> <span data-ttu-id="636f9-222">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-222">0x8: Helper method.</span></span>|
|`ReJITID`|`win:UInt64`|<span data-ttu-id="636f9-223">方法的 ReJIT ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-223">ReJIT ID of the method.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-224">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-224">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="r2rgetentrypoint-event"></a><span data-ttu-id="636f9-225">R2RGetEntryPoint 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-225">R2RGetEntryPoint event</span></span>

|<span data-ttu-id="636f9-226">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-226">Event</span></span>|<span data-ttu-id="636f9-227">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-227">Event ID</span></span>|<span data-ttu-id="636f9-228">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-228">Description</span></span>|
|----------------|---------------|-----------------|
|`R2RGetEntryPoint`|<span data-ttu-id="636f9-229">159</span><span class="sxs-lookup"><span data-stu-id="636f9-229">159</span></span>|<span data-ttu-id="636f9-230">R2R 入口点查找结束时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-230">Raised when an R2R entry point lookup ends.</span></span>|

|<span data-ttu-id="636f9-231">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-231">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-232">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-232">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-233">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-233">`JITKeyword` (0x10)</span></span> |<span data-ttu-id="636f9-234">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-234">Informational (4)</span></span>|
|<span data-ttu-id="636f9-235">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-235">`NGenKeyword` (0x20)</span></span> |<span data-ttu-id="636f9-236">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-236">Informational (4)</span></span>|

|<span data-ttu-id="636f9-237">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-237">Field name</span></span>|<span data-ttu-id="636f9-238">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-238">Data type</span></span>|<span data-ttu-id="636f9-239">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-239">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-240">R2R 方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-240">Unique identifier of a R2R method.</span></span>|
|`MethodNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-241">要查找的方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-241">The namespace of method being looked up.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="636f9-242">要查找的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-242">The name of the method being looked up.</span></span>|
|`MethodSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-243">方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-243">Signature of the method (comma-separated list of type names).</span></span>|
|`EntryPoint`|`win:UInt64`|<span data-ttu-id="636f9-244">指向 R2R 方法入口点的指针</span><span class="sxs-lookup"><span data-stu-id="636f9-244">The pointer to the entry point of the R2R method</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-245">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-245">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="r2rgetentrypointstart-event"></a><span data-ttu-id="636f9-246">R2RGetEntryPointStart 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-246">R2RGetEntryPointStart event</span></span>

|<span data-ttu-id="636f9-247">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-247">Event</span></span>|<span data-ttu-id="636f9-248">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-248">Event ID</span></span>|<span data-ttu-id="636f9-249">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-249">Description</span></span>|
|----------------|---------------|-----------------|
|`R2RGetEntryPointStart`|<span data-ttu-id="636f9-250">160</span><span class="sxs-lookup"><span data-stu-id="636f9-250">160</span></span>|<span data-ttu-id="636f9-251">R2R 入口点查找开始时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-251">Raised when an R2R entry point lookup starts.</span></span>|

|<span data-ttu-id="636f9-252">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-252">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-253">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-253">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-254">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-254">`JITKeyword` (0x10)</span></span> |<span data-ttu-id="636f9-255">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-255">Informational (4)</span></span>|
|<span data-ttu-id="636f9-256">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-256">`NGenKeyword` (0x20)</span></span> |<span data-ttu-id="636f9-257">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-257">Informational (4)</span></span>|

|<span data-ttu-id="636f9-258">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-258">Field name</span></span>|<span data-ttu-id="636f9-259">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-259">Data type</span></span>|<span data-ttu-id="636f9-260">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-260">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-261">R2R 方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-261">Unique identifier of a R2R method.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-262">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-262">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodloadverbose_v1-event"></a><span data-ttu-id="636f9-263">MethodLoadVerbose_V1 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-263">MethodLoadVerbose_V1 event</span></span>

|<span data-ttu-id="636f9-264">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-264">Event</span></span>|<span data-ttu-id="636f9-265">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-265">Event ID</span></span>|<span data-ttu-id="636f9-266">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-266">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodLoadVerbose_V1`|<span data-ttu-id="636f9-267">143</span><span class="sxs-lookup"><span data-stu-id="636f9-267">143</span></span>|<span data-ttu-id="636f9-268">当方法为 JIT 加载的或加载 NGEN 映像时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-268">Raised when a method is JIT-loaded or an NGEN image is loaded.</span></span> <span data-ttu-id="636f9-269">动态和泛型方法始终使用此版本进行方法加载。</span><span class="sxs-lookup"><span data-stu-id="636f9-269">Dynamic and generic methods always use this version for method loads.</span></span> <span data-ttu-id="636f9-270">JIT 帮助器始终使用此版本。</span><span class="sxs-lookup"><span data-stu-id="636f9-270">JIT helpers always use this version.</span></span>|

|<span data-ttu-id="636f9-271">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-271">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-272">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-272">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-273">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-273">`JITKeyword` (0x10)</span></span> |<span data-ttu-id="636f9-274">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-274">Informational (4)</span></span>|
|<span data-ttu-id="636f9-275">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-275">`NGenKeyword` (0x20)</span></span> |<span data-ttu-id="636f9-276">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-276">Informational (4)</span></span>|

|<span data-ttu-id="636f9-277">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-277">Field name</span></span>|<span data-ttu-id="636f9-278">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-278">Data type</span></span>|<span data-ttu-id="636f9-279">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-279">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-280">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-280">Unique identifier of the method.</span></span> <span data-ttu-id="636f9-281">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-281">For JIT helper methods, set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-282">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-282">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-283">起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-283">Start address.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-284">方法长度。</span><span class="sxs-lookup"><span data-stu-id="636f9-284">Method length.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-285">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-285">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-286">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-286">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-287">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-287">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-288">0x4：JIT 编译的方法（否则由 NGen.exe 生成）</span><span class="sxs-lookup"><span data-stu-id="636f9-288">0x4: JIT-compiled method (otherwise, generated by NGen.exe)</span></span><br /><br /> <span data-ttu-id="636f9-289">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-289">0x8: Helper method.</span></span>|
|`MethodNameSpace`|`win:UnicodeString`|<span data-ttu-id="636f9-290">与该方法关联的完整命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-290">Full namespace name associated with the method.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="636f9-291">与该方法关联的完整类名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-291">Full class name associated with the method.</span></span>|
|`MethodSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-292">方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-292">Signature of the method (comma-separated list of type names).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-293">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-293">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodloadverbose_v2-event"></a><span data-ttu-id="636f9-294">MethodLoadVerbose_V2 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-294">MethodLoadVerbose_V2 event</span></span>

|<span data-ttu-id="636f9-295">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-295">Event</span></span>|<span data-ttu-id="636f9-296">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-296">Event ID</span></span>|<span data-ttu-id="636f9-297">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-297">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodLoadVerbose_V1`|<span data-ttu-id="636f9-298">143</span><span class="sxs-lookup"><span data-stu-id="636f9-298">143</span></span>|<span data-ttu-id="636f9-299">当方法为 JIT 加载的或加载 NGEN 映像时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-299">Raised when a method is JIT-loaded or an NGEN image is loaded.</span></span> <span data-ttu-id="636f9-300">动态和泛型方法始终使用此版本进行方法加载。</span><span class="sxs-lookup"><span data-stu-id="636f9-300">Dynamic and generic methods always use this version for method loads.</span></span> <span data-ttu-id="636f9-301">JIT 帮助器始终使用此版本。</span><span class="sxs-lookup"><span data-stu-id="636f9-301">JIT helpers always use this version.</span></span>|

|<span data-ttu-id="636f9-302">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-302">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-303">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-303">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-304">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-304">`JITKeyword` (0x10)</span></span> |<span data-ttu-id="636f9-305">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-305">Informational (4)</span></span>|
|<span data-ttu-id="636f9-306">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-306">`NGenKeyword` (0x20)</span></span> |<span data-ttu-id="636f9-307">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-307">Informational (4)</span></span>|

|<span data-ttu-id="636f9-308">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-308">Field name</span></span>|<span data-ttu-id="636f9-309">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-309">Data type</span></span>|<span data-ttu-id="636f9-310">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-310">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-311">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-311">Unique identifier of the method.</span></span> <span data-ttu-id="636f9-312">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-312">For JIT helper methods, set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-313">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-313">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-314">起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-314">Start address.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-315">方法长度。</span><span class="sxs-lookup"><span data-stu-id="636f9-315">Method length.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-316">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-316">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-317">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-317">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-318">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-318">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-319">0x4：JIT 编译的方法（否则由 NGen.exe 生成）</span><span class="sxs-lookup"><span data-stu-id="636f9-319">0x4: JIT-compiled method (otherwise, generated by NGen.exe)</span></span><br /><br /> <span data-ttu-id="636f9-320">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-320">0x8: Helper method.</span></span>|
|`MethodNameSpace`|`win:UnicodeString`|<span data-ttu-id="636f9-321">与该方法关联的完整命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-321">Full namespace name associated with the method.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="636f9-322">与该方法关联的完整类名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-322">Full class name associated with the method.</span></span>|
|`MethodSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-323">方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-323">Signature of the method (comma-separated list of type names).</span></span>|
|`ReJITID`|`win:UInt64`|<span data-ttu-id="636f9-324">方法的 ReJIT ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-324">ReJIT ID of the method.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-325">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-325">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodunloadverbose_v1-event"></a><span data-ttu-id="636f9-326">MethodUnLoadVerbose_V1 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-326">MethodUnLoadVerbose_V1 event</span></span>

|<span data-ttu-id="636f9-327">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-327">Event</span></span>|<span data-ttu-id="636f9-328">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-328">Event ID</span></span>|<span data-ttu-id="636f9-329">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-329">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodUnLoadVerbose_V1`|<span data-ttu-id="636f9-330">144</span><span class="sxs-lookup"><span data-stu-id="636f9-330">144</span></span>|<span data-ttu-id="636f9-331">在销毁动态方法、卸载模块或销毁应用程序域时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-331">Raised when a dynamic method is destroyed, a module is unloaded, or an application domain is destroyed.</span></span> <span data-ttu-id="636f9-332">动态方法始终使用此版本进行方法卸载。</span><span class="sxs-lookup"><span data-stu-id="636f9-332">Dynamic methods always use this version for method unloads.</span></span>|

|<span data-ttu-id="636f9-333">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-333">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-334">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-334">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-335">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-335">`JITKeyword` (0x10)</span></span> |<span data-ttu-id="636f9-336">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-336">Informational (4)</span></span>|
|<span data-ttu-id="636f9-337">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-337">`NGenKeyword` (0x20)</span></span> |<span data-ttu-id="636f9-338">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-338">Informational (4)</span></span>|

|<span data-ttu-id="636f9-339">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-339">Field name</span></span>|<span data-ttu-id="636f9-340">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-340">Data type</span></span>|<span data-ttu-id="636f9-341">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-341">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-342">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-342">Unique identifier of the method.</span></span> <span data-ttu-id="636f9-343">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-343">For JIT helper methods, set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-344">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-344">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-345">起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-345">Start address.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-346">方法长度。</span><span class="sxs-lookup"><span data-stu-id="636f9-346">Method length.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-347">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-347">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-348">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-348">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-349">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-349">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-350">0x4：JIT 编译的方法（否则由 NGen.exe 生成）</span><span class="sxs-lookup"><span data-stu-id="636f9-350">0x4: JIT-compiled method (otherwise, generated by NGen.exe)</span></span><br /><br /> <span data-ttu-id="636f9-351">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-351">0x8: Helper method.</span></span>|
|`MethodNameSpace`|`win:UnicodeString`|<span data-ttu-id="636f9-352">与该方法关联的完整命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-352">Full namespace name associated with the method.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="636f9-353">与该方法关联的完整类名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-353">Full class name associated with the method.</span></span>|
|`MethodSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-354">方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-354">Signature of the method (comma-separated list of type names).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-355">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-355">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodunloadverbose_v2-event"></a><span data-ttu-id="636f9-356">MethodUnLoadVerbose_V2 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-356">MethodUnLoadVerbose_V2 event</span></span>

|<span data-ttu-id="636f9-357">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-357">Event</span></span>|<span data-ttu-id="636f9-358">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-358">Event ID</span></span>|<span data-ttu-id="636f9-359">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-359">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodUnLoadVerbose_V2`|<span data-ttu-id="636f9-360">144</span><span class="sxs-lookup"><span data-stu-id="636f9-360">144</span></span>|<span data-ttu-id="636f9-361">在销毁动态方法、卸载模块或销毁应用程序域时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-361">Raised when a dynamic method is destroyed, a module is unloaded, or an application domain is destroyed.</span></span> <span data-ttu-id="636f9-362">动态方法始终使用此版本进行方法卸载。</span><span class="sxs-lookup"><span data-stu-id="636f9-362">Dynamic methods always use this version for method unloads.</span></span>|

|<span data-ttu-id="636f9-363">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-363">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-364">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-364">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-365">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-365">`JITKeyword` (0x10)</span></span> |<span data-ttu-id="636f9-366">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-366">Informational (4)</span></span>|
|<span data-ttu-id="636f9-367">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-367">`NGenKeyword` (0x20)</span></span> |<span data-ttu-id="636f9-368">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="636f9-368">Informational (4)</span></span>|

|<span data-ttu-id="636f9-369">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-369">Field name</span></span>|<span data-ttu-id="636f9-370">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-370">Data type</span></span>|<span data-ttu-id="636f9-371">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-371">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-372">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-372">Unique identifier of the method.</span></span> <span data-ttu-id="636f9-373">对于 JIT 帮助器方法，将设置为该方法的起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-373">For JIT helper methods, set to the start address of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-374">此方法所属的模块标识符（0 表示 JIT 帮助器）。</span><span class="sxs-lookup"><span data-stu-id="636f9-374">Identifier of the module to which this method belongs (0 for JIT helpers).</span></span>|
|`MethodStartAddress`|`win:UInt64`|<span data-ttu-id="636f9-375">起始地址。</span><span class="sxs-lookup"><span data-stu-id="636f9-375">Start address.</span></span>|
|`MethodSize`|`win:UInt32`|<span data-ttu-id="636f9-376">方法长度。</span><span class="sxs-lookup"><span data-stu-id="636f9-376">Method length.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-377">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-377">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodFlags`|`win:UInt32`|<span data-ttu-id="636f9-378">0x1：动态方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-378">0x1: Dynamic method.</span></span><br /><br /> <span data-ttu-id="636f9-379">0x2：泛型方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-379">0x2: Generic method.</span></span><br /><br /> <span data-ttu-id="636f9-380">0x4：JIT 编译的方法（否则由 NGen.exe 生成）</span><span class="sxs-lookup"><span data-stu-id="636f9-380">0x4: JIT-compiled method (otherwise, generated by NGen.exe)</span></span><br /><br /> <span data-ttu-id="636f9-381">0x8：帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="636f9-381">0x8: Helper method.</span></span>|
|`MethodNameSpace`|`win:UnicodeString`|<span data-ttu-id="636f9-382">与该方法关联的完整命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-382">Full namespace name associated with the method.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="636f9-383">与该方法关联的完整类名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-383">Full class name associated with the method.</span></span>|
|`MethodSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-384">方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-384">Signature of the method (comma-separated list of type names).</span></span>|
|`ReJITID`|`win:UInt64`|<span data-ttu-id="636f9-385">方法的 ReJIT ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-385">ReJIT ID of the method.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-386">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-386">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodjittingstarted_v1-event"></a><span data-ttu-id="636f9-387">MethodJittingStarted_V1 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-387">MethodJittingStarted_V1 event</span></span>

<span data-ttu-id="636f9-388">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="636f9-388">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="636f9-389">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-389">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-390">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-390">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-391">`JITKeyword` (0x10)</span><span class="sxs-lookup"><span data-stu-id="636f9-391">`JITKeyword` (0x10)</span></span> |<span data-ttu-id="636f9-392">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="636f9-392">Verbose (5)</span></span>|
|<span data-ttu-id="636f9-393">`NGenKeyword` (0x20)</span><span class="sxs-lookup"><span data-stu-id="636f9-393">`NGenKeyword` (0x20)</span></span> |<span data-ttu-id="636f9-394">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="636f9-394">Verbose (5)</span></span>|

|<span data-ttu-id="636f9-395">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-395">Event</span></span>|<span data-ttu-id="636f9-396">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-396">Event ID</span></span>|<span data-ttu-id="636f9-397">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-397">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodJittingStarted_V1`|<span data-ttu-id="636f9-398">145</span><span class="sxs-lookup"><span data-stu-id="636f9-398">145</span></span>|<span data-ttu-id="636f9-399">在方法由 JIT 编译时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-399">Raised when a method is being JIT-compiled.</span></span>|

|<span data-ttu-id="636f9-400">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-400">Field name</span></span>|<span data-ttu-id="636f9-401">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-401">Data type</span></span>|<span data-ttu-id="636f9-402">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-402">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-403">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-403">Unique identifier of the method.</span></span>|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="636f9-404">此方法所属的模块标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-404">Identifier of the module to which this method belongs.</span></span>|
|`MethodToken`|`win:UInt32`|<span data-ttu-id="636f9-405">0 代表动态方法和 JIT 帮助器。</span><span class="sxs-lookup"><span data-stu-id="636f9-405">0 for dynamic methods and JIT helpers.</span></span>|
|`MethodILSize`|`win:UInt32`|<span data-ttu-id="636f9-406">要进行 JIT 编译的方法的公共中间语言 (CIL) 的大小。</span><span class="sxs-lookup"><span data-stu-id="636f9-406">The size of the Common Intermediate Language (CIL) for the method that is being JIT-compiled.</span></span>|
|`MethodNameSpace`|`win:UnicodeString`|<span data-ttu-id="636f9-407">与该方法关联的完整类名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-407">Full class name associated with the method.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="636f9-408">方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-408">Name of the method.</span></span>|
|`MethodSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-409">方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-409">Signature of the method (comma-separated list of type names).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-410">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-410">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodjitinliningsucceeded-event"></a><span data-ttu-id="636f9-411">MethodJitInliningSucceeded 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-411">MethodJitInliningSucceeded event</span></span>

|<span data-ttu-id="636f9-412">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-412">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-413">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-413">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-414">`JITTracingKeyword` (0x1000)</span><span class="sxs-lookup"><span data-stu-id="636f9-414">`JITTracingKeyword` (0x1000)</span></span> |<span data-ttu-id="636f9-415">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="636f9-415">Verbose (5)</span></span>|

|<span data-ttu-id="636f9-416">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-416">Event</span></span>|<span data-ttu-id="636f9-417">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-417">Event ID</span></span>|<span data-ttu-id="636f9-418">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-418">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodJitInliningSucceeded`|<span data-ttu-id="636f9-419">185</span><span class="sxs-lookup"><span data-stu-id="636f9-419">185</span></span>|<span data-ttu-id="636f9-420">当方法由 JIT 编译器成功内联时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-420">Raised when a method is successfully inlined by the JIT compiler.</span></span>|

|<span data-ttu-id="636f9-421">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-421">Field name</span></span>|<span data-ttu-id="636f9-422">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-422">Data type</span></span>|<span data-ttu-id="636f9-423">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-423">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-424">要编译的方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-424">Namespace of the method being compiled.</span></span>|
|`MethodBeingCompiledName`|`win:UnicodeString`|<span data-ttu-id="636f9-425">要编译的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-425">Name of the method being compiled.</span></span>|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-426">要编译的方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-426">Signature of the method (comma-separated list of type names) being compiled.</span></span>|
|`InlinerNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-427">内联方（“父级”）方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-427">The namespace of inliner ("parent") method.</span></span>|
|`InlinerName`|`win:UnicodeString`|<span data-ttu-id="636f9-428">内联方（“父级”）方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-428">Name of the inliner ("parent") method.</span></span>|
|`InlinerNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-429">内联方（“父级”）方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-429">Signature of the inliner ("parent") method (comma-separated list of type names).</span></span>|
|`InlineeNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-430">被内联方（“子级”）方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-430">The namespace of inlinee ("child") method.</span></span>|
|`InlineeName`|`win:UnicodeString`|<span data-ttu-id="636f9-431">被内联方（“子级”）方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-431">Name of the inlinee ("child") method.</span></span>|
|`InlineeNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-432">被内联方（“子级”）方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-432">Signature of the inlinee ("child") method (comma-separated list of type names).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-433">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-433">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodjitinliningfailed-event"></a><span data-ttu-id="636f9-434">MethodJitInliningFailed 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-434">MethodJitInliningFailed event</span></span>

|<span data-ttu-id="636f9-435">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-435">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-436">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-436">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-437">`JITTracingKeyword` (0x1000)</span><span class="sxs-lookup"><span data-stu-id="636f9-437">`JITTracingKeyword` (0x1000)</span></span> |<span data-ttu-id="636f9-438">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="636f9-438">Verbose (5)</span></span>|

|<span data-ttu-id="636f9-439">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-439">Event</span></span>|<span data-ttu-id="636f9-440">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-440">Event ID</span></span>|<span data-ttu-id="636f9-441">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-441">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodJitInliningFailed`|<span data-ttu-id="636f9-442">192</span><span class="sxs-lookup"><span data-stu-id="636f9-442">192</span></span>|<span data-ttu-id="636f9-443">当方法无法由 JIT 编译器内联时引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-443">Raised when a method was failed to be inlined by the JIT compiler.</span></span>|

|<span data-ttu-id="636f9-444">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-444">Field name</span></span>|<span data-ttu-id="636f9-445">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-445">Data type</span></span>|<span data-ttu-id="636f9-446">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-446">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-447">要编译的方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-447">Namespace of the method being compiled.</span></span>|
|`MethodBeingCompiledName`|`win:UnicodeString`|<span data-ttu-id="636f9-448">要编译的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-448">Name of the method being compiled.</span></span>|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-449">要编译的方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-449">Signature of the method (comma-separated list of type names) being compiled.</span></span>|
|`InlinerNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-450">内联方（“父级”）方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-450">The namespace of inliner ("parent") method.</span></span>|
|`InlinerName`|`win:UnicodeString`|<span data-ttu-id="636f9-451">内联方（“父级”）方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-451">Name of the inliner ("parent") method.</span></span>|
|`InlinerNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-452">内联方（“父级”）方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-452">Signature of the inliner ("parent") method (comma-separated list of type names).</span></span>|
|`InlineeNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-453">被内联方（“子级”）方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-453">The namespace of inlinee ("child") method.</span></span>|
|`InlineeName`|`win:UnicodeString`|<span data-ttu-id="636f9-454">被内联方（“子级”）方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-454">Name of the inlinee ("child") method.</span></span>|
|`InlineeNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-455">被内联方（“子级”）方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-455">Signature of the inlinee ("child") method (comma-separated list of type names).</span></span>|
|`FailAlways`|`win:Boolean`|<span data-ttu-id="636f9-456">方法是否被标记为不可内联。</span><span class="sxs-lookup"><span data-stu-id="636f9-456">Whether the method is marked as not inlinable.</span></span>|
|`FailReason`|`win:UnicodeString`|<span data-ttu-id="636f9-457">内联失败的原因。</span><span class="sxs-lookup"><span data-stu-id="636f9-457">Reason inlining failed.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-458">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-458">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodjittailcallsucceeded-event"></a><span data-ttu-id="636f9-459">MethodJITTailCallSucceeded 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-459">MethodJitTailCallSucceeded event</span></span>

|<span data-ttu-id="636f9-460">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-460">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-461">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-461">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-462">`JITTracingKeyword` (0x1000)</span><span class="sxs-lookup"><span data-stu-id="636f9-462">`JITTracingKeyword` (0x1000)</span></span> |<span data-ttu-id="636f9-463">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="636f9-463">Verbose (5)</span></span>|

|<span data-ttu-id="636f9-464">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-464">Event</span></span>|<span data-ttu-id="636f9-465">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-465">Event ID</span></span>|<span data-ttu-id="636f9-466">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-466">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodJitTailCallSucceeded`|<span data-ttu-id="636f9-467">192</span><span class="sxs-lookup"><span data-stu-id="636f9-467">192</span></span>|<span data-ttu-id="636f9-468">当方法可成功被尾调用时由 JIT 编译器引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-468">Raised by the JIT compiler when a method can be successfully tail called.</span></span>|

|<span data-ttu-id="636f9-469">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-469">Field name</span></span>|<span data-ttu-id="636f9-470">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-470">Data type</span></span>|<span data-ttu-id="636f9-471">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-471">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-472">要编译的方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-472">Namespace of the method being compiled.</span></span>|
|`MethodBeingCompiledName`|`win:UnicodeString`|<span data-ttu-id="636f9-473">要编译的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-473">Name of the method being compiled.</span></span>|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-474">要编译的方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-474">Signature of the method (comma-separated list of type names) being compiled.</span></span>|
|`CallerNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-475">调用方方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-475">Namespace of the caller method.</span></span>|
|`CallerName`|`win:UnicodeString`|<span data-ttu-id="636f9-476">调用方方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-476">Name of the caller method.</span></span>|
|`CallerNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-477">调用方方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-477">Signature of the caller method (Comma-separated list of type names).</span></span>|
|`CalleeNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-478">被调用方方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-478">Namespace of the callee method.</span></span>|
|`CalleeName`|`win:UnicodeString`|<span data-ttu-id="636f9-479">被调用方方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-479">Name of the callee method.</span></span>|
|`CalleeNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-480">被调用方方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-480">Signature of the callee method (Comma-separated list of type names).</span></span>|
|`TailPrefix`|`win:Boolean`|<span data-ttu-id="636f9-481">它是否为尾前缀指令。</span><span class="sxs-lookup"><span data-stu-id="636f9-481">Whether it is a tail prefix instruction.</span></span>|
|`TailCallType`|`win:UInt32`|<span data-ttu-id="636f9-482">尾调用的类型。</span><span class="sxs-lookup"><span data-stu-id="636f9-482">The type of tail call.</span></span><br/><br/><span data-ttu-id="636f9-483">0：优化的尾调用 (epilog + jmp)</span><span class="sxs-lookup"><span data-stu-id="636f9-483">0: Optimized tail call (epilog + jmp)</span></span><br/><br/><span data-ttu-id="636f9-484">1：递归的尾调用（方法尾调用自身）</span><span class="sxs-lookup"><span data-stu-id="636f9-484">1: Recursive tail call (method tail calls itself)</span></span><br/><br/><span data-ttu-id="636f9-485">2：帮助程序辅助的尾调用</span><span class="sxs-lookup"><span data-stu-id="636f9-485">2: Helper assisted tail call</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-486">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-486">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodjittailcallfailed-event"></a><span data-ttu-id="636f9-487">MethodJITTailCallFailed 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-487">MethodJitTailCallFailed event</span></span>

|<span data-ttu-id="636f9-488">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-488">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-489">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-489">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-490">`JITTracingKeyword` (0x1000)</span><span class="sxs-lookup"><span data-stu-id="636f9-490">`JITTracingKeyword` (0x1000)</span></span> |<span data-ttu-id="636f9-491">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="636f9-491">Verbose (5)</span></span>|

|<span data-ttu-id="636f9-492">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-492">Event</span></span>|<span data-ttu-id="636f9-493">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-493">Event ID</span></span>|<span data-ttu-id="636f9-494">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-494">Description</span></span>|
|-----------|--------------|-----------------|
|`MethodJitTailCallFailed`|<span data-ttu-id="636f9-495">191</span><span class="sxs-lookup"><span data-stu-id="636f9-495">191</span></span>|<span data-ttu-id="636f9-496">当方法无法被尾调用时由 JIT 编译器引发。</span><span class="sxs-lookup"><span data-stu-id="636f9-496">Raised by the JIT compiler when a method was failed to be tail called.</span></span>|

|<span data-ttu-id="636f9-497">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-497">Field name</span></span>|<span data-ttu-id="636f9-498">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-498">Data type</span></span>|<span data-ttu-id="636f9-499">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-499">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-500">要编译的方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-500">Namespace of the method being compiled.</span></span>|
|`MethodBeingCompiledName`|`win:UnicodeString`|<span data-ttu-id="636f9-501">要编译的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-501">Name of the method being compiled.</span></span>|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-502">要编译的方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-502">Signature of the method (comma-separated list of type names) being compiled.</span></span>|
|`CallerNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-503">调用方方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-503">Namespace of the caller method.</span></span>|
|<span data-ttu-id="636f9-504">`CallerNam`e</span><span class="sxs-lookup"><span data-stu-id="636f9-504">`CallerNam`e</span></span>|`win:UnicodeString`|<span data-ttu-id="636f9-505">调用方方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-505">Name of the caller method.</span></span>|
|`CallerNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-506">调用方方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-506">Signature of the caller method (Comma-separated list of type names).</span></span>|
|`CalleeNamespace`|`win:UnicodeString`|<span data-ttu-id="636f9-507">被调用方方法的命名空间。</span><span class="sxs-lookup"><span data-stu-id="636f9-507">Namespace of the callee method.</span></span>|
|`CalleeName`|`win:UnicodeString`|<span data-ttu-id="636f9-508">被调用方方法的名称。</span><span class="sxs-lookup"><span data-stu-id="636f9-508">Name of the callee method.</span></span>|
|`CalleeNameSignature`|`win:UnicodeString`|<span data-ttu-id="636f9-509">被调用方方法的签名（以逗号分隔的类型名称列表）。</span><span class="sxs-lookup"><span data-stu-id="636f9-509">Signature of the callee method (Comma-separated list of type names).</span></span>|
|`TailPrefix`|`win:Boolean`|<span data-ttu-id="636f9-510">它是否为尾前缀指令。</span><span class="sxs-lookup"><span data-stu-id="636f9-510">Whether it is a tail prefix instruction.</span></span>|
|`FailReason`|`win:UnicodeString`|<span data-ttu-id="636f9-511">尾调用失败的原因。</span><span class="sxs-lookup"><span data-stu-id="636f9-511">Reason tail call failed.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-512">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-512">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="methodiltonativemap-event"></a><span data-ttu-id="636f9-513">MethodILToNativeMap 事件</span><span class="sxs-lookup"><span data-stu-id="636f9-513">MethodILToNativeMap event</span></span>

|<span data-ttu-id="636f9-514">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="636f9-514">Keyword for raising the event</span></span>|<span data-ttu-id="636f9-515">Level</span><span class="sxs-lookup"><span data-stu-id="636f9-515">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="636f9-516">`JittedMethodILToNativeMapKeyword` (0x20000)</span><span class="sxs-lookup"><span data-stu-id="636f9-516">`JittedMethodILToNativeMapKeyword` (0x20000)</span></span> |<span data-ttu-id="636f9-517">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="636f9-517">Verbose (5)</span></span>|

|<span data-ttu-id="636f9-518">事件</span><span class="sxs-lookup"><span data-stu-id="636f9-518">Event</span></span>|<span data-ttu-id="636f9-519">事件 ID</span><span class="sxs-lookup"><span data-stu-id="636f9-519">Event ID</span></span>|<span data-ttu-id="636f9-520">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-520">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodILToNativeMap`|<span data-ttu-id="636f9-521">190</span><span class="sxs-lookup"><span data-stu-id="636f9-521">190</span></span>|<span data-ttu-id="636f9-522">为 JIT 编译的方法映射 IL 到本机映射事件。</span><span class="sxs-lookup"><span data-stu-id="636f9-522">Maps the IL-to-native map event for JIT-compiled methods.</span></span>|

|<span data-ttu-id="636f9-523">字段名</span><span class="sxs-lookup"><span data-stu-id="636f9-523">Field name</span></span>|<span data-ttu-id="636f9-524">数据类型</span><span class="sxs-lookup"><span data-stu-id="636f9-524">Data type</span></span>|<span data-ttu-id="636f9-525">说明</span><span class="sxs-lookup"><span data-stu-id="636f9-525">Description</span></span>|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|<span data-ttu-id="636f9-526">方法的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="636f9-526">Unique identifier of a method.</span></span>|
|`ReJITID`|`win:UInt64`|<span data-ttu-id="636f9-527">方法的 ReJIT ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-527">The ReJIT ID of the method.</span></span>|
|`MethodExtent`|`win:UInt8`|<span data-ttu-id="636f9-528">已执行 JIT 的方法的范围。</span><span class="sxs-lookup"><span data-stu-id="636f9-528">The extent for the jitted method.</span></span>|
|`CountOfMapEntries`|`win:UInt8`|<span data-ttu-id="636f9-529">映射条目数</span><span class="sxs-lookup"><span data-stu-id="636f9-529">Number of map entries</span></span>|
|`ILOffsets`|`win:UInt32`|<span data-ttu-id="636f9-530">IL 偏移量。</span><span class="sxs-lookup"><span data-stu-id="636f9-530">The IL offset.</span></span>|
|`NativeOffsets`|`win:UInt32`|<span data-ttu-id="636f9-531">本机代码偏移量。</span><span class="sxs-lookup"><span data-stu-id="636f9-531">The native code offset.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="636f9-532">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="636f9-532">Unique ID for the instance of CoreCLR.</span></span>|
