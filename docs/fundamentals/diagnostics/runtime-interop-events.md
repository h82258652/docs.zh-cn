---
title: 互操作运行时事件
description: 查看收集特定于互操作的诊断信息的 .NET 运行时事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- Interop events (CoreCLR)
- ETW, EventPipe, LTTng interop events (CoreCLR)
ms.openlocfilehash: 5635fb55b3a6ffa3f5611da80cdb2909e226e2ea
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591058"
---
# <a name="net-runtime-interop-events"></a>.NET 运行时互操作事件

这些运行时事件捕获有关公共中间语言 (CIL) 存根生成的信息。 有关如何将这些事件用于诊断的详细信息，请参阅 [.NET 应用程序日志记录和跟踪](../../core/diagnostics/logging-tracing.md)

## <a name="ilstubgenerated-event"></a>ILStubGenerated 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`InteropKeyword` (0x2000)|信息性 (4)|
  
|事件|事件 ID|在发生以下情况时引发|
|-----------|--------------|-----------------|
|`ILStubGenerated`|88|生成 IL 存根。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt16`|模块标识符。|
|`StubMethodID`|`win:UInt64`|存根方法标识符。|
|`StubFlags`|`win:UInt32`|存根标志：<br /><br /> `0x1` - 反向互操作。<br /><br /> `0x2` - COM 互操作。<br /><br /> `0x4` - 由 NGen.exe 生成的存根。<br /><br /> `0x8` - 委托。<br /><br /> `0x10` - 变量参数。<br /><br /> `0x20` - 非托管被调用方。<br /><br /> `0x40` - 结构封送|
|`ManagedInteropMethodToken`|`win:UInt32`|托管互操作方法的标记。|
|`ManagedInteropMethodNameSpace`|`win:UnicodeString`|托管互操作方法的命名空间和封闭类型。|
|`ManagedInteropMethodName`|`win:UnicodeString`|托管互操作方法的名称。|
|`ManagedInteropMethodSignature`|`win:UnicodeString`|托管互操作方法的签名。|
|`NativeMethodSignature`|`win:UnicodeString`|本机方法签名。|
|`StubMethodSignature`|`win:UnicodeString`|存根方法签名。|
|`StubMethodILCode`|`win:UnicodeString`|存根方法的公共中间语言 (CIL) 代码。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|
