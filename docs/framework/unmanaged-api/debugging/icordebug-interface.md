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
# <a name="icordebug-interface"></a><span data-ttu-id="d7b9e-103">ICorDebug 接口</span><span class="sxs-lookup"><span data-stu-id="d7b9e-103">ICorDebug Interface</span></span>

<span data-ttu-id="d7b9e-104">提供允许开发人员在公共语言运行时 (CLR) 环境中调试应用程序的方法。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-104">Provides methods that allow developers to debug applications in the common language runtime (CLR) environment.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="d7b9e-105">混合模式 (托管代码和本机代码) 调试在非 x86 平台 (如 IA64 和 AMD64) 上不受支持。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-105">Mixed-mode (managed and native code) debugging is not supported on non-x86 platforms (such as IA64 and AMD64).</span></span>  
  
## <a name="methods"></a><span data-ttu-id="d7b9e-106">方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-106">Methods</span></span>  
  
|<span data-ttu-id="d7b9e-107">方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-107">Method</span></span>|<span data-ttu-id="d7b9e-108">描述</span><span class="sxs-lookup"><span data-stu-id="d7b9e-108">Description</span></span>|  
|------------|-----------------|  
|[<span data-ttu-id="d7b9e-109">CanLaunchOrAttach 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-109">CanLaunchOrAttach Method</span></span>](icordebug-canlaunchorattach-method.md)|<span data-ttu-id="d7b9e-110">确定是否可以在当前计算机和运行时配置的上下文中启动新进程或附加到给定进程。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-110">Determines whether launching a new process or attaching to the given process is possible within the context of the current machine and runtime configuration.</span></span>|  
|[<span data-ttu-id="d7b9e-111">CreateProcess 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-111">CreateProcess Method</span></span>](icordebug-createprocess-method.md)|<span data-ttu-id="d7b9e-112">在调试器的控制下启动进程及其主线程。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-112">Launches a process and its primary thread under the control of the debugger.</span></span>|  
|[<span data-ttu-id="d7b9e-113">DebugActiveProcess 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-113">DebugActiveProcess Method</span></span>](icordebug-debugactiveprocess-method.md)|<span data-ttu-id="d7b9e-114">将调试器附加到现有进程。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-114">Attaches the debugger to an existing process.</span></span>|  
|[<span data-ttu-id="d7b9e-115">EnumerateProcesses 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-115">EnumerateProcesses Method</span></span>](icordebug-enumerateprocesses-method.md)|<span data-ttu-id="d7b9e-116">获取正在调试的进程的枚举器。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-116">Gets an enumerator for the processes that are being debugged.</span></span>|  
|[<span data-ttu-id="d7b9e-117">GetProcess 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-117">GetProcess Method</span></span>](icordebug-getprocess-method.md)|<span data-ttu-id="d7b9e-118">返回具有给定进程 ID 的 "ICorDebugProcess" 对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-118">Returns the "ICorDebugProcess" object with the given process ID.</span></span>|  
|[<span data-ttu-id="d7b9e-119">Initialize 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-119">Initialize Method</span></span>](icordebug-initialize-method.md)|<span data-ttu-id="d7b9e-120">初始化 `ICorDebug` 对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-120">Initializes the `ICorDebug` object.</span></span>|  
|[<span data-ttu-id="d7b9e-121">SetManagedHandler 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-121">SetManagedHandler Method</span></span>](icordebug-setmanagedhandler-method.md)|<span data-ttu-id="d7b9e-122">指定托管事件的事件处理程序对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-122">Specifies the event handler object for managed events.</span></span>|  
|[<span data-ttu-id="d7b9e-123">SetUnmanagedHandler 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-123">SetUnmanagedHandler Method</span></span>](icordebug-setunmanagedhandler-method.md)|<span data-ttu-id="d7b9e-124">指定非托管事件的事件处理程序对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-124">Specifies the event handler object for unmanaged events.</span></span>|  
|[<span data-ttu-id="d7b9e-125">Terminate 方法</span><span class="sxs-lookup"><span data-stu-id="d7b9e-125">Terminate Method</span></span>](icordebug-terminate-method.md)|<span data-ttu-id="d7b9e-126">终止 `ICorDebug` 对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-126">Terminates the `ICorDebug` object.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="d7b9e-127">注解</span><span class="sxs-lookup"><span data-stu-id="d7b9e-127">Remarks</span></span>  

 <span data-ttu-id="d7b9e-128">`ICorDebug` 表示调试器进程的事件处理循环。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-128">`ICorDebug` represents an event processing loop for a debugger process.</span></span> <span data-ttu-id="d7b9e-129">调试程序必须等待所有正在调试的进程中的 [ICorDebugManagedCallback：： ExitProcess](icordebugmanagedcallback-exitprocess-method.md) 回调，然后释放此接口。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-129">The debugger must wait for the [ICorDebugManagedCallback::ExitProcess](icordebugmanagedcallback-exitprocess-method.md) callback from all processes being debugged before releasing this interface.</span></span>  
  
 <span data-ttu-id="d7b9e-130">`ICorDebug`对象是控制所有进一步托管调试的初始对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-130">The `ICorDebug` object is the initial object to control all further managed debugging.</span></span> <span data-ttu-id="d7b9e-131">在 .NET Framework 版本1.0 和1.1 中，此对象是 `CoClass` 从 COM 创建的对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-131">In the .NET Framework versions 1.0 and 1.1, this object was a `CoClass` object created from COM.</span></span> <span data-ttu-id="d7b9e-132">在 .NET Framework 版本2.0 中，此对象不再是 `CoClass` 对象。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-132">In the .NET Framework version 2.0, this object is no longer a `CoClass` object.</span></span> <span data-ttu-id="d7b9e-133">它必须通过 [CreateDebuggingInterfaceFromVersion](../hosting/createdebugginginterfacefromversion-function.md) 函数创建，该函数更易于识别。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-133">It must be created by the [CreateDebuggingInterfaceFromVersion](../hosting/createdebugginginterfacefromversion-function.md) function, which is more version-aware.</span></span> <span data-ttu-id="d7b9e-134">此新创建函数使客户端可以获取的特定实现 `ICorDebug` ，这也会模拟调试 API 的特定版本。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-134">This new creation function enables clients to get a specific implementation of `ICorDebug`, which also emulates a specific version of the debugging API.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="d7b9e-135">此接口不支持跨计算机或跨进程远程调用。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-135">This interface does not support being called remotely, either cross-machine or cross-process.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="d7b9e-136">要求</span><span class="sxs-lookup"><span data-stu-id="d7b9e-136">Requirements</span></span>  

 <span data-ttu-id="d7b9e-137">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="d7b9e-137">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="d7b9e-138">**标头**：CorDebug.idl、CorDebug.h</span><span class="sxs-lookup"><span data-stu-id="d7b9e-138">**Header:** CorDebug.idl, CorDebug.h</span></span>  
  
 <span data-ttu-id="d7b9e-139">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="d7b9e-139">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="d7b9e-140">**.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="d7b9e-140">**.NET Framework Versions:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="d7b9e-141">另请参阅</span><span class="sxs-lookup"><span data-stu-id="d7b9e-141">See also</span></span>

- [<span data-ttu-id="d7b9e-142">调试接口</span><span class="sxs-lookup"><span data-stu-id="d7b9e-142">Debugging Interfaces</span></span>](debugging-interfaces.md)
