---
description: 了解详细信息： ICorProfilerCallback10：： EventPipeEventDelivered 方法
title: ICorProfilerCallback10：： EventPipeEventDelivered 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10.EventPipeEventDelivered
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 73f3eb64671b22b1ec16b5a5b1b24115f7f65f6d
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231331"
---
# <a name="icorprofilercallback10eventpipeeventdelivered-method"></a><span data-ttu-id="cb760-103">ICorProfilerCallback10：： EventPipeEventDelivered 方法</span><span class="sxs-lookup"><span data-stu-id="cb760-103">ICorProfilerCallback10::EventPipeEventDelivered Method</span></span>

<span data-ttu-id="cb760-104">每当 EventPipe 事件传递到探查器的当前活动会话时，通知探查器。</span><span class="sxs-lookup"><span data-stu-id="cb760-104">Notifies the profiler whenever an EventPipe event has been delivered to the profiler's currently active session.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="cb760-105">语法</span><span class="sxs-lookup"><span data-stu-id="cb760-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeEventDelivered(
        [in] EVENTPIPE_PROVIDER provider,
        [in] DWORD eventId,
        [in] DWORD eventVersion,
        [in] ULONG cbMetadataBlob,
        [in, size_is(cbMetadataBlob)] LPCBYTE metadataBlob,
        [in] ULONG cbEventData,
        [in, size_is(cbEventData)] LPCBYTE eventData,
        [in] LPCGUID pActivityId,
        [in] LPCGUID pRelatedActivityId,
        [in] ThreadID eventThread,
        [in] ULONG numStackFrames,
        [in, length_is(numStackFrames)] UINT_PTR stackFrames[]);
```  
  
## <a name="parameters"></a><span data-ttu-id="cb760-106">参数</span><span class="sxs-lookup"><span data-stu-id="cb760-106">Parameters</span></span>

<span data-ttu-id="cb760-107">`provider` 中此事件源自的提供程序。</span><span class="sxs-lookup"><span data-stu-id="cb760-107">`provider` [in] The provider that this event originated from.</span></span>

<span data-ttu-id="cb760-108">`eventId` 中要传递的事件的 ID。</span><span class="sxs-lookup"><span data-stu-id="cb760-108">`eventId` [in] The ID of the event being delivered.</span></span>

<span data-ttu-id="cb760-109">`eventVersion` 中要传递的事件的版本。</span><span class="sxs-lookup"><span data-stu-id="cb760-109">`eventVersion` [in] The version of the event being delivered.</span></span>

<span data-ttu-id="cb760-110">`cbMetadataBlob` 中的长度（以字节为单位） `metadataBlob` 。</span><span class="sxs-lookup"><span data-stu-id="cb760-110">`cbMetadataBlob` [in] The length, in bytes, of `metadataBlob`.</span></span>

<span data-ttu-id="cb760-111">`metadataBlob` 中指向事件的元数据 blob 的指针。</span><span class="sxs-lookup"><span data-stu-id="cb760-111">`metadataBlob` [in] A pointer to the metadata blob for the event.</span></span>

<span data-ttu-id="cb760-112">`cbEventData` 中的长度（以字节为单位） `eventData` 。</span><span class="sxs-lookup"><span data-stu-id="cb760-112">`cbEventData` [in] The length, in bytes, of `eventData`.</span></span>

<span data-ttu-id="cb760-113">`eventData` 中事件的负载。</span><span class="sxs-lookup"><span data-stu-id="cb760-113">`eventData` [in] The payload for the event.</span></span>

<span data-ttu-id="cb760-114">`pActivityId` 中指向表示事件的活动 ID 的 GUID 的指针，或为 NULL。</span><span class="sxs-lookup"><span data-stu-id="cb760-114">`pActivityId` [in] A pointer to the GUID that represents the event's activity ID, or NULL.</span></span>

<span data-ttu-id="cb760-115">`pRelatedActivityId` 中指向表示事件相关活动 ID 的 GUID 的指针，或为 NULL。</span><span class="sxs-lookup"><span data-stu-id="cb760-115">`pRelatedActivityId` [in] A pointer to the GUID that represents the event's related activity ID, or NULL.</span></span>

<span data-ttu-id="cb760-116">`eventThread` 中发生事件的线程的 ID。</span><span class="sxs-lookup"><span data-stu-id="cb760-116">`eventThread` [in] The ID of the thread the event occurred on.</span></span>

<span data-ttu-id="cb760-117">`numStackFrames` 中数组中元素的数目 `stackFrames` 。</span><span class="sxs-lookup"><span data-stu-id="cb760-117">`numStackFrames` [in] The number of elements in the `stackFrames` array.</span></span>

<span data-ttu-id="cb760-118">`stackFrames` 中一个代码地址数组，表示事件的托管调用堆栈。</span><span class="sxs-lookup"><span data-stu-id="cb760-118">`stackFrames` [in] An array of code addresses representing the managed callstack of the event.</span></span>

## <a name="requirements"></a><span data-ttu-id="cb760-119">要求</span><span class="sxs-lookup"><span data-stu-id="cb760-119">Requirements</span></span>  

<span data-ttu-id="cb760-120">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="cb760-120">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="cb760-121">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="cb760-121">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="cb760-122">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="cb760-122">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="cb760-123">另请参阅</span><span class="sxs-lookup"><span data-stu-id="cb760-123">See also</span></span>

- [<span data-ttu-id="cb760-124">分析接口</span><span class="sxs-lookup"><span data-stu-id="cb760-124">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="cb760-125">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="cb760-125">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="cb760-126">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="cb760-126">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
- [<span data-ttu-id="cb760-127">ICorProfilerInfo12. EventPipeStartSession 方法</span><span class="sxs-lookup"><span data-stu-id="cb760-127">ICorProfilerInfo12.EventPipeStartSession Method</span></span>](icorprofilerinfo12-eventpipestartsession-method.md)
