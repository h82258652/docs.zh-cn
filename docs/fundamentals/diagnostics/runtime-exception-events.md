---
title: 异常运行时事件
description: 请参阅收集特定于异常和异常处理的诊断信息的 .NET 运行时事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- exception events (CoreCLR)
- exception handling events (CoreCLR)
- ETW, EventPipe, LTTng exception events (CoreCLR)
ms.openlocfilehash: f4f63c8469f9c734b872ddaec8d1d7f7f0427576
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591077"
---
# <a name="net-runtime-exception-events"></a>.NET 运行时异常事件

这些运行时事件捕获有关引发的异常的信息。 有关如何将这些事件用于诊断的详细信息，请参阅 [.NET 应用程序日志记录和跟踪](../../core/diagnostics/logging-tracing.md)

## <a name="exceptionthrown_v1-event"></a>ExceptionThrown_V1 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|错误 (1)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionThrown_V1`|80|引发托管异常。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ExceptionType`|`win:UnicodeString`|异常的类型，例如，`System.NullReferenceException`。|
|`ExceptionMessage`|`win:UnicodeString`|实际的异常消息。|
|`EIPCodeThrow`|`win:Pointer`|指向异常发生位置的指令指针。|
|`ExceptionHR`|`win:UInt32`|异常 [HRESULT](/openspecs/windows_protocols/ms-erref/0642cb2f-2075-4469-918c-4441e69c548a)。|
|`ExceptionFlags`|`win:UInt16`|`0x01`: HasInnerException。<br /><br /> `0x02`: IsNestedException。<br /><br /> `0x04`: IsRethrownException。<br /><br /> `0x08`: IsCorruptedStateException（指示进程状态已损坏，请参阅[处理损坏状态异常](/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions)）。<br /><br /> `0x10`: IsCLSCompliant（从 <xref:System.Exception> 派生的异常符合 CLS，此外的其他异常均不符合 CLS）。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="exceptioncatchstart-event"></a>ExceptionCatchStart 事件

当托管异常 catch 处理程序开始时，将发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionCatchStart`|250|托管异常由运行时处理。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`EIPCodeThrow`|`win:Pointer`|指向异常发生位置的指令指针。|
|`MethodID`|`win:Pointer`|指向发生异常的方法上的方法描述符的指针。|
|`MethodName`|`win:UnicodeString`|发生异常的方法的名称。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="exceptioncatchstop-event"></a>ExceptionCatchStop 事件

当托管异常 catch 处理程序结束时，将发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionCatchStop`|251|托管异常 catch 处理程序已完成。|

## <a name="exceptionfinallystart-event"></a>ExceptionFinallyStart 事件

当托管异常 finally 处理程序开始时，将发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionFinallyStart`|252|托管异常由运行时处理。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`EIPCodeThrow`|`win:Pointer`|指向异常发生位置的指令指针。|
|`MethodID`|`win:Pointer`|指向发生异常的方法上的方法描述符的指针。|
|`MethodName`|`win:UnicodeString`|发生异常的方法的名称。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|

## <a name="exceptionfinallystop-event"></a>ExceptionFinallyStop 事件

当托管异常 catch 处理程序结束时，将发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionFinallyStop`|253|托管异常 finally 处理程序已完成。|

## <a name="exceptionfilterstart-event"></a>ExceptionFilterStart 事件

当托管异常筛选开始时，将发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionFilterStart`|254|托管异常筛选开始。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`EIPCodeThrow`|`win:Pointer`|指向异常发生位置的指令指针。|
|`MethodID`|`win:Pointer`|指向发生异常的方法上的方法描述符的指针。|
|`MethodName`|`win:UnicodeString`|发生异常的方法的名称。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="exceptionfilterstop-event"></a>ExceptionFilterStop 事件

当托管异常筛选结束时，将发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|信息性 (4)|

 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionFilteringStart`|255|托管异常筛选结束。|

## <a name="exceptionthrownstop-event"></a>ExceptionThrownStop 事件

当运行时完成处理引发的托管异常时，将发出此事件。

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`ExceptionKeyword` (0x8000)|信息性 (4)|
  
 下表显示了事件信息。

|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ExceptionThrownStop`|256|托管异常筛选结束。|
