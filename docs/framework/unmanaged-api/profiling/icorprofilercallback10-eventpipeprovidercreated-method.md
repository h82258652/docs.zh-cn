---
description: 了解详细信息： ICorProfilerCallback10：： EventPipeProviderCreated 方法
title: ICorProfilerCallback10：： EventPipeProviderCreated 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10.EventPipeProviderCreated
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 6ce96bf301f1c559923df64edd9dd214c28db918
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231305"
---
# <a name="icorprofilercallback10eventpipeprovidercreated-method"></a><span data-ttu-id="c4a6a-103">ICorProfilerCallback10：： EventPipeProviderCreated 方法</span><span class="sxs-lookup"><span data-stu-id="c4a6a-103">ICorProfilerCallback10::EventPipeProviderCreated Method</span></span>

<span data-ttu-id="c4a6a-104">每当创建 EventPipe 提供程序时，通知探查器。</span><span class="sxs-lookup"><span data-stu-id="c4a6a-104">Notifies the profiler whenever an EventPipe provider is created.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="c4a6a-105">语法</span><span class="sxs-lookup"><span data-stu-id="c4a6a-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeProviderCreated([in] EVENTPIPE_PROVIDER provider);
```  
  
## <a name="parameters"></a><span data-ttu-id="c4a6a-106">参数</span><span class="sxs-lookup"><span data-stu-id="c4a6a-106">Parameters</span></span>

<span data-ttu-id="c4a6a-107">`provider` 中已创建的提供程序。</span><span class="sxs-lookup"><span data-stu-id="c4a6a-107">`provider` [in] The provider that was created.</span></span>

## <a name="requirements"></a><span data-ttu-id="c4a6a-108">要求</span><span class="sxs-lookup"><span data-stu-id="c4a6a-108">Requirements</span></span>  

<span data-ttu-id="c4a6a-109">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="c4a6a-109">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="c4a6a-110">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="c4a6a-110">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="c4a6a-111">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="c4a6a-111">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="c4a6a-112">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c4a6a-112">See also</span></span>

- [<span data-ttu-id="c4a6a-113">分析接口</span><span class="sxs-lookup"><span data-stu-id="c4a6a-113">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="c4a6a-114">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="c4a6a-114">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="c4a6a-115">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="c4a6a-115">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
- [<span data-ttu-id="c4a6a-116">ICorProfilerInfo12. EventPipeAddProviderToSession 方法</span><span class="sxs-lookup"><span data-stu-id="c4a6a-116">ICorProfilerInfo12.EventPipeAddProviderToSession Method</span></span>](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)
