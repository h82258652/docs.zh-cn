---
title: ThreadPool 运行时事件
description: 查看在 .NET Core 中收集有关线程池的诊断信息的 .NET 运行时线程池事件。 线程池事件是工作线程池事件或 I/O 线程池事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- ThreadPool events (CoreCLR)
- ETW, thread pool events (CoreCLR)
ms.openlocfilehash: cdd6041c5842d4922c60e33daf6db366f7d35327
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591083"
---
# <a name="net-runtime-thread-pool-events"></a>.NET 运行时线程池事件

这些事件收集线程池中有关工作线程和 I/O 线程的信息。 有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)

## <a name="iothreadcreate_v1-event"></a>IOThreadCreate_V1 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------------------------------|-----------|
|`IOThreadCreate_V1`|44|在线程池中创建 I/O 线程。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|I/O 线程数，包括新创建的线程。|
|`NumRetired`|`win:UInt64`|已停用的工作线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="iothreadterminate_v1-event"></a>IOThreadTerminate_V1 事件

 下表显示了关键字和级别

|引发事件的关键字|Level
|-----------------------------------|-----------
|`ThreadingKeyword` (0x10000)|信息性 (4)

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`IOThreadTerminate`|45|在线程池中终止 I/O 线程。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|线程池中剩余的 I/O 线程数。|
|`NumRetired`|`win:UInt64`|已停用的 I/O 线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="iothreadretire_v1-event"></a>IOThreadRetire_V1 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`IOThreadRetire_V1`|46|I/O 线程变为停用候选项。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|线程池中剩余的 I/O 线程数。|
|`NumRetired`|`win:UInt64`|已停用的 I/O 线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="iothreadunretire_v1-event"></a>IOThreadUnretire_V1 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`IOThreadUnretire_V1`|47|I/O 线程因在该线程变为停用候选项后的等待期间内达到 I/O 而恢复使用。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|线程池中的 I/O 线程数，包括这一个。|
|`NumRetired`|`win:UInt64`|已停用的 I/O 线程数。|
|`ClrInstanceID`|`Win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadstart-event"></a>ThreadPoolWorkerThreadStart 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadStart`|50|创建工作线程。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|可用于处理工作的工作线程数，包括已在处理工作的工作线程。|
|`RetiredWorkerThreadCount`|`win:UInt32`|不能用于处理工作但被保留以防之后需要更多线程的工作线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadstop-event"></a>ThreadPoolWorkerThreadStop 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadStop`|51|停止工作线程。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|可用于处理工作的工作线程数，包括已在处理工作的工作线程。|
|`RetiredWorkerThreadCount`|`win:UInt32`|不能用于处理工作但被保留以防之后需要更多线程的工作线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadwait-event"></a>ThreadPoolWorkerThreadWait 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadWait`|57|工作线程开始等待运行。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|可用于处理工作的工作线程数，包括已在处理工作的工作线程。|
|`RetiredWorkerThreadCount`|`win:UInt32`|不能用于处理工作但被保留以防之后需要更多线程的工作线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadretirementstart-event"></a>ThreadPoolWorkerThreadRetirementStart 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadRetirementStart`|52|停用工作线程。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|可用于处理工作的工作线程数，包括已在处理工作的工作线程。|
|`RetiredWorkerThreadCount`|`win:UInt32`|不能用于处理工作但被保留以防之后需要更多线程的工作线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadretirementstop-event"></a>ThreadPoolWorkerThreadRetirementStop 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadRetirementStop`|53|停用的工作线程再次变为活动状态。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|可用于处理工作的工作线程数，包括已在处理工作的工作线程。|
|`RetiredWorkerThreadCount`|`win:UInt32`|不能用于处理工作但被保留以防之后需要更多线程的工作线程数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadadjustmentsample-event"></a>ThreadPoolWorkerThreadAdjustmentSample 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadAdjustmentSample`|54|指一个示例的信息收集，即具有一定并发级别的即时吞吐量测量。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Throughput`|`win:Double`|每个时间单位的完成数。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadadjustmentadjustment-event"></a>ThreadPoolWorkerThreadAdjustmentAdjustment 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadAdjustmentAdjustment`|55|当线程注入（爬山）算法确定并发级别发生更改时，在控件中记录更改。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AverageThroughput`|`win:Double`|测量示例的平均吞吐量。|
|`NewWorkerThreadCount`|`win:UInt32`|新的活动工作线程数。|
|`Reason`|`win:UInt32`|调整的原因。<br /><br /> `0x0` - 预热。<br /><br /> `0x1` - 正在初始化。<br /><br /> `0x2` - 随机移动。<br /><br /> `0x3` - 攀移。<br /><br /> `0x4` - 更改点。<br /><br /> `0x5` - 正在稳定。<br /><br /> `0x6` - 匮乏。<br /><br /> `0x7` - 线程已超时。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolworkerthreadadjustmentstats-event"></a>ThreadPoolWorkerThreadAdjustmentStats 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|详细级别 (5)

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadAdjustmentStats`|56|在线程池上收集数据。|

 下表显示了事件数据

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`Duration`|`win:Double`|收集这些统计信息的时间量（以秒为单位）。|
|`Throughput`|`win:Double`|在此间隔期间每秒完成的平均数量。|
|`ThreadWave`|`win:Double`|保留以供内部使用。|
|`ThroughputWave`|`win:Double`|保留以供内部使用。|
|`ThroughputErrorEstimate`|`win:Double`|保留以供内部使用。|
|`AverageThroughputErrorEstimate`|`win:Double`|保留以供内部使用。|
|`ThroughputRatio`|`win:Double`|在此间隔期间因活动工作线程计数变化导致的相对吞吐量变化。|
|`Confidence`|`win:Double`|ThroughputRatio 字段的有效性测量。|
|`NewcontrolSetting`|`win:Double`|将作为活动线程计数未来变化基线的活动工作线程数。|
|`NewThreadWaveMagnitude`|`win:UInt16`|活动线程计数的未来变化量值。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="threadpoolenqueue-event"></a>ThreadPoolEnqueue 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|详细级别 (5)

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolEnqueue`|61|工作项已排入线程池队列。|

 下表显示了事件数据

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`WorkID`|`win:Pointer`|指向工作请求的指针。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="threadpooldequeue-event"></a>ThreadPoolDequeue 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|详细级别 (5)

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolDequeue`|62|工作项已从线程池队列中取消排队。|

 下表显示了事件数据

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`WorkID`|`win:Pointer`|指向工作请求的指针。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="threadpoolioenqueue-event"></a>ThreadPoolIOEnqueue 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|详细级别 (5)

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolIOEnqueue`|63|异步 IO 完成后，线程对 IO 完成通知进行排队。|

 下表显示了事件数据

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`NativeOverlapped`|`win:Pointer`|保留以供内部使用。|
|`Overlapped`|`win:Pointer`|保留以供内部使用。|
|`MultiDequeues`|`win:Boolean`|保留以供内部使用。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="threadpooliodequeue-event"></a>ThreadPoolIODequeue 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|详细级别 (5)

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolIODequeue`|64|线程对 IO 完成通知取消排队。|

 下表显示了事件数据

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`NativeOverlapped`|`win:Pointer`|保留以供内部使用。|
|`Overlapped`|`win:Pointer`|保留以供内部使用。|
|`MultiDequeues`|`win:Boolean`|保留以供内部使用。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="threadpooliopack-event"></a>ThreadPoolIOPack 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|详细级别 (5)|

 下表显示了事件信息。

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ThreadPoolIOPack`|65|调用了 ThreadPool 重叠的 IO 包。|

 下表显示了事件数据

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`NativeOverlapped`|`win:Pointer`|保留以供内部使用。|
|`Overlapped`|`win:Pointer`|保留以供内部使用。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="threadcreating-event"></a>ThreadCreating 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`ThreadCreating`|70|已创建线程。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ID`|`win:Pointer`|线程 ID|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="threadrunning-event"></a>ThreadRunning 事件

 下表显示了关键字和级别。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ThreadingKeyword` (0x10000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`ThreadRunning`|71|线程已开始运行。|

 下表显示了事件数据。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ID`|`win:Pointer`|线程 ID|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|
