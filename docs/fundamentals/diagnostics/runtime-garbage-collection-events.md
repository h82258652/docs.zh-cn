---
title: 垃圾回收运行时事件
description: 查看收集特定于 .NET 垃圾回收器的诊断信息的 .NET 运行时事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- GC events
- garbage collection events (CoreCLR)
- ETW, EventPipe, LTTng garbage collection events (CoreCLR)
ms.openlocfilehash: 2799a93f351baf23ec7a359b0b4b2be5c216dc4d
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591061"
---
# <a name="net-runtime-garbage-collection-events"></a><span data-ttu-id="cbfa8-103">.NET 运行时垃圾回收事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-103">.NET runtime garbage collection events</span></span>

<span data-ttu-id="cbfa8-104">这些事件可收集有关垃圾回收的信息。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-104">These events collect information pertaining to garbage collection.</span></span> <span data-ttu-id="cbfa8-105">它们有助于诊断和调试，包括确定执行垃圾回收的次数、垃圾回收期间释放的内存量等。有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-105">They help in diagnostics and debugging, including determining how many times garbage collection was performed, how much memory was freed during garbage collection, etc. For more information about how to use these events for diagnostic purposes, see [logging and tracing .NET applications](../../core/diagnostics/logging-tracing.md)</span></span>

## <a name="gcstart_v2-event"></a><span data-ttu-id="cbfa8-106">GCStart_V2 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-106">GCStart_V2 Event</span></span>

<span data-ttu-id="cbfa8-107">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-107">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-108">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-108">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-109">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-109">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-110">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-110">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-111">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-111">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-112">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-112">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-113">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-113">Event</span></span>|<span data-ttu-id="cbfa8-114">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-114">Event ID</span></span>|<span data-ttu-id="cbfa8-115">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-115">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCStart_V1`|<span data-ttu-id="cbfa8-116">1</span><span class="sxs-lookup"><span data-stu-id="cbfa8-116">1</span></span>|<span data-ttu-id="cbfa8-117">垃圾回收已启动。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-117">A garbage collection has started.</span></span>|

<span data-ttu-id="cbfa8-118">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-118">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-119">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-119">Field name</span></span>|<span data-ttu-id="cbfa8-120">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-120">Data type</span></span>|<span data-ttu-id="cbfa8-121">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-121">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|<span data-ttu-id="cbfa8-122">第 *n* 代垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-122">The *n* th garbage collection.</span></span>|
|`Depth`|`win:UInt32`|<span data-ttu-id="cbfa8-123">正在回收的代。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-123">The generation that is being collected.</span></span>|
|`Reason`|`win:UInt32`|<span data-ttu-id="cbfa8-124">垃圾回收的触发原因：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-124">Why the garbage collection was triggered:</span></span><br /><br /> <span data-ttu-id="cbfa8-125">`0x0` - 小型对象堆分配。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-125">`0x0` - Small object heap allocation.</span></span><br /><br /> <span data-ttu-id="cbfa8-126">`0x1` - 已引发。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-126">`0x1` - Induced.</span></span><br /><br /> <span data-ttu-id="cbfa8-127">`0x2` - 内存不足。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-127">`0x2` - Low memory.</span></span><br /><br /> <span data-ttu-id="cbfa8-128">`0x3` - 空。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-128">`0x3` - Empty.</span></span><br /><br /> <span data-ttu-id="cbfa8-129">`0x4` - 大型对象堆分配。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-129">`0x4` - Large object heap allocation.</span></span><br /><br /> <span data-ttu-id="cbfa8-130">`0x5` - 空间外（针对小型对象堆）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-130">`0x5` - Out of space (for small object heap).</span></span><br /><br /> <span data-ttu-id="cbfa8-131">`0x6` - 空间外（针对大型对象堆）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-131">`0x6` - Out of space (for large object heap).</span></span><br /><br /> <span data-ttu-id="cbfa8-132">`0x7` - 已引发，但不是强制作为阻塞。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-132">`0x7` - Induced but not forced as blocking.</span></span>|
|`Type`|`win:UInt32`|<span data-ttu-id="cbfa8-133">`0x0` - 后台垃圾回收外发生了阻止垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-133">`0x0` - Blocking garbage collection occurred outside background garbage collection.</span></span><br /><br /> <span data-ttu-id="cbfa8-134">`0x1` - 后台垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-134">`0x1` - Background garbage collection.</span></span><br /><br /> <span data-ttu-id="cbfa8-135">`0x2` - 后台垃圾回收时发生了阻止垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-135">`0x2` - Blocking garbage collection occurred during background garbage collection.</span></span>|
|`ClrInstanceID`|<span data-ttu-id="cbfa8-136">win:UInt16</span><span class="sxs-lookup"><span data-stu-id="cbfa8-136">win:UInt16</span></span>|<span data-ttu-id="cbfa8-137">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-137">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="gcend_v1-event"></a><span data-ttu-id="cbfa8-138">GCEnd_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-138">GCEnd_V1 Event</span></span>

<span data-ttu-id="cbfa8-139">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-139">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-140">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-140">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-141">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-141">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-142">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-142">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-143">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-143">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-144">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-144">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-145">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-145">Event</span></span>|<span data-ttu-id="cbfa8-146">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-146">Event ID</span></span>|<span data-ttu-id="cbfa8-147">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-147">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCEnd_V1`|<span data-ttu-id="cbfa8-148">2</span><span class="sxs-lookup"><span data-stu-id="cbfa8-148">2</span></span>|<span data-ttu-id="cbfa8-149">垃圾回收已结束。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-149">The garbage collection has ended.</span></span>|

<span data-ttu-id="cbfa8-150">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-150">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-151">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-151">Field name</span></span>|<span data-ttu-id="cbfa8-152">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-152">Data type</span></span>|<span data-ttu-id="cbfa8-153">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-153">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|<span data-ttu-id="cbfa8-154">第 *n* 代垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-154">The *n* th garbage collection.</span></span>|
|`Depth`|`win:UInt32`|<span data-ttu-id="cbfa8-155">已回收的代。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-155">The generation that was collected.</span></span>|
|`ClrInstanceID`|<span data-ttu-id="cbfa8-156">win:UInt16</span><span class="sxs-lookup"><span data-stu-id="cbfa8-156">win:UInt16</span></span>|<span data-ttu-id="cbfa8-157">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-157">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="gcheapstats_v2-event"></a><span data-ttu-id="cbfa8-158">GCHeapStats_V2 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-158">GCHeapStats_V2 Event</span></span>

<span data-ttu-id="cbfa8-159">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-159">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-160">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-160">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-161">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-161">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-162">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-162">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-163">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-163">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-164">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-164">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-165">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-165">Event</span></span>|<span data-ttu-id="cbfa8-166">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-166">Event ID</span></span>|<span data-ttu-id="cbfa8-167">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-167">Description</span></span>|
|-----------|--------------|-----------------|
|`GCHeapStats_V2`|<span data-ttu-id="cbfa8-168">4</span><span class="sxs-lookup"><span data-stu-id="cbfa8-168">4</span></span>|<span data-ttu-id="cbfa8-169">在每次垃圾回收结束时显示堆统计信息。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-169">Shows the heap statistics at the end of each garbage collection.</span></span>|

<span data-ttu-id="cbfa8-170">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-170">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-171">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-171">Field name</span></span>|<span data-ttu-id="cbfa8-172">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-172">Data type</span></span>|<span data-ttu-id="cbfa8-173">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-173">Description</span></span>|
|----------------|---------------|-----------------|
|`GenerationSize0`|`win:UInt64`|<span data-ttu-id="cbfa8-174">第 0 代内存的大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-174">The size, in bytes, of generation 0 memory.</span></span>|
|`TotalPromotedSize0`|`win:UInt64`|<span data-ttu-id="cbfa8-175">从第 0 代提升到第 1 代的字节数。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-175">The number of bytes that are promoted from generation 0 to generation 1.</span></span>|
|`GenerationSize1`|`win:UInt64`|<span data-ttu-id="cbfa8-176">第 1 代内存的大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-176">The size, in bytes, of generation 1 memory.</span></span>|
|`TotalPromotedSize1`|`win:UInt64`|<span data-ttu-id="cbfa8-177">从第 1 代提升到第 2 代的字节数。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-177">The number of bytes that are promoted from generation 1 to generation 2.</span></span>|
|`GenerationSize2`|`win:UInt64`|<span data-ttu-id="cbfa8-178">第 2 代内存的大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-178">The size, in bytes, of generation 2 memory.</span></span>|
|`TotalPromotedSize2`|`win:UInt64`|<span data-ttu-id="cbfa8-179">上次回收后仍存在于第 2 代中的字节数。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-179">The number of bytes that survived in generation 2 after the last collection.</span></span>|
|`GenerationSize3`|`win:UInt64`|<span data-ttu-id="cbfa8-180">大型对象堆的大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-180">The size, in bytes, of the large object heap.</span></span>|
|`TotalPromotedSize3`|`win:UInt64`|<span data-ttu-id="cbfa8-181">上次回收后仍存在于大型对象堆中的字节数。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-181">The number of bytes that survived in the large object heap after the last collection.</span></span>|
|`FinalizationPromotedSize`|`win:UInt64`|<span data-ttu-id="cbfa8-182">准备终结的对象的总大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-182">The total size, in bytes, of the objects that are ready for finalization.</span></span>|
|`FinalizationPromotedCount`|`win:UInt64`|<span data-ttu-id="cbfa8-183">已准备好进行终结的对象的数目。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-183">The number of objects that are ready for finalization.</span></span>|
|`PinnedObjectCount`|`win:UInt32`|<span data-ttu-id="cbfa8-184">固定（不可移动）对象的数目。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-184">The number of pinned (unmovable) objects.</span></span>|
|`SinkBlockCount`|`win:UInt32`|<span data-ttu-id="cbfa8-185">正在使用的同步块的数目。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-185">The number of synchronization blocks in use.</span></span>|
|`GCHandleCount`|`win:UInt32`|<span data-ttu-id="cbfa8-186">使用中的垃圾回收句柄的数目。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-186">The number of garbage collection handles in use.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-187">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-187">Unique ID for the instance of CoreCLR.</span></span>|
|`GenerationSize4`|`win:UInt64`|<span data-ttu-id="cbfa8-188">已固定对象堆的大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-188">The size, in bytes, of the pinned object heap.</span></span>|
|`TotalPromotedSize4`|`win:UInt64`|<span data-ttu-id="cbfa8-189">上次回收后仍存在于固定对象堆中的字节数。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-189">The number of bytes that survived in the pinned object heap after the last collection.</span></span>|

## <a name="gccreatesegment_v1-event"></a><span data-ttu-id="cbfa8-190">GCCreateSegment_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-190">GCCreateSegment_V1 event</span></span>

<span data-ttu-id="cbfa8-191">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-191">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-192">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-192">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-193">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-193">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-194">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-194">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-195">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-195">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-196">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-196">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-197">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-197">Event</span></span>|<span data-ttu-id="cbfa8-198">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-198">Event ID</span></span>|<span data-ttu-id="cbfa8-199">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-199">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCCreateSegment_V1`|<span data-ttu-id="cbfa8-200">5</span><span class="sxs-lookup"><span data-stu-id="cbfa8-200">5</span></span>|<span data-ttu-id="cbfa8-201">已创建的新的垃圾回收段。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-201">A new garbage collection segment has been created.</span></span> <span data-ttu-id="cbfa8-202">此外，当在已运行的进程中启用跟踪时，将为每个现有段引发此事件。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-202">In addition, when tracing is enabled on a process that is already running, this event is raised for each existing segment.</span></span>|

<span data-ttu-id="cbfa8-203">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-203">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-204">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-204">Field name</span></span>|<span data-ttu-id="cbfa8-205">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-205">Data type</span></span>|<span data-ttu-id="cbfa8-206">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-206">Description</span></span>|
|----------------|---------------|-----------------|
|`Address`|`win:UInt64`|<span data-ttu-id="cbfa8-207">段的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-207">The address of the segment.</span></span>|
|`Size`|`win:UInt64`|<span data-ttu-id="cbfa8-208">段的大小。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-208">The size of the segment.</span></span>|
|`Type`|`win:UInt32`|<span data-ttu-id="cbfa8-209">0x0 - 小型对象堆。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-209">0x0 - Small object heap.</span></span><br /><br /> <span data-ttu-id="cbfa8-210">0x0 - 大型对象堆。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-210">0x1 - Large object heap.</span></span><br /><br /> <span data-ttu-id="cbfa8-211">0x2 - 只读堆。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-211">0x2 - Read-only heap.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-212">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-212">Unique ID for the instance of CoreCLR.</span></span>|

<span data-ttu-id="cbfa8-213">请注意，由垃圾回收器分配的段的大小特定于实现，且随时可能更改（包括定期更新）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-213">Note that the size of segments allocated by the garbage collector is implementation-specific and is subject to change at any time, including in periodic updates.</span></span> <span data-ttu-id="cbfa8-214">应用程序不应假设特定段的大小或依赖于此大小，也不应尝试配置段分配可用的内存量。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-214">Your app should never make assumptions about or depend on a particular segment size, nor should it attempt to configure the amount of memory available for segment allocations.</span></span>

## <a name="gcfreesegment_v1-event"></a><span data-ttu-id="cbfa8-215">GCFreeSegment_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-215">GCFreeSegment_V1 event</span></span>

<span data-ttu-id="cbfa8-216">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-216">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-217">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-217">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-218">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-218">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-219">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-219">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-220">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-220">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-221">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-221">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-222">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-222">Event</span></span>|<span data-ttu-id="cbfa8-223">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-223">Event ID</span></span>|<span data-ttu-id="cbfa8-224">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-224">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCFreeSegment_V1`|<span data-ttu-id="cbfa8-225">6</span><span class="sxs-lookup"><span data-stu-id="cbfa8-225">6</span></span>|<span data-ttu-id="cbfa8-226">已释放垃圾回收段。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-226">A garbage collection segment has been released.</span></span>|

<span data-ttu-id="cbfa8-227">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-227">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-228">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-228">Field name</span></span>|<span data-ttu-id="cbfa8-229">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-229">Data type</span></span>|<span data-ttu-id="cbfa8-230">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-230">Description</span></span>|
|----------------|---------------|-----------------|
|`Address`|`win:UInt64`|<span data-ttu-id="cbfa8-231">段的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-231">The address of the segment.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-232">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-232">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="gcrestarteebegin_v1-event"></a><span data-ttu-id="cbfa8-233">GCRestartEEBegin_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-233">GCRestartEEBegin_V1 event</span></span>

<span data-ttu-id="cbfa8-234">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-234">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-235">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-235">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-236">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-236">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-237">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-237">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-238">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-238">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-239">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-239">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-240">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-240">Event</span></span>|<span data-ttu-id="cbfa8-241">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-241">Event ID</span></span>|<span data-ttu-id="cbfa8-242">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-242">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCRestartEEBegin_V1`|<span data-ttu-id="cbfa8-243">7</span><span class="sxs-lookup"><span data-stu-id="cbfa8-243">7</span></span>|<span data-ttu-id="cbfa8-244">已开始从公共语言运行时挂起进行恢复。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-244">Resumption from common language runtime suspension has begun.</span></span>|

<span data-ttu-id="cbfa8-245">此事件没有任何事件数据。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-245">This event does not have any event data.</span></span>

## <a name="gcrestarteeend_v1-event"></a><span data-ttu-id="cbfa8-246">GCRestartEEEnd_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-246">GCRestartEEEnd_V1 event</span></span>

<span data-ttu-id="cbfa8-247">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-247">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-248">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-248">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-249">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-249">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-250">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-250">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-251">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-251">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-252">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-252">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-253">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-253">Event</span></span>|<span data-ttu-id="cbfa8-254">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-254">Event Id</span></span>|<span data-ttu-id="cbfa8-255">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-255">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCRestartEEEnd_V1`|<span data-ttu-id="cbfa8-256">3</span><span class="sxs-lookup"><span data-stu-id="cbfa8-256">3</span></span>|<span data-ttu-id="cbfa8-257">从公共语言运行时挂起进行的恢复已结束。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-257">Resumption from common language runtime suspension has ended.</span></span>|

<span data-ttu-id="cbfa8-258">此事件没有任何事件数据。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-258">This event does not have any event data.</span></span>

## <a name="gcsuspendeeend_v1-event"></a><span data-ttu-id="cbfa8-259">GCSuspendEEEnd_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-259">GCSuspendEEEnd_V1 event</span></span>

<span data-ttu-id="cbfa8-260">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-260">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-261">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-261">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-262">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-262">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-263">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-263">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-264">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-264">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-265">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-265">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-266">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-266">Event</span></span>|<span data-ttu-id="cbfa8-267">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-267">Event ID</span></span>|<span data-ttu-id="cbfa8-268">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-268">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCSuspendEEEnd_V1`|<span data-ttu-id="cbfa8-269">8</span><span class="sxs-lookup"><span data-stu-id="cbfa8-269">8</span></span>|<span data-ttu-id="cbfa8-270">结束垃圾回收执行引擎的挂起。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-270">End of suspension of the execution engine for garbage collection.</span></span>|

<span data-ttu-id="cbfa8-271">此事件没有任何事件数据。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-271">This event does not have any event data.</span></span>

## <a name="gcsuspendeebegin_v1-event"></a><span data-ttu-id="cbfa8-272">GCSuspendEEBegin_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-272">GCSuspendEEBegin_V1 event</span></span>

<span data-ttu-id="cbfa8-273">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-273">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-274">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-274">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-275">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-275">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-276">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-276">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-277">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-277">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-278">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-278">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-279">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-279">Event</span></span>|<span data-ttu-id="cbfa8-280">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-280">Event ID</span></span>|<span data-ttu-id="cbfa8-281">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-281">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCSuspendEEBegin_V1`|<span data-ttu-id="cbfa8-282">8</span><span class="sxs-lookup"><span data-stu-id="cbfa8-282">8</span></span>|<span data-ttu-id="cbfa8-283">开始挂起垃圾回收的执行引擎。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-283">Start of suspension of the execution engine for garbage collection.</span></span>|

|<span data-ttu-id="cbfa8-284">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-284">Field name</span></span>|<span data-ttu-id="cbfa8-285">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-285">Data type</span></span>|<span data-ttu-id="cbfa8-286">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-286">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|<span data-ttu-id="cbfa8-287">第 *n* 代垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-287">The *n* th garbage collection.</span></span>|
|`Reason`|`win:UInt32`|<span data-ttu-id="cbfa8-288">EE 挂起的原因。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-288">Reason for EE suspension.</span></span><br/><br/><span data-ttu-id="cbfa8-289">`0x0`：因其他事件而暂停</span><span class="sxs-lookup"><span data-stu-id="cbfa8-289">`0x0`: Suspend for Other</span></span><br/><br/><span data-ttu-id="cbfa8-290">`0x1`：因 GC 而暂停。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-290">`0x1`: Suspend for GC.</span></span><br/><br/><span data-ttu-id="cbfa8-291">`0x2`：因 AppDomain 关闭而暂停。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-291">`0x2`: Suspend for AppDomain shutdown.</span></span><br/><br/><span data-ttu-id="cbfa8-292">`0x3`：因代码间距调整而暂停。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-292">`0x3`: Suspend for code pitching.</span></span><br/><br/><span data-ttu-id="cbfa8-293">`0x4`：因关闭而暂停。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-293">`0x4`: Suspend for shutdown.</span></span><br/><br/><span data-ttu-id="cbfa8-294">`0x5`：因调试器而暂停。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-294">`0x5`: Suspend for debugger.</span></span><br/><br/><span data-ttu-id="cbfa8-295">`0x6`：因 GC 准备而暂停。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-295">`0x6`: Suspend for GC Prep.</span></span><br/><br/><span data-ttu-id="cbfa8-296">`0x7`：因调试程序扫描而暂停</span><span class="sxs-lookup"><span data-stu-id="cbfa8-296">`0x7`: Suspend for debugger sweep</span></span>|

## <a name="gcallocationtick_v3-event"></a><span data-ttu-id="cbfa8-297">GCAllocationTick_V3 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-297">GCAllocationTick_V3 event</span></span>

<span data-ttu-id="cbfa8-298">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-298">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-299">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-299">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-300">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-300">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-301">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-301">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-302">详细 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-302">Verbose (4)</span></span>|

<span data-ttu-id="cbfa8-303">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-303">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-304">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-304">Event</span></span>|<span data-ttu-id="cbfa8-305">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-305">Event ID</span></span>|<span data-ttu-id="cbfa8-306">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-306">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCAllocationTick_V3`|<span data-ttu-id="cbfa8-307">10</span><span class="sxs-lookup"><span data-stu-id="cbfa8-307">10</span></span>|<span data-ttu-id="cbfa8-308">每次大约分配 100 KB。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-308">Each time approximately 100 KB is allocated.</span></span>|

<span data-ttu-id="cbfa8-309">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-309">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-310">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-310">Field name</span></span>|<span data-ttu-id="cbfa8-311">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-311">Data type</span></span>|<span data-ttu-id="cbfa8-312">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-312">Description</span></span>|
|----------------|---------------|-----------------|
|`AllocationAmount`|`win:UInt32`|<span data-ttu-id="cbfa8-313">分配大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-313">The allocation size, in bytes.</span></span> <span data-ttu-id="cbfa8-314">对于小于 ULONG（4,294,967,295 字节）长度的分配，此值为精确值。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-314">This value is accurate for allocations that are less than the length of a ULONG (4,294,967,295 bytes).</span></span> <span data-ttu-id="cbfa8-315">如果分配长度更大，则此字段包含了截断的值。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-315">If the allocation is greater, this field contains a truncated value.</span></span> <span data-ttu-id="cbfa8-316">对于非常大的分配使用 `AllocationAmount64` 。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-316">Use `AllocationAmount64` for very large allocations.</span></span>|
|`AllocationKind`|`win:UInt32`|<span data-ttu-id="cbfa8-317">`0x0` - 小型对象分配（小型对象堆中的分配）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-317">`0x0` - Small object allocation (allocation is in small object heap).</span></span><br /><br /> <span data-ttu-id="cbfa8-318">`0x1` - 大型对象分配（大型对象堆中的分配）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-318">`0x1` - Large object allocation (allocation is in large object heap).</span></span>|
|`AllocationAmount64`|`win:UInt64`|<span data-ttu-id="cbfa8-319">分配大小（以字节为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-319">The allocation size, in bytes.</span></span> <span data-ttu-id="cbfa8-320">对于非常大的分配，此值为精确值。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-320">This value is accurate for very large allocations.</span></span>|
|`TypeId`|`win:Pointer`|<span data-ttu-id="cbfa8-321">MethodTable 的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-321">The address of the MethodTable.</span></span> <span data-ttu-id="cbfa8-322">如果在此事件期间分配了几种类型的对象，则此地址为对应于分配的最后一个对象（导致超过 100 KB 阙值的对象）的 MethodTable 地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-322">When there are several types of objects that were allocated during this event, this is the address of the MethodTable that corresponds to the last object allocated (the object that caused the 100 KB threshold to be exceeded).</span></span>|
|`TypeName`|`win:UnicodeString`|<span data-ttu-id="cbfa8-323">已分配的类型的名称。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-323">The name of the type that was allocated.</span></span> <span data-ttu-id="cbfa8-324">如果在此事件期间分配了几种类型的对象，则此地址为对应于分配的最后一个类型（导致超过 100 KB 阙值的对象）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-324">When there are several types of objects that were allocated during this event, this is the type of the last object allocated (the object that caused the 100 KB threshold to be exceeded).</span></span>|
|`HeapIndex`|`win:UInt32`|<span data-ttu-id="cbfa8-325">此对象所分配到的堆。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-325">The heap where the object was allocated.</span></span> <span data-ttu-id="cbfa8-326">与工作站垃圾回收一起运行时，此值为 0（零）。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-326">This value is 0 (zero) when running with workstation garbage collection.</span></span>|
|`Address`|`win:Pointer`|<span data-ttu-id="cbfa8-327">上次分配的对象的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-327">The address of the last allocated object.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-328">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-328">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="gccreateconcurrentthread_v1-event"></a><span data-ttu-id="cbfa8-329">GCCreateConcurrentThread_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-329">GCCreateConcurrentThread_V1 event</span></span>

<span data-ttu-id="cbfa8-330">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-330">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-331">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-331">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-332">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-332">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-333">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-333">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-334">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-334">Informational (4)</span></span>|
|<span data-ttu-id="cbfa8-335">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-335">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="cbfa8-336">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-336">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-337">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-337">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-338">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-338">Event</span></span>|<span data-ttu-id="cbfa8-339">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-339">Event ID</span></span>|<span data-ttu-id="cbfa8-340">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-340">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCCreateConcurrentThread_V1`|<span data-ttu-id="cbfa8-341">11</span><span class="sxs-lookup"><span data-stu-id="cbfa8-341">11</span></span>|<span data-ttu-id="cbfa8-342">已创建并发垃圾回收线程。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-342">Concurrent garbage collection thread was created.</span></span>|

<span data-ttu-id="cbfa8-343">此事件没有任何事件数据。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-343">This event does not have any event data.</span></span>

## <a name="gcterminateconcurrentthread_v1-event"></a><span data-ttu-id="cbfa8-344">GCTerminateConcurrentThread_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-344">GCTerminateConcurrentThread_V1 event</span></span>

<span data-ttu-id="cbfa8-345">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-345">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-346">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-346">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-347">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-347">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-348">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-348">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-349">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-349">Informational (4)</span></span>|
|<span data-ttu-id="cbfa8-350">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-350">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="cbfa8-351">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-351">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-352">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-352">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-353">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-353">Event</span></span>|<span data-ttu-id="cbfa8-354">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-354">Event ID</span></span>|<span data-ttu-id="cbfa8-355">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-355">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCTerminateConcurrentThread_V1`|<span data-ttu-id="cbfa8-356">12</span><span class="sxs-lookup"><span data-stu-id="cbfa8-356">12</span></span>|<span data-ttu-id="cbfa8-357">已终止并发垃圾回收线程。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-357">Concurrent garbage collection thread was terminated.</span></span>|

<span data-ttu-id="cbfa8-358">此事件没有任何事件数据。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-358">This event does not have any event data.</span></span>

## <a name="gcfinalizersbegin_v1-event"></a><span data-ttu-id="cbfa8-359">GCFinalizersBegin_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-359">GCFinalizersBegin_V1 event</span></span>

<span data-ttu-id="cbfa8-360">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-360">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-361">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-361">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-362">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-362">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-363">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-363">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-364">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-364">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-365">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-365">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-366">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-366">Event</span></span>|<span data-ttu-id="cbfa8-367">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-367">Event ID</span></span>|<span data-ttu-id="cbfa8-368">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-368">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCFinalizersBegin_V1`|<span data-ttu-id="cbfa8-369">14</span><span class="sxs-lookup"><span data-stu-id="cbfa8-369">14</span></span>|<span data-ttu-id="cbfa8-370">开始运行终结器。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-370">The start of running finalizers.</span></span>|

<span data-ttu-id="cbfa8-371">此事件没有任何事件数据。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-371">This event does not have any event data.</span></span>

## <a name="gcfinalizersend_v1-event"></a><span data-ttu-id="cbfa8-372">GCFinalizersEnd_V1 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-372">GCFinalizersEnd_V1 event</span></span>

<span data-ttu-id="cbfa8-373">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-373">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-374">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-374">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-375">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-375">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-376">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-376">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-377">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-377">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-378">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-378">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-379">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-379">Event</span></span>|<span data-ttu-id="cbfa8-380">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-380">Event ID</span></span>|<span data-ttu-id="cbfa8-381">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-381">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCFinalizersEnd_V1`|<span data-ttu-id="cbfa8-382">13</span><span class="sxs-lookup"><span data-stu-id="cbfa8-382">13</span></span>|<span data-ttu-id="cbfa8-383">结束运行终结器。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-383">The end of running finalizers.</span></span>|

<span data-ttu-id="cbfa8-384">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-384">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-385">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-385">Field name</span></span>|<span data-ttu-id="cbfa8-386">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-386">Data type</span></span>|<span data-ttu-id="cbfa8-387">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-387">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|<span data-ttu-id="cbfa8-388">运行的终结器数。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-388">The number of finalizers that were run.</span></span>|
|`ClrInstanceID`|<span data-ttu-id="cbfa8-389">win:UInt16</span><span class="sxs-lookup"><span data-stu-id="cbfa8-389">win:UInt16</span></span>|<span data-ttu-id="cbfa8-390">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-390">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="setgchandle-event"></a><span data-ttu-id="cbfa8-391">SetGCHandle 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-391">SetGCHandle event</span></span>

<span data-ttu-id="cbfa8-392">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-392">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-393">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-393">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-394">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-394">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-395">`GCHandleKeyword` (0x2)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-395">`GCHandleKeyword` (0x2)</span></span>|<span data-ttu-id="cbfa8-396">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-396">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-397">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-397">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-398">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-398">Event</span></span>|<span data-ttu-id="cbfa8-399">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-399">Event ID</span></span>|<span data-ttu-id="cbfa8-400">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-400">Raised when</span></span>|
|-----------|--------------|-----------------|
|`SetGCHandle`|<span data-ttu-id="cbfa8-401">30</span><span class="sxs-lookup"><span data-stu-id="cbfa8-401">30</span></span>|<span data-ttu-id="cbfa8-402">已设置 GC 句柄。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-402">A GC Handle has been set.</span></span>|

<span data-ttu-id="cbfa8-403">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-403">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-404">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-404">Field name</span></span>|<span data-ttu-id="cbfa8-405">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-405">Data type</span></span>|<span data-ttu-id="cbfa8-406">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-406">Description</span></span>|
|----------------|---------------|-----------------|
|`HandleID`|`win:Pointer`|<span data-ttu-id="cbfa8-407">已分配的句柄的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-407">The address of the allocated handle.</span></span>|
|`ObjectID`|`win:Pointer`|<span data-ttu-id="cbfa8-408">创建了句柄的对象的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-408">The address of the object whose handle was created.</span></span>|
|`Kind`|`win:UInt32`|<span data-ttu-id="cbfa8-409">已设置的 GC 句柄的类型。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-409">The type of GC handle that was set.</span></span> <br /><br /><span data-ttu-id="cbfa8-410">`0x0`：WeakShort</span><span class="sxs-lookup"><span data-stu-id="cbfa8-410">`0x0`: WeakShort</span></span> <br/><br/><span data-ttu-id="cbfa8-411">`0x1`：WeakLong</span><span class="sxs-lookup"><span data-stu-id="cbfa8-411">`0x1`: WeakLong</span></span> <br /><br /><span data-ttu-id="cbfa8-412">`0x2`：Strong</span><span class="sxs-lookup"><span data-stu-id="cbfa8-412">`0x2`: Strong</span></span> <br /><br /><span data-ttu-id="cbfa8-413">`0x3`：Pinned</span><span class="sxs-lookup"><span data-stu-id="cbfa8-413">`0x3`: Pinned</span></span> <br /><br /><span data-ttu-id="cbfa8-414">`0x4`：Variable</span><span class="sxs-lookup"><span data-stu-id="cbfa8-414">`0x4`: Variable</span></span><br /><br /><span data-ttu-id="cbfa8-415">`0x5`：RefCounted</span><span class="sxs-lookup"><span data-stu-id="cbfa8-415">`0x5`: RefCounted</span></span> <br /><br /><span data-ttu-id="cbfa8-416">`0x6`：Dependent</span><span class="sxs-lookup"><span data-stu-id="cbfa8-416">`0x6`: Dependent</span></span><br /><br /><span data-ttu-id="cbfa8-417">`0x7`：AsyncPinned</span><span class="sxs-lookup"><span data-stu-id="cbfa8-417">`0x7`: AsyncPinned</span></span><br /><br /><span data-ttu-id="cbfa8-418">`0x8`：SizedRef</span><span class="sxs-lookup"><span data-stu-id="cbfa8-418">`0x8`: SizedRef</span></span>|
|`Generation`|`win:UInt32`|<span data-ttu-id="cbfa8-419">创建了其句柄的对象的生成。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-419">The generation of the object whose handle was created.</span></span>|
|`AppDomainID`|`win:UInt64`|<span data-ttu-id="cbfa8-420">AppDomain ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-420">The AppDomain ID.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-421">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-421">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="destroygchandle-event"></a><span data-ttu-id="cbfa8-422">DestroyGCHandle 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-422">DestroyGCHandle event</span></span>

<span data-ttu-id="cbfa8-423">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-423">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-424">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-424">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-425">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-425">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-426">`GCHandleKeyword` (0x2)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-426">`GCHandleKeyword` (0x2)</span></span>|<span data-ttu-id="cbfa8-427">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-427">Informational (4)</span></span>|

<span data-ttu-id="cbfa8-428">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-428">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-429">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-429">Event</span></span>|<span data-ttu-id="cbfa8-430">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-430">Event ID</span></span>|<span data-ttu-id="cbfa8-431">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-431">Raised when</span></span>|
|-----------|--------------|-----------------|
|`DestroyGCHandle`|<span data-ttu-id="cbfa8-432">31</span><span class="sxs-lookup"><span data-stu-id="cbfa8-432">31</span></span>|<span data-ttu-id="cbfa8-433">已销毁 GC 句柄。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-433">A GC Handle is destroyed.</span></span>|

<span data-ttu-id="cbfa8-434">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-434">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-435">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-435">Field name</span></span>|<span data-ttu-id="cbfa8-436">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-436">Data type</span></span>|<span data-ttu-id="cbfa8-437">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-437">Description</span></span>|
|----------------|---------------|-----------------|
|`HandleID`|`win:Pointer`|<span data-ttu-id="cbfa8-438">已销毁的句柄的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-438">The address of the destroyed handle.</span></span>|
|`ClrInstanceID`|<span data-ttu-id="cbfa8-439">win:UInt16</span><span class="sxs-lookup"><span data-stu-id="cbfa8-439">win:UInt16</span></span>|<span data-ttu-id="cbfa8-440">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-440">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="pinobjectatgctime-event"></a><span data-ttu-id="cbfa8-441">PinObjectAtGCTime 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-441">PinObjectAtGCTime event</span></span>

<span data-ttu-id="cbfa8-442">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-442">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-443">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-443">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-444">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-444">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-445">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-445">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-446">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-446">Verbose (5)</span></span>|

<span data-ttu-id="cbfa8-447">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-447">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-448">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-448">Event</span></span>|<span data-ttu-id="cbfa8-449">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-449">Event ID</span></span>|<span data-ttu-id="cbfa8-450">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-450">Raised when</span></span>|
|-----------|--------------|-----------------|
|`PinObjectAtGCTime`|<span data-ttu-id="cbfa8-451">33</span><span class="sxs-lookup"><span data-stu-id="cbfa8-451">33</span></span>|<span data-ttu-id="cbfa8-452">对象在 GC 期间已固定。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-452">An object was pinned during a GC.</span></span>|

<span data-ttu-id="cbfa8-453">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-453">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-454">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-454">Field name</span></span>|<span data-ttu-id="cbfa8-455">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-455">Data type</span></span>|<span data-ttu-id="cbfa8-456">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-456">Description</span></span>|
|----------------|---------------|-----------------|
|`HandleID`|`win:Pointer`|<span data-ttu-id="cbfa8-457">句柄的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-457">The address of the handle.</span></span>|
|`ObjectID`|`win:Pointer`|<span data-ttu-id="cbfa8-458">已固定对象的地址。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-458">The address of the pinned object.</span></span>|
|`ObjectSize`|`win:UInt64`|<span data-ttu-id="cbfa8-459">已固定对象的大小。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-459">The size of the pinned object.</span></span>|
|`TypeName`|`win:UnicodeString`|<span data-ttu-id="cbfa8-460">已固定对象的类型的名称。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-460">The name of the type of the pinned object.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-461">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-461">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="gctriggered-event"></a><span data-ttu-id="cbfa8-462">GCTriggered 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-462">GCTriggered event</span></span>

<span data-ttu-id="cbfa8-463">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-463">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-464">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-464">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-465">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-465">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-466">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-466">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-467">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-467">Verbose (5)</span></span>|

<span data-ttu-id="cbfa8-468">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-468">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-469">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-469">Event</span></span>|<span data-ttu-id="cbfa8-470">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-470">Event ID</span></span>|<span data-ttu-id="cbfa8-471">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-471">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCTriggered`|<span data-ttu-id="cbfa8-472">33</span><span class="sxs-lookup"><span data-stu-id="cbfa8-472">33</span></span>|<span data-ttu-id="cbfa8-473">已触发 GC。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-473">A GC has been triggered.</span></span>|

<span data-ttu-id="cbfa8-474">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-474">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-475">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-475">Field name</span></span>|<span data-ttu-id="cbfa8-476">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-476">Data type</span></span>|<span data-ttu-id="cbfa8-477">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-477">Description</span></span>|
|----------------|---------------|-----------------|
|`Reason`|`win:UInt32`|<span data-ttu-id="cbfa8-478">触发 GC 的原因。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-478">The reason a GC was triggered.</span></span><br/><br/><span data-ttu-id="cbfa8-479">`0x0`：AllocSmall</span><span class="sxs-lookup"><span data-stu-id="cbfa8-479">`0x0`: AllocSmall</span></span><br/><br/><span data-ttu-id="cbfa8-480">`0x1`：Induced</span><span class="sxs-lookup"><span data-stu-id="cbfa8-480">`0x1`: Induced</span></span> <br/><br/><span data-ttu-id="cbfa8-481">`0x2`：LowMemory</span><span class="sxs-lookup"><span data-stu-id="cbfa8-481">`0x2`: LowMemory</span></span> <br/><br/><span data-ttu-id="cbfa8-482">`0x3`：Empty</span><span class="sxs-lookup"><span data-stu-id="cbfa8-482">`0x3`: Empty</span></span> <br/><br/><span data-ttu-id="cbfa8-483">`0x4`：AllocLarge</span><span class="sxs-lookup"><span data-stu-id="cbfa8-483">`0x4`: AllocLarge</span></span> <br/><br/><span data-ttu-id="cbfa8-484">`0x5`：OutOfSpaceSmallObjectHeap</span><span class="sxs-lookup"><span data-stu-id="cbfa8-484">`0x5`: OutOfSpaceSmallObjectHeap</span></span> <br/><br/><span data-ttu-id="cbfa8-485">`0x6`：OutOfSpaceLargeObjectHeap</span><span class="sxs-lookup"><span data-stu-id="cbfa8-485">`0x6`: OutOfSpaceLargeObjectHeap</span></span> <br/><br/><span data-ttu-id="cbfa8-486">`0x7`：InducedNoForce</span><span class="sxs-lookup"><span data-stu-id="cbfa8-486">`0x7`:InducedNoForce</span></span> <br/><br/><span data-ttu-id="cbfa8-487">`0x8`：Stress</span><span class="sxs-lookup"><span data-stu-id="cbfa8-487">`0x8`: Stress</span></span> <br/><br/><span data-ttu-id="cbfa8-488">`0x9`：InducedLowMemory</span><span class="sxs-lookup"><span data-stu-id="cbfa8-488">`0x9`: InducedLowMemory</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-489">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-489">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="increasememorypressure-event"></a><span data-ttu-id="cbfa8-490">IncreaseMemoryPressure 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-490">IncreaseMemoryPressure event</span></span>

<span data-ttu-id="cbfa8-491">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-491">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-492">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-492">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-493">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-493">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-494">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-494">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-495">信息 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-495">Information (4)</span></span>|

<span data-ttu-id="cbfa8-496">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-496">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-497">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-497">Event</span></span>|<span data-ttu-id="cbfa8-498">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-498">Event ID</span></span>|<span data-ttu-id="cbfa8-499">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-499">Raised when</span></span>|
|----------------|---------------|-----------------|
|`IncreaseMemoryPressure`|<span data-ttu-id="cbfa8-500">200</span><span class="sxs-lookup"><span data-stu-id="cbfa8-500">200</span></span>|<span data-ttu-id="cbfa8-501">内存压力增加。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-501">Memory pressure was increased.</span></span>|

<span data-ttu-id="cbfa8-502">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-502">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-503">字段名称</span><span class="sxs-lookup"><span data-stu-id="cbfa8-503">Field Name</span></span>|<span data-ttu-id="cbfa8-504">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-504">Data type</span></span>|<span data-ttu-id="cbfa8-505">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-505">Description</span></span>|
|----------------|---------------|-----------------|
|`ClrInstanceID`|<span data-ttu-id="cbfa8-506">win:UInt16</span><span class="sxs-lookup"><span data-stu-id="cbfa8-506">win:UInt16</span></span>|<span data-ttu-id="cbfa8-507">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-507">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="decreasememorypressure-event"></a><span data-ttu-id="cbfa8-508">DecreaseMemoryPressure 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-508">DecreaseMemoryPressure event</span></span>

<span data-ttu-id="cbfa8-509">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-509">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-510">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-510">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-511">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-511">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-512">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-512">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-513">信息 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-513">Information (4)</span></span>|

<span data-ttu-id="cbfa8-514">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-514">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-515">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-515">Event</span></span>|<span data-ttu-id="cbfa8-516">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-516">Event ID</span></span>|<span data-ttu-id="cbfa8-517">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-517">Raised when</span></span>|
|----------------|---------------|-----------------|
|`DecreaseMemoryPressure`|<span data-ttu-id="cbfa8-518">201</span><span class="sxs-lookup"><span data-stu-id="cbfa8-518">201</span></span>|<span data-ttu-id="cbfa8-519">内存压力降低。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-519">Memory pressure was decreased.</span></span>|

<span data-ttu-id="cbfa8-520">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-520">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-521">字段名称</span><span class="sxs-lookup"><span data-stu-id="cbfa8-521">Field Name</span></span>|<span data-ttu-id="cbfa8-522">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-522">Data type</span></span>|<span data-ttu-id="cbfa8-523">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-523">Description</span></span>|
|----------------|---------------|-----------------|
|`BytesFreed`|`win:UInt32`|<span data-ttu-id="cbfa8-524">已释放字节。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-524">Bytes freed.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-525">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-525">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="gcmarkwithtype-event"></a><span data-ttu-id="cbfa8-526">GCMarkWithType 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-526">GCMarkWithType event</span></span>

<span data-ttu-id="cbfa8-527">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-527">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-528">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-528">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-529">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-529">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-530">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-530">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-531">信息 (4)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-531">Information (4)</span></span>|

<span data-ttu-id="cbfa8-532">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-532">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-533">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-533">Event</span></span>|<span data-ttu-id="cbfa8-534">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-534">Event ID</span></span>|<span data-ttu-id="cbfa8-535">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-535">Raised when</span></span>|
|----------------|---------------|-----------------|
|`GCMarkWithType`|<span data-ttu-id="cbfa8-536">202</span><span class="sxs-lookup"><span data-stu-id="cbfa8-536">202</span></span>|<span data-ttu-id="cbfa8-537">GC 标记阶段期间已标记了一个 GC 根。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-537">A GC root has been marked during GC mark phase.</span></span>|

<span data-ttu-id="cbfa8-538">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-538">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-539">字段名称</span><span class="sxs-lookup"><span data-stu-id="cbfa8-539">Field Name</span></span>|<span data-ttu-id="cbfa8-540">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-540">Data type</span></span>|<span data-ttu-id="cbfa8-541">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-541">Description</span></span>|
|----------------|---------------|-----------------|
|`HeapNum`|`win:UInt32`|<span data-ttu-id="cbfa8-542">堆号。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-542">The heap number.</span></span>|
|`ClrInstanceID`|<span data-ttu-id="cbfa8-543">win:UInt16</span><span class="sxs-lookup"><span data-stu-id="cbfa8-543">win:UInt16</span></span>|<span data-ttu-id="cbfa8-544">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-544">Unique ID for the instance of CoreCLR.</span></span>|
|`Type`|`win:UInt32`|<span data-ttu-id="cbfa8-545">GC 根类型。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-545">The GC root type.</span></span><br/><br/><span data-ttu-id="cbfa8-546">`0x0`：Stack</span><span class="sxs-lookup"><span data-stu-id="cbfa8-546">`0x0`: Stack</span></span><br/><br/><span data-ttu-id="cbfa8-547">`0x1`：Finalizer</span><span class="sxs-lookup"><span data-stu-id="cbfa8-547">`0x1`: Finalizer</span></span><br/><br/><span data-ttu-id="cbfa8-548">`0x2`：Handle</span><span class="sxs-lookup"><span data-stu-id="cbfa8-548">`0x2`: Handle</span></span><br/><br/><span data-ttu-id="cbfa8-549">`0x3`：Older</span><span class="sxs-lookup"><span data-stu-id="cbfa8-549">`0x3`: Older</span></span><br/><br/><span data-ttu-id="cbfa8-550">`0x4`：SizedRef</span><span class="sxs-lookup"><span data-stu-id="cbfa8-550">`0x4`: SizedRef</span></span><br/><br/><span data-ttu-id="cbfa8-551">`0x5`：Overflow</span><span class="sxs-lookup"><span data-stu-id="cbfa8-551">`0x5`: Overflow</span></span><br/><br/>|
|`Bytes`|`win:UInt64`|<span data-ttu-id="cbfa8-552">已标记的字节数。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-552">The number of bytes marked.</span></span>|

## <a name="gcjoin_v2-event"></a><span data-ttu-id="cbfa8-553">GCJoin_V2 事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-553">GCJoin_V2 event</span></span>

<span data-ttu-id="cbfa8-554">下表显示了关键字和级别：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-554">The following table shows the keyword and level:</span></span>

|<span data-ttu-id="cbfa8-555">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="cbfa8-555">Keyword for raising the event</span></span>|<span data-ttu-id="cbfa8-556">Level</span><span class="sxs-lookup"><span data-stu-id="cbfa8-556">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="cbfa8-557">`GCKeyword` (0x1)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-557">`GCKeyword` (0x1)</span></span>|<span data-ttu-id="cbfa8-558">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="cbfa8-558">Verbose (5)</span></span>|

<span data-ttu-id="cbfa8-559">下表显示了事件信息：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-559">The following table shows the event information:</span></span>

|<span data-ttu-id="cbfa8-560">事件</span><span class="sxs-lookup"><span data-stu-id="cbfa8-560">Event</span></span>|<span data-ttu-id="cbfa8-561">事件 ID</span><span class="sxs-lookup"><span data-stu-id="cbfa8-561">Event ID</span></span>|<span data-ttu-id="cbfa8-562">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="cbfa8-562">Raised when</span></span>|
|-----------|--------------|-----------------|
|`GCJoin_V2`|<span data-ttu-id="cbfa8-563">203</span><span class="sxs-lookup"><span data-stu-id="cbfa8-563">203</span></span>|<span data-ttu-id="cbfa8-564">已联接的 GC 线程。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-564">A GC thread joined.</span></span>|

<span data-ttu-id="cbfa8-565">下表显示了事件数据：</span><span class="sxs-lookup"><span data-stu-id="cbfa8-565">The following table shows the event data:</span></span>

|<span data-ttu-id="cbfa8-566">字段名</span><span class="sxs-lookup"><span data-stu-id="cbfa8-566">Field name</span></span>|<span data-ttu-id="cbfa8-567">数据类型</span><span class="sxs-lookup"><span data-stu-id="cbfa8-567">Data type</span></span>|<span data-ttu-id="cbfa8-568">说明</span><span class="sxs-lookup"><span data-stu-id="cbfa8-568">Description</span></span>|
|----------------|---------------|-----------------|
|`Heap`|`win:UInt32`|<span data-ttu-id="cbfa8-569">堆号</span><span class="sxs-lookup"><span data-stu-id="cbfa8-569">The heap number</span></span>|
|`JoinTime`|`win:UInt32`|<span data-ttu-id="cbfa8-570">指示在联接开始或结束时是否触发此事件（`0x0` 为联接开始，`0x1` 为联接结束）</span><span class="sxs-lookup"><span data-stu-id="cbfa8-570">Indicates whether this event is fired at the start of a join or end of a join (`0x0` for join start, `0x1` for join end)</span></span>|
|`JoinType`|`win:UInt32`|<span data-ttu-id="cbfa8-571">联接类型。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-571">The join type.</span></span> <br/><br/><span data-ttu-id="cbfa8-572">`0x0`：Last Join</span><span class="sxs-lookup"><span data-stu-id="cbfa8-572">`0x0`: Last Join</span></span><br/><br/><span data-ttu-id="cbfa8-573">`0x1`：Join</span><span class="sxs-lookup"><span data-stu-id="cbfa8-573">`0x1`: Join</span></span> <br/><br/><span data-ttu-id="cbfa8-574">`0x2`：Restart</span><span class="sxs-lookup"><span data-stu-id="cbfa8-574">`0x2`: Restart</span></span> <br/><br/><span data-ttu-id="cbfa8-575">`0x3`：First Reverse Join</span><span class="sxs-lookup"><span data-stu-id="cbfa8-575">`0x3`: First Reverse Join</span></span><br/><br/><span data-ttu-id="cbfa8-576">`0x4`：Reverse Join</span><span class="sxs-lookup"><span data-stu-id="cbfa8-576">`0x4`: Reverse Join</span></span><br/><br/>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="cbfa8-577">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="cbfa8-577">Unique ID for the instance of CoreCLR.</span></span>|
