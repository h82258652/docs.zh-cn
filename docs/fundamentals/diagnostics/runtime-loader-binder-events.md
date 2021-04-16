---
title: 加载器和绑定器运行时事件
description: 查看收集特定于加载器和绑定器 ETW 事件的诊断信息的 .NET 运行时事件，ETW 事件收集有关程序集加载器和绑定器的信息。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- Assembly Loader events (CoreCLR)
- Assembly Binder events (CoreCLR)
- ETW, EventPipe, LTTng assembly loader and binder events (CoreCLR)
ms.openlocfilehash: 2284c580482f6b93a77f44649225ff7e5485666a
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591079"
---
# <a name="net-runtime-loader-and-binder-events"></a>.NET 运行时加载器和绑定器事件

这些事件收集有关加载和卸载程序集和模块的信息。 有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`DomainModuleLoad_V1`|151|在为应用程序域加载模块时引发。|

## <a name="moduleload_v2-event"></a>ModuleLoad_V2 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ModuleLoad_V2`|152|在进程的生存期内加载模块时引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|模块的唯一 ID。|
|`AssemblyID`|`win:UInt64`|此模块所驻留的程序集的 ID。|
|`ModuleFlags`|`win:UInt32`|0x1：非特定于域的模块。<br /><br /> 0x2：模块具有本机映像。<br /><br /> 0x4：动态模块。<br /><br /> 0x8：清单模块。|
|`Reserved1`|`win:UInt32`|保留的字段。|
|`ModuleILPath`|`win:UnicodeString`|模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。|
|`ModuleNativePath`|`win:UnicodeString`|如果存在（以 null 结尾），则为模块本机映像的路径。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|
|`ManagedPdbSignature`|`win:GUID`|匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。|
|`ManagedPdbAge`|`win:UInt32`|写入匹配此模块的托管 PDB 的年限数。|
|`ManagedPdbBuildPath`|`win:UnicodeString`|生成匹配此模块的托管 PDB 的位置的路径。 在某些情况下，这可能只是一个文件名。|
|`NativePdbSignature`|`win:GUID`|匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。|
|`NativePdbAge`|`win:UInt32`|写入匹配此模块的 NGen PDB 的年限数（如果适用）。|
|`NativePdbBuildPath`|`win:UnicodeString`|生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。 在某些情况下，这可能只是一个文件名。|

## <a name="moduleunload_v2-event"></a>ModuleUnload_V2 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ModuleUnload_V2`|153|在进程的生存期内卸载模块时引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|模块的唯一 ID。|
|`AssemblyID`|`win:UInt64`|此模块所驻留的程序集的 ID。|
|`ModuleFlags`|`win:UInt32`|0x1：非特定于域的模块。<br /><br /> 0x2：模块具有本机映像。<br /><br /> 0x4：动态模块。<br /><br /> 0x8：清单模块。|
|`Reserved1`|`win:UInt32`|保留的字段。|
|`ModuleILPath`|`win:UnicodeString`|模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。|
|`ModuleNativePath`|`win:UnicodeString`|如果存在（以 null 结尾），则为模块本机映像的路径。|
|`ClrInstanceID`|``win:UInt16``|CLR 或 CoreCLR 的实例的唯一 ID。|
|`ManagedPdbSignature`|`win:GUID`|匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。|
|`ManagedPdbAge`|`win:UInt32`|写入匹配此模块的托管 PDB 的年限数。|
|`ManagedPdbBuildPath`|`win:UnicodeString`|生成匹配此模块的托管 PDB 的位置的路径。 在某些情况下，这可能只是一个文件名。|
|`NativePdbSignature`|`win:GUID`|匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。|
|`NativePdbAge`|`win:UInt32`|写入匹配此模块的 NGen PDB 的年限数（如果适用）。|
|`NativePdbBuildPath`|`win:UnicodeString`|生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。 在某些情况下，这可能只是一个文件名。|

## <a name="moduledcstart_v2-event"></a>ModuleDCStart_V2 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)

|事件|事件 ID|说明
|-----------|--------------|-----------------|
|`ModuleDCStart_V2`|153|在启动断开期间枚举模块。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|模块的唯一 ID。|
|`AssemblyID`|`win:UInt64`|此模块所驻留的程序集的 ID。|
|`ModuleFlags`|`win:UInt32`|0x1：非特定于域的模块。<br /><br /> 0x2：模块具有本机映像。<br /><br /> 0x4：动态模块。<br /><br /> 0x8：清单模块。|
|`Reserved1`|`win:UInt32`|保留的字段。|
|`ModuleILPath`|`win:UnicodeString`|模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。|
|`ModuleNativePath`|`win:UnicodeString`|如果存在（以 null 结尾），则为模块本机映像的路径。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|
|`ManagedPdbSignature`|`win:GUID`|匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。|
|`ManagedPdbAge`|`win:UInt32`|写入匹配此模块的托管 PDB 的年限数。|
|`ManagedPdbBuildPath`|`win:UnicodeString`|生成匹配此模块的托管 PDB 的位置的路径。 在某些情况下，这可能只是一个文件名。|
|`NativePdbSignature`|`win:GUID`|匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。|
|`NativePdbAge`|`win:UInt32`|写入匹配此模块的 NGen PDB 的年限数（如果适用）。|
|`NativePdbBuildPath`|`win:UnicodeString`|生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。 在某些情况下，这可能只是一个文件名。|

## <a name="moduledcend_v2-event"></a>ModuleDCEnd_V2 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ModuleDCEnd_V2`|154|在结束断开期间枚举模块。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|模块的唯一 ID。|
|`AssemblyID`|`win:UInt64`|此模块所驻留的程序集的 ID。|
|`ModuleFlags`|`win:UInt32`|0x1：非特定于域的模块。<br /><br /> 0x2：模块具有本机映像。<br /><br /> 0x4：动态模块。<br /><br /> 0x8：清单模块。|
|`Reserved1`|`win:UInt32`|保留的字段。|
|`ModuleILPath`|`win:UnicodeString`|模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。|
|`ModuleNativePath`|`win:UnicodeString`|如果存在（以 null 结尾），则为模块本机映像的路径。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|
|`ManagedPdbSignature`|`win:GUID`|匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。|
|`ManagedPdbAge`|`win:UInt32`|写入匹配此模块的托管 PDB 的年限数。|
|`ManagedPdbBuildPath`|`win:UnicodeString`|生成匹配此模块的托管 PDB 的位置的路径。 在某些情况下，这可能只是一个文件名。|
|`NativePdbSignature`|`win:GUID`|匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。|
|`NativePdbAge`|`win:UInt32`|写入匹配此模块的 NGen PDB 的年限数（如果适用）。|
|`NativePdbBuildPath`|`win:UnicodeString`|生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。 在某些情况下，这可能只是一个文件名。|

## <a name="assemblyload_v1-event"></a>AssemblyLoad_V1 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`AssemblyLoad_V1`|154|在加载程序集时引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyID`|`win:UInt64`|程序集的唯一 ID。|
|`AppDomainID`|`win:UInt64`|此程序集的域的 ID。|
|`BindingID`|`win:UInt64`|唯一地标识程序集绑定的 ID。|
|`AssemblyFlags`|`win:UInt32`|0x1：非特定于域的程序集。<br /><br /> 0x2：动态程序集。<br /><br /> 0x4：程序集具有本机映像。<br /><br /> 0x8：可回收程序集。|
|`AssemblyName`|`win:UnicodeString`|完全限定程序集名称。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。

## <a name="assemblyunload_v1-event"></a>AssemblyUnload_V1 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`FireAssemblyUnload_V1`|155|在加载程序集时引发。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyID`|`win:UInt64`|程序集的唯一 ID。|
|`AppDomainID`|`win:UInt64`|此程序集的域的 ID。|
|`BindingID`|`win:UInt64`|唯一地标识程序集绑定的 ID。|
|`AssemblyFlags`|`win:UInt32`|0x1：非特定于域的程序集。<br /><br /> 0x2：动态程序集。<br /><br /> 0x4：程序集具有本机映像。<br /><br /> 0x8：可回收程序集。|
|`AssemblyName`|`win:UnicodeString`|完全限定程序集名称。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="assemblydcstart_v1-event"></a>AssemblyDCStart_V1 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`AssemblyDCStart_V1`|155|在启动断开期间枚举程序集。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyID`|`win:UInt64`|程序集的唯一 ID。|
|`AppDomainID`|`win:UInt64`|此程序集的域的 ID。|
|`BindingID`|`win:UInt64`|唯一地标识程序集绑定的 ID。|
|`AssemblyFlags`|`win:UInt32`|0x1：非特定于域的程序集。<br /><br /> 0x2：动态程序集。<br /><br /> 0x4：程序集具有本机映像。<br /><br /> 0x8：可回收程序集。|
|`AssemblyName`|`win:UnicodeString`|完全限定程序集名称。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="assemblyloadstart-event"></a>AssemblyLoadStart 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`Binder` (0x4)|`AssemblyLoadStart`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`AssemblyLoadStart`|290|已请求程序集加载。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|程序集的名称。|
|`AssemblyPath`|`win:UnicodeString`|程序集名称的路径。|
|`RequestingAssembly`|`win:UnicodeString`|正在请求的（“父”）程序集的名称。|
|`AssemblyLoadContext`|`win:UnicodeString`|程序集的加载上下文。|
|`RequestingAssemblyLoadContext`|`win:UnicodeString`|正在请求的（“父”）程序集的加载上下文。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="assemblyloadstop-event"></a>AssemblyLoadStop 事件

|引发事件的关键字|事件|Level|
|-----------------------------------|-----------|-----------|
|`Binder` (0x4)|`AssemblyLoadStart`|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`AssemblyLoadStart`|291|已请求程序集加载。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|程序集的名称。|
|`AssemblyPath`|`win:UnicodeString`|程序集名称的路径。|
|`RequestingAssembly`|`win:UnicodeString`|正在请求的（“父”）程序集的名称。|
|`AssemblyLoadContext`|`win:UnicodeString`|程序集的加载上下文。|
|`RequestingAssemblyLoadContext`|`win:UnicodeString`|正在请求的（“父”）程序集的加载上下文。|
|`Success`|`win:Boolean`|程序集加载是否成功。|
|`ResultAssemblyName`|`win:UnicodeString`|已加载的程序集的名称。|
|`ResultAssemblyPath`|`win:UnicodeString`|从中加载的程序集的路径。|
|`Cached`|`win:UnicodeString`|是否缓存了加载。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="resolutionattempted-event"></a>ResolutionAttempted 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`Binder` (0x4)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`ResolutionAttempted`|292|已请求程序集加载。

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|程序集的名称。|
|`Stage`|`win:UInt16`|解析阶段。<br/><br/>0：在加载中查找。<br/><br/>1：程序集加载上下文</br><br/>2：部署应用程序集。<br/><br/>3：默认程序集加载上下文回退。 <br/><br/>4：解析附属程序集。 <br/><br/>5：正在解析程序集加载上下文。<br/><br/>6：正在解析 AppDomain 程序集。
|`AssemblyLoadContext`|`win:UnicodeString`|程序集的加载上下文。|
|`Result`|`win:UInt16`|解析尝试的结果。<br/><br/>0：成功<br/><br/>1：未找到程序集<br/><br/>2：版本不兼容<br/><br/>3：程序集名称不匹配<br/><br/>4：失败<br/><br/>5：异常|
|`ResultAssemblyName`|`win:UnicodeString`|已解析的程序集的名称。|
|`ResultAssemblyPath`|`win:UnicodeString`|从中解析的程序集的路径。|
|`ErrorMessage`|`win:UnicodeString`|发生异常时的错误消息。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="assemblyloadcontextresolvinghandlerinvoked-event"></a>AssemblyLoadContextResolvingHandlerInvoked 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`Binder` (0x4)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`AssemblyLoadContextResolvingHandlerInvoked`|293|已调用 [AssemblyLoadContext.Resolving](xref:System.Runtime.Loader.AssemblyLoadContext.Resolving) 处理程序。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|程序集的名称。|
|`HandlerName`|`win:UnicodeString`|已调用的处理程序的名称。|
|`AssemblyLoadContext`|`win:UnicodeString`|程序集的加载上下文。|
|`ResultAssemblyName`|`win:UnicodeString`|已解析的程序集的名称。|
|`ResultAssemblyPath`|`win:UnicodeString`|从中解析的程序集的路径。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="appdomainassemblyresolvehandlerinvoked-event"></a>AppDomainAssemblyResolveHandlerInvoked 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`Binder` (0x4)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`AppDomainAssemblyResolveHandlerInvoked`|294|已调用 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 处理程序。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|程序集的名称。|
|`HandlerName`|`win:UnicodeString`|已调用的处理程序的名称。|
|`ResultAssemblyName`|`win:UnicodeString`|已解析的程序集的名称。|
|`ResultAssemblyPath`|`win:UnicodeString`|从中解析的程序集的路径。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="assemblyloadfromresolvehandlerinvoked-event"></a>AssemblyLoadFromResolveHandlerInvoked 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`Binder` (0x4)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`AssemblyLoadFromResolveHandlerInvoked`|295|已调用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 处理程序。|

|字段名|数据类型|说明|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|程序集的名称。|
|`IsTrackedLoad`|`win:Boolean`|是否跟踪程序集加载。|
|`RequestingAssemblyPath`|`win:UnicodeString`|正在请求的程序集的路径。|
|`ComputedRequestedAssemblyPath`|`win:UnicodeString`|已请求的程序集的路径。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|

## <a name="knownpathprobed-event"></a>KnownPathProbed 事件

|引发事件的关键字|Level|
|-----------------------------------|-----------|-----------|
|`Binder` (0x4)|信息性 (4)|

|事件|事件 ID|说明|
|-----------|--------------|-----------------|
|`KnownPathProbed`|296|探测到了程序集的一个已知路径。|

|字段名称|数据类型|说明|
|----------------|---------------|-----------------|
|`FilePath`|`win:UnicodeString`|已探测的路径。|
|`Source`|`win:UInt16`|已探测的路径的源。<br/><br/>0x0：应用程序集。<br/><br/>0x1：应用本机映像路径。<br/><br/>0x2：应用路径。</br><br/>0x3：平台资源根。<br/><br/>0x4：附属子目录。</br>|
|`Result`|`win:UInt32`|探测的 HRESULT。|
|`ClrInstanceID`|`win:UInt16`|CoreCLR 实例的唯一 ID。|
