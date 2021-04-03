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
# <a name="icorprofilercallback10eventpipeeventdelivered-method"></a>ICorProfilerCallback10：： EventPipeEventDelivered 方法

每当 EventPipe 事件传递到探查器的当前活动会话时，通知探查器。  
  
## <a name="syntax"></a>语法  
  
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
  
## <a name="parameters"></a>参数

`provider` 中此事件源自的提供程序。

`eventId` 中要传递的事件的 ID。

`eventVersion` 中要传递的事件的版本。

`cbMetadataBlob` 中的长度（以字节为单位） `metadataBlob` 。

`metadataBlob` 中指向事件的元数据 blob 的指针。

`cbEventData` 中的长度（以字节为单位） `eventData` 。

`eventData` 中事件的负载。

`pActivityId` 中指向表示事件的活动 ID 的 GUID 的指针，或为 NULL。

`pRelatedActivityId` 中指向表示事件相关活动 ID 的 GUID 的指针，或为 NULL。

`eventThread` 中发生事件的线程的 ID。

`numStackFrames` 中数组中元素的数目 `stackFrames` 。

`stackFrames` 中一个代码地址数组，表示事件的托管调用堆栈。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 接口](icorprofilerinfo12-interface.md)
- [ICorProfilerInfo12. EventPipeStartSession 方法](icorprofilerinfo12-eventpipestartsession-method.md)
