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
# <a name="net-runtime-garbage-collection-events"></a>.NET 运行时垃圾回收事件

这些事件可收集有关垃圾回收的信息。 它们有助于诊断和调试，包括确定执行垃圾回收的次数、垃圾回收期间释放的内存量等。有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)

## <a name="gcstart_v2-event"></a>GCStart_V2 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCStart_V1`|1|垃圾回收已启动。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|第 *n* 代垃圾回收。|
|`Depth`|`win:UInt32`|正在回收的代。|
|`Reason`|`win:UInt32`|垃圾回收的触发原因：<br /><br /> `0x0` - 小型对象堆分配。<br /><br /> `0x1` - 已引发。<br /><br /> `0x2` - 内存不足。<br /><br /> `0x3` - 空。<br /><br /> `0x4` - 大型对象堆分配。<br /><br /> `0x5` - 空间外（针对小型对象堆）。<br /><br /> `0x6` - 空间外（针对大型对象堆）。<br /><br /> `0x7` - 已引发，但不是强制作为阻塞。|
|`Type`|`win:UInt32`|`0x0` - 后台垃圾回收外发生了阻止垃圾回收。<br /><br /> `0x1` - 后台垃圾回收。<br /><br /> `0x2` - 后台垃圾回收时发生了阻止垃圾回收。|
|`ClrInstanceID`|win:UInt16|CoreCLR 实例的唯一 ID。|

## <a name="gcend_v1-event"></a>GCEnd_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCEnd_V1`|2|垃圾回收已结束。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|第 *n* 代垃圾回收。|
|`Depth`|`win:UInt32`|已回收的代。|
|`ClrInstanceID`|win:UInt16|CoreCLR 实例的唯一 ID。|

## <a name="gcheapstats_v2-event"></a>GCHeapStats_V2 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`GCHeapStats_V2`|4|在每次垃圾回收结束时显示堆统计信息。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`GenerationSize0`|`win:UInt64`|第 0 代内存的大小（以字节为单位）。|
|`TotalPromotedSize0`|`win:UInt64`|从第 0 代提升到第 1 代的字节数。|
|`GenerationSize1`|`win:UInt64`|第 1 代内存的大小（以字节为单位）。|
|`TotalPromotedSize1`|`win:UInt64`|从第 1 代提升到第 2 代的字节数。|
|`GenerationSize2`|`win:UInt64`|第 2 代内存的大小（以字节为单位）。|
|`TotalPromotedSize2`|`win:UInt64`|上次回收后仍存在于第 2 代中的字节数。|
|`GenerationSize3`|`win:UInt64`|大型对象堆的大小（以字节为单位）。|
|`TotalPromotedSize3`|`win:UInt64`|上次回收后仍存在于大型对象堆中的字节数。|
|`FinalizationPromotedSize`|`win:UInt64`|准备终结的对象的总大小（以字节为单位）。|
|`FinalizationPromotedCount`|`win:UInt64`|已准备好进行终结的对象的数目。|
|`PinnedObjectCount`|`win:UInt32`|固定（不可移动）对象的数目。|
|`SinkBlockCount`|`win:UInt32`|正在使用的同步块的数目。|
|`GCHandleCount`|`win:UInt32`|使用中的垃圾回收句柄的数目。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|
|`GenerationSize4`|`win:UInt64`|已固定对象堆的大小（以字节为单位）。|
|`TotalPromotedSize4`|`win:UInt64`|上次回收后仍存在于固定对象堆中的字节数。|

## <a name="gccreatesegment_v1-event"></a>GCCreateSegment_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCCreateSegment_V1`|5|已创建的新的垃圾回收段。 此外，当在已运行的进程中启用跟踪时，将为每个现有段引发此事件。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Address`|`win:UInt64`|段的地址。|
|`Size`|`win:UInt64`|段的大小。|
|`Type`|`win:UInt32`|0x0 - 小型对象堆。<br /><br /> 0x0 - 大型对象堆。<br /><br /> 0x2 - 只读堆。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

请注意，由垃圾回收器分配的段的大小特定于实现，且随时可能更改（包括定期更新）。 应用程序不应假设特定段的大小或依赖于此大小，也不应尝试配置段分配可用的内存量。

## <a name="gcfreesegment_v1-event"></a>GCFreeSegment_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCFreeSegment_V1`|6|已释放垃圾回收段。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Address`|`win:UInt64`|段的地址。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="gcrestarteebegin_v1-event"></a>GCRestartEEBegin_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCRestartEEBegin_V1`|7|已开始从公共语言运行时挂起进行恢复。|

此事件没有任何事件数据。

## <a name="gcrestarteeend_v1-event"></a>GCRestartEEEnd_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCRestartEEEnd_V1`|3|从公共语言运行时挂起进行的恢复已结束。|

此事件没有任何事件数据。

## <a name="gcsuspendeeend_v1-event"></a>GCSuspendEEEnd_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCSuspendEEEnd_V1`|8|结束垃圾回收执行引擎的挂起。|

此事件没有任何事件数据。

## <a name="gcsuspendeebegin_v1-event"></a>GCSuspendEEBegin_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCSuspendEEBegin_V1`|8|开始挂起垃圾回收的执行引擎。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|第 *n* 代垃圾回收。|
|`Reason`|`win:UInt32`|EE 挂起的原因。<br/><br/>`0x0`：因其他事件而暂停<br/><br/>`0x1`：因 GC 而暂停。<br/><br/>`0x2`：因 AppDomain 关闭而暂停。<br/><br/>`0x3`：因代码间距调整而暂停。<br/><br/>`0x4`：因关闭而暂停。<br/><br/>`0x5`：因调试器而暂停。<br/><br/>`0x6`：因 GC 准备而暂停。<br/><br/>`0x7`：因调试程序扫描而暂停|

## <a name="gcallocationtick_v3-event"></a>GCAllocationTick_V3 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|详细 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCAllocationTick_V3`|10|每次大约分配 100 KB。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AllocationAmount`|`win:UInt32`|分配大小（以字节为单位）。 对于小于 ULONG（4,294,967,295 字节）长度的分配，此值为精确值。 如果分配长度更大，则此字段包含了截断的值。 对于非常大的分配使用 `AllocationAmount64` 。|
|`AllocationKind`|`win:UInt32`|`0x0` - 小型对象分配（小型对象堆中的分配）。<br /><br /> `0x1` - 大型对象分配（大型对象堆中的分配）。|
|`AllocationAmount64`|`win:UInt64`|分配大小（以字节为单位）。 对于非常大的分配，此值为精确值。|
|`TypeId`|`win:Pointer`|MethodTable 的地址。 如果在此事件期间分配了几种类型的对象，则此地址为对应于分配的最后一个对象（导致超过 100 KB 阙值的对象）的 MethodTable 地址。|
|`TypeName`|`win:UnicodeString`|已分配的类型的名称。 如果在此事件期间分配了几种类型的对象，则此地址为对应于分配的最后一个类型（导致超过 100 KB 阙值的对象）。|
|`HeapIndex`|`win:UInt32`|此对象所分配到的堆。 与工作站垃圾回收一起运行时，此值为 0（零）。|
|`Address`|`win:Pointer`|上次分配的对象的地址。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="gccreateconcurrentthread_v1-event"></a>GCCreateConcurrentThread_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCCreateConcurrentThread_V1`|11|已创建并发垃圾回收线程。|

此事件没有任何事件数据。

## <a name="gcterminateconcurrentthread_v1-event"></a>GCTerminateConcurrentThread_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCTerminateConcurrentThread_V1`|12|已终止并发垃圾回收线程。|

此事件没有任何事件数据。

## <a name="gcfinalizersbegin_v1-event"></a>GCFinalizersBegin_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCFinalizersBegin_V1`|14|开始运行终结器。|

此事件没有任何事件数据。

## <a name="gcfinalizersend_v1-event"></a>GCFinalizersEnd_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCFinalizersEnd_V1`|13|结束运行终结器。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt32`|运行的终结器数。|
|`ClrInstanceID`|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="setgchandle-event"></a>SetGCHandle 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCHandleKeyword` (0x2)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`SetGCHandle`|30|已设置 GC 句柄。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`HandleID`|`win:Pointer`|已分配的句柄的地址。|
|`ObjectID`|`win:Pointer`|创建了句柄的对象的地址。|
|`Kind`|`win:UInt32`|已设置的 GC 句柄的类型。 <br /><br />`0x0`：WeakShort <br/><br/>`0x1`：WeakLong <br /><br />`0x2`：Strong <br /><br />`0x3`：Pinned <br /><br />`0x4`：Variable<br /><br />`0x5`：RefCounted <br /><br />`0x6`：Dependent<br /><br />`0x7`：AsyncPinned<br /><br />`0x8`：SizedRef|
|`Generation`|`win:UInt32`|创建了其句柄的对象的生成。|
|`AppDomainID`|`win:UInt64`|AppDomain ID。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="destroygchandle-event"></a>DestroyGCHandle 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCHandleKeyword` (0x2)|信息性 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`DestroyGCHandle`|31|已销毁 GC 句柄。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`HandleID`|`win:Pointer`|已销毁的句柄的地址。|
|`ClrInstanceID`|win:UInt16|CoreCLR 实例的唯一 ID。|

## <a name="pinobjectatgctime-event"></a>PinObjectAtGCTime 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|详细级别 (5)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`PinObjectAtGCTime`|33|对象在 GC 期间已固定。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`HandleID`|`win:Pointer`|句柄的地址。|
|`ObjectID`|`win:Pointer`|已固定对象的地址。|
|`ObjectSize`|`win:UInt64`|已固定对象的大小。|
|`TypeName`|`win:UnicodeString`|已固定对象的类型的名称。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="gctriggered-event"></a>GCTriggered 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|详细级别 (5)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCTriggered`|33|已触发 GC。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Reason`|`win:UInt32`|触发 GC 的原因。<br/><br/>`0x0`：AllocSmall<br/><br/>`0x1`：Induced <br/><br/>`0x2`：LowMemory <br/><br/>`0x3`：Empty <br/><br/>`0x4`：AllocLarge <br/><br/>`0x5`：OutOfSpaceSmallObjectHeap <br/><br/>`0x6`：OutOfSpaceLargeObjectHeap <br/><br/>`0x7`：InducedNoForce <br/><br/>`0x8`：Stress <br/><br/>`0x9`：InducedLowMemory|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="increasememorypressure-event"></a>IncreaseMemoryPressure 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|----------------|---------------|-----------------|
|`IncreaseMemoryPressure`|200|内存压力增加。|

下表显示了事件数据：

|字段名称|数据类型|说明|
|----------------|---------------|-----------------|
|`ClrInstanceID`|win:UInt16|CoreCLR 实例的唯一 ID。|

## <a name="decreasememorypressure-event"></a>DecreaseMemoryPressure 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|----------------|---------------|-----------------|
|`DecreaseMemoryPressure`|201|内存压力降低。|

下表显示了事件数据：

|字段名称|数据类型|说明|
|----------------|---------------|-----------------|
|`BytesFreed`|`win:UInt32`|已释放字节。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="gcmarkwithtype-event"></a>GCMarkWithType 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|信息 (4)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|----------------|---------------|-----------------|
|`GCMarkWithType`|202|GC 标记阶段期间已标记了一个 GC 根。|

下表显示了事件数据：

|字段名称|数据类型|说明|
|----------------|---------------|-----------------|
|`HeapNum`|`win:UInt32`|堆号。|
|`ClrInstanceID`|win:UInt16|CoreCLR 实例的唯一 ID。|
|`Type`|`win:UInt32`|GC 根类型。<br/><br/>`0x0`：Stack<br/><br/>`0x1`：Finalizer<br/><br/>`0x2`：Handle<br/><br/>`0x3`：Older<br/><br/>`0x4`：SizedRef<br/><br/>`0x5`：Overflow<br/><br/>|
|`Bytes`|`win:UInt64`|已标记的字节数。|

## <a name="gcjoin_v2-event"></a>GCJoin_V2 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|详细级别 (5)|

下表显示了事件信息：

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`GCJoin_V2`|203|已联接的 GC 线程。|

下表显示了事件数据：

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Heap`|`win:UInt32`|堆号|
|`JoinTime`|`win:UInt32`|指示在联接开始或结束时是否触发此事件（`0x0` 为联接开始，`0x1` 为联接结束）|
|`JoinType`|`win:UInt32`|联接类型。 <br/><br/>`0x0`：Last Join<br/><br/>`0x1`：Join <br/><br/>`0x2`：Restart <br/><br/>`0x3`：First Reverse Join<br/><br/>`0x4`：Reverse Join<br/><br/>|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|
