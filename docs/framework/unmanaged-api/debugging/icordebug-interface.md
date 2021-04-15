---
description: 了解详细信息： ICorDebug 接口
title: ICorDebug 接口
ms.date: 03/30/2017
api_name:
- ICorDebug
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebug
helpviewer_keywords:
- ICorDebug interface [.NET Framework debugging]
ms.assetid: 33f431d7-ab1a-494d-8af2-20ab15aba194
topic_type:
- apiref
ms.openlocfilehash: db6c407e3361ebd9c39dd2c3cf7e5b53a69e0559
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494826"
---
# <a name="icordebug-interface"></a>ICorDebug 接口

提供允许开发人员在公共语言运行时 (CLR) 环境中调试应用程序的方法。  
  
> [!NOTE]
> 混合模式 (托管代码和本机代码) 调试在非 x86 平台 (如 IA64 和 AMD64) 上不受支持。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CanLaunchOrAttach 方法](icordebug-canlaunchorattach-method.md)|确定是否可以在当前计算机和运行时配置的上下文中启动新进程或附加到给定进程。|  
|[CreateProcess 方法](icordebug-createprocess-method.md)|在调试器的控制下启动进程及其主线程。|  
|[DebugActiveProcess 方法](icordebug-debugactiveprocess-method.md)|将调试器附加到现有进程。|  
|[EnumerateProcesses 方法](icordebug-enumerateprocesses-method.md)|获取正在调试的进程的枚举器。|  
|[GetProcess 方法](icordebug-getprocess-method.md)|返回具有给定进程 ID 的 "ICorDebugProcess" 对象。|  
|[Initialize 方法](icordebug-initialize-method.md)|初始化 `ICorDebug` 对象。|  
|[SetManagedHandler 方法](icordebug-setmanagedhandler-method.md)|指定托管事件的事件处理程序对象。|  
|[SetUnmanagedHandler 方法](icordebug-setunmanagedhandler-method.md)|指定非托管事件的事件处理程序对象。|  
|[Terminate 方法](icordebug-terminate-method.md)|终止 `ICorDebug` 对象。|  
  
## <a name="remarks"></a>注解  

 `ICorDebug` 表示调试器进程的事件处理循环。 调试程序必须等待所有正在调试的进程中的 [ICorDebugManagedCallback：： ExitProcess](icordebugmanagedcallback-exitprocess-method.md) 回调，然后释放此接口。  
  
 `ICorDebug`对象是控制所有进一步托管调试的初始对象。 在 .NET Framework 版本1.0 和1.1 中，此对象是 `CoClass` 从 COM 创建的对象。 在 .NET Framework 版本2.0 中，此对象不再是 `CoClass` 对象。 它必须通过 [CreateDebuggingInterfaceFromVersion](../hosting/createdebugginginterfacefromversion-function.md) 函数创建，该函数更易于识别。 此新创建函数使客户端可以获取的特定实现 `ICorDebug` ，这也会模拟调试 API 的特定版本。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
