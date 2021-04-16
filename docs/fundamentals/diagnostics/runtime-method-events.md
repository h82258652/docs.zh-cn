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
# <a name="net-runtime-method-events"></a>.NET 运行时方法事件

这些事件收集特定于方法的信息。 符号解析需要这些事件的负载。 此外，这些事件还提供了一些有用的信息，例如已加载和已卸载的方法。 有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)

所有方法事件都具有“信息性 (4)”级别。 所有方法的详细事件都具有“详细级别 (5)”级别。

所有方法事件都是由运行时提供程序下的 `JITKeyword` (0x10) 关键字或 `NGenKeyword` (0x20) 关键字引发的，或是由断开提供程序下的 `JitRundownKeyword` (0x10) 或 `NGENRundownKeyword` (0x20) 引发的。

这些事件的 V2 版本包括 ReJITID（V1 版本不包含）。

## <a name="methodload_v1-event"></a>MethodLoad_V1 事件

下表显示了事件信息：

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodLoad_V1`|141|在实时加载（JIT 加载）方法或者加载 NGEN 映像时引发。 动态和泛型方法不使用此版本进行方法加载。 JIT 帮助器从不使用此版本。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) 运行时提供程序|信息性 (4)|
|`NGenKeyword` (0x20) 运行时提供程序|信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|方法的起始地址。|
|`MethodSize`|`win:UInt32`|方法的大小。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。<br /><br /> 0x8：帮助器方法。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodload_v2-event"></a>MethodLoad_V2 事件

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`MethodLoad_V2`|141|在实时加载（JIT 加载）方法或者加载 NGEN 映像时引发。 动态和泛型方法不使用此版本进行方法加载。 JIT 帮助器从不使用此版本。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) 运行时提供程序|信息性 (4)|
|`NGenKeyword` (0x20) 运行时提供程序|信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|方法的起始地址。|
|`MethodSize`|`win:UInt32`|方法的大小。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。<br /><br /> 0x8：帮助器方法。|
|`ReJITID`|`win:UInt64`|方法的 ReJIT ID。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodunload_v1-event"></a>MethodUnLoad_V1 事件

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`MethodUnLoad_V1`|142|在卸载模块或销毁应用程序域时引发。 动态方法从不使用此版本进行方法卸载。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10)|信息性 (4)|
|`NGenKeyword` (0x20)|信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|方法的起始地址。|
|`MethodSize`|`win:UInt32`|方法的大小。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。<br /><br /> 0x8：帮助器方法。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodunload_v2-event"></a>MethodUnLoad_V2 事件

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`MethodUnLoad_V2`|142|在卸载模块或销毁应用程序域时引发。 动态方法从不使用此版本进行方法卸载。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10)|信息性 (4)|
|`NGenKeyword` (0x20)|信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|方法的起始地址。|
|`MethodSize`|`win:UInt32`|方法的大小。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的代码方法（否则为 NGEN 本机映像代码）。<br /><br /> 0x8：帮助器方法。|
|`ReJITID`|`win:UInt64`|方法的 ReJIT ID。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="r2rgetentrypoint-event"></a>R2RGetEntryPoint 事件

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`R2RGetEntryPoint`|159|R2R 入口点查找结束时引发。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) |信息性 (4)|
|`NGenKeyword` (0x20) |信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|R2R 方法的唯一标识符。|
|`MethodNamespace`|`win:UnicodeString`|要查找的方法的命名空间。|
|`MethodName`|`win:UnicodeString`|要查找的方法的名称。|
|`MethodSignature`|`win:UnicodeString`|方法的签名（以逗号分隔的类型名称列表）。|
|`EntryPoint`|`win:UInt64`|指向 R2R 方法入口点的指针|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="r2rgetentrypointstart-event"></a>R2RGetEntryPointStart 事件

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`R2RGetEntryPointStart`|160|R2R 入口点查找开始时引发。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) |信息性 (4)|
|`NGenKeyword` (0x20) |信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|R2R 方法的唯一标识符。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodloadverbose_v1-event"></a>MethodLoadVerbose_V1 事件

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodLoadVerbose_V1`|143|当方法为 JIT 加载的或加载 NGEN 映像时引发。 动态和泛型方法始终使用此版本进行方法加载。 JIT 帮助器始终使用此版本。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) |信息性 (4)|
|`NGenKeyword` (0x20) |信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|起始地址。|
|`MethodSize`|`win:UInt32`|方法长度。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的方法（否则由 NGen.exe 生成）<br /><br /> 0x8：帮助器方法。|
|`MethodNameSpace`|`win:UnicodeString`|与该方法关联的完整命名空间名称。|
|`MethodName`|`win:UnicodeString`|与该方法关联的完整类名称。|
|`MethodSignature`|`win:UnicodeString`|方法的签名（以逗号分隔的类型名称列表）。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodloadverbose_v2-event"></a>MethodLoadVerbose_V2 事件

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodLoadVerbose_V1`|143|当方法为 JIT 加载的或加载 NGEN 映像时引发。 动态和泛型方法始终使用此版本进行方法加载。 JIT 帮助器始终使用此版本。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) |信息性 (4)|
|`NGenKeyword` (0x20) |信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|起始地址。|
|`MethodSize`|`win:UInt32`|方法长度。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的方法（否则由 NGen.exe 生成）<br /><br /> 0x8：帮助器方法。|
|`MethodNameSpace`|`win:UnicodeString`|与该方法关联的完整命名空间名称。|
|`MethodName`|`win:UnicodeString`|与该方法关联的完整类名称。|
|`MethodSignature`|`win:UnicodeString`|方法的签名（以逗号分隔的类型名称列表）。|
|`ReJITID`|`win:UInt64`|方法的 ReJIT ID。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodunloadverbose_v1-event"></a>MethodUnLoadVerbose_V1 事件

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodUnLoadVerbose_V1`|144|在销毁动态方法、卸载模块或销毁应用程序域时引发。 动态方法始终使用此版本进行方法卸载。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) |信息性 (4)|
|`NGenKeyword` (0x20) |信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|起始地址。|
|`MethodSize`|`win:UInt32`|方法长度。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的方法（否则由 NGen.exe 生成）<br /><br /> 0x8：帮助器方法。|
|`MethodNameSpace`|`win:UnicodeString`|与该方法关联的完整命名空间名称。|
|`MethodName`|`win:UnicodeString`|与该方法关联的完整类名称。|
|`MethodSignature`|`win:UnicodeString`|方法的签名（以逗号分隔的类型名称列表）。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodunloadverbose_v2-event"></a>MethodUnLoadVerbose_V2 事件

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodUnLoadVerbose_V2`|144|在销毁动态方法、卸载模块或销毁应用程序域时引发。 动态方法始终使用此版本进行方法卸载。|

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) |信息性 (4)|
|`NGenKeyword` (0x20) |信息性 (4)|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。 对于 JIT 帮助器方法，将设置为该方法的起始地址。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符（0 表示 JIT 帮助器）。|
|`MethodStartAddress`|`win:UInt64`|起始地址。|
|`MethodSize`|`win:UInt32`|方法长度。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodFlags`|`win:UInt32`|0x1：动态方法。<br /><br /> 0x2：泛型方法。<br /><br /> 0x4：JIT 编译的方法（否则由 NGen.exe 生成）<br /><br /> 0x8：帮助器方法。|
|`MethodNameSpace`|`win:UnicodeString`|与该方法关联的完整命名空间名称。|
|`MethodName`|`win:UnicodeString`|与该方法关联的完整类名称。|
|`MethodSignature`|`win:UnicodeString`|方法的签名（以逗号分隔的类型名称列表）。|
|`ReJITID`|`win:UInt64`|方法的 ReJIT ID。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodjittingstarted_v1-event"></a>MethodJittingStarted_V1 事件

下表显示了关键字和级别：

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITKeyword` (0x10) |详细级别 (5)|
|`NGenKeyword` (0x20) |详细级别 (5)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodJittingStarted_V1`|145|在方法由 JIT 编译时引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。|
|`ModuleID`|`win:UInt64`|此方法所属的模块标识符。|
|`MethodToken`|`win:UInt32`|0 代表动态方法和 JIT 帮助器。|
|`MethodILSize`|`win:UInt32`|要进行 JIT 编译的方法的公共中间语言 (CIL) 的大小。|
|`MethodNameSpace`|`win:UnicodeString`|与该方法关联的完整类名称。|
|`MethodName`|`win:UnicodeString`|方法的名称。|
|`MethodSignature`|`win:UnicodeString`|方法的签名（以逗号分隔的类型名称列表）。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodjitinliningsucceeded-event"></a>MethodJitInliningSucceeded 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITTracingKeyword` (0x1000) |详细级别 (5)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodJitInliningSucceeded`|185|当方法由 JIT 编译器成功内联时引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|要编译的方法的命名空间。|
|`MethodBeingCompiledName`|`win:UnicodeString`|要编译的方法的名称。|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|要编译的方法的签名（以逗号分隔的类型名称列表）。|
|`InlinerNamespace`|`win:UnicodeString`|内联方（“父级”）方法的命名空间。|
|`InlinerName`|`win:UnicodeString`|内联方（“父级”）方法的名称。|
|`InlinerNameSignature`|`win:UnicodeString`|内联方（“父级”）方法的签名（以逗号分隔的类型名称列表）。|
|`InlineeNamespace`|`win:UnicodeString`|被内联方（“子级”）方法的命名空间。|
|`InlineeName`|`win:UnicodeString`|被内联方（“子级”）方法的名称。|
|`InlineeNameSignature`|`win:UnicodeString`|被内联方（“子级”）方法的签名（以逗号分隔的类型名称列表）。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodjitinliningfailed-event"></a>MethodJitInliningFailed 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITTracingKeyword` (0x1000) |详细级别 (5)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodJitInliningFailed`|192|当方法无法由 JIT 编译器内联时引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|要编译的方法的命名空间。|
|`MethodBeingCompiledName`|`win:UnicodeString`|要编译的方法的名称。|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|要编译的方法的签名（以逗号分隔的类型名称列表）。|
|`InlinerNamespace`|`win:UnicodeString`|内联方（“父级”）方法的命名空间。|
|`InlinerName`|`win:UnicodeString`|内联方（“父级”）方法的名称。|
|`InlinerNameSignature`|`win:UnicodeString`|内联方（“父级”）方法的签名（以逗号分隔的类型名称列表）。|
|`InlineeNamespace`|`win:UnicodeString`|被内联方（“子级”）方法的命名空间。|
|`InlineeName`|`win:UnicodeString`|被内联方（“子级”）方法的名称。|
|`InlineeNameSignature`|`win:UnicodeString`|被内联方（“子级”）方法的签名（以逗号分隔的类型名称列表）。|
|`FailAlways`|`win:Boolean`|方法是否被标记为不可内联。|
|`FailReason`|`win:UnicodeString`|内联失败的原因。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodjittailcallsucceeded-event"></a>MethodJITTailCallSucceeded 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITTracingKeyword` (0x1000) |详细级别 (5)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodJitTailCallSucceeded`|192|当方法可成功被尾调用时由 JIT 编译器引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|要编译的方法的命名空间。|
|`MethodBeingCompiledName`|`win:UnicodeString`|要编译的方法的名称。|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|要编译的方法的签名（以逗号分隔的类型名称列表）。|
|`CallerNamespace`|`win:UnicodeString`|调用方方法的命名空间。|
|`CallerName`|`win:UnicodeString`|调用方方法的名称。|
|`CallerNameSignature`|`win:UnicodeString`|调用方方法的签名（以逗号分隔的类型名称列表）。|
|`CalleeNamespace`|`win:UnicodeString`|被调用方方法的命名空间。|
|`CalleeName`|`win:UnicodeString`|被调用方方法的名称。|
|`CalleeNameSignature`|`win:UnicodeString`|被调用方方法的签名（以逗号分隔的类型名称列表）。|
|`TailPrefix`|`win:Boolean`|它是否为尾前缀指令。|
|`TailCallType`|`win:UInt32`|尾调用的类型。<br/><br/>0：优化的尾调用 (epilog + jmp)<br/><br/>1：递归的尾调用（方法尾调用自身）<br/><br/>2：帮助程序辅助的尾调用|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodjittailcallfailed-event"></a>MethodJITTailCallFailed 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JITTracingKeyword` (0x1000) |详细级别 (5)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`MethodJitTailCallFailed`|191|当方法无法被尾调用时由 JIT 编译器引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodBeingCompiledNamespace`|`win:UnicodeString`|要编译的方法的命名空间。|
|`MethodBeingCompiledName`|`win:UnicodeString`|要编译的方法的名称。|
|`MethodBeingCompiledNameSignature`|`win:UnicodeString`|要编译的方法的签名（以逗号分隔的类型名称列表）。|
|`CallerNamespace`|`win:UnicodeString`|调用方方法的命名空间。|
|`CallerNam`e|`win:UnicodeString`|调用方方法的名称。|
|`CallerNameSignature`|`win:UnicodeString`|调用方方法的签名（以逗号分隔的类型名称列表）。|
|`CalleeNamespace`|`win:UnicodeString`|被调用方方法的命名空间。|
|`CalleeName`|`win:UnicodeString`|被调用方方法的名称。|
|`CalleeNameSignature`|`win:UnicodeString`|被调用方方法的签名（以逗号分隔的类型名称列表）。|
|`TailPrefix`|`win:Boolean`|它是否为尾前缀指令。|
|`FailReason`|`win:UnicodeString`|尾调用失败的原因。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="methodiltonativemap-event"></a>MethodILToNativeMap 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|
|`JittedMethodILToNativeMapKeyword` (0x20000) |详细级别 (5)|

|事件|事件 ID|说明|
|----------------|---------------|-----------------|
|`MethodILToNativeMap`|190|为 JIT 编译的方法映射 IL 到本机映射事件。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`MethodID`|`win:UInt64`|方法的唯一标识符。|
|`ReJITID`|`win:UInt64`|方法的 ReJIT ID。|
|`MethodExtent`|`win:UInt8`|已执行 JIT 的方法的范围。|
|`CountOfMapEntries`|`win:UInt8`|映射条目数|
|`ILOffsets`|`win:UInt32`|IL 偏移量。|
|`NativeOffsets`|`win:UInt32`|本机代码偏移量。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|
